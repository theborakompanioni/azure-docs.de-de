---
title: Hilfethemen zum App-Registrierungsportal | Microsoft Docs
description: Eine Beschreibung der verschiedenen Funktionen im App-Registrierungsportal von Microsoft.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: b961254274409215d79b5cb2c9ee230a97b42769
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="app-registration-reference"></a>Referenz zur App-Registrierung
Dieses Dokument enthält Kontext und Beschreibungen zu verschiedenen Features im App-Registrierungsportal von Microsoft: [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Meine Anwendungen
Diese Liste enthält all Ihre Anwendungen, die für die Verwendung mit dem Azure AD v2.0-Endpunkt registriert sind.  Diese Anwendungen können Benutzer sowohl über persönliche Konten vom Microsoft-Konto aus als auch über Geschäfts-, Schul- oder Unikonten von Azure Active Directory aus anmelden.  Weitere Informationen zum Azure AD v2.0-Endpunkt finden Sie in unserer [v2.0-Übersicht](active-directory-appmodel-v2-overview.md).  Diese Anwendungen können auch für die Integration in den Microsoft-Konto-Authentifizierungsendpunkt, `https://login.live.com`, verwendet werden.

## <a name="live-sdk-applications"></a>Live SDK-Anwendungen
Diese Liste enthält all Ihre Anwendungen, die für die ausschließliche Verwendung mit dem Microsoft-Konto registriert sind.  Sie können mit Azure Active Directory in keiner Weise verwendet werden.  Hier finden Sie alle Anwendungen, die zuvor beim MSA-Entwicklerportal unter `https://account.live.com/developers/applications`registriert wurden.  Alle Funktionen, die Sie zuvor unter `https://account.live.com/developers/applications` ausgeführt haben, können jetzt in diesem neuen Portal unter `https://apps.dev.microsoft.com` ausgeführt werden.  Wenn Sie weitere Fragen zu Ihren Microsoft-Kontoanwendungen haben, setzen Sie sich mit uns in Verbindung.

## <a name="application-secrets"></a>Geheime Schlüssel für Anwendungen
Geheime Schlüssel für Anwendungen sind Anmeldeinformationen, mit denen Ihre Anwendung eine zuverlässige [Clientauthentifizierung](http://tools.ietf.org/html/rfc6749#section-2.3) bei Azure AD durchführen kann.  In OAuth und OpenID Connect werden geheime Schlüssel für Anwendungen gemeinhin als `client_secret` bezeichnet.  Im v2.0-Protokoll muss jede Anwendung, die ein Sicherheitstoken an einem adressierbaren Webspeicherort (nach `https` -Schema) empfängt, einen geheimen Schlüssel für Anwendungen verwenden, um sich beim Einlösen dieses Sicherheitstokens bei Azure AD zu identifizieren.  Darüber hinaus wird jedem nativen Client, der auf einem Gerät Token empfängt, die Verwendung eines geheimen Schlüssels für eine Anwendung zur Clientauthentifizierung untersagt, um das Speichern geheimer Schlüssel in einer unsicheren Umgebung zu verhindern.

Jede Anwendung kann immer nur zwei gültige geheime Schlüssel für eine Anwendung enthalten.  Durch das Verwalten von zwei Schlüsseln haben Sie die Möglichkeit, in der gesamten Umgebung Ihrer Anwendung regelmäßige Schlüsselrollover durchzuführen.  Nachdem Sie Ihre Anwendung vollständig zu einem neuen geheimen Schlüssel migriert haben, können Sie den alten geheimen Schlüssel löschen und einen neuen bereitstellen.

Zurzeit sind nur zwei Arten von geheimen Schlüsseln für Anwendungen im App-Registrierungsportal zulässig.  Wenn Sie **Neues Kennwort generieren** auswählen, wird ein gemeinsamer geheimer Schlüssel im entsprechenden Datenspeicher generiert und gespeichert, den Sie in Ihrer Anwendung verwenden können.  Wenn Sie **Neues Schlüsselpaar generieren** wählen, wird ein neues Paar aus öffentlichem und privatem Schlüssel erstellt, das heruntergeladen und für die Clientauthentifizierung bei Azure AD verwendet werden kann.

## <a name="profile"></a>Profil
Im Profilabschnitt im App-Registrierungsportal können Sie die Anmeldeseite für Ihre Anwendung anpassen.  Zurzeit können Sie auf der Anmeldeseite das Anwendungslogo, die URL mit den Vertragsbedingungen und die Datenschutzerklärung ändern.  Das Logo muss ein transparentes Bild im Format 48 x 48 oder 50 x 50 Pixel in einer GIF-, PNG- oder JPEG-Datei von höchstens 15 KB sein.  Ändern Sie die Werte nach Belieben, und sehen Sie sich das Ergebnis auf der Anmeldeseite an.

## <a name="live-sdk-support"></a>Live SDK-Unterstützung
Wenn Sie „Live SDK-Unterstützung“ aktivieren, werden alle von Ihnen erstellten geheimen Schlüssel für Anwendungen sowohl im Azure AD- als auch im Microsoft-Konto-Datenspeicher bereitgestellt.  Dadurch kann Ihre Anwendung direkt in den Microsoft-Kontodienst (login.live.com) integriert werden.  Wenn Sie eine App direkt über Microsoft-Konto (und nicht über den Azure AD-v2.0-Endpunkt) erstellen möchten, sollten Sie sicherstellen, dass die Live SDK-Unterstützung aktiviert ist.

Wenn Sie die Live SDK-Unterstützung deaktivieren, wird der geheime Schlüssel der Anwendung nur in den Azure AD-Datenspeicher geschrieben.  Der Azure AD-Datenspeicher umfasst Vorschriften auf Unternehmensebene, die das Einhalten bestimmter Standards wie z. B. FISMA ermöglichen.  Wenn Sie die Live SDK-Unterstützung aktivieren, kann Ihre Anwendung eventuell keine Kompatibilität mit einigen dieser Standards erzielen.

Wenn Sie ausschließlich den v2.0-Endpunkt von Azure AD verwenden möchten, können Sie die Live SDK-Unterstützung problemlos deaktivieren.


