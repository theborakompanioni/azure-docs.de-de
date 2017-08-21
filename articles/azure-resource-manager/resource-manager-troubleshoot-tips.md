---
title: Grundlegendes zu Azure-Bereitstellungsfehlern | Microsoft-Dokumentation
description: Beschreibt, wie Sie Informationen zu Azure-Bereitstellungsfehlern erhalten.
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Bereitstellungsfehler, Azure-Bereitstellung, in Azure bereitstellen.
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="understand-azure-deployment-errors"></a>Grundlegendes zu Azure-Bereitstellungsfehlern
In diesem Thema werden Bereitstellungsfehler beschrieben, und Sie erfahren, wie Sie weitere Informationen zu einem Fehler ermitteln können. Tipps zum Lösen häufiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Zwei Arten von Fehlern
Es gibt zwei Arten von Fehlern, die auftreten können:

* Überprüfungsfehler
* Bereitstellungsfehler

Die folgende Abbildung zeigt das Aktivitätsprotokoll für ein Abonnement. Es werden zwei Bereitstellungen dargestellt. Bei einer Bereitstellung hat die Vorlage die Überprüfung nicht bestanden (**Überprüfen**), und sie wurde nicht fortgesetzt. Bei der anderen Bereitstellung hat die Vorlage die Überprüfung bestanden. Beim Erstellen der Ressourcen (**Bereitstellungen schreiben**) ist jedoch ein Fehler aufgetreten. 

![Fehlercode anzeigen](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Überprüfungsfehler entstehen durch Szenarien, die vor der Bereitstellung bestimmt werden können. Dazu gehören Syntaxfehler in Ihrer Vorlage oder Versuche, Ressourcen bereitzustellen, die Ihre Abonnementkontingente überschreiten würden. Bereitstellungsfehler können durch Bedingungen verursacht werden, die während des Bereitstellungsprozesses auftreten. Dazu gehören Versuche, auf eine Ressource zuzugreifen, die parallel bereitgestellt wird.

Beide Fehlertypen geben einen Fehlercode zurück, der für die Problembehandlung für die Bereitstellung genutzt werden kann. Beide Fehlertypen werden im [Aktivitätsprotokoll](resource-group-audit.md) angezeigt. Überprüfungsfehler werden allerdings nicht im Bereitstellungsverlauf festgehalten, da die Bereitstellung tatsächlich nie gestartet wurde.

## <a name="determine-error-code"></a>Bestimmen des Fehlercodes

Sie erhalten Informationen über einen Fehler, indem Sie sich die Fehlermeldung und den Fehlercode ansehen. Im Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md) werden Lösungen nach Fehlercode aufgelistet. In diesem Thema wird erläutert, wie der Fehlercode über das Azure-Portal ermittelt werden kann.

### <a name="validation-errors"></a>Überprüfungsfehler

Wenn Sie Bereitstellungen über das Portal ausführen, wird ein Überprüfungsfehler nach dem Senden der Werte angezeigt.

![Anzeigen eines Überprüfungsfehlers im Portal](./media/resource-manager-troubleshoot-tips/validation-error.png)

Wählen Sie die Meldung aus, um weitere Details zu sehen. In der folgenden Abbildung sehen Sie einen **InvalidTemplateDeployment**-Fehler und eine Meldung, die auf eine durch eine Richtlinie blockierte Bereitstellung hinweist.

![Anzeigen von Überprüfungsdetails](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Bereitstellungsfehler

Wenn der Vorgang die Überprüfung besteht, aber ein Fehler während der Bereitstellung auftritt, wird der Fehler in den Benachrichtigungen angezeigt. Wählen Sie die Benachrichtigung aus.

![Benachrichtigungsfehler](./media/resource-manager-troubleshoot-tips/notification.png)

Sie sehen weitere Details zur Bereitstellung. Wählen Sie die Option aus, um weitere Informationen zum Fehler zu finden.

![Fehler bei der Bereitstellung](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Die Fehlermeldung und die Fehlercodes werden angezeigt. Es sind zwei Fehlercodes vorhanden. Der erste Fehlercode (**DeploymentFailed**) ist ein allgemeiner Fehler, der nicht die Informationen bereitstellt, die Sie zum Beheben des Fehlers benötigen. Der zweite Fehlercode (**StorageAccountNotFound**) enthält die Details, die Sie benötigen. 

![Fehlerdetails](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Debugprotokollierung aktivieren
Manchmal benötigen Sie weitere Informationen zur Anforderung und zur Antwort, um den genauen Fehler zu ermitteln. Mithilfe von PowerShell oder der Azure CLI können Sie anfordern, dass während der Bereitstellung zusätzliche Informationen protokolliert werden.

- PowerShell

   Legen Sie in PowerShell den Parameter **DeploymentDebugLogLevel** auf „All“, „ResponseContent“ oder „RequestContent“ fest.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Überprüfen Sie den Anforderungsinhalt mit folgendem Cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Oder überprüfen Sie den Antwortinhalt mit:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Mithilfe dieser Informationen können Sie ermitteln, ob ein Wert in der Vorlage nicht ordnungsgemäß festgelegt wurde.

- Azure-Befehlszeilenschnittstelle

   Untersuchen Sie die Bereitstellungsvorgänge mit folgendem Befehl:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Geschachtelte Vorlage

   Verwenden Sie zum Protokollieren von Debuginformationen zu einer geschachtelten Vorlage das **debugSetting**-Element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Erstellen einer Vorlage zur Problembehandlung
Mitunter ist die einfachste Möglichkeit für die Behandlung von Problemen bei Ihrer Vorlage das Testen von Teilen davon. Sie können eine vereinfachte Vorlage erstellen, die es Ihnen ermöglicht, sich auf den Teil zu konzentrieren, der Ihrer Meinung nach den Fehler verursacht. Nehmen wir beispielsweise an, dass ein Fehler auftritt, wenn Sie auf eine Ressource verweisen. Anstatt sich mit einer gesamten Vorlage zu beschäftigen, erstellen Sie eine Vorlage, die den Teil wiedergibt, der Ihr Problem möglicherweise verursacht. So können Sie besser ermitteln, ob die richtigen Parameter übergeben, Vorlagenfunktionen ordnungsgemäß genutzt und die Ressourcen abgerufen werden, die Sie erwarten.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Ein anderes Beispiel: Es treten Bereitstellungsfehler auf, von denen Sie annehmen, dass sie aufgrund falsch festgelegter Abhängigkeiten entstehen. Testen Sie Ihre Vorlage, indem Sie sie in einfachere Vorlagen aufteilen. Erstellen Sie zunächst eine Vorlage, mit der nur eine einzige Ressource bereitgestellt wird (z.B. eine SQL Server-Instanz). Wenn Sie sicher sind, dass die Ressource richtig definiert ist, fügen Sie eine Ressource hinzu, die davon abhängig ist (z.B. eine SQL-Datenbank). Wenn diese beiden Ressourcen richtig definiert sind, fügen Sie weitere abhängige Ressourcen hinzu (z.B. Überwachungsrichtlinien). Löschen Sie zwischen den jeweiligen Testbereitstellungen die Ressourcengruppe, um sicherzustellen, dass Sie die Abhängigkeiten angemessen testen. 

## <a name="check-deployment-sequence"></a>Überprüfen der Bereitstellungssequenz

Viele Bereitstellungsfehler treten auf, wenn Ressourcen in einer unerwarteten Reihenfolge bereitgestellt werden. Diese Fehler treten auf, wenn Abhängigkeiten nicht ordnungsgemäß festgelegt sind. Wenn eine erforderliche Abhängigkeit nicht vorhanden ist, versucht eine Ressource, einen Wert für eine andere Ressource zu verwenden, die aber noch nicht existiert. Sie erhalten einen Fehler mit dem Hinweis, dass die Ressource nicht gefunden wurde. Diese Art von Fehler kann von Zeit zu Zeit auftreten, weil die Bereitstellungszeit für jede Ressource variieren kann. Der erste Versuch, die Ressourcen bereitzustellen, kann beispielsweise erfolgreich sein, weil eine erforderliche Ressource zufällig rechtzeitig erstellt wird. Der zweite Versuch ist dann aber nicht erfolgreich, weil die benötigte Ressource nicht rechtzeitig vorhanden ist. 

Es ist ratsam, das Einrichten von Abhängigkeiten zu vermeiden, die nicht benötigt werden. Wenn Sie über nicht benötigte Abhängigkeiten verfügen, verlängern Sie die Dauer der Bereitstellung, indem Sie verhindern, dass nicht voneinander abhängige Ressourcen parallel bereitgestellt werden. Außerdem besteht die Gefahr, dass Sie Ringabhängigkeiten erstellen, die die Bereitstellung blockieren. Mit der Funktion [reference](resource-group-template-functions-resource.md#reference) wird eine implizite Abhängigkeit von der angegebenen Ressource erstellt, wenn diese Ressource in derselben Vorlage bereitgestellt wird. Aus diesem Grund verfügen Sie ggf. über eine höhere Zahl von Abhängigkeiten als in der **dependsOn**-Eigenschaft angegeben. Mit der Funktion [resourceId](resource-group-template-functions-resource.md#resourceid) wird keine implizite Abhängigkeit erstellt und nicht überprüft, ob die Ressource vorhanden ist.

Wenn Abhängigkeitsprobleme auftreten, benötigen Sie Informationen zur Reihenfolge der Ressourcenbereitstellung. So zeigen Sie die Reihenfolge der Vorgänge bei der Bereitstellung an:

1. Wählen Sie den Bereitstellungsverlauf für die Ressourcengruppe aus.

   ![Bereitstellungsverlauf auswählen](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Wählen Sie aus dem Verlauf eine Bereitstellung und **Ereignisse** aus.

   ![Bereitstellungsereignisse auswählen](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Überprüfen Sie die Abfolge von Ereignissen für jede Ressource. Achten Sie auf den Status der einzelnen Vorgänge. Die folgende Abbildung zeigt beispielsweise drei parallel bereitgestellte Speicherkonten. Beachten Sie, dass die drei Speicherkonten zur selben Zeit gestartet werden.

   ![Parallele Bereitstellung](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   Die nächste Abbildung zeigt drei Speicherkonten, die nicht parallel bereitgestellt werden. Das zweite Speicherkonto ist vom ersten Speicherkonto abhängig, und das dritte Speicherkonto ist vom zweiten Speicherkonto abhängig. Aus diesem Grund wird das erste Speicherkonto gestartet, akzeptiert und abgeschlossen, bevor das nächste gestartet wird.

   ![Sequenzielle Bereitstellung](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Auch in deutlich komplizierter Szenarien können Sie mit dem gleichen Verfahren ermitteln, wann die Bereitstellung für jede einzelne Ressource gestartet und abgeschlossen wird. Überprüfen Sie Ihre Bereitstellungsereignisse, um herauszufinden, ob die Sequenz anders als erwartet aussieht. Wenn dies der Fall ist, sollten Sie die Abhängigkeiten für diese Ressource neu bewerten.

Resource Manager kennzeichnet Ringabhängigkeiten während der Überprüfung der Vorlage. Es wird eine Fehlermeldung mit dem eindeutigen Hinweis zurückgegeben, dass eine Ringabhängigkeit besteht. So beheben Sie eine Ringabhängigkeit

1. Suchen Sie in Ihrer Vorlage nach der Ressource, die in der Ringabhängigkeit angegeben ist. 
2. Sehen Sie sich für diese Ressource die **dependsOn**-Eigenschaft und alle Vorkommen der Funktion **reference** an, um zu ermitteln, von welchen Ressourcen sie abhängig ist. 
3. Überprüfen Sie diese Ressourcen, um zu ermitteln, von welchen Ressourcen sie abhängig sind. Verfolgen Sie die Abhängigkeiten, bis Sie auf eine Ressource stoßen, die von der Originalressource abhängig ist.
5. Untersuchen Sie für die an der Ringabhängigkeit beteiligten Ressourcen sorgfältig alle Vorkommen der **dependsOn**-Eigenschaft, um alle Abhängigkeiten zu identifizieren, die nicht benötigt werden. Entfernen Sie diese Abhängigkeiten. Wenn Sie unsicher sind, ob eine Abhängigkeit erforderlich ist, können Sie versuchen, sie zu entfernen. 
6. Stellen Sie die Vorlage erneut bereit.

Das Entfernen von Werten aus der **dependsOn**-Eigenschaft kann zu Fehlern beim Bereitstellen der Vorlage führen. Fügen Sie die Abhängigkeit wieder in die Vorlage ein, wenn ein Fehler auftritt. 

Falls sich die Ringabhängigkeit mit dieser Vorgehensweise nicht beseitigen lässt, können Sie erwägen, einen Teil Ihrer Bereitstellungslogik in untergeordnete Ressourcen zu verschieben (z.B. Erweiterungen oder Konfigurationseinstellungen). Konfigurieren Sie diese untergeordneten Ressourcen so, dass sie nach den an der Ringabhängigkeit beteiligten Ressourcen bereitgestellt werden. Nehmen wir beispielsweise an, Sie stellen zwei virtuelle Computer bereit, müssen aber Eigenschaften festlegen, die auf den jeweils anderen verweisen. Sie können diese in der folgenden Reihenfolge bereitstellen:

1. VM1
2. VM2
3. Die Erweiterung auf VM1 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM1 fest, die sie von VM2 abruft.
4. Die Erweiterung auf VM2 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM2 fest, die sie von VM1 abruft.

Diese Vorgehensweise funktioniert auch für App Service-Apps. Erwägen Sie, Konfigurationswerte in eine untergeordnete Ressource der App-Ressource zu verschieben. Sie können zwei Web-Apps in der folgenden Reihenfolge bereitstellen:

1. webapp1
2. webapp2
3. Die Konfiguration für „webapp1“ ist von „webapp1“ und „webapp2“ abhängig. Sie enthält App-Einstellungen mit Werten aus „webapp2“.
4. Die Konfiguration für „webapp2“ ist von „webapp1“ und „webapp2“ abhängig. Sie enthält App-Einstellungen mit Werten aus „webapp1“.


## <a name="next-steps"></a>Nächste Schritte
* Tipps zum Lösen häufiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](resource-manager-deployment-operations.md).

