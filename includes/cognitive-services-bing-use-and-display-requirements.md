07.07.2017

Die hier angegebenen Verwendungs- und Anzeigeanforderungen gelten für Ihre Implementierung der Inhalte und der dazugehörigen Informationen (wie etwa Beziehungen, Metadaten und andere Signale), die über Aufrufe der Bing-Websuche-, Bing-Bildersuche-, Bing-Videosuche-, Bing-News-Suche und Bing-Entitätssuche-API sowie der Bing-Rechtschreibprüfungs-API und der Bing-Vorschlagssuche-API verfügbar sind. Implementierungsdetails im Zusammenhang mit diesen Anforderungen finden Sie in der Dokumentation für bestimmte Features und Ergebnisse.

## <a name="1-bing-spell-check-api-and-bing-autosuggest-api"></a>1. BING-RECHTSCHREIBPRÜFUNGS-API und BING-VORSCHLAGSSUCHE-API.

Folgendes ist untersagt:

•   Kopieren, Speichern oder Zwischenspeichern von Daten, die Sie von der Bing-Rechtschreibprüfungs-API oder von der Bing-Vorschlagssuche-API erhalten. 
  
•   Verwenden von Daten, die Sie von der Bing-Rechtschreibprüfungs-API oder Bing-Vorschlagssuche-API erhalten, im Rahmen einer Machine Learning-Aktivität oder einer ähnlichen algorithmischen Aktivität zum Trainieren, Auswerten oder Verbessern neuer oder vorhandener Dienste, die von Ihnen selbst oder von Dritten angeboten werden.

## <a name="2-search-apis"></a>2. SUCH-APIS

Die Anforderungen der Abschnitte 3 bis 7 gelten für die Bing-Websuche-, Bing-Bildersuche-, Bing-Videosuche- und Bing-News-Suche-API sowie für die Bing-Entitätssuche-API (**Such-APIs**). Die folgenden Definitionen gelten für die Abschnitte 3 bis 7:

•   „Antwort“ bezieht sich auf eine Kategorie von Ergebnissen, die in einer Reaktion zurückgegeben werden. Eine Reaktion der Bing-Websuche-API kann beispielsweise Antworten in den Kategorien „Webseitenergebnisse“, „Bild“, „Video“ und „News“ enthalten.  
  
•   Als „Reaktion“ werden sämtliche Antworten und dazugehörigen Daten bezeichnet, die infolge eines einzelnen Aufrufs einer Such-API empfangen werden.  
  
•   „Ergebnis“ bezieht sich auf ein Informationselement in einer Antwort. Die Daten in Verbindung mit einem einzelnen Nachrichtenartikel sind beispielsweise ein Ergebnis in einer Nachrichtenantwort.

## <a name="3-bing-entity-search-api"></a>3. BING-ENTITÄTSSUCHE-API

Für von der Bing-Entitätssuche-API zurückgegebene Daten gilt Folgendes:

•   Sie dürfen nur Benutzern in den Vereinigten Staaten von Amerika angeboten werden.  

•   Sie dürfen nur als Reaktion auf das Entitätssuchinteresse des Endbenutzers angezeigt werden (beispielsweise im Rahmen einer vom Benutzer angegebenen Abfrage).  
  
•   Sie müssen über einen sichtbaren Link zur bing.com-URL aus der Reaktion verfügen, über den der Benutzer zu den Suchergebnissen der relevanten Abfrage auf bing.com gelangt.  
  
•   Sie müssen im Einklang mit jeglichen weiteren geeigneten Maßnahmen verwendet werden, die sicherstellen, dass Ihre Verwendung der Daten von der Bing-Entitätssuche-API nicht gegen geltende Gesetze verstößt oder die Rechte Dritter verletzt. Beispiel:  
  
  - Es wird sichergestellt, dass Miniaturansichten von Bildern die Größe von Miniaturansichten haben (relativ zum Display des Benutzers). Oder:  
    
  - Bei Verwendung einer Creative Commons-Lizenz wird eine geeignete Lizenz- und Textzuordnung sichergestellt.

Folgendes ist untersagt:

•   Kopieren, Speichern oder Zwischenspeichern von Daten, die Sie von der Bing-Entitätssuche-API erhalten.  
  
•   Verwenden von Daten, die Sie von der Bing-Entitätssuche-API erhalten, im Rahmen einer Machine Learning-Aktivität oder einer ähnlichen algorithmischen Aktivität zum Trainieren, Auswerten oder Verbessern neuer oder vorhandener Dienste, die von Ihnen selbst oder von Dritten angeboten werden.

## <a name="4-bing-web-search-image-search-news-search-and-video-search-apis"></a>4. BING-WEBSUCHE-, BING-BILDERSUCHE-, BING-NEWS-SUCHE- und BING-VIDEOSUCHE-API:

**Internetsuche.** Alle Daten, die in einer Reaktion der Bing-Websuche-, Bing-Bildersuche-, Bing-News-Suche- und Bing-Videosuche-API zurückgegeben werden, dürfen ausschließlich im Rahmen von Internetsuchvorgängen verwendet werden. Internetsuche bedeutet, dass für den angezeigten Inhalt Folgendes gilt (sofern zutreffend):

•   Der Inhalt ist für die direkte Abfrage des Endbenutzers oder für einen anderen Hinweis auf das Suchinteresse oder die Suchabsicht des Benutzers (beispielsweise eine vom Benutzer angegebene Suchabfrage) relevant und eine Reaktion darauf.  
  
•   Der Inhalt hilft Benutzern dabei, Datenquellen zu finden und aufzurufen. (Beispiel: Die bereitgestellten URLs werden als Links implementiert, sodass es sich bei dem Inhalt oder der Namensnennung um einen klickbaren Link handelt, der gut sichtbar zusammen mit den Daten angezeigt wird.)  
  
•   Der Inhalt enthält mehrere Ergebnisse, aus denen der Endbenutzer wählen kann. (Beispiel: Es werden mehrere Ergebnisse aus der Nachrichtenantwort angezeigt – oder alle, falls weniger Ergebnisse zurückgegeben werden.)  
  
•   Der Inhalt ist auf einen für den Zweck der Suche angemessenen Umfang beschränkt. (Beispiel: Miniaturansichten von Bildern haben relativ zum Display des Benutzers die Größe von Miniaturansichten.)  
  
•   Der Inhalt ist für den Endbenutzer gut sichtbar als Internetsuchergebnis gekennzeichnet (etwa durch einen Hinweis wie „Aus dem Web“). Und:  
  
•   Der Inhalt umfasst eine beliebige andere Kombination von geeigneten Maßnahmen, die sicherstellen, dass Ihre Verwendung von Daten, die von den Such-APIs empfangen wurden, nicht gegen geltende Gesetze verstößt oder die Rechte Dritter verletzt. Wenden Sie sich gegebenenfalls an Ihre Rechtsberater, um zu klären, welche Maßnahmen hierfür geeignet sind.

Die einzige Ausnahme für die Anforderung bei der Internetsuche betrifft die URL-Ermittlung, wie weiter unten in Abschnitt 7 (Ermittlung von Nicht-Anzeige-URLs) beschrieben.

**Allgemeines.** Folgendes ist untersagt: 

•   Kopieren, Speichern oder Zwischenspeichern von Daten aus Reaktionen (mit Ausnahme der zulässigen Aufbewahrung gemäß dem Abschnitt zur Dienstkontinuität weiter unten).  
  
•   Ändern des Inhalts von Ergebnissen (mit Ausnahme der Anpassung der Formatierung unter Einhaltung sämtlicher anderer Anforderungen).  
  
•   Entfernen der dem Ergebnisinhalt zugeordneten Namensnennung und URLs.  
  
•   Ändern der Reihenfolge (einschließlich Auslassung) von Ergebnissen in einer Antwort, wenn eine Reihen- oder Rangfolge bereitgestellt wird.  
  
•   Anzeigen anderer Inhalte innerhalb eines beliebigen Teils einer Reaktion auf eine Weise, die einem Endbenutzer vorspiegelt, der andere Inhalt sei Teil der Reaktion.  
  
•   Anzeigen von Werbung, die nicht von Microsoft bereitgestellt wird, auf einer beliebigen Seite, auf der ein beliebiger Teil einer Reaktion angezeigt wird.  
  
•   Verwenden von Daten, die Sie von den Such-APIs erhalten, im Rahmen einer Machine Learning-Aktivität oder einer ähnlichen algorithmischen Aktivität zum Trainieren, Auswerten oder Verbessern neuer oder vorhandener Dienste, die von Ihnen selbst oder von Dritten angeboten werden.

**Werbung.** Werbung darf unabhängig davon, ob Sie von Microsoft oder von einem anderen Anbieter bereitgestellt wird, nicht zusammen mit Antworten angezeigt werden, die (i) von der Bildsuche-API, News-Suche-API oder Videosuche-API stammen oder die (ii) hauptsächlich (oder ausschließlich) nach Bild-, Nachrichten- und/oder Videoergebnissen von anderen Such-APIs gefiltert oder darauf beschränkt sind.

**Branding.** Jede angezeigte Reaktion (bzw. jeder angezeigte Teil einer Reaktion) darf wie unter „https://go.microsoft.com/fwlink/?linkid=833278“ erläutert Microsoft zugeschrieben werden, sofern Microsoft für Ihre spezielle Anwendung keine anderslautenden Angaben gemacht hat.

## <a name="5-transferring-responses"></a>5. Weiterleitung von Reaktionen.

Wenn Sie einem Benutzer die Weiterleitung einer Reaktion von einer Such-API an einen anderen Benutzer ermöglichen (etwa über eine Messaging-App oder über einen Beitrag in sozialen Medien), gilt Folgendes:

•   Weitergeleitete Reaktionen müssen folgende Anforderungen erfüllen:  
  
  - Der Inhalt muss exakt dem Inhalt entsprechen, der dem weiterleitenden Benutzer angezeigt wurde. (Die Formatierung darf jedoch geändert werden.)  
    
  - Es dürfen keine Daten in Metadatenform enthalten sein.  
    
  - Sie müssen einen Hinweis darauf enthalten, dass der Reaktion eine Internetsuche mit Bing zugrunde liegt (beispielsweise „Unterstützt von Bing“, „Weitere Informationen zu diesem Bild auf Bing“, „Mehr zu diesem Bild auf Bing“ oder Verwendung des Bing-Logos).  
    
  - Sie müssen gut sichtbar die vollständige Abfrage anzeigen, die zum Generieren der Reaktion verwendet wurde. Und:  
    
  - Sie müssen einen gut sichtbaren Link oder eine ähnliche Nennung der Quelle enthalten, die der Reaktion zugrunde liegt – entweder direkt oder über „bing.com“ oder „m.bing.com“.  
    
•   Die Weiterleitung von Reaktionen darf nicht automatisiert werden. Eine Weiterleitung muss durch eine Benutzeraktion initiiert werden, die eindeutig auf die Absicht zurückzuführen ist, eine Reaktion weiterzuleiten.  
  
•   Einem Benutzer darf nur die Weiterleitung von Reaktionen ermöglicht werden, die im Zuge der Abfrage des weiterleitenden Benutzers entstanden sind.

## <a name="6-continuity-of-service"></a>6. Dienstkontinuität.

Daten aus Reaktionen von Such-APIs dürfen nicht kopiert, gespeichert oder zwischengespeichert werden. Zur Gewährleistung der Kontinuität beim Dienstzugriff und beim Rendern von Daten ist es unter folgenden Bedingungen jedoch gestattet, Ergebnisse aufzubewahren:

**Gerät.** Sie können einem Endbenutzer die Aufbewahrung von Ergebnissen auf einem Gerät für den kürzeren der beiden folgenden Zeiträume ermöglichen: (i) 24 Stunden ab dem Zeitpunkt der Abfrage oder (ii) bis ein Endbenutzer eine weitere Abfrage für aktualisierte Ergebnisse übermittelt – vorausgesetzt, die aufbewahrten Ergebnisse werden ausschließlich zu folgenden Zwecken verwendet:

•   Um dem Endbenutzer Zugriff auf Ergebnisse zu ermöglichen, die zuvor an diesen Endbenutzer auf diesem Gerät zurückgegeben wurden (etwa im Falle einer Dienstunterbrechung). Oder:  
  
•   Zur Speicherung von Ergebnissen, die für Ihre proaktive Abfrage zurückgegeben wurden, welche in Erwartung des Endbenutzerbedarfs auf der Grundlage der Signale des Endbenutzers personalisiert wurde (etwa bei einer erwarteten Dienstunterbrechung).

**Server.** Spezifische Ergebnisse für einen einzelnen Endbenutzer dürfen sicher auf einem von Ihnen kontrollierten Server aufbewahrt werden, und die aufbewahrten Ergebnisse dürfen angezeigt werden, allerdings ausschließlich zu folgenden Zwecken:

•   Um dem Benutzer den Zugriff auf einen Verlaufsbericht mit Ergebnissen zu ermöglichen, die diesem Benutzer in der Vergangenheit in Ihrer Lösung zurückgegebenen wurden – vorausgesetzt, die Ergebnisse werden (i) nicht länger als 21 Tage ab dem Zeitpunkt der ursprünglichen Abfrage des Endbenutzers aufbewahrt und (ii) nicht als Reaktion auf eine neue oder wiederholte Abfrage eines Endbenutzers angezeigt. Oder:  
  
•   Zur Speicherung von Ergebnissen, die für Ihre proaktive Abfrage zurückgegeben wurden, welche in Erwartung des Endbenutzerbedarfs auf der Grundlage der Signale des Endbenutzers personalisiert wurde – für den kürzeren der beiden folgenden Zeiträume: (i) 24 Stunden ab dem Zeitpunkt der Abfrage oder (ii) bis ein Endbenutzer eine weitere Abfrage übermittelt, um aktualisierte Ergebnisse zu erhalten.

Aufbewahrte Ergebnisse für einen bestimmten Benutzer dürfen in keinem Fall mit Ergebnissen für einen anderen Benutzer vermischt werden. Mit anderen Worten: Die Ergebnisse einzelner Benutzer müssen jeweils separat aufbewahrt und bereitgestellt werden.

**Allgemeines.** Bei jeder Präsentation aufbewahrter Ergebnisse müssen folgende Anforderungen erfüllt sein:

•   Der Zeitpunkt, zu dem die Abfrage gesendet wurde, muss eindeutig und gut sichtbar angegeben sein.  
  
•   Dem Benutzer muss eine Schaltfläche oder ein ähnliches Element zur Verfügung stehen, mit der bzw. mit dem er die Abfrage erneut ausführen und aktualisierte Ergebnisse abrufen kann.  
  
•   Bei der Präsentation der Ergebnisse muss das Bing-Branding erhalten bleiben. Und:  
  
•   Die gespeicherten Ergebnisse müssen innerhalb der angegebenen Fristen gelöscht (und bei Bedarf mit einer neuen Abfrage aktualisiert) werden.

## <a name="7-non-display-url-discovery"></a>7. Ermittlung von Nicht-Anzeige-URLs.

Suchreaktionen dürfen in nicht internetbasierten Suchen ausschließlich zur Ermittlung der URLs von Informationsquellen verwendet werden, bei denen es sich um eine Reaktion auf eine Abfrage Ihres Benutzers oder Kunden handelt. Sie dürfen solche URLs in einen Bericht oder in eine ähnliche Reaktion kopieren, den bzw. die Sie (i) ausschließlich für diesen Benutzer oder Kunden als Reaktion auf die spezifische Abfrage bereitstellen und (ii) der bzw. die aussagekräftige, nützliche Zusatzinhalte enthält, die für die Abfrage relevant sind. Die Anforderungen in den Abschnitten 2 bis 6 dieser Verwendungs- und Anzeigeanforderungen gelten nicht für diese Nicht-Anzeige-Verwendung, allerdings mit folgenden Ausnahmen: 

•   Mit Ausnahme des eingeschränkten Kopierens von URLs (siehe oben) dürfen keine Daten und keine Inhalte, die aus der Suchreaktion stammen oder daraus abgeleitet wurden, zwischengespeichert, kopiert oder gespeichert werden.  
  
•   Sie müssen dafür Sorge tragen, dass Ihre Verwendung von Daten (einschließlich URLs), die von den Such-APIs empfangen wurden, nicht gegen geltende Gesetze verstößt oder die Rechte Dritter verletzt. Und:  
  
•   Sie dürfen die Daten (einschließlich URLs), die Sie von den Such-APIs erhalten, nicht im Rahmen einer Machine Learning-Aktivität oder einer ähnlichen algorithmischen Aktivität zum Trainieren, Auswerten oder Verbessern von Diensten verwenden, die von Ihnen selbst oder von Dritten angeboten werden.

