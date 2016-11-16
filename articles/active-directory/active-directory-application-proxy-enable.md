---
title: Aktivieren des Azure AD-Anwendungsproxys | Microsoft Docs
description: "Aktivieren Sie den Anwendungsproxy über das klassische Azure-Portal, und installieren Sie die Connectors für den Reverseproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d0a5cfe8fe9782cc9b75392d6f21965430ee2347


---
# <a name="enable-application-proxy-in-the-azure-portal"></a>Aktivieren des Anwendungsproxys über das Azure-Portal
In diesem Artikel wird anhand der erforderlichen Schritte beschrieben, wie Sie den Microsoft Azure AD-Anwendungsproxy für Ihr Cloudverzeichnis in Azure AD aktivieren.

Weitere Informationen zu den Vorteilen des Anwendungsproxys finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Voraussetzungen für den Anwendungsproxy
Bevor Sie die Anwendungsproxydienste aktivieren und verwenden können, benötigen Sie Folgendes:

* Ein [Basic- oder Premium-Abonnement](active-directory-editions.md) für Microsoft Azure AD und ein Azure AD-Verzeichnis, für das Sie als globaler Administrator fungieren.
* Einen Server, auf dem Windows Server 2012 R2 bzw. Windows 8.1 oder höher installiert ist und auf dem Sie den Anwendungsproxy-Connector installieren können. Der Server sendet Anforderungen an die Anwendungsproxydienste in der Cloud und benötigt eine HTTP- oder HTTPS-Verbindung mit den Anwendungen, die Sie veröffentlichen.
  
  * Für das einmalige Anmelden an Ihren veröffentlichten Anwendungen sollte dieser Computer in dieselbe AD-Domäne wie die veröffentlichten Anwendungen eingebunden sein.
* Wenn der Pfad durch eine Firewall geschützt ist, sollten Sie sich vergewissern, dass diese so konfiguriert ist, dass der Connector HTTPS-Anforderungen (TCP) an den Anwendungsproxy richten kann. Der Connector verwendet diese Ports zusammen mit untergeordneten Domänen, die Teil der übergeordneten Domänen „msappproxy.net“ und „servicebus.windows.net“ sind. Stellen Sie sicher, dass die folgenden Ports für den **ausgehenden** Datenverkehr geöffnet sind:
  
  | Portnummer | Beschreibung |
  | --- | --- |
  | 80 |Ermöglicht ausgehenden HTTP-Verkehr für die Sicherheitsüberprüfung. |
  | 443 |Ermöglicht die Benutzerauthentifizierung für Azure AD (nur für den Connectorregistrierungsprozess erforderlich). |
  | 10100 - 10120 |Ermöglicht das Zurücksenden von LOB HTTP-Antworten an den Proxy. |
  | 9352, 5671 |Ermöglicht eine Kommunikation zwischen Connector und Azure-Dienst für eingehende Anforderungen. |
  | 9350 |Optional; verbessert die Leistung für eingehende Anforderungen. |
  | 8080 |Aktiviert die Bootstrapsequenz des Connectors sowie eine automatische Aktualisierung des Connectors. |
  | 9090 |Ermöglicht die Connectorregistrierung (nur für den Connectorregistrierungsprozess erforderlich). |
  | 9091 |Ermöglicht die automatische Erneuerung des Vertrauenszertifikats für den Connector. |
  
    Wenn Ihre Firewall Datenverkehr gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden. Stellen Sie außerdem sicher, dass Port 8080 für "NT Authority\System" aktiviert ist.
* Wenn in Ihrer Organisation Proxyserver zum Herstellen einer Verbindung mit dem Internet verwendet werden, helfen Ihnen die Details zur Konfiguration im Blogbeitrag [Working with existing on-premises proxy servers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) (Verwenden von vorhandenen lokalen Proxyservern) weiter.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Schritt 1: Aktivieren des Anwendungsproxys in Azure AD
1. Melden Sie sich als Administrator am [klassischen Azure-Portal](https://manage.windowsazure.com/)an.
2. Gehen Sie zu Active Directory, und wählen Sie das Verzeichnis aus, in dem Sie den Anwendungsproxy aktivieren möchten.
   
    ![Active Directory – Symbol](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Wählen Sie auf der Verzeichnisseite die Option **Konfigurieren** aus, und navigieren Sie nach unten zu **Anwendungsproxy**.
4. Legen Sie die Option **Anwendungsproxydienste für dieses Verzeichnis aktivieren** auf **Aktiviert** fest.
   
    ![Aktivieren des Anwendungsproxys](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Wählen Sie **Jetzt herunterladen** aus. Hierdurch gelangen Sie zum **Download des Azure AD-Anwendungsproxyconnectors**. Lesen und akzeptieren Sie die Lizenzbedingungen, und klicken Sie auf **Herunterladen**, um die Windows Installer-Datei (.exe) für den Connector zu speichern.

## <a name="step-2-install-and-register-the-connector"></a>Schritt 2: Installieren und Registrieren des Connectors
1. Führen Sie die Datei **AADApplicationProxyConnectorInstaller.exe** auf dem Server aus, den Sie gemäß den Vorgaben vorbereitet haben.
2. Befolgen Sie die Anweisungen des Assistenten für die Installation.
3. Während der Installation werden Sie aufgefordert, den Connector beim Anwendungsproxy Ihres Azure AD-Mandanten zu registrieren.
   
   * Geben Sie Ihre globalen Azure AD-Administratoranmeldeinformationen ein. Ihr globaler Administratorenmandant kann von Ihren Microsoft Azure-Anmeldeinformationen abweichen.
   * Achten Sie darauf, dass sich der Administrator, der den Connector registriert, in dem Verzeichnis befindet, in dem Sie auch den Anwendungsproxydienst aktiviert haben. Wenn die Mandantendomäne also beispielsweise „contoso.com“ lautet, muss sich der Administrator als admin@contoso.com oder mit einem anderen Aliasnamen in dieser Domäne anmelden.
   * Falls auf dem Server, auf dem Sie den Connector installieren, die Option **Verstärkte Sicherheitskonfiguration für IE** auf **Ein** festgelegt ist, wird der Registrierungsbildschirm möglicherweise blockiert. Befolgen Sie die Anweisungen in der Fehlermeldung, um den Zugriff zuzulassen. Stellen Sie sicher, dass die erweiterte Sicherheit von Internet Explorer deaktiviert ist.
   * Falls die Connectorregistrierung nicht erfolgreich war, helfen Ihnen die Informationen unter [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)weiter.  
4. Nach Abschluss der Installation werden zwei neue Dienste auf dem Server hinzugefügt:
   
   * **Microsoft AAD-Anwendungsproxyconnector** ermöglicht die Konnektivität.
     
     * **Microsoft AAD Application Proxy Connector Updater** (Updater für Microsoft AAD-Anwendungsproxyconnector) ist ein automatisierter Updatedienst, der in regelmäßigen Abständen prüft, ob neue Versionen des Connectors verfügbar sind, und den Connector ggf. aktualisiert.
     
     ![Anwendungsproxy-Connectordienste – Screenshot](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Klicken Sie im Installationsfenster auf **Fertig stellen** .

Falls Sie hohe Verfügbarkeit benötigen, sollten Sie mindestens zwei Connectors bereitstellen. Wiederholen Sie dazu die oben angegebenen Schritte 2 und 3. Jeder Connector muss separat registriert werden.

Wenn Sie den Connector deinstallieren möchten, müssen Sie sowohl den Connector- als auch den Updatedienst deinstallieren. Starten Sie den Computer neu, um den Dienst vollständig zu entfernen.

## <a name="next-steps"></a>Nächste Schritte
Nun können Sie [Anwendungen mit dem Anwendungsproxy veröffentlichen](active-directory-application-proxy-publish.md).

Wenn Sie über Anwendungen verfügen, die sich in separaten Netzwerken oder an unterschiedlichen Standorten befinden, können Sie Connectorgruppen verwenden, um die verschiedenen Connectors in logischen Einheiten anzuordnen. Weitere Informationen zur Verwendung von Anwendungsproxy-Connectors finden Sie unter [Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Speicherorten mit Connectorgruppen](active-directory-application-proxy-connectors.md).




<!--HONumber=Nov16_HO2-->


