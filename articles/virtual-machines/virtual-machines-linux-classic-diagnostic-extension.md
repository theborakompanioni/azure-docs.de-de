---
title: "Überwachen eines virtuellen Linux-Computers mit einer VM-Erweiterung | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie die Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten einer Linux-VM in Azure verwenden."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
translationtype: Human Translation
ms.sourcegitcommit: 0af5a4e2139a202c7f62f48c7a7e8552457ae76d
ms.openlocfilehash: 11a1f6ea289db38aeb8c2fff2c9b0e6b1f0f044c


---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten eines Linux-VM
## <a name="introduction"></a>Einführung
(**Hinweis**: Die Linux-Diagnoseerweiterung ist als Open Source bei [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) verfügbar. Hier werden jeweils die neuesten Informationen zu der Erweiterung veröffentlicht. Besuchen Sie daher ggf. zuerst die [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic)-Seite.)

Mit der Linux-Diagnoseerweiterung können Benutzer die Linux-VMs überwachen, die unter Microsoft Azure ausgeführt werden. Sie verfügt über die folgenden Funktionen:

* Sammeln und Hochladen der Informationen zur Systemleistung von der Linux-VM in die Speichertabelle des Benutzers, z.B. Diagnose- und syslog-Informationen
* Anpassen der zu sammelnden und hochzuladenden Datenmetriken durch Benutzer
* Hochladen angegebener Protokolldateien in eine festgelegte Speichertabelle durch Benutzer

In der aktuellen Version 2.3 werden folgende Daten verarbeitet:

* Alle Rsyslog-Protokolle von Linux, z.B. System-, Sicherheits- und Anwendungsprotokolle
* Alle Systemdaten, die auf der [System Center Cross Platform Solutions-Website](https://scx.codeplex.com/wikipage?title=xplatproviders)angegeben sind
* Benutzerdefinierte Protokolldateien

Diese Erweiterung funktioniert sowohl mit dem klassischen Bereitstellungsmodell als auch mit dem Resource Manager-Bereitstellungsmodell.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Aktuelle Version der Erweiterung und Einstellung älterer Versionen
Die neueste Version der Erweiterung ist **2.3**. **Ältere Versionen (2.0, 2.1 und 2.2) werden als veraltet markiert und stehen Ende dieses Jahres (2016) nicht mehr zur Verfügung**. Wenn Sie bei der Installation der Linux-Diagnoseerweiterung automatische Nebenversionsupgrades deaktiviert haben, wird dringend empfohlen, die Erweiterung zu deinstallieren und mit aktivierten automatischen Nebenversionsupgrades neu zu installieren. Auf klassischen virtuellen Computern (ASM-Modus) können Sie dazu als Version „2. *“ angeben, wenn Sie die Erweiterung über die plattformübergreifende Azure-Befehlszeilenschnittstelle oder über PowerShell installieren. Auf virtuellen ARM-Computern können Sie hierzu in der VM-Bereitstellungsvorlage Folgendes angeben: "autoUpgradeMinorVersion": true. Darüber hinaus sollte bei jeder neuen Installation der Erweiterung die Option für automatische Nebenversionsupgrades aktiviert sein.

## <a name="enable-the-extension"></a>Aktivieren der Erweiterung
Sie können diese Erweiterung über das [Azure-Portal](https://portal.azure.com/#), über Azure PowerShell oder über Azure-CLI-Skripts aktivieren.

Um die System- und Leistungsdaten direkt vom Azure-Portal aus anzuzeigen und zu konfigurieren, folgen Sie [diesen Schritten im Azure-Blog] (https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/„URL zum Windows-Blog“/).

In diesem Artikel wird beschrieben, wie Sie die Erweiterung mit Azure-CLI-Befehlen aktivieren und konfigurieren. So können Sie die Daten direkt aus der Speichertabelle lesen und anzeigen.

Beachten Sie, dass die hier beschriebenen Konfigurationsmethoden nicht für das Azure-Portal funktionieren. Zum Anzeigen und Konfigurieren der System- und Leistungsdaten direkt über das Azure-Portal muss die Erweiterung über das Portal aktiviert werden.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure Linux Agent ab Version 2.0.6**.
  Beachten Sie, dass die meisten Images des Linux-Katalogs für virtuelle Azure-Computer die Version 2.0.6 oder höher besitzen. Sie können **WAAgent -version** ausführen, um zu ermitteln, welche Version auf dem virtuellen Computer installiert ist. Wenn auf dem virtuellen Computer eine ältere Version als 2.0.6 ausgeführt wird, können Sie sie unter Verwendung [dieser Anweisungen bei GitHub](https://github.com/Azure/WALinuxAgent "Anweisungen") aktualisieren.
* **Azure-Befehlszeilenschnittstelle**. Folgen Sie [dieser Anleitung zur CLI-Installation](../xplat-cli-install.md) , um die Azure-Befehlszeilenschnittstelle auf Ihrem Computer einzurichten. Sobald die Azure-Befehlszeilenschnittstelle installiert ist, können Sie über Ihre Befehlszeilenschnittstelle (Bash, Terminal oder Eingabeaufforderung) mithilfe des Befehls **azure** auf die Befehle der Azure-Befehlszeilenschnittstelle zugreifen. Beispiel:
  
  * Führen Sie **azure vm extension set --help** aus, um ausführliche Hilfeinformationen zu erhalten.
  * Führen Sie **azure login** aus, um sich bei Azure anzumelden.
  * Führen Sie **azure vm list** aus, um alle virtuellen Computer aufzuführen, über die Sie unter Azure verfügen.
* Ein Speicherkonto zum Speichern der Daten. Sie benötigen einen zuvor erstellten Speicherkontonamen und einen Zugriffsschlüssel zum Hochladen von Daten in Ihren Speicher.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Verwenden des Azure-CLI-Befehls zum Aktivieren der Linux-Diagnoseerweiterung
### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Szenario 1: Aktivieren der Erweiterung mit dem Standard-Dataset
Ab Version 2.3 werden standardmäßig folgende Daten gesammelt:

* Alle Rsyslog-Informationen (einschließlich System-, Sicherheits- und Anwendungsprotokolle).  
* Ein Kernsatz von grundlegenden Systemdaten. Eine Beschreibung aller Daten finden Sie auf der [System Center Cross Platform Solutions-Website](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Wenn Sie zusätzliche Daten aktivieren möchten, fahren Sie mit den Schritten in Szenario 2 und 3 fort.

Schritt 1: Erstellen Sie eine Datei namens „PrivateConf.json“ mit folgendem Inhalt:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

Schritt 2: Führen Sie **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json** aus.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Szenario 2: Anpassen der Leistungsüberwachungsmetriken
Dieser Abschnitt beschreibt, wie Sie die Tabelle mit den Leistungs- und Diagnosedaten anpassen.

Schritt 1: Erstellen Sie eine Datei mit dem Namen „PrivateConfig.json“ und dem Inhalt, der in Szenario 1 beschrieben wurde. Erstellen Sie auch eine Datei mit dem Namen „PublicConfig.json“. Geben Sie die Daten an, die Sie sammeln möchten.

Eine Übersicht über alle unterstützten Anbieter und Variablen finden Sie auf der [System Center Cross Platform Solutions-Website](https://scx.codeplex.com/wikipage?title=xplatproviders). Sie können mehrere Abfragen in mehreren Tabellen speichern, indem Sie weitere Abfragen an das Skript anhängen.

Rsyslog-Daten werden standardmäßig gesammelt.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Schritt 2: Führen Sie **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json** aus.

### <a name="scenario-3-upload-your-own-log-files"></a>Szenario 3: Hochladen eigener Protokolldateien
In diesem Abschnitt erfahren Sie, wie Sie bestimmte Protokolldateien sammeln und in Ihr Speicherkonto hochladen. Sie müssen sowohl den Pfad zur Ihrer Protokolldatei als auch den Namen der Tabelle angeben, in der Sie das Protokoll speichern möchten. Sie können mehrere Protokolldateien erstellen, indem Sie mehrere Datei-/Tabelleneinträge an das Skript anfügen.

Schritt 1: Erstellen Sie eine Datei mit dem Namen „PrivateConfig.json“ und dem Inhalt, der in Szenario 1 beschrieben wurde. Erstellen Sie anschließend eine weitere Datei mit dem Namen „PublicConfig.json“ mit folgendem Inhalt:

    {
        "fileCfg" :
        [
            {
                "file" : "/var/log/mysql.err",
                "table" : "mysqlerr"
             }
        ]
    }


Schritt 2: Führen Sie **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json** aus.

Beachten Sie, dass bei Verwendung dieser Einstellung in Erweiterungsversionen vor 2.3 unter Umständen alle in `/var/log/mysql.err` geschriebenen Protokolle in `/var/log/syslog` (oder `/var/log/messages`, abhängig von der Linux-Distribution) dupliziert werden. Zur Vermeidung dieser Doppelprotokollierung können Sie die Protokollierung von `local6`-Einrichtungsprotokollen in Ihrer rsyslog-Konfiguration ausschließen. Dies ist abhängig von Ihrer Linux-Distribution. Bei einem Ubuntu 14.04-System muss beispielsweise die Datei `/etc/rsyslog.d/50-default.conf` geändert werden: Hier können Sie die Zeile `*.*;auth,authpriv.none -/var/log/syslog` durch `*.*;auth,authpriv,local6.none -/var/log/syslog` ersetzen. Dieses Problem wurde in der neuesten Hotfixversion 2.3 (2.3.9007) behoben. Wenn Sie also die Erweiterungsversion 2.3 nutzen, sollte dieses Problem nicht auftreten. Falls es auch nach dem Neustart Ihres virtuellen Computers auftritt, kontaktieren Sie uns, und helfen Sie uns dabei herauszufinden, warum die neueste Hotfixversion nicht automatisch installiert wird.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Szenario 4: Verhindern, dass die Erweiterung Protokolle sammelt
In diesem Abschnitt wird beschrieben, wie Sie verhindern, dass die Erweiterung Protokolle sammelt. Beachten Sie, dass der Überwachungs-Agent-Prozess auch nach dieser Neukonfiguration weiterhin ausgeführt wird. Deaktivieren Sie die Erweiterung, wenn Sie den Überwachungs-Agent-Prozess vollständig beenden möchten. Der Befehl zum Deaktivieren der Erweiterung lautet **azure vm extension set --disable <VM-Name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'**.

Schritt 1: Erstellen Sie eine Datei mit dem Namen „PrivateConfig.json“ und dem Inhalt, der in Szenario 1 beschrieben wurde. Erstellen Sie eine weitere Datei mit dem Namen „PublicConfig.json“ und folgendem Inhalt:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Schritt 2: Führen Sie **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json** aus.

## <a name="review-your-data"></a>Überprüfen der Daten
Die Leistungs- und Diagnosedaten werden in einer Azure Storage-Tabelle gespeichert. Unter [Verwenden des Azure-Tabellenspeichers mit Ruby](../storage/storage-ruby-how-to-use-table-storage.md) erfahren Sie, wie Sie mit Azure-CLI-Skripts auf die Daten in der Speichertabelle zugreifen.

Darüber hinaus können Sie folgende Tools mit grafischer Benutzeroberfläche für den Datenzugriff verwenden:

1. Server-Explorer von Visual Studio Wechseln Sie zum Speicherkonto. Nachdem die VM etwa fünf Minuten lang ausgeführt wurde, werden die vier Standardtabellen angezeigt: „LinuxCpu“, „LinuxDisk“, „LinuxMemory“ und „Linuxsyslog“. Doppelklicken Sie auf den Tabellennamen, um die Daten anzuzeigen.
2. [Azure-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/ "Azure-Speicher-Explorer")angegeben sind

![image](./media/virtual-machines-linux-classic-diagnostic-extension/no1.png)

Wenn Sie fileCfg oder perfCfg aktiviert haben (wie in Szenario 2 und 3 beschrieben), können Sie den Speicher-Explorer von Visual Studio und Azure-Speicher-Explorer verwenden, um nicht standardmäßige Daten anzuzeigen.

## <a name="known-issues"></a>Bekannte Probleme
* In der aktuellen Version (2.3) der Linux-Diagnoseerweiterung kann auf die Rsyslog-Informationen und die benutzerdefinierte Protokolldatei nur per Skript zugegriffen werden.




<!--HONumber=Feb17_HO3-->


