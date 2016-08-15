<properties 
	pageTitle="Übersicht über X12 und das Enterprise Integration Pack | Microsoft Azure App Service | Microsoft Azure" 
	description="Erfahren Sie, wie Sie X12-Vereinbarungen zum Erstellen von Logik-Apps verwenden" 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="app-service-logic" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/08/2016" 
	ms.author="deonhe"/>

# Unternehmensintegration mit X12 

>[AZURE.NOTE]Auf dieser Seite werden die X12-Features von Logik-Apps beschrieben. Informationen zu EDIFACT finden Sie [hier](app-service-logic-enterprise-integration-edifact.md).

## Erstellen einer X12-Vereinbarung 
Bevor Sie X12-Nachrichten austauschen können, müssen Sie eine X12-Vereinbarung erstellen und in Ihrem Integrationskonto speichern. In den folgenden Schritten werden Sie durch das Erstellen einer X12-Vereinbarung begleitet.

### Bevor Sie beginnen, benötigen Sie Folgendes:
- Ein in Ihrem Azure-Abonnement definiertes [Integrationskonto](./app-service-logic-enterprise-integration-accounts.md)
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss der Inhalt der Vereinbarungsdatei dem Vereinbarungstyp entsprechen.


Nachdem Sie [ein Integrationskonto erstellt](./app-service-logic-enterprise-integration-accounts.md) und [Partner hinzugefügt](./app-service-logic-enterprise-integration-partners.md) haben, können Sie eine X12-Vereinbarung erstellen, indem Sie folgende Schritte ausführen:

### Auf der Startseite des Azure-Portals

Nach der Anmeldung beim [Azure-Portal](http://portal.azure.com "Azure-Portal"):
1. Wählen Sie im Menü links **Durchsuchen**.

>[AZURE.TIP]Wenn der Link **Durchsuchen** nicht angezeigt wird, müssen Sie möglicherweise zuerst das Menü erweitern. Wählen Sie dazu den Link **Menü anzeigen** aus, der sich links oben neben dem reduzierten Menü befindet.

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Geben Sie in das Filtersuchfeld *Integration* ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)
3. Wählen Sie auf dem geöffneten Blatt **Integrationskonten** das Integrationskonto aus, in dem Sie die Vereinbarung erstellen möchten. Wenn keine Listen mit Integrationskonten angezeigt werden, [erstellen Sie zunächst ein Konto](./app-service-logic-enterprise-integration-accounts.md "Alles über Integrationskonten"). ![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)
4.  Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel „Vereinbarungen“ nicht angezeigt wird, fügen Sie sie zunächst hinzu. ![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)
5. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)
6. Geben Sie einen **Namen** für die Vereinbarung ein. Wählen Sie dann auf dem geöffneten Blatt „Vereinbarungen“ den **Vereinbarungstyp**, **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** aus. ![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)
7. Nachdem Sie die Eigenschaften der Empfangseinstellungen festgelegt haben, wählen Sie die Schaltfläche **OK**. Wir fahren fort:
8. Wählen Sie **Empfangseinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung empfangen werden, behandelt werden.
9. Das Steuerelement „Empfangseinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Umschläge, Kontrollnummern, Überprüfungen und interne Einstellungen. Konfigurieren Sie diese Eigenschaften basierend auf Ihrer Vereinbarung mit dem Partner, mit dem Sie Nachrichten austauschen. Hier ist eine Ansicht dieser Steuerelemente angegeben. Konfigurieren Sie sie so, wie diese Vereinbarung eingehende Nachrichten identifizieren und verarbeiten soll: ![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)
10. Wählen Sie die Schaltfläche **OK**, um die Einstellungen zu speichern.

### Bezeichner

|Eigenschaft|Beschreibung |
|---|---|
|ISA1 (Autorisierungsqualifizierer)|Wählen Sie den Wert für den Autorisierungsqualifizierer aus der Dropdownliste.|
|ISA2|Optional. Geben Sie den Wert für den Autorisierungsqualifizierer ein. Wenn Sie für ISA1 einen anderen Wert als 00 eingegeben haben, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein.|
|ISA3 (Sicherheitsqualifizierer)|Wählen Sie den Wert für den Sicherheitsqualifizierer aus der Dropdownliste.|
|ISA4|Optional. Geben Sie den Wert für Sicherheitsinformationen ein. Wenn Sie für ISA3 einen anderen Wert als 00 eingegeben haben, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein.|

### Danksagungen 

|Eigenschaft|Beschreibung |
|----|----|
|TA1 erwartet|Wählen Sie dieses Kontrollkästchen, um eine technische Bestätigung (TA1) an den Austauschabsender zurückzugeben. Diese Bestätigungen werden anhand der Sendeeinstellungen für die Vereinbarung an den Austauschabsender gesendet.|
|FA erwartet|Wählen Sie dieses Kontrollkästchen, um eine funktionale Bestätigung (FA) an den Austauschabsender zurückzugeben. Wählen Sie dann, basierend auf den Schemaversionen, mit denen Sie arbeiten, ob Sie die 997 oder 999 Bestätigungen wünschen. Diese Bestätigungen werden anhand der Sendeeinstellungen für die Vereinbarung an den Austauschabsender gesendet.|
|AK2/IK2-Schleife einschließen|Wählen Sie dieses Kontrollkästchen, um die Generierung von AK2-Schleifen in funktionalen Bestätigungen für akzeptierte Transaktionssätze zu aktivieren. Hinweis: Dieses Kontrollkästchen ist nur dann aktiviert, wenn Sie das Kontrollkästchen „FA erwartet“ gewählt haben.|

### Schemas

Wählen Sie ein Schema für jeden Transaktionstyp (ST1) und jede Absenderanwendung (GS2). Die Empfangspipeline disassembliert die eingehende Nachricht durch Abstimmen der Werte für ST1 und GS2 in der eingehenden Nachricht mit den hier festgelegten Werten und des Schemas der eingehenden Nachricht mit dem Schema, das Sie hier festlegen.

|Eigenschaft|Beschreibung |
|----|----|
|Version|Wählen Sie die X12-Version.|
|Transaktionstyp (ST01)|Wählen Sie den Transaktionstyp.|
|Absenderanwendung (GS02)|Wählen Sie die Absenderanwendung.|
|Schema|Wählen Sie die Schemadatei, die Sie verwenden möchten. Schemadateien befinden sich in Ihrem Integrationskonto.|

### Umschläge

|Eigenschaft|Beschreibung |
|----|----|
|ISA11-Verwendung|Verwenden Sie dieses Feld, um das Trennzeichen in einem Transaktionssatz anzugeben:</br></br>Wählen Sie den Standardbezeichner, um die Dezimalschreibweise „.“ anstelle der Dezimalschreibweise des eingehenden Dokuments in der EDI-Empfangspipeline zu verwenden.</br></br>Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. (^) wird z.B. normalerweise als Wiederholungstrennzeichen verwendet. Für HIPAA-Schemas können Sie nur (^) verwenden.|

### Kontrollnummern

|Eigenschaft|Beschreibung |
|----|----|
|Doppelte Austauschkontrollnummern nicht zulassen|Aktivieren Sie diese Option, um doppelte Austauschvorgänge zu blockieren. Wenn ausgewählt, überprüft das BizTalk Services-Portal, ob die Austauschkontrollnummer (ISA13) für den empfangenen Austausch nicht mit der Austauschkontrollnummer übereinstimmt. Wenn eine Übereinstimmung gefunden wird, verarbeitet die Empfangspipeline den Austausch nicht.<br/>Falls Sie doppelte Austauschkontrollnummern nicht zulassen, können Sie die Anzahl der Tage angeben, an denen die Überprüfung durchgeführt wird, indem Sie den entsprechenden Wert für „Innerhalb der folgenden Anzahl von Tagen auf ISA13-Duplikate prüfen:“ angeben.|
|Doppelte Gruppenkontrollnummern nicht zulassen|Aktivieren Sie diese Option, um Austauschvorgänge mit doppelten Gruppenkontrollnummern zu blockieren.|
|Doppelte Transaktionssatz-Kontrollnummern nicht zulassen|Aktivieren Sie diese Option, um Austauschvorgänge mit doppelten Transaktionssatz-Kontrollnummern zu blockieren.|

### Überprüfungen

|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|EDI-Nachrichtentyp, z.B. „850-Bestellung“ bzw. „999-Implementierungsbestätigung“.|
|EDI-Überprüfung|Führt EDI-Überprüfung für Datentypen gemäß den EDI-Eigenschaften Schema, Längeneinschränkungen, leere Datenelemente und nachfolgende Trennzeichen vor.|
|Erweiterte Überprüfung|Wenn der Datentyp nicht EDI ist, werden Datenelementanforderung und zulässige Wiederholung, Aufzählungen und Datenelementlänge (Min./Max.) überprüft.|
|Führende/nachfolgende Nullen zulassen|Zusätzliche führende oder nachfolgende Leer- und Nullzeichen werden beibehalten. Sie werden nicht entfernt.|
|Richtlinie für nachfolgende Trennzeichen|Generiert nachfolgende Trennzeichen für den empfangenen Austausch. Die Optionen umfassen „Nicht zulässig“, „Optional“ und „Erforderlich“.|

### Interne Einstellungen

|Eigenschaft|Beschreibung |
|----|----|
|Implizite Dezimalzahlen im Nn-Format in Zehnerlogarithmuswerte einer Zahl konvertieren|Konvertiert einen im Nn-Format angegebenen EDI-Wert im BizTalk Services-Portal in einen Zehnerlogarithmuswert im XML-Zwischenformat.|
|Leere XML-Tags erstellen, wenn nachfolgende Trennzeichen zulässig sind|Aktivieren Sie dieses Kontrollkästchen, damit der Austauschabsender leere XML-Tags für nachfolgende Trennzeichen einbezieht.|
|Eingehende Batchverarbeitung|Austausch in Transaktionssätze trennen – Transaktionssätze bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag auf den Transaktionssatz angewendet wird. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält BizTalk Services mit dieser Option nur diese Transaktionssätze an. </br></br>Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag angewendet wird. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält BizTalk Services mit dieser Option den gesamten Austausch an.</br></br>Austausch beibehalten – Transaktionssätze bei Fehler anhalten: Lässt den Austausch intakt und erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält BizTalk Services mit dieser Option nur diese Transaktionssätze an, während die Verarbeitung aller anderen Transaktionssätze fortgesetzt wird.</br></br>Austausch beibehalten – Austausch bei Fehler anhalten: Lässt den Austausch intakt und erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält BizTalk Services mit dieser Option den gesamten Austausch an.</br></br>|

Ihre Vereinbarung ist bereit, eingehende Nachrichten zu verarbeiten, die dem Schema entsprechen, das Sie ausgewählt haben.

So konfigurieren Sie die Einstellungen, die Nachrichten verarbeiten, die Sie an Partner senden:
11. Wählen Sie **Sendeeinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung gesendet werden, behandelt werden.

Das Steuerelement „Sendeeinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Umschläge, Kontrollnummern, Zeichensätze und Trennzeichen sowie Überprüfung.

Hier ist eine Ansicht dieser Steuerelemente. Treffen Sie Ihre Auswahl danach, wie Nachrichten verarbeitet werden sollen, die Sie über diese Vereinbarung an Partner senden: ![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)
12. Wählen Sie die Schaltfläche **OK**, um die Einstellungen zu speichern.

### Bezeichner
|Eigenschaft|Beschreibung |
|----|----|
|Autorisierungsqualifizierer (ISA1)|Wählen Sie den Wert für den Autorisierungsqualifizierer aus der Dropdownliste.|
|ISA2|Geben Sie den Wert für den Autorisierungsqualifizierer ein. Wenn dieser Wert nicht 00 ist, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein.|
|Sicherheitsqualifizierer (ISA3)|Wählen Sie den Wert für den Sicherheitsqualifizierer aus der Dropdownliste.|
|ISA4|Geben Sie den Wert für Sicherheitsinformationen ein. Wenn dieser Wert nicht 00 ist, geben Sie für den Wert (ISA4) im Textfeld mindestens ein alphanumerisches Zeichen und maximal 10 ein.|

### Bestätigung
|Eigenschaft|Beschreibung |
|----|----|
|TA1 erwartet|Wählen Sie dieses Kontrollkästchen, um eine technische Bestätigung (TA1) an den Austauschabsender zurückzugeben. Diese Einstellung gibt an, dass der Hostpartner, der die Nachricht sendet, eine Bestätigung vom Gastpartner in der Vereinbarung anfordert. Diese Bestätigungen werden vom Hostpartner basierend auf den Empfangseinstellungen der Vereinbarung erwartet.|
|FA erwartet|Aktivieren Sie dieses Kontrollkästchen, um eine funktionale Bestätigung (FA) an den Absender des Austauschs zu senden, und wählen Sie dann, basierend auf den Schemaversionen, mit denen Sie arbeiten, ob Sie die 997- oder 999-Bestätigungen senden möchten. Diese Bestätigungen werden vom Hostpartner basierend auf den Empfangseinstellungen der Vereinbarung erwartet.|
|FA-Version|Wählen Sie die FA-Version.|

### Schemas
|Eigenschaft|Beschreibung |
|----|----|
|Version|Wählen Sie die X12-Version.|
|Transaktionstyp (ST01)|Wählen Sie den Transaktionstyp.|
|SCHEMA|Wählen Sie das zu verwendende Schema aus. Schemas befinden sich in Ihrem Integrationskonto. Um auf Ihre Schemas zuzugreifen, verknüpfen Sie zuerst Ihr Integrationskonto mit Ihrer Logik-App.|

### Umschläge
|Eigenschaft|Beschreibung |
|----|----|
|ISA11-Verwendung|Verwenden Sie dieses Feld, um das Trennzeichen in einem Transaktionssatz anzugeben:</br></br>Wählen Sie den Standardbezeichner, um die Dezimalschreibweise „.“ anstelle der Dezimalschreibweise des eingehenden Dokuments in der EDI-Empfangspipeline zu verwenden.</br></br>Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. (^) wird z.B. normalerweise als Wiederholungstrennzeichen verwendet. Für HIPAA-Schemas können Sie nur (^) verwenden.</br>|
|Wiederholungstrennzeichen|Geben Sie das Wiederholungstrennzeichen an.|
|Kontrollversionsnummer (ISA12)|Wählen Sie die Version des X12-Standards, die vom BizTalk Services-Portal zum Generieren eines ausgehenden Austauschs verwendet wird.|
|Verwendungsindikator (ISA15)|Geben Sie an, ob der Kontext eines Austauschs Information (I), Produktionsdaten (P) oder Testdaten (T) ist. Die EDI-Empfangspipeline stuft diese Eigenschaft zum Kontext herauf.|
|Schema|Sie können eingeben, wie das BizTalk Services-Portal die GS- und ST-Segmente für einen X12-codierten Austausch generiert, den es an die Sendepipeline sendet.</br></br>Sie können die Werte der Datenelemente GS1, GS2, GS3, GS4, GS5, GS7 und GS8 mit den Werten der Transaktionstyp- und Versions-/Releasedatenelemente verknüpfen. Wenn das BizTalk Services-Portal feststellt, dass eine XML-Nachricht die für den Transaktionstyp festgelegten Werte und Versions-/Releaseelemente in einer Zeile des Rasters enthält, füllt es die Datenelemente GS1, GS2, GS3, GS4, GS5, GS7 und GS8 im Umschlag des ausgehenden Austauschs mit den Werten aus der gleichen Zeile des Rasters auf. Die Werte von Transaktionstyp und Versions-/Releaseelementen müssen eindeutig sein.</br></br>Optional. Wählen Sie für GS1 einen Wert für den Funktionscode aus der Dropdownliste.</br></br>Erforderlich. Geben Sie für GS2 einen alphanumerischen Wert für den Anwendungsabsender mit mindestens zwei Zeichen und maximal 15 Zeichen ein.</br></br>Erforderlich. Geben Sie für GS3 einen alphanumerischen Wert für den Anwendungsempfänger mit mindestens zwei Zeichen und maximal 15 Zeichen ein.</br></br>Optional. Wählen Sie für GS4 CCYYMMDD oder YYMMDD.</br></br>Optional. Wählen Sie für GS5 HHMM, HHMMSS oder HHMMSSdd.</br></br>Optional. Wählen Sie für GS7 einen Wert für die zuständige Agentur aus der Dropdownliste.</br></br>Optional. Geben Sie für GS8 für das identifizierte Dokument einen alphanumerischen Wert mit einer Länge von mindestens 1 und maximal 12 Zeichen an.</br></br>**Hinweis**: Dies sind die Werte, die das BizTalk Services-Portal in die GS-Felder des Austauschs eingibt, den es erstellt, wenn Transaktionstyp und Versions-/Releaseelemente in der gleichen Zeile mit denen übereinstimmen, die mit dem Austausch verknüpft sind.|

### Kontrollnummern
|Eigenschaft|Beschreibung |
|----|----|
|Austauschkontrollnummer (ISA13)|Erforderlich. Geben Sie einen Bereich von Werten für die vom BizTalk Services-Portal zum Generieren eines ausgehenden Austauschs verwendete Austauschkontrollnummer ein. Geben Sie einen numerischen Wert von mindestens 1 und maximal 999.999.999 ein.|
|Gruppenkontrollnummer (GS06)|Erforderlich. Geben Sie einen Bereich von Zahlen ein, die das BizTalk Services-Portal für die Gruppenkontrollnummer verwenden soll. Geben Sie einen numerischen Wert mit einer Länge von mindestens 1 und maximal neun Zeichen ein.|
|Transaktionssatz-Kontrollnummer (ST02)|Geben Sie als Transaktionssatz-Kontrollnummer (ST02) eine Reihe von numerischen Werten für die mittleren Pflichtfelder sowie alphanumerischen Werten für das optionale Präfix und Suffix ein. Die maximale Länge aller vier Felder beträgt neun Zeichen.|
|Präfix|Um den Bereich von Transaktionssatz-Kontrollnummern festzulegen, die in einer Bestätigung verwendet werden, geben Sie Werte in die Felder „Bestätigungskontrollnummer (ST02)“ ein. Geben Sie einen numerischen Wert für die beiden mittleren Felder und einen alphanumerischen Wert (falls gewünscht) für die Felder Präfix und Suffix ein. Die mittleren Felder sind erforderlich und enthalten die minimalen und maximalen Werte für die Kontrollnummer. Präfix und Suffix sind optional. Die maximale Länge aller drei Felder beträgt neun Zeichen.|
|Suffix|Um den Bereich von Transaktionssatz-Kontrollnummern festzulegen, die in einer Bestätigung verwendet werden, geben Sie Werte in die Felder „Bestätigungskontrollnummer (ST02)“ ein. Geben Sie einen numerischen Wert für die beiden mittleren Felder und einen alphanumerischen Wert (falls gewünscht) für die Felder Präfix und Suffix ein. Die mittleren Felder sind erforderlich und enthalten die minimalen und maximalen Werte für die Kontrollnummer. Präfix und Suffix sind optional. Die maximale Länge aller drei Felder beträgt neun Zeichen.|

### Zeichensätze und Trennzeichen
Anders als beim Zeichensatz können Sie für jeden Nachrichtentyp einen anderen Satz zu verwendender Trennzeichen eingeben. Wenn ein Zeichensatz nicht für ein bestimmtes Nachrichtenschema angegeben ist, wird der Standardzeichensatz verwendet.

|Eigenschaft|Beschreibung |
|----|----|
|Zu verwendender Zeichensatz|Wählen Sie den X12-Zeichensatz zum Überprüfen der Eigenschaften, die Sie für die Vereinbarung eingeben.</br></br>**Hinweis**: Das BizTalk Services-Portal verwendet diese Einstellung nur zum Überprüfen der Werte, die für die zugehörigen Vereinbarungseigenschaften eingegeben werden. Die Empfangspipeline oder Sendepipeline ignoriert diese Zeichensatzeigenschaft während der Laufzeitverarbeitung.|
|Schema|Wählen Sie das (+)-Symbol, und wählen Sie ein Schema aus der Dropdownliste. Wählen Sie die zu verwendenden Trennzeichen für das ausgewählte Schema:</br></br>Komponentenelementtrennzeichen: Geben Sie ein einzelnes Zeichen zum Trennen zusammengesetzter Datenelemente an.</br></br>Datenelementtrennzeichen: Geben Sie ein einzelnes Zeichen zum Trennen einfacher Datenelemente in zusammengesetzten Datenelementen an.</br></br></br></br>Ersetzungszeichen: Wählen Sie dieses Kontrollkästchen, wenn die Nutzlastdaten Zeichen enthalten, die auch als Daten-, Segment- oder Komponententrennzeichen verwendet werden. Sie können anschließend ein Ersatzzeichen eingeben. Beim Generieren der ausgehenden X12-Nachricht werden alle Instanzen von Trennzeichen in den Nutzlastdaten durch das angegebene Zeichen ersetzt.</br></br>Segmentabschlusszeichen: Geben Sie ein einzelnes Zeichen ein, um das Ende eines EDI-Segments anzuzeigen.</br></br>Suffix: Wählen Sie das Zeichen, das mit dem Segmentbezeichner verwendet wird. Wenn Sie ein Suffix angeben, kann das Segmentabschlusszeichen-Datenelement leer sein. Wenn das Segmentabschlusszeichen leer gelassen wird, müssen Sie ein Suffix angeben.|

### Überprüfen
|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Durch Auswahl dieser Option wird die Überprüfung auf dem Austauschempfänger aktiviert. Diese Überprüfung führt die EDI-Überprüfung für Transaktionssatz-Datenelemente durch, wobei Datentypen, Längeneinschränkungen und leere Datenelemente sowie nachfolgende Trennzeichen überprüft werden.|
|EDI-Überprüfung||
|Erweiterte Überprüfung|Diese Option ermöglicht die erweiterte Überprüfung von Austauschvorgängen, die vom Austauschabsender empfangen wurden. Dazu gehören zusätzlich zur Überprüfung des XSD-Datentyps die Überprüfung von Feldlänge, Optionalität und Wiederholungsanzahl. Sie können die Erweiterungsüberprüfung aktivieren, ohne die EDI-Überprüfung zu aktivieren, und umgekehrt.|
|Führende/nachfolgende Nullen zulassen|Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund nachfolgender Leerzeichen nicht der Längenanforderung entspricht, jedoch nach Entfernen der Leerzeichen der Längenanforderung entspricht.|
|Nachfolgendes Trennzeichen|Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund führender (oder nachfolgender) Nullen oder Leerzeichen nicht der Längenanforderung entspricht, jedoch nach deren Entfernung der Längenanforderung entspricht.</br></br>Wählen Sie „Nicht zulässig“, wenn Sie keine nachfolgenden Trennzeichen in einem vom Austauschabsender empfangenen Austausch zulassen möchten. Wenn der Austausch nachfolgende Trennzeichen enthält, wird er für ungültig erklärt.</br></br>Wählen Sie „Optional“, um Austauschvorgänge mit oder ohne nachfolgende Trennzeichen zu akzeptieren.</br></br>Wählen Sie „Erforderlich“, wenn der empfangene Austausch nachfolgende Trennzeichen enthalten muss.|

Gehen Sie nach der Auswahl von **OK** auf den geöffneten Blättern wie folgt vor:
13. Wählen Sie auf dem Blatt „Integrationskonto“ die Kachel **Vereinbarungen** aus, auf dem die neu hinzugefügte Vereinbarung aufgeführt ist. ![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)

## Weitere Informationen
- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0803_2016-->