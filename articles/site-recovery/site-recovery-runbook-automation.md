---
title: "Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen | Microsoft Docs"
description: "In diesem Artikel erfahren Sie, wie Sie mit Azure Site Recovery und Azure Automation Wiederherstellungspläne erweitern und komplexe Aufgaben bei der Wiederherstellung zu Azure durchführen können."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: required
ms.date: 10/23/2016
ms.author: ruturajd@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: 9f9a71cd7c6688dfd42dcb7ce52847f2016daf58


---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen
Dieses Lernprogramm beschreibt die Integration von Azure Site Recovery und Azure Automation, um die Erweiterung von Wiederherstellungsplänen zu ermöglichen. Wiederherstellungspläne können die Wiederherstellung Ihrer durch Azure Site Recovery geschützten virtuellen Computer sowohl bei der Replikation zur sekundären Cloud als auch bei der Replikation zu Azure orchestrieren. Darüber hinaus tragen sie zu einer **durchgängig exakten**, **wiederholbaren**, und **automatisierten** Wiederherstellung bei. Beim Failover virtueller Computer zu Azure erweitert die Azure Automation-Integration die Wiederherstellungspläne und ermöglicht die Ausführung von Runbooks für beeindruckende Automatisierungsaufgaben.

Falls Sie noch nicht mit Azure Automation vertraut sind, können Sie sich [hier](https://azure.microsoft.com/services/automation/) registrieren, und [hier](https://azure.microsoft.com/documentation/scripts/) die entsprechenden Beispielskripts herunterladen. Weitere Informationen zu [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) sowie zur Orchestrierung der Wiederherstellung in Azure mithilfe von Wiederherstellungsplänen finden Sie [hier](https://azure.microsoft.com/blog/?p=166264).

In diesem Lernprogramm erfahren Sie, wie Sie Azure Automation-Runbooks in Wiederherstellungspläne integrieren. Wir automatisieren einfache Aufgaben, für die zuvor ein manueller Eingriff erforderlich war, und konvertieren eine Wiederherstellung mit mehreren Schritten in eine Wiederherstellungsaktion, die sich mit nur einem Klick durchführen lässt. Außerdem erfahren Sie, wie Sie Probleme in einem einfachen Skript beheben können.

## <a name="customize-the-recovery-plan"></a>Anpassen des Wiederherstellungsplans
1. Zuerst öffnen wir das Ressourcenblatt des Wiederherstellungsplans. Sie sehen, dass für den Wiederherstellungsplan zwei virtuelle Computer zur Wiederherstellung hinzugefügt wurden.

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
- - -
1. Klicken Sie auf die Schaltfläche „Anpassen“, um mit dem Hinzufügen eines Runbooks zu beginnen. Das Blatt zum Anpassen des Wiederherstellungsplans wird geöffnet.

    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


1. Klicken Sie mit der rechten Maustaste auf die Startgruppe 1, und wählen Sie die Option „Nachfolgende Aktion hinzufügen“.
2. Wählen Sie auf dem neuen Blatt ein Skript aus.
3. Geben Sie dem Skript den Namen „Hello World“.
4. Wählen Sie einen Automation-Kontonamen aus. Dies ist das Azure Automation-Konto. Beachten Sie, dass sich dieses Konto an einem beliebigen geografischen Standort befinden kann, aber unter demselben Abonnement wie der Site Recovery-Tresor vorhanden sein muss.
5. Wählen Sie im Automation-Konto ein Runbook aus. Dies ist das Skript, das während der Ausführung des Wiederherstellungsplans nach der Wiederherstellung der ersten Gruppe ausgeführt wird.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)
6. Wählen Sie „OK“, um das Skript zu speichern. Das Skript wird der Gruppe „Gruppe 1: Start“ für nachfolgende Aktionen hinzugefügt.

    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Wichtige Punkte beim Hinzufügen eines Skripts
1. Sie können mit der rechten Maustaste auf das Skript klicken und „Schritt löschen“ oder „Skript aktualisieren“ wählen.
2. Ein Skript kann unter Azure während des Failovers aus der lokalen Umgebung nach Azure ausgeführt werden, und die Ausführung unter Azure als Skript der primären Seite ist vor dem Herunterfahren während des Failbacks von Azure in die lokale Umgebung möglich.
3. Wenn ein Skript ausgeführt wird, wird ein Wiederherstellungsplan-Kontext eingefügt.

Hier sehen Sie ein Beispiel für die Kontextvariable:

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Die folgende Tabelle enthält jeweils den Namen und die Beschreibung der einzelnen Variablen im Kontext:

| **Variablenname** | **Beschreibung** |
| --- | --- |
| RecoveryPlanName |Name des ausgeführten Plans. Ermöglicht die Durchführung von Aktionen auf der Grundlage des Namens mithilfe des gleichen Skripts. |
| FailoverType |Gibt an, ob es sich um ein Testfailover, um ein geplantes Failover oder um ein nicht geplantes Failover handelt. |
| FailoverDirection |Gibt an, ob die Wiederherstellung auf den primären oder sekundären Standort erfolgt. |
| GroupID |Identifiziert die Gruppennummer innerhalb des Wiederherstellungsplans, wenn der Plan ausgeführt wird |
| VmMap |Array mit allen virtuellen Computern in der Gruppe. |
| VMMap-Schlüssel |Eindeutiger Schlüssel (GUID) für jeden virtuellen Computer. Die GUID entspricht der VMM-ID des virtuellen Computers, sofern zutreffend. |
| RoleName |Name der Azure-VM, die wiederhergestellt wird. |
| CloudServiceName |Name des Azure Cloud-Diensts, unter dem der virtuelle Computer erstellt wird. |
| CloudServiceName (im Resource Manager-Bereitstellungsmodell) |Name der Azure-Ressourcengruppen, unter der der virtuelle Computer erstellt wird. |

## <a name="using-complex-variables-per-recovery-plan"></a>Verwenden von komplexen Variablen pro Wiederherstellungsplan
Es kann sein, dass für ein Runbook mehr Informationen als nur der RecoveryPlanContext erforderlich sind. Es gibt zum Übergeben eines Parameters an ein Runbook keinen alleinigen Mechanismus. Falls Sie dasselbe Skript über mehrere Wiederherstellungspläne hinweg verwenden möchten, können Sie aber die Wiederherstellungsplankontext-Variable „RecoveryPlanName“ und das unten beschriebene experimentelle Verfahren einsetzen, um eine komplexe Azure Automation-Variable in einem Runbook zu nutzen. Im Beispiel unten wird veranschaulicht, wie Sie drei verschiedene komplexe Variablenassets erstellen und im Runbook basierend auf dem Namen des Wiederherstellungsplans verwenden können.

Stellen Sie sich vor, dass Sie drei zusätzliche Parameter in einem Runbook nutzen möchten. Wir codieren sie in einem JSON-Formular: {"Var1":"testautomation","Var2":"Unplanned","Var3":"PrimaryToSecondary"}

Verwenden Sie die [komplexe AA-Variable](../automation/automation-variables.md#variable-types), um ein neues Automation-Asset zu erstellen.
Geben Sie der Variablen den Namen „<RecoveryPlanName>-params“.
Sie können diesen [Verweis zum Erstellen einer komplexen Variablen](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) verwenden.

Benennen Sie die Variable für unterschiedliche Wiederherstellungspläne nach dem folgenden Muster:

1. recoveryPlanName1>-params
2. recoveryPlanName2>-params
3. recoveryPlanName3>-params

Verweisen Sie im Skript dann wie folgt auf die Parameter:

1. Rufen Sie den Namen des Wiederherstellungsplans aus der Variablen „$rpname = $Recoveryplancontext“ ab.
2. Rufen Sie das Asset über „$paramValue = "$($rpname)-params"“ ab.
3. Verwenden Sie dies als komplexe Variable für den Wiederherstellungsplan, indem Sie „Get-AzureAutomationVariable [-AutomationAccountName] <String> -Name $paramValue“ aufrufen.

Beispiel: Erstellen Sie zum Abrufen der komplexen Variablen bzw. des Parameters für den SharepointApp-Wiederherstellungsplan eine komplexe Azure Automation-Variable mit dem Namen „SharepointApp-params“.

Verwenden Sie diese im Wiederherstellungsplan, indem Sie die Variable aus dem Asset extrahieren. Nutzen Sie hierfür die folgende Anweisung: „Get-AzureAutomationVariable [-AutomationAccountName] <String> [-Name] $paramValue“. [Weitere Informationen](https://msdn.microsoft.com/library/dn913772.aspx)

Auf diese Weise kann dasselbe Skript für verschiedene Wiederherstellungspläne eingesetzt werden, indem die planspezifische komplexe Variable in den Assets gespeichert wird.

## <a name="sample-scripts"></a>Beispielskripts
Ein Repository mit Skripts, die Sie direkt in Ihr Automation-Konto importieren können, finden Sie im [OMS-Repository für Skripts von Kristian Nese](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation).

Dieses Skript ist eine Azure Resource Manager-Vorlage, mit der alle unten angegebenen Skripts bereitgestellt werden.

* NSG

Beim NSG-Runbook werden jeder VM im Wiederherstellungsplan öffentliche IP-Adressen zugewiesen und die virtuellen Netzwerkadapter an eine Netzwerksicherheitsgruppe angefügt, um die Standardkommunikation zu ermöglichen.

* PublicIP

Mit dem PublicIP-Runbook werden jeder VM im Wiederherstellungsplan öffentliche IP-Adressen zugewiesen. Der Zugriff auf Computer und Anwendungen hängt von den Firewalleinstellungen auf jedem Gastbetriebssystem ab.

* CustomScript

Beim CustomScript-Runbook werden jeder VM im Wiederherstellungsplan öffentliche IP-Adressen zugewiesen, und es wird eine benutzerdefinierte Skripterweiterung installiert, mit der während der Bereitstellung der Vorlage der Pullvorgang für das Skript durchgeführt wird, auf das Sie verweisen.

* NSGwithCustomScript

Mit dem NSGwithCustomScript-Runbook werden jeder VM im Wiederherstellungsplan öffentliche IP-Adressen zugewiesen und ein benutzerdefiniertes Skript per Erweiterung installiert. Außerdem wird für die virtuellen Netzwerkadapter eine Verbindung mit einer NSG hergestellt, um die eingehende und ausgehende Kommunikation für den Remotezugriff zu ermöglichen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md)

[Übersicht über Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Übersicht über Azure Automation")

[Beispielskripts für Azure Automation](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Beispielskripts für Azure Automation")



<!--HONumber=Nov16_HO3-->


