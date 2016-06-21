<properties
   pageTitle="Erstellen einer Arbeits- oder Schulidentität in AAD | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine Arbeits- oder Schulidentität in Azure Active Directory zur Verwendung mit Ihren virtuellen Windows-Computern erstellen."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="06/06/2016"
   ms.author="rasquill"/>

# Erstellen einer Arbeits- oder Schulidentität in Azure Active Directory zur Verwendung mit Windows-VMs

Wenn Sie ein persönliches Azure-Konto erstellt oder ein persönliches MSDN-Abonnement besitzen und das Azure-Konto erstellt haben, um das MSDN Azure-Guthaben nutzen zu können, haben Sie zum Erstellen als Identität ein *Microsoft-Konto* verwendet. Viele hervorragende Funktionen von Azure – beispielsweise [Ressourcengruppenvorlagen](../resource-group-overview.md) – erfordern ein Arbeits- oder Schulkonto (eine Identität, die von Azure Active Directory verwaltet wird). Zum Erstellen eines neuen Geschäfts- oder Schulkontos können Sie die Anweisungen unten befolgen, da einer der größten Vorteile Ihres persönlichen Azure-Kontos glücklicherweise darin liegt, dass eine Azure Active Directory-Standarddomäne enthalten ist, mit der Sie ein neues Arbeits- oder Schulkonto für die Azure-Funktionen, die dies erfordern, erstellen können.

Aufgrund aktueller Änderungen können Sie Ihr Abonnement jedoch mit jeder Art von Azure-Konto verwalten, wenn Sie die [hier](../xplat-cli-connect.md) beschriebene interaktive Anmeldemethode über den Befehl `azure login` anwenden. Sie können diese Methode anwenden oder die nachfolgenden Anweisungen befolgen. Sie können auch [eine Arbeits- oder Schulidentität in Azure Active Directory zur Verwendung mit Linux-VMs erstellen](virtual-machines-linux-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]

<!---HONumber=AcomDC_0608_2016-->