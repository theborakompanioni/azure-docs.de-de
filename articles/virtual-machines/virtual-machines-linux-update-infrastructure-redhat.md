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
ms.date: 12/01/2016
ms.author: borisb
translationtype: Human Translation
ms.sourcegitcommit: 7ad71094cafb3d401797e8a8b3dd48a9aeeded9e
ms.openlocfilehash: 34c4198c0b1b975caf2c198634bb18e0c4f2ab5b


---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat-Updateinfrastruktur (RHUI) für virtuelle On-Demand-Red Hat Enterprise Linux-Computer in Azure
Virtuelle Computer, die auf der Grundlage der über den Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte Red Hat-Updateinfrastruktur (RHUI) registriert.  Die On-Demand-RHEL-Instanzen haben Zugriff auf ein regionales yum-Repository und können inkrementelle Updates empfangen.

Die von RHUI verwaltete yum-Repositoryliste wird während der Bereitstellung in der RHEL-Instanz konfiguriert. Daher ist keine zusätzliche Konfiguration erforderlich. Führen Sie `yum update` einfach aus, wenn die RHEL-Instanz bereit ist, um die neuesten Updates zu erhalten.

> [!NOTE]
> Die Azure-RHUI wurde kürzlich (September 2016) aktualisiert und erfordert Änderungen an der Konfiguration Ihrer vorhandenen RHEL-Instanzen, um einen unterbrechungsfreien Zugriff auf die Azure-RHUI zu gewährleisten. Ausführliche Informationen finden Sie im Abschnitt „RHUI – Update der Azure-Infrastruktur“.
> 
> 

## <a name="rhui-azure-infrastructure-update"></a>RHUI – Update der Azure-Infrastruktur
Seit September 2016 verfügt Azure über eine neue Gruppe von RHUI-Servern (Red Hat-Updateinfrastruktur). Diese Server werden mit [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) bereitgestellt, damit jeder virtuelle Computer unabhängig von der Region einen einzelnen Endpunkt (rhui-1.micrsoft.com) verwenden kann. Darüber hinaus verwenden die Server ein SSL-Zertifikat, das mit einer bekannten Zertifizierungsstelle verkettet ist (Baltimore Root). Da eine automatische Durchführung dieses Updates für einige Kunden gefährlich wäre, die ACLs oder benutzerdefinierte Routingtabellen für die RHUI-Updateserver verwenden, handelt es sich bei diesem Update um ein zustimmungspflichtiges Update. Auf dieser Seite finden Sie die manuellen Schritte zur Einführung dieser neuen Server sowie ein vollständiges Skript für eine automatisierte Einführung (nach Überprüfung der einzelnen Schritte). Die neuen RHEL-PAYG-Images im Azure Marketplace (Versionen mit einem Datum ab September 2016) verweisen automatisch auf die neuen Azure-RHUI-Server und können ohne weitere Maßnahmen verwendet werden.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Einführungszeitplan für die neue Azure-RHUI
| Datum | Hinweis |
| --- | --- |
| 22. September 2016 |RHUI-Server und Installationsanweisungen werden bereitgestellt. Virtuelle Computer, die mit den neuen RHEL-PAYG-Marketplace-Images (ab September 2016) bereitgestellt werden, verwenden automatisch die neuen RHUI-Server, vorhandene virtuelle Computer müssen jedoch manuell hinzugefügt werden. |
| 1. November 2016 |Ältere RHEL-PAYG-Images für virtuelle Computer (bei denen noch die alten Azure-RHUI-Server verwendet werden), werden aus dem Azure Marketplace-Katalog entfernt. |
| 16. Januar 2017 |Die alten Azure-RHUI-Server werden außer Betrieb gesetzt. Aktualisieren Sie bis zu diesem Termin alle betroffenen virtuellen RHEL-PAYG-Computer, um weiterhin auf die Azure-RHUI zugreifen zu können. |

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Die IP-Adressen der neuen Server für die neue RHUI-Inhaltsübermittlung lauten wie folgt:
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

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Manuelle Aktualisierung zur Verwendung der neuen Azure-RHUI-Server
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

Herunterladen, Überprüfen und Installieren des Client-RMP-Pakets

Download für RHEL 6:

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Download für RHEL 7:

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
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

Vergewissern Sie sich nach Abschluss des Vorgangs, dass Sie über den virtuellen Computer auf die Azure-RHUI zugreifen können.

### <a name="all-in-one-script-for-automating-the-above-task"></a>All-in-One-Skript zum Automatisieren der oben beschriebenen Aufgabe
Mit dem folgenden Skript können Sie bei Bedarf die Aktualisierung der betroffenen virtuellen Computer auf die neuen Azure-RHUI-Server automatisieren.

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI – Übersicht
[Red Hat-Updateinfrastruktur](https://access.redhat.com/products/red-hat-update-infrastructure) bietet eine hoch skalierbare Lösung zum Verwalten von yum-Repositoryinhalten für Red Hat Enterprise Linux-Cloudinstanzen, die von Cloudanbietern mit Red Hat-Zertifizierung gehostet werden. RHUI basiert auf dem Pulp-Upstreamprojekt und ermöglicht Cloudanbietern, die in Red Hat gehosteten Repositoryinhalte lokal zu spiegeln, benutzerdefinierte Repositorys mit eigenen Inhalten zu erstellen und diese Repositorys über ein Inhaltsübermittlungssystem mit Lastenausgleich einer großen Gruppe von Endbenutzern zur Verfügung zu stellen.

## <a name="regions-where-rhui-is-available"></a>Regionen, in denen RHUI verfügbar ist
RHUI ist in allen Regionen verfügbar, in denen RHEL-On-Demand-Images verfügbar sind. Dazu zählen derzeit alle öffentlichen Regionen, die auf der Dashboardseite [Azure-Status](https://azure.microsoft.com/status/) angegeben sind, sowie die Regionen Azure US Government und Azure Deutschland. Bei virtuellen Computern, die auf der Grundlage von On-Demand-RHEL-Images bereitgestellt werden, ist der RHUI-Zugriff im Preis inbegriffen. Zusätzliche regionale/nationale Cloudverfügbarkeit wird im Zuge des Ausbaus der RHEL-On-Demand-Verfügbarkeit aktualisiert.

> [!NOTE]
> Der Zugriff auf die in Azure gehostete RHUI ist auf virtuelle Computer innerhalb der [IP-Bereiche des Microsoft Azure-Datencenters](https://www.microsoft.com/download/details.aspx?id=41653)beschränkt.
> 
> 

## <a name="get-updates-from-another-update-repository"></a>Abrufen von Updates aus einem anderen Updaterepository
Wenn Sie Updates nicht aus der in Azure gehosteten RHUI, sondern aus einem anderen Updaterepository abrufen müssen, müssen Sie die RHUI-Registrierung Ihrer Instanzen aufheben und sie stattdessen bei der gewünschten Updateinfrastruktur registrieren (beispielsweise bei Red Hat Satellite oder Red Hat Customer Portal CDN). Für diese Dienste und die Registrierung für [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)benötigen Sie geeignete Red Hat-Abonnements.

Gehen Sie wie folgt vor, um die Registrierung von RHUI aufzuheben und RHUI anschließend für Ihre Updateinfrastruktur zu registrieren:

1. Bearbeiten Sie „/etc/yum.repos.d/rh-cloud.repo“, und ändern Sie alle Vorkommen von `enabled=1` in `enabled=0`. Beispiel:
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. Bearbeiten Sie „/etc/yum/pluginconf.d/rhnplugin.conf“, und ändern Sie `enabled=0` in `enabled=1`.
3. Führen Sie anschließend die Registrierung bei der gewünschte Infrastruktur durch (beispielsweise beim Red Hat-Kundenportal). Informationen zum Registrieren eines Systems sowie zum Einrichten eines Abonnements für das System beim Red Hat-Kundenportal finden Sie im [Red Hat-Lösungshandbuch](https://access.redhat.com/solutions/253273).

> [!NOTE]
> Der Zugriff auf die in Azure-gehostete RHUI ist im Preis für das RHEL-Image mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) enthalten. Da der virtuelle Computer durch die Aufhebung der Registrierung eines virtuellen PAYG-RHEL-Computers bei der in Azure gehosteten RHUI nicht in einen virtuellen BYOL-Computer (Bring-Your-Own-License) konvertiert wird, fallen unter Umständen doppelte Gebühren an, wenn Sie den gleichen virtuellen Computer bei einer anderen Updatequelle registrieren. 
> 
> Wenn Sie anstelle der in Azure gehosteten RHUI durchweg eine andere Infrastruktur verwenden müssen, empfiehlt es sich unter Umständen, eigene (BYOL-)Images zu erstellen und bereitzustellen. Eine entsprechende Beschreibung finden Sie im Artikel [Vorbereiten eines auf Red Hat basierenden virtuellen Computers für Azure](virtual-machines-linux-redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie einen virtuellen Red Hat Enterprise Linux-Computer auf der Grundlage eines Azure Marketplace-Images mit nutzungsbasierter Bezahlung erstellen und die in Azure gehostete RHUI verwenden möchten, besuchen Sie den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). `yum update` kann in der RHEL-Instanz ohne zusätzliche Einrichtung verwendet werden.




<!--HONumber=Dec16_HO1-->


