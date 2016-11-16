---
title: "Übersicht über das Microsoft Azure-Portal"
description: "Erfahren Sie, wie Sie das Microsoft Azure-Portal verwenden können."
services: 
documentationcenter: 
author: davidwrede
manager: dwrede
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/16/2015
ms.author: dwrede
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 31bea465db1afb5efc22b2280d83d3b50bced329


---
# <a name="microsoft-azure-portal-overview"></a>Übersicht über das Microsoft Azure-Portal
Das Microsoft Azure-Portal ist eine zentrale Anlaufstelle zum Bereitstellen und Verwalten von Azure-Ressourcen.  In diesem Tutorial lernen Sie das Portal kennen und erfahren, wie Sie die folgenden wichtigen Funktionen verwenden:

* Einen **umfassenden Marketplace**, auf dem für Sie Tausende von Produkten und Lösungen von Microsoft und anderen Anbietern verfügbar sind, die erworben bzw. bereitgestellt werden können.
* Eine **einheitliche und skalierbare Suchumgebung** , in der Sie die gewünschten Ressourcen leicht finden und verschiedene Verwaltungsvorgänge durchführen können.
* **Konsistente Verwaltungsseiten** (oder -blätter), mit denen Sie die vielen unterschiedlichen Dienste von Azure verwalten können, indem Sie Einstellungen, Aktionen, Rechnungsinformationen, Integritätsüberwachungs- und Nutzungsdaten und vieles mehr auf einheitliche Weise verfügbar machen.
* Eine **persönliche Benutzeroberfläche** , auf der Sie einen angepassten Startbildschirm mit den Informationen erstellen können, die beim Anmelden immer angezeigt werden sollen.  Sie können auch die Verwaltungsbereiche anpassen, die Kacheln enthalten.
  
  ![Azure-Portal-UI-Ausrichtung][UIOrientation]

## <a name="before-you-get-started"></a>Bevor Sie beginnen
Um dieses Lernprogramm zu bearbeiten, benötigen Sie ein gültiges Azure-Abonnement.  Wenn Sie kein Azure-Abonnement haben, registrieren Sie sich noch heute [für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) .  Sobald Sie über ein Abonnement verfügen, können Sie unter [https://portal.azure.com] auf das Portal zugreifen.

## <a name="how-to-create-a-resource"></a>Erstellen einer Ressource
Azure verfügt über einen Markt mit Tausenden von Elementen, die Sie von einem zentralen Ort aus erstellen können.  Angenommen, Sie möchten einen neuen Windows Server 2012-VM erstellen.  Der Hub +NEU ist Ihr Zugriffspunkt auf eine gut sortierte Reihe wichtiger Kategorien aus dem Marketplace.  Jede Kategorie verfügt über eine kleine Gruppe bestimmter Elemente und einen Link zum Marketplace mit allen Kategorien und der Suchfunktion. Um die neue Windows Server 2012-VM zu erstellen, führen Sie die folgenden Aktionen aus:  

1. Windows Server 2012 gehört zu den ausgewählten Elementen, sodass Sie über die Kategorie „Compute“ darauf zugreifen können.  
2. Füllen Sie das Formular mit einigen grundlegenden Angaben aus.
3. Klicken Sie auf „Erstellen“, und die Bereitstellung des virtuellen Computers beginnt sofort.

Der Hub „Benachrichtigungen“ informiert Sie, wenn die Ressource erstellt wurde, und ein Verwaltungsbereich wird geöffnet (später können Sie zu den Ressourcen gehen).

![Portal-Kategorien][PortalCategories]

## <a name="how-to-find-your-resources"></a>Suchen von Ressourcen
Häufig verwendete Ressourcen können Sie an Ihr Startmenü anheften. Möglicherweise möchten Sie aber nach etwas suchen, worauf Sie weniger häufig zugreifen.  Der unten gezeigte Hub „Durchsuchen“ bietet die Möglichkeit zum Zugriff auf alle Ihre Ressourcen.  Sie können nach Abonnement filtern, Spalten auswählen und ihre Größe ändern und durch Klicken auf einzelne Elemente zu den Blades für die Verwaltung navigieren.

![Hub durchsuchen][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Verwalten und Delegieren des Zugriffs auf eine Ressource
Von diesem Bereich aus können Sie sich mithilfe des Remotedesktop mit dem virtuellen Computer verbinden, wichtige Leistungsmetriken überwachen, den Zugriff auf den virtuellen Computer über rollenbasierten Zugriff (RBAC) steuern, den virtuellen Computer konfigurieren und andere wichtige Verwaltungsaufgaben ausführen.  Das Delegieren des Zugriffs auf der Grundlage von Rollen ist entscheidend für skalierbares Verwalten.  Klicken Sie [hier](active-directory/role-based-access-control-configure.md), um weitere Informationen zu erhalten. Um den Zugriff auf eine Ressource zu delegieren, führen Sie die folgenden Aktionen aus:

1. Navigieren Sie zu der Ressource.
2. Klicken Sie im Abschnitt „Essentials“ auf „Alle Einstellungen“.
3. Klicken Sie in der Liste der Einstellungen auf „Benutzer“.
4. Klicken Sie in der Befehlsleiste auf „Hinzufügen“.
5. Wählen Sie einen Benutzer und eine Rolle.

![Verwalten von Ressourcen][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Anpassen eines Ressourcenbereichs
Die Bereiche für Ihre Ressourcen werden von Azure konfiguriert; die Kacheln in diesen Bereichen können jedoch von Ihnen angepasst werden.  Sie können problemlos in den Anpassungsmodus wechseln, um die Kacheln zu entfernen, ihre Größe zu verändern oder sie anders anzuordnen. Um einen Bereich anzupassen, führen Sie folgende Aktionen aus:

1. Navigieren Sie zu der Ressource.
2. Klicken Sie auf die Schaltfläche „...“ oben auf dem Blatt, das Sie anpassen möchten.
3. Klicken Sie auf „Elemente hinzufügen“.
4. Nun können Sie Elemente per Drag & Drop hinzufügen.  

![Anpassen von Bereichen (Blades)][CustomizeBlades]

## <a name="how-to-get-help"></a>So erhalten Sie Hilfe
Sollten Sie auf ein Problem stoßen, sind wir für Sie da.  Das Portal verfügt über eine Hilfe- und Support-Seite, die Sie in die richtige Richtung verweisen kann.  Abhängig von Ihrem [Supportplan](https://azure.microsoft.com/support/plans/) können Sie Supportanfragen ggf. auch direkt im Portal erstellen.  Nach dem Erstellen einer Supportanfrage können Sie den Lebenszyklus des Tickets vom Portal aus verwalten. Um zur Hilfe- und Support-Seite zu gelangen, gehen Sie zu „Durchsuchen“ und dann zu „Hilfe + Support“.  

![Hilfe und Support][HelpSupport]

## <a name="summary"></a>Zusammenfassung
Wir fassen noch einmal zusammen, was Sie in diesem Tutorial gelernt haben:

* Sie haben gelernt, wie Sie sich anmelden, sich für ein Abonnement registrieren und auf das Portal zugreifen.
* Sie haben gelernt, wie Sie sich auf der Benutzeroberfläche des Portals orientieren und Ressourcen erstellen und suchen.
* Sie haben gelernt, wie Sie eine Ressource erstellen und nach Ressourcen suchen.
* Sie haben die Blätter für die Struktur und die Verwaltung kennengelernt und erfahren, wie Sie unterschiedliche Arten von Ressourcen einheitlich verwalten können.
* Sie haben gelernt, wie Sie das Portal so anpassen können, dass die wichtigen Informationen im Vordergrund stehen.
* Sie haben gelernt, wie Sie den Zugriff auf Ressourcen mit der rollenbasierten Zugriffssteuerung (RBAC) kontrollieren.
* Sie haben erfahren, wie Sie Hilfe und Support erhalten.

Das Microsoft Azure-Portal vereinfacht die Erstellung und Verwaltung Ihrer Anwendungen in der Cloud radikal.  Um auf dem Laufenden zu bleiben, besuchen Sie unseren [Verwaltungsblog](https://azure.microsoft.com/blog/topics/management/). Wir sind stets [offen für Feedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) und nehmen ständig Verbesserungen vor.  Der [Blog von ScottGu](http://weblogs.asp.net/scottgu) ist eine weitere umfangreiche Quelle für alle Azure-Updates.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png



<!--HONumber=Nov16_HO2-->


