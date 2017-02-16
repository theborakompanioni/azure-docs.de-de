---
title: Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory | Microsoft Docs
description: Informationen zum Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 35cde786bbc091c58f4dcb341cd47ce4c4f4b46c
ms.openlocfilehash: 02e52c3aba82ab8e3a8b1dc921731c29e505e23e


---
# <a name="service-to-serivce-authentication-with-data-lake-store-using-azure-active-directory"></a>Dienst-zu-Dienst-Authentifizierung bei Data Lake Store mithilfe von Azure Active Directory
> [!div class="op_single_selector"]
> * [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
> * [Authentifizierung von Endbenutzern](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store oder Azure Data Lake Analytics funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren möchten. Sie haben zwei Möglichkeiten:

* Authentifizierung von Endbenutzern und 
* Dienst-zu-Dienst-Authentifizierung 

Bei beiden Optionen erhält Ihre Anwendung ein OAuth 2.0-Token, das an jede an Azure Data Lake Store oder Azure Data Lake Analytics gestellte Anforderung angefügt wird.

In diesem Artikel wird erläutert, wie Sie eine Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung erstellen. Anweisungen zur Konfiguration von Azure AD-Anwendungen für die Authentifizierung von Endbenutzern finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Ihre Abonnement-ID. Sie können sie über das Azure-Portal abrufen. Sie ist beispielsweise auf dem Blatt „Data Lake Store“ des Kontos verfügbar.
  
    ![Abrufen der Abonnement-ID](./media/data-lake-store-authenticate-using-active-directory/get-subscription-id.png)
* Der Name Ihrer Azure AD-Domäne. Diesen können Sie bestimmen, indem Sie den Mauszeiger über dem rechten oberen Bereich im Azure-Portal bewegen. Im Screenshot unten lautet der Domänenname **contoso.microsoft.com**, und die GUID in Klammern stellt die Mandanten-ID dar. 
  
    ![Abrufen der AAD-Domäne](./media/data-lake-store-authenticate-using-active-directory/get-aad-domain.png)

## <a name="service-to-service-authentication"></a>Dienst-zu-Dienst-Authentifizierung
Dieser Ansatz wird empfohlen, wenn sich Ihre Anwendung automatisch bei Azure AD authentifizieren soll, ohne dass Endbenutzer ihre Anmeldeinformationen angeben müssen. Ihre Anwendung kann sich so lange authentifizieren, wie ihre Anmeldeinformationen gültig sind, was auf einen Zeitraum von Jahren angepasst werden kann.

### <a name="what-do-i-need-to-use-this-approach"></a>Was brauche ich, um diesen Ansatz zu befolgen?
* Name Ihrer Azure AD-Domäne Dieser ist bereits in den in diesem Artikel angegebenen Voraussetzungen aufgeführt.
* Azure AD- **Webanwendung**
* Client-ID für die Azure AD-Webanwendung
* Clientgeheimnis für die Azure AD-Webanwendung
* Tokenendpunkt für die Azure AD-Webanwendung
* Aktivieren Sie den Zugriff für die Azure AD-Webanwendung in der Data Lake Store-Datei bzw. dem Data Lake Store-Ordner oder im Data Lake Analytics-Konto, mit dem Sie arbeiten möchten.

Für Anweisungen zur Erstellung einer Azure AD-Webanwendung und um diese für die oben angegebenen Anforderungen zu konfigurieren, lesen Sie den nachfolgenden Artikel [Erstellen einer Active Directory-Anwendung](#create-an-active-directory-application).

> [!NOTE]
> Standardmäßig ist die Azure AD-Anwendung für die Verwendung des Clientgeheimnisses konfiguriert, das Sie aus der Azure AD-Anwendung abrufen können. Wenn die Azure AD-Anwendung jedoch stattdessen ein Zertifikat verwenden soll, müssen Sie die Azure AD-Webanwendung mithilfe von Azure PowerShell erstellen, wie unter [Erstellen eines Dienstprinzipals mit Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) beschrieben.
> 
> 

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung
In diesem Abschnitt erfahren Sie, wie Sie eine Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Store mithilfe von Azure Active Directory erstellen und konfigurieren können. 

### <a name="step-1-create-an-azure-active-directory-application"></a>Schritt 1: Erstellen Sie eine Azure Active Directory-Anwendung.
> [!NOTE]
> Bei den Schritten unten kommt das Azure-Portal zum Einsatz. Sie können auch eine Azure AD-Anwendung mithilfe von [Azure PowerShell](../resource-group-authenticate-service-principal.md) oder der [Azure-Befehlszeilenschnittstelle](../resource-group-authenticate-service-principal-cli.md) erstellen.
> 
> 

1. Melden Sie sich über das [klassische Portal](https://manage.windowsazure.com/)bei Ihrem Azure-Konto an.
2. Wählen Sie im linken Bereich **Active Directory** aus.
   
     ![Active Directory auswählen](./media/data-lake-store-authenticate-using-active-directory/active-directory.png)
3. Wählen Sie das Active Directory aus, das Sie zum Erstellen der neuen Anwendung nutzen möchten. Wenn Sie über mehr als eine Active Directory-Instanz verfügen, möchten Sie normalerweise die Anwendung im Verzeichnis erstellen, in dem sich Ihr Abonnement befindet. Sie können nur Zugriff für die Ressource in Ihrem Abonnement für Anwendungen gewähren, die sich im selben Verzeichnis wie Ihr Abonnement befinden.  
   
     ![Verzeichnis wählen](./media/data-lake-store-authenticate-using-active-directory/active-directory-details.png)
4. Klicken Sie auf **Anwendungen**, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.
   
     ![Anwendungen anzeigen](./media/data-lake-store-authenticate-using-active-directory/view-applications.png)
5. Wenn Sie in diesem Verzeichnis zuvor noch keine Anwendung erstellt haben, sollten Sie eine Abbildung ähnlich dieser hier sehen. Klicken Sie auf **EINE ANWENDUNG HINZUFÜGEN**
   
     ![Anwendung hinzufügen](./media/data-lake-store-authenticate-using-active-directory/create-application.png)
   
     Oder klicken Sie im unteren Bereich auf **Hinzufügen** .
   
     ![Hinzufügen](./media/data-lake-store-authenticate-using-active-directory/add-icon.png)
6. Geben Sie einen Namen für die Anwendung ein, und wählen Sie den Typ der Anwendung aus, die Sie erstellen möchten. Erstellen Sie für dieses Tutorial eine **WEBANWENDUNG UND/ODER WEB-API** , und klicken Sie auf die Schaltfläche „Weiter“.
   
     ![Anwendung benennen](./media/data-lake-store-authenticate-using-active-directory/tell-us-about-your-application.png)

    > [!TIP]
    > Geben Sie einen Namen für die Anwendung an, nach dem leichter gesucht werden kann. Weiter unten in diesem Tutorial suchen Sie nach dieser Anwendung und weisen ihr ein Data Lake Store-Konto zu.
    > 
    > 

7. Tragen Sie die Eigenschaften Ihrer Anwendung ein. Geben Sie für die **ANMELDE-URL**den URI einer Website an, die Ihre Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. 
   Geben Sie für die **APP-ID URI**die URI an, die Ihre Anwendung identifiziert.
   
     ![Anwendungseigenschaften](./media/data-lake-store-authenticate-using-active-directory/app-properties.png)
   
    Klicken Sie auf das Häkchen, um den Assistenten abzuschließen und die Anwendung zu erstellen.

### <a name="step-2-get-client-id-client-secret-and-token-endpoint"></a>Schritt 2: Rufen Sie die Client-ID, das Clientgeheimnis und den Tokenendpunkt ab.
Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung. Wenn die Anwendung unter ihren eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie außerdem einen Authentifizierungsschlüssel.

1. Klicken Sie auf die Registrierkarte **Konfigurieren**, um das Kennwort für Ihre Anwendung zu konfigurieren.
   
     ![Anwendung konfigurieren](./media/data-lake-store-authenticate-using-active-directory/application-configure.png)
2. Kopieren Sie die **CLIENT-ID**.
   
     ![CLIENT-ID](./media/data-lake-store-authenticate-using-active-directory/client-id.png)
3. Wenn die Anwendung unter eigenen Anmeldeinformationen ausgeführt wird, scrollen Sie nach unten zum Abschnitt **Schlüssel**, und wählen Sie den gewünschten Gültigkeitszeitraum für Ihr Kennwort aus.
   
     ![keys](./media/data-lake-store-authenticate-using-active-directory/create-key.png)
4. Wählen Sie **Speichern** aus, um Ihren Schlüssel zu erstellen.
   
    ![Speichern](./media/data-lake-store-authenticate-using-active-directory/save-icon.png)
   
    Der gespeicherte Schlüssel wird angezeigt, sodass Sie ihn kopieren können. Sie müssen den Schlüssel jetzt kopieren, da Sie ihn später nicht mehr abrufen können.
   
    ![gespeicherter Schlüssel](./media/data-lake-store-authenticate-using-active-directory/save-key.png)
5. Rufen Sie den Tokenendpunkt ab, indem Sie unten auf der Seite die Option **Endpunkte anzeigen** auswählen und den Wert für das Feld **OAuth 2.0-Token-Endpunkt** wie unten gezeigt abrufen.  
   
    ![Mandanten-ID](./media/data-lake-store-authenticate-using-active-directory/save-tenant.png)

### <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Schritt 3: Weisen Sie die Azure AD-Anwendung der Datei oder dem Ordner des Azure Data Lake Store-Kontos zu (nur für die Dienst-zu-Dienst-Authentifizierung).
1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an, und öffnen Sie das Azure Data Lake Store-Konto, das Sie der zuvor erstellten Azure Active Directory-Anwendung zuordnen möchten.
2. Klicken Sie auf dem Blatt Ihres Data Lake-Speicherkontos auf **Daten-Explorer**.
   
    ![Verzeichnisse in Data Lake-Speicherkonto erstellen](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Create directories in Data Lake account")
3. Klicken Sie auf dem Blatt **Daten-Explorer** auf die Datei oder den Ordner, für die bzw. den Sie den Zugriff auf die Azure AD-Anwendung festlegen möchten, und klicken Sie dann auf **Zugriff**. Um den Zugriff auf eine Datei zu konfigurieren, müssen Sie auf dem Blatt **Dateivorschau** auf **Zugriff** klicken.
   
    ![Zugriffssteuerungslisten für Data Lake-Dateisystem festlegen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Set ACLs on Data Lake file system")
4. Auf dem Blatt **Zugriff** sind der Standardzugriff und der benutzerdefinierte Zugriff aufgeführt, die dem Stamm bereits zugewiesen wurden. Klicken Sie auf das Symbol **Hinzufügen** , um Zugriffssteuerungslisten auf benutzerdefinierter Ebene hinzuzufügen.
   
    ![Standardzugriff und benutzerdefinierten Zugriff auflisten](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "List standard and custom access")
5. Klicken Sie auf das Symbol **Hinzufügen**, um das Blatt **Benutzerdefinierten Zugriff hinzufügen** zu öffnen. Klicken Sie auf diesem Blatt auf **Benutzer oder Gruppe auswählen**, und suchen Sie dann auf dem Blatt **Benutzer oder Gruppe auswählen** nach der Azure Active Directory-Anwendung, die Sie zuvor erstellt haben. Wenn Sie über viele Gruppen verfügen, in denen Sie suchen können, können Sie das Textfeld oben zum Filtern nach dem Gruppennamen verwenden. Klicken Sie auf die Gruppe, die Sie hinzufügen möchten, und klicken Sie dann auf **Auswählen**.
   
    ![Gruppe hinzufügen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Add a group")
6. Klicken Sie auf **Berechtigungen auswählen**, wählen Sie die Berechtigungen aus, und legen Sie fest, ob die Berechtigungen als Standard-ACL und/oder Zugriffs-ACL zugewiesen werden sollen. Klicken Sie auf **OK**.
   
    ![Berechtigungen für Gruppe zuweisen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Assign permissions to group")
   
    Weitere Informationen zu Berechtigungen in Data Lake Store und zu Standard- und Zugriffs-ACLs finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).
7. Klicken Sie auf dem Blatt **Benutzerdefinierten Zugriff hinzufügen** auf **OK**. Die neu hinzugefügte Gruppe mit den zugeordneten Berechtigungen wird jetzt auf dem Blatt **Zugriff** aufgelistet.
   
    ![Berechtigungen für Gruppe zuweisen](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Assign permissions to group")    

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Azure AD-Webanwendung erstellt und die erforderlichen Informationen gesammelt, die für Ihre Clientanwendungen nötig sind, die Sie mithilfe des .NET SDKs, des Java SDKs usw. erstellen. Sie können nun mit den nachfolgend aufgeführten Artikeln fortfahren, in denen erläutert wird, wie Sie die Azure AD-Webanwendung verwenden, um sich zum ersten Mal mit Data Lake Store authentifizieren und anschließend andere Vorgänge im Store durchführen.

* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)




<!--HONumber=Nov16_HO5-->


