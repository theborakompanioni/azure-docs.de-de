---
title: "Delegieren einer Domäne an Azure DNS | Microsoft Docs"
description: "Grundlegendes zum Ändern der Domänendelegierung und zum Verwenden von DNS-Namenserver zum Hosten von Domänen."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---

# <a name="delegate-a-domain-to-azure-dns"></a>Delegieren von Domänen an Azure DNS

Azure DNS ermöglicht es Ihnen, eine DNS-Zone zu hosten und die DNS-Einträge für eine Domäne in Azure zu verwalten. Damit DNS-Abfragen für eine Domäne Azure DNS erreichen, muss die Domäne aus der übergeordneten Domäne an Azure DNS delegiert werden. Denken Sie daran, dass Azure DNS keine Domänenregistrierungsstelle ist. In diesem Artikel erfahren Sie, wie Sie Ihre Domäne an Azure DNS delegieren.

Für Domänen, die von einer Registrierungsstelle erworben wurden, bietet Ihnen die Registrierungsstelle die Möglichkeit zur Einrichtung dieser NS-Einträge. Sie müssen keine Domäne besitzen, um eine DNS-Zone mit diesem Domänennamen in Azure DNS zu erstellen. Sie müssen jedoch Besitzer der Domäne sein, um die Delegierung an Azure DNS bei der Registrierungsstelle einzurichten.

Nehmen wir beispielsweise an, Sie erwerben die Domäne „contoso.net“ und erstellen eine Zone mit dem Namen „contoso.net“ in Azure DNS. Als Besitzer der Domäne bietet Ihre Registrierungsstelle Ihnen die Möglichkeit, die Namenserveradressen (also die NS-Einträge) für Ihre Domäne zu konfigurieren. Die Registrierungsstelle speichert diese NS-Einträge in der übergeordneten Domäne (in diesem Fall „.net“). Daraufhin können Clients auf der ganzen Welt, die versuchen, DNS-Einträge in „contoso.net“ aufzulösen, an Ihre Domäne in der Azure DNS-Zone weitergeleitet werden.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
1. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-domain-delegation/dns.png)

1. Geben Sie auf dem Blatt **DNS-Zone erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:

   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|contoso.net|Der Name der DNS-Zone.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie das Anwendungsgateway erstellen möchten.|
   |**Ressourcengruppe**|**Neu erstellen:** contosoRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

> [!NOTE]
> Die Ressourcengruppe bezieht sich auf den Standort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

## <a name="retrieve-name-servers"></a>Abrufen von Namenservern

Bevor Sie Ihre DNS-Zone an Azure DNS delegieren können, müssen Sie den Namenserver für Ihre Zone ermitteln. Azure DNS weist bei jeder Zonenerstellung Namenserver aus einem Pool zu.

1. Klicken Sie im Azure-Portal nach der Erstellung der DNS-Zone im Bereich **Favoriten** auf **Alle Ressourcen**. Klicken Sie auf dem Blatt **Alle Ressourcen** auf die DNS-Zone **contoso.net**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **contoso.net** in das Feld „Nach Name filtern...“ eingeben und komfortabel auf das Anwendungsgateway zugreifen. 

1. Rufen Sie die Namenserver über das Blatt „DNS-Zone“ ab. In diesem Beispiel wurden der Zone „contoso.net“ die Namenserver „ns1-01.azure-dns.com“, „ns2-01.azure-dns.net“, „ns3-01.azure-dns.org“ und „ns4-01.azure-dns.info“ zugewiesen:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS erstellt in Ihrer Zone automatisch autoritative NS-Einträge mit den zugewiesenen Namenservern.  Die Namen der Namenserver können Sie über Azure PowerShell oder mithilfe der Azure-Befehlszeilenschnittstelle anzeigen, indem Sie diese Einträge abrufen.

In den folgenden Beispielen erfahren Sie auch, wie Sie die Namenserver für eine Zone in Azure DNS über PowerShell und mithilfe der Azure-Befehlszeilenschnittstelle abrufen.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Das folgende Beispiel ist die Antwort.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Das folgende Beispiel ist die Antwort.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Delegieren der Domäne

Nachdem Sie die DNS-Zone erstellt haben und über die Namenserver verfügen, muss die übergeordnete Domäne mit den Azure DNS-Namenservern aktualisiert werden. Jede Registrierungsstelle hat seine eigenen DNS-Verwaltungstools, um die Namenservereinträge für eine Domäne zu ändern. Bearbeiten Sie auf der DNS-Verwaltungsseite der Registrierungsstelle die NS-Einträge, und ersetzen Sie die NS-Einträge durch die von Azure DNS erstellten Einträge.

Wenn Sie eine Domäne an Azure DNS delegieren, müssen Sie die von Azure DNS bereitgestellten Namen der Namenserver verwenden. Es empfiehlt sich, alle vier Namenservernamen zu verwenden – unabhängig vom Namen Ihrer Domäne. Für die Domänendelegierung ist es nicht erforderlich, dass der Namensservername dieselbe Domäne der obersten Ebene wie Ihre Domäne verwendet.

Sie sollten nicht mit Verbindungsdatensätzen auf die IP-Adressen der Azure DNS-Namenserver verweisen, da sich diese IP-Adressen später ändern können. Delegierungen, für die Namen der Namenserver in Ihrer eigenen Zone verwendet werden (manchmal als „Vanity-Namenserver“ bezeichnet), werden derzeit in Azure DNS nicht unterstützt.

## <a name="verify-name-resolution-is-working"></a>Überprüfen, ob die Namensauflösung funktioniert

Nach Abschluss der Delegierung können Sie überprüfen, ob die Namensauflösung funktioniert, indem Sie mithilfe eines Tools wie „nslookup“ den SOA-Eintrag für die Zone abrufen (wird beim Erstellen der Zone automatisch erstellt).

Die Azure DNS-Namenserver müssen nicht angegeben werden, da sie im Rahmen des normalen DNS-Auflösungsvorgangs gefunden werden, sofern die Delegierung korrekt eingerichtet wurde.

```
nslookup -type=SOA contoso.com
```

Hier sehen Sie eine Beispielantwort für den vorherigen Befehl:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Delegieren von Unterdomänen in Azure DNS

Wenn Sie eine separate untergeordnete Zone einrichten möchten, können Sie eine Unterdomäne in Azure DNS delegieren. Angenommen, Sie möchten nach dem Einrichten und Delegieren von „contoso.net“ in Azure DNS die separate untergeordnete Zone „partners.contoso.net“ einrichten.

1. Erstellen Sie die untergeordnete Zone „partners.contoso.net“ in Azure DNS.
2. Suchen Sie die autoritativen NS-Datensätze in der untergeordneten Zone, um die Namenserver abzurufen, die die untergeordnete Zone in Azure DNS hosten.
3. Delegieren Sie die untergeordnete Zone, indem Sie NS-Datensätze in der übergeordneten Zone konfigurieren, die auf die untergeordnete Zone verweisen.

### <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
1. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-domain-delegation/dns.png)

1. Geben Sie auf dem Blatt **DNS-Zone erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:

   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|partners.contoso.net|Der Name der DNS-Zone.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie das Anwendungsgateway erstellen möchten.|
   |**Ressourcengruppe**|**Vorhandene verwenden:** contosoRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

> [!NOTE]
> Die Ressourcengruppe bezieht sich auf den Standort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

### <a name="retrieve-name-servers"></a>Abrufen von Namenservern

1. Klicken Sie nach der Erstellung der DNS-Zone im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie auf dem Blatt **Alle Ressourcen** auf die DNS-Zone **partners.contoso.net**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **partners.contoso.net** in das Feld „Nach Name filtern...“ eingeben und komfortabel auf die DNS-Zone zugreifen.

1. Rufen Sie die Namenserver über das Blatt „DNS-Zone“ ab. In diesem Beispiel wurden der Zone „contoso.net“ die Namenserver „ns1-01.azure-dns.com“, „ns2-01.azure-dns.net“, „ns3-01.azure-dns.org“ und „ns4-01.azure-dns.info“ zugewiesen:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS erstellt in Ihrer Zone automatisch autoritative NS-Einträge mit den zugewiesenen Namenservern.  Die Namen der Namenserver können Sie über Azure PowerShell oder mithilfe der Azure-Befehlszeilenschnittstelle anzeigen, indem Sie diese Einträge abrufen.

### <a name="create-name-server-record-in-parent-zone"></a>Erstellen des Namenservereintrags in der übergeordneten Zone

1. Navigieren Sie im Azure-Portal zur DNS-Zone **contoso.net**.
1. Klicken Sie auf **+ Datensatzgruppe**.
1. Geben Sie auf dem Blatt **Datensatzgruppe hinzufügen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**:

   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|partners|Der Name der untergeordneten DNS-Zone.|
   |**Typ**|NS|Verwenden Sie „NS“ für Namenservereinträge.|
   |**TTL**|1|Die Gültigkeitsdauer (Time To Live, TTL).|
   |**TTL-Einheit**|Stunden|Legt die Einheit für die Gültigkeitsdauer auf Stunden fest.|
   |**NAMENSERVER**|{Namenserver aus der Zone „partners.contoso.net“}|Geben Sie alle vier Namenserver aus der Zone „partners.contoso.net“ ein. |

   ![Dns-nameserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Delegieren von Unterdomänen in Azure DNS mit anderen Tools

Die folgenden Beispiele zeigen, wie Sie Unterdomänen in Azure DNS mithilfe von PowerShell und über die Befehlszeilenschnittstelle delegieren:

#### <a name="powershell"></a>PowerShell

Die Vorgehensweise wird im folgenden PowerShell-Beispiel veranschaulicht. Die gleichen Schritte können über das Azure-Portal oder mithilfe der plattformübergreifenden Azure-Befehlsschnittstelle ausgeführt werden.

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Verwenden Sie `nslookup`, um sich zu vergewissern, dass alles ordnungsgemäß eingerichtet ist. Sehen Sie sich dazu den SOA-Eintrag der untergeordneten Zone an.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Ermitteln Sie anhand der Ausgabe die Namenserver für die Zone `partners.contoso.net`.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Erstellen Sie die Datensatzgruppe und die NS-Einträge für die einzelnen Namenserver.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

1. Klicken Sie im Azure-Portal im Bereich **Favoriten** auf **Alle Ressourcen**. Klicken Sie auf dem Blatt „Alle Ressourcen“ auf die Ressourcengruppe **contosorg**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **contosorg** in das Feld **Nach Name filtern...** eingeben und komfortabel auf die Ressourcengruppe zugreifen.
1. Klicken Sie auf dem Blatt **contosorg** auf die Schaltfläche **Löschen**.
1. Zur Bestätigung des Löschvorgangs werden Sie vom Portal zur Eingabe des Ressourcengruppennamens aufgefordert. Geben Sie den Ressourcengruppennamen *contosorg* an, und klicken Sie dann auf **Löschen**. Da beim Löschen einer Ressourcengruppe alle Ressourcen innerhalb der Ressourcengruppe gelöscht werden, überprüfen Sie vor dem Löschen immer den Inhalt der Ressourcengruppe. Das Portal löscht zuerst alle in der Ressourcengruppe enthaltenen Ressourcen und anschließend die Ressourcengruppe selbst. Dieser Vorgang dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von DNS-Zonen](dns-operations-dnszones.md)

[Verwalten von DNS-Einträgen](dns-operations-recordsets.md)

