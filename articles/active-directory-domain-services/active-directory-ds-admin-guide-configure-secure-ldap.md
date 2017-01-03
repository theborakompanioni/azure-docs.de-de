---
title: Konfigurieren von sicherem LDAP (LDAPS) in Azure AD Domain Services | Microsoft Docs
description: "Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne"
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
ms.date: 09/21/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 53cf596d1c1df77872696f2a55363cf3a83d5efe


---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurieren von sicherem LDAP (LDAPS) für eine durch Azure AD Domain Services verwaltete Domäne
Dieser Artikel zeigt, wie Sie sicheres LDAP (LDAPS, Secure Lightweight Directory Access Protocol) für eine durch Azure AD Domain Services verwaltete Domäne aktivieren können. Sicheres LDAP ist auch bekannt als „Lightweight Directory Access Protocol (LDAP) über Secure Sockets Layer (SSL)/Transport Layer Security (TLS)“.

## <a name="before-you-begin"></a>Voraussetzungen
Um die in diesem Artikel beschriebenen Aufgaben auszuführen, benötigen Sie Folgendes:

1. Ein gültiges **Azure-Abonnement**.
2. Ein **Azure AD-Verzeichnis** – entweder synchronisiert mit einem lokalen Verzeichnis oder als reines Cloud-Verzeichnis
3. **Azure AD Domain Services** müssen für das Azure AD-Verzeichnis aktiviert sein. Wenn dies noch nicht der Fall ist, führen Sie alle Aufgaben im Leitfaden [Erste Schritte](active-directory-ds-getting-started.md)aus.
4. Ein **Zertifikat, das zum Aktivieren von sicherem LDAP verwendet werden kann**
   
   * **Empfohlen** : Beschaffen Sie ein Zertifikat von Ihrer Unternehmenszertifizierungsstelle oder von einer öffentlichen Zertifizierungsstelle. Diese Konfigurationsoption bietet mehr Sicherheit.
   * Alternativ kann können Sie [ein selbstsigniertes Zertifikat erstellen](#task-1---obtain-a-certificate-for-secure-ldap). Das Verfahren dafür wird weiter unten in diesem Artikel beschrieben.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Anforderungen an ein Zertifikat für sicheres LDAP
Erwerben Sie ein gültiges Zertifikat, das den folgenden Richtlinien entspricht, bevor Sie sicheres LDAP aktivieren. Es kommt zu Fehlern, wenn Sie beim Aktivieren von sicherem LDAP für Ihre verwaltete Domäne ein ungültiges oder falsches Zertifikat verwenden.

1. **Vertrauenswürdiger Aussteller** : Das Zertifikat muss von einer Zertifizierungsstelle ausgestellt sein, der die Computer vertrauen, die über sicheres LDAP eine Verbindung mit der Domäne herstellen. Hierbei kann es sich um die Unternehmenszertifizierungsstelle Ihrer Organisation oder um eine öffentliche Zertifizierungsstelle handeln, der diese Computer vertrauen.
2. **Lebensdauer** : Das Zertifikat muss mindestens für die nächsten 3 bis 6 Monate gültig sein. Der Zugriff auf Ihre verwaltete Domäne über sicheres LDAP wird unterbrochen, wenn das Zertifikat abläuft.
3. **Antragstellername**: Als Name des Antragstellers muss im Zertifikat für die verwaltete Domäne ein Platzhalter angegeben werden. Wenn Ihre Domäne z. B. „contoso100.com“ heißt, muss als Antragstellername im Zertifikat „*.contoso100.com“ angegeben sein. Legen Sie als DNS-Name (bzw. alternativen Antragstellernamen) diesen Platzhalternamen fest.
4. **Schlüsselverwendung** : Das Zertifikat muss für die folgenden Verwendungszwecke konfiguriert sein: digitale Signaturen und Schlüsselverschlüsselung.
5. **Zertifikatzweck** : Das Zertifikat muss für die SSL-Serverauthentifizierung gültig sein.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Aufgabe 1: Erwerben eines Zertifikats für sicheres LDAP
Zuerst müssen Sie ein Zertifikat erwerben, das Sie zum Zugriff auf die verwaltete Domäne über sicheres LDAP verwenden. Sie haben zwei Möglichkeiten:

* Erwerben Sie ein SSL-Zertifikat von einer Zertifizierungsstelle. Hierbei kann es sich um die Unternehmenszertifizierungsstelle Ihrer Organisation oder um eine öffentliche Zertifizierungsstelle handeln.
* Erstellen eines selbstsignierten Zertifikats

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Option A (empfohlen): Erwerben eines Zertifikats für sicheres LDAP von einer Zertifizierungsstelle
Wenn Ihre Organisation eine unternehmensinterne Public Key-Infrastruktur (PKI) unterhält, müssen Sie das Zertifikat über die Unternehmenszertifizierungsstelle (ZS) für Ihre Organisation erwerben. Wenn Ihre Organisation ihre Zertifikate von einer öffentlichen Zertifizierungsstelle erhält, müssen Sie das Zertifikat für sicheres LDAP bei dieser öffentlichen Zertifizierungsstelle erwerben.

Stellen Sie beim Anfordern des Zertifikats sicher, dass alle Anforderungen erfüllt werden, die unter [Anforderungen an ein Zertifikat für sicheres LDAP](#requirements-for-the-secure-ldap-certificate)aufgeführt sind.

> [!NOTE]
> Die Clientcomputer, die eine Verbindung mit der verwalteten Domäne über sicheres LDAP herstellen sollen, müssen dem Aussteller des Zertifikats für sicheres LDAP (LDAPS) vertrauen.
> 
> 

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Option B: Erstellen eines selbstsignierten Zertifikats für sicheres LDAP
Sie können sich unter folgenden Umständen für das Erstellen eines selbstsignierten Zertifikats für sicheres LDAP entscheiden:

* Die Zertifikate in Ihrer Organisation werden nicht von einer Unternehmenszertifizierungsstelle ausgestellt, oder
* Sie möchten auf absehbare Zeit kein Zertifikat von einer öffentlichen Zertifizierungsstelle verwenden.

**Erstellen eines selbstsignierten Zertifikats mit PowerShell**

Öffnen Sie auf Ihrem Windows-Computer als **Administrator** ein neues PowerShell-Fenster, und geben Sie die folgenden Befehle ein, um ein neues selbstsigniertes Zertifikat zu erstellen.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Ersetzen Sie im vorstehenden Beispiel „contoso100.com“ durch den DNS-Domänennamen der Domäne, die durch die Azure AD Domain Services verwaltet wird.

![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Die neu erstellten selbstsignierten Zertifikate werden im Zertifikatspeicher des lokalen Computers platziert.

## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Aufgabe 2: Exportieren der Zertifikate für sicheres LDAP in eine PFX-Datei
Vergewissern Sie sich vor dem Ausführen dieser Aufgabe, dass Sie das Zertifikat für sicheres LDAP von Ihrer Unternehmenszertifizierungsstelle oder von einer öffentlichen Zertifizierungsstelle erhalten oder ein selbstsigniertes Zertifikat erstellt haben.

Gehen Sie folgendermaßen vor, um das Zertifikat für sicheres LDAP (LDAPS) in eine PFX-Datei zu exportieren.

1. Klicken Sie auf die Schaltfläche **Start**, und geben Sie **R** ein. Geben Sie im Dialogfeld **Ausführen** den Befehl **mmc** ein, und klicken Sie auf **OK**.
   
    ![Starten der MMC-Konsole](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Klicken Sie an der Eingabeaufforderung der Benutzerkontensteuerung** **auf **JA**, um die MMC (Microsoft Management Console) als Administrator zu starten.
3. Klicken Sie im Menü **Datei** auf **Snap-In hinzufügen/entfernen**.
   
    ![Hinzufügen des Snap-Ins zur MMC-Konsole](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. Wählen Sie im Dialogfeld **Snap-Ins hinzufügen bzw. entfernen** das Snap-In **Zertifikate** aus, und klicken Sie auf die Schaltfläche **Hinzufügen >**.
   
    ![Hinzufügen des Zertifikat-Snap-Ins zur MMC-Konsole](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. Wählen Sie im Assistenten **Zertifikat-Snap-In** die Option **Computerkonto** aus, und klicken Sie auf **Weiter**.
   
    ![Hinzufügen des Zertifikat-Snap-Ins für das Computerkonto](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Wählen Sie auf der Seite **Computer auswählen** die Option **Lokaler Computer: (Computer, auf dem diese Konsole ausgeführt wird)** aus, und klicken Sie auf **Fertig stellen**.
   
    ![Hinzufügen des Zertifikat-Snap-Ins – Computer auswählen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. Klicken Sie im Dialogfeld **Snap-Ins hinzufügen bzw. entfernen** auf **OK**, um der MMC das Zertifikat-Snap-In hinzuzufügen.
   
    ![Hinzufügen des Zertifikat-Snap-Ins zur MMC-Konsole – fertig](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Klicken Sie im MMC-Fenster auf den **Konsolenstamm**, um ihn zu erweitern. Das Zertifikat-Snap-In sollte angezeigt werden. Klicken Sie auf **Zertifikate (Lokaler Computer)** , um diesen Knoten zu erweitern. Klicken Sie nacheinander auf die Knoten **Eigene Zertifikate** und **Zertifikate**, um sie zu erweitern.
   
    ![Öffnen des eigenen Zertifikatspeichers](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Sie sollten das zuvor erstellte selbstsignierte Zertifikat sehen. Sie können die Eigenschaften des Zertifikats überprüfen und sich vergewissern, dass der Fingerabdruck demjenigen entspricht, der beim Erstellen des Zertifikats in den PowerShell-Fenstern angezeigt wurde.
10. Wählen Sie das selbstsignierte Zertifikat aus, und **klicken Sie mit der rechten Maustaste darauf**. Wählen Sie im Kontextmenü **Alle Aufgaben** und anschließend **Exportieren** aus.
    
    ![Exportieren des Zertifikats](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. Klicken Sie im Zertifikatexport-Assistenten** **auf **Weiter**.
    
    ![Exportieren des Zertifikats – Assistent](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Wählen Sie auf der Seite **Privaten Schlüssel exportieren** die Option **Ja, privaten Schlüssel exportieren** aus, und klicken Sie auf **Weiter**.
    
    ![Exportieren des Zertifikats – privater Schlüssel](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)
    
    > [!WARNING]
    > Sie MÜSSEN den privaten Schlüssel gemeinsam mit dem Zertifikat exportieren. Wenn Sie eine PFX-Datei bereitstellen, die nicht den privaten Schlüssel für das Zertifikat enthält, kommt es beim Aktivieren von sicherem LDAP für Ihre verwaltete Domäne zu Fehlern.
    > 
    > 
13. Wählen Sie auf der Seite **Format der zu exportierenden Datei** als Dateiformat für das zu exportierende Zertifikat die Option **Privater Informationsaustausch – PKCS #12 (.PFX)** aus.
    
    ![Exportieren des Zertifikats – Dateiformat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)
    
    > [!NOTE]
    > Es wird nur das Dateiformat .PFX unterstützt. Exportieren Sie das Zertifikat nicht im Dateiformat .CER.
    > 
    > 
14. Wählen Sie auf der Seite **Sicherheit** die Option **Kennwort** aus, und geben Sie ein Kennwort zum Schutz der PFX-Datei ein. Notieren Sie sich dieses Kennwort, denn Sie benötigen es in der nächsten Aufgabe. Klicken Sie zum Fortfahren auf **Weiter** .
    
    ![Kennwort für den Zertifikatexport ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)
    
    > [!NOTE]
    > Notieren Sie sich dieses Kennwort. Sie benötigen es zum Aktivieren von sicherem LDAP für diese verwaltete Domäne in [Aufgabe 3: Aktivieren von sicherem LDAP für die verwaltete Domäne](#task-3---enable-secure-ldap-for-the-managed-domain)
    > 
    > 
15. Geben Sie auf der Seite **Zu exportierende Datei** den Dateinamen und den Speicherort für den Export des Zertifikats an.
    
    ![Pfad für den Zertifikatexport](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Klicken Sie auf der folgenden Seite auf **Fertig stellen** , um das Zertifikat in eine PFX-Datei zu exportieren. Nach dem Export des Zertifikats wird ein Bestätigungsdialogfeld angezeigt.
    
    ![Exportieren des Zertifikats – fertig](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)

## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Aufgabe 3: Aktivieren von sicherem LDAP für die verwaltete Domäne
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
7. Klicken Sie auf das Ordnersymbol unter **PFX-DATEI MIT ZERTIFIKAT** , um die PFX-Datei mit dem Zertifikat anzugeben, das Sie für den Zugriff auf die verwaltete Domäne über sicheres LDAP verwenden möchten. Geben Sie außerdem das Kennwort ein, das sie beim Exportieren des Zertifikats in die PFX-Datei angegeben haben. Klicken Sie dann unten auf die Schaltfläche „Fertig“.
   
    ![Angeben einer PFX-Datei und eines Kennworts für sicheres LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Auf der Registerkarte **Konfigurieren** wird der Abschnitt **Domänendienste** abgeblendet dargestellt und befindet sich für einige Minuten im Zustand **Ausstehend**. Während dieses Zeitraums wird das LDAPS-Zertifikat auf Richtigkeit überprüft und sicheres LDAP für die verwaltete Domäne konfiguriert.
   
    ![Sicheres LDAP – Status „Ausstehend“](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)
   
   > [!NOTE]
   > Es dauert ungefähr 10 bis 15 Minuten, bis sicheres LDAP für die verwaltete Domäne aktiviert ist. Wenn das bereitgestellte Zertifikat für sicheres LDAP nicht den erforderlichen Kriterien entspricht, wird LDAPS nicht für Ihr Verzeichnis aktiviert, und Sie sehen einen Fehler. Mögliche Fehlerursachen sind z.B. ein falscher Domänenname, ein abgelaufenes oder bald ablaufendes Zertifikat usw.
   > 
   > 
9. Wenn sicheres LDAP für Ihre verwaltete Domäne erfolgreich aktiviert wurde, wird die Meldung **Ausstehend** nicht mehr angezeigt. Sie sehen dann den Fingerabdruck des Zertifikats.
   
    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Aufgabe 4: Aktivieren des Zugriffs über sicheres LDAP und eine Internetverbindung
**Optionale Aufgabe** : Wenn Sie nicht über eine Internetverbindung mit LDAPS auf die verwaltete Domäne zugreifen möchten, überspringen Sie diese Konfigurationsaufgabe.

Vor dem Ausführen dieser Aufgabe müssen Sie die in [Aufgabe 3](#task-3---enable-secure-ldap-for-the-managed-domain)beschriebenen Schritte vollständig ausgeführt haben.

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

Konfigurieren Sie Ihren externen DNS-Anbieter so, dass der DNS-Name der verwalteten Domäne (z.B. „contoso100.com“) auf diese externe IP-Adresse verweist. In unserem Beispiel müssen Sie den folgenden DNS-Eintrag erstellen:

    contoso100.com  -> 52.165.38.113

Das ist schon alles. Sie können jetzt über das Internet und sicheres LDAP eine Verbindung mit der verwalteten Domäne herstellen.

> [!WARNING]
> Denken Sie daran, dass die Clientcomputer dem Aussteller des Zertifikats für LDAPS vertrauen müssen, damit die Verbindung mit der verwalteten Domäne über LDAPS erfolgreich hergestellt werden kann. Wenn Sie eine Unternehmenszertifizierungsstelle oder eine für die Öffentlichkeit vertrauenswürdige Zertifizierungsstelle verwenden, ist kein weiterer Schritt erforderlich, da Clientcomputer diesen Zertifikatausstellern vertrauen. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie den öffentlichen Teil des selbstsignierten Zertifikats im vertrauenswürdigen Zertifikatspeicher auf den Clientcomputern installieren.
> 
> 

<br>

## <a name="related-content"></a>Verwandte Inhalte
* [Verwalten einer durch die Azure AD Domain Services verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)




<!--HONumber=Jan17_HO1-->


