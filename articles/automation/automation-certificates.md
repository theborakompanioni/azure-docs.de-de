---
title: Zertifikatobjekte in Azure Automation | Microsoft Docs
description: "Zertifikate können sicher in Azure Automation gespeichert werden, sodass sie von Runbooks oder DSC-Konfigurationen zur Authentifizierung bei Azure und Drittanbieterressourcen verwendet werden können.  Dieser Artikel stellt eine ausführliche Beschreibung von Zertifikaten bereit und zeigt, wie diese in Textrunbooks und grafischen Runbooks eingesetzt werden."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
translationtype: Human Translation
ms.sourcegitcommit: 89e5486f3302098f3a1d49e4390ec5b21617d778
ms.openlocfilehash: fd1737a420c132dace9307436bfea98a9bde94a0

---

# <a name="certificate-assets-in-azure-automation"></a>Zertifikatobjekte in Azure Automation

Zertifikate können sicher in Azure Automation gespeichert werden, sodass Sie aus Runbooks oder DSC-Konfigurationen mithilfe der Aktivität **Get-AzureRmAutomationRmCertificate** für Azure Resource Manager-Ressourcen darauf zugreifen können. Auf diese Weise können Sie Runbooks und DSC-Konfigurationen erstellen, die Zertifikate für die Authentifizierung verwenden oder diese zu Azure oder zu Drittanbieterressourcen hinzufügen.

> [!NOTE] 
> Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird mit einem Masterzertifikat verschlüsselt und in Azure Automation gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel für das Automation-Konto mit dem Masterzertifikat verschlüsselt und anschließend zum Verschlüsseln des Objekts verwendet.
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten von Automation-Variablen mit Windows PowerShell verwendet. Sie gehören zum Lieferumfang des [Azure PowerShell-Moduls](../powershell-install-configure.md) , das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

|Cmdlets|Beschreibung|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Ruft Informationen über ein Zertifikat zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. Sie können mithilfe der Aktivität "Get-AutomationCertificate" nur das Zertifikat selbst abrufen.|
|[New-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Erstellt ein neues Zertifikat in Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Entfernt ein Zertifikat aus Azure Automation.|Erstellt ein neues Zertifikat in Azure Automation.
|[Set-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Legt die Eigenschaften für ein vorhandenes Zertifikat fest, lädt die Zertifikatdatei hoch und legt das Kennwort für eine PFX-Datei fest.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Lädt ein Dienstzertifikat für den angegebenen Clouddienst hoch.|


## <a name="creating-a-new-certificate"></a>Erstellen eines neues Zertifikats

Wenn Sie ein neues Zertifikat erstellen, laden Sie eine CER- oder PFX-Datei in Azure Automation hoch. Wenn Sie das Zertifikat als exportierbar kennzeichnen, können Sie es aus dem Azure Automation-Zertifikatspeicher übertragen. Ist das Zertifikat nicht exportierbar, können Sie es nur zum Signieren innerhalb des Runbooks oder der DSC-Konfiguration verwenden.


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>So erstellen Sie ein neues Zertifikat mit dem Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf die Kachel **Objekte**, um das Blatt **Objekte** zu öffnen.
1. Klicken Sie auf die Kachel **Zertifikate**, um das Blatt **Zertifikate** zu öffnen.
1. Klicken Sie oben im Blatt auf **Zertifikat hinzufügen** .
2. Geben Sie im Feld **Name** einen Namen für das Zertifikat ein.
2. Klicken Sie unterhalb von **Zertifikatdatei hochladen** auf **Datei auswählen**, um nach einer CER- oder PFX-Datei zu suchen.  Wenn Sie eine PFX-Datei auswählen, geben Sie ein Kennwort an, und legen Sie fest, ob das Zertifikat exportiert werden kann.
1. Klicken Sie auf **Erstellen** , um das neue Zertifikatobjekt zu speichern.


### <a name="to-create-a-new-certificate-with-windows-powershell"></a>So erstellen Sie ein neues Zertifikat mit Windows PowerShell

Das folgende Beispiel zeigt, wie Sie ein neues Automation-Zertifikat erstellen und es als exportierbar kennzeichnen. In diesem Beispiel wird eine vorhandene PFX-Datei importiert.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Verwenden eines Zertifikats

Sie müssen die Aktivität **Get-AutomationCertificate** verwenden, um ein Zertifikat zu verwenden. Eine Verwendung des Cmdlets [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) ist nicht möglich, da dieses Cmdlet Informationen zum Zertifikatobjekt, aber nicht zum Zertifikat selbst zurückgibt.

### <a name="textual-runbook-sample"></a>Beispiel für ein Textrunbook

Der folgende Beispielcode zeigt, wie Sie ein Zertifikat zu einem Clouddienst in einem Runbook hinzufügen. In diesem Beispiel wird das Kennwort aus einer verschlüsselten Automation-Variable abgerufen.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Beispiel für ein grafisches Runbook

Sie können einem grafischen Runbook **Get-AutomationCertificate** hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors mit der rechten Maustaste auf das Zertifikat klicken und **Zum Zeichenbereich hinzufügen** auswählen.

![Hinzufügen eines Zertifikats zum Zeichenbereich](media/automation-certificates/automation-certificate-add-to-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung eines Zertifikats in einem grafischen Runbook.  Es handelt sich um das oben gezeigte Beispiel zum Hinzufügen eines Zertifikats zu einem Clouddienst aus einem Textrunbook.

![Beispiel für grafische Inhaltserstellung ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Verknüpfungen zum Steuern des logischen Ablaufs von Aktivitäten, die Ihr Runbook ausführen soll, finden Sie unter [Verknüpfungen bei der grafischen Erstellung](automation-graphical-authoring-intro.md#links-and-workflow). 



<!--HONumber=Feb17_HO2-->


