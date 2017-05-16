---
title: "Azure-Protokollintegration – Häufig gestellte Fragen | Microsoft Docs"
description: "In diesen häufig gestellten Fragen finden Sie Antworten zur Azure-Protokollintegration."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2017
ms.author: TomSh
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: f539fc2945b9c6646660d50713d11dd7d822d06f
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Azure-Protokollintegration – Häufig gestellte Fragen
In diesen häufig gestellten Fragen finden Sie Antworten zur Azure-Protokollintegration. Hierbei handelt es sich um einen Dienst, mit dem Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren können. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Ressourcen (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse im Zusammenhang mit Ihren Anwendungen aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

## <a name="is-the-azure-log-integration-software-free"></a>Ist die Integration der Software für die Azure-Protokollintegration kostenlos?
Ja. Es fallen keine Gebühren für die Integration der Software für die Azure-Protokollintegration an.

## <a name="where-is-azure-log-integration-available"></a>Wo ist die Azure-Protokollintegration erhältlich?

Derzeit ist sie im kommerziellen Azure sowie Azure Government erhältlich, in China und Deutschland ist sie nicht verfügbar.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Wie kann ich die Speicherkonten anzeigen, aus denen die Azure-Protokollintegration Azure-VM-Protokolle bezieht?
Führen Sie den Befehl **azlog source list**aus.

## <a name="how-can-i-update-the-proxy-configuration"></a>Wie kann ich die Proxykonfiguration aktualisieren?
Falls Ihre Proxyeinstellung keinen direkten Azure-Speicherzugriff ermöglicht, öffnen Sie die Datei **AZLOG.EXE.CONFIG** (im Verzeichnis **C:\Programme\Microsoft Azure-Protokollintegration**). Fügen Sie in der Datei den Abschnitt **defaultProxy** mit der Proxyadresse Ihrer Organisation hinzu. Beenden Sie anschließend den Dienst mithilfe des Befehls **net stop azlog**, und starten Sie ihn mithilfe des Befehls **net start azlog** wieder.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Wie kann ich die Abonnementinformationen in Windows-Ereignissen anzeigen?
Fügen Sie beim Hinzufügen der Quelle die Abonnement-ID ( **subscriptionid** ) an den Anzeigenamen an.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
Der XML-Code des Ereignisses enthält die folgenden Metadaten – einschließlich der Abonnement-ID:

![Ereignis-XML][1]

## <a name="error-messages"></a>Fehlermeldungen
### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Warum tritt beim Ausführen des Befehls **azlog createazureid**der folgende Fehler auf?
Fehler

  *Failed to create AAD Application - Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37 - Reason = 'Forbidden' - Message = 'Insufficient privileges to complete the operation.' (Fehler beim Erstellen der AAD-Anwendung. Mandant: 72f988bf-86f1-41af-91ab-2d7cd011db37. Ursache: Unzulässig. Meldung: Nicht genügend Berechtigungen zum Abschließen des Vorgangs.)*

**azlog createazureid** versucht, in allen Azure AD-Mandanten für die Abonnements, auf die das angemeldete Azure-Konto Zugriff hat, einen Dienstprinzipal zu erstellen. Falls es sich bei Ihrer Azure-Anmeldung nur um einen Gastbenutzer des Azure AD-Mandanten handelt, tritt bei dem Befehl ein Fehler mit dem Hinweis auf, dass die Berechtigungen für den Vorgang nicht ausreichen. Bitten Sie den Mandantenadministrator, Ihr Konto als Benutzer hinzuzufügen.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Warum tritt beim Ausführen des Befehls **azlog authorize**der folgende Fehler auf?
Fehler

  *Warning creating Role Assignment - AuthorizationFailed: The client janedo@microsoft.com with object id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/70d95299-d689-4c97-b971-0d8ff0000000'.* (Warnung beim Erstellen der Rollenzuweisung: Autorisierungsfehler. Client „janedo@microsoft.com“ mit der Objekt-ID „fe9e03e4-4dad-4328-910f-fd24a9660bd2“ hat keine Berechtigung zum Ausführen der Aktion „Microsoft.Authorization/roleAssignments/write“ über Bereich „/subscriptions/70d95299-d689-4c97-b971-0d8ff000000“.)

Der Befehl **azlog authorize** weist dem mit **Azlog createazureid** erstellten Azure AD-Dienstprinzipal für die angegebenen Abonnements die Rolle „Leser“ zu. Handelt es sich bei der Azure-Anmeldung nicht um einen Co-Administrator oder Besitzer des Abonnements, tritt ein Fehler mit einem Hinweis auf eine nicht erfolgreiche Autorisierung auf. Für diesen Vorgang wird die in Azure verfügbare rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) des Co-Administrators oder Besitzers benötigt.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Wo finde ich die Definition der Eigenschaften im Überwachungsprotokoll?
Siehe:

* [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md)
* [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Wo finde ich Details zu Azure Security Center-Warnungen?
[Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Wie kann ich ändern, was von der VM-Diagnose erfasst wird?
Ausführliche Informationen zum Abrufen, Ändern und Festlegen der Azure-Diagnose in einer Windows-Konfiguration *(WAD)* finden Sie unter [Aktivieren der Azure-Diagnose auf einem virtuellen Azure-Computer unter Windows mithilfe von PowerShell](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) . Beispiel:

### <a name="get-the-wad-config"></a>Abrufen der WAD-Konfiguration
    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Ändern der WAD-Konfiguration
In der folgenden Beispielkonfiguration werden lediglich Ereignisse mit den Ereignis-IDs 4624 und 4625 aus dem Sicherheitsereignisprotokoll erfasst. Microsoft Antimalware-Ereignisse werden auf der Grundlage des Systemereignisprotokolls erfasst. Ausführliche Informationen zur Verwendung von XPath-Ausdrücken finden Sie unter [Nutzen von Ereignissen](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85) .

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Festlegen der WAD-Konfiguration
    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Vergewissern Sie sich nach dem Vornehmen von Änderungen im Speicherkonto, dass die richtigen Ereignisse erfasst werden.

Wenn während der Installation und Konfiguration Probleme auftreten, öffnen Sie eine [Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request), und wählen Sie **Protokollintegration** als den Dienst aus, für den Sie Support anfordern.


<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png

