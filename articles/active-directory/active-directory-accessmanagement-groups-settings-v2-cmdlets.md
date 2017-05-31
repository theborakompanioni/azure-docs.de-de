---
title: "Azure Active Directory PowerShell-Cmdlets für die Gruppenverwaltung in Azure AD | Microsoft-Dokumentation"
description: "Auf dieser Seite finden Sie PowerShell-Beispiele für die Verwaltung von Gruppen in Azure Active Directory."
keywords: Azure AD, Azure Active Directory, PowerShell, Gruppen, Gruppenverwaltung
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand;rodejo
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: d7be54b508a845d6746fd65887e3339ff371a287
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Azure Active Directory-Cmdlets Version 2 für die Gruppenverwaltung
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-groups-create-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Im folgenden Dokument finden Sie Beispiele für die Verwendung von PowerShell zur Verwaltung von Gruppen in Azure Active Directory (Azure AD).  Darüber hinaus erhalten Sie Informationen zum Einrichten des Azure AD PowerShell-Moduls. Zunächst müssen Sie [das Azure AD PowerShell-Modul herunterladen](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="installing-the-azure-ad-powershell-module"></a>Installieren des Azure AD PowerShell-Moduls
Verwenden Sie die folgenden Befehle, um das Azure AD PowerShell-Modul zu installieren:

    PS C:\Windows\system32> install-module azuread

Überprüfen Sie mithilfe des folgenden Befehls, ob das Modul installiert wurde:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Sie können die Cmdlets jetzt im Modul verwenden. Eine ausführliche Beschreibung der Cmdlets im Azure AD-Modul finden Sie in der [Onlinereferenzdokumentation](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connecting-to-the-directory"></a>Herstellen der Verbindung mit dem Verzeichnis
Bevor Sie Gruppen mithilfe der Azure AD PowerShell-Cmdlets verwalten können, müssen Sie Ihre PowerShell-Sitzung mit dem Verzeichnis verbinden, das verwaltet werden soll. Führen Sie zu diesem Zweck den folgenden Befehl aus:

    PS C:\Windows\system32> Connect-AzureAD

Das Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen auf, die Sie für den Zugriff auf Ihr Verzeichnis verwenden möchten. In diesem Beispiel verwenden wir „ karen@drumkit.onmicrosoft.com “ für den Zugriff auf das Demoverzeichnis. Das Cmdlet zeigt in einer Bestätigung an, dass die Sitzung erfolgreich mit Ihrem Verzeichnis verbunden wurde:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Sie können die Azure AD-Cmdlets nun zum Verwalten von Gruppen in Ihrem Verzeichnis verwenden.

## <a name="retrieving-groups"></a>Abrufen von Gruppen
Zum Abrufen von vorhandenen Gruppen aus Ihrem Verzeichnis können Sie das Cmdlet „Get-AzureADGroups“ verwenden. Um alle Gruppen innerhalb des Verzeichnisses abzurufen, verwenden Sie das Cmdlet ohne Parameter:

    PS C:\Windows\system32> get-azureadgroup

Das Cmdlet gibt alle Gruppen innerhalb des verbundenen Verzeichnisses zurück.

Zum Abrufen einer bestimmten Gruppe verwenden Sie den Parameter „-objectID“ und geben die Objekt-ID der Gruppe an:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Das Cmdlet gibt die Gruppe zurück, deren Objekt-ID mit dem eingegebenen Wert des Parameters übereinstimmt:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Mithilfe des Parameters „-filter“ können Sie nach einer bestimmten Gruppe suchen. Dieser Parameter verwendet eine ODATA-Filterklausel und gibt alle Gruppen zurück, die dem Filter entsprechen. Beispiel:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Beachten Sie, dass die Azure AD PowerShell-Cmdlets den OData-Abfragestandard implementieren. Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Erstellen von Gruppen
Zum Erstellen einer neuen Gruppe in Ihrem Verzeichnis verwenden Sie das Cmdlet „New-AzureADGroup“. Dieses Cmdlet erstellt eine neue Sicherheitsgruppe „Marketing“:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualisieren von Gruppen
Zum Aktualisieren einer vorhandenen Gruppe verwenden Sie das Cmdlet „Set-AzureADGroup“. In diesem Beispiel ändern wir die Eigenschaft „DisplayName“ der Gruppe „Intune Administrators“. Zunächst suchen wir mit dem Cmdlet „Get-AzureADGroup“ nach der Gruppe. Dabei filtern wir das Ergebnis mithilfe des Attributs „DisplayName“:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Anschließend ändern wir die Eigenschaft „Description“ in den neuen Wert „Intune Device Administrators“:

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Wenn wir nun erneut nach der Gruppe suchen, sehen wir, dass die Eigenschaft „Description“ mit dem neuen Wert aktualisiert wurde:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Löschen von Gruppen
Zum Löschen von Gruppen aus Ihrem Verzeichnis verwenden Sie das Cmdlet „Remove-AzureADGroup“ wie folgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Verwalten von Gruppenmitgliedern
Zum Hinzufügen neuer Mitglieder zu einer Gruppe verwenden Sie das Cmdlet „Add-AzureADGroupMember“. Mit diesem Befehl wird der Gruppe „Intune Administrators“ aus dem vorherigen Beispiel ein Mitglied hinzugefügt:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Der Parameter „-ObjectId“ ist die Objekt-ID der Gruppe, der ein Mitglied hinzugefügt werden soll, „-RefObjectId“ ist die Objekt-ID des Benutzers, der als Gruppenmitglied hinzugefügt werden soll.

Zum Abrufen der vorhandenen Mitglieder einer Gruppe verwenden Sie das Cmdlet „Get-AzureADGroupMember“, wie in diesem Beispiel gezeigt:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Um das Mitglied zu entfernen, das der Gruppe zuvor hinzugefügt wurde, verwenden Sie das Cmdlet „Remove-AzureADGroupMember“, wie hier gezeigt:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Zum Überprüfen der Gruppenmitgliedschaft(en) eines Benutzers verwenden Sie das Cmdlet „Select-AzureADGroupIdsUserIsMemberOf“. Dieses Cmdlet verwendet als Parameter die Objekt-ID des Benutzers, für den die Gruppenmitgliedschaften überprüft werden sollen, sowie eine Liste mit Gruppen, für die die Mitgliedschaften überprüft werden sollen. Die Liste der Gruppen muss in Form einer komplexen Variable vom Typ „Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck“ angegeben werden. Daher müssen wir zunächst eine Variable dieses Typs erstellen:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Anschließend geben wir Werte für die Gruppen-IDs an, die im Attribut „GroupIds“ dieser komplexen Variable überprüft werden sollen:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Wenn z.B. die Gruppenmitgliedschaften eines Benutzers mit der Objekt-ID „72cd4bbd-2594-40a2-935c-016f3cfeeeea“ für die Gruppen in „$g“ überprüft werden sollen, verwenden wir folgenden Befehl:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Der zurückgegebene Wert ist eine Liste mit Gruppen, in denen dieser Benutzer Mitglied ist. Diese Methode kann auch verwendet werden, um die Mitgliedschaft in Kontaktlisten, Gruppen oder Dienstprinzipalen für eine Liste von Gruppen zu überprüfen. Verwenden Sie dazu „Select-AzureADGroupIdsContactIsMemberOf“, „Select-AzureADGroupIdsGroupIsMemberOf“ oder „Select-AzureADGroupIdsServicePrincipalIsMemberOf“.

## <a name="managing-owners-of-groups"></a>Verwalten von Gruppenbesitzern
Zum Hinzufügen von Besitzern zu einer Gruppe verwenden Sie das Cmdlet „Add-AzureADGroupOwner“:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Der Parameter „-ObjectId“ ist die Objekt-ID der Gruppe, der ein Besitzer hinzugefügt werden soll, „-RefObjectId“ ist die Objekt-ID des Benutzers, der als Besitzer der Gruppe hinzugefügt werden soll.

Zum Abrufen der Besitzer einer Gruppe verwenden Sie das Cmdlet „Get-AzureADGroupOwner“:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Das Cmdlet gibt die Liste der Besitzer für die angegebene Gruppe zurück:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Zum Entfernen eines Besitzers aus einer Gruppe verwenden Sie „Remove-AzureADGroupOwner“:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Active Directory PowerShell finden Sie unter [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

