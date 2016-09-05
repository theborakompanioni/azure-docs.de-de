<properties
   pageTitle="Erstellen eines Azure-Dienstprinzipals mithilfe von PowerShell | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie mithilfe von Azure PowerShell eine Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tomfitz"/>

# Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wenn eine Ihrer Anwendungen oder eines Ihrer Skripts Zugriff auf Ressourcen benötigt, möchten Sie diesen Prozess wahrscheinlich nicht unter den Anmeldeinformationen eines Benutzers ausführen. Der Benutzer hat unter Umständen andere Berechtigungen als Sie dem Prozess zuweisen möchten, und die Aufgaben des Benutzers können sich unter Umständen ändern. Stattdessen können Sie für die Anwendung eine Identität erstellen, die Anmeldeinformationen für die Authentifizierung und Rollenzuweisungen enthält. Die Anwendung meldet sich bei jeder Ausführung mit dieser Identität an. In diesem Thema erfahren Sie, wie Sie mithilfe von [Azure PowerShell](powershell-install-configure.md) alle Komponenten und Einstellungen einrichten, die Sie benötigen, um eine Anwendung mit eigenen Anmeldeinformationen und einer eigenen Identität auszuführen.

Dazu erstellen Sie in diesem Artikel zwei Objekte: die AD-Anwendung (Active Directory) und den Dienstprinzipal. Die AD-Anwendung enthält die Anmeldeinformationen (eine Anwendungs-ID und ein Kennwort oder Zertifikat). Der Dienstprinzipal enthält die Rollenzuweisung. Über die AD-Anwendung können Sie viele Dienstprinzipale erstellen. Dieses Thema konzentriert sich auf eine Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet. Falls Ihre Anwendung in mehreren Organisationen ausführbar sein muss, können Sie auch Anwendungen mit mehreren Mandanten erstellen. Anwendungen mit mehreren Mandanten werden üblicherweise für SaaS-Anwendungen (Software-as-a-Service) verwendet. Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

Für die Verwendung von Active Directory müssen Sie mit zahlreichen Konzepten vertraut sein. Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Mit Azure PowerShell stehen Ihnen für die Authentifizierung Ihrer AD-Anwendung zwei Optionen zur Verfügung:

 - password
 - Zertifikat

Wenn Sie sich nach dem Einrichten Ihrer AD-Anwendung über ein anderes Programmierframework (wie Python, Ruby oder Node.js) bei Azure anmelden möchten, ist die Kennwortauthentifizierung wahrscheinlich die beste Option. Setzen Sie sich mit den Beispielen für die Authentifizierung in den unterschiedlichen Frameworks im Abschnitt [Beispielanwendungen](#sample-applications) auseinander, bevor Sie sich für ein Kennwort oder für ein Zertifikat entscheiden.

## Abrufen der Mandanten-ID

Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Da Sie diesen Wert sowohl für die kennwortbasierte als auch für die zertifikatbasierte Authentifizierung benötigen, rufen wir diesen Wert nun ab.

1. Melden Sie sich bei Ihrem Konto an.

        Add-AzureRmAccount

2. Wenn Sie nur über ein einziges Abonnement verfügen, können Sie Folgendes verwenden:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Wenn Sie über mehrere Abonnements verfügen, geben Sie das für die AD-App vorgesehene Abonnement an. Wählen Sie das Abonnement aus, in dem sich Ihr Active Directory befindet. Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](./active-directory/active-directory-administer.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Fahren Sie nun mit einem der folgenden Abschnitte für eine [kennwortbasierte](#create-service-principal-with-password) oder [zertifikatbasierte](#create-service-principal-with-certificate) Authentifizierung fort.

## Erstellen eines Dienstprinzipals mit Kennwort

Dieser Abschnitt enthält die Schritte zum Erstellen der AD-Anwendung und des Dienstprinzipals mit einem Kennwort.

1. Erstellen Sie wie folgt eine neue Active Directory-Anwendung: Geben Sie einen Anzeigenamen für die Anwendung, den URI zu einer Seite mit einer Beschreibung der Anwendung (der Link wird nicht geprüft), die URIs, mit denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität an.

        $azureAdApplication = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -Password "<Your_Password>"

     Untersuchen Sie das neue Anwendungsobjekt.

        $azureAdApplication
        
     Beachten Sie vor allem die **ApplicationId**-Eigenschaft, die benötigt wird, um Dienstprinzipale und Rollenzuweisungen zu erstellen und das Zugriffstoken abzurufen.

        DisplayName             : exampleapp
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        ApplicationObjectId     : c95e67a3-403c-40ac-9377-115fa48f8f39
        AvailableToOtherTenants : False
        AppPermissions          : {}
        IdentifierUris          : {https://www.contoso.org/example}
        ReplyUrls               : {}


     Über die AD-Anwendung müssen Sie einen Dienstprinzipal erstellen und ihm eine Rolle zuweisen.

2. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

3. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Geben Sie beim **ServicePrincipalName**-Parameter entweder die **ApplicationId** oder die **IdentifierUris** an, die Sie beim Erstellen der Anwendung verwendet haben. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md). Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Im nächsten Abschnitt erfahren Sie, wie Sie sich mit den Anmeldeinformationen über PowerShell anmelden. Wenn Sie die Anmeldeinformationen jedoch in Ihrer Codeanwendung verwenden möchten, müssen Sie mit diesem Thema nicht fortfahren. In diesem Fall können Sie sich in den [Beispielanwendungen](#sample-applications) Beispiele für die Anmeldung mit Anwendungs-ID und Kennwort ansehen.

### Bereitstellen von Anmeldeinformationen über PowerShell

Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Erstellen Sie ein neues **PSCredential**-Objekt, das Ihre Anmeldeinformationen enthält, indem Sie den Befehl **Get-Credential** ausführen. Vor dem Ausführen dieses Befehls benötigen Sie **ApplicationId** oder **IdentifierUris**. Sorgen Sie daher dafür, dass die entsprechende Angabe zum Einfügen zur Verfügung steht.

        $creds = Get-Credential

2. Sie werden zum Eingeben Ihrer Anmeldeinformationen aufgefordert. Verwenden Sie als Benutzernamen die **ApplicationId** oder die **IdentifierUris**, die Sie beim Erstellen der Anwendung verwendet haben. Verwenden Sie das Kennwort, das Sie beim Erstellen des Kontos angegeben haben.

     ![Anmeldeinformationen eingeben](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

4. Melden Sie sich als Dienstprinzipal an, indem Sie angeben, dass dieses Konto ein Dienstprinzipal ist, und das Anmeldeinformationsobjekt bereitstellen. Sie benötigen die Mandanten-ID, die Sie im Schritt [Abrufen der Mandanten-ID](#get-tenant-id) abgerufen haben.

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

5. Sie können das Profil speichern, wenn Sie das aktuelle Zugriffstoken in einer späteren Sitzung verwenden möchten.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Außerdem können Sie das Profil öffnen und seinen Inhalt untersuchen. Beachten Sie, dass es ein Zugriffstoken enthält.
        
6. Bei der nächsten gewünschten Ausführung von Code als Dienstprinzipal müssen Sie sich dann nicht mehr manuell anmelden, sondern nur noch das Profil laden.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Das Zugriffstoken läuft nach einer bestimmten Zeit ab. Die Verwendung eines gespeicherten Profils funktioniert also nur so lange, wie das Token gültig ist.
        
## Erstellen eines Dienstprinzipals mit Zertifikat

Dieser Abschnitt enthält die Schritte zum Erstellen einer AD-Anwendung und eines Dienstprinzipals mit einem Zertifikat.

1. Erstellen eines selbstsignierten Zertifikats Führen Sie bei Verwendung von **Windows 10 oder Windows Server 2016 Technical Preview** den folgenden Befehl aus:

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
     Ihre Variable enthält Informationen zum Zertifikat (einschließlich des Fingerabdrucks).
     
        Directory: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

        Thumbprint                                Subject
        ----------                                -------
        724213129BD2B950BB3F64FAB0C877E9348B16E9  CN=exampleapp

     Wenn Sie **nicht** Windows 10 oder Windows Server 2016 Technical Preview verwenden, steht das Cmdlet **New-SelfSignedCertificate** nicht zur Verfügung. Laden Sie stattdessen das PowerShell-Skript [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6) (Generator für selbst signierte Zertifikate) herunter, und führen Sie die folgenden Befehle aus, um ein Zertifikat zu generieren. Dieser Schritt ist nicht erforderlich, wenn Sie das Zertifikat bereits im vorherigen Beispiel erstellt haben.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\New-SelfSignedCertificateEx.ps1
        New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
        $cert = Get-ChildItem -Path cert:\CurrentUser\My* -DnsName exampleapp

2. Rufen Sie den Schlüsselwert aus dem Zertifikat ab.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

4. Erstellen Sie eine Anwendung im Verzeichnis.

        $azureAdApplication = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
        
    Untersuchen Sie das neue Anwendungsobjekt.

        $azureAdApplication

    Beachten Sie Folgendes: Die **ApplicationId**-Eigenschaft wird benötigt, um Dienstprinzipale und Rollenzuweisungen zu erstellen und Zugriffstoken abzurufen.

        DisplayName             : exampleapp
        Type                    : Application
        ApplicationId           : 1975a4fd-1528-4086-a992-787dbd23c46b
        ApplicationObjectId     : 9665e5f3-84b7-4344-92e2-8cc20ad16a8b
        AvailableToOtherTenants : False
        AppPermissions          : {}
        IdentifierUris          : {https://www.contoso.org/example}
        ReplyUrls               : {}    


5. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

6. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Geben Sie beim **ServicePrincipalName**-Parameter entweder die **ApplicationId** oder die **IdentifierUris** an, die Sie beim Erstellen der Anwendung verwendet haben. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md). Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Der nächste Abschnitt veranschaulicht die zertifikatbasierte Anmeldung über PowerShell.

### Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript

Geben Sie zum Authentifizieren in Ihrem Skript das Konto als Dienstprinzipal und den Zertifikatfingerabdruck, die Anwendungs-ID und die Mandanten-ID an. Diese Werte sind bereits in den Variablen **$azureAdApplication.ApplicationId**, **$cert.Thumbprint** und **$tenant** enthalten. Zur Automatisierung des Skripts können Sie die Werte als Umgebungsvariablen speichern und bei der Ausführung wieder abrufen oder sie in Ihr Skript einbinden.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint 000000 -ApplicationId 000000 -TenantId 0000000

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

Verwenden Sie bei Bedarf Folgendes, um die Anwendungs-ID später abzurufen:

    (Get-AzureRmADApplication -IdentifierUri "https://www.contoso.org/example").ApplicationId
        
Verwenden Sie bei Bedarf Folgendes, um den Zertifikatfingerabdruck später abzurufen:

    (Get-ChildItem -Path cert:\CurrentUser\My* -DnsName exampleapp).Thumbprint

Falls Sie die Mandanten-ID später abrufen müssen, finden Sie entsprechende Informationen unter [Abrufen der Mandanten-ID](#get-tenant-id).

## Beispielanwendungen

Die folgenden Beispielanwendungen veranschaulichen die Anmeldung als Dienstprinzipal:

**.NET**

- [Deploy an SSH Enabled VM with a Template with .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage mit .NET)
- [Manage Azure resources and resource groups with .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit .NET)

**Java**

- [Getting Started with Resources - Deploy Using ARM Template - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/) (Erste Schritte mit Ressourcen – Bereitstellen mithilfe einer Azure Resource Manager-Vorlage – in Java)
- [Getting Started with Resources - Manage Resource Group - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//) (Erste Schritte mit Ressourcen – Verwalten von Ressourcengruppen – in Java)

**Python**

- [Deploy an SSH Enabled VM with a Template in Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Python)
- [Manage Azure resources and resource groups with Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Python)

**Node.js**

- [Deploy an SSH Enabled VM with a Template in Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Node.js)
- [Manage Azure resources and resource groups with Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Node.js)

**Ruby**

- [Deploy an SSH Enabled VM with a Template in Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Ruby)
- [Manage Azure resources and resource groups with Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Ruby)

## Nächste Schritte
  
- Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

<!---HONumber=AcomDC_0824_2016-->