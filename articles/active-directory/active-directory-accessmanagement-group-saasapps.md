---
title: Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Gruppen in Azure Active Directory Premium oder Basic den Zugriff auf SaaS-Anwendungen zuweisen, die in Azure Active Directory integriert sind.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 818f4b515926c35078b3118978f3accbf3bbb65b
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen
Mit Azure Active Directory (Azure AD) mit einer Azure AD Premium- oder Azure AD-Basic-Lizenz können Sie Gruppen verwenden, um Zugriff auf eine SaaS-Anwendung zu gewähren, die in Azure AD integriert ist. Wenn Sie beispielsweise der Marketingabteilung Zugriff auf fünf verschiedene SaaS-Anwendungen erteilen möchten, können Sie eine Gruppe erstellen, die die Benutzer der Marketingabteilung enthält, und diese Gruppe dann diesen fünf SaaS-Anwendungen zuweisen, die von der Marketingabteilung benötigt werden. Auf diese Weise sparen Sie Zeit, da Sie die Mitgliedschaft für die Marketingabteilung zentral verwalten können. Benutzer werden der Anwendung zugewiesen, wenn sie als Mitglieder der Marketinggruppe hinzugefügt werden. Ebenso wird die Zuweisung der Benutzer zur Anwendung entfernt, wenn sie als Mitglieder aus der Marketinggruppe entfernt werden. Diese Funktion kann für Hunderte von Anwendungen verwendet werden, die Sie aus dem Azure AD-Anwendungskatalog hinzufügen können.

> [!IMPORTANT]
> Sie können diese Funktionen nur verwenden, nachdem Sie eine Azure AD Premium-Testversion gestartet oder Azure AD Premium- bzw. Azure AD Basic-Lizenzen erworben haben.
> Geschachtelte Gruppenmitgliedschaften werden für die gruppenbasierte Zuordnung zu Anwendungen derzeit nicht unterstützt.

**So weisen Sie einem Benutzer oder einer Gruppe den Zugriff auf eine SaaS-Anwendung zu**

1. Wählen Sie im [Azure AD Admin Center](https://aad.portal.azure.com) die Option **Unternehmensanwendungen**.
2. Wählen Sie eine der aus dem Anwendungskatalog hinzugefügten Anwendungen aus, um diese zu öffnen.
3. Wählen Sie **Benutzer und Gruppen** und dann **Alle Benutzer**.
4. Wählen Sie unter **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**, um die Auswahlliste **Benutzer und Gruppen** zu öffnen.
6. Wählen Sie beliebig viele Gruppen oder Benutzer aus, und klicken oder tippen Sie dann auf **Auswählen**, um diese der Liste **Zuweisung hinzufügen** hinzuzufügen. An dieser Stelle können Sie auch einem Benutzer eine Rolle zuweisen.
7. Wählen Sie **Zuweisen**, um die Benutzer oder Gruppen der ausgewählten Unternehmensanwendung zuzuweisen.

### <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

