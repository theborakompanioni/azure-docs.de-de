---
title: "Übersetzen von Links und URLs – Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: d716476deb81f6627cf03401f78ab399ae940e68
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---

# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Umleiten von hartcodierten Links für Apps, die mit Azure AD-Anwendungsproxy veröffentlicht wurden

Azure AD-Anwendungsproxy stellt Ihre lokalen Apps Benutzern zur Verfügung, die remote oder an eigenen Geräten arbeiten. Einige Apps wurden jedoch für eine lokale Zielgruppe entwickelt, sodass häufig in den HTML-Code lokale Links eingebettet sind, die nicht ordnungsgemäß funktionieren, wenn die App remote verwendet wird. Das geschieht meistens, wenn mehrere lokale Anwendungen aufeinander verweisen und Ihre Benutzer Links zum Navigieren zwischen den Apps erwarten, anstatt externe URLs für jede App eingeben zu müssen.

Zur Lösung dieses Problems wurde das Linkübersetzungsfeature des Anwendungsproxys entwickelt. Wenn Sie über Apps verfügen, die direkt auf interne Endpunkte oder Ports verweisen, können Sie diese internen URLs den veröffentlichten externen Anwendungsproxy-URLs zuordnen. Wenn Sie die Linkübersetzung aktivieren, durchsucht Anwendungsproxy HTML-, CSS- und ausgewählte JavaScript-Tags für veröffentlichte interne Links. Anschließend werden diese übersetzt, sodass Vorgänge für Ihre Benutzer ohne Unterbrechung stattfinden.

>[!NOTE]
>Das Linkübersetzungsfeature ist für Mandanten vorgesehen, die aus verschiedenen Gründen keine benutzerdefinierten Domänen verwenden können, damit sie über dieselben internen und externen URLs für ihre Apps verfügen. Bevor Sie dieses Feature aktivieren, finden Sie heraus, ob [benutzerdefinierte Domänen in Azure AD-Anwendungsproxy](active-directory-application-proxy-custom-domains.md) für Sie funktionieren.

## <a name="how-link-translation-works"></a>Funktionsweise der Linkübersetzung

Wenn der Proxyserver nach der Authentifizierung die Anwendungsdaten an den Benutzer übergibt, scannt Anwendungsproxy die Anwendung auf hartcodierte Links und ersetzt diese durch die jeweiligen veröffentlichten externen URLs.

### <a name="which-links-are-affected"></a>Welche Links sind betroffen?

Das Linkübersetzungsfeature sucht nur nach Links, die sich in Codetags im Text einer App befinden. Anwendungsproxy weist ein separates Feature für die Übersetzung von Cookies oder URLs in Headern auf. 

Es gibt zwei verbreitete Typen von internen Links in lokalen Anwendungen:

- **Relative interne Links**, die auf eine freigegebene Ressource in einer lokalen Dateistruktur wie `/claims/claims.html` zeigen. Diese Links funktionieren automatisch in Apps, die über Anwendungsproxy veröffentlicht werden, und funktionieren auch weiterhin mit oder ohne Linkübersetzung. 
- **Hartcodierte interne Links** zu anderen lokalen Apps wie `http://expenses` oder veröffentlichten Dateien wie `http://expenses/logo.jpg`. Das Linkübersetzungsfeature funktioniert für hartcodierte interne Links und ändert diese, sodass sie auf die externen URLs zeigen, die für Remotebenutzer gelten.

### <a name="how-do-apps-link-to-each-other"></a>Wie funktionieren Links zwischen Apps?

Die Linkübersetzung ist für jede Anwendung aktiviert, damit Sie die Kontrolle über die Benutzererfahrung für die einzelnen Apps behalten. Aktivieren Sie die Linkübersetzung für eine App, wenn die Links *aus* dieser App übersetzt werden sollen, nicht Links *auf* diese App. 

Beispiel: Angenommen, drei Ihrer Anwendungen werden über Anwendungsproxy veröffentlicht und verweisen mit Links aufeinander: „Vorteile“, „Ausgaben“ und „Reisen“. Es gibt eine vierte App, „Feedback“, die nicht über Anwendungsproxy veröffentlicht wird.

Wenn Sie die Linkübersetzung für Vorteile aktivieren, werden die Links zu „Ausgaben“ und „Reisen“ an die externen URLs für diese Apps umgeleitet, der Link zu „Feedback“ wird jedoch nicht umgeleitet, da keine externe URL vorhanden ist. Links von „Ausgaben“ und „Reisen“ zurück zu „Vorteile“ funktionieren nicht, da die Linkübersetzung für diese beiden Apps nicht aktiviert wurde.

![Links von „Vorteile“ zu anderen Apps, wenn Linkübersetzung aktiviert ist](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Welche Links werden nicht übersetzt?

Zum Verbessern der Leistung und Sicherheit werden einige Links nicht übersetzt:

- Links außerhalb von Codetags 
- Interne Links, die von anderen Programmen geöffnet wurden Links, die über E-Mail oder Sofortnachrichten gesendet wurden oder in anderen Dokumenten enthalten sind, werden nicht übersetzt. Den Benutzern muss bekannt sein, dass sie zur externen URL navigieren müssen.

Wenn Sie eines dieser beiden Szenarien unterstützen möchten, können Sie dieselben internen und externen URLs verwenden, sodass keine Notwendigkeit für eine Linkübersetzung besteht.  

## <a name="enable-link-translation"></a>Aktivieren der Linkübersetzung

Die ersten Schritte mit der Linkübersetzung sind einfache Klicks auf Schaltflächen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wechseln Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**, wählen Sie die App, die Sie verwaltenden möchten, und dann **Anwendungsproxy** aus.
3. Legen Sie **URLs im Hauptteil der Anwendung übersetzen** auf **Ja** fest.

   ![Wählen Sie „Ja“ aus, um die URLs im Hauptteil der Anwendung zu übersetzen.](./media/application-proxy-link-translation/select_yes.png)zu erstellen und zu verwalten.
4. Klicken Sie zum Übernehmen der Änderungen auf **Speichern**.

Wenn Ihre Benutzer auf diese Anwendung zugreifen, scannt der Proxy nun automatisch auf interne URLs, die über Anwendungsproxy für Ihren Mandanten veröffentlicht wurden.

## <a name="send-feedback"></a>Feedback senden

Wir sind auf Ihre Hilfe angewiesen, damit dieses Feature für alle Ihre Apps funktioniert. Wir durchsuchen mehr als 30 Tags in HTML und CSS und werten derzeit aus, welche JavaScript-Fälle unterstützt werden sollen. Wenn Sie ein Beispiel für generierte Links haben, die nicht übersetzt werden, senden Sie einen Codeausschnitt an [Anwendungsproxy-Feedback](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden von benutzerdefinierten Domänen mit Azure AD-Anwendungsproxy, damit diese dieselbe interne und externe URL aufweisen](active-directory-application-proxy-custom-domains.md)

