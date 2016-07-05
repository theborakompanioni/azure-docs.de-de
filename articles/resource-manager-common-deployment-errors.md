<properties
   pageTitle="Problembehandlung bei häufigen Azure-Bereitstellungsfehlern | Microsoft Azure"
   description="Beschreibt, wie Sie häufige Fehler bei der Bereitstellung mit Azure Resource Manager beheben können."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# Problembehandlung bei häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager

In diesem Thema wird beschrieben, wie Sie einige allgemeine Fehler beheben können, die beim Bereitstellen von Ressourcen in Azure auftreten können. Falls Sie weitere Einzelheiten zum Bereitstellungsfehler benötigen, lesen Sie zunächst [Anzeigen von Bereitstellungsvorgängen](resource-manager-troubleshoot-deployments-portal.md). Kehren Sie anschließend zu diesem Artikel zurück, um Unterstützung beim Beheben des Fehlers zu erhalten.

## Ungültige Vorlage oder Ressource

Wenn Sie eine Fehlermeldung erhalten, dass die Vorlage oder eine Eigenschaft für eine Ressource ungültig ist, fehlt in Ihrer Vorlage möglicherweise ein Zeichen. Dieser Fehler passiert schnell, wenn Sie Vorlagenausdrücke verwenden, da der Ausdruck in Anführungszeichen steht, sodass die Überprüfung durch JSON erfolgt, der Editor den Fehler aber möglicherweise nicht erkennt. Beispielsweise enthält die folgenden Namenszuweisung für ein Speicherkonto Klammern, drei Funktionen, drei Sätze mit Klammern, einen Satz mit einfachen Anführungszeichen und eine Eigenschaft:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Wenn Sie nicht die entsprechende Syntax bereitstellen, wird die Vorlage einen Wert erstellen, der sich von Ihrer Absicht stark unterscheidet.

Je nachdem, wo sich das fehlende Zeichen in Ihrer Vorlage befindet, erhalten Sie eine Fehler, dass die Vorlage oder eine Ressource ungültig ist. Der Fehler kann auch darauf angeben, dass der Bereitstellungsprozess den Vorlagensprachausdruck nicht verarbeiten konnte. Wenn Sie diese Art von Fehler erhalten, überprüfen Sie sorgfältig die Ausdruckssyntax.

## Der Ressourcenname ist bereits vorhanden oder wird bereits von einer anderen Ressource verwendet.

Bei einigen Ressourcen, insbesondere Speicherkonten, Datenbankservern und Websites, müssen Sie einen im gesamten Azure-Dienst eindeutigen Namen für die Ressource angeben. Sie können einen eindeutigen Namen erstellen, indem Sie die Benennungskonvention mit dem Ergebnis der [uniqueString](resource-group-template-functions.md#uniquestring)-Funktion verketten.
 
    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 

## Die Ressource kann während der Bereitstellung nicht gefunden werden

Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden, um eine Abhängigkeit zur anderen Ressource herzustellen. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Wenn Sie versuchen, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie können diesen Fehler verhindern, indem Sie die Abhängigkeit in der Web-App festlegen.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }

## Member „copy“ wurde für das Objekt nicht gefunden.

Dieser Fehler tritt auf, wenn Sie das Element **copy** auf einen Teil der Vorlage angewendet haben, der dieses Element nicht unterstützt. Sie können das copy-Element nur auf einen Ressourcentyp anwenden. Sie können „copy“ nicht auf eine Eigenschaft in einem Ressourcentyp anwenden. Beispiel: Sie wenden „copy“ auf einen virtuellen Computer an, können das Element aber nicht auf die Betriebssystem-Datenträger für einen virtuellen Computer anwenden. In einigen Fällen können Sie eine untergeordnete Ressource in eine übergeordnete Ressource umwandeln, um eine Kopierschleife zu erstellen. Weitere Informationen zur Verwendung von „copy“ finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md).

## SKU nicht verfügbar

Beim Bereitstellen einer Ressource (in der Regel ein virtueller Computer) werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

    Code: SkuNotAvailable
    Message: The requested tier for resource '<resource>' is currently not available in location '<location>' for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.

Sie erhalten diesen Fehler, wenn die ausgewählte Ressourcen-SKU (z.B. die Größe des virtuellen Computers) für den ausgewählten Standort nicht verfügbar ist. Sie haben zwei Optionen, dieses Problem zu beheben:

1.	Melden Sie sich beim Portal an, und beginnen Sie mit dem Hinzufügen einer neuen Ressource über die UI. Beim Festlegen der Werte werden die verfügbaren SKUs für die Ressource angezeigt. 

    ![Verfügbare SKUs](./media/resource-manager-common-deployment-errors/view-sku.png)

2.	Wenn Sie keine geeignete SKU in dieser Region oder einer anderen Region finden, die Ihre Geschäftsanforderungen erfüllt, wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support).


## Kein registrierter Anbieter gefunden

Beim Bereitstellen von Ressourcen werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

    Dode: NoRegisteredProviderFound
    Message: No registered resource provider found for location '<location>' and API version '<api-version>' for type '<resource-type>'.

Für diesen Fehler gibt es drei Gründe:

1. Der Standort wird für den Ressourcentyp nicht unterstützt.
2. Die API-Version wird für den Ressourcentyp nicht unterstützt.
3. Der Ressourcenanbieter wurde für Ihr Abonnement nicht registriert.

Die Fehlermeldung enthält in der Regel Vorschläge für die unterstützten Standorte und API-Versionen. Sie können Ihre Vorlage in einen der vorgeschlagenen Werte ändern. Die meisten Anbieter werden automatisch über das Azure-Portal oder die verwendete Befehlszeilenschnittstelle registriert. Wenn Sie zuvor noch keinen bestimmten Ressourcenanbieter verwendet haben, müssen Sie diesen Anbieter unter Umständen registrieren. Mit den folgenden Befehlen können Sie weitere Informationen zu Ressourcenanbietern anzeigen.

### PowerShell

Verwenden Sie zum Anzeigen des Registrierungsstatus **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Um einen Anbieter zu registrieren, verwenden Sie **Register-AzureRmResourceProvider**, und geben Sie den Namen des zu registrierenden Ressourcenanbieters an.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Verwenden Sie zum Abrufen der unterstützten Standorte für einen bestimmten Ressourcentyp Folgendes:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Verwenden Sie zum Abrufen der unterstützten API-Versionen für einen bestimmten Ressourcentyp Folgendes:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### Azure-Befehlszeilenschnittstelle

Mit dem Befehl `azure provider list` können Sie feststellen, ob der Anbieter registriert ist.

    azure provider list
        
Um einen Ressourcenanbieter zu registrieren, verwenden Sie den Befehl `azure provider register`, und geben Sie den zu registrierenden *Namespace* an.

    azure provider register Microsoft.Cdn

Verwenden Sie zum Anzeigen der unterstützten Standorte und API-Versionen für einen Ressourcenanbieter Folgendes:

    azure provider show -n Microsoft.Compute --json > compute.json
    
## Das Kontingent wurde überschritten

Probleme können auftreten, wenn eine Bereitstellung ein Kontingent überschreitet (etwa für eine Ressourcengruppe, ein Abonnement, ein Konto oder Ähnliches). Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr Kernen als der zulässigen Anzahl bereitzustellen, erhalten Sie eine Fehlermeldung, die darauf hinweist, dass das Kontingent überschritten wurde. Die vollständigen Kontingentinformationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](azure-subscription-service-limits.md).

Zum Untersuchen der Kontingente Ihres eigenen Abonnements für Kerne können Sie den Befehl `azure vm list-usage` in der Azure-CLI verwenden. Im folgenden Beispiel wird veranschaulicht, dass das Kernkontingent für ein kostenloses Testkonto 4 ist:

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

In PowerShell können Sie alternativ das Cmdlet **Get-AzureRmVMUsage** verwenden.

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

> [AZURE.NOTE] Denken Sie daran, dass für Ressourcengruppen das Kontingent für jede einzelne Region und nicht für das gesamte Abonnement gilt. Wenn Sie 30 Kerne in der Region "USA, Westen" bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne für "USA, Westen" anfordern. Wenn Sie 30 Kerne in allen Regionen, auf die Sie Zugriff haben, bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne in allen Regionen anfordern.


## Fehler bei der Autorisierung

Möglicherweise wird während der Bereitstellung ein Fehler angezeigt, da das Konto oder ein Dienstprinzipal, der versucht die Ressourcen bereitzustellen, keinen Zugriff zum Ausführen dieser Aktionen hat. Mit Azure Active Directory können Sie oder Ihr Administrator sehr genau kontrollieren, welche Identitäten auf welche Ressourcen Zugriff haben. Wenn Ihr Konto der Leserrolle zugewiesen ist, kann es beispielsweise keine neuen Ressourcen erstellen. In diesem Fall wird eine Fehlermeldung angezeigt, die darauf hinweist, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](./active-directory/role-based-access-control-configure.md).

Neben rollenbasierter Zugriffssteuerung können Ihre Bereitstellungsaktionen auch durch Richtlinien für das Abonnement eingeschränkt werden. Durch Richtlinien kann der Administrator Konventionen für alle Ressourcen durchsetzen, die in dem Abonnement bereitgestellt sind. Ein Administrator kann beispielsweise fordern, dass ein bestimmter Tagwert für einen Ressourcentyp angegeben wird. Wenn Sie die Richtlinienanforderungen nicht erfüllt haben, wird während der Bereitstellung ein Fehler ausgegeben. Weitere Informationen zu Richtlinien finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs](resource-manager-policy.md).

## Problembehandlung bei virtuellen Computern 

| Error | Artikel |
| -------- | ----------- |
| Fehler der benutzerdefinierten Skripterweiterung | [Problembehandlung bei Fehlern im Zusammenhang mit Azure Windows-VM-Erweiterungen](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />oder<br />[Problembehandlung für Fehler bei Azure-Erweiterungen für virtuelle Linux-Computer](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) | 
| Fehler bei der Bereitstellung des Betriebssystemimage | [Behandeln von Problemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure (Resource Manager-Bereitstellungsmodell)](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />oder<br />[Behandeln von Ressourcen-Manager-Bereitstellungsproblemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md) | 
| Fehler bei der Zuordnung | [Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Windows-Computern in Azure](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />oder<br />[Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Linux-Computern in Azure](./virtual-machines/virtual-machines-linux-allocation-failure.md) | 
| Secure Shell (SSH)-Fehler beim Herstellen einer Verbindung | [Problembehandlung für SSH-Verbindungen (Secure Shell) mit einem Linux-basierten virtuellen Azure-Computer](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) | 
| Fehler beim Herstellen einer Verbindung mit einer Anwendung, die auf einem virtuellen Computer ausgeführt wird | [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird (Windows)](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />oder<br />[Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird (Linux)](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) | 
| Fehler bei Remotedesktopverbindungen | [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) | 
| Verbindungsfehler, die durch eine erneute Bereitstellung behoben werden | [Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) | 
| Clouddienstfehler | [Behandeln von Problemen mit der Clouddienstbereitstellung](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) | 

## Problembehandlung bei anderen Diensten 

Die folgende Tabelle ist keine vollständige Liste der Problembehandlung für Azure. Stattdessen konzentriert sie sich auf Probleme im Zusammenhang mit der Bereitstellung oder Konfiguration von Ressourcen. Wenn Sie Hilfe bei der Problembehandlung von Laufzeitproblemen mit einer Ressource benötigen, lesen Sie die Informationen in der Dokumentation für den jeweiligen Azure-Dienst.

| Dienst | Artikel |
| -------- | -------- |
| Automation | [Tipps zur Problembehandlung für häufige Fehler in Azure Automation](./automation/automation-troubleshooting-automation-errors.md) | 
| Azure Stack | [Microsoft Azure Stack troubleshooting](./azure-stack/azure-stack-troubleshooting.md) (Problembehandlung für Microsoft Azure Stack) | 
| Azure Stack | [Web Apps and Azure Stack](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md) (Web-Apps und Azure Stack) | 
| Data Factory | [Problembehandlung bei Data Factory](./data-factory/data-factory-troubleshoot.md) | 
| Service Fabric | [Häufig auftretende Probleme und Problembehandlung beim Bereitstellen von Diensten in Azure Service Fabric](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) | 
| Site Recovery | [Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Speicher | [Microsoft Azure-Speicher: Überwachung, Diagnose und Problembehandlung](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Beheben von Problemen mit der Bereitstellung von StorSimple-Geräten](./storsimple/storsimple-troubleshoot-deployment.md) | 
| SQL-Datenbank | [Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](./sql-database/sql-database-troubleshoot-common-connection-issues.md) | 
| SQL Data Warehouse | [Problembehandlung bei Azure SQL Data Warehouse](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) | 

## Verstehen, wann eine Bereitstellung bereit ist 

Azure Resource Manager meldet eine erfolgreiche Bereitstellung, wenn alle Anbieter erfolgreich von der Bereitstellung zurückgegeben werden. Dies bedeutet jedoch nicht zwangsläufig, dass Ihre Ressourcengruppe aktiv ist und für Benutzer bereitsteht. Zum Beispiel muss eine Bereitstellung Upgrades herunterladen, auf andere Ressourcen ohne Vorlagen warten oder komplexe Skripte oder andere ausführbare Aktivitäten installieren, die Azure nicht kennt, da es sich um keine Aktivität handelt, die ein Anbieter überwacht. In diesen Fällen kann einige Zeit vergehen, bis Ihre Ressourcen tatsächlich nutzungsbereit sind. Daher sollten Sie davon ausgehen, dass der Bereitstellungsstatus einige Zeit bevor Ihre Bereitstellung genutzt werden kann, erfolgreich ist.

Sie können jedoch Azure daran hindern, einen erfolgreichen Bereitstellungsstatus zu melden, indem Sie ein benutzerdefiniertes Skript für Ihre benutzerdefinierte Vorlage erstellen -- beispielsweise mithilfe der [CustomerScriptExtension](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/) -- das weiß, wie die gesamte Bereitstellung für eine systemweite Einsatzbereitschaft zu überwachen ist und nur erfolgreich zurückkehrt, wenn Benutzer mit der gesamten Bereitstellung interagieren können. Wenn Sie sicherstellen möchten, dass Ihre Erweiterung die letzte ist, die ausgeführt wird, verwenden Sie die **dependsOn**-Eigenschaft in Ihrer Vorlage.

## Nächste Schritte

- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
- Weitere Informationen zu Aktionen zum Bestimmen der Fehler während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen](resource-manager-troubleshoot-deployments-portal.md).

<!---HONumber=AcomDC_0622_2016-->