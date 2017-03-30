---
title: Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen | Microsoft Docs
description: "Verwalten der Einstellungen für Gruppen mithilfe von Azure Active Directory-Cmdlets."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 5c30f459d9fed71fede2da71306a9b48892566f3
ms.lasthandoff: 03/22/2017


---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen

Hinweis: Dieser Inhalt gilt ausschließlich für einheitliche Gruppen, die auch als Office 365-Gruppen bezeichnet werden.

Office 365-Gruppeneinstellungen werden mithilfe eines „Settings“- und eines „SettingsTemplate“-Objekts konfiguriert. Anfänglich werden keine Settings-Objekte im Verzeichnis angezeigt. Dies bedeutet, dass Ihr Verzeichnis mit den Standardeinstellungen konfiguriert ist. Um die Standardeinstellungen zu ändern, müssen Sie mithilfe einer Einstellungsvorlage ein neues Einstellungsobjekt erstellen. Einstellungsvorlagen werden von Microsoft definiert. Es werden verschiedene Einstellungsvorlagen unterstützt. Zum Konfigurieren von Gruppeneinstellungen für Ihr Verzeichnis verwenden Sie die Vorlage mit dem Namen „Group.Unified“. Zum Konfigurieren von Gruppeneinstellungen für eine einzelne Gruppe verwenden Sie die Vorlage mit dem Namen „Group.Unified.Guest“. Diese Vorlage dient zum Verwalten des Gastzugriffs auf eine Gruppe. 

Die Cmdlets gehören zum Azure Active Directory PowerShell V2-Modul. Weitere Informationen zu diesem Modul und eine Anleitung zum Herunterladen und Installieren des Moduls auf Ihrem Computer finden Sie unter [Azure Active Directory PowerShell, Version 2](https://docs.microsoft.com/en-us/powershell/azuread/).

## <a name="create-settings-at-the-directory-level"></a>Erstellen von Einstellungen auf Verzeichnisebene
Über diese Schritte werden auf Verzeichnisebene Einstellungen erstellt, die für alle einheitlichen Gruppen im Verzeichnis gelten.

1. In den „DirectorySettings“-Cmdlets müssen Sie die ID des „SettingsTemplate“-Objekts angeben, das Sie verwenden möchten. Wenn Sie diese ID nicht kennen, gibt dieses Cmdlet die Liste aller Einstellungsvorlagen zurück:

    PS C:> Get-AzureADDirectorySettingTemplate

Bei Aufruf dieses Cmdlets werden alle verfügbaren Vorlagen zurückgegeben:

```
Id                                   DisplayName         Description
--                                   -----------         -----------
62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
```
2. Um eine URL zu Nutzungsrichtlinien hinzuzufügen, müssen Sie zunächst das SettingsTemplate-Objekt abrufen, das den Wert für die Nutzungsrichtlinien-URL definiert, also die Group.Unified-Vorlage:

    $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b

3. Erstellen Sie danach basierend auf dieser Vorlage ein neues Einstellungsobjekt:

    $Setting = $template.CreateDirectorySetting()
    
4. Aktualisieren Sie dann den Wert für die Nutzungsrichtlinie:

    $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
    
5. Zum Schluss wenden Sie die Einstellungen an:

    New-AzureADDirectorySetting -DirectorySetting $settings

Nach erfolgreichem Abschluss gibt das Cmdlet die ID des neuen Einstellungsobjekts zurück:


    Id                                   DisplayName TemplateId                           Values
    --                                   ----------- ----------                           ------
    c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...


Folgende Einstellungen sind im SettingsTemplate-Objekt „Group.Unified“ definiert.

| **Einstellung** | **Beschreibung** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Typ: Boolescher Wert<li>Standard: True |Das Flag, das angibt, ob die Erstellung von einheitlichen Gruppen im Verzeichnis zulässig ist. |
|  <ul><li>GroupCreationAllowedGroupId<li>Typ: Zeichenfolge<li>Standardwert: “” |GUID der Sicherheitsgruppe, deren Mitgliedern das Erstellen von einheitlichen Gruppen erlaubt ist, auch wenn der Wert von „EnableGroupCreation“ FALSE ist. |
|  <ul><li>UsageGuidelinesUrl<li>Typ: Zeichenfolge<li>Standardwert: “” |Ein Link zu den Nutzungsrichtlinien für die Gruppe. |
|  <ul><li>ClassificationDescriptions<li>Typ: Zeichenfolge<li>Standardwert: “” | Eine durch Trennzeichen getrennte Liste mit Klassifizierungsbeschreibungen. |
|  <ul><li>DefaultClassification<li>Typ: Zeichenfolge<li>Standardwert: “” | Die Klassifizierung, die als Standardklassifizierung einer Gruppe verwendet werden soll, falls keine angegeben wurde.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Typ: Zeichenfolge<li>Standardwert: “” |Noch nicht implementiert
|  <ul><li>AllowGuestsToBeGroupOwner<li>Typ: Boolescher Wert<li>Standard: False | Boolescher Wert, der angibt, ob ein Gastbenutzer Besitzer von Gruppen sein kann. |
|  <ul><li>AllowGuestsToAccessGroups<li>Typ: Boolescher Wert<li>Standard: True | Boolescher Wert, der angibt, ob ein Gastbenutzer Zugriff auf den Inhalt einheitlicher Gruppen hat. |
|  <ul><li>GuestUsageGuidelinesUrl<li>Typ: Zeichenfolge<li>Standardwert: “” | Die URL eines Links zu den Leitlinien für die Nutzung des Gastzugriffs. |
|  <ul><li>AllowToAddGuests<li>Typ: Boolescher Wert<li>Standard: True | Boolescher Wert, der angibt, ob das Hinzufügen von Gästen zu diesem Verzeichnis erlaubt ist.|
|  <ul><li>ClassificationList<li>Typ: Zeichenfolge<li>Standardwert: “” |Eine durch Trennzeichen getrennte Liste der gültigen Klassifizierungswerte, die auf einheitliche Gruppen angewendet werden können. |
|  <ul><li>EnableGroupCreation<li>Typ: Boolescher Wert<li>Standard: True | Boolescher Wert, der angibt, ob Benutzer, die keine Administratoren sind, neue einheitliche Gruppen erstellen können. |
'

## <a name="read-settings-at-the-directory-level"></a>Lesen von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen gelesen, die für alle Office-Gruppen im Verzeichnis gelten.

1. Lesen aller vorhandenen Verzeichniseinstellungen:

    `Get-AzureADDirectorySetting -All $True'

Dieses Cmdlet gibt eine Liste aller Verzeichniseinstellungen zurück: ' Id                                   DisplayName   TemplateId                           Values
--                                   -----------   ----------                           ------
c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...`


2. Lesen aller Einstellungen für eine bestimmte Gruppe:

    `Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups`

3. Lesen aller Werte von Verzeichniseinstellungen für ein bestimmtes Verzeichniseinstellungsobjekt mithilfe der Einstellungs-ID „GUID“:

    `(Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values'

Dieses Cmdlet gibt die Namen und Werte in diesem Einstellungsobjekt für diese bestimmte Gruppe zurück: ' Name                          Value
----                          -----
ClassificationDescriptions DefaultClassification PrefixSuffixNamingRequirement AllowGuestsToBeGroupOwner     False AllowGuestsToAccessGroups     True GuestUsageGuidelinesUrl GroupCreationAllowedGroupId AllowToAddGuests              True UsageGuidelinesUrl            <https://guideline.com> ClassificationList EnableGroupCreation           True` '

## <a name="update-settings-for-a-specific-group"></a>Aktualisieren von Einstellungen für eine bestimmte Gruppe

1. Suchen der Einstellungsvorlage mit dem Namen „Groups.Unified.Guest“

    Get-AzureADDirectorySettingTemplate

    Id                                   DisplayName            Description
    --                                   -----------            -----------
    62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...

2. Abrufen des Vorlagenobjekts für die Vorlage „Groups.Unified.Guest“:

    $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9

3. Erstellen eines neuen Einstellungsobjekts anhand der Vorlage:


    $Setting = $Template.CreateDirectorySetting()


4. Festlegen der Einstellung auf den erforderlichen Wert:


    $Setting["AllowToAddGuests"]=$False


6. Erstellen der neuen Einstellung für die erforderliche Gruppe im Verzeichnis:


    New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting


    Id                                   DisplayName TemplateId                           Values
    --                                   ----------- ----------                           ------
    25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...


## <a name="update-settings-at-the-directory-level"></a>Aktualisieren von Einstellungen auf Verzeichnisebene

Mit diesen Schritten werden auf Verzeichnisebene Einstellungen aktualisiert, die für alle einheitlichen Gruppen im Verzeichnis gelten. In diesen Beispielen wird davon ausgegangen, dass in Ihrem Verzeichnis bereits ein „Settings“-Objekt vorhanden ist.

1. Ermitteln des vorhandenen „Settings“-Objekts:

    Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ`

    Id                                   DisplayName   TemplateId                           Values
    --                                   -----------   ----------                           ------
    c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...

    $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323`

3. Aktualisieren des Werts:

    $Setting["AllowToAddGuests"] = "false"`

4. Aktualisieren der Einstellung:

    Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting`

## <a name="remove-settings-at-the-directory-level"></a>Entfernen von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen entfernt, die für alle Office-Gruppen im Verzeichnis gelten.

    Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c`

## <a name="cmdlet-syntax-reference"></a>Referenz der Cmdletsyntax
Weitere Informationen zu Azure Active Directory PowerShell finden Sie unter [Azure Active Directory-Cmdlets](https://docs.microsoft.com/en-us/powershell/azuread/).

## <a name="additional-reading"></a>Zusätzliche Lektüre

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

