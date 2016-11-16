---
title: "Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau] | Microsoft Docs"
description: "Mit den Lösungen für die VM-Verwaltung werden Ihre virtuellen Azure Resource Manager-Computer nach einem Zeitplan gestartet und beendet und mit Log Analytics proaktiv überwacht."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/07/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b0fec06e4a167e615381fca17def46923d9f0f1b


---
# <a name="startstop-vms-during-offhours-preview-solution-in-automation"></a>Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau] in Automation
Mit der Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau] können Sie Ihre Azure Resource Manager-VMs nach einem benutzerdefinierten Zeitplan starten und beenden. Außerdem können Sie die erfolgreiche Ausführung der Automation-Aufträge, mit denen die virtuellen Computer gestartet und beendet werden, mit OMS Log Analytics verfolgen.  

## <a name="prerequisites"></a>Voraussetzungen
* Für die Runbooks wird ein [Ausführendes Azure-Konto](automation-sec-configure-azure-runas-account.md) verwendet.  Das ausführende Konto ist die bevorzugte Authentifizierungsmethode, da anstelle eines Kennworts, das ablaufen oder sich häufig ändern kann, eine Zertifikatauthentifizierung verwendet wird.  
* Mit dieser Lösung können nur VMs verwaltet werden, die unter demselben Abonnement und in derselben Ressourcengruppe wie das Automation-Konto enthalten sind.  
* Diese Lösung wird nur in den folgenden Azure-Regionen bereitgestellt: „Australien, Südosten“, „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“.  Die Runbooks zum Verwalten des VM-Zeitplans können für VMs in jeder beliebigen Region verwendet werden.  
* Für das Senden von E-Mail-Benachrichtigungen, sobald die Runbooks zum Starten und Beenden von VMs abgeschlossen sind, ist ein Office 365-Unternehmensabonnement erforderlich.  

## <a name="solution-components"></a>Lösungskomponenten
Diese Lösung besteht aus den folgenden Ressourcen, die importiert und dem Automation-Konto hinzugefügt werden.

### <a name="runbooks"></a>Runbooks
| Runbook | Beschreibung |
| --- | --- |
| CleanSolution-MS-Mgmt-VM |Mit diesem Runbook werden alle enthaltenen Ressourcen und Zeitpläne entfernt, wenn Sie die Lösung aus Ihrem Abonnement löschen. |
| SendMailO365-MS-Mgmt |Für dieses Runbook wird eine E-Mail über Office 365 Exchange gesendet. |
| StartByResourceGroup-MS-Mgmt-VM |Dieses Runbook dient zum Starten von VMs (sowohl klassische als auch ARM-basierte VMs), die in einer Liste mit Azure-Ressourcengruppen enthalten sind. |
| StopByResourceGroup-MS-Mgmt-VM |Dieses Runbook dient zum Beenden von VMs (sowohl klassische als auch ARM-basierte VMs), die in einer Liste mit Azure-Ressourcengruppen enthalten sind. |

<br>

### <a name="variables"></a>Variablen
| Variable | Beschreibung |
| --- | --- |
| Runbook **SendMailO365-MS-Mgmt** | |
| SendMailO365-IsSendEmail-MS-Mgmt |Gibt an, ob mit den Runbooks „StartByResourceGroup-MS-Mgmt-VM“ und „StopByResourceGroup-MS-Mgmt-VM“ nach dem Abschluss des Vorgangs E-Mail-Benachrichtigungen gesendet werden können.  Wählen Sie **True**, um E-Mail-Warnungen zu aktivieren, und sonst **False**. Der Standardwert ist **False**. |
| Runbook **StartByResourceGroup-MS-Mgmt-VM** | |
| StartByResourceGroup-ExcludeList-MS-Mgmt-VM |Geben Sie die Namen der VMs ein, die vom Verwaltungsvorgang ausgeschlossen werden sollen. Trennen Sie die Namen per Semikolon (;). Bei den Werten wird die Groß-/Kleinschreibung berücksichtigt, und das Platzhalterzeichen (Sternchen) wird unterstützt. |
| StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt |Text, der am Anfang des E-Mail-Texts angefügt wird. |
| StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt |Gibt den Namen des Automation-Kontos an, in dem das E-Mail-Runbook enthalten ist.  **Ändern Sie diese Variable nicht.** |
| StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt |Gibt den Namen des E-Mail-Runbooks an.  Wird von den Runbooks „StartByResourceGroup-MS-Mgmt-VM“ und „StopByResourceGroup-MS-Mgmt-VM“ zum Senden von E-Mails verwendet.  **Ändern Sie diese Variable nicht.** |
| StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt |Gibt den Namen der Ressourcengruppe an, in der das E-Mail-Runbook enthalten ist.  **Ändern Sie diese Variable nicht.** |
| StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt |Gibt den Text für die Betreffzeile der E-Mail an. |
| StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt |Gibt die Empfänger der E-Mail an.  Trennen Sie Namen per Semikolon (;). |
| StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM |Geben Sie die Namen der VMs ein, die vom Verwaltungsvorgang ausgeschlossen werden sollen. Trennen Sie die Namen per Semikolon (;). Bei den Werten wird die Groß-/Kleinschreibung berücksichtigt, und das Platzhalterzeichen (Sternchen) wird unterstützt.  Mit dem Standardwert (Sternchen) werden alle Ressourcengruppen in das Abonnement einbezogen. |
| StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM |Gibt das Abonnement an, unter dem die VMs enthalten sind, die mit dieser Lösung verwaltet werden sollen.  Dies muss dasselbe Abonnement sein, in dem sich das Automation-Konto der Lösung befindet. |
| Runbook **StopByResourceGroup-MS-Mgmt-VM** | |
| StopByResourceGroup-ExcludeList-MS-Mgmt-VM |Geben Sie die Namen der VMs ein, die vom Verwaltungsvorgang ausgeschlossen werden sollen. Trennen Sie die Namen per Semikolon (;). Bei den Werten wird die Groß-/Kleinschreibung berücksichtigt, und das Platzhalterzeichen (Sternchen) wird unterstützt. |
| StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt |Text, der am Anfang des E-Mail-Texts angefügt wird. |
| StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt |Gibt den Namen des Automation-Kontos an, in dem das E-Mail-Runbook enthalten ist.  **Ändern Sie diese Variable nicht.** |
| StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt |Gibt den Namen der Ressourcengruppe an, in der das E-Mail-Runbook enthalten ist.  **Ändern Sie diese Variable nicht.** |
| StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt |Gibt den Text für die Betreffzeile der E-Mail an. |
| StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt |Gibt die Empfänger der E-Mail an.  Trennen Sie Namen per Semikolon (;). |
| StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM |Geben Sie die Namen der VMs ein, die vom Verwaltungsvorgang ausgeschlossen werden sollen. Trennen Sie die Namen per Semikolon (;). Bei den Werten wird die Groß-/Kleinschreibung berücksichtigt, und das Platzhalterzeichen (Sternchen) wird unterstützt.  Mit dem Standardwert (Sternchen) werden alle Ressourcengruppen in das Abonnement einbezogen. |
| StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM |Gibt das Abonnement an, unter dem die VMs enthalten sind, die mit dieser Lösung verwaltet werden sollen.  Dies muss dasselbe Abonnement sein, in dem sich das Automation-Konto der Lösung befindet. |

<br>

### <a name="schedules"></a>Zeitpläne
| Schedule | Beschreibung |
| --- | --- |
| StartByResourceGroup-Schedule-MS-Mgmt |Zeitplan für das Runbook „StartByResourceGroup“, mit dem das Starten von VMs durchgeführt wird, die von dieser Lösung verwaltet werden. |
| StopByResourceGroup-Schedule-MS-Mgmt |Zeitplan für das Runbook „StopByResourceGroup“, mit dem das Herunterfahren von VMs durchgeführt wird, die von dieser Lösung verwaltet werden. |

### <a name="credentials"></a>Anmeldeinformationen
| Anmeldeinformation | Beschreibung |
| --- | --- |
| O365Credential |Gibt ein gültiges Office 365-Benutzerkonto zum Senden von E-Mails an.  Ist nur erforderlich, wenn die Variable „SendMailO365-IsSendEmail-MS-Mgmt“ auf **True** festgelegt ist. |

## <a name="configuration"></a>Konfiguration
Führen Sie die folgenden Schritte aus, um die Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau] dem Automation-Konto hinzuzufügen und anschließend die Variablen zum Anpassen der Lösung zu konfigurieren.

1. Wählen Sie auf der Startseite im Azure-Portal die Kachel **Marketplace**.  Falls die Kachel nicht mehr auf der Startseite angeheftet sein sollte, wählen Sie links im Navigationsbereich die Option **Neu**.  
2. Geben Sie auf dem Blatt „Marketplace“ im Suchfeld **VM starten** ein, und wählen Sie in den Suchergebnissen dann die Lösung **Start/Stop VMs during off-hours [Preview]** (Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau]) aus.  
3. Sehen Sie sich auf dem Blatt **Start/Stop VMs during off-hours [Preview]** (Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau]) für die ausgewählte Lösung die Zusammenfassung an, und klicken Sie auf **Erstellen**.  
4. Das Blatt **Lösung hinzufügen** wird angezeigt, und Sie werden aufgefordert, die Lösung vor dem Importieren in Ihr Automation-Abonnement zu konfigurieren.<br><br> ![Blatt „Lösung hinzufügen“ der VM-Verwaltung](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5. Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Arbeitsbereich**. Hier wählen Sie einen OMS-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, unter dem sich das Automation-Konto befindet. Sie können aber auch einen neuen OMS-Arbeitsbereich erstellen.  Wenn Sie noch nicht über einen OMS-Arbeitsbereich verfügen, können Sie die Option **Neuen Arbeitsbereich erstellen** wählen und auf dem Blatt für den **OMS-Arbeitsbereich** wie folgt vorgehen: 
   
   * Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** an.
   * Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   * Unter **Ressourcengruppe** können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.  
   * Wählen Sie einen **Speicherort**aus.  Derzeit können nur die Standorte **Australien, Südosten**, **USA, Osten**, **Asien, Südosten** und **Europa, Westen** ausgewählt werden.
   * Wählen Sie einen **Tarif**aus.  Für die Lösung sind zwei OMS-Tarife verfügbar: kostenlos (Free) und kostenpflichtig.  Beim Free-Tarif gelten Beschränkungen für die Datenmenge, die pro Tag erfasst werden kann, die Aufbewahrungsdauer und die Laufzeit von Runbookaufträgen in Minuten.  Für den kostenpflichtigen OMS-Tarif gilt keine Beschränkung für die täglich erfasste Datenmenge.  
     
     > [!NOTE]
     > Der kostenpflichtige Standard-Tarif wird zwar als Option angezeigt, ist aber nicht verfügbar.  Wenn Sie diesen Tarif auswählen und mit der Erstellung der Lösung in Ihrem Abonnement fortfahren, tritt ein Fehler auf.  Dies wird behoben sein, wenn die Lösung offiziell veröffentlicht wird.<br>Bei Verwendung dieser Lösung werden nur Automation-Auftragsminuten und die Protokollerfassung genutzt.  Mit der Lösung werden Ihrer Umgebung keine weiteren OMS-Knoten hinzugefügt.  
     > 
     > 
6. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen auf dem Blatt **OMS-Arbeitsbereich** auf **Erstellen**.  Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  Sie gelangen wieder zum Blatt **Lösung hinzufügen**.  
7. Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Automation-Konto**.  Bei der Erstellung eines neuen OMS-Arbeitsbereichs müssen Sie auch ein neues Automation-Konto erstellen, das dem zuvor angegebenen neuen OMS-Arbeitsbereich zugeordnet wird, einschließlich Azure-Abonnement, -Ressourcengruppe und -Region.  Sie können die Option **Automation-Konto erstellen** wählen und auf dem Blatt **Automation-Konto hinzufügen** Folgendes angeben: 
   
   * Geben Sie im Feld **Name** den Namen des Automation-Kontos ein.
     
     Alle anderen Optionen werden basierend auf dem ausgewählten OMS-Arbeitsbereich automatisch aufgefüllt, und diese Optionen können nicht geändert werden.  Ein ausführendes Azure-Konto ist die Standardmethode für die Authentifizierung von Runbooks in dieser Lösung.  Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt.  Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 
     
     Andernfalls können Sie auch ein vorhandenes ausführendes Automation-Konto wählen.  Beachten Sie, dass das ausgewählte Konto nicht bereits mit einem anderen OMS-Arbeitsbereich verknüpft sein darf. Andernfalls wird auf dem Blatt eine Meldung mit einem entsprechenden Hinweis angezeigt.  Wenn bereits eine Verknüpfung besteht, müssen Sie ein anderes ausführendes Automation-Konto wählen oder ein neues Konto erstellen.<br><br> ![Automation-Konto bereits mit OMS-Arbeitsbereich verknüpft](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>
8. Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Konfiguration**. Das Blatt **Parameter** wird angezeigt.  Auf dem Blatt **Parameter** werden Sie zur Ausführung der folgenden Schritte aufgefordert:  
   
   * Geben Sie die **Zielressourcengruppennamen** an. Dies sind Ressourcengruppennamen, die mit dieser Lösung verwaltete VMs enthalten.  Sie können mehr als einen Namen eingeben, wenn Sie ein Semikolon als Trennzeichen verwenden (Groß-/Kleinschreibung wird berücksichtigt).  Die Verwendung eines Platzhalterzeichens wird unterstützt, wenn Sie einen Vorgang für VMs in allen Ressourcengruppen des Abonnements durchführen möchten.
   * Wählen Sie einen **Zeitplan** mit einem wiederkehrenden Datum und einer Uhrzeit zum Starten und Beenden der VMs in den Zielressourcengruppen.  
9. Wählen Sie **Erstellen**, nachdem Sie die Konfiguration der erforderlichen Anfangseinstellungen für die Lösung abgeschlossen haben.  Alle Einstellungen werden überprüft, und anschließend wird versucht, die Lösung unter Ihrem Abonnement bereitzustellen.  Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="collection-frequency"></a>Sammlungshäufigkeit
Die Daten des Automation-Auftragsprotokolls und -Auftragsdatenstroms werden alle fünf Minuten im OMS-Repository erfasst.  

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie die VM-Verwaltungslösung in Ihrem OMS-Arbeitsbereich hinzufügen, wird die Kachel **StartStopVM View** (StartStopVM-Ansicht) Ihrem OMS-Dashboard hinzugefügt.  Auf dieser Kachel werden eine Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Kachel „StartStopVM View“ (StartStopVM-Ansicht) der VM-Verwaltung](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In Ihrem Automation-Konto können Sie auf die Lösung zugreifen und sie verwalten, indem Sie die Kachel **Lösungen** wählen und dann auf dem Blatt **Lösungen** in der Liste die Lösung **Start-Stop-VM[Workspace]** wählen.<br><br> ![Liste mit Automation-Lösungen](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Nach dem Auswählen der Lösung wird das Blatt für die Lösung **Start-Stop-VM[Workspace]** angezeigt, dem Sie wichtige Details entnehmen können. Ein Beispiel hierfür ist die Kachel **StartStopVM**, wie in Ihrem OMS-Arbeitsbereich, auf der eine Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt werden, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Blatt für Automation-VM-Lösungen](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Hier können Sie auch Ihren OMS-Arbeitsbereich öffnen und eine weitere Analyse der Auftragsdatensätze durchführen.  Klicken Sie einfach auf **Alle Einstellungen**, und wählen Sie auf dem Blatt **Einstellungen** die Option **Schnellstart** und auf dem Blatt **Schnellstart** dann die Option **OMS-Portal**.   Eine neue Registerkarte bzw. eine neue Browsersitzung wird geöffnet, und Ihr OMS-Arbeitsbereich, der Ihrem Automation-Konto und -Abonnement zugeordnet ist, wird angezeigt.  

### <a name="configuring-email-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen
Um das Senden von E-Mail-Benachrichtigungen nach dem Starten und Beenden von VM-Runbooks zu aktivieren, müssen Sie die **O365Credential**-Anmeldeinformationen ändern und mindestens die folgenden Variablen verwenden:

* SendMailO365-IsSendEmail-MS-Mgmt
* StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
* StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

Führen Sie die folgenden Schritte aus, um die **O365Credential**-Anmeldeinformationen zu konfigurieren:

1. Klicken Sie im Automation-Konto oben im Fenster auf **Alle Einstellungen**. 
2. Wählen Sie auf dem Blatt **Einstellungen** im Abschnitt **Automation-Ressourcen** die Option **Objekte**. 
3. Wählen Sie auf dem Blatt **Objekte** die Kachel **Anmeldeinformationen** und auf dem Blatt **Anmeldeinformationen** die Option **O365Credential**.  
4. Geben Sie einen gültigen Office 365-Benutzernamen und das zugehörige Kennwort ein, und klicken Sie dann auf **Speichern**, um die Änderungen zu speichern.  

Führen Sie die folgenden Schritte aus, um die oben beschriebenen Variablen zu konfigurieren:

1. Klicken Sie im Automation-Konto oben im Fenster auf **Alle Einstellungen**. 
2. Wählen Sie auf dem Blatt **Einstellungen** im Abschnitt **Automation-Ressourcen** die Option **Objekte**. 
3. Wählen Sie auf dem Blatt **Objekte** die Kachel **Variablen** und dann auf dem Blatt **Variablen** die oben angegebene Variable. Ändern Sie anschließend deren Wert, indem Sie sich nach der Beschreibung oben im Abschnitt [Variablen](##variables) richten.  
4. Klicken Sie auf **Speichern**, um die Änderungen der Variablen zu speichern.   

### <a name="modifying-the-startup-and-shutdown-schedule"></a>Ändern des Zeitplans für das Starten und Herunterfahren
Bei der Verwaltung des Zeitplans für das Starten und Herunterfahren werden bei dieser Lösung die gleichen Schritte ausgeführt, die unter [Planen eines Runbooks in Azure Automation](automation-scheduling-a-runbook.md) beschrieben werden.  Beachten Sie, dass Sie die Zeitplankonfiguration nicht ändern können.  Sie müssen den vorhandenen Zeitplan deaktivieren und dann einen neuen erstellen und eine Verknüpfung mit dem Runbook **StartByResourceGroup-MS-Mgmt-VM** oder **StopByResourceGroup-MS-Mgmt-VM** einrichten, für das der Zeitplan gelten soll.   

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Automation erstellt zwei Arten von Datensätzen im OMS-Repository.

### <a name="job-logs"></a>Auftragsprotokolle
| Eigenschaft | Beschreibung |
| --- | --- |
| Aufrufer |Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| Kategorie |Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobLogs“. |
| CorrelationId |Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt. |
| JobId |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| operationName |Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“. |
| resourceId |Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto. |
| ResourceGroup |Gibt den Namen der Ressourcengruppe des Runbookauftrags an. |
| ResourceProvider |Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“. |
| ResourceType |Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto. |
| resultType |Der Status des Runbookauftrags.  Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Erfolgreich |
| resultDescription |Beschreibt den resultierenden Zustand des Runbookauftrags.  Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen |
| RunbookName |Gibt den Namen des Runbooks an. |
| SourceSystem |Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „:OpsManager“. |
| StreamType |Gibt den Typ des Ereignisses an. Mögliche Werte:<br>- Ausführlich<br>- Ausgabe<br>- Fehler<br>- Warnung |
| SubscriptionId |Gibt die Abonnement-ID des Auftrags an. |
| Time |Ausführungsdatum und -uhrzeit des Runbookauftrags. |

### <a name="job-streams"></a>Auftragsdatenströme
| Eigenschaft | Beschreibung |
| --- | --- |
| Aufrufer |Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| Kategorie |Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobStreams“. |
| JobId |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| operationName |Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“. |
| ResourceGroup |Gibt den Namen der Ressourcengruppe des Runbookauftrags an. |
| resourceId |Gibt die Ressourcen-ID in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto. |
| ResourceProvider |Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“. |
| ResourceType |Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto. |
| resultType |Das Ergebnis des Runbookauftrags zum Zeitpunkt der Ereignisgenerierung.  Mögliche Werte:<br>- InProgress |
| resultDescription |Enthält den Ausgabedatenstrom des Runbooks. |
| RunbookName |Der Name des Runbooks. |
| SourceSystem |Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „OpsManager“. |
| StreamType |Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich |
| Time |Ausführungsdatum und -uhrzeit des Runbookauftrags. |

Wenn Sie Protokollsuchen durchführen, bei denen Datensätze der Kategorie **JobLogs** oder **JobStreams** zurückgegeben werden, können Sie die Ansicht **JobLogs** oder **JobStreams** wählen. Es werden dann mehrere Kacheln angezeigt, mit denen die von der Suche zurückgegebenen Updates zusammengefasst werden.

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Auftragsdatensätze, die mit dieser Lösung erfasst wurden. 

| Abfrage | Beschreibung |
| --- | --- |
| Suchen nach Aufträgen für das Runbook „StartVM“, die erfolgreich abgeschlossen wurden |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; measure count() by JobId_g |
| Suchen nach Aufträgen für das Runbook „StopVM“, die erfolgreich abgeschlossen wurden |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; measure count() by JobId_g |
| Anzeigen des Auftragsstatus in Abhängigkeit der Zeit für die Runbooks „StartVM“ und „StopVM“ |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") |

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md)
* Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md)
* Weitere Informationen zu OMS Log Analytics und Datenerfassungsquellen finden Sie unter [Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics](../log-analytics/log-analytics-azure-storage.md)




<!--HONumber=Nov16_HO2-->


