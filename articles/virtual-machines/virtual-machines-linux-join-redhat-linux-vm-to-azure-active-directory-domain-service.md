---
title: "Verknüpfen einer RedHat Linux-VM mit einer über Azure Active Directory DS verwalteten Domäne | Microsoft Docs"
description: "So verknüpfen Sie eine vorhandene RedHat Enterprise Linux 7-VM mit einer über Azure Active Directory Domain Services verwalteten Domäne."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 4e764e0d115c787f78593116f29c89e7c1e0e527
ms.openlocfilehash: 0c596b630c44ff68a01c7dbd3173c192fafac125


---

# <a name="join-a-redhat-linux-vm-to-an-azure-active-directory-domain-service"></a>Verknüpfen einer RedHat Linux-VM mit einer über Azure Active Directory Domain Services verwalteten Domäne

Dieser Artikel zeigt, wie Sie eine Red Hat Enterprise Linux 7-VM (RHEL) mit einer von Azure Active Directory Domain Services (AADDS) verwalteten Domäne verknüpfen.  Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md)

- [einen Azure Active Directory Domain Services-DC](../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Schnellbefehle

Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen.

### <a name="switch-the-azure-cli-to-classic-deployment-mode"></a>Umstellen der Azure-CLI in den klassischen Bereitstellungsmodus

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>Suchen nach einer RHEL-Version und einem RHEL-Image

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Erstellen einer Redhat Linux-VM

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p P@ssw0rd! \
-z "Small" \
-e \
-l "West US"
```

### <a name="ssh-to-the-vm"></a>SSH-Verbindung mit der VM

```bash
ssh -i ~/.ssh/id_rsa ahmet@
```

### <a name="update-yum-packages"></a>Aktualisieren der YUM-Pakete

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Installieren der benötigten Pakete

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Nachdem die erforderlichen Pakete auf dem virtuellen Linux-Computer installiert sind, besteht die nächste Aufgabe darin, den virtuellen Computer in die verwaltete Domäne einzubinden.

### <a name="discover-the-aad-domain-services-managed-domain"></a>Ermitteln der durch AAD Domain Services verwalteten Domäne

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Initialisieren von Kerberos

Stellen Sie sicher, dass Sie einen Benutzer angeben, der zur Administratorengruppe für Azure AD-Domänencontroller gehört. Nur diese Benutzer können Computer in die verwaltete Domäne einbinden.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-the-machine-to-the-domain"></a>Verknüpfen des Computers mit der Domäne

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-the-machine-is-joined-to-the-domain"></a>Sicherstellen, dass der Computer mit der Domäne verknüpft ist

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Nächste Schritte

* [Red Hat-Updateinfrastruktur (RHUI) für virtuelle On-Demand-Red Hat Enterprise Linux-Computer in Azure](virtual-machines-linux-update-infrastructure-redhat?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager](virtual-machines-linux-key-vault-setup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bereitstellen und Verwalten von virtuellen Computern mithilfe von Azure Resource Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Nov16_HO5-->


