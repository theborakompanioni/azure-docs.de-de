---
title: "DocumentDB-Automatisierung – Resource Manager – Azure CLI 1.0 | Microsoft-Dokumentation"
description: "Verwenden Sie Azure-Ressourcen-Manager-Vorlagen oder die CLI zur Bereitstellung eines DocumentDB-Datenbankkontos. DocumentDB ist eine cloudbasierte NoSQL-Datenbank für JSON-Daten."
services: documentdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: eae5eec6-0e27-442c-abfc-ef6b7fd3f8d2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 4725f4200446434bfcb8754aac9bf0d99f8a7526
ms.lasthandoff: 03/21/2017


---
# <a name="automate-documentdb-account-creation-using-azure-cli-10-and-azure-resource-manager-templates"></a>Automatisieren der Erstellung von DocumentDB-Konten mithilfe der Azure CLI 1.0 und Azure Resource Manager-Vorlagen
> [!div class="op_single_selector"]
> * [Azure-Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

In diesem Artikel erfahren Sie, wie Sie ein Azure DocumentDB-Konto mithilfe von Azure Resource Manager-Vorlagen oder direkt mit der Azure CLI 1.0 (Command-Line Interface, Befehlszeilenschnittstelle) erstellen können. Weitere Informationen zum Erstellen eines DocumentDB-Kontos über das Azure Portal finden Sie unter [Erstellen eines DocumentDB-Kontos über das Azure-Portal](documentdb-create-account.md).

DocumentDB-Datenbankkonten sind derzeit die einzige DocumentDB-Ressource, die mithilfe von Resource Manager-Vorlagen und der Azure CLI 1.0 erstellt werden kann.

## <a name="getting-ready"></a>Vorbereitung
Vor der Verwendung der Azure CLI 1.0 mit Azure-Ressourcengruppen benötigen Sie die richtige Version und ein Azure-Konto. Wenn Sie nicht über die Azure CLI 1.0 verfügen, [installieren Sie sie](../cli-install-nodejs.md).

### <a name="update-your-azure-cli-10-version"></a>Aktualisieren der Azure CLI 1.0-Version
Geben Sie an der Eingabeaufforderung `azure --version` ein, um zu überprüfen, ob Sie bereits Version 0.10.4 oder höher installiert haben. Sie werden in diesem Schritt vielleicht aufgefordert, an einer Microsoft Azure-CLI-Datensammlung teilzunehmen, und können „j“ oder „n“ wählen.

    azure --version
    0.10.4 (node: 4.2.4)

Wenn Ihre Version nicht 0.10.4 oder höher entspricht, müssen Sie mit einem der nativen Installer die [Azure CLI 1.0 installieren](../cli-install-nodejs.md) oder aktualisieren. Sie können die CLI auch mit **npm** und Eingabe von `npm update -g azure-cli` aktualisieren oder mit `npm install -g azure-cli` installieren.

### <a name="set-your-azure-account-and-subscription"></a>Festlegen Ihres Azure-Kontos und -Abonnements
Wenn Sie noch kein Azure-Abonnement, aber ein Visual Studio-Abonnement besitzen, können Sie Ihre [Visual Studio-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)aktivieren. Oder Sie registrieren sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

Zum Verwenden der Azure-Ressourcen-Manager-Vorlagen benötigen Sie eine Geschäfts-, Schul- oder Microsoft-Kontoidentität. Geben Sie den folgenden Befehl ein, wenn Sie eines dieser Konten besitzen:

    azure login

Die folgende Ausgabe wird erzeugt:

    info:    Executing command login
    |info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin.
    Enter the code E1A2B3C4D to authenticate.

> [!NOTE]
> Wenn Sie kein Azure-Konto besitzen, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass Sie einen anderen Kontotyp benötigen. Informationen zum Erstellen eines Kontos auf der Basis Ihres aktuellen Azure-Kontos finden Sie unter [Erstellen von Arbeits- oder Schulidentitäten in Azure Active Directory](../virtual-machines/virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Öffnen Sie [https://aka.ms/devicelogin](https://aka.ms/devicelogin) in einem Browser, und geben Sie den in der Befehlsausgabe angegebenen Code ein.

![Screenshot mit dem Geräteanmeldungsbildschirm für die Microsoft Azure CLI 1.0](media/documentdb-automation-resource-manager-cli/azure-cli-login-code.png)

Nach der Eingabe des Codes wählen Sie die gewünschte Identität im Browser aus und geben bei Bedarf Ihren Benutzernamen und Ihr Kennwort an.

![Screenshot zur Auswahl des Microsoft-Identitätskontos, das dem gewünschten Azure-Abonnement zugewiesen ist](media/documentdb-automation-resource-manager-cli/identity-cli-login.png)

Bei erfolgreicher Anmeldung wird der folgende Bestätigungsbildschirm angezeigt. Sie können das Browserfenster jetzt schließen.

![Screenshot mit der Bestätigung der Anmeldung bei der plattformübergreifenden Microsoft Azure-Befehlszeilenschnittstelle](media/documentdb-automation-resource-manager-cli/login-confirmation.png)

In der Befehlsshell wird außerdem die folgende Ausgabe angezeigt:

    /info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

Zusätzlich zu der hier beschriebenen interaktiven Anmeldemethode sind zusätzliche Azure CLI 1.0-Anmeldemethoden verfügbar. Informationen zu weiteren Methoden und zur Behandlung mehrerer Abonnements finden Sie unter [Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure-Befehlszeilenschnittstelle (Azure CLI 1.0)](../xplat-cli-connect.md).

### <a name="switch-to-azure-cli-10-resource-group-mode"></a>Wechseln in den Azure CLI 1.0-Ressourcengruppenmodus
Standardmäßig startet die Azure CLI 1.0 im Service Management-Modus (**asm**-Modus). Geben Sie Folgendes ein, um in den Ressourcengruppen-Modus zu wechseln.

    azure config mode arm

Die folgende Ausgabe wird zurückgegeben:

    info:    Executing command config mode
    info:    New mode is arm
    info:    config mode command OK

Sie können ggf. zum Standardbefehlssatz wechseln, indem Sie `azure config mode asm` eingeben.

### <a name="create-or-retrieve-your-resource-group"></a>Erstellen oder Abrufen der Ressourcengruppe
Wenn Sie ein DocumentDB-Konto erstellen möchten, benötigen Sie zuerst eine Ressourcengruppe. Wenn Sie den Namen der gewünschten Ressourcengruppe bereits kennen, fahren Sie mit [Schritt 2](#create-documentdb-account-cli)fort.

Wenn Sie eine Liste aller aktuellen Ressourcengruppen anzeigen möchten, führen Sie den folgenden Befehl aus, und notieren Sie sich den Namen der gewünschten Ressourcengruppe:

    azure group list

Wenn Sie eine Ressourcengruppe erstellen möchten, führen Sie den folgenden Befehl aus, und geben Sie den Namen der neu zu erstellenden Ressourcengruppe und die Region an, in der Sie die Ressourcengruppe erstellen möchten:

    azure group create <resourcegroupname> <resourcegrouplocation>

* `<resourcegroupname>` darf nur alphanumerische Zeichen, Punkte, Unterstriche, den Bindestrich „-“ und Klammern enthalten und darf nicht mit einem Punkt enden.
* `<resourcegrouplocation>` muss einer der Regionen entsprechen, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

Beispieleingabe:

    azure group create new_res_group westus

Die folgende Ausgabe wird erzeugt:

    info:    Executing command group create
    + Getting resource group new_res_group
    + Creating resource group new_res_group
    info:    Created resource group new_res_group
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group
    data:    Name:                new_res_group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).

## <a name="understanding-resource-manager-templates-and-resource-groups"></a>Grundlegendes zu Resource Manager-Vorlagen und -Ressourcengruppen
Die meisten Anwendungen basieren auf einer Kombination verschiedener Ressourcentypen (z. B. mindestens ein DocumentDB-Konto, Speicherkonten, ein virtuelles Netzwerk oder ein Content Delivery Network). Die standardmäßige Azure-Service-Management-API und das Azure-Portal stellte diese Elemente mithilfe eines Ansatzes auf Dienstbasis dar. Bei dieser Vorgehensweise müssen Sie die einzelnen Dienste einzeln und nicht als eine logische Bereitstellungseinheit bereitstellen und verwalten (oder weitere Tools suchen, die dies ermöglichen).

*Azure-Ressourcen-Manager-Vorlagen* ermöglichen es Ihnen, diese verschiedenen Ressourcen als eine logische Bereitstellungseinheit auf deklarative Weise bereitzustellen und zu verwalten. Anstatt Azure imperativisch mitzuteilen, wie ein Befehl nach dem anderen bereitgestellt wird, beschreiben Sie die gesamte Bereitstellung in einer JSON-Datei – alle Ressourcen und zugeordnete Konfigurations- und Bereitstellungsparameter – und geben an Azure weiter, dass diese Ressourcen als eine Gruppe bereitgestellt werden sollen.

Weitere Informationen zu Azure-Ressourcengruppen und ihrer Nutzung finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

## <a id="quick-create-documentdb-account"></a>Aufgabe: Erstellen eines DocumentDB-Kontos für eine einzelne Region
Befolgen Sie die Anweisungen in diesem Abschnitt, um ein DocumentDB-Konto für eine einzelne Region zu erstellen. Hierfür können Sie die Azure CLI 1.0 mit oder ohne Resource Manager-Vorlagen verwenden.

### <a id="create-single-documentdb-account-cli-arm"></a> Erstellen eines DocumentDB-Kontos für eine einzelne Region per Azure CLI 1.0 ohne Resource Manager-Vorlagen
Erstellen Sie ein DocumentDB-Konto in der neuen oder vorhandenen Ressourcengruppe, indem Sie den folgenden Befehl an der Eingabeaufforderung eingeben:

> [!TIP]
> Wenn Sie diesen Befehl in Azure PowerShell oder Windows PowerShell ausführen, erhalten Sie eine Fehlermeldung über ein unerwartetes Token. Führen Sie den Befehl stattdessen an der Windows-Eingabeaufforderung aus.
>
>

    azure resource create -g <resourcegroupname> -n <databaseaccountname> -r "Microsoft.DocumentDB/databaseAccounts" -o 2015-04-08 -l <resourcegrouplocation> -p "{\"databaseAccountOfferType\":\"Standard\",\"ipRangeFilter\":\"<ip-range-filter>\",\"locations\":["{\"locationName\":\"<databaseaccountlocation>\",\"failoverPriority\":\"<failoverPriority>\"}"]}"

* `<resourcegroupname>` darf nur alphanumerische Zeichen, Punkte, Unterstriche, den Bindestrich „-“ und Klammern enthalten und darf nicht mit einem Punkt enden.
* `<resourcegrouplocation>` ist die Region der aktuellen Ressourcengruppe.
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [DocumentDB – Firewallunterstützung](documentdb-firewall-support.md).
* `<databaseaccountname>` darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.
* `<databaseaccountlocation>` muss einer der Regionen entsprechen, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

Beispieleingabe:

    azure resource create -g new_res_group -n samplecliacct -r "Microsoft.DocumentDB/databaseAccounts" -o 2015-04-08 -l westus -p "{\"databaseAccountOfferType\":\"Standard\",\"ipRangeFilter\":\"\",\"locations\":["{\"locationName\":\"westus\",\"failoverPriority\":\"0\"}"]}"

Bei der Bereitstellung Ihres neuen Kontos wird die folgende Ausgabe erzeugt:

    info:    Executing command resource create
    + Getting resource samplecliacct
    + Creating resource samplecliacct
    info:    Resource samplecliacct is updated
    data:
    data:    Id:        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group/providers/Microsoft.DocumentDB/databaseAccounts/samplecliacct
    data:    Name:      samplecliacct
    data:    Type:      Microsoft.DocumentDB/databaseAccounts
    data:    Parent:
    data:    Location:  West US
    data:    Tags:
    data:
    info:    resource create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).

Nachdem der Befehl ausgeführt wurde, befindet sich das Konto einige Minuten im Zustand **Wird erstellt**, bis es in den Zustand **Online** wechselt und verwendet werden kann. Sie können den Status des Kontos im [Azure-Portal](https://portal.azure.com)auf dem Blatt **DocumentDB-Konten** überprüfen.

### <a id="create-single-documentdb-account-cli-arm"></a> Erstellen eines DocumentDB-Kontos für eine einzelne Region über die Azure CLI 1.0 mit Resource Manager-Vorlagen
Mit der Anleitung in diesem Abschnitt wird beschrieben, wie Sie ein DocumentDB-Konto mithilfe einer Azure Resource Manager-Vorlage und einer optionalen Parameterdatei erstellen. In beiden Fällen handelt es sich um JSON-Dateien. Mithilfe einer Vorlage können Sie genaue Vorgaben machen und diese ohne Fehler wiederverwenden.

Erstellen Sie eine lokale Vorlagendatei mit folgendem Inhalt: Geben Sie der Datei den Namen "azuredeploy.json".

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "type": "string"
            },
            "locationName1": {
                "type": "string"
            }
        },
        "variables": {},
        "resources": [
            {
                "apiVersion": "2015-04-08",
                "type": "Microsoft.DocumentDb/databaseAccounts",
                "name": "[parameters('databaseAccountName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "databaseAccountOfferType": "Standard",
                    "ipRangeFilter": "",
                    "locations": [
                        {
                            "failoverPriority": 0,
                            "locationName": "[parameters('locationName1')]"
                        }
                    ]
                }
            }
        ]
    }

failoverPriority muss auf 0 festgelegt werden, da dies ein Konto für eine einzelne Region ist. Eine failoverPriority von 0 bedeutet, dass diese Region als [Schreibregion für das DocumentDB-Konto][scaling-globally] beibehalten werden soll.
Sie können den Wert an der Befehlszeile eingeben oder eine Parameterdatei erstellen, um den Wert anzugeben.

Wenn Sie eine Parameterdatei erstellen möchten, kopieren Sie den folgenden Inhalt in eine neue Datei, und geben Sie ihr den Namen "azuredeploy.parameters.json". Wenn Sie den Datenbankkontonamen an der Eingabeaufforderung angeben möchten, können Sie den Vorgang ohne Erstellen dieser Datei fortsetzen.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "value": "samplearmacct"
            },
            "locationName1": {
                "value": "westus"
            }
        }
    }

Ändern Sie in der Datei „azuredeploy.parameters.json“ das Feld mit dem Wert `"samplearmacct"` in den gewünschten Datenbanknamen, und speichern Sie die Datei. `"databaseAccountName"` darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein. Aktualisieren Sie das Feld mit dem Wert `"locationName1"` in die Region, in der Sie das DocumentDB-Konto erstellen möchten.

Wenn Sie ein DocumentDB-Konto in der Ressourcengruppe erstellen möchten, führen Sie den folgenden Befehl aus, und geben Sie den Pfad der Vorlagendatei, den Pfad der Parameterdatei oder den Parameterwert, den Namen der Ressourcengruppe für die Bereitstellung und einen Bereitstellungsnamen (-n ist optional) an.

So verwenden Sie eine Parameterdatei

    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g <resourcegroupname> -n <deploymentname>

* `<PathToTemplate>` ist der Pfad zur Datei „azuredeploy.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<PathToParameterFile>` ist der Pfad zur Datei „azuredeploy.parameters.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<resourcegroupname>` ist der Name der vorhandenen Ressourcengruppe, der ein DocumentDB-Datenbankkonto hinzugefügt werden soll.
* `<deploymentname>` ist der optionale Name der Bereitstellung.

Beispieleingabe:

    azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json -g new_res_group -n azuredeploy

ODER führen Sie den folgenden Befehl aus, um den Namensparameter für das Datenbankkonto ohne Parameterdatei anzugeben. In diesem Fall werden Sie zur Eingabe des Werts aufgefordert:

    azure group deployment create -f <PathToTemplate> -g <resourcegroupname> -n <deploymentname>

Beispiel für Eingabeaufforderung und Eintrag eines Datenbankkontos mit dem Namen „samplearmacct“:

    azure group deployment create -f azuredeploy.json -g new_res_group -n azuredeploy
    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    databaseAccountName: samplearmacct

Bei der Bereitstellung des Kontos werden folgende Informationen angezeigt:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "azuredeploy"
    + Waiting for deployment to complete
    +
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Running
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Succeeded
    data:    DeploymentName     : azuredeploy
    data:    ResourceGroupName  : new_res_group
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-11-30T18:50:23.6300288Z
    data:    Mode               : Incremental
    data:    CorrelationId      : 4a5d4049-c494-4053-bad4-cc804d454700
    data:    DeploymentParameters :
    data:    Name                 Type    Value
    data:    -------------------  ------  ------------------
    data:    databaseAccountName  String  samplearmacct
    data:    locationName1        String  westus
    info:    group deployment create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).  

Nachdem der Befehl ausgeführt wurde, befindet sich das Konto einige Minuten im Zustand **Wird erstellt**, bis es in den Zustand **Online** wechselt und verwendet werden kann. Sie können den Status des Kontos im [Azure-Portal](https://portal.azure.com)auf dem Blatt **DocumentDB-Konten** überprüfen.

## <a id="quick-create-documentdb-with-mongodb-api-account"></a>Aufgabe: Erstellen einer DocumentDB-Instanz für eine einzelne Region: API für MongoDB-Konto
Befolgen Sie die Anweisungen in diesem Abschnitt, um eine API für das MongoDB-Konto für eine einzelne Region zu erstellen. Hierfür können Sie die Azure CLI 1.0 mit Resource Manager-Vorlagen verwenden.

### <a id="create-single-documentdb-with-mongodb-api-account-cli-arm"></a> Erstellen eines MongoDB-Kontos für eine einzelne Region über die Azure CLI 1.0 mit Resource Manager-Vorlagen
In diesem Abschnitt wird beschrieben, wie Sie mithilfe einer Azure Resource Manager-Vorlage und einer optionalen Parameterdatei (beides JSON-Dateien) eine API für ein MongoDB-Konto erstellen. Mithilfe einer Vorlage können Sie genaue Vorgaben machen und diese ohne Fehler wiederverwenden.

Erstellen Sie eine lokale Vorlagendatei mit folgendem Inhalt: Geben Sie der Datei den Namen "azuredeploy.json".

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "type": "string"
            },
            "locationName1": {
                "type": "string"
            }
        },
        "variables": {},
        "resources": [
            {
                "apiVersion": "2015-04-08",
                "type": "Microsoft.DocumentDb/databaseAccounts",
                "name": "[parameters('databaseAccountName')]",
                "location": "[resourceGroup().location]",
                "kind": "MongoDB",
                "properties": {
                    "databaseAccountOfferType": "Standard",
                    "ipRangeFilter": "",
                    "locations": [
                        {
                            "failoverPriority": 0,
                            "locationName": "[parameters('locationName1')]"
                        }
                    ]
                }
            }
        ]
    }

Die Variante muss auf MongoDB festgelegt werden, um anzugeben, dass dieses Konto MongoDB-APIs unterstützt. Wenn keine Varianteneigenschaft angegeben ist, wird standardmäßig ein natives DocumentDB-Konto verwendet.

failoverPriority muss auf 0 festgelegt werden, da dies ein Konto für eine einzelne Region ist. Eine failoverPriority von 0 bedeutet, dass diese Region als [Schreibregion für das DocumentDB-Konto][scaling-globally] beibehalten werden soll.
Sie können den Wert an der Befehlszeile eingeben oder eine Parameterdatei erstellen, um den Wert anzugeben.

Wenn Sie eine Parameterdatei erstellen möchten, kopieren Sie den folgenden Inhalt in eine neue Datei, und geben Sie ihr den Namen "azuredeploy.parameters.json". Wenn Sie den Datenbankkontonamen an der Eingabeaufforderung angeben möchten, können Sie den Vorgang ohne Erstellen dieser Datei fortsetzen.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "value": "samplearmacct"
            },
            "locationName1": {
                "value": "westus"
            }
        }
    }

Ändern Sie in der Datei „azuredeploy.parameters.json“ das Feld mit dem Wert `"samplearmacct"` in den gewünschten Datenbanknamen, und speichern Sie die Datei. `"databaseAccountName"` darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein. Aktualisieren Sie das Feld mit dem Wert `"locationName1"` in die Region, in der Sie das DocumentDB-Konto erstellen möchten.

Wenn Sie ein DocumentDB-Konto in der Ressourcengruppe erstellen möchten, führen Sie den folgenden Befehl aus, und geben Sie den Pfad der Vorlagendatei, den Pfad der Parameterdatei oder den Parameterwert, den Namen der Ressourcengruppe für die Bereitstellung und einen Bereitstellungsnamen (-n ist optional) an.

So verwenden Sie eine Parameterdatei

    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g <resourcegroupname> -n <deploymentname>

* `<PathToTemplate>` ist der Pfad zur Datei „azuredeploy.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<PathToParameterFile>` ist der Pfad zur Datei „azuredeploy.parameters.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<resourcegroupname>` ist der Name der vorhandenen Ressourcengruppe, der ein DocumentDB-Datenbankkonto hinzugefügt werden soll.
* `<deploymentname>` ist der optionale Name der Bereitstellung.

Beispieleingabe:

    azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json -g new_res_group -n azuredeploy

ODER führen Sie den folgenden Befehl aus, um den Namensparameter für das Datenbankkonto ohne Parameterdatei anzugeben. In diesem Fall werden Sie zur Eingabe des Werts aufgefordert:

    azure group deployment create -f <PathToTemplate> -g <resourcegroupname> -n <deploymentname>

Beispiel für Eingabeaufforderung und Eintrag eines Datenbankkontos mit dem Namen „samplearmacct“:

    azure group deployment create -f azuredeploy.json -g new_res_group -n azuredeploy
    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    databaseAccountName: samplearmacct

Bei der Bereitstellung des Kontos werden folgende Informationen angezeigt:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "azuredeploy"
    + Waiting for deployment to complete
    +
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Running
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Succeeded
    data:    DeploymentName     : azuredeploy
    data:    ResourceGroupName  : new_res_group
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-11-30T18:50:23.6300288Z
    data:    Mode               : Incremental
    data:    CorrelationId      : 4a5d4049-c494-4053-bad4-cc804d454700
    data:    DeploymentParameters :
    data:    Name                 Type    Value
    data:    -------------------  ------  ------------------
    data:    databaseAccountName  String  samplearmacct
    data:    locationName1        String  westus
    info:    group deployment create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).  

Nachdem der Befehl ausgeführt wurde, befindet sich das Konto einige Minuten im Zustand **Wird erstellt**, bis es in den Zustand **Online** wechselt und verwendet werden kann. Sie können den Status des Kontos im [Azure-Portal](https://portal.azure.com)auf dem Blatt **DocumentDB-Konten** überprüfen.

## <a id="create-multi-documentdb-account"></a>Aufgabe: Erstellen eines DocumentDB-Kontos für mehrere Regionen
DocumentDB bietet die Möglichkeit zum [globalen Verteilen von Daten][distribute-globally] auf verschiedene [Azure-Regionen](https://azure.microsoft.com/regions/#services). Beim Erstellen eines DocumentDB-Kontos können Sie die Regionen angeben, in denen der Dienst vorhanden sein soll. Befolgen Sie die Anweisungen in diesem Abschnitt, um ein DocumentDB-Konto für mehrere Regionen zu erstellen. Hierfür können Sie die Azure CLI 1.0 mit oder ohne Resource Manager-Vorlagen verwenden.

### <a id="create-multi-documentdb-account-cli"></a> Erstellen eines DocumentDB-Kontos für mehrere Regionen über die Azure CLI 1.0 ohne Resource Manager-Vorlagen
Erstellen Sie ein DocumentDB-Konto in der neuen oder vorhandenen Ressourcengruppe, indem Sie den folgenden Befehl an der Eingabeaufforderung eingeben:

> [!TIP]
> Wenn Sie diesen Befehl in Azure PowerShell oder Windows PowerShell ausführen, erhalten Sie eine Fehlermeldung über ein unerwartetes Token. Führen Sie den Befehl stattdessen an der Windows-Eingabeaufforderung aus.
>
>

    azure resource create -g <resourcegroupname> -n <databaseaccountname> -r "Microsoft.DocumentDB/databaseAccounts" -o 2015-04-08 -l <resourcegrouplocation> -p "{\"databaseAccountOfferType\":\"Standard\",\"ipRangeFilter\":\"<ip-range-filter>\",\"locations\":["{\"locationName\":\"<databaseaccountlocation1>\",\"failoverPriority\":\"<failoverPriority1>\"},{\"locationName\":\"<databaseaccountlocation2>\",\"failoverPriority\":\"<failoverPriority2>\"}"]}"

* `<resourcegroupname>` darf nur alphanumerische Zeichen, Punkte, Unterstriche, den Bindestrich „-“ und Klammern enthalten und darf nicht mit einem Punkt enden.
* `<resourcegrouplocation>` ist die Region der aktuellen Ressourcengruppe.
* `<ip-range-filter>` gibt die Gruppe der IP-Adressen oder IP-Adressbereiche im CIDR-Format an, die als Liste der zulässigen Client-IPs für ein bestimmtes Datenbankkonto aufgenommen wird. IP-Adressen und -Adressbereiche müssen durch Kommas voneinander getrennt werden, und sie dürfen keine Leerzeichen enthalten. Weitere Informationen finden Sie unter [DocumentDB – Firewallunterstützung](documentdb-firewall-support.md).
* `<databaseaccountname>` darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein.
* `<databaseaccountlocation1>` und `<databaseaccountlocation2>` müssen Regionen entsprechen, in denen DocumentDB allgemein verfügbar ist. Die aktuelle Liste der Regionen finden Sie auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services).

Beispieleingabe:

    azure resource create -g new_res_group -n samplecliacct -r "Microsoft.DocumentDB/databaseAccounts" -o 2015-04-08 -l westus -p "{\"databaseAccountOfferType\":\"Standard\",\"ipRangeFilter\":\"\",\"locations\":["{\"locationName\":\"westus\",\"failoverPriority\":\"0\"},{\"locationName\":\"eastus\",\"failoverPriority\":\"1\"}"]}"

Bei der Bereitstellung Ihres neuen Kontos wird die folgende Ausgabe erzeugt:

    info:    Executing command resource create
    + Getting resource samplecliacct
    + Creating resource samplecliacct
    info:    Resource samplecliacct is updated
    data:
    data:    Id:        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group/providers/Microsoft.DocumentDB/databaseAccounts/samplecliacct
    data:    Name:      samplecliacct
    data:    Type:      Microsoft.DocumentDB/databaseAccounts
    data:    Parent:
    data:    Location:  West US
    data:    Tags:
    data:
    info:    resource create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).

Nachdem der Befehl ausgeführt wurde, befindet sich das Konto einige Minuten im Zustand **Wird erstellt**, bis es in den Zustand **Online** wechselt und verwendet werden kann. Sie können den Status des Kontos im [Azure-Portal](https://portal.azure.com)auf dem Blatt **DocumentDB-Konten** überprüfen.

### <a id="create-multi-documentdb-account-cli-arm"></a> Erstellen eines DocumentDB-Kontos für mehrere Regionen über die Azure CLI 1.0 mit Resource Manager-Vorlagen
Mit der Anleitung in diesem Abschnitt wird beschrieben, wie Sie ein DocumentDB-Konto mithilfe einer Azure Resource Manager-Vorlage und einer optionalen Parameterdatei erstellen. In beiden Fällen handelt es sich um JSON-Dateien. Mithilfe einer Vorlage können Sie genaue Vorgaben machen und diese ohne Fehler wiederverwenden.

Erstellen Sie eine lokale Vorlagendatei mit folgendem Inhalt: Geben Sie der Datei den Namen "azuredeploy.json".

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "type": "string"
            },
            "locationName1": {
                "type": "string"
            },
            "locationName2": {
                "type": "string"
            }
        },
        "variables": {},
        "resources": [
            {
                "apiVersion": "2015-04-08",
                "type": "Microsoft.DocumentDb/databaseAccounts",
                "name": "[parameters('databaseAccountName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "databaseAccountOfferType": "Standard",
                    "ipRangeFilter": "",
                    "locations": [
                        {
                            "failoverPriority": 0,
                            "locationName": "[parameters('locationName1')]"
                        },
                        {
                            "failoverPriority": 1,
                            "locationName": "[parameters('locationName2')]"
                        }
                    ]
                }
            }
        ]
    }

Die obige Vorlagendatei kann verwendet werden, um ein DocumentDB-Konto mit zwei Regionen zu erstellen. Um das Konto mit mehr Regionen zu erstellen, fügen Sie es dem Array „locations“ und anschließend die entsprechenden Parameter hinzu.

Für eine der Regionen muss failoverPriority den Wert 0 aufweisen. Hiermit wird angegeben, dass diese Region als [Schreibregion für das DocumentDB-Konto][scaling-globally] beibehalten werden soll. Die Werte für die Failoverpriorität müssen unter den Standorten eindeutig sein, und der höchste Wert für die Failoverpriorität muss kleiner als die Gesamtzahl von Regionen sein. Sie können den Wert an der Befehlszeile eingeben oder eine Parameterdatei erstellen, um den Wert anzugeben.

Wenn Sie eine Parameterdatei erstellen möchten, kopieren Sie den folgenden Inhalt in eine neue Datei, und geben Sie ihr den Namen "azuredeploy.parameters.json". Wenn Sie den Datenbankkontonamen an der Eingabeaufforderung angeben möchten, können Sie den Vorgang ohne Erstellen dieser Datei fortsetzen.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "value": "samplearmacct"
            },
            "locationName1": {
                "value": "westus"
            },
            "locationName2": {
                "value": "eastus"
            }
        }
    }

Ändern Sie in der Datei „azuredeploy.parameters.json“ das Feld mit dem Wert `"samplearmacct"` in den gewünschten Datenbanknamen, und speichern Sie die Datei. `"databaseAccountName"` darf nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und muss zwischen 3 und 50 Zeichen lang sein. Ändern Sie die Wertfelder `"locationName1"` und `"locationName2"` in die Region, in der Sie das DocumentDB-Konto erstellen möchten.

Wenn Sie ein DocumentDB-Konto in der Ressourcengruppe erstellen möchten, führen Sie den folgenden Befehl aus, und geben Sie den Pfad der Vorlagendatei, den Pfad der Parameterdatei oder den Parameterwert, den Namen der Ressourcengruppe für die Bereitstellung und einen Bereitstellungsnamen (-n ist optional) an.

So verwenden Sie eine Parameterdatei

    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g <resourcegroupname> -n <deploymentname>

* `<PathToTemplate>` ist der Pfad zur Datei „azuredeploy.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<PathToParameterFile>` ist der Pfad zur Datei „azuredeploy.parameters.json“, die in Schritt 1 erstellt wurde. Wenn der Pfadname Leerzeichen enthält, setzen Sie diesen Parameter in doppelte Anführungszeichen.
* `<resourcegroupname>` ist der Name der vorhandenen Ressourcengruppe, der ein DocumentDB-Datenbankkonto hinzugefügt werden soll.
* `<deploymentname>` ist der optionale Name der Bereitstellung.

Beispieleingabe:

    azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json -g new_res_group -n azuredeploy

ODER führen Sie den folgenden Befehl aus, um den Namensparameter für das Datenbankkonto ohne Parameterdatei anzugeben. In diesem Fall werden Sie zur Eingabe des Werts aufgefordert:

    azure group deployment create -f <PathToTemplate> -g <resourcegroupname> -n <deploymentname>

Beispiel für Eingabeaufforderung und Eintrag eines Datenbankkontos mit dem Namen „samplearmacct“:

    azure group deployment create -f azuredeploy.json -g new_res_group -n azuredeploy
    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    databaseAccountName: samplearmacct

Bei der Bereitstellung des Kontos werden folgende Informationen angezeigt:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "azuredeploy"
    + Waiting for deployment to complete
    +
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Running
    +
    info:    Resource 'new_res_group' of type 'Microsoft.DocumentDb/databaseAccounts' provisioning status is Succeeded
    data:    DeploymentName     : azuredeploy
    data:    ResourceGroupName  : new_res_group
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-11-30T18:50:23.6300288Z
    data:    Mode               : Incremental
    data:    CorrelationId      : 4a5d4049-c494-4053-bad4-cc804d454700
    data:    DeploymentParameters :
    data:    Name                 Type    Value
    data:    -------------------  ------  ------------------
    data:    databaseAccountName  String  samplearmacct
    data:    locationName1        String  westus
    data:    locationName2        String  eastus
    info:    group deployment create command OK

Beim Auftreten von Fehlern finden Sie weitere Informationen unter [Problembehandlung](#troubleshooting).  

Nachdem der Befehl ausgeführt wurde, befindet sich das Konto einige Minuten im Zustand **Wird erstellt**, bis es in den Zustand **Online** wechselt und verwendet werden kann. Sie können den Status des Kontos im [Azure-Portal](https://portal.azure.com)auf dem Blatt **DocumentDB-Konten** überprüfen.

## <a name="troubleshooting"></a>Problembehandlung
Wenn Sie Fehlermeldungen wie `Deployment provisioning state was not successful` beim Erstellen Ihrer Ressourcengruppe oder Ihres Datenbankkontos erhalten, haben Sie einige Möglichkeiten für die Problembehandlung.

> [!NOTE]
> Enthält der Name des Datenbankkontos ungültige Zeichen oder wird ein Ort angegeben, an dem DocumentDB nicht verfügbar ist, führt das zu einem Bereitstellungsfehler. Namen von Datenbankkonten dürfen nur Kleinbuchstaben, Ziffern und den Bindestrich „-“ enthalten und müssen zwischen 3 und 50 Zeichen lang sein. Alle gültigen Orte für Datenbank-Konten sind auf der Seite [Azure Regionen](https://azure.microsoft.com/regions/#services)aufgeführt.
>
>

* Die Ausgabe enthält die folgenden `Error information has been recorded to C:\Users\wendy\.azure\azure.err`, und überprüfen Sie dann die Fehlerinformationen in der Datei „azure.err“.
* Nützliche Informationen finden Sie möglicherweise in der Protokolldatei für die Ressourcengruppe. Führen Sie den folgenden Befehl aus, um die Protokolldatei anzuzeigen:

        azure group log show <resourcegroupname> --last-deployment

    Beispieleingabe:

        azure group log show new_res_group --last-deployment

    Zusätzliche Informationen finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen in Azure](../azure-resource-manager/resource-manager-common-deployment-errors.md) .
* Fehlerinformationen sind auch im Azure-Portal verfügbar, wie im folgenden Screenshot dargestellt. So navigieren Sie zu Fehlerinformationen: Klicken Sie auf der Navigationsleiste auf "Ressourcengruppen", wählen Sie die fehlerhafte Ressourcengruppe aus, und klicken Sie dann im Bereich "Zusammenfassung" des Blatts "Ressourcengruppe" auf das Datum der letzten Bereitstellung, und wählen Sie auf dem Blatt "Bereitstellungsverlauf" die fehlerhafte Bereitstellung aus. Klicken Sie dann auf dem Blatt "Bereitstellung" auf das mit einem roten Ausrufezeichen gekennzeichnete Vorgangsdetail. Die Statusmeldung der fehlerhaften Bereitstellung wird auf dem Blatt "Vorgangsdetails" angezeigt.

    ![Screenshot des Azure-Portals mit dem Navigationspfad zur Bereitstellungsfehlermeldung](media/documentdb-automation-resource-manager-cli/portal-troubleshooting-deploy.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun über ein DocumentDB-Datenbankkonto verfügen, besteht der nächste Schritt darin, eine DocumentDB-Datenbank zu erstellen. Mit einer der folgenden Methoden können Sie eine Datenbank erstellen:

* Im Azure-Portal, wie unter [Erstellen einer DocumentDB-Sammlung und -Datenbank mit dem Azure-Portal](documentdb-create-collection.md) beschrieben
* Mit den C# .NET-Beispielen im Projekt [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) des Repositorys [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) auf GitHub.
* Die [DocumentDB-SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB verfügt über SDKs für .NET, Java, Python, Node.js und JavaScript-API.

Nach dem Erstellen der Datenbank müssen Sie der Datenbank [mindestens eine Sammlung hinzufügen](documentdb-create-collection.md) und den Sammlungen dann [Dokumente hinzufügen](documentdb-view-json-document-explorer.md).

Wenn eine Sammlung Dokumente enthält, können Sie in [DocumentDB SQL](documentdb-sql-query.md) an den Dokumenten [Abfragen ausführen](documentdb-sql-query.md#ExecutingSqlQueries), indem Sie den [Abfrage-Explorer](documentdb-query-collections-query-explorer.md) im Portal, die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eines der [SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx) verwenden.

Weitere Informationen zu DocumentDB finden Sie in folgenden Ressourcen:

* [Lernpfad für DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
* [Ressourcenmodell und Konzepte von DocumentDB](documentdb-resources.md)

Weitere Vorlagen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[distribute-globally]: https://azure.microsoft.com/en-us/documentation/articles/documentdb-distribute-data-globally
[scaling-globally]: https://azure.microsoft.com/en-us/documentation/articles/documentdb-distribute-data-globally/#scaling-across-the-planet

