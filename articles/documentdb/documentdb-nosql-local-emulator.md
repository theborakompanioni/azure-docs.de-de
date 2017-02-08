---
title: Lokale Entwicklung mit dem DocumentDB-Emulator | Microsoft-Dokumentation
description: "Mit dem DocumentDB-Emulator können Sie Ihre Anwendung kostenlos lokal entwickeln und testen, ohne ein Azure-Abonnement zu erstellen."
services: documentdb
documentationcenter: 
keywords: DocumentDB-Emulator
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 6c5bf8907a5f69e45e7b62fb466bdc53460e9029
ms.openlocfilehash: 86a5911e99e7631b09604afcb0f53ed2887b576b


---
# <a name="use-the-azure-documentdb-emulator-for-development-and-testing"></a>Einsatz des Azure-DocumentDB-Emulators für Entwicklung und Tests

[**Emulator herunterladen**](https://aka.ms/documentdb-emulator)

Der Azure DocumentDB-Emulator bietet eine lokale Umgebung, die zu Entwicklungszwecken den Azure DocumentDB-Dienst emuliert. Durch Verwendung des DocumentDB-Emulators können Sie Ihre Anwendung lokal entwickeln und testen, ohne ein Azure-Abonnement zu erstellen oder sonstige Kosten zu verursachen. Wenn Sie mit der Funktion der Anwendung im DocumentDB-Emulator zufrieden sind, können Sie zur Verwendung eines Azure DocumentDB-Kontos in der Cloud wechseln.

Es wird empfohlen, für den Einstieg das folgende Video anzusehen, in dem Kirill Gavrylyuk Ihnen die ersten Schritte mit dem DocumentDB-Emulator zeigt.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="documentdb-emulator-system-requirements"></a>DocumentDB-Emulator – Systemanforderungen
Für den DocumentDB-Emulator gelten folgende Hardware- und Softwareanforderungen:

* Softwareanforderungen
  * Windows Server 2012 R2, Windows Server 2016 oder Windows 10
*   Minimale Hardwareanforderungen
  * 2 GB RAM
  * 10 GB verfügbarer Festplattenspeicher

## <a name="installing-the-documentdb-emulator"></a>Installieren des DocumentDB-Emulators
Sie können den DocumentDB-Emulator aus dem [Microsoft Download Center](https://aka.ms/documentdb-emulator) herunterladen und installieren. 

> [!NOTE]
> Zum Installieren, Konfigurieren und Ausführen des DocumentDB-Emulators benötigen Sie Administratorrechte auf dem Computer.

## <a name="checking-for-documentdb-emulator-updates"></a>Suchen nach Updates für den DocumentDB-Emulator
Der DocumentDB-Emulator enthält einen integrierten Azure DocumentDB-Daten-Explorer, um in DocumentDB gespeicherte Daten zu durchsuchen, neue Sammlungen zu erstellen und Sie darüber zu informieren, wenn ein neues Update zum Download verfügbar ist. 

> [!NOTE]
> Der Zugriff auf Daten, die in einer bestimmten Version des DocumentDB-Emulators erstellt wurden, ist bei Verwendung einer anderen Version nicht gewährleistet. Wenn Sie die Daten langfristig beibehalten müssen, wird empfohlen, diese Daten in einem Azure DocumentDB-Konto zu speichern, nicht im DocumentDB-Emulator. 

## <a name="how-the-documentdb-emulator-works"></a>Funktionsweise des DocumentDB-Emulators
Der DocumentDB-Emulator bietet eine sehr detailgetreue Emulation des DocumentDB-Diensts. Er unterstützt die gleichen Funktionen wie Azure DocumentDB, einschließlich Erstellen und Abfragen von JSON-Dokumenten, Bereitstellen und Skalieren von Sammlungen und Ausführen von gespeicherten Prozeduren und Triggern. Sie können Anwendungen mit dem DocumentDB-Emulator entwickeln und testen und diese in Azure auf globaler Ebene bereitstellen, indem Sie nur eine einzige Konfigurationsänderung am Verbindungsendpunkt für DocumentDB vornehmen.

Wir haben zwar eine sehr detailgetreue Emulation des tatsächlichen DocumentDB-Diensts erstellt, allerdings unterscheidet sich die Implementierung des DocumentDB-Emulators von der des Diensts. Der DocumentDB-Emulator verwendet Standardkomponenten des Betriebssystems, wie z.B. das lokale Dateisystem für Persistenz und den HTTPS-Protokollstapel für Verbindungen. Dies bedeutet, dass einige Funktionen, die die Azure-Infrastruktur benötigen, über den DocumentDB-Emulator nicht verfügbar sind. Hierzu gehören beispielsweise die globale Replikation, Lese-/Schreibvorgänge mit Latenz im einstelligen Millisekundenbereich und optimierbare Konsistenzebenen.


## <a name="authenticating-requests-against-the-documentdb-emulator"></a>Authentifizieren von Anforderungen an den DocumentDB-Emulator
Genau wie bei Azure DocumentDB in der Cloud muss jede Anforderung, die Sie für den DocumentDB-Emulator durchführen, authentifiziert werden. Der DocumentDB-Emulator unterstützt ein einziges festgelegtes Konto und einen bekannten Authentifizierungsschlüssel für die Authentifizierung per Hauptschlüssel. Dieses Konto und dieser Schlüssel sind die einzigen Anmeldeinformationen, die für den DocumentDB-Emulator verwendet werden dürfen. Sie lauten wie folgt:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> Der vom DocumentDB-Emulator unterstützte Hauptschlüssel ist nur für die Verwendung mit dem Emulator gedacht. Sie können das DocumentDB-Produktionskonto und den zugehörigen Schlüssel nicht mit dem DocumentDB-Emulator verwenden. 

Darüber hinaus unterstützt der DocumentDB-Emulator, genau wie der Azure DocumentDB-Dienst, nur die sichere Kommunikation über SSL.

## <a name="start-and-initialize-the-documentdb-emulator"></a>Starten und Initialisieren des DocumentDB-Emulators

Wählen Sie zum Starten des Azure-DocumentDB-Emulators die Schaltfläche „Start“, oder drücken Sie die WINDOWS-TASTE. Beginnen Sie mit der Eingabe von **DocumentDB-Emulator**, und wählen Sie den Emulator aus der Liste der Anwendungen aus. 

![Wählen Sie die Schaltfläche „Start“, oder drücken Sie die WINDOWS-TASTE, beginnen Sie mit der Eingabe von **DocumentDB-Emulator**, und wählen Sie den Emulator aus der Liste der Anwendungen aus.](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-start.png)

Wenn der Emulator ausgeführt wird, wird im Infobereich der Windows-Taskleiste ein Symbol angezeigt. Der DocumentDB-Emulator wird standardmäßig auf dem lokalen Computer ausgeführt und lauscht auf Port 8081.

![Benachrichtigung des lokalen DocumentDB-Emulators auf der Taskleiste](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-taskbar.png)

Der DocumentDB-Emulator wird standardmäßig im Verzeichnis `C:\Program Files\DocumentDB Emulator` installiert. Sie können den Emulator auch über die Befehlszeile starten und beenden. Weitere Informationen finden Sie unter [Referenz zum Befehlszeilentool](#command-line).

## <a name="start-the-local-emulator-data-explorer"></a>Starten des lokalen Daten-Explorers des Emulators

Beim Starten des lokalen Emulators wird der DocumentDB-Daten-Explorer automatisch in ihrem Browser geöffnet. Die Adresse wird folgendermaßen angezeigt. [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Wenn Sie den Explorer schließen und ihn später erneut öffnen möchten, können Sie entweder die URL in Ihrem Browser öffnen oder den Explorer über den DocumentDB-Emulator im Windows-Taskleistensymbol starten, wie unten gezeigt.

![Startprogramm für den Daten-Explorer des lokalen DocumentDB-Emulators](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-data-explorer-launcher.png)

## <a name="developing-with-the-documentdb-emulator"></a>Entwickeln mit dem DocumentDB-Emulator
Sobald der DocumentDB-Emulator auf Ihrem Desktop ausgeführt wird, können Sie jedes unterstützte [DocumentDB SDK](documentdb-sdk-dotnet.md) oder die [DocumentDB-REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) verwenden, um mit dem Emulator zu interagieren. Der DocumentDB-Emulator enthält auch einen integrierten Daten-Explorer, mit dem Sie Sammlungen erstellen und Dokumente anzeigen und bearbeiten können, ohne Code schreiben zu müssen. 

    // Connect to the DocumentDB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==",
        new ConnectionPolicy { EnableEndpointDiscovery = false });

> [!NOTE]
> Beim Herstellen einer Verbindung mit dem Emulator müssen Sie in der Verbindungskonfiguration die Einstellung „EnableEndpointDiscovery = false“ festlegen.

Wenn Sie die [DocumentDB-Protokollunterstützung für MongoDB](documentdb-protocol-mongodb.md) verwenden, geben Sie folgende Verbindungszeichenfolge an:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10250/admin?ssl=true&3t.sslSelfSignedCerts=true

Sie können vorhandene Tools wie [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) verwenden, um eine Verbindung mit dem DocumentDB-Emulator herzustellen. Sie können auch mit dem [DocumentDB-Datenmigrationstool](https://github.com/azure/azure-documentdb-datamigrationtool) Daten zwischen dem DocumentDB-Emulator und dem Azure DocumentDB-Dienst migrieren.

## <a name="export-the-documentdb-emulator-ssl-certificate"></a>Exportieren des SSL-Zertifikats für den DocumentDB-Emulator

.NET-Sprachen und -Runtime verwenden den Windows-Zertifikatspeicher, um sichere Verbindungen mit dem lokalen DocumentDB-Emulator herzustellen. Andere Sprachen bieten andere Methoden für die Verwaltung und Verwendung von Zertifikaten. Java verwendet einen eigenen [Zertifikatspeicher](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), Python dagegen verwendet [Wrapper für Socketobjekte](https://docs.python.org/2/library/ssl.html).

Um ein Zertifikat zur Verwendung mit Sprachen und Laufzeiten abzurufen, die nicht in den Windows-Zertifikatspeicher integriert sind, müssen Sie dieses mithilfe des Windows-Zertifikat-Managers exportieren. Starten Sie den Manager, indem Sie „certlm.msc“ ausführen oder die Schrittanweisungen im Artikel [Exportieren der DocumentDB-Emulatorzertifikate](./documentdb-nosql-local-emulator-export-ssl-certificates.md) befolgen. Sobald der Zertifikat-Manager ausgeführt wird, öffnen Sie die persönlichen Zertifikate, wie unten gezeigt, und exportieren Sie das Zertifikat mit dem Anzeigenamen „DocumentDBEmulatorCertificate“ als Base64-codierte X.509-CER-Datei.

![SSL-Zertifikat für den lokalen DocumentDB-Emulator](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-ssl_certificate.png)

Importieren Sie das X.509-Zertifikat in den Java-Zertifikatspeicher, indem Sie die Anweisungen unter [Hinzufügen eines Zertifikats zum Java CA-Zertifikatspeicher](https://docs.microsoft.com/en-us/azure/java-add-certificate-ca-store) befolgen.  Sobald das Zertifikat in den cacerts-Speicher importiert wurde, können Java- und MongoDB-Anwendungen eine Verbindung mit dem lokalen DocumentDB-Emulator herstellen.

## <a name="a-idcommand-lineadocumentdb-emulator-command-line-tool-reference"></a><a id="command-line"></a>Referenz zum DocumentDB-Emulator-Befehlszeilentool
Sie können die Befehlszeile im Installationspfad verwenden, um den Emulator zu starten und zu beenden, Optionen zu konfigurieren und andere Vorgänge auszuführen.

### <a name="command-line-syntax"></a>Befehlszeilensyntax

    DocumentDB.Emulator.exe [/shutdown] [/datapath] [/port] [/mongoport] [/directports] [/key] [/?]

Geben Sie zum Anzeigen der Liste der Optionen an der Eingabeaufforderung `DocumentDB.Emulator.exe /?` ein.

<table>
<tr>
  <td><strong>Option</strong></td>
  <td><strong>Beschreibung</strong></td>
  <td><strong>Befehl</strong></td>
  <td><strong>Argumente</strong></td>
</tr>
<tr>
  <td>[Keine Argumente]</td>
  <td>Startet den DocumentDB-Emulator mit den Standardeinstellungen.</td>
  <td>DocumentDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>Shutdown</td>
  <td>Fährt den DocumentDB-Emulator herunter.</td>
  <td>DocumentDB.Emulator.exe /Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>Hilfe</td>
  <td>Zeigt die Liste der Befehlszeilenargumente an.</td>
  <td>DocumentDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>Datapath</td>
  <td>Gibt den Pfad an, in dem Datendateien gespeichert werden sollen.</td>
  <td>DocumentDB.Emulator.exe /datapath=&lt;Datenpfad&gt;</td>
  <td>&lt;datapath&gt;: Ein zugreifbarer Pfad.</td>
</tr>
<tr>
  <td>Port</td>
  <td>Gibt die für den Emulator zu verwendende Portnummer an.  Der Standardwert ist 8081.</td>
  <td>DocumentDB.Emulator.exe /port=&lt;Port&gt;</td>
  <td>&lt;port&gt;: Einzelne Portnummer.</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Gibt die für die MongoDB-Kompatibilitäts-API zu verwendende Portnummer an. Der Standardwert ist 10250.</td>
  <td>DocumentDB.Emulator.exe /mongoport=&lt;MongoPort&gt;</td>
  <td>&lt;mongoport&gt;: Einzelne Portnummer.</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Gibt die für die direkte Konnektivität zu verwendenden Ports an. Die Standardwerte sind 10251,10252,10253,10254.</td>
  <td>DocumentDB.Emulator.exe /directports:&lt;DirectPorts&gt;</td>
  <td>&lt;directports&gt;: Eine durch Trennzeichen getrennte Liste mit 4 Ports.</td>
</tr>
<tr>
  <td>Schlüssel</td>
  <td>Autorisierungsschlüssel für den Emulator. Der Schlüssel muss die Base64-Codierung eines 64-Bytes-Vektors sein.</td>
  <td>DocumentDB.Emulator.exe /key:&lt;Schlüssel&gt;</td>
  <td>&lt;key&gt;: Der Schlüssel muss die Base64-Codierung eines 64-Bytes-Vektors sein.</td>
</tr>
<tr>
  <td>EnableThrottling</td>
  <td>Gibt an, dass das Verhalten für die Anforderungsbegrenzung aktiviert ist.</td>
  <td>DocumentDB.Emulator.exe /enablethrottling</td>
  <td></td>
</tr>
<tr>
  <td>DisableThrottling</td>
  <td>Gibt an, dass das Verhalten für die Anforderungsbegrenzung deaktiviert ist.</td>
  <td>DocumentDB.Emulator.exe /disablethrottling</td>
  <td></td>
</tr>
</table>

## <a name="differences-between-the-documentdb-emulator-and-azure-documentdb"></a>Unterschiede zwischen dem DocumentDB-Emulator und Azure DocumentDB 
Da der DocumentDB-Emulator eine emulierte Umgebung bereitstellt, die auf einer lokalen Entwicklerarbeitsstation ausgeführt wird, gibt es einige funktionelle Unterschiede zwischen dem Emulator und einem Azure DocumentDB-Konto in der Cloud:

* Der DocumentDB-Emulator unterstützt nur ein einziges festgelegtes Konto und einen bekannten Hauptschlüssel.  Im DocumentDB-Emulator ist es nicht möglich, einen Schlüssel neu zu generieren.
* Der DocumentDB-Emulator ist kein skalierbarer Speicherdienst und unterstützt keine große Anzahl von Sammlungen.
* Der DocumentDB-Emulator simuliert keine verschiedenen [DocumentDB-Konsistenzebenen](documentdb-consistency-levels.md).
* Der DocumentDB-Emulator simuliert keine [Replikation in mehreren Regionen](documentdb-distribute-data-globally.md).
* Der DocumentDB-Emulator unterstützt nicht die Außerkraftsetzungen von Dienstkontingenten, die im Azure DocumentDB-Dienst verfügbar sind (z.B. Dokumentgrößenlimits, größerer partitionierter Sammlungsspeicher).
* Möglicherweise ist Ihre Kopie des DocumentDB-Emulators nicht auf dem neuesten Stand der Änderungen im Azure DocumentDB-Dienst. Verwenden Sie daher den [DocumentDB-Kapazitätsplaner](https://www.documentdb.com/capacityplanner), um den erforderlichen Produktionsdurchsatz (RUs, Request Units) für Ihre Anwendung richtig zu einzuschätzen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu DocumentDB finden Sie unter [Einführung in Azure DocumentDB](documentdb-introduction.md).
* Um mit dem Entwickeln mithilfe des DocumentDB-Emulators zu beginnen, laden Sie eins der [unterstützten DocumentDB SDKs](documentdb-sdk-dotnet.md) herunter.



<!--HONumber=Dec16_HO3-->


