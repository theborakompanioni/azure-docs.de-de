---
title: Automatisieren der Bereitstellung einer VM in Amazon Web Services | Microsoft Docs
description: In diesem Artikel wird gezeigt, wie Sie Azure Automation nutzen, um das Erstellen einer Amazon Web Service-VM zu automatisieren
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 1d85c01a-d795-4523-8194-84fc15b53838
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/14/2017
ms.author: tiandert; bwren
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: e0b784006b4933fe986890c09afa965934511784
ms.lasthandoff: 04/15/2017


---
# <a name="azure-automation-scenario---provision-an-aws-virtual-machine"></a>Azure Automation-Szenario – Bereitstellen eines virtuellen AWS-Computers
In diesem Artikel wird gezeigt, wie Sie Azure Automation zur Bereitstellung eines virtuellen Computers in Ihrem AWS-Abonnement (Amazon Web Services) nutzen und für diesen virtuellen Computer einen bestimmten Namen festlegen können – ein Vorgang, der in AWS als „Tagging“ des virtuellen Computers bezeichnet wird.

## <a name="prerequisites"></a>Voraussetzungen
Um die in diesem Artikel beschriebenen Aufgaben ausführen zu können, benötigen Sie ein Azure Automation-Konto und ein AWS-Abonnement. Weitere Informationen zum Einrichten eines Azure Automation-Kontos sowie zum Konfigurieren dieses Kontos mit den Anmeldeinformationen Ihres AWS-Abonnements finden Sie unter [Authentifizieren mit Amazon Web Services konfigurieren](automation-configure-aws-account.md).  Da in den nachfolgenden Schritten auf dieses Konto verwiesen wird, sollte dieses Konto mit den Anmeldeinformationen Ihres AWS-Abonnements erstellt oder aktualisiert werden, bevor Sie fortfahren.

## <a name="deploy-amazon-web-services-powershell-module"></a>Bereitstellen des Amazon Web Services-PowerShell-Moduls
Unser Runbook für die VM-Bereitstellung nutzt das AWS-PowerShell-Modul zur Durchführung seiner Aufgaben. Führen Sie die folgenden Schritte aus, um das Modul zu Ihrem Automation-Konto hinzuzufügen, das mit den Anmeldeinformationen Ihres AWS-Abonnements konfiguriert ist.  

1. Öffnen Sie Ihren Webbrowser, navigieren Sie zum [PowerShell-Katalog](http://www.powershellgallery.com/packages/AWSPowerShell/), und klicken Sie auf **In Azure Automation bereitstellen**.<br><br> ![Importieren des AWS-PS-Moduls](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Sie werden auf die Azure-Anmeldeseite geleitet, und nach der Authentifizierung werden Sie zum Azure-Portal weitergeleitet, in dem das folgende Blatt angezeigt wird.<br><br> ![Blatt „Modul importieren“](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Ressourcengruppe aus, und geben Sie auf dem Blatt „Parameter“ die folgenden Informationen an:
   
   * Wählen Sie in der Dropdownliste **Neues oder vorhandenes Automation-Konto (Zeichenfolge)** die Option **Vorhanden** aus.  
   * Geben Sie im Feld **Name des Automation-Kontos (Zeichenfolge)** den genauen Namen des Automation-Kontos ein, das die Anmeldeinformationen für Ihr AWS-Abonnement umfasst.  Wenn Sie z.B. ein dediziertes Konto **AWSAutomation** erstellt haben, dann geben Sie diesen Namen in das Feld ein.
   * Wählen Sie aus der Dropdownliste **Standort des Automation-Kontos** die entsprechende Region aus.
4. Nachdem Sie die erforderlichen Informationen eingegeben haben, klicken Sie auf **Erstellen**.
   
   > [!NOTE]
   > Beim Importieren eines PowerShell-Moduls in Azure Automation werden auch die Cmdlets extrahiert, und diese Aktivitäten erscheinen erst, wenn der Import des Moduls und das Extrahieren der Cmdlets vollständig abgeschlossen sind. Dieser Vorgang kann einige Minuten dauern.  
   > <br>
   > 
   > 
5. Öffnen Sie im Azure-Portal Ihr Automation-Konto, auf das in Schritt 3 verwiesen wurde.
6. Klicken Sie auf die Kachel **Assets**, und wählen Sie auf dem Blatt **Assets** die Kachel **Module**.
7. Auf dem Blatt **Module** wird das Modul **AWSPowerShell** in der Liste angezeigt.

## <a name="create-aws-deploy-vm-runbook"></a>Erstellen eines Runbooks zur Bereitstellung einer VM in AWS
Nach der Bereitstellung des AWS-PowerShell-Moduls kann nun ein Runbook erstellt werden, mit dem sich die Bereitstellung eines virtuellen Computers in AWS unter Verwendung eines PowerShell-Skripts automatisieren lässt. Die Schritte unten zeigen, wie Sie ein natives PowerShell-Skript in Azure Automation nutzen.  

> [!NOTE]
> Weitere Optionen und Informationen zu diesem Skript finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages/New-AwsVM/DisplayScript).
> 

1. Laden Sie das PowerShell-Skript „New-AwsVM“ aus dem PowerShell-Katalog herunter. Öffnen Sie dazu eine PowerShell-Sitzung, und geben Sie Folgendes ein:<br>
   ```
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf die Kachel **Runbooks** .  
3. Wählen Sie auf dem Blatt **Runbooks** die Option **Runbook hinzufügen**.
4. Wählen Sie auf dem Blatt **Runbook hinzufügen** die Option **Schnellerfassung** (Neues Runbook erstellen).
5. Geben Sie auf dem Blatt **Runbook-Eigenschaften** im Feld „Name“ einen Namen für Ihr Runbook ein, und wählen Sie in der Dropdownliste **Runbooktyp** die Option **PowerShell**. Klicken Sie dann auf **Erstellen**.<br><br> ![Blatt „Modul importieren“](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
6. Wenn das Blatt „PowerShell-Runbook bearbeiten“ angezeigt wird, kopieren Sie das PowerShell-Skript, und fügen Sie es im Bereich zur Runbookerstellung ein.<br><br> ![Runbook-PowerShell-Skript](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    > [!NOTE]
    > Beachten Sie Folgendes, wenn Sie mit dem PowerShell-Beispielskript arbeiten:
    > 
    > * Das Runbook enthält eine Reihe von Standardparameterwerten. Evaluieren Sie alle Standardwerte, und aktualisieren Sie diese bei Bedarf.
    > * Wenn Sie Ihre AWS-Anmeldeinformationen als Anmeldeinformationsasset gespeichert haben, dessen Name von **AWScred**abweicht, müssen Sie das Skript in Zeile 57 entsprechend aktualisieren.  
    > * Bei der Arbeit mit den AWS-CLI-Befehlen in PowerShell müssen Sie – insbesondere bei diesem Beispielrunbook – die AWS-Region angeben. Andernfalls tritt bei den Cmdlets ein Fehler auf.  Weitere Einzelheiten finden Sie im Dokument zu den AWS-Tools für PowerShell im AWS-Thema [Specify AWS Region (Angeben der AWS-Region)](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) .  
    >

7. Um eine Liste mit Imagenamen aus Ihrem AWS-Abonnement abzurufen, starten Sie PowerShell ISE, und importieren Sie das AWS-PowerShell-Modul.  Führen Sie die Authentifizierung gegenüber AWS durch, indem Sie **Get-AutomationPSCredential** in Ihrer ISE-Umgebung durch **AWScred = Get-Credential** ersetzen.  Sie werden zur Eingabe Ihrer Anmeldeinformationen aufgefordert und können Ihre **Zugriffsschlüssel-ID** als Benutzername und Ihren **geheimen Zugriffsschlüssel** als Kennwort eingeben.  Betrachten Sie das folgende Beispiel:  

        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile

    Die folgende Ausgabe wird zurückgegeben:<br><br>
   ![AWS-Images abrufen](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
8. Kopieren Sie einen der Imagenamen, und fügen Sie ihn in eine Automation-Variable ein, auf die im Runbook per **$InstanceType**verwiesen wird. Da in diesem Beispiel das kostenlose mehrstufige AWS-Abonnement verwendet wird, verwenden wir für unser Beispielrunbook **t2.micro** .  
9. Speichern Sie das Runbook, und klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen. Klicken Sie dann bei Aufforderung auf **Ja**.

### <a name="testing-the-aws-vm-runbook"></a>Testen des AWS-VM-Runbooks
Bevor wir mit dem Testen des Runbooks fortfahren, müssen einige Dinge überprüft werden. Dies bedeutet Folgendes:  

* Ein Asset namens **AWScred** wurde für die Authentifizierung gegenüber AWS erstellt, oder das Skript wurde aktualisiert, um auf den Namen Ihres Anmeldeinformationsassets zu verweisen.    
* Das AWS-PowerShell-Modul wurde in Azure Automation importiert.  
* Es wurde ein neues Runbook erstellt, und Parameterwerte wurden bei Bedarf überprüft und aktualisiert.  
* Für **Ausführliche Datensätze protokollieren** und optional **Statusdatensätze protokollieren** unter der Runbookeinstellung **Protokollierung und Nachverfolgung** wurde **Ein** festgelegt.<br><br> ![Runbookprotokollierung und -nachverfolgung](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png)  

1. Wir möchten das Runbook hier nur starten. Klicken Sie daher auf **Start** und dann auf dem Blatt „Runbook starten“ auf **OK**.
2. Geben Sie auf dem Blatt „Runbook starten“ einen Wert für **VMname**an.  Akzeptieren Sie die Standardwerte für die anderen Parameter, die Sie zuvor im Skript vorkonfiguriert haben.  Klicken Sie auf **OK**, um den Runbookauftrag zu starten.<br><br> ![New-AwsVM-Runbook starten](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
3. Ein Auftragsbereich für den soeben erstellten Runbookauftrag erscheint. Schließen Sie diesen Bereich.
4. Sie können den Fortschritt des Auftrags und Ausgabe-**Datenströme** anzeigen, indem Sie auf dem Blatt des Runbookauftrags auf die Kachel **Alle Protokolle** klicken.<br><br> ![Datenstromausgabe](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
5. Um die Bereitstellung der VM zu bestätigen, melden Sie sich bei der AWS-Verwaltungskonsole an (sofern Sie noch nicht angemeldet sind).<br><br> ![Per AWS-Konsole bereitgestellte VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die ersten Schritte mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md)
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)
* Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
* Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Native PowerShell Script Support in Azure Automation (Native PowerShell-Skriptunterstützung in Azure Automation)](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)


