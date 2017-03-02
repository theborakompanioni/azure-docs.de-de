---
title: Verwendung des Datei-Connectors in Logik-Apps | Microsoft-Dokumentation
description: Erstellen und Konfigurieren des Datei-Connectors oder einer API-App und Verwenden in einer Logik-App in Azure App Service
author: rajeshramabathiran
manager: erikre
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07ceb81a-f8f6-4901-a7a2-06a9ac47efd5
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2016
ms.author: rajram
translationtype: Human Translation
ms.sourcegitcommit: 5b1b65e3d1066bea6958fa6461a157ee39fbe7dc
ms.openlocfilehash: 35bd4561f6e8f4482cb084eb293b506eb9269cf7
ms.lasthandoff: 02/15/2017


---
# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit dem Datei-Connector und das Hinzufügen zur Logik-App
> [!NOTE]
> Diese Version des Artikels gilt für die Logik-Apps-Schemaversion 2014-12-01-preview.
>
>

Stellen Sie eine Verbindung mit einem Dateisystem her, um z. B. Dateien auf einem Hostcomputer hochzuladen und herunterzuladen. Logik-Apps können basierend auf einer Vielzahl von Datenquellen ausgelöst werden und Connectors anbieten, um Daten abzurufen und zu verarbeiten. Sie können den Datei-Connector dem geschäftlichen Workflow hinzufügen und Daten im Rahmen dieses Workflows in einer Logik-App verarbeiten.

Der Datei-Connector verwendet den Hybrid Connection Manager für eine Hybridkonnektivität mit dem Hostdateisystem.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Erstellen eines Datei-Connectors für Ihre Logik-App
Zur Verwendung des Datei-Connectors müssen Sie zunächst eine Instanz der Datei-Connector-API-App erstellen. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie den Azure Marketplace mit der Option "+NEU" auf der linken Seite im Azure-Portal.
2. Suchen Sie nach „Datei-Connector“.
3. Wählen Sie in den Suchergebnissen die Vorschauversion des Datei-Connectors **** .
4. Klicken Sie auf die Schaltfläche **Erstellen** .
5. Konfigurieren Sie den Datei-Connector wie folgt:   
   ![][1]

   * **Name** : Geben Sie einen Namen für den Datei-Connector ein.
   * **Paketeinstellungen**
     * **Stammordner** – Geben Sie den Pfad des Stammordners auf dem Hostcomputer an. Beispiel: D:\FileConnectorTest
     * **Service Bus-Verbindungszeichenfolge** - Geben Sie eine Dienstbus-Verbindungszeichenfolge an. Stellen Sie sicher, dass der Service Bus-Namespace vom Typ "Standard" ist und NICHT "Basic", um Service Bus Relays zu ermöglichen.  Service Bus Relay wird für die Verbindung mit dem Hybrid Connection Manager verwendet.
   * **App Service-Plan** - Wählen oder erstellen Sie einen App Service-Plan.
   * **Tarif** – Wählen Sie einen Tarif für den Connector.
   * **Ressourcengruppe** – Wählen oder erstellen Sie eine Ressourcengruppe, in der sich der Connector befinden soll.
   * **Abonnement** : Wählen Sie ein Abonnement aus, in dem dieser Connector erstellt werden soll.
   * **Standort** – Wählen Sie den geografischen Standort, an dem Sie den Connector bereitstellen möchten.
6. Klicken Sie auf "Erstellen". Ein neuer Datei-Connector wird erstellt.

## <a name="configure-hybrid-connection-manager"></a>Hybrid Connection Manager konfigurieren
Sobald die API-App-Instanz erstellt wurde, navigieren Sie zu deren Dashboard.  Klicken Sie dazu auf „Durchsuchen“ > „API-Apps“ >, und wählen Sie Ihre Datei-Connector-API-App aus.  Hier muss der Hybrid Connection Manager konfiguriert werden.
Weitere Informationen zum Konfigurieren des Hybrid Connection Managers und zur Problembehebung finden Sie unter [Hybrid Connection Manager verwenden].

## <a name="using-the-file-connector-in-your-logic-app"></a>Verwenden des Datei-Connectors in Logik-Apps
Sobald Ihre API-App erstellt wurde, können Sie den Datei-Connector als Aktion für Ihre Logik-App verwenden. Gehen Sie hierzu wie folgt vor:

1. Erstellen Sie eine neue Logik-App, und wählen Sie dieselbe Ressourcengruppe aus, in der sich der Datei-Connector befindet. Befolgen Sie die Anweisungen zum [Erstellen einer neuen Logik-App].
2. Öffnen Sie „Trigger und Aktionen“ innerhalb der erstellten Logik-App, um den Logik-App-Designer zu öffnen und den Workflow zu konfigurieren.
3. Der Datei-Connector wird im Abschnitt „API-Apps in dieser Ressourcengruppe“ im Katalog auf der rechten Seite angezeigt.
4. Sie können die Datei-Connector-API-App im Editor bearbeiten, indem Sie auf „Datei-Connector“ klicken. Der Datei-Connector macht einen Trigger und vier Aktionen verfügbar:   
   ![][5]
5. Diese machen wiederum verschiedene Eigenschaften verfügbar. In der folgenden Abbildung sind die Eigenschaften für den Trigger und die Dateiabrufaktion aufgelistet:   
   ![][6]
6. Nachdem Sie diese konfiguriert haben, können Sie die Trigger und Aktionen im Datenfluss nutzen. Auf ähnliche Weise können auch andere Aktionen konfiguriert werden.

> [!NOTE]
> Der Datei-Trigger löscht die Datei, nachdem sie erfolgreich aus dem Ordner gelesen wurde.
>
>

## <a name="file-connector-rest-apis"></a>Datei-Connector-REST-APIs
Um den Connector außerhalb einer Logik-App zu verwenden, können die vom Connector zur Verfügung gestellten REST-APIs genutzt werden. Sie können diese API-Definitionen mit „Durchsuchen“ > „API-App“ > „Datei-Connector“ anzeigen. Klicken Sie jetzt auf die Lupe "API-Definition" im Abschnitt "Zusammenfassung", um alle APIs anzuzeigen, die von diesem Connector zur Verfügung gestellt werden:   
![][7]

Ausführliche Informationen zu den APIs finden sie unter [File Connector](Datei-Connector).

## <a name="do-more-with-your-connector"></a>Mehr mit Ihrem Connector machen
Nachdem der Connector nun erstellt ist, können Sie ihn mit einer Logik-App einem Geschäftsworkflow hinzufügen. Informationen finden Sie unter [Was sind Logik-Apps?](../logic-apps/logic-apps-what-are-logic-apps.md).

> [!NOTE]
> Wenn Sie Azure Logic Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [Logik-App testen](https://azure.microsoft.com/try/app-service/logic/)sofort kostenlos eine kurzlebige Starter-Logik-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
>
>

Anzeigen der Swagger-REST-API-Referenz unter [Referenz zu Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch Leistungsstatistiken überprüfen und die Sicherheit zum Connector steuern. Siehe [Überwachen der Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Erstellen einer neuen Logik-App]: app-service-logic-create-a-logic-app.md
[File connector API definition]: https://msdn.microsoft.com/library/dn936296.aspx
[Hybrid Connection Manager verwenden]: ../app-service-web/web-sites-hybrid-connection-get-started.md

