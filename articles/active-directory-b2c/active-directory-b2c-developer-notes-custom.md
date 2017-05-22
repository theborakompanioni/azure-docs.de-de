---
title: 'Azure Active Directory B2C: Entwicklerhinweise zur Verwendung von benutzerdefinierten Richtlinien | Microsoft-Dokumentation'
description: "Enthält Hinweise für Entwickler, die das Konfigurieren und Verwalten von B2C mit benutzerdefinierten Richtlinien betreffen."
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
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 72447a234c5889f36b10b828d28775fc5995c4d4
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Versionshinweise für benutzerdefinierte Azure Active Directory B2C-Richtlinien – Öffentliche Vorschauversion
Der Featuresatz für **benutzerdefinierte Richtlinien** ist jetzt als öffentliche Vorschauversion für alle Azure AD B2C-Kunden (Azure Active Directory B2C) zur Evaluierung verfügbar.  Dieser Featuresatz ist für fortgeschrittene Identitätsentwickler bestimmt, die sehr komplexe Identitätslösungen erstellen.  

Bei diesem Featuresatz müssen Entwickler das Identity Experience Framework (IEF) direkt per Bearbeitung der XML-Datei konfigurieren.  Dieses Konfigurationsverfahren ist leistungsstark und komplex.  Fortgeschrittene Identitätsentwickler, die das IEF verwenden, sollten Zeit für das Lernen einplanen, z.B. für das Durcharbeiten von exemplarischen Vorgehensweisen und Lesen von Referenzdokumenten. 

## <a name="features-included-in-this-public-preview"></a>In der öffentlichen Vorschauversion enthaltene Features
Mit den neuen Features, die mit der öffentlichen Vorschauversion eingeführt werden, können Entwickler die folgenden Aufgaben durchführen:
1. Erstellen und Hochladen von benutzerdefinierten User Journeys für die Authentifizierung mit benutzerdefinierten Richtlinien
* Beschreiben von User Journeys als Austauschvorgänge zwischen Anspruchsanbietern (Schritt für Schritt)
* Definieren der bedingten Verzweigung in User Journeys
2. Integrieren von REST-API-fähigen Diensten in Ihren User Journeys der benutzerdefinierten Authentifizierung
3. Hinzufügen des Verbunds mit Identitätsanbietern, die mit dem OpenIDConnect-Standard konform sind
4. Hinzufügen des Verbunds mit Identitätsanbietern, die auf dem SAML 2.0-Protokoll basieren



## <a name="terms-of-the-public-preview"></a>Bedingungen der öffentlichen Vorschauversion
1. Die Nutzung der neuen Features sollte nur zu Evaluierungszwecken erfolgen.
2. Die neuen Features sind nicht für die Nutzung in einer Produktionsumgebung bestimmt.
3. Vereinbarungen zum Servicelevel (SLAs) gelten für die neuen Features nicht. 4. Supportanfragen können über die üblichen Supportkanäle gesendet werden.
5. Es liegt kein festes Datum für die allgemeine Verfügbarkeit vor.
6. Microsoft behält sich das Recht vor, Szenarien und User Journeys, die den vorgesehenen Anwendungsumfang des Azure AD B2C-Produkts als CIAM-Plattform (Customer Identity and Access Management) überschreiten, zu kennzeichnen und aus beliebigen Gründen abzulehnen oder einzuschränken.

## <a name="responsibilities-for-custom-policy-feature-set-developers"></a>Zuständigkeiten für Entwickler von Funktionssätzen für benutzerdefinierte Richtlinien
Die manuelle Richtlinienkonfiguration gewährt eingehenderen Zugriff auf die zugrunde liegende Plattform von Azure AD B2C und führt zur Erstellung eines eindeutigen und vollständig anpassbaren Vertrauensframeworks.  Die nahezu unendlichen Permutationen von benutzerdefinierten Identitätsanbietern, Vertrauensstellungen, Integrationen in externe Dienste und Schritt-für-Schritt-Workflows stellen für fortgeschrittene Entwickler, die sie nutzen, eine höhere Anforderung dar.
Um von der öffentlichen Vorschauversion in vollem Umfang zu profitieren, ist es ratsam, dass sich Entwickler, die den Funktionssatz für benutzerdefinierte Richtlinien verwenden, an folgende Vorgaben halten:
* Machen Sie sich mit der Konfigurationssprache der Identity Experience Engine (IEE) und der Verwaltung von Schlüsseln/Geheimnissen vertraut.
* Übernehmen Sie den Besitz für Szenarien und benutzerdefinierte Integrationen.
* Führen Sie methodische Szenariotests durch.
* Nutzen Sie die bewährten Methoden für die Softwareentwicklung und das Staging, und verwenden Sie mindestens eine Entwicklungs-/Testumgebung und eine Produktionsumgebung.
* Informieren Sie sich regelmäßig über neue Entwicklungen in Bezug auf die Identitätsanbieter und Dienste, die Sie für die Integration nutzen. Behalten Sie beispielsweise die Änderungen von Geheimnissen, geplanten/ungeplanten Änderungen am Dienst usw. im Auge. Richten Sie die aktive Überwachung ein, und überwachen Sie die Reaktionsfähigkeit der Produktionsumgebungen.
* Halten Sie die E-Mail-Adressen der Kontaktpersonen aktuell, und sorgen Sie dafür, dass auf die E-Mails des Microsoft-Livewebsite-Teams schnell reagiert wird.
* Ergreifen Sie ohne Verzögerung Maßnahmen, wenn Sie vom Microsoft-Livewebsite-Team entsprechend aufgefordert werden. 


>[!NOTE]
>Diese Funktionen können später in integrierte Azure AD-Richtlinien eingefügt werden, damit sie für alle Entwickler besser zugänglich sind.

## <a name="next-steps"></a>Nächste Schritte
[Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md)

