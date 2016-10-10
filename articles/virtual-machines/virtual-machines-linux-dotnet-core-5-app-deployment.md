<properties
   pageTitle="Automatisieren der Anwendungsbereitstellung mit VM-Erweiterungen | Microsoft Azure"
   description=".NET Core-Tutorial für virtuelle Azure-Computer"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# Anwendungsbereitstellung mit Azure Resource Manager-Vorlagen

Nachdem Sie alle Infrastrukturanforderungen ermittelt und in einer Bereitstellungsvorlage berücksichtigt haben, muss die eigentliche Anwendungsbereitstellung durchgeführt werden. In diesem Kontext meint „Anwendungsbereitstellung“ das Installieren der eigentlichen Anwendungsbinärdateien auf Azure-Ressourcen. Für das Music Store-Beispiel müssen auf jedem virtuellen Computer .NET Core, NGINX und Supervisor installiert und konfiguriert werden. Die Music Store-Binärdateien müssen auf jedem virtuellen Computer installiert und die Music Store-Datenbank muss vorab erstellt werden.

In diesem Dokument erfahren Sie, wie Sie mithilfe von VM-Erweiterungen die Anwendungsbereitstellung und -konfiguration auf virtuellen Azure-Computern automatisieren können. Alle Abhängigkeiten und individuellen Konfigurationen werden hervorgehoben. Stellen Sie am besten vorab eine Instanz der Lösung in Ihrem Azure-Abonnement bereit, und orientieren Sie sich an der Azure Resource Manager-Vorlage. Die vollständige Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## Konfigurationsskript

VM-Erweiterungen sind spezielle Programme, die zur Automatisierung des Konfigurationsprozesses für virtuelle Computer ausgeführt werden. Erweiterungen stehen für verschiedenste Zwecke (etwa Virenschutz, Protokollierungskonfiguration und Docker-Konfiguration) zur Verfügung. Mit einer benutzerdefinierten Skripterweiterung können Sie ein beliebiges Skript für einen virtuellen Computer ausführen. Beim Music Store-Beispiel konfiguriert die benutzerdefinierte Skripterweiterung die virtuellen Ubuntu-Computer und installiert die Music Store-Anwendung.

Machen Sie sich zunächst mit dem auszuführenden Skript vertraut, bevor Sie sich mit der Vorgehensweise zum Deklarieren von VM-Erweiterungen in einer Azure Resource Manager-Vorlage beschäftigen. Dieses Skript konfiguriert den virtuellen Ubuntu-Computer, der als Host für die Music Store-Anwendung fungiert. Wenn das Skript ausgeführt wird, installiert es sämtliche erforderliche Software sowie die Music Store-Anwendung über die Quellcodeverwaltung und bereitet die Datenbank vor.

Weitere Informationen zum Hosten einer .NET Core-Anwendung unter Linux finden Sie unter [Publish to a Linux Production Environment](https://docs.asp.net/en/latest/publishing/linuxproduction.html) (Veröffentlichen in einer Linux-Produktionsumgebung).


```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## VM-Skripterweiterung

VM-Erweiterungen können zur Buildzeit für einen virtuellen Computer ausgeführt werden. Hierzu muss die Erweiterungsressource in die Azure Resource Manager-Vorlage eingeschlossen werden. Die Erweiterung kann mithilfe des Visual Studio-Assistenten zum Hinzufügen von Ressourcen oder durch Einfügen von gültigem JSON-Code hinzugefügt werden. Die Skripterweiterungsressource ist innerhalb der Ressource des virtuellen Computers geschachtelt, wie im folgenden Beispiel zu sehen.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).

Beachten Sie, dass das Skript im folgenden JSON-Code in GitHub gespeichert ist. Dieses Skript könnte aber auch in Azure-Blobspeicher gespeichert sein. Bei Azure Resource Manager-Vorlagen kann die Zeichenfolge für die Skriptausführung außerdem so erstellt werden, dass Vorlagenparameterwerte als Parameter für die Skriptausführung verwendet werden. In diesem Fall werden Daten beim Bereitstellen der Vorlagen angegeben, und diese Werte können dann beim Ausführen des Skripts verwendet werden.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Weitere Informationen zum Verwenden der benutzerdefinierten Skripterweiterung finden Sie unter [Verwenden der benutzerdefinierten Skripterweiterung für virtuelle Linux-Computer mit Azure Resource Manager-Vorlagen](./virtual-machines-linux-extensions-customscript.md).

## Nächster Schritt

<hr>

[Sehen Sie sich weitere Azure Resource Manager-Vorlagen an.](https://github.com/Azure/azure-quickstart-templates)

<!---HONumber=AcomDC_0928_2016-->