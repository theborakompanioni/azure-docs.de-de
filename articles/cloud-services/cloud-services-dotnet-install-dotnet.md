---
title: Installieren von .NET in einer Clouddienstrolle | Microsoft Docs
description: Dieser Artikel beschreibt die manuelle Installation eines .NET Frameworks in Clouddienstweb- und Workerrollen.
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 0ac8ca0c5407925728ed0431294a3234b58d6e63
ms.openlocfilehash: 04506596ba21c3ebef7237eaad8c5d786ad672fe
ms.lasthandoff: 02/27/2017

---

# <a name="install-net-on-a-cloud-service-role"></a>Installieren von .NET in einer Clouddienstrolle
Dieser Artikel beschreibt die Installation von .NET Framework-Versionen auf Clouddienst-Web- und Workerrollen, die nicht im Gast-BS enthalten sind. Mit diesen Schritten können Sie z.B. .NET 4.6.1 für die Azure-Gast-BS-Familie 4 installieren, die keine Version von .NET 4.6 enthält. Aktuelle Informationen zu Gast-BS-Versionen finden Sie unter [Neuigkeiten zur Version des Azure-Gastbetriebssystems](cloud-services-guestos-update-matrix.md).

>[!NOTE]
>Gast-BS 5 umfasst .NET 4.6.

>[!IMPORTANT]
>Azure SDK 2.9 enthält eine Einschränkung für die Bereitstellung von .NET 4.6 auf Gast-BS 4 oder früher. Eine Korrektur ist [hier](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) verfügbar.

Die Installation von .NET auf ihren Web- und Workerrollen schließt die Erfassung des .NET Installationspakets als Teil Ihres Cloudprojekts und außerdem das Starten des Installationsprogramms als Teil der Startaufgaben der Rolle ein.  

## <a name="add-the-net-installer-to-your-project"></a>Das Installationsprogramm für .NET zu Ihrem Projekt hinzufügen
* Laden Sie das Webinstallationsprogramm für das .NET Framework herunter, das Sie installieren möchten
  * [Webinstallationsprogramm für .NET 4.6.1](http://go.microsoft.com/fwlink/?LinkId=671729)
* Für eine Webrolle
  1. Klicken Sie im **Projektmappen-Explorer** unter **Rollen** im Clouddienstprojekt mit der rechten Maustaste auf Ihre Rolle, und wählen Sie **Hinzufügen > Neuer Ordner** aus. Erstellen Sie einen Ordner mit dem Namen *bin*
  2. Klicken Sie mit der rechten Maustaste auf den Ordner **bin**, und wählen Sie **Hinzufügen > Vorhandenes Element** aus. Wählen Sie das Installationsprogramm für .NET aus und fügen Sie es dem Ordner "Bin" hinzu.
* Für eine Workerrolle
  1. Klicken Sie mit der rechten Maustaste auf Ihre Rolle, und wählen Sie **Hinzufügen > Vorhandenes Element** aus. Wählen Sie das Installationsprogramm für .NET aus und fügen Sie zur Rolle hinzu. 

Dateien, die auf diese Weise dem Inhaltsordner "Rolle" hinzugefügt werden, werden automatisch dem Clouddienstpaket hinzugefügt und an einem einheitlichen Ort auf dem virtuellen Computer bereitgestellt. Wiederholen Sie diesen Vorgang für alle Web- und Workerrollen in Ihrem Clouddienst, sodass alle Rollen über eine Kopie des Installationsprogramms verfügen.

> [!NOTE]
> Installieren Sie .NET 4.6.1 für Ihre Clouddienstrolle, selbst wenn Ihre Anwendung .NET 4.6 verwendet. Das Azure-Gast-BS enthält die Updates [3098779](https://support.microsoft.com/kb/3098779) und [3097997](https://support.microsoft.com/kb/3097997). Die Installation von .NET 4.6 zusätzlich zu diesen Updates kann zu Problemen bei der Ausführung Ihrer .NET-Anwendungen führen, weshalb Sie direkt .NET 4.6.1 anstelle von .NET 4.6 installieren sollten. Weitere Informationen finden Sie unter [KB 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Inhalte der Rolle mit Installer-Dateien][1]

## <a name="define-startup-tasks-for-your-roles"></a>Definieren von Startaufgaben für Ihre Rollen
Mit Startaufgaben können Sie Vorgänge ausführen, bevor eine Rolle gestartet wird. Die Installation von .NET Framework als Teil der Startaufgabe stellt sicher, dass das Framework installiert ist, bevor Ihr Anwendungscode ausgeführt wird. Weitere Informationen zu Startaufgaben finden Sie unter: [Startaufgaben in Azure ausführen](cloud-services-startup-tasks.md). 

1. Fügen Sie der Datei *ServiceDefinition.csdef* unter den Knoten **Webrolle** oder **Workerrolle** für alle Rollen Folgendes hinzu:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    Die oben genannte Konfiguration führt den Konsolenbefehl *install.cmd* mit Administratorrechten aus, damit dieser das .NET Framework installieren kann. Über die Konfiguration wird auch ein LocalStorage mit dem Namen *NETFXInstall*erstellt. Das Startskript stellt das temporäre Verzeichnis so ein, dass diese lokale Speicherressource genutzt wird, damit das Installationsprogramm für .NET-Framework von dieser Ressource heruntergeladen und installiert wird. Es ist wichtig, die Größe dieser Ressource auf mindestens 1.024 MB einzustellen, damit das Framework ordnungsgemäß installiert werden kann. Weitere Informationen zu Startaufgaben finden Sie unter [Allgemeine Startaufgaben für Clouddienste](cloud-services-startup-tasks-common.md) 
2. Erstellen Sie die Datei **install.cmd**, und fügen Sie sie allen Rollen hinzu, indem mit der rechten Maustaste auf die Rolle klicken und **Hinzufügen > Vorhandenes Element...** auswählen. Nun sollten alle Rollen über die .NET Installerdateien sowie die Datei install.cmd verfügen.
   
    ![Inhalte der Rolle mit allen Dateien][2]
   
   > [!NOTE]
   > Verwenden Sie einen einfachen Texteditor wie Notepad, um diese Datei zu erstellen. Wenn Sie Visual Studio verwenden, um eine Textdatei erstellen und diese dann in  ".cmd" umbenennen, enthält die Datei möglicherweise weiterhin eine UTF-8-Bytereihenfolge-Markierung und das Ausführen der ersten Zeile des Skripts führt zu einem Fehler. Verwenden Sie Visual Studio, um die Datei zu erstellen, fügen Sie eine REM (Anmerkung) zur ersten Zeile der Datei hinzu, damit diese bei der Ausführung ignoriert wird. 
   > 
   > 
3. Fügen Sie das folgende Skript aus der **install.cmd** -Datei hinzu:
   
    ```cmd
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
   
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
   
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
   
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
   
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
   
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
   
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
   
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
   
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
   
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
   
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
   
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
   
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
   
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
   
    :exit
    EXIT /B 0
    ```
   
    Das Installationsskript überprüft, ob die angegebene .NET-Framework-Version bereits auf dem Computer installiert ist, indem die Registrierungsdatenbank abgefragt wird. Wenn die .NET-Version nicht installiert ist, wird das .NET-Webinstallationsprogramm gestartet. Zur Unterstützung bei der Problembehandlung protokolliert das Skript alle Aktivitäten in einer Datei mit dem Namen *startuptasklog-(currentdatetime).txt*, die unter *InstallLogs* lokal gespeichert ist.
   
   > [!NOTE]
   > Das Skript zeigt aus Gründen der Kontinuität immer noch die Installation von .NET 4.5.2 oder .NET 4.6. Es ist nicht erforderlich, .NET 4.5.2 manuell zu installieren, da es bereits für das Azure-Gastbetriebssystem verfügbar ist. Anstatt .NET 4.6 zu installieren, sollten Sie gemäß [KB 3118750](https://support.microsoft.com/kb/3118750).NET 4.6.1 installieren.
   > 
   > 

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfigurieren der Diagnose zur Übertragung der Startaufgabenprotokolle in Blobspeicher
Zur Vereinfachung der Problembehandlung können Sie die Azure-Diagnose so konfigurieren, dass alle Protokolldateien, die durch das Startskript oder Installationsprogramm für .NET erstellt wurden, in Blobspeicher übertragen werden. Bei diesem Ansatz können Sie die Protokolle anzeigen, indem Sie einfach die Protokolldateien aus dem Blobspeicher herunterladen, anstatt sich per Remotedesktop mit der Rolle anzumelden.

Öffnen Sie zur Konfiguration der Diagnose *diagnostics.wadcfgx* und fügen Sie Folgendes unter dem Knoten **Verzeichnisse** hinzu: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Dadurch wird die Azure-Diagnose zur Übertragung aller Dateien im Verzeichnis *log* unter der Ressource *NETFXInstall* in das Diagnose-Speicherkonto des Blobcontainers *netfx-install* konfiguriert.

## <a name="deploying-your-service"></a>Ihren Dienst bereitstellen
Wenn Sie Ihren Dienst bereitstellen, wird die Startaufgabe ausgeführt und das .NET Framework installiert, sofern es nicht bereits installiert ist. Ihre Rollen befinden sich im Status "Beschäftigt", während das Framework installiert wird und werden möglicherweise sogar neu gestartet, wenn die Framework-Installation dies erforderlich macht. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Installieren von .NET Framework][Installing the .NET Framework]
* [Ermitteln der installierten .NET Framework-Versionen][How to: Determine Which .NET Framework Versions Are Installed]
* [Beheben von Problemen bei .NET Framework-Installationen][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing the .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png



