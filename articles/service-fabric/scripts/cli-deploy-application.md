---
title: "Azure Service Fabric-CLI-Skript – Bereitstellungsbeispiel"
description: Bereitstellen einer Anwendung in einem Azure Service Fabric-Cluster mithilfe der Azure Service Fabric-CLI
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 74d54b314e81dc260e800e32d6c3edd5215c82b4
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Bereitstellen einer Anwendung in einem Service Fabric-Cluster

Dieses Beispielskript kopiert ein Anwendungspaket in einen Clusterimagespeicher, registriert den Anwendungstyp im Cluster und erstellt eine Anwendungsinstanz aus dem Anwendungstyp. Alle Standarddienste werden ebenfalls zu diesem Zeitpunkt erstellt.

Installieren Sie ggf. die [Service Fabric-CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Bereitstellen einer Anwendung in einem Cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Abschluss kann die Anwendung mit dem [Entfernungsskript](cli-remove-application.md) entfernt werden. Das Entfernungsskript löscht die Anwendungsinstanz, hebt die Registrierung des Anwendungstyps auf und löscht das Anwendungspaket aus dem Imagespeicher.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Service Fabric-Befehlszeilenschnittstelle](../service-fabric-cli.md).

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).

