---
title: Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen in Azure | Microsoft-Dokumentation
description: "Lastenausgleich in Konfigurationen mit primären und sekundären IP-Adressen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: cf1e68c7b37b2506de007bdf24eea63a27187a33
ms.lasthandoff: 03/22/2017

---

# <a name="load-balancing-on-multiple-ip-configurations-using-the-azure-portal"></a>Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen mithilfe des Azure-Portals

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](load-balancer-multiple-ip-cli.md)

Dieser Artikel beschreibt die Verwendung von Azure Load Balancer mit mehreren IP-Adressen an einer sekundären Netzwerkschnittstelle (NIC). In diesem Szenario verwenden wir zwei virtuelle Computer mit Windows, von denen jeder eine primäre und eine sekundäre NIC aufweist. Die sekundären NICs haben jeweils zwei IP-Konfigurationen. Jeder virtuelle Computer hostet die beiden Websites „contoso.com“ und „fabrikam.com“. Jede Website ist an eine der IP-Konfigurationen der sekundären NIC gebunden. Wir verwenden Azure Load Balancer, um zwei Front-End-IP-Adressen verfügbar zu machen (jeweils eine für jede Website), damit der Datenverkehr an die entsprechende IP-Konfiguration für die Website verteilt werden kann. Dieses Szenario verwendet die gleiche Portnummer auf beiden Front-Ends sowie beide Back-End-Pool-IP-Adressen.

![Abbildung zum LB-Szenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Voraussetzungen
In diesem Beispiel wird angenommen, dass Sie über eine Ressourcengruppe mit dem Namen *contosofabrikam* und folgender Konfiguration verfügen:
 -  Enthalten sind ein virtuelles Netzwerk mit dem Namen *meinVNetz*, zwei virtuelle Computer *VM1* bzw. *VM2* innerhalb der gleichen Verfügbarkeitsgruppe *meineVerfügbarkeitsgruppe*. 
 - Jeder virtuelle Computer verfügt über eine primäre NIC und eine sekundäre NIC. Die primären NICs heißen *VM1NIC1* und *VM2NIC1*, und die sekundären NICs tragen die Namen *VM1NIC2* und *VM2NIC2*. Weitere Informationen zum Erstellen von virtuellen Computern mit mehreren NICs finden Sie unter [Erstellen einer VM mit mehreren Netzwerkkarten (NICs) mithilfe von PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Schritte für den Lastenausgleich bei Konfigurationen mit mehreren IP-Adressen

Führen Sie die Schritte unten aus, um das in diesem Artikel beschriebene Szenario umzusetzen:

### <a name="step-1-configure-the-secondary-nics-for-each-vm"></a>SCHRITT 1: Konfigurieren der sekundären NICs für jeden virtuellen Computer

Fügen Sie für jeden virtuellen Computer in Ihrem virtuellen Netzwerk die IP-Konfiguration für die sekundäre NIC wie folgt hinzu:  

1. Navigieren Sie in einem Browser zum Azure-Portal: http://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie oben links auf dem Bildschirm auf das Symbol „Ressourcengruppe“, und klicken Sie dann auf die Ressourcengruppe, in der sich die VMs befinden (z.B. *contosofabrikam*). Daraufhin wird das Blatt **Ressourcengruppen** angezeigt, auf dem alle Ressourcen zusammen mit den Netzwerkschnittstellen für die virtuellen Computer aufgelistet sind.
3. Fügen Sie der sekundären NIC jedes virtuellen Computers in folgender Weise eine IP-Konfiguration hinzu:
    1. Wählen Sie die Netzwerkkarte aus, der Sie die IP-Konfiguration hinzufügen möchten.
    2. Klicken Sie auf dem Blatt, das für die ausgewählte NIC angezeigt wird, auf **IP-Konfigurationen**. Klicken Sie im oberen Bereich des angezeigten Blatts auf **Hinzufügen**.
    3. Fügen Sie auf dem Blatt **IP-Konfigurationen hinzufügen** der NIC in folgender Weise eine zweite IP-Konfiguration hinzu: 
        1. Geben Sie einen Namen für Ihre sekundäre IP-Konfiguration ein (benennen Sie beispielsweise für VM1 und VM2 die IP-Konfigurationen *VM1NIC2-ipconfig2* bzw. *VM2NIC2-ipconfig2*).
        2. Wählen Sie als **Private IP-Adresse** für **Zuteilung** **Statisch** aus.
        3. Klicken Sie auf **OK**.
        4. Wenn die zweite IP-Konfiguration für die sekundäre NIC abgeschlossen ist, wird sie auf dem Einstellungsblatt **IP-Konfigurationen** für die betreffende NIC angezeigt.

### <a name="step-2-create-a-load-balancer"></a>SCHRITT 2: Erstellen eines Load Balancers

Erstellen Sie wie folgt einen Load Balancer:

1. Navigieren Sie in einem Browser zum Azure-Portal: http://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie oben links auf dem Bildschirm auf **Neu** > **Netzwerk** > **Load Balancer**. Klicken Sie dann auf **Erstellen**.
3. Geben Sie auf dem Blatt **Load Balancer erstellen** einen Namen für den Load Balancer ein. In diesem Fall heißt er *meinlb*.
4. Erstellen Sie unter „Öffentliche IP-Adresse“ eine neue öffentliche IP-Adresse mit dem Namen **ÖffentlicheIP1**.
5. Wählen Sie unter „Ressourcengruppe“ die vorhandene Ressourcengruppe Ihrer virtuellen Computer aus (z.B. *contosofabrikam*). Wählen Sie einen geeigneten Speicherort aus, und klicken Sie dann auf **OK**. Die Bereitstellung des Load Balancers wird dann gestartet und wird einige Minuten dauern.
6. Nach der Bereitstellung wird der Load Balancer in Ihrer Ressourcengruppe als Ressource angezeigt.

### <a name="step-3-configure-the-frontend-ip-pool"></a>SCHRITT 3: Konfigurieren des Front-End-IP-Adresspools

Konfigurieren Sie den Front-End-IP-Adresspool für jede Website (Contoso und Fabrikam) wie folgt:

1. Klicken Sie im Portal auf **Weitere Dienste** > geben Sie **Öffentliche IP-Adresse** in das Filterfeld ein, und klicken sie dann auf **Öffentliche IP-Adressen**. Klicken Sie im oberen Bereich des angezeigten Blatts auf **Hinzufügen**.
2. Konfigurieren Sie wie folgt zwei öffentliche IP-Adressen (*ÖffentlicheIP1* und *ÖffentlicheIP2*) für beide Websites (Contoso und Fabrikam):
    1. Geben Sie einen Namen für Ihre Front-End-IP-Adresse ein.
    2. Wählen Sie als **Ressourcengruppe** die vorhandene Ressourcengruppe der virtuellen Computer aus (zum Beispiel *contosofabrikam*).
    3. Wählen Sie als **Speicherort** den gleichen Speicherort wie für die virtuellen Computer aus.
    4. Klicken Sie auf **OK**.
    5. Nachdem die beiden öffentlichen IP-Adressen erstellt wurden, werden sie beide auf dem Blatt **Öffentliche IP-Adresse** angezeigt.
3. Klicken Sie im Portal auf **Weitere Dienste**, geben Sie im Filterfeld **Load Balancer** ein, und klicken Sie dann auf **Load Balancer**.  
4. Wählen Sie den Load Balancer (*meinlb*) aus, dem Sie den Front-End-IP-Pool hinzufügen möchten.
5. Wählen Sie unter **Einstellungen** **Front-End-Pools** aus. Klicken Sie im oberen Bereich des angezeigten Blatts auf **Hinzufügen**.
6. Geben Sie einen Namen für Ihre Front-End-IP-Adresse ein (*farbikamfe* oder **contosofe*).
7. Klicken Sie auf **IP-Adresse**, und wählen Sie auf dem Blatt **Öffentliche IP-Adresse auswählen** die IP-Adressen für Ihr Front-End aus (*ÖffentlicheIP1* oder *ÖffentlicheIP2*).
8. Wiederholen Sie die Schritte 3 bis 7 in diesem Abschnitt, um die zweite Front-End-IP-Adresse zu erstellen.
9. Wenn die Konfiguration des Front-End-IP-Pools abgeschlossen ist, werden beide Front-End-IP-Adressen auf dem Blatt **Front-End-IP-Pool** Ihres Load Balancers angezeigt. 
    
### <a name="step-4-configure-the-backend-pool"></a>SCHRITT 4: Konfigurieren des Back-End-Pools   
Konfigurieren Sie die Back-End-Adresspools Ihres Load Balancers für jede Website (Contoso und Fabrikam) wie folgt:
        
1. Klicken Sie im Portal auf **Weitere Dienste**, geben Sie im Filterfeld „Load Balancer“ ein, und klicken Sie dann auf **Load Balancer**.  
2. Wählen Sie den Load Balancer (*meinlb*) aus, dem Sie die Back-End-Pools hinzufügen möchten.
3. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** aus. Geben Sie einen Namen für den Back-End-Pool ein (z.B. *contosopool* oder *fabrikampool*). Klicken Sie dann auf die oben auf dem Blatt angezeigte Schaltfläche **Hinzufügen**. 
4. Wählen Sie als **Verknüpft mit** **Verfügbarkeitsgruppe** aus.
5. Wählen Sie unter **Verfügbarkeitsgruppe** die Option **meineVerfügbarkeitsgruppe** aus.
6. Fügen Sie die Zielnetzwerk-IP-Konfigurationen für beide virtuelle Computer wie folgt hinzu (siehe Abbildung 2):  
    1. Wählen Sie für **Virtueller Zielcomputer** den virtuellen Computer aus, den Sie dem Back-End-Pool hinzufügen möchten (z.B. VM1 oder VM2).
    2. Wählen Sie unter **Netzwerk-IP-Konfiguration** die IP-Konfiguration der sekundären NICs für den betreffenden virtuellen Computer aus (zum Beispiel VM1NIC2-ipconfig2 oder VM2NIC2-ipconfig2).
    ![LB-Szenarioabbildung](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **Abbildung 2**: Konfigurieren des Load Balancers mit Back-End-Pools  
7. Klicken Sie auf **OK**.
8. Wenn die Konfiguration des Back-End-Pools abgeschlossen ist, werden beide Back-End-Adresspools auf dem Blatt **Back-End-Pool** Ihres Load Balancers angezeigt.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>SCHRITT 5: Konfigurieren eines Integritätstests für Ihren Load Balancer
Konfigurieren Sie wie folgt einen Integritätstest für Ihren Load Balancer:
    1. Klicken Sie im Portal auf „Weitere Dienste“ >, geben Sie im Filterfeld „Load Balancer“ ein, und klicken Sie dann auf **Load Balancer**.  
    2. Wählen Sie den Load Balancer aus, dem Sie die Back-End-Pools hinzufügen möchten.
    3. Wählen Sie unter **Einstellungen** die Option **Integritätstest** aus. Klicken Sie im oberen Bereich des angezeigten Blatts auf **Hinzufügen**.
    4. Geben Sie einen Namen für den Integritätstest ein (z.B. „HTTP“), und klicken Sie auf **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>SCHRITT 6: Konfigurieren von Lastenausgleichsregeln
Konfigurieren Sie wie folgt Lastenausgleichsregeln (*HTTPc* und *HTTPf*) für jede Website:
    
1. Wählen Sie unter **Einstellungen** die Option **Integritätstest** aus. Klicken Sie im oberen Bereich des angezeigten Blatts auf **Hinzufügen**.
2. Geben Sie als **Name** einen Namen für die Lastenausgleichsregel ein (beispielsweise *HTTPc* für Contoso oder *HTTPf* für Fabrikam)
3. Wählen Sie als Front-End-IP-Adresse die Front-End-IP-Adresse aus (z.B. *Contosofe* oder *Fabrikamfe*)
4. Behalten Sie für **Port** und **Back-End-Port** den Standardwert **80** bei.
5. Klicken Sie für **Floating IP (Direct Server Return)** auf **Aktiviert**.
6. Klicken Sie auf **OK**.
7. Wiederholen Sie die Schritte 1 bis 6 in diesem Abschnitt, um die zweite Lastenausgleichsregel zu erstellen.
8. Wenn die Konfiguration der Lastenausgleichsregeln abgeschlossen ist, werden beide Regeln (*HTTPc* und *HTTPf*) auf dem Blatt **Lastenausgleichsregeln** Ihres Load Balancers angezeigt.

### <a name="step-7-configure-dns-records"></a>SCHRITT 7: Konfigurieren von DNS-Einträgen
Abschließend müssen Sie DNS-Ressourceneinträge konfigurieren, um auf die entsprechende Front-End-IP-Adresse des Load Balancers zu verweisen. Sie können Ihre Domänen in Azure DNS hosten. Weitere Informationen zur Verwendung von Azure DNS mit Lastenausgleich finden Sie unter [Verwenden von Azure DNS mit anderen Azure-Diensten](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie in [Verwenden von Lastenausgleichsdiensten in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md), wie Sie Lastenausgleichsdienste in Azure kombinieren.
- Erfahren Sie in [Log Analytics für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md), wie Sie in Azure verschiedene Protokolltypen verwenden, um den Load Balancer zu verwalten und eventuelle Fehler zu beheben.

