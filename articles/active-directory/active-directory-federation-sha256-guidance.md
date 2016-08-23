<properties
	pageTitle="Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für O365 | Microsoft Azure"
	description="Diese Seite bietet eine Anleitung zum Ändern des SHA-Algorithmus für die Verbundvertrauensstellung mit O365."
    keywords="SHA1,SHA256,O365,federation,aadconnect,adfs,ad fs,change sha,federation trust,relying party trust"
	services="active-directory"
	documentationCenter=""
	authors="anandyadavmsft"
	manager="samueld"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="anandy"/>

#Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite für O365

##Übersicht

Die AD FS signieren ihre Token in Azure Active Directory, um sicherzustellen, dass diese nicht manipuliert werden können. Diese Signatur kann auf SHA-1 oder SHA-256 basieren. Microsoft Azure Active Directory unterstützt jetzt Token, die mit einem SHA-256-Algorithmus signiert wurden, und empfiehlt, den Signaturalgorithmus für Token auf SHA-256 festzulegen, um ein Höchstmaß an Sicherheit zu erzielen. Dieser Artikel erläutert, wie Sie den Tokensignaturalgorithmus auf das sicherere Level SHA-256 festlegen.

##Ändern des Tokensignaturalgorithmus

Sobald Sie den Signaturalgorithmus gemäß den unten stehenden Schritten festgelegt haben, beginnt AD FS damit, die Token für die Vertrauensstellung der vertrauenden Seite für O365 mit SHA-256 zu signieren. Weitere Konfigurationsänderungen sind nicht erforderlich, und diese Änderung hat keinerlei Auswirkungen auf die Endbenutzer, die auf Office 365 oder andere Azure AD-Anwendungen zugreifen.

###Verwenden der Verwaltungskonsole

* Öffnen Sie die AD FS-Verwaltungskonsole auf dem primären AD FS-Server.
* Erweitern Sie den AD FS-Knoten, und klicken Sie auf „Vertrauensstellungen der vertrauenden Seite“.
* Klicken Sie mit der rechten Maustaste auf Ihre Vertrauensstellung der vertrauenden Seite für O365/Azure, und wählen Sie „Eigenschaften“ aus.
* Wählen Sie die Registerkarte „Erweitert“ aus, und wählen Sie „SHA-256“ als sicheren Hashalgorithmus.
* Klicken Sie auf „OK“.

![SHA256-Signaturalgorithmus – MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

###Verwenden von AD FS PowerShell-Cmdlets

* Öffnen Sie PowerShell mit Administratorrechten auf einem beliebigen AD FS-Server.
* Legen Sie den sicheren Hashalgorithmus mithilfe des Cmdlets Set-AdfsRelyingPartyTrust fest.

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

##Weitere Informationen

* [Reparieren der O365-Vertrauensstellung mit AAD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)

<!---HONumber=AcomDC_0810_2016-->