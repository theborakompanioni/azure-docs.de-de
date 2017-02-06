---
title: "Installieren eines Active Directory-Replikatdomänencontrollers in Azure | Microsoft Docs"
description: "Ein Lernprogramm, in dem Sie erfahren, wie Sie einen Domänencontroller aus einer lokalen Active Directory-Gesamtstruktur auf einem virtuellen Azure-Computer installieren."
services: virtual-network
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: virtual-network
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 4edd2696c9a5709ded6e2a3e352090775335f0d2
ms.openlocfilehash: 56dd9b73a31d27c2c953bf4350e55e851f634b79


---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installieren eines Active Directory-Replikatdomänencontrollers in einem virtuellen Azure-Netzwerk
In diesem Thema wird gezeigt, wie zusätzliche Domänencontroller (die auch als Replikatdomänencontroller bezeichnet werden) für eine lokale Active Directory-Domäne auf virtuellen Azure-Computern in einem virtuellen Azure-Netzwerk installiert werden.

Folgende Themen könnten für Sie ebenfalls von Interesse sein:

* Sie können optional eine neue Active Directory-Gesamtstruktur in einem virtuellen Azure-Netzwerk installieren. Diese Schritte finden Sie unter [Installieren einer neuen Active Directory-Gesamtstruktur auf einem virtuellen Azure-Netzwerk](active-directory-new-forest-virtual-machine.md).
* Weitere Anleitungen zu den Konzepten für die Installation von Active Directory-Domänendiensten (AD DS) auf einem virtuellen Azure-Netzwerk finden Sie unter [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Computern in Microsoft Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Szenariodiagramm
In diesem Szenario müssen externe Benutzer auf Anwendungen zugreifen, die auf in die Domäne eingebundenen Servern ausgeführt werden. Die virtuellen Computer, auf denen die Anwendungsserver und die Replikatdomänencontroller ausgeführt werden, sind in einem virtuellen Azure-Netzwerk installiert. Das virtuelle Netzwerk kann über eine [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-site-to-site-create.md), wie in der folgenden Abbildung dargestellt, mit dem lokalen Netzwerk verbunden werden, oder Sie können als schnellere Verbindung [ExpressRoute](../expressroute/expressroute-locations-providers.md) verwenden.

Die Anwendungsserver und Domänencontroller werden in separaten Clouddiensten bereitgestellt, um die Computeverarbeitung zu verteilen, sowie in [Verfügbarkeitsgruppen](../virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), um eine bessere Fehlertoleranz zu erzielen.
Die Domänencontroller werden durch die Active Directory-Replikation untereinander und mit lokalen Domänencontroller repliziert. Es sind keine Synchronisierungstools erforderlich.

![Diagramm eines Active Directory-Replikatdomänencontrollers in einem virtuellen Azure-Netzwerk][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Erstellen eines Active Directory-Standorts für das virtuelle Netzwerk in Azure
Es bietet sich an, einen Standort in Active Directory zu erstellen, der die Netzwerkregion entsprechend dem virtuellen Netzwerk darstellt. Dadurch werden die Authentifizierung, Replikation sowie andere Vorgänge am Standort des Domänencontrollers optimiert. In den folgenden Schritten wird erläutert, wie ein Standort erstellt wird. Weitere Hintergrundinformationen erhalten Sie unter [Hinzufügen eines neuen Standorts](https://technet.microsoft.com/library/cc781496.aspx).

1. Öffnen Sie „Active Directory-Standorte und -Dienste“: **Server Manager** > **Tools** > **Active Directory-Standorte und -Dienste**.
2. Erstellen Sie einen Standort, der die Region darstellt, in der Sie ein virtuelles Azure-Netzwerk erstellt haben: Klicken Sie auf **Standorte** > **Aktion** > **Neuer Standort** > geben Sie den Namen des neuen Standorts ein, z.B. Azure USA, Westen > wählen Sie eine Standortverknüpfung aus > **OK**.
3. Erstellen Sie ein Subnetz, und ordnen Sie es dem neuen Standort zu: Doppelklicken Sie auf **Standorte** > klicken Sie mit der rechten Maustaste auf **Subnetze** > **Neues Subnetz** > geben Sie den IP-Adressbereich des virtuellen Netzwerks ein (z.B. 10.1.0.0/16 im Szenariodiagramm) > wählen Sie den neuen Azure-Standort aus > **OK**.

## <a name="create-an-azure-virtual-network"></a>Erstellen eines virtuellen Azure-Netzwerks
1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Neu** > **Netzwerkdienste** > **Virtuelles Netzwerk** > **Benutzerdefiniert erstellen**, und verwenden Sie die folgenden Werte, um den Assistenten abzuschließen.

   | Seite des Assistenten… | Einzugebende Werte |
   | --- | --- |
   |  **Details zum virtuellen Netzwerk** |<p>Name: Geben Sie einen Namen für das virtuelle Netzwerk ein, z.B. WestUSVNet.</p><p>Region: Wählen Sie die nächstgelegene Region aus.</p> |
   |  **DNS-Server und VPN-Konnektivität** |<p>DNS-Server: Geben Sie den Namen und die IP-Adresse von mindestens einem lokalen DNS-Server an.</p><p>Konnektivität: Wählen Sie **Ein Site-to-Site-VPN konfigurieren** aus.</p><p>Lokales Netzwerk: Geben Sie ein neues lokales Netzwerk an.</p><p>Wenn Sie ExpressRoute anstelle eines VPN verwenden, finden Sie weitere Informationen unter [Konfigurieren einer ExpressRoute-Verbindung über einen Exchange-Anbieter](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Site-to-Site-Konnektivität** |<p>Name: Geben Sie einen Namen für das lokale Netzwerk an.</p><p>IP-Adresse des VPN-Geräts: Geben Sie die öffentliche IP-Adresse des Geräts an, das mit dem virtuellen Netzwerk verbunden wird. Das VPN-Gerät kann sich nicht hinter einer NAT befinden.</p><p>Adresse: Geben Sie die Adressbereiche für das lokale Netzwerk an (z.B. 192.168.0.0/16 im Szenariodiagramm).</p> |
   |  **Adressräume des virtuellen Netzwerks** |<p>Adressraum: Geben Sie den IP-Adressbereich für virtuelle Computer an, die im virtuellen Azure-Netzwerk ausgeführt werden sollen (z.B. 10.1.0.0/16 im Szenariodiagramm). Dieser Adressbereich darf nicht mit den Adressbereichen des lokalen Netzwerks überlappen.</p><p>Subnetze: Geben Sie einen Namen und eine Adresse für ein Subnetz für die Anwendungsserver (z.B. Front-End, 10.1.1.0/24) und für die Domänencontroller an (z.B. Back-End, 10.1.2.0/24).</p><p>Klicken Sie auf **Gatewaysubnetz hinzufügen**.</p> |
2. Als Nächstes konfigurieren Sie das Gateway des virtuellen Netzwerks, um eine sichere Site-to-Site-VPN-Verbindung zu erstellen. Anweisungen hierzu finden Sie unter [Konfigurieren des Gateways für das virtuelle Netzwerk](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem neuen virtuellen Netzwerk und einem lokalen VPN-Gerät. Anweisungen hierzu finden Sie unter [Konfigurieren des Gateways für das virtuelle Netzwerk](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .

## <a name="create-azure-vms-for-the-dc-roles"></a>Erstellen von virtuellen Azure-Computern für die DC-Rollen
Wiederholen Sie die folgenden Schritte, um virtuelle Computer zum Hosten der DC-Rolle nach Bedarf zu erstellen. Sie sollten mindestens zwei virtuelle DC bereitstellen, um Fehlertoleranz und Redundanz zu gewährleisten. Wenn das virtuelle Azure-Netzwerk über mindestens zwei Domänencontroller verfügt, die auf ähnliche Weise konfiguriert sind (d. h. beide sind GCs, führen DNS-Server aus und beide führen keine FSMO-Rolle aus usw.), stellen Sie die virtuellen Computer, auf denen diese DCs ausgeführt werden, in eine Verfügbarkeitsgruppe, um eine verbesserte Fehlertoleranz zu erreichen.
Um die virtuellen Computer mithilfe von Windows PowerShell anstelle der Benutzeroberfläche zu erstellen, lesen Sie [Verwenden von Azure PowerShell zum Erstellen und Vorabkonfigurieren Windows-basierter virtueller Computer](../virtual-machines/virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Neu** > **Compute** > **Virtueller Computer** > **Aus Katalog**. Verwenden Sie die folgenden Werte, um den Assistenten abzuschließen. Übernehmen Sie den Standardwert für eine Einstellung, sofern kein anderer Wert empfohlen wird oder erforderlich ist.

   | Seite des Assistenten… | Einzugebende Werte |
   | --- | --- |
   |  **Image auswählen** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguration des virtuellen Computers** |<p>Name des virtuellen Computers: Geben Sie einen Namen mit einer einzelnen Bezeichnung ein (z. B. AzureDC1).</p><p>Neuer Benutzername: Geben Sie den Namen eines Benutzers ein. Dieser Benutzer wird Mitglied der lokalen Administratorgruppe auf dem virtuellen Computer. Sie benötigen diesen Namen, um sich zum ersten Mal bei dem virtuellen Computer anzumelden. Das integrierte Konto mit dem Namen "Administrator" funktioniert nicht.</p><p>Neues Kennwort/Bestätigen: Geben Sie ein Kennwort ein.</p> |
   |  **Konfiguration des virtuellen Computers** |<p>Clouddienst: Wählen Sie für den ersten virtuellen Computer <b>Einen neuen Cloud-Dienst erstellen</b> aus, und wählen Sie denselben Cloud-Dienstnamen aus, wenn Sie weitere virtuelle Computer erstellen, die die DC-Rolle hosten.</p><p>DNS-Name des Clouddiensts: Geben Sie einen global eindeutigen Namen ein.</p><p>Region/Affinitätsgruppe/Virtuelles Netzwerk: Geben Sie den Namen des virtuellen Netzwerks an (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie für den ersten virtuellen Computer <b>Ein automatisch generiertes Speicherkonto verwenden</b> aus, und wählen Sie dann denselben Speicherkontonamen aus, wenn Sie weitere virtuelle Computer erstellen, die die DC-Rolle hosten.</p><p>Verfügbarkeitsgruppe: Wählen Sie <b>Verfügbarkeitsgruppe erstellen</b> aus.</p><p>Name der Verfügbarkeitsgruppe: Geben Sie einen Namen für die Verfügbarkeitsgruppe ein, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann denselben Namen aus, wenn Sie weitere virtuelle Computer erstellen.</p> |
   |  **Konfiguration des virtuellen Computers** |<p>Wählen Sie <b>VM-Agent installieren</b> und alle anderen erforderlichen Erweiterungen aus.</p> |
2. Fügen Sie an jeden virtuellen Computer, der die DC-Serverrolle ausführt, einen Datenträger an. Der zusätzliche Speicherplatz ist erforderlich, um die AD-Datenbank, Protokolle und SYSVOL zu speichern. Geben Sie eine Größe für den Datenträger (z. B. 10 GB) an, und belassen Sie für **Hostcacheeinstellungen** die Einstellung **Keine**. Weitere Informationen finden Sie unter [Anfügen eines Datenträgers an einen virtuellen Windows-Computer](../virtual-machines/virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Nachdem Sie sich erstmals auf dem virtuellen Computer angemeldet haben, öffnen Sie **Server-Manager** > **Datei- und Speicherdienste**, um auf diesem Datenträger ein Volume mit NTFS zu erstellen.
4. Reservieren Sie eine statische IP-Adresse für virtuelle Computer, auf denen die DC-Rolle ausgeführt wird. Laden Sie zum Reservieren einer statischen IP-Adresse den Microsoft-Webplattform-Installer herunter, [installieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs) , und führen Sie anschließend das Cmdlet "Set-AzureStaticVNetIP" aus. Beispiel:

    'Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM

Weitere Informationen zum Festlegen einer statischen IP-Adresse finden Sie unter [Konfigurieren einer statischen internen IP-Adresse für einen virtuellen Computer](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installieren von AD DS auf virtuellen Azure-Computern
Melden Sie sich bei einem virtuellen Computer an, und stellen Sie sicher, dass Sie über die Site-to-Site-VPN-Verbindung bzw. die ExpressRoute-Verbindung mit Ressourcen im lokalen Netzwerk verbunden sind. Installieren Sie dann AD DS auf den virtuellen Azure-Computern. Sie können denselben Prozess wie bei der Installation eines zusätzlichen Domänencontrollers im lokalen Netzwerk verwenden (UI, Windows PowerShell oder eine Antwortdatei). Stellen Sie beim Installieren von AD DS sicher, dass Sie das neue Volume für den Standort der AD-Datenbank, der Protokolle und von SYSVOL angeben. Wenn Sie Ihre Kenntnisse über die AD DS-Installation auffrischen möchten, finden Sie unter [Installieren von Active Directory Domain Services (Stufe 100)](https://technet.microsoft.com/library/hh472162.aspx) oder [Installieren eines Windows Server 2012-Domänencontrollerreplikats in einer vorhandenen Domäne (Stufe 200)](https://technet.microsoft.com/library/jj574134.aspx) weitere Informationen.

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Erneutes Konfigurieren des DNS-Servers für das virtuelle Netzwerk
1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf den Namen des virtuellen Netzwerks und anschließend auf die Registerkarte **Konfigurieren**, um die [IP-Adressen der DNS-Server für das virtuelle Netzwerk neu zu konfigurieren](../virtual-network/virtual-networks-manage-dns-in-vnet.md). Auf diese Weise werden die den Replikatdomänencontrollern zugewiesenen statischen IP-Adressen anstelle der IP-Adressen eines lokalen DNS-Servers verwendet.
2. Um sicherzustellen, dass alle virtuellen Computer des Replikatdomänencontroller im virtuellen Netzwerk für die Verwendung der DNS-Server im virtuellen Netzwerk konfiguriert sind, klicken Sie auf **Virtuelle Computer**, auf die Statusspalte für die einzelnen virtuellen Computer und dann auf **Neustart**. Warten Sie, bis der virtuelle Computer den Status **Wird ausgeführt** anzeigt, bevor Sie versuchen, sich anzumelden.

## <a name="create-vms-for-application-servers"></a>Erstellen von virtuellen Computern für Anwendungsserver
1. Wiederholen Sie die folgenden Schritte, um virtuelle Computer zu erstellen, die als Anwendungsserver ausgeführt werden. Übernehmen Sie den Standardwert für eine Einstellung, sofern kein anderer Wert empfohlen wird oder erforderlich ist.

   | Seite des Assistenten… | Einzugebende Werte |
   | --- | --- |
   |  **Image auswählen** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguration des virtuellen Computers** |<p>Name des virtuellen Computers: Geben Sie einen Namen mit einer einzelnen Bezeichnung ein (z. B. AppServer1).</p><p>Neuer Benutzername: Geben Sie den Namen eines Benutzers ein. Dieser Benutzer wird Mitglied der lokalen Administratorgruppe auf dem virtuellen Computer. Sie benötigen diesen Namen, um sich zum ersten Mal bei dem virtuellen Computer anzumelden. Das integrierte Konto mit dem Namen "Administrator" funktioniert nicht.</p><p>Neues Kennwort/Bestätigen: Geben Sie ein Kennwort ein.</p> |
   |  **Konfiguration des virtuellen Computers** |<p>Clouddienst: Wählen Sie für den ersten virtuellen Computer **Einen neuen Cloud-Dienst erstellen** aus, und wählen Sie denselben Cloud-Dienstnamen aus, wenn Sie weitere virtuelle Computer erstellen, die die Anwendung hosten.</p><p>DNS-Name des Clouddiensts: Geben Sie einen global eindeutigen Namen ein.</p><p>Region/Affinitätsgruppe/Virtuelles Netzwerk: Geben Sie den Namen des virtuellen Netzwerks an (z. B. WestUSVNet).</p><p>Speicherkonto: Wählen Sie für den ersten virtuellen Computer **Ein automatisch generiertes Speicherkonto verwenden** aus, und wählen Sie dann denselben Speicherkontonamen aus, wenn Sie weitere virtuelle Computer erstellen, die die Anwendung hosten.</p><p>Verfügbarkeitsgruppe: Wählen Sie **Verfügbarkeitsgruppe erstellen** aus.</p><p>Name der Verfügbarkeitsgruppe: Geben Sie einen Namen für die Verfügbarkeitsgruppe ein, wenn Sie den ersten virtuellen Computer erstellen, und wählen Sie dann denselben Namen aus, wenn Sie weitere virtuelle Computer erstellen.</p> |
   |  **Konfiguration des virtuellen Computers** |<p>Wählen Sie <b>VM-Agent installieren</b> und alle anderen erforderlichen Erweiterungen aus.</p> |
2. Nachdem alle virtuellen Computer bereitgestellt wurden, melden Sie sich an, und stellen eine Verbindung zur Domäne her. Klicken Sie im **Server-Manager** auf **Lokaler Server** > **ARBEITSGRUPPE** > **Ändern…**, wählen Sie dann **Domäne**aus, und geben Sie den Namen der lokalen Domäne ein. Geben Sie die Anmeldeinformationen eines Domänenbenutzers ein, und starten Sie den virtuellen Computer neu, um das Beitreten zu der Domäne abzuschließen.

Um die virtuellen Computer mithilfe von Windows PowerShell anstelle der Benutzeroberfläche zu erstellen, lesen Sie [Verwenden von Azure PowerShell zum Erstellen und Vorabkonfigurieren Windows-basierter virtueller Computer](../virtual-machines/virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Weitere Informationen zum Verwenden von Windows PowerShell finden Sie unter [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx) und [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Hochladen vorhandener lokaler Hyper-V-Domänencontroller in Azure mithilfe von Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Installieren einer neuen Active Directory-Gesamtstruktur auf einem virtuellen Azure-Netzwerk](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IT Pro IaaS: (01) Grundlagen zu virtuellen Computern](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) Erstellen virtueller Netzwerke und Herstellen standortübergreifender Verbindungen](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
* [Azure-Verwaltungs-Cmdlets](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png



<!--HONumber=Dec16_HO4-->


