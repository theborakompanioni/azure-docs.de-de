---
title: "Microsoft Authenticator-Anmeldung per Telefon – Azure- und Microsoft-Konten | Microsoft-Dokumentation"
description: "Melden Sie sich bei Ihrem Microsoft-Konto über Ihr Smartphone an, anstatt Ihr Kennwort einzugeben. In diesem Artikel werden häufig gestellte Fragen zu diesem Feature beantwortet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: librown
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: ec52f719f33db0d893d707b59293fa75a8ff4314
ms.openlocfilehash: 033392783fd98cbdb23fd0254c93687754808286
ms.lasthandoff: 02/10/2017

---
# <a name="sign-in-with-your-phone-not-your-password"></a>Anmelden per Smartphone anstelle Ihres Kennworts

Mit der Microsoft Authenticator-App können Sie Ihre Konten noch besser schützen, indem Sie nach der Kennworteingabe eine Überprüfung in zwei Schritten durchführen. Aber wussten Sie, dass die App das Kennwort für Ihr persönliches Microsoft-Konto vollständig ersetzen kann? 

Dieses Feature steht auf IOS- und Android-Geräten zur Verfügung und kann für persönliche Microsoft-Konten verwendet werden. 

## <a name="how-it-works"></a>So funktioniert's

Viele Benutzer verwenden die Microsoft Authenticator-App für die Überprüfung in zwei Schritten, wenn sie sich bei ihrem Microsoft-Konto anmelden. Hierzu geben sie ihr Kennwort ein und wechseln anschließend in die App, um entweder eine Benachrichtigung zu bestätigen oder einen Überprüfungscode abzurufen. Bei der Anmeldung per Telefon wird die Kennworteingabe übersprungen, und die gesamte Identitätsüberprüfung findet auf Ihrem Smartphone statt. Das Verfahren funktioniert immer noch wie die Überprüfung in zwei Schritten: Es wird etwas abgefragt, das Sie wissen, und etwas, das Sie haben. Das Smartphone ist auch hier das, was Sie haben. Als etwas, das Sie wissen, wird nun allerdings die Eingabe der Smartphone-PIN oder des biometrischen Schlüssels angefordert. 

## <a name="how-to-get-started"></a>Erste Schritte

Gehen Sie wie folgt vor, um sich mit Ihrem Smartphone bei Ihrem persönlichen Microsoft-Konto anzumelden: 

1. Aktivieren Sie für Ihr Konto die Anmeldung per Telefon. 

  - Falls Sie noch nicht über die Microsoft Authenticator-App verfügen, installieren Sie sie, und fügen Sie Ihr persönliches Microsoft-Konto hinzu, wie auf der [Microsoft Authenticator-Seite](microsoft-authenticator-app-how-to.md) beschrieben. Neu hinzugefügte Konten werden automatisch aktiviert, sodass es gleich losgehen kann.

  - Wenn Sie Microsoft Authenticator bereits für die Überprüfung in zwei Schritten verwenden, wählen Sie auf der Startseite der App Ihr Konto und anschließend im Dropdownmenü die Option „Anmeldung per Telefon aktivieren“ aus.

  >[!NOTE] 
  >Zum Schutz Ihres Kontos muss Ihr Gerät mit einer PIN oder mit einem biometrischen Schlüssel gesperrt sein. Falls Sie Ihr Smartphone ohne Sperre verwenden, werden Sie vor der Aktivierung der Anmeldung per Telefon in einem Popup zur Einrichtung einer Sperre aufgefordert. 

3. Auf den meisten Seiten, auf denen Sie für gewöhnlich das Kennwort für Ihr Microsoft-Konto eingeben, steht ein Link vom Typ **Stattdessen eine App verwenden** zur Verfügung. Verwenden Sie diesen Link, um sich mithilfe Ihres Smartphones anzumelden. 

4. Microsoft sendet daraufhin eine Benachrichtigung an Ihr Smartphone. Bestätigen Sie die Benachrichtigung, um sich bei Ihrem Konto anzumelden.   

## <a name="faq"></a>Häufig gestellte Fragen 

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Inwiefern ist die Anmeldung per Telefon sicherer als die Eingabe eines Kennworts?  

Die meisten Benutzer melden sich heutzutage mit einem Benutzernamen und einem Kennwort bei einer Website oder App an.  Leider gehen Kennwörter nicht selten verloren, werden gestohlen oder von Hackern erraten. Wenn Sie die Microsoft Authenticator-App für die Anmeldung einrichten, generieren wir auf Ihrem Smartphone einen Schlüssel, mit dem Sie Ihr Konto entsperren können. Dieser Schlüssel wird durch die PIN oder den biometrischen Schlüssel geschützt, die bzw. den Sie bereits auf Ihrem Smartphone verwenden.  Wenn Sie sich mit Ihrem Smartphone anmelden, wird Ihre Identität unter Verwendung dieses Schlüssels sicher mit zwei Faktoren bestätigt: dem Smartphone selbst und Ihrer Fähigkeit, das Gerät zu entsperren. 

Der verwendete Schlüssel ähnelt den Schlüsseln in Windows Hello und entspricht den UAF-Spezifikationen der FIDO-Allianz. Ihre biometrischen Daten werden nur lokal zum Schutz Ihrer Daten verwendet und niemals an die Cloud gesendet oder dort gespeichert. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Wo kann ich mein Kennwort durch mein Smartphone ersetzen, und wo benötige ich weiterhin das Kennwort?  

Nach aktuellem Stand steht die Anmeldung per Telefon nur zur Verfügung für Web-Apps und Webdienste, die auf persönlichen Microsoft-Konten basieren, für iOS- oder Android-Apps, die ein persönliches Microsoft-Konto verwenden, oder für Apps unter Windows 10, die ein persönliches Microsoft-Konto verwenden. Wenn Sie sich bei einer dieser Websites oder Web-Apps anmelden, finden Sie auf der Seite, auf der Sie normalerweise Ihr Kennwort eingeben, einen Link vom Typ **Stattdessen eine App verwenden**. 

Die Anmeldung per Telefon kann derzeit nicht verwendet werden, um einen Windows-PC, eine Xbox oder eine Desktopversion von Microsoft-Apps (beispielsweise Office-Apps) zu entsperren. 
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Ersetzt dieses Feature die Überprüfung in zwei Schritten? Sollte ich sie deaktivieren?   

Manchmal. Wir arbeiten zwar an einer Ausweitung der Anmeldung per Telefon, noch gibt es aber Bereiche im Ökosystem von Microsoft, in denen sie noch nicht unterstützt wird. Dort wird für die sichere Anmeldung weiterhin die Überprüfung in zwei Schritten verwendet. Aus diesem Grund sollten Sie die Überprüfung in zwei Schritten für Ihr Konto nicht deaktivieren. 
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-will-i-have-to-approve-two-notifications"></a>Kann ich die Überprüfung in zwei Schritten für mein Konto aktiviert lassen, oder muss ich dann zwei Benachrichtigungen bestätigen?

Nein, müssen Sie nicht. Wenn Sie sich bei Ihrem Microsoft-Konto mit Ihrem Smartphone anmelden, zählt dies als Überprüfung in zwei Schritten. Anstatt Ihr Kennwort einzugeben und eine Benachrichtigung zu bestätigen, bestätigen Sie Ihre Identität, indem Sie Ihr Smartphone entsperren und eine Benachrichtigung bestätigen. Sie erhalten keine zweite Benachrichtigung, die Sie bestätigen müssen.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Wie kann ich auf mein Konto zugreifen, wenn ich mein Smartphone verliere oder nicht zur Hand habe?  

Sie können auf der Anmeldeseite jederzeit auf **Stattdessen ein Kennwort verwenden** klicken und wieder Ihr Kennwort verwenden. Bei Verwendung der Überprüfung in zwei Schritten benötigen Sie weiterhin eine zweite Methode, um Ihre Anmeldung zu bestätigen. Daher sollten Sie immer über zusätzliche, aktuelle Sicherheitsinformationen für Ihr Konto verfügen. Ihre Sicherheitsinformationen können Sie unter „https://account.live.com/proofs/manage“ verwalten. 
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Wie kann ich die Verwendung des Features beenden und wieder zur Eingabe meines Kennworts zurückkehren?

Klicken Sie bei der Anmeldung auf **Stattdessen ein Kennwort verwenden**. Wir speichern Ihre aktuelle Auswahl und stellen bei der nächsten Anmeldung das entsprechende Verfahren bereit. Falls Sie irgendwann doch wieder zur Anmeldung per Telefon wechseln möchten, klicken Sie auf **Stattdessen eine App verwenden**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>Kann ich mich mithilfe der App bei allen meinen Konten mit Microsoft anmelden?   
Diese Funktion steht derzeit nur für persönliche Microsoft-Konten zur Verfügung. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Kann ich mich mithilfe meines Smartphones bei meinem PC anmelden?  
Für Ihren PC empfehlen wir unter Windows 10 die Anmeldung mit Windows Hello per Gesichtserkennung, Fingerabdruck oder PIN.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>Kann ich mich mit meinem Windows Phone anmelden?  
Diese Funktion wird momentan nicht für Microsoft Authenticator unter Windows Phone entwickelt. 

## <a name="next-steps"></a>Nächste Schritte
Falls Sie die Microsoft Authenticator-App noch nicht heruntergeladen haben, probieren Sie sie doch einmal aus. Die App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071) erhältlich, und die Anmeldung per Telefon steht in der Microsoft Authenticator-App für [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) zur Verfügung.

Antworten auf allgemeine Fragen zur App finden Sie unter [Microsoft Authenticator-App – häufig gestellte Fragen](microsoft-authenticator-app-faq.md).

