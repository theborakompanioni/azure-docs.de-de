---
title: Erstellen einer Funktionen-App im Azure-Portal | Microsoft Docs
description: "Erstellen Sie eine neue Funktionen-App in Azure App Service über das Portal."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fb8eaa4bb122cd6bb7e3f72bb5f7000103bffb29
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-function-app-from-the-azure-portal"></a>Erstellen einer Funktionen-App im Azure-Portal

Für Azure-Funktionen-Apps wird die Infrastruktur von Azure App Service verwendet. In diesem Thema wird erläutert, wie Sie eine Funktionen-App im Azure-Portal erstellen. Bei einer Funktionen-App handelt es sich um den Container, der die Ausführung einzelner Funktionen hostet. Wenn Sie eine Funktionen-App im App Service-Hostingplan erstellen, kann die Funktionen-App sämtliche Funktionen von App Service verwenden.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Geben Sie beim Erstellen einer Funktionen-App einen gültigen **App-Namen** an. Dieser darf nur Buchstaben, Zahlen und Bindestriche enthalten. Unterstriche (**_**) sind nicht zulässig. 

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name Ihres Speicherkontos muss innerhalb von Azure eindeutig sein. 

Nach der Erstellung der Funktionen-App können Sie einzelne Funktionen in einer oder mehreren Sprachen erstellen. Die Funktionen können Sie [über das Portal](functions-create-first-azure-function.md#create-function), über [Continuous Deployment](functions-continuous-deployment.md) oder durch [Hochladen mit FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) erstellen.  

## <a name="service-plans"></a>Servicepläne

Azure Functions bietet zwei verschiedene Servicepläne: Verbrauchsplan und App Service-Plan. Der Verbrauchsplan weist automatisch Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf horizontal hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder horizontal herunterskaliert. Mit dem App Service-Plan hat die Funktionen-App Zugriff auf alle Funktionen von App Service. Sie müssen Ihren Serviceplan bei der Erstellung der Funktionen-App auswählen. Derzeit kann er dann nicht mehr geändert werden. Weitere Informationen finden Sie unter [Auswählen eines Azure Functions-Hostingplans](functions-scale.md).

Wenn Sie beabsichtigen, JavaScript-Funktionen für einen App Service-Plan zu verwenden, sollten Sie einen Plan mit einer kleineren Anzahl von Kernen auswählen. Weitere Informationen finden Sie in der [JavaScript-Referenz für Funktionen](functions-reference-node.md#choose-single-core-app-service-plans). 

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Beim Erstellen einer Funktionen-App in App Service müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob Storage, Queue Storage und Table Storage unterstützt. Intern wird Storage in Functions für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen verwendet. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten, Azure Storage Premium und allgemeine Speicherkonten mit Replikation von ZRS. Diese Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine Funktionen-App erstellt wird.

>[!NOTE]
>Wenn der verbrauchsbasierte Hostingplan verwendet wird, werden die Funktionscode- und Bindungskonfigurationsdateien in Azure File Storage im Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

Weitere Informationen zu Typen von Speicherkonten finden Sie unter [Einführung in die Azure Storage-Dienste] (../storage/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]




