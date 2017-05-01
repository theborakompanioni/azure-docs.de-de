---
title: "Übersicht über den Azure-Instanzmetadatendienst | Microsoft-Dokumentation"
description: "RESTful-Schnittstelle zum Abrufen von Informationen über Compute-, Netzwerk- und anstehende Wartungsereignisse eines virtuellen Computers."
services: virtual-machines-windows, virtual-machines-linux,virtual-machines-scale-sets, cloud-services
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: 
ms.service: azure-instancemetadataservice
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2017
ms.author: harijay
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: b5c0268e1a0ebfcb33781678a367090cad865907
ms.lasthandoff: 04/11/2017

---

# <a name="azures-instance-metadata-service---preview"></a>Instanzmetadatendienst von Azure – Vorschau

> [!NOTE] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Der Instanzmetadatendienst bietet Informationen bezüglich der ausgeführten Instanzen von virtuellen Computern. Diese Informationen können zum Verwalten und Konfigurieren Ihrer Instanzen in Azure verwendet werden. Der Instanzmetadatendienst von Azure ist ein RESTful-Endpunkt, der für alle IaaS-VMs verfügbar ist, die mit dem neuen [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/?redirectedfrom=MSDN) erstellt wurden. Der Endpunkt steht unter einer bekannten, nicht routingfähigen IP-Adresse zur Verfügung (*169.254.169.254*), auf die nur von innerhalb der VM zugegriffen werden kann. Dieser Dienst stellt wichtige Daten im Zusammenhang mit der Instanz eines virtuellen Computers bereit, z.B. die Netzwerkkonfiguration und anstehende Wartungsereignisse. Weitere Informationen finden Sie unter [Metadatenkategorien](#instance-metadata-data-categories).

### <a name="important-information"></a>Wichtige Informationen
Derzeit befindet sich dieser Dienst in der **Vorschauphase**, und er hostet Informationen über eine Instanz eines virtuellen Computers und anstehende Wartungsereignisse. Der Dienst wird weiter aktualisiert, und auf dieser Seite werden die spezifischen verfügbaren [Datenkategorien](#instance-metadata-data-categories) aufgeführt.


## <a name="service-availability"></a>Dienstverfügbarkeit
Die aktuelle Vorschau steht in der Region **USA, Westen-Mitte** von Azure zur Verfügung. Die folgende Tabelle wird mit Regionen aktualisiert, in denen die Dienstvorschau neu eingeführt wird.
Sie können den Instanzmetadatendienst testen, indem Sie eine VM mit dem [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/?redirectedfrom=MSDN) oder dem [Azure-Portal](http://portal.azure.com) erstellen und den Beispielen im Abschnitt mit Beispielen folgen, um auf den Metadatendienst zuzugreifen. 

Regionen, in denen die Vorschau des Instanzmetadatendiensts verfügbar ist|
------------------------------------------------------------|
USA, Westen-Mitte |



## <a name="retrieving-instance-metadata"></a>Abrufen von Instanzmetadaten 

Instanzmetadaten stehen für die Ausführung von VMs zur Verfügung, die mit dem [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/?redirectedfrom=MSDN) erstellt/verwaltet werden. Verwenden Sie für den Zugriff auf alle Datenkategorien für eine Instanz eines virtuellen Computers den folgenden URI:

```
 curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-03-01"
```

Die Standardausgabe für alle Instanzmetadaten hat das JSON-Format (Inhaltstyp Anwendung/JSON)

## <a name="usage-examples"></a>Anwendungsbeispiele
Im Folgenden finden Sie einige Beispiele und die Nutzungssemantik für den Instanzmetadatendienst.

### <a name="versioning"></a>Versionsverwaltung 
Der Instanzmetadatendienst nutzt die Versionsverwaltung. Versionen sind obligatorisch, und die aktuelle Vorschauversion ist 2017-03-01.


```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-03-01"
```

Wenn wir neuere Versionen hinzufügen, kann auf ältere Versionen aus Kompatibilitätsgründen weiterhin zugegriffen werden, falls Ihre Skripts von bestimmten Datenformaten abhängig sind. Beachten Sie, dass die aktuelle Vorschauversion möglicherweise nicht verfügbar ist, sobald der Dienst allgemein zur Verfügung steht.


### <a name="data-output"></a>Datenausgabe
Standardmäßig gibt der Instanzmetadatendienst Daten im JSON-Format zurück (Inhaltstyp Anwendung/JSON). Unterschiedliche Knotenelemente können ggf. Daten in anderen Standardformaten zurückgeben. Die folgende Tabelle bietet eine Kurzübersicht über Datenformate. 

Element | Standarddatenformat | Andere Formate
--------|---------------------|--------------
/instance | json | text
/scheduledevents | json | (Keine)

Verwenden Sie als Textformat in der Anforderungs-URL „format=text“, Beispiel: 

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-03-01&format=text"
```
### <a name="security"></a>Sicherheit
Auf den Instanzmetadaten-Endpunkt kann nur innerhalb der ausgeführten Instanz des virtuellen Computers an einer nicht routingfähigen IP-Adresse (169.254.169.254) zugegriffen werden. Darüber hinaus wird jede Anforderung mit **X-Forwarded-For-Header** vom Metadatendienst abgelehnt wird. Zudem müssen Anforderungen **Metadata:true**-Header enthalten, die gesendet werden, um sicherzustellen, dass die eigentliche Anforderung direkt vorgesehen war und nicht Teil einer unbeabsichtigten Umleitung ist. 
### <a name="error"></a>Error
Wenn ein Datenelement nicht gefunden wird oder Anforderungen ungültig sind, gibt der Instanzmetadatendienst standardmäßige HTTP-Fehler zurück. Im Folgenden sind einige Beispiele von Rückgabecodes aufgeführt: 

HTTP-Rückgabecode | Grund
----------------|-------
200 | OK
400 | Ungültige Anforderung, fehlender Header, Übergabe von „-H Metadata:true“
404 | Nicht gefunden, angefordertes Element ist nicht vorhanden 
405 | Nicht unterstützte Methode 
429 | Zu viele Anforderungen, derzeit werden nur bis zu 5 Abfragen pro Sekunde unterstützt

### <a name="examples"></a>Beispiele
#### <a name="retrieving-the-network-information"></a>Abrufen der Netzwerkinformationen 

**Anforderung**
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-03-01"

```
**Antwort**
> [!NOTE] 
> Die Antwort ist eine JSON-Zeichenfolge. Folgende Ausgabe wird mit einem Hilfsprogramm wie [jq](https://stedolan.github.io/jq/) mit JSON formatiert.
>

```
{
  "interface": [
    {
      "ipv4": {
        "ipaddress": [
          {
            "ipaddress": "10.0.0.4",
            "publicip": "<>.<>.<>.<>"
          }
        ],
        "subnet": [
          {
            "address": "10.0.0.0",
            "dnsservers": [
              {
                "ipaddress": "10.0.0.2"
              },
              {
                "ipaddress": "10.0.0.3"
              }
            ],
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipaddress": []
      },
      "mac": "000D3A00FA89"
    }
  ]
}
```

#### <a name="retrieving-public-ip-address"></a>Abrufen der öffentlichen IP-Adresse

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipaddress/0/publicip?api-version=2017-03-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Abrufen aller Metadaten für eine Instanz

**Anforderung**
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-03-01"
```
**Antwort**
> [!NOTE]
> Die Antwort ist eine JSON-Zeichenfolge. Folgende Ausgabe wird mit einem Hilfsprogramm wie [jq](https://stedolan.github.io/jq/) mit JSON formatiert.
>

```
{
"compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipaddress": [
            {
              "ipaddress": "10.0.0.4",
              "publicip": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.0.0",
              "dnsservers": [
                {
                  "ipaddress": "10.0.0.2"
                },
                {
                  "ipaddress": "10.0.0.3"
                }
              ],
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipaddress": []
        },
        "mac": "000D3A00FA89"
      }
    ]
  }
}

```
#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Abrufen von Metadaten in einem virtuellen Windows-Computer

Die Instanzmetadaten können unter Windows über das PowerShell-Hilfsprogramm „curl“ abgerufen werden. Führen Sie an einer PowerShell-Eingabeaufforderung die folgenden Befehle aus: 

**Anforderung**
```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-03-01 | select -ExpandProperty Content
```
**Antwort**
>[!NOTE]
> Die Antwort ist eine JSON-Zeichenfolge. Die folgende Ausgabe wurde mit dem PowerShell-Hilfsprogramm „ConvertTo-Json“ mit JSON formatiert.
> 

```
{
    "compute":  {
                    "location":  "CentralUSEUAP",
                    "name":  "SQLTest",
                    "offer":  "SQL2016SP1-WS2016",
                    "osType":  "Windows",
                    "platformFaultDomain":  "0",
                    "platformUpdateDomain":  "0",
                    "publisher":  "MicrosoftSQLServer",
                    "sku":  "Enterprise",
                    "version":  "13.0.400110",
                    "vmId":  "453945c8-3923-4366-b2d3-ea4c80e9b70e",
                    "vmSize":  "Standard_DS2"
                },
    "network":  {
                    "interface":  [
                                      "@{ipv4=; ipv6=; mac=002248020E1E}"
                                  ]
                }
}
```


## <a name="instance-metadata-data-categories"></a>Datenkategorien von Instanzmetadaten
Die folgende Tabelle enthält eine Liste der über Instanzmetadaten verfügbaren Datenkategorien.

Daten | Beschreibung
-----|------------
location | Azure-Region, in der die VM ausgeführt wird 
name | Name des virtuellen Computers 
offer | Angebotsinformationen für das VM-Image, diese Werte sind nur für Images vorhanden, die über den Azure-Imagekatalog bereitgestellt werden 
Herausgeber | Herausgeber des VM-Images
sku | Spezifische SKU für das VM-Image  
Version | Version des VM-Images 
osType | Linux oder Windows 
platformUpdateDomain |  [Updatedomäne](virtual-machines-windows-manage-availability.md), in der die VM ausgeführt wird. 
platformFaultDomain | [Fehlerdomäne](virtual-machines-windows-manage-availability.md), in der die VM ausgeführt wird.
vmId | Eindeutiger Bezeichner der VM, weitere Informationen finden Sie [hier](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)
vmSize | [Größe des virtuellen Computers](virtual-machines-windows-sizes.md)
ipv4/ipaddress | Lokale IP-Adresse der VM 
ipv4/publicip | Öffentliche IP-Adresse für die Instanz 
subnet/address | Adresse für das Subnetz 
subnet/dnsservers/ipaddress1 | Primärer DNS-Server
subnet/dnsservers/ipaddress2 | Sekundärer DNS-Server
subnet/prefix | Subnetzpräfix, Beispiel 24
ipv6/ipaddress | IPv6-Adresse der VM
mac | VM-Mac-Adresse 
scheduledevents | Siehe [scheduledevents](virtual-machines-scheduled-events.md)



## <a name="example-scenarios-for-usage"></a>Beispielszenarien für die Verwendung  

### <a name="tracking-vm-running-on-azure"></a>Nachverfolgen einer in Azure ausgeführten VM 

Als Dienstanbieter müssen Sie möglicherweise die Anzahl von VMs nachverfolgen, auf denen Ihre Software ausgeführt wird, oder benötigen Agents, die die Eindeutigkeit der VM überwachen müssen. Um eine eindeutige ID für eine VM abrufen zu können, verwenden Sie das vmId-Feld des Instanzmetadatendiensts. 

**Anforderung**
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-03-01&format=text"
```
**Antwort**
```
5c08b38e-4d57-4c23-ac45-aca61037f084

```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Platzierung von Containern und Datenpartitionen basierend auf der Fehler-/Updatedomäne 

In bestimmten Szenarien ist die Platzierung unterschiedlicher Replikate von primärer Bedeutung. Beispielsweise bei der [HDFS-Replikatplatzierung](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) oder bei der Containerplatzierung über einen [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) müssen Sie die platformFaultDomain und die platformUpdateDomain kennen, auf denen die VM ausgeführt wird. Sie können diesen Datenpunkt direkt über die Instanzmetadaten abfragen.

**Anforderung**
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-03-01&format=text" 
```
**Antwort**
```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Abrufen von weiteren Informationen zur VM während einer Supportanfrage 

Als Dienstanbieter erhalten Sie möglicherweise eine Supportanfrage, für die Sie weitere Informationen zur VM erfahren möchten. Wenn Sie den Kunden auffordern, die Computemetadaten weiterzuleiten, können Sie dem Supportmitarbeiter grundlegende Informationen über die Art von VM in Azure bereitstellen. 

**Anforderung**
```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-03-01"
```
**Antwort**
> [!NOTE] 
> Die Antwort ist eine JSON-Zeichenfolge. Folgende Ausgabe wird mit einem Hilfsprogramm wie [jq](https://stedolan.github.io/jq/) mit JSON formatiert.
>

```
{
"compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }

}
```

## <a name="faq"></a>Häufig gestellte Fragen
1. Ich erhalte die Fehlermeldung „400 – Ungültige Anforderung. Der erforderliche Metadatenheader wurde nicht angegeben“. Was bedeutet dies?
   * Für den Instanzmetadatendienst muss der Header „Metadata:true“ in der Anforderung übergeben werden. Durch Übergeben des Headers im REST-Aufruf wird der Zugriff auf den Instanzmetadatendienst ermöglicht. 
2. Warum erhalte ich keine Compute-Informationen für meine VM?
   * Derzeit unterstützt der Instanzmetadatendienst nur in Azure Resource Manager erstellte Instanzen. In Zukunft wird möglicherweise Unterstützung für Cloud Services-VMs hinzugefügt. 
3. Ich habe meinen virtuellen Computer vor einiger Zeit über den Azure Resource Manager erstellt. Warum sehe ich keine Computemetadateninformationen?
   * Fügen Sie für VMs, die nach September 2016 erstellt wurden, ein [Tag](../azure-resource-manager/resource-group-using-tags.md) hinzu, damit Computemetadaten angezeigt werden. Fügen Sie für eine ältere VM (die vor September 2016 erstellt wurde) Erweiterungen oder Datenträger zur VM hinzu, bzw. entfernen Sie diese, um Metadaten zu aktualisieren.
4. Warum erhalte ich den Fehler 500 – „Interner Serverfehler“?
   * Derzeit ist die Vorschau des Instanzmetadatendiensts nur in der Region „USA, Westen-Mitte“ verfügbar. Stellen Sie Ihre VMs in den unterstützten Regionen bereit.  
4. Wie kann ich weitere Fragen/Kommentare weiterleiten?
   * Senden Sie Ihre Kommentare an „feedback.azure.com“.
    
## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [scheduledevents] (virtual-machines-scheduled-events.md)

