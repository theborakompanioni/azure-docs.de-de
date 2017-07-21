---
title: "Azure AD v2 – Windows-Desktop: Erste Schritte – Konfiguration | Microsoft-Dokumentation"
description: "Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert. | Microsoft Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1dfaa7ade664e43dcb9aa788b0197ca17e6ec4cc
ms.contentlocale: de-de


---

## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4. Klicken Sie auf `Add Platform`, wählen Sie `Native Application` aus, und klicken Sie auf „Speichern“.
5. Kopieren Sie die GUID in „Anwendungs-ID“, wechseln Sie zurück zu Visual Studio, öffnen Sie `App.xaml.cs`, und ersetzen Sie `your_client_id_here` durch die Anwendungs-ID, die Sie gerade registriert haben:

```csharp
private static string ClientId = "your_application_id_here";
```

