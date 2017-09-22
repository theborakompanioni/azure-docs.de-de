---
title: "Konfigurieren von Bereitstellungsquellen für App Services in Azure Stack | Microsoft-Dokumentation"
description: "Informationen, wie ein Dienstadministrator Bereitstellungsquellen (Git, GitHub, BitBucket, Dropbox und OneDrive) für App Service in Azure Stack konfiguriert"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: anwestg
ms.translationtype: HT
ms.sourcegitcommit: de674af369080ad7eb608608685e293f2326c8e6
ms.openlocfilehash: be4b032e8f7370f696c47a7b8e82c0a819529f4d
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="configure-deployment-sources"></a>Konfigurieren von Bereitstellungsquellen

App Service in Azure Stack unterstützt die bedarfsgesteuerte Bereitstellung aus mehreren Quellcodeverwaltungsanbietern.  Dieses Feature ermöglicht Anwendungsentwicklern eine Bereitstellung direkt aus ihren Repositorys zur Quellcodeverwaltung.  Damit Mandanten App Service für das Herstellen einer Verbindung mit ihren Repositorys konfigurieren können, müssen Administratoren zuerst die Integration zwischen App Service in Azure Stack und dem Quellcodeverwaltungsanbieter konfigurieren.  Neben lokalem Git werden die folgenden Quellcodeverwaltungsanbieter unterstützt:

* GitHub
* BitBucket
* OneDrive
* Dropbox

## <a name="view-deployment-sources-in-app-service-administration"></a>Anzeigen der Bereitstellungsquellen in der App Service-Verwaltung

1. Melden Sie sich beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an.
2. Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus.  ![App Service Resource Provider Admin][1] (Administrator von App Service-Ressourcenanbieter)
3. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).  Hier sehen Sie die Liste aller konfigurierten Bereitstellungsquellen.
    ![App Service Resource Provider Admin: Source Control Configuration][2] (Administrator von App Service-Ressourcenanbieter: Konfiguration der Quellcodeverwaltung)

## <a name="configure-github"></a>Konfigurieren von GitHub

> [!NOTE]
> Sie benötigen ein GitHub-Konto, um diese Aufgabe abzuschließen.  Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Melden Sie sich bei GitHub an, navigieren Sie zu https://www.github.com/settings/developers, und klicken Sie auf **Register a new application** (Neue Anwendung registrieren). ![GitHub – Registrieren einer neuen Anwendung][3]
2. Geben Sie in **Application name** einen Anwendungsnamen ein, z.B. App Service in Azure Stack
3. Geben Sie in **Homepage URL** die URL der Startseite ein.  **Die URL der Startseite muss die Adresse des Azure Stack-Portals sein**, z.B. https://portal.local.azurestack.external.
4. Geben Sie in **Application Description** eine Anwendungsbeschreibung ein.
5. Geben Sie in **Authorization callback URL** die Autorisierungsrückruf-URL ein.  In einer Standardbereitstellung von Azure Stack hat die URL das Format „https://portal.local.azurestack.external/tokenauthorize“. Wenn Sie eine andere Domäne verwenden, ersetzen Sie „azurestack.local“ durch Ihre Domäne. ![GitHub – Registrieren einer neuen Anwendung mit aufgefüllten Werten][4]
6. Klicken Sie zum Registrieren der Anwendung auf **Register application**.  Nun wird die Seite mit der **Client ID** und dem **Client Secret** (geheimen Clientschlüssel) für die Anwendung angezeigt.
    ![GitHub: Anwendungsregistrierung abgeschlossen][5]
7.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an. 
8.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus. 
9. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
10. Kopieren Sie die **Client-ID** und den **geheimen Clientschlüssel** in die entsprechenden Eingabefelder für GitHub.
11. Klicken Sie auf **Speichern**.
12. Wenn Sie keine anderen Bereitstellungsquellen konfigurieren möchten, fahren Sie mit dem [Planen der Reparatur von Verwaltungsrollen](azure-stack-app-service-configure-deployment-sources.md#schedule-repair-of-management-roles) fort.


## <a name="configure-bitbucket"></a>Konfigurieren von BitBucket

> [!NOTE]
> Sie benötigen ein BitBucket-Konto, um diese Aufgabe abzuschließen.  Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Melden Sie sich bei BitBucket an, und navigieren Sie unter Ihrem Konto zu **Integrations** (Integrationen). ![BitBucket-Dashboard – Integrationen][7]
2. Klicken Sie unter „Access Management“ (Zugriffsverwaltung) auf **OAuth** und **Add Consumer** (Consumer hinzufügen). ![BitBucket: Hinzufügen eines OAuth-Consumers][8]
3. Geben Sie in **Name** einen Namen für den Consumer ein, z.B. App Service in Azure Stack
4. Geben Sie in **Description** eine Beschreibung der Anwendung ein.
5. Geben Sie in **Callback URL** die Rückruf-URL ein.  In einer Standardbereitstellung von Azure Stack hat die Rückruf-URL das Format „https://portal.local.azurestack.external/TokenAuthorize“. Wenn Sie eine andere Domäne verwenden, ersetzen Sie „azurestack.local“ durch Ihre Domäne.  Die URL muss die hier aufgeführte Groß-/Kleinschreibung befolgen, damit die BitBucket-Integration erfolgreich ist.
6. Geben Sie die **URL** ein. Dieses URL muss die des Azure Stack-Portals sein, z.B. https://portal.local.azurestack.external.
7. Wählen Sie die erforderlichen **Berechtigungen** aus. **Repositorys**: **Lesen** **Webhooks**: **Lesen und schreiben**
8. Klicken Sie auf **Speichern**.  Unter **OAuth consumers** (OAuth-Consumer) wird diese neue Anwendung zusammen mit **Key** (Schlüssel) und **Secret** (Geheimnis) angezeigt.
    ![BitBucket: Anwendungsliste][9]
9.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an. 
10.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus. 
11. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
12. Kopieren Sie den **Schlüssel** in das Eingabefeld **Client-ID** und das **Geheimnis** in das Eingabefeld **Client Secret** (Geheimer Clientschlüssel) für BitBucket.
13. Klicken Sie auf **Speichern**.
14. Wenn Sie keine anderen Bereitstellungsquellen konfigurieren möchten, fahren Sie mit dem [Planen der Reparatur von Verwaltungsrollen](azure-stack-app-service-configure-deployment-sources.md#schedule-repair-of-management-roles) fort.

## <a name="configure-onedrive"></a>Konfigurieren von OneDrive

> [!NOTE]
> OneDrive for Business-Konten werden zurzeit nicht unterstützt.  Sie benötigen für diese Aufgabe ein mit einem OneDrive-Konto verknüpftes Microsoft-Konto.  Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Navigieren Sie zu „https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm“, und melden Sie sich mit Ihrem Microsoft-Konto an.
2. Klicken Sie unter **Meine Anwendungen**
![OneDrive-Anwendungen][10] auf **Eine App hinzufügen**.
3. Geben Sie für „Registrierung einer neuen Anwendung“ einen **Namen** wie **App Service in Azure Stack** ein, und klicken Sie auf **Anwendung erstellen**.
4. Der nächste Bildschirm enthält die Eigenschaften der neuen Anwendung. Notieren Sie die Angaben für **Anwendungs-ID**
![OneDrive-Anwendungseigenschaften][11].
5. Klicken Sie unter **Anwendungsgeheimnisse** auf **Neues Kennwort generieren**, und notieren Sie das Kennwort in **Neues Kennwort wurde generiert**, das Ihr Anwendungsgeheimnis ist.
> [!NOTE]
> Notieren Sie unbedingt das neue Kennwort, da es nicht mehr abgerufen werden kann, nachdem Sie in dieser Phase auf „OK“ geklickt haben.
6. Klicken Sie unter **Plattformen** auf **Plattform hinzufügen**, und wählen Sie **Web** aus.
7. Geben Sie den **Umleitungs-URI** ein.  In einer Standardbereitstellung von Azure Stack hat die Umleitungs-URL das Format „https://portal.local.azurestack.external/tokenauthorize“. Wenn Sie eine andere Domäne verwenden, ersetzen Sie „azurestack.local“ durch Ihre Domäne. ![OneDrive-Anwendung – Hinzufügen einer Webplattform][12]
8. Legen Sie **Microsoft Graph-Berechtigungen** - **Delegierte Berechtigungen** fest.
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive-Anwendung: Graph-Berechtigungen][13]
10. Klicken Sie auf **Speichern**.
11.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an. 
12.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus. 
13. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
14. Kopieren Sie die **Anwendungs-ID** in das Eingabefeld **Client-ID** und das **Kennwort** in das Eingabefeld **Geheimer Clientschlüssel** für OneDrive.
15. Klicken Sie auf **Speichern**.
16. Wenn Sie keine anderen Bereitstellungsquellen konfigurieren möchten, fahren Sie mit dem [Planen der Reparatur von Verwaltungsrollen](azure-stack-app-service-configure-deployment-sources.md#schedule-repair-of-management-roles) fort.

## <a name="configure-dropbox"></a>Konfigurieren von Dropbox

> [!NOTE]
> Sie benötigen für diese Aufgabe ein Dropbox-Konto.  Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Navigieren Sie zu „https://www.dropbox.com/developers/apps“, und melden Sie sich mit Ihrem Dropbox-Konto an.
2. Klicken Sie auf **Create app** 
![Dropbox applications][14] (Anwendung erstellen>Dropbox-Anwendungen).
3. Aktivieren Sie **Dropbox API**.
4. Legen Sie die Zugriffsebene auf **App Folder** (App-Ordner) fest.
5. Geben Sie einen **Namen** für Ihre Anwendung ein.
![Registrierung der Dropbox-Anwendung][15]
6. Klicken Sie auf **Create app** (App erstellen).  Nun wird eine Seite mit den Einstellungen für die App angezeigt, wie z.B. **App key** (App-Schlüssel) und **App secret** (App-Geheimnis).
7. Prüfen Sie, ob **App folder name** (Name des App-Ordners) auf **App Service in Azure Stack** festgelegt ist.
8. Legen Sie den **OAuth 2 Redirect URI** (OAuth 2-Umleitungs-URI) fest, und klicken Sie auf **Add** (Hinzufügen).  In einer Standardbereitstellung von Azure Stack hat die Umleitungs-URL das Format „https://portal.local.azurestack.external/tokenauthorize“. Wenn Sie eine andere Domäne verwenden, ersetzen Sie „azurestack.local“ durch Ihre Domäne. ![Dropbox-Anwendungskonfiguration][16]
9.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an. 
10.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus. 
11. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
12. Kopieren Sie den **Application Key** (Anwendungsschlüssel) in das Eingabefeld **Client-ID** und das **App secret** (App-Geheimnis) in das Eingabefeld **Geheimer Clientschlüssel** für Dropbox.
13. Klicken Sie auf **Speichern**.
14. Wenn Sie keine anderen Bereitstellungsquellen konfigurieren möchten, fahren Sie mit dem [Planen der Reparatur von Verwaltungsrollen](azure-stack-app-service-configure-deployment-sources.md#schedule-repair-of-management-roles) fort.

## <a name="schedule-repair-of-management-roles"></a>Planen der Reparatur von Verwaltungsrollen
Damit die in der Konfiguration der verschiedenen Bereitstellungsquellen aktualisierten Einstellungen übernommen werden, müssen die Verwaltungsrollen repariert werden.  Dieser Vorgang stellt sicher, dass die Konfigurationswerte ordnungsgemäß übernommen und die konfigurierten Bereitstellungsquellen den Mandanten zur Verfügung gestellt werden.

1. Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster beim Azure Stack-Administratorportal (https://adminportal.local.azurestack.external) als Dienstadministrator an.
2. Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus.
3. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
4. Kopieren Sie die **Client-ID** und den **geheimen Clientschlüssel** in die entsprechenden Eingabefelder für GitHub.
5. Klicken Sie unten auf der Seite auf **Speichern**
6. Klicken Sie auf **Rollen**.
7. Klicken Sie auf **Verwaltungsserver**.
8. Klicken Sie auf **Alle reparieren**, und wählen Sie **Ja**.  Dieser Vorgang plant eine Reparatur auf allen Verwaltungsservern, um die Integration abzuschließen.  Die Reparaturvorgänge erfolgen mit minimierten Ausfallzeiten.
    ![Administrator von App Service-Ressourcenanbieter – Rollen - Verwaltungsserver – Alles reparieren][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png

