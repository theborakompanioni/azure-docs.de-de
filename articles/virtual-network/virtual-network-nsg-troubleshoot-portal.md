---
title: Problembehandlung bei Netzwerksicherheitsgruppen – Portal | Microsoft Docs
description: Informationen zur Problembehandlung bei Netzwerksicherheitsgruppen im Azure Resource Manager-Bereitstellungsmodell über das Azure-Portal.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa

---
# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Wenn Sie Netzwerksicherheitsgruppen (NSGs) auf dem virtuellen Computer (VM) konfiguriert haben und Probleme bei der Verbindung mit dem virtuellen Computer auftreten, bietet dieser Artikel eine Übersicht über Diagnosefunktionen für NSGs zur weiteren Problembehandlung.

Über NSGs können Sie die Typen des Datenverkehrs steuern, der bei den virtuellen Computern ein- und ausgeht. NSGs können auf Subnetze in einem virtuellen Azure-Netzwerk (VNET), auf Netzwerkschnittstellen (NIC) oder auf beide angewendet werden. Die auf eine NIC angewendeten effektiven Regeln sind eine Aggregation der Regeln, die in den auf eine NIC angewendeten NSGs und in dem Subnetz vorhanden sind, mit dem sie verbunden ist. Die Regeln für diese NSGs können manchmal in Konflikt miteinander stehen und sich auf die Netzwerkverbindung eines virtuellen Computers auswirken.  

Sie können alle effektiven Sicherheitsregeln der NSGs anzeigen, die auf NICs der virtuellen Computer angewendet werden. In diesem Artikel wird erläutert, wie Verbindungsprobleme bei virtuellen Computern mithilfe dieser Regeln im Azure Resource Manager-Bereitstellungsmodell behoben werden. Wenn Sie nicht mit dem VNET- und NSG-Konzept vertraut sind, finden Sie entsprechende Informationen in den Übersichtsartikeln zu [virtuellen Netzwerken](virtual-networks-overview.md) und [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Verwenden von effektiven Sicherheitsregeln zur Problembehandlung des Datenverkehrsflusses auf virtuellen Computern
Das folgende Szenario ist ein Beispiel für ein häufig auftretendes Verbindungsproblem:

Der virtuelle Computer *VM1* gehört zum Subnetz *Subnet1* in einem VNET mit dem Namen *WestUS-VNet1*. Der Versuch, mit RDP über den TCP-Port 3389 eine Verbindung mit dem virtuellen Computer herzustellen, schlägt fehl. NSGs werden auf die NIC *VM1-NIC1* sowie auf das Subnetz *Subnet1* angewendet. Der Datenverkehr an TCP-Port 3389 ist zulässig in der NSG, die der Netzwerkschnittstelle *VM1-NIC1*zugeordnet ist, der TCP-Ping an Port 3389 von VM1 schlägt jedoch fehl.

Obwohl in diesem Beispiel TCP-Port 3389 verwendet wird, können anhand der folgenden Schritte auch Fehler bei eingehenden und ausgehenden Verbindungen über jeden anderen Port ermittelt werden.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Anzeigen der effektiven Sicherheitsregeln für einen virtuellen Computer
Führen Sie die folgenden Schritte aus, um Probleme bei NSGs für einen virtuellen Computer zu beheben:

Sie können die vollständige Liste der effektiven Sicherheitsregeln für eine NIC über den virtuellen Computer selbst anzeigen. Zudem können Sie NSG-Regeln für die NIC und das Subnetz auf dem Blatt für die effektiven Regeln hinzufügen, ändern und löschen, wenn Sie über die Berechtigungen zum Durchführen dieser Vorgänge verfügen.

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann in der angezeigten Liste auf **Virtuelle Computer**.
3. Wählen Sie in der angezeigten Liste einen virtuellen Computer für die Problembehandlung aus. Für den entsprechenden virtuellen Computer wird dann ein Blatt mit Optionen angezeigt.
4. Klicken Sie auf **Diagnose und Problembehandlung**, und wählen Sie dann ein gängiges Problem aus. In diesem Beispiel wird **Ich kann keine Verbindung mit meinem virtuellen Windows-Computer herstellen** ausgewählt. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Unter der Problembeschreibung werden verschiedene Schritte angezeigt (siehe folgende Abbildung): 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Klicken Sie in der Liste der empfohlenen Schritte auf *effective security group rules* (Effektive Sicherheitsgruppenregeln).
6. Das Blatt **Get effective security rules** (Effektive Sicherheitsregeln abrufen) wird angezeigt, wie in der folgenden Abbildung dargestellt:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    Beachten Sie die folgenden Abschnitte in der Abbildung:
   
   * **Bereich:** ist auf den in Schritt 3 ausgewählten virtuellen Computer *VM1*festgelegt.
   * **Netzwerkschnittstelle:** *VM1-NIC1* ist ausgewählt. Ein virtueller Computer kann über mehrere Netzwerkschnittstellen (NIC) verfügen. Jede NIC kann eindeutige effektive Sicherheitsregeln aufweisen. Bei der Problembehandlung müssen Sie möglicherweise die effektiven Sicherheitsregeln für jede NIC anzeigen.
   * **Associated NSGs** (Zugeordnete NSGs): NSGs können auf die NIC sowie das Subnetz angewendet werden, mit dem die NIC verbunden ist. In der Abbildung wurde eine NSG sowohl auf die NIC als auch auf das Subnetz angewendet, mit dem sie verbunden ist. Sie können auf die Namen der NSGs klicken, um Regeln in den NSGs direkt zu ändern.
   * **Registerkarte „VM1-nsg“:** In der Abbildung wird die Liste der Regeln für die auf die NIC angewendete NSG angezeigt. Beim Erstellen einer NSG werden in Azure immer mehrere Standardregeln erstellt. Die Standardregeln können nicht entfernt werden, Sie können sie jedoch durch Regeln mit höherer Priorität überschreiben. Weitere Informationen zu Standardregeln finden Sie im Artikel [Übersicht über NSGs](virtual-networks-nsg.md#default-rules) .
   * **Spalte „ZIEL“:** Für einige Regeln enthält diese Spalte Text, für andere sind Adresspräfixe aufgeführt. Der Text entspricht dem Namen der Standardtags, die beim Erstellen auf die Sicherheitsregel angewendet werden. Die Tags sind vom System bereitgestellte Bezeichner, die mehrere Präfixe darstellen. Bei der Auswahl einer Regel mit einem Tag, etwa *AllowInternetOutBound*, werden die Präfixe auf dem Blatt **Address prefixes** (Adresspräfixe) aufgeführt.
   * **Herunterladen:** Die Liste der Regeln kann lang sein. Sie können eine CSV-Datei der Regeln für die Offlineanalyse herunterladen, indem Sie auf **Herunterladen** klicken und die Datei speichern.
   * **Eingehende Regel „AllowRDP“:** Diese Regel ermöglicht RDP-Verbindungen mit dem virtuellen Computer.
7. Klicken Sie auf die Registerkarte **Subnet1-NSG** , um die effektiven Regeln der NSG anzuzeigen, die auf das Subnetz angewendet wird (siehe folgende Abbildung): 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    Beachten Sie die *eingehende* **denyRDP** . Eingehende Regeln, die auf das Subnetz angewendet werden, werden vor den auf die Netzwerkschnittstelle angewendeten Regeln ausgewertet. Da die Ablehnungsregel auf das Subnetz angewendet wird, schlägt die Anforderung für die Verbindungsherstellung mit TCP-Port 3389 fehl, weil die Zulassungsregel für die NIC niemals ausgewertet wird. 
   
    Die Regel *denyRDP* ist also der Grund, warum die RDP-Verbindung fehlschlägt. Durch Entfernen der Regel sollte das Problem behoben sein.
   
   > [!NOTE]
   > Wenn der der NIC zugeordnete virtuelle Computer nicht aktiv ist oder keine NSGs auf die NIC oder das Subnetz angewendet wurden, werden keine Regeln angezeigt.
   > 
   > 
8. Zum Bearbeiten der NSG-Regeln klicken Sie im Abschnitt *Associated NSGs* (Zugeordnete NSGs) auf **Subnet1-NSG** .
   Dadurch wird das Blatt **Subnet1-NSG** geöffnet. Sie können die Regeln direkt bearbeiten, indem Sie auf **Eingangssicherheitsregeln**klicken.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. Nach dem Entfernen der eingehenden Regel *denyRDP* in **Subnet1-NSG** und dem Hinzufügen der Regel *allowRDP* wird die in der folgenden Abbildung dargestellte Liste der effektiven Regeln angezeigt:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    Vergewissern Sie sich, dass TCP-Port 3389 geöffnet ist, indem Sie eine RDP-Verbindung mit dem virtuellen Computer öffnen oder das Tool PsPing verwenden. Weitere Informationen zu PsPing finden Sie auf der [PsPing-Downloadseite](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Anzeigen der effektiven Sicherheitsregeln für eine Netzwerkschnittstelle
Wenn der Datenverkehr auf dem virtuellen Computer für eine bestimmte NIC beeinträchtigt ist, können Sie eine vollständige Liste der effektiven Regeln für die NIC über den Kontext der Netzwerkschnittstellen anzeigen, indem Sie die folgenden Schritte ausführen:

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann in der angezeigten Liste auf **Netzwerkschnittstellen**.
3. Wählen Sie eine Netzwerkschnittstelle aus. In der folgenden Abbildung wird eine NIC mit dem Namen *VM1-NIC1* ausgewählt.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    Beachten Sie, dass **Bereich** auf die ausgewählte Netzwerkschnittstelle festgelegt ist. Weitere Informationen zu den angezeigten zusätzlichen Angaben finden Sie in Schritt 6 des Abschnitts zum **** Beheben von Problemen bei NSGs für einen virtuellen Computer in diesem Artikel.
   
   > [!NOTE]
   > Wenn eine NSG von einer Netzwerkschnittstelle entfernt wird, ist die Subnetz-NSG für die betreffende NIC weiterhin gültig. In diesem Fall werden in der Ausgabe nur Regeln der Subnetz-NSG angezeigt. Regeln werden nur angezeigt, wenn die NIC an einen virtuellen Computer angefügt ist.
   > 
   > 
4. Regeln für NSGs, die einer NIC und einem Subnetz zugeordnet sind, können direkt bearbeitet werden. Die entsprechenden Informationen finden Sie in Schritt 8 des Abschnitts **Anzeigen der effektiven Sicherheitsregeln für einen virtuellen Computer** in diesem Artikel.

## <a name="view-effective-security-rules-for-a-network-security-group-(nsg)"></a>Anzeigen der effektiven Sicherheitsregeln für eine Netzwerksicherheitsgruppe (NSG)
Wenn Sie NSG-Regeln ändern, können Sie die Auswirkung der für einen bestimmten virtuellen Computer hinzugefügten Regeln überprüfen. Sie können eine vollständige Liste der effektiven Sicherheitsregeln für alle NICs anzeigen, auf die eine bestimmte NSG angewendet wird, ohne auf dem Blatt der entsprechenden NSG den Kontext wechseln zu müssen. Führen Sie zur Problembehandlung der effektiven Regeln in einer NSG die folgenden Schritte aus:

1. Melden Sie sich unter „https://portal.azure.com“ beim Azure-Portal an.
2. Klicken Sie auf **Weitere Dienste** und dann in der angezeigten Liste auf **Netzwerksicherheitsgruppen**.
3. Wählen Sie eine NSG aus. In der folgenden Abbildung wurde eine NSG mit dem Namen „VM1-nsg“ ausgewählt.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    Beachten Sie die folgenden Abschnitte in der vorherigen Abbildung:
   
   * **Bereich:** ist auf die ausgewählte NSG festgelegt.
   * **Virtueller Computer:** Wenn eine NSG auf ein Subnetz angewendet wird, wird sie auf alle Netzwerkschnittstellen angewendet, die an alle mit dem Subnetz verbundenen virtuellen Computer angefügt sind. In dieser Liste werden alle virtuellen Computer angezeigt, auf die die NSG angewendet wird. Sie können alle virtuellen Computer in der Liste auswählen.
     
     > [!NOTE]
     > Wenn eine NSG nur auf ein leeres Subnetz angewendet wird, werden keine virtuellen Computer aufgelistet. Wenn eine NSG auf eine NIC angewendet wird, die keinem virtuellen Computer zugeordnet ist, wird diese NIC auch nicht aufgeführt. 
     > 
     > 
   * **Netzwerkschnittstelle:** Ein virtueller Computer kann über mehrere Netzwerkschnittstellen verfügen. Sie können eine an den ausgewählten virtuellen Computer angefügte Netzwerkschnittstelle auswählen.
   * **Associated NSGs** (Zugeordnete NSGs): Eine NIC kann zu jeder Zeit bis zu zwei effektive NSGs aufweisen, wobei eine auf die NIC und die andere auf das Subnetz angewendet wird. Obwohl „VM1-nsg“ als Bereich ausgewählt ist, werden in der Ausgabe beide NSGs angezeigt, wenn die NIC eine effektive Subnetz-NSG aufweist.
4. Regeln für NSGs, die einer NIC oder einem Subnetz zugeordnet sind, können direkt bearbeitet werden. Die entsprechenden Informationen finden Sie in Schritt 8 des Abschnitts **Anzeigen der effektiven Sicherheitsregeln für einen virtuellen Computer** in diesem Artikel.

Weitere Informationen zu den angezeigten zusätzlichen Angaben finden Sie in Schritt 6 des Abschnitts **Anzeigen der effektiven Sicherheitsregeln für einen virtuellen Computer** in diesem Artikel.

> [!NOTE]
> Obwohl auf ein Subnetz und eine NIC jeweils nur eine NSG angewendet werden kann, kann eine NSG mehreren NICs und mehreren Subnetzen zugeordnet werden.
> 
> 

## <a name="considerations"></a>Überlegungen
Beachten Sie bei der Problembehandlung von Verbindungsproblemen die folgenden Punkte:

* NSG-Standardregeln blockieren den eingehenden Zugriff über das Internet und lassen nur eingehenden VNET-Datenverkehr zu. Es müssen explizit Regeln hinzugefügt werden, die den eingehenden Zugriff über das Internet nach Bedarf zulassen.
* Wenn die Netzverbindung eines virtuellen Computers nicht aufgrund von NSG-Sicherheitsregeln fehlschlägt, kann das Problem folgende Ursachen haben:
  * Firewallsoftware, die im Betriebssystem des virtuellen Computers ausgeführt wird
  * Es sind Routen für virtuelle Geräte oder den lokalen Datenverkehr konfiguriert. Der Internetdatenverkehr kann über die Tunnelerzwingung lokal umgeleitet werden. Eine RDP/SSH-Verbindung über das Internet mit dem virtuellen Computer wird mit dieser Einstellung möglicherweise nicht hergestellt, je nachdem, wie dieser Datenverkehr auf der lokalen Hardware behandelt wird. Im Artikel [Problembehandlung bei Routen](virtual-network-routes-troubleshoot-powershell.md) finden Sie Informationen zur Diagnose von Problemen bei Routen, die möglicherweise den eingehenden und ausgehenden Datenverkehr auf dem virtuellen Computer verhindern. 
* Beim Peering von VNETs wird das Tag „VIRTUAL_NETWORK“ standardmäßig automatisch erweitert und enthält Präfixe für die per Peering verknüpften VNETs. Sie können diese Präfixe in der Liste **ExpandedAddressPrefix** anzeigen, um Probleme im Hinblick auf VNET-Peering-Verbindungen zu beheben. 
* Effektive Sicherheitsregeln werden nur angezeigt, wenn der NIC oder dem Subnetz des virtuellen Computers eine NSG zugeordnet ist. 
* Wenn der NIC oder dem Subnetz keine NSGs zugeordnet sind und dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen ist, sind alle Ports für den eingehenden und ausgehenden Zugriff geöffnet. Wenn der virtuelle Computer über eine öffentliche IP-Adresse verfügt, empfiehlt es sich dringend, NSGs auf die NIC oder das Subnetz anzuwenden.

<!--HONumber=Oct16_HO2-->


