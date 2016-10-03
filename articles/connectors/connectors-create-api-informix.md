<properties
    pageTitle="Hinzufügen des Informix-Connectors zu Ihren Logik-Apps | Microsoft Azure"
    description="Übersicht über den Informix-Connector mit REST-API-Parametern"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/19/2016"
   ms.author="plarsen"/>


# Erste Schritte mit dem Informix-Connector
Mit dem Microsoft Connector für Informix werden Logik-Apps mit Ressourcen verbunden, die in einer IBM Informix-Datenbank gespeichert sind. Dieser Connector enthält einen Microsoft-Client zum Kommunizieren mit IBM Informix-Remoteservercomputern in einem TCP/IP-Netzwerk, wie Clouddatenbanken (z.B. IBM Informix für Windows unter Azure-Virtualisierung) und lokale Datenbanken mit lokalem Datengateway. Eine Liste mit unterstützten IBM Informix-Plattformen und -Versionen finden Sie am Ende dieses Themas.

Der Connector unterstützt die folgenden Datenbankvorgänge:

- Datenbanktabellen auflisten
- Eine Zeile mit SELECT lesen
- Alle Zeilen mit SELECT lesen
- Eine Zeile mit INSERT hinzufügen
- Eine Zeile mit UPDATE ändern
- Eine Zeile mit DELETE entfernen

In diesem Thema wird veranschaulicht, wie Sie den Connector in einer Logik-App zum Verarbeiten von Datenbankvorgängen verwenden.

>[AZURE.NOTE] Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.

Weitere Informationen zu Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Logik-App-Aktionen
Der Connector unterstützt die folgenden Logik-App-Aktionen:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## Definieren der Logik-App zum Auflisten von Tabellen
Das Definieren einer Logik-App für einen beliebigen Vorgang umfasst viele Schritte, die über das Microsoft Azure-Portal ausgeführt werden. Sie können eine Logik-App-Aktion definieren, um Tabellen in einer Informix-Datenbank aufzulisten, bei der der Connector angewiesen wird, eine Informix-Schemaanweisung (CALL SYSIBM.SQLTABLES) zu verarbeiten.

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixgetTables**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Get tables (Preview)** (Informix – Tabellen abrufen (Vorschau)).

	![](./media/connectors-create-api-informix/InformixconnectorActions.png)

6.	Aktivieren Sie im Konfigurationsbereich **Informix - Get tables** (Informix – Tabellen abrufen) das **Kontrollkästchen**, um **Connect via on-premises data gateway** (Über lokales Datengateway verbinden) zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
	- Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise **ibmserver01:9089** ein.
	- Geben Sie einen Wert für **Datenbank** ein. Geben Sie beispielsweise **nwind** ein.
	- Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie beispielsweise **Standard** aus.
	- Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise **informix** ein.
	- Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise **Password1** ein.
	- Wählen Sie einen Wert für **Gateway** aus. Klicken Sie beispielsweise auf **datagateway01**.
7. Klicken Sie auf **Erstellen** und dann auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

8.	Klicken Sie auf dem Blatt **InformixgetTables** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
9.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_tables**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte eine Liste mit den Tabellen enthalten sein.

	![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## Definieren von Informix-Verbindungen
Der Connector unterstützt Verbindungen mit lokalen Datenbanken und Clouddatenbanken, indem die folgenden Verbindungseigenschaften verwendet werden.

Eigenschaft | Beschreibung
--- | ---
server | Die erforderliche server-Eigenschaft akzeptiert einen Zeichenfolgenwert im IPv4- oder IPv6-Format, der für eine TCP/IP-Adresse oder einen Alias steht, gefolgt von einer per Doppelpunkt angefügten TCP/IP-Portnummer. 
database | Die erforderliche database-Eigenschaft akzeptiert einen Zeichenfolgenwert, der für einen „DRDA Relational Database Name“ (RDBNAM) steht. Informix akzeptiert eine 128-Byte-Zeichenfolge („database“ ist ein „IBM Informix database name“ (dbname)).
authentication | Die optionale authentication-Eigenschaft akzeptiert einen Listenelementwert, und zwar entweder „Basic“ oder „Windows“ (Kerberos). 
username | Die erforderliche username-Eigenschaft akzeptiert einen Zeichenfolgenwert.
password | Die erforderliche password-Eigenschaft akzeptiert einen Zeichenfolgenwert.
gateway | Die erforderliche gateway-Eigenschaft akzeptiert einen Listenelementwert, der für das lokale Datengateway steht, das für Logik-Apps in der Speichergruppe definiert ist.  

## Definieren einer Verbindung über das lokale Gateway
Der Connector kann auf eine lokale Informix-Datenbank über das lokale Datengateway zugreifen. Weitere Informationen finden Sie in den Themen zum Gateway.

1. Aktivieren Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen**, um **Connect via gateway** (Über Gateway verbinden) zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
2. Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise **ibmserver01:9089** ein.
3. Geben Sie einen Wert für **Datenbank** ein. Geben Sie beispielsweise **nwind** ein.
4. Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie z.B. **Standard**.
5. Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise **informix** ein.
6. Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise **Password1** ein.
7. Wählen Sie einen Wert für **Gateway** aus. Klicken Sie beispielsweise auf **datagateway01**.
8. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## Definieren einer Cloudverbindung
Der Connector kann auf eine Informix-Clouddatenbank zugreifen.

1. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)).
2. Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie beispielsweise **hisdemo2** ein.
3. Geben Sie einen Wert für **Informix-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise **hisdemo2.cloudapp.net:9089** ein.
3. Geben Sie einen Wert für **Informix-Datenbankname** ein. Geben Sie beispielsweise **nwind** ein.
4. Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise **informix** ein.
5. Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise **Password1** ein.
6. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## Definieren der Logik-App zum Abrufen aller Zeilen mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen aller Zeilen einer Informix-Tabelle definieren, bei der der Connector angewiesen wird, eine SELECT-Informix-Anweisung zu verarbeiten (z.B. **SELECT * FROM AREA**).

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixgetRows**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Get rows (Preview)** (Informix – Zeilen abrufen (Vorschau)).
6. Klicken Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen (Vorschau)) auf **Verbindung ändern**.
7. Klicken Sie im Konfigurationsbereich **Verbindungen** auf **Neu erstellen**.

	![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
  
8. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)).
	- Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie beispielsweise **HISDEMO2** ein.
	- Geben Sie einen Wert für **Informix-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise **HISDEMO2.cloudapp.net:9089** ein.
	- Geben Sie einen Wert für **Informix-Datenbankname** ein. Geben Sie beispielsweise **nwind** ein.
	- Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise **informix** ein.
	- Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise **Password1** ein.
9. Klicken Sie auf **Erstellen**, um fortzufahren.

	![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

10. Klicken Sie in der Liste **Tabellenname** auf den **Pfeil nach unten** und dann auf **AREA**.
11. Optional können Sie auf **Erweiterte Optionen anzeigen** klicken, um Abfrageoptionen anzugeben.
12. Klicken Sie auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)

13.	Klicken Sie auf dem Blatt **InformixgetRows** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
14.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_rows**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte eine Liste mit den Zeilen enthalten sein.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## Definieren einer Logik-App zum Hinzufügen einer Zeile mit INSERT
Sie können eine Logik-App-Aktion zum Hinzufügen einer Zeile in einer Informix-Tabelle definieren, bei der der Connector angewiesen wird, eine INSERT-Informix-Anweisung zu verarbeiten (z.B. **INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)**).

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixinsertRow**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Insert row (Preview)** (Informix – Zeile einfügen (Vorschau)).
6. Klicken Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen (Vorschau)) auf **Verbindung ändern**.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine Verbindung aus. Klicken Sie beispielsweise auf **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Klicken Sie in der Liste **Tabellenname** auf den **Pfeil nach unten** und dann auf **AREA**.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise „**99999**“ für **AREAID**, „**Area 99999**“ und „**102**“ für **REGIONID** ein.
10. Klicken Sie auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
 
11.	Klicken Sie auf dem Blatt **InformixinsertRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
12.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_rows**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte die neue Zeile enthalten sein.

	![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## Definieren einer Logik-App zum Abrufen einer Zeile mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen einer Zeile einer Informix-Tabelle definieren, bei der der Connector angewiesen wird, eine SELECT WHERE-Informix-Anweisung zu verarbeiten (z.B. **SELECT FROM AREA WHERE AREAID = '99999'**).

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixgetRow**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Get rows (Preview)** (Informix – Zeilen abrufen (Vorschau)).
6. Klicken Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen (Vorschau)) auf **Verbindung ändern**.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Klicken Sie beispielsweise auf **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Klicken Sie in der Liste **Tabellenname** auf den **Pfeil nach unten** und dann auf **AREA**.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise „**99999**“ für **AREAID** ein.
10. Optional können Sie auf **Erweiterte Optionen anzeigen** klicken, um Abfrageoptionen anzugeben.
11. Klicken Sie auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)

12.	Klicken Sie auf dem Blatt **InformixgetRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
13.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_rows**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte die Zeile enthalten sein.

	![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## Definieren einer Logik-App zum Ändern einer Zeile mit UPDATE
Sie können eine Logik-App-Aktion zum Ändern einer Zeile in einer Informix-Tabelle definieren, bei der der Connector angewiesen wird, eine UPDATE-Informix-Anweisung zu verarbeiten (z.B. **UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)**).

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixupdateRow**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Update row (Preview)** (Informix – Zeile aktualisieren (Vorschau)).
6. Klicken Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen (Vorschau)) auf **Verbindung ändern**.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Klicken Sie beispielsweise auf **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Klicken Sie in der Liste **Tabellenname** auf den **Pfeil nach unten** und dann auf **AREA**.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise „**99999**“ für **AREAID**, „**Updated 99999**“ und „**102**“ für **REGIONID** ein.
10. Klicken Sie auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)

11.	Klicken Sie auf dem Blatt **InformixupdateRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
12.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_rows**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte die neue Zeile enthalten sein.

	![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## Definieren einer Logik-App zum Entfernen einer Zeile mit DELETE
Sie können eine Logik-App-Aktion zum Entfernen einer Zeile einer Informix-Tabelle definieren, bei der der Connector angewiesen wird, eine DELETE-Informix-Anweisung zu verarbeiten (z.B. **DELETE FROM AREA WHERE AREAID = '99999'**).

### Definieren einer Logik-App-Instanz
1.	Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2.	Geben Sie **Name** (z.B. „**InformixdeleteRow**“), **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Klicken Sie auf **An Dashboard anheften**, und wählen Sie **Erstellen**.

### Definieren von Logik-App-Trigger und -Aktion
1.	Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Blank LogicApp** (Leere LogicApp).
2.	Klicken Sie in der Liste **Trigger** auf **Wiederholung**.
3.	Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**, klicken Sie auf die Dropdownliste **Häufigkeit**, um die Option **Tag** auszuwählen, und klicken Sie dann auf **Intervall**, um **7** einzugeben.
4.	Klicken Sie auf das Feld **+ Neuer Schritt** und dann auf **Aktion hinzufügen**.
5.	Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Search for more actions** (Nach weiteren Aktionen suchen) den Text **informix** ein, und klicken Sie dann auf **Informix - Delete row (Preview)** (Informix – Zeile löschen (Vorschau)).
6. Klicken Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen (Vorschau)) auf **Verbindung ändern**.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Klicken Sie beispielsweise auf **hisdemo2**.

	![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)

8. Klicken Sie in der Liste **Tabellenname** auf den **Pfeil nach unten** und dann auf **AREA**.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise „**99999**“ für **AREAID** ein.
10. Klicken Sie auf **Speichern**.

	![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)

11.	Klicken Sie auf dem Blatt **InformixdeleteRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** auf den zuerst aufgeführten Eintrag (letzte Ausführung).
12.	Klicken Sie auf dem Blatt **Logic app run** (Logik-App-Ausführung) auf **Testlaufdetails**. Klicken Sie in der Liste **Aktion** auf **Get\_rows**. Der Wert für **Status** sollte **Erfolgreich** lauten. Klicken Sie auf den Link **Inputs** (Eingaben). Sehen Sie sich die Eingaben an. Klicken Sie auf den Link **Outputs** (Ausgaben). Sehen Sie sich die Ausgaben an. Darin sollte die gelöschte Zeile enthalten sein.

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
Der Connector unterstützt die hier angegebenen IBM Informix-Versionen, wenn diese für die Unterstützung von Clientverbindungen vom Typ „Distributed Relational Database Architecture“ (DRDA) konfiguriert sind.
- IBM Informix 12.1
- IBM Informix 11.7

 
## Nächste Schritte

[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md) über die anderen verfügbaren Connectors für Logik-Apps.

<!---HONumber=AcomDC_0921_2016-->