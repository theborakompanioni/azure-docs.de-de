<properties
	pageTitle="Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers | Microsoft Azure"
	description="Listet die verschiedenen Möglichkeiten zum Erstellen eines virtuellen Linux-Computers in Azure auf und enthält Links zu weiteren Anweisungen."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure-services"
	ms.date="04/12/2016"
	ms.author="iainfou"/>

# Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers mit Resource Manager

Virtuelle Computer können mit Azure und dem Resource Manager-Bereitstellungsmodell auf unterschiedliche Arten erstellt werden, um den Anforderungen unterschiedlicher Benutzer und Szenarien gerecht zu werden. Dieser Artikel fasst die Unterschiede und die Entscheidungen zusammen, die Sie beim Erstellen Ihres virtuellen Linux-Computers treffen können.

## Tool-Optionen

### Befehlsshell: Azure-Befehlszeilenschnittstelle 

Verwenden Sie an der CLI die Azure-Befehlszeilenschnittstelle. Weitere Informationen zum Installieren der Azure-Befehlszeilenschnittstelle mittels npm, Docker-Container oder Installationsskript finden Sie [hier](../xplat-cli-install.md). In den folgenden Tutorials wird die Verwendung der Azure-Befehlszeilenschnittstelle anhand von Beispielen veranschaulicht:

* [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md) 

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)

### GUI: Azure-Portal

Die grafische Benutzeroberfläche des [Azure-Portals](https://portal.azure.com) bietet eine einfache Möglichkeit zum Testen eines virtuellen Computers. Dies gilt insbesondere dann, wenn Sie noch nicht viel Erfahrung mit Azure haben, da hierzu keine Installation auf Ihrem System erforderlich ist. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen eines virtuellen Linux-Computers im Azure-Portal](virtual-machines-linux-portal-create.md) 

## Betriebssystem und Image-Optionen

In beiden Fällen muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch ein eigenes Image hochladen.

### Azure-Images

In allen obigen Artikeln können Sie einfach ein vorhandenes Azure-Image verwenden, um einen virtuellen Computer zu erstellen und ihn für die Vernetzung, den Lastenausgleich oder andere Anforderungen anzupassen. Über das Portal ist der Azure Marketplace für unter Azure bereitgestellte Images verfügbar. Eine ähnliche Liste erhalten Sie unter Verwendung der Befehlszeile. Führen Sie über die Azure-Befehlszeilenschnittstelle beispielsweise den Befehl `azure vm image list` aus, um eine nach Ort und Herausgeber sortierte Liste aller verfügbaren Images zu erhalten. Beispiele für das Navigieren zu und Verwenden von verfügbaren Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).

### Verwenden eines eigenen Image

Sollten Sie spezielle Anpassungen benötigen, verwenden Sie ein Image, das auf einem vorhandenen virtuellen Azure-Computer basiert, indem Sie diesen virtuellen Computer *erfassen* oder ein eigenes, auf einer virtuellen Festplatte (Virtual Hard Disk, VHD) gespeichertes Image hochladen. Weitere Informationen zu unterstützten Distributionen und zur Verwendung eigener Images finden Sie in den folgenden Artikeln:

* [Von Azure unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)

* [Informationen zu nicht unterstützten Distributionen](virtual-machines-linux-create-upload-generic.md)

* [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage](virtual-machines-linux-capture-image.md)

## Nächste Schritte

* Probieren Sie eines der Tutorials aus, um einen virtuellen Linux-Computer über das [Portal](virtual-machines-linux-portal-create.md), mit der [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md) oder mit einer Azure Resource Manager-[Vorlage](virtual-machines-linux-cli-deploy-templates.md) zu erstellen.

* Nach dem Erstellen eines virtuellen Linux-Computers können Sie problemlos [einen Datenträger hinzufügen](virtual-machines-linux-add-disk.md).

* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](virtual-machines-linux-using-vmaccess-extension.md).

<!---HONumber=AcomDC_0601_2016-->