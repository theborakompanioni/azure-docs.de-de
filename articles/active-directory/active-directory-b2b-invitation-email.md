---
title: Die Elemente der Einladungs-E-Mail von Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: "Vorlage für die Einladungs-E-Mail von Azure Active Directory B2B-Zusammenarbeit"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/01/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 8fdd2993e6b3f0345948f35753ebb7a20ed174e3
ms.openlocfilehash: 5a31469014b7b0c7ea8a6d6a207646694c6c0b50


---


# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a>Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit

Einladungs-E-Mails sind eine wichtige Komponente zum Onboarding von Partnern als Benutzer von B2B-Zusammenarbeit in Azure AD. Das Hauptziel besteht darin, das Vertrauen des Empfängers zu gewinnen und der E-Mail Rechtmäßigkeit und Glaubwürdigkeit zu verleihen, um sicherzustellen, dass der Empfänger ohne Bedenken die Schaltfläche **Loslegen** wählt und die Einladung annimmt. Dies ist ein wichtiger Aspekt zum Verringern von Konflikten bei der Freigabe. Außerdem soll die E-Mail natürlich auch hervorragend aussehen.

## <a name="explaining-the-email"></a>Erläuterungen zur E-Mail
Sehen wir uns einige Elemente der E-Mail an, damit Sie diese Funktionen optimal nutzen können.

### <a name="subject"></a>Betreff
Der Betreff der E-Mail folgt dem folgenden Muster: Sie werden zur Organisation &lt;Mandantenname&gt; eingeladen

### <a name="from-address"></a>Absenderadresse
Wir verwenden ein LinkedIn-ähnliches Muster für die Absenderadresse. Unser Ziel ist klarzustellen, wer der einladende Benutzer ist und welchem Unternehmen er angehört. Außerdem soll deutlich werden, dass die E-Mail von einer Microsoft-E-Mail-Adresse stammt. Das Format lautet: &lt;Anzeigename des einladenden Benutzers&gt; von &lt;Mandantenname&gt; (über Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Antworten an
Die Antwort-E-Mail-Adresse wird auf die E-Mail-Adresse des einladenden Benutzers festgelegt (falls verfügbar). Wenn eine Antwort auf die E-Mail gesendet wird, gelangt diese zurück an den einladenden Benutzer.

### <a name="branding"></a>Branding
Die Einladungs-E-Mails von Ihrem Mandanten verwenden das Unternehmensbranding, das Sie möglicherweise für Ihren Mandanten eingerichtet haben. Wenn Sie diese Option nutzen möchten, finden Sie hier die Informationen zur Konfiguration. Das Bannerlogo wird in der E-Mail angezeigt. Befolgen Sie die hier aufgeführten Anweisungen zur Bildgröße und -qualität, um ein optimales Ergebnis zu erzielen. Darüber hinaus wird auch der Unternehmensname in der Handlungsaufforderung angezeigt.

### <a name="call-to-action"></a>Call-to-Action (Handlungsaufforderung)
Die Handlungsaufforderung besteht aus zwei Teilen: der Erläuterung, warum der Empfänger die E-Mail erhalten hat, und der Aktion, die vom Empfänger erwartet wird.
- Der Bereich „Warum“ wird über das folgende Muster abgedeckt: Sie wurden eingeladen, auf Anwendungen in der Organisation &lt;Mandantenname&gt; zuzugreifen

- Der Bereich „Erwartete Aktion“ wird durch die Schaltfläche **Loslegen** dargestellt. Wenn der Empfänger hinzugefügt wurde, ohne dass eine Einladung erforderlich ist, wird diese Schaltfläche nicht angezeigt.

### <a name="inviters-information"></a>Informationen zum einladenden Benutzer
Der Anzeigename des einladenden Benutzers ist in der E-Mail enthalten. Wenn Sie ein Profilbild für Ihr Azure AD-Konto eingerichtet haben, ist auch dieses Bild in der Einladungs-E-Mail enthalten. Beides dient dazu, das Vertrauen Ihres Empfängers in die E-Mail zu erhöhen.
Wenn der einladende Benutzer noch kein Profilbild eingerichtet hat, erstellt Azure AD anstelle des Bilds ein Symbol mit den Initialen des einladenden Benutzers, wie hier gezeigt:

  ![Anzeigen der Initialen des einladenden Benutzers](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>body
Hier ist die Nachricht enthalten, die der einladende Benutzer in der Oberfläche eingegeben oder über die Einladungs-API angegeben hat. Dies ist nur ein einfaches Textfeld, das aus Sicherheitsgründen keine HTML-Tags verarbeitet.

### <a name="footer-section"></a>Fußzeilenabschnitt

Die Fußzeile enthält die Microsoft-Unternehmensmarke und informiert den Empfänger, wenn die E-Mail von einem nicht überwachten Alias aus gesendet wurde. Sonderfälle:

- Der einladende Benutzer besitzt keine E-Mail-Adresse im einladenden Mandanten.

  ![Der einladende Benutzer besitzt keine E-Mail-Adresse im einladenden Mandanten – Abbildung](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- Der Empfänger muss die Einladung nicht annehmen.

  ![Der Empfänger muss die Einladung nicht annehmen](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO1-->


