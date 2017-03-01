---
title: "Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mit dem Azure-Portal | Microsoft Docs"
description: "Erstellen von Hosteinträgen für Azure DNS und Erstellen von Ressourceneintragssätzen und Einträgen mit dem Azure-Portal"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f93905f4-e82e-45db-b490-878d318e6aba
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 2752579b6277a27ca1e932fbd2c402ac3135e80c
ms.lasthandoff: 02/27/2017

---

# <a name="create-dns-record-sets-and-records-by-using-the-azure-portal"></a>Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mit dem Azure-Portal. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

In allen Beispielen auf dieser Seite werden DNS-Einträge vom Typ „A“ verwendet. Der Prozess für andere Eintragstypen ist ähnlich.

Wenn Ihr neuer Eintrag den gleichen Namen und Typ besitzt wie ein bereits vorhandener Eintrag, müssen Sie ihn dem vorhandenen Ressourceneintragssatz hinzufügen&mdash;. Informationen finden Sie unter [Verwalten von DNS-Einträgen und -Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md). Besitzt Ihr neuer Eintrag einen anderen Namen und Typ als alle bereits vorhandenen Einträge, müssen Sie wie weiter unten beschrieben einen neuen Ressourceneintragssatz erstellen.

## <a name="create-records-in-a-new-record-set"></a>Erstellen von Einträgen in einem neuen Ressourceneintragssatz

Im folgenden Beispiel wird die Erstellung eines Ressourceneintragssatzes und von Einträgen mit dem Azure-Portal schrittweise erläutert.

1. Melden Sie sich beim Portal an.
2. Gehen Sie zum Blatt der **DNS-Zone** , in der Sie einen Ressourceneintragssatz erstellen möchten.
3. Wählen Sie oben auf dem Blatt **DNS-Zone** die Option **+ Ressourceneintragssatz** aus, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.

    ![Neuer Ressourceneintragssatz](./media/dns-getstarted-create-recordset-portal/newrecordset500.png)

4. Benennen Sie auf dem Blatt **Ressourceneintragssatz hinzufügen** Ihren Ressourceneintragssatz. Sie können dem Ressourceneintragssatz z.B. den Namen „**www**“ geben.

    ![Ressourceneintragssatz hinzufügen](./media/dns-getstarted-create-recordset-portal/addrecordset500.png)

5. Wählen Sie den gewünschten Eintragstyp. Wählen Sie z.B. **A**.
6. Legen Sie die **Gültigkeitsdauer**fest. Die Gültigkeitsdauer im Portal beträgt standardmäßig eine Stunde.
7. Fügen Sie die Details der einzelnen Einträge im Ressourceneintragssatz hinzu. Da der Eintragstyp „A“ lautet, müssen Sie in diesem Fall die IP-Adressen des A-Eintrags (eine IP-Adresse pro Zeile) hinzufügen.
8. Nachdem Sie die IP-Adressen hinzugefügt haben, wählen Sie **OK** am unteren Rand des Blatts. Der DNS-Ressourceneintragssatz wird erstellt.

### <a name="verify-name-resolution"></a>Überprüfen der Namensauflösung

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Einträge auf den Azure DNS-Namenservern vorhanden sind.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie [die DNS-Abfrage direkt an einen der Namenserver für Ihre Zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Fügen Sie im folgenden Befehl die passenden Werte für Ihre Eintragszone ein.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie [Ihren Domänennamen an die Azure DNS-Namenserver delegieren](dns-domain-delegation.md).

Informationen zur Verwaltung Ihrer Ressourceneintragssätze und Einträge finden Sie unter [So erstellen und verwalten Sie DNS-Einträge und Ressourceneintragssätze mit dem Azure-Portal](dns-operations-recordsets-portal.md).

