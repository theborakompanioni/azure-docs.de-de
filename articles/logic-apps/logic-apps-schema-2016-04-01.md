---
title: "Schemaaktualisierungen 1. Juni 2016 – Azure Logic Apps | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie JSON-Definitionen für Azure Logic Apps mit der Schemaversion 2016-06-01 erstellen."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 43df04d6478e44c82c88b17d916cfc9fe4afc03e
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Schemaaktualisierungen für Azure Logic Apps – 1. Juni 2016

Diese neue Schema- und API-Version für Azure Logic Apps enthält wichtige Verbesserungen, mit denen Logik-Apps zuverlässiger und einfacher zu verwenden sind:

* Mit [Bereichen](#scopes) (Scopes) können Sie Aktionen als Sammlungen mit Aktionen gruppieren oder schachteln.
* [Bedingungen und Schleifen](#conditions-loops) sind jetzt erstklassige Aktionen.
* Genauere Sortierung der Ausführung von Aktionen mit der `runAfter`-Eigenschaft (ersetzt `dependsOn`)

Informationen zum Durchführen des Upgrades für Ihre Logik-App vom Schema vom 1. August 2015 (Vorschau) auf das Schema vom 1. Juni 2016 finden Sie im [entsprechenden Abschnitt](##upgrade-your-schema).

<a name="scopes"></a>
## <a name="scopes"></a>Bereiche

Dieses Schema enthält Bereiche, mit denen Sie Aktionen gruppieren oder schachteln können. Beispielsweise kann eine Bedingung eine andere Bedingung enthalten. Informieren Sie sich unter [Schleifen, Bereiche und Auflösen von Batches in Logik-Apps](../logic-apps/logic-apps-loops-and-scopes.md) über die Syntax von Bereichen, oder sehen Sie sich dieses einfache Beispiel zu Bereichen an:

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

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a>Änderungen bei Bedingungen und Schleifen

In den bisherigen Schemaversionen handelte es sich bei Bedingungen und Schleifen um Parameter, die einer einzelnen Aktion zugeordnet waren. Mit diesem Schema wird diese Einschränkung aufgehoben, sodass Bedingungen und Schleifen jetzt als Aktionstypen angezeigt werden. Informieren Sie sich unter [Schleifen, Bereiche und Auflösen von Batches in Logik-Apps](../logic-apps/logic-apps-loops-and-scopes.md) über Schleifen und Bereiche, oder sehen Sie sich dieses einfache Beispiel für eine Bedingungsaktion an:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
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
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a>runAfter-Eigenschaft

Die `runAfter`-Eigenschaft ersetzt die `dependsOn`-Eigenschaft und ermöglicht eine höhere Genauigkeit beim Angeben der Ausführungsreihenfolge für Aktionen basierend auf dem Status vorheriger Aktionen.

Die `dependsOn`-Eigenschaft war gleichbedeutend mit dem Vorgang „Aktion wurde ausgeführt und war erfolgreich“. Es spielte keine Rolle, wie oft eine Aktion ausgeführt werden sollte – basierend darauf, ob die vorherige Aktion erfolgreich war, nicht erfolgreich war oder übersprungen wurde. Die `runAfter`-Eigenschaft bietet diese Flexibilität in Form eines Objekts, mit dem die Namen aller Aktionen angegeben werden, nach denen das Objekt ausgeführt wird. Außerdem wird mit dieser Eigenschaft ein Array mit Statusangaben definiert, die als Trigger zulässig sind. Sie erstellen beispielsweise diese `runAfter`-Eigenschaft, wenn die Ausführung nach einem erfolgreichen Schritt A und auch nach einem erfolgreichen oder fehlgeschlagenen Schritt B erfolgen soll:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Durchführen des Upgrades für Ihr Schema

Zum Durchführen des Upgrades auf die neue Schemaversion sind nur wenige Schritte erforderlich. Der Upgradeprozess umfasst das Ausführen des Upgradeskripts, das Speichern als neue Logik-App und, falls gewünscht, das Überschreiben der vorherigen Logik-App.

1. Öffnen Sie Ihre Logik-App im Azure-Portal.

2. Navigieren Sie zu **Übersicht**. Wählen Sie in der Logik-App-Symbolleiste die Option **Schema aktualisieren**.
   
    ![Auswählen von „Schema aktualisieren“][1]
   
    Die aktualisierte Definition wird zurückgegeben, und Sie können sie bei Bedarf kopieren und in eine Ressourcendefinition einfügen. 
    Es wird **jedoch dringend empfohlen**, die Option **Speichern unter** zu wählen, um sicherzustellen, dass alle Verbindungsverweise in der aktualisierten Logik-App gültig sind.

3. Wählen Sie in der Symbolleiste des Blatts für das Upgrade die Option **Speichern unter**.

4. Geben Sie den Logiknamen und -status ein. Wählen Sie die Option **Erstellen**, um die aktualisierte Logik-App bereitzustellen.

5. Vergewissern Sie sich, dass die aktualisierte Logik-App wie gewünscht funktioniert.
   
   > [!NOTE]
   > Bei Verwendung eines manuellen oder anforderungsbasierten Triggers ändert sich die Rückruf-URL in der neuen Logik-App. Testen Sie die neue URL, um sicherzustellen, dass alles richtig funktioniert. Zum Beibehalten der vorherigen URLs können Sie Ihre vorhandene Logik-App klonen.

6. *Optional*: Wählen Sie neben **Schema aktualisieren** die Option **Klonen**, um die vorherige Logik-App mit der neuen Schemaversion zu überschreiben. Dieser Schritt ist nur erforderlich, wenn Sie die Ressourcen-ID oder Anforderungstrigger-URL Ihrer Logik-App beibehalten möchten.

### <a name="upgrade-tool-notes"></a>Hinweise zum Upgradetool

#### <a name="mapping-conditions"></a>Zuordnungsbedingungen

In der aktualisierten Definition gruppiert das Tool die „wahren“ und „falschen“ Verzweigungsaktionen so gut wie möglich als Bereiche. Insbesondere das Designermuster von `@equals(actions('a').status, 'Skipped')` sollte als `else`-Aktion angezeigt werden. Wenn vom Tool aber nicht erkennbare Muster ermittelt werden, werden ggf. für die „wahre“ und die „falsche“ Verzweigung separate Bedingungen erstellt. Sie können die Aktionen nach dem Upgrade bei Bedarf neu zuordnen.

#### <a name="foreach-loop-with-condition"></a>foreach-Schleife mit Bedingung

Im neuen Schema können Sie die Filteraktion verwenden, um das Muster einer `foreach`-Schleife mit einer Bedingung pro Element zu replizieren. Diese Änderung sollte aber automatisch während des Upgrades durchgeführt werden. Die Bedingung wird zu einer Filteraktion vor der foreach-Schleife, um nur ein Array mit Elementen zurückzugeben, die die Bedingung erfüllen, und dieses Array wird an die foreach-Aktion übergeben. Ein Beispiel hierzu finden Sie unter [Schleifen, Bereiche und Auflösen von Batches in Logik-Apps](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Ressourcentags

Nach dem Upgrade sind die Ressourcentags nicht mehr vorhanden, sodass Sie sie für den aktualisierten Workflow zurücksetzen müssen.

## <a name="other-changes"></a>Weitere Änderungen

### <a name="renamed-manual-trigger-to-request-trigger"></a>Umbenennung des Triggers „manual“ in „request“

Der Triggertyp `manual` wurde ausgesondert und in `request` mit dem Typ `http` umbenannt. Mit dieser Änderung wird für mehr Konsistenz für die Art von Muster gesorgt, für dessen Erstellung der Trigger verwendet wird.

### <a name="new-filter-action"></a>Neue Filteraktion

Zum Filtern eines großen Arrays in eine kleinere Gruppe von Elementen akzeptiert der neue Typ `filter` ein Array und eine Bedingung, wertet die Bedingung für die einzelnen Elemente aus und gibt ein Array mit Elementen zurück, die diese Bedingung erfüllen.

### <a name="restrictions-for-foreach-and-until-actions"></a>Einschränkungen für die Aktionen „foreach“ und „until“

Die `foreach`- und `until`-Schleifen sind auf eine einzelne Aktion beschränkt.

### <a name="new-trackedproperties-for-actions"></a>Neue trackedProperties-Eigenschaft für Aktionen

Aktionen können jetzt über eine zusätzliche Eigenschaft mit dem Namen `trackedProperties` verfügen. Sie ist den Eigenschaften `runAfter` und `type` gleichgeordnet. Mit diesem Objekt werden bestimmte Aktionseingaben oder -ausgaben angegeben, die Sie im Rahmen der Ausgabe eines Workflows in die Azure-Diagnosetelemetrie einbeziehen möchten. Beispiel:

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
* [Erstellen von Logik-App-Definitionen](../logic-apps/logic-apps-author-definitions.md)
* [Erstellen einer Bereitstellungsvorlage für Logik-Apps](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png

