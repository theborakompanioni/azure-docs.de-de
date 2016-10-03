<properties
   pageTitle="Konfigurieren eines Docker-Hosts mit VirtualBox | Microsoft Azure"
   description="Schrittweise Anweisungen zum Konfigurieren einer Docker-Standardinstanz mithilfe von Docker Machine und VirtualBox"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# Konfigurieren eines Docker-Hosts mit VirtualBox

## Übersicht
Dieser Artikel führt Sie durch die Konfiguration einer Docker-Standardinstanz mithilfe von Docker Machine und VirtualBox. Bei Verwendung der [Betaversion von Docker für Windows](http://beta.docker.com/) ist diese Konfiguration nicht erforderlich.

## Voraussetzungen
Die folgenden Tools müssen installiert werden:

- [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox)

## Konfigurieren des Docker-Clients mit Windows PowerShell

Um einen Docker-Client zu konfigurieren, öffnen Sie einfach Windows PowerShell, und führen Sie die folgenden Schritte aus:

1. Erstellen Sie eine Standardinstanz des Docker-Hosts.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Stellen Sie sicher, dass die Standardinstanz konfiguriert ist und ausgeführt wird. (Für eine Instanz mit dem Namen „default“ sollte angezeigt werden, dass sie ausgeführt wird.)

    ```PowerShell
	docker-machine ls 
    ```
		
	![docker-machine ls output][0]
 
1. Legen Sie „default“ als aktuellen Host fest, und konfigurieren Sie die Shell.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Zeigen Sie die aktiven Docker-Container an. Die Liste sollte leer sein.

    ```PowerShell
	docker ps
    ```

	![docker ps output][1]
 
> [AZURE.NOTE] Bei jedem Neustart Ihres Entwicklungscomputers müssen Sie Ihren lokalen Docker-Host neu starten. Führen Sie dazu an einer Eingabeaufforderung den folgenden Befehl aus: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png

<!---HONumber=AcomDC_0921_2016-->