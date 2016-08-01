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
   ms.date="07/19/2016"
   ms.author="tomfitz"/>

# Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wenn eine Ihrer Anwendungen oder eines Ihrer Skripts Zugriff auf Ressourcen benötigt, möchten Sie diesen Prozess wahrscheinlich nicht unter den Anmeldeinformationen eines Benutzers ausführen. Der Benutzer hat unter Umständen andere Berechtigungen als Sie dem Prozess zuweisen möchten, und die Aufgaben des Benutzers können sich unter Umständen ändern. Stattdessen können Sie für die Anwendung eine Identität erstellen, die Anmeldeinformationen für die Authentifizierung und Rollenzuweisungen enthält. Die Anwendung meldet sich bei jeder Ausführung mit dieser Identität an. In diesem Thema erfahren Sie, wie Sie mithilfe der [Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](xplat-cli-install.md) alles einrichten, was Sie benötigen, um eine Anwendung unter eigenen Anmeldeinformationen und einer eigenen Identität auszuführen.

Dazu erstellen Sie in diesem Artikel zwei Objekte: die AD-Anwendung (Active Directory) und den Dienstprinzipal. Die AD-Anwendung enthält die Anmeldeinformationen (eine Anwendungs-ID und ein Kennwort oder Zertifikat). Der Dienstprinzipal enthält die Rollenzuweisung. Über die AD-Anwendung können Sie viele Dienstprinzipale erstellen. Dieses Thema konzentriert sich auf eine Anwendung mit nur einem Mandanten, wobei die Anwendung nur zur Ausführung in einer einzelnen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet. Falls Ihre Anwendung in mehreren Organisationen ausführbar sein muss, können Sie auch Anwendungen mit mehreren Mandanten erstellen. Anwendungen mit mehreren Mandanten werden üblicherweise für SaaS-Anwendungen (Software-as-a-Service) verwendet. Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

Für die Verwendung von Active Directory müssen Sie mit zahlreichen Konzepten vertraut sein. Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Mit der Azure-Befehlszeilenschnittstelle stehen Ihnen für die Authentifizierung Ihrer AD-Anwendung zwei Optionen zur Verfügung:

 - Kennwort
 - Zertifikat

Wenn Sie sich nach dem Einrichten Ihrer AD-Anwendung über ein anderes Programmierframework (wie Python, Ruby oder Node.js) bei Azure anmelden möchten, ist die Kennwortauthentifizierung wahrscheinlich die beste Option. Setzen Sie sich mit den Beispielen für die Authentifizierung in den unterschiedlichen Frameworks im Abschnitt [Beispielanwendungen](#sample-applications) auseinander, bevor Sie sich für ein Kennwort oder für ein Zertifikat entscheiden.

## Abrufen der Mandanten-ID

Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Da Sie diesen Wert sowohl für die kennwortbasierte als auch für die zertifikatbasierte Authentifizierung benötigen, rufen wir diesen Wert nun ab.

1. Melden Sie sich bei Ihrem Konto an.

        azure config mode arm
        azure login

1. Wenn Sie die Mandanten-ID für das derzeit authentifizierte Abonnement abrufen, müssen Sie nicht die Abonnement-ID als Parameter angeben. Mit dem Switch **-r** wird der Wert ohne Anführungszeichen abgerufen.

        tenantId=$(azure account show -s <subscriptionId> --json | jq -r '.[0].tenantId')

Fahren Sie nun mit einem der folgenden Abschnitte für eine [kennwortbasierte](#create-service-principal-with-password) oder [zertifikatbasierte](#create-service-principal-with-certificate) Authentifizierung fort.


## Erstellen eines Dienstprinzipals mit Kennwort

Dieser Abschnitt enthält die Schritte zum Erstellen eines Dienstprinzipals mit einem Kennwort und zum Zuweisen einer Rolle.

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Geben Sie einen Anzeigenamen für die Anwendung an, außerdem die URI zu einer Seite mit einer Beschreibung der Anwendung (der Link wird nicht geprüft), die URIs, von denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität. Dieser Befehl erstellt sowohl die AD-Anwendung als auch den Dienstprinzipal.

        azure ad sp create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>
        
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

2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md). Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt.

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Im nächsten Abschnitt erfahren Sie, wie Sie sich mit den Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle anmelden. Wenn Sie die Anmeldeinformationen dagegen in Ihrer Codeanwendung verwenden möchten, müssen in diesem Thema nicht weiterlesen. In diesem Fall können Sie sich in den [Beispielanwendungen](#sample-applications) Beispiele für die Anmeldung mit Anwendungs-ID und Kennwort ansehen.

### Angeben von Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle

1. Verwenden Sie als Benutzername den Dienstprinzipalnamen, der beim Erstellen des Dienstprinzipals zurückgegeben wurde. Wenn Sie die Anwendungs-ID abrufen müssen, führen Sie den folgenden Befehl aus. Geben Sie den Namen der Active Directory-Anwendung im **search**-Parameter an.

        appId=$(azure ad app show --search exampleapp --json | jq -r '.[0].appId')

3. Melden Sie sich als Dienstprinzipal an.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant "$tenantId"

    Sie werden aufgefordert, das Kennwort einzugeben. Geben Sie das Kennwort ein, das Sie beim Erstellen des Kontos angegeben haben.

        info:    Executing command login
        Password: ********

Sie sind nun als Dienstprinzipal für den von Ihnen erstellten Dienstprinzipal authentifiziert.

## Erstellen eines Dienstprinzipals mit Zertifikat

Dieser Abschnitt enthält die Schritte zum Erstellen eines Dienstprinzipals für einen Dienstprinzipal mit Zertifikatauthentifizierung. Für diese Schritte muss [OpenSSL](http://www.openssl.org/) installiert sein.

1. Erstellen eines selbstsignierten Zertifikats

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinieren Sie die öffentlichen und privaten Schlüssel.

        cat privkey.pem cert.pem > examplecert.pem

3. Öffnen Sie die Datei **examplecert.pem**, und kopieren Sie die Zertifikatdaten. Suchen Sie nach der langen Zeichenfolge zwischen **-----BEGIN CERTIFICATE-----** und **-----END CERTIFICATE-----**.

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Stellen Sie die Anwendungs-ID bereit, die im vorherigen Schritt zurückgegeben wurde.

        azure ad sp create -n "exampleapp" --home-page "https://www.contoso.org" -i "https://www.contoso.org/example" --key-value <certificate data>
        
    Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt.
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md). Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt.

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

### Bereitstellen eines Zertifikats über automatisiertes Azure-CLI-Skript

Geben Sie für die Authentifizierung mit Azure-CLI den Zertifikatfingerabdruck, die Zertifikatdatei, Anwendungs-ID und Mandanten-ID an.

    azure login --service-principal --tenant 00000 -u 000000 --certificate-file C:\certificates\examplecert.pem --thumbprint 000000

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

Verwenden Sie bei Bedarf Folgendes, um den Zertifikatfingerabdruck abzurufen und nicht benötigte Zeichen zu entfernen:

    openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
Ein Fingerabdruck wie der folgende wird zurückgegeben:

    30996D9CE48A0B6E0CD49DBB9A48059BF9355851

Verwenden Sie bei Bedarf Folgendes, um die Anwendungs-ID abzurufen:

    azure ad app show --search exampleapp --json | jq -r '.[0].appId'

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
  
- Im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md) erfahren Sie in ausführlichen Schritten, wie Sie eine Anwendung zur Verwaltung von Ressourcen in Azure integrieren.
- Weitere Informationen zur Verwendung von Zertifikaten und der Azure-Befehlszeilenschnittstelle finden Sie unter [Certificate-based auth with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx) (Zertifikatbasierte Authentifizierung mit Azure-Dienstprinzipalen über die Linux-Befehlszeile).

<!---HONumber=AcomDC_0720_2016-->