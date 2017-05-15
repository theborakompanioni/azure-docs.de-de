---
title: "Azure Active Directory B2C: Self-Service-Kennwortzurücksetzung | Microsoft Docs"
description: "In diesem Thema wird das Einrichten der Self-Service-Kennwortrücksetzung für Ihre Kunden in Azure Active Directory B2C demonstriert."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: Human Translation
ms.sourcegitcommit: 4b13c040a15bef2f04d2cd2126e2270d061898bd
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Einrichten der Self-Service-Kennwortrücksetzung für Ihre Kunden
Das Feature für die Self-Service-Kennwortrücksetzung ermöglicht Ihren Kunden (die für lokale Konten registriert sind) das eigenständige Zurücksetzen ihrer Kennwörter. Dadurch wird die Belastung für Ihre Supportmitarbeiter erheblich reduziert, insbesondere, wenn Ihre Anwendung Millionen von Kunden besitzt, die sie regelmäßig verwenden. Derzeit unterstützen wir nur die Verwendung einer verifizierten E-Mail-Adresse als Wiederherstellungsmethode. Wir werden zukünftig zusätzliche Wiederherstellungsmethoden (verifizierte Telefonnummer, Sicherheitsfragen usw.) hinzufügen.

> [!NOTE]
> Dieser Artikel gilt für die Self-Service-Kennwortzurücksetzung im Kontext einer Anmeldungsrichtlinie. Informationen zu vollständig anpassbaren Richtlinien zur Kennwortrücksetzung, die in Ihrer App aufgerufen werden, finden Sie in [diesem Artikel](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Standardmäßig ist für Ihr Verzeichnis die Self-Service-Kennwortzurücksetzung nicht aktiviert. Verwenden Sie die folgenden Schritte zum Aktivieren der Funktion:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie Ihr Verzeichnis erstellt haben.
2. Navigieren Sie auf der Navigationsleiste links zur Active Directory-Erweiterung.
3. Suchen Sie Ihr Verzeichnis auf der Registerkarte **Verzeichnis** , und klicken Sie darauf.
4. Klicken Sie auf die Registerkarte **Configure** .
5. Scrollen Sie nach unten zum Abschnitt **Richtlinie zum Zurücksetzen des Benutzerkennworts**, und legen Sie die Option **Benutzer, für die das Zurücksetzen des Kennworts aktiviert ist** auf **JA** fest. Beachten Sie, dass die Option **Alternative E-Mail-Adresse** aktiviert ist, und lassen Sie diese so.
   
    ![Self-Service-Kennwortzurücksetzung](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Klicken Sie unten auf der Seite auf **Speichern** . Sie haben es geschafft!

Zum Testen verwenden Sie das Feature „Jetzt ausführen“ für eine beliebige Anmelderichtlinie mit lokalen Konten als Identitätsanbieter. Klicken Sie auf der lokalen Kontoanmeldeseite (auf der Sie eine E-Mail-Adresse und ein Kennwort oder einen Benutzernamen und ein Kennwort eingeben) auf **Sie können nicht auf Ihr Konto zugreifen?** , um die Benutzererfahrung zu überprüfen.

> [!NOTE]
> Die Seiten der Self-Service-Kennwortrücksetzung können mithilfe des [Features für Unternehmensbranding](../active-directory/active-directory-add-company-branding.md)angepasst werden.
> 
> 


