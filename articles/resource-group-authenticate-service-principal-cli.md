<properties
   pageTitle="Erstellen einer AD-Anwendung mit der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
   description="Es wird beschrieben, wie Sie die Azure-Befehlszeilenschnittstelle zum Erstellen einer Active Directory-Anwendung verwenden und dafür Zugriff auf Ressourcen per rollenbasierter Zugriffssteuerung gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
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

# Verwenden der Azure-Befehlszeilenschnittstelle zum Erstellen einer Active Directory-Anwendung für den Zugriff auf Ressourcen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

In diesem Thema wird gezeigt, wie Sie die [Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](xplat-cli-install.md) zum Erstellen einer Active Directory-Anwendung (AD) verwenden. Beispiele hierfür sind ein automatisierter Prozess, eine Anwendung oder ein Dienst mit Zugriff auf andere Ressourcen Ihres Abonnements. Unter Azure Resource Manager können Sie die rollenbasierte Zugriffssteuerung nutzen, um einer Anwendung die Durchführung zulässiger Aktionen zu gewähren.

In diesem Artikel erstellen Sie zwei Objekte: die AD-Anwendung und den Dienstprinzipal. Die AD-Anwendung befindet sich unter dem Mandanten, für den die App registriert ist, und definiert den auszuführenden Prozess. Der Dienstprinzipal enthält die Identität der AD-Anwendung und wird zum Zuweisen von Berechtigungen verwendet. Über die AD-Anwendung können Sie viele Dienstprinzipale erstellen. Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Es gibt zwei Möglichkeiten, wie Sie die Anwendung authentifizieren können:

 - Kennwort: Ist geeignet, wenn sich ein Benutzer während der Ausführung interaktiv anmelden möchte.
 - Zertifikat: Ist für unbeaufsichtigte Skripts geeignet, bei denen ohne Benutzerinteraktion authentifiziert werden muss.
 
## Erstellen einer AD-Anwendung mit Kennwort

In diesem Abschnitt führen Sie die Schritte zum Erstellen der AD-Anwendung mit einem Kennwort aus.

1. Wechseln Sie in den Azure-Ressourcen-Manager-Modus, und melden Sie sich bei Ihrem Konto an.

        azure config mode arm
        azure login

2. Erstellen Sie eine neue AD-Anwendung, indem Sie den Befehl **azure ad app create** ausführen. Geben Sie einen Anzeigenamen für die Anwendung an, außerdem die URI zu einer Seite mit einer Beschreibung der Anwendung (der Link wird nicht geprüft), die URIs, von denen die Anwendung identifiziert wird, und das Kennwort für Ihre Anwendungsidentität.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Die AD-Anwendung wird zurückgegeben. Die „AppId“-Eigenschaft wird benötigt, um Dienstprinzipale zu erstellen und Zugriffstoken abzurufen.

        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK

### Erstellen des Dienstprinzipals und Zuweisen zur Rolle

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Stellen Sie die Anwendungs-ID bereit, die im vorherigen Schritt zurückgegeben wurde.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
    Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt.
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK

2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md).

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

### Manuelles Bereitstellen von Anmeldeinformationen über Azure-CLI

Sie haben eine AD-Anwendung und einen Dienstprinzipal für diese Anwendung erstellt. Sie haben den Dienstprinzipal einer Rolle zugewiesen. Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen. Sie können die Anmeldeinformationen für den Dienstprinzipal beim Ausführen bedarfsgesteuerter Skripts oder Befehle manuell bereitstellen.

1. Bestimmen Sie die **TenantId** des Abonnements, das den Dienstprinzipal enthält. Wenn Sie die Mandanten-ID für das derzeit authentifizierte Abonnement abrufen, müssen Sie nicht die Abonnement-ID als Parameter angeben. Mit dem Switch **-r** wird der Wert ohne Anführungszeichen abgerufen.

        tenantId=$(azure account show -s <subscriptionId> --json | jq -r '.[0].tenantId')

2. Verwenden Sie als Benutzernamen die **AppId**, die Sie beim Erstellen des Dienstprinzipals verwendet haben. Wenn Sie die Anwendungs-ID abrufen müssen, führen Sie den folgenden Befehl aus. Geben Sie den Namen der Active Directory-Anwendung im **search**-Parameter an.

        appId=$(azure ad app show --search exampleapp --json | jq -r '.[0].appId')

3. Melden Sie sich als Dienstprinzipal an.

        azure login -u "$appId" --service-principal --tenant "$tenantId"

    Sie werden aufgefordert, das Kennwort einzugeben. Geben Sie das Kennwort ein, das Sie beim Erstellen des Kontos angegeben haben.

        info:    Executing command login
        Password: ********

Sie sind nun als Dienstprinzipal für die von Ihnen erstellte AD-Anwendung authentifiziert.

## Erstellen einer AD-Anwendung mit einem Zertifikat

In diesem Abschnitt führen Sie die Schritte zum Erstellen eines Dienstprinzipals für eine AD-Anwendung aus, die ein Zertifikat zur Authentifizierung verwendet. Es muss [OpenSSL](http://www.openssl.org/) installiert sein, damit Sie diese Schritte ausführen können.

1. Erstellen eines selbstsignierten Zertifikats

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinieren Sie die öffentlichen und privaten Schlüssel.

        cat privkey.pem cert.pem > examplecert.pem

3. Öffnen Sie die Datei **examplecert.pem**, und kopieren Sie die Zertifikatdaten. Suchen Sie nach der langen Zeichenfolge zwischen **-----BEGIN CERTIFICATE-----** und **-----END CERTIFICATE-----**.

4. Erstellen Sie eine neue Active Directory-Anwendung durch die Ausführung des Befehls **azure ad app create**, und geben Sie die Zertifikatdaten, die Sie im vorherigen Schritt kopiert haben, als Schlüsselwert an.

        azure ad app create -n "exampleapp" --home-page "https://www.contoso.org" -i "https://www.contoso.org/example" --key-value <certificate data>

    Die Active Directory-Anwendung wird zurückgegeben. Die „AppId“-Eigenschaft wird benötigt, um Dienstprinzipale zu erstellen und Zugriffstoken abzurufen.

        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK

### Erstellen des Dienstprinzipals und Zuweisen zur Rolle

1. Erstellen Sie einen Dienstprinzipal für Ihre Anwendung. Stellen Sie die Anwendungs-ID bereit, die im vorherigen Schritt zurückgegeben wurde.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
    Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt.
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel wird dem Dienstprinzipal die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md).

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

### Vorbereiten von Werten für Ihr Skript

In Ihrem Skript übergeben Sie drei Werte, die für die Anmeldung als Dienstprinzipal benötigt werden. Was Sie benötigen:

- Anwendungs-ID
- Mandanten-ID 
- Zertifikatfingerabdruck

Sie haben die Anwendungs-ID und den Zertifikatfingerabdruck bereits in den vorherigen Schritten gesehen. Wenn Sie diese Werte später abrufen müssen, werden die Befehle unten angezeigt (zusammen mit dem Befehl zum Abrufen der Mandanten-ID).

1. Verwenden Sie Folgendes, um den Zertifikatfingerabdruck abzurufen und nicht benötigte Zeichen zu entfernen:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Ein Fingerabdruck wie der folgende wird zurückgegeben:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Verwenden Sie zum Abrufen der Mandanten-ID Folgendes:

        azure account show -s <subscriptionId> --json | jq -r '.[0].tenantId'

3. Verwenden Sie zum Abrufen der Anwendungs-ID Folgendes:

        azure ad app show --search exampleapp --json | jq -r '.[0].appId'

### Bereitstellen eines Zertifikats über automatisiertes Azure-CLI-Skript

Sie haben eine Active Directory-Anwendung und einen Dienstprinzipal für diese Anwendung erstellt. Sie haben den Dienstprinzipal einer Rolle zugewiesen. Nun müssen Sie sich als Dienstprinzipal anmelden, um Aufgaben als Dienstprinzipal zu erledigen.

Geben Sie für die Authentifizierung mit Azure-CLI den Zertifikatfingerabdruck, die Zertifikatdatei, Anwendungs-ID und Mandanten-ID an.

        azure login --service-principal --tenant {tenant-id} -u {app-id} --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## Nächste Schritte
  
- Beispiele für die .NET-Authentifizierung finden Sie unter [Azure Resource Manager SDK für .NET](resource-manager-net-sdk.md).
- Beispiele für die Java-Authentifizierung finden Sie unter [Azure Resource Manager SDK für Java](resource-manager-java-sdk.md). 
- Beispiele für die Python-Authentifizierung finden Sie unter [Resource Management Authentication](https://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementauthentication.html) (Ressourcenverwaltungsauthentifizierung) für Python.
- Beispiele für die REST-Authentifizierung finden Sie unter [Resource Manager-REST-APIs](resource-manager-rest-api.md).
- Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
- Weitere Informationen zur Verwendung von Zertifikaten und der Azure-Befehlszeilenschnittstelle finden Sie unter [Certificate-based auth with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx) (Zertifikatbasierte Authentifizierung mit Azure-Dienstprinzipalen über Linux-Befehlszeile). 

<!---HONumber=AcomDC_0601_2016-->