---
title: "Verwaltung der Benutzerbereitstellung für Unternehmens-Apps in der Azure Active Directory-Vorschau | Microsoft Docs"
description: "Erfahren Sie, wie Sie mithilfe der Azure Active Directory-Vorschau die Benutzerkontobereitstellung für Unternehmens-Apps verwalten."
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/12/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 103eade46452d979705e06dd77441f42d7a514b8


---
# <a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Vorschau: Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im neuen Azure-Portal
Dieser Artikel beschreibt die Verwendung des [Azure-Portals](https://portal.azure.com) zur Verwaltung der automatischen Bereitstellung und der Aufhebung der Bereitstellung von Benutzerkonten für Anwendungen, die dies unterstützen – insbesondere für Anwendungen, die aus der Kategorie „Vorgestellt“ des [Azure Active Directory-Anwendungskatalogs](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) hinzugefügt wurden. Diese Verwaltungsfunktionen im neuen Azure-Portal liegen derzeit als öffentliche Vorschau vor, und dieser Artikel beschreibt die neuen Funktionen sowie einige temporäre Einschränkungen, die während des Vorschauzeitraums gelten. [Was enthält die Vorschauversion?](active-directory-preview-explainer.md)

Weitere Informationen zur automatisierten Benutzerbereitstellung sowie zu deren Funktionsweise finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-the-new-portal"></a>Suchen Ihrer Apps im neuen Portal
Ab September 2016 können alle Anwendungen, die von einem Verzeichnisadministrator mithilfe des [Azure Active Directory-Anwendungskatalogs](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) innerhalb des [klassischen Azure-Portals](https://manage.windowsazure.com) für einmaliges Anmelden konfiguriert wurden, im neuen Azure-Portal angezeigt und verwaltet werden.

Diese Anwendungen finden Sie im Abschnitt **Unternehmensanwendungen** des neuen Azure-Portals. Dieser kann über das Menü **Weitere Dienste** im linken Navigationsbereich aufgerufen werden. Unternehmens-Apps sind Apps, die bereitgestellt wurden und von Benutzern in Ihrer Organisation verwendet werden.

![Blatt „Unternehmensanwendungen“][0]

Wählen Sie auf der linken Seite den Link **All applications** (Alle Anwendungen), um eine Liste aller konfigurierten Apps anzuzeigen, einschließlich Apps, die aus dem Katalog hinzugefügt wurden. Beim Auswählen einer App wird das Ressourcenblatt die App geladen, in dem Berichte für App angezeigt und eine Reihe von Einstellungen verwaltet werden können.

Einstellungen für die Bereitstellung von Benutzerkonten können durch Auswählen der Option **Bereitstellung** auf der linken Seite verwaltet werden.

![Blatt „Anwendungsressource“][1]

## <a name="provisioning-modes"></a>Bereitstellungsmodi
Das Blatt **Bereitstellung** beginnt mit dem Menü **Modus**, das anzeigt, welche Bereitstellungsmodi für eine Unternehmensanwendung unterstützt werden, und deren Konfiguration ermöglicht. Die verfügbaren Optionen umfassen:

* **Automatisch**: Diese Option wird angezeigt, wenn Azure AD die automatische API-basierte Bereitstellung bzw. das Aufheben der Bereitstellung von Benutzerkonten für diese Anwendung unterstützt. Bei Auswahl dieses Modus wird eine Schnittstelle angezeigt, die Administratoren durch folgende Aufgaben führt: Konfiguration von Azure AD zum Herstellen einer Verbindung mit der Benutzerverwaltungs-API der Anwendung, Erstellen von Kontozuordnungen und -workflows, die den Fluss von Benutzerkontodaten zwischen Azure AD und der App definieren und Verwaltung des Azure AD-Bereitstellungsdiensts.
* **Manuell** : Diese Option wird angezeigt, wenn Azure AD die automatische Bereitstellung von Benutzerkonten für diese Anwendung nicht unterstützt. Diese Option bedeutet, dass in der Anwendung gespeicherte Benutzerkontodatensätze basierend auf den Benutzerverwaltungs- und Bereitstellungsfunktionen dieser Anwendung (einschließlich SAML Just-in-Time-Bereitstellung) mit einem externen Prozess verwaltet werden müssen.

## <a name="configuring-automatic-user-account-provisioning"></a>Konfigurieren der automatischen Bereitstellung von Benutzerkonten
Bei Auswahl der Option **Automatisch** wird ein Bildschirm angezeigt, der in vier Abschnitte unterteilt ist:

### <a name="admin-credentials"></a>Administratoranmeldeinformationen
Hier müssen die Anmeldeinformationen für Azure AD eingegeben werden, um eine Verbindung mit der Benutzerverwaltungs-API der Anwendung herzustellen. Die erforderliche Eingabe variiert je nach Anwendung. Informationen zu den Anmeldeinformationstypen und den Anforderungen für bestimmte Anwendungen finden Sie im [Konfigurationstutorial der jeweiligen Anwendung](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Durch Auswählen der Schaltfläche **Verbindung testen** können Sie die Anmeldeinformationen testen, indem Azure AD versucht, mit den angegebenen Anmeldeinformationen eine Verbindung mit der Bereitstellungs-App der App herzustellen.

### <a name="mappings"></a>Zuordnungen
Hier können Administratoren anzeigen und bearbeiten, welche Benutzerattribute zwischen Azure AD und der Zielanwendung fließen, wenn Benutzerkonten bereitgestellt oder aktualisiert werden.

Zwischen Azure AD-Benutzerobjekten und den Benutzerobjekten der einzelnen SaaS-Apps ist eine vorkonfigurierte Sammlung von Zuordnungen vorhanden. Einige Apps verwalten andere Objekttypen, z. B. Gruppen oder Kontakte. Bei Auswahl einer dieser Zuordnungen in die Tabelle wird rechts der Mapping-Editor geöffnet, in dem Zuordnungen angezeigt und angepasst werden können.

![Blatt „Anwendungsressource“][2]

Zu den unterstützten Anpassungen während der ersten Vorschau gehören:

* Aktivieren und Deaktivieren von Zuordnungen für bestimmte Objekte, z. B. das Azure AD-Benutzerobjekt an das Benutzerobjekt der SaaS-App.
* Bearbeiten der Attribute, die vom Azure AD-Benutzerobjekt an das Benutzerobjekt der App übermittelt werden. Weitere Informationen zur Attributzuordnung finden Sie unter [Grundlegendes zu Attributzuordnungstypen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Filtern der Bereitstellungsaktionen, die Azure AD für die Zielanwendung ausführen soll; dies ist eine neue Funktion im Azure-Portal. Anstatt eine vollständige Synchronisierung von Objekten durch Azure AD zuzulassen können Sie die ausgeführten Aktionen beschränken. Beispiel: Wenn Sie nur **Aktualisieren**auswählen, aktualisiert Azure AD nur vorhandene Benutzerkonten in einer Anwendung und erstellt keine neuen. Wird nur **Erstellen**ausgewählt, erstellt Azure nur neue Benutzerkonten, aktualisiert jedoch keine vorhandenen Konten. Dieses Feature ermöglicht Administratoren das Erstellen verschiedener Zuordnungen für die Kontoerstellung und das Aktualisieren von Workflows. Die uneingeschränkte Möglichkeit zum Erstellen mehrerer Zuordnungen pro App ist für einen späteren Zeitpunkt im Vorschauzeitraum geplant.

### <a name="settings"></a>Einstellungen
In diesem Abschnitt können Administratoren den Azure AD-Bereitstellungsdienst für die ausgewählte Anwendung starten und beenden. Außerdem haben sie die Möglichkeit, den Bereitstellungscache zu leeren und den Dienst neu zu starten.

Wenn die Bereitstellung für eine Anwendung zum ersten Mal aktiviert wird, legen Sie den Bereitstellungsstatus** **auf **Ein** fest, um den Dienst zu aktivieren. Daraufhin führt der Azure AD-Bereitstellungsdienst eine anfängliche Synchronisierung durch. Hierzu liest er die im Abschnitt **Benutzer und Gruppen** zugewiesenen Benutzer, fragt die Zielanwendung für diese Benutzer ab und führt dann die im Azure AD-Abschnitt **Zuordnungen** definierten Bereitstellungsaktionen durch. Während dieses Vorgangs speichert der Bereitstellungsdienst Daten im Cache dazu, welche Benutzerkonten er verwaltet, damit die Bereitstellung nicht verwalteter Konten innerhalb der Zielanwendungen, die nicht im Zuweisungsumfang enthalten waren, nicht aufgehoben wird. Nach der anfänglichen Synchronisierung synchronisiert der Bereitstellungsdienst automatisch Benutzer- und Gruppenobjekte in einem Intervall von 10 Minuten.

Durch Ändern des **Bereitstellungsstatus** in **Aus** wird der Bereitstellungsdienst lediglich angehalten. In diesem Status werden von Azure keine Benutzer- oder Gruppenobjekte in der App erstellt, aktualisiert oder entfernt. Wird der Status wieder zu „Ein“ geändert, fährt der Dienst fort, wo er unterbrochen wurde.

Wenn das Kontrollkästchen zum **Deaktivieren des aktuellen Status und Starten der Synchronisierung** ausgewählt und dann gespeichert wird, wird der Bereitstellungsdienst beendet, verwirft die zwischengespeicherten Daten zu den von Azure AD verwalteten Konten, startet die Dienste neu und führt die anfängliche Synchronisierung erneut aus. Diese Option ermöglicht Administratoren das erneute Starten des Bereitstellungsprozesses.

### <a name="synchronization-details"></a>Synchronisierungsdetails
Dieser Abschnitt enthält weitere Details zum Vorgang des Bereitstellungsdiensts, einschließlich der ersten und letzten Ausführung des Bereitstellungsdiensts für die Anwendung, sowie die Anzahl der verwalteten Benutzer- und Gruppenkonten.

Es wird ein Link zum **Bericht über die Bereitstellungsaktivität** bereitgestellt. Dieser Bericht enthält ein Protokoll zu allen Benutzern und Gruppen, die zwischen Azure AD und der Zielanwendung erstellt, aktualisiert und entfernt wurden. Darüber hinaus wird ein Link zum **Fehlerbericht der Bereitstellung** bereitgestellt. Dieser enthält detaillierte Fehlermeldungen für Benutzer- und Gruppenobjekte, die nicht gelesen, erstellt, aktualisiert oder entfernt werden konnten. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG



<!--HONumber=Jan17_HO1-->


