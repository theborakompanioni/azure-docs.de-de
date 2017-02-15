---
title: "Continuous Delivery für Clouddienste mit Visual Studio Online | Microsoft Docs"
description: "Es wird beschrieben, wie Sie Continuous Delivery für Azure-Cloud-Apps einrichten, ohne den Diagnosespeicherschlüssel in den Dienstkonfigurationsdateien zu speichern."
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: 165ab7363efaf90eaab41098f71e2f1b846c346e
ms.openlocfilehash: 7e70f92d4d1333ca6cbac5876e5ccbc763bd915c

---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a>Sicheres Speichern des Cloud Services-Diagnosespeicherschlüssels und Einrichten von Continuous Integration und Deployment in Azure per Visual Studio Online
 Diese Vorgehensweise wird heutzutage für Open-Source-Projekte häufig verwendet. Das Speichern von Anwendungsgeheimnissen in Konfigurationsdateien ist kein sicherer Ansatz mehr, da Sicherheitsrisiken bestehen, bei denen Geheimnisse aus öffentlichen Quellcodeverwaltungen entwendet werden. Das Speichern von Geheimnissen als Klartext in einer Datei in einer Continuous Integration-Pipeline ist ebenfalls nicht sicher, da es sich bei Buildservern um gemeinsame Ressourcen in der Cloudumgebung handeln kann. In diesem Artikel wird beschrieben, wie die Sicherheitsbedenken während der Entwicklung und des Continuous Integration-Prozesses mit Visual Studio und Visual Studio Online ausgeräumt werden können.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Entfernen des Diagnosespeicherschlüssel-Geheimnisses in der Projektkonfigurationsdatei
Für die Cloud Services-Diagnoseerweiterung ist Azure-Speicher zum Speichern der Diagnoseergebnisse erforderlich. Bisher wurde die Speicherverbindungszeichenfolge in den Cloud Services-Konfigurationsdateien (.cscfg) angegeben und konnte in die Quellcodeverwaltung eingecheckt werden. In der aktuellen Azure SDK-Version haben wir das Verhalten geändert, bei dem nur ein Teil der Verbindungszeichenfolge gespeichert und der Schlüssel durch ein Token ersetzt wurde. Mit den folgenden Schritten wird beschrieben, wie die neuen Cloud Services-Tools funktionieren:

### <a name="1-open-the-role-designer"></a>1. Öffnen des Rollendesigners
* Doppelklicken Sie auf eine Cloud Services-Rolle (bzw. klicken Sie mit der rechten Maustaste), um den Rollendesigner zu öffnen.

![Öffnen des Rollendesigners][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. Im Abschnitt „Diagnose“ wird das neue Kontrollkästchen „Don’t remove storage key secret from project“ (Speicherschlüsselgeheimnis nicht aus Projekt entfernen) hinzugefügt.
* Wenn Sie den lokalen Speicheremulator verwenden, ist dieses Kontrollkästchen deaktiviert, da für die lokale Verbindungszeichenfolge kein Geheimnis verwaltet werden muss (UseDevelopmentStorage=true).

![Verbindungszeichenfolge für lokalen Speicheremulator ist nicht geheim][1]

* Wenn Sie ein neues Projekt erstellen, ist dieses Kontrollkästchen standardmäßig deaktiviert. Dies führt dazu, dass der Speicherschlüsselabschnitt der ausgewählten Speicherverbindungszeichenfolge durch ein Token ersetzt wird. Den Wert des Tokens finden Sie im AppData-Roaming-Ordner des aktuellen Benutzers, z.B. „C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService“.

> Beachten Sie, dass der Ordner „user\AppData“ über die Benutzeranmeldung der Zugriffssteuerung unterliegt und als sicherer Ort zum Speichern von Entwicklungsgeheimnissen angesehen wird.
> 
> 

![Der Speicherschlüssel wird im Benutzerprofilordner gespeichert.][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Auswählen eines Diagnosespeicherkontos
* Wählen Sie ein Speicherkonto in dem Dialogfeld aus, das durch Klicken auf die Schaltfläche „…“ geöffnet wird . Beachten Sie, dass die generierte Speicherverbindungszeichenfolge den Speicherkontoschlüssel nicht enthält.
* Beispiel: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)

### <a name="4----debugging-the-project"></a>4.    Debuggen des Projekts
* Drücken Sie F5, um das Debuggen in Visual Studio zu starten. Alles sollte wie zuvor funktionieren.
  ![Starten des lokalen Debuggens][3]

### <a name="5-publish-project-from-visual-studio"></a>5. Veröffentlichen des Projekts aus Visual Studio
* Starten Sie das Veröffentlichungsdialogfeld, und führen Sie die Anmeldungsschritte aus, um die Anwendung in Azure zu veröffentlichen.

### <a name="6-additional-information"></a>6. Zusätzliche Informationen
> Hinweis: Der Bereich „Einstellungen“ im Rollendesigner bleibt vorerst unverändert. Verwenden Sie die Registerkarte „Konfigurationen“, wenn Sie die Geheimnisverwaltungsfunktion für die Diagnose verwenden möchten.
> 
> 

![Hinzufügen von Einstellungen][4]

> Hinweis: Wenn er aktiviert ist, wird der Application Insights-Schlüssel als Klartext gespeichert. Der Schlüssel wird nur zum Hochladen von Inhalten verwendet, sodass kein Risiko besteht, dass vertrauliche Daten kompromittiert werden.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Erstellen und Veröffentlichen eines Cloud Services-Projekts mit Visual Studio Online-Aufgabenvorlagen
* Die folgenden Schritte zeigen, wie Sie Continuous Integration für ein Cloud Services-Projekt mit Visual Studio Online-Aufgaben einrichten:
  ### <a name="1----obtain-a-vso-account"></a>1.    Beschaffen eines VSO-Kontos
* [Erstellen Sie ein Visual Studio Online-Konto][Erstellen eines Visual Studio Online-Kontos], falls Sie noch kein Konto dieser Art besitzen.
* [Erstellen Sie ein Teamprojekt][Erstellen eines Teamprojekts] in Ihrem Visual Studio Online-Konto.

### <a name="2----setup-source-control-in-visual-studio"></a>2.    Einrichten der Quellcodeverwaltung in Visual Studio
* Stellen Sie eine Verbindung mit einem Teamprojekt her.

![Herstellen einer Verbindung mit einem Teamprojekt][5]

![Auswählen des gewünschten Teamprojekts][6]

* Fügen Sie das Projekt der Quellcodeverwaltung hinzu.

![Hinzufügen des Projekts zur Quellcodeverwaltung][7]

![Zuordnen des Projekts zu einem Ordner der Quellcodeverwaltung][8]

* Checken Sie das Projekt über Team Explorer ein.

![Einchecken des Projekts in die Quellcodeverwaltung][9]

### <a name="3----configure-build-process"></a>3.    Konfigurieren des Buildprozesses
* Navigieren Sie zu Ihrem Teamprojekt, und fügen Sie eine „Neue Buildprozessvorlage“ hinzu.

![Hinzufügen eines neuen Builds][10]

* Wählen Sie die Buildaufgabe aus.

![Hinzufügen einer Buildaufgabe][11]

![Auswählen einer Visual Studio-Build-Aufgabenvorlage][12]

* Bearbeiten Sie die Eingabe für die Buildaufgabe. Passen Sie die Buildparameter gemäß Ihren Anforderungen an.

![Konfigurieren der Buildaufgabe][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Konfigurieren Sie die Buildvariablen.

![Konfigurieren von Buildvariablen][14]

* Fügen Sie eine Aufgabe zum Hochladen der Buildablage hinzu.

![Auswählen der Option zum Veröffentlichen der Buildablageaufgabe][15]

![Konfigurieren der Option zum Veröffentlichen der Buildablageaufgabe][16]

* Führen Sie den Buildvorgang durch.

![Neuen Build in Warteschlange][17]

![Anzeigen der Buildzusammenfassung][18]

* Wenn der Buildvorgang erfolgreich war, wird ein Ergebnis angezeigt, das der Abbildung unten ähnelt.

![Buildergebnis][19]

### <a name="4----configure-release-process"></a>4.    Konfigurieren des Freigabeprozesses
* Erstellen Sie eine neue Freigabeversion.

![Erstellen einer neuen Freigabeversion][20]

* Wählen Sie die Azure Cloud Services-Bereitstellungsaufgabe aus.

![Auswählen der Azure Cloud Services-Bereitstellungsaufgabe][21]

* Da der Speicherkontoschlüssel nicht in die Quellcodeverwaltung eingecheckt wird, müssen wir den geheimen Schlüssel zum Festlegen der Diagnoseerweiterungen angeben. Erweitern Sie den Abschnitt **Erweiterte Optionen für das Erstellen des neuen Diensts**, und bearbeiten Sie die Parametereingabe für **Diagnosespeicher-Kontoschlüssel**. Für diese Eingabe sind mehrere Zeilen für das Schlüssel-Wert-Paar im Format **[RoleName]:$(StorageAccountKey)** erforderlich.

> Hinweis: Wenn sich Ihr Diagnosespeicherkonto unter demselben Abonnement befindet, unter dem Sie auch die Cloud Services-Anwendung veröffentlichen, müssen Sie den Schlüssel nicht in den Eingabedaten für die Bereitstellungsaufgabe eingeben. Im Rahmen der Bereitstellung werden die Speicherinformationen aus Ihrem Abonnement abgerufen.
> 
> 

![Konfigurieren der Cloud Services-Bereitstellungsaufgabe][22]

* Verwenden Sie geheime Buildvariablen, um Speicherschlüssel zu speichern. Klicken Sie zum Maskieren einer Variablen als Geheimnis rechts vom Bereich für die Variableneingabe auf das Sperrsymbol.

![Speichern von Speicherschlüsseln in geheimen Buildvariablen][23]

* Erstellen Sie eine neue Freigabeversion, und stellen Sie Ihr Projekt in Azure bereit.

![Erstellen einer neuen Freigabeversion][24]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Festlegen von Diagnoseerweiterungen für Azure Cloud Services finden Sie unter [Aktivieren der Diagnose mithilfe von PowerShell in Azure Cloud Services][Aktivieren der Diagnose mithilfe von PowerShell in Azure Cloud Services].

[Erstellen eines Visual Studio Online-Kontos]:https://www.visualstudio.com/team-services/
[Erstellen eines Teamprojekts]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Aktivieren der Diagnose mithilfe von PowerShell in Azure Cloud Services]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png



<!--HONumber=Nov16_HO3-->


