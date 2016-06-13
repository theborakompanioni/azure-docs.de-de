<properties
   pageTitle="Erstellen einer AD-Anwendung mit PowerShell | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Azure PowerShell zum Erstellen einer Active Directory-Anwendung verwenden und dafür Zugriff auf Ressourcen per rollenbasierter Zugriffssteuerung gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
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
   ms.date="05/26/2016"
   ms.author="tomfitz"/>

# Verwenden von Azure PowerShell zum Erstellen einer Active Directory-Anwendung zum Zugreifen auf Ressourcen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


In diesem Thema wird gezeigt, wie Sie [Azure PowerShell](powershell-install-configure.md) zum Erstellen einer Active Directory-Anwendung (AD) verwenden. Beispiele hierfür sind ein automatisierter Prozess, eine Anwendung oder ein Dienst mit Zugriff auf andere Ressourcen Ihres Abonnements. Unter Azure Resource Manager können Sie die rollenbasierte Zugriffssteuerung nutzen, um einer Anwendung die Durchführung zulässiger Aktionen zu gewähren.

In diesem Artikel erstellen Sie zwei Objekte: die AD-Anwendung und den Dienstprinzipal. Die AD-Anwendung befindet sich unter dem Mandanten, für den die App registriert ist, und definiert den auszuführenden Prozess. Der Dienstprinzipal enthält die Identität der AD-Anwendung und wird zum Zuweisen von Berechtigungen verwendet. Über die AD-Anwendung können Sie viele Dienstprinzipale erstellen. Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Es gibt zwei Möglichkeiten, wie Sie die Anwendung authentifizieren können:

 - Kennwort: Ist geeignet, wenn sich ein Benutzer während der Ausführung interaktiv anmelden möchte.
 - Zertifikat: Ist für unbeaufsichtigte Skripts geeignet, bei denen ohne Benutzerinteraktion authentifiziert werden muss.

## Erstellen einer AD-Anwendung mit Kennwort

In diesem Abschnitt führen Sie die Schritte zum Erstellen der AD-Anwendung mit einem Kennwort aus.

1. Melden Sie sich bei Ihrem Konto an.

        Add-AzureRmAccount

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


### Erstellen des Dienstprinzipals und Zuweisen zur Rolle

Über die AD-Anwendung müssen Sie einen Dienstprinzipal erstellen und ihm eine Rolle zuweisen.

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Geben Sie beim **ServicePrincipalName**-Parameter entweder die **ApplicationId** oder die **IdentifierUris** an, die Sie beim Erstellen der Anwendung verwendet haben. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md).

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

### Manuelles Bereitstellen von Anmeldeinformationen über PowerShell

Sie haben eine Active Directory-Anwendung und einen Dienstprinzipal für diese Anwendung erstellt. Sie haben den Dienstprinzipal einer Rolle zugewiesen. Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen. Sie können die Anmeldeinformationen für die Anwendung beim Ausführen bedarfsgesteuerter Skripts oder Befehle manuell bereitstellen.

1. Erstellen Sie ein neues **PSCredential**-Objekt, das Ihre Anmeldeinformationen enthält, indem Sie den Befehl **Get-Credential** ausführen.

        $creds = Get-Credential

2. Sie werden zum Eingeben Ihrer Anmeldeinformationen aufgefordert. Verwenden Sie als Benutzernamen die **ApplicationId** oder die **IdentifierUris**, die Sie beim Erstellen der Anwendung verwendet haben. Verwenden Sie das Kennwort, das Sie beim Erstellen des Kontos angegeben haben.

     ![Anmeldeinformationen eingeben](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

3. Rufen Sie das Abonnement ab, in dem die Rollenzuweisung erstellt wurde. Dieses Abonnement wird verwendet, um die **TenantId** des Mandanten abzurufen, in dem sich die Rollenzuweisung des Dienstprinzipals befindet.

        $subscription = Get-AzureRmSubscription

     Wenn Ihr Konto mit mehr als einem Abonnement verknüpft ist, müssen Sie einen Abonnementnamen oder eine ID angeben, um das gewünschte Abonnement auszuwählen.
     
        $subscription = Get-AzureRmSubscription -SubscriptionName "Azure MSDN - Visual Studio Ultimate"

4. Melden Sie sich als Dienstprinzipal an, indem Sie angeben, dass dieses Konto ein Dienstprinzipal ist, und das Anmeldeinformationsobjekt bereitstellen.

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $subscription.TenantId
        
     Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

5. Sie können das Profil speichern, wenn Sie das aktuelle Zugriffstoken in einer späteren Sitzung verwenden möchten.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Außerdem können Sie das Profil öffnen und seinen Inhalt untersuchen. Beachten Sie, dass es ein Zugriffstoken enthält.
        
6. Bei der nächsten gewünschten Ausführung von Code als Dienstprinzipal müssen Sie sich dann nicht mehr anmelden, sondern nur noch das Profil laden.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Das Zugriffstoken läuft nach einer bestimmten Zeit ab. Die Verwendung eines gespeicherten Profils funktioniert also nur so lange, wie das Token gültig ist. Verwenden Sie ein Zertifikat, um unbeaufsichtigte Skripts dauerhaft auszuführen.
        
## Erstellen einer AD-Anwendung mit einem Zertifikat

In diesem Abschnitt führen Sie die Schritte zum Erstellen einer AD-Anwendung mit einem Zertifikat aus.

1. Erstellen eines selbstsignierten Zertifikats

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
     Sie erhalten die Informationen zum Zertifikat, einschließlich des Fingerabdrucks.
     
        Directory: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

        Thumbprint                                Subject
        ----------                                -------
        724213129BD2B950BB3F64FAB0C877E9348B16E9  CN=exampleapp

2. Rufen Sie den Schlüsselwert aus dem Zertifikat ab.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

3. Melden Sie sich beim Azure-Konto an.

        Add-AzureRmAccount

4. Erstellen Sie eine Anwendung im Verzeichnis.

        $azureAdApplication = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
        
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


### Erstellen des Dienstprinzipals und Zuweisen zur Rolle

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Geben Sie beim **ServicePrincipalName**-Parameter entweder die **ApplicationId** oder die **IdentifierUris** an, die Sie beim Erstellen der Anwendung verwendet haben. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md).

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

### Vorbereiten von Werten für Ihr Skript

In Ihrem Skript übergeben Sie drei Werte, die für die Anmeldung als Dienstprinzipal benötigt werden. Was Sie benötigen:

- Anwendungs-ID
- Mandanten-ID 
- Zertifikatfingerabdruck

Sie haben die Anwendungs-ID und den Zertifikatfingerabdruck bereits in den vorherigen Schritten gesehen. Wenn Sie diese Werte später abrufen müssen, werden die Befehle unten angezeigt (zusammen mit dem Befehl zum Abrufen der Mandanten-ID).

1. Verwenden Sie zum Abrufen der Mandanten-ID Folgendes:

        (Get-AzureRmSubscription).TenantId 

    Oder geben Sie den Namen des Abonnements an, wenn Sie über mehr als ein Abonnement verfügen:

        (Get-AzureRmSubscription -SubscriptionName "Azure MSDN - Visual Studio Ultimate").TenantId
        
2. Verwenden Sie zum Abrufen der Anwendungs-ID Folgendes:

        (Get-AzureRmADApplication -IdentifierUri "https://www.contoso.org/example").ApplicationId
        
3. Verwenden Sie zum Abrufen des Zertifikatfingerabdrucks Folgendes:

        (Get-ChildItem -Path cert:\CurrentUser\My* -DnsName exampleapp).Thumbprint

### Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript

Sie haben eine Active Directory-Anwendung und einen Dienstprinzipal für diese Anwendung erstellt. Sie haben den Dienstprinzipal einer Rolle zugewiesen. Nun müssen Sie sich als Dienstprinzipal anmelden, um Aufgaben als Dienstprinzipal zu erledigen.

Geben Sie zum Authentifizieren in Ihrem Skript das Konto als Dienstprinzipal und den Zertifikatfingerabdruck, die Anwendungs-ID und die Mandanten-ID an.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint {thumbprint} -ApplicationId {applicationId} -TenantId {tenantid}

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## Nächste Schritte
  
- Beispiele für die .NET-Authentifizierung finden Sie unter [Azure Resource Manager SDK für .NET](resource-manager-net-sdk.md).
- Beispiele für die Java-Authentifizierung finden Sie unter [Azure Resource Manager SDK für Java](resource-manager-java-sdk.md). 
- Beispiele für die Python-Authentifizierung finden Sie unter [Resource Management Authentication](https://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementauthentication.html) (Ressourcenverwaltungsauthentifizierung) für Python.
- Beispiele für die REST-Authentifizierung finden Sie unter [Resource Manager-REST-APIs](resource-manager-rest-api.md).
- Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

<!---HONumber=AcomDC_0601_2016-->