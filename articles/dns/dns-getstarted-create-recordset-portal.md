---
title: Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mit dem Azure-Portal | Microsoft Docs
description: Erstellen von Hosteinträgen für Azure DNS und Erstellen von Ressourceneintragssätzen und Einträgen mit dem Azure-Portal
services: dns
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: cherylmc

---
# Erstellen von DNS-Ressourceneintragssätzen und -Einträgen mit dem Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)
> 
> 

Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mit dem Azure-Portal. Nach dem Erstellen der DNS-Zone fügen Sie die DNS-Ressourceneinträge für Ihre Domäne hinzu. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Erstellen eines Ressourceneintragssatzes und eines Ressourceneintrags
Im folgenden Beispiel wird die Erstellung eines Ressourceneintragssatzes und von Einträgen mit dem Azure-Portal schrittweise erläutert. Wir verwenden den DNS-Eintragstyp „A“.

1. Melden Sie sich beim Portal an.
2. Gehen Sie zum Blatt der **DNS-Zone**, in der Sie einen Ressourceneintragssatz erstellen möchten.
3. Wählen Sie oben auf dem Blatt **DNS-Zone** **Ressourceneintragssatz**, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.
   
    ![Neuer Ressourceneintragssatz](./media/dns-getstarted-create-recordset-portal/newrecordset500.png)
4. Benennen Sie auf dem Blatt **Ressourceneintragssatz hinzufügen** Ihren Ressourceneintragssatz. Sie können dem Ressourceneintragssatz z.B. den Namen „**www**“ geben.
   
    ![Hinzufügen eines Ressourceneintragssatzes](./media/dns-getstarted-create-recordset-portal/addrecordset500.png)
5. Wählen Sie den gewünschten Eintragstyp. Wählen Sie z.B. **A**.
6. Legen Sie die **Gültigkeitsdauer** fest. Die Gültigkeitsdauer im Portal beträgt standardmäßig eine Stunde.
7. Fügen Sie die IP-Adressen hinzu, eine IP-Adresse pro Zeile. Wenn Sie den oben vorgeschlagenen Ressourceneintragssatz-Namen und Eintragstyp wählen, wird die IPv4-IP-Adresse dem **A**-Eintrag für den „www“-Ressourceneintragssatz hinzugefügt.
8. Nachdem Sie die IP-Adressen hinzugefügt haben, wählen Sie **OK** am unteren Rand des Blatts. Der DNS-Ressourceneintragssatz wird erstellt.

## Nächste Schritte
Informationen zur Verwaltung Ihrer Ressourceneintragssätze und Einträge finden Sie unter [So erstellen und verwalten Sie DNS-Einträge und Ressourceneintragssätze mit dem Azure-Portal](dns-operations-recordsets-portal.md).

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

<!---HONumber=AcomDC_0817_2016-->