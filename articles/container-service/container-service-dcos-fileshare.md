---
title: "Dateifreigabe für Azure DC/OS-Cluster | Microsoft-Dokumentation"
description: "Bereitstellen und Einbinden einer Dateifreigabe für einen DC/OS-Cluster in Azure Container Service"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure, FileShare, cifs
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2017
ms.author: juliens
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 6d40821327a9df47bb85ea12ecd33e4a0f49e39e
ms.lasthandoff: 03/22/2017


---
# <a name="create-and-mount-a-file-share-to-a-dcos-cluster"></a>Bereitstellen und Einbinden einer Dateifreigabe für einen DC/OS-Cluster
In diesem Artikel wird untersucht, wie eine Dateifreigabe unter Azure erstellt und in jeden Agent und Master des DC/OS-Clusters eingebunden wird. Das Einrichten einer Dateifreigabe vereinfacht das Freigeben von Dateien über Ihren gesamten Cluster hinweg im Hinblick auf Konfiguration, Zugriff, Protokolle und mehr.

Damit Sie dieses Beispiel durcharbeiten können, benötigen Sie einen DC/OS-Cluster, der im Azure Container Service konfiguriert ist. Weitere Informationen dazu finden Sie unter [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md).

## <a name="create-a-file-share-on-microsoft-azure"></a>Erstellen einer Dateifreigabe auf Microsoft Azure
### <a name="using-the-portal"></a>Verwenden des Portals

1. Melden Sie sich beim Portal an.
2. Erstellen Sie ein Speicherkonto.
   
  ![Azure-Container-Service – Speicherkonto erstellen](media/container-service-dcos-fileshare/createSA.png)

3. Klicken Sie nach der Erstellung im Abschnitt **Dienste** auf **Dateien**.
   
  ![Azure Container Service-Abschnitt „Dateien“](media/container-service-dcos-fileshare/filesServices.png)

4. Klicken Sie auf **+ Dateifreigabe**, und geben Sie einen Namen für diese neue Freigabe ein (**Kontingent** ist nicht obligatorisch).
   
  ![Azure Container Service – „+ Dateifreigabe“](media/container-service-dcos-fileshare/newFileShare.png)  

### <a name="using-azure-cli-20"></a>Mithilfe von Azure-CLI 2.0

Wenn es erforderlich ist, [installieren Sie das Azure-CLI, und richten Sie es ein](/cli/azure/install-azure-cli.md).

```azurecli
################# Change these four parameters ##############
DCOS_PERS_STORAGE_ACCOUNT_NAME=anystorageaccountname
DCOS_PERS_RESOURCE_GROUP=AnyResourceGroupName
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=demoshare
#############################################################

# Create the storage account with the parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-the-share-in-your-cluster"></a>Einbinden der Freigabe in Ihren Cluster

Im nächsten Schritt müssen wir diese Freigabe auf jedem virtuellen Computer innerhalb des Clusters mithilfe des CIFS-Tools/Protokolls einbinden. Dies erfolgt mithilfe der folgenden Befehlszeile: `mount -t cifs`.

Hier ist ein Beispiel unter Verwendung von:
* Speicherkontoname **`anystorageaccountname`**
* Fiktivem Kontoschlüssel **`P/GuXXXuoRtIVsV+faSfLhuNyZDrTzPmZDm3RyCL4XS6ghyiHYriN12gl+w5JMN2gXGtOhCzxFf2JuGqXXXX1w==`** 
* Bereitstellungspunkt **`/mnt/share/demoshare`**

```bash
sudo mount -t cifs //anystorageaccountname.file.core.windows.net/demoshare /mnt/share/demoshare -o vers=3.0,username=anystorageaccountname,password=P/GuXXXuoRtIVsV+faSfLhuNyZDrTzPmZDm3RyCL4XS6ghyiHYriN12gl+w5JMN2gXGtOhCzxFf2JuGqXXXX1w==,dir_mode=0777,file_mode=0777
```

Wir führen diesen Befehl auf jedem virtuellen Computer Ihres Clusters aus (Master- und Agentknoten). Wenn Sie über eine große Anzahl Agents verfügen, empfiehlt sich die Automatisierung dieses Vorgangs mithilfe von Skripts.  

### <a name="set-up-scripts"></a>Einrichten von Skripts

1. Stellen Sie zunächst eine SSH-Verbindung mit dem Master (oder dem ersten Master) Ihres DC/OS-basierten Clusters her. Beispielsweise `ssh userName@masterFQDN –A –p 22`, wobei der masterFQDN der vollqualifizierte Domänenname des virtuellen Mastercomputers ist.

2. Kopieren Sie Ihren privaten Schlüssel in das Arbeitsverzeichnis (~) auf dem Master.

3. Ändern Sie die Berechtigungen dafür mit dem folgenden Befehl: `chmod 600 yourPrivateKeyFile`.

4. Importieren Sie Ihren privaten Schlüssel mithilfe des `ssh-add yourPrivateKeyFile`-Befehls. Möglicherweise müssen Sie `eval ssh-agent -s` ausführen, wenn der erste Versuch keinen Erfolg hat.

5. Erstellen Sie auf dem Master zwei Dateien mit Ihrem bevorzugten Editor, wie etwa vi, nano oder vim: 
  
  * Eine mit dem Skript, das auf jedem virtuellen Computer ausgeführt werden soll, und dem Namen **cifsMount.sh** 
  * Eine weitere zum Herstellen aller SSH-Verbindungen, die das erste Skript aufrufen sollen, die den Namen **mountShares.sh** erhält


```bash
# cifsMount.sh

# Install the cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/demoshare" ]; then sudo mkdir -p "/mnt/share/demoshare" ; fi

# Mount the share under the previous local folder created
sudo mount -t cifs //anystorageaccountname.file.core.windows.net/demoshare /mnt/share/demoshare -o vers=3.0,username=anystorageaccountname,password=P/GuXXXuoRtIVsV+faSfLhuNyZDrTzPmZDm3RyCL4XS6ghyiHYriN12gl+w5JMN2gXGtOhCzxFf2JuGqXXXX1w==,dir_mode=0777,file_mode=0777
```
  
```bash
# mountShares.sh

# Install jq used for the next command
sudo apt-get install jq

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/demoshare" ]; then sudo mkdir -p "/mnt/share/demoshare" ; fi

# Mount the share on the current vm (master)
sudo mount -t cifs //anystorageaccountname.file.core.windows.net/demoshare /mnt/share/demoshare -o vers=3.0,username=anystorageaccountname,password=P/GuXXXuoRtIVsV+faSfLhuNyZDrTzPmZDm3RyCL4XS6ghyiHYriN12gl+w5JMN2gXGtOhCzxFf2JuGqXXXX1w==,dir_mode=0777,file_mode=0777

# Get the IP address of each node using the mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes
  
# From the previous file created, run our script to mount our share on each node
cat nodes | while read line
  do
    ssh `whoami`@$line -o StrictHostKeyChecking=no -i yourPrivateKeyFile < ./cifsMount.sh
    done
```  
> [!IMPORTANT]
> Sie müssen den Befehl **'mount'** um Ihre eigenen Einstellungen erweitern, wie etwa den Namen Ihres Speicherkontos und das Kennwort.
>  

Der Ordner, in dem Sie die erwähnten Skripts erstellt haben, sollte nun drei Dateien aufweisen:  

* **cifsMount.sh**
* **mountShares.sh**
* **IhrePrivaterSchlüsselDatei** 

### <a name="run-the-scripts"></a>Ausführen der Skripts

Führen Sie die **mountShares.sh**-Datei mit dem folgenden Befehl aus: `sh mountShares.sh`.

Die Ausgabe des Ergebnisses sollte im Terminal angezeigt werden. Nach der Ausführung der Skripts können Sie die Dateifreigabe in Ihrem Cluster verwenden.

Sie können die Skripts optimieren, aber dieses Beispiel ist geradlinig und dient zur Anleitung.

> [!NOTE] 
> Diese Methode wird nicht für Szenarien mit hohen Anforderungen an IOPS empfohlen, sie ist aber sehr nützlich, um Dokumente und Informationen im gesamten Cluster freizugeben.
>

## <a name="next-steps"></a>Nächste Schritte
* Erhalten Sie weitere Informationen zum [Verwalten Ihrer DC/OS-Container](container-service-mesos-marathon-ui.md).
* DC/OS-Containerverwaltung über die [Marathon-REST-API](container-service-mesos-marathon-rest.md).
