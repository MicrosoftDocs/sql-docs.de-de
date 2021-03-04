---
description: Dieses JDBC-Codebeispiel veranschaulicht, wie Sie Azure Key Vault als Anbieter eines Schlüsselspeichers einsetzen, wenn Always Encrypted verwendet wird.
title: Beispiel für Azure Key Vault
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: lilgreenbird
ms.author: v-susanh
ms.openlocfilehash: caba4b7efb21aa2930564ec9a1d51147dd4d9dfb
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836977"
---
# <a name="azure-key-vault-sample"></a>Beispiel für Azure Key Vault

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

## <a name="sample-application-using-azure-key-vault-feature"></a>Beispielanwendung mit Azure Key Vault-Feature

Diese Anwendung ist mit dem JDBC-Treiber 9.2 und höher, Azure-Security-Keyvault (Version 4.2.1), Azure-Identity (Version 1.1.3) und den jeweiligen Abhängigkeiten ausführbar. Die zugrunde liegenden Abhängigkeiten können aufgelöst werden, indem Sie diese Bibliotheken der Projektobjektmodelldatei (Project Object Model, POM) des Projekts hinzufügen. Weitere Informationen zu Featureabhängigkeiten finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](../../connect/jdbc/feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

```java
import java.net.URISyntaxException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;
import com.microsoft.aad.msal4j.UserNamePasswordParameters;

import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionAzureKeyVaultProvider;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerConnection;
import com.microsoft.sqlserver.jdbc.SQLServerException;
import com.microsoft.sqlserver.jdbc.SQLServerKeyVaultAuthenticationCallback;

public class AKV {

    static String connectionUrl = "jdbc:sqlserver://localhost;integratedSecurity=true;database=test;columnEncryptionSetting=enabled";
    static String applicationClientID = "Your Client ID";
    static String applicationKey = "Your Application Key";
    static String keyID = "Your Key ID";
    static String cmkName = "AKV_CMK_JDBC";
    static String cekName = "AKV_CEK_JDBC";
    static String akvTable = "akvTable";

    static String createTableSQL = "create table " + akvTable + " ("
            + "PlainNvarcharMax nvarchar(max) null,"
            + "RandomizedNvarcharMax nvarchar(max) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', COLUMN_ENCRYPTION_KEY = "
            + cekName + ") NULL,"
            + "DeterministicNvarcharMax nvarchar(max) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', COLUMN_ENCRYPTION_KEY = "
            + cekName + ") NULL" + ");";

    public static void main(String[] args)
            throws ClassNotFoundException, Exception {
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        try (Connection connection = DriverManager.getConnection(connectionUrl);
                Statement statement = connection.createStatement()) {
            statement.execute("DBCC FREEPROCCACHE");

            statement.execute("DBCC FREEPROCCACHE");    
            System.out.println("Create SQLServerColumnEncryptionAzureKeyVaultProvider with 'authenticationCallback'");
            /* Constructor added in 7.0.0 driver version [Supports SQLServerKeyVaultAuthenticationCallback in 7.0 for backwards compatibility]
             * This constructor is recommended to replace the above deprecated constructor */
            SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider2 = new SQLServerColumnEncryptionAzureKeyVaultProvider(
                    tryAuthenticationCallback());
            setupKeyStoreProviders(akvProvider2.getName(), akvProvider2);
            testAKV(akvProvider2.getName(), akvProvider2, connection, statement);

            statement.execute("DBCC FREEPROCCACHE");
            System.out.println("Create SQLServerColumnEncryptionAzureKeyVaultProvider with 'clientId' and 'clientKey'");
            /* Constructor added in 6.2.2 driver version [Continued Support] */
            SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider3 = new SQLServerColumnEncryptionAzureKeyVaultProvider(
                    applicationClientID, applicationKey);
            setupKeyStoreProviders(akvProvider3.getName(), akvProvider3);
            testAKV(akvProvider3.getName(), akvProvider3, connection, statement);

            statement.execute("DBCC FREEPROCCACHE");
            System.out.println("Create SQLServerColumnEncryptionAzureKeyVaultProvider with 'token credential'");
            /* see Azure Identity client library for Java 
             * https://docs.microsoft.com/java/api/overview/azure/identity-readme?view=azure-java-stable */
            ClientSecretCredential tokenCredential = new ClientSecretCredentialBuilder().tenantId(tenantID)
                .clientId(applicationClientID).clientSecret(applicationKey).build();
            /* Constructor added in 9.2.0 driver version */
            SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider4 = new SQLServerColumnEncryptionAzureKeyVaultProvider(
                    tokenCredential);
            setupKeyStoreProviders(akvProvider4.getName(), akvProvider4);
            testAKV(akvProvider4.getName(), akvProvider4, connection, statement);

            System.exit(0);
        }
    }

    private static SQLServerKeyVaultAuthenticationCallback tryAuthenticationCallback()
            throws URISyntaxException, SQLServerException {
        SQLServerKeyVaultAuthenticationCallback authenticationCallback = new SQLServerKeyVaultAuthenticationCallback() {

            @Override
            public String getAccessToken(String authority, String resource, String scope) {
                try {                                                                                                              IClientCredential credential = ClientCredentialFactory.createFromSecret(applicationKey);
                    ConfidentialClientApplication confidentialClientApplication = ConfidentialClientApplication
                            .builder(applicationClientID, credential).authority(authority).build();
                    Set<String> scopes = new HashSet<>();
                    scopes.add(scope);
                    return confidentialClientApplication.acquireToken(ClientCredentialParameters.builder(scopes).build()).get().accessToken();
                } catch (Exception e) {
                    fail(TestResource.getResource("R_unexpectedException") + e.getMessage());
                }
                return null;
            }
        };

        return authenticationCallback;

    }

    private static void testAKV(String CUSTOM_AKV_PROVIDER_NAME,
            SQLServerColumnEncryptionKeyStoreProvider akvProvider,
            Connection connection, Statement statement)
            throws SQLException, InterruptedException {

        dropTable(statement);
        dropKeys(statement);

        System.out.println("createCMK");
        createCMK(CUSTOM_AKV_PROVIDER_NAME, statement);

        System.out.println("createCEK");
        createCEK(akvProvider, statement);

        System.out.println("create Table");
        statement.execute(createTableSQL);

        System.out.println("populate");
        populateCharNormalCase(connection);

        System.out.println("run the test");
        testChar(statement);
    }

    private static void setupKeyStoreProviders(String CUSTOM_AKV_PROVIDER_NAME,
            SQLServerColumnEncryptionKeyStoreProvider akvProvider)
            throws SQLServerException {
        /* unregister all previously registered providers if any */
        SQLServerConnection.unregisterColumnEncryptionKeyStoreProviders();
        Map<String, SQLServerColumnEncryptionKeyStoreProvider> map1 = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
        map1.put(CUSTOM_AKV_PROVIDER_NAME, akvProvider);
        SQLServerConnection.registerColumnEncryptionKeyStoreProviders(map1);
    }

    private static void dropTable(Statement statement) throws SQLException {
        statement.executeUpdate("if object_id('" + akvTable
                + "','U') is not null" + " drop table " + akvTable);
    }

    private static void dropKeys(Statement statement) throws SQLException {
        statement.executeUpdate(
                "if exists (SELECT name from sys.column_encryption_keys where name='"
                        + cekName + "')" + " begin"
                        + " drop column encryption key " + cekName + " end");
        statement.executeUpdate(
                "if exists (SELECT name from sys.column_master_keys where name='"
                        + cmkName + "')" + " begin" + " drop column master key "
                        + cmkName + " end");
    }

    private static void createCMK(String CUSTOM_AKV_PROVIDER_NAME,
            Statement statement) throws SQLException {
        String _createColumnMasterKeyTemplate = String.format(
                "CREATE COLUMN MASTER KEY [%s] WITH ( KEY_STORE_PROVIDER_NAME = '%s', KEY_PATH = '%s');",
                cmkName, CUSTOM_AKV_PROVIDER_NAME, keyID);
        statement.execute(_createColumnMasterKeyTemplate);
    }

    private static void createCEK(
            SQLServerColumnEncryptionKeyStoreProvider storeProvider,
            Statement statement) throws SQLServerException, SQLException {
        String letters = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa";
        byte[] valuesDefault = letters.getBytes();
        byte[] key = storeProvider.encryptColumnEncryptionKey(keyID, "RSA_OAEP",
                valuesDefault);
        String cekSql = "CREATE COLUMN ENCRYPTION KEY " + cekName
                + " WITH VALUES " + "(COLUMN_MASTER_KEY = " + cmkName
                + ", ALGORITHM = 'RSA_OAEP', ENCRYPTED_VALUE = 0x"
                + bytesToHexString(key, key.length) + ")" + ";";
        statement.execute(cekSql);
    }

    final static char[] hexChars = {'0', '1', '2', '3', '4', '5', '6', '7', '8',
            '9', 'A', 'B', 'C', 'D', 'E', 'F'};

    private static String bytesToHexString(byte[] b, int length) {
        StringBuilder sb = new StringBuilder(length * 2);
        for (int i = 0; i < length; i++) {
            int hexVal = b[i] & 0xFF;
            sb.append(hexChars[(hexVal & 0xF0) >> 4]);
            sb.append(hexChars[(hexVal & 0x0F)]);
        }
        return sb.toString();
    }

    private static void populateCharNormalCase(Connection connection)
            throws SQLException {
        String sql = "insert into " + akvTable + " values(?,?,?)";
        try (PreparedStatement pstmt = connection.prepareStatement(sql)) {
            for (int i = 1; i <= 5; i++) { //Insert 5 rows
                for (int j = 1; j <= 3; j++) {
                    pstmt.setNString(j, "Row " + i + " Column " + j);
                }
                pstmt.execute();
            }
        }
    }

    /**
     * Rerieves the table
     *
     * @throws SQLException
     */
    private static void testChar(Statement statement) throws SQLException {
        try (ResultSet rs = statement
                .executeQuery("select * from " + akvTable);) {
            int numberOfColumns = rs.getMetaData().getColumnCount();
            while (rs.next()) {
                for (int i = 1; i <= numberOfColumns; i++) {
                    System.out.println(rs.getString(i));
                }
            }
        }
    }
}
```

## <a name="see-also"></a>Siehe auch

[Azure Key Vault-Beispielversion 7.0](../../connect/jdbc/azure-key-vault-sample-version-7.0.md)  
[Azure Key Vault-Beispielversion 6.2.2](../../connect/jdbc/azure-key-vault-sample-version-6.2.2.md)  
[Azure Key Vault-Beispielversion 6.0.0](../../connect/jdbc/azure-key-vault-sample-version-6.0.0.md) 
