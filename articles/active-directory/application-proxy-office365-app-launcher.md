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
ms.date: 03/25/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 28f100276511c1ae978466870ff48f885dd53c28
ms.lasthandoff: 04/03/2017


---

# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Festlegen einer benutzerdefinierten Startseite für veröffentlichte Apps mithilfe eines Azure AD-Anwendungsproxys

In diesem Artikel wird das Konfigurieren von Apps erläutert, um Benutzer zu einer benutzerdefinierten Startseite weiterzuleiten, wenn sie Apps über den Azure Active Directory (Azure AD)-Zugriffsbereich und das Office 365-App-Startfeld aufrufen.

>[!NOTE]
>Das Anwendungsproxy-Feature ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

Mithilfe des Azure AD PowerShell-Moduls können Sie URLs für benutzerdefinierte Startseiten definieren. Dies ist hilfreich, wenn Benutzer direkt zu einer bestimmten Seite Ihrer App weitergeleitet werden sollen (beispielsweise zu *https://expenseApp-contoso.msappproxy.net/login/login.aspx*).

>[!NOTE]
>Wenn Sie Benutzern Zugriff auf veröffentlichte Apps erteilen, werden die Apps im [Azure AD-Zugriffsbereich](active-directory-saas-access-panel-introduction.md) und im [Office 365-Startfeld](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher) angezeigt.

Benutzer, die Ihre Apps starten, werden standardmäßig zur Stammdomänen-URL der veröffentlichten App weitergeleitet. Die Zielseite wird in der Regel als URL der Startseite festgelegt. Für die Back-End-App „http://ExpenseApp“ wird die URL beispielsweise als *https://expenseApp-contoso.msappproxy.net* veröffentlicht. Die URL der Startseite wird standardmäßig auf *https://expenseApp-contoso.msappproxy.net* festgelegt.

## <a name="determine-the-home-page-url"></a>Bestimmen der URL der Startseite

Bevor Sie die URL der Startseite einrichten, beachten Sie Folgendes:

* Bei dem angegebenen Pfad muss es sich um einen Unterdomänenpfad der Stammdomänen-URL handeln.

 Wenn auf Ihre veröffentlichte App also beispielsweise über die URL der Stammdomäne (Beispiel: https://intranet-contoso.msappproxy.net/) zugegriffen werden kann, muss die URL der Startseite, die Sie konfigurieren, mit „https://intranet-contoso.msappproxy.net/“ beginnen.

* Falls die URL der Stammdomäne beispielsweise „https://apps.contoso.com/app1/“ lautet, muss die URL der Startseite, die Sie konfigurieren, mit „https://apps.contoso.com/app1/“ beginnen.

* Nach einer Änderung der veröffentlichten App wird der Wert der Startseiten-URL unter Umständen zurückgesetzt. Wenn Sie die App aktualisieren möchten, sollten Sie die URL der Startseite erneut überprüfen und bei Bedarf aktualisieren.

Im nächsten Abschnitt durchlaufen Sie die Einrichtung einer benutzerdefinierten Startseiten-URL für die veröffentlichte App. 

## <a name="install-the-azure-ad-powershell-module"></a>Installieren des Azure AD PowerShell-Moduls

Bevor Sie mithilfe von PowerShell eine benutzerdefinierte Startseiten-URL definieren, installieren Sie ein nicht standardmäßiges Paket des Azure AD PowerShell-Moduls. Das entsprechende Paket können Sie aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureAD/1.1.23.0) herunterladen. Dieser verwendet den Graph-API-Endpunkt. 

Führen Sie folgende Schritte aus, um das Paket mithilfe von PowerShell zu installieren:

1. Öffnen Sie ein standardmäßiges PowerShell-Fenster, und führen Sie dann den folgenden Befehl aus:

    ```
     Install-Module -Name AzureAD -RequiredVersion 1.1.23.0
    ```
    Wenn Sie den Befehl als Benutzer ohne Administratorrechte ausführen, verwenden Sie die Option **-scope currentuser**.
2. Wählen Sie während der Installation **J** aus, um zwei Pakete von „Nuget.org“ zu installieren. Beide Pakete sind erforderlich. 

## <a name="set-a-custom-home-page-url-by-using-the-azure-ad-powershell-module"></a>Festlegen einer benutzerdefinierten Startseiten-URL mithilfe des Azure AD PowerShell-Moduls

Sie haben das Azure AD PowerShell-Modul installiert und können nun die Startseiten-URL festlegen. Befolgen Sie hierzu die Anweisungen in den nächsten beiden Abschnitten.

### <a name="step-1-find-the-objectid-of-the-app"></a>Schritt 1: Ermitteln der Objekt-ID der App

Rufen Sie die Objekt-ID der App ab, und suchen Sie in der App nach ihrer Startseite.

1. Öffnen Sie PowerShell, und importieren Sie das Azure AD-Modul mithilfe des folgenden Befehls:

    ```
    Import-Module AzureAD
    ```

2. Melden Sie sich mit dem folgenden Cmdlet beim Azure AD-Modul an, und befolgen Sie dann die Anweisungen auf dem Bildschirm. Melden Sie sich als Mandantenadministrator an.

    ```
    Connect-AzureAD
    ```
3. Verwenden Sie dieses Cmdlet, um die App basierend auf der Startseite, die *sharepoint-iddemo* enthält, zu suchen. Zum Bearbeiten der App ersetzen Sie den folgenden Wert durch einen für die App passenden Wert:

    ```
    Get-AzureADApplications | where { $_.Homepage -like “*sharepoint-iddemo*” } | fl DisplayName, Homepage, ObjectID
    ```
4. Das Ergebnis sieht etwa wie das hier gezeigte aus. Kopieren Sie die GUID (ObjectID), sodass Sie diese in „Schritt 2: Aktualisieren der Startseiten-URL“ verwenden können.

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="step-2-update-the-home-page-url"></a>Schritt 2: Aktualisieren der Startseiten-URL

Führen Sie im PowerShell-Modul, das Sie in „Schritt 1: Ermitteln der Objekt-ID der App“ verwendet haben, die folgenden Schritte aus:

1. Prüfen Sie, ob Sie die richtige App verwenden, und ersetzen Sie *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* durch die GUID (ObjectID), die Sie im vorherigen Schritt kopiert haben.

    ```
    Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Nach dem Überprüfen der App können Sie die Startseite jetzt wie folgt aktualisieren:

2. Erstellen Sie ein leeres Anwendungsobjekt, um die gewünschten Änderungen zu speichern.  

 >[!NOTE]
 >Hierbei handelt es sich lediglich um eine Variable für die zu aktualisierenden Werte. Es wurde also nicht wirklich etwas erstellt.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Legen Sie die Startseiten-URL auf den gewünschten Wert fest. Der Wert muss ein Unterdomänenpfad der veröffentlichten App sein. Wenn Sie die Startseiten-URL also beispielsweise von *https://sharepoint-iddemo.msappproxy.net/* in *https://sharepoint-iddemo.msappproxy.net/hybrid/* ändern, werden die App-Benutzer direkt zu der benutzerdefinierten Startseite weitergeleitet.

    ```
    $appnew.Homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Nehmen Sie das Update mit der GUID (ObjectID) vor, die Sie in „Schritt 1: Ermitteln der Objekt-ID der App“ kopiert haben.

    ```
    Set-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 - Application $appnew
    ```
5. Starten Sie die App neu, um zu prüfen, ob die Änderung erfolgreich war.

    ```
    Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Sämtliche Änderungen, die Sie an der App vornehmen, können die Startseiten-URL zurücksetzen. Wiederholen Sie in diesem Fall den Prozess in „Schritt 2: Aktualisieren der Startseiten-URL“.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy](application-proxy-enable-remote-access-sharepoint.md)
- [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md)

