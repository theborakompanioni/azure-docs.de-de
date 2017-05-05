---
title: Herstellen einer Verbindung zwischen Operations Manager und Log Analytics | Microsoft Docs
description: "Zur Bewahrung Ihrer bisherigen Investitionen in System Center Operations Manager sowie zur Nutzung erweiterter Funktionen mit Log Analytics können Sie Operations Manager mit Ihrem OMS-Arbeitsbereich verknüpfen."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: c0a988a11722cfefb242f573c5a3affe21e6b6b4
ms.lasthandoff: 04/20/2017

---

# <a name="connect-operations-manager-to-log-analytics"></a>Herstellen einer Verbindung zwischen Operations Manager und Log Analytics
Zur Bewahrung Ihrer bisherigen Investitionen in System Center Operations Manager sowie zur Nutzung erweiterter Funktionen mit Log Analytics können Sie Operations Manager mit Ihrem OMS-Arbeitsbereich verknüpfen.  Die Kombination der Möglichkeiten von OMS mit den Vorteilen von Operations Manager ermöglicht Folgendes:

* Weitere Verwendung der Integritätsüberwachung für Ihre IT-Dienste mit Operations Manager
* Verwaltung der Integration mit Ihren ITSM-Lösungen (mit Unterstützung von Incident Management und Problemverwaltung)
* Verwaltung des Lebenszyklus von Agents, die auf lokalen und öffentlichen Cloud-IaaS-VMs bereitgestellt und mit Operations Manager überwacht werden

Die Verknüpfung mit System Center Operations Manager kommt Ihrer Dienstbetriebsstrategie zugute, da Sie dadurch beim Sammeln, Speichern und Analysieren von Daten aus Operations Manager von der Geschwindigkeit und Effizienz von OMS profitieren.  OMS ist hilfreich beim Korrelieren und Ermitteln problembedingter Fehler sowie beim Ermitteln von Wiederholungen und unterstützt somit Ihren vorhandenen Problemverwaltungsprozess.   Die Flexibilität des Suchmoduls bei der Untersuchung von Leistungs-, Ereignis- und Warnungsdaten sowie umfassende Dashboards und Berichtsfunktionen zur sinnvollen Betrachtung der Daten machen OMS zu einer optimalen Ergänzung von Operations Manager.

Die Agents, die Daten an die Verwaltungsgruppe von Operations Manager melden, sammeln Daten von Ihren Servern auf der Grundlage der Log Analytics-Datenquellen und -Lösungen, die Sie in Ihrem OMS-Abonnement aktiviert haben.  Je nach aktivierter Lösung werden Daten aus diesen Lösungen entweder direkt von einem Operations Manager-Verwaltungsserver oder (aufgrund des Umfangs der Daten, die auf dem mit Agent verwalteten System gesammelt werden) direkt vom Agent an den OMS-Webdienst gesendet. Der Verwaltungsserver leitet die OMS-Daten direkt an den OMS-Webdienst weiter; sie werden gar nicht erst in die OperationsManager- oder OperationsManagerDW-Datenbank geschrieben.  Sollte die Verbindung zwischen einem Verwaltungsserver und dem OMS-Webdienst getrennt werden, werden die Daten lokal zwischengespeichert, bis die Kommunikation mit OMS wiederhergestellt ist.  Ist der Verwaltungsserver aufgrund einer geplanten Wartung oder eines ungeplanten Ausfalls offline, wird die Verbindung mit OMS von einem anderen Verwaltungsserver in der Verwaltungsgruppe übernommen.  

Das nachstehende Diagramm zeigt die Verbindung zwischen den Verwaltungsservern und Agents in einer Verwaltungsgruppe von System Center Operations Manager und OMS, einschließlich der Richtung und des Ports.   

![oms-operations-manager-integration-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Sofern es entsprechend Ihren IT-Sicherheitsrichtlinien unzulässig ist, dass Computer in Ihrem Netzwerk eine Internetverbindung herstellen, können die Verwaltungsserver so konfiguriert werden, dass sie eine Verbindung mit dem OMS-Gateway herstellen, um Konfigurationsinformationen zu empfangen und gesammelte Daten abhängig von der aktivierten Lösung zu senden.  Weitere Informationen und Schritte zum Konfigurieren der Operations Manager-Verwaltungsgruppe für die Kommunikation mit dem OMS-Dienst über ein OMS-Gateway finden Sie unter [Verbinden von Computern mit der OMS über das OMS-Gateway](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Systemanforderungen
Vergewissern Sie sich zunächst anhand der folgenden Informationen, dass Sie über die erforderlichen Komponenten verfügen.

* OMS unterstützt nur Operations Manager 2016, Operations Manager 2012 SP1 UR6 und Operations Manager 2012 R2 UR2 sowie jeweils höhere Versionen.  Proxyunterstützung wurde in Operations Manager 2012 SP1 UR7 und Operations Manager 2012 R2 UR3 hinzugefügt.
* Alle Operations Manager-Agents müssen die Mindestanforderungen in Bezug auf die Unterstützung erfüllen. Stellen Sie sicher, dass Agents über das mindestens erforderliche Update verfügen. Andernfalls schlägt der Datenverkehr für den Windows-Agent fehl, und das Operations Manager-Ereignisprotokoll enthält viele Fehler.
* Ein OMS-Abonnement.  Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Herstellen einer Verbindung zwischen Operations Manager und OMS
Führen Sie die folgenden Schritte aus, um Ihre Operations Manager-Verwaltungsgruppe mit einem Ihrer OMS-Arbeitsbereiche zu verknüpfen.

1. Wählen Sie in der Operations Manager-Konsole den Arbeitsbereich **Administration** aus.
2. Erweitern Sie den Knoten für Operations Management Suite, und klicken Sie auf **Verbindung**.
3. Klicken Sie auf den Link **Register to Operations Management Suite** (Bei Operations Management Suite registrieren).
4. Geben Sie auf der Seite **Operations Management Suite Onboarding Wizard (Assistent zum Integrieren von Operations Management Suite)** die E-Mail-Adresse oder Telefonnummer sowie das Kennwort des mit dem neuen OMS-Arbeitsbereich verknüpften Administratorkontos ein, und klicken Sie auf **Anmelden**.
5. Nach erfolgreicher Authentifizierung werden Sie auf der Seite **Operations Management Suite Onboarding Wizard: Select Workspace** (Assistent zum Integrieren von Operations Management Suite: Arbeitsbereich auswählen) zum Auswählen Ihres OMS-Arbeitsbereichs aufgefordert.  Falls Sie über mehrere Arbeitsbereiche verfügen, wählen Sie in der Dropdownliste den Arbeitsbereich aus, den Sie in der Operations Manager-Verwaltungsgruppe registrieren möchten, und klicken Sie anschließend auf **Weiter**.
   
   > [!NOTE]
   > Operations Manager unterstützt immer nur einen einzelnen OMS-Arbeitsbereich. Die Verbindung und Computer, die bei OMS mit dem vorherigen Arbeitsbereich registriert wurden, werden von OMS entfernt.
   > 
   > 
6. Überprüfen Sie auf der Seite **Assistent zum Integrieren von Operations Management Suite: Zusammenfassung** Ihre Einstellungen, und klicken Sie auf **Erstellen**, wenn die Einstellungen korrekt sind.
7. Klicken Sie auf der Seite **Operations Management Suite Onboarding Wizard: Finish (Assistent zum Integrieren von Operations Management Suite: Fertig stellen)** auf **Schließen**.

### <a name="add-agent-managed-computers"></a>Hinzufügen von mit Agent verwalteten Computern
Nach dem Konfigurieren der Integration in Ihren OMS-Arbeitsbereich wird lediglich eine Verbindung mit OMS hergestellt, es werden aber keine Daten von den Agents gesammelt, die Daten an Ihre Verwaltungsgruppe melden. Dazu müssen Sie zuerst festlegen, welche mit Agent verwalteten Computer Daten für Log Analytics sammeln sollen. Sie können die Computerobjekte entweder einzeln oder aber eine Gruppe mit Windows-Computerobjekten auswählen. Sie können keine Gruppe auswählen, die Instanzen einer anderen Klasse enthält (etwa logische Datenträger oder SQL-Datenbanken).

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Erweitern Sie den Knoten für Operations Management Suite, und klicken Sie auf **Verbindung**.
3. Klicken Sie unter der Überschrift „Aktionen“ (rechts im Bereich) auf den Link **Computer/Gruppe hinzufügen** .
4. Im Dialogfeld **Computersuche** können Sie nach Computern oder Gruppen suchen, die von Operations Manager überwacht werden. Wählen Sie Computer oder Gruppen aus, die in OMS aufgenommen werden sollen, und klicken Sie auf **Hinzufügen** und dann auf **OK**.

Computer und Gruppen, die unter Operations Management Suite zum Sammeln von Daten über den Knoten für verwaltete Computer konfiguriert sind, können in der Betriebskonsole im Arbeitsbereich **Administration** angezeigt werden.  Hier können Sie Computer und Gruppen nach Bedarf hinzufügen und entfernen.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Konfigurieren von OMS-Proxyeinstellungen in der Betriebskonsole
Führen Sie die folgenden Schritte aus, wenn sich zwischen Verwaltungsgruppe und OMS-Webdienst ein interner Proxyserver befindet.  Diese Einstellungen werden zentral über die Verwaltungsgruppe verwaltet und an mit Agent verwaltete Systeme verteilt, die im Datensammlungsbereich für OMS enthalten sind.  Dies ist von Vorteil, wenn bestimmte Lösungen den Verwaltungsserver umgehen und Daten direkt an den OMS-Webdienst senden.

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Erweitern Sie „Operations Management Suite“, und klicken Sie auf **Verbindungen**.
3. Klicken Sie in der Ansicht „OMS-Verbindung“ auf **Proxyserver konfigurieren**.
4. Wählen Sie auf der Seite **Operations Management Suite Wizard: Proxy Server (Operations Management Suite-Assistent: Proxyserver)** die Option **Proxyserver für Zugriff auf Operations Management Suite verwenden** aus, geben Sie die URL mit der Portnummer ein (z.B. http://corpproxy:80), und klicken Sie anschließend auf **Fertig stellen**.

Falls Ihr Proxyserver eine Authentifizierung erfordert, führen Sie die folgenden Schritte aus, um Anmeldeinformationen und Einstellungen zu konfigurieren, die an verwaltete Computer weitergegeben werden müssen, die in der Verwaltungsgruppe Daten an OMS melden.

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Wählen Sie unter **RunAs Configuration** (RunAs-Konfiguration) die Option **Profile** aus.
3. Öffnen Sie das Profil **System Center Advisor Run As Profile Proxy** .
4. Klicken Sie im Assistenten für das ausführende Profil auf „Hinzufügen“, um ein ausführendes Konto zu verwenden. Sie können ein neues [ausführendes Konto](https://technet.microsoft.com/library/hh321655.aspx) erstellen oder ein vorhandenes Konto verwenden. Dieses Konto muss über ausreichende Berechtigungen für die Weiterleitung über den Proxyserver verfügen.
5. Um das zu verwaltende Konto festzulegen, wählen Sie **A selected class, group, or object** (Einer bestimmten Klasse oder Gruppe bzw. eines bestimmten Objekts) aus, und klicken Sie auf **Select** (Auswählen). Klicken Sie anschließend auf **Group** (Gruppe), um das Feld **Group Search** (Gruppensuche) zu öffnen.
6. Suchen Sie nach **Microsoft System Center Advisor-Überwachungsservergruppe**, und wählen Sie sie aus.  Klicken Sie nach dem Auswählen der Gruppe auf **OK**, um das Feld **Gruppensuche** zu schließen.
7. Klicken Sie auf **OK**, um das Dialogfeld **Ausführendes Konto hinzufügen** zu schließen.
8. Klicken Sie auf **Speichern** , um den Assistenten abzuschließen und Ihre Änderungen zu speichern.

Nachdem die Verbindung erstellt wurde und Sie die Agents konfiguriert haben, die Daten sammeln und an OMS melden sollen, wird in der Verwaltungsgruppe die folgende Konfiguration angewendet (nicht zwingend in der hier angegebenen Reihenfolge):

* Das ausführende Konto **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** wird erstellt.  Es wird mit dem ausführenden Profil **Microsoft System Center Advisor Run As Profile Blob** (Microsoft System Center Advisor – ausführendes Profil – Blob) verknüpft und auf zwei Klassen ausgerichtet: **Collection Server** und **Operations Manager Management Group**.
* Zwei Connectors werden erstellt.  Der erste heißt **Microsoft.SystemCenter.Advisor.DataConnector** und wird automatisch mit einem Abonnement konfiguriert, das alle Warnungen, die von Instanzen aller Klassen in der Verwaltungsgruppe generiert werden, an OMS Log Analytics weiterleitet. Der zweite Connector heißt **Advisor Connector**und ist für die Kommunikation mit dem OMS-Webdienst und für die Weitergabe von Daten zuständig.
* Agents und Gruppen, die Sie zum Sammeln von Daten in der Verwaltungsgruppe ausgewählt haben, werden der **Microsoft System Center Advisor-Überwachungsservergruppe**hinzugefügt.

## <a name="management-pack-updates"></a>Management Pack-Updates
Nach Abschluss der Konfiguration stellt die Operations Manager-Verwaltungsgruppe eine Verbindung mit dem OMS-Dienst her.  Der Verwaltungsserver wird mit dem Webdienst synchronisiert und erhält aktualisierte Konfigurationsinformationen in Form von Management Packs für die aktivierten Lösungen, die mit Operations Manager verknüpft werden.   Operations Manager sucht nach Updates für diese Management Packs. Verfügbare Updates werden automatisch herunterladen und importiert.  Dieses Verhalten wird insbesondere durch zwei Regeln gesteuert:

* **Microsoft.SystemCenter.Advisor.MPUpdate** : Aktualisiert die grundlegenden OMS-Management Packs. Wird standardmäßig alle 12 Stunden ausgeführt.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** : Aktualisiert lösungsbezogene Management Packs, die in Ihrem Arbeitsbereich aktiviert sind. Wird standardmäßig alle fünf Minuten ausgeführt.

Sie können diese beiden Regeln überschreiben und so entweder das automatische Herunterladen verhindern, indem Sie sie deaktivieren, oder die Häufigkeit ändern, mit der der Verwaltungsserver eine Synchronisierung mit OMS durchführt, um zu ermitteln, ob ein neues Management Pack verfügbar ist und heruntergeladen werden soll.  Führen Sie die Schritte unter [How to Override a Rule or Monitor (Vorgehensweise: Überschreiben einer Regel oder Überwachung)](https://technet.microsoft.com/library/hh212869.aspx) aus, um einen Sekundenwert für den Häufigkeitsparameter (**Frequency**) anzugeben und so den Synchronisierungszeitplan zu ändern, oder ändern Sie den Aktivierungsparameter (**Enabled**), um die Regeln zu deaktivieren.  Verwenden Sie als Ziel für die Überschreibungen alle Objekte der Klasse „Operations Manager Management Group“.

Wenn Sie zur Steuerung von Management Pack-Versionen in Ihrer Produktionsverwaltungsgruppe weiterhin Ihren vorhandenen Änderungssteuerungsprozess verwenden möchten, können Sie die Regeln deaktivieren und sie während bestimmter Zeiten, in denen Updates zulässig sind, wieder aktivieren. Falls Ihre Umgebung eine Entwicklungs- oder QA-Verwaltungsgruppe mit Internetzugriff enthält, können Sie diese Verwaltungsgruppe zur Unterstützung dieses Szenarios mit einem OMS-Arbeitsbereich konfigurieren.  Dadurch können Sie die iterativen Versionen der OMS-Management Packs vor der Freigabe für die Produktionsverwaltungsgruppe überprüfen und evaluieren.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Umstellen einer Operations Manager-Gruppe auf einen neuen OMS-Arbeitsbereich
1. Melden Sie sich an Ihrem OMS-Abonnement an, und erstellen Sie in [Microsoft Operations Management Suite](http://oms.microsoft.com/)einen neuen Arbeitsbereich.
2. Öffnen Sie die Operations Manager-Konsole mit einem Konto, das Mitglied der Administratorrolle von Operations Manager ist, und wählen Sie den Arbeitsbereich **Administration** aus.
3. Erweitern Sie „Operations Management Suite“, und wählen Sie die Option **Verbindungen**.
4. Wählen Sie in der Mitte des Bereichs den Link **Re-configure Operations Management Suite** (Operations Management Suite neu konfigurieren) aus.
5. Führen Sie die Schritte unter **Operations Management Suite Onboarding Wizard** (Assistent zum Integrieren von Operations Management Suite) aus, und geben Sie die E-Mail-Adresse oder Telefonnummer und das Kennwort des mit dem neuen OMS-Arbeitsbereich verknüpften Administratorkontos ein.
   
   > [!NOTE]
   > Auf der Seite **Operations Management Suite Onboarding Wizard: Select Workspace** (Assistent zum Integrieren von Operations Management Suite: Arbeitsbereich auswählen) wird der vorhandene Arbeitsbereich angezeigt, der verwendet wird.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>Überprüfen der Operations Manager-Integration für OMS
Es gibt verschiedene Methoden, um zu überprüfen, ob die OMS-Integration in Operations Manager erfolgreich war.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Überprüfen der Integration über das OMS-Portal
1. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen** .
2. Wählen Sie die Option **Connected Sources**(Verbundene Quellen) aus.
3. In der Tabelle im Abschnitt für System Center Operations Manager sollten der Name der Verwaltungsgruppe sowie die Anzahl von Agents und die Angabe, wann zuletzt Daten empfangen wurden, angezeigt werden.
   
   ![oms-settings-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Beachten Sie den Wert **Arbeitsbereich-ID** links auf der Seite „Einstellungen“.  Sie verwenden ihn unten für die Überprüfung anhand der Operations Manager-Verwaltungsgruppe.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Überprüfen der Integration über die Betriebskonsole
1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Wählen Sie **Management Packs** aus, und geben Sie im Textfeld für die **Suche** die Zeichenfolge **Advisor** oder **Intelligence** ein.
3. Abhängig von den aktivierten Lösungen wird in den Suchergebnissen ein entsprechendes Management Pack aufgeführt.  Wenn Sie also etwa die Lösung „Alert Management“ aktiviert haben, enthält die Liste das Management Pack „Microsoft System Center Advisor Alert Management“.
4. Navigieren Sie in der Ansicht **Überwachung** zur Ansicht **Operations Management Suite\Integritätsstatus**.  Wählen Sie im Bereich **Management Server State** (Zustand des Verwaltungsservers) einen Verwaltungsserver aus. Stellen Sie dann im Bereich **Detailansicht** sicher, dass der Wert für die Eigenschaft **Authentication service URI** (Authentifizierungsdienst-URI) mit der OMS-Arbeitsbereich-ID übereinstimmt.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>Entfernen der Integration für OMS
Wenn Sie die Integration zwischen der Operations Manager-Verwaltungsgruppe und dem OMS-Arbeitsbereich nicht mehr benötigen, müssen Sie mehrere Schritte ausführen, um die Verbindung und Konfiguration in der Verwaltungsgruppe richtig zu entfernen. Mit dem folgenden Verfahren aktualisieren Sie den OMS-Arbeitsbereich, indem Sie nacheinander den Verweis auf Ihre Verwaltungsgruppe, die OMS-Connectors und die Management Packs zur Unterstützung von OMS löschen.   

Management Packs für die aktivierten Lösungen, die in Operations Manager integriert werden, sowie die für die Unterstützung der Integration im OMS-Dienst erforderlichen Management Packs können in der Verwaltungsgruppe nicht ohne Weiteres gelöscht werden.  Dies ist darauf zurückzuführen, dass einige der OMS-Management Packs Abhängigkeiten von anderen zugehörigen Management Packs aufweisen.  Um Management Packs mit einer Abhängigkeit von anderen Management Packs zu löschen, laden Sie das Skript zum [Entfernen eines Management Packs mit Abhängigkeiten](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) vom TechNet Script Center herunter.  

1. Öffnen Sie die Operations Manager-Befehlsshell mit einem Konto, das Mitglied der Administratorrolle von Operations Manager ist.
   
    > [!WARNING]
    > Vergewissern Sie sich, dass die Namen Ihrer benutzerdefinierten Management Packs nicht das Wort „Advisor“ oder „IntelligencePack“ enthalten, bevor Sie den Vorgang fortsetzen. Andernfalls werden diese mit den folgenden Schritten aus der Verwaltungsgruppe gelöscht.
    > 

2. Geben Sie an der Eingabeaufforderung der Befehlsshell `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Geben Sie anschließend `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. Zum Entfernen der restlichen Management Packs mit einer Abhängigkeit von anderen System Center Advisor-Management Packs verwenden Sie das Skript *RecursiveRemove.ps1*, das Sie zuvor vom TechNet Script Center heruntergeladen haben.  
 
    > [!NOTE]
    > Löschen Sie das Microsoft System Center Advisor- oder Microsoft System Center Advisor Internal-Management Pack nicht.  
    >  

5. Öffnen Sie die Operations Manager-Betriebskonsole mit einem Konto, das Mitglied der Administratorrolle von Operations Manager ist.
6. Wählen Sie unter **Verwaltung** den Knoten **Management Packs** aus, und geben Sie im **Suchfeld** den Text **Advisor** ein. Überprüfen Sie dann, ob die folgenden Management Packs noch in die Verwaltungsgruppe importiert werden:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor – Intern
7. Klicken Sie im OMS-Portal auf die Kachel **Einstellungen** .
8. Wählen Sie die Option **Connected Sources**(Verbundene Quellen).
9. In der Tabelle sollte im Abschnitt „System Center Operations Manager“ der Name der Verwaltungsgruppe angezeigt werden, die Sie aus dem Arbeitsbereich entfernen möchten.  Klicken Sie unter der Spalte **Last Data** (Letzte Daten) auf **Entfernen**.  
   
    > [!NOTE]
    > Der Link zum **Entfernen** wird erst angezeigt, wenn für 14 Tage keine Aktivität der verbundenen Verwaltungsgruppe erkannt wird.  
    > 

10. In einem Fenster werden Sie dazu aufgefordert zu bestätigen, dass Sie die Entfernung durchführen möchten.  Klicken Sie auf **Ja** , um den Vorgang fortzusetzen. 

Gehen Sie wie folgt vor, um den Connector „Microsoft.SystemCenter.Advisor.DataConnector“ und den Connector „Advisor Connector“ zu löschen: Speichern Sie das unten angegebene PowerShell-Skript auf Ihrem Computer, und führen Sie es aus, indem Sie die folgenden Beispiele verwenden.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Wenn der Computer, auf dem Sie dieses Skript ausführen, kein Verwaltungsserver ist, sollte darauf die Operations Manager-Befehlsshell installiert sein. Dies hängt von der Version Ihrer Verwaltungsgruppe ab.
> 
> 

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Wenn Sie planen, die Verwaltungsgruppe später wieder mit einem OMS-Arbeitsbereich zu verbinden, müssen Sie die Management Pack-Datei `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` aus dem letzten Updaterollup importieren, das Sie auf die Verwaltungsgruppe angewendet haben.  Sie finden diese Datei im Ordner `%ProgramFiles%\Microsoft System Center 2012` oder `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>Nächste Schritte
* [Fügen Sie Log Analytics-Lösungen aus dem Lösungskatalog hinzu](log-analytics-add-solutions.md) , um Funktionen hinzuzufügen und Daten zu sammeln.
* [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](log-analytics-proxy-firewall.md) beschreibt, wie Sie vorgehen müssen, damit Agents mit dem Log Analytics-Dienst kommunizieren können, wenn Ihre Organisation einen Proxyserver oder eine Firewall verwendet.


