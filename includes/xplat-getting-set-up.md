---
services: virtual-machines
title: "Einrichten der Azure-CLI für die Dienstverwaltung"
author: squillace
solutions: 
manager: timlt
editor: tysonn
ms.service: virtual-machine
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: linux
ms.workload: infrastructure
ms.date: 04/13/2015
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: e664ce9426a2852a35dfdade5d41a9ce8b37a3b7
ms.openlocfilehash: f18a4705c9fd57790bff9cfdd23c19fd30fe0d32


---
## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Die folgenden Schritte unterstützen Sie bei der Verwendung der Azure-Befehlszeilenschnittstelle mit der neuesten Version und dem richtigen Abonnement. Wenn Sie die Azure-Befehlszeilenschnittstelle noch installieren und mit Ihrem Konto verbinden müssen, lesen Sie zunächst [Azure-Befehlszeilenschnittstelle (Azure-CLI)](../articles/xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Schritt 1: Aktualisieren der Azure-CLI-Version
Um die Azure-Befehlszeilenschnittstelle für Befehle im Dienstverwaltungsmodus zu verwenden, sollten Sie über eine möglichst neue Version verfügen. Geben Sie zum Überprüfen Ihrer Version `azure --version`ein. Die Ausgabe sollte folgendermaßen aussehen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Wenn Sie Ihre Version der Azure-Befehlszeilenschnittstelle aktualisieren möchten, lesen Sie die Informationen unter [Azure-CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Schritt 2: Festlegen von Azure-Konto und -Abonnement
Nachdem Sie für die Azure-CLI die Verbindung mit dem gewünschten Konto hergestellt haben, kann es sein, dass Sie über mehr als ein Abonnement verfügen. Wenn dies der Fall ist, sollten Sie die für Ihr Konto verfügbaren Abonnements durch Eingabe von `azure account list` auflisten und dann das Abonnement auswählen, das Sie verwenden möchten. Geben Sie hierzu `azure account set <subscription id or name> true` ein, wobei *subscription id or name* entweder die ID oder der Name des Abonnements ist, das Sie in der aktuellen Sitzung verwenden möchten. Folgendes sollte angezeigt werden:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [!NOTE]
> Wenn Sie noch nicht über ein Azure-Konto, aber bereits über ein MSDN-Abonnement verfügen, erhalten Sie ein kostenloses Azure-Guthaben, indem Sie Ihren [Azure-Vorteil für MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren. Sie können aber auch ein kostenloses Konto verwenden. Beides ist für den Azure-Zugriff möglich.
> 
> 




<!--HONumber=Jan17_HO3-->


