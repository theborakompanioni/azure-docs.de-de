<properties
   pageTitle="Interagieren mit Berichten mithilfe der JavaScript-API | Microsoft Azure"
   description="Power BI Embedded, Interagieren mit Berichten mithilfe der JavaScript-API"
   services="power-bi-embedded"
   documentationCenter=""
   authors="jocaplan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="08/26/2016"
   ms.author="jocaplan"/>

# Interagieren mit Power BI-Berichten mithilfe der JavaScript-API

Mithilfe der Power BI-JavaScript-API können Sie mühelos Power BI-Berichte in Ihre Anwendungen einbetten. Mit der API können die Anwendungen programmgesteuert mit anderen Berichtselementen wie Seiten und Filtern interagieren. Diese Interaktivität macht Power BI-Berichte zu einem integrierten Bestandteil Ihrer Anwendung.

Zum Einbetten eines Power BI-Berichts in Ihre Anwendung verwenden Sie einen IFrame, der als Teil der Anwendung gehostet wird. Wie Sie in der folgenden Abbildung sehen können, fungiert der IFrame als Grenze zwischen Ihrer Anwendung und dem Bericht.

![Power BI Embedded, IFrame ohne JavaScript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Der IFrame erleichtert den Einbettungsprozess erheblich, ohne die JavaScript-API können der Bericht und die Anwendung jedoch nicht miteinander interagieren. Durch diese mangelnde Interaktion kann der Eindruck entstehen, dass der Bericht nicht wirklich ein Teil der Anwendung ist. Der Bericht und die Anwendung müssen wie in der folgenden Abbildung gezeigt in beiden Richtungen miteinander kommunizieren.

![Power BI Embedded, IFrame mit JavaScript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Die Power BI-JavaScript-API ermöglicht Ihnen das Schreiben von Code, der die IFrame-Grenze sicher passieren kann. Dadurch kann die Anwendung programmgesteuert eine Aktion in einem Bericht ausführen und auf Ereignisse von Aktionen lauschen, die Benutzer innerhalb des Berichts durchführen.

## Welche Möglichkeiten bietet Ihnen die Power BI-JavaScript-API?
Mit der JavaScript-API können Sie Berichte verwalten, zu Seiten in einem Bericht navigieren, einen Bericht filtern und Einbettungsereignisse behandeln. Das folgende Diagramm zeigt die Struktur der API.

![Power BI-JavaScript-API – Diagramm](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### Verwalten von Berichten
Mit der JavaScript-API können Sie das Verhalten auf der Berichts- und Seitenebene verwalten:

- Sicheres Einbetten eines bestimmten Power BI-Berichts in die Anwendung (testen Sie die [Demoanwendung zum Einbetten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1))
  - Festlegen des Zugriffstokens
- Konfigurieren des Berichts
  - Aktivieren und Deaktivieren der Filter- und Seitennavigationsbereiche (testen Sie die [Demoanwendung zum Aktualisieren von Einstellungen](http://azure-samples.github.io/powerbi-angular-client/#/scenario6))
  - Festlegen von Standardeinstellungen für Seiten und Filter (testen Sie die [Demo zu Standardeinstellungen](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Aktivieren und Deaktivieren des Vollbildmodus

[Weitere Informationen zum Einbetten eines Berichts](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### Navigieren zu Seiten in einem Bericht
Mit der JavaScript-API können Sie alle Seiten in einem Bericht ermitteln und die aktuelle Seite festlegen. Testen Sie die [Demoanwendung zur Navigation](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Weitere Informationen zur Seitennavigation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### Filtern eines Berichts
Die JavaScript-API bietet einfache und erweiterte Filterfunktionen für eingebettete Berichte und Berichtsseiten. Testen Sie die [Demoanwendung zum Filtern](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), und sehen Sie sich einige einführende Codebeispiele an.


#### Einfache Filter
Ein einfacher Filter wird auf eine Spalte oder Hierarchieebene angewendet und enthält eine Liste von Werten, die ein- oder ausgeschlossen werden sollen.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### Erweiterte Filter
Erweiterte Filter verwenden den logischen Operator AND oder OR und akzeptieren eine oder zwei Bedingungen, die jeweils über einen eigenen Operator und Wert verfügen. Folgende Bedingungen werden unterstützt:

- Keine
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Contains
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Weitere Informationen zum Filtern](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### Behandeln von Ereignissen
Ihre Anwendung kann nicht nur Informationen in den IFrame senden, sondern auch Informationen zu den folgenden Ereignissen aus dem IFrame empfangen:

- Einbetten
  - loaded
  - Fehler
- Berichte
  - pageChanged
  - dataSelected (demnächst verfügbar)

[Weitere Informationen zur Behandlung von Ereignissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## Nächste Schritte
Weitere Informationen zur Power BI-JavaScript-API finden Sie unter folgenden Links:

- [JavaScript-API-Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Objektmodellreferenz](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Beispiele
  - [Angular](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Livedemo](https://microsoft.github.io/PowerBI-JavaScript/demo/)

<!---HONumber=AcomDC_0831_2016-->