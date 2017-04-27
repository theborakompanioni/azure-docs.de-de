---
title: "Erstellen einer Identität für die Azure-App mit der Azure CLI | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle eine Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/31/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 4ea75e08a630ad777444ea3a3cb85f4bb0efe01f
ms.lasthandoff: 04/03/2017


---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-authenticate-service-principal.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
> * [Portal](resource-group-create-service-principal-portal.md)
> 
> 

Wenn eine App oder ein Skript Zugriff auf Ressourcen benötigt, können Sie eine Identität für die App einrichten und sie mit ihren eigenen Anmeldeinformationen authentifizieren. Diese Identität wird als Dienstprinzipal bezeichnet. Dieser Ansatz ermöglicht Ihnen Folgendes:

* Sie können der App-Identität Berechtigungen zuweisen, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.
* Sie können ein Zertifikat für die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts verwenden.

In diesem Artikel erfahren Sie, wie Sie mithilfe der [Azure-Befehlszeilenschnittstelle 1.0](../cli-install-nodejs.md) eine Anwendung einrichten, die unter eigenen Anmeldeinformationen und einer eigenen Identität ausgeführt wird. Installieren Sie die neueste Version der [Azure-Befehlszeilenschnittstelle 1.0](../cli-install-nodejs.md), um sicherzustellen, dass Ihre Umgebung mit den Beispielen in diesem Artikel übereinstimmt.

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Zum Abschließen dieses Themas benötigen Sie sowohl in der Azure Active Directory-Instanz als auch im Azure-Abonnement ausreichende Berechtigungen. Insbesondere müssen Sie eine App in der Active Directory-Instanz erstellen und den Dienstprinzipal einer Rolle zuweisen können. 

Die einfachste Möglichkeit zum Überprüfen, ob Ihr Konto über die erforderlichen Berechtigungen verfügt, ist über das Portal. Siehe [Überprüfen der erforderlichen Berechtigung im Portal](resource-group-create-service-principal-portal.md#required-permissions).

Fahren Sie nun mit dem Abschnitt für eine [kennwortbasierte](#create-service-principal-with-password) oder [zertifikatbasierte](#create-service-principal-with-certificate) Authentifizierung fort.

## <a name="create-service-principal-with-password"></a>Erstellen eines Dienstprinzipals mit Kennwort
Dieser Abschnitt enthält die Schritte zum Erstellen der AD-Anwendung mit einem Kennwort und das Zuweisen der Rolle „Leser“ zum Dienstprinzipal.

1. Melden Sie sich bei Ihrem Konto an.
   
   ```azurecli
   azure login
   ```
2. Zum Erstellen einer App-Identität geben Sie den Namen der App und ein Kennwort wie im folgenden Befehl an:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt. Die mit den Dienstprinzipalnamen aufgeführte GUID wird bei der Anmeldung benötigt. Diese GUID hat den gleichen Wert wie die App-ID. In den Beispielanwendungen wird dieser Wert als `Client ID` bezeichnet. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle „Leser“ hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Geben Sie für den objectid-Parameter die Objekt-ID an, die Sie beim Erstellen der Anwendung verwendet haben. Vor der Ausführung dieses Befehls müssen Sie einige Zeit für die Verteilung des neuen Dienstprinzipals in Active Directory einplanen. Wenn Sie diese Befehle manuell ausführen, vergeht in der Regel genügend Zeit zwischen Aufgaben. In einem Skript sollten Sie einen Schritt für eine Pause zwischen den Befehlen hinzufügen (z.B. `sleep 15`). Wenn Sie die Fehlermeldung erhalten, dass der Prinzipal nicht im Verzeichnis vorhanden ist, führen Sie den Befehl erneut aus.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
Das ist alles! AD-Anwendung und Dienstprinzipal sind eingerichtet. Der nächste Abschnitt veranschaulicht die Anmeldung mit den Anmeldeinformationen über die Azure-CLI. Wenn Sie die Anmeldeinformationen in Ihrer Codeanwendung verwenden möchten, müssen Sie mit diesem Thema nicht fortfahren. In diesem Fall können Sie sich in den [Beispielanwendungen](#sample-applications) Beispiele für die Anmeldung mit Anwendungs-ID und Kennwort ansehen. 

### <a name="provide-credentials-through-azure-cli"></a>Angeben von Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle
Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Verwenden Sie zum Abrufen der Mandanten-ID für Ihr derzeit authentifiziertes Abonnement Folgendes:
   
   ```azurecli
   azure account show
   ```
   
   Ausgabe des Befehls:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Wenn Sie die Mandanten-ID eines anderen Abonnements abrufen möchten, verwenden Sie den folgenden Befehl:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Wenn Sie die zur Anmeldung zu verwendende Client-ID abrufen müssen, verwenden Sie:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     Der für die Anmeldung zu verwendende Wert ist die in den Dienstprinzipalnamen aufgeführte GUID.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Melden Sie sich als Dienstprinzipal an.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Sie werden aufgefordert, das Kennwort einzugeben. Geben Sie das Kennwort ein, das Sie beim Erstellen des Kontos angegeben haben.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Sie sind nun als Dienstprinzipal für den von Ihnen erstellten Dienstprinzipal authentifiziert.

Alternativ können Sie die REST-Vorgänge über die Befehlszeile aufrufen, um sich anzumelden. Sie können das Zugriffstoken aus der Authentifizierungsantwort abrufen, um es in anderen Vorgängen zu verwenden. Ein Beispiel zum Abrufen des Zugriffstokens durch Aufrufen der REST-Vorgänge finden Sie unter [Generieren eines Zugriffstokens](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Erstellen eines Dienstprinzipals mit Zertifikat
In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen eines selbstsignierten Zertifikats
* Erstellen der AD-Anwendung mit dem Zertifikat und dem Dienstprinzipal
* Zuweisen der Rolle „Leser“ zum Dienstprinzipal

Für diese Schritte muss [OpenSSL](http://www.openssl.org/) installiert sein.

1. Erstellen eines selbstsignierten Zertifikats
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. Im vorherigen Schritt werden die beiden Dateien „privkey.pem“ und „cert.pem“ erstellt. Kombinieren Sie die öffentlichen und privaten Schlüssel in einer Datei.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Öffnen Sie die Datei **examplecert.pem**, und suchen Sie nach der langen Zeichenfolge zwischen **-----BEGIN CERTIFICATE-----** und **-----END CERTIFICATE-----**. Kopieren Sie die Zertifikatdaten. Beim Erstellen des Dienstprinzipals übergeben Sie diese Daten als Parameter.

4. Melden Sie sich bei Ihrem Konto an.

   ```azurecli
   azure login
   ```
5. Um den Dienstprinzipal zu erstellen, geben Sie den Namen der App und die Zertifikatdaten wie im folgenden Befehl an:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   Der neue Dienstprinzipal wird zurückgegeben. Die Objekt-ID wird beim Gewähren von Berechtigungen benötigt. Die mit den Dienstprinzipalnamen aufgeführte GUID wird bei der Anmeldung benötigt. Diese GUID hat den gleichen Wert wie die App-ID. In den Beispielanwendungen wird dieser Wert als „Client ID“ bezeichnet. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Gewähren Sie dem Dienstprinzipal Berechtigungen für Ihr Abonnement. In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle „Leser“ hinzu, die Berechtigung zum Lesen aller Ressourcen im Abonnement gewährt. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Geben Sie für den objectid-Parameter die Objekt-ID an, die Sie beim Erstellen der Anwendung verwendet haben. Vor der Ausführung dieses Befehls müssen Sie einige Zeit für die Verteilung des neuen Dienstprinzipals in Active Directory einplanen. Wenn Sie diese Befehle manuell ausführen, vergeht in der Regel genügend Zeit zwischen Aufgaben. In einem Skript sollten Sie einen Schritt für eine Pause zwischen den Befehlen hinzufügen (z.B. `sleep 15`). Wenn Sie die Fehlermeldung erhalten, dass der Prinzipal nicht im Verzeichnis vorhanden ist, führen Sie den Befehl erneut aus.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Bereitstellen eines Zertifikats über automatisiertes Azure-CLI-Skript
Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen.

1. Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Active Directory. Verwenden Sie zum Abrufen der Mandanten-ID für Ihr derzeit authentifiziertes Abonnement Folgendes:
   
   ```azurecli
   azure account show
   ```
   
   Ausgabe des Befehls:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Wenn Sie die Mandanten-ID eines anderen Abonnements abrufen möchten, verwenden Sie den folgenden Befehl:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Verwenden Sie Folgendes, um den Zertifikatfingerabdruck abzurufen und nicht benötigte Zeichen zu entfernen:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Ein Fingerabdruck wie der folgende wird zurückgegeben:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Wenn Sie die zur Anmeldung zu verwendende Client-ID abrufen müssen, verwenden Sie:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   Der für die Anmeldung zu verwendende Wert ist die in den Dienstprinzipalnamen aufgeführte GUID.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Melden Sie sich als Dienstprinzipal an.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Sie sind nun als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt haben.

## <a name="change-credentials"></a>Ändern von Anmeldeinformationen

Zum Ändern der Anmeldeinformationen für eine AD-App, entweder aufgrund einer Sicherheitsgefährdung oder des Ablaufs von Anmeldeinformationen, verwenden Sie `azure ad app set`.

Zum Ändern eines Kennworts verwenden Sie:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

Zum Ändern eines Zertifikatwerts verwenden Sie:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Debuggen

Wenn Sie einen Dienstprinzipal erstellen, können folgende Fehler auftreten:

* **„Authentication_Unauthorized“** oder **„Kein Abonnement in diesem Kontext gefunden.“** – Dieser Fehler wird angezeigt, wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) zum Registrieren einer App im Active Directory hat. In der Regel wird dieser Fehler angezeigt, wenn in Ihrem Active Directory nur Administratorbenutzer Apps registrieren können und Ihr Konto kein Administratorkonto ist. Bitten Sie Ihren Administrator, Sie entweder einer Administratorrolle zuzuweisen oder es Benutzern zu ermöglichen, Apps zu registrieren.

* Ihr Konto **„hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' über Bereich '/subscriptions/{guid}'.“**  – Dieser Fehler wird angezeigt, wenn Ihr Konto nicht über ausreichende Berechtigungen verfügt, um eine Rolle einer Identität zuzuweisen. Bitten Sie Ihren Abonnementadministrator, Sie der Rolle „Benutzerzugriffsadministrator“ hinzuzufügen.

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
* Weitere Informationen zur Verwendung von Zertifikaten und der Azure-Befehlszeilenschnittstelle finden Sie unter [Certificate-based auth with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx)(Zertifikatbasierte Authentifizierung mit Azure-Dienstprinzipalen über die Linux-Befehlszeile). 


