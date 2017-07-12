---
title: "Ausfüllen bestimmter Felder für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation"
description: "Informationen zum Ausfüllen bestimmter Felder beim Registrieren einer benutzerdefiniert entwickelten Anwendung bei Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 9a57185938ebe9af0ca1395e75545473f45b9889
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="how-to-fill-out-specific-fields-for-a-custom-developed-application" class="xliff"></a>

# Ausfüllen bestimmter Felder für eine benutzerdefiniert entwickelte Anwendung

Dieser Artikel bietet eine kurze Beschreibung aller verfügbaren Felder im Registrierungsformular für Anwendungen im [Azure-Portal](https://portal.azure.com).

<a id="register-a-new-application" class="xliff"></a>

## Registrieren einer neuen Anwendung

-   Um eine neue Anwendung zu registrieren, navigieren Sie zum [Azure-Portal](https://portal.azure.com).

-   Klicken Sie im linken Navigationsbereich auf **Azure Active Directory**.

-   Wählen Sie **App-Registrierungen** aus, und klicken Sie auf **Hinzufügen**.

-   Dadurch wird das Formular zur Registrierung der Anwendung geöffnet.

<a id="fields-in-the-application-registration-form" class="xliff"></a>

## Felder im Registrierungsformular für Anwendungen


| Feld            | Beschreibung                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Name             | Der Namen der Anwendung. Dieser sollte mindestens vier Zeichen umfassen.                |
| Anwendungstyp | **Web-App/Web-API**: Eine Anwendung, die eine Web-App, eine Web-API oder beides repräsentiert. 
| |**Nativ**: Eine Anwendung, die auf dem Gerät oder Computer eines Benutzers installiert werden kann.           |
| Anmelde-URL      | Die URL, unter der sich Benutzer bei Ihrer Anwendung anmelden können.                                  |

Wenn Sie die oben aufgeführten Felder ausgefüllt haben, wird Ihre Anwendung im Azure-Portal registriert und Sie werden auf die Anwendungsseite weitergeleitet. Mit der Schaltfläche **Einstellungen** im Bereich der Anwendung öffnen Sie die Einstellungsseite, die weitere Felder enthält, mit denen Sie Ihre Anwendung anpassen können. Die folgende Tabelle beschreibt alle Felder auf der Einstellungsseite. Beachten Sie, dass Ihnen möglicherweise nur ein Teil dieser Felder angezeigt wird, je nachdem, ob Sie eine Webanwendung oder eine native Anwendung erstellt haben.

| Feld           | Beschreibung                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anwendungs-ID  | Wenn Sie eine Anwendung registrieren, weist Azure AD Ihrer Anwendung eine Anwendungs-ID zu. Die Anwendungs-ID kann verwendet werden, um Ihre Anwendung in Authentifizierungsanforderungen bei Azure AD eindeutig zu identifizieren und um auf Ressourcen wie die Graph-API zuzugreifen.                                                          |
| App-ID-URI      | Hierbei sollte es sich um einen eindeutigen URI handeln, üblicherweise in der Form **https://&lt;Mandantenname\_&gt;/&lt;Anwendungsname\_&gt;.** Dieser URI wird während der Autorisierungsgewährung als eindeutiger Bezeichner verwendet, um die Ressource anzugeben, für die das Token ausgestellt werden soll. Er wird auch zum „aud“-Anspruch im ausgestellten Zugriffstoken. |
| Neues Logo hochladen | Mit diesem Verfahren können Sie ein Logo für Ihre Anwendung hochladen. Das Logo muss im Format BMP, JPG oder PNG vorliegen, und die Datei sollte kleiner als 100 KB sein. Die Bildmaße sollten 215 × 215 Pixel betragen, die Bildmitte sollte 94 × 94 Pixel groß sein.                                                       |
| URL der Startseite   | Dies ist die Anmelde-URL, die während der Registrierung der Anwendung angegeben wurde.                                                                                                                                                                                                                                              |
| Abmelde-URL      | Dies ist die Abmelde-URL für das einmalige Abmelden. Azure AD sendet eine Abmeldeanforderung an diese URL, wenn ein Benutzer seine Azure AD-Sitzung über eine beliebige andere registrierte Anwendung löscht.                                                                                                                                       |
| Mehrinstanzenfähig  | Diese Option gibt an, ob die Anwendung von mehreren Mandanten verwendet werden kann. Dies bedeutet in der Regel, dass externe Organisationen Ihre Anwendung nutzen können, indem sie die Anwendung in ihrem Mandanten registrieren und ihr Zugriff auf die Organisationsdaten gewähren.                                                                   |
| Antwort-URLs      | Die Antwort-URLs sind die Endpunkte, an denen Azure AD von Ihrer Anwendung angeforderte Token zurückgibt.                                                                                                                                                                                                          |
| Umleitungs-URIs   | Bei nativen Anwendungen werden Benutzer nach erfolgreicher Autorisierung an diese URIs weitergeleitet. Azure AD prüft, ob der Umleitungs-URI, den Ihre Anwendung in der OAuth 2.0-Anforderung bereitstellt, mit einem der im Portal registrierten Werte übereinstimmt.                                                            |
| Schlüssel            | Sie können Schlüssel erstellen, um programmgesteuert und ohne Benutzerinteraktion auf Web-APIs zuzugreifen, die von Azure AD gesichert werden. Geben Sie auf der Seite \*\*Schlüssel\*\* eine Beschreibung des Schlüssels sowie das Ablaufdatum ein, und speichern Sie die Informationen, um den Schlüssel zu generieren. Stellen Sie sicher, dass Sie den Schlüssel an sicherer Stelle speichern, da Sie später nicht mehr darauf zugreifen können.             |

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Verwalten von Anwendungen mit Azure Active Directory](active-directory-enable-sso-scenario.md)

