---
title: Features von Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation
description: "Übersicht über die Features von Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: 
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: juluk
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: e789283ab9b25f634c50b341ca882bbf9c70a225
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="features--tools-for-azure-cloud-shell"></a>Features und Tools für Azure Cloud Shell
Azure Cloud Shell ist eine browserbasierte Shell zum Verwalten und Entwickeln von Azure-Ressourcen.

Cloud Shell bietet eine vorkonfigurierte Shell, auf die mit Browsern zugegriffen werden kann, für die Verwaltung von Azure-Ressourcen dadurch entfällt für Sie die Installation, Versionsverwaltung und Wartung eines Computers.

Cloud Shell stellt Computer auf Anforderungsbasis bereit, und daher wird der Status des Computers nicht sitzungsübergreifend beibehalten. Da Cloud Shell für interaktive Sitzungen vorgesehen ist, werden Shells automatisch nach 10 Minuten Shellinaktivität beendet.

## <a name="tools"></a>Tools
|Kategorie   |Name   |
|---|---|
|Linux-Shellinterpreter|Bash<br> sh               |
|Azure-Tools            |Azure CLI 2.0 und 1.0     |
|Text-Editoren           |Vim<br> Nano<br> Emacs       |
|Quellcodeverwaltung         |Git                    |
|Buildtools            |Make<br> Maven<br> npm<br> pip         |
|Container             |Docker<br> Kubectl<br> DC/OS-CLI         |
|Datenbanken              |MySQL-Client<br> PostgreSQL-Client<br> SQLCMD-Hilfsprogramm      |
|Sonstige                  |iPython-Client |

## <a name="language-support"></a>Sprachunterstützung
|Sprache   |Version   |
|---|---|
|.NET       |1.01       |
|Go         |1.7        |
|Node.js    |6.9.4      |
|Python     |2.7 und 3.5|

## <a name="secure-automatic-authentication"></a>Sichern der automatischen Authentifizierung
Cloud Shell authentifiziert Zugriff auf Konten für Azure CLI 2.0 sicher und automatisch.

## <a name="azure-files-persistence"></a>Azure Files-Persistenz
Da Cloud Shell auf Anforderungsbasis über einen temporären Computer zugewiesen wird, werden lokale Dateien außerhalb von „$Home“ und der Computerstatus nicht sitzungsübergreifend beibehalten.
Damit Sie Dateien sitzungsübergreifend beibehalten können, wird Ihnen beim ersten Start von Cloud Shell das Anfügen einer Azure-Dateifreigabe gezeigt.
Anschließend fügt Cloud Shell Ihren Speicher automatisch für alle zukünftigen Sitzungen an.

[Erfahren Sie mehr über das Anfügen von Azure-Dateifreigaben in Cloud Shell.](persisting-shell-storage.md)

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md) 
[Erfahren Sie mehr über Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)
