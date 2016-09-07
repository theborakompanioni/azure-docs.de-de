<properties
   pageTitle="Navigieren zu und Auswählen von Windows-VM-Images | Microsoft Azure"
   description="Erfahren Sie, wie Sie den Herausgeber, das Angebot und die SKU für Images ermitteln, wenn Sie mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Windows-Computer erstellen."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell oder der Befehlszeilenschnittstelle

In diesem Thema wird beschrieben, wie Sie Herausgeber von VM-Images sowie entsprechende Angebote, SKUs und Versionen für jeden Ort finden, an dem Sie Bereitstellungen ins Auge fassen. Beispielsweise sind einige der häufig verwendeten Windows-VM-Images:

## Tabelle mit häufig verwendeten Windows-Images


| PublisherName | Angebot | Sku |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV | DynamicsNAV | 2015 |
| MicrosoftSharePoint | MicrosoftSharePointServer | 2013 |
| MicrosoftSQLServer | SQL 2014 WS2012R2 | Enterprise-Optimized-for-DW |
| MicrosoftSQLServer | SQL 2014 WS2012R2 | Enterprise-Optimized-for-OLTP |
| MicrosoftWindowsServer | Windows Server | 2012-R2-Datacenter |
| MicrosoftWindowsServer | Windows Server | 2012-Datacenter |
| MicrosoftWindowsServer | Windows Server | 2008-R2-SP1 |
| MicrosoftWindowsServer | Windows Server | Windows-Server-Technical-Preview |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials | WindowsServerEssentials |
| MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | 2012R2 |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]

<!---HONumber=AcomDC_0824_2016-->