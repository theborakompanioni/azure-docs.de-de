---
title: Mein erstes Python-Runbook in Azure Automation | Microsoft-Dokumentation
description: "Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen Python-Runbooks vertraut machen können."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: eslesar
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 26a1681e662814fbe513bec1b8d2bc040885d632
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="my-first-python-runbook"></a>Mein erstes Python-Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

Dieses Tutorial führt Sie durch die Erstellung eines [Python-Runbooks](automation-runbook-types.md#python-runbooks) in Azure Automation. Wir beginnen mit einem einfachen Runbook, das wir testen und veröffentlichen. Dabei erläutern wir, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend ändern wir das Runbook, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Abschließend fügen wir Runbookparameter hinzu, um die Stabilität des Runbooks zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen.  Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, sollte es sich nicht um einen virtuellen Computer in der Produktionsumgebung handeln.

## <a name="create-a-new-runbook"></a>Erstellen eines neuen Runbooks

Wir erstellen zunächst ein einfaches Runbook, das den Text *Hello World*ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
   Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites)erwähnte Anmeldeinformationsobjekt befinden.
1. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
   ![RunbooksControl](media/automation-first-runbook-textual-python/runbooks-control-tiles.png)
1. Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen** klicken.
1. Nennen Sie das Runbook *MyFirstRunbook-Python*.
1. Da wir hier ein [Python-Runbook](automation-runbook-types.md#python-runbooks) erstellen, wählen Sie als **Runbooktyp** die Option **Python 2** aus.
1. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den Text-Editor zu öffnen.

## <a name="add-code-to-the-runbook"></a>Hinzufügen von Code zum Runbook

Wir fügen nun einen einfachen Befehl zum Ausgeben des Texts „Hallo Welt“ hinzu:

```python
print("Hello World!")
```

Klicken Sie auf **Speichern**, um das Runbook zu speichern.

## <a name="test-the-runbook"></a>Testen des Runbooks

Bevor wir das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, möchten wir uns vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die **Entwurfsversion** des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich** , um den Testbereich zu öffnen.
   ![Test Pane](media/automation-first-runbook-textual-python/automation-runbook-edit-controls-test.png)
1. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
1. Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.
   Der Auftrag besitzt zunächst den Status *In der Warteschlange*. Hiermit wird angegeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.
1. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In unserem Fall: *Hello World*.
1. Schließen Sie den Testbereich, um zum Zeichenbereich zurückzukehren.

## <a name="publish-and-start-the-runbook"></a>Veröffentlichen und Starten des Runbooks

Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Wir müssen das Runbook veröffentlichen, um es in der Produktionsumgebung ausführen zu können.
Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben.
In unserem Fall ist noch keine veröffentlichte Version vorhanden, da wir das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.
   ![Schaltfläche "Veröffentlichen"](media/automation-first-runbook-textual-python/automation-runbook-edit-controls-publish.png)
1. Wenn Sie jetzt einen Bildlauf nach links durchführen, um das Runbook im Bereich **Runbooks** anzuzeigen, zeigt das Runbook **Veröffentlicht** als **Erstellungsstatus** an.
1. Scrollen Sie wieder nach rechts , um den Bereich für **MyFirstRunbook-Python** anzuzeigen.
   Mit den Optionen am oberen Rand können wir das Runbook starten, das Runbook anzeigen, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um den Start über einen HTTP-Aufruf zu ermöglichen.
1. Wir möchten das Runbook hier nur starten. Klicken Sie daher auf **Start** und dann auf dem Blatt „Runbook starten“ auf **OK**.
1. Ein Auftragsbereich für den erstellten Runbookauftrag wird angezeigt. Dieser Bereich kann zwar geschlossen werden, aber in diesem Fall lassen wir ihn geöffnet, um den Status des Auftrags verfolgen zu können.
1. Der Auftragsstatus wird unter **Auftragszusammenfassung** angezeigt und entspricht den Statusoptionen, die wir bereits beim Testen des Runbooks gesehen haben.
1. Wenn der Runbookstatus *Abgeschlossen*lautet, klicken Sie auf **Ausgabe**. Der Bereich "Ausgabe" wird geöffnet, und der Text *Hello World*.
1. Schließen Sie den Ausgabebereich.
1. Klicken Sie auf **Alle Protokolle**, um den Bereich „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabedatenstrom sollte nur *Hello World* angezeigt werden. Hier können aber auch andere Datenströme für einen Runbookauftrag (wie etwa „Ausführlich“ und „Fehler“) angezeigt werden, sofern das Runbook Schreibvorgänge dafür durchführt.
1. Schließen Sie den Datenstrom- und den Auftragsbereich, um zum Bereich „MyFirstRunbook-Python“ zurückzukehren.
1. Klicken Sie auf **Aufträge** , um den Auftragsbereich für dieses Runbook zu öffnen. Dadurch werden alle von diesem Runbook erstellten Aufträge aufgeführt. Hier wird nur ein einzelner Auftrag aufgeführt, da wir den Auftrag bislang erst einmal ausgeführt haben.
1. Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den wir uns beim Starten des Runbooks angesehen haben. So können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## <a name="add-authentication-to-manage-azure-resources"></a>Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen

Wir haben unser Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Wir möchten damit Azure-Ressourcen verwalten.
Für die Verwaltung von Azure-Ressourcen muss das Skript bei der Authentifizierung die Anmeldeinformationen für Ihr [Automation-Konto](automation-offering-get-started.md) verwenden.

> [!NOTE]
> Das Automation-Konto muss mit der Dienstprinzipalfunktion erstellt worden sein, damit ein RunAs-Zertifikat vorhanden ist.
> Wenn Ihr Automation-Konto nicht mit dem Dienstprinzipal erstellt wurde, können Sie die Authentifizierung mithilfe der unter [Authentifizieren bei den Azure-Verwaltungsbibliotheken für Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) beschriebenen Methode durchführen.

1. Öffnen Sie den Text-Editor, indem Sie im Bereich „MyFirstRunbook-Python“ auf **Bearbeiten** klicken.
1. Fügen Sie für die Authentifizierung bei Azure den folgenden Code hinzu:
   ```python
   import os
   from azure.mgmt.compute import ComputeManagementClient
   import azure.mgmt.resource
   import automationassets

   def get_automation_runas_credential(runas_connection):
       from OpenSSL import crypto
       import binascii
       from msrestazure import azure_active_directory
       import adal

       # Get the Azure Automation RunAs service principal certificate
       cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
       pks12_cert = crypto.load_pkcs12(cert)
       pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())

       # Get run as connection information for the Azure Automation service principal
       application_id = runas_connection["ApplicationId"]
       thumbprint = runas_connection["CertificateThumbprint"]
       tenant_id = runas_connection["TenantId"]

       # Authenticate with service principal certificate
       resource ="https://management.core.windows.net/"
       authority_url = ("https://login.microsoftonline.com/"+tenant_id)
       context = adal.AuthenticationContext(authority_url)
       return azure_active_directory.AdalAuthentication(
       lambda: context.acquire_token_with_client_certificate(
               resource,
               application_id,
               pem_pkey,
               thumbprint)
       )

   # Authenticate to Azure using the Azure Automation RunAs service principal
   runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
   azure_credential = get_automation_runas_credential(runas_connection)
   ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Hinzufügen von Code zum Erstellen eines Python-Computeclients und Starten des virtuellen Computers

Erstellen Sie für die Verwendung von virtuellen Azure-Computern eine Instanz des [Azure-Compute-Clients für Python](https://docs.microsoft.com/python/api/azure.mgmt.compute.compute.computemanagementclient?view=azure-python).

Verwenden Sie den Computeclient zum Starten des virtuellen Computers. Fügen Sie den folgenden Code zum Runbook hinzu:

```python
# Initialize the compute management client with the RunAs credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
azure_credential,
  str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start("MyResourceGroup", "TestVM")
async_vm_start.wait()
```

_MyResourceGroup_ ist der Name der Ressourcengruppe, die den virtuellen Computer enthält, und _TestVM_ ist der Name des virtuellen Computers, der gestartet werden soll. 

Testen Sie das Runbook, und führen Sie es erneut aus, um zu ermitteln, ob der virtuelle Computer gestartet wird.

## <a name="use-input-parameters"></a>Verwenden von Eingabeparametern

Das Runbook verwendet derzeit hartcodierte Werte für den Namen der Ressourcengruppe und des virtuellen Computers.
Wir fügen nun Code hinzu, der diese Werte aus Eingabeparametern abruft.

Wir verwenden die Variable `sys.argv`, um die Parameterwerte abzurufen.
Fügen Sie im Runbook den folgenden Code unmittelbar nach den anderen `import`-Anweisungen hinzu:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Dadurch wird das `sys`-Modul importiert, und es werden zwei Variablen für die Namen der Ressourcengruppe und des virtuellen Computers erstellt.
Beachten Sie, dass das Element der Argumentliste (`sys.argv[0]`) der Name des Skripts ist und nicht vom Benutzer eingegeben wird.

Jetzt können wir die letzten beiden Zeilen des Runbooks ändern, um die Eingabeparameterwerte anstelle der hartcodierten Werte zu verwenden:

```python
async_vm_start = compute_client.virtual_machines.start(resource_group_name, vm_name)
async_vm_start.wait()
```

Beim Starten eines Python-Runbooks (entweder über das Blatt **Test** oder als veröffentlichtes Runbook) können Sie die Werte für Parameter auf dem Blatt **Runbook starten** unter **Parameter** eingeben.

![Feld für Parameterwerte](media/automation-first-runbook-textual-python/runbook-python-param-highlight.png)

Wenn Sie mit der Eingabe eines Werts im ersten Feld begonnen haben, wird jeweils ein weiteres Feld angezeigt, sodass Sie die erforderliche Anzahl von Parameterwerten eingeben können.

Die Werte stehen dem Skript wie im soeben hinzugefügten Code als `sys.argv`-Array zur Verfügung.

Geben Sie den Namen Ihrer Ressourcengruppe als Wert für den ersten Parameter und den Namen des zu startenden virtuellen Computers als Wert für den zweiten Parameter ein.

![Eingeben von Parameterwerten](media/automation-first-runbook-textual-python/runbook-python-params.png)

Klicken Sie auf **OK**, um das Runbook zu starten. Das Runbook wird ausgeführt und startet den angegebenen virtuellen Computer.

## <a name="next-steps"></a>Nächste Schritte

* Erste Schritte mit PowerShell-Runbooks werden in [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md)
* Informationen über die ersten Schritte mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md)
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)
* Weitere Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
* Weitere Informationen zum Entwickeln für Azure mit Python finden Sie unter [Azure für Python-Entwickler](https://docs.microsoft.com/en-us/python/azure/?view=azure-python).

