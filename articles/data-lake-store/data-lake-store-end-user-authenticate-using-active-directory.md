---
title: Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory | Microsoft Docs
description: Informationen zum Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory
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
ms.date: 10/17/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 7e7c9fd2fe7e6327cd39c4c26583c8fd556c9044


---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory
> [!div class="op_single_selector"]
> * [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
> * [Authentifizierung von Endbenutzern](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store oder Azure Data Lake Analytics funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren möchten. Sie haben zwei Möglichkeiten:

* Authentifizierung von Endbenutzern und 
* Dienst-zu-Dienst-Authentifizierung 

Bei beiden Optionen erhält Ihre Anwendung ein OAuth 2.0-Token, das an jede an Azure Data Lake Store oder Azure Data Lake Analytics gestellte Anforderung angefügt wird.

Dieser Artikel erläutert, wie Sie eine Azure AD-Webanwendung für die Authentifizierung von Endbenutzern erstellen. Für Anweisungen zur Konfiguration von Azure AD-Anwendungen für die Dienst-zu-Dienst-Authentifizierung gehen Sie auf die Seite [Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Ihre Abonnement-ID. Sie können sie über das Azure-Portal abrufen. Sie ist beispielsweise auf dem Blatt „Data Lake Store“ des Kontos verfügbar.
  
    ![Abrufen der Abonnement-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)
* Der Name Ihrer Azure AD-Domäne. Diesen können Sie bestimmen, indem Sie den Mauszeiger über dem rechten oberen Bereich im Azure-Portal bewegen. Im Screenshot unten lautet der Domänenname **contoso.microsoft.com**, und die GUID in Klammern stellt die Mandanten-ID dar. 
  
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
Ihre Anwendung kann Azure AD Benutzeranmeldeinformationen direkt bereitstellen. Diese Methode funktioniert nur mit Benutzerkonten mit Organisations-ID. Sie ist nicht kompatibel mit persönlichen bzw. „Live ID“-Benutzerkonten, die z.B. auf @outlook.com oder @live.com. enden. Darüber hinaus ist diese Methode nicht kompatibel mit Benutzerkonten, die die zweistufige Authentifizierung von Azure AD benötigen.

### <a name="what-do-i-need-to-use-this-approach"></a>Was brauche ich, um diesen Ansatz zu befolgen?
* Name Ihrer Azure AD-Domäne Dieser ist bereits in den in diesem Artikel angegebenen Voraussetzungen aufgeführt.
* Azure AD-**Webanwendung**
* Client-ID für die Azure AD-Webanwendung
* Antwort-URI für die Azure AD-Webanwendung
* Festlegen der delegierten Berechtigungen

Für Anweisungen zur Erstellung einer Azure AD-Webanwendung und um diese für die oben angegebenen Anforderungen zu konfigurieren, lesen Sie den nachfolgenden Artikel [Erstellen einer Active Directory-Anwendung](#create-an-active-directory-application). 

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung
In diesem Abschnitt erfahren Sie, wie Sie eine Azure AD-Webanwendung für die Authentifizierung von Endbenutzern mit Azure Data Lake Store mithilfe von Azure Active Directory erstellen und konfigurieren können.

### <a name="step-1-create-an-azure-active-directory-application"></a>Schritt 1: Erstellen Sie eine Azure Active Directory-Anwendung.
> [!NOTE]
> Bei den Schritten unten kommt das Azure-Portal zum Einsatz. Sie können auch eine Azure AD-Anwendung mithilfe von [Azure PowerShell](../resource-group-authenticate-service-principal.md) oder der [Azure-Befehlszeilenschnittstelle](../resource-group-authenticate-service-principal-cli.md) erstellen.
> 
> 

1. Melden Sie sich über das [klassische Portal](https://manage.windowsazure.com/)bei Ihrem Azure-Konto an.
2. Wählen Sie im linken Bereich **Active Directory** aus.
   
     ![Active Directory auswählen](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory.png)
3. Wählen Sie das Active Directory aus, das Sie zum Erstellen der neuen Anwendung nutzen möchten. Wenn Sie über mehr als eine Active Directory-Instanz verfügen, möchten Sie normalerweise die Anwendung im Verzeichnis erstellen, in dem sich Ihr Abonnement befindet. Sie können nur Zugriff für die Ressource in Ihrem Abonnement für Anwendungen gewähren, die sich im selben Verzeichnis wie Ihr Abonnement befinden.  
   
     ![Verzeichnis wählen](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory-details.png)
4. Klicken Sie auf **Anwendungen**, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.
   
     ![Anwendungen anzeigen](./media/data-lake-store-end-user-authenticate-using-active-directory/view-applications.png)
5. Wenn Sie in diesem Verzeichnis zuvor noch keine Anwendung erstellt haben, sollten Sie eine Abbildung ähnlich dieser hier sehen. Klicken Sie auf **EINE ANWENDUNG HINZUFÜGEN**
   
     ![Anwendung hinzufügen](./media/data-lake-store-end-user-authenticate-using-active-directory/create-application.png)
   
     Oder klicken Sie im unteren Bereich auf **Hinzufügen** .
   
     ![Hinzufügen](./media/data-lake-store-end-user-authenticate-using-active-directory/add-icon.png)
6. Geben Sie einen Namen für die Anwendung ein, und wählen Sie den Typ der Anwendung aus, die Sie erstellen möchten. Erstellen Sie für dieses Tutorial eine **WEBANWENDUNG UND/ODER WEB-API** , und klicken Sie auf die Schaltfläche „Weiter“.
   
     ![Anwendung benennen](./media/data-lake-store-end-user-authenticate-using-active-directory/tell-us-about-your-application.png)
7. Tragen Sie die Eigenschaften Ihrer Anwendung ein. Geben Sie für die **ANMELDE-URL**den URI einer Website an, die Ihre Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. 
   Geben Sie für die **APP-ID URI**die URI an, die Ihre Anwendung identifiziert.
   
     ![Anwendungseigenschaften](./media/data-lake-store-end-user-authenticate-using-active-directory/app-properties.png)
   
    Klicken Sie auf das Häkchen, um den Assistenten abzuschließen und die Anwendung zu erstellen.

### <a name="step-2-get-client-id-reply-uri-and-set-delegated-permissions"></a>Schritt 2: Rufen Sie die Client-ID ab, beantworten Sie den URI, und legen Sie delegierte Berechtigungen fest
1. Klicken Sie auf die Registrierkarte **Konfigurieren**, um das Kennwort für Ihre Anwendung zu konfigurieren.
   
     ![Anwendung konfigurieren](./media/data-lake-store-end-user-authenticate-using-active-directory/application-configure.png)
2. Kopieren Sie die **CLIENT-ID**.
   
     ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/client-id.png)
3. Kopieren Sie unter dem Abschnitt **Einmaliges Anmelden** den **Antwort-URI**.
   
    ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-get-reply-uri.png)
4. Klicken Sie im Abschnitt **Berechtigungen für andere Anwendungen** auf **Anwendung hinzufügen**
   
    ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)
5. Wählen Sie im Assistenten **Berechtigungen für andere Anwendungen** die Option **Azure Data Lake** und **Windows** **Azure-Service-Dienstverwaltungs-API** aus, und klicken Sie auf das Häkchen.
6. In der Standardeinstellung sind die **delegierten Berechtigungen** für die neu hinzugefügten Dienste auf Null festgelegt. Klicken Sie auf das Dropdownfeld **Delegierte Berechtigungen** für Azure Data Lake und den Windows Azure-Verwaltungsdienst, und wählen Sie die verfügbaren Kontrollkästchen aus, um die Werte auf 1 festzulegen. Das Ergebnis sollte wie folgt aussehen:
   
     ![CLIENT-ID](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
7. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Azure AD-Webanwendung erstellt und die erforderlichen Informationen gesammelt, die für Ihre Clientanwendungen nötig sind, die Sie mithilfe des .NET SDKs, des Java SDKs usw. erstellen. Sie können nun mit den nachfolgend aufgeführten Artikeln fortfahren, in denen erläutert wird, wie Sie die Azure AD-Webanwendung verwenden, um sich zum ersten Mal mit Data Lake Store authentifizieren und anschließend andere Vorgänge im Store durchführen.

* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)




<!--HONumber=Nov16_HO3-->


