---
title: 'Dienst-zu-Dienst-Authentifizierung: Data Lake Store mit Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe von Azure Active Directory die Dienst-zu-Dienst-Authentifizierung mit Data Lake Store implementieren.
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
ms.date: 01/10/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 9019a4115e81a7d8f1960098b1138cd437a0460b
ms.openlocfilehash: dac6c9f3be7b4535f8cb30a9ec0c1e398ca5ff28


---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Dienst-zu-Dienst-Authentifizierung mit Data Lake Store mithilfe von Azure Active Directory
> [!div class="op_single_selector"]
> * [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md)
> * [Authentifizierung von Endbenutzern](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store verwendet Azure Active Directory für die Authentifizierung. Vor dem Erstellen einer Anwendung, die mit Azure Data Lake Store oder Azure Data Lake Analytics funktioniert, müssen Sie entscheiden, wie Sie Ihre Anwendung bei Azure Active Directory (Azure AD) authentifizieren möchten. Sie haben zwei Möglichkeiten:

* Authentifizierung von Endbenutzern 
* Dienst-zu-Dienst-Authentifizierung (dieser Artikel) 

Bei beiden Optionen erhält Ihre Anwendung ein OAuth 2.0-Token, das an jede an Azure Data Lake Store oder Azure Data Lake Analytics gestellte Anforderung angefügt wird.

In diesem Artikel wird erläutert, wie Sie eine Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung erstellen. Anweisungen zur Konfiguration von Azure AD-Anwendungen für die Authentifizierung von Endbenutzern finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).

## <a name="create-an-active-directory-application"></a>Erstellen einer Active Directory-Anwendung
In diesem Abschnitt erfahren Sie, wie Sie eine Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Store mithilfe von Azure Active Directory erstellen und konfigurieren können. Hinweis: Wenn Sie eine Active Directory-Anwendung erstellen, wird zwar ein Dienstprinzipal, aber keine App und auch kein Code für Sie erstellt.

### <a name="step-1-create-an-azure-active-directory-application"></a>Schritt 1: Erstellen Sie eine Azure Active Directory-Anwendung.
1. Melden Sie sich über das [klassische Portal](https://manage.windowsazure.com/)bei Ihrem Azure-Konto an.
2. Wählen Sie im linken Bereich **Active Directory** aus.
   
     ![Active Directory auswählen](./media/data-lake-store-authenticate-using-active-directory/active-directory.png)
3. Wählen Sie das Active Directory aus, das Sie zum Erstellen der neuen Anwendung nutzen möchten. Wenn Sie über mehr als eine Active Directory-Instanz verfügen, möchten Sie normalerweise die Anwendung im Verzeichnis erstellen, in dem sich Ihr Abonnement befindet. Sie können nur Zugriff für die Ressource in Ihrem Abonnement für Anwendungen gewähren, die sich im selben Verzeichnis wie Ihr Abonnement befinden.  
   
     ![Verzeichnis wählen](./media/data-lake-store-authenticate-using-active-directory/active-directory-details.png)
4. Klicken Sie auf **Anwendungen**, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.
   
     ![Anwendungen anzeigen](./media/data-lake-store-authenticate-using-active-directory/view-applications.png)
5. Wenn Sie in diesem Verzeichnis bisher noch keine Anwendung erstellt haben, sollten Sie eine Anzeige ähnlich der folgenden sehen. Klicken Sie auf **EINE ANWENDUNG HINZUFÜGEN**
   
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

#### <a name="note-down-the-following-properties-that-you-will-need-for-the-next-steps"></a>Notieren Sie sich die folgenden, für die nächsten Schritte benötigten Eigenschaften:
1. Den Namen der in Schritt 1.6 erstellten Webanwendungs-ID
2. Die in Schritt 2.2 abgerufene Client-ID
3. Den in Schritt 2.4 erstellten Schlüssel
4. Die in Schritt 2.5 abgerufene Mandanten-ID

### <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Schritt 3: Weisen Sie die Azure AD-Anwendung der Datei oder dem Ordner des Azure Data Lake Store-Kontos zu (nur für die Dienst-zu-Dienst-Authentifizierung).
1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an, und öffnen Sie das Azure Data Lake Store-Konto, das Sie der zuvor erstellten Azure Active Directory-Anwendung zuordnen möchten.
2. Klicken Sie auf dem Blatt Ihres Data Lake-Speicherkontos auf **Daten-Explorer**.
   
    ![Erstellen von Verzeichnissen im Data Lake Store-Konto](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "Erstellen von Verzeichnissen im Data Lake Store-Konto")
3. Klicken Sie auf dem Blatt **Daten-Explorer** auf die Datei oder den Ordner, für die bzw. den Sie den Zugriff auf die Azure AD-Anwendung festlegen möchten, und klicken Sie dann auf **Zugriff**. Um den Zugriff auf eine Datei zu konfigurieren, müssen Sie auf dem Blatt **Dateivorschau** auf **Zugriff** klicken.
   
    ![Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "Festlegen von Zugriffssteuerungslisten für das Data Lake-Dateisystem")
4. Auf dem Blatt **Zugriff** sind der Standardzugriff und der benutzerdefinierte Zugriff aufgeführt, die dem Stamm bereits zugewiesen wurden. Klicken Sie auf das Symbol **Hinzufügen** , um Zugriffssteuerungslisten auf benutzerdefinierter Ebene hinzuzufügen.
   
    ![Auflisten von Standardzugriff und benutzerdefiniertem Zugriff](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "Auflisten von Standardzugriff und benutzerdefiniertem Zugriff")
5. Klicken Sie auf das Symbol **Hinzufügen**, um das Blatt **Benutzerdefinierten Zugriff hinzufügen** zu öffnen. Klicken Sie auf diesem Blatt auf **Benutzer oder Gruppe auswählen**, und suchen Sie dann auf dem Blatt **Benutzer oder Gruppe auswählen** nach der Azure Active Directory-Anwendung, die Sie zuvor erstellt haben. Wenn Sie über viele Gruppen verfügen, in denen Sie suchen können, können Sie das Textfeld oben zum Filtern nach dem Gruppennamen verwenden. Klicken Sie auf die Gruppe, die Sie hinzufügen möchten, und klicken Sie dann auf **Auswählen**.
   
    ![Hinzufügen einer Gruppe](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Hinzufügen einer Gruppe")
6. Klicken Sie auf **Berechtigungen auswählen**, wählen Sie die Berechtigungen aus, und legen Sie fest, ob die Berechtigungen als Standard-ACL und/oder Zugriffs-ACL zugewiesen werden sollen. Klicken Sie auf **OK**.
   
    ![Zuweisen von Berechtigungen zu einer Gruppe](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Zuweisen von Berechtigungen zu einer Gruppe")
   
    Weitere Informationen zu Berechtigungen in Data Lake Store und zu Standard- und Zugriffs-ACLs finden Sie unter [Zugriffssteuerung in Data Lake Store](data-lake-store-access-control.md).
7. Klicken Sie auf dem Blatt **Benutzerdefinierten Zugriff hinzufügen** auf **OK**. Die neu hinzugefügte Gruppe mit den zugeordneten Berechtigungen wird jetzt auf dem Blatt **Zugriff** aufgelistet.
   
    ![Zuweisen von Berechtigungen zu einer Gruppe](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Zuweisen von Berechtigungen zu einer Gruppe")    

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Azure AD-Webanwendung erstellt und die erforderlichen Informationen gesammelt, die für Ihre Clientanwendungen nötig sind, die Sie mithilfe des .NET SDKs, des Java SDKs usw. erstellen. Sie können nun mit den nachfolgend aufgeführten Artikeln fortfahren, in denen erläutert wird, wie Sie die Azure AD-Webanwendung verwenden, um sich zum ersten Mal mit Data Lake Store authentifizieren und anschließend andere Vorgänge im Store durchführen.

* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)

In diesem Artikel wurden die grundlegenden Schritte erläutert, die zum Einrichten eines Benutzerprinzipals für Ihre Anwendung erforderlich sind. Weitere Informationen finden Sie in den folgenden Artikeln:
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals mit Zertifikat](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Other methods to authenticate to Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios) (Andere Azure AD-Authentifizierungsmethoden)





<!--HONumber=Jan17_HO4-->


