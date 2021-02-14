---
title: Erstellen einer Entwurfszeitkomponente für ein benutzerdefiniertes Berichtselement | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Laufzeitkomponente für ein benutzerdefiniertes Berichtselement erstellen und die Eigenschaft für diese Komponente in der Entwurfsumgebung definieren.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: b3e15a4a-98f8-4dbb-b847-bbcb20327051
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c6d70bba220624cd43ca1ced80cbe89ba936460
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061955"
---
# <a name="creating-a-custom-report-item-run-time-component"></a>Erstellen einer Laufzeitkomponente für ein benutzerdefiniertes Berichtselement
  Die Laufzeitkomponente für ein benutzerdefiniertes Berichtselement ist in einer [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Komponente mithilfe einer beliebigen CLS-kompatiblen Sprache implementiert und wird während der Laufzeit vom Prozessor aufgerufen. Sie definieren die Eigenschaften für die Laufzeitkomponente in der Entwurfsumgebung, indem Sie die entsprechende Entwurfszeitkomponente für ein benutzerdefiniertes Berichtselement ändern.  
  
 Ein Beispiel für ein vollständig implementiertes benutzerdefiniertes Berichtselement finden Sie unter [SQL Server Reporting Services-Produktbeispiele](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="definition-and-instance-objects"></a>Definitions- and Instanzobjekte  
 Bevor Sie ein benutzerdefiniertes Berichtselement implementieren, sollten Sie den Unterschied zwischen *Definitionsobjekten* und *Instanzobjekten* kennen. Definitionsobjekte stellen die RDL-Darstellung des benutzerdefinierten Berichtselements bereit, wohingegen es sich bei Instanzobjekten um die bewerteten Versionen von Definitionsobjekten handelt. Für jedes Element des Berichts gibt es nur ein Definitionsobjekt. Beim Zugreifen auf Eigenschaften auf einem Definitionsobjekt, die Ausdrücke enthalten, erhalten Sie die nicht bewertete Ausdruckszeichenfolge. Instanzobjekte enthalten die bewerteten Versionen der Definitionsobjekte und können in einer 1:n-Beziehung mit dem Definitionsobjekt eines Elements stehen. Wenn beispielsweise ein Bericht einen <xref:Microsoft.ReportingServices.OnDemandReportRendering.Tablix>-Datenbereich besitzt, der ein <xref:Microsoft.ReportingServices.OnDemandReportRendering.CustomReportItem> in einer Detailzeile enthält, ist nur ein einziges Definitionsobjekt vorhanden, es gibt jedoch für jede Zeile im Datenbereich eine Instanz.  
  
## <a name="implementing-the-icustomreportitem-interface"></a>Implementieren der ICustomReportItem-Schnittstelle  
 Um ein **CustomReportItem** zu erstellen, müssen Sie die <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem>-Schnittstelle implementieren, die in der Datei „Microsoft.ReportingServices.ProcessingCore.dll“ definiert ist:  
  
```csharp  
namespace Microsoft.ReportingServices.OnDemandReportRendering  
{  
    public interface ICustomReportItem  
    {  
        void GenerateReportItemDefinition(CustomReportItem customReportItem);  
void EvaluateReportItemInstance(CustomReportItem customReportItem);  
    }  
}  
```  
  
 Nachdem die <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem>-Schnittstelle implementiert ist, werden zwei Methodenstubs generiert: <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A> und <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A>. Die <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A>-Methode wird zuerst aufgerufen. Sie wird zum Festlegen der Definitionseigenschaften and zum Erstellen des <xref:Microsoft.ReportingServices.OnDemandReportRendering.Image>-Objekts verwendet, das sowohl die zum Rendern des Elements verwendeten Definitions- und die Instanzeigenschaften enthält. Nachdem die Definitionsobjekte bewertet wurden, wird die <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A>-Methode aufgerufen. Sie stellt die Instanzobjekte zum Rendern des Elements bereit.  
  
 Im Folgenden sehen Sie eine Beispielimplementierung eines benutzerdefinierten Berichtselements, das den Namen der Steuerung als Bild rendert.  
  
```csharp  
namespace Microsoft.Samples.ReportingServices  
{  
    using System;  
    using System.Collections.Generic;  
    using System.Collections.Specialized;  
    using System.Drawing.Imaging;  
    using System.IO;  
    using System.Text;  
    using Microsoft.ReportingServices.OnDemandReportRendering;  
  
    public class PolygonsCustomReportItem : ICustomReportItem  
    {  
        #region ICustomReportItem Members  
  
        public void GenerateReportItemDefinition(CustomReportItem cri)  
        {  
            // Create the Image object that will be   
            // used to render the custom report item  
            cri.CreateCriImageDefinition();  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
        }  
  
        public void EvaluateReportItemInstance(CustomReportItem cri)  
        {  
            // Get the Image definition  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
  
            // Create the image for the custom report item  
            polygonImage.ImageInstance.ImageData = DrawImage(cri);  
        }  
  
        #endregion  
  
        /// <summary>  
        /// Creates an image of the CustomReportItem's name  
        /// </summary>  
        private byte[] DrawImage(CustomReportItem customReportItem)  
        {  
            int width = 1;          // pixels  
            int height = 1;         // pixels  
            int resolution = 75;    // dpi  
  
            System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            System.Drawing.Graphics graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Get the Font for the Text  
            System.Drawing.Font font = new System.Drawing.Font(System.Drawing.FontFamily.GenericMonospace,  
                12, System.Drawing.FontStyle.Regular);  
  
            // Get the Brush for drawing the Text  
            System.Drawing.Brush brush = new System.Drawing.SolidBrush(System.Drawing.Color.LightGreen);  
  
            // Get the measurements for the image  
            System.Drawing.SizeF maxStringSize = graphics.MeasureString(customReportItem.Name, font);  
            width = (int)(maxStringSize.Width + 2 * font.GetHeight(resolution));  
            height = (int)(maxStringSize.Height + 2 * font.GetHeight(resolution));  
  
            bitmap.Dispose();  
            bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            graphics.Dispose();  
            graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Draw the text  
            graphics.DrawString(customReportItem.Name, font, brush, font.GetHeight(resolution),   
                font.GetHeight(resolution));  
  
            // Create the byte array of the image data  
            MemoryStream memoryStream = new MemoryStream();  
            bitmap.Save(memoryStream, ImageFormat.Bmp);  
            memoryStream.Position = 0;  
            byte[] imageData = new byte[memoryStream.Length];  
            memoryStream.Read(imageData, 0, imageData.Length);  
  
            return imageData;  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Custom Report Item Architecture (Architektur des benutzerdefinierten Berichtselements)](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [Erstellen einer Entwurfszeitkomponente für ein benutzerdefiniertes Berichtselement](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)   
 [Custom Report Item Class Libraries (Klassenbibliotheken für ein benutzerdefiniertes Berichtselement)](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [Vorgehensweise: Bereitstellen eines benutzerdefinierten Berichtselements](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
  
