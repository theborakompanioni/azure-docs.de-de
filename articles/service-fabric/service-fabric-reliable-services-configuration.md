<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric Reliable Services | Microsoft Azure"
   description="Erfahren Sie mehr über das Konfigurieren zustandsbehafteter Reliable Services in Azure Service Fabric."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# Konfigurieren zustandsbehafteter Reliable Services

Es gibt zwei Sets von Konfigurationseinstellungen für Reliable Services. Ein Set gilt global für alle Reliable Services im Cluster, während das andere für den jeweiligen Reliable Service spezifisch ist.

## Globale Konfiguration

Die globale Konfiguration für Reliable Services wird im Clustermanifest für den Cluster im Abschnitt „KtlLogger“ angegeben. Sie können daher jetzt für das freigegebene Protokoll Folgendes konfigurieren: den Speicherort und die Größe, sowie die vom Protokollierungstool verwendeten globalen Speicherlimits. Das Clustermanifest ist eine einzelne XML-Datei, die Einstellungen und Konfigurationen für alle Knoten und Dienste im Cluster enthält. Die Datei heißt normalerweise „ClusterManifest.xml“. Mit dem PowerShell-Befehl Get-ServiceFabricClusterManifest können Sie das Clustermanifest für Ihren Cluster sehen.

### Konfigurationsnamen

|Name|Unit|Standardwert|Hinweise|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilobytes|8388608|KB-Mindestwert, der im Kernelmodus dem Schreibpuffer-Speicherpool des Protokollierungstools zugeordnet wird. Dieser Speicherpool wird zum Zwischenspeichern von Zustandsinformationen verwendet, bevor auf den Datenträger geschrieben wird.|
|WriteBufferMemoryPoolMaximumInKB|Kilobytes|Keine Begrenzung|Maximale Größe, auf die der Schreibpuffer-Speicherpool des Protokollierungstools anwachsen kann.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID an, die zum Identifizieren der standardmäßigen freigegebenen Protokolldatei verwendet wird. Die Datei wird von allen Reliable Services auf allen Knoten im Cluster verwendet, die in ihren dienstspezifischen Konfigurationen nicht die SharedLogId angeben. Falls SharedLogId angegeben wird, muss SharedLogPath ebenfalls angegeben werden.|
|SharedLogPath|Vollständig qualifizierter Pfadname|""|Gibt den vollständig qualifizierten Pfad zur freigegebenen Protokolldatei an. Die Datei wird von allen Reliable Services auf allen Knoten im Cluster verwendet, die in ihren dienstspezifischen Konfigurationen nicht den SharedLogPath angeben. Aber wenn SharedLogPath angegeben ist, muss SharedLogId ebenfalls angegeben werden.|
|SharedLogSizeInMB|Megabytes|8192|Gibt den MB-Wert für den Festplattenspeicher an, der für das freigegebene Protokoll statisch zugeordnet wird. Der Wert muss größer oder gleich 2.048 sein.|

### Beispiel für einen Clustermanifestabschnitt
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### Hinweise
Das Protokollierungstool verfügt über einen globalen Pool mit Speicher, der aus einem nicht ausgelagerten Kernelspeicher zugeordnet wird. Dieser ist für alle Reliable Services auf einem Knoten zum Zwischenspeichern von Zustandsdaten verfügbar, bevor diese in das dedizierte, dem Reliable Service-Replikat zugeordnete Protokoll geschrieben werden. Die Poolgröße wird mit WriteBufferMemoryPoolMinimumInKB und den Einstellungen von WriteBufferMemoryPoolMaximumInKB gesteuert. Mit WriteBufferMemoryPoolMinimumInKB wird sowohl die Anfangsgröße des Speicherpools als auch die kleinste Größe angegeben, auf die der Speicherpool verkleinert werden kann. WriteBufferMemoryPoolMaximumInKB ist die maximale Größe, auf die der Speicherpool anwachsen kann. Jedes geöffnete Reliable Services-Replikat kann die Größe des Speicherpools um einen vom System bestimmten Betrag maximal auf die in WriteBufferMemoryPoolMaximumInKB angegebene Größe erhöhen. Falls der Bedarf an Speicher aus dem Speicherpool die Verfügbarkeit übersteigt, werden Speicheranforderungen zurückgestellt, bis wieder Speicher verfügbar ist. Falls der Schreibpuffer-Speicherpool zu klein für eine bestimmte Konfiguration ist, kann dies die Leistung negativ beeinträchtigen.

Die Einstellungen von SharedLogId und SharedLogPath werden immer zusammen verwendet, um die GUID und den Speicherort für das standardmäßige freigegebene Protokoll für alle Knoten im Cluster zu definieren. Das standardmäßige freigegebene Protokoll wird für alle Reliable Services verwendet, bei denen die Einstellungen nicht in der Datei „Settings.xml“ für den jeweiligen Dienst angegeben werden. Um die beste Leistung zu erzielen, sollten freigegebene Protokolldateien auf Datenträgern gespeichert werden, die ausschließlich für die freigegebene Protokolldatei verwendet werden. So werden Konflikte reduziert.

Mit SharedLogSizeInMB wird die Menge an Festplattenspeicher angegeben, die für das standardmäßige freigegebene Protokoll auf allen Knoten vorab zugewiesen werden soll. SharedLogId und SharedLogPath müssen nicht angegeben sein, um SharedLogSizeInMB angeben zu können.


## Dienstspezifische Konfiguration
Sie können die Standardkonfigurationen für zustandsbehaftete Reliable Services mit dem Konfigurationspaket (Config) oder der Dienstimplementierung (Code) ändern.

+ **Config**: Die Konfiguration über das Konfigurationspaket erfolgt für jeden Dienst in der Anwendung durch Ändern der Datei „Settings.xml“, die im Stammverzeichnis des Microsoft Visual Studio-Pakets im Ordner „Config“ generiert wurde.
+ **Code**: Die Konfiguration über Code erfolgt durch das Erstellen eines ReliableStateManager mithilfe eines ReliableStateManagerConfiguration-Objekts und der Festlegung der passenden Optionen.

Standardmäßig sucht die Azure Service Fabric-Laufzeit in der Datei „Settings.xml“ nach vordefinierten Abschnittsnamen und nutzt die Konfigurationswerte beim Erstellen der zugrunde liegenden Laufzeitkomponenten.

>[AZURE.NOTE] Sofern Sie die Konfiguration nicht per Code vornehmen möchten, löschen Sie **auf keinen Fall** die Abschnittsnamen der folgenden Konfigurationen in der Datei „Settings.xml“, die in der Visual Studio-Projektmappe generiert wird. Das Umbenennen des Konfigurationspakets oder der Abschnittsnamen erfordert eine Änderung des Codes beim Konfigurieren von ReliableStateManager.


### Replicator-Sicherheitskonfiguration
Replicator-Sicherheitskonfigurationen werden verwendet, um den während der Replikation verwendeten Kommunikationskanal zu schützen. Dies bedeutet, dass Dienste nicht ihren gegenseitigen Replikationsdatenverkehr erkennen können. Dadurch wird sichergestellt, dass die Daten nicht nur hochverfügbar, sondern auch sicher sind. Standardmäßig wird die Replikationssicherheit durch einen leeren Sicherheitskonfigurationsabschnitt verhindert.

### Standardmäßiger Abschnittsname
ReplicatorSecurityConfig

>[AZURE.NOTE] Zum Ändern dieses Abschnittsnamens überschreiben Sie den ReplicatorSecuritySectionName-Parameter für den ReliableStateManagerConfiguration-Konstruktor beim Erstellen von ReliableStateManager für diesen Dienst.


### Replicator-Konfiguration
Replicator-Konfigurationen werden zum Konfigurieren des Replicators verwendet, der für die hohe Zuverlässigkeit des zustandsbehafteten Reliable Service verantwortlich ist. Er repliziert und speichert den Zustand zu diesem Zweck lokal. Die Standardkonfiguration wird von der Visual Studio-Vorlage generiert und sollte ausreichen. Dieser Abschnitt befasst sich mit zusätzlichen Konfigurationen, die zum Optimieren des Replicators verfügbar sind.

### Standardmäßiger Abschnittsname
ReplicatorConfig

>[AZURE.NOTE] Zum Ändern dieses Abschnittsnamens überschreiben Sie den ReplicatorSettingsSectionName-Parameter für den ReliableStateManagerConfiguration-Konstruktor beim Erstellen von ReliableStateManager für diesen Dienst.


### Konfigurationsnamen
|Name|Unit|Standardwert|Hinweise|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0,015|So lange wartet der Replicator auf dem sekundären Replicator nach dem Empfang eines Vorgangs, bevor er eine Bestätigung an den primären Replicator sendet. Alle anderen Bestätigungen, die für innerhalb dieses Intervalls verarbeitete Vorgänge gesendet werden, werden als eine einzelne Antwort gesendet.|
|ReplicatorEndpoint|–|Kein Standardwert – Erforderlicher Parameter|Die IP-Adresse und der Port, die der primäre/sekundäre Replicator für die Kommunikation mit anderen Replicatoren in der Replikatgruppe verwendet. Dabei sollte im Dienstmanifest auf einen TCP-Ressourcenendpunkt verwiesen werden. Weitere Informationen zum Definieren von Endpunktressourcen in einem Dienstmanifest finden Sie unter [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md). |
|MaxPrimaryReplicationQueueSize|Anzahl der Vorgänge|8192|Die maximale Anzahl der Vorgänge in der primären Warteschlange. Ein Vorgang wird freigegeben, nachdem der primäre Replicator eine Bestätigung von allen sekundären Replicators empfangen hat. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl der Vorgänge|16384|Die maximale Anzahl der Vorgänge in der sekundären Warteschlange. Ein Vorgang wird freigegeben, nachdem sein Zustand durch Persistenz hochverfügbar gemacht wurde. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|CheckpointThresholdInMB|MB|50|Die Menge an Speicherplatz für Protokolldateien, nach dem der Status geprüft wird.|
|MaxRecordSizeInKB|KB|1024|Die maximale Datensatzgröße, die der Replicator in das Protokoll schreiben kann. Dieser Wert muss ein Vielfaches von 4 und größer als 16 sein.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID zum Identifizieren der freigegebenen Protokolldatei an, die mit diesem Replikat verwendet wird. Diese Einstellung sollte von Diensten normalerweise nicht verwendet werden. Aber wenn SharedLogId angegeben ist, muss SharedLogPath ebenfalls angegeben werden.|
|SharedLogPath|Vollständig qualifizierter Pfadname|""|Gibt den vollständig qualifizierten Pfad an, in dem die freigegebene Protokolldatei für dieses Replikat erstellt wird. Diese Einstellung sollte von Diensten normalerweise nicht verwendet werden. Aber wenn SharedLogPath angegeben ist, muss SharedLogId ebenfalls angegeben werden.|
|SlowApiMonitoringDuration|Sekunden|300|Legt das Überwachungsintervall für verwaltete API-Aufrufe fest. Beispiel: Vom Benutzer bereitgestellte Sicherungsrückruffunktion. Nach Ablauf des Intervalls wird ein Warnbericht zur Integrität an den Health Manager gesendet.|

### Beispielkonfiguration per Code
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### Beispiel für eine Konfigurationsdatei
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### Hinweise
"BatchAcknowledgementInterval" steuert die Replikationslatenz. Der Wert "0" ergibt die geringstmögliche Latenz, allerdings auf Kosten des Durchsatzes (da eine größer Anzahl von Bestätigungsnachrichten gesendet und verarbeitet werden muss, von denen jede weniger Bestätigungen enthält). Je größer der Wert für "BatchAcknowledgementInterval" ist, um so höher ist der Gesamtdurchsatz der Replikation, zu Lasten einer höheren Vorgangslatenz. Daraus ergibt sich direkt die Latenz von Transaktions-Commits.

Der Wert für "MaxStreamSizeInMB" bestimmt die Menge des Speicherplatzes, den der Replicator zum Speichern von Zustandsinformationen in der dedizierten Protokolldatei des Replikats verwenden kann. Das Festlegen dieses Werts auf einen höheren Wert als die Standardeinstellung kann zu einer kürzeren Dauer der Neukonfiguration führen, wenn dem Satz ein neues Replikat hinzugefügt wird. Dies liegt an der partiellen Statusübertragung, die aufgrund der Verfügbarkeit eines umfangreicheren Vorgangsverlaufs im Protokoll stattfindet. Die Wiederherstellung eines Replikats nach einem Absturz kann dadurch allerdings mehr Zeit in Anspruch nehmen.

Die Einstellung MaxRecordSizeInKB definiert die maximale Größe eines Datensatzes, der vom Replicator in die Protokolldatei geschrieben werden kann. In den meisten Fällen ist die Standardgröße von 1024 KB für Datensätze optimal. Wenn für den Dienst aber größere Datenelemente Teil der Zustandsinformationen sind, muss dieser Wert ggf. erhöht werden. Es nützt wenig, für MaxRecordSizeInKB einen Wert unter 1024 festzulegen, da kleinere Datensätze nur den für sie erforderlichen Speicherplatz belegen. Dieser Wert muss in der Regel nur in seltenen Ausnahmefällen geändert werden.

Die Einstellungen SharedLogId und SharedLogPath werden immer zusammen verwendet. Sie ermöglichen einem Dienst, ein separates freigegebenes Protokoll aus dem freigegebenen Standardprotokoll für den Knoten zu verwenden. Zur Optimierung der Effizienz sollten so viele Dienste wie möglich dasselbe freigegebene Protokoll angeben. Freigegebene Protokolldateien sollten auf Datenträgern gespeichert werden, die ausschließlich für die freigegebene Protokolldatei verwendet werden. So werden Konflikte durch die Bewegungen des Lesekopfs reduziert. Dieser Wert muss in der Regel nur in seltenen Ausnahmefällen geändert werden.

## Nächste Schritte
 - [Debuggen einer Service Fabric-Anwendung in Visual Studio](service-fabric-debugging-your-application.md)
 - [Entwicklerreferenz für zuverlässige Dienste](https://msdn.microsoft.com/library/azure/dn706529.aspx)

<!---HONumber=AcomDC_0921_2016-->