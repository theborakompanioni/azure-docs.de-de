<properties
	pageTitle="Azure Insights: Empfohlene Methoden der automatischen Skalierung in Azure Insights. | Microsoft Azure"
	description="Lernen Sie die Prinzipien der effektiven Verwendung der automatischen Skalierung in Azure Insights."
	authors="kamathashwin"
	manager=""
	editor=""
	services="monitoring"
	documentationCenter="monitoring"/>

<tags
	ms.service="monitoring"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/15/2016"
	ms.author="ashwink"/>

# Empfohlene Methoden für die automatische Skalierung in Azure Insights

Die folgenden Abschnitte helfen Ihnen, die besten Methoden für die automatische Skalierung in Azure Insights zu verstehen. Nach dem Lesen dieser Informationen sind Sie besser in der Lage, die automatische Skalierung effektiv in Ihrer Azure-Infrastruktur zu verwenden.

## Konzepte der automatischen Skalierung

- Eine Ressource kann nur *eine* Einstellung für die automatische Skalierung haben.
- Eine Einstellung für die automatische Skalierung kann ein oder mehrere Profile beinhalten, und jedes Profil kann eine oder mehrere Regeln für die automatische Skalierung beinhalten.
- Eine Einstellung für die automatische Skalierung skaliert Instanzen horizontal *hoch*, indem die Anzahl der Instanzen erhöht und *herunter*, indem die Anzahl der Instanzen verringert wird. Eine Einstellung für die automatische Skalierung hat einen Höchst-, Mindest- und Standardwert von Instanzen.
- Eine automatische Skalierungsaufgabe liest die zugeordnete Skalierungsmetrik und überprüft dabei, ob die konfigurierten Schwellenwerte zum horizontalen Hoch- oder Herunterskalieren überschritten wurden. Eine Liste der Metriken, nach der die automatische Skalierung vorgenommen werden kann, können Sie unter [Azure Insights autoscaling common metrics](insights-autoscale-common-metrics.md) (Übliche Metriken für die automatische Skalierung in Azure Insights) anzeigen lassen.
- Alle Schwellenwerte werden auf Instanzebene berechnet. Beispielsweise besagt „Skaliere um eine Instanz horizontal hoch, wenn die durchschnittliche CPU-Auslastung > 80 % ist, wenn die Instanzenanzahl 2 ist“, dass horizontal hochskaliert werden soll, wenn die durchschnittliche CPU-Auslastung über alle Instanzen hinweg größer als 80 % ist.
- Fehlerbenachrichtigungen erhalten Sie immer über E-Mail. Insbesondere erhalten der Besitzer, Mitwirkende und Leser der Zielressource eine E-Mail. Sie erhalten auch immer eine *Wiederherstellungs*-E-Mail, wenn die automatische Skalierung nach einem Ausfall wiederhergestellt ist und wieder normal funktioniert.
- Sie können es optional so einrichten, dass Sie eine Benachrichtigung über erfolgreiche Skalierungsaktionen über E-Mail und Webhooks erhalten.

## Empfohlene Methoden für die automatische Skalierung

Verwenden Sie die folgenden empfohlenen Methoden für die automatische Skalierung.

### Stellen Sie sicher, dass sich die Höchst- und Mindestwerte unterscheiden und eine angemessene Spanne zwischen ihnen liegt.
Falls Ihre Einstellung ein Maximum von „2“ und ein Minimum von „2“ hat und dabei die Anzahl der aktuellen Instanzen „2“ ist, kann keine automatische Skalierung durchgeführt werden. Eine empfohlene Einstellung ist die Beibehaltung einer passenden Spanne zwischen der maximalen und der minimalen Instanzenanzahl. Die automatische Skalierung skaliert immer zwischen diesen inklusiven Grenzwerten. Nehmen Sie jedoch einmal an, dass Sie sich entschließen, die Anzahl der Instanzen manuell auf einen Wert über den Höchstwert zu skalieren (aktualisieren). Beim nächsten Ausführen einer automatischen Skalierungsaufgabe wird überprüft, ob die aktuelle Instanzenanzahl größer als der Höchstwert ist – falls dies der Fall ist, skaliert sie horizontal auf den Höchstwert herunter, unabhängig vom Schwellenwert, der in den Regeln festgelegt wurde. Falls die Anzahl Ihrer aktuellen Instanzen niedriger als die Minimalanzahl ist, wird beim nächsten Ausführen einer automatischen Skalierungsaufgabe auf die Mindestanzahl an Instanzen horizontal hochskaliert.

### Verwenden Sie immer eine Regelkombination für das horizontale Hoch- und Herunterskalieren, die eine Erhöhung und Verringerung durchführt

Falls Sie nur einen Teil dieser Kombination verwenden (= „nur horizontal Hochskalieren“ oder „nur horizontal Herunterskalieren“), wird die automatische Skalierung die Hoch- oder Herunterskalierung vornehmen, bis das Maximum oder das Minimum erreicht ist.

### Wechseln Sie nicht zwischen dem Azure-Portal und dem klassischen Azure-Portal, wenn Sie die automatische Skalierung verwalten
Verwenden Sie für Cloud Services und App Services (Web-Apps) das Azure-Portal (portal.azure.com), um Einstellungen für die automatische Skalierung zu erstellen und zu verwalten. Verwenden Sie PowerShell, die Befehlszeilenschnittstelle (CLI) oder die REST-API, um die Einstellung für die automatische Skalierung in Skalierungsgruppen in virtuellen Computern zu erstellen und zu verwalten. Wechseln Sie nicht zwischen dem klassischen Azure-Portal (manage.windowsazure.com) und dem Azure-Portal (portal.azure.com), wenn Sie die Konfigurationen für die automatische Skalierung verwalten. Für das klassische Azure-Portal und das zugrunde liegende Back-End gibt es Beschränkungen. Wechseln Sie zum Azure-Portal, um die automatische Skalierung mittels grafischer Benutzeroberfläche zu verwalten. Ihre Optionen sind die Verwendung der PowerShell, der CLI oder der REST-API (über den Azure-Ressourcen-Explorer) in der automatischen Skalierung.

### Wählen Sie die passende Statistik für Ihre Diagnosemetrik aus
Für die Diagnosemetriken können Sie zwischen *Durchschnitt*, *Minimum*, *Maximum* und *Gesamt* als Metrik, nach der skaliert werden soll, auswählen. Die am häufigsten verwendete Statistik ist *Durchschnitt*.

### Wählen Sie die Schwellenwerte für alle Metriktypen sorgfältig
Wir empfehlen die sorgfältige Auswahl verschiedener Schwellenwerte für das horizontale Hoch- und Herunterskalieren, je nach Praxissituation.

Wir empfehlen Ihnen, die Einstellungen für die automatische Skalierung *nicht* wie in den unten gezeigten Beispielen vorzunehmen. In diesen sind die Schwellenwerte für die Bedingungen des horizontalen Hoch- und Herunterskalierens gleich oder sehr ähnlich.

- Erhöhe die Anzahl der Instanzen um 1, wenn Threadanzahl <= 600
- Verringere die Anzahl der Instanzen um 1, wenn Threadanzahl >= 600


Sehen wir uns nun ein Beispiel an, das zu möglicherweise verwirrendem Verhalten führen kann. Nehmen Sie an, es gibt anfangs 2 Instanzen, und die durchschnittliche Anzahl der Threads pro Instanz wächst auf 625. Die automatische Skalierung skaliert hoch und fügt eine dritte Instanz hinzu. Nehmen Sie jetzt an, dass sich die durchschnittliche Threadanzahl über alle Instanzen hinweg auf 575 verringert. Bevor herunterskaliert wird, versucht die automatische Skalierung, den Endzustand nach ausgeführtem Herunterskalieren abzuschätzen. Falls beispielsweise 575 x 3 (aktuelle Instanzenanzahl) = 1.725 / 2 (Anzahl der Instanzen, wenn herunterskaliert wird) = 862,5 Threads. Das bedeutet, dass die automatische Skalierung nach dem horizontalen Herunterskalieren sofort wieder horizontal hochskalieren muss, falls die durchschnittliche Threadanzahl gleich bleibt oder sich gar nur leicht verringert. Falls jedoch wieder horizontal hochskaliert wird, würde sich der ganze Vorgang wiederholen, was zu einer Endlosschleife führen würde. Um diese *flatterhafte* Situation zu vermeiden, wird gar nicht erst zentral herunterskaliert. Stattdessen wird diese Bedingung übersprungen und beim nächsten Ausführen dieses Dienstauftrags neu bewertet. Das könnte viele Leute verwirren, da die automatische Skalierung scheinbar bei einer durchschnittlichen Threadanzahl von 575 nicht funktioniert.

Dieses Schätzverhalten während des horizontalen Herunterskalierens soll flatterhafte Situationen verhindern. Sie sollten dieses Verhalten im Hinterkopf behalten, wenn Sie für das horizontale Hoch- und Herunterskalieren die gleichen Schwellenwerte wählen.

Wir empfehlen eine angemessene Spanne zwischen den Schwellenwerten für das horizontale Hoch- und Herunterskalieren. Ziehen Sie stattdessen beispielsweise diese bessere Regelkombination in Betracht:

- Erhöhe die Anzahl der Instanzen um 1, wenn prozentuale CPU-Auslastung >= 80
- Verringere die Anzahl der Instanzen um 1, wenn prozentuale CPU-Auslastung <= 60

Sehen wir uns nun an, wie dieses Beispiel funktioniert. Angenommen, es gibt zu Anfang 2 Instanzen. Falls die durchschnittliche prozentuale CPU-Auslastung über alle Instanzen 80 erreicht, wird automatisch hochskaliert und eine dritte Instanz hinzugefügt. Nehmen Sie jetzt an, dass im Laufe der Zeit die prozentuale CPU-Auslastung auf 60 fällt. Die Autoscale-Regel zum horizontalen Herunterskalieren schätzt jetzt den Endzustand, falls horizontal herunterskaliert werden sollte. Falls beispielsweise 60 x 3 (aktuelle Instanzenanzahl) = 180 / 2 (Anzahl der Instanzen wenn herunterskaliert wird) = 90 Die automatische Skalierung skaliert also nicht horizontal herunter, da sie sofort wieder horizontal hochskalieren müsste. Stattdessen wird das horizontale Herunterskalieren übersprungen. Nehmen Sie jetzt an, dass bei der nächsten Überprüfung die CPU-Auslastung auf 50 fällt. Jetzt schätzt sie wieder – 50 x 3 Instanzen = 150 / 2 Instanzen = 75, was unter dem Schwellenwert von 80 für das horizontale Hochskalieren liegt, daher wird erfolgreich horizontal auf 2 Instanzen herunterskaliert.

### Überlegungen zu Skalierungsschwellenwerten für spezielle Metriken
 Für spezielle Metriken, wie die Metrik für die Länge der Speicher- oder Service Bus-Warteschlange, ist der Schwellenwert die durchschnittliche Anzahl der Nachrichten, die pro aktueller Anzahl von Instanzen verfügbar ist. Wählen Sie den Schwellenwert für diese Metrik sorgfältig aus.

Veranschaulichen wir diesen Punkt mit einem Beispiel, um sicherzustellen, dass Sie dieses Verhalten besser verstehen.

- Erhöhe die Anzahl der Instanzen um 1, wenn Anzahl der Nachrichten in Speicherwarteschlange >= 50
- Verringere die Anzahl der Instanzen um 1, wenn Anzahl der Nachrichten in Speicherwarteschlange <= 10

Angenommen, es gibt zu Anfang 2 Instanzen. Nehmen Sie dann an, dass weiterhin Nachrichten eintreffen, und dass die Anzahl beim Ansehen der Speicherwarteschlange bei 50 liegt. Sie könnten annehmen, dass die automatische Skalierung eine horizontale Hochskalierungsaktion starten sollte. Beachten Sie jedoch, dass es sich immer noch um 50 / 2 = 25 Nachrichten pro Instanz handelt. Daher wird keine horizontale Hochskalierung ausgeführt. Damit die erste horizontale Hochskalierung ausgeführt wird, sollte die Gesamtanzahl der Nachrichten in der Speicherwarteschlange 100 sein. Nehmen Sie jetzt an, dass die Gesamtanzahl der Nachrichten 100 erreicht. Eine dritte Instanz wird aufgrund einer horizontalen Hochskalierungsaktion hinzugefügt. Die nächste horizontale Hochskalierungsaktion wird erst durchgeführt, wenn die Gesamtzahl der Nachrichten in der Warteschlange 150 erreicht. Sehen wir uns nun die horizontale Herunterskalierungsaktion an. Nehmen Sie an, dass die Anzahl der Instanzen 3 ist. Die erste horizontale Herunterskalierungsaktion wird durchgeführt, wenn die Gesamtzahl der Nachrichten in der Warteschlange 30 beträgt, da 30 / 3 = 10 Nachrichten pro Instanz ergeben, den Schwellenwert zum horizontalen Herunterskalieren.

### Überlegungen zur Skalierung, wenn mehrere Profile in einer Einstellung für die automatische Skalierung konfiguriert sind

In einer Einstellung für die automatische Skalierung können Sie ein Standardprofil wählen, das immer angewendet wird, unabhängig von Zeitplänen oder der Zeit. Alternativ können Sie aber auch ein periodisches Profil oder ein Profil für eine festgelegte Periode mit Datums- und Zeitbereich wählen.

Wenn automatische Skalierungsdienste sie verarbeiten, werden sie immer in der folgenden Reihenfolge überprüft:

1. Profil für ein festgelegtes Datum
2. Periodisches Profil
3. Standardprofil („immer“)

Sobald die Bedingung eines Profils erfüllt wird, überprüft die automatische Skalierung nicht mehr zusätzlich die nächste Profilbedingung auf der nächstuntersten Ebene. Die automatische Skalierung verarbeitet immer nur ein Profil gleichzeitig. Das bedeutet, dass Sie, wenn Sie eine Verarbeitungsbedingung von einer niedrigeren Ebene beachtet haben wollen, diese Regeln auch im aktuellen Profil festlegen müssen.

Sehen wir uns dies mithilfe eines Beispiels an:

Die Abbildung unten zeigt eine Einstellung für die automatische Skalierung mit einem Standardprofil mit einem Instanzenminimum von 2 und einem Instanzenmaximum von 10. In diesem Beispiel wurden die Regeln so konfiguriert, dass horizontal hochskaliert wird, wenn die Anzahl der Meldungen in der Warteschlange größer als 10 ist, und horizontal herunterskaliert wird, wenn die Anzahl der Meldungen in der Warteschlange kleiner als 3 ist. Die Ressource kann jetzt also zwischen 2 und 10 Instanzen hoch- bzw. -herunterskalieren.

Zusätzlich wurde ein periodisches Profil für Montag eingerichtet. Es wurde für ein Instanzenminimum von 2 und ein Instanzenmaximum von 12 festgelegt. Das heißt, dass am Montag bei der erstmaligen Überprüfung dieser Bedingung durch die automatische Skalierung, diese auf das neue Minimum von 3 horizontal hochskaliert, falls die Instanzenanzahl bereits 2 ist. Solange die automatische Skalierung bei der Überprüfung feststellt, dass diese Profilbedingung erfüllt ist (Montag), wird sie nur die CPU-basierten Regeln für das horizontale Skalieren (hoch/herunter) ausführen, die für dieses Profil konfiguriert wurden. Zu diesem Zeitpunkt wird die Länge der Warteschlange nicht überprüft. Falls Sie jedoch wollen, dass die Bedingung der Länge der Warteschlange ebenfalls überprüft wird, sollten Sie diese Regeln aus dem Standardprofil auch in Ihr Montagsprofil übernehmen.

Wenn die automatische Skalierung zurück zum Standardprofil wechselt, wird auch zuerst überprüft, ob die Höchst- und Mindestbedingungen erfüllt werden. Falls die Anzahl der Instanzen zu diesem Zeitpunkt 12 ist, wird auf 10, den für das Standardprofil zulässigen Höchstwert, herunterskaliert.

![Einstellungen für die automatische Skalierung](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### Skalierungsüberlegungen, wenn in einem Profil mehrere Regeln konfiguriert sind
Es gibt Fälle, in denen Sie möglicherweise mehrere Regeln innerhalb eines Profils festlegen müssen. Die folgenden Regeln für die automatische Skalierung werden von Diensten verwendet, wenn mehrere Regeln festgelegt sind.

Beim *horizontalen Hochskalieren* wird die automatische Skalierung durchgeführt, sobald eine Regel erfüllt wird. Beim *horizontalen Herunterskalieren* wird die automatische Skalierung nur ausgeführt, wenn alle Regeln erfüllt werden.
 
Nehmen Sie an, Sie hätten die folgenden vier Regeln für die automatische Skalierung:
 
- Falls CPU-Auslastung < 30 %, skaliere horizontal um 1 herunter
- ​Falls Arbeitsspeicherauslastung < 50 %, skaliere horizontal um 1 herunter
- ​Falls CPU-Auslastung > 75 %, skaliere horizontal um 1 hoch
- ​Falls Arbeitsspeicherauslastung > 75 %, skaliere horizontal um 1 hoch

Daraufhin erfolgt Folgendes:
- Falls die CPU-Auslastung 76 % und die Arbeitsspeicherauslastung 50 % beträgt, skalieren wir horizontal hoch.
- Falls die CPU-Auslastung 50 % und die Arbeitsspeicherauslastung 76 % beträgt, skalieren wir horizontal hoch.
 
Andererseits wird, wenn die CPU-Auslastung 25 % und die Arbeitsspeicherauslastung 51 % beträgt, **nicht** automatisch horizontal herunterskaliert. Um horizontal herunterzuskalieren, muss die CPU-Auslastung 29 % und die Arbeitsspeicherauslastung 49 % betragen.

### Wählen Sie als Standard immer eine sichere Anzahl an Instanzen
Die Standardinstanzenanzahl ist wichtig, da die automatische Skalierung Ihren Dienst auf diese Instanzenanzahl skaliert, wenn keine Metriken zur Verfügung stehen. Wählen Sie daher eine Standardanzahl an Instanzen, die für Ihre Workloads sicher ist.

### Konfigurieren der Benachrichtigungen für das automatische Skalieren
Die automatische Skalierung benachrichtigt die Administratoren und die Mitwirkenden der Ressource per E-Mail, wenn eine der folgenden Situationen auftritt:
- Der Autoskalierungsdienst führt keine Aktion aus.
- Für den Autoskalierungsdienst stehen keine Metriken zur Verfügung, auf deren Grundlage eine Skalierungsentscheidung getroffen werden kann.
- Metriken stehen wieder zur Verfügung (Wiederherstellung), um eine Skalierungsentscheidung zu treffen. Zusätzlich zu den oben aufgeführten Bedingungen, können Sie E-Mail- oder Webhook-Benachrichtigungen konfigurieren, um bei erfolgreichen Skalierungsaktionen informiert zu werden.

<!---HONumber=AcomDC_0803_2016-->