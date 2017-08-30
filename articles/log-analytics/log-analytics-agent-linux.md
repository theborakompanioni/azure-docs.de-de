---
title: Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS) | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie in Azure, einer anderen Cloud oder lokal gehostete Linux-Computer mit dem OMS Agent für Linus mit OMS verbunden werden können."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 1c05f68235aafd0fa098a3b0edaba1258df09380
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a>Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS) 

Mit Microsoft Operations Management Suite (OMS) können Sie Daten sammeln und verarbeiten, die auf Linux-Computern oder in Containerlösungen wie Docker generiert wurden – als physische Server oder virtuelle Computer in Ihrem lokalen Rechenzentrum oder als virtuelle Computer in einem cloudgehosteten Dienst wie Amazon Web Services (AWS) oder Microsoft Azure. Sie können auch in OMS verfügbare Verwaltungslösungen verwenden, z.B. die Änderungsnachverfolgung zum Identifizieren von Konfigurationsänderungen oder die Updateverwaltung zum Verwalten von Softwareupdates, um den Lebenszyklus Ihrer virtuellen Linux-Computer proaktiv zu verwalten. 

Der OMS-Agent für Linux kommuniziert ausgehend über TCP-Port 443 mit dem OMS-Dienst. Falls der Computer für die Kommunikation über das Internet eine Verbindung mit einer Firewall oder einem Proxyserver herstellt, finden Sie unter [Konfigurieren des Agents für die Verwendung mit einem HTTP-Proxyserver oder dem OMS-Gateway](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) weitere Informationen zu den Konfigurationsänderungen, die angewendet werden müssen.  Wenn Sie den Computer mit System Center 2016 – Operations Manager oder Operations Manager 2012 R2 überwachen, kann er mit dem OMS-Dienst mehrfach vernetzt werden, um Daten zu sammeln und an den Dienst weiterzuleiten, wobei er weiterhin von Operations Manager überwacht werden kann.  Linux-Computer, die durch eine in OMS integrierte Operations Manager-Verwaltungsgruppe überwacht werden, empfangen keine Konfiguration für Datenquellen und weitergeleitete Daten über die Verwaltungsgruppe.  Der OMS-Agent kann nicht für die Berichterstattung für mehr als einen Arbeitsbereich konfiguriert werden.  

Sofern nach Ihren IT-Sicherheitsrichtlinien Computer in Ihrem Netzwerk keine Internetverbindung herstellen dürfen, kann der Agent so konfiguriert werden, dass er eine Verbindung mit dem OMS-Gateway herstellt, um Konfigurationsinformationen zu empfangen und gesammelte Daten abhängig von der aktivierten Lösung zu senden. Weitere Informationen und Anleitungen zum Konfigurieren Ihres OMS-Linux-Agents für die Kommunikation mit dem OMS-Dienst über ein OMS-Gateway finden Sie unter [Verbinden von Computern mit OMS über das OMS-Gateway](log-analytics-oms-gateway.md).  

Das folgende Diagramm zeigt die Verbindung zwischen dem durch einen Agent verwalteten Linux-Computer und OMS, einschließlich der Richtung und der Ports.

![Diagramm für die Kommunikation direkter Agents mit OMS](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Systemanforderungen
Prüfen Sie zunächst anhand der folgenden Informationen, ob die Voraussetzungen erfüllt sind.

### <a name="supported-linux-operating-systems"></a>Unterstützte Linux-Betriebssysteme
Die folgenden Linux-Distributionen werden offiziell unterstützt.  Der OMS-Agent für Linux kann jedoch auch auf anderen Verteilungen ausgeführt werden, die hier nicht aufgeführt sind.

* Amazon Linux 2012.09 bis 2015.09 (x86/x64)
* CentOS Linux 5, 6 und 7 (x86/x64)
* Oracle Linux 5, 6 und 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 und 7 (x86/x64)
* Debian GNU/Linux 6, 7 und 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 und 12 (x86/x64)

### <a name="network"></a>Netzwerk
Die Aufstellung unten enthält die Proxy- und Firewall-Konfigurationsinformationen, die der Linux-Agent benötigt, um mit OMS zu kommunizieren. Ausgehender Datenverkehr von Ihrem Netzwerk zum OMS-Dienst. 

|Agent-Ressource| Ports |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.blob.core.windows.net/ | Port 443|   
|*.azure-automation.net | Port 443|  

### <a name="package-requirements"></a>Paketanforderungen

 **Erforderliches Paket**   | **Beschreibung**   | **Mindestversion**
--------------------- | --------------------- | -------------------
Glibc | GNU C-Bibliothek   | 2.5-12 
Openssl | OpenSSL-Bibliotheken | 0.9.8e oder 1.0
Curl | cURL-Webclient | 7.15.5
Python-ctypes | | 
PAM | Module für austauschbare Authentifizierung | 

> [!NOTE]
>  Zum Sammeln von syslog-Nachrichten sind entweder rsyslog oder syslog-ng erforderlich. Der Standard-syslog-Daemon in Version 5 von Red Hat Enterprise Linux, CentOS und Oracle Linux-Version (sysklog) wird für die syslog-Ereigniserfassung nicht unterstützt. Der rsyslog-Daemon sollte installiert und so konfiguriert werden, dass er sysklog ersetzt, um syslog-Daten von dieser Version dieser Distributionen zu sammeln. 

Der Agent umfasst mehrere Pakete. Die Release-Datei enthält die folgenden Pakete, die durch Ausführen des Shell-Pakets mit `--extract` verfügbar sind:

**Paket** | **Version** | **Beschreibung**
----------- | ----------- | --------------
omsagent | 1.4.0 | Der Operations Management Suite-Agent für Linux
omsconfig | 1.1.1 | Konfigurations-Agent für den OMS-Agent
omi | 1.2.0 | Open Management Infrastructure (OMI) – ein kompakter CIM-Server
scx | 1.6.3 | OMI-CIM-Anbieter für Leistungsmetriken für das Betriebssystem
apache-cimprov | 1.0.1 | Apache HTTP Server-Anbieter für die Leistungsüberwachung für OMI Installiert, wenn Apache HTTP Server erkannt wird
mysql-cimprov | 1.0.1 | MySQL Server-Anbieter für die Leistungsüberwachung für OMI Installiert, wenn ein MySQL-/MariaDB-Server erkannt wird
docker-cimprov | 1.0.0 | Docker-Anbieter für OMI. Installiert, wenn Docker erkannt wird

### <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilität mit dem System Center Operations Manager
Der OMS-Agent für Linux gibt Binärdateien für System Center Operations Manager-Agent frei. Wenn Sie den OMS-Agent für Linux auf einem derzeit von Operation Manager verwalteten System installieren, wird ein Upgrade für die OMI- und SCX-Pakete auf dem Computer auf eine neuere Version durchgeführt. In diesem Release sind die OMS- und System Center 2016 – Operations Manager-/Operations Manager 2012 R2-Agents für Linux kompatibel. 

> [!NOTE]
> System Center 2012 SP1 und frühere Versionen sind derzeit nicht kompatibel mit dem OMS-Agent für Linux und werden nicht unterstützt.<br>
> Wenn der OMS-Agent für Linux auf einem Computer installiert ist, der derzeit nicht von Operations Manager überwacht wird, und Sie den Computer später damit überwachen möchten, müssen Sie die [OMI-Konfiguration](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) modifizieren, bevor Sie die Erkennung des Computers starten. **Dieser Schritt ist *nicht* erforderlich, wenn der Operations Manager-Agent vor dem OMS-Agent für Linux installiert wird.**

### <a name="system-configuration-changes"></a>Systemkonfigurationsänderungen
Nach der Installation des OMS-Agents für Linux-Pakete werden die folgenden zusätzlichen systemweiten Konfigurationsänderungen angewendet. Diese Artefakte werden entfernt, wenn das „omsagent“-Paket deinstalliert wird.

* Ein Benutzer ohne Berechtigungen mit dem Namen `omsagent` wird erstellt. Dies ist das Konto, unter dem der omsagent-Daemon ausgeführt wird.
* Eine „include“-Datei für sudo-Benutzer wird unter „/etc/sudoers.d/omsagent“ erstellt. Diese autorisiert omsagent dazu, die syslog- und omsagent-Daemons neu zu starten. Falls „include“-sudo-Direktiven nicht in der installierten Version von sudo unterstützt werden, werden diese Einträge in „/etc/sudoers“ geschrieben.
* Die syslog-Konfiguration wird geändert, um eine Teilmenge von Ereignissen an den Agent weiterzuleiten. Weitere Informationen finden Sie unten im Abschnitt **Konfigurieren der Datensammlung**

### <a name="upgrade-from-a-previous-release"></a>Upgrade von einem früheren Release
Ein Upgrade von Versionen vor 1.0.0-47 wird in diesem Release unterstützt. Beim Ausführen der Installation mit dem Befehl `--upgrade` wird für alle Komponenten des Agents ein Upgrade auf die neueste Version durchgeführt.

## <a name="installing-the-agent"></a>Installieren des Agents

Dieser Abschnitt beschreibt, wie der OMS-Agent für Linux mit einem Bundle installiert wird, das Debian- und RPM-Pakete für die einzelnen Agent-Komponenten enthält.  Er kann direkt installiert oder zum Abrufen der einzelnen Pakete extrahiert werden.  

Zuerst benötigen Sie Ihre ID und den Schlüssel für den OMS-Arbeitsbereich. Diese Daten erhalten Sie, indem Sie zum [klassischen OMS-Portal](https://mms.microsoft.com) wechseln.  Wählen Sie auf der Seite **Übersicht** im oberen Menü die Option **Einstellungen**, und navigieren Sie anschließend zu **Verbundene Quellen\Linux-Server**.  Der Wert wird jeweils rechts von **Arbeitsbereichs-ID** und **Primärschlüssel** angezeigt.  Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.    

1. Laden Sie den aktuellen [OMS-Agent für Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) oder [OMS-Agent für Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) über GitHub herunter.  
2. Übertragen Sie das entsprechende Paket (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
3. Installieren Sie das Paket mit einem der Argumente `--install` oder `--upgrade`. 

    > [!NOTE]
    > Wenn bereits Pakete installiert sind, z.B. wenn der System Center Operations Manager-Agent für Linux bereits installiert ist, verwenden Sie das Argument `--upgrade`. Geben Sie für die Verbindung mit Operations Management Suite während der Installation die Parameter `-w <WorkspaceID>` und `-s <Shared Key>` an.


#### <a name="to-install-and-onboard-directly"></a>Installieren und direktes Integrieren
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a>Durchführen eines Upgrades für das Agent-Paket
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Installieren und Integrieren in einem Arbeitsbereich in der Cloud für US-Behörden
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Konfigurieren des Agents für die Verwendung mit einem HTTP-Proxyserver oder einem OMS-Gateway
Der OMS-Agent für Linux unterstützt die Kommunikation mit dem OMS-Dienst über einen HTTP- oder HTTPS-Proxyserver oder ein OMS-Gateway.  Es wird sowohl die anonyme als auch die Standardauthentifizierung (Benutzername und Kennwort) unterstützt.  

### <a name="proxy-configuration"></a>Proxykonfiguration
Der Wert für die Proxykonfiguration weist die folgende Syntax auf:

`[protocol://][user:password@]proxyhost[:port]`

Eigenschaft|Beschreibung
-|-
Protocol|HTTP oder HTTPS
user|Optionaler Benutzername für die Proxyauthentifizierung
password|Optionales Kennwort für die Proxyauthentifizierung
proxyhost|Adresse oder FQDN des Proxyservers/OMS-Gateways
port|Optionale Portnummer für den Proxyserver/das OMS-Gateway

Beispiel: `http://user01:password@proxy01.contoso.com:8080`

Der Proxyserver kann während der Installation oder durch Ändern der Konfigurationsdatei „proxy.conf“ nach der Installation angegeben werden.   

### <a name="specify-proxy-configuration-during-installation"></a>Angeben der Proxykonfiguration während der Installation
Das Argument `-p` oder `--proxy` für das omsagent-Installationspaket gibt die zu verwendende Proxykonfiguration an. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Definieren der Proxykonfiguration in einer Datei
Die Proxykonfiguration kann in den Dateien `/etc/opt/microsoft/omsagent/proxy.conf` und `/etc/opt/microsoft/omsagent/conf/proxy.conf ` festgelegt werden. Die Dateien können direkt erstellt oder bearbeitet werden, die Berechtigungen müssen aber aktualisiert werden, damit der Benutzer „omiuser“ eine Leseberechtigung für die Dateien hat. Beispiel:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Entfernen der Proxykonfiguration
Um eine zuvor definierte Proxykonfiguration zu entfernen und die direkte Verbindung wiederherzustellen, entfernen Sie die Datei „proxy.conf“:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a>Integration in die Operations Management Suite
Wenn eine Arbeitsbereichs-ID und der zugehörige Schlüssel nicht während der Paketinstallation bereitgestellt wurden, muss der Agent anschließend bei Operations Management Suite registriert werden.

### <a name="onboarding-using-the-command-line"></a>Integrieren über die Befehlszeile
Führen Sie den Befehl „omsadmin.sh“ unter Angabe der Arbeitsbereichs-ID und des Schlüssels für den Arbeitsbereich aus. Dieser Befehl muss als Root-Benutzer (mit erhöhten Rechten durch sudo) ausgeführt werden:
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Integrieren mithilfe einer Datei
1.  Erstellen Sie die Datei `/etc/omsagent-onboard.conf`. Die Datei muss für Root lesbar und schreibbar sein.
`sudo vi /etc/omsagent-onboard.conf`
2.  Fügen Sie die folgenden Zeilen mit Ihrer Arbeitsbereichs-ID und dem gemeinsam verwendeten Schlüssel in die Datei ein:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Führen Sie den folgenden Befehl aus, um die Integration mit OMS durchzuführen: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  Die Datei wird nach dem Onboarding gelöscht.

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>Aktivieren des OMS-Agents für Linux für das Senden von Meldungen an System Center Operations Manager
Führen Sie die folgenden Schritte durch, um den OMS-Agent für Linux so zu konfigurieren, dass Meldungen an eine System Center Operations Manager-Verwaltungsgruppe gesendet werden.  

1. Bearbeiten Sie die Datei `/etc/opt/omi/conf/omiserver.conf`
2. Stellen Sie sicher, dass die Zeile, die mit **httpsport=** beginnt, den Port 1270 definiert. Beispiel: `httpsport=1270`
3. Starten Sie den OMI-Server neu: `sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Agentprotokolle
Die Protokolle für den OMS-Agent für Linux finden Sie unter: `/var/opt/microsoft/omsagent/<workspace id>/log/`. Die Protokolle für das Programm omsconfig (Agentkonfiguration) finden Sie unter: `/var/opt/microsoft/omsconfig/log/`. Protokolle für die OMI- und SCX-Komponenten (zur Bereitstellung von Leistungsmetrikdaten) finden Sie unter: `/var/opt/omi/log/ and /var/opt/microsoft/scx/log`.

### <a name="log-rotation-configuration"></a>Protokollrotationskonfiguration##
Die Protokollrotationskonfiguration für omsagent finden Sie unter: `/etc/logrotate.d/omsagent-<workspace id>`.

Die Standardeinstellungen sind folgende: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Deinstallieren des OMS-Agents für Linux
Die Agent-Pakete können deinstalliert werden, indem Sie die SH-Bundledatei mit dem `--purge`-Argument ausführen. Hierdurch werden der Agent und dessen Konfiguration vollständig vom Computer entfernt.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a>Problem: keine Verbindung über den Proxy mit OMS möglich

#### <a name="probable-causes"></a>Mögliche Ursachen
* Der während der Integration angegebene Proxy war falsch.
* Die OMS-Dienstendpunkte sind nicht in der Zulassungsliste in Ihrem Rechenzentrum enthalten. 

#### <a name="resolutions"></a>Lösungen
1. Führen Sie das Onboarding des OMS-Diensts mit dem OMS-Agent für Linux durch, indem Sie dem folgenden Befehl mit der Option `-v` ausführen. Dadurch wird die ausführliche Ausgabe des Agents über eine Proxy-Verbindung an den OMS-Dienst möglich. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Lesen Sie den Abschnitt [Konfigurieren des Agents für die Verwendung mit einem HTTP-Proxyserver(#configuring the-agent-for-use-with-a-http-proxy-server), um sicherzustellen, dass der Agent ordnungsgemäß für die Kommunikation über einen Proxyserver konfiguriert wurde.    
* Vergewissern Sie sich erneut, dass die folgenden OMS-Dienstendpunkte in der Zulassungsliste enthalten sind:

    |Agent-Ressource| Ports |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.blob.core.windows.net/ | Port 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problem: Beim Versuch der Integration erhalten Sie einen 403-Fehler.

#### <a name="probable-causes"></a>Mögliche Ursachen
* Das Datum und die Uhrzeit auf dem Linux-Server sind falsch. 
* Die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel sind falsch.

#### <a name="resolution"></a>Lösung

1. Überprüfen Sie die Uhrzeit auf dem Linux-Server mit dem Befehl „date“. Wenn die Zeit um +/-15 Minuten von der aktuellen Uhrzeit abweicht ist, tritt bei der Integration ein Fehler auf. Aktualisieren Sie zur Behebung dieses Problems das Datum und/oder die Zeitzone des Linux-Servers. 
2. Überprüfen Sie, ob die neueste Version des OMS-Agents für Linux installiert ist.  Die neueste Version benachrichtigt Sie nun darüber, ob der Onboardingfehler durch die Zeitabweichung verursacht wird.
3. Führen Sie das Onboarding gemäß den obigen Installationsanweisungen in diesem Thema mit korrekten Daten für die Arbeitsbereich-ID und den Arbeitsbereichsschlüssel erneut durch.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problem: In der Protokolldatei sind direkt nach der Integration die Fehler 500 und 404 enthalten.
Dies ist ein bekanntes Problem, das beim ersten Hochladen von Linux-Daten in einen OMS-Arbeitsbereich auftritt. Gesendete Daten und die Ausführung des Diensts sind davon nicht betroffen.

### <a name="issue--you-are-not-seeing-any-data-in-the-oms-portal"></a>Problem: Im OMS-Portal werden keine Daten angezeigt.

#### <a name="probable-causes"></a>Mögliche Ursachen

- Fehler beim Onboarding zum OMS-Dienst
- Verbindung zum OMS-Dienst ist blockiert
- Der OMS-Agent für Linux-Daten ist überlastet.

#### <a name="resolutions"></a>Lösungen
1. Überprüfen Sie, ob die Integration des OMS-Diensts erfolgreich war, indem Sie überprüfen, ob die folgende Datei vorhanden ist: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Erneutes Onboarding mithilfe der Befehlszeilenanweisungen `omsadmin.sh`
3. Wenn Sie einen Proxy verwenden, lesen Sie die Proxylösungsschritte weiter oben.
4. In einigen Fällen, wenn der OMS-Agent für Linux nicht mit dem OMS-Dienst kommunizieren kann, werden Daten auf dem Agent bis zur vollständigen Puffergröße von 50 MB in eine Warteschlange gestellt. Der OMS-Agent für Linux sollte über den folgenden Befehl neu gestartet werden: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Dieses Problem wurde ab der Agent-Version 1.1.0-28 behoben.
> 
