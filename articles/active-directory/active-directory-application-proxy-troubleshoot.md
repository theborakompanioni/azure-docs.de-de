
---
title: Problembehandlung von Anwendungsproxys | Microsoft Docs
description: Behandelt die Problembehandlung von Azure AD-Anwendungsproxys.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 0051bbeaac458e8df07e5c7aa186fbefb526f50e
ms.lasthandoff: 03/15/2017

---


# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Beheben von Problemen mit Anwendungsproxys und Fehlermeldungen
Wenn beim Zugriff auf eine veröffentlichte Anwendung oder beim Veröffentlichen von Anwendungen Fehler auftreten, überprüfen Sie die folgenden Optionen, um zu ermitteln, ob der Microsoft Azure AD-Anwendungsproxy ordnungsgemäß funktioniert:

* Öffnen Sie die Windows Services-Konsole, und vergewissern Sie sich, dass der Dienst **Microsoft AAD-Anwendungsproxy-Connector** aktiviert ist und ausgeführt wird. Sie können gegebenenfalls auch – wie in der folgenden Abbildung gezeigt – die Eigenschaftenseite des Anwendungsproxy-Diensts anzeigen:   
  ![Screenshot: Fenster mit Microsoft AAD-Anwendungsproxy-Connector-Eigenschaften](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Öffnen Sie die Ereignisanzeige, und suchen Sie unter **Anwendungs- und Dienstprotokolle** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Administrator** nach Ereignissen, die sich auf den Anwendungsproxy-Connector beziehen.
* Bei Bedarf sind ausführlichere Protokolle verfügbar, indem Sie die Analyse- und Debugprotokolle aktivieren und das Sitzungsprotokoll des Anwendungsproxy-Connectors aktivieren.

Weitere Informationen zum Azure AD-Problembehandlungstool finden Sie unter [Troubleshooting tool to validate connector networking prerequisites](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites) (Problembehandlungstool zum Überprüfen der Netzwerkvoraussetzungen für den Connector).

## <a name="the-page-is-not-rendered-correctly"></a>Die Seite wird nicht richtig gerendert
Möglicherweise treten Probleme mit dem Rendern der Anwendung oder einer fehlerhaften Funktionsweise auf, ohne dass Sie spezifische Fehlermeldungen erhalten. Dies kann der Fall sein, wenn Sie den Artikelpfad veröffentlicht haben, für die Anwendung aber Inhalt erforderlich ist, der außerhalb des Pfads vorliegt.

Wenn Sie beispielsweise den Pfad https://yourapp/app veröffentlichen, die Anwendung aber Bilder in https://yourapp/media aufruft, werden sie nicht gerendert. Stellen Sie sicher, dass Sie die Anwendung mit dem Pfad der höchsten benötigten Ebene veröffentlichen, damit der gesamte relevante Inhalt einbezogen wird. In diesem Beispiel wäre das http://yourapp/.

Wenn Sie den Pfad so ändern, dass er referenzierten Inhalt enthält, Benutzer aber an einen Link tiefer im Pfad verwiesen werden sollen, helfen Ihnen die Informationen im folgenden Blogbeitrag weiter: [Setting the right link for Application Proxy applications in the Azure AD access panel and Office 365 app launcher](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/)(Festlegen des richtigen Links für Anwendungsproxy-Anwendungen im Azure AD-Zugriffsbereich und Office 365-Startprogramm).

## <a name="connector-errors"></a>Connectorfehler

Verwenden Sie den [Azure AD Application Proxy Connector Ports Test Tool (Testtool der Anwendungsproxy-Connectortools von Azure AD)](https://aadap-portcheck.connectorporttest.msappproxy.net/), um sicherzustellen, dass Ihr Connector den Anwendungsproxydienst erreichen kann. Stellen Sie zumindest sicher, dass die Region USA (Mitte) und die Ihnen am nächsten gelegene Region alle über grüne Häkchen verfügen. Darüber hinaus bedeuten mehr grüne Häkchen größere Resilienz. 

Wenn während der Connector-Installation durch den Assistenten ein Fehler bei der Registrierung auftritt, haben Sie zwei Möglichkeiten, um den Grund dafür anzuzeigen. Sehen Sie entweder im Ereignisprotokoll unter **Anwendungs- und Dienstprotokolle\Microsoft\AadApplicationProxy\Connector\Admin** nach, oder führen Sie den folgenden Windows PowerShell-Befehl aus.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

Sobald Sie den Connectorfehler im Ereignisprotokoll gefunden haben, beheben Sie das Problem anhand der Liste gängiger Fehler:

- **Fehler bei der Connector-Registrierung: Stellen Sie sicher, dass Sie den Anwendungsproxy im Azure-Verwaltungsportal aktiviert und Ihren Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: „Es ist mindestens ein Fehler aufgetreten.“**

  Wenn Sie das Registrierungsfenster geschlossen haben, ohne sich bei Azure AD anzumelden, führen Sie den Connector-Assistenten erneut aus, und registrieren Sie den Connector.

  Wenn das Registrierungsfenster geöffnet und dann sofort geschlossen wird, ohne dass Sie sich anmelden können, erhalten Sie ggf. diese Fehlermeldung. Dieser Fehler tritt auf, wenn in Ihrem System ein Netzwerkfehler vorliegt. Stellen Sie sicher, dass es möglich ist, mit einem Browser eine Verbindung mit einer öffentlichen Website herzustellen, und dass die Ports wie unter [Voraussetzungen für den Anwendungsproxy](active-directory-application-proxy-enable.md)angegeben geöffnet sind.

- **Fehler bei der Connectorregistrierung: Stellen Sie sicher, dass Ihr Computer mit dem Internet verbunden ist. Fehler: „An https://connector.msappproxy.net:9090/register/RegisterConnector lauschte kein Endpunkt, oder die Nachricht konnte nicht angenommen werden. Ursache dieses Fehlers ist häufig eine falsche Adresse oder SOAP-Aktion. Weitere Informationen können Sie, sofern vorhanden, der InnerException entnehmen.“**

  Wenn Sie sich mithilfe Ihres Azure AD-Benutzernamens und -Kennworts anmelden und dann diese Fehlermeldung erhalten, sind ggf. alle Ports über 8081 blockiert. Stellen Sie sicher, dass die erforderlichen Ports geöffnet sind. Weitere Informationen finden Sie unter [Voraussetzungen für den Anwendungsproxy](active-directory-application-proxy-enable.md).

- **Klartextfehler wird im Registrierungsfenster angezeigt. Der Vorgang kann nicht fortgesetzt werden.**

  Wenn diese Fehlermeldung angezeigt und das Fenster dann geschlossen wird, haben Sie den Benutzernamen oder das Kennwort falsch eingegeben. Versuchen Sie es erneut. 

- **Fehler bei der Connector-Registrierung: Stellen Sie sicher, dass Sie den Anwendungsproxy im Azure-Verwaltungsportal aktiviert und Ihren Active Directory-Benutzernamen und das Kennwort richtig eingegeben haben. Fehler: „AADSTS50059: In der Anforderung oder in den bereitgestellten Anmeldeinformationen wurden keine Informationen gefunden, die den Mandanten identifizieren, und bei der Suche nach dem Dienstprinzipal-URI ist ein Fehler aufgetreten.** 

  Sie versuchen, sich mithilfe eines Microsoft-Kontos und nicht mithilfe einer Domäne anzumelden, die Bestandteil der Organisations-ID des Verzeichnisses ist, auf das Sie zugreifen möchten. Stellen Sie sicher, dass der Administrator Teil des gleichen Domänennamens wie die Mandantendomäne ist. Wenn die Azure AD-Domäne z. B. contoso.com ist, sollte der Administrator admin@contoso.com lauten. 

- **Fehler beim Abrufen der aktuellen Ausführungsrichtlinie für die Ausführung von PowerShell-Skripts.** 

  Falls die Installation des Connectors nicht erfolgreich verläuft, stellen Sie sicher, dass die PowerShell-Ausführungsrichtlinie nicht deaktiviert ist. 

  1. Öffnen Sie den Gruppenrichtlinien-Editor. 
  2. Wechseln Sie zu **Computerkonfiguration** > **Administrative Vorlagen** > **Windows-Komponenten** > **Windows PowerShell**, und doppelklicken Sie auf **Skriptausführung aktivieren**. 
  3. Die Ausführungsrichtlinie kann entweder auf **Nicht konfiguriert** oder auf **Aktiviert** festgelegt sein. Stellen Sie bei der Einstellung **Aktiviert** sicher, dass die Ausführungsrichtlinie unter „Optionen“ auf **Lokale Skripts und remote signierte Skripts zulassen** oder **Alle Skripts zulassen** festgelegt ist. 

- **Der Connector konnte die Konfiguration nicht herunterladen.** 

  Das Clientzertifikat des Connectors, das für die Authentifizierung verwendet wird, ist abgelaufen. Dies kann auch auftreten, wenn Sie den Connector hinter einem Proxy installiert haben. In diesem Fall kann der Connector nicht auf das Internet zugreifen und ist nicht in der Lage, Anwendungen für Remotebenutzer bereitzustellen. Erneuern Sie die Vertrauensstellung manuell mithilfe des Cmdlets `Register-AppProxyConnector` in Windows PowerShell. Wenn sich der Connector hinter einem Proxy befindet, ist es notwendig, den Connector-Konten „Netzwerkdienste“ und „Lokales System“ Zugriff auf das Internet zu gewähren. Hierzu können sie entweder Zugriff auf den Proxy erhalten, oder sie werden zur Umgehung des Proxys eingestellt. 

- **Fehler bei der Connectorregistrierung: Stellen Sie sicher, dass Sie ein globaler Administrator Ihres Active Directory-Verzeichnisses sind, um den Connector zu registrieren. Fehler: „Die Registrierungsanforderung wurde verweigert.“** 

  Der Alias, mit dem Sie versuchen, sich anzumelden, ist kein Administrator für diese Domäne. Ihr Connector wird immer für das Verzeichnis installiert, das Besitzer der Domäne des Benutzers ist. Stellen Sie sicher, dass das Administratorkonto, mit dem Sie sich anmelden möchten, über globale Berechtigungen für den Azure AD-Mandanten verfügt. 

## <a name="kerberos-errors"></a>Kerberos-Fehler

Diese Liste umfasst die häufiger auftretenden Fehler bei der Kerberos-Einrichtung und -Konfiguration und enthält Vorschläge zu deren Auflösung.

- **Fehler beim Abrufen der aktuellen Ausführungsrichtlinie für die Ausführung von PowerShell-Skripts.** 

  Falls die Installation des Connectors nicht erfolgreich verläuft, stellen Sie sicher, dass die PowerShell-Ausführungsrichtlinie nicht deaktiviert ist. 

  1. Öffnen Sie den Gruppenrichtlinien-Editor. 
  2. Wechseln Sie zu **Computerkonfiguration** > **Administrative Vorlagen** > **Windows-Komponenten** > **Windows PowerShell**, und doppelklicken Sie auf **Skriptausführung aktivieren**. 
  3. Die Ausführungsrichtlinie kann entweder auf **Nicht konfiguriert** oder auf **Aktiviert** festgelegt sein. Stellen Sie bei der Einstellung **Aktiviert** sicher, dass die Ausführungsrichtlinie unter „Optionen“ auf **Lokale Skripts und remote signierte Skripts zulassen** oder **Alle Skripts zulassen** festgelegt ist. 

- **12008 – Azure AD hat die maximale Anzahl zulässiger Kerberos-Authentifizierungsversuche beim Back-End-Server überschritten.** 

  Dieser Fehler deutet möglicherweise auf eine falsche Konfiguration zwischen Azure AD und dem Back-End-Anwendungsserver oder ein Problem bei der Konfiguration von Datum und Uhrzeit auf beiden Computern hin. Der Back-End-Server hat das von Azure AD erstellte Kerberos-Ticket abgelehnt. Stellen Sie sicher, dass Azure AD und der Back-End-Anwendungsserver korrekt konfiguriert sind. Stellen Sie sicher, dass die Konfiguration von Datum und Uhrzeit in Azure AD und auf dem Back-End-Anwendungsserver synchronisiert ist. 

- **13016 – Azure AD kann kein Kerberos-Ticket für den Benutzer abrufen, da kein UPN im Edgetoken oder im Zugriffscookie vorliegt.** 

  Es gibt ein Problem mit der STS-Konfiguration. Korrigieren Sie die UPN-Anspruchskonfiguration im STS. 

- **13019 – Azure AD kann aufgrund des folgenden allgemeinen API-Fehlers kein Kerberos-Ticket für den Benutzer abrufen.** 

  Dieses Ereignis deutet möglicherweise auf eine falsche Konfiguration zwischen Azure AD und dem Domänencontrollerserver oder ein Problem bei der Konfiguration von Datum und Uhrzeit auf beiden Computern hin. Der Domänencontroller hat das von Azure AD erstellte Kerberos-Ticket abgelehnt. Stellen Sie sicher, dass die Konfiguration von Azure AD und des Back-End-Anwendungsservers korrekt ist, insbesondere die SPN-Konfiguration. Sorgen Sie dafür, dass Azure AD in dieselbe Domäne wie der Domänencontroller eingebunden ist, um sicherzustellen, dass der Domänencontroller eine Vertrauensstellung mit Azure AD herstellt. Stellen Sie sicher, dass die Konfiguration von Datum und Uhrzeit in Azure AD und auf dem Domänencontroller synchronisiert ist. 

- **13020 – Azure AD kann kein Kerberos-Ticket für den Benutzer abrufen, da der Back-End-Server-SPN nicht definiert ist.** 

  Dieses Ereignis deutet möglicherweise auf eine falsche Konfiguration zwischen Azure AD und dem Domänencontrollerserver oder ein Problem bei der Konfiguration von Datum und Uhrzeit auf beiden Computern hin. Der Domänencontroller hat das von Azure AD erstellte Kerberos-Ticket abgelehnt. Stellen Sie sicher, dass die Konfiguration von Azure AD und des Back-End-Anwendungsservers korrekt ist, insbesondere die SPN-Konfiguration. Sorgen Sie dafür, dass Azure AD in dieselbe Domäne wie der Domänencontroller eingebunden ist, um sicherzustellen, dass der Domänencontroller eine Vertrauensstellung mit Azure AD herstellt. Stellen Sie sicher, dass die Konfiguration von Datum und Uhrzeit in Azure AD und auf dem Domänencontroller synchronisiert ist. 

- **13022 – Azure AD kann den Benutzer nicht authentifizieren, da der Back-End-Server auf Kerberos-Authentifizierungsversuche mit einem HTTP 401-Fehler reagiert.** 

  Dieses Ereignis deutet möglicherweise auf eine falsche Konfiguration zwischen Azure AD und dem Back-End-Anwendungsserver oder ein Problem bei der Konfiguration von Datum und Uhrzeit auf beiden Computern hin. Der Back-End-Server hat das von Azure AD erstellte Kerberos-Ticket abgelehnt. Stellen Sie sicher, dass Azure AD und der Back-End-Anwendungsserver korrekt konfiguriert sind. Stellen Sie sicher, dass die Konfiguration von Datum und Uhrzeit in Azure AD und auf dem Back-End-Anwendungsserver synchronisiert ist. 

## <a name="end-user-errors"></a>Endbenutzerfehler

Diese Liste enthält Fehler, die bei Ihren Endbenutzern möglicherweise auftreten, wenn sie nicht erfolgreich auf die App zugreifen können. 

- **Die Website kann die Seite nicht anzeigen.** 

  Der Benutzer erhält unter Umständen beim Versuch, auf die von Ihnen veröffentlichte App zuzugreifen, diese Fehlermeldung, wenn die Anwendung eine IWA-Anwendung ist. Der definierte SPN für diese Anwendung ist möglicherweise falsch. Stellen Sie für IWA-Apps sicher, dass für diese Anwendung der richtige SPN konfiguriert ist. 

- **Die Website kann die Seite nicht anzeigen.** 

  Der Benutzer erhält unter Umständen beim Versuch, auf die von Ihnen veröffentlichte App zuzugreifen, diese Fehlermeldung, wenn die Anwendung eine OWA-Anwendung ist. Mögliche Gründe hierfür sind:  
  - Der definierte SPN für diese Anwendung ist falsch. Stellen Sie sicher, dass für diese Anwendung der richtige SPN konfiguriert ist.
  - Der Benutzer, der auf die Anwendung zugreifen möchte, verwendet für die Anmeldung ein Microsoft-Konto anstelle des richtigen Unternehmenskontos, oder der Benutzer ist ein Gastbenutzer. Vergewissern Sie sich, dass Benutzer sich mithilfe ihres Unternehmenskontos anmelden, das der Domäne der veröffentlichten Anwendung entspricht. Microsoft-Konto- und Gastbenutzer können nicht auf IWA-Anwendungen zugreifen.
  - Der Benutzer, der auf die Anwendung zugreifen möchte, ist für diese Anwendung auf der lokalen Seite nicht ordnungsgemäß definiert. Stellen Sie sicher, dass dieser Benutzer über die entsprechenden Berechtigungen verfügt, wie für diese Back-End-Anwendung auf dem lokalen Computer definiert.

- **Auf diese Unternehmens-App kann nicht zugegriffen werden. Sie haben keine Befugnis, auf diese Anwendung zuzugreifen. Fehler bei der Autorisierung. Stellen Sie sicher, dass Sie dem Benutzer Zugriff auf diese Anwendung zuweisen.** 

  Unter Umständen erhalten Benutzer diesen Fehler, wenn sie versuchen, auf die von Ihnen veröffentlichte App zuzugreifen, wenn sie für die Anmeldung Microsoft-Konten anstelle ihrer Unternehmenskonten verwenden. Gastbenutzer erhalten diese Fehlermeldung möglicherweise auch. Microsoft-Kontobenutzer und Gäste können nicht auf IWA-Anwendungen zugreifen. Vergewissern Sie sich, dass Benutzer sich mithilfe ihres Unternehmenskontos anmelden, das der Domäne der veröffentlichten Anwendung entspricht. 

  Möglicherweise haben Sie den Benutzer nicht der Anwendung zugewiesen. Wechseln Sie zur Registerkarte **Anwendung**, und weisen Sie unter **Benutzer und Gruppen** diesen Benutzer oder die Benutzergruppe dieser Anwendung zu.

- **Auf diese Unternehmens-App kann momentan nicht zugegriffen werden. Versuchen Sie es später erneut. Der Connector hat das Zeitlimit überschritten.** 

  Die Benutzer erhalten beim Versuch, auf die von Ihnen veröffentlichte App zuzugreifen, unter Umständen diese Fehlermeldung, wenn sie für diese Anwendung auf der lokalen Seite nicht ordnungsgemäß definiert sind. Stellen Sie sicher, dass die Benutzer über die entsprechenden Berechtigungen verfügen, wie für diese Back-End-Anwendung auf dem lokalen Computer definiert. 

- **Auf diese Unternehmens-App kann nicht zugegriffen werden. Sie haben keine Befugnis, auf diese Anwendung zuzugreifen. Fehler bei der Autorisierung. Stellen Sie sicher, dass der Benutzer eine Lizenz für Azure Active Directory Premium oder Basic besitzt.**

  Möglicherweise erhalten Benutzer diese Fehlermeldung beim Zugriff auf die von Ihnen veröffentlichte App, wenn ihnen nicht explizit eine Premium-/Basic-Lizenz vom Administrator des Abonnenten zugewiesen wurde. Wechseln Sie zur Active Directory-Registerkarte **Lizenzen** des Abonnenten, und stellen Sie sicher, dass diesem Benutzer oder dieser Benutzergruppe eine Premium- oder Basic-Lizenz zugewiesen ist.

## <a name="my-error-wasnt-listed-here"></a>Mein Fehler wurde hier nicht aufgelistet

Wenn ein Fehler oder ein Problem mit dem Azure AD-Anwendungsproxy auftritt, der in diesem Handbuch zur Problembehandlung nicht aufgeführt ist, möchten wir gerne davon erfahren. Senden Sie eine E-Mail mit Details zum aufgetretenen Fehler an unser [Feedbackteam](mailto:aadapfeedback@microsoft.com).

## <a name="see-also"></a>Weitere Informationen
* [Aktivieren des Azure AD-Anwendungsproxys](active-directory-application-proxy-enable.md)
* [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md)
* [Einmaliges Anmelden aktivieren](active-directory-application-proxy-sso-using-kcd.md)
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png

