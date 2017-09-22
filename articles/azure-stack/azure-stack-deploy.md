---
title: "Voraussetzungen für die Azure Stack Development Kit-Bereitstellung | Microsoft-Dokumentation"
description: "Erfahren Sie, welche Umgebungs- und Hardwareanforderungen für das Azure Stack Development Kit (Cloudbetreiber) gelten."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 32a21d9b-ee42-417d-8e54-98a7f90f7311
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/11/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: e4e2293149b64056d1ec9d1900119e9f4f511d06
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="azure-stack-deployment-prerequisites"></a>Voraussetzungen für die Azure Stack-Bereitstellung
Stellen Sie vor der Bereitstellung des Azure Stack [Development Kits](azure-stack-poc.md) sicher, dass Ihr Computer die folgenden Anforderungen erfüllt:


## <a name="hardware"></a>Hardware
| Komponente | Minimum | Empfohlen |
| --- | --- | --- |
| Festplatten: Betriebssystem |1 Betriebssystem-Datenträger mit mindestens 200 GB verfügbarem Speicherplatz für die Systempartition (SSD oder HDD) |1 Betriebssystem-Datenträger mit mindestens 200 GB verfügbarem Speicherplatz für die Systempartition (SSD oder HDD) |
| Festplattenlaufwerke: Allgemeine Development Kit-Daten* |4 Datenträger. Jeder Datenträger stellt mindestens 140 GB Kapazität bereit (SSD oder HDD). Alle verfügbaren Datenträger werden verwendet. |4 Datenträger. Jeder Datenträger stellt eine Kapazität von mindestens 250 GB bereit (SSD oder HDD). Alle verfügbaren Datenträger werden verwendet. |
| Compute: CPU |Dual-Socket: 12 physische Kerne (insgesamt) |Dual-Socket: 16 physische Kerne (insgesamt) |
| Compute: Arbeitsspeicher |96 GB RAM |128 GB RAM (Mindestanforderung für die Unterstützung von PaaS-Ressourcenanbietern)|
| Compute: BIOS |Hyper-V aktiviert (mit SLAT-Unterstützung) |Hyper-V aktiviert (mit SLAT-Unterstützung) |
| Netzwerk: NIC |Windows Server 2012 R2-Zertifizierung für NIC erforderlich; keine speziellen Features erforderlich |Windows Server 2012 R2-Zertifizierung für NIC erforderlich; keine speziellen Features erforderlich |
| HW-Logo-Zertifizierung |[Certified for Windows Server 2012 R2 (Zertifiziert für Windows Server 2012 R2)](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certified for Windows Server 2012 R2 (Zertifiziert für Windows Server 2012 R2)](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

\*Sie benötigen mehr als diese empfohlene Kapazität, wenn Sie viele [Marketplace-Elemente](azure-stack-download-azure-marketplace-item.md) aus Azure hinzufügen möchten.

**Laufwerkkonfiguration des Datenträgers:** Alle Datenlaufwerke müssen den gleichen Typ (SAS oder SATA) und die gleiche Kapazität aufweisen. Wenn SAS-Laufwerke verwendet werden, müssen diese über einen einzelnen Pfad angefügt werden (MPIO, Multipfad-Unterstützung wird nicht bereitgestellt).

**HBA-Konfigurationsoptionen**

* (Bevorzugt) Einfacher HBA
* RAID-HBA – Adapter muss im Durchleitungsmodus konfiguriert sein
* RAID-HBA: Datenträger sollten als einzelner Datenträger, RAID-0, konfiguriert werden.

**Unterstützte Kombinationen aus Bus und Medientypen**

* SATA-HDD
* SAS-HDD
* RAID-HDD
* RAID-SSD (wenn der Medientyp nicht angegeben/unbekannt\* ist)
* SATA-SSD + SATA-HDD
* SAS-SSD + SAS-HDD

\*RAID-Controller ohne Durchleitungsfunktion können den Medientyp nicht erkennen. Domänencontroller dieser Art kennzeichnen HDD und SSD als unbekannt. In diesem Fall wird die SSD als persistenter Speicher und nicht als Cachegerät verwendet. Daher können Sie das Development Kit auf diesen SSDs bereitstellen.

**Beispiel-HBAs:**LSI 9207-8i, LSI-9300-8i oder LSI-9265-8i im Pass-Through-Modus

OEM-Beispielkonfigurationen sind verfügbar.

## <a name="operating-system"></a>Betriebssystem
|  | **Anforderungen** |
| --- | --- |
| **Betriebssystemversion** |Windows Server 2012 R2 oder höher. Die Version des Betriebssystems ist vor dem Starten der Bereitstellung nicht wichtig, da der Hostcomputer auf der virtuellen Festplatte gestartet wird, die in der Azure Stack-Installation enthalten ist. Das Betriebssystem und alle erforderlichen Patches sind bereits in das Image integriert. Verwenden Sie keine Schlüssel zum Aktivieren von Windows Server-Instanzen, die im Development Kit verwendet werden. |

## <a name="deployment-requirements-check-tool"></a>Tool zur Überprüfung von Bereitstellungsanforderungen
Nach der Installation des Betriebssystems können Sie mit der [Bereitstellungsprüfung für Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) sicherstellen, dass Ihre Hardware alle Anforderungen erfüllt.

## <a name="account-requirements"></a>Kontoanforderungen
In der Regel stellen Sie das Development Kit bereit, wenn eine Internetverbindung besteht und Sie eine Verbindung mit Microsoft Azure herstellen können. In diesem Fall müssen Sie ein Azure Active Directory (Azure AD)-Konto zum Bereitstellen des Development Kits konfigurieren.

Wenn Ihre Umgebung nicht mit dem Internet verbunden ist oder Sie Azure AD nicht verwenden möchten, können Sie Azure Stack mithilfe von Active Directory-Verbunddienste (AD FS) bereitstellen. Das Development Kit enthält eigene AD FS- und Active Directory Domain Services-Instanzen. Wenn Sie diese Bereitstellungsoption nutzen, müssen Sie vorab Konten einrichten.

>[!NOTE]
Wenn Sie die AD FS-Bereitstellungsoption nutzen, müssen Sie Azure Stack erneut bereitstellen, um zu Azure AD zu wechseln.

### <a name="azure-active-directory-accounts"></a>Azure Active Directory-Konten
Zum Bereitstellen von Azure Stack über ein Azure AD-Konto müssen Sie vor dem Ausführen des PowerShell-Bereitstellungsskripts ein Azure AD-Konto vorbereiten. Dieses Konto wird der globale Administrator für den Azure AD-Mandanten. Es wird zum Bereitzustellen und Delegieren von Anwendungen und Dienstprinzipalen für alle Azure Stack-Dienste verwendet, die mit Azure Active Directory und Graph-API interagieren. Es fungiert außerdem als Besitzer des Standardanbieterabonnements (den Sie später jedoch ändern können). Mit diesem Konto können Sie sich beim Administratorportal Ihres Azure Stack-Systems anmelden.

1. Erstellen Sie ein Azure AD-Konto, das Verzeichnisadministrator für mindestens eine Azure AD-Instanz ist. Wenn Sie bereits eines haben, können Sie es verwenden. Andernfalls können Sie unter [http://azure.microsoft.com/de-de/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (<http://go.microsoft.com/fwlink/?LinkID=717821> für China) ein kostenloses Konto erstellen. Wenn Sie [Azure Stack zu einem späteren Zeitpunkt bei Azure registrieren](azure-stack-register.md) möchten, benötigen Sie auch ein Abonnement in diesem neu erstellten Konto.
   
    Bewahren Sie diese Anmeldeinformationen zur Verwendung in Schritt 6 der [Bereitstellung des Development Kits](azure-stack-run-powershell-script.md#deploy-the-development-kit) auf. Dieses *Dienstadministrator*-Konto kann Ressourcenclouds, Benutzerkonten, Mandantenpläne, Kontingente und Preise konfigurieren und verwalten. Im Portal ist es möglich, Websiteclouds und private Clouds mit virtuellen Computern sowie Pläne zu erstellen und Benutzerabonnements zu verwalten.
2. [Erstellen](azure-stack-add-new-user-aad.md) Sie mindestens ein Konto, damit Sie sich als Mandant beim Development Kit anmelden können.
   
   | **Azure Active Directory-Konto** | **Unterstützt?** |
   | --- | --- |
   | Geschäfts-, Schul- oder Unikonto mit gültigem öffentlichem Azure-Abonnement |Ja |
   | Microsoft-Konto mit gültigem öffentlichem Azure-Abonnement |Ja |
   | Geschäfts-, Schul- oder Unikonto mit gültigem Azure-Abonnement für China |Ja |
   | Geschäfts-, Schul- oder Unikonto mit gültigem US Government Azure-Abonnement |Ja |

## <a name="network"></a>Netzwerk
### <a name="switch"></a>Switch
Ein verfügbarer Port auf einem Switch für den Development Kit-Computer  

Der Development Kit-Computer unterstützt das Herstellen einer Verbindung mit einem Switchzugriffsport oder Trunkport. Auf dem Switch sind keine speziellen Funktionen erforderlich. Wenn Sie einen Trunkport verwenden oder eine VLAN-ID benötigen, müssen Sie die VLAN-ID als Bereitstellungsparameter angeben. Beispiele finden Sie in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Subnetz
Verbinden Sie den Development Kit-Computer nicht mit den folgenden Subnetzen:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Diese Subnetze sind für die internen Netzwerke innerhalb der Development Kit-Umgebung reserviert.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Es wird nur IPv4 unterstützt. Sie können keine IPv6-Netzwerke erstellen.

### <a name="dhcp"></a>DHCP
Stellen Sie sicher, dass in dem Netzwerk, mit dem die Netzwerkkarte eine Verbindung herstellt, ein DHCP-Server verfügbar ist. Wenn DHCP nicht verfügbar ist, müssen Sie ein weiteres statisches IPv4-Netzwerk neben dem vom Host verwendeten vorbereiten. Sie müssen diese IP-Adresse und das Gateway als Bereitstellungsparameter angeben. Beispiele finden Sie in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Zugriff auf das Internet
Azure Stack benötigt entweder direkten Zugriff auf das Internet oder über einen transparenten Proxy. Azure Stack unterstützt nicht die Konfiguration eines Webproxys, um Zugriff auf das Internet zu aktivieren. Sowohl die Host-IP und als auch die neue IP, die „MAS-BGPNAT01“ zugewiesen ist (über DHCP oder als statische IP-Adresse) müssen auf das Internet zugreifen können. Port 80 und 443 werden unter den Domänen „graph.windows.net“ und „login.microsoftonline.com“ verwendet.

## <a name="telemetry"></a>Telemetrie

Telemetrie unterstützt uns bei der Entwicklung zukünftiger Versionen von Azure Stack. Sie ermöglicht uns ein schnelles Reagieren auf Feedback, das Bereitstellen neuer Funktionen und das Verbessern der Qualität. Microsoft Azure Stack umfasst Windows Server 2016 und SQL Server 2014. Bei keinem dieser Produkte wurden die Standardeinstellungen geändert, und für beide gelten die Microsoft Enterprise-Datenschutzbestimmungen. Azure Stack enthält auch Open Source-Software, die nicht geändert wurde, um Telemetriedaten an Microsoft zu senden. Hier sehen Sie einige Beispiele für Azure Stack-Telemetriedaten:

- Informationen zur Bereitstellungsregistrierung
- Wenn eine Warnung geöffnet bzw. geschlossen wird
- Anzahl der Netzwerkressourcen

Zur Unterstützung des Telemetriedatenflusses muss Port 443 (HTTPS) in Ihrem Netzwerk geöffnet sein. Der Clientendpunkt ist https://vortex-win.data.microsoft.com.

Wenn Sie keine Telemetrie für Azure Stack bereitstellen möchten, können Sie diese Funktion auf dem Development Kit-Host und den virtuellen Computern der Infrastruktur wie nachfolgend beschrieben deaktivieren.

### <a name="turn-off-telemetry-on-the-development-kit-host-optional"></a>Deaktivieren der Telemetrie auf dem Host der Development Kit (optional)

>[!NOTE]
Wenn Sie Telemetrie für den Development Kit-Host deaktivieren möchten, muss dies vor dem Ausführen des Bereitstellungsskripts erfolgen.

Starten Sie „CloudBuilder.vhdx“ vor dem [Ausführen des Skripts „asdk-installer.ps1“]() zum Bereitstellen des Development Kit-Hosts, und führen Sie das folgende Skript in einem PowerShell-Fenster mit erhöhten Rechten aus:
```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host 
Set-ItemProperty-Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0'  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Das Festlegen von **AllowTelemetry** auf 0 deaktiviert Telemetrie für die Bereitstellung von Windows und Azure Stack. Nur kritische Sicherheitsereignisse vom Betriebssystem werden gesendet. Diese Einstellung steuert die Windows-Telemetrie für alle Hosts und virtuellen Computern der Infrastruktur und wird auf neue Knoten/virtuelle Computer erneut angewendet, wenn horizontale Hochskalierungen erfolgen.


### <a name="turn-off-telemetry-on-the-infrastructure-virtual-machines-optional"></a>Deaktivieren der Telemetrie auf den virtuellen Computern der Infrastruktur (optional)

Führen Sie nach erfolgreicher Bereitstellung das folgende Skript auf dem Development Kit-Host in einem PowerShell-Fenster mit erhöhten Rechten aus (als AzureStack\AzureStackAdmin-Benutzer):

```powershell
$AzSVMs= get-vm |  where {$_.Name -like "AzS-*"}
### Show current AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
### Set & Get updated AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {Set-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name "AllowTelemetry" -Value '0'}
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
```

Informationen zur Konfiguration der SQL Server-Telemetrie finden Sie unter [So konfigurieren Sie SQL Server 2016](https://support.microsoft.com/en-us/help/3153756/how-to-configure-sql-server-2016-to-send-feedback-to-microsoft).

### <a name="usage-reporting"></a>Nutzungsberichte

Bei der Registrierung wird Azure Stack auch für die Weiterleitung von Nutzungsinformationen an Azure konfiguriert. Nutzungsberichte werden unabhängig von Telemetriedaten gesteuert. Sie können Nutzungsberichte bei der [Registrierung](azure-stack-register.md) mithilfe des Skripts auf Github deaktivieren. Legen Sie einfach den Parameter **$reportUsage** auf **$false** fest.

Das Format der Nutzungsdaten ist unter [Report Azure Stack usage data to Azure](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-usage-reporting) (Melden von Azure Stack-Nutzungsdaten an Azure) beschrieben. Für Benutzer des Azure Stack Development Kits fallen keine Kosten an. Diese Funktion ist im Development Kit enthalten, sodass Sie die Funktionsweise von Nutzungsberichten testen können. 


## <a name="next-steps"></a>Nächste Schritte
[Herunterladen des Bereitstellungspakets für das Azure Stack Development Kit](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Bereitstellen des Azure Stack Development Kits](azure-stack-run-powershell-script.md)


