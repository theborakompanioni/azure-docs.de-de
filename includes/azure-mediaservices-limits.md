>[!NOTE]
>Für Ressourcen ohne feste Vorgabe können Sie die Erhöhung von Kontingenten anfordern, indem Sie ein Supportticket öffnen. Erstellen Sie **keine** zusätzlichen Azure Media Services-Konten, um zu versuchen, höhere Grenzwerte zu erzielen.

| Ressource | Standardlimit | 
| --- | --- | 
| Azure Media Services (AMS)-Konten in einem einzelnen Abonnement | 25 (feststehend) |
| Objekte pro AMS-Konto | 1.000.000|
| Verkettete Aufgaben pro Auftrag | 30 (feststehend) |
| Objekte pro Aufgabe | 50 |
| Objekte pro Auftrag | 100 |
| Aufträge pro AMS-Konto | 50.000<sup>2</sup> |
| Eindeutige Locators, die einem Objekt gleichzeitig zugeordnet sind | 5<sup>4</sup> |
| Livekanäle pro AMS-Konto  |5|
| Programme im angehaltenen Zustand pro Kanal  |50|
| Programme im ausgeführten Zustand pro Kanal  |3|
| Streamingendpunkte im ausgeführten Zustand pro AMS-Konto|2|
| Streamingeinheiten pro Streamingendpunkt  |10 |
| Reservierte Einheiten für Medien (Media Reserved Units; RUs) pro AMS-Konto |25 (S1, S2)<br/>10 (S3) <sup>1</sup> | 
| Speicherkonten | 1.000<sup>5</sup> (feststehend) |
| Richtlinien | |1,000,000<sup>6</sup> |
 
<sup>1</sup> S3 RUs sind nicht in der Region „Indien, Westen“ verfügbar.

<sup>2</sup> Diese Zahl umfasst fertig gestellte, aktive und abgebrochene Aufträge sowie Aufträge in der Warteschlange. Gelöschte Aufträge sind nicht enthalten. Sie können die alten Aufträge mithilfe von **IJob.Delete** oder der HTTP **DELETE**-Anforderung löschen.

<sup>3</sup> Wenn Sie eine Anforderung zum Auflisten von Auftragsentitäten senden, werden maximal 1.000 Entitäten pro Anforderung zurückgegeben. Falls Sie alle gesendeten Aufträge nachverfolgen müssen, können Sie "Nach oben"/"Überspringen" wie in [OData-Systemabfrageoptionen](http://msdn.microsoft.com/library/gg309461.aspx)beschrieben verwenden.

<sup>4</sup> Locators sind nicht für die Verwaltung der Zugriffssteuerung pro Benutzer konzipiert. Um einzelnen Benutzern verschiedene Zugriffsrechte zu erteilen, verwenden Sie Lösungen zur Verwaltung digitaler Rechte (Digital Rights Management, DRM). Weitere Informationen finden Sie in [diesem](../articles/media-services/media-services-content-protection-overview.md) Abschnitt.

<sup>5</sup> Die Speicherkonten müssen aus demselben Azure-Abonnement stammen.

<sup>6</sup> Es gilt ein Grenzwert von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. für die Locator-Richtlinie oder für ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Wenn Sie immer die gleichen Tage und Zugriffsberechtigungen usw. verwenden, sollten Sie die gleiche Richtlinien-ID verwenden.



<!--HONumber=Jan17_HO2-->


