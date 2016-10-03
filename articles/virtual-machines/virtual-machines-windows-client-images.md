<properties
   pageTitle="Verwenden von Windows-Clientimages für Entwicklungs-/Testszenarien | Microsoft Azure"
   description="Nutzen von Visual Studio-Abonnementvorteilen zum Bereitstellen von Windows 7/8/10 in Azure für Entwicklungs-/Testszenarien"
   services="virtual-machines-windowse"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# Verwendung des Windows-Clients in Azure für Entwicklungs-/Testszenarien

Sie können Windows 7, Windows 8 oder Windows 10 in Azure für Entwicklungs-/Testszenarien verwenden, sofern Sie über ein entsprechendes Visual Studio-Abonnement (früher MSDN) verfügen. Dieser Artikel beschreibt die erforderlichen Berechtigungen für die Ausführung des Windows-Clients in Azure und die Verwendung von Images aus dem Azure-Katalog.


## Abonnementberechtigung
Aktive Visual Studio-Abonnenten (Personen, die eine Visual Studio-Abonnementlizenz erworben haben) können den Windows-Client für Entwicklungs- und Testzwecke verwenden. Der Windows-Client kann auf Ihrer eigenen Hardware und virtuellen Azure-Computern, die in einem Azure-Abonnement beliebigen Typs ausgeführt werden, verwendet werden. Der Windows-Client darf nicht in Azure für die normale Produktion bereitgestellt oder verwendet werden, bzw. von Personen verwendet werden, die nicht aktive Abonnenten von Visual Studio sind.

Der Einfachheit halber haben wir Ihnen bestimmte Windows 10-Images aus dem Azure-Katalog in [Geeignete Angebote](#eligible-offers) zur Verfügung gestellt. Visual Studio-Abonnenten können auch in jedem Angebotstyp ein 64-Bit-Image für Windows 7, Windows 8 oder Windows 10 [angemessen vorbereiten und erstellen](virtual-machines-windows-prepare-for-upload-vhd-image.md) und dann [in Azure hochladen](virtual-machines-windows-upload-image.md). Die Verwendung bleibt auf Entwicklung/Test durch aktive Visual Studio-Abonnenten beschränkt.


## Geeignete Angebote
In der folgenden Tabelle werden die Angebots-IDs aufgeführt, die über den Azure-Katalog in Windows 10 bereitgestellt werden können. Die Windows 10-Images sind nur für die folgenden Angebote sichtbar. Für Visual Studio-Abonnenten, die den Windows-Client in einem anderen Angebotstyp ausführen müssen, müssen Sie ein 64-Bit-Image für Windows 7, Windows 8 oder Windows 10 [angemessen vorbereiten und erstellen](virtual-machines-windows-prepare-for-upload-vhd-image.md) und [dann in Azure hochladen](virtual-machines-windows-upload-image.md).

| Angebotsname | Angebotsnummer | Verfügbare Client-images |
|:-----------|:------------:|:-----------------------:|
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/) | 0023P | Windows 10 |
| [Visual Studio Enterprise-Abonnenten (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/) | 0029P | Windows 10 |
| [Visual Studio Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0059p/) | 0059P | Windows 10 |
| [Visual Studio Test Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0060p/) | 0060P | Windows 10 |
| [Visual Studio Premium mit MSDN (Vorteil)](https://azure.microsoft.com/offers/ms-azr-0061p/) | 0061P | Windows 10 |
| [Visual Studio Enterprise-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0063p/) | 0063P | Windows 10 |
| [Visual Studio Enterprise-Abonnenten (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Enterprise Dev/Test](https://azure.microsoft.com/ofers/ms-azr-0148p/) | 0148P | Windows 10 |


## Überprüfen Ihres Azure-Abonnements
Wenn Sie Ihre Angebots-ID nicht kennen, können Sie sie über das Azure-Portal oder das Kontoportal abrufen.

Die Abonnement-Angebots-ID finden Sie im Azure-Portal im Blatt „Abonnements“:

![Details der Angebots-ID aus dem Azure-Portal](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png)

Sie können die Angebots-ID auch auf der [Registerkarte „Abonnements“](http://account.windowsazure.com/Subscriptions) des Azure-Kontoportals anzeigen:

![Details der Angebots-ID aus dem Azure-Kontoportal](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png)


## Nächste Schritte
Jetzt können Sie Ihre VMs mit [PowerShell](virtual-machines-windows-ps-create.md), [Resource Manager-Vorlagen](virtual-machines-windows-ps-template.md) oder [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) bereitstellen.

<!---HONumber=AcomDC_0921_2016-->