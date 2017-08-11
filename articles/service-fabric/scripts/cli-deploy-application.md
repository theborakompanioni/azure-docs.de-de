---
title: "Azure CLI-Skriptbeispiel – Bereitstellen einer Anwendung in einem Cluster"
description: "Azure CLI-Skriptbeispiel – Bereitstellen einer Anwendung in einem Service Fabric-Cluster."
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
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: de6ec4378a05656ecefefa7d5994a4f5dad404ba
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Bereitstellen einer Anwendung in einem Service Fabric-Cluster

Dieses Beispielskript kopiert ein Anwendungspaket in einen Clusterimagespeicher, registriert den Anwendungstyp im Cluster und erstellt eine Anwendungsinstanz aus dem Anwendungstyp.  Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese Dienste in diesem Schritt erstellt.

Installieren Sie ggf. die [Azure-Befehlszeilenschnittstelle](../service-fabric-azure-cli-2-0.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Bereitstellen einer Anwendung in einem Cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nachdem das Beispielskript ausgeführt wurde, kann das Skript unter [Entfernen einer Anwendung](cli-remove-application.md) dazu verwendet werden, die Anwendungsinstanz zu entfernen, die Registrierung des Anwendungstyps aufzuheben und das Anwendungspaket aus dem Imagespeicher zu löschen.

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.


| Befehl | Hinweise |
|---|---|
| [sf cluster select](/cli/azure/sf/cluster#select) | Wählt den Cluster aus, mit dem gearbeitet werden soll. |
| [sf application upload](/cli/azure/sf/application#upload) | Lädt die Anwendungsdateien und -manifeste hoch. |
| [sf application provision](/cli/azure/sf/application#provision) | Registriert die Anwendung beim Cluster.|
| [sf application create](/cli/azure/sf/application#create) | Erstellt eine Instanz der Anwendung und stellt alle definierten Dienste für die Knoten bereit. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](../service-fabric-azure-cli-2-0.md).

Zusätzliche Azure CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure CLI-Beispiele](../samples-cli.md).

