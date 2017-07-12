---
title: Bereitstellen eines virtuellen Computers mit C# und einer Resource Manager-Vorlage | Microsoft Docs
description: Erfahren Sie, wie Sie mit C# und einer Resource Manager-Vorlage einen virtuellen Azure-Computer bereitstellen.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: davidmu
ms.translationtype: Human Translation
ms.sourcegitcommit: 61fd58063063d69e891d294e627ae40cb878d65b
ms.openlocfilehash: daff6ab4c0eaf17d1cb488f1c16aa111b6ed9a88
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---
<a id="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template" class="xliff"></a>

# Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage
In diesem Artikel wird gezeigt, wie Sie eine Azure Resource Manager-Vorlage mithilfe von C# bereitstellen. Mit dieser [Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json) wird ein einzelner virtueller Computer mit Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Eine ausführliche Beschreibung der Ressource des virtuellen Computers finden Sie unter [Virtuelle Computer in einer Azure Resource Manager-Vorlage](template-description.md). Weitere Informationen zu allen Ressourcen in einer Vorlage finden Sie unter [Exemplarische Vorgehensweise zu Azure Resource Manager-Vorlagen](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Die Ausführung dieser Schritte dauert etwa 10 Minuten.

<a id="step-1-create-a-visual-studio-project" class="xliff"></a>

## Schritt 1: Erstellen eines Visual Studio-Projekts

In diesem Schritt stellen Sie sicher, dass Visual Studio installiert ist, und erstellen eine Konsolenanwendung, die zum Bereitstellen der Vorlage verwendet wird.

1. Wenn Sie dies noch nicht getan haben, installieren Sie [Visual Studio](https://www.visualstudio.com/).
2. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
3. Wählen Sie unter **Vorlagen** > **Visual C#** die Option **Konsolen-App (.NET Framework)** aus, geben Sie Name und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.

<a id="step-2-install-libraries" class="xliff"></a>

## Schritt 2: Installieren von Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Ausführen dieser Schritte benötigen. Sie benötigen die Azure Resource Manager-Bibliothek und die Active Directory-Authentifizierungsbibliothek, um die Ressourcen zu erstellen. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und klicken Sie auf **NuGet-Pakete für Projektmappe verwalten...** und dann auf **Durchsuchen**.
2. Geben Sie *Microsoft.IdentityModel.Clients.ActiveDirectory* in das Suchfeld ein, wählen Sie Ihr Projekt aus, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
3. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen**aus. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

<a id="step-3-create-credentials-used-to-authenticate-requests" class="xliff"></a>

## Schritt 3: Erstellen der Anmeldeinformationen zum Authentifizieren von Anforderungen

Bevor Sie mit diesem Schritt beginnen, stellen Sie sicher, dass Sie Zugriff auf einen [Active Directory-Dienstprinzipal](../../resource-group-authenticate-service-principal.md) haben. Vom Dienstprinzipal erhalten Sie ein Token zum Authentifizieren von Anforderungen an Azure Resource Manager.

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie die folgenden using-Anweisungen dann am Anfang der Datei den vorhandenen Anweisungen hinzu:

    ```
    using Microsoft.Azure;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Rest;
    using System.IO;
    ```

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:

    ```
    private static async Task<AuthenticationResult> GetAccessTokenAsync()
    {
      var cc = new ClientCredential("client-id", "client-secret");
      var context = new AuthenticationContext("https://login.microsoftonline.com/tenant-id");
      var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
      if (token == null)
      {
        throw new InvalidOperationException("Could not get the token.");
      } 
      return token;
    }
    ```

    Ersetzen Sie die folgenden Werte:
    
    - *client-id* mit dem Bezeichner der Azure Active Directory-Anwendung. Diesen Bezeichner finden Sie auf dem Blatt „Eigenschaften“ der AD-Anwendung. Um die AD-Anwendung im Azure-Portal zu suchen, klicken Sie im Ressourcenmenü auf **Azure Active Directory**, und klicken Sie dann auf **App-Registrierungen**.
    - *client-secret* mit dem Zugriffsschlüssel der AD-Anwendung. Diesen Bezeichner finden Sie auf dem Blatt „Eigenschaften“ der AD-Anwendung.
    - *tenant-id* mit der Mandanten-ID Ihres Abonnements. Die Mandanten-ID finden Sie auf dem Blatt „Eigenschaften“ für Azure Active Directory im Azure-Portal. Sie ist als *Verzeichnis-ID* bezeichnet.

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    var token = GetAccessTokenAsync();
    var credential = new TokenCredentials(token.Result.AccessToken);
    ```

4. Speichern Sie die Datei "Program.cs".

<a id="step-4-create-a-resource-group" class="xliff"></a>

## Schritt 4: Erstellen einer Ressourcengruppe

Auch wenn Sie eine Ressourcengruppe aus einer Vorlage erstellen können, wird mit der aus dem Katalog ausgewählten Vorlage keine Ressourcengruppe erstellt. In diesem Schritt fügen Sie den Code zum Erstellen einer Ressourcengruppe hinzu.

1. Um Werte für die Anwendung anzugeben, fügen Sie Variablen für die Main-Methode der Program-Klasse hinzu:

    ```
    var groupName = "myResourceGroup";
    var subscriptionId = "subsciptionId";
    var deploymentName = "deploymentName";
    var location = "location";
    ```

    Ersetzen Sie die folgenden Werte:
    
    - *myResourceGroup* durch den Namen der Ressourcengruppe, die erstellt wird
    - *subscriptionId* mit Ihrer Abonnement-ID. Die Abonnement-ID finden Sie auf dem Blatt „Abonnements“ im Azure-Portal.
    - *deploymentName* durch den Namen der Bereitstellung
    - *location* durch die [Azure-Region](https://azure.microsoft.com/regions/), in der Sie die Ressourcen erstellen möchten

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu erstellen:

    ```
    public static async Task<ResourceGroup> CreateResourceGroupAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId,
      string location)
    {
      var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };

      Console.WriteLine("Creating the resource group...");
      var resourceGroup = new ResourceGroup { Location = location };
      return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(
        groupName, 
        resourceGroup);
    }
    ```

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    var rgResult = CreateResourceGroupAsync(
      credential,
      groupName,
      subscriptionId,
      location);
    Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
    Console.ReadLine();
    ```

<a id="step-5-create-a-parameters-file" class="xliff"></a>

## Schritt 5: Erstellen einer Parameterdatei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, müssen Sie eine Parameterdatei erstellen, die die Werte enthält. Die Parameterdatei wird verwendet, wenn Sie die Vorlage bereitstellen. Die Vorlage aus dem Katalog, die Sie verwenden, erwartet Werte für die Parameter *adminUserName*, *adminPassword* und *dnsLabelPrefix*.

Führen Sie in Visual Studio folgende Schritte aus:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und klicken Sie dann auf **Hinzufügen** > **Neues Element**.
2. Klicken Sie auf **Web**, wählen Sie **JSON-Datei** aus, geben Sie *Parameters.json* als Namen ein, und klicken Sie dann auf **Hinzufügen**.
3. Öffnen Sie die Datei „parameters.json“, und fügen Sie den folgenden JSON-Inhalt hinzu:

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "mytestacct1" },
        "adminPassword": { "value": "mytestpass1" },
        "dnsLabelPrefix": { "value": "mydns1" }
      }
    }
    ```

    Ersetzen Sie die Parameterwerte durch Werte, die in Ihrer Umgebung funktionieren.

4. Speichern Sie die Parameters.json-Datei.

<a id="step-6-deploy-a-template" class="xliff"></a>

## Schritt 6: Bereitstellen einer Vorlage

In diesem Beispiel müssen Sie eine Vorlage aus dem Azure-Vorlagenkatalog bereitstellen und Parameterwerte aus der lokalen Datei übermitteln, die Sie erstellt haben. 

1. Fügen Sie zum Bereitstellen der Vorlage die folgende Methode der Program-Klasse hinzu:

    ```
    public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
      TokenCredentials credential,
      string groupName,
      string deploymentName,
      string subscriptionId)
    {
    
      var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };

      Console.WriteLine("Creating the template deployment...");
      var deployment = new Deployment();
      deployment.Properties = new DeploymentProperties
        {
          Mode = DeploymentMode.Incremental,
          TemplateLink = new TemplateLink("https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json"),
          Parameters = File.ReadAllText("..\\..\\Parameters.json")
        };
      
      return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
        groupName,
        deploymentName,
        deployment
      );
    }
    ```

    Sie können auch eine Vorlage aus einem lokalen Ordner bereitstellen, indem Sie die Template-Eigenschaft anstelle der TemplateLink-Eigenschaft verwenden.

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    var dpResult = CreateTemplateDeploymentAsync(
      credential,
      groupName,
      deploymentName,
      subscriptionId);
    Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
    Console.ReadLine();
    ```

<a id="step-7-delete-the-resources" class="xliff"></a>

## Schritt 7: Löschen der Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Sie müssen nicht jede Ressource einzeln aus einer Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden alle darin enthaltenen Ressourcen automatisch gelöscht.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:

   ```
   public static async void DeleteResourceGroupAsync(
     TokenCredentials credential,
     string groupName,
     string subscriptionId)
   {
     Console.WriteLine("Deleting resource group...");
     var resourceManagementClient = new ResourceManagementClient(credential)
       { SubscriptionId = subscriptionId };
     await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
   }
   ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

   ```
   DeleteResourceGroupAsync(
     credential,
     groupName,
     subscriptionId);
   Console.ReadLine();
   ```

<a id="step-8-run-the-console-application" class="xliff"></a>

## Schritt 8: Ausführen der Konsolenanwendung

Die vollständige Ausführung dieser Konsolenanwendung sollte etwa fünf Minuten dauern. 

1. Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit denselben Anmeldeinformationen an, die Sie für Ihr Abonnement verwenden.

2. Wenn der Status *Erfolgreich* angezeigt wird, drücken Sie die **EINGABETASTE** . 

    **1 Erfolgreich** wird auch im Azure-Portal auf dem Blatt „Übersicht“ für Ihre Ressourcengruppe unter „Bereitstellungen“ angezeigt.

3. Bevor Sie die **EINGABETASTE** drücken, um das Löschen der Ressourcen zu starten, können Sie sich ein paar Minuten Zeit nehmen, um die Erstellung der Ressourcen im Azure-Portal zu überprüfen. Klicken Sie auf den Bereitstellungsstatus, um Informationen zur Bereitstellung anzuzeigen.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Falls bei der Bereitstellung Probleme aufgetreten sind, sollten Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../../resource-manager-common-deployment-errors.md) lesen.
* Informationen zum Bereitstellen einer virtuellen Maschine und deren unterstützende Ressourcen finden Sie unter [Bereitstellen eines virtuellen Azure-Computers mit C#](csharp.md).

