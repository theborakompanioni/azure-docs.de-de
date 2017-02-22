---
title: "Erstellen einer Geschäfts-, Uni- oder Schulidentität in AAD für Linux | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Arbeits- oder Schulidentität in Azure Active Directory zur Verwendung mit Ihren virtuellen Linux-Computern erstellen."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: b0f86d77-c669-4aa1-a095-c2aa4d9105fe
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 1d35114800ba024d0c201840722df2c6a74d1d61
ms.openlocfilehash: 0eaa83fc19316708685eed0ca994112bc09153e5


---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Erstellen von Arbeits- oder Schulidentitäten in Azure Active Directory für die Verwendung mit Linux-VMs
Wenn Sie ein persönliches Azure-Konto erstellt oder ein persönliches MSDN-Abonnement besitzen und das Azure-Konto erstellt haben, um das MSDN Azure-Guthaben nutzen zu können, haben Sie zum Erstellen als Identität ein *Microsoft-Konto* verwendet. Viele hervorragende Funktionen von Azure – beispielsweise [Ressourcengruppenvorlagen](../azure-resource-manager/resource-group-overview.md) – erfordern ein Geschäfts-, Schul- oder Unikonto (eine Identität, die von Azure Active Directory verwaltet wird). Zum Erstellen eines neuen Geschäfts- oder Schulkontos können Sie die Anweisungen unten befolgen, da einer der größten Vorteile Ihres persönlichen Azure-Kontos glücklicherweise darin liegt, dass eine Azure Active Directory-Standarddomäne enthalten ist, mit der Sie ein neues Arbeits- oder Schulkonto für die Azure-Funktionen, die dies erfordern, erstellen können.

Aufgrund aktueller Änderungen können Sie Ihr Abonnement jedoch mit jeder Art von Azure-Konto verwalten, wenn Sie die [hier](../xplat-cli-connect.md) beschriebene interaktive Anmeldemethode über den Befehl `azure login` anwenden. Sie können diese Methode anwenden oder die nachfolgenden Anweisungen befolgen. Sie können auch [eine Arbeits- oder Schulidentität in Azure Active Directory zur Verwendung mit Windows-VMs erstellen](virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]




<!--HONumber=Jan17_HO4-->


