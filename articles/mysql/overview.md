---
title: "Übersicht über den relationalen Datenbankdienst Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Übersicht über den relationalen Datenbankdienst Azure-Datenbank für MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 3eff97a9e7568566cd01a4eb3c7ee66806eac4cc
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---


# <a name="what-is-azure-database-for-mysql-service-introduction"></a>Was ist Azure-Datenbank für MySQL? Einführung in den Dienst
Azure-Datenbank für MySQL ist ein relationaler Datenbankdienst in der Microsoft-Cloud, der auf dem Datenbankmodul [MySQL Community Edition](https://www.mysql.com/products/community/) basiert.  Azure-Datenbank für MySQL bietet Folgendes:

- Vorhersagbare Leistung auf mehreren Serviceleveln
- Dynamische Skalierbarkeit ohne Ausfallzeit für Anwendungen
- Integrierte Hochverfügbarkeit
- Datenschutz

Diese Funktionen erfordern fast keine Administration und werden alle ohne zusätzliche Kosten bereitgestellt. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Darüber hinaus können Sie Ihre Anwendung weiterhin mit den Open-Source-Tools und der Plattform Ihrer Wahl entwickeln und mit der Geschwindigkeit und Effizienz liefern, die Ihr Unternehmen verlangt, ohne dass Sie sich dafür neue Fähigkeiten aneignen müssen.

![Konzeptdiagramm zu Azure-Datenbank für MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Dieser Artikel bietet eine Einführung in die wesentlichen Konzepte und Features von Azure-Datenbank für MySQL im Hinblick auf Leistung, Skalierbarkeit und Verwaltbarkeit sowie Links zu detaillierten Informationen. Diese Schnellstarts erleichtern Ihnen den Einstieg:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Eine Reihe von Azure CLI-Beispielen finden Sie unter:
- [Azure CLI-Beispiele für Azure-Datenbank für MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen von Leistung und Skalierung ohne Ausfallzeiten
Für den Dienst Azure-Datenbank für MySQL gibt es zwei Dienstebenen: „Basic“ und „Standard“. Jeder Tarif bietet verschiedene Leistungsstufen und Funktionen zur Unterstützung geringer und hoher Datenbankworkloads. Sie können Ihre erste App mit einer kleinen Datenbank für wenige Dollar im Monat erstellen und dann ganz ohne Ausfallzeit die Dienstebene ändern, um Ihre Lösung entsprechend den Anforderungen zu skalieren. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie bezahlen nur für die benötigten Ressourcen und auch nur dann, wenn Sie sie verwenden.

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Wie entscheiden Sie sich beim Anpassen für die richtige Lösung? Verwenden Sie die integrierten Features für Leistungsüberwachung und Warnungen in Kombination mit den Leistungsbewertungen nach Compute-Einheit. Mit diesen Features können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens je nach Ihren derzeitigen bzw. projektbezogenen Leistungsanforderungen schnell bewerten. Details finden Sie unter [Konzepte: Diensttarife](concepts-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Aufrechterhalten von App-Ausführung und Geschäftskontinuität
Durch die Unterstützung eines globalen Netzwerks von Microsoft-verwalteten Rechenzentren stellt die branchenführende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel (SLA) von 99,99 % sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. Mit jedem Azure-Datenbank für MySQL-Server nutzen Sie die integrierte Sicherheit, die Fehlertoleranz und die Datensicherheit, die Sie andernfalls entwerfen oder kaufen, erstellen und verwalten müssten. Mit Azure-Datenbank für MySQL können Sie über die Point-in-Time-Wiederherstellung den früheren Status eines Servers innerhalb der letzten 35 Tage wiederherstellen.

## <a name="secure-your-data"></a>Sichern der Daten
Azure-Datenbank für MySQL führt die Tradition der Datensicherheit von Azure-Datenbankdiensten fort – mit Features zum Einschränken des Zugriffs, zum Schützen ruhender und bewegter Daten und zum Überwachen von Aktivitäten. Informationen zur Sicherheit der Azure-Plattform finden Sie im [Azure Trust Center](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx).

Die Azure-Datenbank für den MySQL-Dienst verwendet Speicherverschlüsselung für ruhende Daten. Die Daten, einschließlich der Sicherungen, werden auf dem Datenträger verschlüsselt (mit Ausnahme von temporären Dateien, die vom Modul während der Ausführung von Abfragen erstellt werden). Der Dienst verwendet das in der Azure-Speicherverschlüsselung inbegriffene AES-256-Bit-Verschlüsselungsverfahren und die Schlüssel werden vom System verwaltet. Die Speicherverschlüsselung ist immer aktiviert und kann nicht deaktiviert werden.

In der Standardeinstellung ist die Azure-Datenbank für den MySQL-Dienst so konfiguriert, dass [SSL-Verbindungssicherheit](./concepts-ssl-connection-security.md) für Daten während Übertragungen innerhalb des Netzwerks erforderlich ist. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.  Wenn Ihre Clientanwendung keine SSL-Konnektivität unterstützt, können Sie optional die Erzwingung von SSL zum Herstellen einer Verbindung mit dem Datenbankdienst deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun eine Einführung in Azure-Datenbank für MySQL erhalten haben, in der die Frage „Was ist Azure-Datenbank für MySQL?“ beantwortet wurde, sind Sie für Folgendes bereit:
- Auf der Seite mit der Preisgestaltung finden Sie Kostenvergleiche und Rechner. [Preise](https://azure.microsoft.com/pricing/details/mysql/)
- Legen Sie los, und erstellen Sie Ihren ersten Server. [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md)
- Erstellen Sie Ihre erste App in Python, PHP, Ruby, C\#, Java oder Node.js: [Konnektivitätsbibliotheken zum Herstellen einer Verbindung mit Azure-Datenbank für MySQL](concepts-connection-libraries.md)

