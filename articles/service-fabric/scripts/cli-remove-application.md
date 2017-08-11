---
title: "Azure CLI-Skriptbeispiel – Entfernen einer Anwendung aus einem Cluster"
description: "Azure CLI-Skriptbeispiel – Entfernen einer Anwendung aus einem Service Fabric-Cluster."
services: service-fabric
documentationcenter: 
author: thraka
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
ms.openlocfilehash: 77fa78199854d10c4897d56da12c8e359ae491f3
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz und hebt die Registrierung für einen Anwendungstyp und eine Version im Cluster auf.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Als Nächstes werden die Anwendungsdateien aus dem Imagespeicher gelöscht. 

Installieren Sie ggf. die [Azure-Befehlszeilenschnittstelle](../service-fabric-azure-cli-2-0.md).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Entfernen einer Anwendung aus einem Cluster")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [sf cluster select](/cli/azure/sf/cluster#select) | Wählt den Cluster aus, mit dem gearbeitet werden soll. |
| [sf application delete](/cli/azure/sf/application#delete) | Löscht die Anwendungsinstanz aus dem Cluster. |
| [sf application unprovision](/cli/azure/sf/application#unprovision) | Hebt die Registrierung eines Service Fabric-Anwendungstyps und einer Version aus dem Cluster auf|
| [sf application package-delete](/cli/azure/sf/application#package-delete) | Entfernt ein Service Fabric-Anwendungspakets aus dem Imagespeicher |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Dokumentation der Azure-Befehlszeilenschnittstelle](../service-fabric-azure-cli-2-0.md).

Zusätzliche Azure CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure CLI-Beispiele](../samples-cli.md).

