<properties 
pageTitle="Bereitstellen von SAP IDES EHP7 SP3 für SAP ERP 6.0 in Microsoft Azure | Microsoft Azure" 
description="Bereitstellen von SAP IDES EHP7 SP3 für SAP ERP 6.0 in Microsoft Azure" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/>
<tags  
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/>


# Bereitstellen von SAP IDES EHP7 SP3 für SAP ERP 6.0 in Microsoft Azure 

In diesem Artikel wird beschrieben, wie Sie SAP IDES mit SQL Server und einem Windows-Betriebssystem über die SAP Cloud Appliance Library 3.0 in Microsoft Azure bereitstellen. In den Screenshots ist dieser Prozess Schritt für Schritt dargestellt. Das Bereitstellen von anderen Lösungen in der Liste funktioniert in Bezug auf den Prozess genauso. Sie müssen lediglich eine andere Lösung auswählen.

[Hier](https://cal.sap.com/) können Sie mit der Nutzung der SAP Cloud Appliance Library (SAP CAL) beginnen. Es gibt auch einen Blog von SAP zur neuen [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).


In den folgenden Screenshots ist Schritt für Schritt dargestellt, wie Sie SAP IDES in Microsoft Azure bereitstellen. Der Prozess funktioniert für andere Lösungen genauso.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Die erste Abbildung zeigt alle Lösungen, die in Microsoft Azure verfügbar sind. Um den Prozess zu erläutern, wurde die markierte Windows-basierte SAP IDES-Lösung ausgewählt, die nur in Azure verfügbar ist.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Zuerst muss ein neues SAP CAL-Konto erstellt werden. Für Azure sind derzeit zwei Optionen verfügbar: Standard-Azure und Azure in China (Festland), das vom Partnerunternehmen 21Vianet betrieben wird.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Außerdem muss die ID des Azure-Abonnements eingegeben werden, die Sie im Azure-Portal finden. Weiter unten wird auch beschrieben, wie Sie die ID beschaffen können. Anschließend muss ein Azure-Verwaltungszertifikat heruntergeladen werden.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Im neuen Azure-Portal befindet sich das Element „Abonnements“ auf der linken Seite. Klicken Sie darauf, um alle aktiven Abonnements für den Benutzer anzuzeigen.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Durch die Auswahl eines Abonnements und der anschließenden Auswahl von „Verwaltungszertifikate“ wird veranschaulicht, dass für das neue Azure Resource Manager-Modell ein neues Konzept mit „Dienstprinzipalen“ verwendet wird. SAP CAL ist für dieses neue Modell noch nicht angepasst worden, sodass es weiterhin erforderlich ist, dass das „klassische“ Modell und das frühere Azure-Portal mit Verwaltungszertifikaten funktioniert.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Hier ist das frühere Azure-Portal dargestellt. Durch das Hochladen eines Verwaltungszertifikats erhält SAP CAL die Berechtigungen zum Erstellen von virtuellen Computern in einem Kundenabonnement. Auf der Registerkarte „ABONNEMENTS“ befindet sich die Abonnement-ID, die im SAP CAL-Portal eingegeben werden muss.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Auf der zweiten Registerkarte ist dann das Hochladen des Verwaltungszertifikats möglich, das von SAP CAL zuvor heruntergeladen wurde.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Ein kleines Dialogfeld zum Auswählen der heruntergeladenen Zertifikatdatei wird angezeigt.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Sobald das Zertifikat hochgeladen wurde, kann die Verbindung zwischen SAP CAL und dem Azure-Kundenabonnement in SAP CAL getestet werden. Eine kleine Meldung mit dem Hinweis, dass die Verbindung gültig ist, wird angezeigt.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Nach dem Einrichten eines Kontos muss eine Lösung ausgewählt werden, die bereitgestellt werden soll, und eine Instanz muss erstellt werden. Im einfachen Modus ist dies nicht schwierig. Geben Sie einen Instanznamen ein, wählen Sie eine Azure-Region aus, und definieren Sie das Masterkennwort für die Lösung.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Nach einiger Zeit wird je nach Größe und Komplexität (Schätzung wird durch SAP CAL angegeben) die Lösung als „aktiv“ und bereit für die Nutzung angezeigt. Der Vorgang ist also sehr einfach.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Wenn wir uns einige Details der Lösung ansehen, wird deutlich, welche Art von VMs bereitgestellt wurde. In diesem Fall ist ein einziger virtueller Azure-Computer der Größe D12 vorhanden, der durch SAP CAL erstellt wurde.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Im Azure-Portal beginnt der Name der virtuellen Computer jeweils mit dem gleichen Instanznamen, der in SAP CAL vergeben wurde.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Nun ist es möglich, für die Lösung über die Schaltfläche „Verbinden“ im SAP CAL-Portal eine Verbindung herzustellen. Das kleine Dialogfeld enthält einen Link zu einem Benutzerhandbuch, in dem alle Standardanmeldeinformationen für die Arbeit mit der Lösung beschrieben sind. [Hier](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) finden Sie den Link zum Leitfaden für die IDES-Lösung.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Eine weitere Option ist es, sich beim virtuellen Windows-Clientcomputer anzumelden und beispielsweise die vorkonfigurierte SAP GUI zu starten.

<!---HONumber=AcomDC_0921_2016-->