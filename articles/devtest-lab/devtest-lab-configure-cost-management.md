<properties
	pageTitle="Konfigurieren des Kostenmanagements | Microsoft Azure"
	description="Erfahren Sie, wie Sie die DevTest Labs-Features für das Kostenmanagement konfigurieren."
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
	ms.date="05/08/2016"
	ms.author="tarcher"/>

# Konfigurieren des Kostenmanagements

## Übersicht

Mithilfe des DevTest Labs-Features für das Kostenmanagement können Sie die Kosten Ihres Labs nachverfolgen. Dieser Artikel veranschaulicht, wie Sie das Diagramm **Monatlicher geschätzter Kostentrend** verwenden, um die bisherigen geschätzten Kosten für den aktuellen Kalendermonat sowie die veranschlagten Kosten am Monatsende für den aktuellen Kalendermonat anzuzeigen.

## Aktivieren des Diagramms „Monatlicher geschätzter Kostentrend“

Um das Diagramm „Monatlicher geschätzter Kostentrend“ zu aktivieren, führen Sie diese Schritte aus:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Tippen Sie auf **Durchsuchen**, und tippen Sie dann in der Liste auf **DevTest Labs**.

1. Tippen Sie in der Liste der Labs auf das gewünschte Lab.

1. Tippen Sie auf **Einstellungen**.

	![Einstellungen](./media/devtest-lab-configure-cost-management/lab-blade-settings.png)

1. Tippen Sie auf dem Blatt **Einstellungen** für das Lab unter **Kostenrichtlinien** auf **Kostenschwellenwerte**.

	![Menü](./media/devtest-lab-configure-cost-management/menu.png)
 
1. Tippen Sie auf dem Blatt **Kostenschwellenwerte** auf **Ein**, um das Feature zu aktivieren, und auf **Aus**, um es zu deaktivieren.

1. Tippen Sie auf **Speichern**.

Nachdem Sie das Feature aktiviert haben, kann es mehrere Stunden dauern, bis das Diagramm Ihre geschätzten und veranschlagten Kosten anzeigt. Dies liegt daran, dass ein Dienst diese Informationen zwar jede Stunde erfasst, aber einige Stunden nach der Sammlung der Livedaten ausgeführt wird. Beispiel: Sie starten einen virtuellen Computer um 1:00 morgens. Es dauert einige Stunden, bis die Kosten für diesen virtuellen Computer in das Kostendiagramm aufgenommen werden.
 
Der folgende Screenshot zeigt ein Beispiel eines Kostendiagramms.

![Kostendiagramm](./media/devtest-lab-configure-cost-management/graph.png)

Der Wert **Geschätzte Kosten** stellt die bisherigen geschätzten Kosten für den aktuellen Kalendermonat dar. Der Wert **Veranschlagte Kosten** repräsentiert die geschätzten Kosten für den gesamten aktuellen Kalendermonat.

Oberhalb des Diagramms wird darauf hingewiesen, dass es sich bei den in diesem Diagramm angezeigten Kosten um *geschätzte* Kosten handelt, die anhand der geltenden Preise für die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) kalkuliert werden. Darüber hinaus wird bei der Kostenkalkulation Folgendes *nicht* berücksichtigt:

- Sonderpreise. Zurzeit können keine Sonderpreise (wie in Ihrem Abonnement angezeigt) berücksichtigt werden, die Sie mit Microsoft oder Microsoft-Partnern ausgehandelt haben. Es werden die Preise für die nutzungsbasierte Bezahlung verwendet.
- Steuern.
- Rabatte.
- Rechnungswährung. Zurzeit werden die Labkosten nur in USD angezeigt.

## Nächste Schritte

Diese Schritte könnten Sie als Nächstes ausführen:

- [Definieren von Labrichtlinien](./devtest-lab-set-lab-policy.md): Erfahren Sie, wie Sie die verschiedenen Richtlinien festlegen, mit denen Ihr Lab und die zugehörigen virtuellen Computer gesteuert werden. 
- [Erstellen eines benutzerdefinierten Image](./devtest-lab-create-template.md): Wenn Sie einen virtuellen Computer erstellen, geben Sie eine Basis an, wobei es sich entweder um ein benutzerdefiniertes Image oder ein Marketplace-Image handeln kann. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen.
- [Konfigurieren von Marketplace-Images](./devtest-lab-configure-marketplace-images.md): DevTest Labs unterstützt die Erstellung neuer virtueller Computer auf der Basis von Azure Marketplace-Images. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen neuer virtueller Computer in einem Lab verwendet werden können.
- [Erstellen eines virtuellen Computers in einem Lab](./devtest-lab-add-vm-with-artifacts.md): In diesem Artikel wird veranschaulicht, wie Sie einen neuen virtuellen Computer aus einem Basis-Image erstellen (entweder ein benutzerdefiniertes Image oder ein Marketplace-Image) und wie Sie mit Artefakten im virtuellen Computer arbeiten.

<!---HONumber=AcomDC_0518_2016-->