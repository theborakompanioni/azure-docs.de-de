---
title: Aufheben des Zugriffs eines Benutzers auf eine Anwendung | Microsoft-Dokumentation
description: Informationen zum Aufheben des Zugriffs eines Benutzers auf eine Anwendung
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
ms.openlocfilehash: 8d4f2cec35a8edfec9b8830a077b8aa65ca0c229
ms.contentlocale: de-de
ms.lasthandoff: 04/11/2017

---

<a id="how-to-remove-a-users-access-to-an-application" class="xliff"></a>

# Aufheben des Zugriffs eines Benutzers auf eine Anwendung

In diesem Artikel erhalten Sie Informationen zum Aufheben des Zugriffs eines Benutzers auf eine Anwendung.

<a id="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application" class="xliff"></a>

## Ich möchte die Zuweisung eines bestimmten Benutzers oder einer bestimmten Gruppe zu einer Anwendung aufheben.

Um eine Benutzer- oder Gruppenzuweisung zu einer Anwendung aufzuheben, führen Sie die im Artikel [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) beschriebenen Schritte aus.

.## Ich möchte den gesamten Zugriff auf eine Anwendung für alle Benutzer deaktivieren.

Um alle Benutzeranmeldungen bei einer Anwendung zu deaktivieren, führen Sie die im Artikel [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) beschriebenen Schritte aus.

<a id="i-want-to-delete-an-application-entirely" class="xliff"></a>

## Ich möchte eine Anwendung vollständig löschen.

Befolgen Sie die nachstehenden Anweisungen, um **eine Anwendung zu löschen**:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, die Sie löschen möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie auf dem Blatt **Übersicht** der Anwendung auf **Löschen**.

<a id="i-want-to-disable-all-future-user-consent-operations-to-any-application" class="xliff"></a>

## Ich möchte alle zukünftigen Vorgänge für die Benutzerzustimmung für jede Anwendung deaktivieren.

Die Deaktivierung der Benutzerzustimmung für das gesamte Verzeichnis führt dazu, dass Endbenutzer keiner Anwendung zustimmen können. Administratoren können weiterhin im Namen von Benutzern zustimmen. Weitere Informationen zur Anwendungszustimmung sowie zum Aktivieren bzw. Deaktivieren der Zustimmung finden Sie unter [Grundlegendes zur Benutzer- und Administratorzustimmung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Befolgen Sie die nachstehenden Anweisungen, um **alle zukünftigen Vorgänge für die Benutzerzustimmung in Ihrem gesamten Verzeichnis zu deaktivieren**:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Benutzereinstellungen**.

6.  Deaktivieren Sie alle zukünftigen Vorgänge für die Benutzerzustimmung, indem Sie **Benutzer können Apps Zugriff auf ihre Daten gewähren** auf **Nein** festlegen und dann auf die Schaltfläche **Speichern** klicken.


<a id="next-steps" class="xliff"></a>

# Nächste Schritte
[Verwalten des Zugriffs auf Apps](active-directory-managing-access-to-apps.md)

