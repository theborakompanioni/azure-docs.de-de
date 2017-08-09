---
title: Erste Schritte mit der Azure-Protokollintegration | Microsoft Docs
description: "Hier erfahren Sie, wie Sie den Azure-Protokollintegrationsdienst installieren und Protokolle aus Azure Storage sowie Azure-Überwachungsprotokolle und Azure Security Center-Warnungen integrieren."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: a5c51817688140cc2778602b4c1d5184ae4729a0
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure-Protokollintegration mit Azure-Diagnoseprotokollierung und Windows-Ereignisweiterleitung
Mit der Azure-Protokollintegration (AzLog) können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren. Diese Integration bietet ein einheitliches Sicherheitsdashboard für alle Ihre Ressourcen (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse im Zusammenhang mit Ihren Anwendungen aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.
>[!NOTE]
Weitere Informationen zur Azure-Protokollintegration finden Sie in der [Übersicht zur Azure-Protokollintegration](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Dieser Artikel hilft Ihnen bei den ersten Schritten mit der Azure-Protokollintegration, indem er sich auf die Installation des Azlog-Diensts und die Integration des Diensts mit der Azure-Diagnose konzentriert. Der Azure-Protokollintegrationsdienst ist dann in der Lage, Protokollinformationen von Windows-Ereignissen aus dem Windows-Sicherheitsereigniskanal von virtuellen Computern zu erfassen, die in Azure IaaS bereitgestellt werden. Dies ist der „Ereignisweiterleitung“ sehr ähnlich, die Sie möglicherweise lokale verwendet haben.

>[!NOTE]
>Die Möglichkeit, die Ausgabe der Azure-Protokollintegration in SIEM einzubringen, wird von SIEM selbst bereitgestellt. Weitere Informationen finden Sie unter [Integrieren der Azure-Protokollintegration mit lokalem SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

Der Azure-Protokollintegrationsdienst wird auf einem physischen oder virtuellen Computer ausgeführt, der Windows Server 2008 R2 oder höher als Betriebssystem verwendet (vorzugsweise Windows Server 2012 R2 oder Windows Server 2016).

Der physische Computer kann lokal ausgeführt werden (oder am Standort eines Hosters). Wenn Sie den Azure-Protokollintegrationsdienst auf einem virtuellen Computer ausführen, kann sich dieser virtuelle Computer an einem lokalen Standort oder in einer öffentlichen Cloud wie Microsoft Azure befinden.

Der physische oder virtuelle Computer, der den Azure-Protokollintegrationsdienst ausführt, erfordert Netzwerkverbindungen mit der öffentlichen Azure-Cloud. Die Schritte in diesem Artikel enthalten Details zur Konfiguration.

## <a name="prerequisites"></a>Voraussetzungen
Die Installation von AzLog erfordert mindestens die folgenden Elemente:
* Ein **Azure-Abonnement**. Falls Sie über kein Abonnement verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren.
* Ein **Speicherkonto**, das für die Azure-Diagnoseprotokollierung verwendet werden kann (Sie können ein vorkonfiguriertes Speicherkonto verwenden oder ein neues erstellen – weiter unten in diesem Artikel wird gezeigt, wie das Speicherkonto konfiguriert wird).
  >[!NOTE]
  Je nach Szenario ist möglicherweise kein Speicherkonto erforderlich. Für das in diesem Artikel beschriebene Azure-Diagnose-Szenario ist eines erforderlich.
* **Zwei Systeme**: Ein Computer, der den Azure-Protokollintegrationsdienst ausführt und ein Computer, der überwacht wird und dessen Protokollinformationen an den Computer mit dem Azlog-Dienst gesendet werden.
   * Ein zu überwachender Computer – dies ist ein virtueller Computer, der als [virtueller Azure-Computer](../virtual-machines/virtual-machines-windows-overview.md) ausführt wird.
   * Einen Computer, auf dem der Azure-Protokollintegrationsdienst ausgeführt wird. Dieser Computer erfasst alle Protokollinformationen, die später in Ihr SIEM importiert werden.
    * Dieses System kann sich an einem lokalen Standort oder in Microsoft Azure befinden.  
    * Es muss eine x64-Version von Windows Server 2008 R2 SP1 oder höher ausgeführt werden und .NET 4.5.1 installiert sein. Sie können die installierte .NET-Version ermitteln, indem Sie den Artikel [Gewusst wie: Feststellen, welche Versionen von .NET Framework installiert sind](https://msdn.microsoft.com/library/hh925568) lesen.  
    Es muss eine Verbindung mit dem Azure-Speicherkonto vorhanden sein, das für die Azure-Diagnoseprotokollierung verwendet wird. Anweisungen zum Bestätigen dieser Verbindung werden später in diesem Artikel bereitgestellt.

Eine kurze Demonstration des Vorgangs zum Erstellen eines virtuellen Computers mit dem Azure-Portal finden Sie im folgenden Video.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung
Während Sie die Azure-Protokollintegration testen, können Sie ein beliebiges System verwenden, das die Mindestanforderungen zum Betriebssystem erfüllt. Allerdings kann es die Auslastung für eine Produktionsumgebung erforderlich machen, das zentrale oder horizontale Hochskalieren einzuplanen.

Sie können mehrere Instanzen des Azure-Protokollintegrationdiensts ausführen (eine Instanz pro physischem oder virtuellem Computer), wenn das Ereignisvolumen entsprechend hoch ist. Zudem können Sie für Speicherkonten der Azure-Diagnose einen Lastenausgleich für Windows (WAD) durchführen und die Anzahl von Abonnements, die für die Instanzen bereitgestellt werden, muss auf Ihrer Kapazität basieren.
>[!NOTE]
Zu diesem Zeitpunkt liegen keine bestimmten Empfehlungen dazu vor, wann Instanzen von Computern für die Azure-Protokollintegration (d. h. Computer, auf denen der Azure-Protokollintegrationsdienst ausgeführt wird) oder für Speicherkonten bzw. Abonnements horizontal hochskaliert werden müssen. Die Entscheidungen hinsichtlich der Skalierung sollten auf Ihren Beobachtungen zur Leistung in den einzelnen Bereichen basieren.

Sie haben auch die Möglichkeit, den Azure-Protokollintegrationsdienst zentral hochzuskalieren, um die Leistung zu verbessern. Die folgenden Leistungsmetriken können Ihnen dabei helfen, die Größe von Computern zu bestimmen, die Sie dafür auswählen, den Azure-Protokollintegrationsdienst auszuführen:
* Auf einem Computer mit acht Prozessoren (Kernen) kann eine einzelne Azlog Integrator-Instanz pro Tag etwa 24 Millionen Ereignisse (rund eine Million pro Stunde) verarbeiten.

* Auf einem Computer mit vier Prozessoren (Kernen) kann eine einzelne Azlog Integrator-Instanz pro Tag etwa 1,5 Millionen Ereignisse (rund 62.500 pro Stunde) verarbeiten.

## <a name="install-azure-log-integration"></a>Installieren der Azure-Protokollintegration
Sie müssen die Installationsdatei für die [Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324) herunterladen, um die Azure-Protokollintegration installieren zu können. Führen Sie die Setuproutine aus, und entscheiden Sie dann, ob Sie Telemetriedaten an Microsoft senden möchten.  

![Installationsbildschirm mit aktiviertem Kontrollkästchen für Telemetrie](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Es wird empfohlen, dass Sie Microsoft das Sammeln von Telemetriedaten gestatten. Sie können die Erfassung von Telemetriedaten ausschalten, indem Sie diese Option deaktivieren.
>


Der Dienst „Azure-Protokollintegration“ sammelt Telemetriedaten des Computers, auf dem er installiert ist.  

Folgende Telemetriedaten werden erfasst:

* Ausnahmen, die während der Ausführung der Azure-Protokollintegration auftreten
* Metriken zu der Anzahl von verarbeiteten Abfragen und Ereignissen
* Statistiken zur Verwendung von Azlog.exe-Befehlszeilenoptionen

Der Installationsvorgang wird im folgenden Video behandelt.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Schritte nach der Installation und zur Validierung
Nach Abschluss der grundlegenden Setuproutine sind Sie bereit, die auf die Installation folgenden Schritte sowie die Validierungsschritte auszuführen:
1. Öffnen Sie ein PowerShell-Fenster mit erhöhten Rechten, und navigieren Sie zu **C:\Programme\Microsoft Azure Log Integration**.
2. Der erste Schritt, den Sie ausführen müssen, ist das Abrufen der importierten AzLog-Cmdlets. Sie können dies durch Ausführen des Skripts **LoadAzlogModule.ps1** erreichen (beachten Sie den „\“ im folgenden Befehl). Geben Sie **.\LoadAzlogModule.ps1** ein, und drücken Sie die **EINGABETASTE**.  
Es sollte jetzt etwas angezeigt werden, das der folgenden Abbildung ähnelt. </br></br>
![Installationsbildschirm mit aktiviertem Kontrollkästchen für Telemetrie](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Jetzt müssen Sie „AzLog“ konfigurieren, um eine bestimmte Azure-Umgebung zu verwenden. Eine „Azure-Umgebung“ ist der „Typ“ des Rechenzentrums für die Azure-Cloud, mit dem Sie arbeiten möchten. Obwohl es zu diesem Zeitpunkt mehrere Azure-Umgebungen gibt, sind die relevanten Optionen derzeit entweder **AzureCloud** oder **AzureUSGovernment**.   Stellen Sie in Ihrer PowerShell-Umgebung mit erhöhten Rechten sicher, dass Sie sich in **C:\Programme\Microsoft Azure Log Integration\** befinden. </br></br>
    Führen Sie dort den folgenden Befehl aus: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud`` (für Azure Commercial)

      >[!NOTE]
      Wenn der Befehl erfolgreich ausgeführt wird, erhalten Sie kein Feedback.  Wenn Sie die Azure-Cloud der US-Regierung verwenden möchten, würden Sie **AzureUSGovernment** (für die -Name-Variable) verwenden. Andere Azure-Clouds werden zu diesem Zeitpunkt nicht unterstützt.  
4. Bevor Sie ein System überwachen können, benötigen Sie den Namen des Speicherkontos, das für die Azure-Diagnose verwendet wird.  Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, und suchen Sie nach dem virtuellen Computer, den Sie überwachen möchten. Wählen Sie im Abschnitt **Eigenschaften** die Option **Diagnoseeinstellungen** aus.  Klicken Sie auf **Agent**, und notieren Sie sich den angegebenen Namen des Speicherkontos. Dieser Kontoname ist in einem späteren Schritt erforderlich.
![Azure-Diagnoseeinstellungen](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Azure-Diagnoseeinstellungen](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Wenn die Überwachung während der Erstellung des virtuellen Computers nicht aktiviert wurde, erhalten Sie die Möglichkeit, sie zu aktivieren (wie oben gezeigt).
5. Jetzt wenden wir uns wieder dem Computer für die Azure-Protokollintegration zu. Wir müssen überprüfen, ob Sie auf dem System, auf dem Sie die Azure-Protokollintegration installiert haben, über die Verbindung zum Speicherkonto verfügen. Der physische oder virtuelle Computer, auf dem der Azure-Protokollintegrationsdienst ausgeführt wird, benötigt Zugriff auf das Speicherkonto, um gemäß der Konfiguration auf den einzelnen überwachten Systemen von der Azure-Diagnose protokollierte Informationen abzurufen.  
  1. Sie können Azure-Speicher-Explorer [hier](http://storageexplorer.com/) herunterladen.
  2. Führen Sie die Setuproutine durch.
  3. Nach Abschluss der Installation klicken Sie auf **Weiter**, und lassen Sie das Kontrollkästchen **Microsoft Azure-Speicher-Explorer starten** aktiviert.  
  4. Melden Sie sich bei Azure an.
  5. Überprüfen Sie, ob das Speicherkonto angezeigt wird, das Sie für Azure-Diagnose konfiguriert haben.  
![Speicherkonten](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Beachten Sie, dass unter den Speicherkonten einige Optionen vorhanden sind. Eine dieser Optionen ist **Tabellen**. Unter **Tabellen** sollte eine Option mit der Bezeichnung **WADWindowsEventLogsTable** angezeigt werden. </br></br>
   ![Speicherkonten](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integrieren der Azure-Diagnoseprotokollierung
In diesem Schritt konfigurieren Sie den Computer, auf dem der Azure-Protokollintegrationsdienst ausgeführt wird, um eine Verbindung mit dem Speicherkonto herzustellen, das die Protokolldateien enthält.
Zum Abschließen dieses Schritts sind einige Dinge vorab erforderlich.  
* **FriendlyNameForSource**: Dies ist ein Anzeigename, den Sie auf das Speicherkonto anwenden können, dass Sie für den virtuellen Computer zum Speichern der Informationen von der Azure-Diagnose konfiguriert haben.
* **StorageAccountName**: Dies ist der Name des Speicherkontos, das Sie beim Konfigurieren der Azure-Diagnose angegeben haben.  
* **StorageKey**: Dies ist der Speicherschlüssel für das Speicherkonto, in dem die Azure-Diagnoseinformationen für diesen virtuellen Computer gespeichert werden.  

Führen Sie die folgenden Schritte aus, um den Speicherschlüssel zu erhalten:
 1. Navigieren Sie zum [Azure-Portal](http://portal.azure.com).
 2. Scrollen Sie im Navigationsbereich der Azure-Konsole zum unteren Rand, und klicken Sie dann auf **Weitere Dienste**.

 ![Weitere Dienste](./media/security-azure-log-integration-get-started/more-services.png)
 3. Geben Sie **Speicher** in das Textfeld **Filter** ein. Klicken Sie auf **Speicherkonten** (dies wird nach der Eingabe von **Speicher** angezeigt).

   ![Filterfeld](./media/security-azure-log-integration-get-started/filter.png)
 4. Es wird eine Liste der Speicherkonten angezeigt. Doppelklicken Sie auf das Konto, das Sie dem Protokollspeicher zugewiesen haben.

   ![Liste der Speicherkonten](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Klicken Sie im Abschnitt **Einstellungen** auf **Zugriffsschlüssel**.

  ![Zugriffsschlüssel](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopieren Sie **Schlüssel1**, und speichern Sie ihn an einem sicheren Ort, auf den Sie im nächsten Schritt zugreifen können.

   ![Zwei Zugriffsschlüssel](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Öffnen Sie auf dem Server, auf dem Sie die Azure-Protokollintegration installiert haben, ein Eingabeaufforderungsfenster mit erhöhten Rechten (beachten Sie, dass wir hier ein Eingabeaufforderungsfenster mit erhöhten Rechten verwenden, keine PowerShell-Konsole mit erhöhten Rechten).
 8. Navigieren Sie zu **C:\Programme\Microsoft Azure Log Integration**
 9. Führen Sie ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` aus. </br> Beispiel: ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``. Soll in der Ereignis-XML die Abonnement-ID angezeigt werden, fügen Sie am Ende des Anzeigenamens die Abonnement-ID an: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` oder beispielsweise .``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Warten Sie bis zu 60 Minuten, und sehen Sie sich dann die aus dem Speicherkonto abgerufenen Ereignisse an. Öffnen Sie hierzu **Ereignisanzeige > Windows-Protokolle > Weitergeleitete Ereignisse** im Azlog Integrator.

Im folgenden Video werden die oben beschriebenen Schritte gezeigt.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Was, wenn im Ordner „Weitergeleitete Ereignisse“ keine Daten angezeigt werden?
Sollten im Ordner **Weitergeleitete Ereignisse** nach einer Stunde immer noch keine Daten angezeigt werden, gehen Sie wie folgt vor:

1. Überprüfen Sie den Computer, auf dem der Azure-Protokollintegrationsdienst ausgeführt wird, und vergewissern Sie sich, dass er auf Azure zugreifen kann. Öffnen Sie zum Testen der Konnektivität das [Azure-Portal](http://portal.azure.com) über den Browser.
2. Vergewissern Sie sich, dass das Benutzerkonto **Azlog** über Schreibberechtigungen für den Ordner **users\Azlog** verfügt.
  <ol type="a">
   <li>Öffnen Sie **Windows-Explorer** .</li>
  <li> Navigieren Sie zu **c:\users** .</li>
  <li> Klicken Sie mit der rechten Maustaste auf **c:\users\Azlog** .</li>
  <li> Klicken Sie auf **Sicherheit**  .</li>
  <li> Klicken Sie auf **NT Service\Azlog**, und überprüfen Sie die Berechtigungen für das Konto. Wenn das Konto auf dieser Registerkarte fehlt oder die entsprechenden Berechtigungen aktuell nicht angezeigt werden, können Sie dem Konto auf dieser Registerkarte die erforderlichen Rechte erteilen.</li>
  </ol>
3. Vergewissern Sie sich, dass das Speicherkonto, das mithilfe des Befehls **Azlog source add** hinzugefügt wurde, beim Ausführen des Befehls **Azlog source list** aufgeführt wird.
4. Überprüfen Sie unter **Ereignisanzeige > Windows-Protokolle > Anwendung**, ob von der Azure-Protokollintegration möglicherweise Fehler gemeldet wurden.


Wenn während der Installation und Konfiguration Probleme auftreten, öffnen Sie eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md), und wählen Sie **Protokollintegration** als den Dienst aus, für den Sie Support anfordern.

Eine weitere Supportoption ist das [MSDN-Forum für die Azure-Protokollintegration](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Hier kann sich die Community gegenseitig mit Fragen, Antworten, Tipps und Tricks zur optimalen Verwendung der Azure-Protokollintegration unterstützen. Darüber hinaus überwacht das Team für die Azure-Protokollintegration dieses Forum und bietet nach Möglichkeit entsprechende Hilfe an.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-Protokollintegration finden Sie in folgenden Dokumenten:

* [Microsoft Azure Log Integration for Azure logs (Microsoft Azure-Protokoll-Integration für Azure-Protokolle)](https://www.microsoft.com/download/details.aspx?id=53324): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Introduction to Azure log integration](security-azure-log-integration-overview.md) (Einführung in die Azure-Protokollintegration): Dieses Dokument stellt die Azure-Protokollintegration, die wichtigsten Funktionen sowie die Funktionsweise vor.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird. Dies ist die derzeit geltende Empfehlung für die Konfiguration von SIEM-Komponenten. Holen Sie zunächst weitere Informationen bei Ihrem SIEM-Hersteller ein.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration](../security-center/security-center-integrating-alerts-with-log-integration.md): In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Aktivitätsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
* [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Vorgänge Ihrer Azure-Ressourcen verschaffen können.

