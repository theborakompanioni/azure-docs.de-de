---
title: Integrieren von Anwendungen in Azure Active Directory | Microsoft Docs
description: "Erfahren Sie, wie Sie eine Anwendung in Azure AD hinzufügen, aktualisieren oder sie aus Azure Active Directory (Azure AD) entfernen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: ae637be5-0b71-4b1e-b1fe-b83df3eb4845
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: mbaldwin;bryanla
translationtype: Human Translation
ms.sourcegitcommit: b5dbb8c28bd6b2bdbb53939314348104bbbe4f34
ms.openlocfilehash: f6ec7634a1d21c7205ac8ae7377a312ed386ee61


---
# <a name="integrating-applications-with-azure-active-directory"></a>Integrieren von Anwendungen in Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Unternehmensentwickler und SaaS-Anbieter (Software-as-a-Service) können kommerzielle Clouddienste oder Branchenanwendungen entwickeln, die in Azure Active Directory (Azure AD) integriert werden können, um eine sichere Anmeldung und Autorisierung für ihre Dienste bereitzustellen Um eine Anwendung oder einen Dienst in Azure AD integrieren zu können, muss ein Entwickler zunächst die Details zu seiner Anwendung mithilfe des klassischen Azure-Portals in Azure AD registrieren.

Der vorliegende Artikel zeigt, wie Sie eine Anwendung in Azure AD hinzufügen, aktualisieren oder aus sie aus Azure AD entfernen. Sie lernen die verschiedenen Anwendungstypen kennen, die in Azure AD integriert werden können, und erfahren, wie Sie Ihre Anwendungen für den Zugriff auf andere Ressourcen (z. B. auf Web-APIs) konfigurieren.

Weitere Informationen zu den beiden Azure AD-Objekten, die eine registrierte Anwendung darstellen, und der Beziehung zwischen ihnen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md). Weitere Informationen zu den Richtlinien zum Branding bei der Anwendungsentwicklung mit Azure Active Directory finden Sie unter [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md).

## <a name="adding-an-application"></a>Hinzufügen einer Anwendung
Jede Anwendung muss zunächst in einem Azure AD-Mandanten registriert werden, um die Funktionen von Azure AD nutzen zu können. Dieser Registrierungsvorgang umfasst das Angeben von Details zu Ihrer Anwendung in Azure AD. Beispielsweise muss die URL für den Speicherort angegeben werden, die URL, an die nach der Authentifizierung eines Benutzers Antworten gesendet werden sollen, die URI zum Identifizieren der App usw.

Wenn Sie eine Webanwendung entwickeln, für die nur eine Benutzeranmeldung in Azure AD erforderlich ist, können Sie einfach die nachstehend aufgeführten Anweisungen befolgen. Wenn Ihre Anwendung Anmeldeinformationen oder Berechtigungen für den Zugriff auf eine Web-API benötigt oder Benutzer aus anderen Azure AD-Mandanten darauf zugreifen müssen, fahren Sie mit dem Abschnitt [Aktualisieren einer Anwendung](#updating-an-application) fort, um Ihre Anwendung weiter zu konfigurieren.

### <a name="to-register-a-new-application-in-the-azure-portal"></a>Registrieren einer neuen Anwendung im Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im linken Navigationsbereich **Weitere Dienste**, und klicken Sie auf **App-Registrierungen** und auf **Hinzufügen**.
4. Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Spezifische Beispiele für Webanwendungen oder native Anwendungen finden Sie in unseren [Schnellstarts](active-directory-developers-guide.md).
  * Geben Sie für Webanwendungen die **Anmelde-URL** an. Dies ist die Basis-URL Ihrer App, unter der sich Benutzer anmelden können, z.B. `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Geben Sie für native Anwendungen einen **Umleitungs-URI** an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.
5. Nach Abschluss der Registrierung weist Azure AD Ihrer Anwendung eine eindeutige Client-ID zu, die Anwendungs-ID. Ihre Anwendung wird hinzugefügt, und Sie werden auf die Seite "Schnellstart" für Ihre Anwendung umgeleitet. Abhängig davon, ob es sich bei Ihrer Anwendung um eine Web- oder eine systemeigene Anwendung handelt, werden andere Optionen zum Hinzufügen weiterer Funktionen zu Ihrer Anwendung angezeigt. Nachdem Ihre Anwendung hinzugefügt wurde, können Sie beginnen, Ihre Anwendung zu aktualisieren, um Benutzern die Anmeldung zu ermöglichen, auf Web-APIs in anderen Anwendungen zuzugreifen oder eine mehrinstanzenfähige Anwendung zu konfigurieren (mit der es anderen Organisationen ermöglicht wird, auf Ihre Anwendung zuzugreifen).

> [!NOTE]
> Standardmäßig wird die neu erstellte Anwendung bei der Registrierung so konfiguriert, dass sich Benutzer aus Ihrem Verzeichnis bei der Anwendung anmelden können.
> 
> 

## <a name="updating-an-application"></a>Aktualisieren einer Anwendung
Nachdem Ihre Anwendung in Azure AD registriert wurde, muss sie ggf. aktualisiert werden, um Zugriff auf Web-APIs bereitzustellen, für andere Unternehmen zur Verfügung gestellt zu werden usw.  In diesem Abschnitt werden verschiedene Möglichkeiten beschrieben, Ihre Anwendung weiter zu konfigurieren. Zunächst erhalten Sie eine Übersicht über das Consent Framework. Dieses müssen Sie kennen, wenn Sie Ressourcen-/API-Anwendungen zur Nutzung durch Clientanwendungen erstellen, die von Entwicklern in Ihrer oder einer anderen Organisation erstellt wurden.

Weitere Informationen zur Funktionsweise der Authentifizierung in Azure AD finden Sie unter [Authentifizierungsszenarien für Azure AD](active-directory-authentication-scenarios.md).

### <a name="overview-of-the-consent-framework"></a>Übersicht über das Consent Framework
Das Consent Framework von Azure AD erleichtert die Entwicklung mehrinstanzenfähiger Web- und nativer Clientanwendungen, die Zugriff auf Web-APIs benötigen, die von einem anderen Azure AD-Mandanten gesichert werden als dem, bei dem die Clientanwendung registriert ist. Zu diesen Web-APIs gehören neben Ihren eigenen Web-APIs die Graph-API, Office 365 und anderen Microsoft-Dienste. Das Framework basiert darauf, dass Benutzer oder Administratoren ihre Zustimmung zur Registrierung einer Anwendung in ihrem Verzeichnis erteilen. Diese Zustimmung kann auch den Zugriff auf Verzeichnisdaten umfassen.

Wenn beispielsweise eine Webclientanwendung die Web-API-/Ressourcenanwendung von Office 365 zum Lesen von Kalenderinformationen über einen Benutzer aufrufen muss, muss dieser Benutzer der Clientanwendung seine Zustimmung erteilen. Anschließend kann die Clientanwendung die Office 365-Web-API im Namen des Benutzers aufrufen und die Kalenderinformationen nach Bedarf verwenden.

Das Consent Framework basiert auf OAuth 2.0 und seinen verschiedenen Datenflüssen, z. B. Authorization Code Grant und Client Credentials Grant. Dabei kommen öffentliche oder vertrauliche Clients zum Einsatz. Durch die Verwendung von OAuth 2.0 ermöglicht Azure AD die Entwicklung zahlreicher verschiedener Typen von Clientanwendungen, z. B. für Telefon, Tablet, Server oder Web, und ermöglicht den Zugriff auf die erforderlichen Ressourcen.

Ausführlichere Informationen zum Consent Framework finden Sie unter [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx), [Authentifizierungsszenarios für Azure AD](active-directory-authentication-scenarios.md) und im Office 365-Thema [Grundlegendes zur Authentifizierung mit Office 365-APIs](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks).

#### <a name="example-of-the-consent-experience"></a>Beispiel für die Zustimmungsbenutzeroberfläche
Die folgenden Schritte zeigen, wie das Consent Framework in der Benutzeroberfläche für den Anwendungsentwickler und für den Benutzer umgesetzt wird.

1. Legen Sie im Azure-Portal auf der Seite für die Konfiguration Ihrer Webclientanwendung die erforderlichen Berechtigungen für Ihre Anwendung fest, indem Sie die Menüs im Abschnitt „Erforderliche Berechtigungen“ verwenden.
   
    ![Berechtigungen für andere Anwendungen](./media/active-directory-integrating-applications/requiredpermissions.png)
2. Angenommen, die Berechtigungen für Ihre Anwendung wurden aktualisiert, die Anwendung wird ausgeführt, und ein Benutzer möchte die Anwendung zum ersten Mal verwenden. Falls die Anwendung noch kein Zugriffs- oder Aktualisierungstoken abgerufen hat, muss die Anwendung über den Autorisierungsendpunkt von Azure AD einen Autorisierungscode anfordern, der anschließend verwendet werden kann, um ein neues Zugriffs- und Aktualisierungstoken abzurufen.
3. Falls der Benutzer noch nicht authentifiziert wurde, wird er zur Anmeldung bei Azure AD aufgefordert.
   
    ![Benutzer- oder Administratoranmeldung bei Azure AD](./media/active-directory-integrating-applications/usersignin.png)
4. Nachdem der Benutzer sich angemeldet hat, ermittelt Azure AD, ob für den Benutzer eine Seite für die Zustimmungserteilung angezeigt werden muss. Das Ergebnis dieser Ermittlung ist davon abhängig, ob der Benutzer (oder der Administrator seiner Organisation) die Anwendungszustimmung bereits erteilt hat. Wenn die Zustimmung noch nicht erteilt wurde, fordert Azure AD den Benutzer zur Zustimmung auf und zeigt an, welche Berechtigungen für die Funktionalität benötigt werden. Der Berechtigungssatz, der im Dialogfeld für die Zustimmung angezeigt wird, ist identisch mit der Auswahl unter „Delegierte Berechtigungen“ im Azure-Portal.
   
    ![Oberfläche für die Benutzerzustimmung](./media/active-directory-integrating-applications/consent.png)
5. Nachdem der Benutzer seine Zustimmung erteilt hat, wird ein Autorisierungscode an Ihre Anwendung zurückgegeben, mit dem ein Zugriffs- und Aktualisierungstoken abgerufen werden kann. Weitere Informationen zu diesem Datenfluss finden Sie im Abschnitt [Webanwendung zu Web-API](active-directory-authentication-scenarios.md#web-application-to-web-api) im Thema [Authentifizierungsszenarios für Azure AD](active-directory-authentication-scenarios.md).

6. Als Administrator können Sie auch für alle Benutzer in Ihrem Mandanten den delegierten Berechtigungen einer Anwendung zustimmen. Dadurch verhindern Sie, dass das Zustimmungsdialogfeld jedem Benutzer im Mandanten angezeigt wird. Rufen Sie dazu auf Ihrer Anwendungsseite das [Azure-Portal](https://portal.azure.com) auf. Klicken Sie auf dem Blatt **Einstellungen** für Ihre Anwendung auf **Erforderliche Berechtigungen**, und klicken Sie auf die Schaltfläche **Berechtigungen erteilen**. 

    ![Erteilen von Berechtigungen für explizite Administratorzustimmung](./media/active-directory-integrating-applications/grantpermissions.png)
    
> [!NOTE]
> Ihre explizite Zustimmung über die Schaltfläche **Berechtigungen erteilen** Schaltfläche ist derzeit für Single-Anwendungen (SPA) mit ADAL.js erforderlich. Wird das Zugriffstoken ohne Zustimmungsaufforderung angefordert, schlägt es fehl, wenn noch keine Berechtigung erteilt wurde.   

### <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs
Damit eine Webanwendung oder vertrauliche Clientanwendung an einem Flow zur Autorisierungsgenehmigung teilnehmen kann, bei dem eine Authentifizierung (und das Abrufen eines Zugriffstokens) erforderlich ist, benötigt sie sichere Anmeldeinformationen. Die Standardauthentifizierungsmethode im Azure-Portal ist Client-ID + symmetrischer Schlüssel. In diesem Abschnitt werden die erforderlichen Konfigurationsschritte zum Bereitstellen des geheimen Schlüssels für die Anmeldeinformationen des Clients behandelt.

Bevor ein Client Zugriff auf eine Web-API erhält, die durch eine Ressourcenanwendung (d.h. Azure AD Graph-API) bereitgestellt wird, stellt das Consent Framework außerdem sicher, dass dem Client die erforderlichen Berechtigungen basierend auf den angeforderten Berechtigungen erteilt werden. Standardmäßig können alle Anwendungen Berechtigungen aus Azure Active Directory (Graph-API) und der Azure Service Management-API auswählen. Hierbei ist die Azure AD-Berechtigung "Anmeldung aktivieren und Benutzerprofil lesen" bereits standardmäßig ausgewählt. Wenn Ihre Clientanwendung bei einem Office 365-Azure AD-Mandanten registriert ist, stehen Web-APIs und Berechtigungen für SharePoint und Exchange Online ebenfalls zur Auswahl. In den Dropdownmenüs neben der gewünschten Web-API können Sie zwischen [zwei Arten von Berechtigungen](active-directory-dev-glossary.md#permissions) wählen:

* Anwendungsberechtigungen: Ihre Clientanwendung benötigt direkten Zugriff auf die Web-API selbst (kein Benutzerkontext). Für diese Art von Berechtigung ist die Zustimmung des Administrators erforderlich, sie steht nicht für systemeigene Clientanwendungen zur Verfügung.
* Delegierte Berechtigungen: Ihre Clientanwendung benötigt als angemeldeter Benutzer Zugriff auf die Web-API. Der Zugriff ist jedoch durch die ausgewählte Berechtigung eingeschränkt. Diese Art von Berechtigung kann von einem Benutzer erteilt werden – es sei denn, die Berechtigung ist so konfiguriert, dass sie die Zustimmung durch einen Administrator erfordert. 

> [!NOTE]
> Durch Hinzufügen einer delegierten Berechtigung zu einer Anwendung erteilen Sie den Benutzern im Mandanten nicht automatisch Ihre Zustimmung wie zuvor im klassischen Azure-Portal. Die Benutzer müssen den hinzugefügten delegierten Berechtigungen bei der Ausführung noch manuell zustimmen, sofern der Administrator nicht auf die Schaltfläche **Erteilen von Berechtigungen** im Abschnitt **Erforderliche Berechtigungen** der Anwendungsseite im Azure-Portal klickt. 

#### <a name="to-add-credentials-or-permissions-to-access-web-apis"></a>So fügen Sie Anmeldeinformationen oder Zugriffsberechtigungen für Web-APIs hinzu
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Klicken Sie auf dem Blatt mit den Einstellungen auf den Abschnitt „Schlüssel“, um einen geheimen Schlüssel für die Anmeldeinformationen Ihrer Webanwendung hinzuzufügen.  
   
   * Fügen Sie eine Beschreibung für Ihren Schlüssel hinzu, und wählen Sie eine Dauer von 1 oder 2 Jahren. 
   * Die Spalte ganz rechts enthält nach dem Speichern der Konfigurationsänderungen den Schlüsselwert. Sie müssen unbedingt zu diesem Abschnitt zurückkehren und ihn nach dem Klicken auf „Speichern“ kopieren, damit Sie ihn zur Laufzeit während der Authentifizierung in Ihrer Clientanwendung verwenden können.
5. Klicken Sie zum Hinzufügen von Berechtigungen für Ressourcen-APIs von Ihrem Client auf dem Blatt „Einstellungen“ auf den Abschnitt „Erforderliche Berechtigungen“. 
   
   * Klicken Sie dann auf die Schaltfläche „Hinzufügen“.
   * Klicken Sie auf „Hiermit wählen Sie eine API aus.“, um den Typ der Ressourcen auszuwählen, unter denen Sie auswählen möchten.
   * Durchsuchen Sie die Liste der verfügbaren APIs, oder verwenden Sie das Suchfeld, um die verfügbaren Ressourcenanwendungen in Ihrem Verzeichnis auszuwählen, die eine Web-API verfügbar machen. Klicken Sie auf die Ressource, an der Sie interessiert sind, und dann auf **Wählen**.
   * Nach dem Auswählen können Sie zum Menü **Auswahlberechtigungen** wechseln, in dem Sie „Anwendungsberechtigungen“ und „Delegierte Berechtigungen“ für Ihre Anwendung auswählen können.
   
6. Wenn Sie fertig sind, klicken Sie auf die Schaltfläche **Fertig**.

> [!NOTE]
> Wenn Sie auf die Schaltfläche **Fertig** klicken, werden basierend auf den von Ihnen konfigurierten Berechtigungen für andere Anwendungen automatisch die Berechtigungen für die Anwendung in Ihrem Verzeichnis festgelegt.  Sie können diese Anwendungsberechtigungen auf den Blatt **Einstellungen** anzeigen.
> 
> 

### <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs
Sie können eine Web-API entwickeln und sie Clientanwendungen zur Verfügung stellen, indem Sie [Zugriffsbereiche](active-directory-dev-glossary.md#scopes) und [Rollen](active-directory-dev-glossary.md#roles) verfügbar machen. Eine ordnungsgemäß konfigurierte Web-API wird auf die gleiche Weise wie andere Microsoft-Web-APIs (einschließlich der Graph-API und der Office 365-APIs) zur Verfügung gestellt. Zugriffsbereiche und Rollen werden durch Ihr [Anwendungsmanifest](active-directory-dev-glossary.md#application-manifest) verfügbar gemacht. Hierbei handelt es sich um eine JSON-Datei, die die Identitätskonfiguration Ihrer Anwendung repräsentiert.  

Im folgende Abschnitt erfahren Sie, wie Sie Zugriffsbereiche verfügbar machen, indem Sie das Manifest der Ressourcenanwendung bearbeiten.

#### <a name="adding-access-scopes-to-your-resource-application"></a>Hinzufügen von Zugriffsbereichen zu Ihrer Ressourcenanwendung
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Klicken Sie auf der Anwendungsseite auf **Manifest**, um den Inline-Manifest-Editor zu öffnen. 
5. Ersetzen Sie den „oauth2Permissions“-Knoten mit dem folgenden JSON-Codeausschnitt. Dieser Codeausschnitt ist ein Beispiel dafür, wie Sie einen Bereich verfügbar machen, der als „Benutzeridentitätswechsel“ bekannt ist. Damit kann ein Ressourcenbesitzer einer Clientanwendung eine Art delegierten Zugriff auf eine Ressource gewähren. Stellen Sie sicher, dass Sie den Text und die Werte für Ihre eigene Anwendung ändern:
   
        "oauth2Permissions": [
        {
            "adminConsentDescription": "Allow the application full access to the Todo List service on behalf of the signed-in     user",
            "adminConsentDisplayName": "Have full access to the Todo List service",
            "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
            "isEnabled": true,
            "type": "User",
            "userConsentDescription": "Allow the application full access to the todo service on your behalf",
            "userConsentDisplayName": "Have full access to the todo service",
            "value": "user_impersonation"
            }
        ],
   
    Der Wert „id“ muss eine neu generierte GUID sein. Diese erstellen Sie mithilfe eines [Tools zum Generieren von GUIDs](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx) oder programmgesteuert. Die GUID repräsentiert einen eindeutigen Bezeichner für die Berechtigung, die von der Web-API bereitgestellt wird. Nachdem der Client ordnungsgemäß zum Anfordern des Zugriffs auf Ihre Web-API konfiguriert wurde und die Web-API-aufruft, stellt er ein OAuth 2.0 JWT-Token bereit, dessen Bereichsanspruch (scp) auf den oben genannten Wert festgelegt ist. In diesem Fall ist dies "User_impersonation".
   
   > [!NOTE]
   > Sie können später bei Bedarf zusätzliche Bereiche verfügbar machen. Berücksichtigen Sie, dass Ihre Web-API verschiedene Berechtigungen verfügbar machen kann, die einer Vielzahl von unterschiedlichen Funktionen zugeordnet sind. Jetzt können Sie den Zugriff auf die Web-API steuern, indem Sie den Bereichsanspruch (scp) im empfangenen OAuth 2.0 JWT-Token verwenden.
   > 
   > 
6. Klicken Sie auf **Speichern**, um das Manifest zu speichern. Ihre Web-API ist jetzt so konfiguriert, dass sie von anderen Anwendungen im Verzeichnis verwendet werden kann.

#### <a name="to-verify-the-web-api-is-exposed-to-other-applications-in-your-directory"></a>So überprüfen Sie, ob die Web-API für andere Anwendungen in Ihrem Verzeichnis bereitgestellt wird
1. Klicken Sie im oberen Menü auf **App-Registrierungen**, wählen Sie die Clientanwendung aus, für die Sie den Zugriff auf die Web-API konfigurieren möchten, und navigieren Sie zum Blatt „Einstellungen“.
2. Wählen Sie im Abschnitt **Erforderliche Berechtigungen** die Web-API aus, für die Sie soeben eine Berechtigung bereitgestellt haben. Wählen Sie im Dropdownmenü „Delegierte Berechtigungen“ die neue Berechtigung aus.

![Anzeige von Aufgabenlistenberechtigungen](./media/active-directory-integrating-applications/todolistpermissions.png)

#### <a name="more-on-the-application-manifest"></a>Weitere Informationen zum Anwendungsmanifest
Das Anwendungsmanifest dient als Mechanismus zum Aktualisieren der Anwendungsentität, der alle Attribute der Identitätskonfiguration einer Azure AD-Anwendung einschließlich der erörterten API-Zugriffsbereiche definiert. Weitere Informationen zur Anwendungsentität finden Sie in der [Dokumentation zur Anwendungsentität der Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Dort finden Sie umfassende Referenzinformationen zu den Anwendungsentitätselementen, die verwendet werden, um Berechtigungen für Ihre API anzugeben:  

* Das appRoles-Mitglied, das eine Sammlung von [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)-Entitäten darstellt, die zum Definieren der **Anwendungsberechtigungen** für eine Web-API verwendet werden können  
* Das oauth2Permissions-Mitglied, das eine Sammlung von [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)-Entitäten darstellt, die zum Definieren der **delegierten Berechtigungen** für eine Web-API verwendet werden können

Weitere Informationen zu Anwendungsmanifestkonzepten im Allgemeinen finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](active-directory-application-manifest.md).

### <a name="accessing-the-azure-ad-graph-and-office-365-apis"></a>Zugreifen auf die Azure AD-Graph- und Office 365-APIs
Wie bereits zuvor erwähnt, können Sie zusätzlich zum Verfügbarmachen von/Zugreifen auf APIs über Ihre eigenen Ressourcenanwendungen auch Ihre Clientanwendung für den Zugriff auf APIs aktualisieren, die von Microsoft-Ressourcen verfügbar gemacht werden.  Die Azure AD Graph-API, die in der Liste der Berechtigungen für andere Anwendungen als „Azure Active Directory“ bezeichnet wird, ist standardmäßig für alle Anwendungen verfügbar, die mit Azure AD registriert sind. Wenn Sie Ihre Clientanwendung in einem Azure AD-Mandanten registrieren, der von Office 365 bereitgestellt wurde, können Sie auch auf alle Berechtigungen zugreifen, die durch die APIs für verschiedene Office 365-Ressourcen verfügbar gemacht werden.

Eine vollständige Erläuterung zu Zugriffsbereichen, die durch Folgendes verfügbar gemacht werden:  

* Graph-API von Azure AD, siehe Artikel [Berechtigungsbereiche | Graph-API-Konzepte](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).
* Office 365-APIs, finden Sie im Artikel [Authentifizierung und Autorisierung mit dem allgemeinen Consent Framework](https://msdn.microsoft.com/office/office365/howto/application-manifest) . Unter [Einrichten der Office 365-Entwicklungsumgebung](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment) finden Sie eine umfangreiche Erörterung zum Erstellen einer Client-App, die in Office 365-APIs integriert wird.

> [!NOTE]
> Aufgrund einer aktuellen Einschränkung können systemeigene Clientanwendungen die Azure AD Graph-API nur aufrufen, wenn sie die Berechtigung "Zugriff auf das Verzeichnis Ihrer Organisation" verwenden.  Diese Einschränkung gilt nicht für Webanwendungen.
> 
> 

### <a name="configuring-multi-tenant-applications"></a>Konfigurieren von mehrinstanzenfähigen Anwendungen
Wenn Sie Azure AD eine Anwendung hinzufügen, möchten Sie vielleicht, dass auf Ihre Anwendung nur durch Benutzer in Ihrer Organisation zugegriffen werden kann. Alternativ sollen evtl. nur Benutzer in externen Organisationen auf Ihre Anwendung zugreifen können. Diese zwei Anwendungstypen werden als Einzelinstanzanwendung bzw. als mehrinstanzenfähige Anwendung bezeichnet. Sie können die Konfiguration einer Einzelinstanzanwendung so ändern, dass Sie eine mehrinstanzenfähige Anwendung erhalten. Dies wird weiter unten in diesem Abschnitt behandelt.

Es ist wichtig, die Unterschiede zwischen einer Einzelinstanzanwendung und einer mehrinstanzenfähigen Anwendung zu verstehen:  

* Eine Einzelinstanzanwendung ist für die Verwendung in einem einzelnen Unternehmen gedacht. Es handelt sich dabei zumeist um eine Branchenanwendung (Line-of-Business, LOB), die von einem Unternehmensentwickler geschrieben wurde. Auf eine Einzelinstanzanwendung greifen nur Benutzer aus einem einzelnen Verzeichnis zu. Daher muss sie auch nur in einem Verzeichnis bereitgestellt werden.
* Eine mehrinstanzenfähige Anwendung ist für die Verwendung in vielen Organisationen vorgesehen. Hierbei handelt es sich um eine SaaS-Webanwendung (Software-as-a-Service), die in der Regel von einem unabhängigen Softwarehersteller (ISV, Independent Software Vendor) geschrieben wurde. Mehrinstanzenfähige Anwendungen müssen in jedem Verzeichnis bereitgestellt werden, in dem sie verwendet werden. Für die Registrierung ist also jeweils die Zustimmung des Benutzers oder Administrators erforderlich, unterstützt durch das Azure AD-Consent Framework. Beachten Sie, dass alle nativen Clientanwendungen standardmäßig mehrinstanzenfähig sind, da sie auf dem Gerät des Ressourcenbesitzers installiert sind. Weitere Informationen zum Consent Framework finden Sie oben in der „Übersicht über das Consent Framework“.

#### <a name="enabling-external-users-to-grant-your-application-access-to-their-resources"></a>Externen Benutzern ermöglichen, Ihrer Anwendung Zugriff auf ihre Ressourcen zu gewähren
Wenn Sie eine Anwendung schreiben, die Sie Ihren Kunden oder Partnern außerhalb Ihrer Organisation zur Verfügung stellen möchten, müssen Sie die Anwendungsdefinition im Azure-Portal aktualisieren.

> [!NOTE]
> Wenn Sie die Mehrinstanzenfähigkeit aktivieren, müssen Sie sicherstellen, dass der App-ID-URI Ihrer Anwendung zu einer überprüften Domäne gehört. Darüber hinaus muss die Rückgabe-URL mit "https://" beginnen. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md).
> 
> 

So aktivieren Sie den Zugriff auf Ihre App für externe Benutzer: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Klicken Sie auf dem Blatt „Einstellungen“ auf **Eigenschaften** und schieben Sie den Regler für **Mehrinstanzenfähig** auf **Ja**.

Nachdem Sie die oben genannten Änderungen vorgenommen haben, können Benutzer und Administratoren in anderen Organisationen Ihrer Anwendung Zugriff auf ihr Verzeichnis und andere Daten gewähren.

#### <a name="triggering-the-azure-ad-consent-framework-at-runtime"></a>Auslösen von Azure AD-Consent Framework zur Laufzeit
Zur Verwendung des Consent Frameworks müssen mehrinstanzenfähige Clientanwendungen mithilfe von OAuth 2.0 eine Autorisierung anfordern. Es stehen [Codebeispiele](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant) zur Verfügung, die zeigen, wie eine Webanwendung, eine native Anwendung oder eine Server-/Daemonanwendung Autorisierungscodes und Zugriffstoken zum Aufrufen von Web-APIs anfordert.

Ihre Webanwendung kann ggf. auch eine Registrierungs-Benutzeroberfläche für Benutzer zur Verfügung stellen. Wenn Sie eine Registrierungsbenutzeroberfläche anbieten, muss der Benutzer üblicherweise auf eine Schaltfläche zur Registrierung klicken, die den Browser an den Azure AD OAuth2.0-Autorisierungsendpunkt oder einen OpenID Connect-Endpunkt mit Benutzerinformationen umleitet. Diese Endpunkte ermöglichen es der Anwendung, Informationen zum neuen Benutzer durch Untersuchen des id_token-Objekts abzurufen. Nach der Registrierungsphase wird der Benutzer, ähnlich wie oben im Abschnitt „Übersicht über das Consent Framework“ gezeigt, zur Zustimmung aufgefordert.

Alternativ kann Ihre Webanwendung auch eine Benutzeroberfläche bereitstellen, die es Administratoren ermöglicht, ihr Unternehmen zu registrieren. Eine solche Benutzeroberfläche würde den Benutzer ebenfalls an den Azure AD-OAuth 2.0-Autorisierungsendpunkt umleiten. In diesem Fall können Sie auch einen Parameter „prompt=admin_consent“ an den Autorisierungsendpunkt übergeben, um das Aufrufen der Benutzeroberfläche für die Administratorzustimmung zu erzwingen, in der der Administrator die Zustimmung im Namen seiner Organisation erteilt. Nur ein Benutzer, der sich mit einem Konto authentifiziert, das zur globalen Administratorrolle gehört, kann zustimmen; andere Benutzer erhalten eine Fehlermeldung. Bei erfolgter Zustimmung enthält die Antwort "admin_consent=true". Bei der Einlösung eines Zugriffstokens erhalten Sie ebenfalls ein "id_token", das Informationen zu der Organisation und zum Administrator bereitstellt, der sich für Ihre Anwendung registriert hat.

### <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivieren der impliziten OAuth 2.0-Gewährung für Single-Page-Anwendungen
Single-Page-Anwendungen (SPAs) sind in der Regel mit einem JavaScript-intensiven Front-End aufgebaut, das im Browser ausgeführt wird, der das Web-API-Back-End der Anwendung zum Ausführen deren Geschäftslogik aufruft. Für SPAs, die in Azure AD gehostet werden, verwenden Sie die implizite OAuth 2.0-Gewährung zum Authentifizieren des Benutzers für Azure AD und rufen ein Token ab, mit denen Sie Aufrufe vom JavaScript-Client der Anwendung an ihre Back-End-Web-API sichern können. Nachdem der Benutzer seine Zustimmung erteilt hat, können mit dem gleichen Authentifizierungsprotokoll Token zum Sichern von Aufrufen zwischen dem Client und anderen für die Anwendung konfigurierten Web-API-Ressourcen abgerufen werden. Weitere Informationen zur impliziten Gewährung der Autorisierung, damit Sie entscheiden können, ob diese sich für Ihr Anwendungsszenario eignet, finden Sie unter [Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD) ](active-directory-dev-understanding-oauth2-implicit-grant.md).

Die implizite OAuth 2.0-Gewährung ist für Anwendungen standardmäßig deaktiviert. Sie können die implizite OAuth 2.0-Gewährung für Ihre Anwendung aktivieren, indem Sie den Wert `oauth2AllowImplicitFlow` in ihrem [Anwendungsmanifest](active-directory-application-manifest.md) festlegen. Dabei handelt es sich um eine JSON-Datei, die die Identitätskonfiguration Ihrer Anwendung darstellt.

#### <a name="to-enable-oauth-20-implicit-grant"></a>So aktivieren Sie die implizite OAuth 2.0-Gewährung
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Klicken Sie auf der Anwendungsseite auf **Manifest**, um den Inline-Manifest-Editor zu öffnen.
   Suchen Sie den Wert „oauth2AllowImplicitFlow“ und ändern sie ihn in „True“. Der Standardwert ist „false“.
   
    `"oauth2AllowImplicitFlow": true,`
5. Speichern Sie das aktualisierte Manifest. Nach dem Speichern des Manifests ist Ihre Web-API für die Verwendung der impliziten OAuth 2.0-Gewährung zum Authentifizieren von Benutzern konfiguriert.


## <a name="removing-an-application"></a>Entfernen einer Anwendung
In diesem Abschnitt wird die Vorgehensweise beim Entfernen einer Anwendung aus Ihrem Azure AD-Mandanten beschrieben.

### <a name="removing-an-application-authored-by-your-organization"></a>Entfernen einer Anwendung, die von Ihrer Organisation erstellt wurde
Dies sind die Anwendungen, die unter den Filter „Anwendungen im Besitz meines Unternehmens“ auf der Hauptseite „Anwendungen“ für Ihren Azure AD-Mandanten angezeigt werden. Technisch gesehen sind dies Anwendungen, die Sie entweder manuell über das klassische Azure-Portal oder programmgesteuert über PowerShell oder die Graph-API registriert haben. Genauer gesagt, werden sie sowohl durch ein Anwendungs- als auch Dienstprinzipalobjekt in Ihrem Mandanten dargestellt. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md) .

#### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>So entfernen Sie eine Einzelinstanzanwendung aus Ihrem Verzeichnis
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Klicken Sie auf der Anwendungsseite auf **Löschen**.
5. Klicken Sie in der Bestätigungsmeldung auf **Ja** .

#### <a name="to-remove-a-multi-tenant-application-from-your-directory"></a>So entfernen Sie eine mehrinstanzenfähige Anwendung aus Ihrem Verzeichnis
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite Ihr Konto wählen.
3. Wählen Sie im oberen Menü **Azure Active Directory**, klicken Sie auf **App-Registrierungen** und dann auf die Anwendung, die Sie konfigurieren möchten. Die Schnellstart-Seite der Anwendung und das Blatt mit den Einstellungen der Anwendungen werden geöffnet.
4. Wählen Sie auf dem Blatt „Einstellungen“ die **Eigenschaften** und schieben Sie den Regler für **Mehrinstanzenfähig** auf **Nein**. Dies konvertiert Ihre Anwendung in eine Einzelinstanzanwendung. Die Anwendung verbleibt jedoch weiterhin in einer Organisation, die bereits ihre Zustimmung für diese Anwendung erteilt hat.
5. Klicken Sie auf der Anwendungsseite auf **Löschen**.
6. Klicken Sie in der Bestätigungsmeldung auf **Ja** .

### <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Entfernen einer mehrinstanzenfähigen Anwendung, die von einer anderen Organisation autorisiert ist
Dies ist eine Teilmenge der Anwendungen, die unter dem Filter „Anwendungen, die mein Unternehmen verwendet“ auf der Hauptseite „Anwendungen“ für Ihren Azure AD-Mandanten angezeigt werden, insbesondere diejenigen, die nicht in der Liste „Anwendungen im Besitz meines Unternehmens“ aufgeführt sind. Technisch gesehen sind dies mehrinstanzenfähige Anwendungen, die bei der Zustimmung registriert wurden. Genauer gesagt, werden sie nur durch ein Dienstprinzipalobjekt in Ihrem Mandanten dargestellt. Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md) .

Um den Zugriff einer mehrinstanzenfähigen Anwendung auf Ihr Verzeichnis zu entziehen (nachdem die Zustimmung erteilt wurde), muss der Unternehmensadministrator über ein Azure-Abonnement zum Entziehen des Zugriffs über das Azure-Portal verfügen. Alternativ kann der Unternehmensadministrator [Azure AD PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=294151) verwenden, um den Zugriff zu entziehen.

## <a name="next-steps"></a>Nächste Schritte
* Unter [Brandingrichtlinien für integrierte Apps](active-directory-branding-guidelines.md) finden Sie Tipps zu Darstellungsfragen für Ihre App.
* Ausführliche Informationen zu den Beziehungen zwischen Anwendungsobjekten und Dienstprinzipalobjekte der Anwendung finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](active-directory-application-objects.md).
* Weitere Informationen zur Rolle des App-Manifests finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](active-directory-application-manifest.md).
* Unter [Azure AD-Entwicklerglossar](active-directory-dev-glossary.md) finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).
* Im [Active Directory-Entwicklerhandbuch](active-directory-developers-guide.md) finden Sie eine Übersicht über alle für Entwickler relevanten Inhalte.




<!--HONumber=Feb17_HO2-->


