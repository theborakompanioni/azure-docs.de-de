---
title: "Azure Application Gateway – CRS-Regelgruppen und -Regeln von Web Application Firewall | Microsoft-Dokumentation"
description: "Diese Seite enthält Informationen zu CRS-Regelgruppen und -Regeln von Web Application Firewall."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 9f4445b7ceb5b39c7907b1ca4c7bf0ad52ba8212
ms.lasthandoff: 03/30/2017


---

# <a name="list-of-web-application-firewall-crs-rule-groups-and-rules-offered"></a>Liste mit CRS-Regelgruppen und -Regeln von Web Application Firewall

Mit der Application Gateway Web Application Firewall (WAF) werden Webanwendungen vor allgemeinen Sicherheitsrisiken und Exploits geschützt. Hierfür werden Regeln verwendet, die basierend auf den OWASP-Kernregelsätzen 2.2.9 oder 3.0 definiert sind. Diese Regeln können einzeln deaktiviert werden. In diesem Artikel sind die aktuellen Regeln und Regelsätze enthalten.

In den folgenden Tabellen sind die Regelgruppen und Regeln aufgeführt, die bei Verwendung von Application Gateway mit Web Application Firewall verfügbar sind.  Jede Tabelle enthält die Regeln einer Regelgruppe für eine bestimmte CRS-Version.

##<a name="owasp30"></a> OWASP_3.0

### <a name="crs910"></a>  <p x-ms-format-detection="none">REQUEST-910-IP-REPUTATION</p>

|RuleId|Beschreibung|
|---|---|
|910011|Regel 910011|
|910012|Regel 910012|
|910000|Anforderung von bekanntem schädlichem Client (basierend auf vorherigen Datenverkehrsverletzungen)|
|910100|Client-IP-Adresse stammt von einem Standort eines Lands mit HOHEM Risiko|
|910120|Regel 910120|
|910130|Regel 910130|
|910150|Übereinstimmung mit HTTP-Sperrliste für IP-Adresse der Suchmaschine|
|910160|Übereinstimmung mit HTTP-Sperrliste für Spammer-IP-Adresse|
|910170|Übereinstimmung mit HTTP-Sperrliste für verdächtige IP-Adresse|
|910180|Übereinstimmung mit HTTP-Sperrliste für Harvester-IP-Adresse|
|910013|Regel 910013|
|910014|Regel 910014|
|910015|Regel 910015|
|910016|Regel 910016|
|910017|Regel 910017|
|910018|Regel 910018|

### <a name="crs911"></a> <p x-ms-format-detection="none">REQUEST-911-METHOD-ENFORCEMENT</p>

|RuleId|Beschreibung|
|---|---|
|911011|Regel 911011|
|911012|Regel 911012|
|911100|Methode ist gemäß Richtlinie nicht zulässig|
|911013|Regel 911013|
|911014|Regel 911014|
|911015|Regel 911015|
|911016|Regel 911016|
|911017|Regel 911017|
|911018|Regel 911018|

### <a name="crs912"></a> <p x-ms-format-detection="none">REQUEST-912-DOS-PROTECTION</p>

|RuleId|Beschreibung|
|---|---|
|912100|Regel 912100|
|912012|Regel 912012|
|912120|DoS-Angriff (Denial of Service) von %@{tx.real_ip} identifiziert (%@{tx.dos_block_counter} Treffer seit letzter Warnung)|
|912130|Regel 912130|
|912140|Regel 912140|
|912150|Regel 912150|
|912160|Regel 912160|
|912170|Potenzieller DoS-Angriff (Denial of Service) von %@{tx.real_ip} – Anz. von Anforderungsbursts = %@{ip.dos_burst_counter}|
|912013|Regel 912013|
|912014|Regel 912014|
|912019|Regel 912019|
|912171|Potenzieller DoS-Angriff (Denial of Service) von %@{tx.real_ip} – Anz. von Anforderungsbursts = %@{ip.dos_burst_counter}|
|912015|Regel 912015|
|912016|Regel 912016|
|912017|Regel 912017|
|912018|Regel 912018|

### <a name="crs913"></a> <p x-ms-format-detection="none">REQUEST-913-SCANNER-DETECTION</p>

|RuleId|Beschreibung|
|---|---|
|913011|Regel 913011|
|913012|Regel 913012|
|913100|Benutzer-Agent mit Zuordnung zu Sicherheitsscanner gefunden|
|913110|Anforderungsheader mit Zuordnung zu Sicherheitsscanner gefunden|
|913120|Anforderungsdateiname/-argument mit Zuordnung zu Sicherheitsscanner gefunden|
|913013|Regel 913013|
|913014|Regel 913014|
|913101|Benutzer-Agent mit Zuordnung zu Skripterstellungs- bzw. generischem HTTP-Client gefunden|
|913102|Benutzer-Agent mit Zuordnung zu Webcrawler/Bot gefunden|
|913015|Regel 913015|
|913016|Regel 913016|
|913017|Regel 913017|
|913018|Regel 913018|

### <a name="crs920"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|RuleId|Beschreibung|
|---|---|
|920011|Regel 920011|
|920012|Regel 920012|
|920100|Ungültige HTTP-Anforderungszeile|
|920130|Fehler beim Analysieren des Anforderungstexts|
|920140|Fehler bei strenger Überprüfung des mehrteiligen Anforderungstexts =     PE %@{REQBODY_PROCESSOR_ERROR}     BQ %@{MULTIPART_BOUNDARY_QUOTED}     BW %@{MULTIPART_BOUNDARY_WHITESPACE}     DB %@{MULTIPART_DATA_BEFORE}     DA %@{MULTIPART_DATA_AFTER}     HF %@{MULTIPART_HEADER_FOLDING}     LF %@{MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING}     IQ %@{MULTIPART_INVALID_QUOTING}     IH %@{MULTIPART_INVALID_HEADER_FOLDING}     FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|920160|Content-Length-HTTP-Header ist nicht numerisch.|
|920170|GET- oder HEAD-Anforderung mit Textinhalt|
|920180|POST-Anforderung mit fehlendem Content-Length-Header|
|920190|Bereich = Ungültiger Wert für letztes Byte|
|920210|Mehrere bzw. in Konflikt stehende Verbindungsheaderdaten gefunden|
|920220|Versuchter Missbrauch der URL-Codierung|
|920240|Versuchter Missbrauch der URL-Codierung|
|920250|Versuchter Missbrauch der UTF8-Codierung|
|920260|Versuchter Missbrauch: Unicode (volle/halbe Breite)|
|920270|Ungültiges Zeichen in der Anforderung (Zeichen NULL)|
|920280|Fehlender Hostheader in Anforderung|
|920290|Leerer Hostheader|
|920310|Anforderung hat einen leeren Accept-Header|
|920311|Anforderung hat einen leeren Accept-Header|
|920330|Leerer Benutzer-Agent-Header|
|920340|Anforderung enthält Inhalt, aber keinen Content-Type-Header|
|920350|Hostheader ist eine numerische IP-Adresse|
|920380|Zu viele Argumente in der Anforderung|
|920360|Argumentname ist zu lang|
|920370|Argumentwert ist zu lang|
|920390|Gesamtzahl für Argumente überschritten|
|920400|Hochgeladene Datei zu groß|
|920410|Gesamtgröße der hochgeladenen Dateien zu hoch|
|920420|Anforderungsinhaltstyp ist gemäß Richtlinie nicht zulässig|
|920430|HTTP-Protokollversion ist gemäß Richtlinie nicht zulässig|
|920440|URL-Dateierweiterung wird durch Richtlinie eingeschränkt|
|920450|HTTP-Header ist durch Richtlinie eingeschränkt (%@{MATCHED_VAR})|
|920013|Regel 920013|
|920014|Regel 920014|
|920200|Bereich = Zu viele Felder (mehr als 6)|
|920201|Bereich = Zu viele Felder für PDF-Anforderung (mehr als 35)|
|920230|Mehrere URL-Codierungen erkannt|
|920300|Fehlender Accept-Header für Anforderung|
|920271|Ungültiges Zeichen in der Anforderung (nicht druckbare Zeichen)|
|920320|Benutzer-Agent-Header fehlt|
|920015|Regel 920015|
|920016|Regel 920016|
|920272|Ungültiges Zeichen in der Anforderung (außerhalb des Bereichs von druckbaren Zeichen unterhalb von ASCII 127)|
|920017|Regel 920017|
|920018|Regel 920018|
|920202|Bereich = Zu viele Felder für PDF-Anforderung (mehr als 6)|
|920273|Ungültiges Zeichen in der Anforderung (außerhalb des sehr strengen Satzes)|
|920274|Ungültiges Zeichen in Anforderungsheadern (außerhalb des sehr strengen Satzes)|
|920460|Regel 920460|

### <a name="crs921"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|RuleId|Beschreibung|
|---|---|
|921011|Regel 921011|
|921012|Regel 921012|
|921100|HTTP Request Smuggling-Angriff|
|921110|HTTP Request Smuggling-Angriff|
|921120|HTTP Response Splitting-Angriff|
|921130|HTTP Response Splitting-Angriff|
|921140|HTTP Header Injection-Angriff über Header|
|921150|HTTP Header Injection-Angriff über Nutzlast (CR/LF erkannt)|
|921160|HTTP Header Injection-Angriff über Nutzlast (CR/LF und Headername erkannt)|
|921013|Regel 921013|
|921014|Regel 921014|
|921151|HTTP Header Injection-Angriff über Nutzlast (CR/LF erkannt)|
|921015|Regel 921015|
|921016|Regel 921016|
|921170|Regel 921170|
|921180|HTTP-Parameterverunreinigung (%@{TX.1})|
|921017|Regel 921017|
|921018|Regel 921018|

### <a name="crs930"></a> <p x-ms-format-detection="none">REQUEST-930-APPLICATION-ATTACK-LFI</p>

|RuleId|Beschreibung|
|---|---|
|930011|Regel 930011|
|930012|Regel 930012|
|930100|Path Traversal-Angriff (/../)|
|930110|Path Traversal-Angriff (/../)|
|930120|Zugriffsversuch auf Betriebssystemdatei|
|930130|Zugriffsversuch auf Datei mit eingeschränktem Zugriff|
|930013|Regel 930013|
|930014|Regel 930014|
|930015|Regel 930015|
|930016|Regel 930016|
|930017|Regel 930017|
|930018|Regel 930018|

### <a name="crs931"></a> <p x-ms-format-detection="none">REQUEST-931-APPLICATION-ATTACK-RFI</p>

|RuleId|Beschreibung|
|---|---|
|931011|Regel 931011|
|931012|Regel 931012|
|931100|Möglicher RFI-Angriff (Remote File Inclusion) = Verwendung von IP-Adresse für URL-Parameter|
|931110|Möglicher RFI-Angriff (Remote File Inclusion) = Verwendung eines häufigen und für RFI anfälligen Parameternamens mit URL-Nutzlast|
|931120|Möglicher RFI-Angriff (Remote File Inclusion) = Verwendung von URL-Nutzlast mit nachgestelltem Fragezeichen (?)|
|931013|Regel 931013|
|931014|Regel 931014|
|931130|Möglicher RFI-Angriff (Remote File Inclusion) = Domänenexterner Verweis/Link|
|931015|Regel 931015|
|931016|Regel 931016|
|931017|Regel 931017|
|931018|Regel 931018|

### <a name="crs932"></a> <p x-ms-format-detection="none">REQUEST-932-APPLICATION-ATTACK-RCE</p>

|RuleId|Beschreibung|
|---|---|
|932011|Regel 932011|
|932012|Regel 932012|
|932120|Ausführung eines Remotebefehls = Windows PowerShell-Befehl gefunden|
|932130|Ausführung eines Remotebefehls = Unix Shell-Ausdruck gefunden|
|932140|Ausführung eines Remotebefehls = Windows-FOR/IF-Befehl gefunden|
|932160|Ausführung eines Remotebefehls = Unix Shell-Code gefunden|
|932170|Ausführung eines Remotebefehls = Shellshock (CVE-2014-6271)|
|932171|Ausführung eines Remotebefehls = Shellshock (CVE-2014-6271)|
|932013|Regel 932013|
|932014|Regel 932014|
|932015|Regel 932015|
|932016|Regel 932016|
|932017|Regel 932017|
|932018|Regel 932018|

### <a name="crs933"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|RuleId|Beschreibung|
|---|---|
|933011|Regel 933011|
|933012|Regel 933012|
|933100|PHP Injection-Angriff = Öffnendes/Schließendes Tag gefunden|
|933110|PHP Injection-Angriff = Upload von PHP-Skriptdatei gefunden|
|933120|PHP Injection-Angriff = Konfigurationsdirektive gefunden|
|933130|PHP Injection-Angriff = Variablen gefunden|
|933150|PHP Injection-Angriff = PHP-Funktionsname mit hohem Risikofaktor gefunden|
|933160|PHP Injection-Angriff = PHP-Funktionsaufruf mit hohem Risikofaktor gefunden|
|933180|PHP Injection-Angriff = Aufruf einer Variablenfunktion gefunden|
|933013|Regel 933013|
|933014|Regel 933014|
|933151|PHP Injection-Angriff = PHP-Funktionsname mit mittlerem Risikofaktor gefunden|
|933015|Regel 933015|
|933016|Regel 933016|
|933131|PHP Injection-Angriff = Variablen gefunden|
|933161|PHP Injection-Angriff = PHP-Funktionsaufruf mit niedrigem Wert gefunden|
|933111|PHP Injection-Angriff = Upload von PHP-Skriptdatei gefunden|
|933017|Regel 933017|
|933018|Regel 933018|

### <a name="crs941"></a> <p x-ms-format-detection="none">REQUEST-941-APPLICATION-ATTACK-XSS</p>

|RuleId|Beschreibung|
|---|---|
|941011|Regel 941011|
|941012|Regel 941012|
|941100|XSS-Angriff per libinjection erkannt|
|941110|XSS-Filter – Kategorie 1 = Skripttagvektor|
|941130|XSS-Filter – Kategorie 3 = Attributvektor|
|941140|XSS-Filter – Kategorie 4 = JavaScript-URI-Vektor|
|941150|XSS-Filter – Kategorie 5 = Unzulässige HTML-Attribute|
|941180|Schlüsselwörter von Sperrliste für Knotenvalidierung|
|941190|IE-XSS-Filter – Angriff erkannt|
|941200|IE-XSS-Filter – Angriff erkannt|
|941210|IE-XSS-Filter – Angriff erkannt|
|941220|IE-XSS-Filter – Angriff erkannt|
|941230|IE-XSS-Filter – Angriff erkannt|
|941240|IE-XSS-Filter – Angriff erkannt|
|941260|IE-XSS-Filter – Angriff erkannt|
|941270|IE-XSS-Filter – Angriff erkannt|
|941280|IE-XSS-Filter – Angriff erkannt|
|941290|IE-XSS-Filter – Angriff erkannt|
|941300|IE-XSS-Filter – Angriff erkannt|
|941310|Falsch formatierte US-ASCII-Codierung für XSS-Filter – Angriff erkannt|
|941350|UTF-7-Codierung – IE XSS – Angriff erkannt|
|941013|Regel 941013|
|941014|Regel 941014|
|941320|Möglicher XSS-Angriff erkannt – HTML-Taghandler|
|941015|Regel 941015|
|941016|Regel 941016|
|941017|Regel 941017|
|941018|Regel 941018|

### <a name="crs942"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|RuleId|Beschreibung|
|---|---|
|942011|Regel 942011|
|942012|Regel 942012|
|942100|SQL Injection-Angriff per libinjection erkannt|
|942140|SQL Injection-Angriff = Häufige Datenbanknamen erkannt|
|942160|Erkennt Blind SQLI-Tests per sleep() oder benchmark().|
|942170|Erkennt SQL Benchmark- und Sleep Injection-Angriffsversuche, einschließlich bedingter Abfragen.|
|942230|Erkennt Angriffsversuche mit bedingter SQL-Einschleusung.|
|942270|Suche nach grundlegender SQL-Einschleusung. Häufig verwendete Angriffszeichenfolge für MySQL Oracle und andere.|
|942290|Ermittelt Angriffsversuche mit grundlegender MongoDB SQL-Einschleusung.|
|942320|Erkennt Einschleusungen von gespeicherten Prozeduren/Funktionen für MySQL und PostgreSQL.|
|942350|Erkennt MySQL-UDF-Einschleusung und andere Versuche der Manipulation von Daten bzw. der Struktur.|
|942013|Regel 942013|
|942014|Regel 942014|
|942150|Angriff mit Einschleusung von SQL-Befehlen|
|942410|Angriff mit Einschleusung von SQL-Befehlen|
|942440|SQL-Kommentarsequenz erkannt|
|942450|Hexadezimale SQL-Codierung identifiziert|
|942015|Regel 942015|
|942016|Regel 942016|
|942251|Erkennt HAVING-Einschleusungen.|
|942460|Warnung: Erkennung einer Metazeichenanomalie – Wiederholte Non-Word-Zeichen|
|942017|Regel 942017|
|942018|Regel 942018|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Beschreibung|
|---|---|
|943011|Regel 943011|
|943012|Regel 943012|
|943100|Möglicher Session Fixation-Angriff = Festlegung von Cookiewerten in HTML|
|943110|Möglicher Session Fixation-Angriff = SessionID-Parametername mit domänenexternem Verweiser|
|943120|Möglicher Session Fixation-Angriff = SessionID-Parametername ohne Verweiser|
|943013|Regel 943013|
|943014|Regel 943014|
|943015|Regel 943015|
|943016|Regel 943016|
|943017|Regel 943017|
|943018|Regel 943018|

##<a name="owasp229"></a> OWASP_2.2.9

### <a name="crs20"></a> crs_20_protocol_violations

|RuleId|Beschreibung|
|---|---|
|960911|Ungültige HTTP-Anforderungszeile|
|981227|Apache-Fehler = Ungültiger URI in Anforderung|
|960912|Fehler beim Analysieren des Anforderungstexts|
|960914|Fehler bei strenger Überprüfung des mehrteiligen Anforderungstexts =     PE %@{REQBODY_PROCESSOR_ERROR}     BQ %@{MULTIPART_BOUNDARY_QUOTED}     BW %@{MULTIPART_BOUNDARY_WHITESPACE}     DB %@{MULTIPART_DATA_BEFORE}     DA %@{MULTIPART_DATA_AFTER}     HF %@{MULTIPART_HEADER_FOLDING}     LF %@{MULTIPART_LF_LINE}     SM %@{MULTIPART_SEMICOLON_MISSING}     IQ %@{MULTIPART_INVALID_QUOTING}     IH %@{MULTIPART_INVALID_HEADER_FOLDING}     FLE %@{MULTIPART_FILE_LIMIT_EXCEEDED}|
|960915|Mehrteiliger Parser hat eine mögliche fehlende Übereinstimmung für eine Grenze erkannt.|
|960016|Content-Length-HTTP-Header ist nicht numerisch.|
|960011|GET- oder HEAD-Anforderung mit Textinhalt|
|960012|POST-Anforderung mit fehlendem Content-Length-Header|
|960902|Ungültige Verwendung der Identitätscodierung|
|960022|Erwartungsheader für HTTP 1.0 unzulässig|
|960020|Für Pragma-Header ist für HTTP/1.1-Anforderungen ein Cache-Control-Header erforderlich.|
|958291|Bereich = Feld ist vorhanden und beginnt mit 0.|
|958230|Bereich = Ungültiger Wert für letztes Byte|
|958295|Mehrere bzw. in Konflikt stehende Verbindungsheaderdaten gefunden|
|950107|Versuchter Missbrauch der URL-Codierung|
|950109|Mehrere URL-Codierungen erkannt|
|950108|Versuchter Missbrauch der URL-Codierung|
|950801|Versuchter Missbrauch der UTF8-Codierung|
|950116|Versuchter Missbrauch: Unicode (volle/halbe Breite)|
|960901|Ungültiges Zeichen in Anforderung|
|960018|Ungültiges Zeichen in Anforderung|

### <a name="crs21"></a> crs_21_protocol_anomalies

|RuleId|Beschreibung|
|---|---|
|960008|Fehlender Hostheader in Anforderung|
|960007|Leerer Hostheader|
|960015|Fehlender Accept-Header für Anforderung|
|960021|Anforderung hat einen leeren Accept-Header|
|960009|Fehlender Benutzer-Agent-Header für Anforderung|
|960006|Leerer Benutzer-Agent-Header|
|960904|Anforderung enthält Inhalt, aber keinen Content-Type-Header|
|960017|Hostheader ist eine numerische IP-Adresse|

### <a name="crs23"></a> crs_23_request_limits

|RuleId|Beschreibung|
|---|---|
|960209|Argumentname ist zu lang|
|960208|Argumentwert ist zu lang|
|960335|Zu viele Argumente in der Anforderung|
|960341|Gesamtzahl für Argumente überschritten|
|960342|Hochgeladene Datei zu groß|
|960343|Gesamtgröße der hochgeladenen Dateien zu hoch|

### <a name="crs30"></a> crs_30_http_policy

|RuleId|Beschreibung|
|---|---|
|960032|Methode ist gemäß Richtlinie nicht zulässig|
|960010|Anforderungsinhaltstyp ist gemäß Richtlinie nicht zulässig|
|960034|HTTP-Protokollversion ist gemäß Richtlinie nicht zulässig|
|960035|URL-Dateierweiterung wird durch Richtlinie eingeschränkt|
|960038|HTTP-Header ist durch Richtlinie eingeschränkt|

### <a name="crs35"></a> crs_35_bad_robots

|RuleId|Beschreibung|
|---|---|
|990002|Anforderung gibt an, dass die Website mit einem Sicherheitsscanner gescannt wurde|
|990901|Anforderung gibt an, dass die Website mit einem Sicherheitsscanner gescannt wurde|
|990902|Anforderung gibt an, dass die Website mit einem Sicherheitsscanner gescannt wurde|
|990012|Nicht autorisierter Websitecrawler|

### <a name="crs40"></a> crs_40_generic_attacks

|RuleId|Beschreibung|
|---|---|
|960024|Warnung: Erkennung einer Metazeichenanomalie – Wiederholte Non-Word-Zeichen|
|950008|Einschleusung von nicht dokumentierten ColdFusion-Tags|
|950010|Angriff mit LDAP-Einschleusung|
|950011|Angriff mit SSI-Einschleusung|
|950018|Universelle PDF-XSS-URL erkannt|
|950019|Angriff mit E-Mail-Einschleusung|
|950012|HTTP Request Smuggling-Angriff|
|950910|HTTP Response Splitting-Angriff|
|950911|HTTP Response Splitting-Angriff|
|950117|Remote File Inclusion-Angriff|
|950118|Remote File Inclusion-Angriff|
|950119|Remote File Inclusion-Angriff|
|950120|Möglicher RFI-Angriff (Remote File Inclusion) = Domänenexterner Verweis/Link|
|981133|Regel 981133|
|981134|Regel 981134|
|950009|Session Fixation-Angriff|
|950003|Session Fixation|
|950000|Session Fixation|
|950005|Zugriffsversuch auf Remotedatei|
|950002|Zugriff auf Systembefehl|
|950006|Einschleusung von Systembefehl|
|959151|Angriff mit PHP-Einschleusung|
|958976|Angriff mit PHP-Einschleusung|
|958977|Angriff mit PHP-Einschleusung|

### <a name="crs41sql"></a> crs_41_sql_injection_attacks

|RuleId|Beschreibung|
|---|---|
|981231|SQL-Kommentarsequenz erkannt|
|981260|Hexadezimale SQL-Codierung identifiziert|
|981320|SQL Injection-Angriff = Häufige Datenbanknamen erkannt|
|981300|Regel 981300|
|981301|Regel 981301|
|981302|Regel 981302|
|981303|Regel 981303|
|981304|Regel 981304|
|981305|Regel 981305|
|981306|Regel 981306|
|981307|Regel 981307|
|981308|Regel 981308|
|981309|Regel 981309|
|981310|Regel 981310|
|981311|Regel 981311|
|981312|Regel 981312|
|981313|Regel 981313|
|981314|Regel 981314|
|981315|Regel 981315|
|981316|Regel 981316|
|981317|Warnung: Anomalieerkennung für SQL-SELECT-Anweisung|
|950007|Angriff mit blinder Einschleusung von SQL-Befehlen|
|950001|Angriff mit Einschleusung von SQL-Befehlen|
|950908|Angriff mit Einschleusung von SQL-Befehlen|
|959073|Angriff mit Einschleusung von SQL-Befehlen|
|981272|Erkennt Blind SQLI-Tests per sleep() oder benchmark().|
|981250|Erkennt SQL Benchmark- und Sleep Injection-Angriffsversuche, einschließlich bedingter Abfragen.|
|981241|Erkennt Angriffsversuche mit bedingter SQL-Einschleusung.|
|981276|Suche nach grundlegender SQL-Einschleusung. Häufig verwendete Angriffszeichenfolge für MySQL Oracle und andere.|
|981270|Ermittelt Angriffsversuche mit grundlegender MongoDB SQL-Einschleusung.|
|981253|Erkennt Einschleusungen von gespeicherten Prozeduren/Funktionen für MySQL und PostgreSQL.|
|981251|Erkennt MySQL-UDF-Einschleusung und andere Versuche der Manipulation von Daten bzw. der Struktur.|

### <a name="crs41xss"></a> crs_41_xss_attacks

|RuleId|Beschreibung|
|---|---|
|973336|XSS-Filter – Kategorie 1 = Skripttagvektor|
|973338|XSS-Filter – Kategorie 3 = JavaScript-URI-Vektor|
|981136|Regel 981136|
|981018|Regel 981018|
|958016|XSS-Angriff (Cross-Site Scripting)|
|958414|XSS-Angriff (Cross-Site Scripting)|
|958032|XSS-Angriff (Cross-Site Scripting)|
|958026|XSS-Angriff (Cross-Site Scripting)|
|958027|XSS-Angriff (Cross-Site Scripting)|
|958054|XSS-Angriff (Cross-Site Scripting)|
|958418|XSS-Angriff (Cross-Site Scripting)|
|958034|XSS-Angriff (Cross-Site Scripting)|
|958019|XSS-Angriff (Cross-Site Scripting)|
|958013|XSS-Angriff (Cross-Site Scripting)|
|958408|XSS-Angriff (Cross-Site Scripting)|
|958012|XSS-Angriff (Cross-Site Scripting)|
|958423|XSS-Angriff (Cross-Site Scripting)|
|958002|XSS-Angriff (Cross-Site Scripting)|
|958017|XSS-Angriff (Cross-Site Scripting)|
|958007|XSS-Angriff (Cross-Site Scripting)|
|958047|XSS-Angriff (Cross-Site Scripting)|
|958410|XSS-Angriff (Cross-Site Scripting)|
|958415|XSS-Angriff (Cross-Site Scripting)|
|958022|XSS-Angriff (Cross-Site Scripting)|
|958405|XSS-Angriff (Cross-Site Scripting)|
|958419|XSS-Angriff (Cross-Site Scripting)|
|958028|XSS-Angriff (Cross-Site Scripting)|
|958057|XSS-Angriff (Cross-Site Scripting)|
|958031|XSS-Angriff (Cross-Site Scripting)|
|958006|XSS-Angriff (Cross-Site Scripting)|
|958033|XSS-Angriff (Cross-Site Scripting)|
|958038|XSS-Angriff (Cross-Site Scripting)|
|958409|XSS-Angriff (Cross-Site Scripting)|
|958001|XSS-Angriff (Cross-Site Scripting)|
|958005|XSS-Angriff (Cross-Site Scripting)|
|958404|XSS-Angriff (Cross-Site Scripting)|
|958023|XSS-Angriff (Cross-Site Scripting)|
|958010|XSS-Angriff (Cross-Site Scripting)|
|958411|XSS-Angriff (Cross-Site Scripting)|
|958422|XSS-Angriff (Cross-Site Scripting)|
|958036|XSS-Angriff (Cross-Site Scripting)|
|958000|XSS-Angriff (Cross-Site Scripting)|
|958018|XSS-Angriff (Cross-Site Scripting)|
|958406|XSS-Angriff (Cross-Site Scripting)|
|958040|XSS-Angriff (Cross-Site Scripting)|
|958052|XSS-Angriff (Cross-Site Scripting)|
|958037|XSS-Angriff (Cross-Site Scripting)|
|958049|XSS-Angriff (Cross-Site Scripting)|
|958030|XSS-Angriff (Cross-Site Scripting)|
|958041|XSS-Angriff (Cross-Site Scripting)|
|958416|XSS-Angriff (Cross-Site Scripting)|
|958024|XSS-Angriff (Cross-Site Scripting)|
|958059|XSS-Angriff (Cross-Site Scripting)|
|958417|XSS-Angriff (Cross-Site Scripting)|
|958020|XSS-Angriff (Cross-Site Scripting)|
|958045|XSS-Angriff (Cross-Site Scripting)|
|958004|XSS-Angriff (Cross-Site Scripting)|
|958421|XSS-Angriff (Cross-Site Scripting)|
|958009|XSS-Angriff (Cross-Site Scripting)|
|958025|XSS-Angriff (Cross-Site Scripting)|
|958413|XSS-Angriff (Cross-Site Scripting)|
|958051|XSS-Angriff (Cross-Site Scripting)|
|958420|XSS-Angriff (Cross-Site Scripting)|
|958407|XSS-Angriff (Cross-Site Scripting)|
|958056|XSS-Angriff (Cross-Site Scripting)|
|958011|XSS-Angriff (Cross-Site Scripting)|
|958412|XSS-Angriff (Cross-Site Scripting)|
|958008|XSS-Angriff (Cross-Site Scripting)|
|958046|XSS-Angriff (Cross-Site Scripting)|
|958039|XSS-Angriff (Cross-Site Scripting)|
|958003|XSS-Angriff (Cross-Site Scripting)|
|973300|Möglicher XSS-Angriff erkannt – HTML-Taghandler|
|973301|XSS-Angriff erkannt|
|973302|XSS-Angriff erkannt|
|973303|XSS-Angriff erkannt|
|973304|XSS-Angriff erkannt|
|973305|XSS-Angriff erkannt|
|973306|XSS-Angriff erkannt|
|973307|XSS-Angriff erkannt|
|973308|XSS-Angriff erkannt|
|973309|XSS-Angriff erkannt|
|973311|XSS-Angriff erkannt|
|973313|XSS-Angriff erkannt|
|973314|XSS-Angriff erkannt|
|973331|IE-XSS-Filter – Angriff erkannt|
|973315|IE-XSS-Filter – Angriff erkannt|
|973330|IE-XSS-Filter – Angriff erkannt|
|973327|IE-XSS-Filter – Angriff erkannt|
|973326|IE-XSS-Filter – Angriff erkannt|
|973346|IE-XSS-Filter – Angriff erkannt|
|973345|IE-XSS-Filter – Angriff erkannt|
|973324|IE-XSS-Filter – Angriff erkannt|
|973323|IE-XSS-Filter – Angriff erkannt|
|973348|IE-XSS-Filter – Angriff erkannt|
|973321|IE-XSS-Filter – Angriff erkannt|
|973320|IE-XSS-Filter – Angriff erkannt|
|973318|IE-XSS-Filter – Angriff erkannt|
|973317|IE-XSS-Filter – Angriff erkannt|
|973329|IE-XSS-Filter – Angriff erkannt|
|973328|IE-XSS-Filter – Angriff erkannt|

### <a name="crs42"></a> crs_42_tight_security

|RuleId|Beschreibung|
|---|---|
|950103|Path Traversal-Angriff|

### <a name="crs45"></a> crs_45_trojans

|RuleId|Beschreibung|
|---|---|
|950110|Backdoor-Zugriff|
|950921|Backdoor-Zugriff|
|950922|Backdoor-Zugriff|

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Customize web application firewall rules through the portal](application-gateway-customize-waf-rules-portal.md) (Anpassen von Web Application Firewall-Regeln über das Portal) darüber, wie Sie WAF-Regeln deaktivieren können.

[1]: ./media/application-gateway-integration-security-center/figure1.png
