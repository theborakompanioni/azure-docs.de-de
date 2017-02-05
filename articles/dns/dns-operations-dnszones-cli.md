---
title: Verwalten von DNS-Zonen mithilfe der Befehlszeilenschnittstelle | Microsoft Docs
description: "Sie können DNS-Zonen mithilfe der Azure-Befehlszeilenschnittstelle (CLI) verwalten. Aktualisieren, Löschen und Erstellen von DNS-Zonen in Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: 6fe10238adea0cbe1a47c05767930a363645028b

---

# <a name="how-to-manage-dns-zones-using-cli"></a>Verwalten von DNS-Zonen mithilfe der Befehlszeilenschnittstelle (CLI)

> [!div class="op_single_selector"]
> * [Azure-Befehlszeilenschnittstelle](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

In dieser Anleitung wird erläutert, wie die DNS-Zonenressourcen mithilfe der plattformübergreifenden Azure-Befehlszeilenschnittstelle verwaltet werden.

Für diese Anweisungen wird die Microsoft Azure-Befehlszeilenschnittstelle (CLI) verwendet. Aktualisieren Sie vor dem Verwenden von Azure DNS-Befehlen auf die neueste Version der Azure-Befehlszeilenschnittstelle (mindestens 0.9.8). Geben Sie `azure -v` ein, um zu überprüfen, welche Version der Azure-Befehlszeilenschnittstelle derzeit auf Ihrem Computer installiert ist. Sie können die Azure-Befehlszeilenschnittstelle für Windows, Linux oder Mac installieren. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

Azure DNS ist ein nur über Azure-Ressourcen-Manager verfügbarer Dienst. Er besitzt keine ASM-API. Sie müssen sicherstellen, dass die Azure-Befehlszeilenschnittstelle für den Resource Manager-Modus konfiguriert ist. Verwenden Sie dazu den `azure config mode arm`-Befehl.

Falls die Meldung:*„Fehler: ‚dns‘ ist kein Azure-Befehl“*angezeigt wird, verwenden Sie wahrscheinlich die Azure-Befehlszeilenschnittstelle im ASM-Modus und nicht im Resource Manager-Modus.

## <a name="create-a-new-dns-zone"></a>Erstellen einer neuen DNS-Zone

Informationen zum Erstellen einer neuen DNS-Zone zum Hosten Ihrer Domäne finden Sie unter [Erstellen einer Azure-DNS-Zone mithilfe der Befehlszeilenschnittstelle](dns-getstarted-create-dnszone-cli.md).

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone `azure network dns zone show`:

```azurecli
azure network dns zone show myresourcegroup contoso.com
```

Der Vorgang gibt eine DNS-Zone mit deren ID, der Anzahl der Datensätze und Tags zurück.

## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Verwenden Sie zum Abrufen von DNS-Zonen innerhalb einer Ressourcengruppe `azure network dns zone list`.

```azurecli
azure network dns zone list myresourcegroup
```

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `azure network dns zone set`vorgenommen werden. Dadurch wird keine der DNS-Datensatzgruppen in der Zone aktualisiert (siehe [Verwalten von DNS-Einträgen](dns-operations-recordsets.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Dies ist derzeit auf die Azure Resource Manager-„Tags“ für die Zonenressource beschränkt. Weitere Informationen finden Sie unter [Etags und Tags](dns-getstarted-create-dnszone.md#tagetag) .

```azurecli
azure network dns zone set myresourcegroup contoso.com -t prod=value2
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe von `azure network dns zone delete`gelöscht werden. Dieser Vorgang hat einen optionalen Switch *-q* , der die Eingabeaufforderung zur Bestätigung des Entfernens der DNS-Zone unterdrückt.

Vor dem Löschen einer DNS-Zone in Azure DNS müssen Sie alle Datensatzgruppen löschen, mit Ausnahme der NS- und SOA-Einträge im Zonenstamm, die beim Erstellen der Zone automatisch erstellt wurden.

```azurecli
azure network dns zone delete myresourcegroup contoso.com
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen [Ressourceneintragssätze und Einträge](dns-getstarted-create-recordset-cli.md) zum Auflösen von Namen für Ihre Internetdomäne erstellt werden.




<!--HONumber=Nov16_HO3-->


