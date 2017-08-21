---
title: "Skriptbeispiel von Azure PowerShell – Hinzufügen eines Anwendungszertifikats zu einem Cluster | Microsoft-Dokumentation"
description: "Skriptbeispiel von Azure PowerShell – Hinzufügen eines Anwendungszertifikats zu einem Cluster"
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 3212944a8bd882ced86e841944ac60bd1845e3d1
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Fügen Sie ein Anwendungszertifikat zu einem Service Fabric-Cluster hinzu

In diesem Beispielskript wird ein selbst signiertes Zertifikat im angegebenen Azure Key Vault erstellt, und es wird auf allen Knoten des Service Fabric-Clusters installiert. Das Zertifikat wird außerdem in einen lokalen Ordner heruntergeladen. Der Name des heruntergeladenen Zertifikats ist identisch mit dem Namen des Zertifikats im Schlüsseltresor. Passen Sie die Parameter nach Bedarf an.

Installieren Sie bei Bedarf Azure PowerShell mithilfe der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Fügen Sie ein Anwendungszertifikat zu einem Cluster hinzu")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Add-AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Fügen Sie den VM-Skalierungsgruppen, die den Cluster bilden, ein neues Anwendungszertifikat hinzu.  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

