---
title: Anpassen des Entwicklerportals in Azure API Management | Microsoft Docs
description: Erfahren Sie, wie Sie das Entwicklerportal in Azure API Management anpassen.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 13431648e87d085161ad278dc991d49f7872be34
ms.openlocfilehash: 60213f885020a5ba36d6ada0812f755e06b3c48b


---
# <a name="customize-the-developer-portal-in-azure-api-management"></a>Anpassen des Entwicklerportals in Azure API Management
Diese Anleitung beschreibt, wie Sie das Erscheinungsbild des Entwicklerportals in Azure API Management an Ihre Marke anpassen können.

## <a name="change-page-headers"> </a>Ändern des Texts oder Logos in der Kopfzeile
Einer der Schlüsselaspekte der Portalanpassung ist die Möglichkeit, den Text am oberen Rand aller Seiten durch Ihren Firmennamen oder Ihr Logo zu ersetzen.

Die Inhalte im Entwicklerportal werden über das Herausgeberportal geändert, das im Azure-Portal aufgerufen werden kann. Um dorthin zu gelangen, klicken Sie auf der Dienstsymbolleiste Ihrer API Management-Instanz auf **Herausgeberportal**.

![Herausgeberportal][api-management-management-console]

Das Entwicklerportal basiert auf einem Content Management-System oder CMS. Bei der auf jeder Seite angezeigten Kopfzeile handelt es sich um einen Inhaltstyp, der auch Widget genannt wird. Um den Inhalt des Widgets zu bearbeiten, klicken Sie auf **Widgets** im Menü **Entwicklerportal** auf der linken Seite, und wählen Sie das Widget **Header** in der Liste aus.

![Kopfzeilen-Widget][api-management-widgets-header]

Sie können den Inhalt der Fußzeile im Feld **Body** bearbeiten. Ändern Sie den Text in "Fabrikam-Entwicklerportal", und klicken Sie dann unten in der Seite auf **Speichern** .

Die neue Kopfzeile wird jetzt auf allen Seiten im Entwicklerportal angezeigt.

> Klicken Sie in der oberen Leiste auf **Entwicklerportal** , um das Entwicklerportal aus dem Herausgeberportal zu öffnen.
> 
> 

## <a name="change-headers-styling"> </a>Ändern des Stils der Kopfzeilen
Farben, Schriftarten, Schriftgrößen, Abstände und andere stilverwandte Elemente aller Portalseiten werden durch Stilregeln definiert. Wenn Sie die Stile bearbeiten möchten, öffnen Sie links im **Entwicklerportal** die Anpassungssymbolleiste, indem Sie auf das Anpassungssymbol zeigen, und wählen Sie dann auf der Symbolleiste die Option „Stile“ aus.

![Schaltfläche der Anpassungssymbolleiste][api-management-customization-toolbar-button]

Stilregeln können grundsätzlich auf zwei Arten bearbeitet werden: Sie können sich entweder die standardmäßig angezeigte Liste mit allen verwendeten Stilregeln ansehen und einen Stil nach Bedarf ändern oder die Option **Select an element on the page** (Element auf der Seite auswählen) auswählen und dann auf der Seite auf eine beliebige Stelle klicken, um nur die Stile für das entsprechende Element anzuzeigen.

In diesem Abschnitt werden wir nur den Stil für die Kopfzeile ändern. Klicken Sie auf der Symbolleiste für den Stil-Editor auf die Option **Select an element on the page** (Element auf der Seite auswählen). 

![Anpassungssymbolleiste][api-management-customization-toolbar]

Die Elemente werden nun hervorgehoben, wenn Sie den Mauszeiger darüber bewegen, um das Element anzugeben, dessen Stile Sie bearbeiten, wenn Sie darauf klicken. Bewegen Sie die Maus über den Text mit dem Firmennamen in der Kopfzeile ("Fabrikam-Entwicklerportal", wenn Sie die Anweisungen im vorigen Abschnitt ausgeführt haben), und klicken Sie darauf. Eine Liste mit benannten und kategorisierten Stilregeln wird daraufhin im Stil-Editor angezeigt. Jede Regel steht für eine Stileigenschaft des ausgewählten Elements. Für den ausgewählten Kopfzeilentext befindet sich der Schriftgrad beispielsweise in @font-size-h1 und der Name der Schriftart mit Alternativen in @headings-font-family.

> Falls Sie mit [Bootstrap][bootstrap] vertraut sind, werden Sie feststellen, dass es sich bei diesen Regeln um [LESS-Variablen][LESS variables] aus dem Bootstrap-Design für das Entwicklerportal handelt.
> 
> 

Ändern Sie nun die Farbe der Überschrift. Wählen Sie den Eintrag im Feld **@headings-color** aus, und geben Sie **#000000** ein. Dies ist der Hexadezimalcode für die Farbe Schwarz. Daraufhin wird am Ende des Textfelds ein rechteckiger Farbindikator angezeigt. Wenn Sie auf diesen Indikator klicken, können Sie in einer Farbauswahl eine Farbe auswählen.

![Farbauswahl][api-management-customization-toolbar-color-picker]

Vorgenommene Änderungen werden als Echtzeitvorschau angezeigt, sind jedoch nur für Administratoren sichtbar. Um die Änderungen für alle Benutzer sichtbar zu machen, klicken Sie im Stil-Editor auf **Veröffentlichen** , und bestätigen Sie die Änderungen.

![Veröffentlichungsmenü][api-management-customization-toolbar-publish-form]

> Wenn Sie die Stilregeln für ein anderes Element auf der Seite ändern möchten, gehen Sie auf die gleiche Weise wie für die Kopfzeile vor. Klicken Sie im Stil-Editor auf **Element auswählen** , wählen Sie das betreffende Element aus, und ändern Sie die Werte der auf dem Bildschirm angezeigten Stilregeln.
> 
> 

## <a name="edit-page-contents"> </a>Bearbeiten der Seiteninhalte
Das Entwicklerportal besteht aus automatisch generierten Seiten, z. B. "APIs", "Produkte", "Anwendungen", "Probleme" und manuell erstellten Inhalten. Da das Portal auf einem Content Management-System basiert, können Sie diese Inhalte jederzeit erstellen.

Klicken Sie im Menü **Entwicklerportal** des Herausgeberportals auf **Inhalte**, um eine Liste aller vorhandenen Inhaltsseiten anzuzeigen.

![Inhalt verwalten][api-management-customization-manage-content]

Klicken Sie auf die **Willkommensseite**, um den Inhalt zu bearbeiten, der auf der Startseite des Entwicklerportals angezeigt wird. Nehmen Sie die gewünschten Änderungen vor, zeigen Sie die Änderungen in einer Vorschau an, und klicken Sie anschließend auf **Jetzt veröffentlichen**, um sie für alle Benutzer sichtbar zu machen.

> Die Startseite verwendet ein spezielles Layout, mit dem ein Banner am oberen Rand angezeigt werden kann. Dieses Banner kann nicht im Abschnitt **Inhalt** bearbeitet werden. Um dieses Banner zu bearbeiten, klicken Sie im **Entwicklerportal** auf **Widgets**, wählen Sie in der Dropdownliste **Aktuelle Ebene** die Ebene **Startseite** aus, und öffnen Sie dann im Abschnitt **Ausgewählte** das Element **Banner**. Sie können den Inhalt dieses Widgets ebenso wie alle anderen Seiten bearbeiten.
> 
> 

## <a name="next-steps"> </a>Nächste Schritte
* Erfahren Sie, wie Sie den Inhalt von Enwicklerportalseiten mithilfe von [Entwicklerportalvorlagen](api-management-developer-portal-templates.md)anpassen.

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-portal/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/



<!--HONumber=Feb17_HO1-->


