---
title: "Firewallunterstützung und IP-Access Control in Azure DocumentDB | Microsoft-Dokumentation"
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
ms.date: 03/10/2017
ms.author: ankshah
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 7acbdda2e8179219c21370d20d30a94feb405fce
ms.lasthandoff: 03/11/2017


---
# <a name="documentdb-firewall-support"></a>DocumentDB-Firewallunterstützung
Zum Sichern von in einem Azure DocumentDB-Datenbankkonto gespeicherten Daten bietet DocumentDB Unterstützung für ein auf einem Geheimnis basierendes [Autorisierungsmodell](https://msdn.microsoft.com/library/azure/dn783368.aspx), das einen starken hashbasierten Nachrichtenauthentifizierungscode (HMAC) nutzt. DocumentDB unterstützt nun neben dem auf einem Geheimnis basierenden Autorisierungsmodell durch Richtlinien gesteuerte IP-basierte Access Control für die eingehende Firewallunterstützung. Dieses Modell ähnelt den Firewallregeln eines herkömmlichen Datenbanksystems sehr und bietet zusätzliche Sicherheit für das DocumentDB-Datenbankkonto. Mit diesem Modell können Sie nun ein DocumentDB-Datenbankkonto so konfigurieren, dass es nur über eine genehmigte Gruppe von Computern und/oder Clouddiensten zugänglich ist. Für den Zugriff auf DocumentDB-Ressourcen über diese genehmigten Gruppen von Computern und Diensten muss der Aufrufer weiterhin ein gültiges Autorisierungstoken vorlegen.

## <a name="ip-access-control-overview"></a>Übersicht über die IP-Access Control
Standardmäßig ist ein DocumentDB-Datenbankkonto über das öffentliche Internet zugänglich, solange die Anforderung zusammen mit einem gültigen Autorisierungstoken erfolgt. Zum Konfigurieren der auf IP-Richtlinien basierten Access Control muss der Benutzer die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format angeben, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. Nachdem diese Konfiguration angewendet wurde, werden alle Anforderungen von Computern, die nicht in dieser Zulassungsliste enthalten sind, vom Server blockiert.  In der folgenden Abbildung ist der Verbindungsverarbeitungsablauf für die IP-basierte Access Control dargestellt.

![Abbildung zum Verbindungsprozess für die IP-basierte Access Control](./media/documentdb-firewall-support/documentdb-firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Verbindungen über Clouddienste
In Azure ist es sehr üblich, dass Clouddienste Servicelogik der mittleren Ebene mithilfe von DocumentDB hosten. Um den Zugriff auf ein DocumentDB-Datenbankkonto über einen Clouddienst zu ermöglichen, muss die öffentliche IP-Adresse des Clouddiensts der Liste der zulässigen IP-Adressen, die dem DocumentDB-Datenbankkonto zugeordnet ist, durch [Konfigurieren der IP-Access Control-Richtlinie](#configure-ip-policy) hinzugefügt werden.  Dadurch wird sichergestellt, dass alle Rolleninstanzen von Clouddiensten Zugriff auf das DocumentDB-Datenbankkonto haben. Sie können die IP-Adressen für Ihre Clouddienste im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit der öffentlichen IP-Adresse für einen Clouddienst im Azure-Portal](./media/documentdb-firewall-support/documentdb-public-ip-addresses.png)

Wenn Sie Ihren Clouddienst horizontal hochskalieren, indem Sie zusätzliche Rolleninstanzen hinzufügen, haben diese neuen Instanzen automatisch Zugriff auf das DocumentDB-Datenbankkonto, da sie zum gleichen Clouddienst gehören.

## <a name="connections-from-virtual-machines"></a>Verbindungen über virtuelle Computer
[Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/) oder [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) können auch zum Hosten von Diensten der mittleren Ebene mithilfe von DocumentDB verwendet werden.  Um das DocumentDB-Datenbankkonto für den Zugriff von virtuellen Computern zu konfigurieren, müssen die öffentlichen IP-Adressen des virtuellen Computers und/oder der VM-Skalierungsgruppe durch [Konfigurieren der IP-Access Control-Richtlinie](#configure-ip-policy) als eine der zulässigen IP-Adressen für das DocumentDB-Datenbankkonto konfiguriert werden. Sie können die IP-Adressen für virtuelle Computer im Azure-Portal abrufen (siehe dazu den folgenden Screenshot).

![Screenshot mit einer öffentlichen IP-Adresse für einen virtuellen Computer im Azure-Portal](./media/documentdb-firewall-support/documentdb-public-ip-addresses-dns.png)

Wenn Sie der Gruppe weitere virtuelle Computerinstanzen hinzufügen, erhalten diese automatisch Zugriff auf das DocumentDB-Datenbankkonto.

## <a name="connections-from-the-internet"></a>Verbindungen über das Internet
Wenn Sie über einen Computer im Internet auf ein DocumentDB-Datenbankkonto zugreifen, muss die Client-IP-Adresse oder der Client-IP-Adressbereich des Computers der Liste der zulässigen IP-Adressen für das DocumentDB-Datenbankkonto hinzugefügt werden. 

## <a id="configure-ip-policy"></a> Konfigurieren der IP-Access Control-Richtlinie
Die IP-Zugriffssteuerungsrichtlinie kann im Azure-Portal oder programmgesteuert über die [Azure CLI](documentdb-automation-resource-manager-cli.md), über [Azure PowerShell](documentdb-manage-account-with-powershell.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) durch Aktualisierung der `ipRangeFilter`-Eigenschaft festgelegt werden. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Beispiel:&13;.91.6.132,13.91.6.1/24. Achten Sie beim Aktualisieren Ihres Datenbankkontos mithilfe dieser Methoden darauf, dass alle Eigenschaften ausgefüllt sind, um eine Zurücksetzung auf die Standardeinstellungen zu vermeiden.

> [!NOTE]
> Durch Aktivieren einer IP-Access Control-Richtlinie für Ihr DocumentDB-Datenbankkonto wird der Zugriff auf das DocumentDB-Datenbankkonto von Computern außerhalb der konfigurierten Liste der zulässigen IP-Adressbereiche blockiert. Aufgrund dieses Modells wird auch das Durchsuchen der Vorgänge auf Datenebene über das Portal blockiert, um die Integrität der Access Control sicherzustellen.

Um die Entwicklung zu vereinfachen, unterstützt das Azure-Portal Sie dabei, die IP-Adresse Ihres Clientcomputers zu ermitteln und der Zulassungsliste hinzuzufügen, sodass auf Ihrem Computer ausgeführte Apps auf das DocumentDB-Konto zugreifen können. Beachten Sie, dass die Client-IP-Adresse so ermittelt wird, wie sie im Portal zu sehen ist. Es kann sich um die Client-IP-Adresse Ihres Computers handeln, aber auch um die IP-Adresse Ihres Netzwerkgateways. Vergessen Sie nicht, diese Adresse vor dem Übergang in die Produktion zu entfernen.

Um die IP-Zugriffssteuerungsrichtlinie im Azure-Portal festzulegen, wechseln Sie zum Blatt des DocumentDB-Kontos, und klicken Sie im Navigationsmenü auf **Firewall** und dann auf **EIN**. 

![Screenshot: Öffnen des Blatts „Firewall“ im Azure-Portal](./media/documentdb-firewall-support/documentdb-azure-portal-firewall.png)

Geben Sie im neuen Fenster an, ob das Azure-Portal auf das Konto zugreifen kann, und fügen Sie nach Bedarf weitere Adressen und Adressbereiche hinzu. Klicken Sie dann auf **Speichern**.  

![Screenshot: Konfigurieren von Firewalleinstellungen im Azure-Portal](./media/documentdb-firewall-support/documentdb-azure-portal-firewall-configure.png)

## <a name="troubleshooting-the-ip-access-control-policy"></a>Problembehandlung für die IP-Access Control-Richtlinie
### <a name="portal-operations"></a>Portalvorgänge
Durch Aktivieren einer IP-Access Control-Richtlinie für Ihr DocumentDB-Datenbankkonto wird der Zugriff auf das DocumentDB-Datenbankkonto von Computern außerhalb der konfigurierten Liste der zulässigen IP-Adressbereiche blockiert. Wenn Sie also Vorgänge auf Portaldatenebene wie das Durchsuchen von Sammlungen und das Abfragen von Dokumenten aktivieren möchten, müssen Sie den Zugriff durch das Azure-Portal auf dem Blatt **Firewall** im Portal explizit zulassen. 

![Screenshot: Zulassen des Zugriffs auf das Azure-Portal](./media/documentdb-firewall-support/documentdb-azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>SDK und REST-API
Aus Sicherheitsgründen wird beim Zugriff über das SDK oder die REST-API von Computern, die in der Zulassungsliste nicht enthalten sind, die generische Antwort „404 Nicht gefunden“ ohne weitere Einzelheiten zurückgegeben. Überprüfen Sie die Liste der zulässigen IP-Adressen, die für Ihr DocumentDB-Datenbankkonto konfiguriert ist, um sicherzustellen, dass die passende Richtlinienkonfiguration für das DocumentDB-Datenbankkonto angewendet wird.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur netzwerkbezogenen Leistungssteigerung finden Sie unter [Tipps zur Leistungssteigerung](documentdb-performance-tips.md).


