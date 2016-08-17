<properties
   pageTitle="Continuous Integration für Service Fabric | Microsoft Azure"
   description="Hier erhalten Sie eine Übersicht über die Einrichtung der Continuous Integration für eine Service Fabric-Anwendung mit Visual Studio Team Services (VSTS)."
   services="service-fabric"
   documentationCenter="na"
   authors="mthalman-msft"
   manager="timlt"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/01/2016"
   ms.author="mthalman" />

# Einrichten der Continuous Integration für eine Service Fabric-Anwendung mit Visual Studio Team Services

In diesem Artikel werden die Schritte zum Einrichten der Continuous Integration (CI) für eine Azure Service Fabric-Anwendung mit Visual Studio Team Services (VSTS) beschrieben, um die automatische Erstellung, Verpackung und Bereitstellung Ihrer Anwendung sicherzustellen.

Dieses Dokument wurde für die aktuelle Prozedur erstellt und wird wahrscheinlich im Laufe der Zeit geändert.

## Voraussetzungen

Führen Sie zum Einstieg diese Schritte aus:

1. Stellen Sie sicher, dass Sie Zugriff auf ein Team Services-Konto haben, oder [erstellen Sie selbst eines](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

2. Stellen Sie sicher, dass Sie Zugriff auf ein Team Services-Teamprojekt haben, oder [erstellen Sie selbst eines](https://www.visualstudio.com/docs/setup-admin/create-team-project).

3. Vergewissern Sie sich, dass Sie über einen Service Fabric-Cluster verfügen, in dem Sie Ihre Anwendung bereitstellen können. Sie können auch einen im [Azure-Portal](service-fabric-cluster-creation-via-portal.md), über eine [Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) oder [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md) erstellen.

4. Stellen Sie sicher, dass Sie bereits ein Service Fabric-Anwendungsprojekt (.sfproj) erstellt haben. Sie müssen über ein Projekt verfügen, das mit Service Fabric SDK 2.1 oder höher erstellt oder aktualisiert wurde (die SFPROJ-Datei muss einen ProjectVersion-Eigenschaftswert von 1.1 oder höher aufweisen).

>[AZURE.NOTE] Benutzerdefinierte Build-Agents sind nicht mehr erforderlich. In Team Services gehostete Agents sind jetzt in der Service Fabric-Software für die Clusterverwaltung vorinstalliert, sodass Sie Ihre Anwendungen direkt aus diesen Agents bereitstellen können.

## Konfigurieren und Freigeben Ihrer Quelldateien

Sie müssen zunächst ein Veröffentlichungsprofil für den Bereitstellungsprozess vorbereiten, der in Team Services erfolgt. Das Veröffentlichungsprofil muss für den Cluster konfiguriert werden, den Sie zuvor erstellt haben:

1.	Wählen Sie ein Veröffentlichungsprofil in Ihrem Anwendungsprojekt, das Sie für Ihren Continuous Integration-Workflow verwenden möchten. Befolgen Sie die [Anweisungen](service-fabric-publish-app-remote-cluster.md) zum Veröffentlichen einer Anwendung in einem Remotecluster. Sie müssen allerdings Ihre Anwendung nicht tatsächlich veröffentlichen. Sie können im Dialogfeld „Veröffentlichen“ einfach auf den Link **Speichern** klicken, sobald Sie alles wie gewünscht konfiguriert haben.
2.	Wenn Sie möchten, dass für Ihre Anwendung bei jeder in Team Services stattfindenden Bereitstellung ein Upgrade erfolgt, müssen Sie das Veröffentlichungsprofil so konfigurieren, das Upgrades aktiviert sind. Stellen Sie im gleichen Dialogfeld „Veröffentlichen“, das im 1. Schritt verwendet wurde, sicher, dass das Kontrollkästchen **Upgrade der Anwendung ausführen** aktiviert ist. Erfahren Sie mehr zum [Konfigurieren zusätzlicher Upgradeeinstellungen](service-fabric-visualstudio-configure-upgrade.md). Klicken Sie auf den Link **Speichern**, um die Einstellungen im Veröffentlichungsprofil zu speichern.
3.	Stellen Sie sicher, dass Sie Ihre Änderungen im Veröffentlichungsprofil gespeichert haben, und brechen Sie das Dialogfeld „Veröffentlichen“ ab.
4.	Jetzt es Zeit ist, [die Quelldateien Ihres Anwendungsprojekt](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) für Team Services freizugeben. Sobald in Team Services auf Ihre Quelldateien zugegriffen werden kann, können Sie zum nächsten Schritt zum Erzeugen von Builds übergehen.

## Erstellen einer Builddefinition

Eine Team Services-Builddefinition beschreibt einen Workflow, der aus verschiedenen Buildschritten besteht, die nacheinander ausgeführt werden. Ziel der Builddefinition, die Sie erstellen, ist das Generieren eines Service Fabric-Anwendungspakets sowie einiger zusätzlicher Dateien, mit deren Hilfe die Anwendung schließlich in einem Cluster bereitgestellt werden kann. Erfahren Sie mehr zu Team Services-[Builddefinitionen](https://www.visualstudio.com/docs/build/define/create).

### Erstellen einer Definition aus der Buildvorlage

1.	Öffnen Sie Ihr Teamprojekt in Visual Studio Team Services.
2.	Wählen Sie die Registerkarte **Build** aus.
3.	Wählen Sie das grüne Pluszeichen (**+**) aus, um eine neue Builddefinition zu erstellen.
4.	Wählen Sie im eingeblendeten Dialogfeld in der Vorlagenkategorie **Build** den Eintrag **Azure Service Fabric-Anwendung** aus.
5.	Wählen Sie **Weiter**.
6.	Wählen Sie das Repository und die Branch aus, das/die mit Ihrer Service Fabric-Anwendung verknüpft ist.
7.	Wählen Sie die Agent-Warteschlange aus, die Sie verwenden möchten. Gehostete Agents werden unterstützt.
8.	Klicken Sie auf **Erstellen**.
9. Speichern Sie die Builddefinition, und benennen Sie sie.
10. Es folgt eine Beschreibung der Buildschritte, die von der Vorlage generiert werden:

| Buildschritt | Beschreibung |
| --- | --- |
| NuGet-Wiederherstellung | Stellt die NuGet-Pakete für die Projektmappe wieder her. |
| Projektmappe erstellen (SLN-Datei) | Erstellt die gesamte Projektmappe. |
| Projektmappe erstellen (SFPROJ-Datei) | Generiert das Service Fabric-Anwendungspaket an, das zum Bereitstellen der Anwendung verwendet wird. Beachten Sie, dass als Speicherort des Anwendungspakets das Buildverzeichnis für Artefakte angegeben ist. |
| Service Fabric-App-Versionen aktualisieren | Aktualisiert die Versionswerte in den Manifestdateien des Anwendungspakets, um Upgrades zu unterstützen. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Aufgabe](https://go.microsoft.com/fwlink/?LinkId=820529). |
| Dateien kopieren | Kopiert das Veröffentlichungsprofil und Anwendungsparameterdateien in die Artefakte des Builds, die für die Bereitstellung genutzt werden. |
| Artefakt veröffentlichen | Veröffentlicht die Artefakte des Builds. Dies ermöglicht einer Releasedefinition das Nutzen der Artefakte des Builds. |

### Überprüfen des Standardaufgabensatzes

1.	Überprüfen Sie im Eingabefeld **Projektmappe** die Buildschritte **NuGet-Wiederherstellung** und **Projektmappe erstellen**. Standardmäßig erfolgen diese Buildschritte für alle Projektmappendateien, die im dazugehörigen Repository enthalten sind. Wenn die Builddefinition nur für eine dieser Projektmappendateien verwendet werden soll, müssen Sie den Pfad zu dieser Datei explizit aktualisieren.
2.	Überprüfen Sie im Eingabefeld **Projektmappe** den Buildschritt **Anwendung packen**. Für diesen Buildschritt wird angenommen, dass im Repository nur ein Service Fabric-Anwendungsprojekt (.sfproj) vorhanden ist. Wenn Sie mehrere solcher Dateien in Ihrem Repository haben und nur eine davon für diese Builddefinition verwenden möchten, müssen Sie den Pfad zu dieser Datei explizit aktualisieren. Wenn Sie mehrere Anwendungsprojekte in Ihrem Repository packen möchten, müssen Sie zusätzliche **Visual Studio-Buildschritte** in der Builddefinition erstellen, die für jeweils ein bestimmtes Anwendungsprojekt gelten. Dann müssen Sie auch das Feld **MSBuild-Argumente** für jeden dieser Buildschritte aktualisieren, damit der Speicherort des Pakets für jeden Schritt eindeutig ist.
3.	Überprüfen Sie das Versionsverwaltungsverhalten, das im Buildschritt **Service Fabric-App-Versionen aktualisieren** definiert ist. Standardmäßig fügt dieser Buildschritt die Buildnummer an alle Versionswerte in den Manifestdateien des Anwendungspakets an. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Aufgabe](https://go.microsoft.com/fwlink/?LinkId=820529). Dies ist nützlich zur Unterstützung von Upgrades Ihrer Anwendung, da jede Upgradebereitstellung im Vergleich zur vorherigen Bereitstellung verschiedene Versionswerte erfordert. Wenn Sie nicht vorhaben, Anwendungsupgrades in Ihrem Workflow zu nutzen, können Sie diesen Buildschritt deaktivieren. Dieser Schritt muss sogar deaktiviert werden, wenn Sie einen Build erstellen möchten, der zum Überschreiben einer vorhandenen Service Fabric-Anwendung verwendet werden kann. In diesem Fall tritt bei der Bereitstellung ein Fehler auf, wenn die Version der vom Build erstellten Anwendung nicht der Version der Anwendung im Cluster entspricht.
4.	Wenn Ihre Projektmappe ein .NET Core-Projekt umfasst, müssen Sie sicherstellen, dass Ihre Builddefinition einen Buildschritt enthält, der die von beliebigen project.json-Dateien definierten Abhängigkeiten wiederherstellt. Gehen Sie dazu folgendermaßen vor:
   1. Wählen Sie **Buildschritt hinzufügen...** aus.
   2. Suchen Sie die Aufgabe **Befehlszeile** auf der Registerkarte „Hilfsprogramm“, und klicken Sie auf die zugehörige Schaltfläche „Hinzufügen“.
   3. Verwenden Sie folgende Werte für die Eingabefelder der Aufgabe:
      1. Tool: dotnet
      2. Argumente: restore
   4. Ziehen Sie die Aufgabe direkt hinter den Schritt **NuGet-Wiederherstellung**.
5.	Speichern Sie die Änderungen, die Sie an der Builddefinition vorgenommen haben.

### Ausprobieren

Wählen Sie **Build zur Warteschlange hinzufügen** aus, um einen Build manuell zu starten. Buildvorgänge werden auch beim Pushvorgang oder Einchecken ausgelöst. Nachdem Sie bestätigt haben, dass der Build erfolgreich ausgeführt wird, können Sie nun zum Definieren einer Releasedefinition übergehen, die zum Bereitstellen Ihrer Anwendung in einem Cluster dient.

## Erstellen einer Releasedefinition

Eine Team Services-Releasedefinition beschreibt einen Workflow, der aus verschiedenen Aufgaben besteht, die nacheinander ausgeführt werden. Ziel der Releasedefinition, die Sie erstellen, ist das Bereitstellen eines erstellten Anwendungspakets in einem Cluster. Bei gemeinsamer Verwendung können die Builddefinition und Releasedefinition den gesamten Workflow ausführen, und zwar beginnend mit den Quelldateien und endend mit einer im Cluster ausgeführten Anwendung. Erfahren Sie mehr zu Team Services-[Releasedefinitionen](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### Erstellen einer Definition aus der Releasevorlage

1.	Öffnen Sie Ihr Projekt in Visual Studio Team Services.
2.	Wählen Sie die Registerkarte **Release** aus.
3.	Um eine neue Releasedefinition zu erstellen, wählen Sie das grüne Pluszeichen **+** und dann im Menü die Option **Releasedefinition erstellen** aus.
4.	Wählen Sie im eingeblendeten Dialogfeld in der Vorlagenkategorie **Bereitstellung** den Eintrag **Azure Service Fabric-Bereitstellung** aus.
5.	Wählen Sie **Weiter**.
6.	Wählen Sie die Builddefinition aus, die Sie als Quelle für diese Releasedefinition verwenden möchten. Die Releasedefinition verweist auf die Artefakte, die von der ausgewählten Builddefinition erstellt wurden.
7.	Aktivieren Sie das Kontrollkästchen **Continuous Deployment**, wenn Team Services automatisch ein neues Release erstellen und die Service Fabric-Anwendung bereitstellen soll, wenn ein Build abgeschlossen ist.
8.	Wählen Sie die Agent-Warteschlange aus, die Sie verwenden möchten. Gehostete Agents werden unterstützt.
9.	Klicken Sie auf **Erstellen**.
10.	Bearbeiten Sie den Namen der Definition durch Klicken auf das Stiftsymbol oben auf der Seite.
11.	Wählen Sie im Eingabefeld **Clusterverbindung** der Aufgabe den Cluster aus, in dem die Anwendung bereitgestellt werden soll. Die Clusterverbindung enthält die erforderlichen Informationen, mit deren Hilfe die Bereitstellungsaufgabe eine Verbindung mit dem Cluster herstellen kann. Wenn noch keine Verbindung mit Ihrem Cluster vorhanden ist, wählen Sie den Link **Verwalten** neben dem Feld aus, um eine Verbindung hinzuzufügen. Führen Sie auf der eingeblendeten Seite die folgenden Schritte aus:
    1. Wählen Sie im Menü **Neuer Dienstendpunkt** und dann **Azure Service Fabric** aus.
    2. Wählen Sie den Typ der Authentifizierung für den Cluster aus, der von diesem Endpunkt genutzt wird.
    2. Geben Sie im Feld **Verbindungsname** einen Namen für die Verbindung ein. In der Regel verwenden Sie den Namen des Clusters.
    3. Definieren Sie im Feld **Clusterendpunkt** die URL für den Clientverbindungsendpunkt. Beispiel: https://contoso.westus.cloudapp.azure.com:19000.
    4. Wenn Sie Azure Active Directory-Anmeldeinformationen für die Herstellung der Verbindung mit dem Cluster verwenden möchten, geben Sie die entsprechenden Daten in den Feldern **Benutzername** und **Kennwort** ein.
    5. Für die zertifikatbasierte Authentifizierung definieren Sie im Feld **Clientzertifikat** die Base64-Codierung der Clientzertifikatdatei. In der kontextbezogenen Hilfe zu diesem Feld erfahren Sie, wie Sie diesen Wert abrufen. Wenn Ihr Zertifikat durch ein Kennwort geschützt ist, legen Sie das Kennwort im Feld **Kennwort** fest.
    6. Bestätigen Sie Ihre Änderungen durch Klicken auf **OK**. Nachdem Sie zu Ihrer Releasedefinition zurückgekehrt sind, klicken Sie im Feld **Clusterverbindung** auf das Aktualisierungssymbol, um den gerade hinzugefügten Endpunkt anzuzeigen.
12.	Speichern Sie die Releasedefinition.

Die erstellte Definition besteht aus einer einzigen Aufgabe: **Service Fabric-Anwendungsbereitstellung**. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Aufgabe](https://go.microsoft.com/fwlink/?LinkId=820528).

### Überprüfen der Standardeinstellungen der Vorlage

1.	Überprüfen Sie, ob im Eingabefeld **Veröffentlichungsprofil** die Aufgabe **Service Fabric-Anwendung bereitstellen** vorhanden ist. Standardmäßig verweist dieses Feld in den Artefakten des Builds auf ein Veröffentlichungsprofil mit dem Namen „Cloud.xml“. Wenn Sie auf ein anderes Veröffentlichungsprofil verweisen möchten oder der Build in seinen Artefakten mehrere Anwendungspakete enthält, müssen Sie den Pfad entsprechend ändern.
2.	Überprüfen Sie, ob im Eingabefeld **Anwendungspaket** die Aufgabe **Service Fabric-Anwendung bereitstellen** vorhanden ist. Standardmäßig wird auf den Pfad des Standardanwendungspakets verwiesen, der in der Builddefinitionsvorlage verwendet wurde. Wenn Sie den Pfad des Standardanwendungspakets in der Builddefinition geändert haben, müssen Sie den Pfad entsprechend aktualisieren.

### Ausprobieren

Wählen Sie im Menü **Release** den Befehl **Release erstellen**, um ein Release manuell zu erstellen. Wählen Sie im eingeblendeten Dialogfeld den Build aus, auf dem das Release basieren soll, und klicken Sie dann auf **Erstellen**. Wenn Sie Continuous Deployment aktiviert haben, werden Releases auch automatisch erstellt, wenn die zugeordnete Builddefinition einen Build abgeschlossen hat.

## Nächste Schritte

Weitere Informationen zu Continuous Integration für Service Fabric-Anwendungen finden Sie in den folgenden Artikeln:

 - [Startseite der Team Services-Dokumentation](https://www.visualstudio.com/docs/overview)
 - [Buildverwaltung in Team Services](https://www.visualstudio.com/docs/build/overview)
 - [Releaseverwaltung in Team Services](https://www.visualstudio.com/docs/release/overview)

<!---HONumber=AcomDC_0803_2016-->