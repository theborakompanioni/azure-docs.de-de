<properties
	pageTitle="Abgelehnte oder fehlerhafte SSH-Verbindung mit einem virtuellen Linux-Computer | Microsoft Azure"
	description="Beheben Sie SSH-Fehler wie etwa fehlgeschlagene oder abgelehnte SSH-Verbindungen auf virtuellen Azure-Computern unter Linux."
	keywords="SSH-Verbindung abgelehnt, SSH-Fehler, Azure SSH, SSH-Verbindungsfehler"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="iainfou"/>

# Behandeln von Problemen, Fehlern oder Ablehnungen im Zusammenhang mit der SSH-Verbindung mit einem virtuellen Azure Linux-Computer

Es gibt verschiedene Gründe dafür, dass SSH-Fehler (Secure Shell) oder SSH-Verbindungsfehler auftreten oder dass die SSH-Verbindung abgelehnt wird, wenn Sie versuchen, eine Verbindung mit einem Linux-basierten virtuellen Azure-Computer herzustellen. Dieser Artikel hilft Ihnen, diese Probleme zu ermitteln und zu beheben.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](http://azure.microsoft.com/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](http://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](http://azure.microsoft.com/support/faq/).

## Virtuelle Computer, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden

Sie können die Anmeldeinformationen oder SSHD entweder über Befehle der Azure-Befehlszeilenschnittstelle oder mithilfe der [Azure-VMAccessForLinux-Erweiterung](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) zurücksetzen. Versuchen Sie nach jedem Problembehandlungsschritt, die Verbindung mit dem virtuellen Computer erneut herzustellen.

### Voraussetzungen für die Azure-Befehlszeilenschnittstelle

[Installieren Sie die Azure-Befehlszeilenschnittstelle, und stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../xplat-cli-install.md) (falls noch nicht geschehen). Melden Sie sich mit dem `azure login`-Befehl an, und stellen Sie sicher, dass Sie sich im Resource Manager-Modus befinden (`azure config mode arm`).

Stellen Sie sicher, dass der [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) Version 2.0.5 oder höher installiert ist.

### Zurücksetzen von SSHD
Die SSHD-Konfiguration selbst kann fehlerhaft sein, oder beim Dienst ist ein Fehler aufgetreten. Sie können SSHD zurücksetzen, um sicherzustellen, dass die SSH-Konfiguration gültig ist.

#### Azure-Befehlszeilenschnittstelle
```bash
azure vm reset-access -g <resource group> -n <vm name> -r
```

#### Erweiterungen für den Zugriff auf virtuelle Computer
Zugriffserweiterungen werden in einer JSON-Datei gelesen, die definiert, welche Aktion ausgeführt werden soll – z.B. Zurücksetzen von SSH, Zurücksetzen eines SSH-Schlüssels, Hinzufügen eines neuen Benutzers usw. Erstellen Sie zuerst eine Datei mit dem Namen „PrivateConf.json“ mit folgendem Inhalt:

```bash
{  
	"reset_ssh":"True"
}
```

Führen Sie anschließend manuell die `VMAccessForLinux`-Erweiterung aus, um die SSHD-Verbindung zurückzusetzen:

```bash
azure vm extension set <resource group> <vm name> VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
```

### Zurücksetzen von SSH-Anmeldeinformationen für einen Benutzer
Wenn SSHD ordnungsgemäß funktioniert, können Sie das Kennwort für einen bestimmten Benutzer zurücksetzen.

#### Azure-Befehlszeilenschnittstelle
```bash
azure vm reset-access -g <resource group> <vm name> -u <username> -p <new password>
```

Wenn Sie die SSH-Schlüsselauthentifizierung verwenden, können Sie den SSH-Schlüssel für einen bestimmten Benutzer zurücksetzen:

```bash
azure vm reset-access -g <resource group> -n <vm name> -u <usernamer> -M <~/.ssh/azure_id_rsa.pub>
```

#### Erweiterungen für den Zugriff auf virtuelle Computer
Erstellen Sie eine Datei mit dem Namen „PrivateConf.json“ mit folgendem Inhalt:

```bash
{
	"username":"Username", "password":"NewPassword"
}
```

Alternativ dazu können Sie eine Datei mit dem Namen „PrivateConf.json“ und dem folgenden Inhalt erstellen, um den SSH-Schlüssel für einen bestimmten Benutzer zurückzusetzen:

```bash
{
	"username":"Username", "ssh_key":"ContentsOfNewSSHKey"
}
```

Nach den oben genannten Schritten führen Sie manuell die `VMAccessForLinux`-Erweiterung aus, um die SSH-Benutzeranmeldeinformationen zurückzusetzen:

```
azure vm extension set <resource group> <vmname> VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
```

### Erneutes Bereitstellen eines virtuellen Computers
Sie können einen virtuellen Computer in Azure auf einem anderen Knoten erneut bereitstellen und dadurch möglicherweise zugrunde liegende Netzwerkprobleme beheben. Um einen virtuellen Computer im Azure-Portal erneut bereitzustellen, wählen Sie **Durchsuchen** > **Virtuelle Computer ** > *Ihr virtueller Linux-Computer* > **Erneut bereitstellen**. Informationen hierzu finden Sie unter [Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen](virtual-machines-windows-redeploy-to-new-node.md). Zurzeit können Sie virtuelle Computer nicht über die Azure-Befehlszeilenschnittstelle erneut bereitstellen.

> [AZURE.NOTE] Beachten Sie, dass nach Beenden dieses Vorgangs kurzlebige Datenträgerdaten verloren gehen und dynamische, dem virtuellen Computer zugeordnete IP-Adressen aktualisiert werden.


## Mit dem klassischen Bereitstellungsmodell erstellte virtuelle Computer

Führen Sie die folgenden Schritte aus, um die häufigsten SSH-Verbindungsfehler für virtuelle Computer zu beheben, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Versuchen Sie nach jedem Schritt, die Verbindung mit dem virtuellen Computer erneut herzustellen.

- Setzen Sie den Remotezugriff über das [Azure-Portal](https://portal.azure.com) zurück. Wählen Sie im Azure-Portal die Option **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Remotezugriff zurücksetzen** aus.

- Starten Sie den virtuellen Computer neu. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Neustart**.

	ODER

	Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Virtuelle Computer** > **Instanzen** > **Neustart**.

- Stellen Sie den virtuellen Computer auf einem neuen Azure-Knoten erneut bereit. Informationen hierzu finden Sie unter [Einen virtuellen Computer auf einem neuen Azure-Knoten erneut bereitstellen](virtual-machines-windows-redeploy-to-new-node.md).

	Beachten Sie, dass nach Beenden dieses Vorgangs kurzlebige Datenträgerdaten verloren gehen und dynamische, dem virtuellen Computer zugeordnete IP-Adressen aktualisiert werden.

- Führen Sie die unter [Zurücksetzen des Kennworts oder des SSH-Schlüssels für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md) beschriebenen Anweisungen aus:
	- Zurücksetzen des Kennworts oder des SSH-Schlüssels
	- Erstellen Sie ein neues _sudo_-Benutzerkonto.
	- Zurücksetzen der SSH-Konfiguration

- Überprüfen Sie die Ressourcenintegrität des virtuellen Computers auf etwaige Plattformprobleme.<br> Wählen Sie **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Einstellungen** > **Integrität prüfen**.


## Zusätzliche Ressourcen

- Wenn Sie nach Ausführung der Schritte immer noch nicht keine SSH-Verbindung mit Ihrem virtuellen Computer herstellen können, finden Sie unter [Ausführliche Schritte zur Problembehandlung bei SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) Hinweise zu zusätzlichen Netzwerkkonfigurationen und weitere Schritte, damit Sie das Problem lösen können.

- Weitere Informationen zur Problembehandlung beim Anwendungszugriff finden Sie unter [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](virtual-machines-linux-troubleshoot-app-connection.md).

- Weitere Informationen zur Problembehandlung bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden, finden Sie unter [Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md).

<!---HONumber=AcomDC_0713_2016-->