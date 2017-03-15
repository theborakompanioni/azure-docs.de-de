---
title: "Konfigurieren eines virtuellen Azure-Netzwerks (klassisch) – Netzwerkkonfigurationsdatei | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie virtuelle Netzwerke (klassisch) ändern, indem Sie eine Netzwerkkonfigurationsdatei über das Azure-Portal (klassisch) exportieren, bearbeiten und importieren."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: 3827dc0958c51fa0c4ecb1a2e8e3b7bbed42a75a
ms.lasthandoff: 02/28/2017


---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Konfigurieren eines virtuellen Netzwerks (klassisch) mithilfe einer Netzwerkkonfigurationsdatei
Sie können ein virtuelles Netzwerk (klassisch) über das Azure-Portal (klassisch) oder mithilfe einer Netzwerkkonfigurationsdatei konfigurieren. Sie können ein virtuelles Netzwerk nicht mithilfe einer Netzwerkkonfigurationsdatei im Azure Resource Manager-Bereitstellungsmodell erstellen oder ändern. Darüber hinaus können Sie ein virtuelles Netzwerk (klassisch) nicht über das Azure-Portal erstellen oder ändern.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Erstellen und Ändern einer Netzwerkkonfigurationsdatei
Die einfachste Möglichkeit zum Erstellen einer Netzwerkkonfigurationsdatei ist der Export der Netzwerkeinstellungen aus einer vorhandenen Konfiguration eines virtuellen Netzwerks (klassisch) und die anschließende Bearbeitung der Datei, damit sie die Einstellungen enthält, die Sie für Ihre virtuellen Netzwerke konfigurieren möchten.

Wenn Sie die Netzwerkkonfigurationsdatei bearbeiten möchten, können Sie sie einfach öffnen, die notwendigen Änderungen vornehmen und die Datei speichern. Sie können einen beliebigen *XML* -Editor verwenden, um Änderungen an der Netzwerkkonfigurationsdatei vorzunehmen. 

Befolgen Sie die Anleitungen zu [Schemaeinstellungen zur Netzwerkkonfigurationsdatei](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure betrachtet ein Subnetz, in dem etwas bereitgestellt wurde, als **in Gebrauch**. Wenn Sie ein Subnetz in Gebrauch ist, kann es nicht geändert werden. Verschieben Sie vor allen Änderungen alles, was Sie im Subnetz bereitgestellt haben, in ein anderes Subnetz, das nicht verändert wird.   Siehe [Verschieben eines virtuellen Computers oder einer Rolleninstanz in ein anderes Subnetz](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-azure-portal-classic"></a>Exportieren und Importieren von Einstellungen eines virtuellen Netzwerks über das Azure-Portal (klassisch)
Sie können Netzwerkkonfigurationseinstellungen, die in Ihrer Netzwerkkonfigurationsdatei enthalten sind, mithilfe von PowerShell oder des Verwaltungsportals importieren oder exportieren. Die folgenden Anweisungen helfen Ihnen beim Exportieren und Importieren mithilfe des Verwaltungsportals. 

### <a name="to-export-your-network-settings"></a>So exportieren Sie die Netzwerkeinstellungen
Beim Export werden alle Einstellungen für die virtuellen Netzwerke in Ihrem Abonnement in eine XML-Datei geschrieben. 

1. Melden Sie sich beim [Azure-Portal (klassisch)](https://manage.windowsazure.com/) an.
2. Klicken Sie im Portal am unteren Rand der Seite **Netzwerke** auf **Exportieren**. 
3. Überprüfen Sie im Fenster **Netzwerkkonfiguration exportieren** , ob Sie das Abonnement ausgewählt haben, für das Sie die Netzwerkeinstellungen exportieren möchten. Markieren Sie anschließend das Häkchen in der unteren rechten Ecke. 
4. Wenn Sie dazu aufgefordert werden, speichern Sie die Datei *NetworkConfig.xml* am gewünschten Speicherort.

### <a name="to-import-your-network-settings"></a>So importieren Sie die Netzwerkeinstellungen
1. Klicken Sie im Navigationsbereich des Portals unten links auf **Neu**.
2. Klicken Sie auf **Network Services** -> **Virtuelles Netzwerk** -> **Konfiguration importieren**.
3. Wechseln Sie auf der Seite **Die Netzwerkkonfigurationsdatei importieren** zu Ihrer Netzwerkkonfigurationsdatei, und klicken Sie dann auf den Pfeil für **Weiter**.
4. Auf der Seite **Netzwerk wird erstellt** wird auf dem Bildschirm angezeigt, welche Teile Ihrer Netzwerkkonfiguration geändert oder erstellt werden. Wenn Sie die Änderungen für richtig erachten, klicken Sie auf das Häkchen, um mit dem Aktualisieren oder Erstellen Ihres virtuellen Netzwerks fortzufahren. 


