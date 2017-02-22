---
title: Bereitstellen eines virtuellen SQL Server-Computers | Microsoft Docs
description: Verwenden Sie das Portal, um in Azure einen virtuellen SQL Server-Computer zu erstellen und eine Verbindung damit herzustellen. In diesem Tutorial wird der Resource Manager-Modus verwendet.
services: virtual-machines-windows
documentationcenter: na
author: rothja
editor: 
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 55a4b22c3bb097c688446a5ec22f60baecf44ffe
ms.openlocfilehash: 0dea81ef42d9225ee3780ffd2ad67a37c8a4a2ed


---
# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

In diesem umfassenden Tutorial wird veranschaulicht, wie Sie das Azure-Portal zum Bereitstellen eines virtuellen Computers mit SQL Server verwenden.

Der Azure Virtual Machine-Katalog enthält mehrere Images mit Microsoft SQL Server. Mit wenigen Klicks können Sie eines der SQL-VM-Images im Katalog auswählen und in Ihrer Azure-Umgebung bereitstellen.

In diesem Lernprogramm lernen Sie Folgendes:

* [Auswählen eines SQL-VM-Image aus dem Katalog](#select-a-sql-vm-image-from-the-gallery)
* [Konfigurieren und Erstellen der VM](#configure-the-vm)
* [Öffnen der VM mit Remotedesktop](#open-the-vm-with-remote-desktop)
* [Herstellen einer Remoteverbindung mit SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Auswählen eines SQL-VM-Image aus dem Katalog
1. Melden Sie sich mit Ihrem Konto beim [Azure-Portal](https://portal.azure.com) an.
   
   > [!NOTE]
   > Wenn Sie kein Azure-Konto haben, sollten Sie die Seite [Kostenlose einmonatige Testversion](https://azure.microsoft.com/pricing/free-trial/)besuchen.
   > 
   > 
2. Klicken Sie im Azure-Portal auf **Neu**. Das Portal wird mit dem Blatt **Neu** geöffnet. Die SQL Server-VM-Vorlagen befinden sich im Marketplace in der Gruppe **Compute**.
3. Klicken Sie auf dem Blatt **Neu** auf **Compute** und anschließend auf **Alle anzeigen**.
4. Geben Sie im Textfeld **Filter** die Zeichenfolge „SQL Server“ ein, und drücken Sie die EINGABETASTE.

   ![Azure-Blatt „Virtuelle Computer“](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Überprüfen Sie die verfügbaren SQL Server-Vorlagen.
6. Für jede Vorlage sind eine SQL Server-Version und ein Betriebssystem angegeben. Wählen Sie in der Liste ein Image aus. Sehen Sie sich dann das Blatt mit den Details an, auf dem eine Beschreibung des VM-Image enthalten ist.
   
   > [!NOTE]
   > SQL-VM-Images enthalten die Lizenzkosten für SQL Server in den minutenbezogenen Preisen der von Ihnen erstellten VM. Es gibt noch eine andere Option, nämlich die Verwendung von „Bring-Your-Own-License“ (BYOL), bei der Sie nur für die VM zahlen. Diese Imagenamen haben das Präfix {BYOL}. Weitere Informationen zu dieser Option finden Sie unter [Erste Schritte mit SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
   > 
   > 
7. Stellen Sie unter **Bereitstellungsmodell auswählen** sicher, dass **Resource Manager** ausgewählt ist. Das Resource Manager-Bereitstellungsmodell ist das empfohlene Bereitstellungsmodell für neue virtuelle Computer. Klicken Sie auf **Erstellen**.
   
    ![SQL-VM mit Resource Manager erstellen](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Konfigurieren der VM
Es sind fünf Blätter zum Konfigurieren eines virtuellen SQL Server-Computers vorhanden.

| Schritt | Beschreibung |
| --- | --- |
| **Grundlagen** |[Grundeinstellungen konfigurieren](#1-configure-basic-settings) |
| **Größe** |[VM-Größe auswählen](#2-choose-virtual-machine-size) |
| **Einstellungen** |[Optionale Features konfigurieren](#3-configure-optional-features) |
| **SQL Server-Einstellungen** |[SQL Server-Einstellungen konfigurieren](#4-configure-sql-server-settings) |
| **Zusammenfassung** |[Zusammenfassung prüfen](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Grundeinstellungen konfigurieren
Geben Sie auf dem Blatt **Grundeinstellungen** die folgenden Informationen an:

* Geben Sie einen eindeutigen **Namen**für den virtuellen Computer ein.
* Geben Sie einen **Benutzernamen** für das lokale Administratorkonto auf der VM ein. Dieses Konto wird auch der festen SQL Server-Serverrolle **sysadmin** hinzugefügt.
* Geben Sie ein sicheres **Kennwort**an.
* Wenn Sie über mehrere Abonnements verfügen, müssen Sie überprüfen, ob das Abonnement für die neue VM korrekt ist.
* Geben Sie im Feld **Ressourcengruppe** einen Namen für eine neue Ressourcengruppe ein. Klicken Sie alternativ auf **Vorhandene verwenden**, um eine vorhandene Ressourcengruppe zu verwenden. Bei einer Ressourcengruppe handelt es sich um eine Sammlung verwandter Ressourcen in Azure (virtuelle Computer, Speicherkonten, virtuelle Netzwerke usw.).
  
  > [!NOTE]
  > Die Verwendung einer neuen Ressourcengruppe ist hilfreich, wenn Sie SQL Server-Bereitstellungen in Azure testen oder sich gerade damit vertraut machen. Löschen Sie nach Beendigung des Tests die Ressourcengruppe, um die VM und alle Ressourcen, die dieser Ressourcengruppe zugeordnet sind, automatisch zu löschen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../../../azure-resource-manager/resource-group-overview.md).
  > 
  > 
* Wählen Sie einen **Standort** für die Bereitstellung aus.
* Klicken Sie auf **OK** , um die Einstellungen zu speichern.
  
    ![Blatt mit SQL-Grundeinstellungen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. VM-Größe auswählen
Wählen Sie beim Schritt **Größe** auf dem Blatt **Größe auswählen** eine Größe für den virtuellen Computer aus. Auf dem Blatt werden zuerst die empfohlenen Computergrößen angezeigt. Dies erfolgt basierend auf der von Ihnen ausgewählten Vorlage. Außerdem werden die monatlichen Kosten für die Ausführung der VM geschätzt.

![Optionen für die Größe der SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Für Produktionsworkloads empfehlen wir die Auswahl der Größe für einen virtuellen Computer, für die [Storage Premium](../../../storage/storage-premium-storage.md)unterstützt wird. Falls diese Leistungsebene für Sie nicht erforderlich ist, können Sie die Schaltfläche **Alle anzeigen** verwenden, um alle Optionen für die Computergröße anzuzeigen. Beispielsweise können Sie eine kleinere Computergröße für eine Entwicklungs- oder Testumgebung verwenden.

> [!NOTE]
> Weitere Informationen zu den Größen von virtuellen Computern finden Sie unter [Größen für virtuelle Computer](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu SQL Server-VM-Größen finden Sie unter [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md).
> 
> 

Wählen Sie Ihre Computergröße aus, und klicken Sie dann auf **Auswählen**.

## <a name="3-configure-optional-features"></a>3. Optionale Features konfigurieren
Konfigurieren Sie im Blatt **Einstellungen** die Bereiche Azure-Speicher, Netzwerk und Überwachung für den virtuellen Computer.

* Geben Sie unter **Speicher** als **Datenträgertyp** entweder „Standard“ oder „Premium (SSD)“ an. Für Produktionsworkloads wird Storage Premium empfohlen.

> [!NOTE]
> Wenn Sie „Premium (SSD)“ für eine Computergröße auswählen, für die Storage Premium nicht unterstützt wird, ändert sich Ihre Computergröße automatisch.  
> 
> 

* Unter **Speicherkonto**können Sie den automatisch angegebenen Speicherkontonamen übernehmen. Sie können auch auf **Speicherkonto** klicken, um ein vorhandenes Konto auszuwählen und den Speicherkontotyp zu konfigurieren. Standardmäßig erstellt Azure ein neues Speicherkonto mit lokal redundantem Speicher. Weitere Informationen zu Speicheroptionen finden Sie unter [Azure Storage-Replikation](../../../storage/storage-redundancy.md).
* Unter **Netzwerk**können Sie die automatisch angegebenen Werte übernehmen. Sie können auch auf jedes Feature klicken, um **Virtuelles Netzwerk**, **Subnetz**, **Öffentliche IP-Adresse** und **Netzwerksicherheitsgruppe** manuell zu konfigurieren. Behalten Sie die Standardwerte für dieses Tutorial bei.
* In Azure ist die **Überwachung** mit demselben Speicherkonto, das für den virtuellen Computer angegeben wurde, standardmäßig aktiviert. Sie können diese Einstellungen hier ändern.
* Geben Sie unter **Verfügbarkeitsgruppe**eine Verfügbarkeitsgruppe an. In diesem Tutorial können Sie hier **Keine**wählen. Falls Sie die Einrichtung von SQL AlwaysOn-Verfügbarkeitsgruppen planen, können Sie die Verfügbarkeit konfigurieren, um die Neuerstellung der virtuellen Maschine zu vermeiden.  Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Klicken Sie auf **OK**, wenn Sie mit dem Konfigurieren der Einstellungen fertig sind.

## <a name="4-configure-sql-server-settings"></a>4. SQL Server-Einstellungen konfigurieren
Konfigurieren Sie im Blatt **SQL Server-Einstellungen** die speziellen Einstellungen und Optimierungen für SQL Server. Die Einstellungen, die Sie für SQL Server konfigurieren können, lauten wie folgt:

| Einstellung |
| --- |
| [Konnektivität](#connectivity) |
| [Authentifizierung](#authentication) |
| [Speicherkonfiguration](#storage-configuration) |
| [Automatisiertes Patchen](#automated-patching) |
| [Automatisierte Sicherung](#automated-backup) |
| [Azure-Schlüsseltresor-Integration](#azure-key-vault-integration) |
| [R Services](#r-services) |

### <a name="connectivity"></a>Konnektivität
Geben Sie unter **SQL-Konnektivität**den Zugriffstyp an, den Sie für die SQL Server-Instanz auf dieser VM verwenden möchten. Wählen Sie für dieses Tutorial die Option **Öffentlich (Internet)** , um für Computer oder Dienste im Internet Verbindungen mit SQL Server zuzulassen. Wenn diese Option aktiviert ist, konfiguriert Azure die Firewall und die Netzwerksicherheitsgruppe automatisch, um Datenverkehr über Port 1433 zuzulassen.  

![SQL – Konnektivitätsoptionen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Um über das Internet eine Verbindung mit SQL Server herzustellen, müssen Sie auch die SQL Server-Authentifizierung aktivieren. Dies ist im nächsten Abschnitt beschrieben.

> [!NOTE]
> Es ist möglich, Ihrer SQL Server-VM weitere Einschränkungen für die Netzwerkkommunikation hinzuzufügen. Hierzu können Sie die Netzwerksicherheitsgruppe bearbeiten, nachdem die VM erstellt wurde. Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../../../virtual-network/virtual-networks-nsg.md)
> 
> 

Falls Sie die Verbindungen mit dem Datenbankmodul über das Internet nicht aktivieren möchten, wählen Sie eine der folgenden Optionen:

* **Lokal (nur innerhalb der VM)** , um Verbindungen zu SQL Server nur von innerhalb des virtuellen Computers zuzulassen.
* **Privat (innerhalb des Virtual Network)** , um Verbindungen zu SQL Server von Computern oder Diensten in demselben virtuellen Netzwerk zuzulassen.

> [!NOTE]
> Mit dem VM-Image für die SQL Server Express Edition wird das TCP/IP-Protokoll nicht automatisch aktiviert. Dies gilt auch für die Konnektivitätsoptionen „Öffentlich“ und „Privat“. Für die Express Edition müssen Sie den SQL Server-Konfigurations-Manager verwenden, um [das TCP/IP-Protokoll nach dem Erstellen der VM manuell zu aktivieren](#configure-sql-server-to-listen-on-the-tcp-protocol) .
> 
> 

Generell sollten Sie die Sicherheit erhöhen, indem Sie die restriktivste Konnektivität wählen, die für Ihr Szenario zulässig ist. Bei allen Optionen können Sie aber Netzwerksicherheitsgruppen-Regeln und die SQL-/Windows-Authentifizierung verwenden, um für Sicherheit zu sorgen.

**Port** wird standardmäßig 1433 verwendet. Sie können eine andere Portnummer angeben.
Weitere Informationen finden Sie unter [Verbinden mit einem virtuellen SQL Server-Computer (Resource Manager) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Authentifizierung
Wenn Sie die SQL Server-Authentifizierung benötigen, klicken Sie unter **Aktivieren** under **Aktivieren**besuchen.

![SQL Server-Authentifizierung](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Wenn Sie auf SQL Server über das Internet zugreifen möchten (Konnektivitätsoption „Öffentlich“), müssen Sie die SQL-Authentifizierung hier aktivieren. Für den öffentlichen Zugriff auf SQL Server muss die SQL-Authentifizierung verwendet werden.
> 
> 

Geben Sie beim Aktivieren der SQL Server-Authentifizierung **Anmeldename** und **Kennwort** an. Dieser Benutzername ist als Anmeldung für die SQL Server-Authentifizierung konfiguriert und ist Mitglied der festen Serverrolle **sysadmin** . Weitere Informationen zu Authentifizierungsmodi finden Sie unter [Auswählen eines Authentifizierungsmodus](http://msdn.microsoft.com/library/ms144284.aspx) .

Wenn Sie die SQL Server-Authentifizierung nicht aktivieren, können Sie das lokale Administratorkonto auf der VM verwenden, um die Verbindung mit der SQL Server-Instanz herzustellen.

### <a name="storage-configuration"></a>Speicherkonfiguration
Klicken Sie auf **Speicherkonfiguration** , um die Speicheranforderungen anzugeben.

![SQL – Speicherkonfiguration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Wenn Sie den Standardspeicher wählen, ist diese Option nicht verfügbar. Die automatische Speicheroptimierung ist nur für Storage Premium verfügbar.
> 
> 

Sie können die Anforderungen als Eingabe-/Ausgabevorgänge pro Sekunde (IOPS), Durchsatz in MB/s und Gesamtspeichergröße angeben. Konfigurieren Sie diese Werte mit den Schiebereglern. Das Portal berechnet basierend auf diesen Anforderungen automatisch die Anzahl der Datenträger.

Standardmäßig optimiert Azure den Speicher für 5.000 IOPS, 200 MB und 1 TB Speicherplatz. Sie können diese Speichereinstellungen basierend auf der Workload ändern. Wählen Sie unter **Speicher optimiert für**eine der folgenden Optionen:

* **Allgemein** ist die Standardeinstellung und bietet Unterstützung für die meisten Workloads.
* **Transaktional** wird der Speicher für herkömmliche OLTP-Datenbankworkloads optimiert.
* **Data Warehousing** wird der Speicher für Analyse- und Berichterstellungsworkloads optimiert.

> [!NOTE]
> Die Obergrenzen der Schieberegler variieren je nach ausgewählter Größe des virtuellen Computers.
> 
> 

### <a name="automated-patching"></a>Automatisiertes Patchen
**Automatisiertes Patchen** ist standardmäßig aktiviert. Beim automatisierten Patchen kann Azure automatisch Patches für SQL Server und das Betriebssystem anwenden. Geben Sie einen Wochentag, eine Uhrzeit und eine Dauer für das Wartungsfenster an. Azure führt das Patchen in diesem Wartungsfenster durch. Für die Zeitplanung des Wartungsfensters wird die Uhrzeit des VM-Gebietsschemas verwendet. Wenn Sie nicht möchten, dass SQL Server und das Betriebssystem automatisch gepatcht werden, klicken Sie auf **Deaktivieren**.  

![SQL – Automatisiertes Patchen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Weitere Informationen finden Sie unter [Automatisches Patchen für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatisierte Sicherung
Aktivieren Sie automatische Datenbanksicherungen für alle Datenbanken unter **Automatisierte Sicherung**. Die automatisierte Sicherung ist standardmäßig deaktiviert.

Wenn Sie die automatisierte SQL-Sicherung aktivieren, können Sie Folgendes konfigurieren:

* Aufbewahrungszeitraum (Tage) für Sicherungen
* Verwendetes Speicherkonto für Sicherungen
* Verschlüsselungsoption und Kennwort für Sicherungen
* Sichern von Systemdatenbanken
* Konfigurieren des Sicherungszeitplans

Klicken Sie auf **Aktivieren**, um die Sicherung zu verschlüsseln. Geben Sie dann das **Kennwort**an. Azure erstellt ein Zertifikat zum Verschlüsseln der Sicherungen und verwendet das angegebene Kennwort, um das Zertifikat zu schützen.

![SQL – Automatisierte Sicherung](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure-Schlüsseltresor-Integration
Klicken Sie zum Speichern von Sicherheitsgeheimnissen für die Verschlüsselung in Azure auf **Azure Key Vault-Integration** und dann auf **Aktivieren**.

![SQL – Azure-Schlüsseltresor-Integration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

In der folgenden Tabelle sind die Parameter aufgeführt, die zum Konfigurieren der Azure-Schlüsseltresor-Integration erforderlich sind.

| PARAMETER | Beschreibung | BEISPIEL |
| --- | --- | --- |
| **Schlüsseltresor-URL** |Gibt den Speicherort des Schlüsseltresors an. |https://contosokeyvault.vault.azure.net/ |
| **Prinzipalname** |Gibt den Namen des Azure Active Directory-Dienstprinzipals an. Dieser Name wird auch als Client-ID bezeichnet. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Geheimer Schlüssel des Prinzipals** |Der geheime Schlüssel des Azure Active Directory-Dienstprinzipals. Dieser geheime Schlüssel wird auch als geheimer Clientschlüssel bezeichnet. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Name der Anmeldeinformationen** |**Anmeldeinformationsname**: Die Azure-Schlüsseltresor-Integration erstellt Anmeldeinformationen in SQL Server, damit der virtuelle Computer Zugriff auf den Schlüsseltresor hat. Wählen Sie einen Namen für diese Anmeldeinformation. |mycred1 |

Weitere Informationen finden Sie unter [Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-ps-sql-keyvault.md).

Klicken Sie auf **OK**, wenn Sie mit dem Konfigurieren der SQL Server-Einstellungen fertig sind.

### <a name="r-services"></a>R Services
Für die SQL Server 2016 Enterprise Edition verfügen Sie über die Option zum Aktivieren von [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). Dies ermöglicht die Verwendung der erweiterten Analyse mit SQL Server 2016. Klicken Sie auf **Aktivieren** on the **SQL Server Settings** geöffnet.

![SQL Server R Services aktivieren](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

> [!NOTE]
> Für SQL Server-Images, bei denen es sich nicht um die 2016 Enterprise Edition handelt, ist die Option zum Aktivieren von R Services deaktiviert.
> 
> 

## <a name="5-review-the-summary"></a>5. Zusammenfassung prüfen
Überprüfen Sie auf dem Blatt **Zusammenfassung** die angezeigten Informationen, und klicken Sie auf **OK**, um SQL Server, die Ressourcengruppe und die für diesen virtuellen Computer angegebenen Ressourcen zu erstellen.

Sie können die Bereitstellung über das Azure-Portal überwachen. Auf der Schaltfläche **Benachrichtigungen** oben auf der Seite wird der grundlegende Status der Bereitstellung angezeigt.

> [!NOTE]
> Damit Sie sich einen Eindruck von Bereitstellungszeiten verschaffen können, habe ich eine SQL-VM für die Region „USA, Osten“ mit Standardeinstellungen bereitgestellt. Diese Testbereitstellung hat insgesamt 26 Minuten gedauert. Je nach Region und den gewählten Einstellungen kann es aber sein, dass die Bereitstellung bei Ihnen schneller oder langsamer geht.
> 
> 

## <a name="open-the-vm-with-remote-desktop"></a>Öffnen der VM mit Remotedesktop
Führen Sie die folgenden Schritte aus, um mit Remotedesktop eine Verbindung mit dem virtuellen Computer herzustellen:

1. Nachdem die Azure-VM erstellt wurde, wird auf dem Azure-Dashboard das Symbol für die VM angezeigt. Sie können auch darauf zugreifen, indem Sie die Liste mit den vorhandenen virtuellen Computern durchsuchen. Klicken Sie auf den neuen virtuellen SQL-Computer. Auf dem Blatt **Virtueller Computer** werden die Details zum virtuellen Computer angezeigt.
2. Klicken Sie oben auf dem Blatt **Virtueller Computer** auf **Verbinden**.
3. Im Browser wird eine RDP-Datei für die VM heruntergeladen. Öffnen Sie die RDP-Datei.
    ![Vom Remotedesktop zur SQL-VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
4. Sie werden von der Remotedesktopverbindung informiert, dass der Herausgeber dieser Remoteverbindung nicht identifiziert werden kann. Klicken Sie auf **Verbinden** , um den Vorgang fortzusetzen.
5. Klicken Sie im Dialogfeld **Windows-Sicherheit** auf **Anderes Konto verwenden**.
6. Geben Sie unter **Benutzername** den **\<user name>** ein, wobei <user name> der Benutzername ist, den Sie beim Konfigurieren der VM angegeben haben. Sie müssen vor dem Namen einen vorangestellten umgekehrten Schrägstrich hinzufügen.
7. Geben Sie das **Kennwort** ein, das Sie für diese VM konfiguriert haben, und klicken Sie dann auf **OK**, um die Verbindung herzustellen.
8. Wenn in einem weiteren Dialogfeld **Remotedesktopverbindung** gefragt wird, ob die Verbindung hergestellt werden soll, klicken Sie auf **Ja**.

Nachdem Sie eine Verbindung mit dem virtuellen SQL Server-Computer hergestellt haben, können Sie SQL Server Management Studio starten und mit Ihren Anmeldeinformationen für den lokalen Administrator eine Verbindung mit der Windows-Authentifizierung herstellen. Wenn Sie die SQL Server-Authentifizierung aktiviert haben, können Sie die Verbindung auch per SQL-Authentifizierung herstellen, indem Sie den während der Bereitstellung konfigurierten SQL-Benutzernamen und das Kennwort verwenden.

Durch den Zugriff auf den Computer können Sie die Computer- und SQL Server-Einstellungen je nach Ihren Anforderungen direkt ändern. Beispielsweise können Sie die Firewalleinstellungen konfigurieren oder die SQL Server-Konfigurationseinstellungen ändern.

## <a name="connect-to-sql-server-remotely"></a>Herstellen einer Remoteverbindung mit SQL Server
In diesem Tutorial haben wir den Zugriffstyp **Öffentlich** für den virtuellen Computer und die **SQL Server-Authentifizierung** ausgewählt. Mit diesen Einstellungen wurde der virtuelle Computer automatisch so konfiguriert, dass SQL Server-Verbindungen von jedem Client über das Internet zulässig sind (vorausgesetzt, diese verfügen über die richtige SQL-Anmeldung).

> [!NOTE]
> Wenn Sie bei der Bereitstellung nicht die Option „Öffentlich“ gewählt haben, müssen Sie weitere Schritte ausführen, um den Zugriff auf die SQL Server-Instanz über das Internet zu ermöglichen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem virtuellen SQL Server-Computer](virtual-machines-windows-sql-connect.md).
> 
> 

In den folgenden Abschnitten wird gezeigt, wie Sie eine Verbindung mit Ihrer SQL Server-Instanz auf der VM von einem anderen Computer über das Internet herstellen.

> [!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md) und [Häufig gestellte Fragen](virtual-machines-windows-sql-server-iaas-faq.md).

Eine Übersicht über SQL Server auf virtuellen Azure-Computern als Video finden Sie unter [Azure VM is the best platform for SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)(Azure VM ist die beste Plattform für SQL Server 2016).

[Sehen Sie sich den Lernpfad für SQL Server auf virtuellen Azure-Computern an](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .




<!--HONumber=Feb17_HO1-->


