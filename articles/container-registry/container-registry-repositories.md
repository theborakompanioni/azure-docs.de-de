---
title: Azure-Containerregistrierungsrepositorys | Microsoft-Dokumentation
description: "Verwenden der Azure-Containerregistrierungsrepositorys für Docker-Images"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 06b809c31cecef1714f60d04657eb74c611be8cb
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017

---
# <a name="azure-container-registry-repositories"></a>Azure-Containerregistrierungsrepositorys

Mit der Azure-Containerregistrierung können Sie Containerimages in Repositorys speichern. Indem Sie Images in Repositorys speichern, können Sie Gruppen von Images (oder Versionen von Images) in einer isolierten Umgebung ablegen. Sie können diese Repositorys angeben, wenn Sie Images in Ihre Registrierung übertragen (Push).


## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Containerregistrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Installieren Sie das [Docker-Modul](https://docs.docker.com/engine/installation/), um Ihren lokalen Computer als Docker-Host einzurichten und auf die Befehle der Docker CLI zuzugreifen.
* **Abrufen eines Images (Pull)**: Rufen Sie ein Image aus der öffentlichen Docker Hub-Registrierung ab, markieren Sie es mit einem Tag, und übertragen Sie es in Ihre Registrierung. Anleitungen dazu, wie Images übertragen und abrufen, finden Sie unter [Pushübertragung eines Docker-Images in eine private Azure-Registrierung](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-the-portal"></a>Anzeigen von Repositorys im Portal

Nachdem Sie Images in Ihre Containerregistrierung übertragen haben, wird im Azure-Portal eine Liste der Repositorys angezeigt, in denen die Images gehostet werden.

Wenn Sie die Schritte im Artikel [Pushübertragung eines Docker-Images in eine private Azure-Registrierung](container-registry-get-started-docker-cli.md) ausgeführt haben, verfügen Sie jetzt über ein Nginx-Image in Ihrer Containerregistrierung. Im Rahmen der Anweisungen sollten Sie einen Namespace für das Image angegeben haben. Im folgenden Beispiel wird das NGinx-Image mit dem Befehl in das Repository „Samples“ übertragen:

```
docker push myregistry.azurecr.io/samples/nginx
```
 Die Azure-Containerregistrierung unterstützt Repositorynamespaces mit mehreren Ebenen. Mit dieser Funktion können Sie Sammlungen mit Images für eine bestimmte App oder eine Sammlung von Apps für bestimmte Entwicklungs- oder Betriebsteams gruppieren. Weitere Informationen zu Repositorys in Containerregistrierungen finden Sie unter [Private Docker-Containerregistrierungen in Azure](container-registry-intro.md).

So zeigen Sie die Containerregistrierungsrepositorys an:

1. Anmelden beim Azure-Portal
2. Wählen Sie auf dem Blatt **Azure Containerregistrierung** die Registrierung aus, die Sie untersuchen möchten.
3. Klicken Sie auf dem Blatt „Registrierung“ auf **Repositorys**, um eine Liste aller Repositorys und Images anzuzeigen.
4. (Optional) Wählen Sie ein bestimmtes Image aus, um die Tags anzuzeigen

![Repositorys im Portal](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen vertraut gemacht haben, können Sie mit der Verwendung Ihrer Registrierung beginnen! Beginnen Sie beispielsweise, indem Sie Containerimages in einem [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/)-Cluster bereitstellen.

