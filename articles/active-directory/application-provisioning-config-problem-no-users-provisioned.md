---
title: "Es werden keine Benutzer für eine Azure AD-Kataloganwendung bereitgestellt | Microsoft-Dokumentation"
description: "Informationen zur Problembehandlung bei häufigen Problemen, die auftreten, wenn Benutzer nicht in einer Azure AD-Kataloganwendung angezeigt werden, die Sie für die Benutzerbereitstellung mit Azure AD konfiguriert haben"
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
ms.date: 05/04/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 0b63c7d15ca7570033a889dc1b57ebd202f4627b
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---


# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Es werden keine Benutzer für eine Azure AD-Kataloganwendung bereitgestellt

Nachdem die automatische Bereitstellung für eine Anwendung konfiguriert wurde (einschließlich der Überprüfung, ob die Anmeldeinformationen für die App gültig sind, die Azure AD bereitgestellt wurden, um die Verbindung mit der Anwendung herzustellen). Anschließend wird durch Folgendes bestimmt, ob Benutzer und/oder Gruppen für die App bereitgestellt wurden:

-   Welche Benutzer und Gruppen wurden der Anwendung **zugewiesen**. Weitere Informationen zur Zuweisung finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Ob **Attributzuordnungen** aktiviert und so konfiguriert sind, dass gültige Attribute von Azure AD mit der App synchronisiert werden. Weitere Informationen zu Attributzuordnungen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Ob ein **Bereichsfilter** vorhanden ist, der Benutzer auf Basis bestimmter Attributwerte filtert. Weitere Informationen zu Bereichsfiltern finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Wenn Sie beobachten, dass Benutzer nicht bereitgestellt werden, ziehen Sie die Überwachungsprotokolle in Azure AD zu Rate, und suchen Sie nach Protokolleinträgen für einen bestimmten Benutzer.

Sie können auf die Überwachungsprotokolle für die Bereitstellung im Azure-Portal auf der Registerkarte **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt; Überwachungsprotokolle** zugreifen. Filtern Sie die Protokolle nach der Kategorie **Kontobereitstellung**, um nur die Bereitstellungsereignisse für die jeweilige App anzuzeigen. Sie können auf Grundlage der „übereinstimmenden ID“ nach Benutzern suchen, die für die Benutzer in den Attributzuordnungen konfiguriert wurde. Wenn Sie z. B. den „Benutzerprinzipalnamen“ oder die „E-Mail-Adresse“ als übereinstimmendes Attribut auf der Azure AD-Seite konfiguriert haben und der Benutzer nicht bereitgestellt wurde, weist dies den Wert „audrey@contoso.com“ auf. Durchsuchen Sie dann die Überwachungsprotokolle nach „audrey@contoso.com“, und überprüfen Sie anschließend die zurückgegebenen Einträge.

Die Überwachungsprotokolle für die Bereitstellung zeichnen alle vom Bereitstellungsdienst durchgeführten Vorgänge auf, einschließlich der Abfragen von Azure AD für zugewiesene Benutzer, die sich im Bereitstellungsbereich befinden. Zudem fragen sie die Ziel-App in Bezug auf das Vorkommen dieser Benutzer ab, wobei die Benutzerobjekte zwischen dem System verglichen werden. Fügen Sie dann auf Basis des Vergleichs das Benutzerkonto zum Zielsystem hinzu oder aktualisieren bzw. deaktivieren Sie es.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Zu berücksichtigende allgemeine Problembereiche bei der Bereitstellung

Nachfolgend finden Sie eine Liste der allgemeinen Problembereiche, für die Sie Detailinformationen anzeigen können, wenn Sie wissen, wo Sie starten müssen.

* [Bereitstellungsdienst wird anscheinend nicht gestartet](#provisioning-service-does-not-appear-to-start)
* [Überwachungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Bereitstellungsdienst wird anscheinend nicht gestartet

Wenn Sie für **Bereitstellungsstatus** im Abschnitt **Azure Active Directory &gt; Unternehmens-Apps &gt; \[Anwendungsname\] &gt;Bereitstellung** des Azure-Portals die Option **Ein** festlegen. Auf dieser Seite werden jedoch nach anschließendem erneuten Laden keine anderen Statusdetails angezeigt. Es ist wahrscheinlich, dass der Dienst ausgeführt wird, aber noch keine anfängliche Synchronisierung abgeschlossen hat. Überprüfen Sie die oben beschriebenen **Überwachungsprotokolle**, um zu ermitteln, welche Vorgänge der Dienst durchführt und ob Fehler aufgetreten sind.

>[!NOTE]
>Eine anfängliche Synchronisierung kann zwischen 20 Minuten und mehreren Stunden dauern. Dies hängt von der Größe des Azure AD-Verzeichnisses und der Anzahl der Benutzer im Bereitstellungsbereich ab. Nachfolgende Synchronisierungen nach der anfänglichen Synchronisierung werden schneller durchgeführt, da der Bereitstellungsdienst Wasserzeichen speichert, die den Status beider Systeme nach der Erstsynchronisierung darstellen. Dies verbessert die Leistung von nachfolgenden Synchronisierungen.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Überwachungsprotokolle besagen, dass Benutzer „übersprungen“ und nicht bereitgestellt wurden, obwohl sie zugewiesen sind

Wenn ein Benutzer in den Überwachungsprotokollen als „übersprungen“ angezeigt wird, ist es sehr wichtig, die erweiterten Details in der Protokollmeldung zu lesen, um die Ursache zu ermitteln. Nachfolgend sind häufige Ursachen und Lösungen aufgeführt:

-   **Es wurde ein Bereichsfilter konfiguriert****, der einen Benutzer basierend auf einem Attributwert herausfiltert**. Weitere Informationen zu Bereichsfiltern finden Sie unter <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **Der Benutzer ist „nicht wirklich berechtigt“.** Wenn diese bestimmte Fehlermeldung angezeigt wird, liegt ein Problem mit dem Benutzerzuweisungseintrag vor, der in Azure AD gespeichert wird. Heben Sie die Zuweisung des Benutzers (oder der Gruppe) zur App auf, und weisen Sie ihn (oder sie) erneut zu, um dieses Problem zu beheben. Weitere Informationen zu Zuweisungen finden Sie unter <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Ein erforderliches Attribut fehlt oder wurde für einen Benutzer nicht ausgefüllt.** Es ist beim Einrichten der Bereitstellung wichtig zu beachten, dass die Attributzuordnungen und Workflows überprüft und konfiguriert werden, die festlegen, welche Benutzereigenschaften (oder Gruppeneigenschaften) von Azure AD zur Anwendung gelangen. Dazu zählt das Festlegen der „übereinstimmenden Eigenschaft“, mit der Benutzer/Gruppen zwischen den beiden Systemen eindeutig identifiziert und abgeglichen werden. Weitere Informationen zu diesem wichtigen Vorgang finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Attributzuordnungen für Gruppen:** Die Bereitstellung von Gruppennamen und Gruppendetails zusätzlich zu den Mitgliedern, falls dies für einige Anwendungen unterstützt wird. Sie können diese Funktionen aktivieren oder deaktivieren, indem Sie die **Zuordnung** für Gruppenobjekte aktivieren oder deaktivieren, die auf der Registerkarte **Bereitstellung** angezeigt wird. Wenn die Bereitstellung von Gruppen aktiviert ist, überprüfen Sie auf jeden Fall die Attributzuordnungen, um sicherzustellen, dass ein entsprechendes Feld für die „übereinstimmende ID“ verwendet wird. Dies kann der Anzeigename oder der E-Mail-Alias sein, da die Gruppe und ihre Mitglieder nicht bereitgestellt werden, wenn die entsprechende Eigenschaft leer ist oder nicht für eine Gruppe in Azure AD gefüllt wurde.

## <a name="next-steps"></a>Nächste Schritte
[Azure AD Connect-Synchronisierung: Grundlegendes zur deklarativen Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning.md)


