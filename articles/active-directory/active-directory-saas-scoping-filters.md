---
title: Bereitstellen von Apps mit Bereichsfiltern | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von Bereichsfiltern verhindern, dass Objekte in Apps mit automatisierter Benutzerbereitstellung auch dann bereitgestellt werden, wenn ein Objekt Ihre Geschäftsanforderungen nicht erfüllt."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: c06c089fb08c19b55246122201c378917a560e14
ms.openlocfilehash: 3e4458f70afce9ebd9477b00afc39b6e84e49319
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017


---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern
In diesem Abschnitt wird die Verwendung von Bereichsfiltern zum Definieren attributbasierter Regeln beschrieben, die festlegen, welche Benutzer der Anwendung bereitgestellt werden.

## <a name="clauses-and-scope-groups"></a>Klauseln und Bereichsgruppen
![Bereichsfilter][1] 

Bereichsfilter werden durch eine oder mehrere **Bereichsgruppen** definiert, die jeweils eine oder mehrere **Klauseln** umfasst. Um die Klauseln für eine bestimmte Bereichsgruppe anzuzeigen, erweitern Sie sie durch Klicken auf den Pfeil links neben dem Gruppennamen.

Eine **Klausel** bestimmt, welche Benutzer die Bereichsfilter passieren dürfen, indem die Attribute der einzelnen Benutzer überprüft werden. Wenn beispielsweise eine Klausel erfordert, dass das state-Attribut eines Benutzers „New York“ lauten muss, werden nur Benutzer in New York in der Anwendung bereitgestellt.

![Bereichsgruppenname][2] 

Jede **Bereichsgruppe** beginnt mit einer obligatorischen **Klausel**, wie im oben abgebildeten Screenshot gezeigt wird. Diese Klausel gibt einfach an, dass der Benutzer zunächst der Anwendung zugewiesen werden muss, bevor sie durch die Bereichsfilter ausgewertet wird. Diese Klausel kann nicht gelöscht oder geändert werden.

Sie können neue Klauseln oder neue Bereichsgruppen durch Wählen der entsprechenden Schaltfläche hinzufügen. Sie können jeder Bereichsgruppe einen Namen geben, indem Sie die Eigenschaft **Bereichsgruppenname** bearbeiten.

## <a name="how-scoping-filters-are-evaluated"></a>Auswerten von Bereichsfiltern
Während der Bereitstellung testen wir jeden zugewiesenen Benutzer mithilfe der Bereichsfilter, um festzustellen, ob dieser Benutzer auf die Anwendung zugreifen darf. Stellen Sie sich jede Klausel als Test vor, der bestanden werden muss, damit der Benutzer nicht herausgefiltert wird. 

Wenn Sie mehrere Bereichsgruppen definiert haben, muss jeder Benutzer sich für mindestens eine von ihnen qualifizieren, um auf die Anwendung zuzugreifen. Innerhalb jeder Bereichsgruppe muss der Benutzer jedoch alle Klauseln bestehen, um sich für diese spezifische Bereichsgruppe zu qualifizieren. 

Mit anderen Worten, Sie können sich Bereichsgruppen als mit OR verknüpft vorstellen, wobei die darin enthaltenen Klauseln mit AND verknüpft sind. Betrachten Sie beispielsweise den folgenden Bereichsfilter:

![Bereichsgruppenname][3]  

Gemäß diesem Bereichsfilter müssen Benutzer die folgenden Kriterien erfüllen, damit sie bereitgestellt werden können:

1. Sie müssen der Anwendung zugewiesen sein.
2. Sie müssen in der Engineeringabteilung arbeiten.
3. Sie müssen entweder in San Francisco oder in Kanada arbeiten.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen](active-directory-saas-app-provisioning.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png

