---
title: Migrieren von Unternehmens-Web-Apps in Azure App Service
description: "Sie erfahren, wie Sie mit dem Migrations-Assistenten für Web-Apps auf schnelle Weise vorhandene IIS-Websites in Azure App Service-Web-Apps migrieren"
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: 
ms.assetid: 2e846fc0-37cc-42e6-ac57-ff442ef16e85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 18d6a8da38b42dcf5c1500f7fc26638aea26a809
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="migrate-an-enterprise-web-app-to-azure-app-service"></a>Migrieren von Unternehmens-Web-Apps in Azure App Service
Sie können Ihre vorhandene Websites, die auf Internet Information Service (IIS) 6 oder höher ausgeführt werden, problemlos zu [App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714)migrieren. 

> [!IMPORTANT]
> Der Support für Windows Server 2003 ist am 14. Juli 2015 ausgelaufen. Wenn Sie derzeit Websites auf einem IIS-Server unter Windows Server 2003 hosten, bietet Web-Apps eine risikoarme, kostengünstige und wenig aufwendige Möglichkeit, Ihre Websites online zu halten. Der Migrations-Assistent für Web-Apps kann Sie bei der Automatisierung des Migrationsprozesses unterstützen. 
> 
> 

[Migrations-Assistent für Web-Apps](https://www.movemetothecloud.net/) kann die IIS-Server-Installation analysieren, identifizieren, welche Websites zu App Service migriert werden, alle Elemente markieren, die nicht migriert werden können oder auf der Plattform nicht unterstützt werden, und dann die Websites und zugeordneten Datenbanken nach Azure migrieren.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a>Während der Kompatibilitätsanalyse überprüfte Elemente
Der Migrations-Assistent erstellt einen Bereitschaftsbericht, um alle möglichen Probleme oder Hindernisse zu identifizieren, die eine erfolgreiche Migration von lokalen IIS zu Azure App Service-Web-Apps verhindern könnten. Einige der wichtigsten zu berücksichtigenden Elemente sind u. a.:

* Portbindungen – Web-Apps unterstützt nur Port 80 für HTTP- und Port 443 für HTTPS-Datenverkehr. Andere Portkonfigurationen werden ignoriert, und der Datenverkehr wird an 80 oder 443 weitergeleitet. 
* Authentifizierung – Web-Apps unterstützt standardmäßig die anonyme Authentifizierung und Formularauthentifizierung, sofern von einer Anwendung angegeben. Windows-Authentifizierung kann nur durch die Integration in Azure Active Directory und ADFS verwendet werden. Aller anderen Formen der Authentifizierung – z. B. Standardauthentifizierung – werden derzeit nicht unterstützt. 
* Globaler Assemblycache (GAC) – Der GAC wird in Web-Apps nicht unterstützt. Wenn Ihre Anwendung Assemblys referenziert, die Sie in der Regel im globalen Assemblycache bereitstellen, müssen Sie in den Ordner "Bin" der Anwendung in Web-Apps bereitstellen. 
* IIS5-Kompatibilitätsmodus – Wird in Web-Apps nicht unterstützt. 
* Anwendungspools – In Web-Apps werden alle Websites und deren untergeordnete Anwendungen in demselben Anwendungspool ausgeführt. Wenn Ihre Website mehrere untergeordnete Anwendungen hat, die mehrere Anwendungspools verwenden, konsolidieren Sie diese in einem einzigen Anwendungspool mit gemeinsamen Einstellungen, oder migrieren Sie jede Anwendung in eine getrennte Web-App.
* COM-Komponenten – Web-Apps erlauben keine Registrierung von COM-Komponenten auf der Plattform. Wenn Ihre Websites oder Anwendungen COM-Komponenten verwenden, müssen Sie sie in verwaltetem Code umschreiben und mit der Website oder Anwendung bereitstellen.
* ISAPI-Erweiterungen – Web-Apps unterstützt die Verwendung von ISAPI-Erweiterungen. Sie müssen die folgenden Schritte ausführen:
  
  * Stellen Sie die DLLs für Ihre Web-App bereit. 
  * Registrieren Sie die DLLs mit [Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)
  * Speichern Sie eine Datei „applicationHost.xdt“ im Stammverzeichnis der Website, deren Inhalt im Abschnitt [Zulassen von Ladevorgängen für beliebige ISAPI-Erweiterungen](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples) beschrieben wird. 
    
  
    
    Weitere Beispiele zur Verwendung von XML-Dokumenttransformationen auf Ihrer Website finden Sie unter [Transformieren einer Microsoft Azure-Website](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).
* Andere Komponenten wie z. B. SharePoint, FrontPage-Servererweiterungen (FPSE), FTP oder SSL-Zertifikate werden nicht migriert.

## <a name="how-to-use-the-web-apps-migration-assistant"></a>Verwenden des Migrations-Assistenten für Web-Apps
In diesem Abschnitt werden Sie ausführlich durch ein Beispiel zum Migrieren einiger Websites geführt, die eine SQL Server-Datenbank verwenden und auf einem lokalen Computer mit Windows Server 2003 R2 (IIS 6.0) ausgeführt werden:

1. Navigieren Sie auf dem IIS-Server oder Client-Computer zu [https://www.movemetothecloud.net/](https://www.movemetothecloud.net/) 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. Installieren Sie den Migrations-Assistenten für Web-Apps, indem Sie auf die Schaltfläche **Dedizierter IIS-Server** klicken. Weitere Optionen werden in der nahen Zukunft bereitgestellt. 
3. Klicken Sie auf **Tool installieren** , um den Migrations-Assistenten für Web-Apps auf Ihrem Computer zu installieren.
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > Sie können auch auf **Für Offline-Installation herunterladen** klicken, um eine ZIP-Datei herunterzuladen und auf Servern ohne Internetverbindung zu installieren. Oder klicken Sie auf **Vorhandenen Bericht zur Migrationsbereitschaft hochladen**, eine erweiterte Option, die zusammen mit einem vorhandenen Migrationsbereitschaftsbericht ausgeführt wird, den Sie zuvor erstellt haben (wird weiter unten erläutert).
   > 
   > 
4. Klicken Sie auf dem Bildschirm **Anwendungsinstallation** auf **Installieren**, um die Installation auf Ihrem Computer vorzunehmen. Bei Bedarf werden auch die entsprechenden Abhängigkeiten wie Web Deploy, DacFX und IIS installiert. 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   Nach der Installation wird der Migrations-Assistent für Web-Apps automatisch gestartet.
5. Wählen Sie **Migrieren von Websites und Datenbanken von einem Remoteserver zu Azure**. Geben Sie die Administratoranmeldeinformationen für den Remoteserver an, und klicken Sie auf **Weiter**. 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   Sie können natürlich auch vom lokalen Server aus migrieren. Die Remoteoption ist nützlich, wenn Sie Websites von einem IIS-Produktionsserver migrieren möchten.
   
   Das Migrationstool prüft an diesem Punkt die IIS-Server-Konfiguration, z. B. Websites, Anwendungen, Anwendungspools und Abhängigkeiten, um für die Migration geeignete Websites zu erkennen. 
6. Der folgende Screenshot zeigt drei Websites – **Default Web Site**, **TimeTracker** und **CommerceNet4**. Alle haben eine zugeordnete Datenbank, die migriert werden soll. Wählen Sie alle Websites aus, die Sie bewerten möchten, und klicken Sie dann auf **Weiter**.
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. Klicken Sie auf **Hochladen** , um den Bereitschaftsbericht hochzuladen. Wenn Sie auf **Datei lokal speichern**klicken, können Sie das Migrationstool später erneut ausführen und den gespeicherten Bereitschaftsbericht wie zuvor beschrieben hochladen.
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   Nachdem Sie den Bereitschaftsbericht hochgeladen haben, führt Azure eine Bereitschaftsanalyse durch und zeigt die Ergebnisse an. Lesen Sie die Bewertungsdetails für jede Website, und stellen Sie sicher, dass Sie alle Probleme verstanden oder behoben haben, bevor Sie fortfahren. 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. Klicken Sie auf **Migration starten**, um die Migration zu starten. Sie werden zu Azure umgeleitet, um sich bei Ihrem Konto anzumelden. Es ist wichtig, dass Sie sich mit einem Konto anmelden, das über ein aktives Azure-Abonnement verfügt. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich [hier](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_) für eine kostenlose Testversion registrieren. 
9. Wählen Sie das Mandantenkonto, das Azure-Abonnement und die Region aus, die für die migrierten Azure-Web-Apps und -Datenbanken verwendet werden sollen, und klicken Sie dann auf **Migration starten**. Sie können die zu migrierenden Websites später auswählen.
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. Auf dem nächsten Bildschirm können Sie die Standardeinstellungen für die Migration ändern, wie z. B.:
    
    * Verwenden einer vorhandenen Azure SQL-Datenbank oder Erstellen einer neuen Azure SQL-Datenbank und Konfigurieren der Anmeldeinformationen
    * Auswählen der zu migrierenden Websites
    * Definieren von Namen für die Azure Web-Apps und deren verknüpften SQL-Datenbanken
    * Anpassen der globalen Einstellungen und Einstellungen auf Websiteebene
    
    Der folgende Screenshot zeigt alle Websites, die für die Migration ausgewählt wurden, mit ihren Standardeinstellungen.
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > Über das Kontrollkästchen **Azure Active Directory aktivieren** der benutzerdefinierten Einstellungen wird die Azure-Web-App in [Azure Active Directory](../active-directory/active-directory-whatis.md) (das **Standardverzeichnis**) integriert. Weitere Informationen zur Synchronisierung von Azure Active Directory mit Ihrem lokalen Active Directory finden Sie unter [Verzeichnisintegration](http://msdn.microsoft.com/library/jj573653).
    > 
    > 
11. Nachdem Sie alle gewünschten Änderungen vorgenommen haben, klicken Sie auf **Erstellen** , um den Migrationsvorgang zu starten. Das Migrationstool erstellt die Azure SQL-Datenbank und Azure-Web-App und veröffentlicht dann die Inhalte der Websites und Datenbanken. Der Migrationsstatus wird im Migrationstools klar angezeigt, und am Ende sehen Sie eine Zusammenfassung mit Einzelheiten zu den migrierten Websites, dem Erfolg sowie Links zu den neu erstellten Azure-Web-Apps. 
    
    Tritt während der Migration ein Fehler auf, so weist das Migrationstool klar auf den Fehler hin und setzt die Änderungen zurück. Sie können einen Fehlerbericht auch direkt an das Entwicklungsteam senden, indem Sie auf die Schaltfläche **Fehlerbericht senden** klicken. Die erfasste Fehleraufrufliste und ein Nachrichtentext werden mitgeschickt. 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    Wenn die Migration erfolgreich und ohne Fehler verlaufen ist, können Sie auch auf die Schaltfläche **Feedback geben** klicken, um direktes Feedback zu geben. 
12. Klicken Sie auf die Links zu den Azure-Web-Apps, und stellen Sie sicher, dass die Migration erfolgreich war.
13. Sie können jetzt die migrierten Web-Apps in Azure App Service verwalten. Melden Sie sich dazu beim [Azure-Portal](https://portal.azure.com)an.
14. Öffnen Sie das Blatt von Web-Apps im Azure-Portal, um Ihre migrierten Websites (dargestellt als Web-Apps) anzuzeigen. Klicken Sie dann auf eine, um sie zu verwalten. Sie können z. B. die kontinuierliche Veröffentlichung, das Erstellen von Sicherungen, die automatische Skalierung und die Überwachung von Nutzung oder Leistung konfigurieren.
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)


