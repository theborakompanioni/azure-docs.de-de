---
title: "Azure Active Directory B2C: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern | Microsoft-Dokumentation"
description: "Dieses Thema veranschaulicht, wie Sie die E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C deaktivieren."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: d8e44a8aade60d21734477d60bccc2bd5194436e
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern
Wenn diese Funktionalität aktiviert ist, ermöglicht Azure Active Directory B2C es Endbenutzern, sich durch Angeben einer E-Mail-Adresse und Erstellen eines lokalen Kontos für Anwendungen zu registrieren. Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Endbenutzer diese während des Registrierungsvorgangs verifizieren müssen. B2C verhindert auch, dass ein automatisierter Prozess böswillig gefälschte Konten für die Anwendungen generiert.

Einige Anwendungsentwickler bevorzugen, die E-Mail-Überprüfung während des Registrierungsvorgangs zu überspringen und diese erst später vom Endbenutzer anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Endbenutzern zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

Standardmäßig ist die E-Mail-Überprüfung in Registrierungsrichtlinien aktiviert. Führen Sie zum Aktivieren der Funktion folgende Schritte aus:

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf **Registrierungsrichtlinien** oder auf **Registrierungs- oder Anmelderichtlinien**, je nachdem, welche Art Richtlinien Sie für die Registrierung konfiguriert haben.
3. Klicken Sie auf Ihre Richtlinie (z.B. „B2C_1_SiUp“), um sie zu öffnen. Klicken Sie oben auf dem Blatt auf **Bearbeiten**.
4. Klicken Sie auf **Seite für die Benutzeroberflächenanpassung**.
5. Klicken Sie auf **Registrierungsseite für lokales Konto**.
6. Klicken Sie im Abschnitt **Registrierungsattribute** in der Spalte **Name** auf **E-Mail-Adresse**.
7. Legen Sie die Option **Überprüfung anfordern** auf **Nein** fest.
8. Klicken Sie im unteren Bereich auf **OK**, bis Sie zum Blatt **Richtlinie bearbeiten** gelangen.
9. Klicken Sie oben auf dem Blatt auf **Speichern** . Sie haben es geschafft!

> [!NOTE]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie den Standardprozess deaktivieren, empfiehlt es sich, ein eigenes Überprüfungssystem einzurichten.
> 
> 

Wir sind stets offen für Feedback und Vorschläge. Wenn Sie Probleme mit diesem Thema oder Vorschläge zur Verbesserung dieses Inhalts haben, würden wir uns über Ihr Feedback unten auf der Seite freuen. Anforderungen neuer Features geben Sie bitte unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) ein.
