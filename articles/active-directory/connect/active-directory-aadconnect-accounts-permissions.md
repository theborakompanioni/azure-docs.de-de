---
title: Azure AD Connect-Konten und -Berechtigungen | Microsoft-Dokumentation
description: Dieses Thema beschreibt die verwendeten und erstellten Konten sowie die erforderlichen Berechtigungen.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 4ef0118435020edc3a922c88a5a55400992cbc09
ms.lasthandoff: 04/07/2017


---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Konten und Berechtigungen
Der Azure AD Connect-Installations-Assistent bietet zwei verschiedene Methoden:

* In den Express-Einstellungen benötigt der Assistent umfangreichere Berechtigungen zum einfachen Einrichten der Konfiguration, damit Sie weder Benutzer erstellen noch Berechtigungen separat konfigurieren müssen.
* In den benutzerdefinierten Einstellungen bietet der Assistent mehr Auswahlmöglichkeiten und Optionen an. Es gibt aber einige Situationen, in denen Sie sicherstellen müssen, dass Sie selbst über die geeigneten Berechtigungen verfügen.

## <a name="related-documentation"></a>verwandten Dokumentation
Wenn Sie die Dokumentation zum [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory-aadconnect.md) nicht gelesen haben, finden Sie in der folgenden Tabelle Links zu verwandten Themen:

|Thema |Link|  
| --- | --- |
|Azure AD Connect herunterladen | [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Installieren mit den Express-Einstellungen | [Expressinstallation von Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Installieren mit benutzerdefinierten Einstellungen | [Benutzerdefinierte Installation von Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Upgrade von DirSync | [Upgrade von Azure AD-Synchronisierungstools (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Nach der Installation | [Überprüfen der Installation und Zuweisen von Lizenzen ](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Installation mit Express-Einstellungen
In den Express-Einstellungen fordert der Installations-Assistent Sie zur Angabe von Anmeldeinformationen eines AD DS-Unternehmensadministrators auf, damit Ihr lokales Active Directory-System mit den erforderlichen Berechtigungen für Azure AD Connect konfiguriert werden kann. Bei einem Upgrade von DirSync werden die Anmeldeinformationen des AD DS-Unternehmensadministrators verwendet, um das Kennwort für das von DirSync verwendete Konto zurückzusetzen. Sie müssen außerdem über die Anmeldeinformationen eines globalen Azure AD-Administrators verfügen.

| Seite des Assistenten | Erfasste Anmeldeinformationen | Erforderliche Berechtigungen | Verwendung |
| --- | --- | --- | --- |
| N/V |Benutzer, der den Installations-Assistenten ausführt |Administrator des lokalen Servers |<li>Erstellt das lokale Konto, das als [Dienstkonto für das Synchronisierungsmodul](#azure-ad-connect-sync-service-account)verwendet wird. |
| Mit Azure AD verbinden |Azure AD-Verzeichnisanmeldeinformationen |Globale Administratorrolle in Azure AD |<li>Aktivieren der Synchronisierung im Azure AD-Verzeichnis</li>  <li>Erstellen des [Azure AD-Kontos](#azure-ad-service-account), das für die fortlaufenden Synchronisierungsvorgänge in Azure AD verwendet wird</li> |
| Herstellen einer Verbindung mit AD DS |Lokale Active Directory-Anmeldeinformationen |Mitglied der  Gruppe „Unternehmensadministratoren“ in Active Directory |<li>Erstellt ein [Konto](#active-directory-account) in Active Directory und gewährt Zugriff darauf. Dieses erstellte Konto dient zum Lesen und Schreiben von Verzeichnisinformationen während der Synchronisierung.</li> |

### <a name="enterprise-admin-credentials"></a>Enterprise-Administratoranmeldeinfos
Diese Anmeldeinformationen werden während der Installation verwendet, nach Abschluss der Installation jedoch nicht mehr. Enterprise-Administratoren und nicht Domänenadministratoren sollten sicherstellen, dass die Berechtigungen in Active Directory in allen Domänen festgelegt werden können.

### <a name="global-admin-credentials"></a>Globale Administratoranmeldeinfos
Diese Anmeldeinformationen werden während der Installation verwendet, nach Abschluss der Installation jedoch nicht mehr. Sie dienen zum Erstellen des [Azure AD-Kontos](#azure-ad-service-account) zum Synchronisieren der Änderungen mit Azure AD. Das Konto aktiviert auch die Synchronisierung als Feature in Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Berechtigungen für das erstellte AD DS-Konto für Express-Einstellungen
Das [Konto](#active-directory-account) , das für Lese- und Schreibvorgänge in AD DS erstellt wurde, besitzt die folgenden Berechtigungen, wenn es mit den Express-Einstellungen erstellt wird:

| Berechtigung | Verwendung |
| --- | --- |
| <li>Verzeichnisänderungen replizieren</li><li>Verzeichnisänderungen replizieren: Alle |Kennwortsynchronisierung |
| Alle Eigenschaften lesen/schreiben: Benutzer |Importieren und Exchange-Hybridbereitstellung |
| Alle Eigenschaften lesen/schreiben: iNetOrgPerson |Importieren und Exchange-Hybridbereitstellung |
| Alle Eigenschaften lesen/schreiben: Gruppe |Importieren und Exchange-Hybridbereitstellung |
| Alle Eigenschaften lesen/schreiben: Kontakt |Importieren und Exchange-Hybridbereitstellung |
| Zurücksetzen des Kennworts |Vorbereitung für das Aktivieren des Rückschreibens von Kennwörtern |

## <a name="custom-settings-installation"></a>Installation mit benutzerdefinierten Einstellungen
Wenn Sie benutzerdefinierte Einstellungen verwenden, muss das Konto für die Verbindung mit Active Directory vor der Installation erstellt werden. Die Berechtigungen, die Sie diesem Konto erteilen müssen, finden Sie unter [Erstellen des AD DS-Kontos](#create-the-ad-ds-account).

| Seite des Assistenten | Erfasste Anmeldeinformationen | Erforderliche Berechtigungen | Verwendung |
| --- | --- | --- | --- |
| N/V |Benutzer, der den Installations-Assistenten ausführt |<li>Administrator des lokalen Servers</li><li>Bei Verwendung einer vollständigen SQL Server-Instanz muss der Benutzer Systemadministrator (SA) in SQL sein.</li> |Erstellt standardmäßig das lokale Konto, das als [Dienstkonto für das Synchronisierungsmodul](#azure-ad-connect-sync-service-account)verwendet wird. Das Konto wird nur erstellt, wenn der Administrator kein bestimmtes Konto angibt. |
| „Synchronisierungsdienste installieren“, Option „Dienstkonto“ |Anmeldeinformationen für Active Directory- oder lokale Konten |Benutzerberechtigungen werden vom Installations-Assistenten gewährt |Wenn der Administrator ein Konto angibt, wird dieses Konto als Dienstkonto für den Synchronisierungsdienst verwendet. |
| Mit Azure AD verbinden |Azure AD-Verzeichnisanmeldeinformationen |Globale Administratorrolle in Azure AD |<li>Aktivieren der Synchronisierung im Azure AD-Verzeichnis</li>  <li>Erstellen des [Azure AD-Kontos](#azure-ad-service-account), das für die fortlaufenden Synchronisierungsvorgänge in Azure AD verwendet wird</li> |
| Verzeichnisse verbinden |Lokale Active Directory-Anmeldeinformationen für jede Gesamtstruktur, die mit Azure AD verbunden wird |Die Berechtigungen hängen davon ab, welche Funktionen Sie aktivieren, und sind unter [Create the AD DS account](#create-the-ad-ds-account) |Dieses Konto dient zum Lesen und Schreiben von Verzeichnisinformationen während der Synchronisierung. |
| AD FS-Server |Für jeden Server in der Liste sammelt der Assistent Anmeldeinformationen, wenn die Anmeldeinformationen des Benutzers, der den Assistenten ausführt, nicht für die Verbindung ausreichen. |Domänenadministrator |Installieren und Konfigurieren der AD FS-Server-Rolle. |
| Webanwendungsproxy-Server |Für jeden Server in der Liste sammelt der Assistent Anmeldeinformationen, wenn die Anmeldeinformationen des Benutzers, der den Assistenten ausführt, nicht für die Verbindung ausreichen. |Lokaler Administrator auf dem Zielcomputer |Installieren und Konfigurieren der WAP-Server-Rolle. |
| Anmeldeinformationen der Proxyvertrauensstellung |Anmeldeinformationen der Verbunddienstvertrauensstellung (die Anmeldeinformationen, die der Proxy zur Registrierung für ein Zertifikat für die Vertrauensstellung vom FS verwendet) |Domänenkonto, dessen Benutzer ein lokaler Administrator des AD FS-Servers ist |Erste Registrierung des vertrauenswürdigen FS-WAP-Zertifikats. |
| Seite „AD FS-Dienstkonto“, Option „Domänenbenutzerkonto verwenden“ |Anmeldeinformationen für das Active Directory-Benutzerkonto |Domänenbenutzer |Das AD-Benutzerkonto, dessen Anmeldeinformationen bereitgestellt wurden, wird als das Anmeldekonto des AD FS-Diensts verwendet. |

### <a name="create-the-ad-ds-account"></a>Erstellen des AD DS-Kontos
Bei der Installation von Azure AD Connect muss das auf der Seite **Verzeichnisse verbinden** angegebene Konto in Active Directory vorhanden sein und über die erforderlichen Berechtigungen verfügen. Der Installations-Assistent führt keine Überprüfung der Berechtigungen durch, mögliche Probleme werden erst während der Synchronisierung ermittelt.

Welche Berechtigungen Sie benötigen, hängt von den aktivierten optionalen Funktionen ab. Wenn Sie über mehrere Domänen verfügen, müssen die Berechtigungen für alle Domänen in der Gesamtstruktur erteilt werden. Wenn Sie keine dieser Features aktivieren, sind die **Domänenbenutzer** -Standardberechtigungen ausreichend.

| Feature | Berechtigungen |
| --- | --- |
| Kennwortsynchronisierung |<li>Verzeichnisänderungen replizieren</li>  <li>Verzeichnisänderungen replizieren: Alle |
| Exchange-Hybridbereitstellung |Schreibberechtigungen für die Attribute, die in [Exchange-Hybridrückschreiben](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) für Benutzer, Gruppen und Kontakte dokumentiert sind |
| Rückschreiben von Kennwörtern |Schreibberechtigungen für die Attribute, die in [Erste Schritte mit der Kennwortverwaltung](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) für Benutzer dokumentiert sind |
| Geräterückschreiben |Berechtigungen, die mit einem PowerShell-Skript erteilt wurden, wie unter [Geräterückschreiben](active-directory-aadconnect-feature-device-writeback.md)beschrieben |
| Gruppenrückschreiben |Lesen, Erstellen, Aktualisieren und Löschen von Gruppenobjekten in der Organisationseinheit, in der sich die Verteilergruppen befinden sollen. |

## <a name="upgrade"></a>Upgrade
Wenn Sie Azure AD Connect auf eine höhere Version aktualisieren, benötigen Sie folgende Berechtigungen:

| Prinzipal | Erforderliche Berechtigungen | Verwendung |
| --- | --- | --- |
| Benutzer, der den Installations-Assistenten ausführt |Administrator des lokalen Servers |Aktualisieren von Binärdateien. |
| Benutzer, der den Installations-Assistenten ausführt |Mitglied von ADSyncAdmins |Vornehmen von Änderungen an den Synchronisierungsregeln und anderen Konfigurationen. |
| Benutzer, der den Installations-Assistenten ausführt |Bei Verwendung einer SQL Server-Instanz mit vollem Funktionsumfang: DBO (oder ähnlich) der Datenbank für das Synchronisierungsmodul |Vornehmen von Änderungen auf Datenbankebene, z. B. Aktualisieren von Tabellen mit neuen Spalten. |

## <a name="more-about-the-created-accounts"></a>Weitere Informationen zu den erstellten Konten
### <a name="active-directory-account"></a>Active Directory-Konto
Wenn Sie Express-Einstellungen verwenden, wird ein Konto für die Synchronisierung in Active Directory erstellt. Das erstellte Konto befindet sich in der Stammdomäne der Gesamtstruktur im Benutzercontainer, und sein Name ist mit dem Präfix **MSOL_** versehen. Das Konto wird mit einem langen, komplexen Kennwort erstellt, das nicht abläuft. Wenn Sie über eine Kennwortrichtlinie in der Domäne verfügen, stellen Sie sicher, dass lange und komplexe Kennwörter für dieses Konto zulässig sind.

![AD-Konto](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Wenn Sie benutzerdefinierte Einstellungen verwenden, sind Sie für das Erstellen des Kontos vor Beginn der Installation zuständig.

### <a name="azure-ad-connect-sync-service-account"></a>Azure AD Connect-Synchronisierungsdienstkonto
Der Synchronisierungsdienst kann unter verschiedenen Konten ausgeführt werden. Er kann unter einem **virtuellen Dienstkonto** (VSA), einem **gruppenverwalteten Dienstkonto** (gMSA/sMSA) oder einem normalen Benutzerkonto ausgeführt werden. Die unterstützten Optionen wurden mit der Connect-Version von April 2017 geändert und treten in Kraft, wenn Sie eine Neuinstallation durchführen. Wenn Sie ein Upgrade von einer früheren Version von Azure AD Connect durchführen, sind diese zusätzlichen Optionen nicht verfügbar.

| Kontotyp | Installationsoption | Beschreibung |
| --- | --- | --- |
| [Virtuelles Dienstkonto](#virtual-service-account) | Express und benutzerdefiniert, April 2017 und höher | Dies ist die Option für alle Expressinstallationen mit Ausnahme von Installationen auf einem Domänencontroller. Für benutzerdefinierte Installationen ist es die Standardoption, sofern keine andere Option verwendet wird. |
| [Gruppenverwaltetes Dienstkonto](#group-managed-service-account) | Benutzerdefiniert, April 2017 und höher | Wenn Sie einen Remotecomputer mit SQL Server verwenden, empfehlen wir den Einsatz eines gruppenverwalteten Dienstkontos. |
| [Benutzerkonto](#user-account) | Express und benutzerdefiniert, April 2017 und höher | Ein Benutzerkonto mit dem Präfix „AAD_“ wird nur während der Installation erstellt, wenn diese unter Windows Server 2008 und auf einem Domänencontroller erfolgt. |
| [Benutzerkonto](#user-account) | Express und benutzerdefiniert, März 2017 und früher | Ein lokales Konto mit dem Präfix „AAD_“ wird während der Installation erstellt. Bei der benutzerdefinierten Installation kann ein anderes Konto angegeben werden. |

Wenn Sie Connect mit einem Build von März 2017 und früher verwenden, setzen Sie das Kennwort für das Dienstkonto nicht zurück, da Windows die Verschlüsselungsschlüssel aus Sicherheitsgründen zerstört. Sie können das Konto nicht in ein anderes Konto ändern, ohne Azure AD Connect neu zu installieren. Wenn Sie ein Upgrade auf einen Build von April 2017 oder höher durchführen, wird das Ändern des Kennworts für das Dienstkonto unterstützt. Sie können jedoch nicht das verwendete Konto ändern.

> [!Important]
> Sie können das Dienstkonto nur bei der erstmaligen Installation festlegen. Das Ändern des Dienstkontos nach Abschluss der Installation wird nicht unterstützt.

Hier sehen Sie eine Tabelle mit den Standard-, den empfohlenen und den unterstützten Optionen für das Synchronisierungsdienstkonto.

Legende:

- **Fettformatierung** kennzeichnet die Standardoption, die in den meisten Fällen der empfohlenen Option entspricht.
- *Kursivformatierung* kennzeichnet die empfohlene Option, sofern diese nicht mit der Standardoption übereinstimmt.
- 2008: Standardoption bei der Installation unter Windows Server 2008
- Nicht fett formatiert: unterstützte Option
- Lokales Konto: lokales Benutzerkonto auf dem Server
- Domänenkonto: Domänenbenutzerkonto
- sMSA: [eigenständig verwaltetes Dienstkonto](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA: [gruppenverwaltetes Dienstkonto](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>Benutzerdefiniert | Remote-SQL</br>Benutzerdefiniert |
| --- | --- | --- | --- |
| **Eigenständiger/Arbeitsgruppencomputer** | Nicht unterstützt | **VSA**</br>Lokales Konto (2008)</br>Lokales Konto |  Nicht unterstützt |
| **In die Domäne eingebundener Computer** | **VSA**</br>Lokales Konto (2008) | **VSA**</br>Lokales Konto (2008)</br>Lokales Konto</br>Domänenkonto</br>sMSA, gMSA | **gMSA**</br>Domänenkonto |
| **Domänencontroller** | **Domänenkonto** | *gMSA*</br>**Domänenkonto**</br>sMSA| *gMSA*</br>**Domänenkonto**|

#### <a name="virtual-service-account"></a>Virtuelles Dienstkonto
Ein virtuelles Dienstkonto ist ein besonderer Kontotyp, der nicht über ein Kennwort verfügt und von Windows verwaltet wird.

![VSA](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

Das VSA ist für den Einsatz in Szenarien vorgesehen, in denen das Synchronisierungsmodul und SQL sich auf demselben Server befinden. Wenn Sie einen SQL-Remotecomputer verwenden, empfehlen wir stattdessen den Einsatz eines [gruppenverwalteten Dienstkontos](#managed-service-account).

Für dieses Feature ist Windows Server 2008 R2 oder höher erforderlich. Wenn Sie Azure AD Connect unter Windows Server 2008 installieren, wird bei der Installation automatisch ein [Benutzerkonto](#user-account) verwendet.

#### <a name="group-managed-service-account"></a>Gruppenverwaltetes Dienstkonto
Wenn Sie einen Remotecomputer mit SQL Server verwenden, empfehlen wir den Einsatz eines **gruppenverwalteten Dienstkontos**. Weitere Informationen zum Vorbereiten von Active Directory für das gruppenverwaltete Benutzerkonto finden Sie unter [Gruppenverwaltete Dienstkonten: Übersicht](https://technet.microsoft.com/library/hh831782.aspx).

Um diese Option zu verwenden, wählen Sie auf der Seite [Erforderliche Komponenten installieren](active-directory-aadconnect-get-started-custom.md#install-required-components) die Optionen **Vorhandenes Dienstkonto verwenden** und **Verwaltetes Dienstkonto**.  
![VSA](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Die Verwendung eines [eigenständig verwalteten Dienstkontos](https://technet.microsoft.com/library/dd548356.aspx) wird ebenfalls unterstützt. Da diese Konten jedoch nur auf dem lokalen Computer verwendet werden können, gibt es keinen praktischen Vorteil gegenüber dem virtuellen Standarddienstkonto.

Für dieses Feature ist Windows Server 2012 oder höher erforderlich. Wenn Sie ein älteres Betriebssystem und Remote-SQL einsetzen, müssen Sie ein [Benutzerkonto](#user-account) verwenden.

#### <a name="user-account"></a>Benutzerkonto
Der Installations-Assistent erstellt ein lokales Dienstkonto (sofern das Konto nicht zur Verwendung in benutzerdefinierten Einstellungen angegeben wird). Das Konto ist mit dem Präfix **AAD_** versehen und wird zur Ausführung des eigentlichen Synchronisierungsdiensts verwendet. Wenn Sie Azure AD Connect auf einem Domänencontroller installieren, wird das Konto in der Domäne erstellt. Wenn Sie einen Remoteserver mit SQL Server oder einen Proxy verwenden, für den eine Authentifizierung erforderlich ist, muss das Dienstkonto **AAD_** in der Domäne vorhanden sein.

![Synchronisierungsdienstkonto](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Das Konto wird mit einem langen, komplexen Kennwort erstellt, das nicht abläuft.

Dieses Konto wird verwendet, um die Kennwörter für die anderen Konten auf sichere Weise zu speichern. Die Kennwörter dieser Konten sind verschlüsselt in der Datenbank gespeichert. Die privaten Schlüssel für die Verschlüsselungsschlüssel sind mit der Verschlüsselung mit geheimem Schlüssel für kryptografische Dienste mithilfe der Windows-Datenschutz-API (DPAPI) geschützt.

Bei Verwendung einer SQL Server-Instanz mit vollem Funktionsumfang wird das Dienstkonto zum DBO der Datenbank, die für das Synchronisierungsmodul erstellt wurde. Der Dienst funktioniert mit anderen Berechtigungen nicht wie vorgesehen. Darüber hinaus wird eine SQL-Anmeldung erstellt.

Das Konto erhält auch Berechtigungen für Dateien, Registrierungsschlüssel und andere Objekte im Zusammenhang mit dem Synchronisierungsmodul.

### <a name="azure-ad-service-account"></a>Azure AD-Dienstkonto
Zur Verwendung durch den Synchronisierungsdienst wird ein Konto in Azure AD erstellt. Dieses Konto kann anhand des Anzeigenamens identifiziert werden.

![AD-Konto](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Der Name des Servers, auf dem das Konto verwendet wird, kann im zweiten Teil des Benutzernamens identifiziert werden. In der Abbildung oben heißt der Server „FABRIKAMCON“. Wenn Sie über Stagingserver verfügen, erhält jeder Server ein eigenes Konto. Es besteht eine Beschränkung auf 10 Synchronisierungsdienstkonten in Azure AD.

Das Dienstkonto wird mit einem langen, komplexen Kennwort erstellt, das nicht abläuft. Diesem wird eine besondere Rolle **Konten für die Verzeichnissynchronisierungsaufgaben** zugewiesen, die nur über Berechtigungen zur Ausführung von Verzeichnissynchronisierungsaufgaben verfügt. Diese besondere integrierte Rolle kann nicht außerhalb des Azure AD Connect-Assistenten gewährt werden, und das Azure-Portal zeigt dieses Konto nur mit der Rolle **Benutzer**an.

![AD-Kontenrolle](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](../active-directory-aadconnect.md).

