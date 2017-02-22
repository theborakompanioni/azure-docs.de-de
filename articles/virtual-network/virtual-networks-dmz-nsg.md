---
title: 'Azure DMZ-Beispiel: Erstellen einer einfachen DMZ mit Netzwerksicherheitsgruppen | Microsoft-Dokumentation'
description: Erstellen einer DMZ mit Netzwerksicherheitsgruppen (NSGs)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
translationtype: Human Translation
ms.sourcegitcommit: cb2e480a45871ad0c956dc976de955ca48ecdfd0
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722


---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Beispiel 1 – Erstellen einer einfachen DMZ unter Verwendung von NSGs mit einer Azure Resource Manager-Vorlage 
[Zurück zur Seite mit Best Practices zu Sicherheitsgrenzen][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-Vorlage](virtual-networks-dmz-nsg.md)
> * [Klassisch – PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

In diesem Beispiel wird eine einfache DMZ mit vier Windows-Servern und Netzwerksicherheitsgruppen erstellt. In diesem Beispiel wird jeder der relevanten Vorlagenabschnitte genau erläutert, um ein besseres Verständnis jedes einzelnen Schritts zu ermöglichen. Es gibt außerdem einen Abschnitt mit verschiedenen Szenarien zum Datenverkehr, in denen Schritt für Schritt erläutert wird, wie der Datenverkehr durch die verschiedenen Sicherheitsstufen in der DMZ geleitet wird. Der Abschnitt „Referenzen“ schließlich enthält den vollständigen Vorlagencode sowie Anweisungen zum Aufbau dieser Umgebung, damit Sie verschiedene Szenarien testen und ausprobieren können. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Eingehende DMZ mit NSG][1]

## <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel umfasst ein Abonnement folgende Ressourcen:

* Eine einzelne Ressourcengruppe
* Ein virtuelles Netzwerk mit zwei Subnetzen: „FrontEnd“ und „BackEnd“
* Eine Netzwerksicherheitsgruppe, die auf beide Subnetze angewendet wird
* Eine Windows Server-Instanz, die einen Anwendungswebserver darstellt ("IIS01")
* Zwei Windows Server-Instanzen, die Back-End-Anwendungsserver darstellen („AppVM01“, „AppVM02“)
* Eine Windows Server-Instanz, die einen DNS-Server darstellt ("DNS01")
* Eine öffentliche IP-Adresse, die mit dem Anwendungswebserver verknüpft ist

Der Abschnitt „Referenzen“ enthält einen Link zu einer Azure Resource Manager-Vorlage, die die in diesem Beispiel beschriebene Umgebung erstellt. Die Erstellung der virtuellen Computer und Netzwerke wird zwar durch die Beispielvorlage ausgeführt, aber dies wird in diesem Dokument nicht im Einzelnen beschrieben. 

**So erstellen Sie diese Umgebung** (ausführliche Anweisungen finden Sie im Abschnitt „Referenzen“ dieses Dokuments):

1. Stellen Sie die Azure Resource Manager-Vorlage unter [Azure-Schnellstartvorlagen][Template] bereit.
2. Installieren Sie die Beispielanwendung unter [Beispielanwendung zur Verwendung in Umgebungen mit Sicherheitsbegrenzung][SampleApp].

>[!NOTE]
>Für eine RDP-Verbindung mit Back-End-Servern in dieser Instanz wird der IIS-Server als „Sprungplattform“ verwendet. Stellen Sie erst eine RDP-Verbindung mit dem IIS-Server her und dann eine RDP-Verbindung des IIS-Servers mit dem Back-End-Server. Alternativ können Sie eine öffentliche IP-Adresse mit jeder Server-NIC verknüpfen, um einfacher RDP-Datenverkehr zu realisieren.
> 
>

In den nächsten Abschnitten werden anhand der wichtigsten Zeilen der Azure Resource Manager-Vorlage detailliert die Netzwerksicherheitsgruppe sowie deren Funktionsweise in diesem Beispiel beschrieben.

## <a name="network-security-groups-nsg"></a>Netzwerksicherheitsgruppen
In diesem Beispiel wird eine Netzwerksicherheitsgruppe (NSG) erstellt und dann mit sechs Regeln geladen. 

>[!TIP]
>Im Allgemeinen sollten Sie zuerst die spezifischeren Regeln zum Zulassen von Aktionen und danach die allgemeineren Regeln zum Ablehnen von Aktionen erstellen. Die zugewiesene Priorität bestimmt, welche Regeln zuerst ausgewertet werden. Sobald eine Regel auf den Datenverkehr zutrifft, werden keine weiteren Regeln ausgewertet. NSG-Regeln können sowohl in eingehender als auch in ausgehender Richtung zutreffen (aus Perspektive des Subnetzes).
>
>

Folgende Regeln werden deklarativ für eingehenden Datenverkehr erstellt:

1. Interner DNS-Datenverkehr (Port 53) wird zugelassen.
2. RDP-Datenverkehr (Port 3389) aus dem Internet zu jedem virtuellen Computer wird zugelassen.
3. HTTP-Datenverkehr (Port 80) aus dem Internet zum Webserver (IIS01) wird zugelassen.
4. Jeglicher Datenverkehr (alle Ports) von IIS01 zu AppVM1 wird zugelassen.
5. Jeglicher Datenverkehr (alle Ports) aus dem Internet in das gesamte VNet (beide Subnetze) wird abgelehnt.
6. Jeglicher Datenverkehr (alle Ports) vom Front-End-Subnetz zum Back-End-Subnetz wird abgelehnt.

Wenn diese Regeln an jedes Subnetz gebunden sind und eine HTTP-Anforderung aus dem Internet an den Webserver eingeht, gilt sowohl Regel 3 (Zulassen) als auch Regel 5 (Verweigern). Da Regel 3 aber eine höhere Priorität hat, wird nur diese Regel angewendet, und Regel 5 wird nicht berücksichtigt. Aus diesem Grund wird die HTTP-Anforderung für den Webserver als zulässig eingestuft. Falls versucht würde, denselben Datenverkehr an den DNS01-Server zu senden, würde Regel 5 (Ablehnen) zuerst angewendet, der Datenverkehr also nicht an den Server übergeben. Mit Regel 6 (Verweigern) wird die Kommunikation des Front-End-Subnetzes mit dem Back-End-Subnetz blockiert (mit Ausnahme von zulässigem Datenverkehr in den Regeln 1 und 4). Dieser Regelsatz dient dem Schutz des Back-End-Netzwerks, falls ein Angreifer die Webanwendung am Front-End attackiert. Der Angreifer hat dann nur beschränkten Zugriff auf das „geschützte“ Back-End-Netzwerk (nur auf Ressourcen, die auf dem Server AppVM01 verfügbar gemacht werden).

Es gibt eine Standardregel für ausgehenden Datenverkehr, die das Senden von Datenverkehr an das Internet zulässt. In diesem Beispiel wird ausgehender Datenverkehr zugelassen, und es werden keine ausgehenden Regeln geändert. Um die Sicherheitsrichtlinie zum Sperren des Datenverkehrs in beiden Richtungen anzuwenden, ist benutzerdefiniertes Routing (User Defined Routing, UDR) erforderlich. Dies wird in Beispiel 3 auf der [Security Boundary Best Practices Page][HOME] (Seite „Sicherheitsbegrenzung – Best Practices“) läutert.

Jede Regel wird wie folgt ausführlicher erläutert:

1. Eine Netzwerksicherheitsgruppen-Ressource muss zum Halten der Regeln instanziiert werden:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. Die erste Regel in diesem Beispiel lässt DNS-Datenverkehr zwischen allen internen Netzwerken und dem DNS-Server im Back-End-Subnetz zu. Die Regel weist einige wichtige Parameter auf:
  * „destinationAddressPrefix“ – Regeln können einen besonderen, „Standardtag“ genannten Adresspräfixtyp verwenden; diese Tags sind vom System bereitgestellte Bezeichner, mit denen auf einfache Weise eine größere Kategorie von Adresspräfixen adressiert werden kann. Diese Regel verwendet das Standardtag „Internet“, um alle Adressen außerhalb des VNets anzugeben. Andere Präfixbezeichnungen sind VirtualNetwork und AzureLoadBalancer.
  * „Direction“ gibt an, in welcher Datenverkehrsflussrichtung diese Regel wirksam wird. Die Richtung kann aus der Perspektive des Subnetzes oder virtuellen Computers bestimmt werden (je nachdem, wie diese NSG gebunden ist). Wenn die Richtung „eingehend“ lautet und Datenverkehr an das Subnetz gesendet wird, wird die Regel angewendet. Auf Datenverkehr, der das Subnetz verlässt, wirkt sich die Regel dagegen nicht aus.
  * Der Parameter „Priority“ legt die Reihenfolge fest, in der ein Datenverkehrsfluss ausgewertet wird. Je niedriger die Nummer ist, desto höher ist die Priorität. Wenn eine Regel auf einen bestimmten Datenverkehrsfluss angewendet wird, werden keine weiteren Regeln verarbeitet. Wenn daher eine Regel mit Priorität 1 Datenverkehr zulässt und eine Regel mit Priorität 2 Datenverkehr ablehnt und beide Regeln auf einen Datenverkehrsfluss zutreffen, wird dieser Datenverkehr zugelassen, da Regel 1 mit der höheren Priorität greift und keine weiteren Regeln angewendet werden.
  * Der Parameter „Access“ gibt an, ob Datenverkehr, auf den diese Regel zutrifft, blockiert („Deny“) oder zugelassen („Allow“) wird.

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. Diese Regel lässt RDP-Datenverkehr aus dem Internet an den RDP-Port auf jedem Server im gebundenen Subnetz zu. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Diese Regel lässt zu, dass eingehender Internetdatenverkehr den Webserver erreicht. Diese Regel ändert das Routingverhalten nicht. Die Regel lässt nur Datenverkehr für IIS01 durch. Wenn also eingehender Datenverkehr aus dem Internet den Webserver als Ziel hat, lässt diese Regel den Datenverkehr zu und verhindert die Auswertung weiterer Regeln. (In der Regel mit Priorität 140 wird sämtlicher eingehender Datenverkehr aus dem Internet blockiert.) Wenn Sie nur HTTP-Datenverkehr verarbeiten, kann diese Regel weiter eingeschränkt werden und nur Zielport 80 zulassen.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Diese Regel lässt Datenverkehr vom IIS01-Server an den AppVM01-Server zu. Eine spätere Regel blockiert sämtlichen Datenverkehr vom Front-End zum Back-End. Wenn der Port bekannt ist, sollte dieser der Regel hinzugefügt werden, um deren Funktionalität zu verbessern. Wenn der IIS-Server z. B. nur Daten an SQL Server auf AppVM01 sendet, sollte der Zielportbereich von "*" (beliebig) in 1433 (dem SQL-Port) geändert werden. So bietet AppVM01 eine geringere Angriffsfläche bei Angriffen auf die Webanwendung.

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Dies Regel lehnt Datenverkehr aus dem Internet an alle Server im Netzwerk ab. Mit den Regeln mit Priorität 110 und 120 wird nur eingehender Datenverkehr aus dem Internet an die Firewall- und RDP-Ports von Servern zugelassen, jeglicher anderer Datenverkehr wird blockiert. Diese Regel ist eine „Fail-safe“-Regel, die alle unerwarteten Datenflüsse blockiert.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. Die letzte Regel lehnt Datenverkehr vom Front-End-Subnetz zum Back-End-Subnetz ab. Da es sich um eine eingehende Regel handelt, wird Datenverkehr in umgekehrter Richtung (vom Back-End zum Front-End) zugelassen.

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Datenverkehrsszenarien
#### <a name="allowed-internet-to-web-server"></a>(*Zugelassen*) Internet an Webserver
1. Ein Internetbenutzer fordert eine HTTP-Seite von der öffentlichen IP-Adresse der NIC an, die mit der IIS01-NIC verknüpft ist.
2. Die öffentliche IP-Adresse übergibt Datenverkehr an das VNet in Richtung IIS01 (Webserver).
3. Das Front-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. NSG-Regel 1 (DNS) trifft nicht zu, weiter zur nächsten Regel.
  2. NSG-Regel 2 (RDP) trifft nicht zu, weiter zur nächsten Regel.
  3. NSG-Regel 3 (Internet an IIS01) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
4. Datenverkehr trifft an interner IP-Adresse des Webservers IIS01 ein (10.0.1.5).
5. IIS01 lauscht auf Webdatenverkehr, empfängt diese Anforderung und beginnt mit der Verarbeitung der Anforderung.
6. IIS01 fragt Informationen von SQL Server unter AppVM01 ab.
7. Keine ausgehenden Regeln im Front-End-Subnetz, Datenverkehr wird zugelassen.
8. Das Back-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. NSG-Regel 1 (DNS) trifft nicht zu, weiter zur nächsten Regel.
  2. NSG-Regel 2 (RDP) trifft nicht zu, weiter zur nächsten Regel.
  3. NSG-Regel 3 (Internet an Firewall) trifft nicht zu, weiter zur nächsten Regel.
  4. NSG-Regel 4 (IIS01 an AppVM01) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
9. AppVM01 empfängt die SQL-Abfrage und antwortet.
10. Da es keine ausgehenden Regeln im Back-End-Subnetz gibt, wird die Antwort zugelassen.
11. Das Front-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. Es gibt keine NSG-Regel für eingehenden Datenverkehr vom Back-End- zum Front-End-Subnetz, daher trifft keine der NSG-Regeln zu.
  2. Da die standardmäßige Systemregel Datenverkehr zwischen Subnetzen zulässt, wird der Datenverkehr zugelassen.
12. Der IIS-Server empfängt die SQL-Antwort, erstellt die HTTP-Antwort und sendet sie an den Anforderer.
13. Da im Front-End-Subnetz keine ausgehenden Regeln vorhanden sind, wird die Antwort zugelassen, und der Internetbenutzer empfängt die angeforderte Webseite.

#### <a name="allowed-rdp-to-iis-server"></a>(*Zugelassen*) RDP an IIS-Server
1. Ein Serveradministrator fordert im Internet eine RDP-Sitzung mit IIS01 über die öffentliche IP-Adresse der NIC an, die mit der IIS01-NIC verknüpft ist (diese öffentliche IP-Adresse kann über das Portal oder PowerShell abgerufen werden).
2. Die öffentliche IP-Adresse übergibt Datenverkehr an das VNet in Richtung IIS01 (Webserver).
3. Das Front-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. NSG-Regel 1 (DNS) trifft nicht zu, weiter zur nächsten Regel.
  2. NSG-Regel 2 (RDP) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
4. Da keine ausgehenden Regeln vorhanden sind, werden Standardregeln angewendet, und der zurückkommende Datenverkehr wird zugelassen.
5. Die RDP-Sitzung wird aktiviert.
6. IIS01 fordert zur Eingabe von Benutzername und Kennwort auf.

>[!NOTE]
>Für eine RDP-Verbindung mit Back-End-Servern in dieser Instanz wird der IIS-Server als „Sprungplattform“ verwendet. Stellen Sie erst eine RDP-Verbindung mit dem IIS-Server her und dann eine RDP-Verbindung des IIS-Servers mit dem Back-End-Server.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Zugelassen*) Webserver-DNS-Lookup auf DNS-Server
1. Der Webserver IIS01 benötigt einen Datenfeed von "www.data.gov", muss jedoch die Adresse auflösen.
2. Die Netzwerkkonfiguration für das VNet listet DNS01 (10.0.2.4 im Back-End-Subnetz) als primären DNS-Server auf, IIS01 sendet die DNS-Anforderung an DNS01.
3. Keine ausgehenden Regeln im Front-End-Subnetz, Datenverkehr wird zugelassen.
4. Das Back-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  * NSG-Regel 1 (DNS) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
5. DNS-Server empfängt Anforderung.
6. Die Adresse ist nicht im DNS-Server zwischengespeichert, daher fragt der DNS-Server die Adresse bei einem DNS-Stammserver im Internet ab.
7. Keine ausgehenden Regeln im Back-End-Subnetz, Datenverkehr wird zugelassen.
8. Der DNS-Server im Internet antwortet. Da diese Sitzung intern initiiert wurde, wird die Antwort zugelassen.
9. Der DNS-Server speichert die Antwort zwischen und gibt die Antwort auf die ursprüngliche Anforderung zurück an IIS01.
10. Keine ausgehenden Regeln im Back-End-Subnetz, Datenverkehr wird zugelassen.
11. Das Front-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. Es gibt keine NSG-Regel für eingehenden Datenverkehr vom Back-End- zum Front-End-Subnetz, daher trifft keine der NSG-Regeln zu.
  2. Da die standardmäßige Systemregel Datenverkehr zwischen Subnetzen zulässt, wird der Datenverkehr zugelassen.
12. IIS01 empfängt die Antwort von DNS01.

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Zugelassen*) Webserver-Zugriffsdatei unter AppVM01
1. IIS01 fordert eine Datei auf AppVM01 an.
2. Keine ausgehenden Regeln im Front-End-Subnetz, Datenverkehr wird zugelassen.
3. Das Back-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. NSG-Regel 1 (DNS) trifft nicht zu, weiter zur nächsten Regel.
  2. NSG-Regel 2 (RDP) trifft nicht zu, weiter zur nächsten Regel.
  3. NSG-Regel 3 (Internet an IIS01) trifft nicht zu, weiter zur nächsten Regel.
  4. NSG-Regel 4 (IIS01 an AppVM01) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
4. AppVM01 empfängt die Anforderung und antwortet mit der Datei (autorisierter Zugriff vorausgesetzt).
5. Da es keine ausgehenden Regeln im Back-End-Subnetz gibt, wird die Antwort zugelassen.
6. Das Front-End-Subnetz beginnt mit der Verarbeitung der eingehenden Regel:
  1. Es gibt keine NSG-Regel für eingehenden Datenverkehr vom Back-End- zum Front-End-Subnetz, daher trifft keine der NSG-Regeln zu.
  2. Da die standardmäßige Systemregel Datenverkehr zwischen Subnetzen zulässt, wird der Datenverkehr zugelassen.
7. Der IIS-Server empfängt die Datei.

#### <a name="denied-rdp-to-backend"></a>(*Verweigert*) RDP an Back-End
1. Ein Internetbenutzer versucht, eine RDP-Verbindung mit Server AppVM01 herzustellen.
2. Da keine öffentlichen IP-Adressen mit der NIC dieses Servers verknüpft sind, würde dieser Datenverkehr nie in das VNet eingehen und den Server nicht erreichen.
3. Sollte jedoch aus beliebigem Grund eine öffentliche IP-Adresse aktiviert sein, würde NSG-Regel 2 (RDP) diesen Datenverkehr zulassen.

>[!NOTE]
>Für eine RDP-Verbindung mit Back-End-Servern in dieser Instanz wird der IIS-Server als „Sprungplattform“ verwendet. Stellen Sie erst eine RDP-Verbindung mit dem IIS-Server her und dann eine RDP-Verbindung des IIS-Servers mit dem Back-End-Server.
>
>

#### <a name="denied-web-to-backend-server"></a>(*Verweigert*) Web an Back-End-Server
1. Ein Internetbenutzer versucht, auf eine Datei auf AppVM01 zuzugreifen.
2. Da keine öffentlichen IP-Adressen mit der NIC dieses Servers verknüpft sind, würde dieser Datenverkehr nie in das VNet eingehen und den Server nicht erreichen.
3. Sollte aus beliebigem Grund eine öffentliche IP-Adresse aktiviert sein, würde NSG-Regel 5 (Internet an VNet) diesen Datenverkehr blockieren.

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Verweigert*) Web-DNS-Lookup auf DNS-Server
1. Ein Internetbenutzer versucht, einen internen DNS-Eintrag auf DNS01 zu suchen.
2. Da keine öffentlichen IP-Adressen mit der NIC dieses Servers verknüpft sind, würde dieser Datenverkehr nie in das VNet eingehen und den Server nicht erreichen.
3. Sollte aus beliebigem Grund eine öffentliche IP-Adresse aktiviert sein, würde NSG-Regel 5 (Internet an VNet) diesen Datenverkehr blockieren (Hinweis: Regel 1 (DNS) würde nicht gelten, da die Quelladresse der Anforderung das Internet ist und Regel 1 nur für das lokale VNet als Quelle gilt).

#### <a name="denied-sql-access-on-the-web-server"></a>(*Verweigert*) SQL-Zugriff auf den Webserver
1. Ein Internetbenutzer fordert SQL-Daten von IIS01 an.
2. Da keine öffentlichen IP-Adressen mit der NIC dieses Servers verknüpft sind, würde dieser Datenverkehr nie in das VNet eingehen und den Server nicht erreichen.
3. Wenn eine öffentliche IP-Adresse aus beliebigem Grund aktiviert wäre, würde das Front-End-Subnetz mit der Verarbeitung der eingehenden Regel beginnen:
  1. NSG-Regel 1 (DNS) trifft nicht zu, weiter zur nächsten Regel.
  2. NSG-Regel 2 (RDP) trifft nicht zu, weiter zur nächsten Regel.
  3. NSG-Regel 3 (Internet an IIS01) trifft zu, Datenverkehr wird zugelassen, Regelverarbeitung wird beendet.
4. Datenverkehr trifft an interner IP-Adresse von IIS01 ein (10.0.1.5).
5. IIS01 lauscht nicht auf Port 1433, daher wird diese Anforderung nicht beantwortet.

## <a name="conclusion"></a>Zusammenfassung
Dieses Beispiel ist eine relativ einfache und direkte Möglichkeit, das Back-End-Subnetz von eingehendem Datenverkehr zu isolieren.

Weitere Beispiele und eine Übersicht über die Sicherheitsgrenzen des Netzwerks finden Sie [hier][HOME].

## <a name="references"></a>Referenzen
### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
Dieses Beispiel verwendet eine vordefinierte Azure Resource Manager-Vorlage in einem von Microsoft verwalteten und für die Community freigegebenen GitHub-Repository. Diese Vorlage kann direkt aus GitHub heraus bereitgestellt oder heruntergeladen und an Ihre Bedürfnisse angepasst werden. 

Die Hauptvorlage ist die Datei mit dem Namen „azuredeploy.json“. Diese Vorlage kann über PowerShell oder CLI (mit der zugehörigen „azuredeploy.parameters.json“-Datei) zum Bereitstellen dieser Vorlage gesendet werden. Erfahrungsgemäß ist die einfachste Möglichkeit die Verwendung der Schaltfläche „Deploy to Azure“ in GitHub auf der Seite „README.md“.

Um die Vorlage bereitzustellen, die dieses Beispiel mithilfe von GitHub und Azure-Portal erstellt, gehen Sie folgendermaßen vor:

1. Navigieren Sie in einem Browser zur [Vorlage][Template].
2. Klicken Sie auf die Schaltfläche „Deploy to Azure“ (oder „Visualize“, um eine grafische Darstellung dieser Vorlage zu sehen).
3. Geben Sie Speicherkonto, Benutzername und Kennwort in das Blatt „Parameter“ ein, und klicken Sie auf **OK**.
5. Erstellen Sie eine Ressourcengruppe für diese Bereitstellung (Sie können eine vorhandene verwenden, aber für optimale Ergebnisse sollten Sie eine neue verwenden).
6. Ändern Sie ggf. die „Abonnement“- und „Speicherort“-Einstellungen für das VNet.
7. Klicken Sie auf **Prüfen Sie die rechtlichen Bedingungen.**, lesen Sie die Bedingungen, und klicken Sie auf **Kaufen**, wenn Sie diesen zustimmen.
8. Klicken Sie auf **Erstellen**, um mit der Bereitstellung dieser Vorlage zu beginnen.
9. Sobald die Bereitstellung erfolgreich abgeschlossen ist, navigieren Sie zu der Ressourcengruppe, die Sie für diese Bereitstellung erstellt haben, um die intern konfigurierten Ressourcen zu sehen.

>[!NOTE]
>Diese Vorlage ermöglicht nur eine RDP-Verbindung mit dem IIS01-Server (die öffentliche IP-Adresse für IIS01 finden Sie im Portal). Für eine RDP-Verbindung mit Back-End-Servern in dieser Instanz wird der IIS-Server als „Sprungplattform“ verwendet. Stellen Sie erst eine RDP-Verbindung mit dem IIS-Server her und dann eine RDP-Verbindung des IIS-Servers mit dem Back-End-Server.
>
>

Um diese Bereitstellung zu entfernen, löschen Sie die Ressourcengruppe, und dann werden alle untergeordneten Ressourcen ebenfalls gelöscht.

#### <a name="sample-application-scripts"></a>Beispielanwendungsskripts
Sobald die Vorlage erfolgreich ausgeführt wurde, können Sie den Webserver und den App-Server mit einer einfachen Webanwendung einrichten, um Tests mit dieser DMZ-Konfiguration zu ermöglichen. Um eine Beispielanwendung für dieses und weitere DMZ-Beispiele zu installieren, finden Sie eine Anwendung dieser Art unter folgendem Link: [Beispielanwendungsskript][SampleApp].

## <a name="next-steps"></a>Nächste Schritte

* Bereitstellen dieses Beispiel
* Erstellen der Beispielanwendung
* Testen unterschiedlicher Datenverkehrsflüsse durch diese DMZ

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Eingehende DMZ mit NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md


<!--HONumber=Jan17_HO1-->


