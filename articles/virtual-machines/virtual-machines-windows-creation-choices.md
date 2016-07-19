<properties
	pageTitle="Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers | Microsoft Azure"
	description="Auflistung der verschiedenen Möglichkeiten zum Erstellen eines virtuellen Windows-Computers mit Ressourcen-Manager"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows"
	ms.workload="infrastructure-services"
	ms.date="07/06/2016"
	ms.author="cynthn"/>

# Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers mit Ressourcen-Manager

Azure bietet verschiedene Möglichkeiten zum Erstellen eines virtuellen Computers, da virtuelle Computer für verschiedene Benutzer und Zwecke geeignet sind. Daher müssen Sie einige Optionen für den virtuellen Computer und dessen Erstellung auswählen. Dieser Artikel bietet einen Überblick über diese Optionen sowie Links zu Anweisungen.

## Azure-Portal

Über das Azure-Portal lassen sich virtuelle Computer sehr einfach testen. Dies gilt insbesondere dann, wenn Sie noch nicht viel Erfahrung mit Azure haben.

[Create a virtual machine running Windows using the portal (Erstellen eines virtuellen Windows-Computers mithilfe des Portals)](virtual-machines-windows-hero-tutorial.md)

## Vorlage

Virtuelle Computer benötigen verschiedene Ressourcen (wie etwa Verfügbarkeitsgruppen und Speicherkonten). Anstatt jede Ressource gesondert bereitzustellen und zu verwalten, können Sie eine Azure Resource Manager-Vorlage erstellen, die für die Bereitstellung aller Ressourcen in einem einzigen koordinierten Vorgang sorgt.

- [Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md)


## Azure PowerShell

Falls Sie lieber in einer Befehlsshell arbeiten, können Sie Azure PowerShell verwenden.

- [Create a Windows VM using PowerShell (Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell)](virtual-machines-windows-ps-create.md)


## Visual Studio

Verwenden Sie Visual Studio, um virtuelle Computer mit den Azure-Tools für Visual Studio und dem Azure SDK zu erstellen, zu verwalten und bereitzustellen.

[Azure-Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

<!---HONumber=AcomDC_0706_2016-->