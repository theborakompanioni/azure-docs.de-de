---
title: "Problembehandlung bei RemoteApp-Cloudsammlungen – Erstellen | Microsoft Docs"
description: Erfahren Sie, wie Sie Probleme beim Erstellen von RemoteApp-Cloud-Sammlungen beheben.
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: e4d94d3f9736378d93e93be6645ed04ade763ca3
ms.openlocfilehash: 022a910e5acfe12c03348df4476cc17f13c5c1d3


---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Problembehandlung bei der Erstellung von RemoteApp-Cloud-Sammlungen
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie beim Erstellen einer Cloudsammlung Probleme haben, überprüfen Sie die folgenden Informationen.

## <a name="your-image-is-invalid"></a>Ihr Image ist ungültig.
Wenn eine Meldung wie „GoldImageInvalid“ angezeigt wird, während Sie darauf warten, dass Azure Ihre Sammlung bereitstellt, bedeutet dies, dass Ihr Vorlagenimage nicht die [definierten Anforderungen für Images](remoteapp-imagereqs.md)erfüllt. Gehen Sie deshalb diese [Anforderungen](remoteapp-imagereqs.md)durch, korrigieren Sie Ihr Image, und versuchen Sie erneut, Ihre Sammlung zu erstellen.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Häufig auftretende Fehler im Azure-Verwaltungsportal
    DNS server could not be reached
    ProvisioningTimeout

Cloud-Sammlungen können oftmals nicht erstellt werden, da Sie benutzerdefinierte Images verwenden.  Wenn die oben genannten Fehler auftreten und Sie benutzerdefinierte Images für das Erstellen der Sammlung verwenden, prüfen Sie Folgendes:

* Vergewissern Sie sich, dass das von Ihnen hochgeladene benutzerdefinierte Image den Anforderungen entspricht.
* Am häufigsten tritt das Problem auf, das ein Image nicht richtig mit Sysprep vorbereitet wurde.  
* Überprüfen Sie, ob das Image in Hyper-V gestartet werden kann, oder versuchen Sie, eine IAAS-VM mit diesem Image direkt in Ihrem Azure-Abonnement zu erstellen. Wenn die VM nicht gestartet werden kann, deutet dies meist darauf hin, dass das benutzerdefinierte Image nicht richtig vorbereitet wurde.  Überprüfen Sie, ob das benutzerdefinierte Image gemäß den Anweisungen in "Erstellen eines benutzerdefinierten Vorlagenimages für RemoteApp" erstellt wurde.

Wenn Sie eines der Microsoft-Images verwenden, die in Ihrem Abonnement enthalten sind, wiederholen Sie das Erstellen der Sammlung. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.

    PlatformImageTrialModeOnly

Wenn dieser Fehler angezeigt wird, deutet dies i. d. R. darauf hin, dass ein Upgrade Ihres Konto auf ein Bezahlkonto durchgeführt wurde, Sie aber versuchen, ein von Microsoft bereitgestelltes Image zu verwenden, das nur während der Testphase des Diensts gültig ist. Versuchen Sie in diesem Fall, Ihre Cloud-Sammlung erneut zu erstellen, und geben Sie dabei das richtige Image an.




<!--HONumber=Feb17_HO3-->


