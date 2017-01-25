---
title: Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph | Microsoft Docs
description: "Hier erhalten Sie eine Einführung in die Abfrage einer Liste mit Risikoereignissen und zugehöriger Informationen in Microsoft Graph aus Azure Active Directory."
services: active-directory
keywords: Azure Active Directory Identity Protection, Risikoereignis, Sicherheitsrisiko, Sicherheitsrichtlinie, Microsoft Graph
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 77031b3bbe2372bf2cac34bac45368ac40911641
ms.openlocfilehash: 9c7c10031c068eeb02b4468ec48bf60aece2f12e


---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph
Microsoft Graph ist der einheitliche API-Endpunkt von Microsoft und stellt die [Azure Active Directory Identity Protection](active-directory-identityprotection.md) -APIs zur Verfügung. Mit der ersten API, **identityRiskEvents**, können Sie in Microsoft Graph eine Liste von [Risikoereignissen](active-directory-identityprotection-risk-events-types.md) und zugehörige Informationen abfragen. In diesem Artikel erhalten Sie eine kurze Einführung in das Abfragen dieser API. Eine ausführliche Einführung, die vollständige Dokumentation und Informationen zum Zugriff auf Graph-Explorer finden Sie auf der Website zu [Microsoft Graph](https://graph.microsoft.io/).


Zum Zugreifen auf die Identity Protection-Daten über Microsoft Graph werden drei Schritte ausgeführt:

1. Fügen Sie eine Anwendung mit einem geheimen Clientschlüssel hinzu. 
2. Authentifizieren Sie sich mit diesem geheimen Schlüssel und einigen anderen Informationen bei Microsoft Graph. Sie erhalten ein Authentifizierungstoken. 
3. Verwenden Sie dieses Token, um Anforderungen an den API-Endpunkt zu senden und Identity Protection-Daten zu erhalten.

Bevor Sie beginnen, benötigen Sie Folgendes:

* Administratorrechte, um die Anwendung in Azure AD erstellen zu können
* Den Namen der Domäne Ihres Mandanten (z.B. „contoso.onmicrosoft.com“)

## <a name="add-an-application-with-a-client-secret"></a>Hinzufügen einer Anwendung mit einem geheimen Clientschlüssel
1. [anmelden](https://manage.windowsazure.com) . 
2. Klicken Sie im linken Navigationsbereich auf **Active Directory**. 
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
4. Klicken Sie im Menü oben auf **Anwendungen**.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen**.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Führen Sie im Dialogfeld **Geben Sie uns Informationen zu Ihrer Anwendung.** die folgenden Schritte aus:
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. Geben Sie im Textfeld **Name** einen Namen für Ihre Anwendung ein (z.B. „AADIP-Risikoereignis-API-Anwendung“).
   
    b. Wählen Sie als **Typ** die Option **Webanwendung und/oder Web-API** aus.
   
    c. Klicken Sie auf **Weiter**.
8. Führen Sie im Dialogfeld **App-Eigenschaften** folgende Schritte aus:
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. Geben Sie im Textfeld **Anmelde-URL** die URL `http://localhost` ein.
   
    b. Geben Sie im Textfeld **App-ID-URI** den URI `http://localhost` ein.
   
    c. Klicken Sie auf **Fertig stellen**.

Sie können Ihre Anwendung nun konfigurieren.

![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a>Gewähren Ihrer Anwendung die Berechtigung zum Verwenden der API
1. Klicken Sie auf der Anwendungsseite im Menü oben auf **Konfigurieren**. 
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. Klicken Sie im Abschnitt **Berechtigungen für andere Anwendungen** auf **Anwendung hinzufügen**.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Führen Sie im Dialogfeld **Berechtigungen für andere Anwendungen** die folgenden Schritte aus:
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Wählen Sie **Microsoft Graph**.
   
    b. Klicken Sie auf **Fertig stellen**.
4. Klicken Sie auf **Anwendungsberechtigungen: 0**, und wählen Sie dann **Read all identity risk event information** (Alle Identitätsrisikoereignis-Informationen lesen) aus.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Klicken Sie unten auf der Seite auf **Speichern** .
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Abrufen eines Zugriffsschlüssels
1. Wählen Sie auf der Anwendungsseite im Abschnitt **Schlüssel** als Dauer „1 Jahr“ aus.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Klicken Sie unten auf der Seite auf **Speichern** .
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. Kopieren Sie im Abschnitt „Schlüssel“ den Wert des neu erstellten Schlüssels, und legen Sie ihn an einem sicheren Speicherort ab.
   
    ![Erstellen einer Anwendung](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Wenn Sie den Schlüssel verlieren, müssen Sie diesen Abschnitt erneut aufrufen und einen neuen Schlüssel erstellen. Geben Sie den Schlüssel nicht weiter: Jeder, der diesen Schlüssel kennt, kann auf Ihre Daten zugreifen.
   > 
   > 
4. Kopieren Sie im Abschnitt **Eigenschaften** die **Client-ID**, und legen Sie sie an einem sicheren Speicherort ab. 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Authentifizieren bei Microsoft Graph und Abfragen der Identitätsrisikoereignis-API
Sie sollten jetzt über Folgendes verfügen:

* Die oben kopierte Client-ID
* Den oben kopierten Schlüssel
* Den Namen der Domäne Ihres Mandanten

Senden Sie für die Authentifizierung eine POST-Anforderung an `https://login.microsoft.com`. Fügen Sie die folgenden Parameter in den Anforderungstext ein:

* grant_type: „**client_credentials**“
* resource: „**https://graph.microsoft.com**“
* client_id: <your client ID>
* client_secret: <your key>

> [!NOTE]
> Für die Parameter **client_id** und **client_secret** müssen Werte angegeben werden.
> 
> 

Ist die Anforderung erfolgreich, wird ein Authentifizierungstoken zurückgegeben.  
Erstellen Sie zum Aufrufen der API einen Header mit dem folgenden Parameter:

    `Authorization`=”<token_type> <access_token>"


Bei der Authentifizierung können Sie den Tokentyp und das Zugriffstoken anhand des zurückgegebenen Tokens ermitteln.

Senden Sie diesen Header als Anforderung an die folgende API-URL: `https://graph.microsoft.com/beta/identityRiskEvents`

Ist die Anforderung erfolgreich, wird als Antwort eine Sammlung der Identitätsrisikoereignisse und zugehöriger Daten im OData-JSON-Format zurückgegeben. Die Antwort kann nach Bedarf analysiert und verarbeitet werden.

Hier sehen Sie Beispielcode für das Authentifizieren und Aufrufen der API mit PowerShell.  
Fügen Sie einfach Ihre Client-ID, den Schlüssel und die Mandantendomäne hinzu.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Nächste Schritte
Herzlichen Glückwunsch, Sie haben gerade Ihren ersten Aufruf an Microsoft Graph ausgeführt.  
Sie können nun Identitätsrisikoereignisse abfragen und die Daten nach Bedarf verwenden.

Weitere Informationen zu Microsoft Graph und zum Erstellen von Anwendungen mithilfe der Graph-API finden Sie in der [Dokumentation](https://graph.microsoft.io/docs) sowie auf der Website zu [Microsoft Graph](https://graph.microsoft.io/). Erstellen Sie außerdem ein Lesezeichen für die Seite [Azure AD Identity Protection-API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root). Dort werden alle in Graph verfügbaren Identity Protection-APIs aufgeführt. Wenn neue Methoden für die Verwendung von Identity Protection über APIs hinzugefügt werden, werden sie auf dieser Seite angezeigt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)
* [Arten von Risikoereignissen, die von Azure Active Directory Identity Protection erkannt werden](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Übersicht über Microsoft Graph](https://graph.microsoft.io/docs)
* [Azure AD Identity Protection Service Root](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)




<!--HONumber=Nov16_HO3-->


