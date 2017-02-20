---
title: "Festlegen einer benutzerdefinierten Startseite für Ihre veröffentlichte Anwendung per Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
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
ms.date: 01/25/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: d543849ccdc3cd93845756954b063ae169b066d5
ms.openlocfilehash: 9f642b03b6c10f6312b2c31bbc810cb6701352e3


---

# <a name="set-a-custom-home-page-for-your-published-application-using-azure-ad-app-proxy"></a>Festlegen einer benutzerdefinierten Startseite für Ihre veröffentlichte Anwendung per Azure AD-Anwendungsproxy

In diesem Artikel erfahren Sie, wie Sie Ihre Anwendung so konfigurieren, dass Benutzer zu einer benutzerdefinierten Startseite weitergeleitet werden, wenn sie über den Azure AD-Zugriffsbereich oder über das App-Startfeld von Office 365 auf Ihre Anwendung zugreifen.

>[!NOTE]
>Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 
 
Mithilfe des Azure AD PowerShell-Moduls können Sie URLs für benutzerdefinierte Startseiten definieren. Dies ist hilfreich, wenn Benutzer direkt zu einer bestimmten Seite Ihrer Anwendung weitergeleitet werden sollen (beispielsweise zu „https://expenseApp-contoso.msappproxy.net/login/login.aspx“).

>[!NOTE]
>Wenn Sie Benutzern den Zugriff auf Ihre veröffentlichten Anwendungen gewähren, werden Ihre Apps im [Azure AD-Zugriffsbereich](active-directory-saas-access-panel-introduction.md) und im [App-Startfeld von Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher) angezeigt. 
>

Benutzer, die Ihre Apps starten, werden standardmäßig zur Stammdomänen-URL der veröffentlichten App weitergeleitet. Die Zielseite wird in der Regel auf die URL der Startseite festgelegt. Für die Back-End-Anwendung „http://ExpenseApp“ wird sie beispielsweise als „https://expenseApp-contoso.msappproxy.net“ veröffentlicht. Die URL der Startseite wird standardmäßig auf „https://expenseApp-contoso.msappproxy.net“ festgelegt.

## <a name="determine-the-home-page-url"></a>Bestimmen der URL der Startseite

Beim Festlegen der URL der Startseite sind mehrere Punkte zu beachten:

* Bei dem angegebenen Pfad muss es sich um einen Unterdomänenpfad der Stammdomänen-URL handeln.

 Wenn auf Ihre veröffentlichte App also beispielsweise über die Startseiten-URL „https://intranet-contoso.msappproxy.net/“ zugegriffen werden kann, muss die URL der Startseite, die Sie konfigurieren, mit „https://intranet-contoso.msappproxy.net/“ beginnen. 
 
* Wenn die Startseiten-URL „https://apps.contoso.com/app1/“ lautet, muss die URL der Startseite mit „https://apps.contoso.com/app1/“ beginnen.

* Nach einer Änderung der veröffentlichten Anwendung wird der Wert der Startseiten-URL unter Umständen zurückgesetzt. Überprüfen Sie daher nach dem Aktualisieren Ihrer Anwendung noch einmal die URL der Startseite, und passen Sie sie bei Bedarf an.


Im nächsten Abschnitt erfahren Sie, wie Sie eine benutzerdefinierte Startseiten-URL für Ihre veröffentlichten Anwendungen einrichten. 

## <a name="install-the-azure-ad-powershell-module"></a>Installieren des Azure AD PowerShell-Moduls

Bevor Sie mithilfe von PowerShell eine benutzerdefinierte Startseiten-URL definieren können, müssen Sie zunächst ein nicht standardmäßiges Paket des Azure AD PowerShell-Moduls installieren.  Das entsprechende Paket können Sie aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureAD/1.1.23.0) herunterladen. (Dieser verwendet den GRAPH-API-Endpunkt.) 

**So installieren Sie das Paket mithilfe von PowerShell:**

1. Öffnen Sie die Standardversion von PowerShell.
2. Führen Sie den folgenden Befehl aus:

```
 Install-Module -Name AzureAD -RequiredVersion 1.1.23.0
 ```
 Falls Sie diesen Befehl nicht als Administrator ausführen, verwenden Sie die Option _-scope currentuser_.
3. Wählen Sie während der Installation „J“ aus, um zwei Pakete von „Nuget.org“ zu installieren.  Beide sind für die Verwendung des Pakets erforderlich. 

##<a name="set-a-custom-home-page-url-using-the-azure-ad-powershell-module"></a>Festlegen einer benutzerdefinierten Startseiten-URL mithilfe des Azure AD PowerShell-Moduls

Nach der Installation des Azure AD PowerShell-Moduls können Sie nun mit zwei einfachen Schritten die URL der Startseite festlegen.

1. Suchen Sie nach der Anwendung, die Sie aktualisieren möchten.
2. Aktualisieren Sie die URL der Startseite für die Anwendung.

###<a name="step-1--find-the-objectid-of-the-application"></a>1. Schritt: Ermitteln der Objekt-ID der Anwendung

Rufen Sie zunächst die Objekt-ID der Anwendung ab, und suchen Sie anschließend anhand der Startseite nach der Anwendung.

1. Öffnen Sie PowerShell.
2. Importieren Sie das Azure AD-Modul.
  
 ```
 Import-Module AzureAD
 ```
3. Melden Sie sich beim Azure AD-Modul an.  Verwenden Sie das folgende Cmdlet, und folgen Sie den Anweisungen auf dem Bildschirm. Wichtig: Melden Sie sich als Mandantenadministrator an.
 
 ```
 Connect-AzureAD
 ```
4. Das folgende Cmdlet sucht die Anwendungen auf der Grundlage der Startseite mit _sharepoint-iddemo_. Das ist die App, die Sie bearbeiten möchten. Dieser Wert muss durch den passenden Wert für Ihre Anwendung ersetzt werden.
  
 ```
 Get-AzureADApplications | where { $_.Homepage -like “*sharepoint-iddemo*” } | fl DisplayName, Homepage, ObjectID
 ```
5. Das Ergebnis sollte in etwa wie die folgende Antwort aussehen. Die GUID (ObjectID-Wert unten) ist das Element, das Sie kopieren müssen.
 
 ```
 DisplayName : SharePoint
 Homepage    : https://sharepoint-iddemo.msappproxy.net/
 ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
 ```
6. Kopieren Sie den GUID-Wert (ObjectID). Sie benötigen ihn im nächsten Schritt.


###<a name="step-2--update-the-homepage-url"></a>2. Schritt: Aktualisieren der Startseiten-URL

Die URL der Startseite wird mit dem gleichen PowerShell-Modul aktualisiert, mit dem Sie zuvor die Anwendungs-ID ermittelt haben. Führen Sie nach der Anmeldung bei PowerShell die folgenden Schritte aus:

1. Vergewissern Sie sich, dass die Anwendung korrekt ist, und ersetzen Sie _8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4_ durch den ObjectID-Wert Ihrer Anwendung, den Sie im ersten Schritt kopiert haben. 
  
 ```
 Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
 ```
 
 Nachdem Sie die Anwendung bestätigt haben, können Sie die Startseite wie folgt aktualisieren:
 
2. Erstellen Sie ein leeres Anwendungsobjekt, um die gewünschten Änderungen zu speichern. Hierbei handelt es sich lediglich um eine Variable für die zu aktualisierenden Werte. Es wurde also nicht wirklich etwas erstellt.
  
 ```
 $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
 ```
3. Legen Sie die Startseite auf den gewünschten Wert fest. Bedenken Sie, dass es sich dabei um einen Unterdomänenpfad der veröffentlichten Anwendung handeln muss. Wenn Sie die Startseite also beispielsweise von _https://sharepoint-iddemo.msappproxy.net/_ in _https://sharepoint-iddemo.msappproxy.net/hybrid/_ ändern, werden Ihre Benutzer direkt zu der benutzerdefinierten Startseite weitergeleitet.
  
 ```
 $appnew.Homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
 ```
4. Zum Schluss muss noch die Aktualisierung durchgeführt werden. Wichtig: Verwenden Sie die kopierte GUID aus dem ersten Schritt.
  
 ```
 Set-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 - Application $appnew
 ```
5. Überprüfen Sie nun die benutzerdefinierte Startseite. Starten Sie hierzu die Anwendung erneut, und vergewissern Sie sich, dass die Änderung erfolgreich war.
  
 ```
 Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
 ```

>[!NOTE]
>Beachten Sie, dass die URL der Startseite bei jeder Änderung, die Sie an Ihrer Anwendung vornehmen, zurückgesetzt werden kann. In diesem Fall müssen Sie diesen Vorgang wiederholen.

##<a name="next-steps"></a>Nächste Schritte

[Enable remote access to SharePoint with Azure AD App Proxy](application-proxy-enable-remote-access-sharepoint.md) (Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy)<br>
[Aktivieren des Anwendungsproxys über das Azure-Portal](https://github.com/Microsoft/azure-docs-pr/blob/master/articles/active-directory/active-directory-application-proxy-enable.md)



<!--HONumber=Feb17_HO1-->


