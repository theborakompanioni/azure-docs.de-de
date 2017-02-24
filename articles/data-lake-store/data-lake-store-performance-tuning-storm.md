---
title: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für Storm | Microsoft-Dokumentation"
description: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für Storm"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
translationtype: Human Translation
ms.sourcegitcommit: 518d586a921926874cd959587ad2730ad346df71
ms.openlocfilehash: cae3f0192d6b0e6204ede4c1855b731a2ffa4ae1


---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Anleitung für die Leistungsoptimierung für Storm in HDInsight und Azure Data Lake Store

Beim Optimieren der Leistung einer Storm-Topologie müssen einige Faktoren berücksichtigt werden.  Es ist wichtig, die Arbeitsschritte der Spouts und Bolts zu verstehen (ob der E/A- bzw. Arbeitsspeicheraufwand hoch ist).

## <a name="prerequisites"></a>Voraussetzungen 

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md) 
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake-Speicherkonto. Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist. 
* **Ausführen eines Storm-Clusters in Azure Data Lake Store**.  Weitere Informationen finden Sie unter [Storm in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview). 
* **Leitfaden für die Optimierung der Leistung von Azure Data Lake Store**.  Allgemeine Leistungskonzepte finden Sie unter [Anleitung für die Leistungsoptimierung von Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

**Optimieren der Parallelität der Topologie**

Eine bessere Leistung kann unter Umständen erzielt werden, indem die Parallelität der Ein-/Ausgabe für Azure Data Lake Store erhöht wird.  
Eine Storm-Topologie verfügt über eine Reihe von Konfigurationen, mit denen die Parallelität bestimmt wird:
* Anzahl von Workerprozessen: Die Worker sind gleichmäßig auf die VMs verteilt.
* Anzahl von Spout Executor-Instanzen
* Anzahl von Bolt Executor-Instanzen
* Anzahl von Spout-Aufgaben
* Anzahl von Bolt-Aufgaben

Beispiel für einen Cluster mit 4 VMs und 4 Workerprozessen, 32 Spout Executors und 32 Spout-Aufgaben sowie 256 Bolt Executors und 512 Bolt-Aufgaben:

Jeder Supervisor, bei dem es sich um einen Workerknoten handelt, verfügt über einen einzelnen Worker-JVM-Prozess, mit dem 4 Spout-Threads und 64 Bolt-Threads verwaltet werden. Innerhalb jedes Threads werden Aufgaben sequenziell ausgeführt. Mit der obigen Konfiguration verfügt jeder Spout-Thread über eine Aufgabe und jeder Bolt-Thread über zwei Aufgaben.

Für Storm gilt Folgendes (hier sind die verschiedenen beteiligten Komponenten und ihre Auswirkung auf den Parallelitätsgrad aufgeführt):
* Der Hauptknoten (bei Storm als „Nimbus“ bezeichnet) wird zum Senden und Verwalten von Aufträgen verwendet. Diese Knoten haben keine Auswirkung auf den Parallelitätsgrad.
* Supervisorknoten: In Azure HDInsight entspricht dies einer Workerknoten-Azure-VM.
* Die Workertasks sind Storm-Prozesse, die auf den VMs ausgeführt werden. Jede Workertask entspricht einer Java-JVM-Instanz. Storm verteilt die von Ihnen angegebene Anzahl von Workerprozessen so gleichmäßig wie möglich auf die Workerknoten.
* Spout und Bolt Executor-Instanzen: Jede Executor-Instanz entspricht einem Thread, der in den Workern (JVMs) ausgeführt wird.
* Storm-Aufgaben: Dies sind logische Aufgaben, die auf diesen Threads jeweils ausgeführt werden. Der Parallelitätsgrad wird hierdurch nicht geändert. Sie sollten also ermitteln, ob Sie mehrere Aufgaben pro Executor benötigen oder ob dies nicht erforderlich ist.

## <a name="guidance"></a>Anleitungen

Bei der Verwendung von Azure Data Lake erzielen Sie die beste Leistung, wenn Sie wie folgt vorgehen:
* Fügen Sie kleinere Anfügungen zu größeren Paketen zusammen (idealerweise 4 MB).
* Führen Sie so viele gleichzeitige Anforderungen wie möglich durch. Da von jedem Bolt-Thread blockierende Lesevorgänge durchgeführt werden, sollten Sie ca. 8 bis 12 Threads pro Kern verwenden, um die NIC und die CPU ausreichend stark auszulasten.  Eine größere VM ermöglicht mehr gleichzeitige Anforderungen.  

## <a name="example"></a>Beispiel

Angenommen, Sie verwenden einen Cluster mit acht Workerknoten mit einer D13v2 Azure-VM.  Eine D13v2 VM hat acht Kerne, sodass sich für die acht Workerknoten insgesamt 64 Kerne ergeben.

Angenommen, es werden acht Bolt-Threads pro Kern verwendet. Bei 64 Kernen bedeutet dies, dass wir insgesamt 512 Bolt Executor-Instanzen (also Threads) benötigen. In diesem Fall beginnen wir beispielsweise mit einer JVM pro VM und verwenden hauptsächlich die Threadparallelität in der JVM, um Parallelität zu erzielen. Dies bedeutet, dass wir acht Workertasks (eine pro Azure-VM) und 512 Bolt Executors benötigen. Bei dieser Konfiguration versucht Storm, die Worker gleichmäßig auf die Workerknoten (Supervisorknoten) zu verteilen und jedem Workerknoten eine JVM zuzuordnen. Innerhalb der Supervisor versucht Storm, die Executors gleichmäßig auf die Supervisor zu verteilen, sodass jeder Supervisor (JVM) acht Threads aufweist.

## <a name="further-tuning"></a>Weitere Optimierung
Nachdem Sie über die grundlegende Topologie verfügen, können Sie überlegen, ob Sie Parameter optimieren möchten:
* **Anzahl von JVMs pro Workerknoten:** Wenn Sie über eine große Datenstruktur verfügen (z.B. eine Suchtabelle), die Sie im Arbeitsspeicher hosten, ist für jede JVM eine separate Kopie erforderlich. Bei Verwendung von weniger JVMs können Sie die Struktur dagegen über viele Threads hinweg verwenden. Für den Ein-/Ausgang des Bolts macht die Anzahl von JVMs keinen so großen Unterschied wie die Anzahl von Threads aus, die für diese JVMs hinzugefügt werden. Der Einfachheit halber empfehlen wir eine JVM pro Worker. Je nachdem, welche Aufgabe Ihr Bolt hat oder welche Anwendungsverarbeitung Sie benötigen, müssen Sie ggf. ermitteln, ob eine Anpassung dieser Zahl sinnvoll ist.
* **Anzahl von Spout Executors:** Da in diesem Beispiel Bolts zum Schreiben in Azure Data Lake verwendet werden, ist die Anzahl von Spouts nicht direkt relevant für die Bolt-Leistung. Je nach Verarbeitungs- oder E/A-Aufwand im Spout kann es ratsam sein, die Spouts zu optimieren, um die beste Leistung zu erzielen. Stellen Sie auf jeden Fall sicher, dass Sie über genügend Spouts verfügen, damit die Bolts ausreichend ausgelastet sind. Die Ausgaberaten der Spouts sollten also dem Durchsatz der Bolts entsprechen. Die tatsächliche Konfiguration hängt vom Spout ab. Dies wird in diesem Artikel nicht beschrieben.
* **Anzahl von Aufgaben:** Jeder Bolt führt einen einzelnen Thread aus. Weitere Aufgaben pro Bolt führen nicht zu einer Erhöhung der Parallelität. Es ergibt sich nur dann ein Vorteil, wenn Ihr Prozess zur Bestätigung des Tupels einen Großteil Ihrer Bolt-Ausführungsdauer einnimmt. Wir empfehlen, viele Tupel in einem größeren Anfügepaket zu gruppieren, bevor Sie eine Bestätigung vom Bolt senden. In den meisten Fällen führen mehrere Aufgaben also nicht zu weiteren Vorteilen.
* **Lokale oder Shuffle-Gruppierung:** Wenn diese Einstellung aktiviert ist, werden Tupel in demselben Workerprozess an Bolts gesendet. Auf diese Weise werden die prozessübergreifende Kommunikation und Netzwerkaufrufe reduziert. Dies wird für die meisten Topologien empfohlen.

Als ersten Ansatz empfehlen wir, mit einem einfachen Szenario zu beginnen und einen Test mit Ihren eigenen Daten durchzuführen, um die oben erwähnten Parameter anzupassen und eine optimale Leistung zu erzielen.

## <a name="tuning-the-spout"></a>Optimieren des Spouts

**Tupeltimeout**

topology.message.timeout.secs: Mit dieser Einstellung wird ermittelt, wie viel Zeit eine Nachricht für den Abschluss des Vorgangs und den Erhalt der Bestätigung benötigen darf, bevor ein Fehler auftritt.

**Maximaler Arbeitsspeicher pro Workerprozess**

Worker.childopts: Mit dieser Einstellung können Sie weitere Befehlszeilenparameter für die Java-Worker angeben. Die am häufigsten verwendete Einstellung hier ist XmX, mit der bestimmt wird, wie viel Arbeitsspeicher dem Heap einer JVM maximal zugeordnet wird.

**Max. ausstehende Spouts**

Topology.max.spout.pending: Diese Konfiguration bestimmt die Anzahl von Tupeln, die sich zu einem beliebigen Zeitpunkt pro Spout-Thread im In-Flight-Zustand (noch nicht auf allen Knoten der Topologie bestätigt) befinden können.

Eine hilfreiche Berechnung ist die Schätzung, wie groß die einzelnen Tupel sind. Ermitteln Sie anschließend, über wie viel Arbeitsspeicher ein Spout-Thread verfügt. Wenn Sie den gesamten Arbeitsspeicher, der einem Thread zugeordnet ist, durch diesen Wert teilen, sollten Sie die Obergrenze für den Parameter zur Bestimmung der maximalen Anzahl von ausstehenden Spouts erhalten.

## <a name="tuning-the-bolt"></a>Optimieren des Bolts
Beim Schreiben in ADLS wird empfohlen, eine Richtlinie für die Größensynchronisierung (Puffer auf Clientseite) auf 4 MiB festzulegen.  Anschließend wird nur dann die Leerung oder ein hsync()-Vorgang durchgeführt, wenn die Puffergröße auf den obigen Wert festgelegt ist.  Der ADLS-Treiber auf der Worker-VM führt diese Pufferung automatisch durch, sofern der Benutzer nicht explizit einen hsync()-Vorgang anstößt.

Der ADLS Storm-Standardbolt verfügt über einen Parameter für die Richtlinie zur Größensynchronisierung (fileBufferSize), den Sie zum Optimieren dieses Parameters verwenden können.

Bei Topologien mit hohem E/A-Aufwand wird empfohlen, dass jeder Bolt-Thread in seine eigene Datei schreibt und eine Dateirotationsrichtlinie (fileRotationSize) festlegt.  Wenn die Datei eine bestimmte Größe erreicht, wird der Datenstrom automatisch geleert, und es wird in eine neue Datei geschrieben.  Die empfohlene Dateigröße für die Rotation ist 1 GB.

**Zeitpunkt der Bestätigung:** In Storm hält ein Spout ein Tupel vor, bis es vom Bolt explizit bestätigt wird.  Wenn ein Tupel vom Bolt gelesen, aber noch nicht bestätigt wurde, bedeutet dies Folgendes: Es ist nicht sichergestellt, dass der Bolt in das Azure Data Lake Store-Back-End übernommen wurde.  Nachdem ein Tupel bestätigt wurde, kann für den Spout vom Bolt die Beibehaltung garantiert werden, damit die Quelldaten von der jeweiligen Quelle, von der gelesen wird, gelöscht werden können.  

**Zur Erzielung der besten Leistung in ADLS:** Es wird empfohlen, dass der Bolt 4 MB an Tupeldaten puffert und diese dann in einem 4-MB-Schreibvorgang an das ADLS-Back-End übermittelt. Nachdem die Daten erfolgreich in den Speicher geschrieben wurden (durch Aufruf von „hflush()“), kann der Bolt die Daten gegenüber dem Spout bestätigen. Dies ist die Vorgehensweise des hier angegebenen Beispiel-Bolts. Es ist auch akzeptabel, eine größere Anzahl von Tupeln vorzuhalten, bevor ein hflush()-Aufruf durchgeführt wird und Tupel bestätigt werden. Hierdurch wird aber die Anzahl von In-Flight-Tupeln erhöht, die vom Spout vorgehalten werden müssen, sodass sich die Menge des erforderlichen Arbeitsspeichers pro JVM erhöht.

Anwendungen verfügen ggf. über die Anforderung, die Bestätigung aus anderen nicht leistungsbezogenen Gründen häufiger durchzuführen (bei Datengrößen von weniger als 4 MB). Da sich dies aber auf den E/A-Durchsatz zum Speicher-Back-End auswirken kann, sollte der Kunde diesen Nachteil sorgfältig gegenüber der E/A-Leistung des Bolts abwägen.

Wenn die Eingangsrate der Tupel nicht hoch genug ist und es daher lange dauert, bis der 4-MB-Puffer gefüllt ist, können Sie entsprechende Maßnahmen treffen:
* Reduzieren Sie die Anzahl von Bolts, damit weniger 4-MB-Puffer gefüllt werden müssen.
* Verwenden Sie eine Richtlinie auf Zeit- oder Anzahlbasis, bei der jeweils nach x Leerungsvorgängen oder y Millisekunden ein hflush()-Vorgang ausgelöst wird und die bisher angesammelten Tupel bestätigt werden.

Beachten Sie, dass der Durchsatz in diesem Fall geringer ist. Bei einer niedrigen Ereignisrate ist der maximale Durchsatz aber sowieso nicht das oberste Ziel.  Mit diesen Maßnahmen können Sie die Gesamtdauer reduzieren, die ein Tupel benötigt, um den Speicher zu durchlaufen. Dies kann von Bedeutung sein, wenn Sie auch bei einer niedrigen Ereignisrate eine Echtzeitpipeline nutzen möchten.  Beachten Sie auch, dass Sie den Parameter „topology.message.timeout_secs“ ebenfalls anpassen sollten, wenn die Eingangsrate der Tupel niedrig ist. So tritt für die Tupel keine Zeitüberschreitung auf, während sie gepuffert oder verarbeitet werden.

## <a name="interpreting-storm-ui"></a>Interpretieren der Storm-UI  
Während der Topologieausführung können Sie die Abläufe in der Storm-UI überwachen. In der UI sind die folgenden Parameter am wichtigsten:

* **Total process execution latency** (Gesamtwartezeit der Prozessausführung): Gibt an, wie lange es im Durchschnitt dauert, bis ein Tupel vom Spout ausgegeben, vom Bolt verarbeitet und dann bestätigt wurde.

* **Total bolt process latency** (Gesamtwartezeit des Boltprozesses): Gibt an, wie lange sich der Tupel im Durchschnitt auf dem Bolt befindet, bis eine Bestätigung eingeht.

* **Total bolt execute latency** (Gesamtwartezeit der Boltausführung): Gibt an, wie lange sich der Bolt im Durchschnitt in der execute-Methode befindet.

* **Number of failures** (Anzahl von Fehlern): Gibt die Anzahl von Tupeln an, die vor dem Eintreten der Zeitüberschreitung nicht vollständig verarbeitet werden konnten.

* **Capacity** (Kapazität): Dies ist eine Kennzahl dafür, wie ausgelastet Ihr System ist. Wenn diese Zahl 1 ist, arbeiten Ihre Bolts so schnell, wie es geht. Erhöhen Sie die Parallelität, wenn der Wert kleiner als 1 ist. Verringern Sie die Parallelität, wenn der Wert größer als&1; ist.

## <a name="common-troubleshooting-scenarios"></a>Häufige Problembehandlungsszenarien
* **Zeitüberschreitungen für hohe Zahl von Tupeln**: Sehen Sie sich die einzelnen Knoten in der Topologie an, um zu ermitteln, wo sich der Engpass befindet. Der häufigste Grund ist, dass die Bolts der Geschwindigkeit der Spouts nicht folgen können. Dies führt dazu, dass Tupel die internen Puffer verstopfen, während sie auf die Verarbeitung warten. Sie können erwägen, den Wert für die Zeitüberschreitung zu erhöhen oder die Anzahl von maximal ausstehenden Spouts zu verringern.

* **Hohe Gesamtwartezeit der Prozessausführung, geringe Boltprozesswartezeit**: In diesem Fall kann es sein, dass die Tupel nicht schnell genug verarbeitet werden. Überprüfen Sie, ob eine ausreichende Zahl von Bestätigungseinheiten („Ackers“) vorhanden ist. Eine andere Möglichkeit besteht darin, dass sie sich zu lange in der Warteschlange befinden, bevor die Bolts mit der Verarbeitung beginnen. Verringern Sie die maximale Anzahl von ausstehenden Spouts.

* **Lange Wartezeit der Boltausführung**: Dies bedeutet, dass die execute()-Methode Ihres Bolts zu lange dauert. Optimieren Sie den Code, oder sehen Sie sich die Schreibgrößen bzw. das Leerungsverhalten an.

## <a name="limitation"></a>Einschränkung
ADLS-Drosselung: Wenn Sie die Bandbreitengrenzwerte von Azure Data Lake Store erreichen, werden Taskfehler angezeigt. Dies lässt sich durch Beobachten der Drosselungsfehler in den Taskprotokollen ermitteln.  Sie können die Parallelität verringern, indem Sie die Containergröße erhöhen.  Wenn Sie für einen Auftrag mehr Parallelität benötigen, können Sie sich an uns wenden.   
Um zu prüfen, ob eine Drosselung vorliegt, müssen Sie die Debugprotokollierung auf Clientseite aktivieren. Gehen Sie hierzu wie folgt vor:

1. Ändern Sie unter „Ambari“ > „Storm“ > „Config“ > „Advanced storm-worker-log4j“ Folgendes:  Ändern Sie &lt;root level="info"&gt; in &lt;root level=”debug”&gt;.  Starten Sie alle Knoten und Dienste neu, damit die Konfiguration wirksam wird.
2. Überwachen Sie die Protokolle der Storm-Topologie auf Workerknoten (unter „/var/log/storm/worker-artifacts/&lt;TopologyName&gt;/&lt;port&gt;/worker.log“) auf ADLS-Drosselungsausnahmen.

## <a name="additional-tuning"></a>Zusätzliche Optimierung
Informationen zur weiteren Leistungsoptimierung für Storm finden Sie in diesem [Blog](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

## <a name="examples-to-run"></a>Beispiele für die Ausführung
Probieren Sie dieses Beispiel auf [GitHub](https://github.com/hdinsight/storm-performance-automation) aus.



<!--HONumber=Feb17_HO2-->


