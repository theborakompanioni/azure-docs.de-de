---
title: Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect | Microsoft Docs
description: "Enthält eine Beschreibung der AD FS-Verwaltung mithilfe von Azure AD Connect und Anpassung der AD FS-Anmeldung für Benutzer mit Azure AD Connect und PowerShell."
keywords: AD FS, ADFS, AD FS Verwaltung, AAD Connect, Connect, anmelden, AD FS Anpassung, Vertrauensstellung reparieren, O365, Verbund, vertrauende Seite
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: da3b8a375d380a0e2f1c2e243a475f2f3ff94bfc
ms.lasthandoff: 03/07/2017


---
# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect
Dieser Artikel enthält Informationen zu den auf die Active Directory-Verbunddienste (AD FS) bezogenen Aufgaben, die mithilfe von Microsoft Azure Active Directory Connect ausgeführt werden können, sowie zu anderen AD FS-Aufgaben, die möglicherweise für die vollständige Konfiguration einer AD FS-Farm erforderlich sind.

| Thema | Inhalt |
|:--- |:--- |
| **AD FS-Verwaltung** | |
| [Reparieren der Vertrauensstellung](#repairthetrust) |Reparieren der Verbundvertrauensstellung mit Office 365 |
| [Hinzufügen eines AD FS-Servers](#addadfsserver) |Erweitern der AD FS-Farm mit einem zusätzlichen AD FS-Server |
| [Hinzufügen eines AD FS-Webanwendungs-Proxyservers](#addwapserver) |Erweitern der AD FS-Farm mit einem zusätzlichen WAP-Server |
| [Hinzufügen einer Verbunddomäne](#addfeddomain) |Hinzufügen einer Verbunddomäne |
| [Aktualisieren des SSL-Zertifikats](active-directory-aadconnectfed-ssl-update.md)| Aktualisieren des SSL-Zertifikats für eine Active Directory-Verbunddienste-Farm (AD FS)|
| **AD FS-Anpassung** | |
| [Hinzufügen eines benutzerdefinierten Firmenlogos oder einer Abbildung](#customlogo) |Anpassen einer AD FS-Anmeldeseite mit einem Firmenlogo und einer Abbildung |
| [Hinzufügen einer Anmeldebeschreibung](#addsignindescription) |Hinzufügen einer Beschreibung für die Anmeldeseite |
| [Ändern von AD FS-Anspruchsregeln](#modclaims) |Ändern der AD FS-Ansprüche für verschiedene Verbundszenarien |

## <a name="ad-fs-management"></a>AD FS-Verwaltung
Azure AD Connect umfasst verschiedene AD FS-bezogene Aufgaben, die mithilfe des Azure AD Connect-Assistenten mit minimalem Benutzereingriff ausgeführt werden können. Nachdem Sie die Installation von Azure AD Connect durch Ausführen des Assistenten abgeschlossen haben, können Sie den Assistenten erneut ausführen, um zusätzliche Aufgaben durchzuführen.

## Reparieren der Vertrauensstellung <a name=repairthetrust></a>
Azure AD Connect kann den aktuellen Status der AD FS- und Azure Active Directory-Vertrauensstellung überprüfen und entsprechende Maßnahmen ergreifen, um die Vertrauensstellung zu reparieren. Führen Sie diese Schritte aus, um Ihre Azure AD- und AD FS-Vertrauensstellung zu reparieren.

1. Wählen Sie in der Liste mit den weiteren Aufgaben die Option **AAD- und ADFS-Vertrauensstellung reparieren** .
   ![AAD- und ADFS-Vertrauensstellung reparieren](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)
2. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen des globalen Administrators für Azure AD an, und klicken Sie auf **Weiter**.
   ![Mit Azure AD verbinden](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)
3. Geben Sie auf der Seite **Anmeldeinformationen für den Remotezugriff** die Anmeldeinformationen für den Domänenadministrator ein.
   ![Anmeldeinformationen für den Remotezugriff](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    Nachdem Sie auf **Weiter**geklickt haben, überprüft Azure AD Connect die Zertifikatintegrität und zeigt etwaige Probleme an.

    ![Status von Zertifikaten](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    Auf der Seite **Bereit zur Konfiguration** wird die Liste der Aktionen angezeigt, die ausgeführt werden, um die Vertrauensstellung zu reparieren.

    ![Bereit zur Konfiguration](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)
4. Klicken Sie auf **Installieren** , um die Vertrauensstellung zu reparieren.

> [!NOTE]
> Azure AD Connect kann nur selbstsignierte Zertifikate reparieren bzw. Maßnahmen dafür ergreifen. Azure AD Connect kann nicht zum Reparieren von Drittanbieterzertifikaten verwendet werden.
>
>

## Hinzufügen eines AD FS-Servers <a name=addadfsserver></a>

> [!NOTE]
> Für Azure AD Connect ist die PFX-Zertifikatdatei erforderlich, um einen AD FS-Server hinzuzufügen. Daher können Sie diesen Vorgang nur ausführen, wenn Sie die AD FS-Farm mit Azure AD Connect konfiguriert haben.
>
>

1. Wählen Sie **Weiteren Verbundserver bereitstellen**, und klicken Sie auf **Weiter**.
   ![Weiterer Verbundserver](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)
2. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen des globalen Administrators für Azure AD ein, und klicken Sie auf **Weiter**.
   ![Mit Azure AD verbinden](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)
3. Geben Sie die Anmeldeinformationen des Domänenadministrators an.
   ![Anmeldeinformationen des Domänenadministrators](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)
4. Sie werden von Azure AD Connect zum Eingeben des Kennworts für die PFX-Datei aufgefordert, die Sie beim Konfigurieren der neuen AD FS-Farm mit Azure AD Connect angegeben haben. Klicken Sie auf **Kennwort eingeben** , um das Kennwort für die PFX-Datei anzugeben.
   ![Zertifikatkennwort](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![SSL-Zertifikat angeben](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)
5. Geben Sie auf der Seite **AD FS-Server** den Servernamen oder die IP-Adresse ein, der bzw. die der AD FS-Farm hinzugefügt werden soll.
   ![AD FS-Server](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)
6. Klicken Sie auf **Weiter**, und führen Sie die Schritte auf der letzten Konfigurationsseite**** aus. Nachdem Azure AD Connect die Server der AD FS-Farm hinzugefügt hat, haben Sie die Möglichkeit, die Konnektivität zu überprüfen.
   ![Bereit zur Konfiguration](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Installation abgeschlossen](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## Hinzufügen eines AD FS-Webanwendungs-Proxyservers <a name=addwapserver></a>

> [!NOTE]
> Für Azure AD Connect ist die PFX-Zertifikatdatei erforderlich, um einen Webanwendungs-Proxyserver hinzuzufügen. Daher können Sie diesen Vorgang nur ausführen, wenn Sie die AD FS-Farm mit Azure AD Connect konfiguriert haben.
>
>

1. Wählen Sie in der Liste mit den verfügbaren Aufgaben die Option **Webanwendungsproxy bereitstellen** .
   ![Deploy web application proxy](media/active-directory-aadconnect-federation-management/WapServer1.PNG)
2. Geben Sie die Anmeldeinformationen des globalen Azure-Administrators an.
   ![Mit Azure AD verbinden](media/active-directory-aadconnect-federation-management/wapserver2.PNG)
3. Geben Sie auf der Seite **SSL-Zertifikat angeben** das Kennwort für die PFX-Datei an, die Sie beim Konfigurieren der AD FS-Farm mit Azure AD Connect angegeben haben.
   ![Zertifikatkennwort](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![SSL-Zertifikat angeben](media/active-directory-aadconnect-federation-management/WapServer4.PNG)
4. Fügen Sie den Server als Webanwendungs-Proxyserver hinzu. Da der Webanwendungs-Proxyserver nicht unbedingt der Domäne angehören muss, werden Sie vom Assistenten zur Eingabe von Administratoranmeldeinformationen für den hinzugefügten Server aufgefordert.
   ![Anmeldeinformationen für Verwaltungsserver](media/active-directory-aadconnect-federation-management/WapServer5.PNG)
5. Geben Sie auf der Seite **Anmeldeinformationen der Proxyvertrauensstellung** Administratoranmeldeinformationen an, um die Proxyvertrauensstellung zu konfigurieren und auf den primären Server in der AD FS-Farm zuzugreifen.
   ![Anmeldeinformationen der Proxyvertrauensstellung](media/active-directory-aadconnect-federation-management/WapServer6.PNG)
6. Auf der Seite **Bereit zur Konfiguration** zeigt der Assistent die Liste mit den Aktionen an, die ausgeführt werden.
   ![Bereit zur Konfiguration](media/active-directory-aadconnect-federation-management/WapServer7.PNG)
7. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen. Nach Abschluss der Konfiguration bietet der Assistent Ihnen die Möglichkeit, die Verbindung mit den Servern zu überprüfen. Klicken Sie auf **Überprüfen** , um die Konnektivität zu überprüfen.
   ![Installation abgeschlossen](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## Hinzufügen einer Verbunddomäne <a name=addfeddomain></a>

Das Hinzufügen einer Domäne, die einen Verbund mit Azure AD bilden soll, ist mithilfe von Azure AD Connect einfach. Azure AD Connect fügt die Domäne für den Verbund hinzu und ändert auch die Anspruchsregeln, damit der richtige Aussteller angezeigt wird, falls Sie über mehrere Domänen in einem Verbund mit Azure AD verfügen.

1. Zum Hinzufügen einer Verbunddomäne wählen Sie die Aufgabe **Weitere Azure AD-Domäne hinzufügen**.
   ![Weitere Azure AD-Domäne](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)
2. Geben Sie auf der nächsten Seite des Assistenten die Anmeldeinformationen des globalen Administrators für Azure AD an.
   ![Mit Azure AD verbinden](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)
3. Geben Sie auf der Seite **Anmeldeinformationen für den Remotezugriff** die Anmeldeinformationen des Domänenadministrators an.
   ![Anmeldeinformationen für den Remotezugriff](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)
4. Auf der nächsten Seite stellt der Assistent eine Liste mit den Azure AD-Domänen bereit, mit denen Sie für Ihr lokales Verzeichnis einen Verbund eingehen können. Wählen Sie die Domäne in der Liste aus.
   ![Azure AD-Domäne](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    Nach der Auswahl der Domäne erhalten Sie vom Assistenten die entsprechenden Informationen zu weiteren Aktionen, die er ausführt, sowie zur Auswirkung der Konfiguration. In einigen Fällen – wenn Sie eine Domäne auswählen, die noch nicht in Azure AD überprüft wurde – erhalten Sie vom Assistenten Informationen zur Überprüfung der Domäne. Ausführliche Informationen finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory-add-domain.md) .
5. Klicken Sie auf **Weiter**. Auf der Seite **Bereit zur Konfiguration** wird die Liste mit den Aktionen angezeigt, die von Azure AD Connect ausgeführt werden. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen.
   ![Bereit zur Konfiguration](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS-Anpassung
Die folgenden Abschnitte enthalten detaillierte Informationen zu einigen häufigen Aufgaben, die zum Anpassen der AD FS-Anmeldeseite erforderlich sind.

## Hinzufügen eines benutzerdefinierten Firmenlogos oder einer Abbildung <a name=customlogo></a>
Zum Ändern des auf der **Anmeldeseite** angezeigten Firmenlogos verwenden Sie das unten angegebene Windows PowerShell-Cmdlet und die entsprechende Syntax.

> [!NOTE]
> Die empfohlene Größe für das Logo beträgt 260 x 35 bei 96 DPI und einer Dateigröße von höchstens 10 KB.
>
>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> Der *TargetName* -Parameter ist erforderlich. Das mit AD FS veröffentlichte Standarddesign heißt „Standard“.
>
>

## Hinzufügen einer Anmeldebeschreibung <a name=addsignindescription></a>
Zum Hinzufügen einer Beschreibung für die **Anmeldeseite**verwenden Sie das folgende Windows PowerShell-Cmdlet und die folgende Syntax:

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## Ändern von AD FS-Anspruchsregeln <a name=modclaims></a>
AD FS unterstützt eine umfangreiche Sprache für Ansprüche, die Sie zum Erstellen von benutzerdefinierten Anspruchsregeln verwenden können. Weitere Informationen finden Sie unter [Rolle der Anspruchsregelsprache](https://technet.microsoft.com/library/dd807118.aspx).

In den folgenden Abschnitten wird beschrieben, wie Sie benutzerdefinierte Regeln für einige Szenarien in Zusammenhang mit dem Azure AD- und AD FS-Verbund schreiben können.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Unveränderliche ID hängt von einem Wert im Attribut ab
Azure AD Connect ermöglicht Ihnen das Festlegen eines Attributs, das als Quellanker verwendet wird, wenn Objekte mit Azure AD synchronisiert werden. Wenn der Wert im benutzerdefinierten Attribut nicht leer ist, kann es ratsam sein, einen Anspruch mit unveränderlicher ID ausgeben. Beispielsweise können Sie **ms-ds-consistencyguid** als Attribut für den Quellanker auswählen und **ImmutableID** als **ms-ds-consistencyguid** ausgeben, falls für das Attribut ein Wert vorhanden ist. Wenn kein Wert für das Attribut vorhanden ist, geben Sie **objectGuid** als unveränderliche ID aus.  Sie können den Satz mit den benutzerdefinierten Anspruchsregeln wie im folgenden Abschnitt beschrieben erstellen.

**Regel 1: Abfragen von Attributen**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Bei dieser Regel fragen Sie die Werte von **ms-ds-consistencyguid** und **objectGuid** für den Benutzer aus Active Directory ab. Ändern Sie den Speichernamen in einen passenden Speichernamen in Ihrer AD FS-Bereitstellung. Ändern Sie außerdem den Anspruchstyp in einen passenden Anspruchstyp für Ihren Verbund, wie Sie dies für **objectGuid** und **ms-ds-consistencyguid** definiert haben.

Durch die Verwendung von **add** anstelle von **issue** vermeiden Sie außerdem das Hinzufügen eines ausgehenden Ausstellungswerts für die Entität und können die Werte als Zwischenwerte verwenden. Sie geben den Anspruch in einer späteren Regel aus, nachdem Sie festgelegt haben, welcher Wert als unveränderliche ID verwendet wird.

**Regel 2: Überprüfen, ob „ms-ds-consistencyguid“ für den Benutzer vorhanden ist**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Diese Regel definiert das temporäre Flag **idflag**, das auf **useguid** festgelegt ist, wenn **ms-ds-concistencyguid** für den Benutzer nicht ausgefüllt ist. Die Logik dahinter ist die Tatsache, dass AD FS leere Ansprüche nicht zulässt. Wenn also die Ansprüche „http://contoso.com/ws/2016/02/identity/claims/objectguid“ und „http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid“ in Regel 1 hinzugefügt werden, erhalten Sie den Anspruch **msdsconsistencyguid** nur, wenn der Wert für den Benutzer ausgefüllt ist. Wenn er nicht ausgefüllt ist, merkt AD FS, dass ein leerer Wert vorhanden ist, und verwirft ihn sofort. Alle Objekte verfügen über **objectGuid**, sodass dieser Anspruch immer vorhanden ist, nachdem Regel 1 ausgeführt wurde.

**Regel 3: Ausgeben von „ms-ds-consistencyguid“ als unveränderliche ID, sofern vorhanden**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dies ist eine implizite **Exist** -Überprüfung. Wenn der Wert für den Anspruch vorhanden ist, geben Sie ihn als unveränderliche ID aus. Im vorherigen Beispiel wird der Anspruch **nameidentifier** verwendet. Sie müssen dies in den entsprechenden Anspruchstyp für die unveränderliche ID in Ihrer Umgebung ändern.

**Regel 4: Ausgeben von „objectGuid“ als unveränderliche ID, wenn „ms-ds-consistencyGuid“ nicht vorhanden ist**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In dieser Regel überprüfen Sie einfach das temporäre Flag **idflag**. Sie entscheiden, ob der Anspruch basierend auf dessen Wert ausgegeben werden soll.

> [!NOTE]
> Die Reihenfolge dieser Regeln ist wichtig.
>
>

### <a name="sso-with-a-subdomain-upn"></a>SSO mit Unterdomänen-UPN
Mit Azure AD Connect können Sie mehrere Domänen für den Verbund hinzufügen. Dies ist unter [Hinzufügen einer neuen Verbunddomäne](active-directory-aadconnect-federation-management.md#addfeddomain) beschrieben. Sie müssen den UPN-Anspruch ändern, damit die Aussteller-ID der Stammdomäne entspricht, und nicht der Unterdomäne, da die Verbundstammdomäne auch das untergeordnete Element abdeckt.

Standardmäßig wird die Anspruchsregel für die Aussteller-ID wie folgt festgelegt:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Aussteller-ID-Standardanspruch](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

Die Standardregel verwendet einfach das UPN-Suffix im Anspruch der Aussteller-ID. John ist z.B. ein Benutzer in „sub.contoso.com“, und „contoso.com“ bildet mit Azure AD einen Verbund. John gibt bei der Anmeldung bei Azure AD john@sub.contoso.com als Benutzernamen ein, und die Aussteller-ID-Standardanspruchsregel in AD FS behandelt ihn wie folgt:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Anspruchswert:** http://sub.contoso.com/adfs/services/trust/

Damit nur die Stammdomäne im Ausstelleranspruchswert enthalten ist, ändern Sie die Anspruchsregel wie folgt:

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Azure AD Connect-Optionen für die Benutzeranmeldung](active-directory-aadconnect-user-signin.md).

