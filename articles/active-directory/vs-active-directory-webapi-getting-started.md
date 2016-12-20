---
title: Erste Schritte mit Azure Active Directory und verbundenen Visual Studio-Diensten (WebApi-Projekte) | Microsoft Docs
description: "Erfahren Sie etwas über die ersten Schritte mit Azure Active Directory in WebApi-Projekten nach dem Herstellen einer Verbindung oder dem Erstellen eines Azure AD mithilfe von verbundenen Visual Studio-Diensten."
services: active-directory
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 05a507e328bb79e976ebccfdfae0dc2d7c82f374


---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Erste Schritte mit Azure Active Directory und verbundenen Visual Studio-Diensten (WebApi-Projekte)
> [!div class="op_single_selector"]
> * [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> * [Was ist passiert?](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Erfordern von Authentifizierung für den Zugriff auf Controller
Alle Controller in Ihrem Projekt wurden mit dem Attribut **Authorize** versehen. Dieses Attribut erfordert, dass der Benutzer vor dem Zugriff auf die durch diese Controller definierten APIs authentifiziert werden muss. Wenn Sie anonymen Zugriff auf diesen Controller erlauben möchten, entfernen Sie dieses Attribut vom Controller. Wenn Sie die Berechtigungen präziser festlegen möchten, wenden Sie das Attribut auf jede Methode an, die Autorisierung erfordert, anstatt es auf die Controllerklasse anzuwenden.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)




<!--HONumber=Nov16_HO3-->


