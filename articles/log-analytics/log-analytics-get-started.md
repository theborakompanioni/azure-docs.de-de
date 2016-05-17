<properties
	pageTitle="Erste Schritte mit Log Analytics | Microsoft Azure"
	description="Log Analytics ist innerhalb weniger Minuten in der Microsoft Operations Management Suite (OMS) einsatzbereit."
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
	ms.date="04/28/2016"
	ms.author="banders"/>


# Erste Schritte mit Log Analytics

Log Analytics ist innerhalb weniger Minuten in der Microsoft Operations Management Suite (OMS) einsatzbereit. Bei der Erstellung eines OMS-Arbeitsbereichs, der einem Konto ähnelt, haben Sie zwei Möglichkeiten:

- Microsoft Operations Management Suite-Website
- Microsoft Azure-Abonnement

Sie können einen OMS-Arbeitsbereich mithilfe der OMS-Website erstellen. Alternativ dazu können Sie einen OMS-Arbeitsbereich mithilfe eines Microsoft Azure-Abonnements erstellen. Beide Arbeitsbereiche sind funktionell gleichwertig. Wenn Sie ein Azure-Abonnement nutzen, können Sie mit diesem Abonnement auch auf andere Azure-Dienste zugreifen. Unabhängig von der Methode, die Sie zum Erstellen des Arbeitsbereichs wählen, erstellen Sie den Arbeitsbereich entweder mit einem Microsoft-Konto oder einem Unternehmenskonto.

Hier wird die Vorgehensweise erläutert:

![Onboardingdiagramm](./media/log-analytics-get-started/oms-onboard-diagram.png)

## Anmelden in drei Schritten über die Operations Management Suite

1. Besuchen Sie die [Operations Management Suite](http://microsoft.com/oms)-Website, und klicken Sie auf **Kostenlos testen**. Melden Sie sich zur Verwendung mit Office 365 oder anderen Microsoft-Diensten mit Ihrem Microsoft-Konto, z. B. Outlook.com, oder mit einem vom Unternehmen oder der Bildungseinrichtung bereitgestellten Organisationskonto an.
2. Geben Sie einen eindeutigen Namen für den Arbeitsbereich ein. Ein Arbeitsbereich ist ein logischer Container, in dem die Verwaltungsdaten gespeichert werden. Er bietet eine Möglichkeit zum Partitionieren von Daten zwischen verschiedenen Teams in Ihrer Organisation, während die Daten für den Arbeitsbereich exklusiv sind. Geben Sie eine E-Mail-Adresse und die Region an, in der Ihre Daten gespeichert werden sollen. ![Erstellen eines Arbeitsbereichs und Verknüpfen von Abonnements](./media/log-analytics-get-started/oms-onboard-create-workspace-link.png)
3. Anschließend können Sie ein neues Azure-Abonnement oder eine Verknüpfung zu einem vorhandenen Azure-Abonnement erstellen. Wenn Sie mit der kostenlosen Testversion fortfahren wollen, klicken Sie auf **Nicht jetzt**.

Sie sind nun bereit, im Operations Management Suite-Portal loszulegen.

Unter [Verwalten des Zugriffs auf Log Analytics](log-analytics-manage-access.md) erfahren Sie mehr über das Einrichten des Arbeitsbereichs und das Verknüpfen vorhandener Azure-Konten mit Arbeitsbereichen, die mit der Operations Management Suite erstellt wurden.

## Schnelles Registrieren mit Microsoft Azure

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich an, durchsuchen Sie die Liste von Diensten, und wählen Sie dann **Log Analytics (OMS)**.![Azure-Portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klicken Sie auf **Hinzufügen**, und wählen Sie dann die Optionen für die folgenden Elemente:
    - Name für den **OMS-Arbeitsbereich**
    - **Abonnement**: Wenn Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement, das Sie dem neuen Arbeitsbereich zuordnen möchten.
    - **Ressourcengruppe**
    - **Standort**
    - **Tarif** ![Schnellerfassung](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klicken Sie auf **Erstellen**, und die Arbeitsbereichsdetails werden im Azure-Portal angezeigt. ![Informationen zum Arbeitsbereich](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Klicken Sie auf den Link zum **OMS-Portal**, um die Operations Management Suite-Website mit Ihrem neuen Arbeitsbereich zu öffnen.

Sie sind jetzt bereit, das Portal der Operations Management Suite zu nutzen.

Unter [Verwalten des Zugriffs auf Log Analytics](log-analytics-manage-access.md) erfahren Sie mehr über das Einrichten des Arbeitsbereichs und das Verknüpfen vorhandener Arbeitsbereiche, die Sie mit der Operations Management Suite für Azure-Abonnements erstellt haben.

## Erste Schritte mit dem Operations Management Suite-Portal
Um Lösungen zu wählen und die Server zu verbinden, die Sie verwalten möchten, klicken Sie auf die Kachel **Einstellungen**, und führen Sie die Schritte in diesem Abschnitt aus.

![Erste Schritte](./media/log-analytics-get-started/oms-onboard-get-started.png)

- **Lösungen hinzufügen**: Wählen Sie die Lösungen aus, die Sie nutzen möchten, und klicken Sie auf **Ausgewählte Lösungen hinzufügen**. ![Lösungen](./media/log-analytics-get-started/oms-onboard-solutions.png)
- **Datenquelle verbinden**: Wählen Sie die Vorgehensweise aus, um eine Verbindung mit der Serverumgebung zum Sammeln von Daten herzustellen:
    - Verbinden Sie alle Windows-Server oder -Clients direkt durch Installieren eines Agents.
    - Verwenden Sie System Center Operations Manager, um die Verwaltungsgruppen oder die gesamte Operations Manager-Bereitstellung anzufügen.
    - Verwenden Sie ein Azure-Speicherkonto, das mit der VM-Erweiterung zur Azure-Diagnose unter Windows oder Linux konfiguriert ist.![Datenquellen](./media/log-analytics-get-started/oms-onboard-data-sources.png)    
- **Protokolle hinzufügen** Konfigurieren Sie mindestens eine Datenquelle, um Ihre Daten aufzufüllen, und wählen Sie dann **Speichern**. Bei Ereignisprotokollen können Sie den Typ der Meldungen angeben, einschließlich Fehler-, Warn- und Informationsmeldungen zur Überwachung.    

    ![Protokolle](./media/log-analytics-get-started/oms-onboard-logs.png)


## Optional können Sie Server durch Installieren eines Agents direkt an die Operations Management Suite anschließen.
1. Klicken Sie auf die Kachel **Einstellungen** und auf die Registerkarte **Verbundene Quellen**, und klicken Sie dann auf **Windows-Agent herunterladen** für die Architektur des Computers, auf dem Sie die Installation durchführen möchten. Sie können den Agent nur unter Windows Server 2008 SP1 oder höher, oder unter Windows 7 SP1 oder höher installieren.
2. Installieren Sie den Agent auf einem oder mehreren Servern. Sie können Agents nacheinander oder mithilfe einer automatisierten Methode mit einem [benutzerdefinierten Skript](log-analytics-windows-agents.md) installieren, oder Sie nutzen eine bereits vorhandene Softwareverteilungslösung.
3. Nachdem Sie dem Lizenzvertrag zugestimmt und den Installationsordner ausgewählt haben, wählen Sie **Agent mit Microsoft Azure Operational Insights verbinden**. (OMS hieß vorher Operational Insights). ![Einrichtung des Agents](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Auf der nächsten Seite müssen Sie Ihre Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel eingeben. Ihre Arbeitsbereichs-ID und der Schlüssel werden auf dem Bildschirm angezeigt, auf dem Sie die Agent-Datei heruntergeladen haben. ![Agent-Schlüssel](./media/log-analytics-get-started/oms-onboard-mma-keys.png) ![Anfügen von Servern](./media/log-analytics-get-started/oms-onboard-key.png)
5. Während der Installation können Sie auf **Erweitert** klicken, um optional Ihren Proxyserver einzurichten und Authentifizierungsinformationen bereitzustellen. Klicken Sie auf die Schaltfläche **Weiter**, um zum Informationsbildschirm des Arbeitsbereichs zurückzukehren.
6. Klicken Sie auf **Weiter**, um Ihre Arbeitsbereichs-ID und den Schlüssel zu überprüfen. Wenn Fehler gefunden werden, können Sie auf **Zurück** klicken, um Korrekturen vorzunehmen. Wenn Ihr Arbeitsbereichs-ID und der Schlüssel erfolgreich überprüft wurden, klicken Sie zum Abschließen der Installation des Agents auf **Installieren**.
7. Melden Sie sich wieder im Operations Management Suite-Portal an, und klicken Sie auf der Übersichtsseite auf die Kachel **Einstellungen**. Ein grünes Häkchen wird angezeigt, wenn die Agents mit dem Operations Management Suite-Dienst kommunizieren. Zunächst wird dies ungefähr 5 bis 10 Minuten dauern.

>[AZURE.NOTE] Kapazitätsverwaltungs- und Konfigurationsbewertungs-Lösungen werden von Servern derzeit nicht unterstützt, die direkt mit der Operations Management Suite verbunden sind.


Sie können auch den Agent mit System Center Operations Manager 2012 SP1 und höher verbinden. Zu diesem Zweck wählen Sie **Verbinden des Agents mit System Center Operations Manager**. Wenn Sie diese Option auswählen, senden Sie Daten an den Dienst, ohne dass zusätzliche Hardware oder Last für Ihre Verwaltungsgruppen erforderlich ist.

Weitere Informationen zum Verbinden von Agents mit der Operations Management Suite finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md).

## Optional können Sie Server mithilfe von System Center Operations Manager verbinden.

1. Wählen Sie in der Operations Manager-Konsole **Verwaltung**.
2. Erweitern Sie den Knoten **Operational Insights**, und wählen Sie dann **Operational Insights-Verbindung**.

  >[AZURE.NOTE] Je nachdem, welches Updaterollup von SCOM Sie verwenden, wird Ihnen ein Knoten für *System Center Advisor*, *Operational Insights* oder die *Operations Management Suite* angezeigt.

3. Klicken Sie oben rechts auf den Link **Registrieren bei Operational Insights**, und befolgen Sie die Anweisungen.
4. Klicken Sie nach Abschluss des Registrierungsassistenten auf den Link **Computer/Gruppe hinzufügen**.
5. Im Dialogfeld **Computersuche** können Sie nach Computern oder Gruppen suchen, die von Operations Manager überwacht werden. Wählen Sie Computer oder Gruppen aus, die in Log Analytics aufgenommen werden sollen, klicken Sie auf **Hinzufügen** und dann auf **OK**. Sie können überprüfen, ob der OMS-Dienst Daten empfängt, indem Sie im Portal der Operations Management Suite zur Kachel **Nutzung** wechseln. Daten sollten in ungefähr 5 bis 10 Minuten angezeigt werden.

Weitere Informationen zum Verbinden von Operations Manager mit der Operations Management Suite erhalten Sie unter [Verbinden von Operations Manager mit Log Analytics](log-analytics-om-agents.md).

## Optional, können Sie Daten von Clouddiensten in Microsoft Azure analysieren.

Mit der Operations Management Suite können Sie schnell Ereignis- und IIS-Protokolle für Cloud-Dienste und virtuelle Computer durchsuchen, indem Sie die Diagnose für Azure Cloud-Dienste aktivieren. Weitere Erkenntnisse zu Ihren virtuellen Azure-Computern erhalten Sie auch durch Installieren von Microsoft Monitoring Agent. Weitere Informationen zum Konfigurieren der Azure-Umgebung für die Verwendung der Operations Management Suite erhalten Sie unter [Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics](log-analytics-azure-storage.md).


## Nächste Schritte

- [Fügen Sie Log Analytics-Lösungen aus dem Lösungskatalog hinzu](log-analytics-add-solutions.md), um Funktionen hinzuzufügen und Daten zu sammeln.
- Machen Sie sich mit [Protokollsuchvorgängen](log-analytics-log-searches.md) vertraut, um ausführliche Informationen anzuzeigen, die von Lösungen gesammelt werden.
- Verwenden Sie [Dashboards](log-analytics-dashboards.md) zum Speichern und Anzeigen eigener benutzerdefinierter Suchvorgänge.

<!---HONumber=AcomDC_0504_2016-->