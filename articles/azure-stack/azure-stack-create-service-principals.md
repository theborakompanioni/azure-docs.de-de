---
title: "Erstellen eines Dienstprinzipals für Azure Stack | Microsoft-Dokumentation"
description: Beschreibt das Erstellen eines neuen Dienstprinzipals, der mit der rollenbasierten Zugriffssteuerung in Azure Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann.
services: azure-resource-manager
documentationcenter: na
author: heathl17
manager: byronr
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b3992b296d5a999601eb69b071559f9d37dacf8f
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="provide-applications-access-to-azure-stack"></a>Bereitstellen des Anwendungszugriffs auf Azure Stack
Wenn eine Anwendung Zugriff benötigt, um Ressourcen über Azure Resource Manager in Azure Stack bereitzustellen oder zu konfigurieren, erstellen Sie einen Dienstprinzipal. Dies ist ein Objekt mit Anmeldeinformationen für Ihre Anwendung.  Sie können für diesen Dienstprinzipal dann nur die erforderlichen Berechtigungen delegieren.  

Beispiel: Sie verfügen über ein Tool für die Konfigurationsverwaltung, das Azure Resource Manager zum Inventarisieren von Azure-Ressourcen einsetzt.  In diesem Szenario können Sie einen Dienstprinzipal erstellen, dem Dienstprinzipal die Rolle „Leser“ gewähren und das Tool für die Konfigurationsverwaltung auf den schreibgeschützten Zugriff beschränken. 

Der Ansatz mit Dienstprinzipalen ist dem Ausführen der App mit Ihren Anmeldeinformationen aus folgenden Gründen vorzuziehen:

* Sie können dem Dienstprinzipal Berechtigungen zuweisen, die sich von Ihren eigenen Kontoberechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.
* Sie müssen keine Anmeldeinformationen für die App ändern, wenn sich Ihre Zuständigkeiten ändern.
* Sie können ein Zertifikat verwenden, um die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts zu automatisieren.  

## <a name="getting-started"></a>Erste Schritte

Je nachdem, wie Sie Azure Stack bereitgestellt haben, beginnen Sie mit dem Erstellen eines Dienstprinzipals.  In diesem Dokument wird das Erstellen eines Dienstprinzipals für [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) und [Active Directory-Verbunddienste (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs) beschrieben.  Nachdem Sie den Dienstprinzipal erstellt haben, können Sie einige Schritte ausführen, die für AD FS und Azure Active Directory gleich sind, um für die Rolle [Berechtigungen zu delegieren](azure-stack-create-service-principals.md#assign-role-to-service-principal).     

## <a name="create-service-principal-for-azure-ad"></a>Erstellen des Dienstprinzipals für Azure AD

Wenn Sie Azure Stack mit Azure AD als Identitätsspeicher bereitgestellt haben, können Sie Dienstprinzipale genauso wie für Azure erstellen.  In diesem Abschnitt erfahren Sie, wie die Schritte über das Portal ausgeführt werden.  Vergewissern Sie sich vorher, ob Sie über die [erforderlichen Azure AD-Berechtigungen](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) verfügen.

### <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals
In diesem Abschnitt erstellen Sie eine Anwendung (Dienstprinzipal) in Azure AD, die Ihre Anwendung repräsentiert.

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com) bei Ihrem Azure-Konto an.
2. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Hinzufügen**.   
3. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung entweder **Web-App/API** oder **Nativ** aus. Wählen Sie nach dem Festlegen der Werte **Erstellen** aus.

Sie haben einen Dienstprinzipal für Ihre Anwendung erstellt.

### <a name="get-credentials"></a>Abrufen von Anmeldeinformationen
Beim programmgesteuerten Anmelden verwenden Sie die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie in Active Directory unter **App Registrierungen** Ihre Anwendung aus.

2. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode. Die Anwendungen im Abschnitt [Beispielanwendungen](#sample-applications) verweisen auf diesen Wert als Client-ID.

     ![CLIENT-ID](./media/azure-stack-create-service-principal/image12.png)
3. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Schlüssel** aus.

4. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert jetzt, da Sie ihn später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID ein, um die Anmeldung als Anwendung durchzuführen. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

![gespeicherter Schlüssel](./media/azure-stack-create-service-principal/image15.png)


Fahren Sie nach Abschluss des Vorgangs mit dem [Zuweisen einer Rolle für Ihre Anwendung](azure-stack-create-service-principals.md#assign-role-to-service-principal) fort.

## <a name="create-service-principal-for-ad-fs"></a>Erstellen eines Dienstprinzipals für AD FS
Wenn Sie Azure Stack mit AD FS bereitgestellt haben, können Sie PowerShell verwenden, um einen Dienstprinzipal zu erstellen, eine Rolle für den Zugriff zuzuweisen und die Anmeldung über PowerShell mit dieser Identität durchzuführen.

### <a name="before-you-begin"></a>Voraussetzungen

[Laden Sie die Tools, die für die Arbeit mit Azure Stack erforderlich sind, auf Ihren lokalen Computer herunter.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>Importieren des PowerShell-Moduls „Identity“
Navigieren Sie nach dem Herunterladen der Tools zum Downloadordner, und importieren Sie das PowerShell-Modul „Identity“, indem Sie den folgenden Befehl verwenden:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

Beim Importieren des Moduls erhalten Sie ggf. den folgenden Fehler: „AzureStack.Connect.psm1 is not digitally signed. The script will not execute on the system“ (AzureStack.Connect.psm1 ist nicht digital signiert. Das Skript wird auf dem System nicht ausgeführt.) Zum Beheben dieses Problems können Sie eine Ausführungsrichtlinie festlegen, um das Ausführen des Skripts mit dem folgenden Befehl in einer PowerShell-Sitzung mit erhöhten Rechten zu ermöglichen:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>Erstellen des Dienstprinzipals
Sie können einen Dienstprinzipal erstellen, indem Sie den folgenden Befehl ausführen. Achten Sie hierbei darauf, dass Sie den Parameter *DisplayName* aktualisieren:
```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose
```
### <a name="assign-a-role"></a>Zuweisen einer Rolle
Nach dem Erstellen des Dienstprinzipals müssen Sie [ihn einer Rolle zuweisen](azure-stack-create-service-principals.md#assign-role-to-service-principal).

### <a name="sign-in-through-powershell"></a>Anmelden über PowerShell
Nachdem Sie eine Rolle zugewiesen haben, können Sie sich mit dem Dienstprinzipal an Azure Stack anmelden, indem Sie den folgenden Befehl verwenden:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Zuweisen einer Rolle zum Dienstprinzipal
Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Navigieren Sie im Azure Stack-Portal zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus. Sie können stattdessen auch eine Ressourcengruppe oder Ressource auswählen.

2. Wählen Sie das entsprechende Abonnement (Ressourcengruppe oder Ressource) aus, dem die Anwendung zugewiesen werden soll.

     ![Abonnement für Zuweisung auswählen](./media/azure-stack-create-service-principal/image16.png)

3. Wählen Sie **Access Control (IAM)** aus.

     ![„Zugriff“ auswählen](./media/azure-stack-create-service-principal/image17.png)

4. Wählen Sie **Hinzufügen**.

5. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten.

6. Suchen Sie nach Ihrer Anwendung, und wählen Sie sie aus.

7. Wählen Sie **OK** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

Nachdem Sie nun einen Dienstprinzipal erstellt und eine Rolle zugewiesen haben, können Sie diese Elemente in Ihrer Anwendung nutzen, um auf Azure Stack-Ressourcen zuzugreifen.  

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Benutzern für AD FS](azure-stack-add-users-adfs.md)
[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
