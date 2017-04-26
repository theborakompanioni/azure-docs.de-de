---
title: "Aufrufen von REST-Endpunkten mit dem „HTTP + Swagger“-Connector für Azure Logic Apps | Microsoft-Dokumentation"
description: "Verbinden mit REST-Endpunkten von Logik-Apps über Swagger mit dem „HTTP + Swagger“-Connector"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: a881336bcb2384aaa57a75483c0e6fcd97096905
ms.lasthandoff: 04/10/2017


---
# <a name="get-started-with-the-http--swagger-action"></a>Erste Schritte mit der Aktion „HTTP + Swagger“

Sie können einen erstklassigen Connector für beliebige REST-Endpunkte über ein [Swagger-Dokument](https://swagger.io) erstellen, indem Sie die Aktion „HTTP + Swagger“ in Ihrem Logik-App-Workflow verwenden. Sie können Logik-Apps auch so erweitern, dass sie beliebige REST-Endpunkte mit erstklassigen Logik-App-Designer-Funktionen aufruft.

Informationen zum Erstellen von Logik-Apps mit Connectors finden Sie unter [Erstellen einer neuen Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Verwenden von „HTTP + Swagger“ als Trigger oder Aktion

Der Trigger und die Aktion „HTTP + Swagger“ funktionieren wie die [HTTP-Aktion](connectors-native-http.md), lassen sich im Logik-App-Designer aber besser verwenden, da sie die API-Struktur und Ausgaben der [Swagger-Metadaten](https://swagger.io) verfügbar machen. Sie können auch den „HTTP + Swagger“-Connector als Trigger verwenden. Wenn Sie einen Abruftrigger implementieren möchten, verwenden Sie das unter [Erstellen benutzerdefinierter APIs zum Aufrufen anderer APIs, Dienste und Systeme aus Logik-Apps](../logic-apps/logic-apps-create-api-app.md#polling-triggers) verwenden.

Weitere Informationen über Logik-App-Trigger und -Aktionen finden Sie [hier](connectors-overview.md).

Im Folgenden finden Sie ein Beispiel der Verwendung des Vorgangs „HTTP + Swagger“ als Aktion in einem Workflow in einer Logik-App.

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Aktionssuchfeld die Zeichenfolge **swagger** ein, um die Aktion „HTTP + Swagger“ anzuzeigen.
   
    ![Aktion „HTTP + Swagger“ auswählen](./media/connectors-native-http-swagger/using-action-1.png)
4. Geben Sie die URL für ein Swagger-Dokument ein:
   
   * Die URL muss ein HTTPS-Endpunkt und für CORS aktiviert sein, damit sie im Logik-App-Designer funktioniert.
   * Wenn das Swagger-Dokument diese Anforderung nicht erfüllt, können Sie [Azure Storage mit aktiviertem CORS](#hosting-swagger-from-storage) zum Speichern des Dokuments verwenden.
5. Klicken Sie auf **Weiter** , um das Swagger-Dokument zu lesen und zum Rendern zu nutzen.
6. Geben Sie alle Parameter an, die ggf. für den HTTP-Aufruf erforderlich sind.
   
    ![Konfigurieren der HTTP-Aktion](./media/connectors-native-http-swagger/using-action-2.png)
7. Klicken Sie zum Speichern und Veröffentlichen der Logik-App auf der Designer-Symbolleiste auf **Speichern**.

### <a name="host-swagger-from-azure-storage"></a>Hosten von Swagger aus Azure Storage
Möglicherweise möchten Sie auf ein Swagger-Dokument verweisen, das nicht gehostet wird, oder nicht die Sicherheits- und CORS-Anforderungen für den Designer erfüllt. Um dieses Problem zu lösen, können Sie das Swagger-Dokument in Azure Storage speichern und CORS aktivieren, um das Dokument zu referenzieren.  

Im Folgenden finden Sie die Schritte zum Erstellen, Konfigurieren und Speichern von Swagger-Dokumenten in Azure Storage:

1. [Erstellen Sie ein Azure-Speicherkonto mit Azure-Blobspeicher](../storage/storage-create-storage-account.md). Legen Sie für diesen Schritt die Berechtigungen auf **Öffentlicher Zugriff**fest.

2. Aktivieren Sie CORS für das Blob. 

   Sie können diese Einstellung mit [diesem PowerShell-Skript](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) automatisch konfigurieren.

3. Laden Sie die Swagger-Datei in das Blob hoch. 

   Hierzu können Sie das [Azure-Portal](https://portal.azure.com) oder ein Tool wie den [Azure-Speicher-Explorer](http://storageexplorer.com/) verwenden.

4. Verweisen Sie mit einem HTTPS-Link auf das Dokument im Azure-Blobspeicher. 

   Der Link hat das folgende Format:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Technische Details
Im Folgenden finden Sie Details zu den von diesem „HTTP + Swagger“-Connector unterstützten Triggern und Aktionen.

## <a name="http--swagger-triggers"></a>„HTTP + Swagger“-Trigger
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. [Weitere Informationen zu Triggern finden Sie hier.](connectors-overview.md) Der Connector „HTTP + Swagger“ verfügt über einen Trigger.

| Trigger | Beschreibung |
| --- | --- |
| HTTP + Swagger |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

## <a name="http--swagger-actions"></a>„HTTP + Swagger“-Aktionen
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der Connector „HTTP + Swagger“ verfügt über eine mögliche Aktion.

| Aktion | Beschreibung |
| --- | --- |
| HTTP + Swagger |Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück. |

### <a name="action-details"></a>Aktionsdetails
Der Connector „HTTP + Swagger“ verfügt über eine mögliche Aktion. Im Anschluss finden Sie Informationen zu den einzelnen Aktionen und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### <a name="http--swagger"></a>HTTP + Swagger
Erstellen Sie eine ausgehende HTTP-Anforderung mit Unterstützung von Swagger-Metadaten.
Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

| Anzeigename | Eigenschaftenname | Beschreibung |
| --- | --- | --- |
| Methode* |method |Zu verwendendes HTTP-Verb. |
| URI* |uri |URI für die HTTP-Anforderung. |
| Headers |Headers |Ein JSON-Objekt für die einzubeziehenden HTTP-Header. |
| Body |Body |Der HTTP-Anforderungstext. |
| Authentifizierung |Authentifizierung |Für die Anforderung zu verwendende Authentifizierung. Weitere Informationen finden Sie unter [HTTP-Connector](connectors-native-http.md#authentication). |

**Ausgabedetails**

HTTP-Antwort

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Antwortheader |
| Body |Objekt |Antwortobjekt |
| Statuscode |int |HTTP-Statuscode |

### <a name="http-responses"></a>HTTP-Antworten
Das Aufrufen verschiedener Aktionen löst unter Umständen bestimmte Antworten aus. Die folgende Tabelle enthält entsprechende Antworten und Beschreibungen.

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler. |

- - -
## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)
* [Andere Connectors](apis-list.md)
