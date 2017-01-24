---
title: Erstellen eines Azure-Dienstprinzipals mithilfe von PowerShell | Microsoft Docs
description: "Hier erfahren Sie, wie Sie mithilfe von Azure PowerShell eine Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/12/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 109ca4a4672d21969096af26a094390673de25d9
ms.openlocfilehash: 5b7c701cc7bd3c4ad586f2e7407fa45f102771d7


---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-authenticate-service-principal.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
> * [Portal](resource-group-create-service-principal-portal.md)
> 
> 

Wenn eine Ihrer Anwendungen oder eines Ihrer Skripts Zugriff auf Ressourcen benötigt, möchten Sie diesen Prozess wahrscheinlich nicht unter Ihren eigenen Anmeldeinformationen ausführen. Sie besitzen möglicherweise andere Berechtigungen, als sie für die Anwendung festlegen möchten, und die Anwendung soll Ihre Anmeldeinformationen nicht weiterhin nutzen, wenn sich Ihre Zuständigkeiten ändern. Stattdessen erstellen Sie für die Anwendung eine Identität, die Anmeldeinformationen für die Authentifizierung und Rollenzuweisungen enthält. Bei jeder Ausführung der App authentifiziert sie sich selbst mit diesen Anmeldeinformationen. In diesem Thema erfahren Sie, wie Sie mithilfe von [Azure PowerShell](/powershell/azureps-cmdlets-docs) alle Komponenten und Einstellungen einrichten, die Sie benötigen, um eine Anwendung mit eigenen Anmeldeinformationen und einer eigenen Identität auszuführen.

Mit PowerShell stehen Ihnen für die Authentifizierung Ihrer AD-Anwendung zwei Optionen zur Verfügung:

* password
* Zertifikat

In diesem Thema wird die Verwendung beider Optionen in PowerShell veranschaulicht. Wenn Sie sich über ein Programmierframework (wie Python, Ruby oder Node.js) bei Azure anmelden möchten, ist die Kennwortauthentifizierung wahrscheinlich die beste Option. Setzen Sie sich mit den Beispielen für die Authentifizierung in den unterschiedlichen Frameworks im Abschnitt [Beispielanwendungen](#sample-applications) auseinander, bevor Sie sich für ein Kennwort oder für ein Zertifikat entscheiden.

## <a name="active-directory-concepts"></a>Active Directory-Konzepte
In diesem Artikel erstellen Sie zwei Objekte: die AD-Anwendung (Active Directory) und den Dienstprinzipal. Die AD-Anwendung ist die globale Darstellung Ihrer Anwendung. Sie enthält die Anmeldeinformationen (eine Anwendungs-ID und ein Kennwort oder Zertifikat). Der Dienstprinzipal ist die lokale Darstellung Ihrer Anwendung in einer Active Directory-Instanz. Er enthält die Rollenzuweisung. Dieses Thema konzentriert sich auf eine Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet. In einer Anwendung mit nur einem Mandanten sind eine AD-App und ein Dienstprinzipal vorhanden.

Sie fragen sich vielleicht, warum Sie beide Objekte benötigen. Dieser Ansatz ergibt mehr Sinn, wenn Sie mehrinstanzenfähige Anwendungen betrachten. Mehrinstanzenfähige Anwendungen werden in der Regel für SaaS-Anwendungen (Software-as-a-Service) verwendet, bei denen Ihre Anwendung in zahlreichen verschiedenen Abonnements ausgeführt wird. Bei mehrinstanzenfähigen Anwendungen besitzen Sie eine AD-App und mehrere Prinzipale (einen in jeder Active Directory-Instanz, die Zugriff auf die App gewährt). Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Zum Abschließen dieses Themas benötigen Sie sowohl in der Azure Active Directory-Instanz als auch im Azure-Abonnement ausreichende Berechtigungen. Insbesondere müssen Sie eine App in der Active Directory-Instanz erstellen und den Dienstprinzipal einer Rolle zuweisen können. 

In der Active Directory-Instanz muss Ihr Konto Administrator (beispielsweise **Globaler Administrator** oder **Benutzeradministrator**) sein. Falls Ihr Konto der Rolle **Benutzer** zugewiesen ist, muss ein Administrator Ihre Berechtigungen erhöhen.

In Ihrem Abonnement benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt. Falls Ihr Konto der Rolle **Mitwirkender** zugeordnet ist, wird ein Fehler angezeigt, wenn Sie versuchen, den Dienstprinzipal einer Rolle zuzuordnen. Auch in diesem Fall muss der Abonnementadministrator Ihnen entsprechende Zugriffsberechtigungen zuweisen.

Fahren Sie nun mit dem Abschnitt für eine [kennwortbasierte](#create-service-principal-with-password) oder [zertifikatbasierte](#create-service-principal-with-certificate) Authentifizierung fort.

## <a name="create-service-principal-with-password"></a>Erstellen eines Dienstprinzipals mit Kennwort
In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen der AD-Anwendung mit einem Kennwort
* Erstellen des AD-Dienstprinzipals
* Zuweisen der Rolle „Leser“ zum Dienstprinzipal

Mit den folgenden drei Cmdlets können diese Schritte schnell ausgeführt werden: 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Sehen wir uns diese Schritte genau an, um sicherzustellen, dass Sie den Prozess nachvollziehen können.

1. Melden Sie sich bei Ihrem Konto an.
   
        Add-AzureRmAccount
2. Erstellen Sie wie folgt eine neue Active Directory-Anwendung: Geben Sie einen Anzeigenamen, den URI mit einer Beschreibung der Anwendung, die URIs, mit denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität an.
   
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"
   
     Bei Anwendungen mit nur einem Mandanten werden die URIs nicht überprüft.
   
     Falls Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) für die Active Directory-Instanz verfügt, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass die Authentifizierung nicht autorisiert ist oder kein Abonnement im Kontext gefunden wurde.
3. Untersuchen Sie das neue Anwendungsobjekt. 
   
        $app
   
     Beachten Sie vor allem die **ApplicationId** -Eigenschaft, die benötigt wird, um Dienstprinzipale und Rollenzuweisungen zu erstellen und das Zugriffstoken abzurufen.
   
        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}
4. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.
   
        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
5. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle **Leser** hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Geben Sie für den **ServicePrincipalName**-Parameter die **ApplicationId** an, die Sie beim Erstellen der Anwendung verwendet haben. 
   
        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid
   
    Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. In der Meldung ist angegeben, dass Ihr Konto **keine Berechtigung zum Ausführen der Aktion „Microsoft.Authorization/roleAssignments/write“ über Bereich „/subscriptions/{guid}“ hat**. 

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Der nächste Abschnitt veranschaulicht die Anmeldung mit den Anmeldeinformationen über PowerShell. Wenn Sie die Anmeldeinformationen in der Codeanwendung verwenden möchten, können Sie zu den [Beispielanwendungen](#sample-applications)wechseln. 

### <a name="provide-credentials-through-powershell"></a>Bereitstellen von Anmeldeinformationen über PowerShell
Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Erstellen Sie ein **PSCredential**-Objekt, das Ihre Anmeldeinformationen enthält, indem Sie den Befehl **Get-Credential** ausführen. Vor dem Ausführen dieses Befehls benötigen Sie die **ApplicationId**-Eigenschaft. Sorgen Sie daher dafür, dass die entsprechende Angabe zum Einfügen zur Verfügung steht.
   
        $creds = Get-Credential
2. Sie werden zum Eingeben Ihrer Anmeldeinformationen aufgefordert. Verwenden Sie als Benutzernamen die **ApplicationId** , die Sie beim Erstellen der Anwendung verwendet haben. Verwenden Sie das Kennwort, das Sie beim Erstellen des Kontos angegeben haben.
   
     ![Anmeldeinformationen eingeben](./media/resource-group-authenticate-service-principal/arm-get-credential.png)
3. Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Wenn Sie nur über ein einziges Abonnement verfügen, können Sie Folgendes verwenden:
   
        $tenant = (Get-AzureRmSubscription).TenantId
   
     Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement an, in dem sich Ihre Active Directory-Instanz befindet. Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).
   
        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
4. Melden Sie sich als Dienstprinzipal an, indem Sie angeben, dass dieses Konto ein Dienstprinzipal ist, und das Anmeldeinformationsobjekt bereitstellen. 
   
        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
   
     Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

### <a name="save-access-token-to-simplify-log-in"></a>Speichern von Zugriffstoken zum Vereinfachen der Anmeldung
Damit Sie die Anmeldeinformationen für den Dienstprinzipal nicht bei jeder Anmeldung angeben müssen, können Sie das Zugriffstoken speichern.

1. Speichern Sie das Profil, wenn Sie das aktuelle Zugriffstoken in einer späteren Sitzung verwenden möchten.
   
        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
   
     Öffnen Sie das Profil, und untersuchen Sie seinen Inhalt. Beachten Sie, dass es ein Zugriffstoken enthält. 
2. Laden Sie einfach das Profil, statt sich manuell anzumelden.
   
        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json

> [!NOTE]
> Das Zugriffstoken läuft nach einer bestimmten Zeit ab. Die Verwendung eines gespeicherten Profils funktioniert also nur so lange, wie das Token gültig ist.
> 
> 

## <a name="create-service-principal-with-certificate"></a>Erstellen eines Dienstprinzipals mit Zertifikat
In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen eines selbstsignierten Zertifikats
* Erstellen der AD-Anwendung mit dem Zertifikat
* Erstellen des AD-Dienstprinzipals
* Zuweisen der Rolle „Leser“ zum Dienstprinzipal

Mithilfe der folgenden Cmdlets können Sie diese Schritte schnell mit Azure PowerShell 2.0 unter Windows 10 oder Windows Server 2016 Technical Preview ausführen. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Sehen wir uns diese Schritte genau an, um sicherzustellen, dass Sie den Prozess nachvollziehen können. In diesem Artikel wird außerdem gezeigt, wie Sie die Aufgaben mit älteren Azure PowerShell- oder Betriebssystemversionen ausführen.

### <a name="create-the-self-signed-certificate"></a>Erstellen des selbstsignierten Zertifikats
Die mit Windows 10 und Windows Server 2016 Technical Preview verfügbare PowerShell-Version enthält ein aktualisiertes **New-SelfSignedCertificate** -Cmdlet für die Erstellung eines selbstsignierten Zertifikats. Ältere Betriebssysteme enthalten das Cmdlet „New-SelfSignedCertificate“, es bietet jedoch nicht die für dieses Thema erforderlichen Parameter. Stattdessen müssen Sie ein Modul zum Generieren des Zertifikats importieren. Dieses Thema zeigt beide Ansätze zum Generieren des Zertifikats basierend auf dem verwendeten Betriebssystem. 

* Führen Sie bei Verwendung von **Windows 10 oder Windows Server 2016 Technical Preview**den folgenden Befehl aus, um ein selbstsigniertes Zertifikat zu erstellen: 
  
        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
* Wenn Sie **nicht mit Windows 10 oder Windows Server 2016 Technical Preview arbeiten**, müssen Sie den [Generator für selbstsignierte Zertifikate](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) aus dem Skriptcenter von Microsoft herunterladen. Extrahieren Sie den Inhalt, und importieren Sie das benötigte Cmdlet.
  
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
     Generieren Sie anschließend das Zertifikat.
  
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Sie haben nun Ihr Zertifikat und können mit dem Erstellen der AD-App fortfahren.

### <a name="create-the-active-directory-app-and-service-principal"></a>Erstellen der Active Directory-App und des Dienstprinzipals
1. Rufen Sie den Schlüsselwert aus dem Zertifikat ab.
   
        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
2. Melden Sie sich beim Azure-Konto an.
   
        Add-AzureRmAccount
3. Erstellen Sie wie folgt eine neue Active Directory-Anwendung: Geben Sie einen Anzeigenamen, den URI mit einer Beschreibung der Anwendung, die URIs, mit denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität an.
   
     Verwenden Sie bei Azure PowerShell 2.0 (August 2016 oder höher) das folgende Cmdlet:
   
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
   
    Verwenden Sie bei Azure PowerShell 1.0 das folgende Cmdlet:
   
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
   
    Bei Anwendungen mit nur einem Mandanten werden die URIs nicht überprüft.
   
    Falls Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) für die Active Directory-Instanz verfügt, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass die Authentifizierung nicht autorisiert ist oder kein Abonnement im Kontext gefunden wurde.
   
    Untersuchen Sie das neue Anwendungsobjekt. 
   
        $app
   
    Beachten Sie Folgendes: Die **ApplicationId** -Eigenschaft wird benötigt, um Dienstprinzipale und Rollenzuweisungen zu erstellen und Zugriffstoken abzurufen.
   
        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}
4. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung, indem Sie die Anwendungs-ID der Active Directory-Anwendung übergeben.
   
        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
5. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle **Leser** hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Geben Sie für den **ServicePrincipalName**-Parameter die **ApplicationId** an, die Sie beim Erstellen der Anwendung verwendet haben.
   
        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid
   
    Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. In der Meldung ist angegeben, dass Ihr Konto **keine Berechtigung zum Ausführen der Aktion „Microsoft.Authorization/roleAssignments/write“ über Bereich „/subscriptions/{guid}“ hat**.

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Der nächste Abschnitt veranschaulicht die zertifikatbasierte Anmeldung über PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript
Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Wenn Sie nur über ein einziges Abonnement verfügen, können Sie Folgendes verwenden:

    $tenant = (Get-AzureRmSubscription).TenantId

Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement an, in dem sich Ihre Active Directory-Instanz befindet. Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](../active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Geben Sie zum Authentifizieren in Ihrem Skript das Konto als Dienstprinzipal und den Zertifikatfingerabdruck, die Anwendungs-ID und die Mandanten-ID an. Zur Automatisierung des Skripts können Sie die Werte als Umgebungsvariablen speichern und bei der Ausführung wieder abrufen oder sie in Ihr Skript einbinden.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## <a name="sample-applications"></a>Beispielanwendungen
Die folgenden Beispielanwendungen veranschaulichen die Anmeldung als Dienstprinzipal:

**.NET**

* [Deploy an SSH Enabled VM with a Template with .NET (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
* [Manage Azure resources and resource groups with .NET (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

* [Getting Started with Resources - Deploy Using ARM Template - in Java (Erste Schritte mit Ressourcen – Bereitstellen mithilfe einer Azure Resource Manager-Vorlage – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
* [Getting Started with Resources - Manage Resource Group - in Java (Erste Schritte mit Ressourcen – Verwalten von Ressourcengruppen – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

* [Deploy an SSH Enabled VM with a Template in Python (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
* [Manage Azure resources and resource groups with Python (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

* [Deploy an SSH Enabled VM with a Template in Node.js (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
* [Manage Azure resources and resource groups with Node.js (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

* [Deploy an SSH Enabled VM with a Template in Ruby (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
* [Manage Azure resources and resource groups with Ruby (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
* Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md). 
* Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](../active-directory/active-directory-authentication-scenarios.md).




<!--HONumber=Dec16_HO2-->


