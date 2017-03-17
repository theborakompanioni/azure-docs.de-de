---
title: Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics | Microsoft Docs
description: Dieser Artikel beschreibt die Schritte zum Verbinden von Configuration Manager mit Log Analytics und den Beginn der Analyse von Daten.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: aca83d2de9247bedacce0fb03efe141d903d8605
ms.openlocfilehash: f93d37ad5be4bf7fdc78d83ec68ba56a427b3e35
ms.lasthandoff: 02/23/2017


---
# <a name="connect-configuration-manager-to-log-analytics"></a>Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics
Sie können den System Center Configuration Manager mit Log Analytics in OMS verbinden, um Gerätesammlungsdaten zu synchronisieren. Dadurch werden Daten aus der Configuration Manager-Hierarchie in OMS verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Log Analytics unterstützt System Center Configuration Manager Current Branch, Version 1606 und höher.  

## <a name="configuration-overview"></a>Konfigurationsübersicht
In den folgenden Schritte wird der Prozess zum Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics zusammengefasst.  

1. Registrieren Sie im Azure-Verwaltungsportal den Configuration Manager als Webanwendung und/oder Web API-App, und stellen Sie sicher, dass Sie die Client-ID und den geheimen Clientschlüssel aus der Registrierung von Azure Active Directory haben. Weitere Informationen zu diesem Schritt finden Sie unter [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md).
2. Im Azure-Verwaltungsportal [stellen Sie den Configuration Manager (der registrierten Web-App) mit Zugriffsberechtigung für OMS bereit](#provide-configuration-manager-with-permissions-to-oms).
3. Im Configuration Manager [fügen Sie eine Verbindung mit dem Assistenten zum Hinzufügen von OMS-Verbindungen hinzu](#add-an-oms-connection-to-configuration-manager).
4. Im Configuration Manager [aktualisieren Sie die Verbindungseigenschaften](#update-oms-connection-properties), wenn das Kennwort oder der geheime Clientschlüssel abläuft oder verloren geht.
5. Mit Informationen über das OMS-Portal [laden und installieren Sie den Microsoft Monitoring Agent](#download-and-install-the-agent) auf dem Computer, auf dem die Configuration Manager Dienstverbindungspunkt-Standortsystemrolle ausgeführt wird. Der Agent sendet die Configuration Manager-Daten zu OMS.
6. In Log Analytics [importieren Sie Sammlungen von Configuration Manager](#import-collections) als Computergruppen.
7. In Log Analytics zeigen Sie die Daten von Configuration Manager als [Computergruppen](log-analytics-computer-groups.md) an.

Erfahren Sie mehr zum Herstellen einer Verbindung von Configuration Manager zu OMS unter [Sync data from Configuration Manager to the Microsoft Operations Management Suite (Synchronisieren von Daten aus Configuration Manager für die Microsoft Operations Management Suite)](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Bereitstellen von Configuration Manager mit Berechtigungen für OMS
Das folgende Verfahren gibt dem Azure-Verwaltungsportal Zugriffsberechtigungen für OMS. Sie müssen Benutzern in der Ressourcengruppe insbesondere die *Rolle „Mitwirkender“* erteilen, damit das Azure-Verwaltungsportal eine Verbindung zwischen Configuration Manager und OMS herstellen kann.

> [!NOTE]
> Sie müssen in OMS Berechtigungen für Configuration Manager angeben. Andernfalls erhalten Sie eine Fehlermeldung, wenn Sie den Konfigurations-Assistenten in Configuration Manager verwenden.
>
>

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und klicken Sie auf **Durchsuchen** > **Log Analytics (OMS)**, um das Blatt „Log Analytics (OMS)“ zu öffnen.  
2. Auf dem Blatt **Log Analytics (OMS)** klicken Sie auf **Hinzufügen** zum Öffnen des Blatts **OMS-Arbeitsbereich**.  
   ![OMS-Blatt](./media/log-analytics-sccm/sccm-azure01.png)
3. Auf dem Blatt **OMS-Arbeitsbereich** geben Sie die folgenden Informationen an, und klicken Sie anschließend auf **OK**.

   * **OMS-Arbeitsbereich**
   * **Abonnement**
   * **Ressourcengruppe**
   * **Standort**
   * **Preisstufe**  
     ![OMS-Blatt](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > Das obige Beispiel erstellt eine neue Ressourcengruppe. Die Ressourcengruppe wird in diesem Beispiel nur dazu verwendet, dem Configuration Manager Berechtigungen für den OMS-Arbeitsbereich zu geben.
     >
     >
4. Klicken Sie auf **Durchsuchen** > **Ressourcengruppen** zum Öffnen des Blatts **Ressourcengruppen**.
5. Klicken SiIm Blatt **Ressourcengruppen** auf die Ressourcengruppe, die Sie oben erstellt haben, um das Blatt „Einstellungen“ &lt;Ressourcengruppenname&gt; zu öffnen.  
   ![Blatt „Einstellungen“ Ressourcengruppe](./media/log-analytics-sccm/sccm-azure03.png)
6. Klicken Sie im Blatt „Einstellungen“ &lt;Ressourcengruppenname&gt; zum Öffnen des Benutzerblatts &lt;Ressourcengruppenname&gt; auf Access Control (IAM).  
   ![Benutzerblatt Ressourcengruppe](./media/log-analytics-sccm/sccm-azure04.png)  
7. Klicken Sie auf dem Benutzerblatt &lt;Ressourcengruppenname&gt; auf **Hinzufügen** zum Öffnen des Blatts **Zugriff hinzufügen**.
8. Klicken Sie auf dem Blatt **Zugriff hinzufügen** auf **Rolle auswählen**, und wählen Sie anschließend eine Rolle für den **Mitwirkenden** aus.  
   ![Auswählen einer Rolle](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klicken Sie auf **Hinzufügen von Benutzern**, wählen Sie den Configuration Manager-Benutzer, klicken Sie auf **Auswählen**, und klicken Sie anschließend auf **OK**.  
   ![Hinzufügen von Benutzern](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Hinzufügen einer OMS-Verbindung zu Configuration Manager
Zum Hinzufügen einer OMS-Verbindung muss die Configuration Manager-Umgebung einen [Dienstverbindungspunkt](https://technet.microsoft.com/library/mt627781.aspx) für den Online-Modus konfiguriert haben.

1. Wählen Sie im Arbeitsbereich **Verwaltung** des Configuration Manager **OMS-Connector**. Daraufhin wird der **Assistent zum Hinzufügen einer OMS-Verbindung** geöffnet. Wählen Sie **Weiter**.
2. Auf dem Bildschirm **Allgemein** vergewissern Sie sich, dass Sie die folgenden Aktionen durchgeführt haben und dass Sie für jedes Element Detailinformationen haben, und wählen Sie anschließend **Weiter** aus.

   1. Im Azure-Verwaltungsportal haben Sie Configuration Manager als Webanwendung und/oder Web API-App registriert und Sie haben die [Client-ID von der Registrierung](../active-directory/active-directory-integrating-applications.md).
   2. Im Azure-Verwaltungsportal haben Sie einen geheimen App-Schlüssel für die registrierte Anwendung in Azure Active Directory erstellt.  
   3. Im Azure-Verwaltungsportal haben Sie der registrierten Web-App eine Zugriffsberechtigung für OMS bereitgestellt.  
      ![Verbindung mit der Seite „Allgemein“ des OMS-Assistenten](./media/log-analytics-sccm/sccm-console-general01.png)
3. Auf dem Bildschirm **Azure Active Directory** konfigurieren Sie Ihre Verbindungseinstellungen zu OMS durch Angabe von **Mandant**, **Client-ID**, und des **geheimen Clientschlüssels**, und wählen Sie anschließend **Weiter**.  
   ![Verbindung zum OMS-Assistenten der Azure Active Directory-Seite](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Wenn Sie alle anderen Verfahren erfolgreich abgeschlossen haben, erscheinen die Informationen auf dem Bildschirm **OMS-Verbindungskonfiguration** automatisch auf dieser Seite. Informationen für die Verbindungseinstellungen sollten für **Azure-Abonnement**, **Azure-Ressourcengruppe**, und **Operations Management Suite-Arbeitsbereich** angezeigt werden.  
   ![Verbindung zum OMS-Assistenten OMS-Verbindungsseite](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Der Assistent stellt eine Verbindung mit dem OMS-Dienst mithilfe der Informationen her, die Sie eingegeben haben. Wählen Sie die Gerätesammlungen, die Sie zur Synchronisation mit OMS verwenden möchten, und klicken Sie anschließend auf **Hinzufügen**.  
   ![Sammlungen auswählen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Überprüfen Sie die Verbindungseinstellungen auf dem Bildschirm **Zusammenfassung**, und wählen Sie **Weiter** aus. Der Bildschirm **Status** zeigt den Verbindungsstatus, sollte **vollständig** sein.

> [!NOTE]
> Sie müssen OMS mit der obersten Ebene in der Hierarchie verbinden. Wenn Sie OMS mit einem eigenständigen primären Standort verbinden und dann eine zentralen Verwaltung der Umgebung hinzufügen, müssen Sie die OMS-Verbindung innerhalb der neuen Hierarchie löschen und neu erstellen.
>
>

Nachdem Sie Configuration Manager mit OMS verknüpft haben, können Sie Sammlungen hinzufügen oder entfernen und die Eigenschaften der OMS-Verbindung anzeigen.

## <a name="update-oms-connection-properties"></a>Aktualisieren der OMS-Verbindungseigenschaften
Wenn das Kennwort oder der geheime Clientschlüssel abläuft oder verloren geht, müssen Sie die OMS-Verbindungseigenschaften manuell aktualisieren.

1. Navigieren Sie im Configuration Manager zu **Clouddienste**, und wählen Sie dann **OMS-Connector** zum Öffnen der Seite **OMS-Verbindungseigenschaften** aus.
2. Klicken Sie auf dieser Seite auf die Registerkarte **Azure Active Directory** zur Anzeige von **Mandant**, **Client-ID**, **Ablauf des geheimen Clientschlüssels**. **Überprüfen Sie** Ihren **geheimen Clientschlüssel**, falls er abgelaufen ist.

## <a name="download-and-install-the-agent"></a>Herunterladen und Installieren des Agents
1. Im OMS-Portal [laden Sie die Setup-Datei von OMS herunter](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Verwenden Sie eine der folgenden Methoden zum Installieren, und konfigurieren Sie den Agent auf dem Computer mit der Configuration Manager Dienstverbindungspunkt-Standortsystemrolle:
   * [Installieren Sie den Agent über Setup](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Installieren des Agents über die Befehlszeile](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Installieren des Agents mithilfe von DSC in Azure Automation](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Importieren von Sammlungen
Nachdem Sie eine OMS-Verbindung zum Configuration Manager hinzugefügt und den Agent auf dem Computer mit der Configuration Manager Dienstverbindungspunkt-Standortsystemrolle installiert haben, ist der nächste Schritt der Import von Sammlungen von Configuration Manager in OMS als Computergruppen.

Nachdem der Import aktiviert ist, werden die Informationen zur Sammlungsmitgliedschaft alle drei Stunden abgerufen, um die Sammlungsmitgliedschaft auf dem neuesten Stand zu halten. Sie können den Import jederzeit deaktivieren.

1. Klicken Sie im OMS-Portal auf **Einstellungen**.
2. Klicken Sie auf die Registerkarte **Computergruppen**, und klicken Sie anschließend auf die Registerkarte **SCCM**.
3. Wählen Sie **Configuration Manager-Sammlungsmitgliedschaften importieren**aus, und klicken Sie anschließend auf **Speichern**.  
   ![Computergruppen – Registerkarte „SCCM“](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Anzeigen von Daten aus Configuration Manager
Nachdem Sie eine OMS-Verbindung zum Configuration Manager hinzugefügt und den Agent auf dem Computer mit der Configuration Manager Dienstverbindungspunkt-Standortsystemrolle installiert haben, werden Daten vom Agent an OMS gesendet. In OMS werden die Configuration Manager-Sammlungen als [Computergruppen](log-analytics-computer-groups.md) angezeigt. Die Anzeige von Gruppen wählen Sie auf der Seite **Configuration Manager** unter **Computergruppen** in **Einstellungen** aus.

Nach dem Import der Sammlungen können Sie sehen, wie viele Computer mit Sammlungsmitgliedschaften erkannt wurden. Sie sehen auch die Anzahl der Sammlungen, die importiert wurden.

![Computergruppen – Registerkarte „SCCM“](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wenn Sie auf eine klicken, wird die Suche geöffnet, wobei entweder alle importierten Gruppen oder alle Computer angezeigt werden, die zu jeder Gruppe gehören. Mithilfe der [Protokollsuche](log-analytics-log-searches.md) können Sie detaillierte Analysen für Configuration Manager-Daten starten.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuche](log-analytics-log-searches.md) für ausführliche Informationen zu den Configuration Manager-Daten.

