<properties
   pageTitle="Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory | Microsoft Azure"
   description="Informationen zum Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>


# <a name="authenticate-with-data-lake-store-using-azure-active-directory"></a>Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store oder Azure Data Lake Analytics funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren möchten. Sie haben zwei Möglichkeiten:

* Authentifizierung von Endbenutzern und 
* Dienst-zu-Dienst-Authentifizierung 

Bei beiden Optionen erhält Ihre Anwendung ein OAuth 2.0-Token, das an jede an Azure Data Lake Store oder Azure Data Lake Analytics gestellte Anforderung angefügt wird.


## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Ihre Abonnement-ID. Sie können sie über das Azure-Portal abrufen. Sie ist beispielsweise auf dem Blatt „Data Lake Store“ des Kontos verfügbar.

    ![Abrufen der Abonnement-ID](./media/data-lake-store-authenticate-using-active-directory/get-subscription-id.png)

* Der Name Ihrer Azure AD-Domäne. Diesen können Sie bestimmen, indem Sie den Mauszeiger über dem rechten oberen Bereich im Azure-Portal bewegen. 

    ![Abrufen der AAD-Domäne](./media/data-lake-store-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Authentifizierung von Endbenutzern

Dieser Ansatz wird empfohlen, wenn sich ein Endbenutzer über Azure AD bei Ihrer Anwendung anmelden soll. Die Anwendung kann dann mit der gleichen Zugriffsstufe wie der angemeldete Endbenutzer auf Azure-Ressourcen zugreifen. Ihre Endbenutzer müssen ihre Anmeldeinformationen in regelmäßigen Abständen eingeben, um weiter Zugriff zu haben.

Das Ergebnis der Endbenutzeranmeldung ist, dass Ihre Anwendung über ein Zugriffs- und ein Aktualisierungstoken verfügt. Das Zugriffstoken wird an jede an Data Lake Store oder Data Lake Analytics gestellte Anforderung angefügt und ist standardmäßig eine Stunde gültig. Mithilfe des Aktualisierungstokens kann ein neues Zugriffstoken abgerufen werden, das bei regelmäßiger Nutzung standardmäßig bis zu zwei Wochen gültig ist. Es gibt zwei Ansätze für die Anmeldung von Endbenutzern.

### <a name="using-the-oauth-2.0-pop-up"></a>Verwenden des OAuth 2.0-Popupfensters

Ihre Anwendung kann das Einblenden eines OAuth 2.0-Autorisierungsfensters auslösen, in das Endbenutzer ihre Anmeldeinformationen eingeben können. Dieses Popupfenster funktioniert auch mit dem zweistufigen Authentifizierungsprozess von Azure AD. 

>[AZURE.NOTE] Von der Azure AD Authentication Library (ADAL) für Python oder Java wird diese Methode noch nicht unterstützt.

### <a name="directly-passing-in-user-credentials"></a>Direktes Übergeben von Benutzeranmeldeinformationen

Ihre Anwendung kann Azure AD Benutzeranmeldeinformationen direkt bereitstellen. Diese Methode funktioniert nur mit Benutzerkonten mit Organisations-ID. Sie ist nicht kompatibel mit persönlichen bzw. „Live ID“-Benutzerkonten, die z.B. auf @outlook.com oder @live.com. enden. Darüber hinaus ist diese Methode nicht kompatibel mit Benutzerkonten, die die zweistufige Authentifizierung von Azure AD benötigen.

### <a name="what-do-i-need-to-use-this-approach?"></a>Was brauche ich, um diesen Ansatz zu befolgen?

* Name der Azure AD-Domäne (bereits in den in diesem Artikel angegebenen Voraussetzungen aufgeführt)

* **Native Azure AD-Clientanwendung** 

* Client-ID für die native Azure AD-Clientanwendung

* Umleitungs-URI für die native Azure AD-Clientanwendung 

Eine Anleitung zum Erstellen einer Azure AD-Anwendung und zum Abrufen der Client-ID finden Sie unter [Erstellen einer Active Directory-Anwendung](../resource-group-create-service-principal-portal.md#create-an-active-directory-application). 

>[AZURE.NOTE] Die Anleitung unter dem obigen Link bezieht sich auf eine Azure AD-Webanwendung. Zur Erstellung einer nativen Clientanwendung müssen jedoch genau die gleichen Schritte ausgeführt werden.

## <a name="service-to-service-authentication"></a>Dienst-zu-Dienst-Authentifizierung

Dieser Ansatz wird empfohlen, wenn sich Ihre Anwendung automatisch bei Azure AD authentifizieren soll, ohne dass Endbenutzer ihre Anmeldeinformationen angeben müssen. Ihre Anwendung kann sich so lange authentifizieren, wie ihre Anmeldeinformationen gültig sind, was auf einen Zeitraum von Jahren angepasst werden kann.

### <a name="what-do-i-need-to-use-this-approach?"></a>Was brauche ich, um diesen Ansatz zu befolgen?

* Name der Azure AD-Domäne (bereits in den in diesem Artikel angegebenen Voraussetzungen aufgeführt)

* Azure AD- **Webanwendung**

* Client-ID für die Azure AD-Webanwendung

    >[AZURE.NOTE] Eine Anleitung zum Erstellen einer Azure AD-Anwendung und zum Abrufen der Client-ID finden Sie unter [Erstellen einer Active Directory-Anwendung](../resource-group-create-service-principal-portal.md#create-an-active-directory-application).
    
* Konfigurieren Sie die Azure AD-Webanwendung so, dass entweder der geheime Clientschlüssel oder ein Zertifikat verwendet wird. Informationen zum Erstellen einer Webanwendung, die mit einem Zertifikat arbeitet, finden Sie unter [Erstellen eines Dienstprinzipals mit Zertifikat](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate).

* Aktivieren Sie den Zugriff für die Azure AD-Webanwendung in der Data Lake Store-Datei bzw. dem Data Lake Store-Ordner oder im Data Lake Analytics-Konto, mit dem Sie arbeiten möchten. Informationen zum Bereitstellen des Zugriffs einer Azure AD-Anwendung auf eine Data Lake Store-Datei bzw. einen Data Lake Store-Ordner finden Sie unter [Zuweisen von Benutzern oder Sicherheitsgruppen als Zugriffssteuerungslisten zum Azure Data Lake Store-Dateisystem](data-lake-store-secure-data.md#filepermissions).

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)



<!--HONumber=Oct16_HO2-->


