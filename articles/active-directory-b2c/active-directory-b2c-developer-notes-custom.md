---
title: 'Azure Active Directory B2C: Entwicklerhinweise zur Verwendung von benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: "Enthält Hinweise für Entwickler, die das Konfigurieren und Verwalten von Azure AD B2C mit benutzerdefinierten Richtlinien betreffen."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Versionshinweise für benutzerdefinierte Azure Active Directory B2C-Richtlinien – Öffentliche Vorschauversion
Der Featuresatz für benutzerdefinierte Richtlinien ist jetzt als öffentliche Vorschauversion für alle Azure AD B2C-Kunden (Azure Active Directory B2C) zur Evaluierung verfügbar. Dieser Featuresatz ist für fortgeschrittene Identitätsentwickler bestimmt, die sehr komplexe Identitätslösungen erstellen.  

Bei diesem Featuresatz müssen Entwickler das Identity Experience Framework direkt per Bearbeitung der XML-Datei konfigurieren. Dieses Konfigurationsverfahren ist leistungsstark und komplex. Fortgeschrittene Identitätsentwickler, die das Identity Experience Framework verwenden, sollten Zeit einplanen, z.B. für das Durcharbeiten von exemplarischen Vorgehensweisen und Lesen von Referenzdokumenten. 

## <a name="features-included-in-this-public-preview"></a>In der öffentlichen Vorschauversion enthaltene Features
Mit den neuen Features, die mit der öffentlichen Vorschauversion eingeführt werden, können Entwickler die folgenden Aufgaben durchführen:<br>

* Erstellen und Hochladen von benutzerdefinierten User Journeys für die Authentifizierung mit benutzerdefinierten Richtlinien 
   * Beschreiben von User Journeys als Austauschvorgänge zwischen Anspruchsanbietern (Schritt für Schritt) 
   * Definieren der bedingten Verzweigung in User Journeys 
* Integrieren von REST-API-fähigen Diensten in User Journeys zur benutzerdefinierten Authentifizierung  
* Hinzufügen von Verbünden mit Identitätsanbietern, die mit dem OpenID Connect-Standard konform sind <br>
* Hinzufügen von Verbünden mit Identitätsanbietern, die auf dem SAML-2.0-Protokoll basieren 

## <a name="terms-of-the-public-preview"></a>Bedingungen der öffentlichen Vorschauversion

* Die Nutzung der neuen Features sollte nur zu Evaluierungszwecken erfolgen.<br>
* Die neuen Features sind nicht für die Verwendung in einer Produktionsumgebung vorgesehen.<br>
* Vereinbarungen zum Servicelevel (SLAs) gelten für die neuen Features nicht. <br>
* Supportanfragen können über die üblichen Supportkanäle gesendet werden. <br>
* Es liegt kein festes Datum für die allgemeine Verfügbarkeit vor.<br>
* Microsoft behält sich vor, Szenarien und User Journeys, die den vorgesehenen Anwendungsumfang des Azure AD B2C-Produkts als CIAM-Plattform (Customer Identity and Access Management) überschreiten, zu kennzeichnen und aus beliebigen Gründen abzulehnen oder einzuschränken.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Zuständigkeiten für Entwickler von Featuregruppen für benutzerdefinierte Richtlinien
Die manuelle Richtlinienkonfiguration gewährt eingehenderen Zugriff auf die zugrunde liegende Plattform von Azure AD B2C und führt zur Erstellung eines eindeutigen und vollständig anpassbaren Vertrauensframeworks. Die möglichen Permutationen von benutzerdefinierten Identitätsanbietern, Vertrauensstellungen, Integrationen in externe Dienste und ausführlichen Workflows stellen für fortgeschrittene Entwickler, die sie nutzen, eine höhere Anforderung dar.

Um von der öffentlichen Vorschauversion in vollem Umfang zu profitieren, ist es ratsam, dass sich Entwickler, die die Featuregruppe für benutzerdefinierte Richtlinien verwenden, an folgende Vorgaben halten:
* Machen Sie sich mit der Konfigurationssprache der Identity Experience Engine und der Verwaltung von Schlüsseln/Geheimnissen vertraut.
* Übernehmen Sie den Besitz für Szenarien und benutzerdefinierte Integrationen.
* Führen Sie methodische Szenariotests durch.
* Nutzen Sie die bewährten Methoden für die Softwareentwicklung und das Staging, und verwenden Sie mindestens eine Entwicklungs-/Testumgebung und eine Produktionsumgebung.
* Informieren Sie sich regelmäßig über neue Entwicklungen bei den von Ihnen integrierten Identitätsanbietern und -diensten. Achten Sie beispielsweise auf Änderungen an Geheimnissen sowie auf geplante und ungeplante Änderungen am Dienst.
* Richten Sie eine aktive Überwachung ein, und überwachen Sie die Reaktionsfähigkeit von Produktionsumgebungen.
* Halten Sie die E-Mail-Adressen der Kontaktpersonen aktuell, und sorgen Sie dafür, dass auf die E-Mails des Microsoft-Livewebsite-Teams schnell reagiert wird.
* Ergreifen Sie ohne Verzögerung Maßnahmen, wenn Sie vom Microsoft-Livewebsite-Team dazu aufgefordert werden. 


>[!NOTE]
>Diese Features können später in integrierte Azure AD-Richtlinien eingefügt werden, damit sie für alle Entwickler besser zugänglich sind.

## <a name="next-steps"></a>Nächste Schritte
[Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md)

