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
	ms.date="04/29/2016"
	ms.author="v-livech"/>

# Erstellen einer Linux-VM mit einer Azure-Vorlage

Dieser Artikel zeigt, wie Sie mithilfe einer Azure-Vorlage schnell einen virtuellen Linux-Computer auf Azure bereitstellen können. Für die Schritte in diesem Artikel ist ein Azure-Konto ([kostenlose Testversion herunterladen](https://azure.microsoft.com/pricing/free-trial/)) und eine Anmeldung bei der [Azure-CLI](../xplat-cli-install.md) (`azure login`) im Resource Manager-Modus (`azure config mode arm`) erforderlich. Sie können mithilfe des [Azure-Portals](virtual-machines-linux-quick-create-portal.md) oder der [Azure-CLI](virtual-machines-linux-quick-create-cli.md) auch schnell einen virtuellen Linux-Computer bereitstellen.


## Schnellbefehl

Ersetzen Sie in den folgenden Befehlsbeispielen die Werte zwischen &lt; und &gt; durch die Werte aus Ihrer eigenen Umgebung.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Ausführliche exemplarische Vorgehensweise

Mit Vorlagen können Sie VMs unter Azure mit Einstellungen erstellen, die Sie während des Startprozesses anpassen möchten, z.B. Benutzer- und Hostnamen. In diesem Artikel geht es um das Starten einer Ubuntu-VM mit einer Azure-Vorlage, bei der eine Netzwerksicherheitsgruppe (NSG) mit nur einem geöffneten Port (22 für SSH) erstellt wird und für die zur Anmeldung SSH-Schlüssel benötigt werden.

Bei Azure Resource Manager-Vorlagen handelt es sich um JSON-Dateien, die für einfache einmalige Aufgaben verwendet werden können, z.B. das Starten einer Ubuntu-VM wie in diesem Artikel. Sie können auch zum Erstellen komplexer Azure-Konfigurationen für ganze Umgebungen verwendet werden, z.B. eine Test-, Entwicklungs- oder Produktionsbereitstellung vom Netzwerk über das Betriebssystem bis zur Bereitstellung des Anwendungsstapels.

## Erstellen der Linux-VM

Im folgenden Codebeispiel wird veranschaulicht, wie Sie `azure group create` aufrufen, um eine Ressourcengruppe zu erstellen, und gleichzeitig einen per SSH geschützten virtuellen Linux-Computer bereitstellen, indem Sie [diese Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) verwenden. Beachten Sie, dass Sie in Ihrem Beispiel Namen verwenden müssen, die für Ihre Umgebung eindeutig sind. In diesem Beispiel wird `quicksecuretemplate` als Ressourcengruppenname, `securelinux` als VM-Name und `quicksecurelinux` als Name einer Unterdomäne verwendet.

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

Sie können mithilfe des Parameters `--template-uri` eine neue Ressourcengruppe erstellen und einen virtuellen Computer bereitstellen. Außerdem können Sie eine Vorlage herunterladen oder lokal erstellen und die Vorlage mit dem Parameter `--template-file` und einem Pfad zur Vorlagendatei als Argument übergeben. Sie werden von der Azure-Befehlszeilenschnittstelle aufgefordert, die für die Vorlage erforderlichen Parameter anzugeben.

## Nächste Schritte

Nachdem Sie Linux-VMs mit Vorlagen erstellt haben, können Sie sich darüber informieren, welche anderen App-Frameworks für die Bereitstellung mit Vorlagen verfügbar sind. Im [Katalog mit den Vorlagen](https://azure.microsoft.com/documentation/templates/) können Sie weitere App-Frameworks für die Bereitstellung auswählen.

<!---HONumber=AcomDC_0504_2016-->