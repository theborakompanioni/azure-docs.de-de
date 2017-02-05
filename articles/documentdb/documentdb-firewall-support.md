---
title: "DocumentDB-Firewallunterstützung | Microsoft Docs"
description: "Erfahren Sie, wie IP-Access Control-Richtlinien für die Firewallunterstützung in Azure DocumentDB-Datenbankkonten verwendet werden."
keywords: "IP-Access Control, Firewallunterstützung"
services: documentdb
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ankshah; kraman
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eb3c5c2adbaedc4bfb1e68f26b88079aeabe50f5


---
# <a name="documentdb-firewall-support"></a>DocumentDB-Firewallunterstützung
Zum Sichern von in einem Azure DocumentDB-Datenbankkonto gespeicherten Daten bietet DocumentDB Unterstützung für ein auf einem Geheimnis basierendes [Autorisierungsmodell](https://msdn.microsoft.com/library/azure/dn783368.aspx), das einen starken hashbasierten Nachrichtenauthentifizierungscode (HMAC) nutzt. DocumentDB unterstützt nun neben dem auf einem Geheimnis basierenden Autorisierungsmodell durch Richtlinien gesteuerte IP-basierte Access Control für die eingehende Firewallunterstützung. Dieses Modell ähnelt den Firewallregeln eines herkömmlichen Datenbanksystems sehr und bietet zusätzliche Sicherheit für das DocumentDB-Datenbankkonto. Mit diesem Modell können Sie nun ein DocumentDB-Datenbankkonto so konfigurieren, dass es nur über eine genehmigte Gruppe von Computern und/oder Clouddiensten zugänglich ist. Für den Zugriff auf DocumentDB-Ressourcen über diese genehmigten Gruppen von Computern und Diensten muss der Aufrufer weiterhin ein gültiges Autorisierungstoken vorlegen.

## <a name="ip-access-control-overview"></a>Übersicht über die IP-Access Control
Standardmäßig ist ein DocumentDB-Datenbankkonto über das öffentliche Internet zugänglich, solange die Anforderung zusammen mit einem gültigen Autorisierungstoken erfolgt. Zum Konfigurieren der auf IP-Richtlinien basierten Access Control muss der Benutzer die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format angeben, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. Nachdem diese Konfiguration angewendet wurde, werden alle Anforderungen von Computern, die nicht in dieser Zulassungsliste enthalten sind, vom Server blockiert.  In der folgenden Abbildung ist der Verbindungsverarbeitungsablauf für die IP-basierte Access Control dargestellt.

![Abbildung zum Verbindungsprozess für die IP-basierte Access Control](./media/documentdb-firewall-support/documentdb-firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Verbindungen über Clouddienste
In Azure ist es sehr üblich, dass Clouddienste Servicelogik der mittleren Ebene mithilfe von DocumentDB hosten. Um den Zugriff auf ein DocumentDB-Datenbankkonto über einen Clouddienst zu ermöglichen, muss die öffentliche IP-Adresse des Clouddiensts der Liste der zulässigen IP-Adressen, die dem DocumentDB-Datenbankkonto zugeordnet ist, mithilfe des [Azure-Supports](#configure-ip-policy) hinzugefügt werden.  Dadurch wird sichergestellt, dass alle Rolleninstanzen von Clouddiensten Zugriff auf das DocumentDB-Datenbankkonto haben. Sie können die IP-Adressen für Ihre Clouddienste im Azure-Portal abrufen (siehe dazu den folgenden Screenshot). 

![Screenshot mit der öffentlichen IP-Adresse für einen Clouddienst im Azure-Portal](./media/documentdb-firewall-support/documentdb-public-ip-addresses.png)

Wenn Sie Ihren Clouddienst horizontal hochskalieren, indem Sie zusätzliche Rolleninstanzen hinzufügen, haben diese neuen Instanzen automatisch Zugriff auf das DocumentDB-Datenbankkonto, da sie zum gleichen Clouddienst gehören.

## <a name="connections-from-virtual-machines"></a>Verbindungen über virtuelle Computer
[Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/) oder [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) können auch zum Hosten von Diensten der mittleren Ebene mithilfe von DocumentDB verwendet werden.  Um das DocumentDB-Datenbankkonto für den Zugriff von virtuellen Computern zu konfigurieren, müssen die öffentlichen IP-Adressen des virtuellen Computers und/oder der VM-Skalierungsgruppe mithilfe des [Azure-Supports](#configure-ip-policy) als eine der zulässigen IP-Adressen für das DocumentDB-Datenbankkonto konfiguriert werden. Sie können die IP-Adressen für virtuelle Computer im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit einer öffentlichen IP-Adresse für einen virtuellen Computer im Azure-Portal](./media/documentdb-firewall-support/documentdb-public-ip-addresses-dns.png)

Wenn Sie der Gruppe weitere virtuelle Computerinstanzen hinzufügen, erhalten diese automatisch Zugriff auf das DocumentDB-Datenbankkonto.

## <a name="connections-from-the-internet"></a>Verbindungen über das Internet
Wenn Sie über einen Computer im Internet auf ein DocumentDB-Datenbankkonto zugreifen, muss die Client-IP-Adresse oder der Client-IP-Adressbereich des Computers der Liste der zulässigen IP-Adressen für das DocumentDB-Datenbankkonto hinzugefügt werden. 

## <a name="a-idconfigure-ip-policya-configuring-the-ip-access-control-policy"></a><a id="configure-ip-policy"></a> Konfigurieren der IP-Access Control-Richtlinie
Stellen Sie über das Azure-Portal eine Anfrage an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) zum Aktivieren der IP-Access Control-Richtlinie für Ihr Datenbankkonto.

1. Wählen Sie auf dem Blatt [Hilfe und Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) die Option **Neue Supportanfrage** aus.
2. Wählen Sie auf dem Blatt **Neue Supportanfrage** die Option **Grundlagen** aus.
3. Wählen Sie auf dem Blatt **Grundlagen** Folgendes aus:
   * **Problemtyp:** Kontingent
   * **Abonnement:** das Abonnement des Kontos, in dem die IP-Access Control-Richtlinie hinzugefügt werden soll.
   * **Kontingenttyp:** DocumentDB
   * **Supportplan:** Kontingentsupport – inbegriffen.
4. Gehen Sie auf dem Blatt **Problem** folgendermaßen vor:
   * **Schweregrad:** Wählen Sie „C – Minimale Auswirkungen“ aus.
   * **Details:** Kopieren Sie den folgenden Text in das Feld, und geben Sie die Kontonamen und IP-Adressen an: „I would like to enable firewall support for my DocumentDB database account. Database account: *Kontonamen einfügen*. Allowed IP address/Ranges: *IP-Adresse/IP-Adressbereich im CIDR-Format einfügen, z.B. 13.91.6.132, 13.91.6.1/24*.“
   * Klicken Sie auf **Weiter**. 
5. Geben Sie auf dem Blatt **Kontaktinformationen** Ihre Kontaktinformationen ein, und klicken Sie auf **Erstellen**. 

Nachdem Ihre Anfrage eingegangen ist, sollte die IP-Access Control innerhalb von 24 Stunden aktiviert werden. Sie werden benachrichtigt, wenn die Anfrage abgeschlossen ist.

![Screenshot der Blätter für „Hilfe und Support“](./media/documentdb-firewall-support/documentdb-firewall-support-request-access.png)

![Screenshot des Blatts „Problem“](./media/documentdb-firewall-support/documentdb-firewall-support-request-access-ticket.png)

## <a name="troubleshooting-the-ip-access-control-policy"></a>Problembehandlung für die IP-Access Control-Richtlinie
### <a name="portal-operations"></a>Portalvorgänge
Durch Aktivieren einer IP-Access Control-Richtlinie für Ihr DocumentDB-Datenbankkonto wird der Zugriff auf das DocumentDB-Datenbankkonto von Computern außerhalb der konfigurierten Liste der zulässigen IP-Adressbereiche blockiert. Aufgrund dieses Modells wird auch das Durchsuchen der Vorgänge auf Datenebene über das Portal blockiert, um die Integrität der Access Control sicherzustellen. 

### <a name="sdk--rest-api"></a>SDK und REST-API
Aus Sicherheitsgründen wird beim Zugriff über das SDK oder die REST-API von Computern, die in der Zulassungsliste nicht enthalten sind, die generische Antwort „404 Nicht gefunden“ ohne weitere Einzelheiten zurückgegeben. Überprüfen Sie die Liste der zulässigen IP-Adressen, die für Ihr DocumentDB-Datenbankkonto konfiguriert ist, um sicherzustellen, dass die passende Richtlinienkonfiguration für das DocumentDB-Datenbankkonto angewendet wird.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur netzwerkbezogenen Leistungssteigerung finden Sie unter [Tipps zur Leistungssteigerung](documentdb-performance-tips.md).




<!--HONumber=Nov16_HO3-->


