---
title: "Hinzufügen des DB2-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den DB2-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: bba03cfb21e8eebdbf67152de9c6e2da6f22f5a2


---
# <a name="get-started-with-the-db2-connector"></a>Erste Schritte mit dem DB2-Connector
Mit dem Microsoft Connector für DB2 werden Logik-Apps mit Ressourcen verbunden, die in einer IBM DB2-Datenbank gespeichert sind. Dieser Connector enthält einen Microsoft-Client zum Kommunizieren mit DB2-Remoteservercomputern über ein TCP/IP-Netzwerk. Dies umfasst Clouddatenbanken, z.B. IBM Bluemix dashDB oder IBM DB2 für Windows, in Azure sowie lokale Datenbanken unter Verwendung des lokalen Datengateways. Siehe [Liste mit unterstützten IBM DB2-Plattformen und -Versionen](connectors-create-api-db2.md#supported-db2-platforms-and-versions) (in diesem Thema).

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps. 
> 
> 

Der DB2-Connector unterstützt die folgenden Datenbankvorgänge:

* Datenbanktabellen auflisten
* Eine Zeile mit SELECT lesen
* Alle Zeilen mit SELECT lesen
* Eine Zeile mit INSERT hinzufügen
* Eine Zeile mit UPDATE ändern
* Eine Zeile mit DELETE entfernen

In diesem Thema wird veranschaulicht, wie Sie den Connector in einer Logik-App zum Verarbeiten von Datenbankvorgängen verwenden.

Weitere Informationen zu Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Verfügbare Aktionen
Der DB2-Connector unterstützt die folgenden Logik-App-Aktionen:

* GetTables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Auflisten von Tabellen
Das Erstellen einer Logik-App für einen beliebigen Vorgang umfasst viele Schritte, die über das Microsoft Azure-Portal ausgeführt werden.

Innerhalb der Logik-App können Sie eine Aktion zum Auflisten von Tabellen in einer DB2-Datenbank hinzufügen. Die Aktion weist den Connector an, eine DB2-Schemaanweisung zu verarbeiten, z.B. `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name**, z. B. `Db2getTables`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest.  
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen suchen** `db2` ein, und wählen Sie dann **DB2 - Get tables (Preview)** (DB2 – Tabellen abrufen [Vorschau]) aus.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. Aktivieren Sie im Konfigurationsbereich **DB2 - Get tables** (DB2 – Tabellen abrufen) das **Kontrollkästchen**, um **Verbinden über lokales Datengateway** zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
   
   * Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise `ibmserver01:50000`.
   * Geben Sie einen Wert für **Datenbank** ein. Geben Sie beispielsweise `nwind`.
   * Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie z. B. **Standard**.
   * Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise `db2admin`.
   * Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise `Password1`.
   * Wählen Sie einen Wert für **Gateway** aus. Wählen Sie z. B. **datagateway01** aus.
7. Wählen Sie **Erstellen** und dann **Speichern** aus. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. Wählen Sie auf dem Blatt **Db2getTables** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
9. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_tables** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die eine Liste mit Tabellen enthalten sollten.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Erstellen der Verbindungen
Der Connector unterstützt Verbindungen mit lokal und in der Cloud gehosteten Datenbanken durch Verwenden der folgenden Verbindungseigenschaften. 

| Eigenschaft | Beschreibung |
| --- | --- |
| server |Erforderlich. Akzeptiert einen Zeichenfolgenwert im IPv4- oder IPv6-Format, der für eine TCP/IP-Adresse oder einen Alias steht, gefolgt von einer mit Doppelpunkt angefügten TCP/IP-Portnummer. |
| database |Erforderlich. Akzeptiert einen Zeichenfolgenwert , der einen DRDA-Namen einer relationalen Datenbank darstellt. DB2 für z/OS akzeptiert eine 16-Byte-Zeichenfolge („database“ ist ein Speicherort vom Typ „IBM DB2 für z/OS“). DB2 für i5/OS akzeptiert eine 18-Byte-Zeichenfolge („database“ ist eine relationale Datenbank vom Typ „IBM DB2 für i). DB2 für LUW akzeptiert eine 8-Byte-Zeichenfolge. |
| authentication |Optional. Akzeptiert einen Listenelementwert, und zwar entweder „Basic“ oder „Windows“ (Kerberos). |
| username |Erforderlich. Akzeptiert einen Zeichenfolgenwert. DB2 für z/OS akzeptiert eine 8-Byte-Zeichenfolge. DB2 für i akzeptiert eine 10-Byte-Zeichenfolge. DB2 für Linux oder UNIX akzeptiert eine 8-Byte-Zeichenfolge. DB2 für Windows akzeptiert eine 30-Byte-Zeichenfolge. |
| password |Erforderlich. Akzeptiert einen Zeichenfolgenwert. |
| gateway |Erforderlich. Akzeptiert einen Listenelementwert, der für das lokale Datengateway steht, das für Logik-Apps in der Speichergruppe definiert ist. |

## <a name="create-the-on-premises-gateway-connection"></a>Erstellen einer Verbindung über das lokale Gateway
Dieser Connector kann über das lokale Gateway auf eine lokale DB2-Datenbank zugreifen. Weitere Informationen finden Sie in den Themen zum Gateway. 

1. Aktivieren Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen**, um **Connect via gateway** (Über Gateway verbinden) zu aktivieren. Sie sehen, dass die Cloudeinstellungen in die lokalen Einstellungen geändert werden.
2. Geben Sie einen Wert für **Server** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise `ibmserver01:50000`.
3. Geben Sie einen Wert für **Datenbank** ein. Geben Sie beispielsweise `nwind`.
4. Wählen Sie einen Wert für **Authentifizierung** aus. Wählen Sie z. B. **Standard**.
5. Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise `db2admin`.
6. Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise `Password1`.
7. Wählen Sie einen Wert für **Gateway** aus. Wählen Sie z. B. **datagateway01** aus.
8. Klicken Sie auf **Erstellen** , um fortzufahren. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Erstellen der Cloudverbindung
Dieser Connector kann auf eine DB2-Clouddatenbank zugreifen. 

1. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)). 
2. Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie beispielsweise `hisdemo2`.
3. Geben Sie einen Wert für **DB2-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise `hisdemo2.cloudapp.net:50000`.
4. Geben Sie einen Wert für **DB2-Datenbankname** ein. Geben Sie beispielsweise `nwind`.
5. Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise `db2admin`.
6. Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise `Password1`.
7. Klicken Sie auf **Erstellen** , um fortzufahren. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Abrufen aller Zeilen mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen aller Zeilen in einer DB2-Tabelle definieren. Diese Aktion weist den Connector an, eine DB2 SELECT-Anweisung zu verarbeiten, z.B. `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name**, z. B. `Db2getRows`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest. 
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen suchen** `db2` ein, und wählen Sie dann **DB2 - Get rows (Preview)** (DB2 – Zeilen abrufen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus.
7. Wählen Sie im Konfigurationsbereich **Verbindungen** die Option **Neu erstellen** aus. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. Lassen Sie im Konfigurationsbereich **Gateways** das **Kontrollkästchen** deaktiviert (**Connect via gateway** (Verbinden über Gateway)).
   
   * Geben Sie einen Wert für **Verbindungsname** ein. Geben Sie beispielsweise `HISDEMO2`.
   * Geben Sie einen Wert für **DB2-Servername** als Adresse bzw. in der Form „Alias:Portnummer“ ein. Geben Sie beispielsweise `HISDEMO2.cloudapp.net:50000`.
   * Geben Sie einen Wert für **DB2-Datenbankname** ein. Geben Sie beispielsweise `NWIND`.
   * Geben Sie einen Wert für **Benutzername** ein. Geben Sie beispielsweise `db2admin`.
   * Geben Sie einen Wert für **Kennwort** ein. Geben Sie beispielsweise `Password1`.
9. Klicken Sie auf **Erstellen** , um fortzufahren.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **BEREICH** aus.
11. Optional können Sie **Erweiterte Optionen anzeigen** auswählen, um Abfrageoptionen anzugeben.
12. Wählen Sie **Speichern**aus. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. Wählen Sie auf dem Blatt **Db2getRows** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
14. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die eine Liste mit Zeilen enthalten sollten.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Eine Zeile mit INSERT hinzufügen
Sie können eine Logik-App-Aktion zum Hinzufügen einer Zeile zu einer DB2-Tabelle definieren. Diese Aktion weist den Connector an, eine DB2 INSERT-Anweisung zu verarbeiten, z.B. `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name**, z. B. `Db2insertRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest. 
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen** den Text **db2** ein, und klicken Sie dann auf **DB2 - Insert row (Preview)** (DB2 – Zeile einfügen [Vorschau]).
6. Wählen Sie in der Aktion **DB2 - Insert row (Preview)** (DB2 – Zeile einfügen [Vorschau]) die Option **Verbindung ändern** aus. 
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine Verbindung aus. Wählen Sie z. B. **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **BEREICH** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**, `Area 99999` und `102` für **REGIONID** ein. 
10. Wählen Sie **Speichern**aus.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. Wählen Sie auf dem Blatt **Db2insertRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die die neue Zeile enthalten sollten.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Abrufen einer Zeile mit SELECT
Sie können eine Logik-App-Aktion zum Abrufen einer Zeile aus einer DB2-Tabelle definieren. Diese Aktion weist den Connector an, eine DB2 SELECT WHERE-Anweisung zu verarbeiten, z.B. `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name** (z. B. **Db2getRow**, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**. 
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest. 
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen suchen** **db2** ein, und wählen Sie dann **db2 - Get rows (Preview)** (DB2 – Zeilen abrufen [Vorschau]) aus.
6. Wählen Sie in der Aktion **Get rows (Preview)** (Zeilen abrufen [Vorschau]) **Verbindung ändern** aus. 
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z. B. **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **BEREICH** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**ein. 
10. Optional können Sie **Erweiterte Optionen anzeigen** auswählen, um Abfrageoptionen anzugeben.
11. Wählen Sie **Speichern**aus. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. Wählen Sie auf dem Blatt **Db2getRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
13. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die die Zeile enthalten sollten.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Ändern einer Zeile mit UPDATE
Sie können eine Logik-App-Aktion zum Ändern einer Zeile in einer DB2-Tabelle definieren. Diese Aktion weist den Connector an, eine DB2 UPDATE-Anweisung zu verarbeiten, z.B. `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name**, z. B. `Db2updateRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**.
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest. 
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Geben Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen** den Text **db2** ein, und klicken Sie dann auf **DB2 - Update row (Preview)** (DB2 – Zeile aktualisieren [Vorschau]).
6. Wählen Sie in der Aktion **DB2 - Update row (Preview)** (DB2 – Zeile aktualisieren [Vorschau]) die Option **Verbindung ändern** aus. 
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z. B. **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **BEREICH** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**, `Updated 99999` und `102` für **REGIONID** ein. 
10. Wählen Sie **Speichern**aus. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. Wählen Sie auf dem Blatt **Db2updateRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die die neue Zeile enthalten sollten.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Eine Zeile mit DELETE entfernen
Sie können eine Logik-App-Aktion zum Entfernen einer Zeile aus einer DB2-Tabelle definieren. Diese Aktion weist den Connector an, eine DB2 DELETE-Anweisung zu verarbeiten, z.B. `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
1. Wählen Sie im **Azure-Startmenü** die Optionen **+** (Pluszeichen), **Web und mobil** und dann **Logik-App**.
2. Geben Sie **Name**, z. B. `Db2deleteRow`, **Abonnement**, **Ressourcengruppe**, **Speicherort** und **App Service-Plan** ein. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus.

### <a name="add-a-trigger-and-action"></a>Hinzufügen eines Triggers und einer Aktion
1. Klicken Sie im **Logik-App-Designer** in der Liste **Vorlagen** auf **Leere Logik-App**. 
2. Klicken Sie in der Liste **Trigger** auf **Wiederholung**. 
3. Klicken Sie für den Trigger **Wiederholung** auf **Bearbeiten**. Wählen Sie die Dropdownliste **Häufigkeit** aus, um die Option **Tag** auszuwählen, und legen Sie **Intervall** auf den Typ **7** fest. 
4. Wählen Sie das Feld **+ Neuer Schritt** und dann **Aktion hinzufügen** aus.
5. Wählen Sie in der Liste **Aktionen** im Bearbeitungsfeld **Nach weiteren Aktionen** den Text **db2** aus, und klicken Sie dann auf **DB2 - Delete row (Preview)** (DB2 – Zeile löschen [Vorschau]).
6. Wählen Sie in der Aktion **DB2 - Delete row (Preview)** (DB2 – Zeile löschen [Vorschau]) die Option **Verbindung ändern** aus. 
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine vorhandene Verbindung aus. Wählen Sie z. B. **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten** und dann **BEREICH** aus.
9. Geben Sie Werte für alle erforderlichen Spalten ein (siehe rotes Sternchen). Geben Sie beispielsweise `99999` für **AREAID**ein. 
10. Wählen Sie **Speichern**aus. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. Wählen Sie auf dem Blatt **Db2deleteRow** in der Liste **Alle Testläufe** unter **Zusammenfassung** den zuerst aufgeführten Eintrag (letzte Ausführung) aus.
12. Wählen Sie auf dem Blatt **Logik-App-Ausführung** die Option **Testlaufdetails** aus. Wählen Sie in der Liste **Aktion** den Eintrag **Get_rows** aus. Der Wert für **Status** sollte **Erfolgreich** lauten. Wählen Sie den Link **Eingaben** aus, um die Eingaben anzuzeigen. Wählen Sie den Link **Ausgaben**aus, um die Ausgaben anzuzeigen, die die gelöschte Zeile enthalten sollten.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Technische Details
## <a name="actions"></a>Actions
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Der DB2-Datenbankconnector enthält die folgenden Aktionen: 

| Aktion | Beschreibung |
| --- | --- |
| [GetRow](connectors-create-api-db2.md#get-row) |Ruft eine einzelne Zeile aus einer DB2-Tabelle ab. |
| [GetRows](connectors-create-api-db2.md#get-rows) |Ruft Zeilen aus einer DB2-Tabelle ab. |
| [InsertRow](connectors-create-api-db2.md#insert-row) |Fügt eine neue Zeile in eine DB2-Tabelle ein. |
| [DeleteRow](connectors-create-api-db2.md#delete-row) |Löscht eine Zeile aus einer DB2-Tabelle. |
| [GetTables](connectors-create-api-db2.md#get-tables) |Ruft Tabellen aus einer DB2-Datenbank ab. |
| [UpdateRow](connectors-create-api-db2.md#update-row) |Aktualisiert eine vorhandene Zeile in einer DB2-Tabelle. |

### <a name="action-details"></a>Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="get-row"></a>Zeile abrufen
Ruft eine einzelne Zeile aus einer DB2-Tabelle ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der DB2-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der abzurufenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### <a name="get-rows"></a>Zeilen abrufen
Ruft Zeilen aus einer DB2-Tabelle ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der DB2-Tabelle |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="insert-row"></a>Zeile einfügen
Fügt eine neue Zeile in eine DB2-Tabelle ein.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der DB2-Tabelle |
| item* |Zeile |In die angegebene DB2-Tabelle einzufügende Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### <a name="delete-row"></a>Zeile löschen
Löscht eine Zeile aus einer DB2-Tabelle.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der DB2-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu löschenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-tables"></a>Tabellen abrufen
Ruft Tabellen aus einer DB2-Datenbank ab.  

Es gibt keine Parameter für diesen Aufruf. 

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="update-row"></a>Zeile aktualisieren
Aktualisiert eine vorhandene Zeile in einer DB2-Tabelle.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der DB2-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu aktualisierenden Zeile |
| item* |Zeile |Zeile mit aktualisierten Werten |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="http-responses"></a>HTTP-Antworten
Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:  

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="supported-db2-platforms-and-versions"></a>Unterstützte DB2-Plattformen und -Versionen
Dieser Connector unterstützt die folgenden IBM DB2-Plattformen und -Versionen sowie IBM DB2-kompatible Produkte (z.B. IBM Bluemix dashDB), die Distributed Relational Database Architecture (DRDA) SQL Access Manager (SQLAM) Version 10 und 11 unterstützen:

* IBM DB2 für z/OS 11.1
* IBM DB2 für z/OS 10.1
* IBM DB2 für i 7.3
* IBM DB2 für i 7.2
* IBM DB2 für i 7.1
* IBM DB2 für LUW 11
* IBM DB2 für LUW 10.5

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.




<!--HONumber=Feb17_HO3-->


