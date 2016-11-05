---
title: Erste Schritte mit Azure Active Directory und verbundenen Visual Studio-Diensten (WebApi-Projekte) | Microsoft Docs
description: Erfahren Sie etwas über die ersten Schritte mit Azure Active Directory in WebApi-Projekten nach dem Herstellen einer Verbindung oder dem Erstellen eines Azure AD mithilfe von verbundenen Visual Studio-Diensten.
services: active-directory
documentationcenter: ''
author: TomArcher
manager: douge
editor: ''

ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: tarcher

---
# Erste Schritte mit Azure Active Directory und verbundenen Visual Studio-Diensten (WebApi-Projekte)
> [!div class="op_single_selector"]
> * [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> * [Was ist passiert?](vs-active-directory-webapi-what-happened.md)
> 
> 

## Erfordern von Authentifizierung für den Zugriff auf Controller
Alle Controller in Ihrem Projekt wurden mit dem Attribut **Authorize** versehen. Dieses Attribut erfordert, dass der Benutzer vor dem Zugriff auf die durch diese Controller definierten APIs authentifiziert werden muss. Wenn Sie anonymen Zugriff auf diesen Controller erlauben möchten, entfernen Sie dieses Attribut vom Controller. Wenn Sie die Berechtigungen präziser festlegen möchten, wenden Sie das Attribut auf jede Methode an, die Autorisierung erfordert, anstatt es auf die Controllerklasse anzuwenden.

[Weitere Informationen zu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

<!---HONumber=AcomDC_0817_2016-->