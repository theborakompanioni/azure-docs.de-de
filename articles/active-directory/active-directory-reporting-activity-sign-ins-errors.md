---
title: "Fehlercodes des Berichts mit den Anmeldeaktivitäten im Azure Active Directory-Portal | Microsoft-Dokumentation"
description: "Enthält eine Referenz zu den Fehlercodes des Berichts mit den Anmeldeaktivitäten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 2a1b7b87df2cd8fa2e98f217480b46f5f6334297
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Fehlercodes des Berichts mit den Anmeldeaktivitäten im Azure Active Directory-Portal

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wer hat sich mit Azure Active Directory angemeldet?
- Für welche Apps wurde die Anmeldung durchgeführt?
- Für welche Anmeldungen sind Fehler aufgetreten, und was waren die Gründe dafür?

In diesem Thema sind die Fehlercodes und die dazugehörigen Beschreibungen aufgeführt. 

## <a name="how-can-i-display-failed-sign-ins"></a>Wie kann ich Anmeldungen anzeigen, für die Fehler aufgetreten sind? 

Ihr erster Einstiegspunkt für alle Daten zu Anmeldeaktivitäten ist **[Anmeldungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** im Abschnitt **Aktivität** von **Azure Active**.


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Anmeldeaktivität")


Im Bericht zu den Anmeldungen können Sie alle fehlgeschlagenen Anmeldungen anzeigen, indem Sie unter **Fehler** die Option **Anmeldestatus** wählen.


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Anmeldeaktivität")

Wenn Sie in der angezeigten Liste auf einen Eintrag klicken, wird das Blatt **Aktivitätsdetails: Anmeldungen** geöffnet. Diese Ansicht enthält alle Details, die von Azure Active Directory zu Anmeldungen nachverfolgt werden, z.B. der **Code des Anmeldefehlers** und eine **Fehlerursache**.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Anmeldeaktivität")


Als Alternative zur Verwendung des Azure-Portals für den Zugriff auf Daten zu Anmeldungen können Sie auch die [Berichterstellungs-API](active-directory-reporting-api-getting-started-azure-portal.md) verwenden.


Der folgende Abschnitt enthält eine vollständige Übersicht über alle möglichen Fehler und die dazugehörigen Beschreibungen. 

## <a name="error-codes"></a>Fehlercodes

| Fehler| Beschreibung |
| --- | --- |
| 50001| Der Dienstprinzipal mit dem Namen X wurde im Mandanten mit dem Namen Y nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde. Es kann auch sein, dass der Ressourcenprinzipal nicht im Verzeichnis gefunden wurde oder ungültig ist.|
| 50008| Die SAML-Assertion fehlt oder wurde im Token falsch konfiguriert.|
| 50011| Die Antwortadresse fehlt, ist falsch konfiguriert oder stimmt nicht mit den für die Anwendung konfigurierten Antwortadressen überein.|
| 50053| Das Konto ist gesperrt, weil der Benutzer zu häufig versucht hat, sich mit einer falschen Benutzer-ID bzw. einem falschen Kennwort anzumelden.|
| 50054| Für die Authentifizierung wurde ein altes Kennwort verwendet.|
| 50055| Ungültiges Kennwort. Es wurde ein abgelaufenes Kennwort eingegeben.|
| 50057| Das Benutzerkonto ist deaktiviert.|
| 50058| In den angegebenen Anmeldeinformationen wurden keine Informationen zur Benutzeridentität gefunden, der Benutzer wurde unter dem Mandanten nicht gefunden, im Hintergrund wurde eine Anforderung gesendet, aber es hat sich kein Benutzer angemeldet, oder der Benutzer konnte vom Dienst nicht authentifiziert werden.|
| 50074| Strenge Authentifizierung (zweite Stufe) ist erforderlich.|
| 50079| Der Benutzer muss sich für die zweistufige Authentifizierung registrieren.|
| 50126| Ungültiger Benutzername bzw. ungültiges Kennwort oder ungültiger lokaler Benutzername bzw. Kennwort.|
| 50131| Wird für verschiedene Fehler in Bezug auf den bedingten Zugriff verwendet. Beispiele: Fehlerhafter Windows-Gerätestatus, Blockierung der Anforderung aufgrund von verdächtigen Aktivitäten und Entscheidungen gemäß Zugriffs- und Sicherheitsrichtlinien.|
| 50133| Die Sitzung ist ungültig, weil das Kennwort abgelaufen ist oder vor Kurzem geändert wurde.|
| 50144| Das Active Directory-Kennwort des Benutzers ist abgelaufen.|
| 65001| Anwendung X ist nicht berechtigt, auf Anwendung Y zuzugreifen, oder die Berechtigung wurde widerrufen. Oder: Der Benutzer oder Administrator hat der Verwendung der Anwendung mit ID X nicht zugestimmt. Senden Sie eine interaktive Autorisierungsanforderung für diesen Benutzer und diese Ressource. Oder: Der Benutzer oder Administrator hat nicht zugestimmt, die Anwendung mit ID X zu verwenden. Senden Sie eine Autorisierungsanforderung an Ihren Mandantenadministrator, damit dieser im Namen der App Y für Ressource Z fungiert.|
| 65005| Die für die Anwendung erforderliche Liste für den Ressourcenzugriff enthält keine Anwendungen, die von der Ressource ermittelt werden können, die Clientanwendung hat den Zugriff auf eine Ressource angefordert, die nicht in der erforderlichen Liste für den Ressourcenzugriff angegeben ist, oder der Graph-Dienst hat „Fehlerhafte Anforderung“ oder „Ressource nicht gefunden“ zurückgegeben.|
| 70001| Die Anwendung mit dem Namen X wurde im Mandanten mit dem Namen Y nicht gefunden. Dies kann auftreten, wenn die Anwendung nicht vom Administrator des Mandanten installiert wurde oder wenn sie von den Benutzern des Mandanten keine Zustimmung erhalten hat. Unter Umständen haben Sie Ihre Authentifizierung an den falschen Mandanten gesendet.|
| 80001| Kein Authentifizierungs-Agent verfügbar.|
| 80002| Zeitüberschreitung für die Anforderung zur Kennwortüberprüfung des Authentifizierungs-Agents.|
| 80003| Der Authentifizierungs-Agent hat eine ungültige Antwort erhalten.|
| 80004| In der Anmeldeanforderung wurde ein falscher Benutzerprinzipalname (UPN) verwendet.|
| 80005| Authentifizierungs-Agent: Fehler aufgetreten.|
| 80007| Der Authentifizierungs-Agent kann keine Verbindung mit Active Directory herstellen.|
| 80010| Der Authentifizierungs-Agent kann das Kennwort nicht entschlüsseln.|
| 81001| Das Kerberos-Ticket des Benutzers ist zu groß.|
| 81002| Das Kerberos-Ticket des Benutzers kann nicht überprüft werden.|
| 81003| Das Kerberos-Ticket des Benutzers kann nicht überprüft werden.|
| 81004| Fehler beim Versuch, die Kerberos-Authentifizierung durchzuführen.|
| 81008| Das Kerberos-Ticket des Benutzers kann nicht überprüft werden.|
| 81009| Das Kerberos-Ticket des Benutzers kann nicht überprüft werden.|
| 81010| Fehler beim nahtlosen einmaligen Anmelden, da das Kerberos-Ticket des Benutzers abgelaufen oder ungültig ist.|
| 81011| Das Benutzerobjekt konnte anhand der Informationen im Kerberos-Ticket des Benutzers nicht gefunden werden.|
| 81012| Der Benutzer, der versucht, sich an Azure AD anzumelden, unterscheidet sich von dem Benutzer, der am Gerät angemeldet ist.|
| 81013| Das Benutzerobjekt konnte anhand der Informationen im Kerberos-Ticket des Benutzers nicht gefunden werden.|
| 90014| Wird in unterschiedlichen Fällen verwendet, wenn ein erwartetes Feld in den Anmeldeinformationen nicht vorhanden ist.|
| 90093| Es wurde ein Graph mit einem unzulässigen Fehlercode für die Anforderung zurückgegeben.|



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](active-directory-reporting-activity-sign-ins.md).


