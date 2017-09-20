---
title: Synchronisieren von Inhalt aus einem Cloudordner in Azure App Service
description: Erfahren Sie, wie Sie Ihre App in Azure App Service durch Inhaltssynchronisierung aus einem Cloudordner bereitstellen.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ddda0e60dc9d5414142791175b77a5a65e3f40e3
ms.lasthandoff: 11/17/2016


---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisieren von Inhalt aus einem Cloudordner in Azure App Service
In diesem Tutorial erfahren Sie, wie Sie Ihre Inhalte in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) bereitstellen, in dem Sie die Inhalte aus beliebten Cloudspeicherdiensten wie Dropbox und OneDrive synchronisieren. 

## <a name="a-nameoverviewaoverview-of-content-sync-deployment"></a><a name="overview"></a>Übersicht über die Bereitstellung von Inhaltssynchronisierung
Die On-Demand Bereitstellung von Inhaltssynchronisierung wird vom [Kudu-Bereitstellungsmodul](https://github.com/projectkudu/kudu/wiki) unterstützt, das in App Service integriert ist. Im [Azure-Portal](https://portal.azure.com)können Sie in Ihrem Cloudspeicher einen Ordner bestimmen, mit Ihrem App-Code und den App-Inhalten in diesem Ordner arbeiten und per Klick auf eine Schaltfläche die Synchronisierung mit App Service durchführen. Inhaltssynchronisierung nutzt den Kudu-Prozess für „Build & Deployment“. 

## <a name="a-namecontentsyncahow-to-enable-content-sync-deployment"></a><a name="contentsync"></a>Aktivieren der Bereitstellung von Inhaltssynchronisierung
Um die Inhaltssynchronisierung aus dem [Azure-Portal](https://portal.azure.com)zu aktivieren, gehen Sie folgendermaßen vor:

1. Klicken Sie im Azure-Portal auf dem Blatt Ihrer App auf **Einstellungen** > **Bereitstellungsquelle**. Klicken Sie auf **Quelle auswählen**, und wählen Sie dann **OneDrive** oder **Dropbox** als Quelle für die Bereitstellung. 
   
    ![Inhaltssynchronisierung](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Infolge grundlegender Unterschiede bei den APIs wird **OneDrive for Business** derzeit nicht unterstützt. 
   > 
   > 
2. Schließen Sie den Authorisierungsworkflow zum Aktivieren von App Service ab, um auf einem bestimmten vordefinierten, gekennzeichneten Pfad für OneDrive oder Dropbox zuzugreifen, in dem alle Ihre App Service-Inhalte gespeichert werden.  
    Nach der Autorisierung gibt Ihnen die App Service-Plattform die Möglichkeit, einen Inhaltsordner unterhalb des festgelegten Inhaltspfads zu erstellen oder einen vorhandenen Inhaltsordner unter diesem festgelegten Inhaltspfad auszuwählen. Die festgelegten Inhaltspfade unter Ihren Cloudspeicherkonten, die für die Synchronisierung von App Service verwendet werden, sind die Folgenden:  
   
   * **OneDrive**: `Apps\Azure Web Apps` 
   * **Dropbox**: `Dropbox\Apps\Azure`
3. Nach der Erstsynchronisierung des Inhalts kann die Inhaltssynchronisierung vom Azure-Portal bei Bedarf initiiert werden. Der Bereitstellungsverlauf ist auf dem Blatt **Bereitstellungen** verfügbar.
   
    ![Bereitstellungsverlauf](./media/app-service-deploy-content-sync/onedrive_sync.png)

Weitere Informationen zur Dropbox-Bereitstellung finden Sie unter [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)(Bereitstellen von Dropbox). 


