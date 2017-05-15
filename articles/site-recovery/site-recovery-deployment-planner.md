---
title: "Azure Site Recovery Deployment Planner für „VMware zu Azure“ | Microsoft-Dokumentation"
description: Dies ist ein Leitfaden zum Azure Site Recovery Deployment Planner.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 2/21/2017
ms.author: nisoneji
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 5c716069bdff2a23bf81b2d2d0793a8616cf9c83
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Deployment Planner
Dieser Artikel ist der Leitfaden zum Deployment Planner (Bereitstellungsplaner) von Azure Site Recovery für Bereitstellungen von „VMware zu Azure“ in der Produktion.

## <a name="overview"></a>Übersicht

Bevor Sie mit dem Schützen von virtuellen VMware-Computern (VMs) mit Site Recovery beginnen, ist es ratsam, basierend auf Ihrer täglichen Datenänderungsrate genügend Bandbreite zuzuordnen, um den gewünschten RPO-Wert (Recovery Point Objective) zu erzielen. Achten Sie darauf, dass Sie lokal die richtige Anzahl von Konfigurationsservern und Prozessservern bereitstellen.

Außerdem ist es erforderlich, den richtigen Typ und die richtige Anzahl von Azure-Zielspeicherkonten zu erstellen. Sie erstellen entweder Standard- oder Storage Premium-Konten und berücksichtigen dabei für Ihre Quellproduktionsserver den Zuwachs, der sich im Laufe der Zeit durch eine vermehrte Nutzung ergeben kann. Sie wählen den Speichertyp pro VM basierend auf den Workloadmerkmalen (z.B. Lese/Schreib-E/A-Vorgänge pro Sekunde [IOPS] oder Datenänderung) und den Site Recovery-Grenzwerten aus.

Bei der öffentlichen Vorschauversion des Site Recovery Deployment Planner handelt es sich um ein Befehlszeilentool, das derzeit nur für das Szenario „VMware zu Azure“ verfügbar ist. Sie können mit diesem Tool per Remoteverbindung ein Profil für Ihre virtuellen VMware-Computer erstellen (ohne jegliche Auswirkung auf die Produktion), um die Anforderungen an die Bandbreite und Azure Storage zu ermitteln und für einen erfolgreichen Ablauf der Replikation und des Testfailovers zu sorgen. Das Tool kann ausgeführt werden, ohne dass lokal Site Recovery-Komponenten installiert werden müssen. Damit Sie ein genaues Ergebnis für den erzielten Durchsatz erhalten, empfehlen wir Ihnen Folgendes: Führen Sie den Planner auf einem Windows Server aus, für den die Mindestanforderungen des Site Recovery-Konfigurationsservers erfüllt werden, den Sie später in einem der ersten Schritte für die Produktion bereitstellen.

Das Tool umfasst die folgenden Details:

**Kompatibilitätsbewertung**

* Bewertung der Eignung von virtuellen Computern basierend auf Datenträgeranzahl, Datenträgergröße, IOPS, Änderungsrate und Starttyp (EFI/BIOS)
* Geschätzte erforderliche Netzwerkbandbreite für die Deltareplikation

**Vergleich von erforderlicher Netzwerkbandbreite und RPO-Bewertung**

* Geschätzte erforderliche Netzwerkbandbreite für die Deltareplikation
* Durchsatz für Site Recovery vom lokalen Standort zu Azure
* Anzahl von VMs für die Batcherstellung basierend auf der geschätzten Bandbreite zur Durchführung der ersten Replikation innerhalb eines bestimmten Zeitraums

**Azure-Infrastrukturanforderungen**

* Speichertypanforderung (Standard- oder Storage Premium-Konto) für jede VM
* Gesamtzahl von Standard- und Storage Premium-Konten zur Einrichtung für die Replikation
* Vorschläge für die Benennung von Speicherkonten gemäß Azure Storage-Vorgabe
* Anordnung von Speicherkonten für alle VMs
* Anzahl von einzurichtenden Azure-Kernen vor dem Testfailover oder Failover für das Abonnement
* Empfohlene Azure-VM-Größe für jede lokale VM

**Anforderungen an die lokale Infrastruktur**
* Erforderliche Anzahl von Konfigurationsservern und Prozessservern für die lokale Bereitstellung

>[!IMPORTANT]
>
>Da die Nutzung m Laufe der Zeit voraussichtlich zunehmen wird, werden alle vorherigen Toolberechnungen mit einem Zuwachsfaktor von 30 Prozent in Bezug auf die Workloadmerkmale durchgeführt, und für alle Profilerstellungsmetriken (Lese/Schreib-IOPS, Datenänderung usw.) wird das 95. Perzentil verwendet. Beide Elemente (für die Berechnung des Zuwachsfaktors und des Perzentils) sind konfigurierbar. Weitere Informationen zum Zuwachsfaktor finden Sie im Abschnitt „Informationen zum Zuwachsfaktor“. Weitere Informationen zum Perzentilwert finden Sie im Abschnitt „Für die Berechnung verwendeter Perzentilwert“.
>

## <a name="requirements"></a>Anforderungen
Das Tool verfügt über zwei Hauptphasen: die Profilerstellung und die Berichterstellung. Es gibt auch noch eine dritte Option, mit der nur der Durchsatz berechnet werden kann. Die Anforderungen für den Server, über den die Profilerstellung und Durchsatzmessung initiiert werden, sind in der folgenden Tabelle aufgeführt:

| Serveranforderung | Beschreibung|
|---|---|
|Profilerstellung und Messung des Durchsatzes| <ul><li>Betriebssystem: Microsoft Windows Server 2012 R2<br>(idealerweise mindestens basierend auf [Empfohlene Größen für den Konfigurationsserver](https://aka.ms/asr-v2a-on-prem-components))</li><li>Computerkonfiguration: 8 vCPUs, 16 GB RAM, 300 GB HDD</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)</li><li>[Microsoft Visual C++ Redistributable für Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Zugriff auf Azure über das Internet von diesem Server</li><li>Azure-Speicherkonto</li><li>Administratorzugriff auf dem Server</li><li>Mindestens 100 GB freier Speicherplatz (bei 1.000 VMs mit durchschnittlich drei Datenträgern, Profilerstellung über 30 Tage)</li><li>Die Einstellungen für die VMware vCenter-Statistikebene sollten auf „2“ oder „Höchste Stufe“ festgelegt werden.</li></ul>|
| Berichterstellung | Windows-PC oder Windows-Server mit Microsoft Excel 2013 oder höher |
| Benutzerberechtigungen | Leseberechtigung für das Benutzerkonto, das zum Zugreifen auf den VMware vCenter-Server/VMware vSphere ESXi-Host während der Profilerstellung verwendet wird |

> [!NOTE]
>
>Mit dem Tool können nur Profile für VMs mit VMDK- und RDM-Datenträgern erstellt werden. Die Profilerstellung für VMs mit iSCSI- oder NFS-Datenträgern ist nicht möglich. Site Recovery unterstützt zwar iSCSI- und NFS-Datenträger für VMware-Server, aber da sich der Deployment Planner nicht auf dem Gast befindet und die Profilerstellung nur mithilfe von vCenter-Leistungsindikatoren durchgeführt wird, hat das Tool keinen Einblick in diese Datenträgertypen.
>

## <a name="download-and-extract-the-public-preview"></a>Herunterladen und Extrahieren der öffentlichen Vorschauversion
1. [Laden Sie die aktuelle Version des Site Recovery Deployment Planner (öffentliche Vorschauversion) herunter.](https://aka.ms/asr-deployment-planner)  
Das Tool ist in einem ZIP-Ordner enthalten. Die aktuelle Version des Tools unterstützt nur das Szenario „VMware zu Azure“.

2. Kopieren Sie den ZIP-Ordner auf den Windows-Server, auf dem Sie das Tool ausführen möchten.  
Sie können das Tool unter Windows Server 2012 R2 ausführen, wenn der Server Netzwerkzugriff hat, um eine Verbindung mit dem vCenter-Server/vSphere ESXi-Host herzustellen, der die VMs für die Profilerstellung enthält. Wir empfehlen Ihnen aber, das Tool auf einem Server auszuführen, dessen Hardwarekonfiguration die Anforderungen der [Größenrichtlinie für Konfigurationsserver](https://aka.ms/asr-v2a-on-prem-components) erfüllt. Führen Sie das Tool auf dem Konfigurationsserver aus, falls Sie Site Recovery-Komponenten bereits lokal bereitgestellt haben.

 Es wird empfohlen, auf dem Server, auf dem Sie das Tool ausführen, die gleiche Hardwarekonfiguration wie auf dem Konfigurationsserver (der über einen integrierten Prozessserver verfügt) zu verwenden. Mit dieser Konfiguration wird sichergestellt, das der vom Tool gemeldete erzielte Durchsatz mit dem tatsächlichen Durchsatz übereinstimmt, den Site Recovery während der Replikation erreichen kann. Die Berechnung des Durchsatzes richtet sich nach der verfügbaren Netzwerkbandbreite auf dem Server und der Hardwarekonfiguration (CPU, Speicher usw.) des Servers. Wenn Sie das Tool auf einem anderen Server ausführen, wird der Durchsatz von diesem Server zu Microsoft Azure berechnet. Da sich die Hardwarekonfiguration des Servers von der des Konfigurationsservers unterscheiden kann, ist es auch möglich, dass der vom Tool gemeldete Durchsatz ungenau ist.

3. Extrahieren Sie den ZIP-Ordner.  
Der Ordner enthält mehrere Dateien und Unterordner. Die ausführbare Datei ist die Datei „ASRDeploymentPlanner.exe“ im übergeordneten Ordner.

    Beispiel:  
    Kopieren Sie die ZIP-Datei auf das Laufwerk „E:\“, und extrahieren Sie sie.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Funktionen
Sie können das Befehlszeilentool (ASRDeploymentPlanner.exe) in einem der folgenden drei Modi ausführen:

1. Profilerstellung  
2. Berichterstellung
3. Durchsatzberechnung

Führen Sie das Tool zuerst im Modus für die Profilerstellung aus, um für die VM die Datenänderung und den IOPS-Wert zu erfassen. Führen Sie im Tool anschließend die Berichterstellung durch, um die Netzwerkbandbreite und die Speicheranforderungen zu ermitteln.

## <a name="profiling"></a>Profilerstellung
Im Profilerstellungsmodus stellt das Deployment Planner-Tool eine Verbindung mit dem vCenter Server bzw. vSphere ESXi-Host her, um Leistungsdaten zur VM zu sammeln.

* Die Profilerstellung wirkt sich nicht negativ auf die Leistung der Produktions-VMs aus, weil keine direkte Verbindung damit hergestellt wird. Alle Leistungsdaten werden über den vCenter-Server/vSphere ESXi-Host ermittelt.
* Um sicherzustellen, dass die Auswirkungen der Profilerstellung auf den Server vernachlässigbar sind, fragt das Tool den vCenter-Server/vSphere ESXi-Host alle 15 Minuten ab. Die Genauigkeit der Profilerstellung wird durch dieses Abfrageintervall nicht beeinträchtigt, weil die Leistungsindikatordaten vom Tool minutengenau gespeichert werden.

### <a name="create-a-list-of-vms-to-profile"></a>Erstellen einer Liste mit VMs für die Profilerstellung
Zuerst benötigen Sie eine Liste mit den VMs, für die die Profilerstellung durchgeführt werden soll. Sie können alle Namen von VMs auf einem vCenter-Server/vSphere ESXi-Host abrufen, indem Sie im Rahmen des folgenden Verfahrens die VMware vSphere PowerCLI-Befehle verwenden. Alternativ dazu können Sie die Anzeigenamen oder IP-Adressen der VMs, für die die Profilerstellung durchgeführt werden soll, auch manuell in einer Datei auflisten.

1. Melden Sie sich an der VM an, auf der VMware vSphere PowerCLI installiert ist.
2. Öffnen Sie die VMware vSphere PowerCLI-Konsole.
3. Stellen Sie sicher, dass die Ausführungsrichtlinie für das Skript aktiviert ist. Wenn sie deaktiviert ist, können Sie die VMware vSphere PowerCLI-Konsole im Administratormodus starten und dann aktivieren, indem Sie den folgenden Befehl ausführen:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Zum Abrufen aller Namen von VMs eines vCenter-Servers/vSphere ESXi-Hosts und Speichern der Liste in einer TXT-Datei führen Sie die beiden hier angegebenen Befehle aus.
Ersetzen Sie &lsaquo;server name&rsaquo;, &lsaquo;user name&rsaquo;, &lsaquo;password&rsaquo; und &lsaquo;outputfile.txt&rsaquo; durch Ihre Angaben:

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

5. Öffnen Sie die Ausgabedatei im Editor, und kopieren Sie die Namen aller VMs, für die Profile erstellt werden sollen, in eine andere Datei (z.B. „ProfileVMList.txt“). Fügen Sie einen VM-Namen pro Zeile ein. Diese Datei wird als Eingabe für den Parameter *-VMListFile* des Befehlszeilentools verwendet.

    ![Liste mit VM-Namen im Deployment Planner](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Starten der Profilerstellung
Nachdem Sie die Liste mit den VMs für die Profilerstellung vorliegen haben, können Sie für das Tool den Modus für die Profilerstellung ausführen. Unten ist die Liste mit den obligatorischen und optionalen Parametern des Tools für die Ausführung im Modus für die Profilerstellung angegeben.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Parametername | Beschreibung |
|---|---|
| -Operation | StartProfiling |
| -Server | Der vollständig qualifizierte Domänenname oder die IP-Adresse des vCenter-Servers/vSphere ESXi-Hosts, für dessen VMs die Profilerstellung durchgeführt werden soll.|
| -User | Der Benutzername für die Herstellung der Verbindung mit dem vCenter-Server/vSphere ESXi-Host. Der Benutzer muss mindestens über Lesezugriff verfügen.|
| -VMListFile |    Die Datei mit der Liste der VMs, für die Profile erstellt werden sollen. Der Dateipfad kann absolut oder relativ sein. Die Datei sollte einen VM-Namen bzw. eine IP-Adresse pro Zeile enthalten. Der in der Datei angegebene Name des virtuellen Computers sollte mit dem VM-Namen auf dem vCenter-Server/vSphere ESXi-Host identisch sein.<br>Die Datei „VMList.txt“ enthält beispielsweise die folgenden VMs:<ul><li>virtual_machine_A</li><li>10.150.29.110</li><li>virtual_machine_B</li><ul> |
| -NoOfDaysToProfile | Gibt an, wie lange die Profilerstellung durchgeführt werden soll (in Tagen). Es wird empfohlen, die Profilerstellung länger als 15 Tage durchzuführen. So kann sichergestellt werden, dass das Workloadmuster in Ihrer Umgebung im angegebenen Zeitraum eingehalten und verwendet wird, um eine genaue Empfehlung zu erhalten. |
| -Directory | (Optional) Der UNC-Pfad (Universal Naming Convention) oder lokale Verzeichnispfad zum Speichern der Daten, die während des Profilerstellungsvorgangs generiert werden. Wenn kein Verzeichnisname angegeben ist, wird das Verzeichnis mit dem Namen „ProfiledData“ unter dem aktuellen Pfad als Standardverzeichnis verwendet. |
| -Password | (Optional) Das Kennwort zum Herstellen der Verbindung mit dem vCenter-Server/vSphere ESXi-Host. Wenn Sie hier kein Kennwort angeben, werden Sie bei der Ausführung des Befehls zur Eingabe aufgefordert.|
| -StorageAccountName | (Optional) Der Name des Speicherkontos zur Ermittlung des Durchsatzes, der für die Replikation von Daten aus der lokalen Umgebung in Azure erreichbar ist. Mit dem Tool werden Testdaten in dieses Speicherkonto hochgeladen, um den Durchsatz zu berechnen.|
| -StorageAccountKey | (Optional) Der Schlüssel des Speicherkontos, der zum Zugreifen auf das Speicherkonto verwendet wird. Navigieren Sie im Azure-Portal zu „Speicherkonten“ > <*Name des Speicherkontos*> > „Einstellungen“ > „Zugriffsschlüssel“ > „Key1“ (oder primärer Zugriffsschlüssel für das klassische Speicherkonto). |
| -Environment | (optional) Dies ist Ihre Azure Storage-Zielkontoumgebung. Diese drei Werte sind möglich: „AzureCloud“, „AzureUSGovernment“ und „AzureChinaCloud“. Der Standardwert ist „AzureCloud“. Verwenden Sie den Parameter, wenn Sie als Azure-Zielregion Clouds vom Typ „Azure US Government“ oder „Azure China“ verwenden. |


Es wird empfohlen, die Profilerstellung für Ihre VMs mindestens 15 bis 30 Tage lang durchzuführen. Während des Zeitraums der Profilerstellung wird „ASRDeploymentPlanner.exe“ weiter ausgeführt. Im Tool wird der Zeitraum für die Profilerstellung in Tagen eingegeben. Wenn Sie die Profilerstellung nur einige Stunden oder Minuten lang durchführen möchten, um das Tool zu testen, müssen Sie in der öffentlichen Vorschauversion den Zeitraum in den entsprechenden Tageswert umrechnen. Wenn die Profilerstellung beispielsweise 30 Minuten dauern soll, muss die Eingabe 30/(60·24) = 0,021 Tage lauten. Die kürzeste zulässige Dauer für die Profilerstellung beträgt 30 Minuten.

Während der Profilerstellung können Sie optional den Namen eines Speicherkontos und den dazugehörigen Schlüssel übergeben, um den Durchsatz zu ermitteln, der für Site Recovery bei der Replikation vom Konfigurationsserver oder Prozessserver zu Azure erreicht werden kann. Wenn der Name des Speicherkontos und der Schlüssel während der Profilerstellung nicht übergeben werden, wird der erreichbare Durchsatz vom Tool nicht berechnet.

Sie können mehrere Instanzen des Tools für verschiedene Gruppen von VMs ausführen. Stellen Sie sicher, dass die VM-Namen in den Gruppen für die Profilerstellung nicht mehr als einmal vorkommen. Wenn Sie beispielsweise Profile für zehn VMs (VM1 bis VM10) erstellt haben und nach einigen Tagen Profile für fünf weitere VMs (VM11 bis VM15) erstellen möchten, können Sie das Tool für die zweite Gruppe von VMs (VM11 bis VM15) über eine andere Befehlszeilenkonsole ausführen. Stellen Sie hierbei sicher, dass die zweite Gruppe von VMs keine Namen der VMs aus der ersten Profilerstellungsinstanz enthält, oder verwenden Sie für die zweite Ausführung ein anderes Ausgabeverzeichnis. Wenn zwei Instanzen des Tools für die Profilerstellung derselben VMs verwendet werden und dabei dasselbe Ausgabeverzeichnis genutzt wird, ist der generierte Bericht fehlerhaft.

VM-Konfigurationen werden zu Beginn des Profilerstellungsvorgangs einmal erfasst und in einer Datei mit dem Namen „VMDetailList.xml“ gespeichert. Diese Informationen werden für die Berichterstellung verwendet. Alle Änderungen der VM-Konfiguration (z.B. erhöhte Anzahl von Kernen, Datenträgern oder NICs) vom Anfang bis zum Ende der Profilerstellung werden nicht erfasst. Wenn sich eine betroffene VM-Konfiguration während der Profilerstellung geändert hat, können Sie dieses Problem für die öffentliche Vorschauversion wie folgt umgehen, um bei der Berichterstellung die aktuellen VM-Details zu erhalten:

* Sichern Sie die Datei „VMdetailList.xml“, und löschen Sie die Datei an ihrem aktuellen Speicherort.
* Übergeben Sie die Argumente „-User“ und „-Password“ während der Berichterstellung.

Mit dem Befehl für die Profilerstellung werden im Verzeichnis der Profilerstellung mehrere Dateien generiert. Löschen Sie keine Dateien, weil sich dies auf die Berichterstellung auswirkt.

#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Beispiel 1: VM-Profilerstellung für 30 Tage, Ermittlung des Durchsatzes von lokal zu Azure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Beispiel 2: VM-Profilerstellung für 15 Tage

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-the-tool"></a>Beispiel 3: VM-Profilerstellung für 1 Stunde zum schnellen Testen des Tools
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Wenn der Server, auf dem das Tool ausgeführt wird, neu gestartet wird oder abstürzt oder wenn Sie das Tool mit STRG+C schließen, werden die Profilerstellungsdaten beibehalten. Es kann aber sein, dass die letzten 15 Minuten der Daten für die Profilerstellung fehlen. Führen Sie das Tool in diesem Fall im Profilerstellungsmodus aus, nachdem der Server neu gestartet wurde.
>* Wenn der Name des Speicherkontos und der dazugehörige Schlüssel übergeben werden, misst das Tool den Durchsatz im letzten Schritt der Profilerstellung. Falls das Tool geschlossen wird, bevor die Profilerstellung abgeschlossen ist, wird der Durchsatz nicht berechnet. Zur Ermittlung des Durchsatzes vor dem Generieren des Berichts können Sie den GetThroughput-Vorgang über die Befehlszeilenkonsole ausführen. Andernfalls sind die Informationen zum Durchsatz nicht im Bericht enthalten.


## <a name="generate-a-report"></a>Generieren eines Berichts
Das Tool generiert eine makrofähige Microsoft Excel-Datei (XLSM-Datei) als Berichtsausgabe, in der alle Empfehlungen für die Bereitstellung zusammengefasst sind. Der Bericht hat den Namen „DeploymentPlannerReport_<*eindeutiger numerischer Bezeichner*>.xlsm“ und wird im angegebenen Verzeichnis gespeichert.

Nach Abschluss der Profilerstellung können Sie das Tool im Berichterstellungsmodus ausführen. Die folgende Tabelle enthält eine Liste mit den erforderlichen und optionalen Toolparametern für die Ausführung im Modus für die Berichterstellung.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Parametername | Beschreibung |
|-|-|
| -Operation | GenerateReport |
| -Server |  Der vollqualifizierte Name oder die IP-Adresse (der gleiche Name bzw. die gleiche IP-Adresse wie bei der Profilerstellung) des vCenter-/vSphere-Servers, auf dem sich die VMs für die Profilerstellung befinden, für die der Bericht erstellt werden soll. Beachten Sie Folgendes: Wenn Sie bei der Profilerstellung einen vCenter-Server verwendet haben, können Sie keinen vSphere-Server für die Berichterstellung verwenden (und umgekehrt).|
| -VMListFile | Die Datei mit der Liste mit den VMs für die Profilerstellung, für die der Bericht erstellt werden soll. Der Dateipfad kann absolut oder relativ sein. Die Datei sollte einen VM-Namen bzw. eine IP-Adresse pro Zeile enthalten. Die VM-Namen, die in der Datei angegeben werden, sollten den VM-Namen auf dem vCenter-Server/vSphere ESXi-Host entsprechen und mit den Angaben für die Profilerstellung übereinstimmen.|
| -Directory | (Optional) Der UNC- oder lokale Verzeichnispfad des Speicherorts, an dem die Daten der Profilerstellung (während der Profilerstellung generierte Dateien) gespeichert werden. Diese Daten werden für die Erstellung des Berichts benötigt. Wenn kein Name vorhanden ist, wird das Verzeichnis „ProfiledData“ verwendet. |
| -GoalToCompleteIR | (Optional) Die Anzahl von Stunden, in denen die erste Replikation der VMs für die Profilerstellung abgeschlossen werden muss. Im erstellten Bericht ist die Anzahl von VMs enthalten, für die die erste Replikation im angegebenen Zeitraum abgeschlossen werden kann. Die Standardeinstellung beträgt 72 Stunden. |
| -User | (Optional) Der Benutzername für die Herstellung der Verbindung mit dem vCenter-/vSphere-Server. Der Name dient zum Abrufen der aktuellen Konfigurationsinformationen der VMs, z.B. Anzahl von Datenträgern, Anzahl von Kernen und Anzahl von NICs, für die Verwendung im Bericht. Wenn der Name nicht angegeben wird, werden die Konfigurationsinformationen verwendet, die zu Beginn der Profilerstellung erfasst wurden. |
| -Password | (Optional) Das Kennwort zum Herstellen der Verbindung mit dem vCenter-Server/vSphere ESXi-Host. Wenn das Kennwort nicht als Parameter angegeben ist, werden Sie später bei der Ausführung des Befehls zum Eingeben aufgefordert. |
| -DesiredRPO | (Optional) Der gewünschte RPO-Wert (Recovery Point Objective) in Minuten. Der Standardwert ist 15 Minuten.|
| -Bandwidth | Bandbreite in MBit/s. Der Parameter wird zum Berechnen des RPO-Werts verwendet, der für die angegebene Bandbreite erzielt werden kann. |
| -StartDate | (Optional) Das Startdatum und die Uhrzeit im Format MM-TT-JJJJ:HH:MM (24-Stunden-Format). *StartDate* muss zusammen mit *EndDate* angegeben werden. Wenn „StartDate“ angegeben ist, wird der Bericht für die Profilerstellungsdaten erstellt, die zwischen „StartDate“ und „EndDate“ erfasst wurden. |
| -EndDate | (Optional) Das Enddatum und die Uhrzeit im Format MM-TT-JJJJ:HH:MM (24-Stunden-Format). *EndDate* muss zusammen mit *StartDate* angegeben werden. Wenn „EndDate“ angegeben ist, wird der Bericht für die Profilerstellungsdaten erstellt, die zwischen „StartDate“ und „EndDate“ erfasst wurden. |
| -GrowthFactor | (Optional) Der Zuwachsfaktor als Prozentsatz. Der Standardwert ist 30 Prozent. |
| -UseManagedDisks | (Optional) UseManagedDisks – Ja/Nein. Die Standardeinstellung ist „Ja“. Die Anzahl der virtuellen Computer, die in einem einzelnen Speicherkonto enthalten sein können, wird basierend auf der Auswahl eines verwalteten Datenträger für das Failover bzw. Testfailover berechnet. |

in einer einzelnen Speicherkontoplatzierung wird bei der Berechnung berücksichtigt, dass das Failover bzw. Testfailover von virtuellen Computern auf einem verwalteten Datenträger anstatt einem nicht verwalteten Datenträger erfolgt. |


#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Beispiel 1: Berichterstellung mit Standardwerten, wenn sich die Profilerstellungsdaten auf dem lokalen Laufwerk befinden
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Beispiel 2: Berichterstellung, wenn sich die Profilerstellungsdaten auf einem Remoteserver befinden
Sie sollten über Lese-/Schreibzugriff für das Remoteverzeichnis verfügen.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Beispiel 3: Berichterstellung mit spezifischer Bandbreite und der Vorgabe, die erste Replikation innerhalb der angegebenen Zeit abzuschließen
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Beispiel 4: Berichterstellung mit Zuwachsfaktor von 5 Prozent (anstelle der Standardeinstellung von 30 Prozent)
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Beispiel 5: Berichterstellung mit einer Teilmenge der Profilerstellungsdaten
Es kann beispielsweise sein, dass Sie über Profilerstellungsdaten für 30 Tage verfügen und den Bericht nur für 20 Tage erstellen möchten.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Beispiel 6: Berichterstellung für RPO von 5 Minuten
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-the-calculation"></a>Für die Berechnung verwendeter Perzentilwert
**Welcher Perzentilstandardwert der Leistungsmetriken, die während der Profilerstellung gesammelt wurden, wird vom Tool für die Berichterstellung verwendet?**

Für das Tool werden standardmäßig die Werte des 95. Perzentils für Lese/Schreib-IOPS, Schreib-IOPS und die Datenänderung verwendet, die während der Profilerstellung aller VMs gesammelt wurden. Mit dieser Metrik wird sichergestellt, dass Spitzen (100. Perzentil), die für Ihre VMs aufgrund von temporären Ereignissen auftreten können, nicht zur Ermittlung der Bandbreitenanforderungen für Ihr Zielspeicherkonto und Ihre Quelle herangezogen werden. Ein temporäres Ereignis kann beispielsweise ein Sicherungsauftrag sein, der einmal pro Tag ausgeführt wird, oder eine regelmäßige Datenbankindizierung, eine Aktivität zur Erstellung eines Analyseberichts oder ähnliche Point-in-Time-Ereignisse mit kurzer Lebensdauer.

Bei Verwendung der Werte des 95. Perzentils erhalten Sie ein genaues Bild der echten Workloadmerkmale und erzielen die beste Leistung, wenn die Workloads in Azure ausgeführt werden. Dieser Wert muss normalerweise nicht geändert werden. Falls Sie den Wert doch ändern möchten (z.B. in das 90. Perzentil), können Sie die Konfigurationsdatei *ASRDeploymentPlanner.exe.config* im Standardordner aktualisieren und speichern, um einen neuen Bericht für die vorhandenen Profilerstellungsdaten zu erstellen.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Informationen zum Zuwachsfaktor
**Warum sollte ich den Zuwachsfaktor beim Planen von Bereitstellungen berücksichtigen?**

Es ist wichtig, dass Sie in Bezug auf Ihre Workloadmerkmale den Zuwachs und eine potenzielle vermehrte Nutzung im Laufe der Zeit berücksichtigen. Wenn Sich Ihre Workloadmerkmale nach der Einrichtung des Schutzes ändern, können Sie in Bezug auf den Schutz nicht zu einem anderen Speicherkonto wechseln, ohne den Schutz zu deaktivieren und anschließend wieder zu aktivieren.

Angenommen, Ihre VM ist derzeit für ein Standardspeicherkonto für die Replikation eingerichtet. Die Wahrscheinlichkeit ist hoch, dass es im Laufe der nächsten drei Monate zu mehreren Änderungen kommt:

* Die Anzahl von Benutzern der Anwendung, die auf der VM ausgeführt wird, erhöht sich.
* Aufgrund der sich daraus ergebenden erhöhten Datenänderungsrate auf der VM muss die VM auf Storage Premium umgestellt werden, damit die Site Recovery-Replikation Schritt halten kann.
* Sie müssen also den Schutz deaktivieren und für ein Storage Premium-Konto wieder aktivieren.

Es wird dringend empfohlen, beim Planen der Bereitstellung den Zuwachs zu berücksichtigen und den Standardwert von 30 Prozent zu verwenden. Sie sind der Experte in Bezug auf das Nutzungsmuster Ihrer Anwendungen und den projizierten Zuwachs und können diese Angabe beim Erstellen des Berichts entsprechend ändern. Außerdem haben Sie die Möglichkeit, mehrere Berichte mit unterschiedlichen Zuwachsfaktoren für dieselben Profilerstellungsdaten zu erstellen, um zu ermitteln, welche Empfehlungen für die Speicherzielbandbreite und die Quellbandbreite für Sie am besten geeignet sind.

Der erstellte Microsoft Excel-Bericht enthält die folgenden Informationen:

* [Input](site-recovery-deployment-planner.md#input) (Eingabe)
* [Empfehlungen](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Empfehlungen mit verfügbarer Bandbreite als Eingabe](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [VM<->Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (VM/Speicher-Anordnung)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatible VMs)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Inkompatible VMs)

![Deployment Planner](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Durchsatzberechnung

Führen Sie das Tool im GetThroughput-Modus aus, um den Durchsatz zu schätzen, der von Site Recovery während der Replikation von der lokalen Umgebung zu Azure erreicht werden kann. Das Tool berechnet den Durchsatz für den Server, auf dem das Tool ausgeführt wird. Idealerweise basiert dieser Server auf den Vorgaben des Leitfadens zur Größe des Konfigurationsservers. Führen Sie das Tool auf dem Konfigurationsserver aus, falls Sie Site Recovery-Infrastrukturkomponenten bereits lokal bereitgestellt haben.

Öffnen Sie eine Befehlszeilenkonsole, und wechseln Sie in den Ordner mit dem Site Recovery-Tool für die Bereitstellungsplanung. Führen Sie die Datei „ASRDeploymentPlanner.exe“ mit den unten angegebenen Parametern aus.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Parametername | Beschreibung |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Optional) Der UNC- oder lokale Verzeichnispfad des Speicherorts, an dem die Daten der Profilerstellung (während der Profilerstellung generierte Dateien) gespeichert werden. Diese Daten werden für die Erstellung des Berichts benötigt. Wenn kein Verzeichnisname angegeben ist, wird das Verzeichnis „ProfiledData“ verwendet. |
| -StorageAccountName | Der Name des Speicherkontos zur Ermittlung der Bandbreite, die für die Replikation von Daten aus der lokalen Umgebung in Azure verbraucht wird. Mit dem Tool werden Testdaten in dieses Speicherkonto hochgeladen, um die verbrauchte Bandbreite zu ermitteln. |
| -StorageAccountKey | Der Schlüssel des Speicherkontos, der zum Zugreifen auf das Speicherkonto verwendet wird. Navigieren Sie im Azure-Portal zu „Speicherkonten“ > <*Name des Speicherkontos*> > „Einstellungen“ > „Zugriffsschlüssel“ > „Key1“ (oder primärer Zugriffsschlüssel für ein klassisches Speicherkonto). |
| -VMListFile | Die Datei mit der Liste der VMs, für die Profile erstellt werden sollen, um die verbrauchte Bandbreite zu berechnen. Der Dateipfad kann absolut oder relativ sein. Die Datei sollte einen VM-Namen bzw. eine IP-Adresse pro Zeile enthalten. Die in der Datei angegebenen VM-Namen sollten mit den VM-Namen auf dem vCenter-Server/vSphere ESXi-Host identisch sein.<br>Die Datei „VMList.txt“ enthält beispielsweise die folgenden VMs:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (optional) Dies ist Ihre Azure Storage-Zielkontoumgebung. Diese drei Werte sind möglich: „AzureCloud“, „AzureUSGovernment“ und „AzureChinaCloud“. Der Standardwert ist „AzureCloud“. Verwenden Sie den Parameter, wenn Sie als Azure-Zielregion Clouds vom Typ „Azure US Government“ oder „Azure China“ verwenden. |

Mit dem Tool werden im angegebenen Verzeichnis mehrere Dateien vom Typ „asrvhdfile<#>.vhd“ (wobei „#“ für eine Zahl steht) mit 64 MB erstellt. Mit dem Tool werden diese Dateien in das Speicherkonto hochgeladen, um den Durchsatz zu ermitteln. Nach dem Messen des Durchsatzes löscht das Tool diese Dateien aus dem Speicherkonto und vom lokalen Server. Wenn das Tool aus irgendeinem Grund beendet wird, während der Durchsatz berechnet wird, werden die Dateien nicht aus dem Speicher oder vom lokalen Server gelöscht. Sie müssen sie manuell löschen.

Der Durchsatz wird zu einem angegebenen Zeitpunkt gemessen. Hierbei handelt es sich um den maximalen Durchsatz, der von Site Recovery während der Replikation erreicht werden kann, wenn alle anderen Faktoren gleich bleiben. Falls eine Anwendung beispielsweise in demselben Netzwerk auf einmal mehr Bandbreite verbraucht, variiert der tatsächliche Durchsatz während der Replikation. Wenn Sie den GetThroughput-Befehl über einen Konfigurationsserver ausführen, hat das Tool keinerlei Informationen zu geschützten VMs und zur laufenden Replikation. Das Ergebnis des gemessenen Durchsatzes ist anders, wenn der GetThroughput-Vorgang ausgeführt wird, während die geschützten VMs über eine hohe Datenänderungsrate verfügen. Wir empfehlen Ihnen, das Tool zu unterschiedlichen Zeiten während der Profilerstellung auszuführen, um zu verstehen, welche Durchsatzstufen jeweils erreicht werden können. Im Bericht wird vom Tool der zuletzt gemessene Durchsatz angezeigt.

### <a name="example"></a>Beispiel
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Führen Sie das Tool auf einem Server aus, der über die gleichen Speicher- und CPU-Merkmale wie der Konfigurationsserver verfügt.
>
> Legen Sie die empfohlene Bandbreite für die Replikation so fest, dass der RPO-Wert in 100 Prozent der Fälle erreicht wird. Nach dem Festlegen der passenden Bandbreite sollten Sie wie folgt vorgehen, falls vom Tool kein Anstieg des erzielten Durchsatzes gemeldet wird:
>
>  1. Überprüfen Sie, ob der Site Recovery-Durchsatz durch die Dienstqualität (Quality of Service, QoS) des Netzwerks eingeschränkt wird.
>
>  2. Überprüfen Sie, ob sich Ihr Site Recovery-Tresor in der nächstgelegenen physisch unterstützten Microsoft Azure-Region befindet, um die Netzwerkwartezeit zu verringern.
>
>  3. Überprüfen Sie Ihre lokalen Speichermerkmale, um zu ermitteln, ob Sie die Hardware (z.B. Wechsel von HDD auf SSD) verbessern können.
>
>  4. Ändern Sie die Site Recovery-Einstellungen auf dem Prozessserver, um [die für die Replikation verwendete Menge an Netzwerkbandbreite zu erhöhen](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Empfehlungen mit gewünschtem RPO-Wert als Eingabe

### <a name="profiled-data"></a>Profilerstellungsdaten

![Ansicht der Profilerstellungsdaten im Deployment Planner](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profiled data period** (Zeitraum der Profilerstellung): Der Zeitraum, in dem die Profilerstellung durchgeführt wurde. Standardmäßig bezieht das Tool alle Profilerstellungsdaten in die Berechnung ein, sofern der Bericht nicht nur für einen bestimmten Zeitraum erstellt wird, indem bei der Berichterstellung die Optionen „StartDate“ und „EndDate“ genutzt werden.

**Server Name** (Servername): Der Name oder die IP-Adresse des VMware vCenter- oder ESXi-Hosts, für dessen VMs der Bericht erstellt wird.

**Desired RPO** (Gewünschter RPO-Wert): Der RPO-Wert (Recovery Point Objective) für Ihre Bereitstellung. Standardmäßig wird die erforderliche Netzwerkbandbreite für RPO-Werte von 15, 30 und 60 Minuten berechnet. Basierend auf der Auswahl werden die betroffenen Werte auf dem Blatt aktualisiert. Wenn Sie beim Erstellen des Berichts den Parameter *DesiredRPOinMin* verwendet haben, wird dieser Wert als Ergebnis unter „Desired RPO“ (Gewünschter RPO-Wert) angezeigt.

### <a name="profiling-overview"></a>Übersicht über die Profilerstellung

![Ergebnisse der Profilerstellung im Deployment Planner](./media/site-recovery-deployment-planner/profiling-overview.png)

**Total Profiled Virtual Machines** (VMs mit Profilerstellung gesamt): Die Gesamtzahl der VMs, für die Profilerstellungsdaten verfügbar sind. Wenn „VMListFile“ Namen von VMs enthält, für die keine Profile erstellt wurden, werden diese VMs in der Berichterstellung nicht berücksichtigt und aus der Gesamtzahl von VMs für die Profilerstellung ausgeschlossen.

**Compatible Virtual Machines** (Kompatible virtuelle Computer): Die Anzahl von VMs, die mit Site Recovery in Azure geschützt werden können. Dies ist die Gesamtzahl von kompatiblen VMs, für die die erforderliche Netzwerkbandbreite, die Anzahl von Speicherkonten, die Anzahl von Azure-Kernen und die Anzahl von Konfigurationsservern und zusätzlichen Prozessservern berechnet werden. Die Details der einzelnen kompatiblen VMs sind im Abschnitt „Kompatible VMs“ enthalten.

**Incompatible Virtual Machines** (Inkompatible virtuelle Computer): Die Anzahl von VMs, für die Profile erstellt wurden, die für den Schutz mit Site Recovery inkompatibel sind. Die Gründe für die Inkompatibilität sind im Abschnitt „Inkompatible VMs“ beschrieben. Wenn „VMListFile“ Namen von VMs enthält, für die keine Profile erstellt wurden, werden diese VMs aus der Anzahl von inkompatiblen VMs ausgeschlossen. Diese VMs werden unten im Abschnitt „Incompatible VMs“ (Inkompatible VMs) unter „Data not found“ (Daten nicht gefunden) aufgeführt.

**Desired RPO** (Gewünschter RPO-Wert): Der gewünschte RPO-Wert (Recovery Point Objective) in Minuten. Der Bericht wird für drei RPO-Werte erstellt: 15 (Standard), 30 und 60 Minuten. Die Bandbreitenempfehlung im Bericht wird basierend auf Ihrer Auswahl in der Dropdownliste „Desired RPO“ (Gewünschter RPO-Wert) oben rechts auf dem Blatt geändert. Wenn Sie den Bericht mit dem Parameter *-DesiredRPO* und einem benutzerdefinierten Wert erstellt haben, wird dieser benutzerdefinierte Wert in der Dropdownliste „Desired RPO“ (Gewünschter RPO-Wert) als Standardwert angezeigt.

### <a name="required-network-bandwidth-mbps"></a>Erforderliche Netzwerkbandbreite (MBit/s)

![Erforderliche Netzwerkbandbreite im Deployment Planner](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**To meet RPO 100 percent of the time** (Erreichung des RPO-Werts in 100 % der Fälle): Die empfohlene Bandbreite in MBit/s, die zugeordnet werden sollte, um den gewünschten RPO-Wert in 100% der Fälle zu erzielen. Diese Menge an Bandbreite muss dediziert für die Deltareplikation im stabilen Zustand für alle kompatiblen VMs bereitgestellt werden, um RPO-Verletzungen zu vermeiden.

**To meet RPO 90 percent of the time** (Erreichung des RPO-Werts in 90 % der Fälle): Wenn Sie aufgrund der Breitbandpreise oder aus einem anderen Grund nicht die Bandbreite festlegen können, die zur Erreichung des gewünschten RPO-Werts in 100% der Fälle erforderlich ist, können Sie auch eine niedrigere Bandbreiteneinstellung wählen, mit der der gewünschte RPO-Wert in 90% der Fälle erreicht wird. Zum besseren Verständnis der Auswirkungen, die mit dem Festlegen dieser geringeren Bandbreite verbunden sind, enthält der Bericht eine Was-wäre-wenn-Analyse zu Anzahl und Dauer der zu erwartenden RPO-Verletzungen.

**Achieved Throughput:** (Erzielter Durchsatz): Der Durchsatz von dem Server, auf dem Sie den GetThroughput-Befehl ausgeführt haben, zu der Microsoft Azure-Region, in der sich das Speicherkonto befindet. Mit diesem Durchsatzwert wird die geschätzte Ebene angegeben, die Sie erreichen können, wenn Sie die kompatiblen VMs mit Site Recovery schützen – vorausgesetzt, die Speicher- und Netzwerkmerkmale Ihres Konfigurations- bzw. Prozessservers bleiben in Bezug auf den Server, auf dem Sie das Tool ausgeführt haben, unverändert.

Für die Replikation sollten Sie die empfohlene Bandbreite so festlegen, dass der RPO-Wert in 100 Prozent der Fälle erreicht wird. Nach dem Festlegen der Bandbreite sollten Sie wie folgt vorgehen, falls vom Tool kein Anstieg des erzielten Durchsatzes gemeldet wird:

1. Überprüfen Sie, ob der Azure Site Recovery-Durchsatz durch die Dienstqualität (Quality of Service, QoS) des Netzwerks eingeschränkt wird.

2. Überprüfen Sie, ob sich Ihr Azure Site Recovery-Tresor in der nächstgelegenen physisch unterstützten Microsoft Azure-Region befindet, um die Netzwerkwartezeit zu verringern.

3. Überprüfen Sie Ihre lokalen Speichermerkmale, um zu ermitteln, ob Sie die Hardware (z.B. Wechsel von HDD auf SSD) verbessern können.

4. Ändern Sie die Azure Site Recovery-Einstellungen auf dem Prozessserver, um [die für die Replikation verwendete Menge an Netzwerkbandbreite zu erhöhen](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Wenn Sie das Tool auf einem Konfigurations- oder Prozessserver ausführen, der bereits über geschützte VMs verfügt, ist es ratsam, das Tool mehrmals auszuführen. Der erzielte Durchsatzwert ändert sich je nach dem Datenänderungsvolumen, das zum jeweiligen Zeitpunkt verarbeitet wird.

Für alle Site Recovery-Bereitstellungen für Unternehmen empfehlen wir die Verwendung von [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Erforderliche Speicherkonten
Im folgenden Diagramm wird die Gesamtzahl von Speicherkonten (Standard und Premium) angegeben, die zum Schützen aller kompatiblen VMs erforderlich sind. Informationen dazu, welches Speicherkonto für eine VM jeweils verwendet werden sollte, finden Sie im Abschnitt „VM/Speicher-Anordnung“.

![Erforderliche Speicherkonten im Deployment Planner](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Erforderliche Anzahl von Azure-Kernen
Dieses Ergebnis gibt die Gesamtzahl von Kernen an, die eingerichtet werden sollten, bevor ein Failover oder Testfailover für alle kompatiblen VMs durchgeführt wird. Falls im Abonnement zu wenig Kerne verfügbar sind, können von Site Recovery bei einem Testfailover oder Failover keine VMs erstellt werden.

![Erforderliche Anzahl von Azure-Kernen im Deployment Planner](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Erforderliche lokale Infrastruktur
Hiermit wird die Gesamtzahl von Konfigurationsservern und zusätzlichen Prozessservern angegeben, die konfiguriert werden müssen, um alle kompatiblen VMs zu schützen. Je nach den unterstützten [empfohlene Größen für den Konfigurationsserver](https://aka.ms/asr-v2a-on-prem-components) werden vom Tool ggf. weitere Server empfohlen. Die Empfehlung basiert auf der täglichen Änderungsrate oder der maximalen Anzahl von geschützten VMs (bei durchschnittlich drei Datenträgern pro VM) – je nachdem, welcher Wert größer ist bzw. auf dem Konfigurationsserver oder zusätzlichen Prozessserver zuerst erreicht wird. Die Details der Gesamtänderungsrate pro Tag und die Gesamtzahl von geschützten Datenträgern finden Sie im Abschnitt „Eingabe“.

![Erforderliche lokale Infrastruktur im Deployment Planner](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Was-wäre-wenn-Analyse
Bei dieser Analyse wird angegeben, wie viele Regelverletzungen während des Zeitraums der Profilerstellung auftreten können, wenn Sie eine geringere Bandbreite festlegen, um den gewünschten RPO-Wert nur in 90% der Fälle zu erreichen. Jeden Tag können eine oder mehrere RPO-Verletzungen auftreten. Im Graphen wird der RPO-Spitzenwert des Tages angezeigt.
Anhand dieser Analyse können Sie entscheiden, ob die Anzahl von RPO-Verletzungen über alle Tage hinweg und die RPO-Spitzenwerte pro Tag für die angegebene geringere Bandbreite akzeptabel sind. Wenn ja, können Sie die geringere Bandbreite für die Replikation zuordnen. Ordnen Sie ansonsten die höhere Bandbreite gemäß dem Vorschlag zu, um den gewünschten RPO-Wert in 100% der Fälle zu erreichen.

![Was-wäre-wenn-Analyse im Deployment Planner](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Empfohlene VM-Batchgröße für die erste Replikation
In diesem Abschnitt wird die Anzahl von VMs empfohlen, die parallel geschützt werden können, um die erste Replikation innerhalb von 72 Stunden mit der vorgeschlagenen Bandbreite durchzuführen und den gewünschten RPO-Wert in 100% der Fälle zu erreichen. Dieser Wert ist konfigurierbar. Verwenden Sie den Parameter *GoalToCompleteIR*, um ihn bei der Berichterstellung zu ändern.

Der hier angegebene Graph zeigt einen Bereich mit Bandbreitenwerten und einer berechneten VM-Batchgrößenanzahl, um die erste Replikation innerhalb von 72 Stunden durchzuführen – basierend auf der ermittelten durchschnittlichen VM-Größe über alle kompatiblen VMs hinweg.

In der öffentlichen Vorschauversion wird im Bericht nicht angegeben, welche VMs in einem Batch enthalten sein sollten. Sie können die im Abschnitt „Kompatible VMs“ angezeigte Datenträgergröße verwenden, um nach den Größen der einzelnen VMs zu suchen und diese für einen Batch auszuwählen, oder Sie können die VMs basierend auf bekannten Workloadmerkmalen auswählen. Die Dauer der ersten Replikation ändert sich proportional basierend auf der tatsächlichen VM-Datenträgergröße, dem genutzten Datenträgerspeicherplatz und dem verfügbaren Netzwerkdurchsatz.

![Empfohlene VM-Batchgröße](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Verwendeter Zuwachsfaktor und Perzentilwerte
In diesem Abschnitt werden unten auf dem Blatt der verwendete Perzentilwert für alle Leistungsindikatoren der VMs für die Profilerstellung (Standardeinstellung: 95. Perzentil) und der für alle Berechnungen verwendete Zuwachsfaktor (Standardeinstellung: 30%) angezeigt.

![Verwendeter Zuwachsfaktor und Perzentilwerte](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Empfehlungen mit verfügbarer Bandbreite als Eingabe

![Empfehlungen mit verfügbarer Bandbreite als Eingabe](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Es kann sein, dass Sie in einer bestimmten Situation wissen, dass Sie keine höhere Bandbreite als x MBit/s für die Site Recovery-Replikation festlegen können. Das Tool ermöglicht Ihnen das Eingeben der verfügbaren Bandbreite (mit Verwendung des Parameters „-Bandwidth“ während der Berichterstellung) und das Ermitteln des erreichbaren RPO-Werts in Minuten. Anhand dieser Angabe zum erreichbaren RPO-Wert können Sie entscheiden, ob Sie zusätzliche Bandbreite einrichten müssen oder ob eine Lösung für die Notfallwiederherstellung mit diesem RPO-Wert für Sie ausreicht.

![Erreichbarer RPO-Wert für Bandbreite von 500 MBit/s](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Eingabe
Das Blatt „Input“ (Eingabe) enthält eine Übersicht über die VMware-Umgebung für die Profilerstellung.

![Übersicht über die VMware-Umgebung für die Profilerstellung](./media/site-recovery-deployment-planner/Input.png)

**Start Date** (Startdatum) und **End Date** (Enddatum): Das Start- und Enddatum der Profilerstellungsdaten, die bei der Berichterstellung berücksichtigt werden. Das Startdatum ist standardmäßig das Datum, an dem die Profilerstellung beginnt, und das Enddatum ist das Datum, an dem die Profilerstellung beendet wird. Dies können die Werte von „StartDate“ und „EndDate“ sein, wenn der Bericht mit diesen Parametern erstellt wird.

**Total number of profiling days** (Gesamtzahl von Profilerstellungstagen): Die gesamte Anzahl von Tagen der Profilerstellung zwischen dem Start- und Enddatum, für die der Bericht erstellt wird.

**Number of compatible virtual machines** (Anzahl von kompatiblen virtuellen Computern): Die Gesamtzahl von kompatiblen VMs, für die die erforderliche Netzwerkbandbreite, die erforderliche Anzahl von Speicherkonten, Microsoft Azure-Kerne, Konfigurationsserver und zusätzliche Prozessserver berechnet werden.

**Total number of disks across all compatible virtual machines** (Datenträger gesamt für alle kompatiblen virtuellen Computer): Die Anzahl, die als eine der Eingaben verwendet wird, um entscheiden zu können, wie viele Konfigurationsserver und zusätzliche Prozessserver in der Bereitstellung verwendet werden sollen.

**Average number of disks per compatible virtual machine** (Durchschnittliche Anzahl von Datenträgern pro kompatiblem virtuellem Computer): Die durchschnittliche Anzahl von Datenträgern, die über alle kompatiblen VMs hinweg berechnet wird.

**Average disk size (GB)** (Durchschnittliche Datenträgergröße (GB)): Durchschnittliche Datenträgergröße, die über alle kompatiblen VMs hinweg berechnet wird.

**Desired RPO (minutes)** (Gewünschter RPO-Wert (Minuten)): Entweder der RPO-Standardwert (Recovery Point Objective) oder der Wert, der für den Parameter „DesiredRPO“ zum Zeitpunkt der Berichterstellung übergeben wird, um die erforderliche Bandbreite zu schätzen.

**Desired bandwidth (Mbps)** (Gewünschte Bandbreite (MBit/s)): Der Wert, den Sie für den Parameter „Bandwidth“ zum Zeitpunkt der Berichterstellung übergeben haben, um den erreichbaren RPO-Wert zu schätzen.

**Observed typical data churn per day (GB)** (Beobachtete typische Datenänderungsrate pro Tag (GB)): Durchschnittliche Datenänderungsrate über alle Tage der Profilerstellung hinweg. Diese Anzahl wird als eine der Eingaben genutzt, um eine Entscheidung über die Anzahl von Konfigurationsservern und zusätzlichen Prozessservern zu treffen, die in der Bereitstellung verwendet werden sollen.


## <a name="vm-storage-placement"></a>VM/Speicher-Anordnung

![VM/Speicher-Anordnung](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Disk Storage Type** (Datenträger-Speichertyp): Entweder „Standard“ oder „Premium“. Dies ist das Speicherkonto, das zum Replizieren aller entsprechenden VMs verwendet wird, die in der Spalte **VMs to Place** (Anzuordnende VMs) angegeben sind.

**Suggested Prefix** (Vorgeschlagenes Präfix): Das vorgeschlagene Präfix aus drei Zeichen, das zum Benennen des Speicherkontos verwendet werden kann. Sie können ein eigenes Präfix verwenden, aber der Vorschlag des Tools basiert auf der [Partitionsbenennungskonvention für Speicherkonten](https://aka.ms/storage-performance-checklist).

**Suggested Account Name** (Vorgeschlagener Kontoname): Der Speicherkontoname nach Einbindung des vorgeschlagenen Präfix. Ersetzen Sie den Namen in den spitzen Klammern (< und >) durch Ihre benutzerdefinierte Eingabe.

**Log Storage Account** (Protokollspeicherkonto): Alle Replikationsprotokolle werden in einem Speicherkonto vom Typ „Standard“ gespeichert. Richten Sie für VMs, die in einem Storage Premium-Konto repliziert werden, ein zusätzliches Standard-Speicherkonto für die Protokollspeicherung ein. Ein einzelnes Standard-Protokollspeicherkonto kann von mehreren Premium-Replikationsspeicherkonten genutzt werden. Für VMs, die unter Standardspeicherkonten repliziert werden, wird dasselbe Speicherkonto für Protokolle verwendet.

**Suggested Log Account Name** (Vorgeschlagener Protokollkontoname): Der Name des Speicherprotokollkontos nach Einbindung des vorgeschlagenen Präfix. Ersetzen Sie den Namen in den spitzen Klammern (< und >) durch Ihre benutzerdefinierte Eingabe.

**Placement Summary** (Zusammenfassung der Anordnung): Eine Übersicht über die gesamte VM-Last für das Speicherkonto zum Zeitpunkt der Replikation und des Testfailovers bzw. Failovers. Folgendes wird angegeben: Gesamtzahl der VMs, die dem Speicherkonto zugeordnet sind, Lese/Schreib-IOPS-Gesamtwert über alle VMs hinweg, die unter diesem Speicherkonto angeordnet werden, Schreib-IOPS-Gesamtwert (Replikation), insgesamt eingerichtete Größe über alle Datenträger hinweg und Gesamtzahl von Datenträgern.

**VMs to Place** (Anzuordnende VMs): Eine Liste mit allen VMs, die im jeweiligen Speicherkonto angeordnet werden sollten, um die optimale Leistung und Nutzung zu erzielen.

## <a name="compatible-vms"></a>Kompatible VMs
![Excel-Tabelle mit kompatiblen VMs](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM Name** (VM-Name): Der VM-Name oder die IP-Adresse, der bzw. die in „VMListFile“ verwendet wird, wenn ein Bericht erstellt wird. In dieser Spalte sind auch die Datenträger (VMDKs) angegeben, die an die VMs angefügt sind. Die Namen enthalten den ESXi-Hostnamen, um vCenter-VMs mit doppelten Namen oder IP-Adressen unterscheiden zu können. Der aufgeführte ESXi-Host ist der Host, auf dem die VM angeordnet wurde, als das Tool während der Profilerstellung die Ermittlung durchgeführt hat.

**VM Compatibility** (VM-Kompatibilität): Mögliche Werte sind **Yes** und **Yes**\*. **Yes**\* steht für Fälle, in denen die VM für [Azure Storage Premium](https://aka.ms/premium-storage-workload) geeignet ist. Hier fällt der Datenträger mit hoher Datenänderungsrate bzw. hohem IOPS-Wert, für den das Profil erstellt wird, in die Kategorie P20 oder P30. Aufgrund der Größe des Datenträgers wird er aber auf P10 bzw. P20 heruntergestuft. Das Speicherkonto entscheidet basierend auf der Größe, welchem Storage Premium-Datenträgertyp ein Datenträger zugeordnet wird. Beispiel:
* Bei weniger als 128 GB ist die Kategorie P10.
* Bei 128 GB bis 512 GB wird die Kategorie P20 verwendet.
* Für den Bereich 512 GB bis 1.023 GB lautet die Kategorie P30.

Falls ein Datenträger aufgrund seiner Workloadmerkmale in die Kategorie P20 oder P30 fällt, aufgrund der Größe aber ein niedrigerer Storage Premium-Datenträgertyp gewählt wird, wird die VM vom Tool als **Yes**\* gekennzeichnet. Sie erhalten vom Tool außerdem die Empfehlung, dass Sie entweder die Größe des Quelldatenträgers an den empfohlenen Storage Premium-Datenträgertyp anpassen oder den Typ des Zieldatenträgers nach dem Failover ändern sollten.

**Storage Type** (Speichertyp): Standard oder Premium.

**Suggested Prefix** (Empfohlenes Präfix): Das dreistellige Präfix für das Speicherkonto.

**Storage Account** (Speicherkonto): Der Name, für den das vorgeschlagene Speicherkontopräfix verwendet wird.

**R/W IOPS (with Growth Factor)** (Lese/Schreib-IOPS (mit Zuwachsfaktor): Der Lese/Schreib-IOPS-Wert für die Spitzenworkload auf dem Datenträger (Standardeinstellung: 95. Perzentil), einschließlich des Faktors für den zukünftigen Zuwachs (Standardeinstellung: 30%). Beachten Sie, dass der Lese/Schreib-IOPS-Gesamtwert einer VM nicht immer die Summe aller Lese/Schreib-IOPS-Werte der einzelnen Datenträger einer VM ist. Der Grund ist, dass der Lese/Schreib-IOPS-Spitzenwert der VM der Spitzenwert der Summe aller Lese/Schreib-IOPS-Werte der einzelnen Datenträger für jede Minute des Profilerstellungszeitraums ist.

**Data Churn (MBps) (with Growth Factor)** (Datenänderung (MBit/s) (mit Zuwachsfaktor)): Die Spitzenänderungsrate auf dem Datenträger (Standardeinstellung: 95. Perzentil), einschließlich des Faktors für den zukünftigen Zuwachs (Standardeinstellung: 30%). Beachten Sie Folgendes: Die gesamte VM-Datenänderung ist nicht immer die Summe der Datenänderung der einzelnen VM-Datenträger, da der Spitzenwert der VM-Datenänderung der Spitzenwert der Summe der Datenänderung seiner einzelnen Datenträger für jede Minute des Profilerstellungszeitraums ist.

**Azure VM Size** (Größe des virtuellen Azure-Computers): Die Idealgröße für die Zuordnung von Azure Cloud Services-VMs für diese lokale VM. Die Zuordnung basiert auf dem lokalen Arbeitsspeicher der VM, der Anzahl von Datenträgern/Kernen/NICs und dem Lese/Schreib-IOPS-Wert. Die Empfehlung ist immer die niedrigste Azure-VM-Größe, bei der alle Merkmale der lokalen VM erfüllt werden.

**Number of Disks** (Anzahl von Datenträgern): Die Gesamtzahl von Datenträgern auf dem virtuellen Computer (Virtual Machine Disks, VMDKs).

**Disk size (GB)** (Datenträgergröße (GB)): Eingerichtete Gesamtgröße aller Datenträger der VM. Im Tool wird auch die Datenträgergröße für die einzelnen Datenträger der VM angezeigt.

**Cores** (Kerne): Die Anzahl von CPU-Kernen auf der VM.

**Memory (MB)** (Arbeitsspeicher (MB)): Der Arbeitsspeicher (RAM) auf der VM.

**NICs**: Die Anzahl von NICs auf der VM.

**Boot Type**: Dies ist der Starttyp der VM. Er kann entweder „BIOS“ oder „EFI“ lauten. Derzeit unterstützt Azure Site Recovery nur den Starttyp „BIOS“. Alle virtuellen Computer mit dem Starttyp „EFI“ sind in der Tabelle unter „Inkompatible VMs“ aufgeführt.

**OS Type**: Dies ist der Betriebssystemtyp der VM. Er kann „Windows“, „Linux“ oder „Other“ lauten.

## <a name="incompatible-vms"></a>Inkompatible VMs

![Excel-Tabelle mit inkompatiblen VMs](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM Name** (VM-Name): Der VM-Name oder die IP-Adresse, der bzw. die in „VMListFile“ verwendet wird, wenn ein Bericht erstellt wird. In dieser Spalte sind auch die Datenträger (VMDKs) angegeben, die an die VMs angefügt sind. Die Namen enthalten den ESXi-Hostnamen, um vCenter-VMs mit doppelten Namen oder IP-Adressen unterscheiden zu können. Der aufgeführte ESXi-Host ist der Host, auf dem die VM angeordnet wurde, als das Tool während der Profilerstellung die Ermittlung durchgeführt hat.

**VM Compatibility** (VM-Kompatibilität): Gibt an, warum die jeweilige VM für die Verwendung mit Site Recovery nicht kompatibel ist. Die Gründe werden für jeden inkompatiblen Datenträger der VM beschrieben. Basierend auf den veröffentlichten [Speichergrenzwerten](https://aka.ms/azure-storage-scalbility-performance) können dies folgende Gründe sein:

* Der Datenträger ist größer als 1.023 GB. Azure Storage unterstützt derzeit keine Datenträger, die größer als 1 TB sind.
* Der Starttyp ist EFI. Derzeit unterstützt Azure Site Recovery für virtuelle Computer nur den Starttyp „BIOS“.

* Die VM-Gesamtgröße (Replikation + TFO) übersteigt den Grenzwert für die Unterstützung von Speicherkonten (35 TB). Diese Inkompatibilität tritt normalerweise auf, wenn ein einzelner Datenträger der VM über ein Leistungsmerkmal verfügt, das den unterstützten Azure- oder Site Recovery-Grenzwert für Standardspeicher überschreitet. Hierdurch fällt die VM in die Storage Premium-Zone. Die maximal unterstützte Größe für ein Storage Premium-Konto beträgt aber 35 TB, und eine einzelne geschützte VM kann nicht über mehrere Speicherkonten hinweg geschützt werden. Beachten Sie außerdem Folgendes: Wenn ein Testfailover auf einer geschützten VM durchgeführt wird, erfolgt dies unter demselben Speicherkonto, unter dem die Replikation durchgeführt wird. Richten Sie in diesem Fall die doppelte Größe des Datenträgers ein, damit die Replikation weiter durchgeführt werden kann und gleichzeitig das Testfailover erfolgreich ist.
* Der IOPS-Quellwert übersteigt den unterstützten IOPS-Speichergrenzwert von 5.000 pro Datenträger.
* Der IOPS-Quellwert übersteigt den unterstützten IOPS-Speichergrenzwert von 80.000 pro VM.
* Die durchschnittliche Datenänderungsrate übersteigt den unterstützten Grenzwert für die Site Recovery-Datenänderungsrate von 10 MBit/s für die durchschnittliche E/A-Größe für den Datenträger.
* Die gesamte Datenänderungsrate für alle Datenträger auf der VM übersteigt den maximal unterstützten Grenzwert für die Site Recovery-Datenänderungsrate von 54 MBit/s pro VM.
* Der durchschnittliche effektive Schreib-IOPS-Wert übersteigt den unterstützten Site Recovery-IOPS-Grenzwert von 840 für den Datenträger.
* Der berechnete Momentaufnahmespeicher übersteigt den unterstützten Grenzwert für Momentaufnahmespeicher von 10 TB.

**R/W IOPS (with Growth Factor)** (Lese/Schreib-IOPS (mit Zuwachsfaktor): Der IOPS-Wert für die Spitzenworkload auf dem Datenträger (Standardeinstellung: 95. Perzentil), einschließlich des Faktors für den zukünftigen Zuwachs (Standardeinstellung: 30%). Beachten Sie, dass der Lese/Schreib-IOPS-Gesamtwert der VM nicht immer die Summe aller Lese/Schreib-IOPS-Werte der einzelnen Datenträger einer VM ist. Der Grund ist, dass der Lese/Schreib-IOPS-Spitzenwert der VM der Spitzenwert der Summe aller Lese/Schreib-IOPS-Werte der einzelnen Datenträger für jede Minute des Profilerstellungszeitraums ist.

**Data Churn (MBps) (with Growth Factor)** (Datenänderung (MBit/s) (mit Zuwachsfaktor)): Die Spitzenänderungsrate auf dem Datenträger (Standardeinstellung: 95. Perzentil), einschließlich des Faktors für den zukünftigen Zuwachs (Standardeinstellung: 30%). Beachten Sie Folgendes: Die gesamte VM-Datenänderung ist nicht immer die Summe der Datenänderung der einzelnen VM-Datenträger, da der Spitzenwert der VM-Datenänderung der Spitzenwert der Summe der Datenänderung seiner einzelnen Datenträger für jede Minute des Profilerstellungszeitraums ist.

**Number of Disks** (Anzahl von Datenträgern): Die Gesamtzahl von VMDKs auf der VM.

**Disk size (GB)** (Datenträgergröße (GB)): Eingerichtete Gesamtgröße aller Datenträger der VM. Im Tool wird auch die Datenträgergröße für die einzelnen Datenträger der VM angezeigt.

**Cores** (Kerne): Die Anzahl von CPU-Kernen auf der VM.

**Memory (MB)** (Arbeitsspeicher (MB)): Die Größe des Arbeitsspeichers (RAM) auf der VM.

**NICs**: Die Anzahl von NICs auf der VM.

**Boot Type**: Dies ist der Starttyp der VM. Er kann entweder „BIOS“ oder „EFI“ lauten. Derzeit unterstützt Azure Site Recovery nur den Starttyp „BIOS“. Alle virtuellen Computer mit dem Starttyp „EFI“ sind in der Tabelle unter „Inkompatible VMs“ aufgeführt.

**OS Type**: Dies ist der Betriebssystemtyp der VM. Er kann „Windows“, „Linux“ oder „Other“ lauten.


## <a name="site-recovery-limits"></a>Site Recovery-Grenzwerte

**Replikationsspeicherziel** | **Durchschnittliche E/A-Größe des Quelldatenträgers** |**Durchschnittliche Datenänderungsrate des Quelldatenträgers** | **Gesamte Datenänderungsrate des Quelldatenträgers pro Tag**
---|---|---|---
Standardspeicher | 8 KB    | 2 MBit/s | 168 GB pro Datenträger
Premium-P10-Datenträger | 8 KB    | 2 MBit/s | 168 GB pro Datenträger
Premium-P10-Datenträger | 16 KB | 4 MBit/s |    336 GB pro Datenträger
Premium-P10-Datenträger | 32 KB oder höher | 8 MBit/s | 672 GB pro Datenträger
Premium-P20- oder -P30-Datenträger | 8 KB    | 5 MBit/s | 421 GB pro Datenträger
Premium-P20- oder -P30-Datenträger | 16 KB oder höher |10 MBit/s | 842 GB pro Datenträger

Dies sind Durchschnittswerte, bei denen eine E/A-Überlappung von 30% angenommen wird. Site Recovery kann einen höheren Durchsatz basierend auf dem Überlappungsverhältnis, höheren Schreibgrößen und dem tatsächlichen Workload-E/A-Verhalten verarbeiten. Für die obigen Zahlen wurde ein typischer Backlog von ca. fünf Minuten vorausgesetzt. Dies bedeutet, dass die Daten nach dem Hochladen verarbeitet werden und innerhalb von fünf Minuten ein Wiederherstellungspunkt erstellt wird.

Diese Grenzwerte basieren auf unseren Tests, können aber nicht alle möglichen E/A-Kombinationen für Anwendungen abdecken. Die tatsächlichen Ergebnisse können je nach Ihrer E/A-Mischung für die Anwendungen variieren. Auch nach der Planung der Bereitstellung ist es zum Erzielen der bestmöglichen Ergebnisse stets zu empfehlen, umfangreiche Anwendungstests per Testfailover durchzuführen, um sich ein eindeutiges Bild der Leistung zu verschaffen.

## <a name="updating-the-deployment-planner"></a>Aktualisieren des Deployment Planner
Gehen Sie wie folgt vor, um den Deployment Planner zu aktualisieren:

1. [Laden Sie die aktuelle Version des Azure Site Recovery Deployment Planner herunter.](https://aka.ms/asr-deployment-planner)

2. Kopieren Sie den ZIP-Ordner auf einen Server, auf dem Sie ihn ausführen möchten.

3. Extrahieren Sie den ZIP-Ordner.

4. Führen Sie einen der folgenden Schritte aus:
 * Falls die aktuelle Version keine Fehlerbehebung für die Profilerstellung enthält und die Profilerstellung bereits mit der aktuellen Planner-Version ausgeführt wird, ist es ratsam, die Profilerstellung fortzusetzen.
 * Wenn die aktuelle Version eine Fehlerbehebung für die Profilerstellung enthält, empfehlen wir Ihnen, die Profilerstellung für die aktuelle Version zu beenden und mit der neuen Version neu zu starten.

  >[!NOTE]
  >
  >Übergeben Sie beim Starten der Profilerstellung mit der neuen Version den gleichen Pfad des Ausgabeverzeichnisses, damit die Profildaten vom Tool an die vorhandenen Dateien angefügt werden. Zum Erstellen des Berichts wird ein vollständiger Satz von Profilerstellungsdaten verwendet. Wenn Sie ein anderes Ausgabeverzeichnis übergeben, werden neue Dateien erstellt und alte Profilerstellungsdaten nicht zum Erstellen des Berichts verwendet.
  >
  >Jeder neue Deployment Planner ist ein kumulatives Update der ZIP-Datei. Es ist nicht erforderlich, die neuesten Dateien in den vorherigen Ordner zu kopieren. Sie können einen neuen Ordner erstellen und verwenden.


## <a name="version-history"></a>Versionsverlauf

### <a name="13"></a>1.3
Aktualisiert: 9. Mai 2017

Folgende neue Funktion wurde hinzugefügt:

* Unterstützung verwalteter Datenträger bei der Berichterstellung wurde hinzugefügt. Die Anzahl der virtuellen Computer, die in einem einzelnen Speicherkonto enthalten sein können, wird basierend auf der Auswahl eines verwalteten Datenträger für das Failover bzw. Testfailover berechnet.        


### <a name="12"></a>1.2
Aktualisiert: 7. April 2017

Folgende Fixes wurden hinzugefügt:

* Die Überprüfung des Starttyps (BIOS oder EFI) für jeden virtuellen Computer wurde hinzugefügt, um zu ermitteln, ob der virtuelle Computer mit dem Schutzverfahren kompatibel ist.
* Informationen zum Betriebssystem wurden für jeden virtuellen Computer in den Tabellen „Kompatible VMs“ und „Inkompatible VMs“ hinzugefügt.
* Der GetThroughput-Vorgang wird für die Microsoft Azure-Regionen „US Government“ und „China“ jetzt unterstützt.
* Einige zusätzliche Voraussetzungsüberprüfungen für vCenter- und ESXi-Server wurden hinzugefügt.
* Es wurde ein fehlerhafter Bericht erstellt, wenn die Gebietsschemaeinstellungen auf einen anderen Wert als „Englisch“ festgelegt waren.


### <a name="11"></a>1.1
Aktualisiert: 9. März 2017

Die folgenden Probleme wurden behoben:

* Das Tool kann keine Profile für VMs erstellen, wenn der vCenter-Server über zwei oder mehr VMs mit dem gleichen Namen oder der gleichen IP-Adresse für verschiedene ESXi-Hosts verfügt.
* Das Kopieren und Durchsuchen war für die Tabellen mit den kompatiblen und nicht kompatiblen virtuellen Computern deaktiviert.

### <a name="10"></a>1.0
Aktualisiert: 23. Februar 2017

Die öffentliche Vorschauversion des Azure Site Recovery Deployment Planner 1.0 weist die folgenden bekannten Probleme auf (an deren Behebung im Rahmen der nächsten Updates gearbeitet wird):

* Das Tool funktioniert nur für Szenarien vom Typ „VMware zu Azure“ und nicht für Bereitstellungen vom Typ „Hyper-V zu Azure“. Verwenden Sie für Szenarien vom Typ „Hyper-V zu Azure“ das [Hyper-V Capacity Planner-Tool](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Der GetThroughput-Vorgang wird für die Microsoft Azure-Regionen „US Government“ und „China“ nicht unterstützt.
* Das Tool kann keine Profile für VMs erstellen, wenn der vCenter-Server über zwei oder mehr VMs mit dem gleichen Namen oder der gleichen IP-Adresse für verschiedene ESXi-Hosts verfügt. In dieser Version überspringt das Tool die Profilerstellung für doppelte Namen und IP-Adressen von VMs in der VMListFile. Die Problemumgehung besteht darin, die Profilerstellung für die VMs nicht mit dem vCenter-Server durchzuführen, sondern mit einem ESXi-Host. Sie müssen für jeden ESXi-Host eine Instanz ausführen.

