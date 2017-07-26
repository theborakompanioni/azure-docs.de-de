---
title: Anpassen von Azure AD-Attributzuordnungen | Microsoft-Dokumentation
description: "Erfahren Sie, was Attributzuordnungen für SaaS-Apps in Azure Active Directory sind und wie Sie sie an Ihre geschäftlichen Anforderungen anpassen können."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 6ca2fdc9c68ea0030d938eeaebd57aafa0e2790f
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory
Microsoft Azure AD bietet Unterstützung für die Benutzerbereitstellung für SaaS-Anwendungen von Drittanbietern, z. B. Salesforce, Google Apps usw. Wenn Sie die Benutzerbereitstellung für eine SaaS-Anwendung eines Drittanbieters aktiviert haben, steuert das Azure-Verwaltungsportal deren Attributwerte mithilfe einer Konfiguration, die als „Attributzuordnung“ bezeichnet wird.

Es ist eine vorkonfigurierte Sammlung von Attributzuordnungen zwischen Azure AD-Benutzerobjekten und den Benutzerobjekten der einzelnen SaaS-Apps vorhanden. Einige Apps verwalten andere Objekttypen, z. B. Gruppen oder Kontakte. <br> 
 Sie können die Standardattributzuordnungen den Anforderungen Ihres Unternehmens entsprechend anpassen. Dies bedeutet, dass Sie vorhandene Attributzuordnungen ändern oder löschen und neue Attributzuordnungen erstellen können.

Im Azure AD-Portal können Sie auf dieses Feature zugreifen, indem Sie auf eine **Zuordnungen**-Konfiguration unter **Bereitstellung** im Abschnitt **Verwalten** einer **Unternehmensanwendung** klicken.


![Salesforce][5] 

Beim Klicken auf eine **Zuordnungen**-Konfiguration wird das zugehörige Blatt **Attributzuordnung** geöffnet.  
Es gibt Attributzuordnungen, die erforderlich sind, damit eine SaaS-Anwendung ordnungsgemäß funktioniert. Für die erforderlichen Attribute ist das Feature **Löschen** nicht verfügbar.


![Salesforce][6]  

Im Beispiel oben können Sie sehen, dass das Attribut **Benutzername** eines verwalteten Objekts in Salesforce mit dem Wert **userPrincipalName** des verknüpften Azure AD-Objekts aufgefüllt wird.

Sie können vorhandene **Attributzuordnungen** durch Klicken auf eine Zuordnung anpassen. Daraufhin wird das Blatt **Attribut bearbeiten** geöffnet.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Grundlegendes zu Attributzuordnungstypen
Mit Attributzuordnungen steuern Sie, wie die Attribute in einer SaaS-Anwendung eines Drittanbieters mit Daten aufgefüllt werden. Vier verschiedene Zuordnungstypen werden unterstützt:

* **Direkt** : Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in Azure AD aufgefüllt.
* **Konstant** : Das Zielattribut wird mit einer bestimmten Zeichenfolge, die Sie angegeben haben, aufgefüllt.
* **Ausdruck** : Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks mit Daten aufgefüllt. 
  Weitere Informationen finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Kein** : Das Zielattribut bleibt unverändert. Wenn das Zielattribut allerdings leer ist, wird es mit dem von Ihnen angegebenen Standardwert aufgefüllt.

Zusätzlich zu diesen vier grundlegenden Attributzuordnungstypen unterstützen benutzerdefinierte Attributzuordnungen das Konzept einer optionalen Zuordnung von **Standardwerten**. Die Standardwertzuordnung stellt sicher, dass ein Zielattribut mit einem Wert aufgefüllt wird, wenn weder in Azure AD noch für das Zielobjekt ein Wert vorhanden ist. Bei der üblichen Konfiguration bleibt dieses Feld leer.


## <a name="understanding-attribute-mapping-properties"></a>Grundlegendes zu Attributzuordnungseigenschaften

Im vorherigen Abschnitt wurde bereits die Attributzuordnungstyp-Eigenschaft eingeführt.
Zusätzlich zu dieser Eigenschaft unterstützen Attributzuordnungen auch die folgenden Attribute:

- **Quellattribut**: das Benutzerattribut vom Quellsystem (z.B. Azure Active Directory).
- **Zielattribut**: das Benutzerattribut auf dem Zielsystem (z.B. ServiceNow).
- **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Quell- und Zielsystem verwendet werden soll. Dies wird in der Regel mit dem userPrincipalName- oder mail-Attribut in Azure AD festgelegt, das in der Regel einem Benutzernamenfeld in einer Zielanwendung zugeordnet ist.
- **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Wenn mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.
- **Diese Zuordnung anwenden**
    - **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.
    - **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.


## <a name="what-you-should-know"></a>Wichtige Informationen

Microsoft Azure AD stellt eine effiziente Implementierung eines Synchronisierungsprozesses zur Verfügung. In einer initialisierten Umgebung werden nur Objekte, die eine Aktualisierung erfordern, während eines Synchronisierungszyklus verarbeitet. Das Aktualisieren von Attributzuordnungen besitzt Auswirkungen auf die Leistung eines Synchronisierungszyklus. Nach einer Aktualisierung der Attributzuordnungskonfiguration müssen alle verwalteten Objekte erneut ausgewertet werden. Es hat sich bewährt, die Anzahl der aufeinanderfolgenden Änderungen an Attributzuordnungen so gering wie möglich zu halten.

## <a name="next-steps"></a>Nächste Schritte

* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung/Bereitstellungsaufhebung von Benutzern für SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png


