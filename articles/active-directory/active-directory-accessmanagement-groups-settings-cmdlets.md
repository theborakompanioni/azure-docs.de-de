---
title: Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen | Microsoft Docs
description: Verwalten der Einstellungen für Gruppen mithilfe von Azure Active Directory-Cmdlets.
services: active-directory
documentationcenter: ''
author: curtand
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2016
ms.author: curtand

---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen
Folgende Einstellungen für einheitliche Gruppen können in Ihrem Verzeichnis konfiguriert werden:

1. Klassifizierungen: die durch Trennzeichen getrennte Liste von Klassifizierungen, die Benutzer in einer Gruppe festlegen können. Beispiele: „Vertraulich“, „Geheim“, „Streng geheim“.
2. Nutzungsrichtlinien-URL: eine URL, die Benutzer zu den von Ihrer Organisation definierten Nutzungsbedingungen für die Verwendung von einheitlichen Gruppen weiterleitet. Diese URL wird auf der Benutzeroberfläche angezeigt, wenn Benutzer Gruppen verwenden.
3. Gruppenerstellung aktiviert: Gibt an, ob keine, einige oder alle Benutzer einheitliche Gruppen erstellen dürfen. Wenn diese Einstellung aktiviert ist, dürfen alle Benutzer Gruppen erstellen. Wenn diese Einstellung deaktiviert ist, darf kein Benutzer Gruppen erstellen. In diesem Fall können Sie eine Sicherheitsgruppe angeben, deren Benutzer dennoch Gruppen erstellen dürfen.

Diese Einstellungen werden mithilfe von Settings- und SettingsTemplate-Objekten konfiguriert. Anfänglich werden keine Settings-Objekte im Verzeichnis angezeigt. Dies bedeutet, dass Ihr Verzeichnis mit den Standardeinstellungen konfiguriert ist. Um die Standardeinstellungen zu ändern, erstellen Sie mithilfe einer Einstellungsvorlage ein neues Einstellungsobjekt. Einstellungsvorlagen werden von Microsoft definiert.

Sie können das Modul mit den Cmdlets für diese Vorgänge von der [Microsoft Connect-Website](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)herunterladen.

## <a name="create-settings-at-the-directory-level"></a>Erstellen von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen erstellt, die für alle Office-Gruppen im Verzeichnis gelten.

1. Wenn Sie nicht wissen, welches SettingsTemplate-Objekt Sie verwenden sollen, gibt dieses Cmdlet die Liste der Einstellungsvorlagen zurück:
   
    `Get-MsolAllSettingTemplate`
   
    ![Liste der Einstellungsvorlagen](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)
2. Um eine URL zu Nutzungsrichtlinien hinzuzufügen, müssen Sie zunächst das SettingsTemplate-Objekt abrufen, das den Wert für die Nutzungsrichtlinien-URL definiert, also die Group.Unified-Vorlage:
   
    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`
3. Erstellen Sie danach basierend auf dieser Vorlage ein neues Einstellungsobjekt:
   
    `$setting = $template.CreateSettingsObject()`
4. Aktualisieren Sie dann den Wert für die Nutzungsrichtlinie:
   
    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`
5. Zum Schluss wenden Sie die Einstellungen an:
   
    `New-MsolSettings –SettingsObject $setting`
   
    ![Hinzufügen einer Nutzungsrichtlinien-URL](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Folgende Einstellungen sind im SettingsTemplate-Objekt „Group.Unified“ definiert.

| **Einstellung** | **Beschreibung** |
| --- | --- |
|  <ul><li>ClassificationList<li>Typ: Zeichenfolge<li>Standardwert: “” |Eine durch Trennzeichen getrennte Liste der gültigen Klassifizierungswerte, die auf einheitliche Gruppen angewendet werden können. |
|  <ul><li>EnableGroupCreation<li>Typ: Boolescher Wert<li> Standard: True |Das Flag, das angibt, ob die Erstellung von einheitlichen Gruppen im Verzeichnis zulässig ist. |
|  <ul><li>GroupCreationAllowedGroupId<li>Typ: Zeichenfolge<li>Standardwert: “” |GUID der Sicherheitsgruppe, für die das Erstellen von einheitlichen Gruppen zulässig ist, auch wenn der EnableGroupCreation-Wert „false“ lautet. |
|  <ul><li>UsageGuidelinesUrl<li>Typ: Zeichenfolge<li>Standardwert: “” |Ein Link zu den Nutzungsrichtlinien für die Gruppe. |

## <a name="read-settings-at-the-directory-level"></a>Lesen von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen gelesen, die für alle Office-Gruppen im Verzeichnis gelten.

1. Lesen aller vorhandenen Verzeichniseinstellungen:
   
    `Get-MsolAllSettings`
2. Lesen aller Einstellungen für eine bestimmte Gruppe:
   
    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`
3. Lesen bestimmter Verzeichniseinstellungen mithilfe der SettingId-GUID:
   
    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`
   
    ![SettingId-GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Aktualisieren von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen aktualisiert, die für alle Office-Gruppen im Verzeichnis gelten.

1. Abrufen des vorhandenen Settings-Objekts:
   
    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`
2. Abrufen des zu aktualisierenden Werts:
   
    `$value = $Setting.GetSettingsValue()`
3. Aktualisieren des Werts:
   
    `$value["AllowToAddGuests"] = "false"`
4. Aktualisieren der Einstellung:
   
    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Entfernen von Einstellungen auf Verzeichnisebene
Mit diesen Schritten werden auf Verzeichnisebene Einstellungen entfernt, die für alle Office-Gruppen im Verzeichnis gelten.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Referenz der Cmdletsyntax
Weitere Informationen zu Azure Active Directory PowerShell finden Sie unter [Azure Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-(group.unified-settingstemplate-object)"></a>SettingsTemplate-Objektreferenz (SettingsTemplate-Objekt „Group.Unified“)
* "name": "EnableGroupCreation", "type": "System.Boolean", "defaultValue": "true", "description": "A boolean flag indicating if the Unified Group creation feature is on."
* "name": "GroupCreationAllowedGroupId", "type": "System.Guid", "defaultValue": "", "description": "GUID of the security group that is whitelisted to create Unified Groups."
* "name": "ClassificationList", "type": "System.String", "defaultValue": "", "description": "A comma-delimited list of valid classification values that can be applied to Unified Groups."
* "name": "UsageGuidelinesUrl", "type": "System.String", "defaultValue": "", "description": "A link to the Group Usage Guidelines."

| Name | Typ | defaultValue | Beschreibung |
| --- | --- | --- | --- |
| "EnableGroupCreation" |"System.Boolean" |„true“ |"A boolean flag indicating if the Unified Group creation feature is on." |
| "GroupCreationAllowedGroupId" |"System.Guid" |"" |"GUID of the security group that is whitelisted to create Unified Groups." |
| "ClassificationList" |"System.String" |"" |"A comma-delimited list of valid classification values that can be applied to Unified Groups." |
| "UsageGuidelinesUrl" |"System.String" |"" |"A link to the Group Usage Guidelines." |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Active Directory PowerShell finden Sie unter [Azure Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Zusätzliche Anleitungen des Microsoft-Programmmanagers Rob de Jong stehen unter [Rob‘s Groups Blog](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad)zur Verfügung.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!--HONumber=Oct16_HO2-->


