<properties
	pageTitle="Installieren von Symantec Endpoint Protection auf einem neuen virtuellen Computer | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie die Sicherheitserweiterung Symantec Endpoint Protection auf einem neuen oder vorhandenen virtuellen Azure-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde, installieren und konfigurieren."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="iainfou"/>

# Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Windows-Computer

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In diesem Artikel wird beschrieben, wie der Symantec Endpoint Protection-Client auf einem vorhandenen virtuellen Windows Server-Computer installiert und konfiguriert wird. Dies ist der vollständige Client, der Dienste wie Viren- und Spywareschutz, Firewall und Eindringschutz enthält. Der Client wird als Sicherheitserweiterung durch die Verwendung des VM Agent installiert.

Wenn Sie über ein vorhandenes Abonnement von Symantec für eine lokale Lösung verfügen, können Sie es zum Schützen Ihrer virtuellen Azure-Computer verwenden. Wenn Sie noch kein Kunde sind, können Sie sich für ein Testabonnement registrieren. Weitere Informationen über diese Lösung finden Sie unter [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec] (Symantec Endpoint Protection auf der Azure-Plattform von Microsoft) (in englischer Sprache). Diese Seite enthält auch Links zu Lizenzierungsinformationen und Anweisungen für das Installieren des Clients, wenn Sie bereits Symantec-Kunde sind.

## Installieren von Symantec Endpoint Protection auf einem vorhandenen virtuellen Computer

Bevor Sie beginnen, benötigen Sie Folgendes:

- Das Azure PowerShell-Modul, Version 0.8.2 oder höher, muss auf Ihrem Arbeitscomputer installiert sein. Sie können die installierte Version von Azure PowerShell mit dem Befehl **Get-Module azure | format-table version** überprüfen. Anweisungen und einen Link zur neuesten Version finden Sie unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell][PS]. Melden Sie sich mit `Add-AzureAccount` bei Ihrem Azure-Abonnement an.

- Der VM-Agent muss auf dem virtuellen Azure-Computer ausgeführt werden.

Stellen Sie zunächst sicher, dass der VM-Agent bereits auf dem virtuellen Computer installiert ist. Füllen Sie den Clouddienstnamen und den Namen des virtuellen Computers aus, und führen Sie dann die folgenden Befehle an einer Azure PowerShell-Eingabeaufforderung mit Administratorrechten aus. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen < und >.

> [AZURE.TIP] Wenn Sie den Namen des Clouddiensts und des virtuellen Computers nicht kennen, führen Sie **Get-AzureVM** aus, um die Namen aller virtuellen Computer im aktuellen Abonnement aufzulisten.

	$CSName = "<cloud service name>"
	$VMName = "<virtual machine name>"
	$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
	write-host $vm.VM.ProvisionGuestAgent

Der Befehl **write-host** zeigt **True** an, wenn der VM-Agent installiert ist. Wenn **False** zurückgegeben wird, nutzen Sie die Anweisungen und den Link zum Download im Azure-Blogbeitrag [VM Agent and Extensions - Part 2][Agent] (VM-Agent und Erweiterungen, Teil 2, in englischer Sprache).

Wenn der VM-Agent installiert ist, führen Sie diese Befehle aus, um den Symantec Endpoint Protection Agent zu installieren.

	$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

	Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

So überprüfen Sie, ob die Symantec-Sicherheitserweiterung installiert wurde und auf dem neuesten Stand ist:

1.	Melden Sie sich beim virtuellen Computer an. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird][Logon].
2.	Klicken Sie für Windows Server 2008 R2 auf **Start > Symantec Endpoint Protection**. Geben Sie für Windows Server 2012 oder Windows Server 2012 R2 im Startbildschirm **Symantec** ein, und klicken Sie dann auf **Symantec Endpoint Protection**.
3.	Wenden Sie im Fenster **Status-Symantec Endpoint Protection** auf der Registerkarte **Status** Updates an, oder führen Sie bei Bedarf einen Neustart aus.

## Zusätzliche Ressourcen

[Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird][Logon]

[Azure-VM-Erweiterungen und Features][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493

<!---HONumber=AcomDC_0831_2016-->