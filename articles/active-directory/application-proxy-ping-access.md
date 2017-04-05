---
title: "PingAccess für Azure AD-Anwendungsproxys | Microsoft-Dokumentation"
description: "Veröffentlichen von Anwendungen mit PingAccess und App-Proxy zum Unterstützen der headerbasierten Authentifizierung."
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
ms.date: 03/21/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: 173607c481d0ba7ceece6310fcd131ff622a0677
ms.lasthandoff: 03/23/2017

---

# <a name="publish-applications-that-support-header-based-authentication-with-pingaccess-for-azure-ad"></a>Veröffentlichen von Anwendungen, die für Azure AD die headerbasierte Authentifizierung mit PingAccess unterstützen

Der Azure Active Directory-Anwendungsproxy und PingAccess arbeiten nun zusammen, um Azure Active Directory-Kunden einen Zugriff auf noch mehr Anwendungen bereitzustellen. PingAccess erweitert die [vorhandenen Angebote für den Anwendungsproxy](active-directory-application-proxy-get-started.md), um den Remotezugriff auf Anwendungen zu ermöglichen, die Header für die Authentifizierung verwenden. 

## <a name="what-is-pingaccess-for-azure-ad"></a>Was ist PingAccess für Azure AD?

Um Ihren Benutzern den Zugriff auf Apps zu ermöglichen, die Header für die Authentifizierung verwenden, veröffentlichen Sie die App für den Remotezugriff sowohl im Anwendungsproxy als auch in PingAccess. Der Anwendungproxy behandelt diese Apps wie alle anderen und verwendet Azure AD zum Authentifizieren des Zugriffs und zum Leiten des Datenverkehrs durch den Connectordienst. PingAccess ist den Apps vorgelagert und übersetzt das Zugriffstoken aus Azure AD in einen Header, sodass die Anwendung die Authentifizierung in einem Format empfängt, das sie lesen kann. 

Ihre Benutzer bemerken keinen Unterschied, wenn sie sich anmelden, um Ihre Unternehmens-Apps zu nutzen. Sie können weiterhin überall und auf beliebigen Geräten arbeiten. Wenn Ihre Benutzer im Büro sind, fangen weder der Anwendungsproxy noch PingAccess den Datenverkehr ab, sodass sich für Ihre Benutzer nichts ändert.

Da die Anwendungsproxyconnectors Remotedatenverkehr zu allen Apps unabhängig von deren Authentifizierungstyp weiterleiten, sorgen sie auch weiter für einen automatischen Lastenausgleich. 

## <a name="how-do-i-get-access"></a>Wie erhalte ich Zugriff?

Da dieses Szenario im Rahmen einer Partnerschaft von Azure Active Directory und PingAccess angeboten wird, benötigen Sie Lizenzen für beide Dienste. Zu Azure Active Directory Premium-Abonnements gehört eine PingAccess-Startlizenz, sodass Sie bis zu 20 Anwendungen mit diesem Flow konfigurieren können. 

Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## <a name="publish-your-first-application"></a>Veröffentlichen Ihrer ersten Anwendung

Dieser Artikel richtet sich an Personen, die in diesem Szenario erstmals eine App veröffentlichen. Zusätzlich zu den Schritten für die Veröffentlichung durchlaufen Sie die ersten Schritte mit dem Anwendungsproxy und PingAccess. Wenn Sie beide Dienste bereits konfiguriert haben, die Veröffentlichungsschritte jedoch noch einmal auffrischen möchten, können Sie die beiden Abschnitte zur Registrierung überspringen.

>[!NOTE]
>Da dieses Szenario auf einer Partnerschaft von Azure AD und PingAccess beruht, gelten einige der Anweisungen für die Ping Identity-Website. 

### <a name="install-an-application-proxy-connector"></a>Installieren eines Anwendungsproxyconnectors

Wenn Sie den Anwendungsproxy bereits aktiviert und einen Connector installiert haben, können Sie diese Schritte überspringen.

Der Anwendungsproxyconnector ist ein Windows Server-Dienst, der den Datenverkehr von den Remotemitarbeitern zu Ihren veröffentlichten Apps weiterleitet. Ausführlichere Installationsanweisungen finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md).

1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie **Azure Active Directory** > **Anwendungsproxy** aus.
3. Wählen Sie **Connector herunterladen** aus, um den Anwendungsproxyconnector herunterzuladen. Folgen Sie den Installationsanweisungen. 
4. Nach dem Herunterladen des Connectors sollte der Anwendungsproxy automatisch für Ihr Verzeichnis aktiviert sein. Falls nicht, können Sie **Anwendungsproxy aktivieren** auswählen. 

![Aktivieren des Anwendungsproxys und Herunterladen des Connectors](./media/application-proxy-ping-access/install-connector.png)

### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Hinzufügen Ihrer App zu Azure AD mit Anwendungsproxy

Dieser Abschnitt besteht aus zwei Teilen. Zunächst müssen Sie die App in Azure AD veröffentlichen. Anschließend müssen Sie einige Informationen über diese App erfassen, die Sie während der PingAccess-Schritte verwenden können. 

#### <a name="publish-the-app"></a>Veröffentlichen der App

1. Falls nicht im letzten Abschnitt erfolgt, melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator an. 
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus. 
3. Wählen Sie oben auf dem Blatt **Hinzufügen** aus. 
4. Wählen Sie **Lokale Anwendung** aus.
5. Füllen Sie die Pflichtfelder mit Informationen zur neuen App aus. Befolgen Sie diese Anleitung für die folgenden Einstellungen:
  - **Interne URL:** Normalerweise geben Sie die URL an, über die Sie zur Anmeldeseite der App gelangen, wenn Sie sich im Unternehmensnetzwerk befinden. Für diese Partnerschaft muss der Connector den PingAccess-Proxy als Startseite der App verwenden. Verwenden Sie dieses Format: `https://<host name of your PA server>:<port>/<App path name>`. Der Standardport ist 3000, Sie können diesen aber in PingAccess konfigurieren.
  - **Methode für die Vorauthentifizierung**: Azure Active Directory
  - **URL in Headern übersetzen**: Nein
6. Klicken Sie unten auf dem Blatt auf **Hinzufügen**. Ihre Anwendung wird hinzugefügt, das Schnellstartmenü wird geöffnet. 
7. Wählen Sie im Schnellstartmenü **Zuweisen eines Benutzers zu Testzwecken** aus, und fügen Sie der Anwendung mindestens einen Benutzer hinzu. Stellen Sie sicher, dass dieses Testkonto auf die lokale Anwendung zugreifen kann. 
8. Wählen Sie **Zuweisen** aus, um die Zuweisung des Testbenutzers zu speichern. 
9. Wählen Sie auf dem Blatt „App-Verwaltung“ **Einmaliges Anmelden** aus. 
10. Wählen Sie im Dropdownmenü **Headerbasierte Anmeldung** aus. Wählen Sie **Speichern**aus.

  ![Auswählen der headerbasierten Anmeldung](./media/application-proxy-ping-access/sso-header.PNG)

11. Schließen Sie das Blatt „Unternehmensanwendungen“, oder scrollen Sie ganz nach links, um zum Menü „Azure Active Directory“ zurückzukehren. 
12. Wählen Sie **App-Registrierungen** aus.
13. Wählen Sie die App aus, die Sie gerade hinzugefügt haben. Klicken Sie dann auf **Antwort-URLs**. 
14. Prüfen Sie, ob die externe URL, die Sie Ihrer App in Schritt 5 zugewiesen haben, in der Liste „Antwort-URLs“ enthalten ist. Falls nicht, fügen Sie sie jetzt hinzu. 
15. Wählen Sie auf der Blatt „App-Einstellungen“ **Erforderliche Berechtigungen** aus. 
16. Wählen Sie **Hinzufügen**. Wählen Sie für die API **Microsoft Azure Active Directory** aus, und klicken Sie dann auf **Auswählen**. Wählen Sie für die Berechtigungen die Optionen **Lesen und schreiben: Alle Anwendungen** und **Anmelden und Benutzerprofil lesen** aus. Klicken Sie dann auf **Auswählen** und **Fertig**.  

  ![Auswählen von Berechtigungen](./media/application-proxy-ping-access/select-permissions.png) 

#### <a name="collect-information-for-the-pingaccess-steps"></a>Erfassen von Informationen für die Schritte in PingAccess

1. Wählen Sie weiterhin auf dem Blatt „Einstellungen“ die Option **Eigenschaften** aus. Speichern Sie den Wert **Anwendungs-ID**. Dieser wird für die Client-ID verwendet, wenn Sie PingAccess konfigurieren.
2. Wählen Sie auf dem Blatt „App-Einstellungen“ die Option **Schlüssel** aus. 
3. Erstellen Sie einen Schlüssel, indem Sie eine Schlüsselbeschreibung eingeben und im Dropdownmenü ein Ablaufdatum wählen. 
4. Wählen Sie **Speichern** aus. Im Feld **Wert** wird eine GUID angezeigt. 

  Speichern Sie diesen Wert nun, da sie ihn nicht erneut anzeigen können, nachdem dieses Fenster geschlossen wurde. 

5. Schließen Sie das Blatt „App-Registrierungen“, oder scrollen Sie ganz nach links, um zum Menü „Azure Active Directory“ zurückzukehren.
6. Wählen Sie **Eigenschaften** aus.
7. Speichern Sie die GUID **Verzeichnis-ID**. 

### <a name="download-pingaccess-and-configure-your-app"></a>Herunterladen von PingAccess und Konfigurieren der App

Die ausführlichen Anleitungen für den PingAccess-Teil dieses Szenarios werden in der Ping Identity-Dokumentation unter [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html) fortgesetzt.

Diese Anleitungen begleiten Sie durch das Einrichten eines PingAccess-Kontos, sofern noch nicht vorhanden, das Installieren des PingAccess-Servers und das Erstellen einer Azure AD OIDC-Anbieterverbindung mithilfe der Verzeichnis-ID, die Sie aus dem Azure-Portal kopiert haben. Anschließend nutzen Sie die Anwendungs-ID und Schlüsselwerte zum Erstellen einer Websitzung auf PingAccess. Danach können Sie die Identitätszuordnung einrichten und einen virtuellen Host, einen virtuellen Standort und eine virtuelle Anwendung erstellen.

### <a name="test-your-app"></a>Testen Ihrer App

Nachdem Sie alle Schritte abgeschlossen haben, sollte Ihre App betriebsbereit sein. Um sie zu testen, öffnen Sie einen Browser und navigieren zur externen URL, die Sie erstellt haben, als Sie die App in Azure veröffentlicht haben. Melden Sie sich mit dem Testkonto an, das Sie der App zugewiesen haben. 

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)

