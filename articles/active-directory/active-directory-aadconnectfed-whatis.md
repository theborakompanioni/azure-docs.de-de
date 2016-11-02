<properties
    pageTitle="Azure AD Connect und Verbund | Microsoft Azure"
    description="Diese Seite ist der zentrale Speicherort für die gesamte Dokumentation zu AD FS-Vorgängen mit Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/14/2016"
    ms.author="anandy"/>



# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect und Verbund

Mit Azure AD Connect können Sie einen Verbund mit dem lokalen AD FS und Azure AD konfigurieren. Mit der Verbundanmeldung können sich Benutzer bei Azure AD-basierten Dienste mit ihren lokalen Kennwörtern anmelden. Während sie in ihrem Unternehmensnetzwerk angemeldet sind, können sie auf Cloudressourcen zugreifen, ohne ihre Kennwörter erneut eingeben zu müssen. Mit der Verbundoption für AD FS können Sie eine neue Windows Server 2012 R2-Farm bereitstellen oder eine Farm angeben.

Dieses Thema ist das zentrale Thema zu verbundbezogenen Funktionen für Azure AD Connect und enthält eine Liste mit Links zu allen anderen verwandten Themen. Links zu Azure AD Connect finden Sie unter "Integrieren Ihrer lokalen Identitäten in Azure Active Directory".

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect – Verbundthemen

| Thema | Inhalt und Relevanz |
|:------|:-----------|
| **Azure AD Connect-Optionen für die Benutzeranmeldung** ||
| [Grundlegendes zu Benutzeranmeldeoptionen](active-directory-aadconnect-user-signin.md) | Beschreibung der verschiedenen Benutzeranmeldeoptionen und ihres Einflusses auf die Benutzerfreundlichkeit der Azure-Anmeldung. |
| **AD FS-Installation mit Azure AD Connect**||
| [Voraussetzungen](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Erforderliche Komponenten für eine erfolgreiche Installation von AD FS über Azure AD Connect|
| [Konfigurieren einer AD FS-Farm](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Installieren einer neuen AD FS-Farm mit Azure AD Connect |
| **Ändern der AD FS-Konfiguration** | |
| [Reparieren der Vertrauensstellung](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Reparieren der aktuellen Vertrauensstellung zwischen lokalem AD FS und Office 365/Azure |
| [Hinzufügen eines neuen AD FS-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Erweitern einer AD FS-Farm mit einer zusätzlichen AD FS-Serverinstallation nach der Erstinstallation |
| [Hinzufügen eines neuen AD FS-WAP-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Erweitern einer AD FS-Farm mit einer zusätzlichen WAP-Serverinstallation nach der Erstinstallation |
| [Hinzufügen einer neuen Verbunddomäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Hinzufügen einer weiteren Domäne für den Verbund mit Azure AD |
|**Aufgaben nach der Installation**||
| [Benutzerdefiniertes Firmenlogo/benutzerdefinierte Abbildung hinzufügen](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Ändern der Anmeldeerfahrung durch Angeben des benutzerdefinierten Logos, das auf der AD FS-Anmeldeseite angezeigt wird |
| [Anmeldebeschreibung hinzufügen](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Ändern der Anmeldebeschreibung auf der AD FS-Anmeldeseite | 
| [Ändern von AD FS-Anspruchsregeln](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Ändern/Hinzufügen von Anspruchsregeln in AD FS entsprechend der Azure AD Connect-Synchronisierungskonfiguration |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)




<!--HONumber=Oct16_HO2-->


