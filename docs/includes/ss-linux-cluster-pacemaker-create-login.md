1. **Erstellen Sie auf allen Servern mit SQL Server eine Server-Anmeldung für Pacemaker**. Die folgende Transact-SQL erstellt eine Anmeldung:

   ```Transact-SQL
   USE [master]
   GO
   CREATE LOGIN [pacemakerLogin] with PASSWORD= N'ComplexP@$$w0rd!'
    
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]
   ```

  Für die Erstellung der Verfügbarkeitsgruppe benötigt der Pacemaker-Benutzer die Berechtigungen ALTER, CONTROL und VIEW DEFINITION für die Verfügbarkeitsgruppe, nachdem diese erstellt wurde, aber bevor ihr irgendwelche Knoten hinzugefügt werden.

1. **Speichern Sie auf allen Servern mit SQL Server die Anmeldeinformationen für die SQL Server-Anmeldung**.

   ```bash
   echo 'pacemakerLogin' >> ~/pacemaker-passwd
   echo 'ComplexP@$$w0rd!' >> ~/pacemaker-passwd
   sudo mv ~/pacemaker-passwd /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd
   sudo chmod 400 /var/opt/mssql/secrets/passwd # Only readable by root
   ```
