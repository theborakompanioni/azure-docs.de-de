---
title: Authentifizieren mit einer Azure-Containerregistrierung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie sich an einer Azure-Containerregistrierung mit einem Azure Active Directory-Dienstprinzipal oder einem Administratorkonto anmelden.
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: 1a5af0b498cfdf1946f5c405d9557b0c2d2c8e63
ms.openlocfilehash: 1e9e54ee935b4c27eb93f72eb99c3ce52cc6c7e2

---
# <a name="authenticate-with-a-container-registry"></a>Authentifizieren für eine Containerregistrierung
Sie melden sich mit dem Befehl `docker login` an, um Containerimages in einer Azure-Containerregistrierung zu verwenden. Sie können sich anmelden, indem Sie entweder einen **[Azure Active Directory-Dienstprinzipal](../active-directory/active-directory-application-objects.md)** oder ein registrierungsspezifisches **Administratorkonto** nutzen. Dieser Artikel enthält weitere Details zu diesen Identitäten. 


> [!NOTE]
> Die Containerregistrierung befindet sich derzeit in der Vorschauphase. In der Vorschauversion werden einzelne Azure Active Directory-Identitäten (Zugriff/Steuerung pro Benutzer) zur Authentifizierung für eine Containerregistrierung nicht unterstützt. 
> 





## <a name="service-principal"></a>Dienstprinzipal

Sie können Ihrer Registrierung einen [Dienstprinzipal zuweisen](container-registry-get-started-azure-cli.md#assign-a-service-principal) und für die grundlegende Docker-Authentifizierung verwenden. Die Nutzung eines Dienstprinzipals wird für die meisten Szenarien empfohlen. Geben Sie die App-ID und das Kennwort des Dienstprinzipals für den Befehl `docker login` ein. Dies wird im folgenden Beispiel veranschaulicht:

```
docker login myregistry-contoso.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Nach der Anmeldung speichert Docker die Anmeldeinformationen zwischen, sodass Sie die App-ID nicht im Kopf behalten müssen.

> [!TIP]
> Bei Bedarf können Sie das Kennwort eines Dienstprinzipals neu generieren, indem Sie den Befehl `az ad sp reset-credentials` ausführen.
> 


Dienstprinzipale ermöglichen den [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md) auf eine Registrierung. Verfügbare Rollen sind „Leser“ (nur Pullzugriff), „Mitwirkender“ (Pull und Push) und „Besitzer“ (Pull, Push und Zuweisung von Rollen für andere Benutzer). Sie können einer Registrierung mehrere Dienstprinzipale zuweisen, damit Sie den Zugriff für unterschiedliche Benutzer oder Anwendungen definieren können. Dienstprinzipale ermöglichen auch „monitorlose“ Verbindungen mit einer Registrierung in Entwickler- oder DevOps-Szenarien, z.B.:

  * Containerbereitstellungen aus einer Registrierung in Orchestrierungssystemen, z.B. DC/OS, Docker Swarm und Kubernetes. Sie können Containerregistrierungen auch per Pullvorgang in andere Azure-Dienste einbinden, z.B. [Container Service](../container-service/index.md), [App Service](../app-service/index.md), [Batch](../batch/index.md) und [Service Fabric](../service-fabric/index.md).
  
  * Lösungen für Continuous Integration und Continuous Deployment (z.B. Visual Studio Team Services oder Jenkins), mit denen Containerimages erstellt und per Pushvorgang in eine Registrierung übertragen werden.
  
  



## <a name="admin-account"></a>Administratorkonto
Für jede von Ihnen erstellte Registrierung wird automatisch auch ein Administratorkonto erstellt. Das Konto ist standardmäßig deaktiviert, aber Sie können es aktivieren und die Anmeldeinformationen verwalten, z.B. über das [Portal](container-registry-get-started-portal.md#manage-registry-settings) oder mit den [Befehlen von Azure CLI 2.0 (Vorschau)](container-registry-get-started-azure-cli.md#manage-admin-credentials). Wenn das Konto aktiviert ist, können Sie den Benutzernamen und das Kennwort an den Befehl `docker login` übergeben, um eine grundlegende Authentifizierung für die Registrierung zu erhalten. Beispiel:

```
docker login myregistry-contoso.azurecr.io -u myAdminName -p myPassword
```

> [!IMPORTANT]
> Das Administratorkonto ist dafür ausgelegt, dass ein einzelner Benutzer auf die Registrierung zugreift (hauptsächlich für Testzwecke). Es wird nicht empfohlen, die Anmeldeinformationen für das Administratorkonto an andere Benutzer weiterzugeben. Alle Benutzer werden von der Registrierung wie ein Benutzer angesehen. Wenn dieses Konto geändert oder deaktiviert wird, wird der Zugriff auf die Registrierung für alle Benutzer deaktiviert, die diese Anmeldeinformationen verwenden. 
> 


### <a name="next-steps"></a>Nächste Schritte
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)
* Weitere Informationen zur Authentifizierung in der Containerregistrierung (Vorschau) finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/). 





<!--HONumber=Nov16_HO4-->


