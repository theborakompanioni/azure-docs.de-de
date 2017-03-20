---
title: 'Endbenutzerauthentifizierung: Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory umsetzen.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/02/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 0f6af54b351235390afa88f1ce156abd839a723f
ms.lasthandoff: 03/03/2017


---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory
> [!div class="op_single_selector"]
> * [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
> * [Authentifizierung von Endbenutzern](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store oder Azure Data Lake Analytics funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren möchten. Sie haben zwei Möglichkeiten:

* Endbenutzerauthentifizierung (dieser Artikel)
* Dienst-zu-Dienst-Authentifizierung

Bei beiden Optionen erhält Ihre Anwendung ein OAuth 2.0-Token, das an jede an Azure Data Lake Store oder Azure Data Lake Analytics gestellte Anforderung angefügt wird.

Dieser Artikel erläutert, wie Sie eine **native Azure AD-Anwendung für die Authentifizierung von Endbenutzern** erstellen. Für Anweisungen zur Konfiguration von Azure AD-Anwendungen für die Dienst-zu-Dienst-Authentifizierung gehen Sie auf die Seite [Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Ihre Abonnement-ID. Sie können sie über das Azure-Portal abrufen. Sie ist beispielsweise auf dem Blatt „Data Lake Store“ des Kontos verfügbar.
  
    ![Abrufen der Abonnement-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Der Name Ihrer Azure AD-Domäne. Diesen können Sie bestimmen, indem Sie den Mauszeiger über dem rechten oberen Bereich im Azure-Portal bewegen. Im Screenshot unten lautet der Domänenname **contoso.onmicrosoft.com**, und die GUID in Klammern stellt die Mandanten-ID dar. 
  
    ![Abrufen der AAD-Domäne](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Authentifizierung von Endbenutzern
Dieser Ansatz wird empfohlen, wenn sich ein Endbenutzer über Azure AD bei Ihrer Anwendung anmelden soll. Die Anwendung kann dann mit der gleichen Zugriffsstufe wie der angemeldete Endbenutzer auf Azure-Ressourcen zugreifen. Ihre Endbenutzer müssen ihre Anmeldeinformationen in regelmäßigen Abständen eingeben, um weiter Zugriff zu haben.

Das Ergebnis der Endbenutzeranmeldung ist, dass Ihre Anwendung über ein Zugriffs- und ein Aktualisierungstoken verfügt. Das Zugriffstoken wird an jede an Data Lake Store oder Data Lake Analytics gestellte Anforderung angefügt und ist standardmäßig eine Stunde gültig. Mithilfe des Aktualisierungstokens kann ein neues Zugriffstoken abgerufen werden, das bei regelmäßiger Nutzung standardmäßig bis zu zwei Wochen gültig ist. Es gibt zwei Ansätze für die Anmeldung von Endbenutzern.

### <a name="using-the-oauth-20-pop-up"></a>Verwenden des OAuth 2.0-Popupfensters
Ihre Anwendung kann das Einblenden eines OAuth 2.0-Autorisierungsfensters auslösen, in das Endbenutzer ihre Anmeldeinformationen eingeben können. Dieses Popupfenster funktioniert auch mit dem zweistufigen Authentifizierungsprozess von Azure AD. 

> [!NOTE]
> Von der Azure AD Authentication Library (ADAL) für Python oder Java wird diese Methode noch nicht unterstützt.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Direktes Übergeben von Benutzeranmeldeinformationen
Ihre Anwendung kann Azure AD Benutzeranmeldeinformationen direkt bereitstellen. Diese Methode funktioniert nur mit Benutzerkonten mit Organisations-ID. Sie ist nicht kompatibel mit persönlichen bzw. Live ID-Benutzerkonten, die beispielsweise auf @outlook.com oder @live.com enden. Darüber hinaus ist diese Methode nicht kompatibel mit Benutzerkonten, die die zweistufige Authentifizierung von Azure AD benötigen.

### <a name="what-do-i-need-to-use-this-approach"></a>Was brauche ich, um diesen Ansatz zu befolgen?
* Name Ihrer Azure AD-Domäne Dieser ist bereits in den in diesem Artikel angegebenen Voraussetzungen aufgeführt.
* **Native Azure AD-Anwendung**
* Client-ID für die native Azure Anwendung
* Antwort-URI für die native Azure AD-Anwendung
* Festlegen der delegierten Berechtigungen


## <a name="step-1-create-an-active-directory-web-application"></a>Schritt 1: Erstellen Sie eine Active Directory-Webanwendung

Erstellen und Konfigurieren Sie eine native Azure AD-Anwendung für die Authentifizierung von Endbenutzern mit Azure Data Lake Store mithilfe von Azure Active Directory. Anweisungen finden Sie unter [Erstellen einer Azure AD-Anwendung](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Wenn Sie die Anweisungen in diesem Link befolgen, stellen Sie sicher, dass Sie beim Typ der Anwendung **Nativ** auswählen, wie auf dem folgenden Screenshot gezeigt.

![Erstellen einer Webanwendung](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Erstellen einer nativen Anwendung")

## <a name="step-2-get-client-id-reply-uri-and-set-delegated-permissions"></a>Schritt 2: Rufen Sie die Client-ID ab, beantworten Sie den URI, und legen Sie delegierte Berechtigungen fest

Unter [Abrufen der Client-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) finden Sie Informationen, wie Sie die Client-ID (auch Anwendungs-ID genannt) der nativen Azure AD-Anwendung abrufen.

Führen Sie folgende Schritte aus, um den Umleitungs-URI abzurufen.

1. Wählen Sie im Azure-Portal **Azure Active Directory** aus, klicken Sie auf **App-Registrierungen**, suchen Sie die native Azure AD-Anwendung, die Sie gerade erstellt haben, und klicken Sie darauf.

2. Klicken Sie auf dem Blatt **Einstellungen** der Anwendung auf **Umleitungs-URIs**.

    ![Abrufen des Umleitungs-URI](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Kopieren Sie den angezeigten Wert.


## <a name="step-3-set-permissions"></a>Schritt 3: Legen Sie Berechtigungen fest

1. Wählen Sie im Azure-Portal **Azure Active Directory** aus, klicken Sie auf **App-Registrierungen**, suchen Sie die native Azure AD-Anwendung, die Sie gerade erstellt haben, und klicken Sie darauf.

2. Klicken Sie auf dem Blatt **Einstellungen** der Anwendung auf **Erforderliche Berechtigungen**, und klicken Sie auf **Hinzufügen**.

    ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. Klicken Sie auf dem Blatt **API-Zugriff hinzufügen** auf **Hiermit wählen Sie eine API aus**, klicken Sie auf **Azure Data Lake** und anschließend auf **Auswählen**.

    ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  Klicken Sie auf dem Blatt **API-Zugriff hinzufügen** auf **Berechtigungen auswählen**, aktivieren Sie das Kontrollkästchen, um **Data Lake Store vollen Zugriff zu gewähren**, und klicken Sie auf **Auswählen**.

    ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klicken Sie auf **Done**.

5. Wiederholen Sie die letzten beiden Schritte, um ebenfalls Berechtigungen für die **Windows Azure-Service-Verwaltungs-API** zu erteilen.
   
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Azure AD-Webanwendung erstellt und die erforderlichen Informationen gesammelt, die für Ihre Clientanwendungen nötig sind, die Sie mithilfe des .NET SDKs, des Java SDKs usw. erstellen. Sie können nun mit den nachfolgend aufgeführten Artikeln fortfahren, in denen erläutert wird, wie Sie die Azure AD-Webanwendung verwenden, um sich zum ersten Mal mit Data Lake Store authentifizieren und anschließend andere Vorgänge im Store durchführen.

* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)


