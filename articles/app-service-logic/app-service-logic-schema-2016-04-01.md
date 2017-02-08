---
title: "Neue Schemaversion „2016-06-01“ | Microsoft Docs"
description: "Erfahren Sie, wie die JSON-Definition für die aktuelle Version der Logik-Apps geschrieben wird."
author: jeffhollan
manager: dwrede
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eec9705688f88cedddedee32c562bdf714e45743


---
# <a name="new-schema-version-2016-06-01"></a>Neue Schemaversion „2016-06-01“
Die neue Schema- und API-Version für Logik-Apps enthält eine Reihe von Verbesserungen, die die Zuverlässigkeit und Benutzerfreundlichkeit von Logik-Apps erhöhen. Es gibt drei wichtige Unterschiede:

1. Es wurden Bereiche hinzugefügt. Dabei handelt es sich um Aktionen, die eine Sammlung von Aktionen enthalten.
2. Bedingungen und Schleifen sind erstklassige Aktionen.
3. Ausführlichere Ausführungsreihenfolge mit der `runAfter`-Eigenschaft (ersetzt `dependsOn`)

Informationen zum Upgraden Ihrer Logik-Apps von der Schemaversion „2015-08-01-preview“ auf die Schemaversion „2016-06-01“ finden Sie weiter unten im [Upgradeabschnitt](#upgrading-to-2016-06-01-schema)

## <a name="1-scopes"></a>1. Bereiche
Eine der größten Veränderungen bei diesem Schema sind die hinzugefügten Bereiche und die Möglichkeit zur Schachtelung von Aktionen.  Dies ist hilfreich, wenn Sie eine Reihe von Aktionen gruppieren oder Aktionen ineinander schachteln möchten, sodass beispielsweise eine Bedingung eine andere Bedingung enthält.  Ausführlichere Informationen zur Bereichssyntax finden Sie [hier](app-service-logic-loops-and-scopes.md). Im Anschluss folgt ein einfaches Bereichsbeispiel:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. Änderungen bei Bedingungen und Schleifen
In den vorherigen Versionen des Schemas handelte es sich bei den Bedingungen und Schleifen um Parameter, die einer einzelnen Aktion zugeordnet wurden.  Diese Einschränkung wurde in diesem Schema beseitigt, sodass Bedingungen und Schleifen nun als Aktionstyp angezeigt werden.  Weitere Informationen finden Sie in [diesem Artikel](app-service-logic-loops-and-scopes.md). Im Anschluss sehen Sie ein einfaches Beispiel für eine Bedingungsaktion:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. RunAfter-Eigenschaft
Die neue `runAfter`-Eigenschaft ersetzt `dependsOn`, um eine höhere Präzision bei der Ausführungsreihenfolge zu ermöglichen.  `dependsOn` bedeutete, dass die Aktion ausgeführt wurde und erfolgreich war. Häufig muss eine Aktion jedoch ausgeführt werden, wenn die vorherige Aktion erfolgreich oder nicht erfolgreich war oder übersprungen wurde.  Diese Flexibilität wird durch `runAfter` ermöglicht.  Das Objekt gibt alle Aktionsnamen an, nach denen es ausgeführt wird, und definiert ein Array mit Statuswerten, die als Trigger zulässig sind.  Wenn die Ausführung also etwa erfolgen soll, wenn Schritt A erfolgreich und Schritt B entweder erfolgreich oder nicht erfolgreich war, können Sie die `runAfter`-Eigenschaft wie folgt verwenden:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Upgrade auf Schemaversion „2016-06-01“
Zum Upgraden auf die Schemaversion „2016-06-01“ sind nur wenige Schritte erforderlich.  Ausführliche Informationen zu den schemabedingten Änderungen finden Sie in [diesem Artikel](app-service-logic-schema-2016-04-01.md).  Der Upgradeprozess umfasst das Ausführen des Upgradeskripts, das Speichern als neue Logik-App und ggf. das Überschreiben der alten Logik-App.

1. Öffnen Sie die aktuelle Logik-App.
2. Klicken Sie auf der Symbolleiste auf die Schaltfläche **Schema aktualisieren** .
   
    ![][1]
   
    Die aktualisierte Definition wird zurückgegeben.  Diese können Sie ggf. kopieren und in eine Ressourcendefinition einfügen. Wir empfehlen jedoch **dringend**, die Schaltfläche **Speichern unter** zu verwenden, um die Gültigkeit aller Verbindungsverweise in der aktualisierten Logik-App sicherzustellen.
3. Klicken Sie auf der Symbolleiste des Upgradeblatts auf die Schaltfläche **Speichern unter** .
4. Geben Sie den Namen und den Logik-App-Status an, und klicken Sie auf **Erstellen** , um die Upgrade-Logik-App bereitzustellen.
5. Vergewissern Sie sich, dass Ihre aktualisierte Logik-App ordnungsgemäß funktioniert.
   
   > [!NOTE]
   > Bei Verwendung eines manuellen oder anforderungsbasierten Triggers hat sich die Rückruf-URL in der neuen Logik-App geändert.  Vergewissern Sie sich unter Verwendung der neuen URL, dass alles funktioniert und dass die bereits vorhandene Logik-App zur Beibehaltung vorheriger URLs geklont werden kann.
   > 
   > 
6. *Optional:* Verwenden Sie die Schaltfläche **Klonen** auf der Symbolleiste (neben dem Symbol **Schema aktualisieren** im obigen Bild), um Ihre vorherige Logik-App mit der neuen Schemaversion zu überschreiben.  Dies ist nur erforderlich, wenn Sie die Ressourcen-ID oder Anforderungstrigger-URL Ihrer Logik-App beibehalten möchten.

### <a name="upgrade-tool-notes"></a>Hinweise zum Upgradetool
#### <a name="condition-mapping"></a>Bedingungszuordnung
Das Tool versucht, die True- und False-Verzweigungsaktionen in der aktualisierten Definition möglichst in einem Bereich zusammenzufassen.  Insbesondere das Designermuster von `@equals(actions('a').status, 'Skipped')` sollte als `else`-Aktion angezeigt werden.  Sollte das Tool jedoch Muster finden, die es nicht erkennt, werden für die True- und False-Verzweigung unter Umständen jeweils separate Bedingungen erstellt.  Aktionen können bei Bedarf nach dem Upgrade neu zugeordnet werden.

#### <a name="foreach-with-condition"></a>ForEach mit Bedingung
Das vorherige Muster einer foreach-Schleife mit einer Bedingung pro Element kann im neuen Schema mit der Filteraktion nachgestellt werden.  Dies sollte beim Upgrade automatisch erfolgen.  Die Bedingung wird zu einer Filteraktion vor der foreach-Schleife, um nur ein Array mit Elementen zurückzugeben, die die Bedingung erfüllen, und dieses Array wird an die foreach-Aktion übergeben.  Ein entsprechendes Beispiel finden Sie in [diesem Artikel](app-service-logic-loops-and-scopes.md)

#### <a name="resource-tags"></a>Ressourcentags
Ressourcentags werden im Zuge des Upgrades entfernt und müssen nach dem Upgrade für den Workflow erneut festgelegt werden.

## <a name="other-changes"></a>Weitere Änderungen
### <a name="manual-trigger-renamed-to-request-trigger"></a>Umbenennung manueller Trigger in Anforderungstrigger
Der Typ `manual` ist veraltet und wurde in `request` (Variante: `http`) umbenannt.  Dies passt besser zur Art der Muster, bei deren Erstellung der Trigger verwendet wird.

### <a name="new-filter-action"></a>Neue Filteraktion
Mit dem neuen Typ „filter“ können Sie ein umfangreiches Array filtern, um eine kleinere Gruppe von Elementen zu erhalten.  Er akzeptiert ein Array und eine Bedingung, wertet die Bedingung für jedes Element aus und gibt ein Array mit Elementen zurück, die die Bedingung erfüllen.

### <a name="foreach-and-until-action-restrictions"></a>Einschränkungen für ForEach- und until-Aktionen
Foreach- und until-Schleifen sind auf eine einzelne Aktion beschränkt.

### <a name="trackedproperties-on-actions"></a>TrackedProperties für Aktionen
Aktionen können nun eine zusätzliche Eigenschaft namens `trackedProperties` (gleichgeordnetes Element für `runAfter` und `type`) besitzen.  Dieses Objekt gibt bestimmte Aktionseingaben oder -ausgaben an, die in die Azure-Diagnosetelemetrie einbezogen werden sollen, die als Teil eines Workflows ausgegeben wird.  Beispiel:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen von Logik-App-Definitionen](app-service-logic-author-definitions.md)
* [Erstellen einer Bereitstellungsvorlage für Logik-Apps](app-service-logic-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png



<!--HONumber=Nov16_HO3-->


