---
title: "Schnellstart für die Azure Stack Development Kit-Bereitstellung | Microsoft-Dokumentation"
description: Informationen zum Bereitstellen des Azure Stack Development Kits
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: erikje
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 81b6282addd1e88e4146367c4dd9a2ee7b8c84bf
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="azure-stack-development-kit-deployment-quickstart"></a>Schnellstart für die Azure Stack Development Kit-Bereitstellung

Das [Azure Stack Development Kit](azure-stack-poc.md) ist eine Test- und Entwicklungsumgebung, die sie zum Evaluieren und Demonstrieren von Azure Stack-Funktionen und -Diensten bereitstellen können. Um sie einsatzbereit zu machen, müssen Sie die Umgebungshardware vorbereiten und einige Skripts ausführen (dies nimmt mehrere Stunden in Anspruch). Anschließend können Sie sich bei den Administrator- und Mandantenportalen anmelden, um Azure Stack zu verwalten und Angebote zu testen. 

1. [**Planen Sie die Hardware, Software und das Netzwerk**](azure-stack-deploy.md). Der Computer, der das Development Kit hostet (der Development Kit-Host) muss die Hardware-, Software- und Netzwerkanforderungen erfüllen. Sie müssen auch zwischen der Verwendung von Azure Active Directory oder Active Directory-Verbunddienste (AD FS) wählen. Achten Sie darauf, diese Voraussetzungen vor dem Beginn der Bereitstellung zu erfüllen, damit der Installationsvorgang reibungslos ausgeführt wird. 

2. [**Laden Sie das Bereitstellungspaket herunter und extrahieren Sie es**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Sie können das Bereitstellungspaket auf den Development Kit-Host oder einen anderen Computer herunterladen. Die extrahierten Bereitstellungsdateien belegen bis zu 60 GB freien Speicherplatz auf dem Datenträger, durch die Verwendung eines anderen Computers lassen sich also die Hardwareanforderungen an den Development Kit-Host reduzieren.

3. [**Bereiten Sie den Development Kit Host**](azure-stack-run-powershell-script.md#prepare-the-development-kit-host) mithilfe des Installationsprogramms vor. Nach diesem Schritt startet der Development Kit-Host „Cloudbuilder.vhdx“ (eine virtuelle Festplatte mit startbarem Betriebssystem und den Azure Stack-Installationsdateien).

4. [**Stellen Sie das Development Kit**](azure-stack-run-powershell-script.md#deploy-the-development-kit) auf dem Development Kit-Host bereit.

5. Wenn Azure Active Directory für Ihre Azure Stack-Bereitstellung verwendet wird, müssen Sie [Azure Stack bei Azure registrieren](azure-stack-register.md), damit Sie [Azure Marketplace-Elemente](azure-stack-download-azure-marketplace-item.md) in Azure Stack herunterladen können.

Nach Abschluss dieser Schritte verfügen Sie über eine Development Kit-Umgebung mit Administrator- und Mandantenportalen. Als Nächstes können Sie [eine Verbindung mit dem Portal herstellen und sich anmelden](azure-stack-connect-azure-stack.md). Dann können Sie mit der Bereitstellung von Ressourcenanbietern, dem Erstellen von [Angeboten](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions) und dem Auffüllen von Azure Stack [Marketplace](azure-stack-marketplace.md) beginnen.

