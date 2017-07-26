---
title: "Verwalten von DNS-Zonen in Azure DNS – Azure-Portal | Microsoft-Dokumentation"
description: "Sie können DNS-Zonen mithilfe des Azure-Portals verwalten. In diesem Artikel wird das Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS beschrieben."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 69a509612e6204fc93dd42bf2fe69cb165b5777c
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Verwalten von DNS-Zonen im Azure-Portal

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

In diesem Artikel wird gezeigt, wie DNS-Zonen mithilfe des Azure-Portals verwaltet werden. Sie können Ihre DNS-Zonen auch mithilfe der plattformübergreifenden [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md) oder mit Azure [PowerShell](dns-operations-dnszones.md) verwalten.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
2. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-operations-dnszones-portal/openzone650.png)

4. Geben Sie auf dem Blatt **DNS-Zone erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:


   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|contoso.com|Der Name der DNS-Zone.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem die DNS-Zone erstellt werden soll.|
   |**Ressourcengruppe**|**Neu erstellen:** contosoDNSRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

> [!NOTE]
> Die Ressourcengruppe bezieht sich auf den Standort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Navigieren Sie im Azure-Portal zu **Weitere Dienste** > **Netzwerk** > **DNS-Zonen**. Jede DNS-Zone ist ihre eigene Ressource, und in dieser Ansicht können Informationen wie die Anzahl von Datensatzgruppen und Namenserver angezeigt werden. Die Spalte **NAMENSERVER** ist nicht in der Standardansicht enthalten, klicken Sie zum Hinzufügen auf **Spalten**, wählen Sie **Namenserver** aus, und klicken Sie auf **Fertig**.

![Auflisten von DNS-Zonen](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

Navigieren Sie zu einer DNS-Zone im Portal. Klicken Sie auf dem Blatt **DNS-Zone** auf **Zone löschen**. Sie werden aufgefordert, das Löschen der DNS-Zone zu bestätigen. Beim Löschen einer DNS-Zone werden auch alle in der Zone enthaltenen Datensätze gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Arbeit mit Ihrer DNS-Zone und Ihren Datensätzen unter [Erste Schritte mit Azure DNS über das Azure-Portal](dns-getstarted-portal.md).
