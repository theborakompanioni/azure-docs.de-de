---
title: 'Azure AD Connect: Passthrough-Authentifizierung - Smart Lockout | Microsoft-Dokumentation'
description: "In diesem Artikel wird beschrieben, wie die Azure Active Directory-Passthrough-Authentifizierung (Azure AD) Ihre lokalen Konten vor Brute-Force-Angriffen auf Ihre Kennwörter in der Cloud schützt."
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 0781a06635219443e8ff794e72d77b967df52c40
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---

# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Azure Active Directory-Passthrough-Authentifizierung: Smart Lockout

## <a name="overview"></a>Übersicht

Azure AD bietet Schutz gegen Brute-Force-Angriffe auf Kennwörter und verhindert, dass echte Benutzer aus ihrer Office 365-Anwendung oder aus SaaS-Anwendungen ausgeschlossen werden. Diese Funktion namens **Smart Lockout** wird unterstützt, wenn die Passthrough-Authentifizierung als Anmeldemethode verwendet wird. Smart Lockout ist standardmäßig für alle Mandanten aktiviert und schützt Ihre Benutzerkonten zu jeder Zeit; es muss nicht extra eingeschaltet werden.

Smart Lockout protokolliert die gescheiterten Anmeldeversuche. Nachdem ein bestimmter **Schwellenwert** erreicht wird, verhängt Smart Lockout **für eine bestimmte Zeit eine Sperre**. Eventuelle vom Angreifer während der Dauer der Sperre unternommene Anmeldeversuche werden abgewehrt. Wird der Angriff fortgesetzt, werden bei weiteren gescheiterten Anmeldeversuchen nach Ablauf der Sperre längere Sperrdauern verhängt.

>[!NOTE]
>Standardmäßig beträgt der Schwellenwert für das Verhängen einer Sperre zehn gescheiterte Anmeldeversuche und die Sperrdauer 60 Sekunden.

Smart Lockout unterscheidet auch zwischen den Anmeldeversuchen echter Benutzer und denen von Angreifern und sperrt in den meisten Fällen nur die Angreifer aus. Diese Funktionalität verhindert, dass die echten Benutzer vorsätzlich von den Angreifern ausgesperrt werden. Um zwischen echten Benutzern und Angreifern zu unterscheiden, verwenden wir das vergangene Anmeldeverhalten, die Benutzergeräte, die verwendeten Browser und sonstige Indikatoren. Wir verbessern unsere Algorithmen ständig.

Da die Passthrough-Authentifizierung Kennwortvalidierungsanforderungen an Ihr lokales Active Directory (AD) weiterleitet, müssen Sie Angreifer daran hindern, Ihre Benutzer aus ihren AD-Konten auszusperren. Da Sie Ihre eigenen Richtlinien für AD-Kontosperrungen anwenden (insbesondere die [**Kontosperrschwelle**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) und die [**Zurücksetzungsdauer des Kontosperrungszählers**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), müssen Sie die AD-Sperrschwelle und die Sperrdauer geeignet konfigurieren, um Angriffe in der Cloud herausfiltern zu können, bevor diese Ihr lokales AD erreichen.

>[!NOTE]
>Die Smart Lookout-Funktion ist kostenlos und ist standardmäßig für alle Kunden _aktiviert_. Jedoch ist das Verändern der AD-Sperrschwelle und der Sperrdauer mit der Graph-API eine Azure AD Premium P2-Funktion.

Um sicherzustellen, dass die lokalen AD-Konten Ihrer Benutzer gut geschützt sind, müssen Sie folgende Voraussetzungen gewährleisten:

1.  Die AD-Sperrschwelle muss _niedriger sein_ als die Schwelle für die AD-Kontosperrung. Es wird empfohlen, die AD-Kontosperrschwelle auf mindestens das Zwei- oder Dreifache der AD-Sperrschwelle zu setzen.
2.  Die AD-Sperrdauer (in Sekunden) muss _länger_ sein als die AD-Zurücksetzungsdauer des Kontosperrungszählers (in Minuten).

## <a name="verify-your-ad-account-lockout-policies"></a>Überprüfen Ihrer AD-Kontosperrungsrichtlinien

Gehen Sie folgendermaßen vor, um Ihre AD-Kontosperrungsrichtlinien zu überprüfen:

1.  Öffnen Sie das Gruppenrichtlinienverwaltungstool.
2.  Bearbeiten Sie die Gruppenrichtlinie, die auf alle Benutzer angewendet wird, beispielsweise die Standarddomänenrichtlinie.
3.  Navigieren Sie zu Computerkonfiguration\Richtlinien\Windows-Einstellungen\Sicherheitseinstellungen\Kontorichtlinien\Richtlinie für Kontosperrung.
4.  Überprüfen Sie Ihre Kontosperrdauer und die Zurücksetzungsdauer des Kontosperrungszählers.

![AD-Kontosperrungsrichtlinien](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values"></a>Verwenden der Graph-API, um die Smart Lockout-Werte Ihres Mandanten zu verwalten

>[!IMPORTANT]
>Das Verändern der AD-Sperrschwelle und der Sperrdauer mit der Graph-API ist eine Azure AD Premium P2-Funktion. Sie müssen dafür auch der globale Administrator Ihres Mandanten sein.

Sie können [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) verwenden, um die Smart Lockout-Werte von AD zu lesen, festzulegen und zu aktualisieren. Sie können diese Vorgänge jedoch auch programmieren.

### <a name="read-smart-lockout-values"></a>Lesen der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten zu lesen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich an Graph Explorer als globaler Administrator für Ihren Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Klicken Sie auf „Berechtigungen ändern“, und wählen Sie die Berechtigung „Directory.ReadWrite.All“.
3. Konfigurieren Sie die Graph-API wie folgt: Setzen Sie die Version auf „BETA“, den Anforderungstyp auf „GET“ und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Klicken Sie auf „Abfrage ausführen“, um die Smart Lockout-Werte Ihres Mandanten anzuzeigen. Wenn Sie die Werte Ihres Mandanten zuvor nicht festgelegt haben, sehen Sie einen leeren Bereich.

### <a name="set-smart-lockout-values"></a>Festlegen der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten festzulegen, führen Sie die folgenden Schritte aus (nur beim ersten Mal):

1. Melden Sie sich bei Graph Explorer als globaler Administrator für Ihren Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Klicken Sie auf „Berechtigungen ändern“, und wählen Sie die Berechtigung „Directory.ReadWrite.All“ aus.
3. Konfigurieren Sie die Graph-API wie folgt: Legen Sie die Version auf „BETA“, den Anforderungstyp auf „POST“ und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings` fest.
4. Kopieren Sie die folgende JSON-Anforderung in das Feld „Anforderungstext“. Ändern Sie die Smart Lockout-Werte geeignet, und verwenden Sie eine zufällige GUID als `templateId`.
5. Klicken Sie auf „Abfrage ausführen“, um die Smart Lockout-Werte Ihres Mandanten anzuzeigen.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Falls Sie diese nicht verwenden, können Sie die Werte für **BannedPasswordList** und **EnableBannedPasswordCheck** leer ("") bzw. FALSE lassen.

Überprüfen Sie mit [diesen Schritten](#read-smart-lockout-values), dass Sie die Smart Lockout-Werte Ihres Mandanten korrekt festgelegt haben.

### <a name="update-smart-lockout-values"></a>Aktualisieren der Smart Lockout-Werte

Um die Smart Lockout-Werte Ihres Mandanten zu aktualisieren, führen Sie die folgenden Schritte aus (sofern Sie die Werte zuvor bereits festgelegt haben):

1. Melden Sie sich bei Graph Explorer als globaler Administrator für Ihren Mandanten an. Wenn Sie dazu aufgefordert werden, gewähren Sie Zugriff für die angeforderten Berechtigungen.
2. Klicken Sie auf „Berechtigungen ändern“, und wählen Sie die Berechtigung „Directory.ReadWrite.All“ aus.
3. [Um die Smart Lockout-Werte Ihres Mandanten zu lesen, führen Sie die folgenden Schritte aus](#read-smart-lockout-values). Kopieren Sie den `id`-Wert (eine GUID) des Elements mit „displayName“ als „PasswordRuleSettings“.
4. Konfigurieren Sie die Graph-API wie folgt: Setzen Sie die Version auf „BETA“, den Anforderungstyp auf „PATCH“ und die URL auf `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` – verwenden Sie die GUID aus Schritt 3 für `<id>`.
5. Kopieren Sie die folgende JSON-Anforderung in das Feld „Anforderungstext“. Ändern Sie die Smart Lockout-Werte geeignet.
6. Klicken Sie auf „Abfrage ausführen“, um die Smart Lockout-Werte Ihres Mandanten zu aktualisieren.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Überprüfen Sie mit [diesen Schritten](#read-smart-lockout-values), dass Sie die Smart Lockout-Werte Ihres Mandanten korrekt aktualisiert haben.

## <a name="next-steps"></a>Nächste Schritte
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Verfassen neuer Feature-Anforderungen

