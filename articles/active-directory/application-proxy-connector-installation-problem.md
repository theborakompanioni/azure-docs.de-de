---
title: Problem beim Installieren des Anwendungsproxy-Agent-Connectors | Microsoft-Dokumentation
description: "Beheben von Problemen, die beim Installieren des Anwendungsproxy-Agent-Connectors auftreten können"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 3dabb8adb67345c56750bb88373f5f740aa19036
ms.lasthandoff: 04/17/2017


---

# <a name="problem-installing-the-application-proxy-agent-connector"></a>Problem beim Installieren des Anwendungsproxy-Agent-Connectors

Der Microsoft AAD-Anwendungsproxyconnector ist eine interne Domänenkomponente, die ausgehende Verbindungen verwendet, um die Konnektivität zwischen dem von der Cloud zur Verfügung gestellten Endpunkt und der internen Domäne einzurichten.

## <a name="general-problem-areas-with-connector-installation"></a>Allgemeine Problembereiche bei der Installation des Connectors

Wenn bei der Installation eines Connectors ein Fehler auftritt, entstammt die Hauptursache in der Regel einem der folgenden Bereiche:

1.  **Konnektivität**: Zum Abschluss einer erfolgreichen Installation muss der neue Connector zukünftige Eigenschaften der Vertrauensstellung registrieren und einrichten. Dies erfolgt durch das Herstellen einer Verbindung mit dem Clouddienst des AAD-Anwendungsproxys.

2.  **Einrichtung einer Vertrauensstellung**: Der neue Connector erstellt ein selbstsigniertes Zertifikat und registriert es beim Clouddienst.

3.  **Authentifizierung des Administrators**: Während der Installation muss der Benutzer Anmeldeinformationen für den Administrator bereitstellen, um die Connectorinstallation abzuschließen.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Überprüfen der Konnektivität mit dem Cloudanwendungsproxy-Dienst und der Microsoft-Anmeldeseite

**Ziel**: Überprüfen Sie, ob der Connectorcomputer eine Verbindung mit dem Registrierungsendpunkt des AAD-Anwendungsproxys und der Microsoft-Anmeldeseite herstellen kann.

1.  Öffnen Sie einen Browser, und wechseln Sie zu der folgenden Webseite: <https://aadap-portcheck.connectorporttest.msappproxy.net>. Überprüfen Sie dann, ob die Verbindungen mit Rechenzentren in den USA (Mitte und Osten) mit den Ports 9090 und 9091 funktionieren.

2.  Wenn keiner dieser Ports erfolgreich ist (kein grünes Häkchen aufweist), stellen Sie sicher, dass die Firewall oder der Back-End-Proxy über \*.msappproxy.net mit ordnungsgemäß definierten Ports 9090 und 9091 verfügt.

3.  Öffnen Sie einen Browser (separate Registerkarte), und wechseln Sie zur folgenden Webseite: <https://login.microsoftonline.com>. Stellen Sie sicher, dass Sie sich bei dieser Seite anmelden können.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Überprüfen der Unterstützung des Vertrauensstellungszertifikats des Anwendungsproxys durch Computer und Back-End-Komponenten

**Ziel**: Überprüfen Sie, ob Connectorcomputer, Back-End-Proxy und Firewall das Zertifikat unterstützen können, das vom Connector für die zukünftige Vertrauensstellung erstellt wurde.

>[!NOTE]
>Der Connector versucht ein SHA512-Zertifikat zu erstellen, das von TLS1.2 unterstützt wird. Wenn TLS 1.2 vom Computer oder der Back-End-Firewall und dem Proxy nicht unterstützt wird, tritt bei der Installation ein Fehler auf.
>
>

**So lösen Sie das Problem:**

1.  Überprüfen Sie, ob der Computer TLS 1.2 unterstützt – Alle Windows-Versionen nach 2012 R2 sollten TLS 1.2 unterstützen. Wenn Ihr Connectorcomputer Version 2012 R2 oder früher verwendet, stellen Sie sicher, dass die folgenden KBs auf dem Computer installiert sind: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>.

2.  Wenden Sie sich an Ihren Netzwerkadministrator, damit der Back-End-Proxy und die Firewall überprüft werden, damit sie SHA512 nicht für ausgehenden Datenverkehr blockieren.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Überprüfen, dass der Connector vom Administrator installiert wird

**Ziel**: Überprüfen Sie, ob der Benutzer, der versucht, den Connector zu installieren, ein Administrator mit den richtigen Anmeldeinformationen ist. Derzeit muss der Benutzer ein globaler Administrator sein, damit die Installation erfolgreich ausgeführt werden kann.

**So überprüfen Sie, ob die Anmeldeinformationen korrekt sind:**

Stellen Sie eine Verbindung mit <https://login.microsoftonline.com> her, und verwenden Sie dieselben Anmeldeinformationen. Überprüfen Sie, ob die Anmeldung erfolgreich war. Sie können die Benutzerrolle überprüfen, indem Sie zu **Azure Active Directory** -&gt; **Benutzer und Gruppen** -&gt; **Alle Benutzer** wechseln. 

Wählen Sie Ihr Benutzerkonto aus, und klicken Sie dann im daraufhin angezeigten Menü auf „Verzeichnisrolle“. Überprüfen Sie, ob die ausgewählte Rolle „Globaler Administrator“ ist. Wenn Sie im Verlauf dieser Schritte nicht auf die Seiten zugreifen können, sind Sie kein globaler Administrator.

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)

