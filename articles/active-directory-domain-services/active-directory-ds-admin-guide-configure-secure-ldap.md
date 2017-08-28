---
title: Konfigurieren von sicherem LDAP (LDAPS) in Azure AD Domain Services | Microsoft Docs
description: "Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 93afa49166c5b31d23237c308b9d34f6d6f3507d
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurieren von sicherem LDAP (LDAPS) für eine über Azure AD Domain Services verwaltete Domäne
Dieser Artikel zeigt, wie Sie sicheres LDAP (LDAPS, Secure Lightweight Directory Access Protocol) für eine durch Azure AD Domain Services verwaltete Domäne aktivieren können. Sicheres LDAP ist auch bekannt als „Lightweight Directory Access Protocol (LDAP) über Secure Sockets Layer (SSL)/Transport Layer Security (TLS)“.

## <a name="before-you-begin"></a>Voraussetzungen
Um die in diesem Artikel beschriebenen Aufgaben auszuführen, benötigen Sie Folgendes:

1. Ein gültiges **Azure-Abonnement**.
2. Ein **Azure AD-Verzeichnis** – entweder synchronisiert mit einem lokalen Verzeichnis oder als reines Cloud-Verzeichnis
3. **Azure AD Domain Services** müssen für das Azure AD-Verzeichnis aktiviert sein. Wenn dies noch nicht der Fall ist, führen Sie alle Aufgaben im Leitfaden [Erste Schritte](active-directory-ds-getting-started.md)aus.
4. Ein **Zertifikat, das zum Aktivieren von sicherem LDAP verwendet werden kann**

   * **Empfehlung:** Beziehen Sie ein Zertifikat von einer vertrauenswürdigen öffentlichen Zertifizierungsstelle. Diese Konfigurationsoption bietet mehr Sicherheit.
   * Alternativ kann können Sie [ein selbstsigniertes Zertifikat erstellen](#task-1---obtain-a-certificate-for-secure-ldap). Das Verfahren dafür wird weiter unten in diesem Artikel beschrieben.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Anforderungen an ein Zertifikat für sicheres LDAP
Erwerben Sie ein gültiges Zertifikat, das den folgenden Richtlinien entspricht, bevor Sie sicheres LDAP aktivieren. Es kommt zu Fehlern, wenn Sie beim Aktivieren von sicherem LDAP für Ihre verwaltete Domäne ein ungültiges oder falsches Zertifikat verwenden.

1. **Vertrauenswürdiger Aussteller**: Das Zertifikat muss von einer Zertifizierungsstelle ausgestellt sein, der die Computer vertrauen, die über sicheres LDAP eine Verbindung mit der Domäne herstellen. Hierbei kann es sich um eine öffentliche Zertifizierungsstelle handeln, die von diesen Computern als vertrauenswürdig eingestuft wird.
2. **Lebensdauer** : Das Zertifikat muss mindestens für die nächsten 3 bis 6 Monate gültig sein. Der Zugriff auf Ihre verwaltete Domäne über sicheres LDAP wird unterbrochen, wenn das Zertifikat abläuft.
3. **Antragstellername**: Als Name des Antragstellers muss im Zertifikat für die verwaltete Domäne ein Platzhalter angegeben werden. Wenn Ihre Domäne z. B. „contoso100.com“ heißt, muss als Antragstellername im Zertifikat „*.contoso100.com“ angegeben sein. Legen Sie als DNS-Name (bzw. alternativen Antragstellernamen) diesen Platzhalternamen fest.
4. **Schlüsselverwendung** : Das Zertifikat muss für die folgenden Verwendungszwecke konfiguriert sein: digitale Signaturen und Schlüsselverschlüsselung.
5. **Zertifikatzweck** : Das Zertifikat muss für die SSL-Serverauthentifizierung gültig sein.

> [!NOTE]
> **Unternehmenszertifizierungsstellen**: Die Verwendung von Secure LDAP-Zertifikaten, die von der Unternehmenszertifizierungsstelle Ihrer Organisation ausgestellt wurden, wird von Azure AD Domain Services nicht unterstützt. Diese Einschränkung gilt, da der Dienst Ihre Unternehmenszertifizierungsstelle nicht als vertrauenswürdige Stammzertifizierungsstelle betrachtet. 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Aufgabe 1: Erwerben eines Zertifikats für sicheres LDAP
Zuerst müssen Sie ein Zertifikat erwerben, das Sie zum Zugriff auf die verwaltete Domäne über sicheres LDAP verwenden. Sie haben zwei Möglichkeiten:

* Erwerben Sie ein SSL-Zertifikat von einer Zertifizierungsstelle. Hierbei kann es sich um eine öffentliche Zertifizierungsstelle handeln.
* Erstellen eines selbstsignierten Zertifikats

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Option A (empfohlen): Erwerben eines Zertifikats für sicheres LDAP von einer Zertifizierungsstelle
Wenn Ihre Organisation ihre Zertifikate von einer öffentlichen Zertifizierungsstelle erhält, müssen Sie das Zertifikat für sicheres LDAP bei dieser öffentlichen Zertifizierungsstelle erwerben.

Stellen Sie beim Anfordern des Zertifikats sicher, dass alle Anforderungen erfüllt werden, die unter [Requirements for the secure LDAP certificate (Anforderungen an ein Zertifikat für sicheres LDAP)](#requirements-for-the-secure-ldap-certificate) aufgeführt sind.

> [!NOTE]
> Die Clientcomputer, die über sicheres LDAP eine Verbindung mit der verwalteten Domäne herstellen sollen, müssen dem Aussteller des Zertifikats für sicheres LDAP vertrauen.
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Option B: Erstellen eines selbstsignierten Zertifikats für sicheres LDAP
Wenn Sie voraussichtlich kein Zertifikat einer öffentlichen Zertifizierungsstelle verwenden, können Sie ein selbstsigniertes Zertifikat für sicheres LDAP erstellen.

**Erstellen eines selbstsignierten Zertifikats mit PowerShell**

Öffnen Sie auf Ihrem Windows-Computer als **Administrator** ein neues PowerShell-Fenster, und geben Sie die folgenden Befehle ein, um ein neues selbstsigniertes Zertifikat zu erstellen.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Ersetzen Sie im vorherigen Beispiel „*.contoso100.com“ durch den DNS-Domänennamen Ihrer verwalteten Domäne. Wenn Sie zum Beispiel eine verwaltete Domäne mit dem Namen „contoso100.onmicrosoft.com“ erstellt haben, ersetzen Sie „*.contoso100.com“ im obigen Script durch „*.contoso100.onmicrosoft.com“.

![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Die neu erstellten selbstsignierten Zertifikate werden im Zertifikatspeicher des lokalen Computers platziert.


## <a name="next-step"></a>Nächster Schritt
[Task 2 - export the secure LDAP certificate to a .PFX file (Aufgabe 2: Exportieren des Zertifikats für sicheres LDAP in eine PFX-Datei)](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)

