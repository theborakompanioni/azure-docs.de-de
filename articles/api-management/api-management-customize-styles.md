---
title: Anpassen von Stilen im Entwicklerportal in Azure API Management | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie die Stile ändern, die für Seiten im Entwicklerportal in Azure API Management verwendet werden."
services: api-management
documentationcenter: 
author: vladvino
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 336d4f80f0357796fb29eb9314c11edfce831a69
ms.openlocfilehash: bd08eb476a4bd7298c5650977b88ba0b24deddec
ms.contentlocale: de-de
ms.lasthandoff: 02/23/2017

---
# <a name="customize-the-styling-of-the-developer-portal-in-azure-api-management"></a>Anpassen von Stilen im Entwicklerportal in Azure API Management
Es gibt drei grundlegende Möglichkeiten, das Entwicklerportal in Azure API Management anzupassen:

* [Bearbeiten des Inhalts von statischen Seiten und Seitenlayoutelementen][modify-content-layout]
* [Aktualisieren der Stile, die für Seitenelemente im gesamten Entwicklerportal verwendet werden][customize-styles] (in diesem Leitfaden beschrieben)
* [Ändern der Vorlagen, die für vom Portal generierte Seiten verwendet werden][portal-templates] (z.B. API-Dokumente, Produkte, Benutzerauthentifizierung usw.)

## <a name="change-headers-styling"> </a>Ändern der Stile von Seitenelementen

Farben, Schriftarten, Schriftgrößen, Abstände und andere stilverwandte Elemente aller Portalseiten werden durch Stilregeln definiert. 

Die Bearbeitung der Stilregeln wird über das **Entwicklerportal** durchgeführt, während Sie als Administrator angemeldet sind. Öffnen Sie hierzu zuerst das Azure-Portal, und klicken Sie in der Dienstsymbolleiste Ihrer API Management-Instanz auf **Herausgeberportal**.

![Herausgeberportal][api-management-management-console]

Klicken Sie anschließend oben rechts auf **Entwicklerportal**. 

![Link „Entwicklerportal“ im Herausgeberportal][api-management-pp-dp-link]

Um die Anpassungssymbolleiste zu öffnen, bewegen Sie den Mauszeiger auf das Anpassungssymbol (oder wählen Sie es aus) und klicken in der Symbolleiste dann auf „Styles“ (Stile).

![Schaltfläche der Anpassungssymbolleiste][api-management-customization-toolbar-button]

Stilregeln können grundsätzlich auf zwei Arten bearbeitet werden: Sie können sich entweder die standardmäßig angezeigte Liste mit allen verwendeten Stilregeln ansehen und einen Stil nach Bedarf ändern oder die Option **Select an element on the page** (Element auf der Seite auswählen) auswählen und dann auf der Seite auf eine beliebige Stelle klicken, um nur die Stile für das entsprechende Element anzuzeigen.

![Anpassungssymbolleiste][api-management-customization-toolbar]

Klicken Sie für dieses Beispiel auf die Option **Select an element on the page** (Element auf der Seite auswählen).  Die Elemente werden nun hervorgehoben, wenn Sie den Mauszeiger darüber bewegen, um das Element anzugeben, dessen Stile Sie bearbeiten, wenn Sie darauf klicken. Bewegen Sie den Mauszeiger auf den Text in der Kopfzeile (normalerweise der Name des Unternehmens), und klicken Sie darauf. Eine Liste mit benannten und kategorisierten Stilregeln wird daraufhin im Stil-Editor angezeigt. Jede Regel steht für eine Stileigenschaft des ausgewählten Elements. Für den ausgewählten Kopfzeilentext befindet sich der Schriftgrad beispielsweise in @font-size-h1 und der Name der Schriftart mit Alternativen in @headings-font-family.

> Falls Sie mit [Bootstrap][bootstrap] vertraut sind, werden Sie feststellen, dass es sich bei diesen Regeln um [LESS-Variablen][LESS variables] aus dem Bootstrap-Design für das Entwicklerportal handelt.
> 
> 

Ändern Sie nun die Farbe der Überschrift. Wählen Sie den Eintrag im Feld **@headings-color** aus, und geben Sie **#000000** ein. Dies ist der Hexadezimalcode für die Farbe Schwarz. Daraufhin wird am Ende des Textfelds ein rechteckiger Farbindikator angezeigt. Wenn Sie auf diesen Indikator klicken, können Sie in einer Farbauswahl eine Farbe auswählen.

![Farbauswahl][api-management-customization-toolbar-color-picker]

Vorgenommene Änderungen werden als Echtzeitvorschau angezeigt, sind jedoch nur für Administratoren sichtbar. Um die Änderungen für alle Benutzer sichtbar zu machen, klicken Sie im Stil-Editor auf **Veröffentlichen** , und bestätigen Sie die Änderungen.

![Veröffentlichungsmenü][api-management-customization-toolbar-publish-form]

> Wenn Sie die Stilregeln für ein anderes Element auf der Seite ändern möchten, gehen Sie auf die gleiche Weise wie für die Kopfzeile vor. Klicken Sie im Stil-Editor auf **Select an element on the page** (Element auf der Seite auswählen), wählen Sie das betreffende Element aus, und ändern Sie die Werte für die auf dem Bildschirm angezeigten Stilregeln.
> 
> 


## <a name="next-steps"> </a>Nächste Schritte
* Erfahren Sie, wie Sie den Inhalt von Enwicklerportalseiten mithilfe von [Entwicklerportalvorlagen](api-management-developer-portal-templates.md)anpassen.

[Change the styling of the headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/

