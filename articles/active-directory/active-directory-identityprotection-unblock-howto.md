<properties
	pageTitle="Azure Active Directory Identity Protection – Aufheben der Blockierung von Benutzern | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie die Blockierung von Benutzern aufheben, die durch eine Azure Active Directory Identity Protection-Richtlinie blockiert wurden."
	services="active-directory"
	keywords="Azure Active Directory Identity Protection, Benutzerblockierung aufheben"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="markvi"/>

#Azure Active Directory Identity Protection – Aufheben der Blockierung von Benutzern

Mit Azure Active Directory Identity Protection können Sie Richtlinien konfigurieren, um Benutzer zu blockieren, wenn die konfigurierten Bedingungen erfüllt sind. Ein blockierter Benutzer wendet sich zur Aufhebung der Blockierung in der Regel an den Helpdesk. In diesem Thema erfahren Sie Schritt für Schritt, wie Sie die Blockierung eines Benutzers wieder aufheben können.


## Ermitteln der Ursache für die Blockierung

Wenn Sie die Blockierung eines Benutzers aufheben möchten, müssen Sie zunächst ermitteln, durch welche Art von Richtlinie der Benutzer blockiert wurde, da hiervon die weitere Vorgehensweise abhängt. Bei Azure Active Directory Identity Protection kann die Blockierung des Benutzers entweder auf eine Anmelderisikorichtlinie oder auf eine Benutzerrisikorichtlinie zurückzuführen sein.

Die Art der Richtlinie, durch die ein Benutzer blockiert wurde, können Sie der Überschrift des Dialogfelds entnehmen, das dem Benutzer bei der Anmeldung angezeigt wurde:

|Richtlinie | Benutzerdialogfeld|
|--- | --- |
|Anmelderisiko | ![Blockierte Anmeldung](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Benutzerrisiko | ![Blockiertes Konto](./media/active-directory-identityprotection-unblock-howto/104.png) |


Dabei gilt Folgendes:

- Ein Benutzer, der durch eine Anmelderisikorichtlinie blockiert wird, wird auch als verdächtige Anmeldung bezeichnet.
- Ein Benutzer, der durch eine Benutzerrisikorichtlinie blockiert wird, wird auch als gefährdetes Konto bezeichnet.

 
## Aufheben der Blockierung verdächtiger Anmeldungen

Zur Aufheben der Blockierung verdächtiger Anmeldungen stehen folgende Optionen zur Verfügung:

1. **Anmelden über einen bekannten Ort oder ein bekanntes Gerät:** Die Blockierung einer verdächtigen Anmeldung ist häufig auf Anmeldeversuche über einen unbekannten Ort oder ein unbekanntes Gerät zurückzuführen. Um zu ermitteln, ob dies die Ursache für die Blockierung ist, muss sich der betroffene Benutzer lediglich über einen bekannten Ort oder über ein bekanntes Gerät anmelden.


3. **Ausschließen von der Richtlinie:** Wenn Sie vermuten, dass die aktuelle Konfiguration Ihrer Anmelderichtlinie Probleme für bestimmte Benutzer verursacht, können Sie diese Benutzer von der Richtlinie ausschließen. Ausführlichere Informationen finden Sie unter [Anmelderisikorichtlinie](active-directory-identityprotection.md#sign-in-risk-policy).
 
4. **Deaktivieren der Richtlinie:** Wenn Sie vermuten, dass Ihre Richtlinienkonfiguration Probleme für alle Benutzer verursacht, können Sie die Richtlinie deaktivieren. Ausführlichere Informationen finden Sie unter [Anmelderisikorichtlinie](active-directory-identityprotection.md#sign-in-risk-policy).


## Aufheben der Blockierung gefährdeter Konten

Zur Aufheben der Blockierung gefährdeter Konten stehen folgende Optionen zur Verfügung:

1. **Zurücksetzen des Kennworts:** Sie können das Kennwort des Benutzers zurücksetzen. Ausführlichere Informationen finden Sie unter [Manuelle Kennwortzurücksetzung](active-directory-identityprotection.md#manual-secure-password-reset).

2. **Schließen aller Risikoereignisse:** Die Benutzerrisikorichtlinie blockiert einen Benutzer, wenn die konfigurierte Benutzerrisikostufe für die Blockierung des Zugriffs erreicht wurde. Sie können die Risikostufe eines Benutzers verringern, indem Sie gemeldete Risikoereignisse manuell schließen. Ausführlichere Informationen finden Sie unter [Manuelles Schließen von Risikoereignissen](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Ausschließen von der Richtlinie:** Wenn Sie vermuten, dass die aktuelle Konfiguration Ihrer Anmelderichtlinie Probleme für bestimmte Benutzer verursacht, können Sie diese Benutzer von der Richtlinie ausschließen. Ausführlichere Informationen finden Sie unter [Benutzerrisikorichtlinie](active-directory-identityprotection.md#user-risk-policy).
 
4. **Deaktivieren der Richtlinie:** Wenn Sie vermuten, dass Ihre Richtlinienkonfiguration Probleme für alle Benutzer verursacht, können Sie die Richtlinie deaktivieren. Ausführlichere Informationen finden Sie unter [Benutzerrisikorichtlinie](active-directory-identityprotection.md#user-risk-policy).




## Nächste Schritte

 Sie möchten mehr über Azure AD Identity Protection erfahren? Weitere Informationen finden Sie unter [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
 

<!---HONumber=AcomDC_0921_2016-->