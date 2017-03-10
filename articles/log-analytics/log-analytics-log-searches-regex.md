---
title: "Reguläre Ausdrücke in OMS Log Analytics-Protokollsuchen | Microsoft-Dokumentation"
description: "Sie können mit dem RegEx-Schlüsselwort in Log Analytics-Protokollsuchen die Ergebnisse nach einem regulären Ausdruck filtern.  Dieser Artikel enthält die Syntax für diese Ausdrücke mit mehreren Beispielen."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 73739f4f154bebe271ce29bd285122ea7f56d769
ms.openlocfilehash: bcf36cdec6c1dda7aa0213c42adf8d0281dc28d2
ms.lasthandoff: 02/23/2017


---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Verwenden regulärer Ausdrücke zum Filtern der Protokollsuchen in Log Analytics

[Protokollsuchen](log-analytics-log-searches.md) ermöglichen Ihnen, Informationen aus dem Log Analytics-Repository zu extrahieren.  [Filterausdrücke](log-analytics-search-reference.md#filter-expression) ermöglichen Ihnen, die Ergebnisse der Suche nach bestimmten Kriterien zu filtern.  Das **RegEx**-Schlüsselwort ermöglicht Ihnen, einen regulären Ausdruck für diesen Filter anzugeben.  

Dieser Artikel enthält nähere Informationen zur Syntax regulärer Ausdrücke, die von Log Analytics verwendet wird.


## <a name="regex-keyword"></a>RegEx-Schlüsselwort

Verwenden Sie die folgende Syntax, um das **RegEx**-Schlüsselwort in einer Protokollsuche einzusetzen.  Sie können die anderen Abschnitte in diesem Artikel verwenden, um die Syntax des regulären Ausdrucks selbst zu bestimmen. 

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Um beispielsweise einen regulären Ausdruck zur Rückgabe von Warnungsdatensätzen des Typs *Warnung* oder *Fehler* einzusetzen, verwenden Sie die folgende Protokollsuche. 

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Teilweise Übereinstimmungen
Beachten Sie, dass der reguläre Ausdruck mit dem gesamten Text der Eigenschaft übereinstimmen muss.  Bei teilweisen Übereinstimmungen werden keine Datensätze zurückgegeben.  Wenn Sie z.B. versucht haben, die von einem Computer mit dem Namen „srv01.contoso.com“ stammenden Datensätze zurückzugeben, würde die folgende Protokollsuche **keine** Datensätze zurückgeben.

    Computer=RegEx("srv..") 

Dies liegt daran, dass nur der erste Teil des Namens mit dem regulären Ausdruck übereinstimmt.  Die folgenden zwei Protokollsuchen würden von diesem Computer stammende Datensätze zurückgeben, da sie mit dem gesamten Namen übereinstimmen. 

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com") 

## <a name="characters"></a>Zeichen
Geben Sie unterschiedliche Zeichen an.

| Zeichen | Beschreibung | Beispiel | Beispielübereinstimmungen |
|:--|:--|:--|:--|
| a | Ein Vorkommen des Zeichens. | Computer=RegEx("srv01.contoso.com") | srv01.contoso.com |
| . | Ein einzelnes Zeichen. | Computer=RegEx("srv...contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| a? | Null (0) oder ein Vorkommen des Zeichens. | Computer=RegEx("srv01?.contoso.com") | srv0.contoso.com<br>srv01.contoso.com |
| a* | Null (0) oder mehrere Vorkommen des Zeichens. | Computer=RegEx("srv01*.contoso.com") | srv0.contoso.com<br>srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| a+ | Ein oder mehrere Vorkommen des Zeichens. | Computer=RegEx("srv01+.contoso.com") | srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Übereinstimmung mit jedem beliebigen einzelnen Zeichen in den Klammern. | Computer=RegEx("srv0[123].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [*a*-*z*] | Übereinstimmung mit einem einzelnen Zeichen im Bereich.  Kann mehrere Bereiche enthalten. | Computer=RegEx("srv0[1-3].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Keines der Zeichen in den Klammern. | Computer=RegEx("srv0[^123].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [^*a*-*z*] | Keines der Zeichen im Bereich. | Computer=RegEx("srv0[^1-3].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Übereinstimmung mit einem Bereich von numerischen Zeichen. | Computer=RegEx("srv[01-03].contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| @ | Eine beliebige Zeichenfolge. | Computer=RegEx("srv@.contoso.com") | srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Mehrere Vorkommen eines Zeichens
Geben Sie mehrere Vorkommen eines bestimmten Zeichens ein.

| Zeichen | Beschreibung | Beispiel | Beispielübereinstimmungen |
|:--|:--|:--|:--|
| a{n} |  *n* Vorkommen des Zeichens. | Computer=RegEx("bw-win-sc01{3}.bwren.lab") | bw-win-sc0111.bwren.lab |
| a{n,} |  *n* oder mehrere Vorkommen des Zeichens. | Computer=RegEx("bw-win-sc01{3,}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab<br>bw-win-sc0111111.bwren.lab |
| a{n,m} |  *n* bis *m* Vorkommen des Zeichens. | Computer=RegEx("bw-win-sc01{3,5}.bwren.lab") | bw-win-sc0111.bwren.lab<br>bw-win-sc01111.bwren.lab<br>bw-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logische Ausdrücke
Wählen Sie aus mehreren Werten.

| Zeichen | Beschreibung | Beispiel | Beispielübereinstimmungen |
|:--|:--|:--|:--|
| &#124; | Logisches OR.  Gibt das Ergebnis zurück, wenn bei einem der Ausdrücke eine Übereinstimmung auftritt. | Type=Alert AlertSeverity=RegEx("Warnung&#124;Fehler") | Warnung<br>Fehler |
| & | Logisches AND.  Gibt das Ergebnis zurück, wenn bei beiden Ausdrücken eine Übereinstimmung auftritt. | EventData=regex("(Sicherheit.\*&.\*Erfolg.\*)") | Sicherheitsüberwachung erfolgreich |


## <a name="literals"></a>Literale
Konvertieren Sie Sonderzeichen in Literalzeichen.  Dies schließt Zeichen ein, die reguläre Ausdrücke mit Funktionalität versehen, wie z.B. ?-\*^\[\]{}\(\)+\|.&.

| Zeichen | Beschreibung | Beispiel | Beispielübereinstimmungen |
|:--|:--|:--|:--|
| \\ | Konvertiert ein Sonderzeichen in ein Literalzeichen. | Status_CF=\\[Fehler\\]@<br>Status_CF=Fehler\\-@ | [Fehler]Datei nicht gefunden.<br>Fehler-Datei nicht gefunden. |


## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit [Protokollsuchen](log-analytics-log-searches.md) vertraut, um Daten im Log Analytics-Repository anzuzeigen und zu analysieren.

