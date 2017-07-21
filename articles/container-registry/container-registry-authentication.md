---
title: Authentifizieren mit einer Azure-Containerregistrierung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie sich an einer Azure-Containerregistrierung mit einem Azure Active Directory-Dienstprinzipal oder einem Administratorkonto anmelden.
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: aa2a6bf3d7d9ec22020036851fc0f2bca37e31bf
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Authentifizieren mit einer privaten Docker-Containerregistrierung
Sie melden sich mit dem Befehl `docker login` an, um Containerimages in einer Azure-Containerregistrierung zu verwenden. Sie können sich anmelden, indem Sie entweder einen **[Azure Active Directory-Dienstprinzipal](../active-directory/active-directory-application-objects.md)** oder ein registrierungsspezifisches **Administratorkonto** nutzen. Dieser Artikel enthält weitere Details zu diesen Identitäten.



## <a name="service-principal"></a>Dienstprinzipal

Sie können Ihrer Registrierung einen [Dienstprinzipal zuweisen](container-registry-get-started-azure-cli.md#assign-a-service-principal) und für die grundlegende Docker-Authentifizierung verwenden. Die Nutzung eines Dienstprinzipals wird für die meisten Szenarien empfohlen. Geben Sie die App-ID und das Kennwort des Dienstprinzipals für den Befehl `docker login` ein. Dies wird im folgenden Beispiel veranschaulicht:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Nach der Anmeldung speichert Docker die Anmeldeinformationen zwischen, sodass Sie die App-ID nicht im Kopf behalten müssen.

> [!TIP]
> Bei Bedarf können Sie das Kennwort eines Dienstprinzipals neu generieren, indem Sie den Befehl `az ad sp reset-credentials` ausführen.
>


Dienstprinzipale ermöglichen den [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md) auf eine Registrierung. Folgende Rollen sind verfügbar:
  * Leser (nur Pullzugriff)
  * Mitwirkender (Pull und Push)
  * Besitzer (Pull, Push und Zuweisen von Rollen an andere Benutzer)

Anonymer Zugriff ist für Azure-Containerregistrierungen nicht verfügbar. Für öffentliche Images können Sie [Docker Hub](https://docs.docker.com/docker-hub/) verwenden.

Sie können einer Registrierung mehrere Dienstprinzipale zuweisen, damit Sie den Zugriff für unterschiedliche Benutzer oder Anwendungen definieren können. Dienstprinzipale ermöglichen auch „monitorlose“ Verbindungen mit einer Registrierung in Entwickler- oder DevOps-Szenarien, z.B.:

  * Containerbereitstellungen aus einer Registrierung in Orchestrierungssystemen, z.B. DC/OS, Docker Swarm und Kubernetes. Sie können Containerregistrierungen auch per Pullvorgang in andere Azure-Dienste einbinden, z.B. [Container Service](../container-service/index.yml), [App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) und weitere Dienste.

  * Lösungen für Continuous Integration und Continuous Deployment (z.B. Visual Studio Team Services oder Jenkins), mit denen Containerimages erstellt und per Pushvorgang in eine Registrierung übertragen werden.





## <a name="admin-account"></a>Administratorkonto
Für jede von Ihnen erstellte Registrierung wird automatisch auch ein Administratorkonto erstellt. Das Konto ist standardmäßig deaktiviert, aber Sie können es aktivieren und die Anmeldeinformationen verwalten, z.B. über das [Portal](container-registry-get-started-portal.md#manage-registry-settings) oder mit den [Befehlen von Azure CLI 2.0](container-registry-get-started-azure-cli.md#manage-admin-credentials). Jedes Administratorkonto erhält zwei Kennwörter, die beide erneut generiert werden können. Die beiden Kennwörter ermöglichen es Ihnen, Verbindungen mit der Registrierung aufrechtzuerhalten, indem Sie ein Kennwort verwenden, während Sie das andere Kennwort neu generieren. Wenn das Konto aktiviert ist, können Sie den Benutzernamen und eines der Kennwörter an den Befehl `docker login` übergeben, um die Standardauthentifizierung für die Registrierung zu erhalten. Beispiel:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> Das Administratorkonto ist dafür ausgelegt, dass ein einzelner Benutzer auf die Registrierung zugreift (hauptsächlich für Testzwecke). Es wird nicht empfohlen, die Anmeldeinformationen für das Administratorkonto an andere Benutzer weiterzugeben. Alle Benutzer werden von der Registrierung wie ein Benutzer angesehen. Wenn dieses Konto geändert oder deaktiviert wird, wird der Zugriff auf die Registrierung für alle Benutzer deaktiviert, die diese Anmeldeinformationen verwenden.
>


### <a name="next-steps"></a>Nächste Schritte
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)
* Weitere Informationen zur Authentifizierung in der Containerregistrierung (Vorschau) finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).

