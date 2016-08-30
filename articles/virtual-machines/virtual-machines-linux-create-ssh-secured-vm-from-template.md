<properties
	pageTitle="Erstellen einer Linux-VM mit einer Azure-Vorlage | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Linux-VM auf Azure mit einer Azure Resource Manager-Vorlage erstellen."
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
	ms.date="08/17/2016"
	ms.author="v-livech"/>

# Erstellen einer Linux-VM mit einer Azure-Vorlage

Dieser Artikel zeigt, wie Sie mithilfe einer Azure-Vorlage schnell einen virtuellen Linux-Computer auf Azure bereitstellen können. Für die Schritte in diesem Artikel sind ein Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)) und die Anmeldung der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) (`azure login`) erforderlich, die sich zudem im Resource Manager-Modus (`azure config mode arm`) befinden muss. Sie können mithilfe des [Azure-Portals](virtual-machines-linux-quick-create-portal.md) oder der [Azure-CLI](virtual-machines-linux-quick-create-cli.md) auch schnell einen virtuellen Linux-Computer bereitstellen.

## Kurze Zusammenfassung der Befehle

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Ausführliche exemplarische Vorgehensweise

Mit Vorlagen können Sie VMs unter Azure mit Einstellungen erstellen, die Sie während des Startprozesses anpassen möchten, z.B. Einstellungen wie Benutzer- und Hostnamen. Für diesen Artikel starten wir eine Azure-Vorlage, indem wir eine Ubuntu-VM mit einer Netzwerksicherheitsgruppe (NSG) mit geöffnetem Port 22 für SSH verwenden.

Azure Resource Manager-Vorlagen sind JSON-Dateien, die für einfache einmalige Aufgaben wie das Starten einer Ubuntu-VM in diesem Artikel verwendet werden können. Außerdem können Azure-Vorlagen auch zum Erstellen komplexer Azure-Konfigurationen für vollständige Umgebungen eingesetzt werden, z.B. eines Bereitstellungsstapels für Tests, Entwicklung oder Produktion.

## Erstellen der Linux-VM

Im folgenden Codebeispiel wird veranschaulicht, wie Sie `azure group create` aufrufen, um eine Ressourcengruppe zu erstellen, und gleichzeitig einen per SSH geschützten virtuellen Linux-Computer bereitstellen, indem Sie [diese Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) verwenden. Beachten Sie, dass Sie in Ihrem Beispiel Namen verwenden müssen, die für Ihre Umgebung eindeutig sind. In diesem Beispiel wird `quicksecuretemplate` als Ressourcengruppenname, `securelinux` als VM-Name und `quicksecurelinux` als Name einer Unterdomäne verwendet.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Ausgabe

```bash
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== vlivech@azure
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

In diesem Beispiel wird eine VM mit dem Parameter `--template-uri` bereitgestellt. Sie können eine Vorlage auch herunterladen oder lokal erstellen und mit dem Parameter `--template-file` mit einem Pfad zur Vorlagendatei als Argument übergeben. Sie werden von der Azure-Befehlszeilenschnittstelle aufgefordert, die für die Vorlage erforderlichen Parameter anzugeben.

## Nächste Schritte

Im [Katalog mit den Vorlagen](https://azure.microsoft.com/documentation/templates/) können Sie weitere App-Frameworks für die Bereitstellung auswählen.

<!---HONumber=AcomDC_0824_2016-->