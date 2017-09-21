---
title: "Bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory"
description: "Bekannte Probleme mit der verwalteten Dienstidentität für Azure Active Directory."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 09/14/2017
ms.author: skwan
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 6fb8317f33ec8c36af8553466665fb2088c49527
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>Bekannte Probleme mit der verwalteten Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

## <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Das Blatt „Konfiguration“ wird nicht im Azure-Portal angezeigt

Wenn das Blatt „VM-Konfiguration“ auf Ihrem virtuellen Computer nicht angezeigt wird, wurde MSI in Ihrer Region noch nicht im Portal aktiviert.  Überprüfen Sie dies später erneut.  Sie können MSI für Ihren virtuellen Computer auch mit [PowerShell](msi-qs-configure-powershell-windows-vm.md) oder der [Azure-CLI](msi-qs-configure-cli-windows-vm.md) aktivieren.

## <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Zuweisen des Zugriffs auf virtuelle Computer im Blatt „Zugriffssteuerung (IAM)“ ist nicht möglich

Wenn im Azure-Portal unter **Zugriffssteuerung (IAM)** > **Berechtigungen hinzufügen** unter **Zugriff zuweisen zu** die Option **Virtueller Computer** nicht angezeigt wird, wurde MSI in Ihrer Region noch nicht im Portal aktiviert. Überprüfen Sie dies später erneut.  Sie können MSI dennoch für die Rollenzuweisung auswählen, indem Sie nach dem MSI-Dienstprinzipal suchen.  Geben Sie den Namen des virtuellen Computers im Feld **Auswählen** ein, und der Dienstprinzipal wird im Suchergebnis angezeigt.

## <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>Der virtuelle Computer kann nicht gestartet werden, nachdem er aus der Ressourcengruppe oder dem Abonnement verschoben wurde

Wenn Sie einen virtuellen Computer verschieben, der sich im Ausführungsstatus befindet, wird er während des Verschiebevorgangs weiterhin ausgeführt. Wenn der virtuelle Computer nach dem Verschieben allerdings beendet und neu gestartet wird, schlägt der Start fehl. Die Ursache dieses Problems besteht darin, dass der virtuelle Computer den Verweis auf die MSI-Identität nicht aktualisiert und weiterhin auf die alte Ressourcengruppe verweist.

**Problemumgehung** 
 
Lösen Sie ein Update auf dem virtuellen Computer aus, damit die richtigen Werte für die MSI abgerufen werden können. Sie können eine VM-Eigenschaftsänderung vornehmen, um den Verweis auf die MSI-Identität zu aktualisieren. Beispielsweise können Sie mit dem folgenden Befehl einen neuen Tagwert auf dem virtuellen Computer festlegen:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Mit diesem Befehl wird das neue Tag „fixVM“ mit dem Wert 1 auf dem virtuellen Computer festgelegt. 
 
Durch das Festlegen dieser Eigenschaft wird der virtuelle Computer mit dem richtigen MSI-Ressourcen-URI aktualisiert. Anschließend sollte es möglich sein, den virtuellen Computer zu starten. 
 
Nachdem der virtuelle Computer gestartet wurde, kann das Tag mithilfe des folgenden Befehls entfernt werden:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Funktioniert MSI mit ADAL (Active Directory Authentication Library) oder MSAL (Microsoft Authentication Library)?

Nein, MSI ist noch nicht in ADAL oder MSAL integriert.

