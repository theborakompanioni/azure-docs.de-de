<properties
	pageTitle="Verbinden von Linux-Computern mit Log Analytics | Microsoft Azure"
	description="Mit Log Analytics können Sie von Linux-Computern generierte Daten sammeln und auf sie reagieren."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/19/2016"
	ms.author="banders"/>

# Verbinden von Linux-Computern mit Log Analytics

Mit Log Analytics können Sie von Linux-Computern generierte Daten sammeln und auf sie reagieren. Mit Daten, die von Linux gesammelt werden und zu OMS hinzugefügt werden, können Sie Linux-Systeme und Containerlösungen wie Docker verwalten, unabhängig vom Standort Ihres Computers, also praktisch von überall. Daher können sich diese Datenquellen in Form von physischen Servern virtuellen Computern in einem in der Cloud gehosteten Dienst wie Amazon Web Services (AWS) oder Microsoft Azure oder in Form des Notebooks auf Ihrem Schreibtisch in Ihrem lokalen Datencenter befinden. Darüber hinaus erfasst OMS Daten von Windows-Computern auf ähnliche Weise. Eine vollständig hybride IT-Umgebung wird also unterstützt.

Sie können Daten von all diesen Quellen mit Log Analytics in OMS über ein einziges Verwaltungsportal anzeigen und verwalten. Dadurch müssen Daten nicht mithilfe verschiedener Systeme überwacht werden und können einfacher verwendet werden. Sie können außerdem beliebige Daten in jede beliebige Business Analytics-Lösung exportieren oder auch in jedes System, das Sie bereits besitzen.

Dieser Artikel ist ein Schnellstarthandbuch, das Ihnen dabei hilft, Daten für Ihre Linux-Computer mithilfe des OMS-Agents für Linux zu sammeln und zu verwalten. Weitere technische Informationen finden Sie unter [OMS Agent for Linux overview](https://github.com/Microsoft/OMS-Agent-for-Linux) (Überblick über den OMS-Agent für Linux) und [OMS Agent for Linux full documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) (Vollständige Dokumentation für den OMS-Agent für Linux) auf Github.


Derzeit können Sie die folgenden Arten von Daten von Linux-Computern sammeln:

- Leistungsmetriken
- Syslog-Ereignisse
- Warnungen von Nagios und Zabbix
- Leistungsmetriken von Docker-Containern, Inventar und Protokollen

## Unterstützte Linux-Versionen

Die Versionen x86 und x64 werden jeweils offiziell auf einer Vielzahl von Linux-Verteilungen unterstützt. Der OMS-Agent für Linux kann jedoch auch auf anderen Verteilungen ausgeführt werden, die hier nicht aufgeführt sind.

- Amazon Linux 2012.09 bis 2015.09
- CentOS Linux 5, 6 und 7
- Oracle Linux 5, 6 und 7
- Red Hat Enterprise Linux-Server 5,6 und 7
- Debian GNU/Linux 6, 7 und 8
- Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
- SUSE Linux Enterprise Server 11 und 12

## OMS-Agent für Linux
Der Operations Management Suite-Agent für Linux umfasst mehrere Pakete. Die Release-Datei enthält die folgenden Pakete, die durch Ausführen des Shell-Pakets mit `--extract` verfügbar sind.

**Paket** | **Version** | **Beschreibung**
----------- | ----------- | --------------
omsagent | 1\.1.0 | Der Operations Management Suite-Agent für Linux
omsconfig | 1\.1.1 | Konfigurations-Agent für den OMS-Agent
omi | 1\.0.8.3 | Open Management Infrastructure (OMI) – ein kompakter CIM-Server
scx | 1\.6.2 | OMI-CIM-Anbieter für Leistungsmetriken für das Betriebssystem
apache-cimprov | 1\.0.0 | Apache HTTP Server-Anbieter für die Leistungsüberwachung für OMI Nur installiert, wenn Apache HTTP Server erkannt wird.
mysql-cimprov | 1\.0.0 | MySQL Server-Anbieter für die Leistungsüberwachung für OMI Nur installiert, wenn MySQL/MariaDB-Server erkannt wird.
docker-cimprov | 0\.1.0 | Docker-Anbieter für OMI. Nur installiert, wenn Docker erkannt wird.

### Zusätzliche Installationsartefakte
Nach der Installation des OMS-Agents für Linux-Pakete, werden die folgenden zusätzlichen systemweiten Konfigurationsänderungen angewendet. Diese Artefakte werden entfernt, wenn das „omsagent“-Paket deinstalliert wird.
- Ein Benutzer ohne Berechtigungen mit dem Namen `omsagent` wird erstellt. Dies ist das Konto, unter dem der omsagent-Daemon ausgeführt wird.
- Im Verzeichnis „/etc/sudoers.d/omsagent“ wird eine „include“-Datei für „sudoer“ erstellt. Diese berechtigt omsagent zum Neustart der syslog- und omsagent-Daemons. Falls die Sudo „include“-Direktiven nicht in der installierten Version von Sudo unterstützt werden, werden diese Einträge in /etc/sudoers geschrieben.
- Die syslog-Konfiguration wird geändert, um eine Teilmenge von Ereignissen an den Agent weiterzuleiten. Weitere Informationen finden Sie unten im Abschnitt **Konfigurieren der Datensammlung**

### Details zur Linux-Datensammlung

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten gesammelt werden.

| Quelle | Direct Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Zabbix|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)| ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|1 Minute|
|Nagios|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)| ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Bei der Ankunft|
|syslog|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)| ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Aus dem Azure-Speicher: 10 Minuten. vom Agent: bei der Ankunft|
|Leistungsindikatoren von Linux|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)| ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Gemäß dem Zeitplan, mindestens 10 Sekunden|
|Änderungsnachverfolgung|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)| ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Stündlich|



### Paketanforderungen
| **Erforderliches Paket** | **Beschreibung** | **Mindestversion**|
|--------------------- | --------------------- | -------------------|
|Glibc |	GNU C-Bibliothek | 2\.5-12|
|Openssl | OpenSSL-Bibliotheken | 0\.9.8e oder 1.0|
Curl | cURL-Webclient | 7\.15.5
|Python-ctypes |Funktionsbibliotheken | –|
|PAM | Module für austauschbare Authentifizierung |– |

>[AZURE.NOTE] Zum Sammeln von syslog-Nachrichten sind entweder rsyslog oder syslog-ng erforderlich. Der Standard-syslog-Daemon in Version 5 von Red Hat Enterprise Linux, CentOS und Oracle Linux-Version (sysklog) wird für die syslog-Ereigniserfassung nicht unterstützt. Der rsyslog-Daemon sollte installiert und so konfiguriert werden, dass er sysklog ersetzt, um syslog-Daten von dieser Version dieser Verteilung zu sammeln.

## Schnellinstallation

Führen Sie die folgenden Befehle aus, um den omsagent herunterzuladen, die Prüfsumme zu validieren und den Agent anschließend zu installieren und zu integrieren. Die Befehle sind für 64-Bit. Die Arbeitsbereichs-ID und der Primärschlüssel befinden sich unter dem OMS-Portal unter **Einstellungen** auf der Registerkarte **Verbundene Datenquellen**.

![Informationen zum Arbeitsbereich](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.1.0-28/omsagent-1.1.0-28.universal.x64.sh
sha256sum ./omsagent-1.1.0-28.universal.x64.sh
sudo sh ./omsagent-1.1.0-28.universal.x64.sh --upgrade -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Es gibt zahlreiche weitere Methoden zum Installieren und Upgraden des Agents. Weitere Informationen dazu finden Sie unter [Steps to install the OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) (Schritte zum Installieren des OMS-Agent für Linux).


## Auswählen der Methode für die Linux-Datensammlung

Wie Sie die Datentypen auswählen, die Sie erfassen möchten, hängt davon ab, ob Sie das OMS-Portal nutzen möchten, oder ob Sie verschiedene Konfigurationsdateien direkt auf Ihren Linux-Clients bearbeiten möchten. Wenn Sie das Portal verwenden, wird die Konfiguration automatisch an all Ihre Linux-Clients gesendet. Wenn Sie unterschiedliche Konfigurationen für verschiedene Linux-Clients benötigen, müssen Sie Client-Dateien einzeln bearbeiten, oder Sie verwenden eine Alternative wie PowerShell DSC, Chef oder Puppet.

Sie können die syslog-Ereignisse und Leistungsindikatoren angeben, die Sie mithilfe der Konfigurationsdateien auf den Linux-Computern sammeln wollen. *Wenn Sie die Datensammlung konfigurieren möchten, indem Sie die Agent-Konfigurationsdateien bearbeiten, sollten Sie die zentrale Konfiguration deaktivieren.* Die unten dargestellten Anweisungen zeigen, wie die Datensammlung in den Konfigurationsdateien des Agent konfiguriert wird sowie die Deaktivierung der zentralen Konfiguration aller OMS-Agents für Linux oder einzelne Computer.

### Deaktivierung der OMS-Verwaltung für einen einzelnen Linux-Computer

Die zentralisierte Datensammlung für die Konfigurationsdaten ist für einen einzelnen Linux-Computer durch das Skript „OMS\_MetaConfigHelper.py“ deaktiviert. Dies kann nützlich sein, wenn eine Teilmenge von Computern eine spezielle Konfiguration aufweisen sollte.

So deaktivieren Sie die zentralisierte Konfiguration:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

So aktivieren Sie die zentralisierte Konfiguration erneut:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## Leistungsindikatoren von Linux

Leistungsindikatoren von Linux sind ähnlich der Windows-Leistungsindikatoren – beide funktionieren ähnlich. Sie können die folgenden Verfahren verwenden, um sie hinzuzufügen und zu konfigurieren. Nachdem sie in OMS hinzugefügt wurden, werden Daten für die Leistungsindikatoren alle 30 Sekunden erfasst.

### So fügen Sie einen Linux-Leistungsindikator in OMS hinzu

1. Sie können Linux-Leistungsindikatoren auf der Seite „Einstellungen“ hinzufügen, um OMS-Agents für Linux mit dem OMS-Portal zu konfigurieren. Klicken Sie dazu auf **Daten**. ![data](./media/log-analytics-linux-agents/oms-settings-data01.png)
2. Klicken Sie auf der Seite **Einstellungen** unter **Daten** auf **Linux-Leistungsindikatoren** und anschließend links auf das Plus-Symbol. Geben Sie den Namen des Indikators ein, den Sie hinzufügen möchten. ![Leistungsindikatoren von Linux](./media/log-analytics-linux-agents/oms-linuxperfcounter01.png)
3. Wenn Sie den vollständigen Namen des Leistungsindikators nicht kennen, können Sie einen Teil des Namens angeben, und eine Liste der verfügbaren Leistungsindikatoren wird angezeigt. Wenn Sie den Indikator gefunden haben, den Sie hinzufügen möchten, klicken Sie auf den Namen in der Liste und anschließend auf das Plus-Symbol, um den Indikator hinzuzufügen.
4. Nachdem Sie den Indikator hinzugefügt haben, wird er in der Liste mit Indikatoren mit einem farbigen Balken markiert angezeigt.
5. Standardmäßig ist die Option **Apply below configuration to my machines** (Unten aufgeführte Konfigurationen auf meinem Computer anwenden) ausgewählt. Wenn Sie das Senden von Konfigurationsdaten deaktivieren möchten, löschen Sie die Sammlung.
6. Wenn Sie mit den Änderungen an den Leistungsindikatoren fertig sind, klicken Sie unten auf der Seite auf **Speichern**, um Ihre Änderungen abzuschließen. Die von Ihnen vorgenommenen Konfigurationsänderungen werden dann normalerweise innerhalb von fünf Minuten an alle OMS-Agents für Linux gesendet, die in OMS registriert sind.

### Konfigurieren von Linux-Leistungsindikatoren in OMS

Sie können für Windows-Leistungsindikatoren eine bestimmte Instanz für jeden Leistungsindikator auswählen. Bei Linux-Leistungsindikatoren gilt jedoch, dass die Instanz eines Indikators, die Sie wählen, für alle untergeordneten Indikatoren des übergeordneten Indikators gilt. Die folgende Tabelle zeigt die allgemeinen Instanzen, die jeweils für die Linux- und Windows-Leistungsindikatoren verfügbar sind.

| **Instanzname** | **Bedeutung** |
| --- | --- |
| \_Total | Gesamtsumme aller Instanzen |
| * | Alle Instanzen |
| (/|/var) | Stimmt mit den Instanzen mit den Namen „/“ oder „/var“ überein. |


Ebenso gilt das Beispielintervall, das Sie für einen übergeordneten Indikator auswählen, für alle untergeordneten Indikatoren. Anders ausgedrückt: Alle untergeordneten Indikatorbeispielintervalle und -instanzen sind miteinander verbunden.

### Hinzufügen und Konfigurieren von Leistungsmetriken mit Linux

Die Leistungsmetriken, die gesammelt werden, werden durch die Konfiguration in /etc/opt/microsoft/omsagent/conf/omsagent.conf gesteuert. Informationen zu verfügbaren Klassen und Metriken für den OMS-Agent für Linux finden Sie unter [Available performance metrics](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) (Verfügbare Leistungsmetriken).

Jedes Objekt oder jede Kategorie von Leistungsindikatoren, die gesammelt werden sollen, sollten in der Konfigurationsdatei als einzelnes `<source>`-Element definiert sein. Die Syntax folgt dem unten angegebenen Muster.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Die konfigurierbaren Parameter dieses Elements sind:

- **Object\_name**: der Objektname für die Sammlung.
- **Instance\_regex**: ein *regulärer Ausdruck*, der definiert, welche Instanzen gesammelt werden sollen. Der Wert `.*` gibt alle Instanzen an. Sie können `_Total` angeben, um die Prozessormetriken nur für die Instanz „\_Total“ zu sammeln. Sie können `(crond|sshd)` angeben, um die Prozessmetriken nur für die „crond“- oder „sshd“-Instanzen zu sammeln.
- **Counter\_name\_regex**: ein *regulärer Ausdruck*, der definiert, welche Indikatoren (für das Objekt) gesammelt werden sollen. Geben Sie `.*` an, um alle Indikatoren für das Objekt zu sammeln. Sie können `.+Swap.+` angeben, um nur die Instanzen für den Auslagerungsbereich für das Speicherobjekt zu sammeln.
- **Interval:**: die Häufigkeit mit der die Indikatoren des Objekts gesammelt werden.

Die Standardkonfiguration für Leistungsmetriken ist:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### Aktivieren von MySQL-Leistungsindikatoren mithilfe von Linux-Befehlen

Falls MySQL-Server oder MariaDB-Server auf dem Computer erkannt wird, wenn das omsagent-Paket installiert wird, wird ein Leistungsüberwachungsanbieter für MySQL-Server automatisch installiert. Dieser Anbieter stellt eine Verbindung mit dem lokalen MySQL/MariaDB-Server her, um Leistungsstatistiken verfügbar zu machen. Sie müssen MySQL-Benutzeranmeldeinformationen konfigurieren, damit der Anbieter auf den MySQL-Server zugreifen kann.

Verwenden Sie den folgenden Beispielbefehl, um ein Standardbenutzerkonto für den MySQL-Server auf „localhost“ zu definieren.

>[AZURE.NOTE] Die Datei mit den Anmeldeinformationen muss vom omsagent-Konto gelesen werden können. Es wird empfohlen, den „mycimprovauth“-Befehl als omsagent auszuführen.


```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'

sudo service omiserverd restart
```


Alternativ können Sie auch die benötigten MySQL Anmeldeinformationen in einer Datei angeben, indem Sie die folgende Datei erstellen: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Weitere Informationen zum Verwalten der MySQL-Anmeldeinformationen für die Überwachung mithilfe der „mysql-auth“-Datei finden Sie unter [Verwalten von Anmeldeinformationen für die MySQL-Überwachung in der Authentifizierungsdatei](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Details zu Objektberechtigungen, die der MySQL-Benutzer für die Sammlung von MySQL-Serverleistungsdaten benötigt, finden Sie unter [Datenbankberechtigungen für MySQL-Leistungsindikatoren](#database-permissions-required-for-mysql-performance-counters).

### Aktivieren von Apache HTTP Server-Leistungsindikatoren mithilfe von Linux-Befehlen

Falls Apache HTTP Server auf dem Computer erkannt wird, wenn das omsagent-Paket installiert wird, wird ein Leistungsüberwachungsanbieter für Apache HTTP Server automatisch installiert. Dieser Anbieter basiert auf einem Apache-„Modul“, das in den Apache HTTP Server geladen werden muss, um auf Leistungsdaten zuzugreifen.

Sie können das Modul mit dem folgenden Befehl laden:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Führen Sie folgenden Befehl aus, um das Apache-Überwachungsmodul zu entfernen:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### So zeigen Sie Leistungsdaten mit Log Analytics an

1. Klicken Sie im Operations Management Suite-Portal auf die Kachel „Protokollsuche“.
2. Geben Sie in der Suchleiste `* (Type=Perf)` ein, um alle Leistungsindikatoren anzuzeigen.


Da OMS auch Windows-Leistungsindikatordaten sammelt, sollten Sie die Suche auf Linux-spezifische Daten einschränken. Im folgenden Beispiel werden Leistungsdaten gezeigt, die spezifisch für einen Beispielserver von Linux namens Chorizo21 sind.

```
Type=Perf Computer=chorizo*
```

![Beispielserver, der in den Suchergebnissen angezeigt wird](./media/log-analytics-linux-agents/oms-perfsearch01.png)

Sie können im Ergebnisbereich auf **Metriken** klicken, um die Indikatoren anzuzeigen, für die die Daten gesammelt wurden. Für jeden Indikator werden Daten in Echtzeit als Graph angezeigt.

![metrics](./media/log-analytics-linux-agents/oms-perfmetrics01.png)


## Syslog

Syslog ist ein Ereignisprotokollierungsprotokoll ähnlich der Windows Ereignisprotokolle. Beide arbeiten ähnlich, wenn sie in OMS angezeigt werden.

### So fügen Sie eine neue Linux-syslog-Facility in OMS hinzu

1. Klicken Sie auf der Seite **Einstellungen** unter **Daten** auf **syslog** und anschließend links auf das Plus-Symbol. Geben Sie den Namen der syslog-Facility ein, den Sie hinzufügen möchten. ![Linux-syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2.	Wenn Sie den vollständigen Namen der Facility nicht kennen, können Sie einen Teil des Namens angeben, und eine Liste der verfügbaren syslog-Facilitys wird angezeigt. Wenn Sie die syslog-Facility gefunden haben, die Sie hinzufügen möchten, klicken Sie auf den Namen in der Liste und anschließend auf das Plus-Symbol, um die syslog-Facility hinzuzufügen.
3.	Nachdem Sie die Facility hinzugefügt haben, wird sie in der Liste mit einem farbigen Balken markiert angezeigt. Wählen Sie anschließend die Schweregrade (Kategorien der Informationen aus der syslog-Facility), die Sie sammeln möchten.
4.	Klicken Sie unten auf der Seite auf **Speichern**, um die Änderungen zu speichern. Die von Ihnen vorgenommenen Konfigurationsänderungen werden dann normalerweise innerhalb von fünf Minuten an alle OMS-Agents für Linux gesendet, die in OMS registriert sind.


### Konfigurieren von Linux-syslog-Räumen unter Linux

Syslog-Ereignisse werden vom syslog-Daemon, z.B. rsyslog oder syslog-ng, an einen lokalen Port gesendet, der vom Agent überwacht wird. Standardmäßig ist der Port 25224. Wenn der Agent installiert ist, wird eine syslog-Standardkonfiguration angewendet. Diese finden Sie unter:


Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog-ng: /etc/syslog-ng/syslog-ng.conf


Die syslog-Konfiguration des OMS-Standardagents lädt syslog-Ereignisse aus allen Räumen mit der Kategorie „Warnung“ oder höher hoch.

>[AZURE.NOTE] Wenn Sie die syslog-Konfiguration bearbeiten, müssen Sie den syslog-Daemon neu starten, damit die Änderungen wirksam werden.

Die syslog-Standardkonfiguration für den OMS-Agent für Linux für OMS ist:

#### Rsyslog

```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### Syslog-ng

```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### So zeigen Sie alle syslog-Ereignisse mit Log Analytics an

1. Klicken Sie im Operations Management Suite-Portal auf die Kachel **Protokollsuche**.
2. Wählen Sie in der Gruppe **Protokollverwaltung** eine vordefinierte syslog-Suche aus, und wählen Sie anschließend eine für die Ausführung aus.

Dieses Beispiel zeigt alle syslog-Ereignisse.

![Syslog-Ereignisse, die in der Protokollsuche angezeigt werden](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Jetzt können Sie die Suchergebnisse filtern.

## Linux-Warnungen

Wenn Sie Nagios oder Zabbix zum Verwalten Ihrer Linux-Computer verwenden, kann OMS von Tools generierte Warnungen empfangen. Es gibt jedoch derzeit keine Möglichkeit, eingehende Warnungsdaten mithilfe des OMS-Portals zu konfigurieren. Stattdessen müssen Sie eine Konfigurationsdatei bearbeiten, die anfängt, Warnungen an OMS zu senden.



### Sammeln Nagios-Warnungen

Sie müssen die folgenden Konfigurationsänderungen vornehmen, um Warnungen zu sammeln, die aus einem Nagios-Server stammen.

1. Gewähren Sie dem Benutzer **omsagent**-Lesezugriff auf die Nagios-Protokolldatei (d.h. /var/log/nagios/nagios.log/var/log/nagios/nagios.log). Sie können den Benutzer **omsagent** der **nagios**-Gruppe hinzufügen, vorausgesetzt, die „nagios.log“-Datei gehört der Gruppe **nagios**.

    ```
    sudo usermod –a -G nagios omsagent
    ```

2. Ändern Sie die Datei „omsagent.confconfiguration“ (/etc/opt/microsoft/omsagent/conf/omsagent.conf). Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht auskommentiert sind:

    ```
    <source>
    type tail
    #Update path to point to your nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```

3. Starten Sie den omsagent-Daemon neu:

    ```
    sudo service omsagent restart
    ```

### Sammeln von Zabbix-Warnungen

Zum Erfassen von Warnungen in einem Zabbix-Server müssen Sie ähnliche Schritte ausführen wie für Nagios oben, es sei denn, Sie müssen einen Benutzer und ein Kennwort im *Klartext* angeben. Dies ist nicht ideal, wird sich aber wahrscheinlich bald ändern. Es wird empfohlen, den Benutzer zu erstellen und diesem nur eine Überwachungsberechtigung zu gewähren, um dieses Problem zu beheben.

Ein Beispielabschnitt der „omsagent.conf“-Konfigurationsdatei (/etc/opt/microsoft/omsagent/conf/omsagent.conf) für Zabbix sollte in etwa wie folgt aussehen:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### Anzeigen von Warnungen in der Log Analytics-Suche

Nachdem Sie Ihre Linux-Computer zum Senden von Warnungen an OMS konfiguriert haben, können Sie einige einfache Protokollsuchabfragen verwenden, um die Warnungen anzuzeigen. Das folgende Suchabfragebeispiel gibt alle aufgezeichneten Warnungen zurück, die generiert wurden. Wenn beispielsweise ein Problem in Ihrer IT-Infrastruktur auftritt, können die Ergebnisse der folgenden Beispielabfrage möglicherweise darauf hindeuten, woher das Problem stammt. Sie können zudem problemlos die Warnungen nach dem Quellsystem filtern, um Ihre Untersuchung einzuschränken. Der Vorteil ist, dass Sie nicht unbedingt von Anfang an mit verschiedenen Verwaltungssystemen arbeiten müssen. Falls Ihre Warnungen an OMS gesendet werden, können Sie dort beginnen.

```
Type=Alert
```

#### So zeigen Sie alle Nagios-Warnungen mit Log Analytics an
1. Klicken Sie im Operations Management Suite-Portal auf die Kachel **Protokollsuche**.
2. Geben Sie in der Abfrageleiste die folgende Suchabfrage ein

    ```
    Type=Alert SourceSystem=Nagios
    ```
![Nagios-Warnungen, die in der Protokollsuche angezeigt werden](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Nachdem die Ergebnisse der Suche angezeigt werden, können Sie zu zusätzlichen Details filtern, wie z.B. *AlertState*.

### So zeigen Sie alle Zabbix-Warnungen mit Log Analytics an
1. Klicken Sie im Operations Management Suite-Portal auf die Kachel **Protokollsuche**.
2. Geben Sie in der Abfrageleiste die folgende Suchabfrage ein

    ```
    Type=Alert SourceSystem=Zabbix
    ```
![Zabbix-Warnungen, die in der Protokollsuche angezeigt werden](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Nachdem die Ergebnisse der Suche angezeigt werden, können Sie zu zusätzlichen Details filtern, wie z.B. *AlertName*.


## Kompatibilität mit dem System Center Operations Manager

Der OMS-Agent für Linux gibt Binärdateien für System Center Operations Manager-Agent frei. Die Installation des OMS-Agents für Linux auf einem derzeit von Operation Manager verwalteten System aktualisiert die OMI- und SCX-Pakete auf dem Computer auf eine neuere Version. Die OMS-Agents für Linux und System Center 2012 R2 sind kompatibel. **System Center 2012 SP1 und frühere Versionen sind derzeit allerdings nicht kompatibel mit dem OMS-Agent für Linux oder werden nicht unterstützt.**

>[AZURE.NOTE] Wenn der OMS-Agent für Linux auf einem Computer installiert ist, der derzeit nicht von Operations Manager verwaltet wird, und Sie den Computer später damit verwalten möchten, müssen Sie die OMI-Konfiguration modifizieren, bevor Sie die Erkennung des Computers starten. **Dieser Schritt ist nicht erforderlich, wenn der Operations Manager-Agent vor dem OMS-Agent für Linux installiert wird.**

### So aktivieren Sie den OMS-Agent für Linux zum Kommunizieren mit Operations Manager

1. Bearbeiten Sie die Datei „/etc/opt/omi/conf/omiserver.conf“
2. Stellen Sie sicher, dass die Zeile, die mit **httpsport=** beginnt, den Port 1270 definiert. Z.B. `httpsport=1270`
3. Starten Sie den OMI-Server neu:

    ```
    service omiserver restart or systemctl restart omiserver
    ```




## Erforderliche Datenbankberechtigungen für MySQL-Leistungsindikatoren

Um einem Benutzer der MySQL-Überwachung Berechtigungen zu gewähren, muss der gewährende Benutzer über die „GRANT-Option“-Berechtigung sowie über die Berechtigung verfügen, die gewährt wird.

Damit ein MySQL-Benutzer Leistungsdaten zurückgeben kann, benötigt der Benutzer Zugriff auf die folgenden Abfragen:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Zusätzlich zu diesen Abfragen muss der MySQL-Benutzer mithilfe der SELECT-Berechtigung Zugriff auf folgende Standardtabellen haben:

- information\_schema
- mysql

Diese Berechtigungen können durch Ausführen der folgenden „grant“-Befehle erteilt werden.

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## Verwalten von MySQL-Anmeldeinformationen in der Authentifizierungsdatei

Die folgenden Abschnitte helfen Ihnen beim Verwalten von MySQL-Anmeldeinformationen.

### Konfigurieren des MySQL-OMI-Anbieters

Der MySQL-OMI-Anbieter erfordert einen vorkonfigurierten MySQL-Benutzer und installierte MySQL-Clientbibliotheken, um eine Abfrage der Leistungs-/Integritätsinformationen der MySQL-Instanz durchzuführen.

### MySQL-OMI-Authentifizierungsdatei

Der MySQL-OMI-Anbieter verwendet eine Authentifizierungsdatei, um zu ermitteln, welche mit „bind-address“ festgelegte Adresse und welcher Port die MySQL-Instanz überwacht und welche Anmeldeinformationen verwendet werden müssen, um Metriken zu sammeln. Während der Installation scannt der MySQL-OMI-Anbieter die „my.cnf“-Konfigurationsdateien von MySQL (Standardspeicherorte) für die mit „bind-address“ festgelegte Adresse sowie den Port und legt teilweise die MySQL-OMI-Authentifizierungsdatei fest.

Fügen Sie eine vorab generierte MySQL-OMI-Authentifizierungsdatei in das richtige Verzeichnis ein, um die Überwachung einer MySQL-OMI-Serverinstanz abzuschließen.

### Authentifizierungsdateiformat

Die MySQL-OMI-Authentifizierungsdatei ist eine Textdatei, die Informationen über Folgendes enthält:

- Port
- Bind-Adresse
- MySQL-Benutzername
- Base64 codiertes Kennwort

Die MySQL-OMI-Authentifizierungsdatei erteilt Berechtigungen für Lese-/Schreibzugriff nur an den Linux-Benutzer, der sie generiert hat.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Eine Standard-MySQL OMI-Authentifizierungsdatei enthält eine Standardinstanz und eine Portnummer, je nachdem, welche Informationen aus der gefundenen MySQL-Konfigurationsdatei verfügbar sind und analysiert wurden.

Die Standardinstanz ist ein Mittel zur einfacheren Verwaltung mehrerer MySQL-Instanzen auf einem Linux-Server und wird von der Instanz mit Port 0 angegeben. Alle hinzugefügten Instanzen übernehmen die Eigenschaften, die von der Standardinstanz festgelegt wurden. Wenn beispielsweise die MySQL-Instanz hinzugefügt wird, die einen Port „3308“ überwacht, werden die mit „bind-address“ festgelegte Adresse, der Benutzername und das Base64 codierte Kennwort der Standardinstanz verwendet, um die Instanz, die 3308 überwacht, zu testen und zu überwachen. Wenn die Instanz auf 3308 an eine andere Adresse gebunden ist und das gleiche MySQL-Benutzernamen- und Kennwortpaar verwendet, ist nur die erneute Spezifikation der mit „bind-address“ festgelegten Adresse erforderlich, und die anderen Eigenschaften werden geerbt.

Beispiele der Authentifizierungsdatei sehen in etwa wie folgt aus.

Standardinstanz und Instanz mit Port 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Standardinstanz und Instanz mit Port 3308 + anderes Base 64 codiertes Kennwort:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Eigenschaft** | **Beschreibung** |
| --- | --- |
| Port | Der Port stellt den aktuellen Port dar, der die MySQL-Instanz überwacht. Der Port 0 bedeutet, dass die folgenden Eigenschaften für die Standardinstanz verwendet werden. |
| Bind-Adresse | die Bind-Adresse ist die aktuelle MySQL-Adresse, die durch „bind-address“ festgelegt wurde |
| username | Dies ist der Benutzername des MySQL-Benutzers, den Sie zur Überwachung der MySQL-Serverinstanz verwenden möchten. |
| Base64 codiertes Kennwort | Das mit Base64 codierte Kennwort des Benutzers, mit dem Sie MySQL überwachen möchten. |
| AutoUpdate | Der Anbieter überprüft erneut Veränderungen in der „my.cnf“-Datei, wenn der MySQL-OMI-Anbieter upgegradet wird, und überschreibt die MySQL-OMI-Authentifizierungsdatei. Legen Sie dieses Flag auf TRUE oder FALSE fest, je nach erforderlichen Updates für die MySQL-OMI-Authentifizierungsdatei. |

#### Speicherort der Authentifizierungsdatei

Die MySQL-OMI-Authentifizierungsdatei sollte sich in folgendem Speicherort befinden und „mysql-auth“ genannt werden:

/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth

Die Datei (und das auth/omsagent-Verzeichnis) sollte dem omsagent-Benutzer gehören.

## Agentprotokolle

Die Protokolle für den OMS-Agent für Linux befinden sich unter:

/var/opt/microsoft/omsagent/log/

Die Protokolle für den OMS-Agent für Linux für das omsconfig-Programm (Agentkonfiguration) befinden sich unter:

/var/opt/microsoft/omsconfig/log/

Protokolle für die OMI- und SCX-Komponenten (die Leistungsmetrikdaten angeben) befinden sich unter:

/var/opt/omi/log/ and /var/opt/microsoft/scx/log

## Bekannte Einschränkungen
Lesen Sie die folgenden Abschnitte, um mehr über die derzeitigen Einschränkungen des OMS-Agents für Linux zu erfahren.

### Azure-Diagnose

Möglicherweise sind weitere Schritte für in Azure ausgeführte virtuelle Linux-Computer erforderlich, um Datensammlungen durch Azure Diagnostics und Operations Management Suite zuzulassen. **Version 2.2** der Diagnoseerweiterung für Linux ist für die Kompatibilität mit dem OMS-Agent für Linux erforderlich.

Weitere Informationen zum Installieren und Konfigurieren der Diagnoseerweiterung für Linux finden Sie unter [Use the Azure CLI command to enable Linux Diagnostic Extension](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension/#use-the-azure-cli-command-to-enable-linux-diagnostic-extension) (Verwenden des Azure-CLI-Befehls zum Aktivieren der Linux-Diagnoseerweiterung).

**Aktualisieren der Linux-Diagnose-Erweiterung von 2.0 auf 2.2 Azure-Befehlszeilenschnittstelle ASM:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Diese Befehlsbeispiele verweisen auf eine Datei namens „PrivateConfig.json.“ Das Format dieser Datei sollte dem folgenden Beispiel ähneln.

```
	{
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
	}
```

### Sysklog wird nicht unterstützt

Zum Sammeln von syslog-Nachrichten sind entweder rsyslog oder syslog-ng erforderlich. Der Standard-syslog-Daemon in Version 5 von Red Hat Enterprise Linux, CentOS und Oracle Linux-Version (sysklog) wird für die syslog-Ereigniserfassung nicht unterstützt. Der rsyslog-Daemon sollte installiert und so konfiguriert werden, dass er sysklog ersetzt, um syslog-Daten von dieser Version dieser Verteilung zu sammeln. Weitere Informationen zum Austauschen von sysklog durch rsyslog finden Sie unter [Install the newly built rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM) (Installieren des neu erstellten rsyslog-RPM).

## Nächste Schritte

- [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog) zum Hinzufügen von Funktionen und zum Sammeln von Daten.
- Vertrautmachen mit den [Suchprotokollen](log-analytics-log-searches.md) zur Anzeige von detaillierten Informationen, die von Lösungen gesammelt wurden.
- Verwenden von [Dashboards](log-analytics-dashboards.md) zum Speichern und Anzeigen von eigenen benutzerdefinierten Suchvorgängen.

<!---HONumber=AcomDC_0504_2016-->