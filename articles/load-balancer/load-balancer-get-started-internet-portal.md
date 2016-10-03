<properties
   pageTitle="Erstellen eines Load Balancers mit Internetzugriff in Resource Manager über das Azure-Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff in Resource Manager über das Azure-Portal erstellen."
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# Erste Schritte zum Erstellen eines Load Balancers mit Internetzugriff im Azure-Portal

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Hier erfahren Sie auch, wie Sie [mithilfe der klassischen Bereitstellung einen Load Balancer mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-portal.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Hier finden Sie eine ausführliche Beschreibung der einzelnen Aufgaben, die zum Erstellen eines Load Balancers durchgeführt werden müssen.

## Was ist erforderlich, um einen Load Balancer mit Internetzugriff zu erstellen?

Zum Bereitstellen eines Load Balancers müssen Sie die folgenden Objekte erstellen und konfigurieren.

- Front-End-IP-Konfiguration: Enthält öffentliche IP-Adressen für eingehenden Netzwerkdatenverkehr.

- Back-End-Adresspool: Enthält Netzwerkschnittstellen (NICs), die virtuellen Computern den Empfang von Netzwerkdatenverkehr des Load Balancers ermöglichen.

- Lastenausgleichsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port im Back-End-Adresspool.

- NAT-Eingangsregeln: Enthält Regeln für das Zuordnen eines öffentlichen Ports des Load Balancers zu einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool.

- Tests: Enthält Integritätstests zum Prüfen der Verfügbarkeit von VM-Instanzen im Back-End-Adresspool.

Unter [Unterstützung von Azure Resource Manager für den Load Balancer](load-balancer-arm.md) erhalten Sie weitere Informationen zu Load Balancer-Komponenten von Azure Resource Manager.


## Einrichten eines Load Balancers im Azure-Portal

> [AZURE.IMPORTANT] In diesem Beispiel wird angenommen, dass Sie das virtuelle Netzwerk **myVNet** erstellt haben. Weitere Informationen hierzu finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Außerdem wird angenommen, dass sich innerhalb von **myVNet** das Subnetz **LB-Subnet-BE** und die beiden virtuellen Computer **web1** und **web2** in derselben Verfügbarkeitsgruppe **myAvailSet** innerhalb von **myVNet** befinden. Um Informationen zum Erstellen virtueller Computer zu erhalten, folgen Sie [diesem Link](../virtual-machines/virtual-machines-windows-hero-tutorial.md).


1. Navigieren Sie in einem Browser zum Azure-Portal [http://portal.azure.com](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

2. Klicken Sie oben links auf dem Bildschirm auf **Neu** > **Netzwerk** > **Load Balancer**.

3. Geben Sie auf dem Blatt **Load Balancer erstellen** einen Namen für den Load Balancer ein. Hier nennen wir ihn **myLoadBalancer**.

4. Wählen Sie unter **Typ** die Option **Öffentlich**.

5. Erstellen Sie unter **Öffentliche IP-Adresse**eine neue öffentliche IP mit dem Namen **myPublicIP**.

6. Wählen Sie unter Ressourcengruppe **myRG**. Wählen Sie einen geeigneten **Speicherort**, und klicken Sie dann auf **OK**. Die Bereitstellung des Load Balancers wird dann gestartet und wird einige Minuten dauern.

![Aktualisieren der Ressourcengruppe eines Load Balancers](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## Erstellen eines Back-End-Adresspools

1. Nach erfolgreicher Bereitstellung Ihres Load Balancers wählen Sie diesen unter Ihren Ressourcen aus. Wählen Sie unter Einstellungen die Option „Back-End-Pools“. Geben Sie einen Namen für Ihren Back-End-Pool ein. Klicken Sie dann auf die oben auf dem Blatt angezeigte Schaltfläche **Hinzufügen**.

2. Klicken Sie auf dem Blatt **Back-End-Pool hinzufügen** auf **Virtuellen Computer hinzufügen**. Wählen Sie unter **Verfügbarkeitsgruppe** die Option **Verfügbarkeitsgruppe auswählen**, und wählen Sie dann **myAvailSet**. Wählen Sie dann auf dem Blatt im Bereich mit den virtuellen Computern die Option **Wählen Sie die virtuellen Computer aus**, und klicken Sie auf **web1** und **web2**, die beiden virtuellen Computer, die für den Load Balancer erstellt wurden. Stellen Sie sicher, dass beide auf der linken Seite mit blauen Häkchen versehen sind, wie im unten stehenden Bild dargestellt. Klicken Sie dann auf diesem Blatt auf **Auswählen** und anschließend auf dem Blatt **Virtuelle Computer auswählen** auf „OK“. Klicken Sie dann auf dem Blatt **Back-End-Pool hinzufügen** auf **OK**.

    ![Hinzufügen zum Back-End-Adresspool](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Stellen Sie sicher, dass Ihre Dropdownliste für Benachrichtigungen in Bezug auf die Speicherung des Back-End-Pools des Load Balancers sowie in Bezug auf die Aktualisierung der Netzwerkschnittstellen für die virtuellen Computer **web1** und **web2** aktualisiert wurde.


## Erstellen von Tests, LB-Regeln und NAT-Regeln

1. Erstellen Sie einen Integritätstest.

    Wählen Sie in den Einstellungen Ihres Load Balancers den Punkt „Tests“. Klicken Sie dann oben auf dem Blatt auf **Hinzufügen**.

    Es gibt zwei Möglichkeiten, einen Test zu konfigurieren: über HTTP oder TCP. Dieses Beispiel zeigt die Konfiguration über HTTP, die Konfiguration per TCP erfolgt auf ähnliche Weise. Aktualisieren Sie die notwendigen Informationen. Wie bereits erwähnt, nimmt **myLoadBalancer** einen Lastenausgleich in Port 80 vor. Der ausgewählte Pfad lautet „HealthProbe.aspx“, das Intervall beträgt 15 Sekunden, und der Fehlerschwellenwert liegt bei 2. Nach der Fertigstellung klicken Sie zum Erstellen des Tests auf **OK**.

    Um weitere Informationen zu den einzelnen Konfigurationen und dazu zu erhalten, wie diese verändert werden können, um Ihre Anforderungen zu erfüllen, bewegen Sie den Mauszeiger über das Symbol „i“.

    ![Hinzufügen eines Tests](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Erstellen Sie eine Load Balancer-Regel.

    Klicken Sie in den Einstellungen Ihres Load Balancers auf die Regeln für den Load Balancer. Klicken Sie auf dem neuen Blatt auf **Hinzufügen**. Geben Sie einen Namen für die Regel ein. Hier ist es „HTTP“. Wählen Sie den Front-End- und den Back-End-Port aus. Hier wurde für beide Port 80 ausgewählt. Wählen Sie **LB-backend** als Back-End-Pool und den zuvor erstellten **HealthProbe** als Test. Andere Konfigurationen können gemäß Ihren Anforderungen eingerichtet werden. Um die Lastenausgleichsregel zu speichern, klicken Sie auf „OK“.

    ![Hinzufügen einer Lastenausgleichsregel](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Erstellen eingehender NAT-Regeln

    Klicken Sie in den Einstellungen Ihres Load Balancers auf „Eingehende NAT-Regeln“. Klicken Sie auf dem neuen Blatt auf **Hinzufügen**. Geben Sie Ihrer eingehenden NAT-Regel einen Namen. Wir nennen Sie hier **inboundNATrule1**. Das Ziel sollte die zuvor erstellte öffentliche IP-Adresse sein. Wählen Sie unter „Dienst“ die Option „Benutzerdefiniert“ aus, und wählen Sie das Protokoll aus, das Sie verwenden möchten. Hier wurde TCP ausgewählt. Geben Sie den Port 3441 und den Zielport ein, in diesem Fall 3389. Um die Regel zu speichern, klicken Sie auf „OK“.

    Nachdem Sie die erste Regel erstellt haben, wiederholen Sie diesen Schritt für die zweite eingehende NAT-Regel mit dem Namen „inboundNATrule2“ von Port 3442 zum Zielport 3389.

    ![Hinzufügen einer eingehenden NAT-Regel](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## Entfernen eines Lastenausgleichs

Zum Löschen eines Load Balancers wählen Sie den Load Balancer aus, den Sie entfernen möchten. Klicken Sie im oberen Bereich des Blatts *Load Balancer* auf **Löschen**. Klicken Sie dann auf **Ja**, wenn Sie dazu aufgefordert werden.

## Nächste Schritte

[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0921_2016-->