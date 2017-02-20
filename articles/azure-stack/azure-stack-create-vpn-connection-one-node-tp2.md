---
title: Erstellen einer Site-to-Site-VPN-Verbindung zwischen zwei virtuellen Netzwerken in unterschiedlichen Azure Stack-POC-Umgebungen | Microsoft-Dokumentation
description: "Schrittweise Anleitung für Cloudadministratoren zum Erstellen einer Site-to-Site-VPN-Verbindung zwischen zwei POC-Umgebungen mit jeweils einem Knoten in TP2."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: scottnap
translationtype: Human Translation
ms.sourcegitcommit: 5104c7996de9dc0597e65be31c28a9aaa1243a90
ms.openlocfilehash: d324290caf5b5a085a2daf67e541c295dffda732


---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-poc-environments"></a>Erstellen einer Site-to-Site-VPN-Verbindung zwischen zwei virtuellen Netzwerken in unterschiedlichen Azure Stack-POC-Umgebungen
## <a name="overview"></a>Übersicht
In diesem Artikel erfahren Sie, wie Sie eine Site-to-Site-VPN-Verbindung zwischen zwei virtuellen Netzwerken in zwei separaten Azure Stack-POC-Umgebungen (Proof of Concept) erstellen. Im Rahmen der Verbindungskonfiguration werden Sie auch mit der Funktionsweise von VPN-Gateways in Azure Stack vertraut gemacht.

> [!NOTE]
> Dieses Dokument gilt speziell für den Azure Stack TP2-POC.
> 
> 

### <a name="connection-diagram"></a>Verbindungsdiagramm
Das folgende Diagramm zeigt die fertige Konfiguration:

![](media/azure-stack-create-vpn-connection-one-node-tp2/image1.png)

### <a name="before-you-begin"></a>Voraussetzungen
Vergewissern Sie sich zunächst, dass Sie über Folgendes verfügen:

* Zwei Server, die die Azure Stack-POC-Hardwareanforderungen aus den [Azure Stack-Bereitstellungsvoraussetzungen](azure-stack-deploy.md) sowie die anderen Voraussetzungen aus diesem Dokument erfüllen.
* Das Azure Stack Technical Preview 2-Bereitstellungspaket.

## <a name="deploy-the-poc-environments"></a>Bereitstellen der POC-Umgebungen
Für diese Konfiguration werden zwei Azure Stack-POC-Umgebungen bereitgestellt.

* Die einzelnen POC-Umgebungen können auf der Grundlage der Bereitstellungsanleitung aus dem Artikel [Deploy Azure Stack POC](azure-stack-run-powershell-script.md) (Bereitstellen von Azure Stack POC) bereitgestellt werden.
  Die POC-Umgebungen werden in diesem Dokument generisch als *POC1* und *POC2* bezeichnet.

## <a name="configure-quotas-for-compute-network-and-storage"></a>Konfigurieren von Compute-, Netzwerk- und Speicherkontingenten
Konfigurieren Sie zunächst *Kontingente* für Compute, Netzwerk und Speicher. Diese Dienste können einem *Plan* und anschließend einem *Angebot* zugeordnet werden, das von Mandanten abonniert werden kann.

> [!NOTE]
> Diese Schritte müssen für jede Azure Stack-POC-Umgebung ausgeführt werden.
> 
> 

Die Kontingenterstellung hat sich im Vergleich zu TP1 geändert. Eine Kontingenterstellungsanleitung für TP2 finden Sie unter <http://aka.ms/mas-create-quotas>. Für diese Übung können Sie die Standardeinstellungen für alle Kontingenteinstellungen beibehalten.

## <a name="create-a-plan-and-offer"></a>Erstellen von Plan und Angebot
[Pläne](azure-stack-key-features.md) sind Gruppen mit mindestens einem Dienst. Als Anbieter können Sie Pläne erstellen und Ihren Mandanten anbieten. Im Gegenzug abonnieren Ihre Mandanten Ihre Angebote, um die Pläne und Dienste, die sie damit enthalten, zu verwenden.

> [!NOTE]
> Diese Schritte müssen für jede Azure Stack-POC-Umgebung ausgeführt werden.
> 
> 

1. Erstellen Sie zunächst einen Plan. Eine entsprechende Anleitung finden Sie in [diesem Onlineartikel](azure-stack-create-plan.md).
2. Erstellen Sie ein Angebot, wie in [diesem Artikel](azure-stack-create-offer.md) beschrieben.
3. Melden Sie sich beim Portal als Mandantenadministrator an, und [abonnieren Sie das erstellte Angebot](azure-stack-subscribe-plan-provision-vm.md).

## <a name="create-the-network-resources-in-poc-1"></a>Erstellen der Netzwerkressourcen in POC1
Als Nächstes erstellen Sie die eigentlichen Ressourcen, die Sie zum Einrichten Ihrer Konfiguration benötigen. Die Vorgehensweise wird in den folgenden Schritten veranschaulicht. In diesen Anweisungen werden die Ressourcen über das Portal erstellt, Sie können aber auch PowerShell verwenden.

![](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="log-in-as-a-tenant"></a>Anmelden als Mandant
Ein Dienstadministrator kann sich als Mandant anmelden, um die Pläne, Angebote und Abonnements zu testen, die den Mandanten zur Verfügung stehen. Falls Sie noch kein Mandantenkonto besitzen, [erstellen Sie ein Mandantenkonto](azure-stack-add-new-user-aad.md), bevor Sie sich anmelden.

### <a name="create-the-virtual-network--vm-subnet"></a>Erstellen des virtuellen Netzwerks und des VM-Subnetzes
1. Melden Sie sich mit einem Mandantenkonto an.
2. Klicken Sie im Azure-Portal auf **Neu**.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)
3. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
4. Klicken Sie im Menü auf **Virtuelles Netzwerk**.
5. Klicken Sie im unteren Bereich des Blatts mit der Ressourcenbeschreibung auf **Erstellen**. Geben Sie gemäß der folgenden Tabelle Werte in die entsprechenden Felder ein:
   
   | **Feld** | **Wert** |
   | --- | --- |
   | Name |vnet-01 |
   | Adressraum |10.0.10.0/23 |
   | Subnetzname |subnet-01 |
   | Subnetzadressbereich |10.0.10.0/24 |
6. Daraufhin sollte das Feld **Abonnement** mit dem zuvor erstellten Abonnement aufgefüllt werden.
7. Als Ressourcengruppe können Sie entweder eine neue Ressourcengruppe erstellen oder **Use existing** (Vorhandene verwenden) auswählen, sofern Sie bereits über eine Ressourcengruppe verfügen.
8. Überprüfen Sie den standardmäßigen Speicherort.
9. Klicken Sie auf **Erstellen**.

### <a name="create-the-gateway-subnet"></a>Erstellen des Gatewaysubnetzes
1. Öffnen Sie über das Dashboard die soeben erstellte Virtual Network-Ressource (vnet-01).
2. Wählen Sie auf dem Blatt mit den Einstellungen die Option **Subnetze** aus.
3. Klicken Sie auf **Gatewaysubnetz**, um dem virtuellen Netzwerk ein Gatewaysubnetz hinzuzufügen.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)
4. Der Name des Subnetzes wird standardmäßig auf **GatewaySubnet** festgelegt.
   Gatewaysubnetze sind spezielle Subnetze und müssen diesen spezifischen Namen besitzen, damit sie ordnungsgemäß funktionieren.
5. Geben Sie im Feld **Adressbereich** den Bereich **10.0.11.0/24** ein.
6. Klicken Sie auf **Erstellen**, um das Gatewaysubnetz zu erstellen.

### <a name="create-the-virtual-network-gateway"></a>Erstellen des virtuellen Netzwerkgateways
1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Liste mit den Netzwerkressourcen den Eintrag **Gateway von Virtual Network** aus.
4. Lesen Sie sich die Beschreibung durch, und klicken Sie auf **Erstellen**.
5. Geben Sie im Feld **Name** die Zeichenfolge **GW1** ein.
6. Klicken Sie auf **Virtuelles Netzwerk**, um ein virtuelles Netzwerk auszuwählen.
   Wählen Sie in der Liste die Option **vnet-01** aus.
7. Klicken Sie auf das Menüelement **Öffentliche IP-Adresse**. Klicken Sie auf dem Blatt **Öffentliche IP-Adresse auswählen** auf **Neu erstellen**.
8. Geben Sie im Feld **Name** die Zeichenfolge **GW1-PiP** ein, und klicken Sie auf **OK**.
9. Für **Gatewaytyp** sollte standardmäßig **VPN** ausgewählt sein. Behalten Sie diese Einstellung bei.
10. Für **VPN-Typ** sollte standardmäßig **Routenbasiert** ausgewählt sein.
    Behalten Sie diese Einstellung bei.
11. Vergewissern Sie sich, dass **Abonnement** und **Speicherort** korrekt sind. Die Ressource kann auf Wunsch an das Dashboard angeheftet werden. Klicken Sie auf **Erstellen**.

### <a name="create-the-local-network-gateway"></a>Erstellen des lokalen Netzwerkgateways
Die Implementierung eines *lokalen Netzwerkgateways* ist bei dieser Azure Stack-Evaluierungsbereitstellung ein wenig anders als bei einer tatsächlichen Azure-Bereitstellung.

Das Konzept eines lokalen Netzwerkgateways ist hier ebenso vorhanden wie in Azure. In einer Azure-Bereitstellung stellt ein lokales Netzwerkgateway allerdings ein (aus Sicht des Mandanten) lokales physisches Gerät dar, mit dem eine Verbindung mit einem virtuellen Netzwerkgateway in Azure hergestellt wird. In dieser Azure Stack-Evaluierungsbereitstellung handelt es sich jedoch bei beiden Enden der Verbindung um virtuelle Netzwerkgateways.

Etwas allgemeiner betrachtet wird mit der Ressource des lokalen Netzwerkgateways immer das Remotegateway am anderen Ende der Verbindung angegeben. Aufgrund des POC-Designs müssen Sie die IP-Adresse des externen Netzwerkadapters auf dem virtuellen NAT-Computer des anderen POC als öffentliche IP-Adresse des lokalen Netzwerkgateways angeben. Anschließend erstellen Sie NAT-Zuordnungen auf dem virtuellen NAT-Computer, um sicherzustellen, dass beide Enden ordnungsgemäß verbunden sind.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Abrufen der IP-Adresse des externen Adapters des virtuellen NAT-Computers
1. Melden Sie sich auf dem physischen Azure Stack-Computer für POC2 an.
2. Öffnen Sie durch Drücken von WINDOWS-TASTE+R das Menü **Ausführen**, geben Sie **mstsc** ein, und drücken Sie die EINGABETASTE.
3. Geben Sie im Feld **Computer** den Namen **MAS-BGPNAT01** ein, und klicken Sie auf  **Verbinden**.
4. Klicken Sie im Startmenü mit der rechten Maustaste auf **Windows PowerShell**, und wählen Sie **Als Administrator ausführen** aus.
5. Geben Sie **ipconfig /all** ein.
6. Suchen Sie nach dem mit Ihrem lokalen Netzwerk verbundenen Ethernet-Adapter, und notieren Sie sich die an diesen Adapter gebundene IPv4-Adresse. In unserer Beispielumgebung wird die Adresse **10.16.167.195** verwendet, Ihre eigene Adresse lautet allerdings anders.
7. Notieren Sie sich diese Adresse. Sie wird als öffentliche IP-Adresse der Ressource des lokalen Netzwerkgateways verwendet, die Sie in POC1 erstellen.

### <a name="create-the-local-network-gateway-resource"></a>Erstellen der Ressource des lokalen Netzwerkgateways
1. Melden Sie sich auf dem physischen Azure Stack-Computer für POC1 an.
2. Geben Sie im Feld **Computer** den Namen **MAS-CON01** ein, und klicken Sie auf **Verbinden**.
3. Klicken Sie im Azure-Portal auf **Neu**.
   
4. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
5. Wählen Sie in der Ressourcenliste den Eintrag für das lokale Netzwerkgateway** **aus.
6. Geben Sie im Feld **Name** die Zeichenfolge **POC2-GW** ein.
7. Die IP-Adresse des anderen Gateways ist Ihnen noch nicht bekannt. Das macht aber nichts, da Sie später hierher zurückkehren und sie ändern können. Geben Sie im Feld **IP-Adresse** die vorläufige Adresse **10.16.167.195** ein.
8. Geben Sie im Feld **Adressraum** den Adressraum des VNets ein, das Sie in POC2 erstellen werden. Der Adressraum wird **10.0.20.0/23** lauten. Geben Sie also diesen Wert ein.
9. Überprüfen Sie **Abonnement**, **Ressourcengruppe** und **Speicherort** auf Richtigkeit, und klicken Sie auf **Erstellen**.

### <a name="create-the-connection"></a>Erstellen der Verbindung
1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Ressourcenliste die Option **Verbindung** aus.
4. Wählen Sie auf dem Blatt **Grundlagen** unter **Verbindungstyp** die Option **Site-to-Site (IPsec)** aus.
5. Wählen Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** aus, und klicken Sie anschließend auf **OK**.
6. Wählen Sie auf dem Blatt **Einstellungen** das virtuelle Netzwerkgateway** **(**GW1**) aus, das Sie zuvor erstellt haben.
7. Wählen Sie das lokale Netzwerkgateway** **(**POC2-GW**) aus, das Sie zuvor erstellt haben.
8. Geben Sie im Feld **Verbindungsname** die Zeichenfolge **POC1-POC2** ein.
9. Geben Sie im Feld **Shared Key (PSK)** (Gemeinsam verwendeter Schlüssel (PSK)) den Wert **12345** ein, und klicken Sie anschließend auf **OK**.

### <a name="create-a-vm"></a>Erstellen eines virtuellen Computers
Zur Überprüfung der Daten, die über die VPN-Verbindung übermittelt werden, benötigen Sie in den POCs virtuelle Computer, die Daten senden und empfangen. Erstellen Sie in POC1 einen virtuellen Computer, und platzieren Sie ihn im VM-Subnetz Ihres virtuellen Netzwerks.

1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Virtual Machines** aus.
3. Wählen Sie in der Liste mit den VM-Images das Image **Windows Server 2012 R2 Datacenter** aus.
4. Geben Sie auf dem Blatt **Grundlagen** im Feld **Name** die Zeichenfolge **VM01** ein.
5. Geben Sie einen gültigen Benutzernamen und ein Kennwort ein. Mit diesem Konto melden Sie sich bei dem virtuellen Computer an, nachdem dieser erstellt wurde.
6. Geben Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** an, und klicken Sie anschließend auf **OK**.
7. Wählen Sie auf dem Blatt **Größe** eine VM-Größe für diese Instanz aus, und klicken Sie anschließend auf **Auswählen**.
8. Auf dem Blatt **Einstellungen** können Sie die Standardwerte übernehmen. Vergewissern Sie sich lediglich, dass das virtuelle Netzwerk **VNET-01** ausgewählt und das Subnetz auf **10.0.10.0/24** festgelegt ist. Klicken Sie auf **OK**.
9. Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie anschließend auf **OK**.

## <a name="create-the-network-resources-in-poc-2"></a>Erstellen der Netzwerkressourcen in POC2
### <a name="log-in-as-a-tenant"></a>Anmelden als Mandant
Ein Dienstadministrator kann sich als Mandant anmelden, um die Pläne, Angebote und Abonnements zu testen, die den Mandanten zur Verfügung stehen. Falls Sie noch kein Mandantenkonto besitzen, [erstellen Sie ein Mandantenkonto](azure-stack-add-new-user-aad.md), bevor Sie sich anmelden.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Erstellen des virtuellen Netzwerks und des VM-Subnetzes
1. Melden Sie sich mit einem Mandantenkonto an.
2. Klicken Sie im Azure-Portal auf **Neu**.
   
3. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
4. Klicken Sie im Menü auf **Virtuelles Netzwerk**.
5. Klicken Sie im unteren Bereich des Blatts mit der Ressourcenbeschreibung auf **Erstellen**. Geben Sie die Werte aus der folgenden Tabelle in die entsprechenden Felder ein:
   
   | **Feld** | **Wert** |
   | --- | --- |
   | Name |vnet-02 |
   | Adressraum |10.0.20.0/23 |
   | Subnetzname |subnet-02 |
   | Subnetzadressbereich |10.0.20.0/24 |
6. Daraufhin sollte das Feld **Abonnement** mit dem zuvor erstellten Abonnement aufgefüllt werden.
7. Als Ressourcengruppe können Sie entweder eine neue Ressourcengruppe erstellen oder **Use existing** (Vorhandene verwenden) auswählen, sofern Sie bereits über eine Ressourcengruppe verfügen.
8. Überprüfen Sie den standardmäßigen **Speicherort**. Das virtuelle Netzwerk kann auf Wunsch an das Dashboard angeheftet werden, um komfortabel darauf zugreifen zu können.
9. Klicken Sie auf **Erstellen**.

### <a name="create-the-gateway-subnet"></a>Erstellen des Gatewaysubnetzes
1. Öffnen Sie über das Dashboard die zuvor erstellte Ressource des virtuellen Netzwerks (**vnet-02**).
2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Subnetze** aus.
3. Klicken Sie auf **Gatewaysubnetz**, um dem virtuellen Netzwerk ein Gatewaysubnetz hinzuzufügen.
   
4. Der Name des Subnetzes wird standardmäßig auf **GatewaySubnet** festgelegt.
   Gatewaysubnetze sind spezielle Subnetze und müssen diesen spezifischen Namen besitzen, damit sie ordnungsgemäß funktionieren.
5. Geben Sie im Feld **Adressbereich** den Bereich **10.0.20.0/24** ein.
6. Klicken Sie auf **Erstellen**, um das Gatewaysubnetz zu erstellen.

### <a name="create-the-virtual-network-gateway"></a>Erstellen des virtuellen Netzwerkgateways
1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Liste mit den Netzwerkressourcen den Eintrag **Gateway von Virtual Network** aus.
4. Lesen Sie sich die Beschreibung durch, und klicken Sie auf **Erstellen**.
5. Geben Sie im Feld **Name** die Zeichenfolge **GW2** ein.
6. Klicken Sie auf **Virtuelles Netzwerk**, um ein virtuelles Netzwerk auszuwählen.
   Wählen Sie in der Liste die Option **vnet-02** aus.
7. Klicken Sie auf **Öffentliche IP-Adresse**. Klicken Sie auf dem Blatt **Öffentliche IP-Adresse auswählen** auf **Neu erstellen**.
8. Geben Sie im Feld **Name** die Zeichenfolge **GW2-PiP** ein, und klicken Sie auf **OK**.
9. Für **Gatewaytyp** sollte standardmäßig **VPN** ausgewählt sein. Behalten Sie diese Einstellung bei.
10. Für **VPN-Typ** sollte standardmäßig **Routenbasiert** ausgewählt sein.
    Behalten Sie diese Einstellung bei.
11. Vergewissern Sie sich, dass **Abonnement** und **Speicherort** korrekt angegeben sind. Die Ressource kann auf Wunsch an das Dashboard angeheftet werden. Klicken Sie auf **Erstellen**.

### <a name="create-the-local-network-gateway"></a>Erstellen des lokalen Netzwerkgateways
#### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Abrufen der IP-Adresse des externen Adapters des virtuellen NAT-Computers
1. Melden Sie sich auf dem physischen Azure Stack-Computer für POC1 an.
2. Öffnen Sie durch Drücken von WINDOWS-TASTE+R das Menü **Ausführen**, geben Sie **mstsc** ein, und drücken Sie die EINGABETASTE.
3. Geben Sie im Feld **Computer** den Namen **MAS-BGPNAT01** ein, und klicken Sie auf  **Verbinden**.
4. Klicken Sie im Startmenü mit der rechten Maustaste auf **Windows PowerShell**, und wählen Sie **Als Administrator ausführen** aus.
5. Geben Sie **ipconfig /all** ein.
6. Suchen Sie nach dem mit Ihrem lokalen Netzwerk verbundenen Ethernet-Adapter, und notieren Sie sich die an diesen Adapter gebundene IPv4-Adresse. In unserer Beispielumgebung wird die Adresse **10.16.169.131** verwendet, Ihre eigene Adresse lautet allerdings anders.
7. Notieren Sie sich diese Adresse. Sie wird später als öffentliche IP-Adresse der Ressource des lokalen Netzwerkgateways verwendet, die Sie in POC1 erstellen.

#### <a name="create-the-local-network-gateway-resource"></a>Erstellen der Ressource des lokalen Netzwerkgateways
1. Melden Sie sich auf dem physischen Azure Stack-Computer für POC2 an.
2. Geben Sie im Feld **Computer** den Namen **MAS-CON01** ein, und klicken Sie auf **Verbinden**.
3. Klicken Sie im Azure-Portal auf **Neu**.
   
4. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
5. Wählen Sie in der Ressourcenliste den Eintrag für das lokale Netzwerkgateway** **aus.
6. Geben Sie im Feld **Name** die Zeichenfolge **POC1-GW** ein.
7. Nun benötigen Sie die öffentliche IP-Adresse, die Sie sich für das virtuelle Netzwerkgateway in POC1 notiert haben. Geben Sie im Feld **IP-Adresse** die Adresse **10.16.169.131** ein.
8. Geben Sie im Feld **Adressraum** den Adressraum von **vnet-01** aus POC1 ein: **10.0.0.0/16**.
9. Überprüfen Sie **Abonnement**, **Ressourcengruppe** und **Speicherort** auf Richtigkeit, und klicken Sie auf **Erstellen**.

## <a name="create-the-connection"></a>Erstellen der Verbindung
1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Netzwerk** aus.
3. Wählen Sie in der Ressourcenliste die Option **Verbindung** aus.
4. Wählen Sie auf dem Blatt **Grundlagen** unter **Verbindungstyp** die Option **Site-to-Site (IPsec)** aus.
5. Wählen Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** aus, und klicken Sie anschließend auf **OK**.
6. Wählen Sie auf dem Blatt **Einstellungen** das virtuelle Netzwerkgateway** **(**GW1**) aus, das Sie zuvor erstellt haben.
7. Wählen Sie das lokale Netzwerkgateway** **(**POC1-GW**) aus, das Sie zuvor erstellt haben.
8. Geben Sie im Feld **Verbindungsname** die Zeichenfolge **POC2-POC1** ein.
9. Geben Sie im Feld **Shared Key (PSK)** (Gemeinsam verwendeter Schlüssel (PSK)) den Wert **12345** ein. Falls Sie einen anderen Wert verwenden, vergessen Sie nicht, dass er dem Wert für den gemeinsam verwendeten Schlüssel entsprechen muss, den Sie für POC1 erstellt haben. Klicken Sie auf **OK**.

## <a name="create-a-vm"></a>Erstellen eines virtuellen Computers
Erstellen Sie in POC1 einen virtuellen Computer, und platzieren Sie ihn im VM-Subnetz Ihres virtuellen Netzwerks.

1. Klicken Sie im Azure-Portal auf **Neu**.
   
2. Wählen Sie im Marketplace-Menü die Option **Virtual Machines** aus.
3. Wählen Sie in der Liste mit den VM-Images das Image **Windows Server 2012 R2 Datacenter** aus.
4. Geben Sie auf dem Blatt **Grundlagen** im Feld **Name** die Zeichenfolge **VM02** ein.
5. Geben Sie einen gültigen Benutzernamen und ein Kennwort ein. Mit diesem Konto melden Sie sich bei dem virtuellen Computer an, nachdem dieser erstellt wurde.
6. Geben Sie Werte für **Abonnement**, **Ressourcengruppe** und **Speicherort** an, und klicken Sie anschließend auf **OK**.
7. Wählen Sie auf dem Blatt **Größe** eine VM-Größe für diese Instanz aus, und klicken Sie anschließend auf **Auswählen**.
8. Auf dem Blatt mit den Einstellungen können Sie die Standardwerte übernehmen. Vergewissern Sie sich lediglich, dass das virtuelle Netzwerk **VNET-02** ausgewählt und das Subnetz auf **20.0.0.0/24** festgelegt ist. Klicken Sie auf **OK**.
9. Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie anschließend auf **OK**.

## <a name="configure-the-nat-vm-in-each-poc-for-gateway-traversal"></a>Konfigurieren des virtuellen NAT-Computers in den einzelnen POCs für das Durchlaufen des Gateways
Da der POC entwurfsbedingt eigenständig und von dem Netzwerk isoliert ist, in dem der physische Host bereitgestellt wird, handelt es sich bei dem externen VIP-Netzwerk, mit dem die Gateways verbunden sind, nicht wirklich um ein externes Netzwerk. Das Netzwerk befindet sich vielmehr hinter einem Router, der eine Netzwerkadressenübersetzung (Network Address Translation, NAT) durchführt. Der Router ist eigentlich ein virtueller Windows Server-Computer (**MAS-BGPNAT01**), der die RRAS-Rolle (Routing and Remote Access Services; Routing- und RAS-Dienste) in der POC-Infrastruktur ausführt. Sie müssen die Netzwerkadressenübersetzung auf dem virtuellen Computer „MAS-BGPNAT01“ so konfigurieren, dass die Site-to-Site-VPN-Verbindung an beiden Enden möglich ist. Hierzu müssen Sie eine statische NAT-Zuordnung erstellen, die die externe Schnittstelle des virtuellen Computers „BGPNAT“ der VIP des Edge-Gatewaypools für die Ports zuordnet, die für eine VPN-Verbindung erforderlich sind.

> [!NOTE]
> Diese Konfiguration muss nur für POC-Umgebungen vorgenommen werden.
> 
> 

### <a name="configure-nat"></a>Konfigurieren der Netzwerkadressenübersetzung
Die folgenden Schritte müssen in BEIDEN POC-Umgebungen ausgeführt werden:

1. Melden Sie sich auf dem physischen Azure Stack-Computer für POC1 an.
2. Öffnen Sie durch Drücken von WINDOWS-TASTE+R das Menü **Ausführen**, geben Sie **mstsc** ein, und drücken Sie die** **EINGABETASTE.
3. Geben Sie im Feld **Computer** den Namen **MAS-BGPNAT01** ein, und klicken Sie auf **Verbinden**.
4. Klicken Sie im Startmenü mit der rechten Maustaste auf **Windows PowerShell**, und wählen Sie **Als Administrator ausführen** aus.
5. Geben Sie **ipconfig /all** ein.
6. Suchen Sie nach dem mit Ihrem lokalen Netzwerk verbundenen Ethernet-Adapter, und notieren Sie sich die an diesen Adapter gebundene IPv4-Adresse. In unserer Beispielumgebung wird die Adresse **10.16.169.131** (rot eingekreist) verwendet, Ihre eigene Adresse lautet allerdings anders.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image16.png)
7. Geben Sie den folgenden PowerShell-Befehl ein, um die externe NAT-Adresse für die Ports festzulegen, die die IKE-Authentifizierung verwenden. Vergessen Sie nicht, die IP-Adresse auf die passende IP-Adresse für Ihre Umgebung festzulegen.
   
       Add-NetNatExternalAddress -NatName BGPNAT -IPAddress 10.16.169.131 -PortStart 499 -PortEnd 501
8. Erstellen Sie als Nächstes eine statische NAT-Zuordnung, um die externe Adresse der öffentlichen Gateway-IP-Adresse zuzuordnen und dadurch den ISAKMP-Port 500 für PHASE 1 des IPsec-Tunnels zuzuordnen.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 500 -InternalPort 500
> [!NOTE] 
> Bei dem Parameter `-InternalAddress` handelt es sich um die öffentliche IP-Adresse des virtuellen Netzwerkgateways, das Sie zuvor erstellt haben.  Diese IP-Adresse finden Sie in den Eigenschaften des Blatts für das virtuelle Netzwerkgateway unter „Öffentliche IP-Adresse“.       

9. Anschließend müssen Sie noch den NAT-Durchlauf konfigurieren. Dabei wird der Port 4500 verwendet, um den vollständigen IPsec-Tunnel über NAT-Geräte einzurichten.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 4500 -InternalPort 4500
> [!NOTE] 
> Bei dem Parameter `-InternalAddress` handelt es sich um die öffentliche IP-Adresse des virtuellen Netzwerkgateways, das Sie zuvor erstellt haben.  Diese IP-Adresse finden Sie in den Eigenschaften des Blatts für das virtuelle Netzwerkgateway unter „Öffentliche IP-Adresse“.       

10. Wiederholen Sie die Schritte 1–9 für POC2.

## <a name="test-the-connection"></a>Testen der Verbindung
Nach der Einrichtung der Site-to-Site-Verbindung sollten Sie sich vergewissern, dass darüber Datenverkehr abgewickelt werden kann. Dazu müssen Sie sich lediglich bei einem virtuellen Computer anmelden, den Sie in einer der POC-Umgebungen erstellt haben, und den virtuellen Computer pingen, den Sie in der anderen Umgebung erstellt haben. Um sicherzustellen, dass der Datenverkehr über die Site-to-Site-Verbindung abgewickelt wird, müssen Sie anstelle der VIP-Adresse die direkte IP-Adresse (DIP) des virtuellen Computers im Remotesubnetz pingen. Hierzu müssen Sie die Adresse am anderen Ende der Verbindung ermitteln und notieren.

### <a name="log-in-to-the-tenant-vm-in-poc1"></a>Anmelden beim virtuellen Mandantencomputer in POC1
1. Melden Sie sich beim physischen Azure Stack-Computer für POC1 und anschließend mithilfe des Mandantenkontos beim Portal an.
2. Klicken Sie auf der linken Navigationsleiste auf **Virtual Machines**.
3. Klicken Sie in der Liste mit den virtuellen Computern auf den virtuellen Computer **VM01**, den Sie zuvor erstellt haben.
4. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Verbinden**.
   
     ![](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Öffnen Sie auf dem virtuellen Computer eine Eingabeaufforderung, und geben Sie **ipconfig /all** ein.
6. Suchen Sie in der Ausgabe nach der **IPv4-Adresse**, und notieren Sie sie. Diese Adresse wird später von POC2 aus gepingt. In der Beispielumgebung wird die Adresse **10.0.10.4** verwendet, in Ihrer Umgebung lautet sie aber unter Umständen anders. Sie sollte allerdings innerhalb des zuvor erstellten Subnetzes **10.0.10.0/24** liegen.

### <a name="log-in-to-the-tenant-vm-in-poc2"></a>Anmelden beim virtuellen Mandantencomputer in POC2
1. Melden Sie sich beim physischen Azure Stack-Computer für POC2 und anschließend mithilfe des Mandantenkontos beim Portal an.
2. Klicken Sie auf der linken Navigationsleiste auf **Virtual Machines**.
3. Klicken Sie in der Liste mit den virtuellen Computern auf den virtuellen Computer **VM02**, den Sie zuvor erstellt haben.
4. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Verbinden**.
   
5. Öffnen Sie auf dem virtuellen Computer eine Eingabeaufforderung, und geben Sie **ipconfig /all** ein.
6. Daraufhin sollte eine IPv4-Adresse aus dem Adressbereich 10.0.20.0/24 angezeigt werden. In der Beispielumgebung wird die Adresse 10.0.20.4 verwendet, Ihre Adresse lautet aber unter Umständen anders.
7. Auf dem virtuellen Computer in POC2 möchten Sie nun über den Tunnel den virtuellen Computer in POC1 pingen. Hierzu pingen Sie die DIP-Adresse, die Sie für „VM01“ erfasst haben.
   In der Beispielumgebung wird die Adresse 10.0.10.4 verwendet. Achten Sie jedoch darauf, dass Sie die Adresse pingen, die Sie in Ihrem Lab notiert haben. Das Ergebnis sollte in etwa wie folgt aussehen:
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
8. Wenn Sie eine Antwort vom virtuellen Remotecomputer erhalten, war der Test erfolgreich. Das VM-Verbindungsfenster kann geschlossen werden. Alternativ können Sie weitere Datenübertragungen ausführen und beispielsweise eine Datei kopieren, um Ihre Verbindung zu testen.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Anzeigen der Datenübertragungsstatistik über die Gatewayverbindung
Auf dem Verbindungsblatt können Sie die Datenmenge ermitteln, die über die Site-to-Site-Verbindung übertragen wird. Mit diesem Test können Sie auch überprüfen, ob der soeben gesendete Ping tatsächlich die VPN-Verbindung durchlaufen hat.

1. Vergewissern Sie sich, dass Sie noch bei einem virtuellen Mandantencomputer in POC2 angemeldet sind, und melden Sie sich anschließend mithilfe Ihres Mandantenkontos beim **Microsoft Azure Stack POC-Portal** an.
2. Klicken Sie auf das Menüelement **Durchsuchen**, und wählen Sie **Verbindungen** aus.
3. Klicken Sie in der Liste auf die Verbindung **POC2-POC1**.
4. Auf dem Verbindungsblatt wird die Statistik für **eingehende Daten** und **ausgehende Daten** angezeigt. Auf dem folgenden Screenshot sehen Sie einige größere Zahlen, die nicht nur von einem Ping stammen können. Diese sind auf zusätzliche Dateiübertragungen zurückzuführen. Bei Ihnen sollten dort Werte angezeigt werden, die nicht Null sind.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)




<!--HONumber=Feb17_HO2-->


