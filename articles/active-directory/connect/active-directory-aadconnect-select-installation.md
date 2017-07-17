---
title: "Azure AD Connect: Wählen Sie Ihren Installationstyp | Microsoft-Dokumentation"
description: "Dieses Thema führt Sie durch die Schritte zum Auswählen des Installationstyps für Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 81549c4bc0cabcf46bc7dc66d7e6662d7f787b1e
ms.openlocfilehash: df6353f84f898a1722e3e985244a90dbd6b22522
ms.contentlocale: de-de
ms.lasthandoff: 01/26/2017

---
# Auswählen des Installationstyps für Azure AD Connect
<a id="select-which-installation-type-to-use-for-azure-ad-connect" class="xliff"></a>
Azure AD Connect bietet zwei Installationstypen für die Neuinstallation: Express und benutzerdefiniert. Dieses Thema hilft Ihnen, zu entscheiden, welche Option Sie während der Installation verwenden.

## Express
<a id="express" class="xliff"></a>
Express ist die am häufigsten verwendete Option und wird bei etwa 90% aller neuen Installationen verwendet. Sie wurde entwickelt, um eine geeignete Konfiguration für die gängigsten Kundenszenarien bereitzustellen.

Sie setzt voraus:

- Sie haben eine einzelne lokale Active Directory-Gesamtstruktur.
- Sie haben ein Unternehmensadministratorkonto, das Sie für die Installation verwenden können.
- Ihr lokales Active Directory umfasst weniger als 100.000 Objekte.

Sie erhalten:

- [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) für lokales einmaliges Anmelden bei Azure AD.
- Eine Konfiguration, die [Benutzer, Gruppen, Kontakte und Windows 10-Computer](active-directory-aadconnectsync-understanding-default-configuration.md) synchronisiert.
- Synchronisierung aller berechtigten Objekte in allen Domänen und allen OEs.
- [Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) ist aktiviert, um sicherzustellen, dass Sie immer die neueste verfügbare Version verwenden.

Optionen, mit denen Sie Express weiterhin verwenden können:

- Wenn Sie nicht alle OEs synchronisieren möchten, können Sie Express weiterhin verwenden, wenn Sie auf der letzten Seite die Auswahl von **Synchronisierungsvorgang starten...*** aufheben. Führen Sie dann den Installations-Assistenten erneut aus, ändern Sie die Organisationseinheiten in [Konfigurationsoptionen](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options), und aktivieren Sie die geplante Synchronisierung.
- Vielleicht möchten Sie eines der Features in Azure AD Premium aktivieren, z.B. das Rückschreiben von Kennwörtern. Durchlaufen Sie zuerst Express, um die Erstinstallation abzuschließen. Führen Sie dann den Installations-Assistenten erneut aus, und ändern Sie die [Konfigurationsoptionen](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## Benutzerdefiniert
<a id="custom" class="xliff"></a>
Der benutzerdefinierte Pfad bietet viel mehr Optionen als Express. Er sollte in allen Fällen verwendet werden, wo die im vorherigen Abschnitt für Express beschriebene Konfiguration für Ihre Organisation nicht repräsentativ ist.

Die Verwendung empfiehlt sich unter folgenden Voraussetzungen:

- Sie haben in Active Directory keinen Zugriff auf ein Unternehmensadministratorkonto.
- Sie haben mehrere Gesamtstrukturen oder planen, zukünftig mehrere Gesamtstrukturen zu synchronisieren.
- Ihre Gesamtstruktur enthält Domänen, die vom Connect-Server aus nicht erreichbar sind.
- Sie planen, Verbund- oder Passthrough-Authentifizierung für die Benutzeranmeldung zu verwenden.
- Sie haben mehr als 100.000 Objekte und benötigen eine volle SQL Server-Instanz.
- Sie möchten gruppenbasierte Filterung und nicht nur domänen- oder OE-basierte Filterung verwenden.

## Upgrade von DirSync
<a id="upgrade-from-dirsync" class="xliff"></a>
Wenn Sie derzeit DirSync verwenden, befolgen Sie die Schritte in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) (Upgrade von DirSync), um Ihre vorhandene Konfiguration zu aktualisieren. Es gibt zwei verschiedene Upgradeoptionen:

- Direktes Upgrade, um Connect auf dem gleichen Server zu installieren.
- Parallele Bereitstellung, um Connect auf einem neuen Server zu installieren, während der vorhandene DirSync-Server immer noch betriebsbereit ist.

## Upgrade von Azure AD Sync
<a id="upgrade-from-azure-ad-sync" class="xliff"></a>
Wenn Sie derzeit Azure AD Sync verwenden, können Sie die [gleichen Schritte](active-directory-aadconnect-upgrade-previous-version.md) ausführen, als wenn Sie von einer Connect-Version auf eine neuere upgraden würden. Es gibt zwei verschiedene Upgradeoptionen:

- Direktes Upgrade, um Connect auf dem gleichen Server zu installieren.
- Swing-Migration, um Connect auf einem neuen Server zu installieren, während der vorhandene Azure AD Sync-Server immer noch betriebsbereit ist.

## Migrieren von FIM2010 oder MIM2016
<a id="migrate-from-fim2010-or-mim2016" class="xliff"></a>
Wenn Sie derzeit Forefront Identity Manager 2010 oder Microsoft Identity Manager 2016 mit dem Azure AD-Connector verwenden, ist Ihre einzige Option die Migration. Führen Sie die in [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration) (Swing-Migration) beschriebenen Schritte aus. Ersetzen Sie in den Schritten jede Erwähnung von Azure AD Sync durch FIM2010/MIM2016.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Verwenden Sie abhängig von der Option, die Sie ausgewählt haben, das Inhaltsverzeichnis auf der linken Seite, um Ihren Artikel mit den detaillierten Schritten zu finden.

