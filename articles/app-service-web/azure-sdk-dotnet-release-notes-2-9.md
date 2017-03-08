---
title: "Versionshinweise zu Azure SDK für .NET 2.9"
description: "Versionshinweise zu Azure SDK für .NET 2.9"
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
ms.date: 02/24/2017
ms.author: juliako;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: 71c5a4f1b816fdcfe7ef267c20e6ed42e6cc51e6
ms.openlocfilehash: 3c3fb275a7c980f71a3a30e6875b9515321bad99
ms.lasthandoff: 03/01/2017


---
# <a name="azure-sdk-for-net-29-release-notes"></a>Versionshinweise zu Azure SDK für .NET 2.9

Dieses Thema enthält die Versionshinweise für Version 2.9 und 2.9.6 des Azure SDK für .NET.

##<a name="azure-sdk-for-net-296-release-summary"></a>Version 2.9.6 des Azure SDK für .NET – Zusammenfassung

Veröffentlichungsdatum: 16.11.2016
 
Mit dieser Version wurden keine grundlegenden Änderungen für Azure SDK 2.9 eingeführt. Es ist auch kein Upgradevorgang erforderlich, um dieses SDK mit vorhandenen Clouddienstprojekten nutzen zu können.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate

- In Visual Studio 2017 RC ist diese Version des Azure SDK für .NET in die Azure-Workload integriert. Alle Tools, die Sie für die Azure-Entwicklung benötigen, sind in Zukunft Teil von Visual Studio 2017 RC. Für Visual Studio 2015 und Visual Studio 2013 ist das SDK weiterhin per WebPI verfügbar. Versionen von Azure SDK für .NET werden für Visual Studio 2013 nicht mehr fortgeführt, nachdem Visual Studio 2017 als fertiges Produkt veröffentlicht wurde. Unter diesem Link können Sie Visual Studio 2017 RC herunterladen: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure-Diagnose

- Das Verhalten, bei dem die Verbindungszeichenfolge nur teilweise gespeichert und der Schlüssel durch ein Token für eine Cloud Services-Diagnosespeicher-Zeichenfolge ersetzt wurde, wurde geändert. Der eigentliche Speicherschlüssel wird jetzt im Benutzerprofilordner gespeichert, damit der Zugriff gesteuert werden kann. Visual Studio liest den Speicherschlüssel für den lokalen Debug- und Veröffentlichungsprozess aus dem Benutzerprofilordner aus. 
- Als Reaktion auf die oben beschriebene Änderung hat das Visual Studio Online-Team die Vorlage für die Azure Cloud Services-Bereitstellungsaufgabe verbessert. Benutzer können den Speicherschlüssel nun zum Festlegen der Diagnoseerweiterung angeben, wenn bei Continuous Integration und Continuous Deployment Veröffentlichungsvorgänge für Azure durchgeführt werden.
- Wir haben es möglich gemacht, eine sichere Verbindungszeichenfolge und Tokenisierung für Azure-Diagnose (WAD) zu speichern, damit Sie Probleme mit der Konfiguration umgebungsübergreifend lösen können.
 
### <a name="windows-server-2016-virtual-machines"></a>Virtuelle Windows Server 2016-Computer

- Visual Studio unterstützt jetzt die Bereitstellung von Cloud Services auf virtuellen Computern der Betriebssystemfamilie 5 (Windows Server 2016). Für vorhandene Clouddienste können Sie Ihre Einstellungen so ändern, dass sie für die neue Betriebssystemfamilie geeignet sind. Wenn Sie neue Clouddienste erstellen und .NET 4.6 oder höher verwenden, wird für den Dienst standardmäßig die Betriebssystemfamilie 5 genutzt.  Weitere Informationen finden Sie [hier](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/) in der Tabelle zur Unterstützung der Gastbetriebssystemfamilien.

#### <a name="known-issues"></a>Bekannte Probleme

- Mit dem Azure .NET SDK 2.9.6 wurde eine Einschränkung eingeführt, die die Bereitstellung von Projekten verhindert, die nicht unterstützte .NET Framework-Versionen (z.B. .NET 4.6) auf einer BS-Familie < 5 verwenden. Eine Problemumgehung wird [hier](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) bereitgestellt.

 
### <a name="azure-in-role-cache"></a>Azure In-Role Cache 

- Die Unterstützung für Azure In-Role Cache endet am 30. November 2016. Nähere Informationen finden Sie [hier](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure Resource Manager-Vorlagen für Azure Stack

- Wir haben Azure Stack als Bereitstellungsziel für Ihre Azure Resource Manager-Vorlagen eingeführt.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK für .NET 2.9 – Zusammenfassung

## <a name="overview"></a>Übersicht
Dieses Dokument enthält die Versionshinweise für das Azure SDK für .NET 2.9. 

Ausführliche Informationen zu Updates in dieser Version finden Sie im [Ankündigungsbeitrag zu Azure SDK 2.9](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 für Visual Studio 2015 Update 2 und Visual Studio „15“-Vorschau
Dieses Update umfasst die folgenden Fehlerbehebungen:

* Problem bei der Generierung des REST-API-Clients, bei dem die Zeichenfolge „Unbekannter Typ“ als Name des Ordners zur Codegenerierung und/oder als Name des im generierten Code eingefügten Namespace angezeigt wurde.
* Problem im Zusammenhang mit geplanten WebJobs, bei dem die Authentifizierungsinformationen nicht an den Vorgang zur Bereitstellung von Scheduler übergeben wurden.

Dieses Update umfasst die folgende neue Funktion:

* Unterstützung für sekundäre App Services auf der Registerkarte „Dienste“ des Dialogfelds zur Bereitstellung von App Services. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake Tools für Visual Studio 2015 Update 2
Dieses Update umfasst Folgendes:

* **Azure Data Lake Tools** für Visual Studio ist jetzt im Azure SDK für .NET enthalten. Das Tool wird beim Installieren von Azure SDK automatisch installiert. 
  
    Das Tool wird regelmäßig aktualisiert, rufen Sie die Updates daher [hier](http://aka.ms/datalaketool) ab.
* **Server-Explorer** können Sie nun alle U-SQL-Metadatenentitäten anzeigen und einige U-SQL-Metadatenentitäten erstellen. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/documentation/services/data-lake-analytics/) Blog.

## <a name="hdinsight-tools"></a>HDInsight-Tools
**HDInsight Tools** für Visual Studio unterstützt jetzt HDInsight Version 3.3, einschließlich der Anzeige von Tez-Diagrammen und anderen Sprachkorrekturen.

## <a name="azure-resource-manager"></a>Azure-Ressourcen-Manager
Neu in dieser Version ist die [Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md)-Unterstützung für Resource Manager-Vorlagen.

## <a name="see-also"></a>Siehe auch
[Ankündigungsbeitrag zu Azure SDK 2.9 (in englischer Sprache)](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)


