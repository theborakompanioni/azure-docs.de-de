---
title: Features von Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation
description: "Übersicht über die Features von Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 3414957a67d67603fdb597b3715a902e8cc3c5bd
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---

# <a name="features-and-tools-for-azure-cloud-shell"></a>Features und Tools für Azure Cloud Shell
Azure Cloud Shell ist eine browserbasierte Shell zum Verwalten und Entwickeln von Azure-Ressourcen.

Cloud Shell bietet eine vorkonfigurierte Shell, auf die mit Browsern zugegriffen werden kann, für die Verwaltung von Azure-Ressourcen dadurch entfällt für Sie die Installation, Versionsverwaltung und Wartung eines Computers.

Cloud Shell stellt Computer auf Anforderungsbasis bereit, und daher wird der Status des Computers nicht sitzungsübergreifend beibehalten. Da Cloud Shell für interaktive Sitzungen vorgesehen ist, werden Shells automatisch nach 20 Minuten Shellinaktivität beendet.

## <a name="bash-in-cloud-shell"></a>Bash in Cloud Shell
### <a name="tools"></a>Tools
|Kategorie   |Name   |
|---|---|
|Linux-Shellinterpreter|Bash<br> sh               |
|Azure-Tools            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) und [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)     |
|Text-Editoren           |Vim<br> Nano<br> Emacs       |
|Quellcodeverwaltung         |Git                    |
|Buildtools            |Make<br> Maven<br> npm<br> pip         |
|Container             |[Docker-CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Draft](https://github.com/Azure/draft)<br> [DC/OS-CLI](https://github.com/dcos/dcos-cli)         |
|Datenbanken              |MySQL-Client<br> PostgreSQL-Client<br> [SQLCMD-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility)      |
|Sonstige                  |iPython-Client<br> [Cloud Foundry-CLI](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a>Sprachunterstützung
|Sprache   |Version   |
|---|---|
|.NET       |1.01       |
|Go         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|Python     |2.7 und 3.5 (Standard)|

## <a name="secure-automatic-authentication"></a>Sichern der automatischen Authentifizierung
Cloud Shell authentifiziert Zugriff auf Konten für Azure CLI 2.0 sicher und automatisch.

## <a name="azure-files-persistence"></a>Azure Files-Persistenz
Da Cloud Shell auf Anforderungsbasis über einen temporären Computer zugewiesen wird, werden Dateien außerhalb von „$Home“ und der Computerstatus nicht sitzungsübergreifend beibehalten.
Damit Sie Dateien sitzungsübergreifend beibehalten können, wird Ihnen beim ersten Start von Cloud Shell das Anfügen einer Azure-Dateifreigabe gezeigt.
Anschließend fügt Cloud Shell Ihren Speicher automatisch für alle zukünftigen Sitzungen an.

[Erfahren Sie mehr über das Anfügen von Azure-Dateifreigaben in Cloud Shell.](persisting-shell-storage.md)

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md) <br>
[Informationen zu Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>
