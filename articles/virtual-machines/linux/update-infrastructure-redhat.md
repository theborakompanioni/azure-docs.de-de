---
title: Red Hat-Updateinfrastruktur (RHUI) | Microsoft Docs
description: "Hier finden Sie Informationen zur Red Hat-Updateinfrastruktur (RHUI) für On-Demand-Red Hat Enterprise Linux-Instanzen in Microsoft Azure."
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/10/2017
ms.author: borisb
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 43d9095bcbd490abc6d01c214a17b8586f92ed1e
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat-Updateinfrastruktur (RHUI) für virtuelle On-Demand-Red Hat Enterprise Linux-Computer in Azure
 Mit der [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) können Cloudanbieter (z.B. Azure) in Red Hat gehostete Repositoryinhalte spiegeln, benutzerdefinierte Repositorys mit Azure-spezifischem Inhalt erstellen und diese für Endbenutzer-VMs zur Verfügung stellen.

RHEL-Images (Red Hat Enterprise Linux) mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) sind bereits für den Zugriff auf die Azure-RHUI vorkonfiguriert. Daher ist keine zusätzliche Konfiguration erforderlich. Führen Sie `sudo yum update` einfach aus, wenn die RHEL-Instanz bereit ist, um die neuesten Updates zu erhalten. Dieser Dienst ist in den RHEL PAYG-Softwaregebühren inbegriffen.

## <a name="important-information-about-azure-rhui"></a>Wichtige Informationen zur Azure-RHUI

1. Die RHUI unterstützt derzeit nur die neueste Nebenversion in einer bestimmten RHEL-Familie (RHEL6 oder RHEL7). Wenn `sudo yum update` ausgeführt wird, wird für die mit der RHUI verbundene RHEL VM-Instanz ein Upgrade auf die neueste Nebenversion durchgeführt. 

    Wenn Sie beispielsweise eine VM aus dem RHEL 7.2 PAYG-Image bereitstellen und `sudo yum update` ausführen, erhalten Sie letztlich eine RHEL 7.4-VM (die neueste Nebenversion der RHEL7-Familie).

    Um dies zu verhindern, müssen Sie ein eigenes Image erstellen, wie im Artikel [Erstellen und Hochladen von Red Hat-basierten virtuellen Computern für Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) beschrieben wird. Anschließend müssen Sie ([direkt auf Red Hat-Servern für die Inhaltsübermittlung](https://access.redhat.com/solutions/253273) oder Red Hat Satellite-Servern) eine Verbindung mit einer anderen Updateinfrastruktur herstellen.

2. Der Zugriff auf die in Azure gehostete RHUI ist im Preis für das RHEL PAYG-Image enthalten. Durch das Aufheben der Registrierung eines virtuellen PAYG-RHEL-Computers bei der von Azure gehosteten RHUI wird der virtuelle Computer nicht in einen BYOL-Typ (Bring Your Own License) konvertiert. Wenn Sie die gleiche VM bei einer anderen Updatequelle registrieren, werden Ihnen möglicherweise _indirekt_ doppelte Gebühren berechnet: zum einen für die Azure-RHEL-Software und zum anderen für das/die zuvor erworbene(n) Red Hat-Abonnement(s). Wenn Sie anstelle der in Azure gehosteten RHUI durchweg eine andere Updateinfrastruktur verwenden müssen, empfiehlt es sich unter Umständen, eigene (BYOL-)Images zu erstellen und bereitzustellen. Eine entsprechende Beschreibung finden Sie unter [Erstellen und Hochladen eines auf Red Hat basierenden virtuellen Computers für Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

3. Zwei Klassen von RHEL PAYG-Images in Azure (RHEL for SAP HANA, RHEL for SAP Business Applications) sind mit dedizierten RHUI-Kanälen verbunden, die gemäß der SAP-Zertifizierung in der jeweiligen RHEL-Nebenversion verbleiben. 

4. Der Zugriff auf die in Azure gehostete RHUI ist auf virtuelle Computer innerhalb der [IP-Bereiche des Microsoft Azure-Datencenters](https://www.microsoft.com/download/details.aspx?id=41653)beschränkt. Wenn Sie für den gesamten VM-Datenverkehr über die lokale Netzwerkinfrastruktur die Proxyfunktion verwenden, müssen Sie möglicherweise benutzerdefinierte Routen für die RHEL PAYG-VMs einrichten, um auf die Azure-RHUI zuzugreifen.

### <a name="the-ips-for-the-rhui-content-delivery-servers"></a>IP-Adressen der Server für die RHUI-Inhaltsübermittlung
RHUI ist in allen Regionen verfügbar, in denen RHEL-On-Demand-Images verfügbar sind. Dazu zählen derzeit alle öffentlichen Regionen, die auf der Seite [Dashboard zum Azure-Status](https://azure.microsoft.com/status/) angegeben sind, sowie die Regionen „Azure US-Regierung“ und „Azure Deutschland“. 

Wenn Sie den Zugriff von virtuellen RHEL-PAYG-Computern per Netzwerkkonfiguration beschränken, stellen Sie sicher, dass die folgenden IP-Adressen zugelassen sind, damit `yum update` funktioniert – abhängig von der Umgebung, in der Sie sich befinden. 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="rhui-azure-infrastructure-update"></a>RHUI – Update der Azure-Infrastruktur

Im September 2016 haben wir eine aktualisierte Azure-RHUI bereitgestellt, und im April 2017 die veraltete Azure-RHUI heruntergefahren. Wenn Sie die RHEL PAYG-Images (oder die zugehörigen Momentaufnahmen) ab September 2016 verwendet haben, werden Sie automatisch mit der neuen Azure-RHUI verbunden. Wenn Sie jedoch über ältere Momentaufnahmen oder VMs verfügen, muss deren Konfiguration wie unten beschrieben manuell aktualisiert werden, um Zugriff auf die Azure-RHUI sicherzustellen.

Die neuen Azure-RHUI-Server werden mit dem [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) bereitgestellt, damit jede VM unabhängig von der Region einen einzelnen Endpunkt (rhui-1.microsoft.com) verwenden kann. 

### <a name="troubleshooting-connection-problems-to-azure-rhui"></a>Beheben von Problemen bei der Verbindung mit der Azure-RHUI
Wenn beim Herstellen einer Verbindung mit der Azure-RHUI von Ihrer Azure-RHEL-PAYG-VM Probleme auftreten, gehen Sie folgendermaßen vor.
1. Überprüfen Sie, welcher Azure-RHUI-Endpunkt für den virtuellen Computer konfiguriert ist.

    Überprüfen Sie, ob die Datei `/etc/yum.repos.d/rh-cloud.repo` in der Basis-URL im Abschnitt `[rhui-microsoft-azure-rhel*]` einen Verweis auf `rhui-[1-3].microsoft.com` enthält. Wenn dies der Fall ist, verwenden Sie die neue Azure-RHUI.

    Verweist die URL mit dem Muster `mirrorlist.*cds[1-4].cloudapp.net` auf einen Speicherort, muss die Konfiguration aktualisiert werden. Wenn Sie die alte VM-Momentaufnahme verwenden, ändern Sie sie dahingehend, dass sie auf die neue Azure-RHUI verweist.

2. Der Zugriff auf die in Azure gehostete RHUI ist auf VMs innerhalb der [IP-Bereiche des Microsoft Azure-Rechenzentrums] beschränkt (https://www.microsoft.com/download/details.aspx?id=41653).
 
3. Wenn Sie die neue Konfiguration verwenden, überprüfen Sie, ob die VM eine Verbindung über den IP-Bereich von Azure herstellt. Wenn nach wie vor keine Verbindung mit der Azure-RHUI hergestellt werden kann, senden Sie eine Supportanfrage an Microsoft oder Red Hat.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Manuelle Aktualisierung zur Verwendung der Azure-RHUI-Server
Dieses Verfahren dient lediglich zu Ihrer Information. RHEL PAYG-Images sind bereits mit der entsprechenden Konfiguration zum Herstellen einer Verbindung mit der Azure-RHUI ausgestattet.

Laden Sie mithilfe von „curl“ die Signatur des öffentlichen Schlüssels herunter:

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Überprüfen Sie den heruntergeladenen Schlüssel:

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Überprüfen Sie `keyid` und `user ID packet` in der Ausgabe:

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Installieren Sie den öffentlichen Schlüssel:

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Laden Sie das Client-RPM-Paket herunter. Überprüfen und installieren Sie es dann. 

> [!NOTE]
> Die Paketversionen ändern sich. Wenn Sie manuell eine Verbindung mit der Azure-RHUI herstellen, können Sie die neueste Version des Clientpakets für die einzelnen RHEL-Familien durch Bereitstellen des neuesten Images aus dem Katalog beziehen.
> 

Download für RHEL 6:

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.1-32.noarch.rpm 
```

Download für RHEL 7:

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.1-19.noarch.rpm  
```

Überprüfung:

```bash
rpm -Kv azureclient.rpm
```

Vergewissern Sie sich in der Ausgabe, dass die Signatur des Pakets in Ordnung (OK) ist.

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Installieren Sie das RPM-Paket:

```bash
sudo rpm -U azureclient.rpm
```

Vergewissern Sie sich nach Abschluss des Vorgangs, dass Sie über die VM auf die Azure-RHUI zugreifen können.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie einen virtuellen Red Hat Enterprise Linux-Computer auf der Grundlage eines Azure Marketplace-Images mit nutzungsbasierter Bezahlung erstellen und die in Azure gehostete RHUI verwenden möchten, besuchen Sie den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). 
