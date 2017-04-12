---
title: "Azure CLI-Beispiele – App Service | Microsoft-Dokumentation"
description: "Azure CLI-Beispiele – App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5d74f8216edd46fcd36d2b7f8763ec8a69ccad42
ms.lasthandoff: 03/31/2017


---
# <a name="azure-cli-samples"></a>Azure CLI-Beispiele

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| | |
|-|-|
|**Erstellen einer App**||
| [Erstellen einer Web-App und Bereitstellen von Code über GitHub](./scripts/app-service-cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App und stellt Code aus einem öffentlichen GitHub-Repository bereit. |
| [Erstellen einer Web-App mit Continuous Deployment über GitHub](./scripts/app-service-cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App mit kontinuierlicher Veröffentlichung von einem GitHub-Repository in Ihrem Besitz. |
| [Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App und konfiguriert einen Codepush aus einem lokalen Git-Repository. |
| [Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung](./scripts/app-service-cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App mit einem Bereitstellungsslot für Änderungen am Stagingcode. |
| [Erstellen einer ASP.NET Core-Web-App in einem Docker-Container](./scripts/app-service-cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App unter Linux und lädt ein Docker-Image aus Docker Hub. |
|**Konfigurieren der App**||
| [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](./scripts/app-service-cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App und ordnet ihr einen benutzerdefinierten Domänennamen zu. |
| [Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App](./scripts/app-service-cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App und bindet das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. |
|**Skalieren der App**||
| [Manuelles Skalieren einer Web-App](./scripts/app-service-cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App und skaliert sie über zwei Instanzen. |
| [Weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur](./scripts/app-service-cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt zwei Azure-Web-Apps in zwei verschiedenen geografischen Regionen und macht sie mithilfe von Azure Traffic Manager über einen einzigen Endpunkt verfügbar. |
|**Verbinden der App mit Ressourcen**||
| [Verbinden einer Web-App mit einer SQL-­Datenbank](./scripts/app-service-cli-app-service-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App und eine SQL-Datenbank und fügt dann die Datenbank-Verbindungszeichenfolge zu den App-Einstellungen hinzu. |
| [Verbinden einer Web-App mit einem Speicherkonto](./scripts/app-service-cli-app-service-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellt eine Azure-Web-App und ein Speicherkonto und fügt dann die Speicherverbindungszeichenfolge zu den App-Einstellungen hinzu. |
| [Herstellen einer Verbindung einer Web-App mit einem Redis-Cache](./scripts/app-service-cli-app-service-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App und einen Redis-Cache und fügt dann die Redis-Verbindungsdetails zu den App-Einstellungen hinzu. |
| [Herstellen einer Verbindung einer Web-App mit einer DocumentDB-Instanz](./scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App und eine DocumentDB-Instanz und fügt dann die DocumentDB-Verbindungsdetails zu den App-Einstellungen hinzu. |
|**Überwachen der App**||
| [Überwachen einer Web-App mit Webserverprotokollen](./scripts/app-service-cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Azure-Web-App, aktiviert die Protokollierung für die App und lädt die Protokolle auf Ihren lokalen Computer herunter. |
| | |
