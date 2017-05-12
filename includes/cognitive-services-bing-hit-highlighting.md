Für Bing wird die Treffermarkierung unterstützt, bei der Abfrageausdrücke (oder andere Ausdrücke, die von Bing als relevant eingestuft werden) in den Anzeigezeichenfolgen einiger Antworten markiert werden. Mit den Feldern `name`, `displayUrl` und `snippet` können beispielsweise die Abfrageausdrücke markiert werden.

Standardmäßig werden in Bing keine Hervorhebungsmarkierungen in Anzeigezeichenfolgen eingefügt. Fügen Sie zum Einbinden der Markierungen den Abfrageparameter `textDecorations` in Ihre Anforderung ein, und legen Sie ihn auf **true** fest. In Bing werden die Abfrageausdrücke mit den Unicode-Zeichen E000 und E001 markiert, um den Anfang und das Ende des Ausdrucks anzugeben. Falls der Abfrageausdruck beispielsweise „Sailing Dinghy“ lautet und beide Ausdrücke im Feld vorhanden sind, wird der Ausdruck wie im folgenden Beispiel in Zeichen für die Treffermarkierung gesetzt:  
  
![Treffermarkierung](./media/cognitive-services-bing-hit-highlighting/bing-hit-highlighting.PNG) 

Bevor die Zeichenfolge auf Ihrer Benutzeroberfläche angezeigt wird, sollten Sie die Unicode-Zeichen durch Zeichen ersetzen, die für das gewünschte Anzeigeformat geeignet sind. Wenn Sie den Text beispielsweise im HTML-Format anzeigen, können Sie den Abfrageausdruck hervorheben, indem Sie E000 durch „<b\>“ und E001 durch „</b\>“ ersetzen. Entfernen Sie die Markierungen der Zeichenfolge, falls Sie keine Formatierung anwenden möchten. 

In Bing haben Sie die Möglichkeit, Unicode-Zeichen oder HTML-Tags als Markierungen bereitzustellen. Fügen Sie den Abfrageparameter `textFormat` ein, um anzugeben, welche Markierungen verwendet werden sollen. Legen Sie `textFormat` auf „Raw“ (Standardeinstellung) fest, um den Inhalt mit Unicode-Zeichen zu markieren, und `textFormat` auf „HTML“, um den Inhalt mit HTML-Tags zu markieren. 
  
Wenn `textDecorations` auf **true** festgelegt ist, können in Bing die unten angegebenen Markierungen in die Anzeigezeichenfolgen von Antworten eingefügt werden. Falls keine HTML-Entsprechung vorhanden ist, enthält die Spalte „HTML“ der Tabelle keine Angabe.

|Unicode|HTML|Beschreibung
|-|-|-
|U+E000|\<b>|Markiert den Anfang des Abfrageausdrucks (Treffermarkierung)
|U+E001|\</b>|Markiert das Ende des Abfrageausdrucks
|U+E002|\<i>|Markiert den Anfang von Text in Kursivdruck 
|U+E003|\</i>|Markiert das Ende von Text in Kursivdruck
|U+E004|\<br/>|Markiert einen Zeilenumbruch
|U+E005||Markiert den Anfang einer Telefonnummer
|U+E006||Markiert das Ende einer Telefonnummer
|U+E007||Markiert den Anfang einer Adresse
|U+E008||Markiert das Ende einer Adresse
|U+E009|\&nbsp;|Markiert ein geschütztes Leerzeichen
|U+E00C|\<strong>|Markiert den Anfang von Text in Fettdruck
|U+E00D|\</strong>|Markiert das Ende von Text in Fettdruck
|U+E00E||Markiert den Anfang von Text, dessen Hintergrund heller als der umgebende Hintergrund sein soll
|U+E00F||Markiert das Ende von Text, dessen Hintergrund heller als der umgebende Hintergrund sein soll
|U+E010||Markiert den Anfang von Text, dessen Hintergrund dunkler als der umgebende Hintergrund sein soll
|U+E011||Markiert das Ende von Text, dessen Hintergrund dunkler als der umgebende Hintergrund sein soll
|U+E012|\<del>|Markiert den Anfang von Text, der durchgestrichen sein soll
|U+E013|\</del>|Markiert das Ende von Text, der durchgestrichen sein soll
|U+E016|\<sub>|Markiert den Anfang von tiefgestelltem Text
|U+E017|\</sub>|Markiert das Ende von tiefgestelltem Text
|U+E018|\<sup>|Markiert den Anfang von hochgestelltem Text
|U+E019|\</sup>|Markiert das Ende von hochgestelltem Text

Das folgende Beispiel enthält die Antwort `Computation` mit Tiefstellungsmarkierungen für einen log(2)-Abfrageausdruck. Das Feld `expression` enthält die Markierungen nur dann, wenn „textDecoration“ auf **true** festgelegt ist.

![Computation-Markierungen](./media/cognitive-services-bing-hit-highlighting/bing-markers-computation.PNG) 

Wenn für die Anforderung keine Anpassungen angefordert werden, lautet der Ausdruck „log10(2)“. 
  
