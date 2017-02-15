---
title: "Ausführliche Schritte zur Behebung von Problemen mit SSH bei einem virtuellen Azure-Computer | Microsoft Docs"
description: "Weitere detaillierte Schritte zur Problembehandlung für SSH-Verbindungen mit einem virtuellen Azure-Computer"
keywords: SSH-Verbindung abgelehnt,SSH-Fehler,Azure SSH,SSH-Verbindungsfehler
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: support-article
ms.date: 09/01/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 9333062851e070a93afc6935e58594711b741f48


---
# <a name="detailed-ssh-troubleshooting-steps"></a>Ausführliche Schritte zur Problembehandlung bei SSH
Wenn der SSH-Client nicht in der Lage ist, den SSH-Dienst auf dem virtuellen Computer zu erreichen, können dafür viele verschiedene Gründe vorliegen.. Wenn Sie die [allgemeinen Schritte zur Problembehandlung bei SSH](virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) befolgt haben, müssen Sie die Ursache der Verbindungsprobleme genauer untersuchen. Dieser Artikel führt Sie durch die detaillierten Schritte zur Problembehandlung, mit denen Sie ermitteln können, wo bei der SSH-Verbindung ein Fehler auftritt und wie Sie diesen beheben.

## <a name="take-preliminary-steps"></a>Durchführen vorbereitender Schritte
Das folgende Diagramm zeigt die beteiligten Komponenten.

![Diagramm mit den Komponenten des SSH-Diensts](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Mit den folgenden Schritten können Sie die Fehlerquelle isolieren und Lösungen oder Problemumgehungen ermitteln.

Überprüfen Sie zunächst den Status des virtuellen Computers im Portal.

Im [Azure-Portal](https://portal.azure.com):

1. Wählen Sie für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, die Option **Durchsuchen** > **Virtuelle Computer (klassisch)** > *VM-Name* aus.
   
    ODER
   
    Wählen Sie für virtuelle Computer, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, die Option **Durchsuchen** > **Virtuelle Computer** > *VM-Name* aus.
   
    Im Statusbereich für den virtuellen Computer sollte **Wird ausgeführt**angezeigt werden. Führen Sie einen Bildlauf nach unten durch, um aktuelle Aktivitäten für Compute-, Speicher- und Netzwerkressourcen anzuzeigen.
2. Wählen Sie **Einstellungen** aus, um Endpunkte, IP-Adressen und andere Einstellungen zu überprüfen.
   
    Um Endpunkte auf virtuellen Computern zu identifizieren, die mit Resource Manager erstellt wurden, überprüfen Sie, ob eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) definiert wurde. Stellen Sie außerdem sicher, dass die Regeln auf die Netzwerksicherheitsgruppe angewendet wurden und dass im Subnetz darauf verwiesen wird.

Gehen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)folgendermaßen für virtuelle Computer vor, die mit dem klassischen Bereitstellungsmodell erstellt wurden:

1. Wählen Sie **Virtuelle Computer** > *VM-Name*aus.
2. Wählen Sie das **Dashboard** des virtuellen Computers aus, um den Status zu überprüfen.
3. Wählen Sie **Überwachen** aus, um aktuelle Aktivitäten für Compute-, Speicher- und Netzwerkressourcen anzuzeigen.
4. Wählen Sie **Endpunkte** aus, und vergewissern Sie sich, dass ein Endpunkt für SSH-Datenverkehr vorhanden ist.

Analysieren Sie zum Überprüfen der Netzwerkverbindung die konfigurierten Endpunkte, und testen Sie, ob der virtuelle Computer über ein anderes Protokoll (z. B. HTTP oder über einen anderen Dienst) erreichbar ist.

Versuchen Sie nach diesen Schritten erneut, die SSH-Verbindung herzustellen.

## <a name="find-the-source-of-the-issue"></a>Ermitteln der Ursache des Problems
Das Verbindungsproblem zwischen dem SSH-Client auf Ihrem Computer und dem SSH-Dienst auf dem virtuellen Azure-Computer ist u.U. auf die folgenden Probleme oder Konfigurationsfehler zurückzuführen:

* [SSH-Clientcomputer](#source-1-ssh-client-computer)
* [Edgegerät der Organisation](#source-2-organization-edge-device)
* [Clouddienst-Endpunkt und die Zugriffssteuerungsliste (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Netzwerksicherheitsgruppen](#source-4-network-security-groups)
* [Linux-basierter virtueller Azure-Computer](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Quelle 1: SSH-Clientcomputer
Überprüfen Sie, ob Ihr Computer SSH-Verbindungen mit einem anderen Linux-basierten lokalen Computer herstellen kann, um Ihren Computer als Ursache des Fehlers auszuschließen.

![Diagramm, in dem die SSH-Clientcomputerkomponenten hervorgehoben sind](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Falls ein Verbindungsfehler auftritt, überprüfen Sie Ihren Computer auf Folgendes:

* Eine lokale Firewall-Einstellung, die eingehenden oder ausgehenden SSH-Datenverkehr (TCP-22) blockiert
* Lokal installierte Proxy-Clientsoftware, die SSH-Verbindungen verhindert
* Lokal installierte Netzwerküberwachungssoftware, die SSH-Verbindungen verhindert
* Andere Arten von Sicherheitssoftware, die Datenverkehr überwachen oder bestimmte Arten von Datenverkehr zulassen/unterbinden

Wenn eine dieser Bedingungen zutrifft, deaktivieren Sie die Software vorübergehend, und versuchen Sie, eine SSH-Verbindung mit einem lokalen Computer herzustellen, um zu ermitteln, warum die Verbindung auf Ihrem Computer blockiert wird. Korrigieren Sie dann in Zusammenarbeit mit Ihrem Netzwerkadministrator die Einstellungen der Software, um SSH-Verbindungen zu ermöglichen.

Vergewissern Sie sich bei Verwendung der Zertifikatauthentifizierung, dass Sie für den SSH-Ordner in Ihrem Basisverzeichnis über folgende Berechtigungen verfügen:

* Chmod 700 ~/.ssh
* Chmod 644 ~/.ssh/\*.pub
* Chmod 600 ~/.ssh/id_rsa (bzw. beliebige andere Dateien, in denen Ihre privaten Schlüssel gespeichert sind)
* Chmod 644 ~/.ssh/known_hosts (enthält Hosts, mit denen Sie eine SSH-Verbindung hergestellt haben)

## <a name="source-2-organization-edge-device"></a>Quelle 2: Edgegerät der Organisation
Überprüfen Sie, ob ein direkt mit dem Internet verbundener Computer SSH-Verbindungen mit Ihrem virtuellen Azure-Computer herstellen kann, um das Edgegerät der Organisation als Fehlerquelle auszuschließen. Wenn der Zugriff auf den virtuellen Computer über ein Standort-zu-Standort-VPN oder über eine Azure ExpressRoute-Verbindung erfolgt, fahren Sie mit [Quelle 4: Netzwerksicherheitsgruppen](#nsg)fort.

![Diagramm, in dem das Edgegerät der Organisation hervorgehoben ist](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Wenn Sie keinen Computer haben, der direkt mit dem Internet verbunden ist, erstellen Sie einen neuen virtuellen Azure-Computer in einer eigenen Ressourcengruppe oder einem eigenen Clouddienst und verwenden ihn. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers unter Linux in Azure](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Löschen Sie die Ressourcengruppe oder den virtuellen Computer und den Clouddienst, wenn Sie Ihre Tests abgeschlossen haben.

Wenn Sie eine SSH-Verbindung mit einem direkt mit dem Internet verbundenen Computer herstellen können, überprüfen Sie das Edgegerät Ihrer Organisation auf Folgendes:

* Eine interne Firewall, die SSH-Verbindungen mit dem Internet blockiert
* Ein Proxy-Server, der SSH-Verbindungen verhindert
* Auf dem Edgegerät ausgeführte Software zur Netzwerküberwachung oder zur Erkennung von Eindringversuchen, die SSH-Verbindungen verhindert

Korrigieren Sie in Zusammenarbeit mit Ihrem Netzwerkadministrator die Einstellungen des Edgegeräts Ihrer Organisation, um SSH-Datenverkehr über das Internet zuzulassen.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Quelle 3: Clouddienst-Endpunkt und ACL
> [!NOTE]
> Diese Quelle gilt nur für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Für virtuelle Computer, die mit Resource Manager erstellt wurden, fahren Sie mit [Quelle 4: Netzwerksicherheitsgruppen](#nsg)fort.
> 
> 

Überprüfen Sie, ob ein anderer virtueller Azure-Computer im gleichen virtuellen Netzwerk eine SSH-Verbindung mit Ihrem virtuellen Computer herstellen kann, um den Clouddienst-Endpunkt und die ACL als Fehlerquelle auszuschließen.

![Diagramm, in dem der Clouddienstendpunkt und ACL hervorgehoben sind](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Ist kein anderer virtueller Computer im gleichen virtuellen Netzwerk vorhanden, können Sie problemlos einen neuen virtuellen Computer erstellen. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Löschen Sie den zusätzlichen virtuellen Computer, wenn Sie Ihre Tests abgeschlossen haben.

Wenn Sie eine SSH-Verbindung mit einem virtuellen Computer im gleichen virtuellen Netzwerk herstellen können, überprüfen Sie Folgendes:

* **Die Endpunktkonfiguration für den SSH-Datenverkehr auf dem virtuellen Zielcomputer.** Der private TCP-Port des Endpunkts sollte dem TCP-Port entsprechen, an dem der SSH-Dienst auf dem virtuellen Computer lauscht. (Der Standardport ist 22.) Überprüfen Sie bei virtuellen Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, die SSH-TCP-Portnummer im Azure-Portal, indem Sie **Durchsuchen** > **Virtuelle Computer (v2)** > *VM-Name* > **Einstellungen** > **Endpunkte** auswählen.
* **Die ACL für den SSH-Datenverkehrsendpunkt auf dem virtuellen Zielcomputer.**  In ACLs können Sie anhand der Quell-IP-Adresse angeben, ob eingehender Datenverkehr aus dem Internet zugelassen oder verweigert wird. Falsch konfigurierte ACLs können dazu führen, dass eingehender SSH-Datenverkehr den Endpunkt nicht erreicht. Überprüfen Sie die ACLs, um sicherzustellen, dass eingehender Verkehr von den öffentlichen IP-Adressen des Proxy- oder eines anderen Edgeservers zugelassen wird. Weitere Informationen finden Sie unter [Was ist eine Netzwerk-Zugriffssteuerungsliste (Access Control List, ACL)?](../virtual-network/virtual-networks-acl.md)

Entfernen Sie den aktuellen Endpunkt, und erstellen Sie einen neuen Endpunkt mit dem SSH-Namen und dem TCP-Port 22 als öffentliche und private Portnummer, um den Endpunkt als Problemquelle auszuschließen. Weitere Informationen finden Sie unter [Festlegen von Endpunkten auf einem virtuellen Computer in Azure](virtual-machines-windows-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Quelle 4: Netzwerksicherheitsgruppen
Netzwerksicherheitsgruppen ermöglichen Ihnen eine präzisere Steuerung des zulässigen eingehenden und ausgehenden Datenverkehrs. Sie können Regeln erstellen, die mehrere Subnetze und Clouddienste in einem virtuellen Azure-Netzwerk umfassen. Überprüfen Sie die Regeln der Netzwerksicherheitsgruppe, um sicherzustellen, dass sowohl eingehender als auch ausgehender SSH-Datenverkehr über das Internet zulässig ist.
Weitere Informationen finden Sie unter [Informationen zu Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Quelle 5: Linux-basierter virtueller Azure-Computer
Die letzte mögliche Problemquelle ist der virtuelle Azure-Computer selbst.

![Diagramm, in dem der Linux-basierte virtuelle Azure-Computer hervorgehoben ist](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Falls noch nicht erfolgt, führen Sie die unter [Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) beschriebenen Anweisungen aus.

Versuchen Sie erneut, die Verbindung von Ihrem Computer aus herzustellen. Wenn dies weiterhin nicht möglich ist, können u.a. die folgenden Probleme der Grund dafür sein:

* Der SSH-Dienst wird auf dem virtuellen Zielcomputer nicht ausgeführt.
* Der SSH-Dienst lauscht nicht am TCP-Port 22. Installieren Sie zu Testzwecken einen Telnet-Client auf Ihrem lokalen Computer, und führen Sie „telnet *Name des Clouddiensts*.cloudapp.net 22“ aus. So können Sie ermitteln, ob der virtuelle Computer die eingehende und ausgehende Kommunikation mit dem SSH-Endpunkt zulässt.
* Für die lokale Firewall auf dem virtuellen Zielcomputer sind Regeln definiert, die eingehenden oder ausgehenden SSH-Datenverkehr verhindern.
* Auf dem virtuellen Azure-Computer wird Software zur Netzwerküberwachung oder zur Erkennung von Eindringversuchen ausgeführt, die SSH-Verbindungen verhindert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Weitere Informationen zur Problembehandlung beim Anwendungszugriff finden Sie unter [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).




<!--HONumber=Nov16_HO3-->


