---
description: ParentCatalog-Eigenschaft – Beispiel (VC++)
title: Beispiel für eine Beispiel Katalog Eigenschaft (VC + +) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- C++
helpviewer_keywords:
- ParentCatalog property [ADOX], VC++ example
ms.assetid: 43ae202e-1972-4aab-9cc1-3b6612bad363
author: rothja
ms.author: jroth
ms.openlocfilehash: 35134dc093129e62ea2b5b32141fbcda9e9d32fd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053945"
---
# <a name="parentcatalog-property-example-vc"></a>ParentCatalog-Eigenschaft – Beispiel (VC++)
Der folgende Code veranschaulicht die Verwendung der Eigenschaft ["Eigenschaft"](./parentcatalog-property-adox.md) , um auf eine anbieterspezifische Eigenschaft zuzugreifen, bevor eine Tabelle an einen Katalog angehängt wird. Die-Eigenschaft ist " **AutoIncrement**", wodurch ein AutoIncrement-Feld in einer Microsoft Jet-Datenbank erstellt wird.  
  
```  
// BeginCreateAutoIncrColumnCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   HRESULT hr = S_OK;  
  
   // Define and initialize ADOX object pointers. These are in ADODB namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   // Define ADODB object pointers.  
   ADODB::_ConnectionPtr m_pCnn = NULL;  
  
   // Define string variables  
   _bstr_t strCnn("Provider='Microsoft.JET.OLEDB.4.0';Data Source='c:\\Northwind.mdb';");  
  
   try {  
      TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof (Catalog)));  
      TESTHR(hr = m_pTable.CreateInstance(__uuidof (Table)));  
  
      // Connect the catalog.  
      m_pCnn->Open (strCnn, "", "", NULL);  
  
      m_pCatalog->PutActiveConnection(variant_t((IDispatch *)m_pCnn));  
  
      m_pTable->Name = "MyContacts";  
      m_pTable->ParentCatalog = m_pCatalog;  
  
      // Create fields and append them to the new Table object.  
      m_pTable->Columns->Append("ContactId", adInteger,0);  
  
      // Make the ContactId column and auto incrementing column  
      m_pTable->Columns->GetItem("ContactId")->Properties->GetItem("AutoIncrement")->Value = true;  
      m_pTable->Columns->Append("CustomerID", adVarWChar,0);  
      m_pTable->Columns->Append("FirstName", adVarWChar,0);  
      m_pTable->Columns->Append("LastName", adVarWChar,0);  
      m_pTable->Columns->Append("Phone", adVarWChar, 20);  
      m_pTable->Columns->Append("Notes", adLongVarWChar,0);  
      m_pCatalog->Tables->Append(_variant_t((IDispatch*)m_pTable));  
  
      // Refresh the database.  
      m_pCatalog->Tables->Refresh();  
  
      printf("Table 'MyContacts' is added.\n");  
  
      // Delete new table, since this is only an example  
      m_pCatalog->Tables->Delete("MyContacts");  
  
      printf("Table 'MyContacts' is deleted.\n");  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
   catch(...) {  
      cout << "Error occurred in CreateAutoIncrColumnX...."<< endl;  
   }  
  
   m_pCatalog = NULL;  
   ::CoUninitialize();  
}  
```