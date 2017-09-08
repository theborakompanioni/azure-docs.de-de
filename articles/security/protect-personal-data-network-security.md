---
title: "Schützen personenbezogener Daten mit Azure-Netzwerksicherheitsfunktionen | Microsoft-Dokumentation"
description: "Schützen personenbezogener Daten mit Azure-Netzwerksicherheitsfunktionen"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 81aec340d595371a314ec0c1361f77a37c406a1e
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Schützen personenbezogener Daten mit Netzwerksicherheitsfunktionen: Azure Application Gateway und Netzwerksicherheitsgruppen

Dieser Artikel enthält Informationen und Verfahren zur Verwendung von Azure Application Gateway und Netzwerksicherheitsgruppen zum Schützen personenbezogener Daten.

Ein wichtiger Bestandteil einer mehrschichtigen Sicherheitsstrategie zur Gewährleistung des Datenschutzes personenbezogener Daten ist der Schutz vor verbreiteten Exploits für Sicherheitslücken, z.B. Einschleusung von SQL-Befehlen oder Cross-Site-Scripting. Das Fernhalten unerwünschten Netzwerkdatenverkehrs von Ihrem virtuellen Azure-Netzwerk trägt zum Schutz vor einer potenziellen Gefährdung sensibler Daten bei. Und Microsoft Azure stellt Ihnen die Tools zum Schutz Ihrer Daten vor Angreifern bereit.

## <a name="scenario"></a>Szenario

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer, der Adria und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben.

Das Unternehmen verwendet Microsoft Azure zum Speichern von Unternehmensdaten in der Cloud und zum Ausführen von Anwendungen auf virtuellen Computern, die diese Daten verarbeiten und auf sie zugreifen. Diese Daten umfassen personenbezogene Informationen, z.B. Namen, Adressen, Telefonnummern und Kreditkarteninformationen, in seiner globalen Kundendatenbank. Dies umfasst außerdem herkömmliche Informationen der Personalverwaltung wie Adressen, Telefonnummern, Steueridentifikationsnummern und andere Informationen zu Unternehmensmitarbeitern an allen Standorten. Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms, die personenbezogene Informationen enthält, um Beziehungen zu aktuellen und früheren Kunden nachzuverfolgen.

Unternehmensmitarbeiter greifen in den externen Geschäftsstellen des Unternehmens auf das Netzwerk zu. Und Reiseveranstalter weltweit haben Zugriff auf bestimmte Unternehmensressourcen und verwenden auf virtuellen Azure-Computern gehostete webbasierte Anwendungen zur Interaktion mit diesen Ressourcen.

## <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen muss personenbezogene Daten von Kunden und Mitarbeitern vor Angreifern schützen, die Schwachstellen in der Software ausnutzen, um bösartigen Code auszuführen, mit dem personenbezogene Daten offengelegt werden könnten, die in den cloudbasierten Anwendungen des Unternehmens gespeichert sind oder verwendet werden.

## <a name="company-goal"></a>Unternehmensziel

Unternehmensziel ist es, sicherzustellen, dass nicht autorisierte Personen nicht die Möglichkeit haben, durch Ausnutzen allgemeiner Sicherheitslücken auf virtuelle Azure-Netzwerke des Unternehmens und die dort vorhandenen Anwendungen und Daten zuzugreifen. 

## <a name="solutions"></a>Lösungen

Microsoft Azure bietet Sicherheitsmechanismen, die unerwünschten Datenverkehr auf virtuellen Azure-Netzwerken verhindern. Der eingehende und ausgehende Datenverkehr wird normalerweise über Firewalls gesteuert. In Azure können Sie das Application Gateway mit der Web Application Firewall und Netzwerksicherheitsgruppen (NSGs) verwenden, die als einfache verteilte Firewall fungieren. Mithilfe dieser Tools können Sie unerwünschten Netzwerkdatenverkehr erkennen und blockieren.

### <a name="application-gatewayweb-application-firewall"></a>Application Gateway/Web Application Firewall

Mit der Komponente [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) von [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) werden Webanwendungen geschützt, die zunehmend Ziele von böswilligen Angriffen sind, die allgemein bekannte Sicherheitslücken ausnutzen. Eine zentralisierte WAF kann Schutz vor Webangriffen bieten und vereinfacht die Sicherheitsverwaltung, ohne dass Änderungen an Anwendungen erforderlich sind.

Azure WAF verarbeitet verschiedene Angriffskategorien, darunter Einschleusung von SQL-Befehlen, Cross-Site-Scripting, Verstöße und Anomalien des HTTP-Protokolls, Bots, Crawler, Scanner, häufige Fehler bei der Anwendungskonfiguration, HTTP-Denial-of-Service, und andere verbreitete Angriffe, z.B. Befehlseinschleusung, HTTP Request Smuggling, HTTP Response Splitting und Remote File Inclusion. 

Sie können ein Anwendungsgateway mit WAF erstellen oder WAF einem vorhandenen Anwendungsgateway hinzufügen. In beiden Fällen ist für Azure Application Gateway ein eigenes Subnetz erforderlich.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Wie erstelle ich ein Anwendungsgateway mit WAF? 

Führen Sie folgende Schritte aus, um ein neues Anwendungsgateway mit aktivierter WAF zu erstellen:

1. Melden Sie sich beim Azure-Portal an, und klicken Sie im Bereich **Favoriten** des Portals auf **Neu**.

2. Klicken Sie auf dem Blatt **Neu** auf **Netzwerk**.

3. Klicken Sie auf **Application Gateway**.

4. Navigieren Sie zum Azure-Portal, und klicken Sie auf **Neu \> Netzwerk \> Application Gateway**.

   ![Erstellen von Anwendungsgateways](media/protect-netsec/app-gateway-01.png)

5. Geben Sie im dann angezeigten Blatt **Grundlagen** die Werte für die folgenden Felder ein: Name, Ebene (Standard oder WAF), SKU-Größe (Klein, Mittel oder Groß), Instanzanzahl (2 für hohe Verfügbarkeit), Abonnement, Ressourcengruppe und Standort.

6. Klicken Sie auf dem Blatt **Einstellungen**, das unter **Virtuelles Netzwerk** angezeigt wird, auf **Virtuelles Netzwerk auswählen**. Damit wird das Blatt „Virtuelles Netzwerk auswählen“ geöffnet.

7. Klicken Sie auf **Neu erstellen**, um das Blatt **Virtuelles Netzwerk erstellen** zu öffnen.

8. Geben Sie die folgenden Werte ein: Name, Adressraum, Subnetzname, Subnetzadressbereich. Klicken Sie auf **OK**.

9. Wählen Sie auf dem Blatt **Einstellungen** unter **Front-End-IP-Konfiguration** den IP-Adresstyp aus.

10. Klicken Sie auf **Öffentliche IP-Adresse auswählen** und dann auf **Neu erstellen**.

11. Übernehmen Sie den Standardwert, und klicken Sie auf **OK**.

12. Wählen Sie auf dem Blatt **Einstellungen** unter **Listenerkonfiguration** die Option „HTTP“ oder „HTTPS“ (unter **Protokoll**) aus. Für die Verwendung von HTTPS ist ein Zertifikat erforderlich.

13. Konfigurieren Sie die spezifischen Einstellungen für WAF: **Firewallstatus** (**Aktiviert**) und **Firewallmodus** (**Prävention**). Wenn Sie **Erkennung** als Modus auswählen, wird der Datenverkehr nur protokolliert.

14. Überprüfen Sie die Seite **Zusammenfassung**, und klicken Sie auf **OK**. Das Anwendungsgateway wird in der Warteschlange platziert und erstellt.

Nachdem das Anwendungsgateway erstellt wurde, können Sie im Portal zu diesem Gateway navigieren, um die Konfiguration fortzusetzen.

![Erstelltes Anwendungsgateway](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a>Wie füge ich WAF einer vorhandenen Anwendung hinzu?

Führen Sie folgende Schritte aus, um ein vorhandenes Anwendungsgateway zur Unterstützung von WAF im Modus „Prävention“ zu aktualisieren:

1. Klicken Sie im Azure-Portal im Bereich **Favoriten** auf **Alle Ressourcen**.

2. Klicken Sie im Blatt **Alle Ressourcen** auf das vorhandene Anwendungsgateway. 
>[!NOTE]
Hinweis: Wenn das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie den Namen im Feld „Nach Name filtern...“ eingeben und komfortabel auf die DNS-Zone zugreifen.
3. Klicken Sie auf **Web Application Firewall**, und aktualisieren Sie die Einstellungen für das Anwendungsgateway: **Upgrade auf WAF-Ebene** (aktiviert), **Firewallstatus** (aktiviert), **Firewallmodus** (Prävention). Sie müssen auch den Regelsatz und deaktivierte Regeln konfigurieren.

Ausführlichere Informationen zum Erstellen eines neuen Anwendungsgateways mit WAF und zum Hinzufügen von WAF zu einem vorhandenen Anwendungsgateway finden Sie unter [Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal).

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Eine [Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit [virtuellen Azure-Netzwerken](https://docs.microsoft.com/azure/virtual-network/) (VNET) verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen virtuellen Computern zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die Regeln für alle Ressourcen, die mit dem Subnetz verbunden sind. Der Datenverkehr kann weiter eingeschränkt werden, indem eine NSG außerdem einer VM oder NIC zugeordnet wird.

NSGs enthalten vier Eigenschaften: Name, Region, Ressourcengruppe und Regeln.
>[!Note]
Obwohl eine NSG in einer Ressourcengruppe enthalten ist, kann sie Ressourcen in beliebigen Ressourcengruppen zugeordnet werden, sofern die Ressource zu derselben Azure-Region wie die NSG gehört.

NSG-Regeln enthalten neun Eigenschaften: Name, Protokoll (TCP, UDP oder \*, das ICMP sowie UDP und TCP umfasst), Quellportbereich, Zielportbereich, Quelladresspräfix, Zieladresspräfix, Richtung (eingehend oder ausgehend), Priorität (zwischen 100 und 4096) und Zugriffstyp (zulassen oder ablehnen). Alle NSGs enthalten einen Satz von Standardregeln, die gelöscht oder durch die von Ihnen erstellten Regeln überschrieben werden können.

#### <a name="how-do-i-implement-nsgs"></a>Wie implementiere ich NSGs?

Die Implementierung von Netzwerksicherheitsgruppen erfordert Planung. Dabei müssen mehrere Entwurfsaspekte berücksichtigt werden. Dazu gehören die Beschränkung der Anzahl der NSGs pro Abonnement und der Regeln pro NSG, der Entwurf von VNETs und Subnetzen, spezielle Regeln, ICMP-Datenverkehr, Isolierung von Ebenen mit Subnetzen, Load Balancer usw.

Weitere Anweisungen zur Planung und Implementierung von NSGs sowie ein Beispielszenario für die Bereitstellung finden Sie unter [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Wie erstelle ich Regeln in einer NSG?

Führen Sie folgende Schritte aus, um Regeln für eingehenden Datenverkehr in einer vorhandenen NSG zu erstellen:

1. Klicken Sie auf **Durchsuchen** und dann auf **Netzwerksicherheitsgruppen**.

2. Klicken Sie in der Liste der NSGs auf **NSG-FrontEnd** und dann auf **Eingangssicherheitsregeln**.

3. Klicken Sie in der Liste der Eingangssicherheitsregeln auf **Hinzufügen**.

4. Geben Sie Werte in den folgenden Feldern ein: Name, Priorität, Quelle, Protokoll, Quellportbereich, Ziel, Zielportbereich und Aktion.

Nach wenigen Sekunden wird die neue Regel in der NSG angezeigt.

![Netzwerksicherheitsregeln](media/protect-netsec/inbound-security.png)

Weitere Anweisungen zum Erstellen von NSGs in Subnetzen, Erstellen von Regeln und Zuordnen einer NSG zu einem Front-End- und Back-End-Subnetz finden Sie unter [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="next-steps"></a>Nächste Schritte

[Azure-Netzwerksicherheit](https://azure.microsoft.com/blog/azure-network-security/)

[Bewährte Methoden für die Azure-Netzwerksicherheit](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Abrufen von Informationen zu einer Netzwerksicherheitsgruppe](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Web Application Firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)

