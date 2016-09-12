<properties
	pageTitle="Monatlicher geschätzter Kostentrend | Microsoft Azure"
	description="Erfahren Sie mehr über das Diagramm „Monatlicher geschätzter Kostentrend“ von DevTest Labs."
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/25/2016"
	ms.author="tarcher"/>

# Monatlicher geschätzter Kostentrend

## Übersicht

Mithilfe des DevTest Labs-Features für das Kostenmanagement können Sie die Kosten Ihres Labs nachverfolgen. Dieser Artikel veranschaulicht, wie Sie das Diagramm **Monatlicher geschätzter Kostentrend** verwenden, um die bisherigen geschätzten Kosten für den aktuellen Kalendermonat sowie die veranschlagten Kosten am Monatsende für den aktuellen Kalendermonat anzuzeigen.

## Anzeigen des Diagramms „Monatlicher geschätzter Kostentrend“

Führen Sie diese Schritte aus, um das Diagramm „Monatlicher geschätzter Kostentrend“ anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie **Settings** aus.

	![Einstellungen](./media/devtest-lab-configure-cost-management/lab-blade-settings.png)

1. Wählen Sie auf dem Blatt **Einstellungen** des Labs unter **Kostenmanagement** **Schwellenwerte**.

	![Menü](./media/devtest-lab-configure-cost-management/menu.png)
 
1. Der folgende Screenshot zeigt ein Beispiel eines Kostendiagramms.

    ![Kostendiagramm](./media/devtest-lab-configure-cost-management/graph.png)

Der Wert **Geschätzte Kosten** stellt die bisherigen geschätzten Kosten für den aktuellen Kalendermonat dar. Der Wert **Veranschlagte Kosten** repräsentiert die geschätzten Kosten für den gesamten aktuellen Kalendermonat. Für diese Berechnung werden die Labkosten der letzten fünf Tage herangezogen.
 
Beachten Sie, dass die Kostenbeträge auf die nächste ganze Zahl aufgerundet werden. Beispiel:

- 5\.01 wird auf 6 aufgerundet.
- 5\.50 wird auf 6 aufgerundet.
- 5\.99 wird auf 6 aufgerundet.

Oberhalb des Diagramms wird darauf hingewiesen, dass es sich bei den in diesem Diagramm angezeigten Kosten um *geschätzte* Kosten handelt, die anhand der geltenden Preise für die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) kalkuliert werden. Darüber hinaus wird bei der Kostenkalkulation Folgendes *nicht* berücksichtigt:

- CSP- und Dreamspark-Abonnements werden derzeit nicht unterstützt, da DevTest Labs die [Azure-Abrechnungs-APIs](../billing-usage-rate-card-overview.md) zum Berechnen der Labkosten verwendet und die Azure-Abrechnungs-APIs keine CSP- oder Dreamspark-Abonnements unterstützen.
- Sonderpreise. Zurzeit können keine Sonderpreise (wie in Ihrem Abonnement angezeigt) berücksichtigt werden, die Sie mit Microsoft oder Microsoft-Partnern vereinbart haben. Es werden die Preise für die nutzungsbasierte Bezahlung verwendet.
- Steuern.
- Rabatte.
- Rechnungswährung. Zurzeit werden die Labkosten nur in USD angezeigt.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Verwandte Blogbeiträge

- [Two more things to keep your cost on track in DevTest Labs (Zwei weitere Möglichkeiten für die Kostenkontrolle in DevTest Labs)](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
- [Why Cost Thresholds? (Gründe für Kostenschwellenwerte)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## Nächste Schritte

Diese Schritte könnten Sie als Nächstes ausführen:

- [Definieren von Labrichtlinien:](./devtest-lab-set-lab-policy.md) Erfahren Sie, wie Sie die verschiedenen Richtlinien festlegen, mit denen Ihr Lab und die zugehörigen virtuellen Computer verwendet werden.
- [Erstellen von benutzerdefinierten Images:](./devtest-lab-create-template.md) Wenn Sie einen virtuellen Computer erstellen, geben Sie eine Basis an. Dabei kann es sich entweder um ein benutzerdefiniertes Image oder ein Marketplace-Image handeln. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen.
- [Konfigurieren von Marketplace-Images:](./devtest-lab-configure-marketplace-images.md) DevTest Labs unterstützt die Erstellung neuer virtueller Computer auf der Basis von Azure Marketplace-Images. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen neuer virtueller Computer in einem Lab verwendet werden können.
- [Erstellen eines virtuellen Computers in einem Lab:](./devtest-lab-add-vm-with-artifacts.md) In diesem Artikel wird veranschaulicht, wie Sie einen neuen virtuellen Computer aus einem Basisimage erstellen (entweder aus einem benutzerdefinierten Image oder einem Marketplace-Image) und wie Sie mit Artefakten auf dem virtuellen Computer arbeiten.

<!---HONumber=AcomDC_0831_2016-->