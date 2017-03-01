---
title: Docker-Images in der Azure-Containerregistrierung | Microsoft-Dokumentation
description: "Push- und Pullübertragung von Docker-Images an eine Azure-Containerregistrierung mit der Docker CLI"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 1e70f41dd15b0243fb8edd137710ac555821434e
ms.lasthandoff: 02/22/2017

---
# <a name="push-your-first-image-to-a-container-registry-using-the-docker-cli"></a>Übertragen des ersten Image in eine Containerregistrierung mit der Docker CLI
Eine Azure-Containerregistrierung speichert und verwaltet private [Docker](http://hub.docker.com)-Containerimages. Dies ähnelt der Art und Weise, wie [Docker Hub](https://hub.docker.com/) öffentliche Docker-Images speichert. Für Ihre Containerregistrierung verwenden Sie die [Docker-Befehlszeilenschnittstelle](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) für die Vorgänge [Anmeldung](https://docs.docker.com/engine/reference/commandline/login/), [Push](https://docs.docker.com/engine/reference/commandline/push/), [Pull](https://docs.docker.com/engine/reference/commandline/pull/) und andere Vorgänge. 

Weitere Hintergrundinformationen und Konzepte finden Sie unter [Was ist die Azure-Containerregistrierung?](container-registry-intro.md).


> [!NOTE]
> Die Containerregistrierung befindet sich derzeit in der Vorschauphase.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Containerregistrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Installieren Sie das [Docker-Modul](https://docs.docker.com/engine/installation/), um Ihren lokalen Computer als Docker-Host einzurichten und auf die Befehle der Docker CLI zuzugreifen.

## <a name="log-in-to-a-registry"></a>Anmelden an einer Registrierung
Führen Sie `docker login` aus, um sich mit Ihren [Registrierungsanmeldeinformationen](container-registry-authentication.md) an der Containerregistrierung anzumelden.

Im folgenden Beispiel werden die ID und das Kennwort eines Azure Active Directory-[Dienstprinzipals](../active-directory/active-directory-application-objects.md) übergeben. Angenommen, Sie haben Ihrer Registrierung für ein Automatisierungsszenario einen Dienstprinzipal zugewiesen. 

```
docker login myregistry-contoso.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Stellen Sie sicher, dass Sie den vollqualifizierten Registrierungsnamen angeben (nur Kleinbuchstaben). In diesem Beispiel lautet er `myregistry-contoso.azurecr.io`.

## <a name="steps-to-pull-and-push-an-image"></a>Schritte für die Pull- und Pushübertragung eines Image
Im folgenden Beispiel wird ein Nginx-Image aus der öffentlichen Docker Hub-Registrierung heruntergeladen, für Ihre private Azure-Containerregistrierung gekennzeichnet, per Pushvorgang an Ihre Registrierung übertragen und dann per Pullvorgang zurückübertragen.

**1. Übertragen des offiziellen Docker-Image per Pullvorgang für Nginx**

Rufen Sie zuerst das öffentliche Nginx-Image auf Ihren lokalen Computer ab.

```
docker pull nginx
```
**2. Starten des Nginx-Containers**

Mit dem folgenden Befehl wird der lokale Nginx-Container interaktiv gestartet (zum Anzeigen der Ausgabe von Nginx), und über Port 8080 wird gelauscht. Wenn die Ausführung des Containers beendet wird, wird er hierbei entfernt.

```
docker run -it --rm -p 8080:80 nginx
```

Navigieren Sie zu [http://localhost:8080](http://localhost:8080), um den ausgeführten Container anzuzeigen. Es wird ein Bildschirm angezeigt, der dem folgenden Bildschirm ähnelt:

![Nginx auf lokalem Computer](./media/container-registry-get-started-docker-cli/nginx.png)

Drücken Sie [STRG]+[C], um den ausgeführten Container zu beenden.

**3. Erstellen eines Alias des Image in Ihrer Registrierung**

Mit dem folgenden Befehl wird ein Alias des Image mit einem vollqualifizierten Pfad zur Registrierung erstellt. In diesem Beispiel wird der `samples`-Namespace angegeben, um den Stamm der Registrierung nicht zu überladen.

```
docker tag nginx myregistry-exp.azurecr.io/samples/nginx
```  

**4. Übertragen des Image an Ihre Registrierung per Pushvorgang**

```
docker push myregistry-contoso.azurecr.io/samples/nginx
``` 

**5. Übertragen des Image aus Ihrer Registrierung per Pullvorgang**

```
docker pull myregistry-contoso.azurecr.io/samples/nginx
``` 

**6. Starten des Nginx-Containers aus der Registrierung**

```
docker run -it --rm -p 8080:80 myregistry-exp.azurecr.io/samples/nginx
```

Navigieren Sie zu [http://localhost:8080](http://localhost:8080), um den ausgeführten Container anzuzeigen.

Drücken Sie [STRG]+[C], um den ausgeführten Container zu beenden.

**6. Entfernen des Image**

```
docker rmi myregistry-contoso.azurecr.io/samples/nginx
```



## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen vertraut gemacht haben, können Sie mit der Verwendung Ihrer Registrierung beginnen! Beginnen Sie beispielsweise, indem Sie Containerimages in einem [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/)-Cluster bereitstellen.




