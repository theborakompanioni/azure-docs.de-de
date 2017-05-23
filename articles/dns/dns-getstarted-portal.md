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
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 93b24e3d9fbb3fbb3ea995271fd63d1e82eb9c9e
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Erste Schritte mit Azure DNS im Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie Ihre erste DNS-Zone und Ihren ersten DNS-Eintrag im Azure-Portal erstellen. Sie können diese Schritte auch mithilfe von Azure PowerShell oder der plattformübergreifenden Azure CLI ausführen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Die einzelnen Schritte werden im weiteren Verlauf beschrieben.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
2. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-getstarted-portal/openzone650.png)

4. Geben Sie auf dem Blatt **DNS-Zone erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:


   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|contoso.com|Der Name der DNS-Zone.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem die DNS-Zone erstellt werden soll.|
   |**Ressourcengruppe**|**Neu erstellen:** contosoDNSRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

> [!NOTE]
> Die Ressourcengruppe bezieht sich auf den Standort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Im folgenden Beispiel werden die einzelnen Schritte zur Erstellung eines neuen A-Eintrags erläutert. Informationen zu anderen Eintragstypen und zum Ändern vorhandener Einträge finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md). 

1. Klicken Sie nach der Erstellung der DNS-Zone im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie auf dem Blatt „Alle Ressourcen“ auf die DNS-Zone **contoso.com**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **contoso.com** in das Feld **Nach Name filtern...** eingeben und komfortabel auf die DNS-Zone zugreifen.

1. Wählen Sie oben auf dem Blatt **DNS-Zone** die Option **+ Ressourceneintragssatz** aus, um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.

1. Geben Sie auf dem Blatt **Datensatzgruppe hinzufügen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**. In diesem Beispiel erstellen Sie einen A-Eintrag.

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|www|Der Name des Eintrags.|
   |**Typ**|A| Die Art des zu erstellenden DNS-Eintrags. Zulässige Werte: „A“, „AAAA“, „CNAME“, „MX“, „NS“, „SRV“, „TXT“ und „PTR“.  Weitere Informationen zu Datensatztypen finden Sie in der [Übersicht über DNS-Zonen und -Einträge](dns-zones-records.md).|
   |**TTL**|1|Die Gültigkeitsdauer (Time To Live, TTL) der DNS-Anforderung.|
   |**TTL-Einheit**|Stunden|Die Zeiteinheit für den TTL-Wert.|
   |**IP-Adresse**|ipAddressValue| Bei diesem Wert handelt es sich um die IP-Adresse, die der DNS-Eintrag auflöst.|

## <a name="view-records"></a>Anzeigen von Datensätzen

Im unteren Teil des Blatts der DNS-Zone finden Sie die Einträge für die DNS-Zone. Hier müssten sowohl die in jeder Zone erstellten DNS- und SOA-Standardeinträge als auch alle neuen, von Ihnen erstellen Einträge angezeigt werden.

![Zone](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualisieren der Namenserver

Wenn Sie sicher sind, dass Ihre DNS-Zone und die Einträge ordnungsgemäß eingerichtet wurden, müssen Sie den Namen Ihrer Domäne für die Verwendung der Azure-DNS-Namenserver konfigurieren. Auf diese Weise können andere Benutzer im Internet Ihre DNS-Einträge finden.

Die Namenserver für Ihre Zone sind im Azure-Portal angegeben:

![Zone](./media/dns-getstarted-portal/viewzonens500.png)

Diese Namenserver müssen in der Domänennamen-Registrierungsstelle konfiguriert werden (das ist die Stelle, bei der Sie den Domänennamen erworben haben). Die Registrierungsstelle bietet die Möglichkeit zur Einrichtung der Namenserver für die Domäne. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

1. Klicken Sie im Azure-Portal im Bereich **Favoriten** auf **Alle Ressourcen**. Klicken Sie auf dem Blatt „Alle Ressourcen“ auf die Ressourcengruppe **MyResourceGroup**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **MyResourceGroup** in das Feld **Nach Name filtern...** eingeben und komfortabel auf die Ressourcengruppe zugreifen.
1. Klicken Sie auf dem Blatt **MyResourceGroup** auf die Schaltfläche **Löschen**.
1. Zur Bestätigung des Löschvorgangs werden Sie vom Portal zur Eingabe des Ressourcengruppennamens aufgefordert. Klicken Sie auf **Löschen**, geben Sie den Ressourcengruppennamen *MyResourceGroup* ein, und klicken Sie anschließend auf **Löschen**. Da beim Löschen einer Ressourcengruppe alle Ressourcen innerhalb der Ressourcengruppe gelöscht werden, überprüfen Sie vor dem Löschen immer den Inhalt der Ressourcengruppe. Das Portal löscht zuerst alle in der Ressourcengruppe enthaltenen Ressourcen und anschließend die Ressourcengruppe selbst. Dieser Vorgang dauert einige Minuten.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

Weitere Informationen zum Verwalten von DNS-Einträgen in Azure DNS finden Sie unter [Verwalten von DNS-Einträgen und - Ressourceneintragssätzen im Azure-Portal](dns-operations-recordsets-portal.md).


