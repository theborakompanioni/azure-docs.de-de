<properties 
	pageTitle="So passen Sie das Azure API Management-Entwicklerportal mithilfe von Vorlagen an | Microsoft Azure" 
	description="Erfahren Sie, wie Sie das Azure API Management-Entwicklerportal mithilfe von Vorlagen anpassen können." 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/11/2016" 
	ms.author="sdanie"/>


# So passen Sie das Azure API Management-Entwicklerportal mithilfe von Vorlagen an

Azure API Management bietet eine Reihe von Anpassungsfeatures, mit denen Administratoren das [Aussehen und Verhalten des Entwicklerportals](api-management-customize-portal.md) sowie die Inhalte der Seiten im Entwicklerportal anpassen können. Zu diesem Zweck werden eine Reihe von Vorlagen verwendet, mit denen die Inhalte der Seiten selbst konfiguriert werden. Unter Verwendung dieser Vorlagen können Sie die Seiteninhalte mithilfe von [DotLiquid](http://dotliquidmarkup.org/)-Syntax und verschiedenen lokalisierten Zeichenfolgenressourcen, Symbolen und Seitensteuerelementen an Ihre Bedürfnisse anpassen.

## Übersicht über die Vorlagen im Entwicklerportal

Die Vorlagen des Entwicklerportals werden im Entwicklerportal von Administratoren der API Management-Dienstinstanz verwaltet. Zum Verwalten der Entwicklervorlagen wechseln Sie im klassischen Azure-Portal zu Ihrer API Management-Dienstinstanz, und klicken Sie auf **Durchsuchen**.

![Entwicklerportal][api-management-browse]

Wenn Sie sich bereits im Herausgeberportal befinden, klicken Sie auf **Entwicklerportal**, um auf das Entwicklerportal zuzugreifen.

![Entwicklerportal: Menü][api-management-developer-portal-menu]

Um auf die Vorlagen im Entwicklerportal zuzugreifen, klicken Sie auf der linken Seite auf das Anpassungssymbol, um das Anpassungsmenü zu öffnen, und wählen Sie **Vorlagen**.

![Entwicklerportal: Vorlagen][api-management-customize-menu]

In der Vorlagenliste werden verschiedene Vorlagenkategorien für die unterschiedlichen Seiten im Entwicklerportal angezeigt. Wenngleich sich die Vorlagen unterscheiden, sind die Schritte zum Bearbeiten und Veröffentlichen der Änderungen identisch. Zum Bearbeiten einer Vorlage klicken Sie auf ihren Namen.

![Entwicklerportal: Vorlagen][api-management-templates-menu]

Wenn Sie auf eine Vorlage klicken, wird die Seite im Entwicklerportal geöffnet, die über diese Vorlage angepasst werden kann. In diesem Beispiel wird die Vorlage **Produktliste** angezeigt. Über die Vorlage **Produktliste** werden die Inhalte des Bereichs festgelegt, der auf der Abbildung in einem roten Rechteck dargestellt ist.

![Vorlage „Produktliste“][api-management-developer-portal-templates-overview]

Mit einigen Vorlage, z. B. mit den **Benutzerprofil**-Vorlagen, werden unterschiedliche Abschnitte derselben Seite angepasst.

![Vorlage „Benutzerprofil“][api-management-user-profile-templates]

Der Editor der einzelnen Vorlagen im Entwicklerportal verfügt über zwei Abschnitte, die im unteren Bildschirmbereich angezeigt werden. Auf der linken Seite befindet sich der Bearbeitungsbereich für die Vorlage, auf der rechten Seite das Datenmodell für die Vorlage.

Der Bearbeitungsbereich der Vorlage umfasst das Markup, um das Aussehen und Verhalten der jeweiligen Seite im Entwicklerportal festzulegen. Für das Markup der Vorlage wird die [DotLiquid](http://dotliquidmarkup.org/)-Syntax verwendet. Ein beliebter Editor für DotLiquid ist [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Änderungen, die während der Bearbeitung an der Vorlage vorgenommen werden, werden in Echtzeit im Browser angezeigt. Für Ihre Kunden sind diese Änderungen jedoch erst sichtbar, wenn Sie die Vorlage [speichern](#to-save-a-template) und [veröffentlichen](#to-publish-a-template).

![Vorlagenmarkup][api-management-template]

Im Bereich **Vorlagendaten** finden Sie Hinweise zum Datenmodell für die Entitäten, die in einer bestimmten Vorlage verwendet werden können. Zu diesem Zweck werden die Livedaten angezeigt, die aktuell im Entwicklerportal sichtbar sind. Um die Vorlagenbereiche zu erweitern, klicken Sie auf das Rechteck oben rechts im Bereich **Vorlagendaten**.

![Vorlagendatenmodell][api-management-template-data]

Im vorstehenden Beispiel werden zwei Produkte im Entwicklerportal angezeigt, die aus den Daten abgerufen wurden, die im Bereich **Vorlagendaten** angezeigt werden, wie im nachfolgenden Beispiel gezeigt.

	{
		"Paging": {
			"Page": 1,
			"PageSize": 10,
			"TotalItemCount": 2,
			"ShowAll": false,
			"PageCount": 1
		},
		"Filtering": {
			"Pattern": null,
			"Placeholder": "Search products"
		},
		"Products": [
			{
				"Id": "56ec64c380ed850042060001",
				"Title": "Starter",
				"Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
				"Terms": "",
				"ProductState": 1,
				"AllowMultipleSubscriptions": false,
				"MultipleSubscriptionsCount": 1
			},
			{
				"Id": "56ec64c380ed850042060002",
				"Title": "Unlimited",
				"Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
				"Terms": null,
				"ProductState": 1,
				"AllowMultipleSubscriptions": false,
				"MultipleSubscriptionsCount": 1
			}
		]
	}

Das Markup der Vorlage **Produktliste** verarbeitet die Daten, um die gewünschte Ausgabe bereitzustellen. Dazu wird die Sammlung von Produkten durchlaufen, um Informationen und einen Link zu den einzelnen Produkten anzuzeigen. Beachten Sie die Elemente `<search-control>` und `<page-control>` im Markup. Mit diesen Elementen wird die Anzeige der Such- und Pagingsteuerelemente auf der Seite festgelegt. `ProductsStrings|PageTitleProducts` ist ein lokalisierter Zeichenfolgenverweis, der den `h2`-Kopfzeilentext für die Seite enthält. Eine Liste der Zeichenfolgenressourcen, Seitensteuerelemente und Symbole, die in Vorlagen des Entwicklerportals verwendet werden können, finden Sie hier: [API Management developer portal templates reference (Referenz zu Vorlagen im API Management-Entwicklerportal)](https://msdn.microsoft.com/library/azure/mt697540.aspx)

	<search-control></search-control>
	<div class="row">
	    <div class="col-md-9">
	        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
	    </div>
	</div>
	<div class="row">
	    <div class="col-md-12">
		{% if products.size > 0 %}
		<ul class="list-unstyled">
		{% for product in products %}
			<li>
				<h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
				{{product.description}}
			</li>	
		{% endfor %}
		</ul>
		<paging-control></paging-control>
		{% else %}
		{% localized "CommonResources|NoItemsToDisplay" %}
		{% endif %}
		</div>
	</div>

## So speichern Sie eine Vorlage

Zum Speichern einer Vorlage klicken Sie im Vorlagen-Editor auf „Speichern“.

![Vorlage speichern][api-management-save-template]

Gespeicherte Änderungen sind im Entwicklerportal erst nach der Veröffentlichung sichtbar.

## So veröffentlichen Sie eine Vorlage

Gespeicherte Vorlagen können einzeln oder gemeinsam veröffentlicht werden. Zum Veröffentlichen einer einzelnen Vorlage klicken Sie im Vorlagen-Editor auf „Veröffentlichen“.

![Vorlage veröffentlichen][api-management-publish-template]

Klicken Sie auf **Ja**, um den Vorgang zu bestätigen und die Vorlagendaten im Entwicklerportal zu übernehmen.

![Veröffentlichung bestätigen][api-management-publish-template-confirm]

Zum Veröffentlichen aller aktuell nicht veröffentlichten Vorlagenversionen klicken Sie in der Vorlagenliste auf **Veröffentlichen**. Nicht veröffentlichte Vorlagen sind mit einem Stern gekennzeichnet, der an den Vorlagennamen angefügt ist. In diesem Beispiel werden die Vorlagen **Produktliste** und **Produkt** veröffentlicht.

![Vorlagen veröffentlichen][api-management-publish-templates]

Klicken Sie auf **Anpassungen veröffentlichen**, um den Vorgang zu bestätigen.

![Veröffentlichung bestätigen][api-management-publish-customizations]

Neu veröffentlichte Vorlagen werden umgehend im Entwicklerportal übernommen.

## So stellen Sie die vorherige Version einer Vorlage wieder her

Zum Wiederherstellen der zuvor veröffentlichten Version einer Vorlage klicken Sie im Vorlagen-Editor auf „Wiederherstellen“.

![Vorlage wiederherstellen][api-management-revert-template]

Klicken Sie auf **Ja**, um zu bestätigen.

![Confirm][api-management-revert-template-confirm]

Die zuvor veröffentlichte Version einer Vorlage wird im Entwicklerportal übernommen, sobald der Wiederherstellungsvorgang abgeschlossen wurde.

## So stellen Sie die Standardversion einer Vorlage wieder her

Der Vorgang zum Wiederherstellen der Standardversion einer Vorlage umfasst zwei Schritte. Zunächst müssen die Vorlagen wiederhergestellt werden, anschließend müssen die wiederhergestellten Versionen veröffentlicht werden.

Zum Wiederherstellen der Standardversion einer einzelnen Vorlage klicken Sie im Vorlagen-Editor auf „Wiederherstellen“.

![Vorlage wiederherstellen][api-management-reset-template]

Klicken Sie auf **Ja**, um zu bestätigen.

![Confirm][api-management-reset-template-confirm]

Um die Standardversion aller Vorlagen wiederherzustellen, klicken Sie in der Vorlagenliste auf **Standardvorlagen wiederherstellen**.

![Vorlagen wiederherstellen][api-management-restore-templates]

Die wiederhergestellten Vorlagen müssen dann einzeln oder gemeinsam veröffentlicht werden. Führen Sie dazu die unter [So veröffentlichen Sie eine Vorlage](#to-publish-a-template) beschriebenen Schritte aus.

## Referenz zu Vorlagen im Entwicklerportal

Eine Referenz zu Vorlagen im Entwicklerportal, Zeichenfolgenressourcen, Symbolen und Seitensteuerelementen finden Sie unter [API Management developer portal templates reference (Referenz zu Vorlagen im API Management-Entwicklerportal)](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## Überblicksvideo ansehen

Im folgenden Video erfahren Sie, wie Sie Vorlagen nutzen können, um die API um eine Diskussionsrunde und Bewertungen zu erweitern oder Vorgangsseiten im Entwicklerportal hinzuzufügen.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png

<!---HONumber=AcomDC_0413_2016-->