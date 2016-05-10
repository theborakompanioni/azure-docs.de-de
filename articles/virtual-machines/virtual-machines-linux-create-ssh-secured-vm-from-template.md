<properties
	pageTitle="Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine geschützte Linux-VM unter Azure mit einer Azure Resource Manager-Vorlage erstellen."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="vlivech"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="04/27/2016"
	ms.author="v-livech"/>

# Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage

Zum Erstellen eines virtuellen Linux-Computers aus einer Vorlage benötigen Sie [die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`).

## Kurze Zusammenfassung der Befehle

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Ausführliche exemplarische Vorgehensweise

Mit Vorlagen können Sie VMs unter Azure mit Einstellungen erstellen, die Sie während des Startprozesses anpassen möchten, z.B. Benutzer- und Hostnamen. In diesem Artikel geht es um das Starten einer Ubuntu-VM mit einer Azure-Vorlage, bei der eine Netzwerksicherheitsgruppe (NSG) mit nur einem geöffneten Port (22 für SSH) erstellt wird und für die zur Anmeldung SSH-Schlüssel benötigt werden.

Bei Azure Resource Manager-Vorlagen handelt es sich um JSON-Dateien, die für einfache einmalige Aufgaben verwendet werden können, z.B. das Starten einer Ubuntu-VM wie in diesem Artikel. Sie können auch zum Erstellen komplexer Azure-Konfigurationen für ganze Umgebungen verwendet werden, z.B. eine Test-, Entwicklungs- oder Produktionsbereitstellung vom Netzwerk über das Betriebssystem bis zur Bereitstellung des Anwendungsstapels.

## Erstellen der Linux-VM

Im folgenden Codebeispiel wird veranschaulicht, wie Sie `azure group create` aufrufen, um eine Ressourcengruppe zu erstellen, und gleichzeitig einen per SSH geschützten virtuellen Linux-Computer bereitstellen, indem Sie [diese Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) verwenden. Beachten Sie, dass Sie in Ihrem Beispiel Namen verwenden müssen, die für Ihre Umgebung eindeutig sind. In diesem Beispiel wird `quicksecuretemplate` als Ressourcengruppenname, `securelinux` als VM-Name und `quicksecurelinux` als Name einer Unterdomäne verwendet.

```bash
chrisl@fedora$ azure group create -n quicksecuretemplate -l eastus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Sie können mithilfe des Parameters `--template-uri` eine neue Ressourcengruppe erstellen und einen virtuellen Computer bereitstellen. Außerdem können Sie eine Vorlage herunterladen oder lokal erstellen und die Vorlage mit dem Parameter `--template-file` und einem Pfad zur Vorlagendatei als Argument übergeben. Sie werden von der Azure-Befehlszeilenschnittstelle aufgefordert, die für die Vorlage erforderlichen Parameter anzugeben.

## Nächste Schritte

Nachdem Sie Linux-VMs mit Vorlagen erstellt haben, können Sie sich darüber informieren, welche anderen App-Frameworks für die Verwendung mit Vorlagen verfügbar sind. Im [Katalog mit den Vorlagen](https://azure.microsoft.com/documentation/templates/) können Sie weitere App-Frameworks für die Bereitstellung auswählen.

<!---HONumber=AcomDC_0504_2016-->