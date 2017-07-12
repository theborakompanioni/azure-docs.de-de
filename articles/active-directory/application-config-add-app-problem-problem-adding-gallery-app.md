---
title: "Probleme beim Hinzufügen einer Azure AD-Kataloganwendung | Microsoft-Dokumentation"
description: "Informationen zu den häufig auftretenden Problemen beim Hinzufügen von Azure AD-Kataloganwendungen und zur Behebung dieser Probleme"
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
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: b429f5fc1be24e63b0e9e065f879f0b0e389403c
ms.contentlocale: de-de
ms.lasthandoff: 04/11/2017

---

<a id="problem-adding-an-azure-ad-gallery-application" class="xliff"></a>

# Probleme beim Hinzufügen einer Azure AD-Kataloganwendung

In diesem Artikel finden Sie Informationen zu den häufig auftretenden Problemen beim Hinzufügen von Azure AD-Kataloganwendungen sowie zu deren Behebung.

<a id="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear" class="xliff"></a>

## Nach dem Klicken auf die Schaltfläche „Hinzufügen“ wurde meine Anwendung erst nach einiger Zeit angezeigt.

Unter bestimmten Umständen kann es 1 bis 2 Minuten (manchmal auch länger) dauern, bis eine Anwendung angezeigt wird, nachdem sie Ihrem Verzeichnis hinzugefügt wurde. Auch wenn dies nicht der normalen Erwartung entspricht, können Sie sehen, dass die Hinzufügung der Anwendung durchgeführt wird, indem Sie auf das Symbol **Benachrichtigungen** (Glockensymbol) oben rechts im [Azure-Portal](https://portal.azure.com/) klicken und nach einer Benachrichtigung des Typs **In Bearbeitung** oder **Abgeschlossen** mit der Bezeichnung **Anwendung erstellen** suchen.

Wenn die Anwendung nicht hinzugefügt wird oder wenn beim Klicken auf die Schaltfläche **Hinzufügen** ein Fehler auftritt, wird eine **Benachrichtigung** mit einem **Fehlerstatus** angezeigt. Wenn Sie weitere Details zu dem Fehler benötigen, um diese einem Supporttechniker mitzuteilen, können Sie ausführliche Informationen zum Fehler anzeigen, indem Sie die Schritte im Abschnitt [Anzeigen von Details einer Portalbenachrichtigung](#how-to-see-the-details-of-a-portal-notification) ausführen.

<a id="i-clicked-the-add-button-and-my-application-didnt-appear" class="xliff"></a>

## Nach dem Klicken auf die Schaltfläche „Hinzufügen“ wurde meine Anwendung nicht angezeigt

In manchen Fällen führen vorübergehende Probleme, Netzwerkprobleme oder ein Fehler dazu, dass eine Anwendung nicht hinzugefügt wird. Dies können Sie feststellen, wenn Sie oben rechts im Azure-Portal auf das Symbol **Benachrichtigungen** (Glockensymbol) klicken und ein rotes (!)-Symbol neben der Benachrichtigung **Anwendung erstellen** angezeigt wird. Dies gibt an, dass beim Erstellen der Anwendung ein Fehler aufgetreten ist.

Wenn beim Klicken auf die Schaltfläche **Hinzufügen** ein Fehler auftritt, wird eine **Benachrichtigung** mit einem **Fehlerstatus** angezeigt. Wenn Sie weitere Details zu dem Fehler benötigen, um diese einem Supporttechniker mitzuteilen, können Sie ausführliche Informationen zum Fehler anzeigen, indem Sie die Schritte im Abschnitt [Anzeigen von Details einer Portalbenachrichtigung](#how-to-see-the-details-of-a-portal-notification) ausführen.

<a id="i-dont-know-how-to-set-up-my-application-once-ive-added-it" class="xliff"></a>

 ## Ich weiß nicht, wie die Anwendung nach dem Hinzufügen eingerichtet wird.

Für allgemeine Informationen zu Anwendungen bietet sich der Artikel [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) als Ausgangspunkt an.

Zudem finden Sie weitere Informationen zur Verwendung und Funktionsweise des einmaligen Anmeldens mit Azure AD in der [Dokumentbibliothek zu Azure AD-Anwendungen](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index).

<a id="how-to-see-the-details-of-a-portal-notification" class="xliff"></a>

## Anzeigen von Details einer Portalbenachrichtigung

Sie können die Details von Portalbenachrichtigungen anzeigen, indem Sie folgende Schritte ausführen:

1.  Klicken Sie in der rechten oberen Ecke des Azure-Portals auf das Symbol **Benachrichtigungen** (Glockensymbol).

2.  Wählen Sie eine Benachrichtigung mit einem **Fehlerstatus** aus (mit einem roten (!) neben dem Namen).

    >[!NOTE]
    >Sie können auf keine Benachrichtigungen klicken, die den Status **Erfolgreich** oder **In Bearbeitung** aufweisen.
    >
    >

3.  Hiermit öffnen Sie das Blatt **Benachrichtigungsdetails**.

4.  Diese Informationen helfen Ihnen, das Problem besser zu verstehen.

5.  Wenn Sie weitere Unterstützung benötigen, können Sie diese Informationen auch einem Supporttechniker oder der Produktgruppe mitteilen, um Hilfe bei Ihrem Problem zu erhalten.

6.  Klicken Sie auf das **Symbol** **Kopieren**rechts neben dem Textfeld **Fehler kopieren**, um alle Benachrichtigungsdetails zu kopieren und einem Support- oder Produktgruppentechniker mitzuteilen.

<a id="how-to-get-help-by-sending-notification-details-to-a-support-engineer" class="xliff"></a>

## Erhalten von Unterstützung durch Senden von Benachrichtigungsdetails an einen Supporttechniker

Es ist sehr wichtig, dass Sie dem Supporttechniker **alle unten aufgelisteten Details** mitteilen, wenn Sie Hilfe benötigen, damit dieser schnell helfen kann. Hierfür können Sie ganz einfach **einen Screenshot erstellen**, oder Sie klicken auf das Symbol **Fehler kopieren** rechts neben dem Textfeld **Fehler kopieren**.

<a id="notification-details-explained" class="xliff"></a>

## Erläuterung der Benachrichtigungsdetails

Im Folgenden wird die Bedeutung der einzelnen Benachrichtigungselemente näher erläutert, und es werden Beispiele genannt.

<a id="essential-notification-items" class="xliff"></a>

### Grundlegende Benachrichtigungselemente

-   **Titel:** der beschreibende Titel der Benachrichtigung

  * Beispiel: **Anwendungsproxyeinstellungen**

-   **Beschreibung:** Beschreibung des Ergebnisses des Vorgangs

    -   Beispiel: **Die eingegebene interne URL wird bereits von einer anderen Anwendung verwendet**

-   **Benachrichtigungs-ID:** die eindeutige ID der Benachrichtigung

    -   Beispiel: **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Clientanforderungs-ID:** die spezifische Anforderungs-ID, die vom Browser erstellt wurde

    -   Beispiel: **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Zeitstempel (UTC):** der Zeitstempel beim Auftreten der Benachrichtigung in UTC

    -   Beispiel: **2017-03-23T19:50:43.7583681Z**

-   **Interne Transaktions-ID:** die interne ID, über die wir den Fehler in unseren Systemen suchen können

    -   Beispiel: **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN:** der Benutzer, der den Vorgang durchgeführt hat

    -   Beispiel: **tperkins@f128.info**

-   **Mandanten-ID:** die eindeutige ID des Mandanten, bei dem der Benutzer Mitglied ist, der den Vorgang durchgeführt hat

    -   Beispiel: **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Benutzerobjekt-ID:** die eindeutige ID des Benutzers, der den Vorgang durchgeführt hat

    -   Beispiel: **17f84be4-51f8-483a-b533-383791227a99**

<a id="detailed-notification-items" class="xliff"></a>

### Detaillierte Benachrichtigungselemente

-   **Anzeigename:** **(kann leer sein)** ein detaillierterer Anzeigename für den Fehler

    -   Beispiel: **Anwendungsproxyeinstellungen**

-   **Status:** der spezifische Status der Benachrichtigung

    -   Beispiel: **Fehler**

-   **Objekt-ID:** **(kann leer sein)** die Objekt-ID, für die der Vorgang durchgeführt wurde

    -   Beispiel: **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Details:** detaillierte Beschreibung des Ergebnisses des Vorgangs

    -   Beispiel: **Interne URL 'http://bing.com/' ist ungültig, da sie bereits verwendet wird**

-   **Fehler kopieren:** Klicken Sie auf das **Kopiersymbol** rechts neben dem Textfeld **Fehler kopieren**, um alle Benachrichtigungsdetails zu kopieren und einem Support- oder Produktgruppentechniker mitzuteilen.

    -   Beispiel: ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Verwalten von Anwendungen mit Azure Active Directory](active-directory-enable-sso-scenario.md)

