---
title: "Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen in Site Recovery | Microsoft-Dokumentation"
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
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 0482b104e18a934142f1cd18fd7265132d5b82bc
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen
Dieses Lernprogramm beschreibt die Integration von Azure Site Recovery und Azure Automation, um die Erweiterung von Wiederherstellungsplänen zu ermöglichen. Wiederherstellungspläne können die Wiederherstellung Ihrer durch Azure Site Recovery geschützten virtuellen Computer sowohl bei der Replikation zur sekundären Cloud als auch bei der Replikation zu Azure orchestrieren. Darüber hinaus tragen sie zu einer **durchgängig exakten**, **wiederholbaren**, und **automatisierten** Wiederherstellung bei. Beim Failover virtueller Computer zu Azure erweitert die Azure Automation-Integration die Wiederherstellungspläne und ermöglicht die Ausführung von Runbooks für beeindruckende Automatisierungsaufgaben.

Falls Sie noch nicht mit Azure Automation vertraut sind, können Sie sich [hier](https://azure.microsoft.com/services/automation/) registrieren, und [hier](https://azure.microsoft.com/documentation/scripts/) die entsprechenden Beispielskripts herunterladen. Weitere Informationen zu [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) sowie zur Orchestrierung der Wiederherstellung in Azure mithilfe von Wiederherstellungsplänen finden Sie [hier](https://azure.microsoft.com/blog/?p=166264).

In diesem Tutorial erfahren Sie, wie Sie Azure Automation-Runbooks in Wiederherstellungspläne integrieren. Wir automatisieren einfache Aufgaben, für die zuvor ein manueller Eingriff erforderlich war, und konvertieren eine aus mehreren Schritten bestehende Wiederherstellung in eine Wiederherstellungsaktion, die sich mit nur einem Klick durchführen lässt.

## <a name="customize-the-recovery-plan"></a>Anpassen des Wiederherstellungsplans
1. Zuerst öffnen wir das Ressourcenblatt des Wiederherstellungsplans. Sie sehen, dass für den Wiederherstellungsplan zwei virtuelle Computer zur Wiederherstellung hinzugefügt wurden.

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
- - -
1. Klicken Sie auf die Schaltfläche „Anpassen“, um mit dem Hinzufügen eines Runbooks zu beginnen.

    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


1. Klicken Sie mit der rechten Maustaste auf die Startgruppe 1, und wählen Sie die Option „Nachfolgende Aktion hinzufügen“.
2. Wählen Sie auf dem neuen Blatt ein Skript aus.
3. Geben Sie dem Skript den Namen „Hello World“.
4. Wählen Sie einen Automation-Kontonamen aus.
    >[!NOTE]
    > Ein Automation-Konto kann sich an einem beliebigen geografischen Standort befinden, aber es muss unter demselben Abonnement wie der Site Recovery-Tresor vorhanden sein.

5. Wählen Sie im Automation-Konto ein Runbook aus. Dieses Runbook ist das Skript, das während der Ausführung des Wiederherstellungsplans nach der Wiederherstellung der ersten Gruppe ausgeführt wird.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)
6. Wählen Sie „OK“, um das Skript zu speichern. Das Skript wird der Gruppe für die nachfolgende Aktion von Startgruppe 1 hinzugefügt.

    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Wichtige Punkte beim Hinzufügen eines Skripts
1. Sie können mit der rechten Maustaste auf das Skript klicken und die Option „Schritt löschen“ oder „Skript aktualisieren“ wählen.
2. Ein Skript kann unter Azure während des Failovers aus der lokalen Umgebung nach Azure ausgeführt werden, und die Ausführung unter Azure als Skript der primären Seite ist vor dem Herunterfahren während des Failbacks von Azure in die lokale Umgebung möglich.
3. Wenn ein Skript ausgeführt wird, wird ein Wiederherstellungsplan-Kontext eingefügt.

Hier ist ein Beispiel für die Kontextvariable angegeben:

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                "ResourceGroupName":"ContosoRG",

                "CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test",

                "RecoveryPointId":"TimeStamp"}

                }

        }


Die folgende Tabelle enthält jeweils den Namen und die Beschreibung der einzelnen Variablen im Kontext:

| **Variablenname** | **Beschreibung** |
| --- | --- |
| RecoveryPlanName |Name des ausgeführten Plans. Mit dieser Variablen können Sie unterschiedliche Aktionen basierend auf dem Namen und per Wiederverwendung des Skripts durchführen. |
| FailoverType |Gibt an, ob es sich um ein Testfailover, um ein geplantes Failover oder um ein nicht geplantes Failover handelt. |
| FailoverDirection |Gibt an, ob die Wiederherstellung auf den primären oder sekundären Standort erfolgt. |
| GroupID |Identifiziert die Gruppennummer innerhalb des Wiederherstellungsplans, wenn der Plan ausgeführt wird |
| VmMap |Array mit allen virtuellen Computern in der Gruppe. |
| VMMap-Schlüssel |Eindeutiger Schlüssel (GUID) für jeden virtuellen Computer. Die GUID entspricht der VMM-ID des virtuellen Computers, sofern zutreffend. |
| SubscriptionId |Die ID des Azure-Abonnements, in dem die VM erstellt wurde. |
| RoleName |Name der Azure-VM, die wiederhergestellt wird. |
| CloudServiceName |Name des Azure Cloud-Diensts, unter dem der virtuelle Computer erstellt wird. |
| ResourceGroupName|Name der Azure-Ressourcengruppen, unter der der virtuelle Computer erstellt wird. |
| RecoveryPointId|Zeitstempel, für den die VM wiederhergestellt wird. |

Sie müssen auch sicherstellen, dass dem Automation-Konto die folgenden Module hinzugefügt werden. Alle Module sollten auf kompatiblen Versionen basieren. Eine einfache Möglichkeit ist die Sicherstellung, dass für alle Module die verfügbare aktuelle Version verwendet wird.
* AzureRM.profile
* AzureRM.Resources
* AzureRM.Automation
* AzureRM.Network
* AzureRM.Compute


### <a name="accessing-all-vms-of-the-vmmap-in-a-loop"></a>Zugreifen auf alle VMs von VmMap in einer Schleife
Verwenden Sie den folgenden Codeausschnitt, um eine Schleife über alle VMs von VmMap hinweg durchzuführen.

```
    $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
    $vmMap = $RecoveryPlanContext.VmMap
     foreach($VMID in $VMinfo)
     {
         $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
             #this check is to ensure that we skip when some data is not available else it will fail
         Write-output "Resource group name ", $VM.ResourceGroupName
         Write-output "Rolename " = $VM.RoleName
         }
     }

```

> [!NOTE]
> Die Werte für den Ressourcengruppennamen und den Rollennamen sind leer, wenn es sich bei dem Skript um eine vorausgehende Aktion einer Startgruppe handelt. Die Werte werden nur aufgefüllt, wenn die VM dieser Gruppe in Bezug auf das Failover erfolgreich ist und das Skript eine vorausgehende Aktion der Startgruppe ist.

## <a name="using-the-same-automation-runbook-with-multiple-recovery-plans"></a>Verwenden desselben Automation-Runbooks mit mehreren Wiederherstellungsplänen

Ein einzelnes Skript kann über mehrere Wiederherstellungspläne genutzt werden, indem externe Variablen verwendet werden. Sie können die [Azure Automation-Variablen](../automation/automation-variables.md) zum Speichern von Parametern verwenden, die Sie für die Ausführung eines Wiederherstellungsplans übergeben können. Indem Sie der Variablen den Namen des Wiederherstellungsplans voranstellen, können Sie einzelne Variablen für jeden Wiederherstellungsplan erstellen und sie als Parameter nutzen. Sie können den Parameter ändern, ohne das Skript zu ändern, und so für eine andere Funktionsweise des Skripts sorgen.

### <a name="using-simple-string-variables-in-runbook-script"></a>Verwenden von einfachen Zeichenfolgenvariablen im Runbookskript

Erwägen Sie die Verwendung eines Skripts, bei dem die Eingabe einer Netzwerksicherheitsgruppe (NSG) auf die VMs eines Wiederherstellungsplans angewendet wird.

Damit das Skript über die Information dazu verfügt, welcher Wiederherstellungsplan ausgeführt wird, können Sie den Wiederherstellungsplankontext verwenden.

```
    workflow AddPublicIPAndNSG {
        param (
              [parameter(Mandatory=$false)]
              [Object]$RecoveryPlanContext
        )

        $RPName = $RecoveryPlanContext.RecoveryPlanName
```

Zum Anwenden einer vorhandenen NSG benötigen Sie den NSG-Namen und die NSG-Ressourcengruppe. Wir geben diese Variablen als Eingabe für die Skripts des Wiederherstellungsplans an. Erstellen Sie zu diesem Zweck in den Automation-Kontoassets zwei Variablen, und stellen Sie ihnen den Namen des Wiederherstellungsplans voran, für den Sie die Parameter erstellen.

1. Erstellen Sie eine Variable zum Speichern des NSG-Namens. Stellen Sie ihr den Namen des Wiederherstellungsplans voran.
    ![Erstellen der Variablen für den NSG-Namen](media/site-recovery-runbook-automation-new/var1.png)

2. Erstellen Sie eine Variable zum Speichern des NSG-Ressourcengruppennamens. Stellen Sie ihr den Namen des Wiederherstellungsplans voran.
    ![Erstellen des NSG-Ressourcengruppennamens](media/site-recovery-runbook-automation-new/var2.png)


Rufen Sie im Skript die Werte der Variablen ab, indem Sie den folgenden Referenzcode verwenden:

```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
```

Als Nächstes können Sie die Variablen im Runbook einsetzen und die NSG auf die Netzwerkschnittstelle des virtuellen Computers anwenden, für den das Failover durchgeführt wurde.

```
     InlineScript {
        if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
```

Erstellen Sie für jeden Wiederherstellungsplan unabhängige Variablen, damit Sie das Skript wiederverwenden und den Namen des Wiederherstellungsplans voranstellen können. Ein vollständiges Skript für dieses Beispiel finden Sie [hier](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="using-complex-variable-to-store-more-information"></a>Verwenden von komplexen Variablen zum Speichern weiterer Informationen

Stellen Sie sich ein Szenario vor, bei dem nur ein Skript für bestimmte VMs eine öffentliche IP-Adresse aktivieren soll. Ein anderes Beispiel ist die Anwendung von unterschiedlichen NSGs auf unterschiedliche virtuelle Computer (nicht alle). Dieses Skript kann dann für alle anderen Wiederherstellungspläne wiederverwendet werden. Jeder Wiederherstellungsplan kann über eine variable Anzahl von virtuellen Computern verfügen (eine SharePoint-Wiederherstellung verfügt beispielsweise über zwei Front-Ends, während eine einfache Branchenanwendung nur ein Front-End hat). Zur Erreichung dieses Ergebnisses ist es unmöglich, separate Variablen pro Wiederherstellungsplan zu erstellen. Hier verwenden wir ein neues Verfahren und erstellen eine [komplexe Variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in den Azure Automation-Kontoassets, indem wir mehrere Werte angeben. Zum Ausführen der folgenden Schritte benötigen Sie Azure PowerShell.

1. Melden Sie sich in Azure PowerShell an Ihrem Abonnement an.

    ```
        login-azurermaccount
        $sub = Get-AzureRmSubscription -Name <SubscriptionName>
        $sub | Select-AzureRmSubscription
    ```

2. Erstellen Sie zum Speichern der Parameter die komplexe Variable, und geben Sie ihr den Namen des Wiederherstellungsplans.

    ```
        $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

    In dieser komplexen Variablen ist *VMDetails* die VM-ID für den geschützten virtuellen Computer. Sie ist unter den Eigenschaften des virtuellen Computers im Portal angegeben. Hier haben wir eine Variable zum Speichern der Details von zwei virtuellen Computern erstellt.

    ![VM-ID zur Verwendung als GUID](media/site-recovery-runbook-automation-new/vmguid.png)

3. Verwenden Sie diese Variable in Ihrem Runbook, und wenden Sie die NSG auf dem virtuellen Computer an, wenn eine VMGUID im Wiederherstellungsplankontext enthalten ist.

    ```
        $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. Führen Sie in Ihrem Runbook einen Schleife durch die VMs des Wiederherstellungsplankontexts durch, und überprüfen Sie, ob die VM auch in **$VMDetailsObj** enthalten ist. Wenn ja, wenden Sie die NSG an, indem Sie auf die Eigenschaften der Variablen zugreifen.
    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If the VM exists in the context, this will not b Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

Sie können dasselbe Skript mit unterschiedlichen Wiederherstellungsplänen verwenden und andere Parameter angeben, indem Sie den Wert für unterschiedliche Wiederherstellungspläne in verschiedenen Variablen speichern.

## <a name="sample-scripts"></a>Beispielskripts
Stellen Sie Beispielskripts mithilfe der unten stehenden Schaltfläche zum Bereitstellen in Azure in Ihrem Automation-Konto bereit.

[![Bereitstellen in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Schauen Sie sich zudem ein kurzes Video zum Wiederherstellen einer WordPress-Anwendung mit zwei Ebenen in Azure an.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md)

[Übersicht über Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Übersicht über Azure Automation")

[Beispielskripts für Azure Automation](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Beispielskripts für Azure Automation")

