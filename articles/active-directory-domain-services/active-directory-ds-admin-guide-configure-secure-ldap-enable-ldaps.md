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
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 3b19f078b0d6dc3e02d951014056406fd1b099a8
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne

## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass Sie [Aufgabe 2: Exportieren der Zertifikate für sicheres LDAP in eine PFX-Datei](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md) fertiggestellt haben.

Entscheiden Sie sich, ob Sie die Vorschauversion des Azure-Portals oder das klassische Azure-Portal verwenden, um diese Aufgabe abzuschließen.
> [!div class="op_single_selector"]
> * **Azure-Portal (Vorschauversion)**: [Aktivieren Sie sicheres LDAP mit dem Azure-Portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **Klassisches Azure-Portal**: [Aktivieren Sie sicheres LDAP mit dem klassischen Azure-Portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview"></a>Aufgabe 3: Aktivieren von sicherem LDAP für die verwaltete Domäne über das Azure-Portal (Vorschauversion)
Führen Sie die folgenden Konfigurationsschritte aus, um sicheres LDAP zu aktivieren:

1. Navigieren Sie zum **[Azure-Portal](https://portal.azure.com)**.

2. Suchen Sie nach „Domänendienste“ im Suchfeld **Search resources** (Ressourcen suchen). Wählen Sie **Azure AD Domain Services** aus dem Suchergebnis aus. Das Blatt **Azure AD Domain Services** listet Ihre verwaltete Domäne auf.

    ![Suchen der bereitgestellten verwalteten Domäne](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klicken Sie auf den Namen der verwalteten Domäne (z.B. „contoso100.com“), um weitere Details zu der Domäne zu erhalten.

    ![Domänendienste – Bereitstellungsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. Klicken Sie im Navigationsbereich auf **Secure LDAP**.

    ![Domänendienste – „Secure LDAP“-Blatt](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Standardmäßig ist der sichere LDAP-Zugriff auf Ihre verwaltete Domäne deaktiviert. Ändern Sie die Einstellung für **Secure LDAP** in **Aktivieren**.

    ![Aktivieren von sicherem LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Standardmäßig ist der sichere LDAP-Zugriff auf Ihre verwaltete Domäne über das Internet deaktiviert. Ändern Sie, falls gewünscht, die Einstellung für **Sicheren LDAP-Zugriff über das Internet aktivieren** in **Aktivieren**. 

6. Klicken Sie auf das Ordnersymbol hinter der **PFX-Datei mit sicherem LDAP-Zertifikat**. Geben Sie den Pfad zu der PFX-Datei mit dem Zertifikat für den sicheren LDAP-Zugriff auf die verwaltete Domäne an.

7. Geben Sie das **Kennwort zum Entschlüsseln der PFX-Datei** an. Geben Sie das gleiche Kennwort an, das Sie beim Exportieren des Zertifikats in die PFX-Datei verwendet haben.

8. Wenn Sie fertig sind, klicken Sie auf die Schaltfläche **Speichern**.

9. Sie sehen eine Benachrichtigung, die Sie darüber informiert, dass sicheres LDAP für die verwaltete Domäne konfiguriert wird. Bis dieser Vorgang abgeschlossen ist, können Sie andere Einstellungen der Domäne nicht ändern.

    ![Konfigurieren von sicherem LDAP für die verwaltete Domäne](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Es dauert ungefähr 10 bis 15 Minuten, bis sicheres LDAP für die verwaltete Domäne aktiviert ist. Wenn das bereitgestellte Zertifikat für sicheres LDAP nicht den erforderlichen Kriterien entspricht, wird LDAPS nicht für Ihr Verzeichnis aktiviert, und Sie sehen einen Fehler. Mögliche Fehlerursachen wären etwa ein falscher Domänenname oder ein bereits abgelaufenes oder bald ablaufendes Zertifikat. Wiederholen Sie in diesem Fall den Vorgang mit einem gültigen Zertifikat.
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Aufgabe 4: Konfigurieren von DNS für den Zugriff auf die verwaltete Domäne über das Internet
> [!NOTE]
> **Optionale Aufgabe** : Wenn Sie nicht über eine Internetverbindung mit LDAPS auf die verwaltete Domäne zugreifen möchten, überspringen Sie diese Konfigurationsaufgabe.
>
>

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview)beschriebenen Schritte vollständig ausgeführt haben.

Nachdem Sie den Zugriff auf Ihre verwaltete Domäne über sicheres LDAP und eine Internetverbindung aktiviert haben, müssen Sie das DNS aktualisieren, damit die Clientcomputer die verwaltete Domäne finden können. Am Ende von Aufgabe 3 wird auf dem Blatt **Eigenschaften** im Feld **EXTERNE IP-ADRESSE FÜR LDAPS-ZUGRIFF** eine externe IP-Adresse angezeigt.

Konfigurieren Sie Ihren externen DNS-Anbieter so, dass der DNS-Name der verwalteten Domäne (beispielsweise „ldaps.contoso100.com“) auf diese externe IP-Adresse verweist. In unserem Beispiel müssen Sie den folgenden DNS-Eintrag erstellen:

    ldaps.contoso100.com  -> 52.165.38.113

Das ist schon alles. Sie können jetzt über das Internet und sicheres LDAP eine Verbindung mit der verwalteten Domäne herstellen.

> [!WARNING]
> Denken Sie daran, dass die Clientcomputer dem Aussteller des Zertifikats für LDAPS vertrauen müssen, damit die Verbindung mit der verwalteten Domäne über LDAPS erfolgreich hergestellt werden kann. Wenn Sie eine für die Öffentlichkeit vertrauenswürdige Zertifizierungsstelle verwenden, ist kein weiterer Schritt erforderlich, da Clientcomputer diesen Zertifikatausstellern vertrauen. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie den öffentlichen Teil des selbstsignierten Zertifikats im vertrauenswürdigen Zertifikatpeicher auf den Clientcomputern installieren.
>
>


## <a name="task-5---lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a>Aufgabe 5: LDAPS-Zugriff auf Ihre verwaltete Domäne über das Internet sperren
> [!NOTE]
> **Optionale Aufgabe**: Wenn Sie den LDAPS-Zugriff auf die verwaltete Domäne über das Internet nicht aktiviert haben, können Sie diese Konfigurationsaufgabe überspringen.
>
>

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview)beschriebenen Schritte vollständig ausgeführt haben.

Das Verfügbarmachen Ihrer verwalteten Domäne für LDAPS-Zugriff über das Internet stellt ein Sicherheitsrisiko dar. Die verwaltete Domäne ist aus dem Internet über den für das sichere LDAP verwendeten Port erreichbar (d.h. Port 636). Aus diesem Grund können Sie sich entscheiden, den Zugriff auf die verwaltete Domäne auf bestimmte bekannte IP-Adressen zu beschränken. Erstellen Sie zur Verbesserung der Sicherheit eine Netzwerksicherheitsgruppe (NSG), und ordnen Sie diese dem Subnetz zu, in dem Sie Azure AD Domain Services aktiviert haben.

Die folgende Tabelle zeigt ein Beispiel einer NSG, die Sie konfigurieren können, um den sicheren LDAP-Zugriff über das Internet zu sperren. Die NSG enthält einen Satz von Regeln, die den eingehenden LDAPS-Zugriff über TCP-Port 636 nur aus einer angegebenen Gruppe von IP-Adressen zulassen. Die standardmäßige Regel „DenyAll“ gilt für sämtlichen eingehenden Datenverkehr aus dem Internet. Die NSG-Regel zum Zulassen des LDAPS-Zugriffs über das Internet von angegebenen IP-Adressen hat eine höhere Priorität als die NSG-Regel „DenyAll“.

![Beispiel-NSG zum Schutz des sicheren LDAPS-Zugriffs über das Internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Weitere Informationen** - [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Verwandte Inhalte
* [Erste Schritte mit Azure AD Domain Services](active-directory-ds-getting-started.md)
* [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
* [Verwalten von Gruppenrichtlinien in einer durch Azure Active Directory Domain Services verwalteten Domäne](active-directory-ds-admin-guide-administer-group-policy.md)
* [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)
* [Erstellen einer Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

