---
title: "Versionshinweise zu Azure SDK für .NET 3.0 | Microsoft-Dokumentation"
description: "Versionshinweise zu Azure SDK für .NET 3.0"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: dea4a174aaf3727d66e9d69d32d433ff24e0d06d
ms.lasthandoff: 03/22/2017


---
# <a name="azure-sdk-for-net-30-release-notes"></a>Versionshinweise zu Azure SDK für .NET 3.0

Dieses Thema enthält die Versionshinweise für Version 3.0 des Azure SDK für .NET.

##<a name="azure-sdk-for-net-30-release-summary"></a>Version 3.0 des Azure SDK für .NET – Zusammenfassung

Herausgabedatum: 07.03.2017
 
Mit dieser Version wurden keine grundlegenden Änderungen für das Azure SDK 3.0 eingeführt. Es ist auch kein Upgradevorgang erforderlich, um dieses SDK mit vorhandenen Clouddienstprojekten nutzen zu können. Um die Verwendung des Azure SDK 3.0 ohne einen Upgradevorgang zu ermöglichen, wird das Azure SDK 3.0 in dieselben Verzeichnisse wie das Azure SDK 2.9 installiert. Bei den meisten Komponenten wurde die Hauptversion seit 2.9 nicht geändert, sondern lediglich die Buildnummer aktualisiert.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- In Visual Studio 2017 ist diese Version des Azure SDK für .NET in die Azure-Workload integriert. Alle Tools, die Sie für die Azure-Entwicklung benötigen, sind in Zukunft Teil von Visual Studio 2017. Für Visual Studio 2015 ist das SDK weiterhin per WebPI verfügbar. Mit der Herausgabe von Visual Studio 2017 werden die Versionen des Azure SDK für .NET für Visual Studio 2013 nicht mehr fortgeführt.

### <a name="azure-diagnostics"></a>Azure-Diagnose

- Das Verhalten, bei dem die Verbindungszeichenfolge nur teilweise gespeichert und der Schlüssel durch ein Token für eine Cloud Services-Diagnosespeicher-Zeichenfolge ersetzt wurde, wurde geändert. Der eigentliche Speicherschlüssel wird jetzt im Benutzerprofilordner gespeichert, damit der Zugriff gesteuert werden kann. Visual Studio liest den Speicherschlüssel für den lokalen Debug- und Veröffentlichungsprozess aus dem Benutzerprofilordner aus. 
- Als Reaktion auf die oben beschriebene Änderung hat das Visual Studio Online-Team die Vorlage für die Azure Cloud Services-Bereitstellungsaufgabe verbessert. Benutzer können den Speicherschlüssel nun zum Festlegen der Diagnoseerweiterung angeben, wenn bei Continuous Integration und Continuous Deployment Veröffentlichungsvorgänge für Azure durchgeführt werden.
- Wir haben es möglich gemacht, eine sichere Verbindungszeichenfolge und Tokenisierung für Azure-Diagnose (WAD) zu speichern, damit Sie Probleme mit der Konfiguration umgebungsübergreifend lösen können.
 
### <a name="windows-server-2016-virtual-machines"></a>Virtuelle Windows Server 2016-Computer

- Visual Studio unterstützt jetzt die Bereitstellung von Cloud Services auf virtuellen Computern der Betriebssystemfamilie 5 (Windows Server 2016). Für vorhandene Clouddienste können Sie Ihre Einstellungen so ändern, dass sie für die neue Betriebssystemfamilie geeignet sind. Wenn Sie neue Clouddienste erstellen und .NET 4.6 oder höher verwenden, wird für den Dienst standardmäßig die Betriebssystemfamilie 5 genutzt.  Weitere Informationen finden Sie [hier](../cloud-services/cloud-services-guestos-update-matrix.md) in der Tabelle zur Unterstützung der Gastbetriebssystemfamilien.

### <a name="known-issues"></a>Bekannte Probleme

- Durch das Azure .NET SDK 3.0 wurde ein Problem beim Entfernen von Visual Studio 2017 in einer parallelen Konfiguration mit Visual Studio 2015 eingeführt.  Wenn Sie das Azure SDK für Visual Studio 2015 installiert haben, werden Microsoft Azure-Speicheremulator und Microsoft Azure-Serveremulator entfernt, wenn Sie Visual Studio 2017 deinstallieren.  Dies erzeugt einen Fehler beim Erstellen und Debuggen von neuen Clouddienstprojekten in Visual Studio 2015. Um dieses Problem zu beheben, müssen Sie das Azure SDK erneut über den Webplattform-Installer installieren.  Das Problem wird in einem zukünftigen Update von Visual Studio 2017 gelöst.  beschrieben.

 
### <a name="azure-in-role-cache"></a>Azure In-Role Cache 

- Die Unterstützung für Azure In-Role Cache hat am 30. November 2016 geendet. Nähere Informationen finden Sie [hier](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).





