---
title: Anzeige von Anwendungen im Zugriffsbereich | Microsoft-Dokumentation
description: Beheben des Problems, dass eine Anwendung im Zugriffsbereich angezeigt wird
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
ms.openlocfilehash: 2429ebef69aaddf28d10cd77bf4ce9072ea71476
ms.contentlocale: de-de
ms.lasthandoff: 04/11/2017

---

<a id="how-applications-appear-on-the-access-panel" class="xliff"></a>

# Anzeige von Anwendungen im Zugriffsbereich

Der Zugriffsbereich ist ein webbasiertes Portal, in dem Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. Diese Anwendungen werden im Namen des Benutzers im Azure AD-Portal konfiguriert. Der Administrator kann die Anwendung für den Benutzer direkt oder für eine Gruppe bereitstellen, zu der der Benutzer gehört. Beide Verfahren führen dazu, dass die Anwendung im Zugriffsbereich des Benutzers angezeigt wird.

<a id="general-issues-to-check-first" class="xliff"></a>

## Allgemeine Probleme, die zuerst überprüft werden sollten

-   Wenn eine Anwendung kürzlich für einen Benutzer oder eine Gruppe entfernt wurde, zu der der Benutzer gehört, sollte sich der Benutzer vom Zugriffsbereich abmelden und nach einigen Minuten wieder anmelden, um festzustellen, ob die Anwendung entfernt wurde.

-   Wenn eine Lizenz kürzlich für einen Benutzer oder eine Gruppe entfernt wurde, zu der der Benutzer gehört, kann es je nach Größe und Komplexität der Gruppe lange dauern, bis die Änderungen durchgeführt wurden. Lassen Sie vor der erneuten Anmeldung beim Zugriffsbereich einige Zeit verstreichen.

<a id="problems-related-to-assigning-applications-to-users" class="xliff"></a>

## Probleme beim Zuweisen von Anwendungen zu Benutzern

Möglicherweise wird eine Anwendung im Zugriffsbereich eines Benutzers angezeigt, da der Benutzer der Anwendung zuvor zugewiesen war. Nachfolgend sind einige Möglichkeiten aufgeführt, dies zu überprüfen:

-   [Überprüfen, ob der Benutzer der Anwendung zugewiesen ist](#check-if-a-user-is-assigned-to-the-application)

-   [Überprüfen, ob dem Benutzer eine Lizenz für die Anwendung zugewiesen ist](#check-if-a-user-is-under-a-license-related-to-the-application)


<a id="check-if-a-user-is-assigned-to-the-application" class="xliff"></a>

### Überprüfen, ob der Benutzer der Anwendung zugewiesen ist

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Benutzer der Anwendung zugewiesen ist:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

6.  **Suchen** Sie den Namen der betreffenden Anwendung.

7.  Klicken Sie auf **Benutzer und Gruppen**.

8.  Überprüfen Sie , ob der Benutzer der Anwendung zugewiesen ist.

  * Wenn Sie den Benutzer aus der Anwendung entfernen möchten, **klicken Sie auf die Zeile** des Benutzers, und wählen Sie **Löschen** aus.

<a id="check-if-a-user-is-under-a-license-related-to-the-application" class="xliff"></a>

### Überprüfen, ob dem Benutzer eine Lizenz für die Anwendung zugewiesen ist

Führen Sie die folgenden Schritte aus, um die Lizenzen zu überprüfen, die einem Benutzer zugewiesen sind:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem gewünschten Benutzer, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen dem Benutzer derzeit zugewiesen sind.

   * Wenn der Benutzer einer Office-Lizenz zugewiesen ist, können im Zugriffsbereich des Benutzers Erstanbieter-Office-Anwendungen angezeigt werden.

<a id="problems-related-to-assigning-applications-to-groups" class="xliff"></a>

## Probleme beim Zuweisen von Anwendungen zu Gruppen

Möglicherweise wird eine Anwendung im Zugriffsbereich eines Benutzers angezeigt, da der Benutzer Mitglied einer Gruppe ist, der die Anwendung zugewiesen ist. Nachfolgend sind einige Möglichkeiten aufgeführt, dies zu überprüfen:

-   [Überprüfen der Gruppenmitgliedschaften eines Benutzers](#check-a-users-group-memberships)

-   [Überprüfen, ob ein Benutzer Mitglied einer Gruppe ist, die einer Lizenz zugewiesen ist](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

<a id="check-a-users-group-memberships" class="xliff"></a>

### Überprüfen der Gruppenmitgliedschaften eines Benutzers

Führen Sie die folgenden Schritte aus, um die Mitgliedschaft einer Gruppe zu überprüfen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem gewünschten Benutzer, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Gruppen**.

8.  Überprüfen Sie, ob der Benutzer einer Gruppe angehört, die der Anwendung zugewiesen ist.

   * Wenn Sie den Benutzer aus der Gruppe entfernen möchten, **klicken Sie auf die Zeile** der Gruppe, und wählen Sie „Löschen“ aus.

<a id="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license" class="xliff"></a>

### Überprüfen, ob ein Benutzer Mitglied einer Gruppe ist, die einer Lizenz zugewiesen ist

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem gewünschten Benutzer, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Gruppen**.

8.  Klicken Sie auf die Zeile für eine bestimmte Gruppe.

9.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen der Gruppe zugewiesen sind.

  * Wenn die Gruppe einer Office-Lizenz zugewiesen ist, können im Zugriffsbereich des Benutzers bestimmte Erstanbieter-Office-Anwendungen angezeigt werden.


<a id="if-these-troubleshooting-steps-do-not-the-resolve-the-issue" class="xliff"></a>

## Wenn das Problem mit diesen Problembehandlungsschritten nicht behoben wird

Öffnen Sie ein Supportticket mit den folgenden Informationen, sofern verfügbar:

-   Fehlerkorrelations-ID

-   UPN (E-Mail-Adresse des Benutzers)

-   Mandanten-ID

-   Browsertyp

-   Zeitzone und Uhrzeit/Zeitraum des Auftretens des Fehlers

-   Fiddler-Ablaufverfolgungen

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Verwalten von Anwendungen mit Azure Active Directory](active-directory-enable-sso-scenario.md)

