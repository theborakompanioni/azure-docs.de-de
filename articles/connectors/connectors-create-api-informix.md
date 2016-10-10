<properties
    pageTitle="Hinzufügen des Informix-Connectors zu Ihren Logik-Apps | Microsoft Azure"
    description="Übersicht über den Informix-Connector mit REST-API-Parametern"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# Erste Schritte mit dem Informix-Connector
Mit dem Microsoft Connector für Informix werden Logik-Apps mit Ressourcen verbunden, die in einer IBM Informix-Datenbank gespeichert sind. Der Informix-Connector enthält einen Microsoft-Client zum Kommunizieren mit Informix-Remoteservercomputern über ein TCP/IP-Netzwerk. Dies umfasst Clouddatenbanken, z.B. IBM Informix für Windows, in Azure sowie lokale Datenbanken unter Verwendung des lokalen Datengateways. Siehe die [Liste mit unterstützten IBM Informix-Plattformen und -Versionen](connectors-create-api-informix.md#supported-informix-platforms-and-versions) (in diesem Thema).

>[AZURE.NOTE] Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.

Der Connector unterstützt die folgenden Datenbankvorgänge:

- Datenbanktabellen auflisten
- Eine Zeile mit SELECT lesen
- Alle Zeilen mit SELECT lesen
- Eine Zeile mit INSERT hinzufügen
- Eine Zeile mit UPDATE ändern
- Eine Zeile mit DELETE entfernen

In diesem Thema wird veranschaulicht, wie Sie den Connector in einer Logik-App zum Verarbeiten von Datenbankvorgängen verwenden.

Weitere Informationen zu Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verfügbare Aktionen
Der Connector unterstützt die folgenden Logik-App-Aktionen:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## Auflisten von Tabellen
Das Erstellen einer Logik-App für einen beliebigen Vorgang umfasst viele Schritte, die über das Microsoft Azure-Portal ausgeführt werden.

Innerhalb der Logik-App können Sie eine Aktion zum Auflisten von Tabellen in einer Informix-Datenbank hinzufügen. Die Aktion weist den Connector an, eine Informix-Schemaanweisung zu verarbeiten, z.B. `CALL SYSIBM.SQLTABLES`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie den **Namen**, z.B. `InformixgetTables`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Get tables (Preview)** (Informix – Tabellen abrufen [Vorschau]) aus.

	![](./media/connectors-create-api-informix/InformixconnectorActions.png)

6.	Aktivieren Sie im Konfigurationsbereich **Informix - Get tables** (Informix – Tabellen abrufen) das **Kontrollkästchen**, um **Connect via on-premises data gateway** (Über lokales Datengateway verbinden) zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
	- Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie z.B. `ibmserver01:9089` ein.
	- Geben Sie einen Wert für **Datenbank** ein. Geben Sie z.B. `nwind` ein.
	- Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie z.B. **Standard**.
	- Geben Sie einen Wert für **Benutzername** ein. Geben Sie z.B. `informix` ein.
	- Geben Sie einen Wert für **Kennwort** ein. Geben Sie z.B. `Password1` ein.
	- Wählen Sie einen Wert für **Gateway** aus. Wählen Sie z.B. **datagateway01** aus.
7. Wählen Sie **Erstellen** und dann **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

8.	Wählen Sie auf dem Blatt **InformixgetTables** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
9.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_tables** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die eine Liste mit Tabellen enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## Erstellen der Verbindungen
Der Connector unterstützt Verbindungen mit lokal und in der Cloud gehosteten Datenbanken durch Verwenden der folgenden Verbindungseigenschaften.

Eigenschaft | Beschreibung
--- | ---
server | Erforderlich. Akzeptiert einen Zeichenfolgenwert im IPv4- oder IPv6-Format, der für eine TCP/IP-Adresse oder einen Alias steht, gefolgt von einer mit Doppelpunkt angefügten TCP/IP-Portnummer. 
database | Erforderlich. Akzeptiert einen Zeichenfolgenwert , der einen DRDA-Namen einer relationalen Datenbank darstellt. Informix akzeptiert eine 128-Byte-Zeichenfolge („database“ ist ein „IBM Informix database name“ (dbname)).
authentication | Optional. Akzeptiert einen Listenelementwert, und zwar entweder „Basic“ oder „Windows“ (Kerberos). 
username | Erforderlich. Akzeptiert einen Zeichenfolgenwert.
password | Erforderlich. Akzeptiert einen Zeichenfolgenwert.
gateway | Erforderlich. Akzeptiert einen Listenelementwert, der für das lokale Datengateway steht, das für Logik-Apps in der Speichergruppe definiert ist.  

## Erstellen einer Verbindung über das lokale Gateway
Dieser Connector kann auf eine lokale Informix-Datenbank über das lokale Datengateway zugreifen. Weitere Informationen finden Sie in den Themen zum Gateway.

1. Aktivieren Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen**, um **Connect via gateway** (Über Gateway verbinden) zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
2. Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie z.B. `ibmserver01:9089` ein.
3. Geben Sie einen Wert für **Datenbank** ein. Geben Sie z.B. `nwind` ein.
4. Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie z.B. **Standard**.
5. Geben Sie einen Wert für **Benutzername** ein. Geben Sie z.B. `informix` ein.
6. Geben Sie einen Wert für **Kennwort** ein. Geben Sie z.B. `Password1` ein.
7. Wählen Sie einen Wert für **Gateway** aus. Wählen Sie z.B. **datagateway01** aus.
8. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## Erstellen der Cloudverbindung
Der Connector kann auf eine Informix-Clouddatenbank zugreifen.

1. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)).
2. Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie z.B. `hisdemo2` ein.
3. Geben Sie einen Wert für **Informix-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie z.B. `hisdemo2.cloudapp.net:9089` ein.
3. Geben Sie einen Wert für **Informix-Datenbankname** ein. Geben Sie z.B. `nwind` ein.
4. Geben Sie einen Wert für **Benutzername** ein. Geben Sie z.B. `informix` ein.
5. Geben Sie einen Wert für **Kennwort** ein. Geben Sie z.B. `Password1` ein.
6. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## Abrufen aller Zeilen mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen aller Zeilen in einer Informix-Tabelle erstellen. Diese Aktion weist den Connector an, eine Informix SELECT-Anweisung zu verarbeiten, z.B. `SELECT * FROM AREA`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixgetRows**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Get rows (Preview)** (Informix – Zeilen abrufen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** die Option **Neu erstellen** aus.

	![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
  
8. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)).
	- Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie z.B. `HISDEMO2` ein.
	- Geben Sie einen Wert für **Informix-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie z.B. `HISDEMO2.cloudapp.net:9089` ein.
	- Geben Sie einen Wert für **Informix-Datenbankname** ein. Geben Sie z.B. `NWIND` ein.
	- Geben Sie einen Wert für **Benutzername** ein. Geben Sie z.B. `informix` ein.
	- Geben Sie einen Wert für **Kennwort** ein. Geben Sie z.B. `Password1` ein.
9. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

10. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **AREA** aus.
11. Optional können Sie **Erweiterte Optionen anzeigen** auswählen, um Abfrageoptionen anzugeben.
12. Wählen Sie **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)

13.	Wählen Sie auf dem Blatt **InformixgetRows** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
14.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die eine Liste mit Zeilen enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## Eine Zeile mit INSERT hinzufügen
Sie können eine Logik-App-Aktion zum Hinzufügen einer Zeile zu einer Informix-Tabelle erstellen. Die Aktion weist den Connector an, eine Informix INSERT-Anweisung zu verarbeiten, z.B. `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie den **Namen**, z.B. `InformixinsertRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Insert row (Preview)** (Informix – Zeile einfügen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine Verbindung aus. Wählen Sie z.B. **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **AREA** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**, `Area 99999` und `102` für **REGIONID** ein.
10. Wählen Sie **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
 
11.	Wählen Sie auf dem Blatt **InformixinsertRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die die neue Zeile enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## Abrufen einer Zeile mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen einer Zeile aus einer Informix-Tabelle erstellen. Diese Aktion weist den Connector an, eine Informix SELECT WHERE-Anweisung zu verarbeiten, z.B. `SELECT FROM AREA WHERE AREAID = '99999'`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie den **Namen**, z.B. `InformixgetRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Get rows (Preview)** (Informix – Zeilen abrufen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z.B. **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **AREA** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID** ein.
10. Optional können Sie **Erweiterte Optionen anzeigen** auswählen, um Abfrageoptionen anzugeben.
11. Wählen Sie **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)

12.	Wählen Sie auf dem Blatt **InformixgetRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
13.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die die Zeile enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## Ändern einer Zeile mit UPDATE
Sie können eine Logik-App-Aktion zum Ändern einer Zeile in einer Informix-Tabelle erstellen. Die Aktion weist den Connector an, eine Informix UPDATE-Anweisung zu verarbeiten, z.B. `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie den **Namen**, z.B. `InformixupdateRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Update row (Preview)** (Informix – Zeile aktualisieren [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z.B. **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **AREA** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**, `Updated 99999` und `102` für **REGIONID** ein.
10. Wählen Sie **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)

11.	Wählen Sie auf dem Blatt **InformixupdateRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die die neue Zeile enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## Eine Zeile mit DELETE entfernen
Sie können eine Logik-App-Aktion zum Entfernen einer Zeile aus einer Informix-Tabelle erstellen. Die Aktion weist den Connector an, eine Informix DELETE-Anweisung zu verarbeiten, z.B. `DELETE FROM AREA WHERE AREAID = '99999'`.

### Erstellen einer Logik-App
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie den **Namen**, z.B. `InformixdeleteRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie auf **An Dashboard anheften** und dann **Erstellen** aus.

### Hinzufügen eines Triggers und einer Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.
4.	Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und wählen Sie dann **Informix - Delete row (Preview)** (Informix – Zeile löschen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z.B. **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **AREA** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID** ein.
10. Wählen Sie **Speichern** aus.

	![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)

11.	Wählen Sie auf dem Blatt **InformixdeleteRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12.	Wählen Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get\_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben** aus, um die Ausgaben anzuzeigen, die die gelöschte Zeile enthalten sollten.

	![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## Technische Details

## Actions
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Der Informix-Datenbankconnector enthält die folgenden Aktionen:

|Aktion|Beschreibung|
|--- | ---|
|[GetRow](connectors-create-api-informix.md#get-row)|Ruft eine einzelne Zeile aus einer Informix-Tabelle ab.|
|[GetRows](connectors-create-api-informix.md#get-rows)|Ruft Zeilen aus einer Informix-Tabelle ab.|
|[InsertRow](connectors-create-api-informix.md#insert-row)|Fügt eine neue Zeile in eine Informix-Tabelle ein.|
|[DeleteRow](connectors-create-api-informix.md#delete-row)|Löscht eine Zeile aus einer Informix-Tabelle.|
|[GetTables](connectors-create-api-informix.md#get-tables)|Ruft Tabellen aus einer Informix-Datenbank ab.|
|[UpdateRow](connectors-create-api-informix.md#update-row)|Aktualisiert eine vorhandene Zeile in einer Informix-Tabelle.|

### Aktionsdetails

Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### Zeile abrufen 
Ruft eine einzelne Zeile aus einer Informix-Tabelle ab.

| Eigenschaftenname| Display Name |Beschreibung|
| ---|---|---|
|table* | Tabellenname |Name der Informix-Tabelle|
|id* | Zeilen-ID |Eindeutiger Bezeichner der abzurufenden Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|


#### Zeilen abrufen 
Ruft Zeilen aus einer Informix-Tabelle ab.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|table*|Tabellenname|Name der Informix-Tabelle|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|value|array|


#### Zeile einfügen 
Fügt eine neue Zeile in eine Informix-Tabelle ein.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|table*|Tabellenname|Name der Informix-Tabelle|
|item*|Zeile|In die angegebene Informix-Tabelle einzufügende Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|


#### Zeile löschen 
Löscht eine Zeile aus einer Informix-Tabelle.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|table*|Tabellenname|Name der Informix-Tabelle|
|id*|Zeilen-ID|Eindeutiger Bezeichner der zu löschenden Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine

#### Tabellen abrufen 
Ruft Tabellen aus einer Informix-Datenbank ab.

Es gibt keine Parameter für diesen Aufruf.

##### Ausgabedetails 
TablesList

| Eigenschaftenname | Datentyp |
|---|---|
|value|array|

#### Zeile aktualisieren 
Aktualisiert eine vorhandene Zeile in einer Informix-Tabelle.

|Eigenschaftenname| Display Name|Beschreibung|
| ---|---|---|
|table*|Tabellenname|Name der Informix-Tabelle|
|id*|Zeilen-ID|Eindeutiger Bezeichner der zu aktualisierenden Zeile|
|item*|Zeile|Zeile mit aktualisierten Werten|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails  
Item

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|


### HTTP-Antworten

Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler ist aufgetreten|
|die Standardeinstellung|Fehler beim Vorgang.|


## Unterstützte Informix-Plattformen und -Versionen
Dieser Connector unterstützt die folgenden IBM Informix-Versionen, wenn diese für die Unterstützung von Clientverbindungen vom Typ „Distributed Relational Database Architecture“ (DRDA) konfiguriert sind.

- IBM Informix 12.1
- IBM Informix 11.7

 
## Nächste Schritte

[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md) über die anderen verfügbaren Connectors für Logik-Apps.

<!---HONumber=AcomDC_0928_2016-->