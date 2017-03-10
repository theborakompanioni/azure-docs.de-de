
---
title: Aktivieren von Enterprise State Roaming in Azure Active Directory | Microsoft Docs
description: "Enthält häufig gestellte Fragen zu Enterprise State Roaming-Einstellungen auf Windows-Geräten. Beim Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzeroberfläche auf allen Windows-Geräten und von einer Reduzierung der Zeit, die zum Konfigurieren eines neuen Geräts benötigt wird."
services: active-directory
keywords: Enterprise State Roaming, Windows-Cloud, Aktivieren von Enterprise State Roaming
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 0628dafc9cc4cb05e678f741d913e00116013459
ms.openlocfilehash: 435f801b254a6c423a6ec3e96037ade62e14af29
ms.lasthandoff: 02/11/2017


---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Aktivieren von Enterprise State Roaming in Azure Active Directory
Enterprise State Roaming ist für Organisationen mit einem Premium Azure Active Directory-Abonnement (Azure AD) verfügbar. Weitere Informationen zum Abrufen eines Azure AD-Abonnements finden Sie auf der [Azure AD-Produktseite](https://azure.microsoft.com/services/active-directory).

Wenn Sie Enterprise State Roaming aktivieren, werden Ihrer Organisation automatisch Lizenzen für ein kostenloses Azure Rights Management-Abonnement mit begrenzter Nutzung zugeteilt. Dieses kostenlose Abonnement ist ausschließlich auf das Ver- und Entschlüsseln von Einstellungen und Anwendungsdaten des Unternehmens beschränkt, die über den Enterprise State Roaming-Dienst synchronisiert werden. Sie müssen über ein kostenpflichtiges Abonnement verfügen, um die vollständigen Funktionen von Azure Rights Management nutzen zu können.

Führen Sie nach dem Erwerb eines Azure AD Premium-Abonnements diese Schritte aus, um Enterprise State Roaming zu aktivieren:

1. Melden Sie sich am klassischen Azure-Portal an.
2. Wählen Sie links die Option **ACTIVE DIRECTORY**und dann das Verzeichnis aus, für das Sie Enterprise State Roaming aktivieren möchten.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Klicken Sie oben auf die Registerkarte **Konfigurieren** .
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Scrollen Sie auf der Seite nach unten, und wählen Sie die Option **BENUTZER KÖNNEN EINSTELLUNGEN UND APP-DATEN DES UNTERNEHMENS SYNCHRONISIEREN**. Klicken Sie dann auf **SPEICHERN**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Damit für ein Windows 10-Gerät das Roaming von Einstellungen mit dem Enterprise State Roaming-Dienst möglich ist, muss das Gerät mit einer Azure AD-Identität authentifiziert werden. Für Geräte, die Azure AD beigetreten sind, ist die primäre Anmeldung des Benutzers die Azure AD-Identität. Es ist also keine zusätzliche Konfiguration erforderlich. Für Geräte, die das herkömmliche lokale Active Directory verwenden, muss der IT-Administrator [in die Domäne eingebundene Geräte mit Azure AD verbinden, um Windows 10-Funktionen zu nutzen](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Synchronisieren der Datenspeicherung
Enterprise State Roaming-Daten werden in [Azure-Regionen](https://azure.microsoft.com/regions/) gehostet, die am besten zu dem Wert für Land/Region passen, der in der Azure Active Directory-Instanz festgelegt ist. Enterprise State Roaming-Daten werden basierend auf drei großen geografischen Regionen partitioniert: Nordamerika, EMEA und APAC. Enterprise State Roaming-Daten für Mandanten befinden sich lokal in der geografischen Region und werden nicht über Regionen hinweg repliziert.  Beispiel: Bei Kunden, deren Wert für Land/Region auf ein EMEA-Land festgelegt ist (z.B. „Frankreich“ oder „Sambia“), werden die Daten in einer oder mehreren Azure-Regionen innerhalb von Europa gehostet.  Bei Kunden, deren Wert für Land/Region in Azure AD auf ein Land in Nordamerika festgelegt ist („USA“ oder „Kanada“), werden die Daten in einer oder mehreren Azure-Regionen innerhalb der USA gehostet.  Bei Kunden, deren Wert für Land/Region in Azure AD auf ein APAC-Land festgelegt ist („Australien“ oder „Neuseeland“), werden die Daten in einer oder mehreren Azure-Regionen innerhalb von Asien gehostet.  Daten aus südamerikanischen Ländern und der Antarktis werden in einer oder mehreren Azure-Regionen innerhalb der USA gehostet.  Der Wert für Land/Region wird im Rahmen der Azure AD-Verzeichniserstellung festgelegt und kann später nicht mehr geändert werden. 

Wenn Sie ausführliche Informationen zum Speicherort der Daten benötigen, können Sie beim [Azure-Support](https://azure.microsoft.com/support/options/)ein Ticket erstellen.

## <a name="manage-enterprise-state-roaming"></a>Verwalten von Enterprise State Roaming
Globale Azure AD-Administratoren können Enterprise State Roaming im klassischen Azure-Portal aktivieren und deaktivieren.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globale Administratoren können die Synchronisierung von Einstellungen auf bestimmte Sicherheitsgruppen beschränken.

Globale Administratoren können auch für jeden Benutzer einen Bericht zum Synchronisierungsstatus anzeigen, indem sie in der Active Directory-Instanz den gewünschten Benutzer in der Liste **BENUTZER** auswählen, auf die Registerkarte **GERÄTE** klicken und die Ansicht **Einstellungen für Gerätesynchronisierung und Daten zu Unternehmens-Apps** auswählen.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Beibehaltung von Daten
Daten, die per Enterprise State Roaming mit Azure synchronisiert werden, werden unendlich lange beibehalten, es sei denn, es wird ein manueller Löschvorgang durchgeführt, oder die betreffenden Daten werden als veraltet eingestuft. 

**Explizite Löschung** : Die Daten werden gelöscht, wenn ein Azure-Administrator einen Benutzer oder ein Verzeichnis löscht oder ein Administrator explizit die Löschung dieser Daten anfordert.

* **Löschung eines Benutzers**: Wenn ein Benutzer in Azure AD gelöscht wird, werden die Roamingdaten des Benutzerkontos zum Löschen markiert und innerhalb von 90 bis 180 Tagen gelöscht. 
* **Löschung eines Verzeichnisses:**Das Löschen eines gesamten Verzeichnisses in Azure AD ist ein unmittelbarer Vorgang. Alle diesem Verzeichnis zugeordneten Einstellungsdaten werden zum Löschen markiert und innerhalb von 90 bis 180 Tagen gelöscht. 
* **Löschung auf Anforderung:**Wenn der Azure AD-Administrator die Einstellungen oder Daten eines bestimmten Benutzers manuell löschen möchte, kann er beim [Azure-Support](https://azure.microsoft.com/support/)ein Ticket erstellen. 

**Löschung von veralteten Daten:**Daten, auf die ein Jahr lang nicht zugegriffen wurde (Aufbewahrungsdauer), werden als veraltet eingestuft und können aus Azure gelöscht werden. Die Aufbewahrungsdauer kann sich ändern, wird jedoch nicht weniger als 90 Tage betragen. Bei den veralteten Daten kann es sich um eine bestimmte Gruppe von Windows-/Anwendungseinstellungen oder um alle Einstellungen eines Benutzers handeln. Beispiel:

* Wenn keine Geräte auf eine bestimmte Sammlung mit Einstellungen zugreifen (z. B. wird eine Anwendung vom Gerät entfernt, oder eine Einstellungsgruppe wie „Design“ wird für alle Geräte eines Benutzers deaktiviert), gilt diese Sammlung nach Ablauf der Aufbewahrungsdauer als veraltet und kann gelöscht werden. 
* Falls ein Benutzer die Einstellungssynchronisierung auf allen eigenen Geräten deaktiviert hat, wird nicht auf die Einstellungsdaten zugegriffen. Alle Einstellungsdaten für diesen Benutzer werden als veraltet eingestuft und können nach Ablauf der Aufbewahrungsdauer gelöscht werden. 
* Wenn der Azure AD-Verzeichnisadministrator Enterprise State Roaming für das gesamte Verzeichnis deaktiviert hat, wird die Synchronisierung der Einstellungen für alle Benutzer im jeweiligen Verzeichnis beendet. Alle Einstellungsdaten aller Benutzer gelten dann als veraltet und können nach Ablauf der Aufbewahrungsdauer gelöscht werden. 

**Wiederherstellung gelöschter Daten:**Die Richtlinie für die Datenaufbewahrung ist nicht konfigurierbar. Nachdem die Daten endgültig gelöscht wurden, können sie nicht wiederhergestellt werden. Beachten Sie hierbei aber, dass die Einstellungsdaten nur aus Azure gelöscht werden, nicht vom Gerät des Endbenutzers. Falls mit einem Gerät zu einem späteren Zeitpunkt wieder eine Verbindung mit dem Enterprise State Roaming-Dienst hergestellt wird, werden die Einstellungen wieder synchronisiert und in Azure gespeichert.

## <a name="related-topics"></a>Verwandte Themen
* [Übersicht über Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Roaming von Einstellungen und Daten – Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10-Roamingeinstellungen – Referenz](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Problembehandlung](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

