<properties
 pageTitle="Bewährte Methoden für die IoT-Sicherheit | Microsoft Azure"
 description="Bewährte Methoden für die Sicherheit Ihrer IoT-Infrastruktur"
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="YuriDio"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/05/2016"
 ms.author="yurid"/>

# Internet der Dinge (IoT) – Bewährte Methoden für die Sicherheit

Zum Schützen einer IoT-Infrastruktur ist eine umfassende Sicherheitsstrategie erforderlich. Vom Sichern von Daten in der Cloud über den Schutz der Datenintegrität beim Übertragen über das öffentliche Internet bis zur sicheren Bereitstellung von Geräten – jede Ebene erhöht die Sicherheit in der allgemeinen Infrastruktur

## Schützen einer IoT-Infrastruktur
 
Diese umfassende Sicherheitsstrategie kann mit aktiver Beteiligung verschiedener mit der Herstellung, Entwicklung und Bereitstellung von IoT-Geräten und -Infrastruktur beteiligten Personen entwickelt und durchgesetzt werden. Diese Personen werden nachfolgend ausführlich beschrieben.

- **IoT-Hardwarehersteller/-integrator**: Dies sind in der Regel die Hersteller der bereitgestellten IoT-Hardware , die Hardwareintegratoren, die Hardware von verschiedenen Herstellern zusammenstellen, oder die Hardwarelieferanten, die Hardware für eine IoT-Bereitstellung liefern, die von anderen Lieferanten hergestellt oder integriert wird.
- **IoT-Lösungsentwickler**: Die Entwicklung einer IoT-Lösung erfolgt in der Regel durch einen Lösungsentwickler, der einem internen Team angehört, oder einen auf diese Aktivität spezialisierten Systemintegrator (SI). Der IoT-Lösungsentwickler kann verschiedene Komponenten der IoT-Lösung von Grund auf neu entwickeln, verschiedene Standard- oder Open Source-Komponenten integrieren oder vorkonfigurierte Lösungen verwenden und geringfügige Anpassungen vornehmen.
- **IoT-Lösungsbereitsteller**: Sobald eine IoT-Lösung entwickelt wird, muss sie vor Ort bereitgestellt werden. Dies umfasst die Bereitstellung von Hardware, die Kopplung von Geräten und die Bereitstellung von Lösungen in Hardwaregeräten oder in der Cloud.
- **IoT-Lösungsoperator**: Sobald die IoT-Lösung bereitgestellt wurde, sind langfristige Vorgänge, Überwachung, Aktualisierung und Wartung erforderlich. Dies kann durch ein internes Team aus IT-Experten, Hardwarebetriebs- und Wartungsteams sowie Domänenspezialisten erfolgen, die das richtige Verhalten der gesamten IoT-Infrastruktur überwachen. 

In den folgenden Abschnitten finden Sie für alle Beteiligten bewährte Methoden zum Entwickeln, Bereitstellen und Betreiben einer sicheren IoT-Infrastruktur.

## IoT-Hardwarehersteller/-integrator

Befolgen Sie diese bewährten Methoden, wenn Sie IoT-Hardwarehersteller oder Hardwareintegrator sind:

- **Mindestanforderungen für Hardware einhalten**: Die Hardware sollte lediglich die für den Betrieb der Hardware erforderlichen Features enthalten. Beispielsweise sollten USB-Ports nur enthalten sein, wenn sie für den Betrieb des Geräts erforderlich sind. Diese zusätzlichen Features machen das Gerät anfällig für unerwünschte Angriffe, die tunlichst vermieden werden sollten. 
- **Hardware manipulationssicher gestalten**: Integrieren Sie einen Mechanismus zum Erkennen von physischer Manipulation an der Hardware, z. B. Öffnen der Geräteabdeckung, Entfernen eines Teils des Geräts usw. Diese Manipulationssignale können Teil des in die Cloud hochgeladenen Datenstroms sein, der die Operatoren über diese Ereignisse benachrichtigt. 
- **Sichere Hardware entwickeln**: Integrieren Sie, falls gemäß COGS zulässig, Sicherheitsfeatures wie sichere und verschlüsselte Speicher und eine auf Trusted Platform Module (TPM) basierende Startfunktion. Diese Features verbessern die Sicherheit der Geräte und schützen somit die gesamte IoT-Infrastruktur.
- **Upgrades schützen**: Das Aktualisieren der Firmware während der Lebensdauer des Geräts ist unvermeidlich. Durch die Entwicklung von Geräten mit sicheren Pfaden für Upgrades und kryptografische Sicherung der Firmwareversion wird das Gerät während und nach Upgrades geschützt.

## IoT-Lösungsentwickler

Befolgen Sie diese bewährten Methoden, wenn Sie IoT-Lösungsentwickler sind:

- **Sichere Softwareentwicklungsmethodik befolgen**: Zur Entwicklung sicherer Software muss von Grund auf die Sicherheit bedacht werden – vom Beginn des Projekts über Implementierung und Tests bis zur Bereitstellung. Diese Methodik wirkt sich auf die Auswahl der Plattformen, Sprachen und Tools aus. Der Microsoft Security Development Lifecycle bietet einen schrittweisen Ansatz für die Entwicklung sicherer Software.
- **Open Source-Software mit Bedacht wählen**: Open Source-Software ermöglicht eine schnelle Lösungsentwicklung. Bei der Auswahl von Open-Source-Software sollten Sie die Aktivitätsstufe der Community für jede Open Source-Komponente berücksichtigen. Durch eine aktive Community wird sichergestellt, dass die Software unterstützt wird und Probleme ermittelt sowie behoben werden. Eine undurchsichtige und inaktive Open Source-Software wird nicht unterstützt, und Probleme werden wahrscheinlich nicht gefunden.
- **Mit Bedacht integrieren**: Viele Software-Sicherheitsschwachstellen liegen zwischen Bibliotheken und APIs. Funktionen, die für die aktuelle Bereitstellung möglicherweise nicht erforderlich sind, stehen möglicherweise immer noch über eine API-Ebene zur Verfügung. Durch Sicherstellen, dass alle Schnittstellen der Komponenten integriert sind, wird die allgemeine Sicherheit gewährleistet.      

## IoT-Lösungsbereitsteller

Befolgen Sie diese bewährten Methoden, wenn Sie IoT-Lösungsbereitsteller sind:

- **Hardware sicher bereitstellen**: Für IoT-Bereitstellungen muss Hardware möglicherweise an unsicheren Orten wie öffentlichen Bereichen oder unbeaufsichtigten Gebieten bereitgestellt werden. Stellen Sie in solchen Situationen sicher, dass die Hardwarebereitstellung bestmöglich vor Manipulation geschützt ist Verfügt die Hardware über USB-Ports oder andere Anschlüsse, müssen diese sicher verdeckt werden. Diese werden häufig als Einstiegspunkt für Angriffe genutzt.
- **Authentifizierungsschlüssel schützen**: Jedes Gerät benötigt während der Bereitstellung Geräte-IDs und die zugehörigen, vom Clouddienst generierten, Authentifizierungsschlüssel. Bewahren Sie diese Schlüssel auch nach der Bereitstellung sicher auf. Jeder kompromittierte Schlüssel kann von einem böswilligen Gerät verwendet werden, um sich als ein vorhandenes Gerät auszugeben.

## IoT-Lösungsoperator

Befolgen Sie diese bewährten Methoden, wenn Sie IoT-Lösungsoperator sind:

- **System aktuell halten**: Stellen Sie sicher, dass Gerätebetriebssysteme und alle Gerätetreiber auf die neueste Version aktualisiert werden. Windows 10 (IoT oder andere SKUs) wird bei Aktivierung der automatischen Updates von Microsoft auf dem aktuellen Stand gehalten und ist somit ein sicheres Betriebssystem für IoT-Geräte. Wenn Sie andere Betriebssysteme wie Linux auf dem aktuellen Stand halten, schützen Sie sie auch vor schädlichen Angriffen. 
- **Vor schädlichen Aktivitäten schützen**: Wenn es das Betriebssystem zulässt, installieren Sie die neueste Antivirus- und Antimalwaresoftware auf jedem Gerätebetriebssystem. Dadurch werden die meisten externen Gefahren verringert. Die meisten modernen Betriebssysteme wie Windows 10 IoT und Linux können durch geeignete Maßnahmen vor dieser Bedrohung geschützt werden. 
- **Häufig überwachen**: Die Überwachung der IoT-Infrastruktur auf sicherheitsbezogene Probleme ist entscheidend für die Reaktion auf Sicherheitsvorfälle. Die meisten Betriebssysteme wie Windows 10 (IoT und andere SKUs) bieten eine integrierte Ereignisprotokollierung, die häufig auf Sicherheitsverletzungen geprüft werden sollte. Die Überwachungsinformationen können als separater Telemetriedatenstrom an den Clouddienst gesendet und analysiert werden.
- **IoT-Infrastruktur physisch schützen**: Die schlimmsten Angriffe gegen die IoT-Infrastruktur beginnen mit dem physischen Zugriff auf Geräte. Der Schutz vor böswilligem Zugriff auf USB-Ports und anderweitigem physischen Zugriff ist eine wichtige Sicherheits- und Schutzmaßnahme. Die Protokollierung des physischen Zugriffs wie die Nutzung des USB-Anschlusses ist der Schlüssel zum Aufdecken von möglichen Verstößen. Auch hier ermöglicht Windows 10 (IoT und andere SKUs) eine ausführliche Protokollierung dieser Ereignisse.
- **Schützen der Cloud-Anmeldeinformationen**: Cloud-Authentifizierungsanmeldeinformationen für die Konfiguration und den Betrieb einer IoT-Bereitstellung sind wahrscheinlich die einfachste Möglichkeit zum Zugreifen und Manipulieren eines IoT-Systems. Schützen Sie die Anmeldeinformationen, indem Sie das Kennwort häufig ändern und diese Anmeldeinformationen nicht auf öffentlichen Computern verwenden. 

Die Funktionen der einzelnen IoT-Geräte variieren. Manche Geräte sind möglicherweise ausgewachsene Computer mit gängigen Betriebssystemen, während auf anderen Geräten möglicherweise sehr einfache Betriebssysteme ausgeführt werden. Die oben aufgeführten bewährten Methoden für die Sicherheit können in unterschiedlichem Ausmaß auf diese Geräte angewendet werden. Falls vorhanden, sollten zusätzliche bewährte Methoden vom Hersteller dieser Geräte für die Sicherheit und Bereitstellung befolgt werden.

Manche ältere und eingeschränkte Geräte sind möglicherweise nicht speziell auf die IoT-Bereitstellung ausgelegt. Diese Geräte können möglicherweise keine Daten verschlüsseln, keine Verbindung mit dem Internet herstellen, keine erweiterte Überwachung durchführen usw. In diesen Fällen kann ein modernes und sicheres Bereichsgateway zum Aggregieren von Daten auf älteren Geräten für die erforderliche Sicherheit dieser über das Internet verbundenen Geräte verwendet werden. In diesem Fall ermöglichen Bereichsgateways eine sichere Authentifizierung, Aushandlung von verschlüsselten Sitzungen, Empfang von Befehlen aus der Cloud und viele andere Sicherheitsfunktionen.

<!---HONumber=AcomDC_0406_2016-->