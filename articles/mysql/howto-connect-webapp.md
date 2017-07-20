---
title: "Verbinden vorhandener Azure App Service-Instanzen mit Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Anweisungen zum ordnungsgemäßen Verbinden einer vorhandenen Azure App Service-Instanz mit Azure-Datenbank für MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 2488a28353f3cfe76dc5aa9f9a9159a37ee9901b
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---

# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Verbinden vorhandener Azure App Service-Instanzen mit Azure-Datenbank für MySQL
Dieses Dokument erläutert das Verbinden vorhandener Azure App Service-Instanzen mit Ihrer Azure-Datenbank für MySQL-Server.

## <a name="before-you-begin"></a>Voraussetzungen
Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Erstellen eines Azure-Datenbank für MySQL-Servers Weitere Informationen finden Sie unter [Erstellen von Azure-Datenbank für MySQL-Server im Portal](quickstart-create-mysql-server-database-using-azure-portal.md) oder [Erstellen von Azure-Datenbank für MySQL-Server mithilfe der Befehlszeilenschnittstelle](quickstart-create-mysql-server-database-using-azure-cli.md).

Es gibt derzeit zwei Lösungen zum Aktivieren des Zugriffs von Azure App Service auf Azure-Datenbank für MySQL. Beide Lösungen umfassen das Einrichten von Firewallregeln auf Serverebene.

## <a name="solution-1---create-a-firewall-rule-to-allow-all-ips"></a>Lösung 1: Erstellen einer Firewallregel für das Zulassen aller IP-Adressen
Azure-Datenbank für MySQL bietet Zugriffssicherheit über eine Firewall zum Schutz Ihrer Daten. Beim Herstellen einer Verbindung von Azure App Service mit Azure-Datenbank für MySQL-Server sollten Sie bedenken, dass die ausgehenden IP-Adressen von App Service dynamisch sind. 

Um die Verfügbarkeit von Azure App Service sicherzustellen, wird empfohlen, diese Lösung zu verwenden, bei der alle IP-Adressen zugelassen werden.

> [!NOTE]
> Microsoft arbeitet an einer langfristigen Lösung, mit der vermieden werden kann, dass für eine Verbindung mit Azure-Datenbank für MySQL alle IP-Adressen für Azure-Dienste zugelassen werden müssen.

1. Klicken Sie auf dem Blatt des MySQL-Servers unter der Überschrift „Einstellungen“ auf **Verbindungssicherheit**, um das Blatt „Verbindungssicherheit“ für Azure-Datenbank für MySQL zu öffnen.

   ![Azure-Portal – Klicken auf „Verbindungssicherheit“](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Geben Sie **REGELNAME**, **START-IP** und **END-IP** ein. Klicken Sie anschließend auf **Save**.
   - Regelname: Allow-All-IPs
   - Start-IP: 0.0.0.0
   - End-IP: 255.255.255.255

   ![Azure-Portal – alle IP-Adressen hinzufügen](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>Lösung 2: Erstellen einer Firewallregel zum expliziten Zulassen ausgehender IP-Adressen
Sie können alle ausgehenden IP-Adressen von Azure App Service explizit hinzufügen.

1. Sehen Sie sich auf dem Blatt „App Service-Eigenschaften“ den Eintrag in **Ausgehende IP-Adressen** an.

   ![Azure-Portal – ausgehende IP-Adressen anzeigen](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. Fügen Sie auf dem MySQL-Blatt „Verbindungssicherheit“ alle ausgehenden IP-Adressen einzeln hinzu.

   ![Azure-Portal – IP-Adressen einzeln hinzufügen](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Vergessen Sie nicht, Ihre Firewallregeln zu **speichern**.

Obwohl Azure App Service versucht, IP-Adressen im Lauf der Zeit konstant zu halten, gibt es Fälle, in denen sich IP-Adressen ändern können. Dazu gehört beispielsweise, wenn die App wiederverwendet oder ein Skalierungsvorgang durchgeführt wird oder wenn regionalen Azure-Rechenzentren zum Erhöhen der Kapazität neue Computer hinzugefügt werden. Wenn die IP-Adressen geändert werden und dadurch keine Verbindung mit dem MySQL-Server hergestellt werden kann, können bei der App Ausfallzeiten auftreten. Berücksichtigen Sie diese potenziellen Probleme, wenn Sie sich für eine der Lösungen oben entscheiden.

## <a name="ssl-configuration"></a>SSL-Konfiguration
Bei Azure-Datenbank für MySQL ist SSL standardmäßig aktiviert. Wenn Ihre Anwendung für Verbindungen mit der Datenbank nicht SSL verwendet, müssen Sie SSL auf dem MySQL-Server deaktivieren. Weitere Informationen zum Konfigurieren von SSL finden Sie unter [Verwenden von SSL mit Azure-Datenbank für MySQL](howto-configure-ssl.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Verbindungszeichenfolgen](howto-connection-string.md).

