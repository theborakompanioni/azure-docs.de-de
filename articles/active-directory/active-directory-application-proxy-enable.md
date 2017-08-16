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
ms.date: 08/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 77acb23f33fd656a12c27107cb159613a8b2aec4
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---

# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Erste Schritte mit dem Anwendungsproxy und Installieren des Connectors
In diesem Artikel wird anhand der erforderlichen Schritte beschrieben, wie Sie den Microsoft Azure AD-Anwendungsproxy für Ihr Cloudverzeichnis in Azure AD aktivieren.

Wenn Sie die Vorteile hinsichtlich Sicherheit und Produktivität noch nicht kennen, die ein Anwendungsproxy Ihrer Organisation bieten kann, erfahren Sie mehr dazu unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Voraussetzungen für den Anwendungsproxy
Bevor Sie die Anwendungsproxydienste aktivieren und verwenden können, benötigen Sie Folgendes:

* Ein [Basic- oder Premium-Abonnement](active-directory-editions.md) für Microsoft Azure AD und ein Azure AD-Verzeichnis, für das Sie als globaler Administrator fungieren.
* Ein Server, auf dem Windows Server 2012 R2 bzw. Windows Server 2016 oder höher installiert ist und auf dem Sie den Anwendungsproxy-Connector installieren können. Der Server muss eine Verbindung mit den Anwendungsproxydiensten in der Cloud sowie mit den lokalen Anwendungen herstellen können, die Sie veröffentlichen.
  * Für das einmalige Anmelden bei Ihren veröffentlichten Anwendungen mithilfe der eingeschränkten Kerberos-Delegierung muss dieser Computer in dieselbe AD-Domäne wie die veröffentlichten Anwendungen eingebunden sein. Weitere Informationen finden Sie unter [Eingeschränkte Kerberos-Delegierung für das einmalige Anmelden mit Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md).

Wenn Ihre Organisation Proxyserver zum Herstellen einer Verbindung mit dem Internet verwendet, finden Sie unter [Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md) Informationen dazu, wie Sie diese konfigurieren, bevor Sie mit dem Anwendungsproxy beginnen.

## <a name="open-your-ports"></a>Öffnen der Ports

Zum Vorbereiten Ihrer Umgebung für den Azure AD-Anwendungsproxy, müssen Sie zuerst die Kommunikation mit Azure-Rechenzentren ermöglichen. Wenn der Pfad durch eine Firewall geschützt ist, sollten Sie sich vergewissern, dass diese so konfiguriert ist, dass der Connector HTTPS-Anforderungen (TCP) an den Anwendungsproxy richten kann.

1. Öffnen Sie die folgenden Ports für den **ausgehenden** Datenverkehr:

   | Portnummer | Wie diese verwendet wird |
   | --- | --- |
   | 80 | Herunterladen der Zertifikatssperrlisten (CRL) bei der Überprüfung des SSL-Zertifikats |
   | 443 | Gesamte ausgehende Kommunikation mit dem Webanwendungsproxy-Dienst |

   Wenn Ihre Firewall Datenverkehr gemäß Ursprungsbenutzern erzwingt, öffnen Sie diese Ports für den Datenverkehr aus Windows-Diensten, die als Netzwerkdienst ausgeführt werden.

   > [!IMPORTANT]
   > Die Tabelle gibt die Portanforderungen für die Connector-Versionen 1.5.132.0 und höher wieder. Wenn Sie eine ältere Connectorversion verwenden, müssen Sie zusätzlich zu Port 80 und 443 auch die folgenden Ports aktivieren: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120.
   >
   >Weitere Informationen zum Aktualisieren Ihres Connectors auf die neueste Version, finden Sie unter [Understand Azure AD Application Proxy connectors (Verstehen von Azure AD-Anwendungsproxy-Connectors)](application-proxy-understand-connectors.md#automatic-updates).

2. Wenn Ihre Firewall oder ihr Proxy DNS-Whitelisting zulässt, können Sie Verbindungen zu msappproxy.net und servicebus.windows.net mittels Whitelist beschränken. Ist dies nicht der Fall, müssen Sie den Zugriff auf die [IP-Adressbereiche für das Azure-Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653) aktivieren, die wöchentlich aktualisiert werden.

3. Microsoft verwendet vier Adressen, um Zertifikate zu überprüfen. Lassen Sie den Zugriff auf die folgenden URLs zu, sofern Sie ihn nicht bereits für andere Produkte gewährt haben:
   * mscrl.microsoft.com:80
   * crl.microsoft.com:80
   * ocsp.msocsp.com:80
   * www.microsoft.com:80

4. Ihr Connector benötigt für den Registrierungsprozess Zugriff auf „login.windows.net“ und „login.microsoftonline.net“.

5. Verwenden Sie den [Azure AD Application Proxy Connector Ports Test Tool (Testtool der Anwendungsproxy-Connectortools von Azure AD)](https://aadap-portcheck.connectorporttest.msappproxy.net/), um sicherzustellen, dass Ihr Connector den Anwendungsproxydienst erreichen kann. Stellen Sie zumindest sicher, dass die Region USA (Mitte) und die Ihnen am nächsten gelegene Region alle über grüne Häkchen verfügen. Darüber hinaus bedeuten mehr grüne Häkchen größere Resilienz.

## <a name="install-and-register-a-connector"></a>Installieren und Registrieren eines Connectors
1. Melden Sie sich als Administrator beim [Azure-Portal](https://portal.azure.com/) an.
2. Das aktuelle Verzeichnis wird unter Ihrem Benutzernamen in der rechten oberen Ecke angezeigt. Wenn Sie Verzeichnisse wechseln möchten, wählen Sie das Symbol aus.
3. Wechseln Sie zu **Azure Active Directory** > **Anwendungsproxy**.

   ![Navigieren zum Anwendungsproxy](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Wählen Sie **Connector herunterladen** aus.

   ![Herunterladen des Connectors](./media/active-directory-application-proxy-enable/download_connector.png)

5. Führen Sie die Datei **AADApplicationProxyConnectorInstaller.exe** auf dem Server aus, den Sie gemäß den Vorgaben vorbereitet haben.
6. Befolgen Sie die Anweisungen des Assistenten für die Installation. Während der Installation werden Sie aufgefordert, den Connector beim Anwendungsproxy Ihres Azure AD-Mandanten zu registrieren.

   * Geben Sie Ihre globalen Azure AD-Administratoranmeldeinformationen ein. Ihr globaler Administratorenmandant kann von Ihren Microsoft Azure-Anmeldeinformationen abweichen.
   * Achten Sie darauf, dass sich der Administrator, der den Connector registriert, in dem Verzeichnis befindet, in dem Sie auch den Anwendungsproxydienst aktiviert haben. Wenn die Mandantendomäne also beispielsweise „contoso.com“ lautet, muss sich der Administrator als admin@contoso.com oder mit einem anderen Aliasnamen in dieser Domäne anmelden.
   * Falls auf dem Server, auf dem Sie den Connector installieren, die Option **Verstärkte Sicherheitskonfiguration für IE** auf **Ein** festgelegt ist, wird der Registrierungsbildschirm möglicherweise nicht angezeigt. Befolgen Sie die Anweisungen in der Fehlermeldung, um Zugriff zu erhalten. Stellen Sie sicher, dass die erweiterte Sicherheit von Internet Explorer deaktiviert ist.

Falls Sie hohe Verfügbarkeit benötigen, sollten Sie mindestens zwei Connectors bereitstellen. Jeder Connector muss separat registriert werden.

## <a name="test-that-the-connector-installed-correctly"></a>Testen der ordnungsgemäßen Installation des Connectors

Sie können feststellen, ob ein neuer Connector ordnungsgemäß installiert wurde, indem Sie ihn entweder im Azure-Portal oder auf Ihrem Server überprüfen. 

Melden Sie sich im Azure-Portal bei Ihrem Mandanten an, und navigieren Sie zu **Azure Active Directory** > **Anwendungsproxy**. Alle Connectors und Connectorgruppen werden auf dieser Seite angezeigt. Wählen Sie einen Connector aus, um Details dazu anzuzeigen oder ihn in eine andere Connectorgruppe zu verschieben. 

Überprüfen Sie auf Ihrem Server die Liste der aktiven Dienste für den Connector und die Connectoraktualisierung. Die Ausführung beider Dienste sollte sofort starten. Wenn dies nicht der Fall ist, aktivieren Sie die Dienste: 

   * **Microsoft AAD-Anwendungsproxyconnector** ermöglicht die Konnektivität.

   * Die **Aktualisierung des Microsoft AAD-Anwendungsproxyconnectors** ist ein automatischer Aktualisierungsdienst. Der Dienst sucht nach neuen Versionen des Connectors und aktualisiert den Connector bei Bedarf.

   ![Anwendungsproxy-Connectordienste – Screenshot](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Informationen zu Connectors und deren Aktualisierung finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md).


## <a name="next-steps"></a>Nächste Schritte
Nun können Sie [Anwendungen mit dem Anwendungsproxy veröffentlichen](application-proxy-publish-azure-portal.md).

Wenn Sie über Anwendungen verfügen, die sich in separaten Netzwerken oder an unterschiedlichen Standorten befinden, verwenden Sie Connectorgruppen, um die verschiedenen Connectors in logischen Einheiten anzuordnen. Weitere Informationen zur Verwendung von Anwendungsproxy-Connectors finden Sie unter [Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Speicherorten mit Connectorgruppen](active-directory-application-proxy-connectors-azure-portal.md).

