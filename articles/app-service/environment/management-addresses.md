---
title: Azure App Service-Umgebung Management-Adressen
description: Listet die Management-Adressen auf, die verwendet werden, um eine App Service-Umgebung zu befehligen
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: 949b445793768f593af205321bf3ab5a7aa150c0
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="app-service-environment-management-addresses"></a>App Service-Umgebung Management-Adressen

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet).  Die ASE muss vom Azure App Service aus zugänglich sein, sodass sie verwaltet werden kann.  Dieser ASE-Verwaltungsdatenverkehr durchläuft das benutzergesteuerte Netzwerk.  Er geht von Azure App Service-Verwaltungsservern auf die öffentliche VIP-Adresse, die der ASE zugeordnet ist.  Ausführliche Informationen über die ASE Netzwerk-Abhängigkeiten finden Sie unter [Networking considerations and the App Service Environment (Networking-Überlegungen und die App Service-Umgebung)][networking].  Allgemeine Informationen zu den ASE finden Sie unter [Einführung in die App Service-Umgebung][intro].

Dieses Dokument listet die Quell-IPs für den Verwaltungsdatenverkehr zur ASE auf. Diese Adressen können zum Erstellen von Netzwerksicherheitsgruppen zum Sperren des eingehenden Datenverkehrs oder nach Bedarf in Routingtabellen verwendet werden.  Um diese Informationen zu nutzen, verwenden Sie:

* die IP-Adressen, die für alle Regionen aufgeführt sind
* die IP-Adressen, die der Region entsprechen, die in Ihrem ASE bereitgestellt wird.

Der eingehende Datenverkehr stammt von diesen IP-Adressen mit den Ports 454 und 455.

| Region | Adressen |
|--------|-----------|
| Alle Regionen | 70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141 |
| USA, Süden-Mitte & USA, Norden-Mitte | 23.102.188.65, 191.236.154.88 |
| Australien, Südosten & Australien, Osten | 23.101.234.41, 104.210.90.65 |
| USA, Westen, USA, Osten | 104.45.227.37, 191.236.60.72 |
| Europa, Westen & Europa, Norden | 191.233.94.45, 191.237.222.191 |
| USA Westen-Mitte & USA, Westen 2 | 13.78.148.75, 13.66.225.188 |
| USA Mitte & USA, Osten 2 | 104.43.165.73, 104.46.108.135 |
| Asien, Osten & Asien, Südosten | 23.99.115.5, 104.215.158.33 |
| Japan, Osten & Japan, Westen | 104.41.185.116, 191.239.104.48 |
| Kanada, Mitte & Kanada, Osten | 40.85.230.101, 40.86.229.100 |
| Vereinigtes Königreich, Westen & Vereinigtes Königreich, Süden | 51.141.8.34, 51.140.185.75 |
| Korea, Süden & Korea, Mitte | 52.231.200.177, 52.231.32.117 |
| Brasilien Süden & USA, Süden-Mitte| 104.41.46.178, 23.102.188.65 |
| Indien Mitte & Indien, Süden | 104.211.98.24, 104.211.225.66 |
| Indien, Westen & Indien, Süden | 104.211.160.229, 104.211.225.66 |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md

