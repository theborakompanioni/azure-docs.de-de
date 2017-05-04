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
ms.date: 04/21/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 1d712ef6987a4af2014bedb54378f288bcf535a8
ms.lasthandoff: 04/22/2017


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

In diesem Artikel wird erläutert, wie Sie eine **Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung** erstellen. Anweisungen zur Konfiguration von Azure AD-Anwendungen für die Authentifizierung von Endbenutzern finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Store mithilfe von Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Schritt 1: Erstellen Sie eine Active Directory-Webanwendung

Erstellen und Konfigurieren Sie eine Azure AD-Webanwendung für die Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Store mithilfe von Azure Active Directory. Anweisungen finden Sie unter [Erstellen einer Azure AD-Anwendung](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Wenn Sie die Anweisungen in diesem Link befolgen, stellen Sie sicher, dass Sie beim Typ der Anwendung **Web-App/API** auswählen, wie auf dem folgenden Screenshot gezeigt.

![Erstellen einer Web-App](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "Erstellen einer Web-App")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Schritt 2: Abrufen von Anwendungs-ID, Authentifizierungsschlüssel und Mandanten-ID
Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung. Wenn die Anwendung unter ihren eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie außerdem einen Authentifizierungsschlüssel.

* Eine Anleitung zum Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels (auch „Clientgeheimnis“ genannt) finden Sie unter [Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Informationen zum Abrufen der Mandanten-ID finden Sie unter [Abrufen der Mandanten-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Schritt 3: Weisen Sie die Azure AD-Anwendung der Datei oder dem Ordner des Azure Data Lake Store-Kontos zu (nur für die Dienst-zu-Dienst-Authentifizierung).
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

## <a name="step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications"></a>Schritt 4: Abrufen des OAuth 2.0-Token-Endpunkts (nur für Java-basierte Anwendungen)

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an, und klicken Sie im linken Bereich auf „Active Directory“.

2. Klicken Sie im linken Bereich auf **App-Registrierungen**.

3. Klicken Sie oben im Blatt „App-Registrierungen“ auf **Endpunkte**.

    ![OAuth-Token-Endpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "OAuth-Token-Endpunkt")

4. Kopieren Sie aus der Liste der Endpunkte den OAuth 2.0-Token-Endpunkt.

    ![OAuth-Token-Endpunkt](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "OAuth-Token-Endpunkt")   

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Azure AD-Webanwendung erstellt und die erforderlichen Informationen gesammelt, die für Ihre Clientanwendungen nötig sind, die Sie mithilfe des .NET SDKs, des Java SDKs usw. erstellen. Sie können nun mit den nachfolgend aufgeführten Artikeln fortfahren, in denen erläutert wird, wie Sie die Azure AD-Webanwendung verwenden, um sich zum ersten Mal mit Data Lake Store authentifizieren und anschließend andere Vorgänge im Store durchführen.

* [Erste Schritte mit Azure Data Lake-Speicher mithilfe des .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Erste Schritte mit Azure Data Lake Store mit dem Java SDK](data-lake-store-get-started-java-sdk.md)

In diesem Artikel wurden die grundlegenden Schritte erläutert, die zum Einrichten eines Benutzerprinzipals für Ihre Anwendung erforderlich sind. Weitere Informationen finden Sie in den folgenden Artikeln:
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals mit Zertifikat](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Other methods to authenticate to Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios) (Andere Azure AD-Authentifizierungsmethoden)



