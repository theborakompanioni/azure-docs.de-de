---
title: "Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps mithilfe eines Azure AD-Anwendungsproxys | Microsoft-Dokumentation"
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 811adc81424b8e53a740ec34f77a7610fc2a72a8
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps mithilfe eines Azure AD-Anwendungsproxys

In diesem Artikel wird erläutert, wie Sie Apps so konfigurieren, dass sie Benutzer zu einer benutzerdefinierten Homepage weiterleiten. Wenn Sie eine Anwendung mit dem Anwendungsproxy veröffentlichen, legen Sie eine interne URL fest. Dies ist jedoch in einigen Fällen nicht die Seite, die den Benutzern zuerst angezeigt werden soll. Legen Sie eine benutzerdefinierte Startseite fest, sodass die Benutzer zu der richtigen Seite gelangen, wenn sie auf die Apps zugreifen. Für die Benutzer wird die benutzerdefinierte Startseite angezeigt, die Sie festlegen, egal ob ihr Zugriff auf die App über den Azure Active Directory-Zugriffsbereich oder den Office 365-App-Starter erfolgt.

Benutzer, die Ihre Apps starten, werden standardmäßig zur Stammdomänen-URL der veröffentlichten App weitergeleitet. Die Zielseite wird in der Regel als URL der Startseite festgelegt. Definieren Sie mithilfe des Azure AD PowerShell-Moduls benutzerdefinierte Homepage-URLs, wenn die App-Benutzer zu einer bestimmten Seite innerhalb der App gelangen sollen. 

Das folgende Beispiel veranschaulicht, warum es für ein Unternehmen sinnvoll sein kann, eine benutzerdefinierte Startseite festzulegen:
- In Ihrem Unternehmensnetzwerk browsen Benutzer zu *https://ExpenseApp/login/login.aspx*, um sich anzumelden und auf Ihre App zuzugreifen.
- Da Sie auf oberste Ebene der Ordnerstruktur andere Ressourcen wie Images besitzen, auf die der Anwendungsproxy zugreifen muss, veröffentlichen Sie die App mit *https://ExpenseApp* als interner URL.
- Die externe Standard-URL ist *https://ExpenseApp-contoso.msappproxy.net*. Hierüber gelangen die Benutzer jedoch nicht auf die Anmeldeseite.  
- Legen Sie *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* als URL der Startseite fest. 

>[!NOTE]
>Wenn Sie Benutzern Zugriff auf veröffentlichte Apps erteilen, werden die Apps im [Azure AD-Zugriffsbereich](active-directory-saas-access-panel-introduction.md) und im [Office 365-Startfeld](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher) angezeigt.

## <a name="before-you-start"></a>Vorbereitung

Bevor Sie die URL der Homepage festlegen, berücksichtigen Sie die folgenden Anforderungen:

* Bei dem angegebenen Pfad muss es sich um einen Unterdomänenpfad der Stammdomänen-URL handeln.

  Falls die URL der Stammdomäne beispielsweise „https://apps.contoso.com/app1/“ lautet, muss die URL der Startseite, die Sie konfigurieren, mit „https://apps.contoso.com/app1/“ beginnen.

* Nach einer Änderung der veröffentlichten App wird der Wert der Startseiten-URL unter Umständen zurückgesetzt. Wenn Sie die App in Zukunft aktualisieren, sollten Sie die URL der Startseite erneut überprüfen und bei Bedarf aktualisieren.

## <a name="change-the-home-page-in-the-azure-portal"></a>Ändern der Startseite im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Navigieren Sie zu **Azure Active Directory** > **App-Registrierungen**, und wählen Sie Ihre Anwendung aus der Liste aus. 
3. Wählen Sie in den Einstellungen die Option **Eigenschaften** aus.
4. Aktualisieren Sie das Feld **URL der Startseite** mit dem neuen Pfad. 

   ![Bereitstellen der neuen Homepage-URL](./media/application-proxy-office365-app-launcher/homepage.png)

5. Wählen Sie **Speichern** aus.

## <a name="change-the-home-page-with-powershell"></a>Ändern der Startseite mit PowerShell

### <a name="install-the-azure-ad-powershell-module"></a>Installieren des Azure AD PowerShell-Moduls

Bevor Sie mithilfe von PowerShell eine benutzerdefinierte Homepage-URL definieren, installieren Sie das Azure AD PowerShell-Modul. Das entsprechende Paket können Sie aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131) herunterladen. Dieser verwendet den Graph-API-Endpunkt. 

Führen Sie diese Schritte aus, um das Paket zu installieren:

1. Öffnen Sie ein standardmäßiges PowerShell-Fenster, und führen Sie dann den folgenden Befehl aus:

    ```
     Install-Module -Name AzureAD
    ```
    Wenn Sie den Befehl als Benutzer ohne Administratorrechte ausführen, verwenden Sie die Option `-scope currentuser`.
2. Wählen Sie während der Installation **J** aus, um zwei Pakete von „Nuget.org“ zu installieren. Beide Pakete sind erforderlich. 

### <a name="find-the-objectid-of-the-app"></a>Ermitteln der Objekt-ID der App

Rufen Sie die Objekt-ID der App ab, und suchen Sie in der App nach ihrer Startseite.

1. Importieren Sie in demselben PowerShell-Fenster das Azure AD-Modul.

    ```
    Import-Module AzureAD
    ```

2. Melden Sie sich beim Azure AD-Modul als Mandantenadministrator an.

    ```
    Connect-AzureAD
    ```
3. Suchen Sie die App basierend auf der URL der Startseite. Sie finden die URL im Portal, indem Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen** navigieren. In diesem Beispiel wird *sharepoint-iddemo* verwendet.

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. Das Ergebnis sieht etwa wie das hier gezeigte aus. Kopieren Sie die GUID der Objekt-ID zur Verwendung im nächsten Abschnitt.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a>Schritt 2: Aktualisieren der URL der Startseite

Erstellen Sie die URL der Startseite, und aktualisieren Sie die Anwendung mit diesem Wert. Führen Sie diese Befehle in demselben PowerShell-Fenster aus. Wenn Sie stattdessen ein neues PowerShell-Fenster verwenden, melden Sie sich mit `Connect-AzureAD` erneut beim Azure AD-Modul an. 

1. Vergewissern Sie sich, dass Sie die richtige App verwenden, und ersetzen Sie *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* durch die Objekt-ID, die Sie im vorherigen Abschnitt kopiert haben.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Nach dem Überprüfen der App können Sie die Startseite jetzt wie folgt aktualisieren:

2. Erstellen Sie ein leeres Anwendungsobjekt, um die gewünschten Änderungen zu speichern. Diese Variable enthält die Werte, die Sie aktualisieren möchten. In diesem Schritt wird nichts erstellt.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Legen Sie die URL der Startseite auf den gewünschten Wert fest. Der Wert muss ein Unterdomänenpfad der veröffentlichten App sein. Wenn Sie die Homepage-URL also beispielsweise von *https://sharepoint-iddemo.msappproxy.net/* in *https://sharepoint-iddemo.msappproxy.net/hybrid/* ändern, gelangen die App-Benutzer direkt zur benutzerdefinierten Homepage.

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Nehmen Sie das Update mit der GUID (ObjectID) vor, die Sie in „Schritt 1: Ermitteln der Objekt-ID der App“ kopiert haben.

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. Starten Sie die App neu, um zu prüfen, ob die Änderung erfolgreich war.

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Sämtliche Änderungen, die Sie an der App vornehmen, können die URL der Startseite zurücksetzen. Wenn die URL der Startseite zurückgesetzt wird, wiederholen Sie die Schritte in diesem Abschnitt, um sie wiederherzustellen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-enable-remote-access-sharepoint.md)
- [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md)

