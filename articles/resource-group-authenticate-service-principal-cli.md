<properties
   pageTitle="Erstellen eines Dienstprinzipals mithilfe der Azure-Befehlszeilenschnittelle | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle eine Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
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
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wenn eine Ihrer Anwendungen oder eines Ihrer Skripts Zugriff auf Ressourcen benötigt, möchten Sie diesen Prozess wahrscheinlich nicht unter Ihren eigenen Anmeldeinformationen ausführen. Sie besitzen möglicherweise andere Berechtigungen, als sie für die Anwendung festlegen möchten, und die Anwendung soll Ihre Anmeldeinformationen nicht weiterhin nutzen, wenn sich Ihre Zuständigkeiten ändern. Stattdessen erstellen Sie für die Anwendung eine Identität, die Anmeldeinformationen für die Authentifizierung und Rollenzuweisungen enthält. Bei jeder Ausführung der App authentifiziert sie sich selbst mit diesen Anmeldeinformationen. In diesem Thema erfahren Sie, wie Sie mithilfe der [Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](xplat-cli-install.md) eine Anwendung einrichten, die unter eigenen Anmeldeinformationen und einer eigenen Identität ausgeführt wird.

Mit der Azure-Befehlszeilenschnittstelle stehen Ihnen für die Authentifizierung Ihrer AD-Anwendung zwei Optionen zur Verfügung:

 - password
 - Zertifikat

In diesem Thema wird die Verwendung beider Optionen in der Azure-CLI veranschaulicht. Wenn Sie sich über ein Programmierframework (wie Python, Ruby oder Node.js) bei Azure anmelden möchten, ist die Kennwortauthentifizierung wahrscheinlich die beste Option. Setzen Sie sich mit den Beispielen für die Authentifizierung in den unterschiedlichen Frameworks im Abschnitt [Beispielanwendungen](#sample-applications) auseinander, bevor Sie sich für ein Kennwort oder für ein Zertifikat entscheiden.

## Active Directory-Konzepte

In diesem Artikel erstellen Sie zwei Objekte: die AD-Anwendung (Active Directory) und den Dienstprinzipal. Die AD-Anwendung ist die globale Darstellung Ihrer Anwendung. Sie enthält die Anmeldeinformationen (eine Anwendungs-ID und ein Kennwort oder Zertifikat). Der Dienstprinzipal ist die lokale Darstellung Ihrer Anwendung in einer Active Directory-Instanz. Er enthält die Rollenzuweisung. Dieses Thema konzentriert sich auf eine Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet. In einer Anwendung mit nur einem Mandanten sind eine AD-App und ein Dienstprinzipal vorhanden.

Sie fragen sich vielleicht, warum Sie beide Objekte benötigen. Dieser Ansatz ergibt mehr Sinn, wenn Sie mehrinstanzenfähige Anwendungen betrachten. Mehrinstanzenfähige Anwendungen werden in der Regel für SaaS-Anwendungen (Software-as-a-Service) verwendet, bei denen Ihre Anwendung in zahlreichen verschiedenen Abonnements ausgeführt wird. Bei mehrinstanzenfähigen Anwendungen besitzen Sie eine AD-App und mehrere Prinzipale (einen in jeder Active Directory-Instanz, die Zugriff auf die App gewährt). Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

## Erforderliche Berechtigungen

Zum Abschließen dieses Themas benötigen Sie sowohl in der Azure Active Directory-Instanz als auch im Azure-Abonnement ausreichende Berechtigungen. Insbesondere müssen Sie eine App in der Active Directory-Instanz erstellen und den Dienstprinzipal einer Rolle zuweisen können.

In der Active Directory-Instanz muss Ihr Konto Administrator (beispielsweise **Globaler Administrator** oder **Benutzeradministrator**) sein. Falls Ihr Konto der Rolle **Benutzer** zugewiesen ist, muss ein Administrator Ihre Berechtigungen erhöhen.

In Ihrem Abonnement benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt. Falls Ihr Konto der Rolle **Mitwirkender** zugeordnet ist, wird ein Fehler angezeigt, wenn Sie versuchen, den Dienstprinzipal einer Rolle zuzuordnen. Auch in diesem Fall muss der Abonnementadministrator Ihnen entsprechende Zugriffsberechtigungen zuweisen.

Fahren Sie nun mit dem Abschnitt für eine [kennwortbasierte](#create-service-principal-with-password) oder [zertifikatbasierte](#create-service-principal-with-certificate) Authentifizierung fort.

## Erstellen eines Dienstprinzipals mit Kennwort

In diesem Abschnitt führen Sie folgende Schritte aus:

- Erstellen der AD-Anwendung mit einem Kennwort und dem Dienstprinzipal
- Zuweisen der Rolle „Leser“ zum Dienstprinzipal

Mit den folgenden Befehlen können diese Schritte schnell ausgeführt werden:

    azure ad sp create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>
    azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

Sehen wir uns diese Schritte genau an, um sicherzustellen, dass Sie den Prozess nachvollziehen können.

1. Melden Sie sich bei Ihrem Konto an.

        azure config mode arm
        azure login

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Geben Sie einen Anzeigenamen, den URI zu einer Seite mit einer Beschreibung der Anwendung, die URIs, von denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität an. Dieser Befehl erstellt sowohl die AD-Anwendung als auch den Dienstprinzipal.

        azure ad sp create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p {your-password}
        
     Bei Anwendungen mit nur einem Mandanten werden die URIs nicht überprüft.
     
     Falls Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) in der Active Directory-Instanz verfügt, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass die Authentifizierung nicht autorisiert ist oder kein Abonnement im Kontext gefunden wurde.
    
     Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt. Der Dienstprinzipalname wird bei der Anmeldung benötigt.
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle **Leser** hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie beim **ServicePrincipalName**-Parameter die **Objekt-ID** an, die Sie beim Erstellen der Anwendung verwendet haben.

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. In der Meldung ist angegeben, dass Ihr Konto **keine Berechtigung zum Ausführen der Aktion „Microsoft.Authorization/roleAssignments/write“ über Bereich „/subscriptions/{guid}“ hat**.

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Der nächste Abschnitt veranschaulicht die Anmeldung mit den Anmeldeinformationen über die Azure-CLI. Wenn Sie die Anmeldeinformationen in Ihrer Codeanwendung verwenden möchten, müssen Sie mit diesem Thema nicht fortfahren. In diesem Fall können Sie sich in den [Beispielanwendungen](#sample-applications) Beispiele für die Anmeldung mit Anwendungs-ID und Kennwort ansehen.

### Angeben von Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle

Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Verwenden Sie zum Abrufen der Mandanten-ID für Ihr derzeit authentifiziertes Abonnement Folgendes:

        azure account show

     Ausgabe des Befehls:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn Sie die Mandanten-ID eines anderen Abonnements abrufen möchten, verwenden Sie den folgenden Befehl:

        azure account show -s {subscription-id}

3. Melden Sie sich als Dienstprinzipal an.

        azure login -u https://www.contoso.org/example --service-principal --tenant {tenant-id}

    Sie werden aufgefordert, das Kennwort einzugeben. Geben Sie das Kennwort ein, das Sie beim Erstellen des Kontos angegeben haben.

        info:    Executing command login
        Password: ********

Sie sind nun als Dienstprinzipal für den von Ihnen erstellten Dienstprinzipal authentifiziert.

## Erstellen eines Dienstprinzipals mit Zertifikat

In diesem Abschnitt führen Sie folgende Schritte aus:

- Erstellen eines selbstsignierten Zertifikats
- Erstellen der AD-Anwendung mit dem Zertifikat und dem Dienstprinzipal
- Zuweisen der Rolle „Leser“ zum Dienstprinzipal

Für diese Schritte muss [OpenSSL](http://www.openssl.org/) installiert sein.

1. Erstellen eines selbstsignierten Zertifikats

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinieren Sie die öffentlichen und privaten Schlüssel.

        cat privkey.pem cert.pem > examplecert.pem

3. Öffnen Sie die Datei **examplecert.pem**, und suchen Sie nach der langen Zeichenfolge zwischen **-----BEGIN CERTIFICATE-----** und **-----END CERTIFICATE-----**. Kopieren Sie die Zertifikatdaten. Beim Erstellen des Dienstprinzipals übergeben Sie diese Daten als Parameter.

1. Melden Sie sich bei Ihrem Konto an.

        azure config mode arm
        azure login

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Geben Sie einen Anzeigenamen, den URI zu einer Seite mit einer Beschreibung der Anwendung, die URIs, von denen die Anwendung identifiziert wird, und die kopierten Zertifikatdaten an. Dieser Befehl erstellt sowohl die AD-Anwendung als auch den Dienstprinzipal.

        azure ad sp create -n "exampleapp" --home-page "https://www.contoso.org" -i "https://www.contoso.org/example" --key-value <certificate data>
        
     Bei Anwendungen mit nur einem Mandanten werden die URIs nicht überprüft.
     
     Falls Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) in der Active Directory-Instanz verfügt, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass die Authentifizierung nicht autorisiert ist oder kein Abonnement im Kontext gefunden wurde.
    
     Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt.
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle **Leser** hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Geben Sie für den **ServicePrincipalName**-Parameter die **Objekt-ID** an, die Sie beim Erstellen der Anwendung verwendet haben.

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. In der Meldung ist angegeben, dass Ihr Konto **keine Berechtigung zum Ausführen der Aktion „Microsoft.Authorization/roleAssignments/write“ über Bereich „/subscriptions/{guid}“ hat**.

### Bereitstellen eines Zertifikats über automatisiertes Azure-CLI-Skript

Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Verwenden Sie zum Abrufen der Mandanten-ID für Ihr derzeit authentifiziertes Abonnement Folgendes:

        azure account show

     Ausgabe des Befehls:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn Sie die Mandanten-ID eines anderen Abonnements abrufen möchten, verwenden Sie den folgenden Befehl:

        azure account show -s {subscription-id}

1. Verwenden Sie Folgendes, um den Zertifikatfingerabdruck abzurufen und nicht benötigte Zeichen zu entfernen:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Ein Fingerabdruck wie der folgende wird zurückgegeben:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

1. Melden Sie sich als Dienstprinzipal an.

        azure login --service-principal --tenant {tenant-id} -u https://www.contoso.org/example --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## Beispielanwendungen

Die folgenden Beispielanwendungen veranschaulichen die Anmeldung als Dienstprinzipal:

**.NET**

- [Deploy an SSH Enabled VM with a Template with .NET (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Manage Azure resources and resource groups with .NET (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit .NET)](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Getting Started with Resources - Deploy Using ARM Template - in Java (Erste Schritte mit Ressourcen – Bereitstellen mithilfe einer Azure Resource Manager-Vorlage – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Getting Started with Resources - Manage Resource Group - in Java (Erste Schritte mit Ressourcen – Verwalten von Ressourcengruppen – in Java)](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Deploy an SSH Enabled VM with a Template in Python (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Manage Azure resources and resource groups with Python (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Python)](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Deploy an SSH Enabled VM with a Template in Node.js (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Manage Azure resources and resource groups with Node.js (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Node.js)](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Deploy an SSH Enabled VM with a Template in Ruby (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Manage Azure resources and resource groups with Ruby (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Ruby)](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## Nächste Schritte
  
- Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
- Weitere Informationen zur Verwendung von Zertifikaten und der Azure-Befehlszeilenschnittstelle finden Sie unter [Certificate-based auth with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx) (Zertifikatbasierte Authentifizierung mit Azure-Dienstprinzipalen über die Linux-Befehlszeile).

<!---HONumber=AcomDC_0914_2016-->