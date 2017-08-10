---
title: "Azure Active Directory B2C: Konfiguration für Twitter | Microsoft-Dokumentation"
description: "Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in mit Azure Active Directory B2C geschützten Anwendungen."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 82a001dd53cdddcf3b360090f3250af593c96fbb
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a>Azure Active Directory B2C: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

## <a name="create-a-twitter-application"></a>Erstellen einer Twitter-Anwendung
Um Twitter als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Twitter-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen dazu ein Twitter-Entwicklerkonto. Wenn Sie über kein Konto verfügen, können Sie unter [https://dev.twitter.com/](https://dev.twitter.com/) ein Konto erstellen.

1. Wechseln Sie zur [Website für Twitter-Entwickler](https://dev.twitter.com/), und melden Sie sich mit Ihren Anmeldeinformationen an.
2. Klicken Sie unter **Tools & Support** auf **My Apps** und dann auf **Create New App**. 
3. Geben Sie im Formular einen Wert für **Name**, **Description** und **Website** ein.
4. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in **Callback URL** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. contosob2c.onmicrosoft.com).
5. Aktivieren Sie das Kontrollkästchen, um **Developer Agreement** zuzustimmen, und klicken Sie auf **Create your Twitter application**.
6. Nachdem die App erstellt wurde, klicken Sie auf die Registerkarte **Keys and Access Tokens**.
7. Kopieren Sie den Wert von **Consumer Key** und **Consumer Secret**. Sie benötigen beide Angaben, um Twitter als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten
1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) .
2. Klicken Sie auf dem B2C-Featureblatt auf **Identitätsanbieter**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Geben Sie als **Name** einen aussagekräftigen Namen für die Konfiguration des Identitätsanbieters ein. Geben Sie z.B. „Twitter“ ein.
5. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **Twitter** aus, und klicken Sie auf **OK**.
6. Klicken Sie auf **Diesen Identitätsanbieter einrichten**, und geben Sie den **Consumer Key** von Twitter als **Client-ID** und das **Consumer Secret** von Twitter als **Clientschlüssel** ein.
7. Klicken Sie auf **OK** und dann auf **Erstellen**, um die Twitter-Konfiguration zu speichern.


