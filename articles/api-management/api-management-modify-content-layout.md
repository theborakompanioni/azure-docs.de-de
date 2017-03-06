---
title: "Ändern von Seiteninhalten im Entwicklerportal in Azure API Management | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Seiteninhalte im Entwicklerportal in Azure API Management bearbeiten.
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
translationtype: Human Translation
ms.sourcegitcommit: ecae1da20551d8372331124b07c4aca2e15f55bb
ms.openlocfilehash: 708c803c36c182ed90e04731b12d4ade00ae7ffb
ms.lasthandoff: 02/23/2017


---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>Ändern des Inhalts und Layouts von Seiten im Entwicklerportal in Azure API Management
Es gibt drei grundlegende Möglichkeiten, das Entwicklerportal in Azure API Management anzupassen:

* [Bearbeiten des Inhalts von statischen Seiten und Seitenlayoutelementen][modify-content-layout] (in diesem Leitfaden beschrieben)
* [Aktualisieren der Stile, die für Seitenelemente im gesamten Entwicklerportal verwendet werden][customize-styles]
* [Ändern der Vorlagen, die für vom Portal generierte Seiten verwendet werden][portal-templates] (z.B. API-Dokumente, Produkte, Benutzerauthentifizierung usw.)

## <a name="page-structure"> </a>Struktur von Seiten im Entwicklerportal

Das Entwicklerportal basiert auf einem Content Management-System. Das Layout jeder Seite wird basierend auf einer Gruppe von kleinen Seitenelementen erstellt, die als „Widgets“ bezeichnet werden:

![Seitenstruktur im Entwicklerportal][api-management-customization-widget-structure]

Alle Widgets können bearbeitet werden. 
* Die Kerninhalte für jede einzelne Seite befinden sich im Widget „Contents“ (Inhalt). Das Bearbeiten einer Seite ist also gleichbedeutend mit dem Bearbeiten des Inhalts dieses Widgets.
* Alle Seitenlayoutelemente sind in den verbleibenden Widgets enthalten. An diesen Widgets vorgenommene Änderungen gelten für alle Seiten. Sie werden als „Layoutwidgets“ bezeichnet.

Bei der täglichen Bearbeitung von Seiten wird normalerweise nur das Widget „Contents“ (Inhalte) geändert, das für jede Seite unterschiedlichen Inhalt aufweist.

## <a name="modify-layout-widget"> </a>Ändern des Inhalts eines Layoutwidgets

Die Inhalte im Entwicklerportal werden über das Herausgeberportal geändert, das im Azure-Portal aufgerufen werden kann. Um dorthin zu gelangen, klicken Sie auf der Dienstsymbolleiste Ihrer API Management-Instanz auf **Herausgeberportal**.

![Herausgeberportal][api-management-management-console]

Klicken Sie zum Bearbeiten des Inhalts dieses Widgets links im Menü **Entwicklerportal** auf **Widgets**. In diesem Beispiel ändern wir den Inhalt des Widgets „Header“ (Kopfzeile). Wählen Sie in der Liste das Widget **Header** (Kopfzeile) aus.

![Kopfzeilen-Widget][api-management-widgets-header]

Sie können den Inhalt der Kopfzeile im Feld **Body** bearbeiten. Ändern Sie den Text wie gewünscht, und klicken Sie dann unten auf der Seite auf **Speichern**.

Die neue Kopfzeile wird jetzt auf allen Seiten im Entwicklerportal angezeigt.

> Klicken Sie in der oberen Leiste auf **Entwicklerportal** , um das Entwicklerportal aus dem Herausgeberportal zu öffnen.
> 
> 

## <a name="edit-page-contents"> </a>Bearbeiten der Seiteninhalte

Klicken Sie im Menü **Entwicklerportal** des Herausgeberportals auf **Inhalte**, um eine Liste aller vorhandenen Inhaltsseiten anzuzeigen.

![Inhalt verwalten][api-management-customization-manage-content]

Klicken Sie auf die **Willkommensseite**, um den Inhalt zu bearbeiten, der auf der Startseite des Entwicklerportals angezeigt wird. Nehmen Sie die gewünschten Änderungen vor, zeigen Sie die Änderungen in einer Vorschau an, und klicken Sie anschließend auf **Jetzt veröffentlichen**, um sie für alle Benutzer sichtbar zu machen.

> Die Startseite verwendet ein spezielles Layout, mit dem ein Banner am oberen Rand angezeigt werden kann. Dieses Banner kann nicht im Abschnitt **Inhalt** bearbeitet werden. Um dieses Banner zu bearbeiten, klicken Sie im **Entwicklerportal** auf **Widgets**, wählen Sie in der Dropdownliste **Aktuelle Ebene** die Ebene **Startseite** aus, und öffnen Sie dann im Abschnitt **Ausgewählte** das Element **Banner**. Sie können den Inhalt dieses Widgets ebenso wie alle anderen Seiten bearbeiten.
> 
> 

## <a name="next-steps"> </a>Nächste Schritte
* [Aktualisieren der Stile, die für Seitenelemente im gesamten Entwicklerportal verwendet werden][customize-styles]
* [Ändern der Vorlagen, die für vom Portal generierte Seiten verwendet werden][portal-templates] (z.B. API-Dokumente, Produkte, Benutzerauthentifizierung usw.)

[Structure of developer portal pages]: #page-structure
[Modifying the contents of a layout widget]: #modify-layout-widget
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png

