---
services: virtual-machines
title: Verwenden der Azure-CLI mit dem Azure Resource Manager
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
ms.openlocfilehash: e2f9d2c74e5dfa0a08f25685062903a985ba641c


---
## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Verwenden der Azure-CLI mit dem Azure-Ressourcen-Manager (ARM)
Bevor Sie die Azure-CLI mit Ressourcen-Manager-Befehlen und -Vorlagen zur Bereitstellung von Azure-Ressourcen und -Workloads über Ressourcengruppen verwenden können, benötigen Sie (natürlich) ein Azure-Konto. Wenn Sie noch kein Konto haben, erhalten Sie [hier eine kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Wenn Sie noch nicht über ein Azure-Konto, aber bereits über ein MSDN-Abonnement verfügen, erhalten Sie ein kostenloses Azure-Guthaben, indem Sie Ihren [Azure-Vorteil für MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren. Sie können aber auch ein kostenloses Konto verwenden. Beides ist für den Azure-Zugriff möglich.
> 
> 

### <a name="step-1-verify-the-azure-cli-version"></a>Schritt 1: Überprüfen der Azure-CLI-Version
Um die Azure-CLI für Befehle und ARM-Vorlagen verwenden zu können, benötigen Sie mindestens Version 0.8.17. Geben Sie zum Überprüfen Ihrer Version `azure --version`ein. Die Ausgabe sollte folgendermaßen aussehen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Wenn Sie Ihre Version der Azure-CLI aktualisieren müssen, lesen Sie die Informationen unter [Azure-CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Schritt 2: Überprüfen der für Azure verwendeten Identität (Arbeit oder Schule)
Sie können den ARM-Befehlsmodus nur nutzen, wenn Sie einen [Azure Active Directory-Mandanten](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) oder einen [Dienstprinzipalnamen](https://msdn.microsoft.com/library/azure/dn132633.aspx) verwenden. (Diese werden auch als *Organisations-IDs*bezeichnet.)

Um Ihre verwendete Identität zu ermitteln, melden Sie sich durch Eingabe von `azure login` an. Geben Sie, wenn Sie dazu aufgefordert werden, Ihren Arbeits- oder Schulbenutzernamen sowie das zugehörige Kennwort ein. Wenn Sie über die erforderliche ID verfügen, sollte etwa Folgendes angezeigt werden:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
    |info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Wenn dies nicht angezeigt wird, müssen Sie einen neuen Mandanten (oder Dienstprinzipal) für Ihre Microsoft-Kontoidentität erstellen. (Dies ist häufig bei persönlichen MSDN-Abonnements oder kostenlosen Testabonnements der Fall.) Informationen zum Erstellen einer Geschäfts-, Schul- oder Uni-ID über Ihr Azure-Konto, das mit einer Microsoft-ID erstellt wurde, finden Sie unter [Zuordnen eines Azure AD-Verzeichnisses zu einem neuen Azure-Abonnement](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). an die Person wenden, die das Konto für Sie erstellt hat.

### <a name="step-3-choose-your-azure-subscription"></a>Schritt 3: Auswählen des Azure-Abonnements
Wenn Sie in Ihrem Azure-Konto nur über ein Abonnement verfügen, wird die Azure-CLI standardmäßig automatisch diesem Abonnement zugeordnet. Wenn Sie über mehrere Abonnements verfügen, müssen Sie Ihr Abonnement durch Eingabe von `azure account set <subscription id or name> true` auswählen, wobei *subscription id or name* entweder die ID oder der Name des Abonnements ist, das Sie in der aktuellen Sitzung verwenden möchten.

Folgendes sollte angezeigt werden:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Schritt 4: Versetzen der Azure-CLI in den ARM-Modus
Um den ARM (Azure-Ressourcen-Manager)-Modus mit der Azure-Befehlszeilenschnittstelle zu verwenden, geben Sie `azure config mode arm`ein. Folgendes sollte angezeigt werden:

    $ azure config mode arm
    info:    New mode is arm

> [!NOTE]
> Sie können wieder zurück zu ASM-Befehlen (Azure Service Management) wechseln, indem Sie `azure config mode asm` eingeben.
> 
> 




<!--HONumber=Jan17_HO3-->


