---
title: Verwenden von Lastenausgleichsdiensten in der Azure-Cloud | Microsoft Docs
description: 'In diesem Tutorial erfahren Sie, wie Sie ein Szenario mithilfe des Lastenausgleichportofolios von Azure erstellen: Traffic Manager, Application Gateway und Load Balancer'
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 90918f6bb23f0f4e3c009506e1b12c028c45f02b

---

# <a name="using-load-balancing-services-in-the-azure-cloud"></a>Verwenden von Lastenausgleichsdiensten in der Azure-Cloud

## <a name="introduction"></a>Einführung

Microsoft Azure bietet zahlreiche Dienste für das Verwalten der Verteilung von Netzwerkdatenverkehr und dem Lastenausgleich. Sie können diese Dienste Ihren Anforderungen entsprechend einzeln verwenden, oder die Methoden kombinieren und die optimale Lösung erstellen.

In diesem Tutorial definieren wir zuerst einen Anwendungsfall beim Kunden, und finden heraus, wie dieser stabiler und leistungsstärker gemacht werden kann, indem wir das folgende Lastenausgleichportofolio von Azure verwenden: Traffic Manager, Application Gateway und Load Balancer. Anschließend erhalten Sie Schritt-für-Schritt-Anleitungen zum Erstellen einer geografisch redundanten Bereitstellung, die Datenverkehr an mehrere virtuelle Computer verteilt, und mit der verschiedene Anforderungstypen verwaltet werden können.

Auf konzeptioneller Ebene spielt jeder dieser Dienste eine wichtige Rolle in der Hierarchie von Lastenausgleichen.

1. **Traffic Manager** enthält einen globalen DNS-Lastenausgleich.  Der Dienst betrachtet eingehende DNS-Anforderungen, und antwortet mit einem fehlerfrei funktionierenden Endpunkt. Das geschieht in Übereinstimmung mit der Routingrichtlinie, die der Kunde ausgewählt hat. Die verschiedenen Optionen für Routingmethoden sind „performance“ (Leistung) zum Weiterleiten des Anfragenden zum nächstgelegenen Endpunkt (in Bezug auf Latenz), „priority“ (Priorität) zum Weiterleiten von Datenverkehr zu einem Endpunkt, der über andere Endpunkte als Backup verfügt, sowie die gewichtete Methode „round robin“ (Gewichtet), die Datenverkehr je nach zugewiesener Gewichtung an jeden Endpunkt verteilt. Der Client stellt eine direkte Verbindung zu diesem Endpunkt her. Azure Traffic Manager erkennt, wenn ein Endpunkt fehlerhaft ist, und leitet den Client an eine andere fehlerfreie Instanz weiter. Weitere Informationen zu diesem Dienst finden Sie in der [Dokumentation zu Azure Traffic Manager](traffic-manager-overview.md).
2. **Application Gateway** verfügt über einen ADC (Application Delivery Controller) als Dienst und bietet damit verschiedene Lastenausgleichsfunktionen auf Ebene 7 für Ihre Anwendung. So wird Kunden die Optimierung der Webfarmproduktivität ermöglicht, indem die CPU-intensive SSL-Beendigung auf das Application Gateway verlagert wird. Andere Routingfunktionen für Ebene 7 umfassen Roundrobin-Verteilung des eingehenden Datenverkehrs, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis, und Möglichkeit zum Hosten von mehreren Websites hinter einem einzelnen Application Gateway. Application Gateway kann als Gateway mit Internetanbindung, rein internes Gateway oder als Kombination dieser beiden Optionen konfiguriert werden. Application Gateway wird vollständig über Azure verwaltet und ist skalierbar und hoch verfügbar. Die Anwendung umfasst viele Diagnose- und Protokollierungsfunktionen zur Verbesserung der Verwaltbarkeit.
3. **Load Balancer** ist ein wesentlicher Bestandteil des Azure SDN-Stapels. Load Balancer ist äußerst leistungsfähig und bietet Lastenausgleichsdienste der Ebene 4 mit niedriger Latenz für alle UDP- und TCP-Protokolle.  Der Dienst verwaltet eingehende und ausgehende Verbindungen.  Sie können öffentliche und interne Lastenausgleichs-Endpunkte konfigurieren und Regeln definieren, um eingehende Verbindungen zu Back-End-Pool-Zielen mit TCP und HTTP Integritätstest-Optionen zuzuordnen, damit die Dienstverfügbarkeit verwaltet werden kann.

## <a name="scenario"></a>Szenario

In diesem Beispielszenario verwenden wir eine einfache Website, die zwei Inhaltstypen unterstützt: Images und dynamisch dargestellte Websites. Diese Website muss geografisch redundant sein, und muss von Benutzern von ihrem nächstgelegenen Standort aus (niedrigste Latenz) besucht werden können. Der Anwendungsentwickler hat entschieden, dass alle URLs, die dem Muster /images/* entsprechen, von einem dedizierten Pool von VMs aus bereitgestellt werden. Dies unterscheidet sich zum Rest der Webfarm.

Außerdem muss der standardmäßige VM-Pool, der den dynamischen Inhalt bereitstellt, mit einer Back-End-Datenbank kommunizieren, die auf einem Cluster mit hoher Verfügbarkeit gehostet wird. Die gesamte Bereitstellung erfolgt durch Azure Resource Manager.

Das Verwenden von Traffic Manager, Application Gateway und Load Balancer ermöglicht es der Website, diese Entwurfsziele zu erreichen:

1. **Multi-Geo Redundanz**: Durch die Verwendung von Traffic Manager wird der Datenverkehr bei Ausfall einer Region nahtlos zur nächsten passenden Region weitergeleitet. Es ist kein Eingreifen durch den Anwendungsbesitzer notwendig.
2. **Reduzierte Latenz**: Nachdem der Kunde durch Azure Traffic Manager automatisch zur nächstgelegenen Region weitergeleitet wird, gibt es geringere Latenz beim Anfordern der Inhalte der Website.
3. **Unabhängige Skalierbarkeit**: Dadurch, dass der Workload der Webanwendung nach Inhaltsart getrennt ist, kann der Anwendungsbesitzer die Anforderungsworkloads unabhängig voneinander skalieren. Durch Application Gateway wird sichergestellt, dass der Verkehr zu den richtigen Pools geleitet wird. Dies hängt von den angegebenen Regeln und dem Zustand der Anwendungsintegrität ab.
4. **Interner Lastenausgleich**: Nachdem sich Load Balancer vor dem Cluster mit hoher Verfügbarkeit befindet, steht der Anwendung nur der jeweilige aktive und fehlerfreie Endpunkt für eine Datenbank zur Verfügung.  Darüber hinaus kann ein Datenbankadministrator den Workload durch das clusterweite Verteilen aktiver und passiver Replikate optimieren. Dies geschieht unabhängig von der Front-End-Anwendung.  Load Balancer liefert Verbindungen zum Cluster mit hoher Verfügbarkeit und stellt sicher, dass nur fehlerfreie Datenbanken Verbindungsanforderungen erhalten.

Das folgende Diagramm zeigt die Architektur dieses Szenarios:

![Bild, das das Szenario (Diagramm) zeigt](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Dieses Beispiel zeigt nur eine von vielen möglichen Konfigurationen, die Azure für Lastenausgleichsdienste bietet. Azure Traffic Manager, Application Gateway und Load Balancer können kombiniert werden, um alle Ihre Anforderungen an den Lastenausgleich gerecht zu erfüllen. Wenn beispielsweise die SSL-Auslagerung oder Verarbeitung der Ebene 7 nicht notwendig ist, kann Load Balancer anstatt Application Gateway verwendet werden.

## <a name="setting-up-the-load-balancing-stack"></a>Einrichten des Lastenausgleichsstapels

### <a name="step-1-create-a-traffic-manager-profile"></a>Schritt 1: Erstellen eines Traffic Manager-Profils

1. Wechseln Sie zum Azure-Portal, klicken Sie auf **Neu**, und suchen Sie im Marketplace nach „Traffic Manager-Profil“
2. Füllen Sie auf dem Blatt „Traffic Manager-Profil erstellen“ die grundlegenden Informationen zum Erstellen eines Traffic Manager-Profils aus:

   * **Name** – Geben Sie Ihren Traffic Manager-Profilen einen DNS-Präfixnamen
   * **Routingmethode** – Wählen Sie die Richtlinie für die Routingmethode für Datenverkehr. Weitere Informationen zu den Methoden finden Sie unter [Informationen zu Traffic Manager-Routingmethoden für Datenverkehr](traffic-manager-routing-methods.md)
   * **Abonnement** – das Abonnement, das das Profil enthält
   * **Ressourcengruppe** – die Ressourcengruppe, die das Traffic Manager-Profil enthalten soll. Es kann eine neue oder eine vorhandene Ressourcengruppe sein
   * **Standort der Ressourcengruppe** – Der Traffic Manager-Dienst ist global und nicht an einen Standort gebunden. Es muss allerdings eine Region für die Gruppe angegeben werden, in der die Metadaten gespeichert werden, die dem Traffic Manager-Profil zugeordnet sind. Dieser Standort hat keine Auswirkung auf die Laufzeitverfügbarkeit des Profils.

3. Klicken Sie zum Generieren des Traffic Manager-Profils auf **Erstellen**

    ![erstellen eines traffic manager-profils](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>Schritt 2: Erstellen des Anwendungsgateways

1. Navigieren Sie zum Azure-Portal, und klicken Sie im linken Menü auf **Neu** > **Netzwerk** > ** Application Gateway**
2. Geben Sie anschließend die allgemeinen Informationen zum Anwendungsgateway an. Wenn Sie fertig sind, klicken Sie auf OK. Folgende Informationen werden für die Grundeinstellungen benötigt:

   * **Name** : der Name für das Anwendungsgateway.
   * **SKU-Größe**: Diese Einstellung gibt die Größe des Anwendungsgateways an. Sie haben die Wahl zwischen drei Größen: klein, mittel und groß.
   * **Anzahl von Instanzen**: Die Anzahl von Instanzen. Dieser Wert muss eine Zahl zwischen 2 und 10 sein.
   * **Ressourcengruppe**: Die Ressourcengruppe, zu der das Anwendungsgateway gehört. Hierbei kann es sich um eine vorhandene oder um eine neue Ressourcengruppe handeln.
   * **Standort**: Die Region für das Anwendungsgateway. Dieser Standort ist mit dem Standort der Ressourcengruppe identisch. Der Standort ist wichtig, da das virtuelle Netzwerk und die öffentliche IP den gleichen Standort wie das Gateway aufweisen müssen.

3. Definieren Sie als nächstes das virtuelle Netzwerk, Subnetz, die Front-End-IP, und das Listenkonfigurationselement für das Anwendungsgateway. In diesem Szenario ist die Front-End-IP-Adresse **Öffentlich**, um das spätere Hinzufügen als Endpunkt zum Traffic Manager-Profil zu ermöglichen.
4. Die nächste Einstellung für das Anwendungsgateway, die Sie konfigurieren müssen, ist die Listenerkonfiguration. Wenn HTTP verwendet wird, ist die Konfiguration vollständig, und Sie können auf **OK** klicken. Um HTTPS zu verwenden, sind weitere Konfigurationen erforderlich. Weitere Informationen finden Sie unter [Erstellen eines Anwendungsgateways](../application-gateway/application-gateway-create-gateway-portal.md) ab Schritt 9.

#### <a name="configure-url-routing-for-application-gateways"></a>Konfigurieren des URL-Routings für Anwendungsgateways

Ein Anwendungsgateway, das mit einer pfadbasierten Regel konfiguriert wird, benötigt bei der Auswahl des Back-End-Pools ein Pfadmuster der angefragten URL zusätzlich zur Roundrobin-Verteilung. In diesem Szenario fügen wir eine pfadbasierte Regel hinzu, um alle URLs mit "/images/\*" zum Imageserverpool zu leiten. Weitere Informationen zum Konfigurieren von URL-Pfad basiertem Routing für ein Anwendungsgateway finden Sie unter [Erstellen einer pfadbasierten Regel für ein Application Gateway über das Portal](../application-gateway/application-gateway-create-url-route-portal.md).

![Diagramm Webebene](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Wechseln Sie aus Ihrer Ressourcengruppe zur Instanz des Anwendungsgateways, das Sie in den vorherigen Schritten erstellt haben.
2. Wählen Sie in den Einstellungen **Back-End-Pools** aus, und klicken Sie auf „Hinzufügen“, um die VMs hinzuzufügen, die Sie den Back-End-Pools der Webebene zuordnen möchten.
3. Geben Sie auf dem Blatt „Back-End-Pool hinzufügen“ den Namen des Back-End-Pools und alle IP-Adressen der Computer ein, die sich im Pool befinden. In diesem Szenario stellen wir eine Verbindung mit zwei Back-End-Serverpools von virtuellen Computern her.

    ![Anwendungsgateways hinzufügen Back-End-Pool](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Wählen Sie anschließend in den Einstellungen des Anwendungsgateways **Regeln** aus, und klicken Sie dann auf die Schaltfläche **Path-based (pfadbasiert)**, um eine neue Regel hinzuzufügen.

    ![Anwendungsgateways, Pfadregel hinzufügen](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Geben Sie auf dem Blatt „Pfadbasierte Regel hinzufügen“ die folgenden Informationen an, um die Regel zu konfigurieren:

   * Grundlegende Einstellungen:
     + **Name** – der Anzeigename für die Regel, auf die Sie über das Portal zugreifen können
     + **Listener** – der für die Regel verwendete Listener
     + **Standard-Back-End-Pool** – der Back-End-Pool, der für die Standardregel verwendet werden soll
     + **Standard HTTP-Einstellungen** – die HTTP-Einstellungen, die für die Standardregel verwendet werden soll
   * Pfadbasierte Regeln:
     + **Name** – der Anzeigename der pfadbasierten Regel
     + **Pfade** – die Pfadregel, die für das Weiterleiten von Datenverkehr verwendet wird.
     + **Back-End-Pool** – der Back-End-Pool, der innerhalb dieser Regel verwendet wird
     + **HTTP-Einstellungen** – die HTTP-Einstellungen, die innerhalb dieser Regel verwendet werden

     > [!IMPORTANT]
     > Pfade: Gültige Pfade müssen mit „/“ beginnen. Der Platzhalter „\*“ darf nur am Ende verwendet werden. Gültige Beispiele sind: /xyz, /xyz\*, oder /xyz/\*

     ![Anwendungsgateway Blatt Pfadregel hinzufügen](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>Schritt 3: Hinzufügen von Anwendungsgateways zu Traffic Manager-Endpunkten

In diesem Szenario wird Traffic Manager mit Instanzen von Application Gateway verbunden (wie in den vorherigen Schritten konfiguriert), die sich in verschiedenen Regionen befinden. Da die Anwendungsgateways jetzt konfiguriert sind, ist der nächste Schritt, sie mit Ihrem Traffic Manager-Profil zu verbinden.

1. Navigieren Sie zu Ihrer Instanz des Traffic Manager-Profils (indem Sie innerhalb Ihrer Ressourcengruppe eine Suche durchführen, oder nach dem Namen des Traffic Manager-Profils innerhalb von „Alle Ressourcen“ suchen).
2. Wählen Sie auf diesem Blatt **Endpunkte** aus, und dann **Hinzufügen**, um einen neuen Endpunkt hinzuzufügen.

    ![Traffic Manager - Endpunkt hinzufügen](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Füllen Sie auf dem Blatt „Endpunkt hinzufügen“ die Informationen zum Erstellen eines neuen Endpunkts aus

   * **Typ** – der Typ des Endpunkts für den Lastenausgleich. In diesem Szenerio handelt es sich um einen Azure-Endpunkt, nachdem wir ihn zu den oben konfigurierten Application Gateway-Instanzen hinzugefügt haben
   * **Name** – der Name des Endpunkts
   * **Zielressourcentyp** – Wählen Sie die öffentliche IP-Adresse aus, und wählen Sie in den folgenden Einstellungen „Zielressource“ die öffentliche IP des oben konfigurierten Anwendungsgateways

     ![Traffic Manager - Endpunkt hinzufügen](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Ab diesem Zeitpunkt können Sie Ihre Einrichtung überprüfen, indem Sie mit dem DNS Ihres Traffic Manager-Profils darauf zugreifen (in diesem Beispiel: TrafficManagerScenario.trafficmanager.net). Sie können Anforderungen erneut senden, VMs/Webserver hoch-/herunterfahren, die in verschiedenen Regionen erstellt wurden, und die Einstellungen für Ihr Traffic Manager-Profil ändern, um Ihre Einrichtung zu überprüfen.

### <a name="step-4-create-the-load-balancer"></a>Schritt 4: Erstellen des Lastenausgleichs

In diesem Szenario verteilt Load Balancer Verbindungen aus der Webebene zu den Datenbanken innerhalb eines Clusters mit hoher Verfügbarkeit.

Wenn Ihr Cluster mit hoher Verfügbarkeit SQL AlwaysOn verwendet, finden Sie Schritt-für-Schritt-Anleitungen unter [Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Weitere Informationen zum Konfigurieren eines internen Lastenausgleichs finden Sie unter [Erstellen eines internen Lastenausgleichs über das Azure-Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)

1. Navigieren Sie zum Azure-Portal, und klicken Sie im linken Menü auf **Neu** > **Netzwerk** > ** Load Balancer**
2. Wählen Sie auf dem Blatt „Lastenausgleich erstellen“ einen Namen für Ihren Lastenausgleich aus
3. Legen Sie den **Typ** auf intern fest, und wählen Sie das entsprechende virtuelle Netzwerk und Subnetz aus, in dem sich der Lastenausgleich befinden soll
4. Wählen Sie unter **IP-Adresszuweisung** entweder Dynamisch oder Statisch aus
5. Wählen Sie als nächstes unter **Ressourcengruppe** die Ressourcengruppe für den Lastenausgleich aus
6. Wählen Sie unter **Location**(Standort) die entsprechende Region aus
7. Klicken Sie schließlich auf **Erstellen**, um den Lastenausgleich zu generieren

#### <a name="connect-backend-database-tier-to-load-balancer"></a>Verbinden der Back-End-Datenbankebene zum Lastenausgleich

1. Suchen Sie in der Ressourcengruppe nach dem Lastenausgleich, den Sie in den vorherigen Schritten erstellt haben.
2. Klicken Sie in den Einstellungen auf **Back-End-Pools**, und dann auf **Hinzufügen**, um einen neuen Back-End-Pool hinzuzufügen
3. Geben Sie auf dem Blatt „Back-End-Pool“ hinzufügen einen Namen für den Back-End-Pool ein
4. Von hier aus können Sie entweder einzelne Computer oder eine Verfügbarkeitsgruppe zum Back-End-Pool hinzufügen.

   ![Lastenausgleichsmodul zum Pool hinzufügen](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

#### <a name="configure-a-probe"></a>Konfigurieren eines Tests

1. Wählen Sie in den Einstellungen Ihres Lastenausgleichs den Punkt **Tests** und dann **Hinzufügen** aus, um einen neuen Test hinzuzufügen
2. Geben Sie auf dem Blatt „Test hinzufügen“ einen **Namen** für den Test ein
3. Wählen Sie **Protokoll** für den Test aus. Bei einer Datenbank ist es vermutlich besser, einen TCP-Test statt eines HTTP-Tests durchzuführen.   Um mehr über Load Balancer-Tests zu erfahren, rufen Sie [Load Balancer-Tests](../load-balancer/load-balancer-custom-probe-overview.md) auf.
4. Geben Sie als nächsten Schritt den **Port** Ihrer Datenbank ein, der beim Zugreifen auf den Test verwendet werden soll.
5. Geben Sie unter **Intervall** an, wie häufig die Anwendung getestet werden soll
6. Geben Sie unter **Fehlerhafter Schwellenwert** die Anzahl von aufeinanderfolgenden fehlgeschlagenen Tests ein, die auftreten muss, damit die Back-End-VM als fehlerhaft betrachtet wird.
7. Klicken Sie auf **OK**, um den Test zu erstellen

   ![Lastenausgleichstest](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

#### <a name="configure-load-balancing-rules"></a>Konfigurieren von Lastenausgleichsregeln

1. Wählen Sie unter den Einstellungen Ihres Lastenausgleichs die **Lastenausgleichsregeln** und **Hinzufügen** aus, um eine Regel zu erstellen
2. Geben Sie auf dem Blatt „Lastenausgleichsregel hinzufügen“ den **Namen** der Lastenausgleichsregel ein
3. Wählen Sie **Front-End-IP-Adresse des Lastenausgleichs**, **Protokoll** und **Port** aus
4. Geben Sie unter **Back-End-Port** den Port an, der im Back-End-Pool verwendet werden soll
5. Wählen Sie den **Back-End-Pool** und den **Test** aus, den Sie in den vorherigen Schritte erstellt haben, um die Regel anzuwenden
6. Wählen Sie unter **Sitzungspersistenz** aus, wie Sitzungen beibehalten werden sollen
7. Geben Sie unter **Leerlauftimeouts** die Anzahl von Minuten vor einem Leerlauftimeout an
8. Wählen Sie für **Floating IP** entweder Deaktiviert oder Aktiviert aus
9. Klicken Sie auf **OK**, um die Regel zu erstellen

### <a name="step-5-connect-web-tier-vms-to-load-balancer"></a>Schritt 5: Verbinden von VMs der Webebene mit Load Balancer

Jetzt konfigurieren wir die IP-Adresse und den Load Balancer-Front-End-Port für alle Datenbankverbindungen, der auf Ihren VMs der Webebene ausgeführt wird. Diese Konfiguration ist spezifisch für die Anwendung, die auf diesen VMs ausgeführt wird. Informationen zum Konfigurieren der IP-Zieladresse und des Ports finden Sie in der Dokumentation der Anwendung. Um die IP-Adresse des Front-End zu suchen, navigieren Sie zum Front-End-IP-Pool auf dem Blatt „Load Balancer Settings“ (Einstellungen für Load Balancer) Im Azure-Portal.

![Load Balancer Front-End-IP-Pool](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Nächste Schritte

* [Was ist Traffic Manager?](traffic-manager-overview.md)
* [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)
* [Übersicht über Azure Load Balancer](../load-balancer/load-balancer-overview.md)



<!--HONumber=Nov16_HO3-->


