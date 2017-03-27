---
title: Erste Schritte mit Azure DNS im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist eine Schrittanleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone und Ihres ersten DNS-Eintrags im Azure-Portal.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 0fee02286f6d5dd4614a933590cdab43cc69237f
ms.lasthandoff: 03/11/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Erste Schritte mit Azure DNS im Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie Ihre erste DNS-Zone und Ihren ersten DNS-Eintrag im Azure-Portal erstellen. Sie können diese Schritte auch mithilfe von Azure PowerShell oder der plattformübergreifenden Azure CLI ausführen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Jeder dieser Schritte wird im Folgenden beschrieben.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
2. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-getstarted-portal/openzone650.png)

4. Geben Sie Ihrer DNS-Zone auf dem Blatt **DNS-Zone erstellen** einen Namen. Beispiel: *contoso.com*.
 
    ![Erstellen einer Zone](./media/dns-getstarted-portal/newzone250.png)

5. Geben Sie als nächstes die gewünschte Ressourcengruppe an. Sie können entweder eine neue Ressourcengruppe erstellen oder eine bereits vorhandene auswählen. Wenn Sie eine neue Ressourcengruppe erstellen möchten, verwenden Sie die Dropdownliste **Speicherort**, um den Speicherort der Ressourcengruppe anzugeben. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

6. Sie können das Kontrollkästchen **An Dashboard anheften** aktiviert lassen, um Ihre neue Zone auf dem Dashboard leichter wiederzufinden. Klicken Sie dann auf **Erstellen**.

    ![An Dashboard anheften](./media/dns-getstarted-portal/pindashboard150.png)

7. Nachdem Sie auf „Erstellen“ klicken, sehen Sie, wie Ihre neue Zone auf dem Dashboard konfiguriert wird.

    ![Wird erstellt](./media/dns-getstarted-portal/creating150.png)

8. Wenn Ihre neue Zone erstellt wurde, wird das Blatt Ihrer neuen Zone auf dem Dashboard geöffnet.


## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Im folgenden Beispiel werden die einzelnen Schritte zur Erstellung eines neuen A-Eintrags erläutert. Informationen zu anderen Eintragstypen und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md). 


1. Wählen Sie oben auf dem Blatt **DNS-Zone** die Option **+ Ressourceneintragssatz** aus, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.

    ![Neuer Ressourceneintragssatz](./media/dns-getstarted-portal/newrecordset500.png)

4. Benennen Sie auf dem Blatt **Ressourceneintragssatz hinzufügen** Ihren Ressourceneintragssatz. Sie können dem Ressourceneintragssatz z.B. den Namen „**www**“ geben.

    ![Ressourceneintragssatz hinzufügen](./media/dns-getstarted-portal/addrecordset500.png)

5. Wählen Sie den gewünschten Eintragstyp. Wählen Sie für dieses Beispiel die Option **A**.
6. Legen Sie die **Gültigkeitsdauer**fest. Die Gültigkeitsdauer beträgt standardmäßig eine Stunde.
7. Fügen Sie die IP-Adresse des Eintrags hinzu.
8. Wählen Sie unten auf dem Blatt **OK**, um den DNS-Eintrag zu erstellen.


## <a name="view-records"></a>Anzeigen von Datensätzen

Im unteren Teil des Blatts der DNS-Zone finden Sie die Einträge für die DNS-Zone. Sie sehen die standardmäßigen NS- und SOA-Einträge, die in jeder Zone erstellt werden, sowie alle neuen, von Ihnen erstellen Einträge.

![Zone](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualisieren der Namenserver

Wenn Sie sicher sind, dass Ihre DNS-Zone und die Einträge ordnungsgemäß eingerichtet wurden, müssen Sie den Namen Ihrer Domäne für die Verwendung der Azure-DNS-Namenserver konfigurieren. Auf diese Weise können andere Benutzer im Internet Ihre DNS-Einträge finden.

Die Namenserver für Ihre Zone sind im Azure-Portal angegeben:

![Zone](./media/dns-getstarted-portal/viewzonens500.png)

Diese Namenserver müssen in der Domänennamen-Registrierungsstelle konfiguriert werden (das ist die Stelle, bei der Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet eine Option an, um die Namenserver für die Domäne einzurichten. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md).


