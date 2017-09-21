---
title: Einrichten der Self-Service-Verwaltung des Anwendungszugriffs in Azure Active Directory | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory erstellen und verwalten und Mitgliedschaften in Sicherheits- oder Office 365-Gruppen anfordern.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 7a7370eb076ba8602a58a260a14bb863c55bc803
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="set-up-azure-active-directory-for-self-service-group-management"></a>Einrichten von Azure Active Directory für die Self-Service-Gruppenverwaltung
Ihre Benutzer können ihre eigenen Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory (Azure AD) erstellen und verwalten. Benutzer können auch Mitgliedschaften in Sicherheitsgruppen oder Office 365-Gruppen anfordern. Der Besitzer der Gruppe kann dann die Mitgliedschaft genehmigen oder verweigern. Die tagtägliche Steuerung der Gruppenmitgliedschaft kann an Personen delegiert werden, die den Geschäftskontext für die betreffende Mitgliedschaft verstehen. Self-Service-Funktionen zur Gruppenverwaltung sind nur für Sicherheitsgruppen und Office 365-Gruppen, aber nicht für E-Mail-aktivierte Sicherheitsgruppen oder Verteilerlisten verfügbar.

Die Self-Service-Gruppenverwaltung gilt derzeit für zwei grundlegende Szenarien: delegierte Gruppenverwaltung und Self-Service-Gruppenverwaltung.

* **Delegierte Gruppenverwaltung** – Beispielsweise ein Administrator, der den Zugriff auf eine SaaS-Anwendung verwaltet, die im Unternehmen verwendet wird. Die Verwaltung dieser Zugriffsrechte wird immer mühsamer, und der Administrator bittet den Geschäftsinhaber, eine neue Gruppe zu erstellen. Der Administrator weist den Zugriff auf die Anwendung der neuen Gruppe zu und fügt der Gruppe alle Personen zu, die bereits auf die Anwendung zugreifen. Der Geschäftsinhaber kann dann weitere Benutzer hinzufügen, und die Benutzer erhalten automatisch Zugriff auf die Anwendung. Der Geschäftsinhaber muss nicht warten, bis der Administrator die Verwaltungsschritte für den Zugriff der Benutzer durchgeführt hat. Wenn der Administrator einem Manager in einer anderen Geschäftseinheit die gleiche Berechtigung erteilt, kann diese Person ebenfalls den Zugriff für ihre eigenen Benutzer verwalten. Weder der Geschäftsinhaber noch der Manager kann die Benutzer des jeweils anderen anzeigen oder verwalten. Der Administrator kann weiterhin alle Benutzer anzeigen, die auf die Anwendung zugreifen können, und kann die Zugriffsrechte bei Bedarf blockieren.
* **Self-Service-Gruppenverwaltung** Ein Beispiel für dieses Szenario sind zwei Benutzer, die beide über unabhängig voneinander eingerichtete SharePoint Online-Websites verfügen. Sie möchten dem anderen Team jeweils Zugriff auf ihre Websites gewähren. Hierzu können sie eine Gruppe in Azure AD erstellen, und in SharePoint Online wählt jeder der beiden Benutzer diese Gruppe dann aus, um Zugriff auf seine Websites bereitzustellen. Wenn ein weiterer Benutzer Zugriff benötigt, fordert er diesen einfach über den Zugriffsbereich an und erhält nach Genehmigung automatisch Zugriff auf beide SharePoint Online-Websites. Später entscheidet einer der Hauptbenutzer, dass alle Personen, die auf die Website zugreifen, auch Zugriff auf eine bestimmte SaaS-Anwendung erhalten sollen. Der Administrator der SaaS-Anwendung kann der SharePoint Online-Website Zugriffsrechte für die Anwendung hinzufügen. Anschließend können alle Personen, deren Zugriff genehmigt wird, auf die beiden SharePoint Online-Websites und diese SaaS-Anwendung zugreifen.

## <a name="make-a-group-available-for-user-self-service"></a>Einrichten einer Gruppe für Self-Service durch Benutzer
1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit dem Konto eines globalen Administrators für das Verzeichnis an.
2. Wählen Sie **Benutzer und Gruppen** und dann **Gruppeneinstellungen**.
3. Legen Sie **Self-Service-Gruppenverwaltung aktiviert** auf **Ja** fest.
4. Legen Sie **Benutzer können Sicherheitsgruppen erstellen** oder **Benutzer können Office 365-Gruppen erstellen** auf **Ja** fest.
  * Wenn diese Einstellungen aktiviert sind, können alle Benutzer in Ihrem Verzeichnis neue Sicherheitsgruppen erstellen und diesen Gruppen Mitglieder hinzufügen. Diese neuen Gruppen werden auch für alle Benutzer im Zugriffsbereich angezeigt. Wenn die Richtlinieneinstellung der Gruppe dies zulässt, können andere Benutzer Anforderungen in Bezug auf den Beitritt zu diesen Gruppen erstellen. 
  * Falls diese Einstellungen deaktiviert sind, können Benutzer keine Gruppen erstellen und keine vorhandenen Gruppen ändern, deren Besitzer sie sind. Sie können aber trotzdem die Mitgliedschaften dieser Gruppen verwalten und Anforderungen anderer Benutzer zum Beitreten zu diesen Gruppen genehmigen.

Sie können auch die Optionen **Benutzer, die Sicherheitsgruppen verwalten können** und **Benutzer, die Office 365-Gruppen verwalten können** verwenden, um für die Self-Service-Gruppenverwaltung Ihrer Benutzer eine feinere Zugriffssteuerung zu erzielen. Wenn die Option **Benutzer können Gruppen erstellen** aktiviert ist, können alle Benutzer in Ihrem Mandanten neue Gruppen erstellen und diesen Mitglieder hinzufügen. Wenn Sie **Some** (Einige) wählen, beschränken Sie die Gruppenverwaltung nur auf eine begrenzte Gruppe von Benutzern. Wenn dieser Switch auf **Some** (Einige) festgelegt ist, müssen Sie Benutzer der Gruppe SSGMSecurityGroupsUsers hinzufügen, bevor diese neue Gruppen erstellen und Mitglieder hinzufügen können. Wenn Sie die Option **Benutzer, die Self-Service für Sicherheitsgruppen verwenden** und **Benutzer, die Office 365-Gruppen verwalten können** auf **Alle** festlegen, können alle Benutzer in Ihrem Mandanten neue Gruppen erstellen.

Sie können auch die Option **Gruppe, die Sicherheitsgruppen verwalten kann** oder **Gruppe, die Office 365-Gruppen verwalten kann** verwenden, um eine einzelne Gruppe anzugeben, deren Mitglieder den Self-Service nutzen können.

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

