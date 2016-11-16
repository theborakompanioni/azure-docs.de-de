---
title: Anwendungs- oder benutzerspezifischer Marathon-Dienst | Microsoft Docs
description: Erstellen eines anwendungs- oder benutzerspezifischen Marathon-Diensts
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container, Marathon, Microservices, DC/OS, Azure
ms.assetid: 16ecc16e-e504-480e-8dc3-cac14e9e1561
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: befd26888b653c660cc614221684732771496aed


---
# <a name="create-an-application-or-userspecific-marathon-service"></a>Erstellen eines anwendungs- oder benutzerspezifischen Marathon-Diensts
Für Azure Container Service wird eine Gruppe von Masterservern bereitgestellt, auf denen wir Apache Mesos und Marathon vorkonfigurieren. Diese Komponenten können zum Orchestrieren Ihrer Anwendungen im Cluster verwendet werden, aber es ist ratsam, die Masterserver nicht für diese Zwecke zu nutzen. Zum Optimieren der Konfiguration von Marathon ist beispielsweise das Anmelden an den Masterservern selbst und das Vornehmen von Änderungen erforderlich. Dies führt zu speziellen Masterservern, die sich vom Standard leicht unterscheiden und unabhängig gepflegt und verwaltet werden müssen. Außerdem ist eine Konfiguration, die für ein Team erforderlich ist, für ein anderes möglicherweise ungeeignet.

In diesem Artikel wird beschrieben, wie Sie einen anwendungs- oder benutzerspezifischen Marathon-Dienst hinzufügen.

Da sich dieser Dienst im Besitz eines einzelnen Benutzers oder Teams befindet, kann er wie gewünscht konfiguriert werden. Außerdem wird mit Azure Container Service sichergestellt, dass der Dienst weiter ausgeführt wird. Wenn der Dienst ausfällt, wird er von Azure Container Service für Sie neu gestartet. Meistens merken Sie gar nicht, dass es zu einem Ausfall gekommen ist.

## <a name="prerequisites"></a>Voraussetzungen
[Stellen Sie eine Instanz von Azure Container Service](container-service-deployment.md) mit dem Orchestratortyp DC/OS bereit, und [stellen Sie sicher, dass der Client eine Verbindung mit Ihrem Cluster herstellen kann](container-service-connect.md). Führen Sie außerdem die folgenden Schritte aus:

[!INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-userspecific-marathon-service"></a>Erstellen eines anwendungs- oder benutzerspezifischen Marathon-Diensts
Erstellen Sie zunächst eine JSON-Konfigurationsdatei, mit der der Name des zu erstellenden Anwendungsdiensts definiert wird. Hier verwenden wir `marathon-alice` als Name für das Framework. Speichern Sie die Datei unter einem Namen wie `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Verwenden Sie als Nächstes die DC/OS-CLI, um die Marathon-Instanz mit den Optionen zu installieren, die in Ihrer Konfigurationsdatei festgelegt sind:

```bash
dcos package install --options=marathon-alice.json marathon
```

Sie sollten jetzt auf der Registerkarte „Dienste“ der DC/OS-Benutzeroberfläche sehen, dass Ihr `marathon-alice` -Dienst ausgeführt wird. Auf die Benutzeroberfläche können Sie unter `http://<hostname>/service/marathon-alice/` direkt zugreifen.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Einrichten der DC/OS-CLI für den Zugriff auf den Dienst
Sie können Ihre DC/OS-CLI optional konfigurieren, um auf diesen neuen Dienst zuzugreifen, indem Sie die `marathon.url`-Eigenschaft wie folgt festlegen, damit sie auf die `marathon-alice`-Instanz zeigt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Sie können mit dem Befehl `dcos config show` überprüfen, für welche Instanz von Marathon die CLI verwendet wird. Sie können den Befehl `dcos config unset marathon.url`nutzen, um wieder zur Masterinstanz des Marathon-Diensts zurückzuwechseln.




<!--HONumber=Nov16_HO2-->


