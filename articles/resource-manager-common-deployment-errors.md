<properties
   pageTitle="Häufige Azure-Bereitstellungsfehler | Microsoft Azure"
   description="Beschreibt, wie Sie häufige Fehler bei der Bereitstellung mit Azure Resource Manager beheben können."
   services="azure-resource-manager"
   documentationCenter=""
   tags=""
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="03/24/2016"
   ms.author="tomfitz"/>

# Beheben von häufigen Fehler beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager

In diesem Thema wird beschrieben, wie Sie einige der häufigsten Fehler beheben können, die beim Bereitstellen von Ressourcen in Azure auftreten können. Informationen zur Problembehandlung für eine Bereitstellung finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen](resource-manager-troubleshoot-deployments-portal.md).

Einige Fehler lassen sich vermeiden, indem Sie Ihre Vorlage und die Parameter vor der Bereitstellung überprüfen. Beispiele zur Überprüfung der Vorlage finden Sie unter [Bereitstellen einer Ressource mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md).

## Der Ressourcenname ist bereits vorhanden

Bei einigen Ressourcen, insbesondere Speicherkonten, Datenbankservern und Websites, müssen Sie einen im gesamten Azure-Dienst eindeutigen Namen für die Ressource angeben. Sie können einen eindeutigen Namen erstellen, indem Sie die Benennungskonvention mit dem Ergebnis der [uniqueString](./resource-group-template-functions/#uniquestring)-Funktion verketten.
 
    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 

## Die Ressource kann während der Bereitstellung nicht gefunden werden

Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach der anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden, um eine Abhängigkeit zur anderen Ressource herzustellen. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Wenn Sie versuchen, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie können diesen Fehler verhindern, indem Sie die Abhängigkeit in der Web-App festlegen.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }

## Speicherort für die Ressource nicht verfügbar

Verwenden Sie beim Angeben eines Standorts für eine Ressource einen von der Ressource unterstützten Standort. Bevor Sie einen Standort für eine Ressource eingeben, überprüfen Sie mit einem der folgenden Befehle, ob der Standort den Ressourcentyp unterstützt.

### PowerShell

Verwenden Sie **Get-AzureRmResourceProvider**, um unterstützte Typen und Speicherorte für einen bestimmten Ressourcenanbieter abzurufen.

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web

Sie erhalten eine Liste der Ressourcentypen für diesen Ressourcenanbieter.

    ProviderNamespace RegistrationState ResourceTypes               Locations
    ----------------- ----------------- -------------               ---------
    Microsoft.Web     Registered        {sites/extensions}          {Brazil South, ...
    Microsoft.Web     Registered        {sites/slots/extensions}    {Brazil South, ...
    Microsoft.Web     Registered        {sites/instances}           {Brazil South, ...
    ...

Verwenden Sie Folgendes, um sich auf die Speicherorte für einen bestimmten Ressourcentyp zu konzentrieren:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Dies gibt die unterstützten Speicherorte zurück:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia

### Azure-Befehlszeilenschnittstelle

Verwenden Sie bei der Azure-Befehlszeilenschnittstelle den Befehl **azure location list**. Da die Liste der Speicherorte lang sein kann und es viele Anbieter gibt, können Sie Tools verwenden, um Anbieter und Speicherorte zu überprüfen, bevor Sie einen Speicherort nutzen, der noch nicht zur Verfügung steht. Das folgende Skript ermittelt mithilfe von **jq** die Standorte, an denen der Ressourcenanbieter für virtuelle Azure-Computer verfügbar ist.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Compute/virtualMachines")'
    
Dies gibt die unterstützten Speicherorte zurück:
    
    {
      "name": "Microsoft.Compute/virtualMachines",
      "location": "East US,East US 2,West US,Central US,South Central US,North Europe,West Europe,East Asia,Southeast Asia,Japan East,Japan West"
    }

### REST-API

Informationen zur REST-API finden Sie unter [Abrufen von Informationen zu einem Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790534.aspx).

## Das Kontingent wurde überschritten

Probleme können auftreten, wenn eine Bereitstellung ein Kontingent überschreitet (etwa für eine Ressourcengruppe, ein Abonnement, ein Konto oder Ähnliches). Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr Kernen als der zulässigen Anzahl bereitzustellen, erhalten Sie eine Fehlermeldung, die darauf hinweist, dass das Kontingent überschritten wurde. Die vollständigen Kontingentinformationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](./azure-subscription-service-limits.md).

Um die Kontingente Ihres eigenen Abonnements für Kerne zu untersuchen, verwenden Sie den `azure vm list-usage`-Befehl in der Azure-CLI. Im folgenden Beispiel wird veranschaulicht, dass das Kernkontingent für ein kostenloses Testkonto 4 ist:

    azure vm list-usage
    
Ausgabe des Befehls:
    
    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Wenn Sie versuchen, eine Vorlage bereitzustellen, die mehr als vier Kerne in der Region "Westen der USA" des oben aufgeführten Abonnements erstellt, wird eine Bereitstellungs-Fehlermeldung angezeigt, die etwa wie folgt aussieht (im Portal oder durch die Untersuchung der Bereitstellungsprotokolle):

    statusCode:Conflict
    serviceRequestId:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    statusMessage:{"error":{"code":"OperationNotAllowed","message":"Operation results in exceeding quota limits of Core. Maximum allowed: 4, Current in use: 4, Additional requested: 2."}}

In PowerShell können Sie alternativ das **Get-AzureRmVMUsage**-Cmdlet verwenden.

    Get-AzureRmVMUsage
    
Ausgabe des Befehls:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In diesen Fällen sollten Sie zum Portal navigieren und ein Supportproblem einreichen, um Ihr Kontingent für die Region, in der Sie diese bereitstellen möchten, zu erhöhen.

> [AZURE.NOTE] Denken Sie daran, dass für Ressourcengruppen das Kontingent für jede einzelne Region und nicht für das gesamte Abonnement gilt. Wenn Sie 30 Kerne in der Region "USA, Westen" bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne für "USA, Westen" anfordern. Wenn Sie 30 Kerne in allen Regionen, auf die Sie Zugriff haben, bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne in allen Regionen anfordern.


## Fehler bei der Autorisierung

Möglicherweise wird während der Bereitstellung ein Fehler angezeigt, da das Konto oder ein Dienstprinzipal, der versucht die Ressourcen bereitzustellen, keinen Zugriff zum Ausführen dieser Aktionen hat. Mit Azure Active Directory können Sie oder Ihr Administrator sehr genau kontrollieren, welche Identitäten auf welche Ressourcen Zugriff haben. Wenn Ihr Konto der Leserrolle zugewiesen ist, kann es beispielsweise keine neuen Ressourcen erstellen. In diesem Fall wird eine Fehlermeldung angezeigt, die darauf hinweist, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md).

Neben rollenbasierter Zugriffskontrolle können Ihre Bereitstellungsaktionen von Richtlinien für das Abonnement eingeschränkt werden. Durch Richtlinien kann der Administrator Konventionen für alle Ressourcen durchsetzen, die in dem Abonnement bereitgestellt sind. Ein Administrator kann beispielsweise fordern, dass ein bestimmter Tagwert für einen Ressourcentyp angegeben wird. Wenn Sie die Richtlinienanforderungen nicht erfüllt haben, wird während der Bereitstellung ein Fehler ausgegeben. Weitere Einzelheiten zu Richtlinien finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](./resource-manager-policy.md).

## Überprüfen der Ressourcenanbieterregistrierung

Ressourcen werden von Ressourcenanbietern verwaltet, und ein Konto oder Abonnement muss für die Verwendung eines bestimmten Anbieters registriert werden. Die meisten Anbieter werden automatisch über das Azure-Portal oder die verwendete Befehlszeilenschnittstelle registriert.

### PowerShell

Verwenden Sie zum Anzeigen des Registrierungsstatus **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Dies gibt alle verfügbaren Ressourcenanbieter und Ihren Registrierungsstatus zurück:

    ProviderNamespace               RegistrationState ResourceTypes
    -----------------               ----------------- -------------
    Microsoft.ApiManagement         Unregistered      {service, validateServiceName, checkServiceNameAvailability}
    Microsoft.AppService            Registered        {apiapps, appIdentities, gateways, deploymenttemplates...}
    Microsoft.Batch                 Registered        {batchAccounts}

Um einen Anbieter zu registrieren, verwenden Sie **Register-AzureRmResourceProvider**, und geben Sie den Namen des zu registrierenden Ressourcenanbieters an.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Sie werden aufgefordert, die Registrierung zu bestätigen, und dann wird ein Status zurückgegeben.

    Confirm
    Are you sure you want to register the provider 'Microsoft.Cdn'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

    ProviderNamespace RegistrationState ResourceTypes
    ----------------- ----------------- -------------
    Microsoft.Cdn     Registering       {profiles, profiles/endpoints,...

### Azure-Befehlszeilenschnittstelle

Um mithilfe der Azure-Befehlszeilenschnittstelle zu sehen, ob der Anbieter zur Nutzung registriert ist, verwenden Sie den `azure provider list` Befehl (es folgt ein gekürztes Beispiel für eine Ausgabe).

    azure provider list
        
Dies gibt alle verfügbaren Ressourcenanbieter und Ihren Registrierungsstatus zurück:
        
    info:    Executing command provider list
    + Getting ARM registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.Compute                Registered
    data:    Microsoft.Network                Registered  
    data:    Microsoft.Storage                Registered
    data:    microsoft.visualstudio           Registered
    ...
    info:    provider list command OK

Um einen Ressourcenanbieter zu registrieren, verwenden Sie den `azure provider register`-Befehl, und geben Sie den zu registrierenden *Namespace* an.

    azure provider register Microsoft.Cdn

### REST-API

Informationen zum Registrierungsstatus finden Sie unter [Abrufen von Informationen zu einem Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790534.aspx).

Informationen zum Registrieren eines Anbieters finden Sie unter [Registrieren eines Abonnements bei einem Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790548.aspx).

## Fehler der benutzerdefinierten Skripterweiterung

Bei einem Fehler für eine benutzerdefinierte Skripterweiterung beim Bereitstellen eines virtuellen Computers siehe [Problembehandlung bei Fehlern im Zusammenhang mit Azure Windows-VM-Erweiterungen](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md) oder [Problembehandlung für Fehler bei Azure-Erweiterungen für virtuelle Linux-Computer](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).

## Verstehen, wann eine Bereitstellung bereit ist 

Azure Resource Manager meldet eine erfolgreiche Bereitstellung, wenn alle Anbieter erfolgreich von der Bereitstellung zurückgegeben werden. Dies bedeutet jedoch nicht zwangsläufig, dass Ihre Ressourcengruppe aktiv ist und für Benutzer bereitsteht. Zum Beispiel muss eine Bereitstellung Upgrades herunterladen, auf andere Ressourcen ohne Vorlagen warten oder komplexe Skripte oder andere ausführbare Aktivitäten installieren, die Azure nicht kennt, da es sich um keine Aktivität handelt, die ein Anbieter überwacht. In diesen Fällen kann einige Zeit vergehen, bis Ihre Ressourcen tatsächlich nutzungsbereit sind. Daher sollten Sie davon ausgehen, dass der Bereitstellungsstatus einige Zeit bevor Ihre Bereitstellung genutzt werden kann, erfolgreich ist.

Sie können jedoch Azure daran hindern, einen erfolgreichen Bereitstellungsstatus zu melden, indem Sie ein benutzerdefiniertes Skript für Ihre benutzerdefinierte Vorlage erstellen -- beispielsweise mithilfe der [CustomerScriptExtension](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/) -- das weiß, wie die gesamte Bereitstellung für eine systemweite Einsatzbereitschaft zu überwachen ist und nur erfolgreich zurückkehrt, wenn Benutzer mit der gesamten Bereitstellung interagieren können. Wenn Sie sicherstellen möchten, dass Ihre Erweiterung die letzte ist, die ausgeführt wird, verwenden Sie die **dependsOn**-Eigenschaft in Ihrer Vorlage.

## Nächste Schritte

- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](./resource-group-audit.md).
- Weitere Informationen zu Aktionen zum Bestimmen der Fehler während der Bereitstellung finden Sie unter [Problembehandlung von Ressourcengruppenbereitstellungen](./resource-manager-troubleshoot-deployments-portal.md).

<!---HONumber=AcomDC_0330_2016-->