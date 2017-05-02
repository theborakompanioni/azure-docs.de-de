---
title: "Azure AD-App-Proxy – erste Schritte und Installieren des Connectors | Microsoft-Dokumentation"
description: "Aktivieren Sie den Anwendungsproxy über das Azure-Portal, und installieren Sie die Connectors für den Reverseproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 8bfdc2f4b86e38d707c026365e3ffcce471d5367
ms.lasthandoff: 04/07/2017


---

# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Erste Schritte mit dem Anwendungsproxy und Installieren des Connectors
In diesem Artikel wird anhand der erforderlichen Schritte beschrieben, wie Sie den Microsoft Azure AD-Anwendungsproxy für Ihr Cloudverzeichnis in Azure AD aktivieren.

Weitere Informationen zu den Vorteilen des Anwendungsproxys finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Voraussetzungen für den Anwendungsproxy
Bevor Sie die Anwendungsproxydienste aktivieren und verwenden können, benötigen Sie Folgendes:

* Ein [Basic- oder Premium-Abonnement](active-directory-editions.md) für Microsoft Azure AD und ein Azure AD-Verzeichnis, für das Sie als globaler Administrator fungieren.
* Einen Server, auf dem Windows Server 2012 R2 bzw. Windows 8.1 oder höher installiert ist und auf dem Sie den Anwendungsproxy-Connector installieren können. Der Server sendet Anforderungen an die Anwendungsproxydienste in der Cloud und benötigt eine HTTP- oder HTTPS-Verbindung mit den Anwendungen, die Sie veröffentlichen.
  * Für das einmalige Anmelden an Ihren veröffentlichten Anwendungen sollte dieser Computer in dieselbe AD-Domäne wie die veröffentlichten Anwendungen eingebunden sein. Weitere Informationen finden Sie unter [Einmaliges Anmelden mit Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md).

Wenn in Ihrer Organisation Proxyserver zum Herstellen einer Verbindung mit dem Internet verwendet werden, helfen Ihnen die Details zur Konfiguration in [Working with existing on-premises proxy servers](application-proxy-working-with-proxy-servers.md) (Verwenden von vorhandenen lokalen Proxyservern) weiter.

## <a name="open-your-ports"></a>Öffnen der Ports

Wenn der Pfad durch eine Firewall geschützt ist, sollten Sie sich vergewissern, dass diese so konfiguriert ist, dass der Connector HTTPS-Anforderungen (TCP) an den Anwendungsproxy richten kann. Der Connector verwendet diese Ports zusammen mit untergeordneten Domänen, die Teil der übergeordneten Domänen „msappproxy.net“ und „servicebus.windows.net“ sind. Stellen Sie sicher, dass die folgenden Ports für den **ausgehenden** Datenverkehr geöffnet sind:

| Portnummer | Beschreibung |
| --- | --- |
| 80 |Ermöglicht ausgehenden HTTP-Verkehr für die Sicherheitsüberprüfung. |
| 443 |Ermöglicht die Benutzerauthentifizierung für Azure AD (nur für den Connectorregistrierungsprozess erforderlich).<br>Aktiviert die Bootstrapsequenz des Connectors sowie automatische Updates<br>Ermöglicht die Kommunikation zwischen Connector und Azure-Dienst für eingehende Anforderungen  |
| 10100 - 10120 |Ermöglicht das Zurücksenden von LOB HTTP-Antworten an den Proxy. |
| 9350 |Optional; verbessert die Leistung für eingehende Anforderungen. |
| 8080 |Aktiviert die Bootstrapsequenz des Connectors sowie automatische Updates für den Connector |
| 9090 |Ermöglicht die Connectorregistrierung (nur für den Connectorregistrierungsprozess erforderlich) |
| 9091 |Ermöglicht die automatische Erneuerung des Vertrauenszertifikats für den Connector |

> [!IMPORTANT]
> Die Tabelle gibt die Portanforderungen für die neueste Version des Connectors an. Wenn Sie weiterhin einen Connector mit einer älteren Version als 1.5 verwenden, müssen Sie auch 9350, 9352 und 5671 aktivieren. Diese Ports ermöglichen die Kommunikation zwischen Connectors und Azure-Dienst für eingehende Anforderungen. 

Wenn Ihre Firewall Datenverkehr gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden. Stellen Sie außerdem sicher, dass Port 8080 für "NT Authority\System" aktiviert ist.

Verwenden Sie den [Azure AD Application Proxy Connector Ports Test Tool (Testtool der Anwendungsproxy-Connectortools von Azure AD)](https://aadap-portcheck.connectorporttest.msappproxy.net/), um sicherzustellen, dass Ihr Connector den Anwendungsproxydienst erreichen kann. Stellen Sie zumindest sicher, dass die Region USA (Mitte) und die Ihnen am nächsten gelegene Region alle über grüne Häkchen verfügen. Darüber hinaus bedeuten mehr grüne Häkchen größere Resilienz. 


## <a name="install-and-register-a-connector"></a>Installieren und Registrieren eines Connectors
1. Melden Sie sich als Administrator beim [Azure-Portal](https://portal.azure.com/) an.
2. Das aktuelle Verzeichnis wird unter Ihrem Benutzernamen in der rechten oberen Ecke angezeigt. Wenn Sie Verzeichnisse wechseln möchten, wählen Sie das Symbol aus.
3. Wechseln Sie zu **Azure Active Directory** > **Anwendungsproxy**.

   ![Navigieren zum Anwendungsproxy](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Wählen Sie **Connector herunterladen** aus.

   ![Herunterladen des Connectors](./media/active-directory-application-proxy-enable/download_connector.png)

4. Führen Sie die Datei **AADApplicationProxyConnectorInstaller.exe** auf dem Server aus, den Sie gemäß den Vorgaben vorbereitet haben.
5. Befolgen Sie die Anweisungen des Assistenten für die Installation.
6. Während der Installation werden Sie aufgefordert, den Connector beim Anwendungsproxy Ihres Azure AD-Mandanten zu registrieren.
   
   * Geben Sie Ihre globalen Azure AD-Administratoranmeldeinformationen ein. Ihr globaler Administratorenmandant kann von Ihren Microsoft Azure-Anmeldeinformationen abweichen.
   * Achten Sie darauf, dass sich der Administrator, der den Connector registriert, in dem Verzeichnis befindet, in dem Sie auch den Anwendungsproxydienst aktiviert haben. Wenn die Mandantendomäne also beispielsweise „contoso.com“ lautet, muss sich der Administrator als admin@contoso.com oder mit einem anderen Aliasnamen in dieser Domäne anmelden.
   * Falls auf dem Server, auf dem Sie den Connector installieren, die Option **Verstärkte Sicherheitskonfiguration für IE** auf **Ein** festgelegt ist, wird der Registrierungsbildschirm möglicherweise blockiert. Befolgen Sie die Anweisungen in der Fehlermeldung, um den Zugriff zuzulassen. Stellen Sie sicher, dass die erweiterte Sicherheit von Internet Explorer deaktiviert ist.
   * Falls die Connectorregistrierung nicht erfolgreich war, helfen Ihnen die Informationen unter [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)weiter.  
7. Nach Abschluss der Installation werden zwei neue Dienste auf dem Server hinzugefügt:
   
   * **Microsoft AAD-Anwendungsproxyconnector** ermöglicht die Konnektivität.
     
   * **Microsoft AAD Application Proxy Connector Updater** (Updater für Microsoft AAD-Anwendungsproxyconnector) ist ein automatisierter Updatedienst, der in regelmäßigen Abständen prüft, ob neue Versionen des Connectors verfügbar sind, und den Connector ggf. aktualisiert.
     
   ![Anwendungsproxy-Connectordienste – Screenshot](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Informationen zu Connectors finden Sie unter [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md) (Grundlegendes zu Azure AD-Anwendungsproxyconnectors). 

Falls Sie hohe Verfügbarkeit benötigen, sollten Sie mindestens zwei Connectors bereitstellen. Wiederholen Sie zum Bereitstellen weiterer Connectors die Schritte 2 und 3. Jeder Connector muss separat registriert werden.

Wenn Sie den Connector deinstallieren möchten, müssen Sie sowohl den Connector- als auch den Updatedienst deinstallieren. Starten Sie den Computer neu, um den Dienst vollständig zu entfernen.

## <a name="next-steps"></a>Nächste Schritte
Nun können Sie [Anwendungen mit dem Anwendungsproxy veröffentlichen](application-proxy-publish-azure-portal.md).

Wenn Sie über Anwendungen verfügen, die sich in separaten Netzwerken oder an unterschiedlichen Standorten befinden, können Sie Connectorgruppen verwenden, um die verschiedenen Connectors in logischen Einheiten anzuordnen. Weitere Informationen zur Verwendung von Anwendungsproxy-Connectors finden Sie unter [Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Speicherorten mit Connectorgruppen](active-directory-application-proxy-connectors-azure-portal.md).


