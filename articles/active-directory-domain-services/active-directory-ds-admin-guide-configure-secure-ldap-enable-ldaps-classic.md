---
title: Konfigurieren von sicherem LDAP (LDAPS) in Azure AD Domain Services | Microsoft Docs
description: "Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
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


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a>Aufgabe 3: Aktivieren von sicherem LDAP für die verwaltete Domäne über das klassische Azure-Portal
Führen Sie die folgenden Konfigurationsschritte aus, um sicheres LDAP zu aktivieren:

1. Navigieren Sie zum **[klassischen Azure-Portal](https://manage.windowsazure.com)**.
2. Wählen Sie den Knoten **Active Directory** im linken Bereich aus.
3. Wählen Sie das Azure AD-Verzeichnis aus (auch als „Mandant“ bezeichnet), für das Sie die Azure AD Domain Services aktiviert haben.

    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicken Sie auf die Registerkarte **Configure** .

    ![Registerkarte „Konfigurieren“ des Verzeichnisses](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Scrollen Sie nach unten bis zum Abschnitt **Domänendienste**. Dort finden Sie eine Option mit der Bezeichnung **Sicheres LDAP (LDAPS)** , wie im folgenden Screenshot gezeigt:

    ![Konfigurationsabschnitt von Domänendienste](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Klicken Sie auf die Schaltfläche **Zertifikat konfigurieren**, um das Dialogfeld **Zertifikat für sicheres LDAP konfigurieren** zu öffnen.

    ![Zertifikat für sicheres LDAP konfigurieren](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Klicken Sie auf das Ordnersymbol hinter **PFX-DATEI MIT ZERTIFIKAT**, um die PFX-Datei mit dem Zertifikat anzugeben, das Sie für den Zugriff auf die verwaltete Domäne über sicheres LDAP verwenden möchten. Geben Sie außerdem das Kennwort ein, das sie beim Exportieren des Zertifikats in die PFX-Datei angegeben haben. Klicken Sie dann unten auf die Schaltfläche „Fertig“.

    ![Angeben einer PFX-Datei und eines Kennworts für sicheres LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Auf der Registerkarte **Konfigurieren** wird der Abschnitt **Domänendienste** abgeblendet dargestellt und befindet sich für einige Minuten im Zustand **Ausstehend**. Während dieses Zeitraums wird das LDAPS-Zertifikat auf Richtigkeit überprüft und sicheres LDAP für die verwaltete Domäne konfiguriert.

    ![Sicheres LDAP – Status „Ausstehend“](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Es dauert ungefähr 10 bis 15 Minuten, bis sicheres LDAP für die verwaltete Domäne aktiviert ist. Wenn das bereitgestellte Zertifikat für sicheres LDAP nicht den erforderlichen Kriterien entspricht, wird LDAPS nicht für Ihr Verzeichnis aktiviert, und Sie sehen einen Fehler. Mögliche Fehlerursachen wären etwa ein falscher Domänenname oder ein bereits abgelaufenes oder bald ablaufendes Zertifikat.
   >
   >

9. Wenn sicheres LDAP für Ihre verwaltete Domäne erfolgreich aktiviert wurde, wird die Meldung **Ausstehend** nicht mehr angezeigt. Sie sehen dann den Fingerabdruck des Zertifikats.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Aufgabe 4: Aktivieren des Zugriffs über sicheres LDAP und eine Internetverbindung
**Optionale Aufgabe** : Wenn Sie nicht über eine Internetverbindung mit LDAPS auf die verwaltete Domäne zugreifen möchten, überspringen Sie diese Konfigurationsaufgabe.

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal)beschriebenen Schritte vollständig ausgeführt haben.

1. Auf der Seite **Konfigurieren** wird im Abschnitt **Domänendienste** die Option **SICHEREN LDAP-ZUGRIFF ÜBER DAS INTERNET AKTIVIEREN** angezeigt. Diese Option ist standardmäßig auf **NEIN** festgelegt, weil der Internetzugriff auf die verwaltete Domäne über sicheres LDAP in der Standardeinstellung deaktiviert ist.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Ändern Sie die Einstellung für **SICHEREN LDAP-ZUGRIFF ÜBER DAS INTERNET AKTIVIEREN** in **JA**. Klicken Sie im unteren Bereich auf **SPEICHERN** .
    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. Auf der Registerkarte **Konfigurieren** wird der Abschnitt **Domänendienste** abgeblendet dargestellt und befindet sich für einige Minuten im Zustand **Ausstehend**. Während dieser Zeit wird der Internetzugriff auf Ihre verwaltete Domäne über sicheres LDAP aktiviert.

    ![Sicheres LDAP – Status „Ausstehend“](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Es dauert ungefähr 10 Minuten, bis der Internetzugriff über sicheres LDAP auf die verwaltete Domäne aktiviert ist.
   >
   >
4. Wenn der Zugriff auf die verwaltete Domäne über eine Internetverbindung und sicheres LDAP erfolgreich aktiviert wurde, wird die Meldung **Ausstehend** nicht mehr angezeigt. Im Feld **EXTERNE IP-ADRESSE FÜR LDAPS-ZUGRIFF**sehen Sie dann die externe IP-Adresse, mit der über LDAPS auf Ihr Verzeichnis zugegriffen werden kann.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Aufgabe 5: Konfigurieren von DNS für den Zugriff auf die verwaltete Domäne über das Internet
**Optionale Aufgabe** : Wenn Sie nicht über eine Internetverbindung mit LDAPS auf die verwaltete Domäne zugreifen möchten, überspringen Sie diese Konfigurationsaufgabe.

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 4](#task-4---enable-secure-ldap-access-over-the-internet)beschriebenen Schritte vollständig ausgeführt haben.

Nachdem Sie den Zugriff auf Ihre verwaltete Domäne über sicheres LDAP und eine Internetverbindung aktiviert haben, müssen Sie das DNS aktualisieren, damit die Clientcomputer die verwaltete Domäne finden können. Am Ende von Aufgabe 4 wird auf der Registerkarte **Konfigurieren** im Feld **EXTERNE IP-ADRESSE FÜR LDAPS-ZUGRIFF** eine externe IP-Adresse angezeigt.

Konfigurieren Sie Ihren externen DNS-Anbieter so, dass der DNS-Name der verwalteten Domäne (beispielsweise „ldaps.contoso100.com“) auf diese externe IP-Adresse verweist. In unserem Beispiel müssen Sie den folgenden DNS-Eintrag erstellen:

    ldaps.contoso100.com  -> 52.165.38.113

Das ist schon alles. Sie können jetzt über das Internet und sicheres LDAP eine Verbindung mit der verwalteten Domäne herstellen.

> [!WARNING]
> Denken Sie daran, dass die Clientcomputer dem Aussteller des Zertifikats für LDAPS vertrauen müssen, damit die Verbindung mit der verwalteten Domäne über LDAPS erfolgreich hergestellt werden kann. Wenn Sie eine Unternehmenszertifizierungsstelle oder eine für die Öffentlichkeit vertrauenswürdige Zertifizierungsstelle verwenden, ist kein weiterer Schritt erforderlich, da Clientcomputer diesen Zertifikatausstellern vertrauen. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie den öffentlichen Teil des selbstsignierten Zertifikats im vertrauenswürdigen Zertifikatspeicher auf den Clientcomputern installieren.
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a>Sperren des LDAPS-Zugriffs auf Ihre verwaltete Domäne über das Internet
> [!NOTE]
> **Optionale Aufgabe**: Wenn Sie den LDAPS-Zugriff auf die verwaltete Domäne über das Internet nicht aktiviert haben, können Sie diese Konfigurationsaufgabe überspringen.
>
>

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 4](#task-4---enable-secure-ldap-access-over-the-internet)beschriebenen Schritte vollständig ausgeführt haben.

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

