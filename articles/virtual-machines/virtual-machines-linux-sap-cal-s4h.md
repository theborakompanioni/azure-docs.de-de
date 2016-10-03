<properties 
pageTitle="Bereitstellen von S/4 HANA oder BW/4 HANA unter Microsoft Azure | Microsoft Azure" 
description="Bereitstellen von S/4 HANA oder BW/4 HANA unter Microsoft Azure" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/>
<tags  
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/>


# Bereitstellen von S/4 HANA oder BW/4 HANA unter Microsoft Azure 

In diesem Artikel wird beschrieben, wie Sie S/4 HANA unter Microsoft Azure über die SAP Cloud Appliance Library 3.0 bereitstellen. In den Screenshots ist dieser Prozess Schritt für Schritt dargestellt. Das Bereitstellen von anderen SAP HANA-basierten Lösungen wie BW/4 HANA funktioniert in Bezug auf den Prozess genauso. Sie müssen lediglich eine andere Lösung auswählen.

[Hier](https://cal.sap.com/) können Sie mit der Nutzung der SAP Cloud Appliance Library (SAP CAL) beginnen. Es gibt auch einen Blog von SAP zur neuen [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).


In den folgenden Screenshots ist Schritt für Schritt dargestellt, wie Sie S/4 HANA unter Microsoft Azure bereitstellen. Der Prozess funktioniert für andere Lösungen wie BW/4 HANA genauso.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Die erste Abbildung zeigt alle HANA-basierten SAP CAL-Lösungen, die unter Microsoft Azure verfügbar sind. Als Beispiel wurde die „lokale SAP S/4 HANA-Edition“ (Lösung unten im Screenshot) für den Prozessdurchlauf ausgewählt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Zuerst muss ein neues SAP CAL-Konto erstellt werden. Für Azure sind derzeit zwei Optionen verfügbar: Standard-Azure und Azure in China (Festland), das vom Partnerunternehmen 21Vianet betrieben wird.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Außerdem muss die ID des Azure-Abonnements eingegeben werden, die Sie im Azure-Portal finden. Weiter unten wird auch beschrieben, wie Sie die ID beschaffen können. Anschließend muss ein Azure-Verwaltungszertifikat heruntergeladen werden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

Im neuen Azure-Portal befindet sich das Element „Abonnements“ auf der linken Seite. Klicken Sie darauf, um alle aktiven Abonnements für den Benutzer anzuzeigen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Durch die Auswahl eines Abonnements und der anschließenden Auswahl von „Verwaltungszertifikate“ wird veranschaulicht, dass für das neue Azure Resource Manager-Modell ein neues Konzept mit „Dienstprinzipalen“ verwendet wird. SAP CAL ist für dieses neue Modell noch nicht angepasst worden, sodass es weiterhin erforderlich ist, dass das „klassische“ Modell und das frühere Azure-Portal mit Verwaltungszertifikaten funktioniert.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Hier ist das frühere Azure-Portal dargestellt. Durch das Hochladen eines Verwaltungszertifikats erhält SAP CAL die Berechtigungen zum Erstellen von virtuellen Computern in einem Kundenabonnement. Auf der Registerkarte „ABONNEMENTS“ befindet sich die Abonnement-ID, die im SAP CAL-Portal eingegeben werden muss.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Auf der zweiten Registerkarte ist dann das Hochladen des Verwaltungszertifikats möglich, das von SAP CAL zuvor heruntergeladen wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Ein kleines Dialogfeld zum Auswählen der heruntergeladenen Zertifikatdatei wird angezeigt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Sobald das Zertifikat hochgeladen wurde, kann die Verbindung zwischen SAP CAL und dem Azure-Kundenabonnement in SAP CAL getestet werden. Eine kleine Meldung mit dem Hinweis, dass die Verbindung gültig ist, wird angezeigt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Nach dem Einrichten eines Kontos muss eine Lösung ausgewählt werden, die bereitgestellt werden soll, und eine Instanz muss erstellt werden. Im einfachen Modus ist dies nicht schwierig. Geben Sie einen Instanznamen ein, wählen Sie eine Azure-Region aus, und definieren Sie das Masterkennwort für die Lösung.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Nach einiger Zeit wird je nach Größe und Komplexität (Schätzung wird durch SAP CAL angegeben) die Lösung als „aktiv“ und bereit für die Nutzung angezeigt. Der Vorgang ist also sehr einfach.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Wenn wir uns einige Details der Lösung ansehen, wird deutlich, welche Art von VMs bereitgestellt wurde. In diesem Fall wurden drei Azure-VMs unterschiedlicher Größe und für verschiedene Zwecke erstellt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Im Azure-Portal werden die virtuellen Computer jeweils mit dem gleichen Instanznamen angezeigt, der in SAP CAL vergeben wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Nun ist es möglich, für die Lösung über die Schaltfläche „Verbinden“ im SAP CAL-Portal eine Verbindung herzustellen. Das kleine Dialogfeld enthält einen Link zu einem Benutzerhandbuch, in dem alle Standardanmeldeinformationen für die Arbeit mit der Lösung beschrieben sind.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Eine weitere Option ist die Anmeldung an der Windows-Client-VM und beispielsweise das Starten der vorkonfigurierten SAP GUI.

<!---HONumber=AcomDC_0921_2016-->