---
title: Problembehandlung bei dynamischen Mitgliedschaften in Gruppen | Microsoft-Dokumentation
description: "Tipps zur Problembehandlung für dynamische Mitgliedschaften in Gruppen in Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: c796550f4e47793285ba7d97cbb866c3820f32cf


---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problembehandlung bei dynamischen Mitgliedschaften für Gruppen
**Ich habe eine Regel für eine Gruppe konfiguriert, die Mitgliedschaften in der Gruppe werden jedoch nicht aktualisiert.**<br/>Überprüfen Sie auf der Registerkarte **Konfigurieren**, ob die Einstellung **Delegierte Gruppenverwaltung aktivieren** auf **Ja** festgelegt ist. Diese Einstellung ist für Sie nur sichtbar, wenn Sie als Benutzer angemeldet sind, dem eine Azure Active Directory Premium-Lizenz zugewiesen ist. Überprüfen Sie die Werte für Benutzerattribute in der Regel: Gibt es Benutzer, die die Regel erfüllen?

**Ich habe eine Regel konfiguriert, aber jetzt werden die vorhandenen Mitglieder der Regel entfernt.**<br/>Dieses Verhalten wird erwartet. Vorhandene Mitglieder der Gruppe werden entfernt, wenn eine Regel aktiviert oder geändert wird. Die nach der Auswertung der Regel verbleibenden Benutzer werden der Gruppe als Mitglieder hinzugefügt.     

**Warum werden Änderungen an der Mitgliedschaft nicht sofort angezeigt, wenn ich eine Regel hinzufüge oder ändere?**<br/> Die dedizierte Mitgliedschaftsauswertung erfolgt in regelmäßigen Abständen in einem asynchronen Hintergrundprozess. Die Dauer dieses Prozesses hängt von der Anzahl der Benutzer in Ihrem Verzeichnis und der Größe der Gruppe, die aufgrund der Regel erstellt wird, ab. Bei Verzeichnissen mit einer geringen Benutzeranzahl werden Änderungen der Gruppenmitgliedschaft üblicherweise innerhalb weniger Minuten angezeigt. Bei Verzeichnissen mit einer hohen Benutzeranzahl kann das Auffüllen 30 Minuten oder länger dauern.

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)




<!--HONumber=Dec16_HO4-->


