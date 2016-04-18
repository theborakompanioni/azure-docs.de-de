<properties 
    pageTitle="Azure RemoteApp – Testen Ihrer Netzwerkbandbreiten-Nutzung mit einigen gängigen Szenarien | Microsoft Azure"
	description="Lernen Sie allgemeine Verwendungsszenarien kennen, um den Bedarf an Netzwerkbandbreite für Azure RemoteApp herauszufinden."
	services="remoteapp"
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/31/2016" 
    ms.author="elizapo" />
    
# Azure RemoteApp – Testen Ihrer Netzwerkbandbreiten-Nutzung mit einigen gängigen Szenarien

Wie in [Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp](remoteapp-bandwidth.md) erläutert, können Sie die Auswirkung von Azure RemoteApp auf Ihr Netzwerk am besten durch einige Nutzungstests herausfinden. Führen Sie diese Tests für einen festgelegten Zeitraum aus, und messen Sie die erforderliche Bandbreite für jedes Szenario. Wenn Sie die Möglichkeit haben, können Sie auch Netzwerkpaketverlust und Netzwerkjitter messen, um die Netzwerkmuster zu verstehen, die speziell in Ihrer Umgebung erstellt werden.

    
Denken Sie beim Bewerten der Bandbreitennutzung daran, dass die Nutzung in Ihrem Unternehmen von Benutzer zu Benutzer variiert. Leser und Schreiber von Texten verwenden z. B. in der Regel weniger Bandbreite als Benutzer, die mit Video arbeiten. Um optimale Ergebnisse zu erzielen, untersuchen Sie Ihre eigenen Benutzeranforderungen, und erstellen Sie eine Kombination der folgenden Szenarien, die die Benutzer in Ihrem Unternehmen am besten darstellt. Denken Sie daran, [die Faktoren zu prüfen, die Bandbreitennutzung und Benutzerfreundlichkeit beeinflussen](remoteapp-bandwidthexperience.md) – dies hilft Ihnen, die idealen Tests zu identifizieren.

Informieren Sie sich zunächst über die Tests, und wählen Sie die Mischung, um sie dann auszuführen. Sie können die Leistung mithilfe der folgenden Tabelle verfolgen.

>[AZURE.NOTE] Wenn Sie keine eigenen Netzwerktests ausführen können, oder keine Zeit dafür haben, ziehen Sie unsere [Schätzungen/Empfehlungen zur Standard-Netzwerkbandbreite](remoteapp-bandwidthguidelines.md) zu Rate. Ihr Bedarf kann jedoch variieren, wenn Sie also Ihre eigenen Tests ausführen *können*, sollten Sie es tun.


## Die Nutzungstests
Diese Tests haben unterschiedliche Laufzeiten und testen verschiedene Funktionen/Features, die Netzwerkbandbreite beanspruchen. Denken Sie daran, die Mischung von Tests auszuwählen, die am besten den individuellen Benutzern in Ihrem Unternehmen entspricht.
 
### PowerPoint Executive/komplex – 900-1.000 Sekunden ausführen

Ein Benutzer präsentiert 45-50 High-Fidelity-Folien mit Microsoft Office PowerPoint im Vollbildmodus. Die Folien sollten Bilder, Übergänge (mit Animationen) und Hintergründe mit Farbverlauf enthalten, die für Ihr Unternehmen typisch sind. Der Benutzer sollte jede Folie mindestens 20 Sekunden anzeigen.
    
Dieses Szenario erzeugt beim Übergang zwischen zwei Folien in der Präsentation intensiven Datenverkehr.
    
### PowerPoint einfach – ~ 620 Sekunden ausführen

Ein Benutzer präsentiert eine einfache PowerPoint-Datei mit ungefähr 30 Folien mit Microsoft Office PowerPoint im Vollbildmodus. Die Folien sind textintensiver als im PowerPoint-Szenario „Executive/komplex“ und haben einfachere Hintergründe und Bilder (schwarze Diagramme).
    
### Internet Explorer – ~ 250 Sekunden ausführen

Ein Benutzer browst mit Internet Explorer im Web. Der Benutzer browst und scrollt durch eine Mischung aus Text, natürlichen Bildern und einigen Schemadiagrammen. Die Webseiten werden auf dem lokalen Laufwerk des Remote Desktop Session Host-Servers (Remotedesktop-Sitzungshost, RD Session Host) als MHT-Datei gespeichert. Der Benutzer scrollt mit den Tasten BILD-AUF, BILD-AB, NACH-OBEN-TASTE, NACH-UNTEN-TASTE mit unterschiedlichen Intervallen für jede Taste/jeden Scrolltyp:
    
    - Down - 250 keystrokes very 500 ms
    - Page Up - 36 keystrokes every 1000 ms
    - Down - 75 keystrokes every 100 ms
    - Page Down - 20 keystrokes every 500 ms
    - Up - 120 keystrokes every 300 ms
    
### PDF-Dokument – einfach – ~ 610 Sekunden ausführen
Ein Benutzer liest und durchsucht ein PDF-Dokument auf verschiedene Weise mithilfe von Adobe Acrobat Reader. Das Dokument sollte aus Tabellen, einfachen Diagramme und mehreren Schriftarten bestehen. Das Dokument ist 35-40 Seiten lang. Der Benutzer scrollt mit zwei unterschiedlichen Geschwindigkeiten, rückwärts und vorwärts, mit vier unterschiedlichen Zoomwerten (an Seite angepasst, an Breite angepasst, 100 % und eine weitere Ihrer Wahl). Das Zoomen stellt sicher, dass der Text (die Schriftart) in unterschiedlichen Größen gerendert wird. Das Scrollen erfolgt mit den Tasten BILD-AUF, BILD-AB, NACH-OBEN-TASTE, NACH-UNTEN-TASTE mit unterschiedlichen Intervallen für jeden Scrolltyp.

### PDF-Dokument – gemischt – ~ 320 Sekunden ausführen
Ein Benutzer liest und durchsucht ein PDF-Dokument auf verschiedene Weise mithilfe von Adobe Acrobat Reader. Das Dokument besteht aus qualitativ hochwertigen Bildern (einschließlich Fotos), Tabellen, einfachen Diagrammen und mehreren Schriftarten. Der Benutzer scrollt mit zwei unterschiedlichen Geschwindigkeiten, rückwärts und vorwärts, mit vier unterschiedlichen Zoomwerten (an Seite angepasst, an Breite angepasst, 100 % und eine weitere Ihrer Wahl). Das Zoomen stellt sicher, dass der Text (die Schriftart) in unterschiedlichen Größen gerendert wird. Das Scrollen erfolgt mit den Tasten BILD-AUF, BILD-AB, NACH-OBEN-TASTE, NACH-UNTEN-TASTE mit unterschiedlichen Intervallen für jeden Scrolltyp.

### Flash-Videowiedergabe – ~ 180 Sekunden ausführen
Ein Benutzer betrachtet ein Adobe Flash-codiertes Video, das in eine Webseite eingebettet ist. Die Webseite wird auf der lokalen Festplatte des Remotedesktop-Sitzungshost-Servers gespeichert. Das Video wird im Internet Explorer durch ein eingebettetes Player-Plug-In wiedergegeben.

Dieses Szenario emuliert Benutzer, die Multimediawebseiten mit anspruchsvollen Inhalten anzeigen. Die meisten Daten sollten über VOBR laufen.

### Word-Remoteeingabe – ~ 1.800 Sekunden ausführen
Ein Benutzer gibt ein Dokument über eine RDP-Sitzung ein. Tastatureingaben werden vom Client über die RDP-Sitzung an ein Dokument in Microsoft Word gesendet, das in der Remotesitzung ausgeführt wird. Die Eingabegeschwindigkeit ist ein Zeichen alle 250 ms (insgesamt 7.050 Zeichen).

Dies ist eines der häufigsten Szenarien für Wissensarbeiter. Dieses Szenario testet die Reaktionsfähigkeit eines Benutzers bei der Eingabe in ein modernes Textverarbeitungssystem. Dieses Szenario reagiert auch auf kleine Änderungen in der Bandbreitennutzung.

## Nachverfolgen der Testergebnisse

Mit der folgenden Tabelle können Sie die Szenarien in Ihrer Umgebung evaluieren. Die unten stehenden Daten dienen nur zur Veranschaulichung – möglicherweise unterscheiden sie sich erheblich von Ihren Beobachtungen.

Der Einfachheit halber gehen wir davon aus, dass alle Szenarien mit der gleichen Auflösung von 1920 x 1080 Pixeln und TCP-Transport in einem Netzwerk mit Latenz (Verzögerung) unter 200 ms und Netzwerkjitter im Bereich 120 ms+ von etwa 1 % getestet werden.

Informationen zur Tabelle:
- **Durchschnittliche Erfahrung** enthält die Netzwerkbandbreite, bei der die Benutzerproduktivität nicht wesentlich beeinträchtigt ist, jedoch gelegentliche Video- oder Audiostörungen auftreten. Das System kann mit Nutzung der dynamischen Logik schnell wiederhergestellt werden. Die Schätzungen der Netzwerkbandbreite versuchen, die Qualität der Benutzerfreundlichkeit zu gewährleisten.
 - **Spürbare Probleme (Haltepunkt)** enthält die Netzwerkbandbreite, bei der Benutzer möglicherweise deutliche Probleme in ihrer Erfahrung feststellen und ihre Produktivität für messbare Zeiträume betroffen ist. An diesem Punkt kommen die RDP-Algorithmen an ihre Grenzen und können die Qualität der Benutzerfreundlichkeit nicht garantieren, weil die Netzwerkbandbreite unzureichend ist.
 - **Empfohlen** enthält die empfohlene Netzwerkbandbreite für gute oder hervorragende Benutzerfreundlichkeit. Sie ist in der Regel einen Schritt höher als der Wert in der entsprechenden Spalte **Durchschnittliche Erfahrung**.
 - **Notizen** enthält Beobachtungen und Kommentare.
 
| Test | Durchschnittliche Erfahrung | Spürbare Probleme (Haltepunkt) | Empfohlene Netzwerkbandbreite | Hinweise |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Executive/komplex PPT | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s bevorzugt | Bei 1 MB/s gehen viele Animationen verloren |
| PPT einfach | 5 MB/s | 256 KB/s | 10 MB/s | Bei 256 KB/s werden Folien mit spürbarer Verzögerung geladen |
| Internet Explorer | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s bevorzugt | Bei 1 MB/s werden Webvideos verwischt und abgehackt angezeigt, bei schnellem Scrollen treten Probleme auf |
| PDF einfach | 1 MB/s | 256 KB/s | 5 MB/s | Bei 256 KB/s dauert es eine Weile, bis die Seite geladen ist |
| PDF gemischt | 1 MB/s | 256 KB/s | 5 MB/s | Bei 256 KB/s dauert das Laden der Seite sehr lange |
| Flash-Videowiedergabe | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s bevorzugt | Bei 1 MB/s wird das Video in niedriger Auflösung angezeigt und einige Frames werden ausgelassen |
| Word-Remoteeingabe | 256 KB/s | 128 KB/s | 1 MB/s | Bei 256 KB/s kann dem Benutzer die Verzögerung zwischen Tastatureingaben auffallen |

Um die Netzwerkbandbreite pro Benutzer zu evaluieren, erstellen Sie eine Mischung aus den oben genannten Szenarien und dem entsprechenden Anteil erforderlicher Netzwerkbandbreite. Wählen Sie den höchsten Wert, der für Ihre Szenarien erforderlich ist. Da Benutzer das System fast nie allein verwenden, ziehen Sie eine Reserve für Benutzer in Betracht, die gleichzeitig im Netzwerk arbeiten.
     
## Weitere Informationen
- [Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp – wie wirken die Netzwerkbandbreite und die Qualität der Benutzerfreundlichkeit zusammen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien (wenn Sie keine eigenen erstellen können)](remoteapp-bandwidthguidelines.md)

<!---HONumber=AcomDC_0406_2016-->