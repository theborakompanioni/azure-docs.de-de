---
title: "Geräterichtlinien für den bedingten Zugriff mit Azure Active Directory auf Office 365-Dienste | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Geräterichtlinien für den bedingten Zugriff bereitgestellt werden, um einen größeren Schutz für Unternehmensressourcen zu sorgen und gleichzeitig die Benutzerkompatibilität sowie den Zugriff auf Dienste zu verwalten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: calebb
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 2d0794781946195fc6fbab413299012e6949a4bc
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Geräterichtlinien für den bedingten Zugriff mit Active Directory auf Office 365-Dienste

Für den bedingten Zugriff müssen verschiedene Faktoren erfüllt sein. Dies schließt u.a. einen mehrstufig authentifizierten Benutzer, ein authentifiziertes Gerät und ein konformes Gerät ein. In diesem Artikel konzentrieren wir uns in erster Linie auf gerätebasierte Bedingungen, mit denen Ihre Organisation den Zugriff auf Office 365-Dienste steuern kann. 

Unternehmensbenutzer möchten bei der Arbeit oder in der Schule bzw. in der Universität über ihre persönlichen Geräte auf Office 365-Dienste wie Exchange und SharePoint Online zugreifen. Sie möchten darüber hinaus, dass der Zugriff auf sichere Weise erfolgt. Durch Geräterichtlinien für den bedingten Zugriff können Sie nicht nur einen größeren Schutz für Unternehmensressourcen sorgen, sondern gleichzeitig den Zugriff auf Dienste für Benutzer erteilen, die konforme Geräte verwenden. Richtlinien für den bedingten Zugriff auf Office 365 können im Microsoft Intune-Portal für den bedingten Zugriff festgelegt werden.

Azure Active Directory (Azure AD) erzwingt Richtlinien für den bedingten Zugriff, um den Zugriff auf Office 365-Dienste zu sichern. Sie können eine Richtlinie für den bedingten Zugriff erstellen, mit der ein Benutzer daran gehindert wird, von einem nicht kompatiblen Gerät auf einen Office 365-Dienst zuzugreifen. Der Benutzer muss zunächst die Geräterichtlinien des Unternehmens erfüllen, bevor ihm der Zugriff auf den Dienst erteilt wird. Alternativ können Sie auch eine Richtlinie erstellen, die von den Benutzern eine Registrierung ihrer Geräte verlangt, um Zugriff auf einen Office 365-Dienst zu erhalten. Richtlinien können auf alle Benutzer in einer Organisation angewendet oder auf ein paar Zielgruppen beschränkt werden. Sie können einer Richtlinie im Laufe der Zeit weitere Zielgruppen hinzufügen.

Eine Voraussetzung für die Durchsetzung von Geräterichtlinien ist, dass Benutzer ihre Geräte beim Azure AD-Geräteregistrierungsdienst registrieren müssen. Wahlweise können Sie die mehrstufige Authentifizierung für Geräte aktivieren, die beim Azure AD-Geräteregistrierungsdienst registriert werden. Es wird empfohlen, die mehrstufige Authentifizierung für den Azure Active Directory-Geräteregistrierungsdienst anzuwenden. Wenn die mehrstufige Authentifizierung aktiviert ist, werden Benutzer, die ihre Geräte beim Azure AD-Geräteregistrierungsdienst registrieren, zur zweistufigen Authentifizierung aufgefordert.

## <a name="how-does-a-conditional-access-policy-work"></a>Wie funktioniert eine Richtlinie für den bedingten Zugriff?

Wenn ein Benutzer über eine unterstützte Geräteplattform Zugriff auf einen Office 365-Dienst anfordert, authentifiziert Azure AD den Benutzer und das Gerät. Azure AD gewährt nur dann Zugriff auf den Dienst, wenn der Benutzer den Richtliniensatz für den Dienst erfüllt. Benutzer von Geräten, die nicht registriert sind, erhalten Anweisungen, wie sie eine Registrierung durchführen und Konformität für den Zugriff auf Office 365-Unternehmensdienste sicherstellen können. Benutzer von iOS- und Android-Geräten müssen ihre Geräte über die Anwendung Intune-Unternehmensportal registrieren. Wenn ein Benutzer sein Gerät registriert, wird das Gerät bei Azure AD registriert und für die Geräteverwaltung und Konformität registriert. Sie müssen den Azure AD-Geräteregistrierungsdienst mit Microsoft Intune verwenden, um die Verwaltung mobiler Geräte für Office 365-Dienste zu ermöglichen. Die Geräteregistrierung ist eine Voraussetzung dafür, dass Benutzer auf Office 365-Dienste zugreifen können, wenn Geräterichtlinien erzwungen werden.

Wenn ein Benutzer sein Gerät erfolgreich registriert, wird das Gerät als vertrauenswürdig eingestuft. Azure AD erteilt einem authentifizierten Benutzer Zugriff mit einmaligem Anmelden auf Unternehmensanwendungen. Azure AD erzwingt eine Richtlinie für den bedingten Zugriff, um nicht nur beim ersten Mal, bei dem der Benutzer Zugriff anfordert, Zugriff auf einen Dienst zu gewähren, sondern jedes Mal bei der Erneuerung einer Zugriffsanforderung. Dem Benutzer wird der Zugriff auf Dienste verweigert, wenn Anmeldeinformationen geändert werden, das Gerät verloren geht oder gestohlen wird oder die Bedingungen der Richtlinie beim Anfordern der Erneuerung des Zugriffs nicht erfüllt werden.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Geräte müssen über den Azure AD-Geräteregistrierungsdienst registriert werden.

Wenn lokale Benutzer authentifiziert werden, sind die Active Directory-Verbunddienste (AD FS) (Version 1.0 und höher) erforderlich. Wenn der Identitätsanbieter keine Unterstützung für die mehrstufige Authentifizierung bietet, tritt ein Fehler bei der mehrstufigen Authentifizierung für Workplace Join auf. Beispielsweise können Sie nicht die mehrstufige Authentifizierung mit AD FS 2.0 verwenden. Bevor Sie die mehrstufige Authentifizierung für den Azure AD-Geräteregistrierungsdienst aktivieren, stellen Sie sicher, dass die lokale AD FS-Instanz die mehrstufige Authentifizierung unterstützt und eine gültige Methode zur mehrstufigen Authentifizierung vorhanden ist. Beispielsweise weist AD FS unter Windows Server 2012 R2 Funktionen für die mehrstufige Authentifizierung auf. Vor der Aktivierung der mehrstufigen Authentifizierung für den Azure AD-Geräteregistrierungsdienst müssen Sie auch eine zusätzliche gültige Authentifizierungsmethode (mehrstufige Authentifizierung) auf dem AD FS-Server festlegen. Weitere Informationen zu unterstützten Methoden für die mehrstufige Authentifizierung in AD FS finden Sie unter [Konfigurieren zusätzlicher Authentifizierungsmethoden für AD FS](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Nächste Schritte

*   Antworten auf häufig gestellte Fragen (FAQs) finden Sie unter [FAQs zum bedingten Zugriff mit Azure Active Directory](active-directory-conditional-faqs.md).

