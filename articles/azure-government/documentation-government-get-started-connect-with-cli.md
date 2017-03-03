---
title: "Verbinden mit Azure Government über die Azure-Befehlszeilenschnittstelle | Microsoft Docs"
description: "Informationen zum Verwalten Ihres Abonnements in Azure Government durch Herstellen einer Verbindung über die Azure-Befehlszeilenschnittstelle"
services: azure-government
cloud: gov
documentationcenter: 
author: zakramer
manager: liki
ms.assetid: c7cbe993-375e-4aed-9aa5-2f4b2111f71c
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: zakramer
translationtype: Human Translation
ms.sourcegitcommit: 6b29e98aa3458a76e186234c2e9800294902bea9
ms.openlocfilehash: 6e070870a93d2636f076b339dd401358735a9bd0
ms.lasthandoff: 02/21/2017


---


# <a name="connect-to-azure-government-with-azure-cli"></a>Herstellen einer Verbindung mit Azure Government über die Azure-Befehlszeilenschnittstelle

Zur Verwendung der Azure-Befehlszeilenschnittstelle müssen Sie anstelle einer Verbindung mit dem öffentlichen Azure eine Verbindung mit Azure Government herstellen. Die Azure-Befehlszeilenschnittstelle kann zum Verwalten eines umfangreichen Abonnements über Skripts oder für den Zugriff auf Features verwendet werden, die derzeit im Azure-Portal nicht verfügbar sind. Wenn Sie die Azure-Befehlszeilenschnittstelle bereits im öffentlichen Azure verwendet haben, ist dies größtenteils identisch.  In Azure Government bestehen folgende Unterschiede:

Es gibt mehrere Möglichkeiten zum [Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/azure/xplat-cli-install). Wenn Node.js bereits installiert ist, ist die Installation des npm-Pakets die einfachste Möglichkeit:

Um die CLI aus einem npm-Paket zu installieren, müssen Sie die aktuellen Versionen von [Node.js und npm](https://nodejs.org/en/download/package-manager/) herunterladen und installieren. Führen Sie anschließend zum Installieren des Azure-CLI-Pakets **npm install** aus:

```bash
npm install -g azure-cli
```

Bei Linux-Distributionen müssen Sie möglicherweise **sudo** wie folgt verwenden, um den Befehl **npm** erfolgreich ausführen zu können:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Falls Sie Node.js und npm für Ihre Linux-Distribution oder Ihr Betriebssystem installieren oder aktualisieren müssen, wird die Installation der aktuellsten LTS-Version von Node.js (4.x) empfohlen. Wenn Sie eine ältere Version verwenden, kommt es möglicherweise zu Fehlern bei der Installation.


Nachdem Sie die Azure-Befehlszeilenschnittstelle installiert haben, müssen Sie sich bei Azure Government anmelden:

```
azure login --username your-user-name@your-gov-tenant.onmicrosoft.com  --environment AzureUSGovernment
```

Nachdem Sie angemeldet sind, können Sie Befehle der Azure-Befehlszeilenschnittstelle wie gewohnt ausführen:

```
azure webapp list my-resource-group
```
