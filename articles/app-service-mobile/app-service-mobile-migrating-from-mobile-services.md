---
title: Migrieren von Mobile Services in eine mobile App Service-App
description: "Erfahren Sie, wie einfach Sie Ihre Mobile Services-Anwendung in eine mobile App Service-App migrieren können."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 06e85d02d82bcc7d57989c1dce97028b24b9f317
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="article-top"></a>Migrieren des vorhandenen Azure Mobile Service zu Azure App Service
Mit der [allgemeinen Verfügbarkeit von Azure App Service]können Azure Mobile Services-Websites problemlos direkt migriert werden, um alle Features von Azure App Service zu nutzen.  Dieses Dokument erläutert, was Sie erwarten können, wenn Sie Ihre Website von Azure Mobile Services zu Azure App Service migrieren.

## <a name="what-does-migration-do"></a>Auswirkungen der Migration auf Ihre Website
Die Migration Ihres Azure Mobile Service wandelt den Mobile Service in eine [Azure App Service]-App um, ohne den Code zu beeinträchtigen.  Ihre Notification Hubs, die SQL-Datenverbindung, Authentifizierungseinstellungen, geplanten Aufträge und Domänennamen bleiben unverändert.  Mobile Clients, die Ihren Azure Mobile Service verwenden, werden weiterhin normal ausgeführt.  Die Migration startet Ihren Dienst neu, sobald er an Azure App Service übertragen wurde.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Gründe zur Migration Ihrer Website
Microsoft empfiehlt Ihnen, Ihren Azure Mobile Service zu migrieren, um die Features von Azure App Service zu nutzen, einschließlich:

* Neue Hostfeatures, einschließlich [WebJobs] und [benutzerdefinierten Domänennamen].
* Überwachung und Problembehandlung mit New Relic oder [Application Insights].
* Integrierte DevOps-Tools, einschließlich [Stagingslots], Rollback und Test in der Produktion.
* [Automatische Skalierung], Lastenausgleich und [Leistungsüberwachung].

Weitere Informationen zu den Vorteilen von Azure App Service finden Sie im Thema [Mobile Services im Vergleich zu App Service].

## <a name="before-you-begin"></a>Voraussetzungen
Bevor Sie mit größeren Arbeiten an Ihrer Website beginnen, sollten Sie Ihre Mobile Service-Skripts und Ihre SQL-Datenbank sichern.

## <a name="migrating-site"></a>Migrieren Ihrer Websites
Bei der Migration werden alle Websites innerhalb einer einzelnen Azure-Region migriert.

So migrieren Sie Ihre Website:

1. Melden Sie sich beim [klassischen Azure-Portal] an.
2. Wählen Sie einen Mobile Service in der Region aus, die Sie migrieren möchten.
3. Klicken Sie auf die Schaltfläche **Zu App Service migrieren**.

   ![Die Schaltfläche „Migrieren“][0]
4. Lesen Sie den Inhalt des App Service-Dialogfelds.
5. Geben Sie den Namen Ihres Mobile Service in das dafür vorgesehene Feld ein.  Wenn Ihr Domänenname beispielsweise „contoso.azure mobile.net“ ist, geben Sie *contoso* in das dafür vorgesehene Feld ein.
6. Klicken Sie auf die Schaltfläche mit dem Häkchen.

Überwachen Sie den Status der Migration im Aktivitätsmonitor. Im klassischen Azure-Portal wird Ihre Website mit dem Status *Migration wird ausgeführt* aufgeführt.

  ![Migrationsaktivitätsmonitor][1]

Jede Migration kann pro Mobile Service, der migriert wird, zwischen 3 und 15 Minuten dauern.  Ihre Website bleibt während der Migration verfügbar.
Am Ende des Migrationsprozesses wird die Website neu gestartet.  Die Website steht während des Neustarts, der ein paar Sekunden dauern kann, nicht zur Verfügung.

## <a name="finalizing-migration"></a>Abschließen der Migration
Planen Sie ein, Ihre Website nach Abschluss des Migrationsvorgangs von einem mobilen Client aus zu testen.  Stellen Sie sicher, dass Sie alle allgemeinen Clientaktionen ohne Änderungen am mobilen Client ausführen können.  

### <a name="update-app-service-tier"></a>Auswählen eines entsprechenden App Service-Tarifs
Nach der Migration zu Azure App Service können Sie Preise flexibler gestalten.

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im Menü „Einstellungen“ auf **App Service-Plan**.
5. Klicken Sie auf die Kachel **Tarif**.
6. Klicken Sie auf die für Ihre Anforderungen geeignete Kachel und dann auf **Auswählen**.  Unter Umständen müssen Sie auf **Alle anzeigen** klicken, um die verfügbaren Tarife zu sehen.

Als Ausgangspunkt empfehlen wir folgende Tarife:

| Mobile Service-Tarif | App Service-Tarif |
|:--- |:--- |
| Free |F1 Free |
| Basic |B1 Basic |
| Standard |S1 Standard |

Sie können bei der Auswahl des richtigen Tarifs für Ihre Anwendung sehr flexibel vorgehen.  Unter [App Service – Preise] finden Sie umfassende Informationen zu den Preisen Ihres neuen App Service.

> [!TIP]
> Der App Service-Tarif Standard umfasst den Zugriff auf viele Features, die unter Umständen nützlich für Sie sein können, z.B. [Stagingslots], automatische Sicherungen und automatische Skalierung.  Informieren Sie sich dort ausführlich über die neuen Funktionen!
>
>

### <a name="review-migration-scheduler-jobs"></a>Überprüfen der migrierten Scheduler-Aufträge
Scheduler-Aufträge sind erst ca. 30 Minuten nach der Migration sichtbar.  Geplante Aufträge werden weiterhin im Hintergrund ausgeführt.
Gehen Sie wie folgt vor, um geplante Aufträge anzuzeigen, wenn sie wieder sichtbar sind:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Durchsuchen >**, geben Sie **Zeitplan** in das Feld *Filter* ein, und wählen Sie die Option **Scheduler-Sammlungen**.

Nach der Migration ist eine begrenzte Anzahl von freien Scheduler-Aufträgen verfügbar.  Überprüfen Sie Ihre Verwendung und die [Azure Scheduler-Pläne].

### <a name="configure-cors"></a>Konfigurieren von CORS bei Bedarf
Ressourcenfreigabe zwischen verschiedenen Ursprüngen ist ein Verfahren, um einer Website den Zugriff auf eine Web-API in einer anderen Domäne zu ermöglichen.  Wenn Sie Azure Mobile Services mit einer zugeordneten Website verwendet haben, müssen Sie im Rahmen der Migration CORS konfigurieren.  Wenn Sie ausschließlich über mobile Geräte auf Azure Mobile Services zugreifen, muss CORS nur in seltenen Fällen konfiguriert werden.

Die migrierten Einstellungen für CORS stehen als App-Einstellung **MS_CrossDomainWhitelist** zur Verfügung.  So migrieren Sie Ihre Website zum App Service-CORS-Raum:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im API-Menü auf **CORS**.
5. Geben Sie alle zulässigen Ursprünge im entsprechenden Feld ein, und drücken Sie nach jeder Eingabe die EINGABETASTE.
6. Sobald die Liste mit den zulässigen Ursprüngen vollständig ist, können Sie auf die Schaltfläche „Speichern“ klicken.

> [!TIP]
> Ein Vorteil der Verwendung von Azure App Service ist, dass Sie Ihre Website und den Mobile Service auf der gleichen Website ausführen können.  Weitere Informationen finden Sie im Abschnitt [Nächste Schritte](#next-steps).
>
>

### <a name="download-publish-profile"></a>Herunterladen eines neuen Veröffentlichungsprofils
Beim der Migration zu Azure App Service wird das Veröffentlichungsprofil der Website geändert.  Wenn Sie Ihre Website aus Visual Studio heraus veröffentlichen möchten, benötigen Sie ein neues Veröffentlichungsprofil.  So laden Sie das neue Veröffentlichungsprofil herunter:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Klicken Sie auf **Veröffentlichungsprofil abrufen**.

Die PublishSettings-Datei wird auf Ihren Computer heruntergeladen.  Sie heißt normalerweise „*Websitename*.PublishSettings“.  Importieren Sie die Veröffentlichungseinstellungen in Ihr vorhandenes Projekt:

1. Öffnen Sie Visual Studio und Ihr Azure Mobile Service-Projekt.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...**.
3. Klicken Sie auf **Importieren**.
4. Klicken Sie auf **Durchsuchen**, und wählen Sie die heruntergeladene Datei mit den Veröffentlichungseinstellungen aus.  Klicken Sie auf **OK**
5. Klicken Sie auf **Verbindung überprüfen**, um sicherzustellen, dass die Veröffentlichungseinstellungen funktionieren.
6. Klicken Sie auf **Veröffentlichen**, um die Website zu veröffentlichen.

## <a name="working-with-your-site"></a>Arbeiten mit Ihrer Website nach der Migration
Nach der Migration können Sie im [Azure-Portal] mit Ihrem neuen App Service arbeiten.  Es folgen einige Hinweise zu bestimmten Vorgängen, die Sie im [klassischen Azure-Portal]ausgeführt haben, mit ihren App Service-Entsprechungen.

### <a name="publishing-your-site"></a>Herunterladen und Veröffentlichen Ihrer migrierten Website
Ihre Website ist über Git oder FTP verfügbar und kann mit verschiedenen Mechanismen, einschließlich WebDeploy, TFS, Mercurial, GitHub und FTP, erneut veröffentlicht werden.  Anmeldeinformationen für die Bereitstellung werden mit dem Rest der Website migriert.  Wenn Sie keine Anmeldeinformationen für die Bereitstellung festgelegt haben, oder sich nicht mehr an sie erinnern, können Sie diese zurücksetzen:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im Menü „VERÖFFENTLICHUNG“ auf **Anmeldeinformationen für die Bereitstellung**.
5. Geben Sie die neuen Anmeldeinformationen für die Bereitstellung in die entsprechenden Felder ein, und klicken Sie auf die Schaltfläche „Speichern“.

Mit diesen Anmeldeinformationen können Sie die Website mit Git klonen, oder Sie können automatische Bereitstellungen von GitHub, TFS oder Mercurial aus einrichten.  Weitere Informationen finden Sie in der [Dokumentation zur Azure App Service-Bereitstellung].

### <a name="appsettings"></a>Anwendungseinstellungen
Die meisten Einstellungen für einen migrierten Mobile Service sind über App-Einstellungen verfügbar.  Sie können eine Übersicht der App-Einstellungen im [Azure-Portal] abrufen.
So zeigen Sie Ihre App-Einstellungen an oder ändern sie:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im Menü „Allgemein“ auf **Anwendungseinstellungen** .
5. Scrollen Sie zum Abschnitt „App-Einstellungen“, und suchen Sie Ihre App-Einstellung.
6. Klicken Sie auf den Wert der App-Einstellung, um ihn zu bearbeiten.  Klicken Sie auf **Speichern**, um den Wert zu speichern.

Sie können gleichzeitig mehrere App-Einstellungen aktualisieren.

> [!TIP]
> Zwei Anwendungseinstellungen haben den gleichen Wert.  Beispielsweise werden *ApplicationKey* und *MS\_ApplicationKey* angezeigt.  Aktualisieren Sie beide Anwendungseinstellungen gleichzeitig.
>
>

### <a name="authentication"></a>Authentifizierung
Alle Authentifizierungseinstellungen sind auf der migrierten Website als App-Einstellungen verfügbar.  Um Ihre Authentifizierungseinstellungen zu aktualisieren, müssen Sie die entsprechenden App-Einstellungen ändern.  Die folgende Tabelle zeigt die entsprechenden App-Einstellungen für Ihren Authentifizierungsanbieter:

| Anbieter | Client-ID | Geheimer Clientschlüssel | Andere Einstellungen |
|:--- |:--- |:--- |:--- |
| Microsoft Account |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Hinweis: **MS\_AadTenants** wird als eine durch Trennzeichen getrennte Liste von Mandantendomänen gespeichert (die Felder „Zulässige Mandanten“ im Mobile Services-Portal).

> [!WARNING]
> **Verwenden Sie nicht die Authentifizierungsmechanismen im Menü „Einstellungen“**
>
> Azure App Service bietet ein separates Authentifizierungs- und Autorisierungssystem „ohne Code“ in den Einstellungen für *Authentifizierung/Autorisierung* und die (veraltete) Option *Mobile Authentifizierung* im Menü „Einstellungen“.  Diese Optionen sind nicht kompatibel mit einem migrierten Azure Mobile Service.  Sie können [Ihre Website aktualisieren](app-service-mobile-net-upgrading-from-mobile-services.md) , um die Azure App Service-Authentifizierung zu nutzen.
>
>

### <a name="easytables"></a>Daten
Die Registerkarte *Daten* in Mobile Services wurde durch *Einfache Tabellen* im Azure-Portal ersetzt.  So greifen Sie auf „Einfache Tabellen“ zu:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im Menü „MOBILE“ auf **Einfache Tabellen**.

Sie können eine Tabelle hinzufügen, indem Sie auf die Schaltfläche **Hinzufügen** klicken, oder auf Ihre vorhandenen Tabellen zugreifen, indem Sie auf den Namen einer Tabelle klicken.  Von diesem Blatt aus können Sie verschiedene Vorgänge durchführen, z.B.:

* Ändern von Tabellenberechtigungen
* Bearbeiten der eingesetzten Skripts
* Verwalten des Tabellenschemas
* Löschen der Tabelle
* Löschen des Tabelleninhalts
* Löschen von bestimmten Zeilen der Tabelle

### <a name="easyapis"></a>API
Die Registerkarte *API* in Mobile Services wurde durch *Einfache APIs* im Azure-Portal ersetzt.  So greifen Sie auf „Einfache APIs“ zu:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Klicken Sie im Menü „MOBILE“ auf **Einfache APIs**.

Ihre migrierten APIs sind auf dem Blatt bereits aufgelistet.  Sie können von diesem Blatt aus auch eine API hinzufügen.  Klicken Sie auf die API, um eine bestimmte API zu verwalten.
Von dem neuen Blatt aus können Sie die Berechtigungen für die API passen und die Skripts für die API bearbeiten.

### <a name="on-demand-jobs"></a>Scheduler-Aufträge
Auf alle Scheduler-Aufträge können Sie im Abschnitt „Scheduler-Auftragssammlungen“ zugreifen.  So greifen Sie auf Scheduler-Aufträge zu:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Durchsuchen >**, geben Sie **Zeitplan** in das Feld *Filter* ein, und wählen Sie die Option **Scheduler-Sammlungen**.
3. Wählen Sie die Auftragssammlung für Ihre Website aus.  Sie hat den Namen „*Websitename*-Jobs“.
4. Klicken Sie auf **Einstellungen**.
5. Klicken Sie unter VERWALTEN auf **Scheduler-Aufträge**.

Geplante Aufträge werden mit der Häufigkeit aufgeführt, die Sie vor der Migration angegeben haben.  Bedarfsgesteuerte Aufträge sind deaktiviert.  So führen Sie einen bedarfsgesteuerten Auftrag aus:

1. Wählen Sie den auszuführenden Auftrag aus.
2. Klicken Sie, falls erforderlich, auf **Aktivieren**, um den Auftrag zu aktivieren.
3. Klicken Sie auf **Einstellungen** und dann auf **Zeitplan**.
4. Wählen Sie als Wiederholung **Einmal** aus, und klicken Sie dann auf **Speichern**.

Ihre bedarfsgesteuerten Aufträge befinden sich in `App_Data/config/scripts/scheduler post-migration`.  Es ist ratsam, alle bedarfsgesteuerten Aufträge in [WebJobs] oder [Funktionen] umzuwandeln.  Schreiben Sie neue Scheduler-Aufträge als [WebJobs] oder [Funktionen].

### <a name="notification-hubs"></a>Notification Hubs
Mobile Services verwendet Notification Hubs für Pushbenachrichtigungen.  Die folgenden App-Einstellungen werden verwendet, um den Notification Hub nach der Migration mit Ihrem Mobile Service zu verknüpfen:

| Anwendungseinstellung | Beschreibung |
|:--- |:--- |
| **MS\_PushEntityNamespace** |Der Notification Hub-Namespace |
| **MS\_NotificationHubName** |Der Notification Hub-Name |
| **MS\_NotificationHubConnectionString** |Die Notification Hub-Verbindungszeichenfolge |
| **MS\_NamespaceName** |Ein Alias für MS_PushEntityNamespace |

Ihr Notification Hub wird über das [Azure-Portal] verwaltet.  Notieren Sie den Notification Hub-Namen (Sie finden ihn über die App-Einstellungen):

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Durchsuchen>** und dann **Notification Hubs**.
3. Klicken Sie auf den Namen des Notification Hubs, der dem Mobile Service zugeordnet ist.

> [!NOTE]
> Wenn der Notification Hub den Typ „Gemischt“ hat, ist er nicht sichtbar.  Notification Hubs des Typs „Gemischt“ setzen sowohl Notification Hubs- als auch ältere Service Bus-Features ein.  [Konvertieren Sie Ihre gemischten Namespaces], bevor Sie fortfahren.  Sobald die Konvertierung abgeschlossen ist, wird Ihr Notification Hub im [Azure-Portal] angezeigt.
>
>

Weitere Informationen finden Sie in der [Notification Hubs] -Dokumentation.

> [!TIP]
> Im [Azure-Portal] befinden sich Notification Hub-Verwaltungsfeatures noch in der Vorschau.  Das [klassischen Azure-Portal] bleibt für die Verwaltung Ihrer sämtlichen Notification Hubs verfügbar.
>
>

### <a name="legacy-push"></a>Legacypush-Einstellungen
Wenn Sie vor der Einführung von Notification Hubs Pushvorgänge für Ihren mobilen Dienst konfiguriert haben, verwenden Sie den *Legacypushvorgang*.  Wenn Sie Push verwenden und in Ihrer Konfiguration keine Notification Hubs aufgelistet werden, verwenden Sie wahrscheinlich den *Legacypushvorgang*.  Dieses Feature wird zusammen mit allen anderen Features migriert.  Allerdings wird empfohlen, ein Upgrade auf Notification Hubs durchzuführen, sobald die Migration abgeschlossen ist.

In der Zwischenzeit stehen die Legacypush-Einstellungen (mit Ausnahme des APNS-Zertifikats) in den App-Einstellungen zur Verfügung.  Aktualisieren Sie das APNS-Zertifikat, indem Sie die entsprechende Datei im Dateisystem ersetzen.

### <a name="app-settings"></a>Andere App-Einstellungen
Die folgenden zusätzlichen App-Einstellungen werden aus Ihrem Mobile Service migriert und sind unter *Einstellungen* > *App Einstellungen*verfügbar:

| Anwendungseinstellung | Beschreibung |
|:--- |:--- |
| **MS\_MobileServiceName** |Der Name Ihrer App |
| **MS\_MobileServiceDomainSuffix** |Das Domänenpräfix, z.B.: azure-mobile.net |
| **MS\_ApplicationKey** |Ihr Anwendungsschlüssel |
| **MS\_MasterKey** |Ihr App-Hauptschlüssel |

Anwendungs- und Hauptschlüssel sind mit den Anwendungsschlüsseln aus Ihrem ursprünglichen Mobile Service identisch.  Insbesondere der Anwendungsschlüssel wird von mobilen Clients gesendet, damit ihre Nutzung der mobilen API geprüft werden kann.

### <a name="cliequivalents"></a>Befehlszeilenentsprechungen
Sie können den *Azure Mobile*-Befehl nicht mehr zum Verwalten Ihrer Azure Mobile Services-Website verwenden.  Stattdessen wurden zahlreiche Funktionen durch den *Azure Site*-Befehl ersetzt.  In der folgenden Tabelle finden Sie Entsprechungen für häufig verwendete Befehle:

| *Azure Mobile*-Befehl | Entsprechender *Azure Site*-Befehl |
|:--- |:--- |
| mobile locations |site location list |
| mobile list |site list |
| mobile show *Name* |site show *Name* |
| mobile restart *Name* |site restart *Name* |
| mobile redeploy *Name* |site deployment redeploy *commitId* *Name* |
| mobile key set *Name* *Typ* *Wert* |site appsetting delete *Schlüssel* *Name* <br/> site appsetting add *Schlüssel*=*Wert* *Name* |
| mobile config list *Name* |site appsetting list *Name* |
| mobile config get *Name* *Schlüssel* |site appsetting show *Schlüssel* *Name* |
| mobile config set *Name* *Schlüssel* |site appsetting delete *Schlüssel* *Name* <br/> site appsetting add *Schlüssel*=*Wert* *Name* |
| mobile domain list *Name* |site domain list *Name* |
| mobile domain add *Name* *Domäne* |site domain add *Domäne* *Name* |
| mobile domain delete *Name* |site domain delete *Domäne* *Name* |
| mobile scale show *Name* |site show *Name* |
| mobile scale change *Name* |site scale mode *Modus* *Name* <br /> site scale instances *Instanzen* *Name* |
| mobile appsetting list *Name* |site appsetting list *Name* |
| mobile appsetting add *Name* *Schlüssel* *Wert* |site appsetting add *Schlüssel*=*Wert* *Name* |
| mobile appsetting delete *Name* *Schlüssel* |site appsetting delete *Schlüssel* *Name* |
| mobile appsetting show *Name* *Schlüssel* |site appsetting delete *Schlüssel* *Name* |

Aktualisieren Sie Authentifizierungs- oder Pushbenachrichtigungseinstellungen, indem Sie die entsprechende Anwendungseinstellung aktualisieren.
Bearbeiten Sie Dateien, und veröffentlichen Sie Ihre Website über FTP oder Git.

### <a name="diagnostics"></a>Diagnose und Protokollierung
Normalerweise ist die Diagnoseprotokollierung in einem Azure App Service deaktiviert.  So aktivieren Sie die Diagnoseprotokollierung:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Das Blatt „Einstellungen“ wird standardmäßig geöffnet.
4. Wählen Sie im Menü „FEATURES“ **Diagnoseprotokolle** .
5. Klicken Sie für die folgenden Protokolle auf **EIN**: **Anwendungsprotokollierung (Dateisystem)**, **Detaillierte Fehlermeldungen** und **Ablaufverfolgung für Anforderungsfehler**.
6. Klicken Sie für Webserverprotokollierung auf **Dateisystem** .
7. Klicken Sie unten auf der Seite auf **Speichern**

So zeigen Sie die Protokolle an:

1. Melden Sie sich beim [Azure-Portal]an.
2. Wählen Sie **Alle Ressourcen** oder **App Services**, und klicken Sie dann auf den Namen Ihres migrierten Mobile Service.
3. Klicken Sie auf die Schaltfläche **Extras**.
4. Wählen Sie im Menü „ÜBERWACHEN“ **Protokollstream** .

Protokolle werden im Fenster angezeigt, wenn sie generiert werden.  Sie können die Protokolle auch zur späteren Analyse mithilfe Ihrer Anmeldeinformationen für die Bereitstellung herunterladen. Weitere Informationen finden Sie in der Dokumentation zur [Protokollierung].

## <a name="known-issues"></a>Bekannte Probleme
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Durch die Löschung des Klons einer migrierten mobilen App wird ein Websiteausfall verursacht
Wenn Sie Ihren migrierten mobilen Dienst mithilfe von Azure PowerShell klonen und anschließend den Klon löschen, wird der DNS-Eintrag für Ihren Produktionsdienst entfernt.  Auf Ihre Website kann über das Internet nicht mehr zugegriffen werden.  

Lösung: Wenn Sie Ihre Website klonen möchten, sollten Sie dafür das Portal verwenden.

### <a name="changing-webconfig-does-not-work"></a>Das Ändern von „Web.config“ funktioniert nicht
Für ASP.NET-Websites werden Änderungen an der Datei `Web.config` nicht übernommen.  Azure App Service erstellt beim Start eine geeignete `Web.config`-Datei, um die Mobile Services-Runtime zu unterstützen.  Sie können bestimmte Einstellungen (z.B. benutzerdefinierte Header) mithilfe einer XML-Transformationsdatei überschreiben.  Erstellen Sie eine Datei mit dem Namen `applicationHost.xdt`. Diese Datei muss im Verzeichnis `D:\home\site` für den Azure-Dienst abgelegt werden.  Laden Sie die Datei `applicationHost.xdt` über ein benutzerdefiniertes Bereitstellungsskript oder direkt mithilfe von Kudu hoch.  Hier ist ein Beispieldokument dargestellt:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Weitere Informationen finden Sie auf GitHub in der Dokumentation zu [XDT-Transformationsbeispielen].

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migrierte Mobile Services können nicht zu Traffic Manager hinzugefügt werden
Wenn Sie ein Traffic Manager-Profil erstellen, können Sie einen migrierten Mobile Service für das Profil nicht direkt auswählen.  Verwenden Sie einen „externen Endpunkt“.  Der externe Endpunkt kann nur mithilfe von PowerShell hinzugefügt werden.  Weitere Informationen finden Sie im [Tutorial zu Traffic Manager](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Nächste Schritte
Ihre Anwendung wurde nicht nur zu App Service migriert, sondern Sie können sogar noch mehr Features nutzen:

* [Stagingslots] zur Bereitstellung ermöglichen Ihnen, Änderungen an Ihrer Website bereitzustellen und A/B-Tests auszuführen.
* [WebJobs] bieten einen Ersatz für bedarfsgesteuerte geplante Aufträge.
* Sie können Ihre Website [fortlaufend bereitstellen], indem Sie die Website mit GitHub, TFS oder Mercurial verknüpfen.
* Sie können Ihre Site mit [Application Insights] überwachen.
* Setzen Sie den gleichen Code für eine Website und eine Mobil-API ein.

### <a name="upgrading-your-site"></a>Aktualisieren Ihrer Mobile Services-Website auf Azure Mobile Apps-SDK
* Für Node.js-basierte Serverprojekte verfügt das neue [Mobile Apps-Node.js-SDK] über mehrere neue Features. Beispielsweise können Sie jetzt lokal entwickeln und debuggen, eine beliebige Node.js-Version nach 0.10 verwenden und eine Anpassung mit beliebiger Express.js-Middleware durchführen.
* Für .NET-basierte Serverprojekte verfügen die neuen [Mobile Apps-SDK-NuGet-Pakete](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) in Bezug auf NuGet-Abhängigkeiten über mehr Flexibilität.  Diese Pakete unterstützen die neue App Service-Authentifizierung und können mit allen ASP.NET-Projekten verwendet werden. Weitere Informationen zu Upgrades finden Sie unter [Aktualisieren des vorhandenen .NET Mobile Service auf App Service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Service – Preise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Automatische Skalierung]: ../app-service-web/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Dokumentation zur Azure App Service-Bereitstellung]: ../app-service-web/web-sites-deploy.md
[klassischen Azure-Portal]: https://manage.windowsazure.com
[Azure-Portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler-Pläne]: ../scheduler/scheduler-plans-billing.md
[fortlaufend bereitstellen]: ../app-service-web/app-service-continuous-deployment.md
[Konvertieren Sie Ihre gemischten Namespaces]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[benutzerdefinierten Domänennamen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[allgemeinen Verfügbarkeit von Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid Connections]: ../app-service/app-service-hybrid-connections.md
[Protokollierung]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobile Apps-Node.js-SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobile Services im Vergleich zu App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Leistungsüberwachung]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Stagingslots]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT-Transformationsbeispielen]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funktionen]: ../azure-functions/functions-overview.md

