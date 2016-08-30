<properties
   pageTitle="Erstellen des VNet-Peerings mit dem Azure-Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein virtuelles Netzwerk mit dem Azure-Portal im Resource Manager erstellen."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/02/2016"
   ms.author="narayanannamalai"/>

# Erstellen eines VNet-Peerings mit dem Azure-Portal

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Gehen Sie wie folgt vor, um mit dem Azure-Portal ein VNet-Peering basierend auf dem oben beschriebenen Szenario zu erstellen.

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Zum Einrichten des VNet-Peerings müssen Sie zwischen zwei VNets zwei Links erstellen, also einen für jede Richtung. Sie können den VNet-Peeringlink zuerst für VNet1 zu VNet2 erstellen. Klicken Sie im Portal auf **Durchsuchen**, und wählen Sie **Virtuelle Netzwerke**.

	![VNet-Peering im Azure-Portal erstellen](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Wählen Sie auf dem Blatt „Virtuelle Netzwerke“ die Option „VNet1“ aus, und klicken Sie auf „Peerings“ und dann auf „Hinzufügen“.

	![Peering auswählen](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Vergeben Sie auf dem Blatt „Peering hinzufügen“ für das Peering den Linknamen „LinkToVnet2“, wählen Sie das Abonnement und das zu verknüpfende virtuelle Netzwerk „VNet2“ aus, und klicken Sie auf „OK“.

	![Link zu VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Gehen Sie nach der Erstellung dieses VNet-Peeringlinks wie unten angegeben vor. Der Linkstatus wird wie folgt angezeigt:

	![Linkstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Erstellen Sie als Nächstes den VNet-Peeringlink für VNet2 zu VNet1. Wählen Sie auf dem Blatt „Virtuelle Netzwerke“ die Option „VNet2“ aus, und klicken Sie auf „Peerings“ und dann auf „Hinzufügen“.

	![Peer aus anderem VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Vergeben Sie auf dem Blatt „Peering hinzufügen“ für das Peering den Linknamen „LinkToVnet1“, wählen Sie das Abonnement und das zu verknüpfende virtuelle Netzwerk aus, und klicken Sie auf „OK“.

	![Kachel "Erstellen eines virtuellen Netzwerks"](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Gehen Sie nach der Erstellung dieses VNet-Peeringlinks wie unten angegeben vor. Der Linkstatus wird wie folgt angezeigt:

	![Endgültiger Linkstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Überprüfen Sie den Status von „LinkToVnet2“, der sich jetzt auch in „Verbunden“ ändert.

	![Endgültiger Linkstatus 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

10. HINWEIS: Das VNet-Peering wird nur eingerichtet, wenn beide Links verbunden sind.

Für die einzelnen Links können jeweils einige Eigenschaften konfiguriert werden:

|Option|Beschreibung|Standard|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Gibt an, ob der Adressraum des per Peering verknüpften VNet als Teil des Virtual\_network-Tags einbezogen wird.|Ja|
|AllowForwardedTraffic|Ermöglicht, dass Datenverkehr, der nicht aus dem per Peering verknüpften VNet stammt, akzeptiert oder verworfen wird.|Nein|
|AllowGatewayTransit|Ermöglicht dem per Peering verknüpften VNet die Verwendung des VNet-Gateways.|Nein|
|UseRemoteGateways|Gibt an, dass das VNet-Gateway des per Peering verknüpften VNet verwendet wird. Für das verknüpfte VNet muss ein Gateway konfiguriert sein, und „AllowGatewayTransit“ muss ausgewählt sein. Sie können diese Option nicht verwenden, wenn Sie ein Gateway konfiguriert haben.|Nein|

Jeder Link in einem VNet-Peering verfügt über die obigen Eigenschaften. Sie können im Portal auf den VNet-Peeringlink klicken und die verfügbaren Optionen ändern. Klicken Sie auf „Speichern“, damit die Änderungen wirksam werden.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. In diesem Beispiel verwenden wir zwei Abonnements (A und B) und zwei Benutzer (UserA und UserB), die jeweils über Rechte für die Abonnements verfügen.
3. Klicken Sie im Portal auf „Durchsuchen“, und wählen Sie „Virtuelle Netzwerke“. Klicken Sie auf das VNet und dann auf „Hinzufügen“.

    ![Szenario 2 durchsuchen](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Wählen Sie auf dem Blatt „Zugriff hinzufügen“ eine Rolle und dann die Option „Netzwerkmitwirkender“ aus. Klicken Sie auf „Benutzer hinzufügen“, geben Sie den Anmeldenamen „UserB“ ein, und klicken Sie auf „OK“.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Dies ist nicht zwingend erforderlich. Das Peering kann auch dann eingerichtet werden, wenn Benutzer individuell Peeringanforderungen für ihre jeweiligen VNets auslösen, solange die Anforderungen übereinstimmen. Das Hinzufügen von privilegierten Benutzern des anderen VNet als Benutzer im lokalen VNet vereinfacht das Setup im Portal.

5. Melden Sie sich anschließend am Azure-Portal mit UserB an, also mit dem privilegierten Benutzer für Abonnement B. Führen Sie die obigen Schritte zum Hinzufügen des Benutzers als „Netzwerkmitwirkender“ aus.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    HINWEIS: Sie können sich abmelden und beide Benutzersitzungen im Browser anmelden, um sich zu vergewissern, dass die Autorisierung erfolgreich aktiviert wurde.

6. Melden Sie sich am Portal als UserA an, navigieren Sie zum Blatt „VNet3“, und klicken Sie auf „Peering“. Aktivieren Sie das Kontrollkästchen „Ich kenne meine Ressourcen-ID“, und geben Sie die Ressourcen-ID für VNet5 im unten angegebenen Format ein.

    /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Network/VirtualNetwork/<VNet-Name>

    ![Ressourcen-ID](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Melden Sie sich am Portal als UserB an, und führen Sie den obigen Schritt aus, um den Peeringlink von VNet5 zu VNet3 zu erstellen.

    ![Ressourcen-ID 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Das Peering wird eingerichtet, und jeder virtuelle Computer in VNet3 sollte nun mit jedem virtuellen Computer in VNet5 kommunizieren können.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Zunächst wird der VNet-Peeringlink von HubVnet zu VNet1 erstellt. Beachten Sie, dass die Option „Weitergeleiteten Datenverkehr zulassen“ für den Link nicht ausgewählt ist.

    ![Einfaches Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Im nächsten Schritt können Peeringlinks von VNet1 zu HubVnet erstellt werden. Beachten Sie, dass die Option „Weitergeleiteten Datenverkehr zulassen“ ausgewählt ist.

    ![Einfaches Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Nachdem das Peering eingerichtet wurde, können Sie [diesen Artikel](virtual-network-create-udr-arm-ps.md) lesen und eine benutzerdefinierte Route (User-Defined Route, UDR) festlegen, um VNet1-Datenverkehr über ein virtuelles Gerät zu leiten und dessen Funktionen zu nutzen. Wenn Sie die Adresse des nächsten Hops der Route angeben, können Sie diese auf die IP-Adresse des virtuellen Geräts im per Peering verknüpften VNet (HubVNet) festlegen.

## Entfernen des VNet-Peerings

1.	Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2.	Wechseln Sie zum Blatt „Virtuelles Netzwerk“, klicken Sie auf „Peerings“ und den Link, den Sie entfernen möchten, und klicken Sie dann auf die Schaltfläche „Löschen“.

    ![Löschen1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Wenn Sie einen Link im VNet-Peering entfernen, wird als Status für den Peeringlink „Verbindung getrennt“ angezeigt.

    ![Löschen2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. Bei diesem Status können Sie den Link erst dann wieder neu erstellen, wenn sich der Status des Peeringlinks in „Initiiert“ ändert. Es wird empfohlen, beide Links zu entfernen, bevor Sie das VNet-Peering neu erstellen.

<!---HONumber=AcomDC_0824_2016-->