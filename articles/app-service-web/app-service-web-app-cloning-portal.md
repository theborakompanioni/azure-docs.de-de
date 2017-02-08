---
title: "Klonen von Web-App über das Azure-Portal"
description: Erfahren Sie, wie Sie Ihre Web-Apps zu neuen Web-Apps mithilfe von Azure-Portal klonen.
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
translationtype: Human Translation
ms.sourcegitcommit: 6b77e338e1c7f0f79ea3c25b0b073296f7de0dcf
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b


---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Klonen der Azure App Service-App über das Azure-Portal
Die Klonfunktion in [Azure App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714) hilft Ihnen problemlos dabei, bereits existierender Web-Apps zu klonen und die neu erstellte App in einer anderen Region oder in derselben Region bereitzustellen. Dadurch können Kunden eine Anzahl von Apps in unterschiedlichen Regionen bereitstellen.

Das Klonen einer App wird zurzeit nur im Premium-Tarif der App Service-Pläne unterstützt. Das neue Feature verwendet die gleichen Einschränkungen wie das Web-Apps-Sicherungsfeature. Informationen dazu finden Sie unter [Sichern von Web-Apps in Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen App
Die Web-App muss im **Premium** -Modus ausgeführt werden, damit Sie einen Klon für die Web-App erstellen können.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/)das Blatt Ihrer Web-App.
2. Klicken Sie auf **Tools**. Klicken Sie auf dem Blatt **Extras** auf **App klonen**.
   
    ![][1]
   
   > [!NOTE]
   > Wenn die Web-App nicht bereits im **Premium** -Modus ausgeführt wird, wird eine Meldung angezeigt, in der auf die unterstützten Modi für das Klonen von Apps hingewiesen wird. An diesem Punkt haben Sie die Option **Upgrade**auszuwählen.
   > 
   > 
3. Im Blatt **Clone App** geben Sie einen Namen für die neue Web-App an sowie für die Ressourcengruppe und den App Service-Plan. Die Benutzer können außerdem auswählen, ob eine Anzahl von Web-App-Quelleinstellungen geklont werden sollen oder nicht.
   
    ![][2]
4. Nach Klicken auf **Erstellen** fängt die Plattform an, einen Klon der Quellen-Web-App zu erstellen.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen App in einer App Service-Umgebung
Im Blatt **Klon App** hat der Kunde hat die Möglichkeit, einen App-Pool in einer vorhandenen App Service-Umgebung auszuwählen.

## <a name="current-restrictions"></a>Aktuelle Einschränkungen
Diese Funktion wird zurzeit als Vorschau angezeigt. Wir arbeiten daran, neue Funktionen im Laufe der Zeit hinzuzufügen. In der folgenden Liste sind die bekannten Einschränkung für die aktuelle Version des Klonens einer App in Azure-Portal aufgeführt:

* Azure Traffic Manager-Einstellungen werden nicht geklont.
* Einstellungen für automatische Skalierung werden nicht geklont
* Einstellungen des Sicherungszeitplans werden nicht geklont
* VNET-Einstellungen werden nicht geklont
* App Insights werden nicht automatisch auf der Ziel-Web-App eingerichtet
* Easy Auth-Einstellungen werden nicht geklont
* Kudu-Erweiterungen werden nicht geklont
* TiP-Regeln werden nicht geklont
* Datenbankinhalte werden nicht geklont

### <a name="references"></a>Referenzen
* [Klonen von Web-App mithilfe von PowerShell](app-service-web-app-cloning.md)
* [Sichern von Web-Apps in Azure App Service](web-sites-backup.md)
* [Erstellen einer App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md)
* [Erstellen einer Web-App in einer App Service-Umgebung](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png



<!--HONumber=Feb17_HO1-->


