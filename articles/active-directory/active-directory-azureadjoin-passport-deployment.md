---
title: Aktivieren von Microsoft Windows Hello for Business in Ihrer Organisation | Microsoft Docs
description: Bereitstellungsanweisungen zum Aktivieren von Microsoft Passport in Ihrer Organisation.
services: active-directory
documentationcenter: 
keywords: Konfigurieren von Microsoft Passport, Bereitstellung von Microsoft Windows Hello for Business
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 6f08fcdb4ec6dd124760cff54c522d998d5a0327
ms.lasthandoff: 12/28/2016


---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Aktivieren von Windows Hello for Business in Ihrer Organisation
Nach der [Verknüpfung von in die Domäne eingebundenen Windows 10-Geräten mit Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md) gehen Sie wie folgt vor, um Windows Hello for Business in Ihrer Organisation zu aktivieren:

1. Bereitstellen von System Center Configuration Manager  
2. Konfigurieren von Richtlinieneinstellungen
3. Konfigurieren des Zertifikatprofils  

## <a name="deploy-system-center-configuration-manager"></a>Bereitstellen von System Center Configuration Manager
Für das Bereitstellen von Benutzerzertifikaten anhand von Windows Hello for Business-Schüsseln sind folgende Elemente erforderlich:

* **System Center Configuration Manager Current Branch** – Sie müssen die Version 1606 oder eine höhere Version installieren. Weitere Informationen finden Sie in der [Dokumentation zu System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) und im [System Center Configuration Manager-Teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Public Key-Infrastruktur (PKI)**: Für das Aktivieren von Microsoft Windows Hello for Business mit Benutzerzertifikaten ist eine PKI erforderlich. Falls diese nicht vorhanden ist oder nicht für Benutzerzertifikate verwendet werden soll, kann ein neuer Domänencontroller bereitgestellt werden, auf dem Windows Server 2016 (Build 10551 oder höher) installiert ist. Führen Sie die Schritte zum [Installieren eines Replikatdomänencontrollers in einer vorhandenen Domäne](https://technet.microsoft.com/library/jj574134.aspx) bzw. zum [Installieren einer neuen Active Directory-Gesamtstruktur beim Erstellen einer neuen Umgebung](https://technet.microsoft.com/library/jj574166) aus. (Die ISO-Dateien stehen unter [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true) zum Download bereit.)

## <a name="configure-policy-settings"></a>Konfigurieren von Richtlinieneinstellungen
Sie haben zwei Optionen, um die Richtlinieneinstellungen für Microsoft Windows Hello for Business zu konfigurieren:

* Gruppenrichtlinie in Active Directory 
* System Center Configuration Manager 

Das Verwenden von Gruppenrichtlinien in Active Directory ist die empfohlene Methode zum Konfigurieren von Richtlinieneinstellungen für Microsoft Windows Hello for Business. 

Das Verwenden von System Center Configuration Manager ist die bevorzugte Methode, wenn sie auch zum Bereitstellen von Zertifikaten verwendet werden sollen. Dieses Szenario:

* Gewährleistet die Kompatibilität mit den neueren Bereitstellungsszenarien
* Erfordert auf der Clientseite Windows 10 Version 1607 oder höher.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfigurieren von Microsoft Windows Hello for Business über Gruppenrichtlinien in Active Directory
**Schritte**:

1. Öffnen Sie den Server-Manager, und navigieren Sie zu **Tools** > **Gruppenrichtlinienverwaltung**.
2. Navigieren Sie von der Gruppenrichtlinienverwaltung zu dem Domänenknoten, der der Domäne entspricht, in der Sie Azure AD Join aktivieren möchten.
3. Klicken Sie mit der rechten Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie dann **Neu** aus. Geben Sie Ihrem Gruppenrichtlinienobjekt einen Namen, z.B. „Windows Hello for Business aktivieren“. Klicken Sie auf **OK**.
4. Klicken Sie mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten**aus.
5. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Windows Hello for Business**.
6. Klicken Sie mit der rechten Maustaste auf **Windows Hello for Business aktivieren**, und wählen Sie dann **Bearbeiten** aus.
7. Aktivieren Sie das Optionsfeld **Aktiviert**, und klicken Sie dann auf **Übernehmen**. Klicken Sie auf **OK**.
8. Sie können das Gruppenrichtlinienobjekt jetzt mit einem Speicherort Ihrer Wahl verknüpfen. Um diese Richtlinie für alle in die Domäne eingebundenen Windows 10-Geräte in Ihrer Organisation zu aktivieren, verknüpfen Sie die Gruppenrichtlinie mit der Domäne. Beispiel:
   * Eine bestimmte Organisationseinheit in Active Directory, in der in die Domäne eingebundene Windows 10-Computer platziert werden
   * Eine bestimmte Sicherheitsgruppe mit in die Domäne eingebundenen Windows 10-Computern, die automatisch bei Azure AD registriert werden

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Konfigurieren von Windows Hello for Business mithilfe von System Center Configuration Manager
**Schritte**:

1. Öffnen Sie den **System Center Configuration Manager**, und navigieren Sie zu **Assets und Kompatibilität > Kompatibilitätseinstellungen > Zugriff auf Unternehmensressourcen > Windows Hello for Business-Profil**.
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. Klicken Sie auf der Symbolleiste oben auf **Windows Hello for Business-Profil erstellen**.
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. Führen Sie im Dialogfeld **Allgemein** die folgenden Schritte aus:
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. Geben Sie im Textfeld **Name** einen Namen für Ihr Profil ein, z. B. **Mein WHfB-Profil**.
   
    b. Klicken Sie auf **Weiter**.
4. Wählen Sie im Dialogfeld **Unterstützte Plattformen** die Plattformen aus, die mit diesem Windows Hello for Business-Profil bereitgestellt werden, und klicken Sie auf **Weiter**.
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. Führen Sie im Dialogfeld **Einstellungen** die folgenden Schritte aus:
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Wählen Sie für **Windows Hello for Business konfigurieren** die Einstellung **Aktiviert** aus.
   
    b. Legen Sie **Trusted Platform Module (TPM) verwenden** auf **Erforderlich** fest. 
   
    c. Wählen Sie als **Authentifizierungsmethode **die Einstellung **Zertifikatbasiert** aus.
   
    d. Klicken Sie auf **Weiter**.
6. Klicken Sie im Dialogfeld **Zusammenfassung** auf **Weiter**.
7. Klicken Sie im Dialogfeld **Abschluss** auf **Schließen**.
8. Klicken Sie auf der Symbolleiste oben auf **Bereitstellen**.
   
    ![Konfigurieren von Windows Hello for Business](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a>Konfigurieren des Zertifikatprofils
Wenn Sie die zertifikatbasierte Authentifizierung für die lokale Authentifizierung verwenden, müssen Sie ein Zertifikatprofil konfigurieren und bereitstellen. Hierzu müssen Sie einen NDES-Server und die Standortrolle „Zertifikatregistrierungspunkt“ im System Center Configuration Manager einrichten. Ausführliche Informationen finden Sie unter [Voraussetzungen für Zertifikatprofile im Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).

1. Öffnen Sie den **System Center Configuration Manager**, und navigieren Sie zu **Assets und Kompatibilität > Kompatibilitätseinstellungen > Zugriff auf Unternehmensressourcen > Zertifikatprofile**.
2. Wählen Sie eine Vorlage mit Smartcard-Anmeldung und erweiterter Schlüsselverwendung (Extended Key Usage, EKU) aus.

Wählen Sie auf der Seite **SCEP-Registrierung** für das Zertifikatprofil als **Schlüsselspeicheranbieter** die Option **In Passport for Work installieren, andernfalls Fehler** aus.

## <a name="next-steps"></a>Nächste Schritte
* [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Weitere Informationen zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Benutzererfahrungen beim Verknüpfen von in die Domäne eingebundenen Windows 10-Geräten mit Azure AD](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)


