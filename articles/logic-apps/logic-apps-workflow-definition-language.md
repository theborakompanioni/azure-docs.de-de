---
title: "Schema der Definitionssprache für Workflows – Azure Logic Apps | Microsoft-Dokumentation"
description: "Definieren von Workflows auf der Grundlage des Workflowdefinitionsschemas für Azure Logic Apps"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 09ccdaa18d383569747612c33b63595c06a0131e
ms.lasthandoff: 03/28/2017

---

# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Schema der Definitionssprache für Workflows für Azure Logic Apps

Eine Workflowdefinition enthält die eigentliche Logik, die im Rahmen Ihrer Logik-App ausgeführt wird. Diese Definition enthält mindestens einen Trigger zum Starten der Logik-App sowie mindestens eine Aktion, die von der Logik-App ausgeführt werden soll.  
  
## <a name="basic-workflow-definition-structure"></a>Grundlegende Workflowdefinitionsstruktur

Die grundlegende Struktur einer Workflowdefinition sieht wie folgt aus:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> Informationen zum Erstellen und Verwalten von Logik-App-Workflows finden Sie in der Dokumentation der [REST-API für die Workflowverwaltung](https://docs.microsoft.com/rest/api/logic/workflows).
  
|Elementname|Erforderlich|Beschreibung|  
|------------------|--------------|-----------------|  
|$schema|Nein|Gibt den Speicherort der JSON-Schemadatei an, die die Version der Definitionssprache beschreibt. Dieser Speicherort ist erforderlich, wenn Sie extern auf eine Definition verweisen. Für dieses Dokument lautet der Speicherort wie folgt: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#`|  
|contentVersion|Nein|Gibt die Definitionsversion an. Wenn Sie einen Workflow mithilfe der Definition bereitstellen, können Sie mit diesem Wert die Verwendung der richtigen Definition sicherstellen.|  
|parameters|Nein|Gibt Dateneingabeparameter für die Definition an. Es können bis zu 50 Parameter definiert werden.|  
|Trigger|Nein|Gibt Informationen für die Trigger an, die den Workflow initiieren. Es können bis zu 250 Trigger definiert werden.|  
|Aktionen|Nein|Gibt Aktionen an, die im Rahmen des Workflows ausgeführt werden. Es können bis zu 250 Aktionen definiert werden.|  
|outputs|Nein|Gibt Informationen zur bereitgestellten Ressource an. Es können bis zu zehn Ausgaben definiert werden.|  
  
## <a name="parameters"></a>Parameter

Dieser Abschnitt gibt alle Parameter an, die zur Bereitstellungszeit in der Workflowdefinition verwendet werden. Alle Parameter müssen zunächst in diesem Abschnitt deklariert werden, damit sie in anderen Abschnitten der Definition verwendet werden können.  
  
Das folgende Beispiel zeigt die Struktur einer Parameterdefinition:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Elementname|Erforderlich|Beschreibung|  
|------------------|--------------|-----------------|  
|Typ|Ja|**Typ:** string <p> **Deklaration:** `"parameters": {"parameter1": {"type": "string"}` <p> **Spezifikation:** `"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typ:** securestring <p> **Deklaration:** `"parameters": {"parameter1": {"type": "securestring"}}` <p> **Spezifikation:** `"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typ:** int <p> **Deklaration:** `"parameters": {"parameter1": {"type": "int"}}` <p> **Spezifikation:** `"parameters": {"parameter1": {"value" : 5}}` <p> **Typ:** bool <p> **Deklaration:** `"parameters": {"parameter1": {"type": "array"}}` <p> **Spezifikation:** `"parameters": {"parameter1": { "value": true }}` <p> **Typ:** array <p> **Deklaration:** `"parameters": {"parameter1": {"type": "array"}}` <p> **Spezifikation:** `"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Typ:** object <p> **Deklaration:** `"parameters": {"parameter1": {"type": "object"}}` <p> **Spezifikation:** `"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Typ:** secureobject <p> **Deklaration:** `"parameters": {"parameter1": {"type": "object"}}` <p> **Spezifikation:** `"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Hinweis:** Die Typen `securestring` und `secureobject` werden bei `GET`-Vorgängen nicht zurückgegeben. Dieser Typ muss für alle Kennwörter, Schlüssel und Geheimnisse verwendet werden.|  
|defaultValue|Nein|Gibt den Standardwert für den Parameter an. Dieser wird verwendet, falls zum Zeitpunkt der Ressourcenerstellung kein Wert angegeben wird.|  
|allowedValues|Nein|Gibt ein Array mit zulässigen Werten für den Parameter an.|  
|metadata|Nein|Gibt zusätzliche Informationen zum Parameter an. Hierzu zählen beispielsweise eine lesbare Beschreibung oder Entwurfszeitdaten für Visual Studio oder andere Tools.|  
  
Das folgende Beispiel veranschaulicht die Verwendung eines Parameters im body-Abschnitt einer Aktion:  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Parameter können auch in Ausgaben verwendet werden.  
  
## <a name="triggers-and-actions"></a>Trigger und Aktionen  

Trigger und Aktionen geben die Aufrufe an, die an der Workflowausführung beteiligt sein können. Ausführliche Informationen zu diesem Abschnitt finden Sie unter [Workflowaktionen und -trigger](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Ausgaben  

Ausgaben geben Informationen an, die von einer Workflowausführung zurückgegeben werden können. Wenn Sie also beispielsweise einen bestimmten Status oder Wert bei jeder Ausführung nachverfolgen möchten, können Sie die entsprechenden Daten in die Ausführungsausgabe einbeziehen. Die Daten erscheinen in der Verwaltungs-REST-API für diese Ausführung sowie auf der Verwaltungsbenutzeroberfläche für diese Ausführung im Azure-Portal. Die Ausgaben können auch an andere externe Systeme wie etwa Power BI weitergeleitet werden, um Dashboards zu erstellen. Ausgaben werden *nicht* verwendet, um auf eingehende Anforderungen für die Dienst-REST-API zu reagieren. Das folgende Beispiel veranschaulicht, wie Sie mithilfe des Aktionstyps `response` auf eine eingehende Anforderung reagieren:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Elementname|Erforderlich|Beschreibung|  
|------------------|--------------|-----------------|  
|key1|Ja|Gibt den Schlüsselbezeichner für die Ausgabe an. Ersetzen Sie **key1** durch einen Namen, den Sie zum Identifizieren der Ausgabe verwenden möchten.|  
|value|Ja|Gibt den der Ausgabe an.|  
|Typ|Ja|Gibt die Art des angegebenen Werts an. Mögliche Werttypen: <p>- `string`<br />- `securestring`<br />- `int`<br />- `bool`<br />- `array`<br />- `object`|
  
## <a name="expressions"></a>Ausdrücke  

Bei den JSON-Werten in der Definition kann es sich um Literalwerte oder um Ausdrücke handeln, die bei der Verwendung der Definition ausgewertet werden. Beispiel:  
  
```json
"name": "value"
```

 oder  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Einige Ausdrücke erhalten ihre Werte von Laufzeitaktionen, die zu Beginn der Ausführung unter Umständen noch nicht vorhanden sind. Einige dieser Werte können mithilfe von **Funktionen** abgerufen werden.  
  
Ausdrücke können an beliebiger Stelle in einem JSON-Zeichenfolgenwert verwendet werden und ergeben immer einen anderen JSON-Wert. Wenn ein JSON-Wert als Ausdruck erkannt wurde, wird der Text des Ausdrucks durch Entfernen des @-Zeichens extrahiert. Falls Sie ein Zeichenfolgenliteral benötigen, das mit einem @-Zeichen beginnt, muss die Zeichenfolge wie folgt mit einem Escapezeichen versehen werden: @. Die folgenden Beispiele veranschaulichen die Auswertung von Ausdrücken.  
  
|JSON-Wert|Ergebnis|  
|----------------|------------|  
|"parameters"|Die Zeichenfolge „parameters“ wird zurückgegeben.|  
|"parameters[1]"|Die Zeichenfolge „parameters[1]“ wird zurückgegeben.|  
|"@@"|Eine Zeichenfolge, die „@“ enthält, wird zurückgegeben (einzelnes Zeichen).|  
|" @"|Eine Zeichenfolge, die „ @“ enthält, wird zurückgegeben (zwei Zeichen).|  
  
Mit der *Zeichenfolgeninterpolation* können Ausdrücke auch innerhalb von Zeichenfolgen verwendet werden. Dabei werden die Ausdrücke in `@{ ... }` eingeschlossen. Beispiel: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName'}"`

Das Ergebnis ist immer eine Zeichenfolge. Somit ist dieses Feature vergleichbar mit der `concat`-Funktion. Angenommen, Sie haben `myNumber` als `42` und `myString` als `sampleString` definiert:  
  
|JSON-Wert|Ergebnis|  
|----------------|------------|  
|"@parameters('myString')"|Gibt `sampleString` als Zeichenfolge zurück.|  
|"@{parameters('myString')}"|Gibt `sampleString` als Zeichenfolge zurück.|  
|"@parameters('myNumber')"|Gibt `42` als *Zahl* zurück.|  
|"@{parameters('myNumber')}"|Gibt `42` als *Zeichenfolge* zurück.|  
|"Answer is: @{parameters('myNumber')}"|Gibt die Zeichenfolge `Answer is: 42` zurück.|  
|"@concat('Answer is: ', string(parameters('myNumber')))"|Gibt die Zeichenfolge `Answer is: 42` zurück.|  
|"Answer is: @@{parameters('myNumber')}"|Gibt die Zeichenfolge `Answer is: @{parameters('myNumber')}` zurück.|  
  
## <a name="operators"></a>Operatoren  

Operatoren sind Zeichen, die Sie innerhalb von Ausdrücken oder Funktionen verwenden können. 
  
|Operator|Beschreibung|  
|--------------|-----------------|  
|.|Mit dem Punktoperator können Sie auf Eigenschaften eines Objekts verweisen.|  
|?|Mit dem Fragezeichenoperator können Sie ohne Laufzeitfehler auf NULL-Eigenschaften eines Objekts verweisen. Mithilfe des folgenden Ausdrucks können Sie beispielsweise NULL-Triggerausgaben behandeln: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|Das einfache Anführungszeichen ist die einzige Möglichkeit zur Umschließung von Zeichenfolgenliteralen. Innerhalb eines Ausdrucks können keine doppelten Anführungszeichen verwendet werden, da dadurch ein Konflikt mit den JSON-Anführungszeichen entsteht, die den gesamten Ausdruck umschließen.|  
|[]|Mit eckigen Klammern können Sie einen Wert aus einem Array mit einem bestimmten Index abzurufen. Wenn Sie also beispielsweise über eine Aktion verfügen, die `range(0,10)` an die `forEach`-Funktion übergibt, können Sie mithilfe dieser Funktion Elemente aus Arrays abrufen:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>Funktionen  

Sie können auch Funktionen innerhalb von Ausdrücken aufrufen. Die folgende Tabelle gibt Aufschluss über die Funktionen, die in einem Ausdruck verwendet werden können.  
  
|Ausdruck|Auswertung|  
|----------------|----------------|  
|"@function('Hello')"|Ruft das Funktionselement der Definition mit dem Zeichenfolgenliteral „Hello“ als ersten Parameter auf.|  
|"@function('It's Cool!')"|Ruft das Funktionselement der Definition mit dem Zeichenfolgenliteral „It's Cool!“ als ersten Parameter auf.|  
|"@function().prop1"|Gibt den Wert der Eigenschaft „prop1“ des `myfunction`-Elements der Definition zurück.|  
|"@function('Hello').prop1"|Ruft das Funktionselement der Definition mit dem Zeichenfolgenliteral „Hello“ als ersten Parameter auf und gibt die Eigenschaft „prop1“ des Objekts zurück.|  
|"@function(parameters('Hello'))"|Wertet den Parameter „Hello“ aus und übergibt den Wert an die Funktion.|  
  
### <a name="referencing-functions"></a>Verweisfunktionen  

Mit diesen Funktionen können Sie auf Ausgaben anderer Aktionen in der Logik-App oder auf Werte verweisen, die beim Erstellen der Logik-App übergeben wurden. So können Sie beispielsweise in einem Schritt auf Daten aus einem anderen Schritt verweisen.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|parameters|Gibt einen in der Definition definierten Parameterwert zurück. <p>`parameters('password')` <p> **Parameternummer:** 1 <p> **Name:** Parameter <p> **Beschreibung:** Erforderlich. Der Name des Parameters mit den gewünschten Werten.|  
|action|Ermöglicht es, den Wert eines Ausdrucks von anderen JSON-Name-Wert-Paaren oder von der Ausgabe der aktuellen Laufzeitaktion abzuleiten. Die im folgenden Beispiel durch „propertyPath“ dargestellte Eigenschaft ist optional. Ohne Angabe von „propertyPath" wird auf das gesamte Aktionsobjekt verwiesen. Diese Funktion kann nur innerhalb von do-until-Bedingungen einer Aktion verwendet werden. <p>`action().outputs.body.propertyPath`|  
|actions|Ermöglicht es, den Wert eines Ausdrucks von anderen JSON-Name-Wert-Paaren oder von der Ausgabe der Laufzeitaktion abzuleiten. Diese Ausdrücke deklarieren explizit, dass eine Aktion von einer anderen Aktion abhängt. Die im folgenden Beispiel durch „propertyPath“ dargestellte Eigenschaft ist optional. Ohne Angabe von „propertyPath" wird auf das gesamte Aktionsobjekt verwiesen. Abhängigkeiten können entweder mit diesem Element oder mithilfe des conditions-Elements angegeben werden. Sie müssen aber nicht beides für die gleiche abhängige Ressource verwenden. <p>`actions('myAction').outputs.body.propertyPath` <p> **Parameternummer:** 1 <p> **Name:** Aktionsname <p> **Beschreibung:** Erforderlich. Der Name der Aktion mit den gewünschten Werten.|  
|trigger|Ermöglicht es, den Wert eines Ausdrucks von anderen JSON-Name-Wert-Paaren oder von der Ausgabe des Laufzeittriggers abzuleiten. Die im folgenden Beispiel durch „propertyPath“ dargestellte Eigenschaft ist optional. Ohne Angabe von „propertyPath" wird auf das gesamte Triggerobjekt verwiesen. <p>`trigger().outputs.body.propertyPath` <p>Bei Verwendung innerhalb der Eingaben eines Triggers gibt die Funktion die Ausgaben der vorherigen Ausführung zurück. Bei Verwendung innerhalb der Bedingung eines Triggers gibt die `trigger`-Funktion hingegen die Ausgaben der aktuellen Ausführung zurück.|  
|actionOutputs|Bei dieser Funktion handelt es sich um die Kurzform für `actions('actionName').outputs`. <p> **Parameternummer:** 1 <p> **Name:** Aktionsname <p> **Beschreibung:** Erforderlich. Der Name der Aktion mit den gewünschten Werten.|  
|actionBody/body|Bei diesen Funktionen handelt es sich um die Kurzform für `actions('actionName').outputs.body`. <p> **Parameternummer:** 1 <p> **Name:** Aktionsname <p> **Beschreibung:** Erforderlich. Der Name der Aktion mit den gewünschten Werten.|  
|triggerOutputs|Bei dieser Funktion handelt es sich um die Kurzform für `trigger().outputs`.|  
|triggerBody|Bei dieser Funktion handelt es sich um die Kurzform für `trigger().outputs.body`.|  
|item|Bei Verwendung innerhalb einer sich wiederholenden Aktion gibt diese Funktion das Element zurück, das sich im Array für diese Iteration der Aktion befindet. Wenn Sie also beispielsweise über eine Aktion verfügen, die für jedes Element eines Nachrichtenarrays ausgeführt wird, können Sie die folgende Syntax verwenden: <p>`"input1" : "@item().subject"`|  
  
### <a name="collection-functions"></a>Auflistungsfunktionen  

Diese Funktionen werden für Auflistungen ausgeführt und gelten im Allgemeinen für Arrays, Zeichenfolgen und manchmal für Wörterbücher.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|contains|Gibt „true“ zurück, wenn das Wörterbuch einen Schlüssel, die Liste einen Wert oder die Zeichenfolge eine Teilzeichenfolge enthält. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`contains('abacaba','aca')` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, in der gesucht werden soll. <p> **Parameternummer:** 2 <p> **Name:** Objekt suchen <p> **Beschreibung:** Erforderlich. Das Objekt, das innerhalb der **Auflistung** gesucht werden soll.|  
|length|Gibt die Anzahl von Elementen in einem Array oder in einer Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `3` zurück:  <p>`length('abc')` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, deren Länge abgerufen werden soll.|  
|empty|Gibt „true“ zurück, wenn das Objekt, das Array oder die Zeichenfolge leer ist. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`empty('')` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, für die geprüft werden soll, ob sie leer ist.|  
|intersection|Gibt ein einzelnes Array oder Objekt zurück, für das gemeinsame Elemente zwischen Arrays oder Objekten übergeben wurden. Die folgende Funktion gibt beispielsweise `[1, 2]` zurück: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>Bei den Parametern für die Funktion kann es sich entweder um eine Gruppe von Objekten oder um eine Gruppe von Arrays (aber nicht um eine Kombination aus beidem) handeln. Sollten zwei Objekte mit dem gleichen Namen vorhanden sein, wird jeweils das letzte Objekt in das endgültige Objekt aufgenommen. <p> **Parameternummer:** 1 ... *n* <p> **Name:** Auflistung *n* <p> **Beschreibung:** Erforderlich. Die auszuwertenden Auflistungen. Ein Objekt muss in allen übergebenen Auflistungen enthalten sein, um in das Ergebnis aufgenommen zu werden.|  
|union|Gibt ein einzelnes Array oder Objekt mit allen Elementen zurück, die sich im an diese Funktion übergebenen Array oder Objekt befinden. Die folgende Funktion gibt beispielsweise `[1, 2, 3, 10, 101]` zurück: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>Bei den Parametern für die Funktion kann es sich entweder um eine Gruppe von Objekten oder um eine Gruppe von Arrays (aber nicht um eine Kombination aus beidem) handeln. Sollten in der endgültigen Ausgabe zwei Objekte mit dem gleichen Namen vorhanden sein, wird jeweils das letzte Objekt in das endgültige Objekt aufgenommen. <p> **Parameternummer:** 1 ... *n* <p> **Name:** Auflistung *n* <p> **Beschreibung:** Erforderlich. Die auszuwertenden Auflistungen. Ein Objekt, das in einer der Sammlungen enthalten ist, ist auch im Ergebnis enthalten.|  
|first|Gibt das erste Element des übergebenen Arrays oder der übergebenen Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `0` zurück: <p>`first([0,2,3])` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, aus der das erste Objekt abgerufen werden soll.|  
|last|Gibt das letzte Element des übergebenen Arrays oder der übergebenen Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `3` zurück: <p>`last('0123')` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, aus der das letzte Objekt abgerufen werden soll.|  
|take|Gibt die ersten **Anzahl** Elemente aus dem übergebenen Array oder aus der übergebenen Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `[1, 2]` zurück:  <p>`take([1, 2, 3, 4], 2)` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, aus der die ersten **Anzahl** Objekte abgerufen werden sollen. <p> **Parameternummer:** 2 <p> **Name:** Anzahl <p> **Beschreibung:** Erforderlich. Die Anzahl von Objekten, die aus der **Auflistung** abgerufen werden sollen. Der Wert muss eine positive ganze Zahl sein.|  
|skip|Gibt die Elemente im Array ab dem Index **Anzahl** zurück. Die folgende Funktion gibt beispielsweise `[3, 4]` zurück: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Parameternummer:** 1 <p> **Name:** Auflistung <p> **Beschreibung:** Erforderlich. Die Auflistung, in der die ersten **Anzahl** Objekte übersprungen werden sollen. <p> **Parameternummer:** 2 <p> **Name:** Anzahl <p> **Beschreibung:** Erforderlich. Die Anzahl von Objekten, die am Beginn der **Auflistung** entfernt werden sollen. Der Wert muss eine positive ganze Zahl sein.|  
|join|Gibt eine Zeichenfolge mit den einzelnen Elementen eines Arrays zurück und verknüpft diese mit einem Trennzeichen. Die folgende Funktion gibt beispielsweise `"1,2,3,4"` zurück:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Auflistung<br /><br /> **Beschreibung:** Erforderlich. Die Auflistung, deren Elemente verknüpft werden sollen.<br /><br /> **Parameternummer:** 2<br /><br /> **Name:** Trennzeichen<br /><br /> **Beschreibung:** Erforderlich. Die Zeichenfolge, die als Trennzeichen für Elemente fungieren soll.|  
  
### <a name="string-functions"></a>Zeichenfolgenfunktionen

Die folgenden Funktionen gelten nur für Zeichenfolgen. Für Zeichenfolgen können auch einige Auflistungsfunktionen verwendet werden.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|concat|Kombiniert eine beliebige Anzahl von Zeichenfolgen. Wenn der Parameter 1 also beispielsweise `p1` lautet, gibt diese Funktion `somevalue-p1-somevalue` zurück: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Parameternummer:** 1 ... *n* <p> **Name:** Zeichenfolge *n* <p> **Beschreibung:** Erforderlich. Die Zeichenfolgen, die zu einer einzelnen Zeichenfolge zusammengefasst werden sollen.|  
|substring|Gibt eine Teilmenge der Zeichen aus einer Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `abc` zurück: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, aus der die Teilzeichenfolge abgerufen wird. <p> **Parameternummer:** 2 <p> **Name:** Startindex <p> **Beschreibung:** Erforderlich. Der Index für den Beginn der Teilzeichenfolge im Parameter 1. <p> **Parameternummer:** 3 <p> **Name:** Länge <p> **Beschreibung:** Erforderlich. Die Länge der Teilzeichenfolge.|  
|replace|Ersetzt eine Zeichenfolge durch eine angegebene Zeichenfolge. Die folgende Funktion gibt beispielsweise `the new string` zurück: <p>`replace('the old string', 'old', 'new')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die nach dem Parameter 2 durchsucht und mit dem Parameter 3 ersetzt wird, wenn der Parameter 2 im Parameter 1 gefunden wird. <p> **Parameternummer:** 2 <p> **Name:** Alte Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die durch den Parameter 3 ersetzt werden soll, wenn im Parameter 1 eine Übereinstimmung gefunden wird. <p> **Parameternummer:** 3 <p> **Name:** Neue Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, mit der die Zeichenfolge im Parameter 2 ersetzt werden soll, wenn im Parameter 1 eine Übereinstimmung gefunden wird.|  
|GUID|Diese Funktion generiert eine global eindeutige Zeichenfolge (GUID). Beispiel für eine GUID, die mit dieser Funktion generiert werden kann: `c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Parameternummer:** 1 <p> **Name:** Format <p> **Beschreibung:** Optional. Ein einzelner Formatbezeichner, der angibt, [wie der Wert dieser GUID formatiert werden soll](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Mögliche Formatparameter sind „N“, „D“, „B“, „P“ und „X“. Sollte kein Format angegeben werden, wird „D“ verwendet.|  
|toLower|Konvertiert eine Zeichenfolge in Kleinbuchstaben. Die folgende Funktion gibt beispielsweise `two by two is four` zurück: <p>`toLower('Two by Two is Four')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die in Kleinbuchstaben konvertiert werden soll. Sollte für ein Zeichen in der Zeichenfolge keine klein geschriebene Variante verfügbar sein, wird das Zeichen unverändert in die zurückgegebene Zeichenfolge eingefügt.|  
|toUpper|Konvertiert eine Zeichenfolge in Großbuchstaben. Die folgende Funktion gibt beispielsweise `TWO BY TWO IS FOUR` zurück: <p>`toUpper('Two by Two is Four')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die in Großbuchstaben konvertiert werden soll. Sollte für ein Zeichen in der Zeichenfolge keine groß geschriebene Variante verfügbar sein, wird das Zeichen unverändert in die zurückgegebene Zeichenfolge eingefügt.|  
|indexof|Sucht den Index eines Werts innerhalb einer Zeichenfolge ohne Berücksichtigung der Groß-/Kleinschreibung. Die folgende Funktion gibt beispielsweise `7` zurück: <p>`indexof('hello, world.', 'world')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die möglicherweise den Wert enthält. <p> **Parameternummer:** 2 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Wert, für den der Index ermittelt werden soll.|  
|lastindexof|Sucht den letzten Index eines Werts innerhalb einer Zeichenfolge ohne Berücksichtigung der Groß-/Kleinschreibung. Die folgende Funktion gibt beispielsweise `3` zurück: <p>`lastindexof('foofoo', 'foo')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die möglicherweise den Wert enthält. <p> **Parameternummer:** 2 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Wert, für den der Index ermittelt werden soll.|  
|startswith|Überprüft, ob die Zeichenfolge mit einem Wert beginnt (ohne Berücksichtigung der Groß-/Kleinschreibung). Die folgende Funktion gibt beispielsweise `true` zurück: <p>`startswith('hello, world', 'hello')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die möglicherweise den Wert enthält. <p> **Parameternummer:** 2 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Wert, mit dem die Zeichenfolge möglicherweise beginnt.|  
|endswith|Überprüft, ob die Zeichenfolge mit einem Wert endet (ohne Berücksichtigung der Groß-/Kleinschreibung). Die folgende Funktion gibt beispielsweise `true` zurück: <p>`endswith('hello, world', 'world')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die möglicherweise den Wert enthält. <p> **Parameternummer:** 2 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Wert, mit dem die Zeichenfolge möglicherweise endet.|  
|split|Teilt die Zeichenfolge mithilfe eines Trennzeichens auf. Die folgende Funktion gibt beispielsweise `["a", "b", "c"]` zurück: <p>`split('a;b;c',';')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die aufzuteilende Zeichenfolge. <p> **Parameternummer:** 2 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Das Trennzeichen.|  
  
### <a name="logical-functions"></a>Logische Funktionen  

Diese Funktionen können innerhalb von Bedingungen zur Auswertung einer beliebigen Art von Logik verwendet werden.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|equals|Gibt „true“ zurück, wenn zwei Werte gleich sind. Wenn „parameter1“ also beispielsweise „someValue“ lautet, gibt diese Funktion `true` zurück: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Parameternummer:** 1 <p> **Name:** Objekt 1 <p> **Beschreibung:** Erforderlich. Das Objekt, das mit **Objekt 2** verglichen werden soll. <p> **Parameternummer:** 2 <p> **Name:** Objekt 2 <p> **Beschreibung:** Erforderlich. Das Objekt, das mit **Objekt 1** verglichen werden soll.|  
|less|Gibt „true“ zurück, wenn das erste Argument kleiner als das zweite Argument ist. Beachten Sie, dass nur Werte vom Typ „integer“, „float“ oder „string“ verwendet werden können. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`less(10,100)` <p> **Parameternummer:** 1 <p> **Name:** Objekt 1 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es kleiner als **Objekt 2** ist. <p> **Parameternummer:** 2 <p> **Name:** Objekt 2 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es größer als **Objekt 1** ist.|  
|lessOrEquals|Gibt „true“ zurück, wenn das erste Argument im Vergleich zum zweiten Argument kleiner oder gleich groß ist. Beachten Sie, dass nur Werte vom Typ „integer“, „float“ oder „string“ verwendet werden können. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`lessOrEquals(10,10)` <p> **Parameternummer:** 1 <p> **Name:** Objekt 1 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es im Vergleich zu **Objekt 2** kleiner oder gleich groß ist. <p> **Parameternummer:** 2 <p> **Name:** Objekt 2 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es im Vergleich zu **Objekt 1** größer oder gleich groß ist.|  
|greater|Gibt „true“ zurück, wenn das erste Argument größer als das zweite Argument ist. Beachten Sie, dass nur Werte vom Typ „integer“, „float“ oder „string“ verwendet werden können. Die folgende Funktion gibt beispielsweise `false` zurück:  <p>`greater(10,10)` <p> **Parameternummer:** 1 <p> **Name:** Objekt 1 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es größer als **Objekt 2** ist. <p> **Parameternummer:** 2 <p> **Name:** Objekt 2 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es kleiner als **Objekt 1** ist.|  
|greaterOrEquals|Gibt „true“ zurück, wenn das erste Argument im Vergleich zum zweiten Argument größer oder gleich groß ist. Beachten Sie, dass nur Werte vom Typ „integer“, „float“ oder „string“ verwendet werden können. Die folgende Funktion gibt beispielsweise `false` zurück: <p>`greaterOrEquals(10,100)` <p> **Parameternummer:** 1 <p> **Name:** Objekt 1 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es im Vergleich zu **Objekt 2** größer oder gleich groß ist. <p> **Parameternummer:** 2 <p> **Name:** Objekt 2 <p> **Beschreibung:** Erforderlich. Das Objekt, für das geprüft werden soll, ob es im Vergleich zu **Objekt 1** kleiner oder gleich groß ist.|  
|und|Gibt „true“ zurück, wenn beide Parameter „true“ sind. Bei beiden Argumenten muss es sich um boolesche Werte handeln. Die folgende Funktion gibt beispielsweise `false` zurück: <p>`and(greater(1,10),equals(0,0))` <p> **Parameternummer:** 1 <p> **Name:** Boolescher Wert 1 <p> **Beschreibung:** Erforderlich. Das erste Argument, das `true` sein muss. <p> **Parameternummer:** 2 <p> **Name:** Boolescher Wert 2 <p> **Beschreibung:** Erforderlich. Das zweite Argument, das `true` sein muss.|  
|oder|Gibt „true“ zurück, wenn einer der Parameter „true“ ist. Bei beiden Argumenten muss es sich um boolesche Werte handeln. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`or(greater(1,10),equals(0,0))` <p> **Parameternummer:** 1 <p> **Name:** Boolescher Wert 1 <p> **Beschreibung:** Erforderlich. Das erste Argument, das möglicherweise `true` ist. <p> **Parameternummer:** 2 <p> **Name:** Boolescher Wert 2 <p> **Beschreibung:** Erforderlich. Das zweite Argument, das möglicherweise `true` ist.|  
|not|Gibt „true“ zurück, wenn die Parameter `false` sind. Bei beiden Argumenten muss es sich um boolesche Werte handeln. Die folgende Funktion gibt beispielsweise `true` zurück: <p>`not(contains('200 Success','Fail'))` <p> **Parameternummer:** 1 <p> **Name:** Boolescher Wert <p> **Beschreibung:** Gibt „true“ zurück, wenn die Parameter `false` sind. Bei beiden Argumenten muss es sich um boolesche Werte handeln. Die folgende Funktion gibt `true` zurück: `not(contains('200 Success','Fail'))`|  
|if|Macht die Rückgabe eines angegebenen Werts davon abhängig, ob der Ausdruck `true` oder `false` ergeben hat.  Die folgende Funktion gibt beispielsweise `"yes"` zurück: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Parameternummer:** 1 <p> **Name:** Ausdruck <p> **Beschreibung:** Erforderlich. Ein boolescher Wert, der bestimmt, welchen Wert der Ausdruck zurückgeben soll. <p> **Parameternummer:** 2 <p> **Name:** True <p> **Beschreibung:** Erforderlich. Der zurückzugebende Wert, wenn der Ausdruck `true` ist. <p> **Parameternummer:** 3 <p> **Name:** False <p> **Beschreibung:** Erforderlich. Der zurückzugebende Wert, wenn der Ausdruck `false` ist.|  
  
### <a name="conversion-functions"></a>Konvertierungsfunktionen  

Diese Funktionen dienen zum Konvertieren zwischen den einzelnen nativen Typen in der Sprache:  
  
- Zeichenfolge  
  
- Ganze Zahl  
  
- Gleitkommawert  
  
- Boolescher Wert  
  
- Arrays  
  
- Wörterbücher  

-   Formulare  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|int|Konvertiert den Parameter in eine ganze Zahl. Die folgende Funktion gibt beispielsweise 100 als Zahl (und nicht als Zeichenfolge) zurück: <p>`int('100')` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in eine ganze Zahl konvertiert wird.|  
|string|Konvertiert den Parameter in eine Zeichenfolge. Die folgende Funktion gibt beispielsweise `'10'` zurück: <p>`string(10)` <p>Sie können auch ein Objekt in eine Zeichenfolge konvertieren. Wenn es sich also beispielsweise bei dem Parameter `myPar` um ein Objekt mit der einzelnen Eigenschaft `abc : xyz` handelt, gibt die folgende Funktion `{"abc" : "xyz"}` zurück: <p>`string(parameters('myPar'))` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in eine Zeichenfolge konvertiert wird.|  
|json|Konvertiert den Parameter in einen JSON-Wert und ist das Gegenteil von `string()`. Die folgende Funktion gibt beispielsweise `[1,2,3]` als Array (und nicht als Zeichenfolge) zurück: <p>`parse('[1,2,3]')` <p>Analog dazu können Sie eine Zeichenfolge in ein Objekt konvertieren. Die folgende Funktion gibt beispielsweise `{ "abc" : "xyz" }` zurück: <p>`json('{"abc" : "xyz"}')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die in einen nativen Typwert konvertiert werden soll. <p>Die `json()`-Funktion unterstützt auch XML-Eingaben. So kann beispielsweise der folgende Parameterwert in JSON konvertiert werden: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>Ergebnis: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|float|Konvertiert das Parameterargument in eine Gleitkommazahl. Die folgende Funktion gibt beispielsweise `10.333` zurück: <p>`float('10.333')` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in eine Gleitkommazahl konvertiert wird.|  
|bool|Konvertiert den Parameter in einen booleschen Wert. Die folgende Funktion gibt beispielsweise `false` zurück: <p>`bool(0)` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in einen booleschen Wert konvertiert wird.|  
|coalesce|Gibt das erste Objekt aus den übergebenen Argumenten zurück, das nicht NULL ist. **Hinweis:** Eine leere Zeichenfolge ist nicht NULL. Wenn also beispielsweise die Parameter 1 und 2 nicht definiert sind, gibt die folgende Funktion `fallback` zurück:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Parameternummer:** 1 ... *n* <p> **Name:**: Objekt*n* <p> **Beschreibung:** Erforderlich. Die Objekte, für die geprüft werden soll, ob sie NULL sind.|  
|base64|Rückkehr zur base64-Darstellung der Eingabezeichenfolge. Die folgende Funktion gibt beispielsweise `c29tZSBzdHJpbmc=` zurück: <p>`base64('some string')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge 1 <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die als Base64-Darstellung codiert werden soll.|  
|base64ToBinary|Gibt eine binäre Darstellung einer Base64-codierten Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise die binäre Darstellung von `some string` zurück: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Base64-codierte Zeichenfolge.|  
|base64ToString|Gibt eine Zeichenfolgendarstellung einer Base64-codierten Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `some string` zurück: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Base64-codierte Zeichenfolge.|  
|Binär|Gibt eine binäre Darstellung eines Werts zurück.  Die folgende Funktion gibt beispielsweise eine binäre Darstellung von `some string` zurück: <p>`binary('some string')` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in einen Binärwert konvertiert wird.|  
|dataUriToBinary|Gibt eine binäre Darstellung eines Daten-URI zurück. Die folgende Funktion gibt beispielsweise die binäre Darstellung von `some string` zurück: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Daten-URI, der in eine binäre Darstellung konvertiert werden soll.|  
|dataUriToString|Gibt eine Zeichenfolgendarstellung eines Daten-URI zurück. Die folgende Funktion gibt beispielsweise `some string` zurück: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge<p> **Beschreibung:** Erforderlich. Der Daten-URI, der in eine Zeichenfolgendarstellung konvertiert werden soll.|  
|dataUri|Gibt einen Daten-URI eines Werts zurück. Die folgende Funktion gibt beispielsweise den Daten-URI `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=` zurück: <p>`dataUri('some string')` <p> **Parameternummer:** 1<p> **Name:** Wert<p> **Beschreibung:** Erforderlich. Der Wert, der in einen Daten-URI konvertiert werden soll.|  
|decodeBase64|Gibt eine Zeichenfolgendarstellung einer Base64-Eingabezeichenfolge zurück. Die folgende Funktion gibt beispielsweise `some string` zurück: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Gibt eine Zeichenfolgendarstellung einer Base64-Eingabezeichenfolge zurück.|  
|encodeUriComponent|Versieht die übergebene Zeichenfolge mit URL-Escapezeichen. Die folgende Funktion gibt beispielsweise `You+Are%3ACool%2FAwesome` zurück: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, in der für URLs ungeeignete Zeichen mit Escapezeichen versehen werden sollen.|  
|decodeUriComponent|Entfernt URL-Escapezeichen aus der übergebenen Zeichenfolge. Die folgende Funktion gibt beispielsweise `You Are:Cool/Awesome` zurück: <p>`encodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, in der für URLs ungeeignete Zeichen decodiert werden sollen.|  
|decodeDataUri|Gibt eine binäre Darstellung einer Eingabezeichenfolge des Daten-URI zurück. Die folgende Funktion gibt beispielsweise die binäre Darstellung von `some string` zurück: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Der Daten-URI, der in eine binäre Darstellung konvertiert werden soll.|  
|uriComponent|Gibt eine URI-codierte Darstellung eines Werts zurück. Die folgende Funktion gibt beispielsweise `You+Are%3ACool%2FAwesome` zurück: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Parameternummer:** 1<p> **Name:** Zeichenfolge <p> **Beschreibung:** Erforderlich. Die Zeichenfolge, die als URI codiert werden soll.|  
|uriComponentToBinary|Gibt eine binäre Darstellung einer URI-codierten Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise eine binäre Darstellung von `You Are:Cool/Awesome` zurück: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer:** 1 <p> **Name:** Zeichenfolge<p> **Beschreibung:** Erforderlich. Die URI-codierte Zeichenfolge.|  
|uriComponentToString|Gibt eine Zeichenfolgendarstellung einer URI-codierten Zeichenfolge zurück. Die folgende Funktion gibt beispielsweise `You Are:Cool/Awesome` zurück: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer:** 1<p> **Name:** Zeichenfolge<p> **Beschreibung:** Erforderlich. Die URI-codierte Zeichenfolge.|  
|xml|Gibt eine XML-Darstellung des Werts zurück. Die folgende Funktion gibt beispielsweise durch `'\<name>Alan\</name>'` dargestellten XML-Inhalt zurück: <p>`xml('\<name>Alan\</name>')` <p>Die `xml()`-Funktion unterstützt auch JSON-Objekteingaben. Der Parameter `{ "abc": "xyz" }` wird beispielsweise in XML-Inhalt konvertiert:`\<abc>xyz\</abc>` <p> **Parameternummer:** 1<p> **Name:** Wert<p> **Beschreibung:** Erforderlich. Der Wert, der in XML konvertiert werden soll.|  
|xpath|Gibt ein Array mit XML-Knoten zurück, die dem xpath-Ausdruck eines Werts entsprechen, zu dem der xpath-Ausdruck ausgewertet wird. <p> **Beispiel 1** <p>Angenommen, der Wert des Parameters `p1` ist eine Zeichenfolgendarstellung des folgenden XML-Codes: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Der Code `xpath(xml(parameters('p1'), '/lab/robot/name')` <p>gibt Folgendes zurück: <p>`[ <name>R1</name>, <name>R2</name> ]` <p>Der Code <p>`xpath(xml(parameters('p1'), ' sum(/lab/robot/parts)')` <p>gibt Folgendes zurück: <p>`13` <p> <p> **Beispiel 2** <p>Angenommen, der folgende XML-Inhalt wird verwendet: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Der Code `@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>oder der Code <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>gibt Folgendes zurück: <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>Der Code `@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>gibt Folgendes zurück: <p>``xyz`` <p> **Parameternummer:** 1 <p> **Name:** XML <p> **Beschreibung:** Erforderlich. Der XML-Code, für den der XPath-Ausdruck ausgewertet werden soll. <p> **Parameternummer:** 2 <p> **Name:** XPath <p> **Beschreibung:** Erforderlich. Der auszuwertende XPath-Ausdruck.|  
|array|Konvertiert den Parameter in ein Array. Die folgende Funktion gibt beispielsweise `["abc"]` zurück: <p>`array('abc')` <p> **Parameternummer:** 1 <p> **Name:** Wert <p> **Beschreibung:** Erforderlich. Der Wert, der in ein Array konvertiert wird.|
|createArray|Erstellt ein Array auf der Grundlage der Parameter. Die folgende Funktion gibt beispielsweise `["a", "c"]` zurück: <p>`createArray('a', 'c')` <p> **Parameternummer:** 1 ... *n* <p> **Name:** Beliebig *n* <p> **Beschreibung:** Erforderlich. Die Werte, die zu einem Array zusammengefasst werden sollen.|
|triggerFormDataValue|Gibt einen einzelnen Wert zurück, der dem Schlüsselnamen aus der formulardatenbasierten oder formularcodierten Triggerausgabe entspricht.  Sollten mehrere Übereinstimmungen gefunden werden, tritt ein Fehler auf.  Die folgende Funktion gibt beispielsweise `bar` zurück: `triggerFormDataValue('foo')`<br /><br />**Parameternummer:** 1<br /><br />**Name:** Schlüsselname<br /><br />**Beschreibung:** Erforderlich. Der Schlüsselname des zurückzugebenden Formulardatenwerts.|
|triggerFormDataMultiValues|Gibt ein Array mit Werten zurück, die dem Schlüsselnamen aus der formulardatenbasierten oder formularcodierten Triggerausgabe entsprechen.  Die folgende Funktion gibt beispielsweise `["bar"]` zurück: `triggerFormDataValue('foo')`<br /><br />**Parameternummer:** 1<br /><br />**Name:** Schlüsselname<br /><br />**Beschreibung:** Erforderlich. Der Schlüsselname der zurückzugebenden Formulardatenwerte.|
|triggerMultipartBody|Gibt den Text für einen Teil in einer mehrteiligen Ausgabe des Triggers zurück.<br /><br />**Parameternummer:** 1<br /><br />**Name:** Index<br /><br />**Beschreibung:** Erforderlich. Der Index des abzurufenden Teils.|
|formDataValue|Gibt einen einzelnen Wert zurück, der dem Schlüsselnamen aus der formulardatenbasierten oder formularcodierten Aktionsausgabe entspricht.  Sollten mehrere Übereinstimmungen gefunden werden, tritt ein Fehler auf.  Die folgende Funktion gibt beispielsweise `bar` zurück: `formDataValue('someAction', 'foo')`<br /><br />**Parameternummer:** 1<br /><br />**Name:** Aktionsname<br /><br />**Beschreibung:** Erforderlich. Der Name der Aktion mit einer formulardatenbasierten oder formularcodierten Antwort.<br /><br />**Parameternummer:** 2<br /><br />**Name:** Schlüsselname<br /><br />**Beschreibung:** Erforderlich. Der Schlüsselname des zurückzugebenden Formulardatenwerts.|
|formDataMultiValues|Gibt ein Array mit Werten zurück, die dem Schlüsselnamen aus der formulardatenbasierten oder formularcodierten Aktionsausgabe entsprechen.  Die folgende Funktion gibt beispielsweise `["bar"]` zurück: `formDataMultiValues('someAction', 'foo')`<br /><br />**Parameternummer:** 1<br /><br />**Name:** Aktionsname<br /><br />**Beschreibung:** Erforderlich. Der Name der Aktion mit einer formulardatenbasierten oder formularcodierten Antwort.<br /><br />**Parameternummer:** 2<br /><br />**Name:** Schlüsselname<br /><br />**Beschreibung:** Erforderlich. Der Schlüsselname der zurückzugebenden Formulardatenwerte.|
|multipartBody|Gibt den Text für einen Teil in einer mehrteiligen Ausgabe einer Aktion zurück.<br /><br />**Parameternummer:** 1<br /><br />**Name:** Aktionsname<br /><br />**Beschreibung:** Erforderlich. Der Name der Aktion mit einer mehrteiligen Antwort.<br /><br />**Parameternummer:** 2<br /><br />**Name:** Index<br /><br />**Beschreibung:** Erforderlich. Der Index des abzurufenden Teils.|

### <a name="math-functions"></a>Mathematische Funktionen  

Diese Funktionen können für folgende Arten von Zahlen verwendet werden: **ganze Zahlen** und **Gleitkommazahlen**.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|add|Gibt das Ergebnis der Addition zweier Zahlen zurück. Die folgende Funktion gibt beispielsweise `20.333` zurück: <p>`add(10,10.333)` <p> **Parameternummer:** 1 <p> **Name:** Summand 1 <p> **Beschreibung:** Erforderlich. Die Zahl, die zu **Summand 2** addiert werden soll. <p> **Parameternummer:** 2 <p> **Name:** Summand 2 <p> **Beschreibung:** Erforderlich. Die Zahl, die zu **Summand 1** addiert werden soll.|  
|sub|Gibt das Ergebnis der Subtraktion zweier Zahlen zurück. Die folgende Funktion gibt beispielsweise `-0.333` zurück: <p>`sub(10,10.333)` <p> **Parameternummer:** 1 <p> **Name:** Minuend <p> **Beschreibung:** Erforderlich. Die Zahl, von der **Subtrahend** abgezogen wird. <p> **Parameternummer:** 2 <p> **Name:** Subtrahend <p> **Beschreibung:** Erforderlich. Die Zahl, die von **Minuend** abgezogen werden soll.|  
|mul|Gibt das Ergebnis der Multiplikation zweier Zahlen zurück. Die folgende Funktion gibt beispielsweise `103.33` zurück: <p>`mul(10,10.333)` <p> **Parameternummer:** 1 <p> **Name:** Multiplikand 1 <p> **Beschreibung:** Erforderlich. Die Zahl, mit der **Multiplikand 2** multipliziert werden soll. <p> **Parameternummer:** 2 <p> **Name:** Multiplikand 2 <p> **Beschreibung:** Erforderlich. Die Zahl, mit der **Multiplikand 1** multipliziert werden soll.|  
|div|Gibt das Ergebnis der Division zweier Zahlen zurück. Die folgende Funktion gibt beispielsweise `1.0333` zurück: <p>`div(10.333,10)` <p> **Parameternummer:** 1 <p> **Name:** Dividend <p> **Beschreibung:** Erforderlich. Die Zahl, die durch den **Divisor** dividiert werden soll. <p> **Parameternummer:** 2 <p> **Name:** Divisor <p> **Beschreibung:** Erforderlich. Die Zahl, durch die der **Dividend** dividiert werden soll.|  
|mod|Gibt den Rest (Modulo) nach der Division der beiden Zahlen zurück. Die folgende Funktion gibt beispielsweise `2` zurück: <p>`mod(10,4)` <p> **Parameternummer:** 1 <p> **Name:** Dividend <p> **Beschreibung:** Erforderlich. Die Zahl, die durch den **Divisor** dividiert werden soll. <p> **Parameternummer:** 2 <p> **Name:** Divisor <p> **Beschreibung:** Erforderlich. Die Zahl, durch die der **Dividend** dividiert werden soll. Nach der Division wird der Rest verwendet.|  
|min|Diese Funktion kann auf zwei Arten aufgerufen werden. <p>Im folgenden Beispiel verwendet `min` ein Array, und die Funktion gibt `0` zurück: <p>`min([0,1,2])` <p>Die Funktion kann aber auch eine kommagetrennte Liste mit Werten verwenden. In diesem Fall wird ebenfalls `0` zurückgegeben: <p>`min(0,1,2)` <p> **Hinweis:** Als Werte sind nur Zahlen zulässig. Wenn der Parameter also ein Array ist, darf dieses ausschließlich Zahlen enthalten. <p> **Parameternummer:** 1 <p> **Name:** Auflistung oder Wert <p> **Beschreibung:** Erforderlich. Entweder ein Array mit Werten, um den kleinsten Wert zu ermitteln, oder der erste Wert einer Gruppe. <p> **Parameternummer:** 2 ... *n* <p> **Name:** Wert *n* <p> **Beschreibung:** Optional. Falls es sich bei dem ersten Parameter um einen Wert handelt, können Sie zusätzliche Werte übergeben. In diesem Fall wird der kleinste aller übergebenen Werte zurückgegeben.|  
|max|Diese Funktion kann auf zwei Arten aufgerufen werden. <p>Im folgenden Beispiel verwendet `max` ein Array, und die Funktion gibt `2` zurück: <p>`max([0,1,2])` <p>Die Funktion kann aber auch eine kommagetrennte Liste mit Werten verwenden. In diesem Fall wird ebenfalls `2` zurückgegeben: <p>`max(0,1,2)` <p> **Hinweis:** Als Werte sind nur Zahlen zulässig. Wenn der Parameter also ein Array ist, darf dieses ausschließlich Zahlen enthalten. <p> **Parameternummer:** 1 <p> **Name:** Auflistung oder Wert <p> **Beschreibung:** Erforderlich. Entweder ein Array mit Werten, um den größten Wert zu ermitteln, oder der erste Wert einer Gruppe. <p> **Parameternummer:** 2 ... *n* <p> **Name:** Wert *n* <p> **Beschreibung:** Optional. Falls es sich bei dem ersten Parameter um einen Wert handelt, können Sie zusätzliche Werte übergeben. In diesem Fall wird der größte aller übergebenen Werte zurückgegeben.|  
|range|Generiert ein Array mit ganzen Zahlen ab einer bestimmten Zahl. Die Länge des zurückgegebenen Arrays wird von Ihnen definiert. <p>Die folgende Funktion gibt beispielsweise `[3,4,5,6]` zurück: <p>`range(3,4)` <p> **Parameternummer:** 1 <p> **Name:** Startindex <p> **Beschreibung:** Erforderlich. Die erste ganze Zahl im Array. <p> **Parameternummer:** 2 <p> **Name:** Anzahl <p> **Beschreibung:** Erforderlich. Die Anzahl von ganzen Zahlen im Array.|  
|rand|Generiert nach dem Zufallsprinzip eine ganze Zahl aus dem angegebenen Bereich (einschließlich Anfang und Ende). Die folgende Funktion kann beispielsweise `42` zurückgeben: <p>`rand(-1000,1000)` <p> **Parameternummer:** 1 <p> **Name:** Mindestwert <p> **Beschreibung:** Erforderlich. Die kleinste ganze Zahl, die zurückgegeben werden kann. <p> **Parameternummer:** 2 <p> **Name:** Maximalwert <p> **Beschreibung:** Erforderlich. Die größte ganze Zahl, die zurückgegeben werden kann.|  
  
### <a name="date-functions"></a>Datumsfunktionen  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|utcnow|Gibt den aktuellen Zeitstempel als Zeichenfolge zurück. Beispiel: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Parameternummer:** 1 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|addseconds|Addiert eine ganzzahlige Anzahl von Sekunden zu einem übergebenen Zeichenfolgenzeitstempel. Die Sekundenanzahl kann positiv oder negativ sein. Sofern kein Formatbezeichner angegeben wird, ist das Ergebnis standardmäßig eine Zeichenfolge im ISO 8601-Format („o“). Beispiel: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Parameternummer:** 1 <p> **Name:** Zeitstempel <p> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit. <p> **Parameternummer:** 2 <p> **Name:** Sekunden <p> **Beschreibung:** Erforderlich. Die Anzahl zu addierender Sekunden. Bei einem negativen Wert werden die Sekunden subtrahiert. <p> **Parameternummer:** 3 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|addminutes|Addiert eine ganzzahlige Anzahl von Minuten zu einem übergebenen Zeichenfolgenzeitstempel. Die Minutenanzahl kann positiv oder negativ sein. Sofern kein Formatbezeichner angegeben wird, ist das Ergebnis standardmäßig eine Zeichenfolge im ISO 8601-Format („o“). Beispiel: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Parameternummer:** 1 <p> **Name:** Zeitstempel <p> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit. <p> **Parameternummer:** 2 <p> **Name:** Minuten <p> **Beschreibung:** Erforderlich. Die Anzahl zu addierender Minuten. Bei einem negativen Wert werden die Minuten subtrahiert. <p> **Parameternummer:** 3 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|addhours|Addiert eine ganzzahlige Anzahl von Stunden zu einem übergebenen Zeichenfolgenzeitstempel. Die Stundenanzahl kann positiv oder negativ sein. Sofern kein Formatbezeichner angegeben wird, ist das Ergebnis standardmäßig eine Zeichenfolge im ISO 8601-Format („o“). Beispiel: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Parameternummer:** 1 <p> **Name:** Zeitstempel <p> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit. <p> **Parameternummer:** 2 <p> **Name:** Stunden <p> **Beschreibung:** Erforderlich. Die Anzahl zu addierender Stunden. Bei einem negativen Wert werden die Stunden subtrahiert. <p> **Parameternummer:** 3 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|adddays|Addiert eine ganzzahlige Anzahl von Tagen zu einem übergebenen Zeichenfolgenzeitstempel. Die Tagesanzahl kann positiv oder negativ sein. Sofern kein Formatbezeichner angegeben wird, ist das Ergebnis standardmäßig eine Zeichenfolge im ISO 8601-Format („o“). Beispiel: `2015-02-23T13:27:36Z`: <p>`addseconds('2015-03-15T13:27:36Z', -20)` <p> **Parameternummer:** 1 <p> **Name:** Zeitstempel <p> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit. <p> **Parameternummer:** 2 <p> **Name:** Tage <p> **Beschreibung:** Erforderlich. Die Anzahl zu addierender Tage. Bei einem negativen Wert werden die Tage subtrahiert. <p> **Parameternummer:** 3 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|formatDateTime|Gibt eine Zeichenfolge im Datumsformat zurück. Sofern kein Formatbezeichner angegeben wird, ist das Ergebnis standardmäßig eine Zeichenfolge im ISO 8601-Format („o“). Beispiel: `2015-02-23T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Parameternummer:** 1 <p> **Name:** Datum <p> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit dem Datum. <p> **Parameternummer:** 2 <p> **Name:** Format <p> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|startOfHour|Gibt für einen übergebenen Zeichenfolgenzeitstempel den Beginn der Stunde zurück. Beispiel: `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.<br /><br />**Parameternummer:** 2<br /><br /> **Name:** Format<br /><br /> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.|  
|startOfDay|Gibt für einen übergebenen Zeichenfolgenzeitstempel den Beginn des Tages zurück. Beispiel: `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.<br /><br />**Parameternummer:** 2<br /><br /> **Name:** Format<br /><br /> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.| 
|startOfMonth|Gibt für einen übergebenen Zeichenfolgenzeitstempel den Beginn des Monats zurück. Beispiel: `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.<br /><br />**Parameternummer:** 2<br /><br /> **Name:** Format<br /><br /> **Beschreibung:** Optional. Entweder ein [Formatbezeichner aus einem einzelnen Zeichen](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) oder ein [benutzerdefiniertes Formatmuster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx), mit dem die gewünschte Formatierung für den Wert des Zeitstempels angegeben wird. Ohne Formatangabe wird das ISO 8601-Format („o“) verwendet.| 
|dayOfWeek|Gibt die Wochentagkomponente eines Zeichenfolgenzeitstempels zurück.  Sonntag ist 0, Montag ist 1 usw. Beispiel: `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.| 
|dayOfMonth|Gibt die Monatstagkomponente eines Zeichenfolgenzeitstempels zurück. Beispiel: `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.| 
|dayOfYear|Gibt die Jahrestagkomponente eines Zeichenfolgenzeitstempels zurück. Beispiel: `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.| 
|ticks|Gibt die Zeiteinheitseigenschaft eines Zeichenfolgenzeitstempels zurück. Beispiel: `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Parameternummer:** 1<br /><br /> **Name:** Zeitstempel<br /><br /> **Beschreibung:** Erforderlich. Eine Zeichenfolge mit der Uhrzeit.| 
  
### <a name="workflow-functions"></a>Workflowfunktionen  

Mit diesen Funktionen können Sie zur Laufzeit Informationen zum eigentlichen Workflow abrufen.  
  
|Funktionsname|Beschreibung|  
|-------------------|-----------------|  
|listCallbackUrl|Gibt eine Zeichenfolge zurück, die aufgerufen werden kann, um den Trigger oder die Aktion aufzurufen. <p> **Hinweis:** Diese Funktion kann nur im Kontext von **httpWebhook** und **apiConnectionWebhook**, aber nicht im Kontext von **manual**, **recurrence**, **http** oder **apiConnection** verwendet werden. <p>Die Funktion `listCallbackUrl()` gibt beispielsweise Folgendes zurück: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|workflow|Diese Funktion liefert zur Laufzeit sämtliche Informationen zum eigentlichen Workflow. Sie können alles abrufen, was in der Verwaltungs-API zur Verfügung steht. Hierzu zählen beispielsweise Name, Standort und Ressourcen-ID. Ausführliche Informationen zu diesen Eigenschaften finden Sie unter [Workflows](http://go.microsoft.com/fwlink/p/?LinkID=525617). Die folgende Funktion gibt beispielsweise einen Standort (etwa `westus`) zurück: <p>`workflow().location` <p> **Hinweis:** `@workflow` wird derzeit innerhalb von Triggern unterstützt.|

## <a name="next-steps"></a>Nächste Schritte

[Workflowaktionen und -trigger](logic-apps-workflow-actions-triggers.md)

