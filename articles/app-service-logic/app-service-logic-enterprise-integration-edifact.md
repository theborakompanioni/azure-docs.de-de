<properties 
	pageTitle="Unternehmensintegration mit EDIFACT | Microsoft Azure" 
	description="Es wird beschrieben, wie Sie EDIFACT-Vereinbarungen zum Erstellen von Logik-Apps verwenden." 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="jeffhollan" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="app-service-logic" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/26/2016" 
	ms.author="jonfan"/>

# Unternehmensintegration mit EDIFACT 

> [AZURE.NOTE] Auf dieser Seite werden die EDIFACT-Features von Logik-Apps beschrieben. Informationen zu X12 erhalten Sie [hier](app-service-logic-enterprise-integration-x12.md).

## Erstellen einer EDIFACT-Vereinbarung 
Bevor Sie EDIFACT-Nachrichten austauschen können, müssen Sie eine EDIFACT-Vereinbarung erstellen und in Ihrem Integrationskonto speichern. In den folgenden Schritten werden Sie durch das Erstellen einer EDIFACT-Vereinbarung geführt.

### Bevor Sie beginnen, benötigen Sie Folgendes:
- Ein in Ihrem Azure-Abonnement definiertes [Integrationskonto](./app-service-logic-enterprise-integration-accounts.md)
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md), die bereits in Ihrem Integrationskonto definiert sind

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss der Inhalt in den Nachrichten, die Sie an den Partner senden und vom Partner empfangen, mit dem Vereinbarungstyp übereinstimmen.


Nachdem Sie [ein Integrationskonto erstellt](./app-service-logic-enterprise-integration-accounts.md) und [Partner hinzugefügt](./app-service-logic-enterprise-integration-partners.md) haben, können Sie eine EDIFACT-Vereinbarung erstellen, indem Sie folgende Schritte ausführen:

### Auf der Startseite des Azure-Portals

Nach der Anmeldung beim [Azure-Portal](http://portal.azure.com "Azure-Portal"):
1. Wählen Sie im Menü links **Durchsuchen**.

>[AZURE.TIP]Wenn der Link **Durchsuchen** nicht angezeigt wird, müssen Sie möglicherweise zuerst das Menü erweitern. Wählen Sie dazu den Link **Menü anzeigen** aus, der sich links oben neben dem reduzierten Menü befindet.

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)
2. Geben Sie in das Filtersuchfeld *Integration* ein, und wählen Sie in der Ergebnisliste **Integrationskonten** aus. ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)
3. Wählen Sie auf dem geöffneten Blatt **Integrationskonten** das Integrationskonto aus, in dem Sie die Vereinbarung erstellen möchten. Wenn keine Listen mit Integrationskonten angezeigt werden, [erstellen Sie zunächst ein Konto](./app-service-logic-enterprise-integration-accounts.md "Alles über Integrationskonten"). ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)
4.  Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel „Vereinbarungen“ nicht angezeigt wird, fügen Sie sie zunächst hinzu. ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)
5. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Schaltfläche **Hinzufügen** aus. ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)
6. Geben Sie einen **Namen** für die Vereinbarung ein. Wählen Sie dann auf dem geöffneten Blatt „Vereinbarungen“ den **Vereinbarungstyp** für EDIFACT, **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** aus. ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)
7. Wählen Sie nach dem Festlegen der Vereinbarungseigenschaften die Option **Empfangseinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung empfangen werden, behandelt werden.
8. Das Steuerelement „Empfangseinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Kontrollnummern, Überprüfungen, internen Einstellungen und Batchverarbeitung. Konfigurieren Sie diese Eigenschaften basierend auf Ihrer Vereinbarung mit dem Partner, mit dem Sie Nachrichten austauschen. Hier ist eine Ansicht dieser Steuerelemente dargestellt. Konfigurieren Sie sie so, wie diese Vereinbarung eingehende Nachrichten identifizieren und verarbeiten soll: ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)
9. Wählen Sie die Schaltfläche **OK**, um die Einstellungen zu speichern.

### Bezeichner

|Eigenschaft|Beschreibung |
|---|---|
|UNB6.1 (Empfängerverweiskennwort)|Geben Sie einen alphanumerischen Wert mit 1 bis 14 Zeichen ein.|
|UNB6.2 (Empfängerverweisqualifizierer)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal zwei Zeichen ein.|

### Danksagungen 

|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtenquittung (CONTRL)|Wählen Sie dieses Kontrollkästchen, um eine technische Bestätigung (CONTRL) an den Austauschabsender zurückzugeben. Die Bestätigung wird anhand der Sendeeinstellungen für die Vereinbarung an den Austauschabsender gesendet.|
|Bestätigung (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, um eine funktionale Bestätigung (CONTRL) an den Austauschabsender zurückzugeben. Die Bestätigung wird anhand der Sendeeinstellungen für die Vereinbarung an den Austauschabsender gesendet.|

### Schemas

|Eigenschaft|Beschreibung |
|----|----|
|UNH2.1 (TYP)|Wählen Sie einen Transaktionssatztyp aus.|
|UNH2.2 (VERSION)|Geben Sie die Nachrichtenversionsnummer ein. (Mindestens ein Zeichen, maximal drei Zeichen.)|
|UNH2.3 (FREIGABE)|Geben Sie die Nachrichtenfreigabenummer ein. (Mindestens ein Zeichen, maximal drei Zeichen.)|
|UNH2.5 (ZUGEWIESENER CODE FÜR ZUORDNUNG)|Geben Sie den zugewiesenen Code ein. (Maximal sechs Zeichen. Muss alphanumerisch sein.)|
|UNG2.1 (ID FÜR APP-SENDER)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 35 Zeichen ein.|
|UNG2.2 (CODEQUALIFIZIERER FÜR APP-SENDER)|Geben Sie einen alphanumerischen Wert mit maximal vier Zeichen ein.|
|SCHEMA|Wählen Sie das zuvor hochgeladene Schema, das Sie verwenden möchten, im zugeordneten Integrationskonto aus.|

### Kontrollnummern

|Eigenschaft|Beschreibung |
|----|----|
|Doppelte Austauschkontrollnummern nicht zulassen|Aktivieren Sie dieses Kontrollkästchen, um doppelte Austauschvorgänge zu blockieren. Bei Aktivierung wird mit der EDIFACT-Decodierungsaktion sichergestellt, dass die Austauschkontrollnummer (UNB5) für den empfangenen Austausch nicht mit einer zuvor verarbeiteten Austauschkontrollnummer übereinstimmt. Wenn eine Übereinstimmung erkannt wird, wird der Austausch nicht verarbeitet.
|Innerhalb der folgenden Anzahl von Tagen auf UNB5-Duplikate prüfen|Wenn Sie sich gegen das Zulassen von doppelten Austauschkontrollnummern entschieden haben, können Sie die Anzahl von Tagen angeben, nach denen die Überprüfung durchgeführt werden soll. Geben Sie hierzu den gewünschten Wert für die Option **Innerhalb der folgenden Anzahl von Tagen auf UNB5-Duplikate prüfen** ein.|
|Doppelte Gruppenkontrollnummern nicht zulassen|Aktivieren Sie dieses Kontrollkästchen, um Austauschvorgänge mit doppelten Gruppenkontrollnummern (UNG5) zu blockieren.|
|Doppelte Transaktionssatz-Kontrollnummern nicht zulassen|Aktivieren Sie dieses Kontrollkästchen, um Austauschvorgänge mit doppelten Transaktionssatz-Kontrollnummern (UNH1) zu blockieren.|
|EDIFACT-Bestätigungskontrollnummer|Um die Transaktionssatz-Verweisnummern zur Verwendung in einer Bestätigung anzugeben, geben Sie einen Wert für das Präfix, einen Bereich von Verweisnummern und ein Suffix ein.|

### Überprüfungen

|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Geben Sie den Nachrichtentyp an. Wenn eine Überprüfungszeile abgeschlossen ist, wird jeweils eine andere automatisch hinzugefügt. Falls keine Regeln angegeben sind, wird die als Standardeinstellung markierte Zeile für die Validierung verwendet.|
|EDI-Überprüfung|Aktivieren Sie dieses Kontrollkästchen, um eine EDI-Überprüfung für Datentypen gemäß den EDI-Eigenschaften Schema, Längeneinschränkungen, leere Datenelemente und nachgestellte Trennzeichen durchzuführen.|
|Erweiterte Überprüfung|Aktivieren Sie dieses Kontrollkästchen, um die erweiterte Überprüfung (XSD) von Austauschvorgängen zu ermöglichen, die vom Austauschabsender empfangen wurden. Dazu gehören zusätzlich zur Überprüfung des XSD-Datentyps die Überprüfung von Feldlänge, Optionalität und Wiederholungsanzahl.|
|Führende/nachgestellte Nullen zulassen|Wählen Sie **Zulässig**, um führende/nachgestellte Nullen zuzulassen, und **Nicht zulässig**, um führende/nachgestellte Nullen nicht zuzulassen. Oder wählen Sie **Beschneiden**, um führende/nachgestellte Nullen abzuschneiden.|
|Führende/nachgestellte Nullen abschneiden|Aktivieren Sie dieses Kontrollkästchen, um alle führenden oder nachgestellten Nullen abzuschneiden.|
|Richtlinie für nachgestellte Trennzeichen|Wählen Sie **Nicht zulässig**, wenn Sie keine nachgestellten Trennzeichen in einem Austauschvorgang zulassen möchten, die vom Austauschabsender empfangen wurden. Wenn der Austauschvorgang nachgestellte Trennzeichen enthält, wird er als ungültig deklariert. Wählen Sie **Optional**, um Austauschvorgänge mit oder ohne nachgestellte Trennzeichen zu akzeptieren. Wählen Sie **Erforderlich**, wenn der empfangene Austauschvorgang zwingend nachgestellte Trennzeichen enthalten muss.|

### Interne Einstellungen

|Eigenschaft|Beschreibung |
|----|----|
|Leere XML-Tags erstellen, wenn nachfolgende Trennzeichen zulässig sind|Aktivieren Sie dieses Kontrollkästchen, damit der Austauschabsender leere XML-Tags für nachfolgende Trennzeichen einbezieht.|
|Eingehende Batchverarbeitung|Beispiele für Optionen:</br></br>**Austausch in Transaktionssätze trennen – Transaktionssätze bei Fehler anhalten**: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag auf den Transaktionssatz angewendet wird. Wenn ein oder mehrere Transaktionssätze im Austausch die Überprüfung nicht bestehen, werden nur diese Transaktionssätze angehalten. Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag angewendet wird. Wenn bei dieser Option ein oder mehrere Transaktionssätze im Austausch die Überprüfung nicht bestehen, wird der gesamte Austausch angehalten.</br></br>**Austausch beibehalten – Transaktionssätze bei Fehler anhalten**: Lässt den Austausch intakt und erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn ein oder mehrere Transaktionssätze im Austausch die Überprüfung nicht bestehen, werden bei dieser Option nur diese Transaktionssätze angehalten, und alle anderen Transaktionssätze werden verarbeitet.</br></br>**Austausch beibehalten – Austausch bei Fehler anhalten**: Lässt den Austausch intakt und erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, wird bei dieser Option der gesamte Austausch angehalten.|

Ihre Vereinbarung ist bereit, eingehende Nachrichten zu verarbeiten, die den von Ihnen ausgewählten Einstellungen entsprechen.

So konfigurieren Sie die Einstellungen, die Nachrichten verarbeiten, die Sie an Partner senden:
10. Wählen Sie **Sendeeinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung gesendet werden, behandelt werden.

Das Steuerelement „Sendeeinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Umschläge, Zeichensätze und Trennzeichen, Kontrollnummern sowie Überprüfung.

Hier ist eine Ansicht dieser Steuerelemente. Treffen Sie Ihre Auswahl basierend darauf, wie Sie Nachrichten verarbeiten möchten, die Sie über diese Vereinbarung an Partner senden: ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)
11. Wählen Sie die Schaltfläche **OK**, um die Einstellungen zu speichern.

### Bezeichner
|Eigenschaft|Beschreibung |
|----|----|
|UNB1.2 (Syntaxversion)|Wählen Sie einen Wert zwischen **1** und **4** aus.|
|UNB2.3 (Adresse für Rückmeldung des Absenders)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 14 Zeichen ein.|
|UNB3.3 (Adresse für Rückmeldung des Empfängers)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 14 Zeichen ein.|
|UNB6.1 (Empfängerverweiskennwort)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 14 Zeichen ein.|
|UNB6.2 (Empfängerverweisqualifizierer)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal zwei Zeichen ein.|
|UNB7 (Anwendungsverweis-ID)|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 14 Zeichen ein.|

### Bestätigung
|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtenquittung (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, wenn der gehostete Partner den Empfang einer technischen Bestätigung (CONTRL) erwartet. Diese Einstellung gibt an, dass der gehostete Partner, der die Nachricht sendet, eine Bestätigung vom Gastpartner anfordert.|
|Bestätigung (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, wenn der gehostete Partner den Empfang einer funktionalen Bestätigung (CONTRL) erwartet. Diese Einstellung gibt an, dass der gehostete Partner, der die Nachricht sendet, eine Bestätigung vom Gastpartner anfordert.|
|SG1/SG4-Schleife für akzeptierte Transaktionssätze generieren|Wenn Sie sich für die Anforderung einer funktionalen Bestätigung entschieden haben, können Sie dieses Kontrollkästchen aktivieren, um die Generierung von SG1/SG4-Schleifen in funktionalen CONTRL-Bestätigungen für akzeptierte Transaktionssätze zu erzwingen.|

### Schemas
|Eigenschaft|Beschreibung |
|----|----|
|UNH2.1 (TYP)|Wählen Sie einen Transaktionssatztyp aus.|
|UNH2.2 (VERSION)|Geben Sie die Nachrichtenversionsnummer ein.|
|UNH2.3 (FREIGABE)|Geben Sie die Nachrichtenfreigabenummer ein.|
|SCHEMA|Wählen Sie das zu verwendende Schema aus. Schemas befinden sich in Ihrem Integrationskonto. Um auf Ihre Schemas zuzugreifen, verknüpfen Sie zuerst Ihr Integrationskonto mit Ihrer Logik-App.|

### Umschläge
|Eigenschaft|Beschreibung |
|----|----|
|UNB8 (Verarbeitungsprioritätscode)|Geben Sie einen alphabetischen Wert ein, der nicht länger als ein Zeichen ist.|
|UNB10 (Kommunikationsvereinbarung )|Geben Sie einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 40 Zeichen ein.|
|UNB11 (Testindikator)|Aktivieren Sie dieses Kontrollkästchen, um anzugeben, dass es sich beim generierten Austausch um Testdaten handelt.|
|UNA-Segment übernehmen (Zeichenfolgedienstanweisung)|Aktivieren Sie dieses Kontrollkästchen, um ein UNA-Segment für den zu sendenden Austausch zu generieren.|
|UNG-Segmente übernehmen (Funktionsgruppenheader)|Aktivieren Sie dieses Kontrollkästchen, um Gruppierungssegmente im Funktionsgruppenheader in den Nachrichten zu erstellen, die an den Gastpartner gesendet werden. Die folgenden Werte werden verwendet, um die UNG-Segmente zu erstellen:</br></br>Geben Sie für **UNG1** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal sechs Zeichen ein.</br></br>Geben Sie für **UNG2.1** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 35 Zeichen ein.</br></br>Geben Sie für **UNG2.2** einen alphanumerischen Wert mit einer Länge von maximal vier Zeichen ein.</br></br>Geben Sie für **UNG3.1** einen alphanumerischen Wert mit eine Länge von mindestens einem und maximal 35 Zeichen ein.</br></br>Geben Sie für **UNG3.2** einen alphanumerischen Wert mit einer Länge von maximal vier Zeichen ein.</br></br>Geben Sie für **UNG6** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal drei Zeichen ein.</br></br>Geben Sie für **UNG7.1** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal drei Zeichen ein.</br></br>Geben Sie für **UNG7.2** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal drei Zeichen ein.</br></br>Geben Sie für **UNG7.3** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal sechs Zeichen ein.</br></br>Geben Sie für **UNG8** einen alphanumerischen Wert mit einer Länge von mindestens einem und maximal 14 Zeichen ein.|

### Zeichensätze und Trennzeichen
Anders als beim Zeichensatz können Sie für jeden Nachrichtentyp einen anderen Satz zu verwendender Trennzeichen eingeben. Wenn ein Zeichensatz nicht für ein bestimmtes Nachrichtenschema angegeben ist, wird der Standardzeichensatz verwendet.

|Eigenschaft|Beschreibung |
|----|----|
|UNB1.1 (Systembezeichner)|Wählen Sie den EDIFACT-Zeichensatz aus, der auf den ausgehenden Austauschvorgang angewendet werden soll.|
|Schema|Wählen Sie in der Dropdownliste ein Schema aus. Wenn eine Zeile abgeschlossen ist, wird eine neue Zeile hinzugefügt. Wählen Sie die zu verwendenden Trennzeichen für das ausgewählte Schema:</br></br>**Komponentenelementtrennzeichen** – Geben Sie ein einzelnes Zeichen zum Trennen zusammengesetzter Datenelemente an.</br></br>**Datenelementtrennzeichen** – Geben Sie ein einzelnes Zeichen zum Trennen einfacher Datenelemente in zusammengesetzten Datenelementen an.</br></br></br></br>**Ersetzungszeichen** – Wählen Sie dieses Kontrollkästchen, wenn die Nutzlastdaten Zeichen enthalten, die auch als Daten-, Segment- oder Komponententrennzeichen verwendet werden. Sie können anschließend ein Ersatzzeichen eingeben. Beim Generieren der ausgehenden EDIFACT-Nachricht werden alle Instanzen von Trennzeichen in den Nutzlastdaten durch das angegebene Zeichen ersetzt.</br></br>**Segmentabschlusszeichen** – Geben Sie ein einzelnes Zeichen ein, um das Ende eines EDI-Segments anzuzeigen.</br></br>**Suffix** – Wählen Sie das Zeichen, das mit dem Segmentbezeichner verwendet wird. Wenn Sie ein Suffix angeben, kann das Segmentabschlusszeichen-Datenelement leer sein. Wenn das Segmentabschlusszeichen leer gelassen wird, müssen Sie ein Suffix angeben.|

### Kontrollnummern
|Eigenschaft|Beschreibung |
|----|----|
|UNB5 (Austauschkontrollnummer)|Geben Sie ein Präfix, einen Bereich mit Werten für die Austauschkontrollnummer und ein Suffix ein. Diese Werte werden verwendet, um einen ausgehenden Austausch zu generieren. Das Präfix und das Suffix sind optional. Die Kontrollnummer ist erforderlich. Die Kontrollnummer wird für jede neue Nachricht um den Wert 1 erhöht. Das Präfix und das Suffix bleibt jeweils gleich.|
|UNG5 (Gruppenkontrollnummer)|Geben Sie ein Präfix, einen Bereich mit Werten für die Austauschkontrollnummer und ein Suffix ein. Diese Werte werden verwendet, um die Gruppenkontrollnummer zu generieren. Das Präfix und das Suffix sind optional. Die Kontrollnummer ist erforderlich. Die Kontrollnummer wird für jede neue Nachricht um den Wert 1 erhöht, bis der Maximalwert erreicht ist. Das Präfix und das Suffix bleiben gleich.|
|UNH1 (Verweisnummer des Nachrichtenheaders)|Geben Sie ein Präfix, einen Bereich mit Werten für die Austauschkontrollnummer und ein Suffix ein. Diese Werte werden verwendet, um die Verweisnummer des Nachrichtenheaders zu generieren. Das Präfix und das Suffix sind optional. Die Verweisnummer ist erforderlich. Die Verweisnummer wird für jede neue Nachricht um den Wert 1 erhöht. Das Präfix und das Suffix bleibt jeweils gleich.|

### Überprüfungen
|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Durch Auswahl dieser Option wird die Überprüfung auf dem Austauschempfänger aktiviert. Diese Überprüfung führt die EDI-Überprüfung für Transaktionssatz-Datenelemente durch, wobei Datentypen, Längeneinschränkungen und leere Datenelemente sowie nachfolgende Trennzeichen überprüft werden.|
|EDI-Überprüfung|Aktivieren Sie dieses Kontrollkästchen, um eine EDI-Überprüfung für Datentypen gemäß den EDI-Eigenschaften Schema, Längeneinschränkungen, leere Datenelemente und nachgestellte Trennzeichen durchzuführen.|
|Erweiterte Überprüfung|Diese Option ermöglicht die erweiterte Überprüfung von Austauschvorgängen, die vom Austauschabsender empfangen wurden. Dazu gehören zusätzlich zur Überprüfung des XSD-Datentyps die Überprüfung von Feldlänge, Optionalität und Wiederholungsanzahl. Sie können die Erweiterungsüberprüfung aktivieren, ohne die EDI-Überprüfung zu aktivieren, und umgekehrt.|
|Führende/nachgestellte Nullen zulassen|Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund nachfolgender Leerzeichen nicht der Längenanforderung entspricht, jedoch nach Entfernen der Leerzeichen der Längenanforderung entspricht.|
|Führende/nachgestellte Nullen abschneiden|Wenn Sie diese Option aktivieren, werden die führenden und nachgestellten Nullen abgeschnitten.|
|Nachfolgendes Trennzeichen|Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund führender (oder nachgestellter) Nullen oder Leerzeichen nicht der Längenanforderung entspricht (jedoch nach deren Entfernung).</br></br>Wählen Sie **Nicht zulässig**, wenn Sie keine nachgestellten Trennzeichen in einem vom Austauschabsender empfangenen Austausch zulassen möchten. Wenn der Austausch nachgestellte Trennzeichen enthält, wird er für ungültig erklärt.</br></br>Wählen Sie **Optional**, um Austauschvorgänge mit oder ohne nachgestellte Trennzeichen zu akzeptieren.</br></br>Wählen Sie **Erforderlich**, wenn der empfangene Austausch nachgestellte Trennzeichen enthalten muss.|

Gehen Sie auf dem geöffneten Blatt nach der Auswahl von **OK** wie folgt vor:
12. Wählen Sie auf dem Blatt „Integrationskonto“ die Kachel **Vereinbarungen** aus, auf dem die neu hinzugefügte Vereinbarung aufgeführt ist. ![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)

## Weitere Informationen
- [Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0803_2016-->