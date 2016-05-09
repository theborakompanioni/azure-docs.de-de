<properties
	pageTitle="Erstellen von Docker-Hosts in Azure mit Docker Machine | Microsoft Azure"
	description="Beschreibt die Verwendung von Docker Machine zum Erstellen von Docker-Hosts in Azure."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="squillace"
	manager="timlt"
	editor="tysonn"/>

<tags
	ms.service="virtual-machines-linux"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure-services"
	ms.date="04/20/2016"
	ms.author="rasquill"/>

# Verwenden eines Docker-Computers mit dem Azure-Treiber

[Docker](https://www.docker.com/) ist einer der beliebtesten Virtualisierungsansätze, bei dem Linux-Container anstelle virtueller Computer genutzt werden, um Anwendungsdaten und Computing auf gemeinsam genutzten Ressourcen zu isolieren. In diesem Thema wird beschrieben, wann und wie Sie [Docker Machine](https://docs.docker.com/machine/) (der `docker-machine`-Befehl) zum Erstellen neuer virtueller Linux-Computer in Azure verwenden, die als Docker-Host für Ihre Linux-Container aktiviert werden.


## Erstellen von virtuellen Computern mit Docker Machine

Erstellen Sie Docker-Host-VMs in Azure mit dem `docker-machine create`-Befehl mithilfe des `azure`-Treiberarguments für die Treiberoption (`-d`) und alle anderen Argumente.

Das folgende Beispiel beruht auf den Standardwerten, öffnet aber Port 80 auf dem virtuellen Computer für das Internet zum Testen mit einem nginx-Container, macht `ops` zum angemeldeten Benutzer für SSH und ruft den neuen virtuellen Computer `machine` auf.

Geben Sie `docker-machine create --driver azure` ein, um die Optionen und ihre Standardwerte anzuzeigen. Sie können auch die [Docker-Azure-Treiber-Dokumentation](https://docs.docker.com/machine/drivers/azure/) lesen. (Beachten Sie: Wenn Sie die zweistufige Authentifizierung aktiviert haben, werden Sie aufgefordert, die Authentifizierung mithilfe der zweiten Stufe durchzuführen).

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Die Ausgabe sollte etwa wie folgt aussehen, je nachdem, ob Sie die zweistufige Authentifizierung in Ihrem Konto konfiguriert haben.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## Konfigurieren Ihrer Docker-Shell

Geben Sie nun `docker-machine env <VM name>` ein, um zu sehen, was Sie tun müssen, um die Shell zu konfigurieren.

```bash
docker-machine env machine
```

Damit werden die Umgebungsinformationen ausgedruckt, die etwa wie folgt aussehen. Beachten Sie, dass die IP-Adresse zugewiesen wurde, die Sie zum Testen der VM benötigen.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Sie können entweder den empfohlenen Konfigurationsbefehl ausführen oder die Umgebungsvariablen selbst festlegen.

## Ausführen eines Containers

Jetzt können Sie einen einfachen Webserver ausführen, um zu testen, ob alles ordnungsgemäß funktioniert. Hier verwenden wir ein standardmäßiges nginx-Image, geben an, dass es auf Port 80 lauschen soll, und dass beim Neustart des virtuellen Computers der Container ebenfalls neu starten soll (`--restart=always`).

```bash
docker run -d -p 80:80 --restart=always nginx
```

Die Ausgabe sollte etwa folgendermaßen aussehen:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## Testen des Containers

Untersuchen Sie ausgeführte Container mit `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Und prüfen Sie, ob Sie den ausgeführten Container sehen. Geben Sie `docker-machine ip <VM name>` ein, um die IP-Adresse zu suchen (wenn Sie sie seit dem `env`-Befehl vergessen haben):

![Ausführen des ngnix-Containers](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## Nächste Schritte

Bei Interesse können Sie die [Docker-VM-Erweiterung](virtual-machines-linux-dockerextension.md) für Azure ausprobieren, um den gleichen Vorgang mithilfe der Azure-Befehlszeilenschnittstelle oder Azure Resource Manager-Vorlagen auszuführen.

<!---HONumber=AcomDC_0427_2016-->