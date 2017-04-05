---
title: Herstellen einer Azure DocumentDB-Verbindung mit BI-Analysetools | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie den Azure DocumentDB-ODBC-Treiber zum Erstellen von Tabellen und Ansichten verwenden, damit normalisierte Daten in BI- und Datenanalysesoftware angezeigt werden können."
keywords: ODBC, ODBC-Treiber
services: documentdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 03/27/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 5f712c7fa9b6ee06f7c89de40ba4227a925a35ce
ms.lasthandoff: 03/28/2017


---

# <a name="connect-to-documentdb-using-bi-analytics-tools-with-the-odbc-driver"></a>Herstellen einer DocumentDB-Verbindung mithilfe von BI-Analysetools per ODBC-Treiber

Mit dem DocumentDB-ODBC-Treiber können Sie eine Verbindung mit DocumentDB über die BI-Analysetools herstellen, z.B. SQL Server Integration Services, Power BI Desktop und Tableau, um Visualisierungen Ihrer DocumentDB-Daten in diesen Lösungen zu analysieren und zu erstellen.

Der DocumentDB-ODBC-Treiber ist ODBC 3.8-konform und unterstützt die ANSI SQL-92-Syntax. Der Treiber verfügt über umfassende Features zum Renormalisieren von Daten in DocumentDB. Mit dem Treiber können Sie Daten in DocumentDB als Tabellen und Ansichten darstellen. Sie können damit SQL-Vorgänge für die Tabellen und Ansichten durchführen, z.B. das Gruppieren nach Abfragen, Einfügungen, Updates und Löschungen.

## <a name="why-do-i-need-to-normalize-my-data"></a>Warum muss ich meine Daten normalisieren?
DocumentDB ist eine NoSQL-Datenbank und ermöglicht daher die schnelle Entwicklung von Apps. Anwendungen können ihr Datenmodell „on the fly“ durchlaufen und sind nicht auf ein striktes Schema beschränkt. Eine einzelne DocumentDB-Datenbank kann JSON-Dokumente mit unterschiedlichen Strukturen enthalten. Dies ist eine gute Voraussetzung für die schnelle Anwendungsentwicklung. Aber wenn Sie Daten analysieren und mit Datenanalyse- und BI-Tools Berichte dazu erstellen möchten, müssen die Daten häufig vereinfacht und an ein bestimmtes Schema angepasst werden.

An dieser Stelle kommt der ODBC-Treiber ins Spiel. Mit dem ODBC-Treiber können Sie Daten in DocumentDB jetzt in Tabellen und Ansichten renormalisieren, die Ihre Anforderungen an die Datenanalyse und Berichterstellung erfüllen. Die renormalisierten Schemas haben keinerlei Auswirkung auf die zugrunde liegenden Daten, und die Entwickler sind nicht auf ihre Nutzung beschränkt. Sie ermöglichen ihnen lediglich das Nutzen von ODBC-konformen Tools zum Zugreifen auf die Daten. Ihre DocumentDB-Datenbank ist dann nicht mehr nur das Lieblingstool Ihres Entwicklungsteams, sondern auch die Datenanalysten werden sie zu schätzen wissen.

Nun wenden wir uns dem ODBC-Treiber zu.

## <a id="install"></a>Schritt 1: Installieren des DocumentDB-ODBC-Treibers

1. Laden Sie die Treiber für Ihre Umgebung herunter:

    * [Microsoft Azure DocumentDB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) für 64-Bit-Windows
    * [Microsoft Azure DocumentDB ODBC 32x64-bit.msi](https://aka.ms/documentdb-odbc-32x64) für 32- und 64-Bit-Windows
    * [Microsoft Azure DocumentDB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) für 32-Bit-Windows

    Führen Sie die MSI-Datei, mit der der **Installations-Assistent für den Microsoft Azure DocumentDB-ODBC-Treiber** gestartet wird, lokal aus. 
2. Verwenden Sie im Installations-Assistenten die Standardeinstellungen, um den ODBC-Treiber zu installieren.
3. Öffnen Sie die App **ODBC-Datenquellenadministrator** auf Ihrem Computer, indem Sie beispielsweise im Windows-Suchfeld **ODBC-Datenquellen** eingeben. 
    Sie können sich vergewissern, dass der Treiber installiert wurde, indem Sie auf die Registerkarte **Treiber** klicken und sicherstellen, dass der **Microsoft DocumentDB ODBC-Treiber** aufgeführt ist.

    ![ODBC-Datenquellenadministrator für DocumentDB](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver.png)

## <a id="connect"></a>Schritt 2: Herstellen der Verbindung mit Ihrer DocumentDB-Datenbank

1. Klicken Sie nach dem [Installieren des DocumentDB-ODBC-Treibers](#install) im Fenster **ODBC-Datenquellenadministrator** auf **Hinzufügen**. Sie können einen Benutzer- oder System-DSN erstellen. In diesem Beispiel erstellen wir einen Benutzer-DSN.
2. Wählen Sie im Fenster **Neue Datenquelle erstellen** die Option **Microsoft DocumentDB-ODBC-Treiber**, und klicken Sie dann auf **Fertig stellen**.
3. Geben Sie im Fenster **DocumentDB ODBC Driver DSN Setup** (DocumentDB-ODBC-Treiber – DSN-Setup) Folgendes ein: 

    ![Fenster „DocumentDB ODBC Driver DSN Setup“ (DocumentDB-ODBC-Treiber – DSN-Setup)](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-dsn-setup.png)
    - **Datenquellenname**: Dies ist der Anzeigename für den ODBC-DSN. Dieser Name muss für Ihr DocumentDB-Konto eindeutig sein. Vergeben Sie also einen passenden Namen, falls Sie mehrere Konten verwenden.
    - **Beschreibung**: Eine kurze Beschreibung der Datenquelle.
    - **Host**: Der URI für Ihr DocumentDB-Konto. Sie können den URI im Azure-Portal über das Blatt „DocumentDB Keys“ (DocumentDB-Schlüssel) abrufen. Dies ist im folgenden Screenshot dargestellt. 
    - **Zugriffsschlüssel**: Der Primär- oder Sekundärschlüssel für den Lese-/Schreibzugriff oder schreibgeschützten Zugriff über das Blatt mit den DocumentDB-Schlüsseln im Azure-Portal. Dies ist im folgenden Screenshot dargestellt. Wir empfehlen Ihnen die Verwendung des Schlüssels für den schreibgeschützten Zugriff, falls der DSN nur für die schreibgeschützte Datenverarbeitung und Berichterstellung verwendet wird.
    ![Blatt „DocumentDB Keys“ (DocumentDB-Schlüssel)](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-keys.png)
    - **Encrypt Access Key for** (Zugriffsschlüssel verschlüsseln für): Treffen Sie basierend auf den Benutzern dieses Computers die beste Wahl. 
4. Klicken Sie auf die Schaltfläche **Testen**, um sicherzustellen, dass Sie eine Verbindung mit Ihrem DocumentDB-Konto herstellen können. 
5. Klicken Sie auf **Erweiterte Optionen**, und legen Sie die folgenden Werte fest:
    - **Query Consistency** (Abfragekonsistenz): Wählen Sie die [Konsistenzebene](documentdb-consistency-levels.md) für Ihre Vorgänge aus. Die Standardeinstellung lautet „Sitzung“.
    - **Anzahl von Wiederholungen**: Geben Sie ein, wie oft für einen Vorgang ein Wiederholungsversuch durchgeführt werden soll, falls die erste Anforderung aufgrund einer Drosselung des Diensts nicht abgeschlossen werden konnte.
    - **Schemadatei**: Hier haben Sie verschiedene Optionen.
        - Wenn Sie diesen Eintrag unverändert (leer) lassen, scannt der Treiber die erste Seite der Daten für alle Sammlungen, um jeweils das Schema für die Sammlung zu ermitteln. Dies wird als Sammlungszuordnung bezeichnet. Ohne definierte Schemadatei muss der Treiber den Scanvorgang bei jeder Treibersitzung durchführen. Dies kann zu einer längeren Startdauer für Anwendungen führen, die den DSN nutzen. Es ist ratsam, für einen DSN immer eine Schemadatei zuzuordnen.
        - Falls Sie bereits über eine Schemadatei verfügen (ggf. mit dem [Schema-Editor](#schema-editor) erstellt), können Sie auf **Durchsuchen** klicken, zur Datei navigieren und dann auf **Speichern** und auf **OK** klicken.
        - Wenn Sie ein neues Schema erstellen möchten, klicken Sie auf **OK** und anschließend im Hauptfenster auf **Schema-Editor**. Fahren Sie dann mit den Informationen im [Schema-Editor](#schema-editor) fort. Achten Sie beim Erstellen der neuen Schemadatei darauf, zurück zum Fenster **Erweiterte Optionen** zu wechseln, um die neu erstellte Schemadatei einzufügen.

6. Nachdem Sie die Schritte im Fenster **DocumentDB ODBC Driver DSN Setup** (DocumentDB-ODBC-Treiber – DSN-Setup) ausgeführt und das Fenster geschlossen haben, wird der neue Benutzer-DSN auf der Registerkarte „Benutzer-DSN“ hinzugefügt.

    ![Neuer DocumentDB-ODBC-DSN auf der Registerkarte „Benutzer-DSN“](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Schritt 3: Erstellen einer Schemadefinition mit der Methode für die Sammlungszuordnung

Es gibt zwei Arten von Samplingmethoden, die Sie verwenden können: **Sammlungszuordnung** und **Tabellentrennzeichen**. In einer Samplingsitzung können beide Samplingmethoden verwendet werden, aber für eine Sammlung ist jeweils nur eine bestimmte Samplingmethode zulässig. Mit den Schritten unten wird ein Schema für die Daten in einer oder mehreren Sammlungen mithilfe der Methode „Sammlungszuordnung“ erstellt. Mit dieser Samplingmethode werden die Daten auf der Seite einer Sammlung abgerufen, um die Struktur der Daten zu ermitteln. Hierbei wird eine Sammlung in eine Tabelle auf ODBC-Seite transponiert. Diese Samplingmethode ist effizient und schnell, wenn die Daten einer Sammlung homogen sind. Falls eine Sammlung heterogene Daten enthält, empfehlen wir Ihnen die Verwendung der [Tabellentrennzeichen-Zuordnungsmethode](#table-mapping) (table-delimiters). Dies ist eine robustere Samplingmethode zum Ermitteln der Datenstrukturen einer Sammlung. 

1. Klicken Sie nach Abschluss der Schritte 1 bis 4 unter [Herstellen der Verbindung mit Ihrer DocumentDB-Datenbank](#connect) im Fenster **DocumentDB ODBC Driver DSN Setup** (DocumentDB-ODBC-Treiber – DSN-Setup) auf **Schema-Editor**.

    ![Schaltfläche „Schema-Editor“ im Fenster „DocumentDB ODBC Driver DSN Setup“ (DocumentDB-ODBC-Treiber – DSN-Setup)](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-schema-editor.png)
2. Klicken Sie im Fenster **Schema-Editor** auf **Neu erstellen**.
    Im Fenster **Generate Schema** (Schema generieren) werden alle Sammlungen des DocumentDB-Kontos angezeigt. 
3. Wählen Sie mindestens eine Sammlung für das Sampling aus, und klicken Sie dann auf **Sample** (Beispiel). 
4. Auf der Registerkarte **Entwurfsansicht** sind die Datenbank, das Schema und die Tabelle dargestellt. In der Tabellenansicht zeigt der Scanvorgang die Eigenschaften an, die den Spaltennamen zugeordnet sind (SQL-Name, Quellname usw.).
    Sie können für jede Spalte SQL-Spaltenname, SQL-Typ, SQL-Länge (falls zutreffend), Staffelung (falls zutreffend), Genauigkeit (falls zutreffend) und „Nullable“ angeben bzw. ändern.
    - Sie können **Hide Column** (Spalte ausblenden) auf **true** (wahr) festlegen, wenn Sie diese Spalte aus den Abfrageergebnissen ausschließen möchten. Spalten mit der Einstellung „Hide Column = true“ werden nicht für die Auswahl und Projektion zurückgegeben, sind aber weiterhin Teil des Schemas. Beispielsweise können Sie alle für das DocumentDB-System erforderlichen Eigenschaften ausblenden, die mit „_“ beginnen.
    - Die Spalte **id** ist das einzige Feld, das nicht ausgeblendet werden kann, da es als Primärschlüssel im normalisierten Schema verwendet wird. 
5. Klicken Sie nach dem Definieren des Schemas auf **Datei** | **Speichern**, navigieren Sie zum Verzeichnis, in dem das Schema gespeichert werden soll, und klicken Sie dann auf **Speichern**.

    Falls Sie dieses Schema zu einem späteren Zeitpunkt mit einem DSN verwenden möchten, können Sie das Fenster „DocumentDB ODBC Driver DSN Setup“ (DocumentDB-ODBC-Treiber – DSN-Setup) (über den ODBC-Datenquellenadministrator) öffnen, auf „Erweiterte Optionen“ klicken und dann im Feld „Schemadatei“ zum gespeicherten Schema navigieren. Mit dem Speichern einer Schemadatei unter einem vorhandenen DSN wird die DSN-Verbindung an die vom Schema definierten Daten und die dazugehörige Struktur angepasst.

## <a id="table-mapping"></a>Schritt 4: Erstellen einer Schemadefinition mit der Tabellentrennzeichen-Zuordnungsmethode

Es gibt zwei Arten von Samplingmethoden, die Sie verwenden können: **Sammlungszuordnung** und **Tabellentrennzeichen**. In einer Samplingsitzung können beide Samplingmethoden verwendet werden, aber für eine Sammlung ist jeweils nur eine bestimmte Samplingmethode zulässig. 

Mit den folgenden Schritten wird ein Schema für die Daten in einer oder mehreren Sammlungen erstellt, indem die Zuordnungsmethode **Tabellentrennzeichen** verwendet wird. Wir empfehlen Ihnen die Verwendung dieser Samplingmethode, wenn Ihre Sammlungen heterogene Daten enthalten. Sie können diese Methode verwenden, um das Sampling an eine Gruppe von Attributen und die dazugehörigen Werte anzupassen. Wenn ein Dokument beispielsweise eine Type-Eigenschaft enthält, können Sie das Sampling auf die Werte dieser Eigenschaft beschränken. Das Endergebnis des Samplings ist eine Gruppe von Tabellen für jeden einzelnen Wert, den Sie für „Type“ angegeben haben. Mit „Type = Car“ wird beispielsweise eine Tabelle mit PKW erzeugt, während mit „Type = Plane“ eine Tabelle mit Flugzeugen erzeugt wird.

1. Klicken Sie nach Abschluss der Schritte 1 bis 4 unter [Herstellen der Verbindung mit Ihrer DocumentDB-Datenbank](#connect) im Fenster „DocumentDB ODBC Driver DSN Setup“ (DocumentDB-ODBC-Treiber – DSN-Setup) auf **Schema-Editor**.
2. Klicken Sie im Fenster **Schema-Editor** auf **Neu erstellen**.
    Im Fenster **Generate Schema** (Schema generieren) werden alle Sammlungen des DocumentDB-Kontos angezeigt. 
3. Wählen Sie auf der Registerkarte **Beispielansicht** in der Spalte **Mapping Definition** (Zuordnungsdefinition) für die Sammlung auf **Bearbeiten**. Wählen Sie im Fenster **Mapping Definition** (Zuordnungsdefinition) dann die Methode **Table Delimiters** (Tabellentrennzeichen). Gehen Sie wie folgt vor:

    a. Geben Sie im Feld **Attribute** den Namen einer Trennzeicheneigenschaft ein. Dies ist eine Eigenschaft in Ihrem Dokument, an die Sie das Sampling anpassen möchten, z.B. „City“. Drücken Sie anschließend die EINGABETASTE. 

    b. Wenn Sie das Sampling für das eben eingegebene Attribut nur auf bestimmte Werte beschränken möchten, wählen Sie das Attribut im Auswahlfeld aus, geben im Feld **Wert** einen Wert ein (z.B. Seattle) und drücken die EINGABETASTE. Sie können fortfahren, mehrere Werte für Attribute hinzuzufügen. Stellen Sie dabei lediglich sicher, dass beim Eingeben der Werte das richtige Attribut ausgewählt ist.

    Gehen Sie wie folgt vor, wenn Sie für **Attribute** beispielsweise den Wert „City“ angeben und die Tabelle auf Zeilen mit den Städten „New York“ und „Dubai“ als Wert für „City“ beschränken möchten: Geben Sie im Feld „Attribute“ den Text „City“ und dann im Feld **Werte** die Städte „New York“ und „Dubai“ ein.
4. Klicken Sie auf **OK**. 
5. Klicken Sie im Fenster **Schema-Editor** auf **Sample** (Beispiel), nachdem Sie die Zuordnungsdefinitionen für die Sammlungen, für die das Sampling durchgeführt werden soll, erstellt haben.
     Sie können für jede Spalte SQL-Spaltenname, SQL-Typ, SQL-Länge (falls zutreffend), Staffelung (falls zutreffend), Genauigkeit (falls zutreffend) und „Nullable“ angeben bzw. ändern.
    - Sie können **Hide Column** (Spalte ausblenden) auf **true** (wahr) festlegen, wenn Sie diese Spalte aus den Abfrageergebnissen ausschließen möchten. Spalten mit der Einstellung „Hide Column = true“ werden nicht für die Auswahl und Projektion zurückgegeben, sind aber weiterhin Teil des Schemas. Beispielsweise können Sie alle für das DocumentDB-System erforderlichen Eigenschaften ausblenden, die mit „_“ beginnen.
    - Die Spalte **id** ist das einzige Feld, das nicht ausgeblendet werden kann, da es als Primärschlüssel im normalisierten Schema verwendet wird. 
6. Klicken Sie nach dem Definieren des Schemas auf **Datei** | **Speichern**, navigieren Sie zum Verzeichnis, in dem das Schema gespeichert werden soll, und klicken Sie dann auf **Speichern**.
7. Klicken Sie im angezeigten Fenster **DocumentDB ODBC Driver DSN Setup** (DocumentDB-ODBC-Treiber – DSN-Setup) auf **Erweiterte Optionen**. Navigieren Sie dann im Feld **Schemadatei** zur gespeicherten Schemadatei, und klicken Sie auf **OK**. Klicken Sie erneut auf **OK**, um den DSN zu speichern. Das von Ihnen erstellte Schema wird unter dem DSN gespeichert. 

## <a name="optional-creating-views"></a>(Optional) Erstellen von Ansichten
Im Rahmen des Samplingprozesses können Sie Ansichten definieren und erstellen. Diese Ansichten entsprechen SQL-Ansichten. Sie sind schreibgeschützt und auf die Auswahl und Projektionen der definierten DocumentDB-SQL beschränkt. 

Klicken Sie im Fenster **Schema-Editor** in der Spalte **View Definitions** (Ansichtsdefinitionen) in der Zeile mit der Sammlung, für die das Sampling erstellt werden soll, auf **Hinzufügen**. Gehen Sie im Fenster **View Definitions** (Ansichtsdefinitionen) dann wie folgt vor:
1. Klicken Sie auf **Neu**, geben Sie einen Namen für die Ansicht ein (z.B. „EmployeesfromSeattleView“), und klicken Sie dann auf **OK**.
2. Geben Sie im Fenster **Ansicht bearbeiten** eine DocumentDB-Abfrage ein. Hierbei muss es sich um eine DocumentDB-SQL-Abfrage handeln, z.B. `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`. Klicken Sie anschließend auf **OK**.

Sie können beliebig viele Ansichten erstellen. Nachdem Sie die Ansichten definiert haben, können Sie das Sampling für die Daten durchführen. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Schritt 5: Anzeigen von Daten in BI-Tools, z.B. Power BI Desktop

Sie können Ihren neuen DSN verwenden, um für DocumentDB eine Verbindung mit beliebigen ODBC-konformen Tools herzustellen. In diesem Schritt wird lediglich veranschaulicht, wie Sie eine Verbindung mit Power BI Desktop herstellen und eine Power BI-Visualisierung erstellen.

1. Öffnen Sie Power BI Desktop.
2. Klicken Sie auf **Get Data** (Daten abrufen).
3. Klicken Sie im Fenster **Get Data** (Daten abrufen) auf **Other (Andere)** | **ODBC** | **Verbinden**.
4. Wählen Sie im Fenster **From ODBC** (Von ODBC) den von Ihnen erstellten Datenquellennamen aus, und klicken Sie anschließend auf **OK**. Sie können die Einträge unter **Erweiterte Optionen** leer lassen.
5. Wählen Sie im Fenster **Mit einem ODBC-Treiber auf eine Datenquelle zugreifen** die Option **Standard oder Benutzerdefiniert**, und klicken Sie dann auf **Verbinden**. Es ist nicht erforderlich, die **Eigenschaften für Anmeldedaten-Verbindungszeichenfolge** einzufügen.
6. Erweitern Sie im Fenster **Navigator** im linken Bereich die Datenbank und das Schema, und wählen Sie dann die Tabelle aus. Der Ergebnisbereich enthält die Daten gemäß dem von Ihnen verwendeten Schema.
7. Aktivieren Sie das Kontrollkästchen vor dem Tabellennamen, und klicken Sie dann auf **Laden**, um die Daten in Power BI Desktop zu visualisieren.
8. Wählen Sie in Power BI Desktop ganz links die Registerkarte „Daten“ aus, ![Registerkarte „Daten“ in Power BI Desktop](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-data-tab.png) um sich zu vergewissern, dass die Daten importiert wurden.
9. Sie können jetzt mit Power BI visuelle Elemente erstellen, indem Sie auf die Registerkarte „Bericht“ ![Registerkarte „Bericht“ in Power BI Desktop](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-report-tab.png) und auf **Neues visuelles Element** klicken und anschließend die Kachel anpassen. Weitere Informationen zur Erstellung von Visualisierungen in Power BI Desktop finden Sie unter [Visualisierungstypen in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Problembehandlung

Stellen Sie beim Auftreten des folgenden Fehlers sicher, dass die Werte für **Host** und **Zugriffsschlüssel**, die Sie in [Schritt 2](#connect) im Azure-Portal kopiert haben, korrekt sind. Wiederholen Sie anschließend den Vorgang. Verwenden Sie im Azure-Portal die Kopierschaltflächen rechts von den Werten **Host** und **Zugriffsschlüssel**, um die Werte frei von Fehlern zu kopieren.

    [HY000]: [Microsoft][DocumentDB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DocumentDB finden Sie unter [Was ist DocumentDB?](documentdb-introduction.md).

