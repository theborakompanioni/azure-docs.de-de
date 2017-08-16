---
title: "Übersicht über Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Übersicht über Azure Cloud Shell."
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
ms.date: 07/10/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 7165633cd354eeea2e3619f839338e6af1524e56
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="overview-of-azure-cloud-shell-preview"></a>Übersicht über Azure Cloud Shell (Vorschau)
Azure Cloud Shell ist eine interaktive, über den Browser zugängliche Shell für die Verwaltung von Azure-Ressourcen.

![](media/overview-pic.png)

## <a name="features"></a>Features
### <a name="browser-based-shell-experience"></a>Browserbasierte Shelloberfläche
Cloud Shell ermöglicht den Zugriff auf eine browserbasierte Befehlszeilenumgebung, die extra auf Azure-Verwaltungsaufgaben ausgerichtet ist. Nutzen Sie Cloud Shell, um unabhängig von einem lokalen Computer in einer Weise zu arbeiten, die Ihnen nur die Cloud bieten kann.

### <a name="pre-configured-azure-workstation"></a>Vorkonfigurierte Azure-Arbeitsstation
In Cloud Shell sind gängige Befehlszeilentools und Sprachunterstützung vorinstalliert, damit Sie schneller arbeiten können.

[Hier können Sie die vollständige Toolliste für Azure Cloud Shell anzeigen.](features.md#tools)

### <a name="automatic-authentication"></a>Automatische Authentifizierung
Cloud Shell führt in jeder Sitzung eine sichere Authentifizierung durch, sodass Sie über die Azure CLI 2.0 sofort auf Ihre Ressourcen zugreifen können.

### <a name="connect-your-azure-file-storage"></a>Herstellen einer Verbindung mit Ihrem Azure-Dateispeicher
Cloud Shell-Computer sind temporär und erfordern folglich das Einbinden einer Azure-Dateifreigabe als `clouddrive`, um Ihr $Home-Verzeichnis dauerhaft bereitzustellen.
Beim ersten Start werden Sie von Cloud Shell darauf hingewiesen, dass eine Ressourcengruppe, ein Speicherkonto und eine Dateifreigabe in Ihrem Namen erstellt werden. Dieser Schritt ist nur einmal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. 

#### <a name="create-new-storage"></a>Erstellen von neuem Speicher
![](media/basic-storage.png)

Ein lokal redundantes Speicherkonto (Locally Redundant Storage, LRS) kann mit einer Azure-Dateifreigabe erstellt werden, die standardmäßig ein 5-GB-Datenträgerimage umfasst. Die Dateifreigabe wird zum Zweck der Interaktion mit dem Datenträgerimage, das zum Synchronisieren und dauerhaften Bereitstellen Ihres $Home-Verzeichnisses verwendet wird, als `clouddrive` eingebunden. Es gelten die üblichen Speicherkosten.

Drei Ressourcen werden in Ihrem Namen erstellt:
1. Eine Ressourcengruppe namens `cloud-shell-storage-<region>`
2. Ein Speicherkonto namens `cs<uniqueGuid>`
3. Eine Dateifreigabe namens `cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Alle Dateien in Ihrem $Home-Verzeichnis, wie etwa SSH-Schlüssel, werden beständig in Ihrem Datenträgerimage gespeichert, das in der eingebundenen Dateitreigabe gespeichert ist. Wenden Sie beim Speichern von Dateien in Ihrem $Home-Verzeichnis und der eingebundenen Dateifreigabe bewährte Methoden an.

#### <a name="use-existing-resources"></a>Verwenden vorhandener Ressourcen
![](media/advanced-storage.png)

Es wird außerdem eine erweiterte Option bereitgestellt: Sie können vorhandene Ressourcen mit der Cloud Shell verbinden. Klicken Sie auf „Erweiterte Einstellungen anzeigen“, wenn die Aufforderung zur Speichereinrichtung angezeigt wird, um zusätzliche Optionen auszuwählen. Dropdownlisten werden für Ihre zugewiesene Cloud Shell-Region und die lokal/global redundanten Speicherkonten gefiltert.

[Weitere Informationen zu Cloud Shell-Speicher, dem Aktualisieren von Dateifreigaben und dem Upload/Download von Dateien.] (persisting-shell-storage.md)

## <a name="concepts"></a>Konzepte
* Cloud Shell wird auf einem temporären Computer ausgeführt, der pro Sitzung und pro Benutzer bereitgestellt wird.
* Nach 20 Minuten ohne interaktive Aktivitäten tritt für Cloud Shell ein Timeout ein.
* Der Zugriff auf Cloud Shell ist nur möglich, wenn eine Dateifreigabe angefügt ist.
* Cloud Shell wird ein Computer pro Benutzerkonto zugewiesen.
* Berechtigungen werden auf einen normalen Linux-Benutzer festgelegt.

[Weitere Informationen zu allen Cloud Shell-Features.](features.md)

## <a name="examples"></a>Beispiele
* Erstellen oder Bearbeiten von Skripts zum Automatisieren der Azure-Verwaltung
* Gleichzeitiges Verwalten von Ressourcen über das Azure-Portal und die Azure CLI 2.0
* Testen der Azure CLI 2.0

[Probieren Sie all diese Beispiele beim Cloud Shell-Schnellstart aus.](quickstart.md)

## <a name="pricing"></a>Preise
Der Computer, auf dem Cloud Shell gehostet wird, ist kostenlos. Als Voraussetzung muss eine Azure-Dateifreigabe eingebunden sein, um Ihr $Home-Verzeichnis dauerhaft zu speichern. Es gelten die üblichen Speicherkosten.

## <a name="supported-browsers"></a>Unterstützte Browser
Cloud Shell wird für Chrome, Edge und Safari empfohlen. Cloud Shell wird zwar für Chrome, Firefox, Safari, Internet Explorer und Edge unterstützt, unterliegt jedoch bestimmten Browsereinstellungen.

## <a name="troubleshooting"></a>Problembehandlung
1. Mit einem Azure Active Directory-Abonnement kann ich aufgrund des Fehlers „Error: 400 DisallowedOperation“ (Fehler: 400 Unzulässiger Vorgang) keinen Speicher erstellen. Um dieses Problem zu beheben, verwenden Sie ein Azure-Abonnement, das Speicherressourcen erstellen kann. AD-Abonnements sind nicht in der Lage, Azure-Ressourcen zu erstellen.

Informationen zu bestimmten bekannten Einschränkungen finden Sie unter [Einschränkungen von Cloud Shell](limitations.md).

