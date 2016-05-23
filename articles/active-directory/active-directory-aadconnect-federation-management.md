<properties
	pageTitle="Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect | Microsoft Azure"
	description="AD FS-Verwaltung mithilfe von Azure AD Connect und Anpassung der AD FS-Anmeldung für Benutzer mithilfe von Azure AD Connect und Powershell"
	services="active-directory"
	documentationCenter=""
	authors="anandyadavmsft"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="anandy"/>

# Verwaltung der Active Directory-Verbunddienste und Anpassung mit Azure AD Connect

Dieser Artikel enthält Informationen zu den verschiedenen auf die Active Directory-Verbunddienste (AD FS) bezogenen Aufgaben, die mithilfe von Azure AD Connect ausgeführt werden können, sowie zu anderen AD FS-Aufgaben, die möglicherweise für die vollständige Konfiguration einer AD FS-Farm erforderlich sind.

## AD FS-Verwaltung.

Azure AD Connect bietet verschiedene AD FS-bezogene Aufgaben, die mithilfe des Azure AD Connect-Assistenten mit minimalem Benutzereingriff ausgeführt werden können. Nachdem Sie die Installation von Azure AD Connect durch Ausführen des Assistenten abgeschlossen haben, können Sie den Assistenten erneut ausführen, um zusätzliche Aufgaben durchzuführen.

### Reparieren der Vertrauensstellung

Azure AD Connect kann den aktuellen Status des AD FS- und Azure AD-Vertrauensstellung überprüfen und entsprechende Maßnahmen ergreifen, um die Vertrauensstellung zu reparieren. Führen Sie die folgenden Schritte aus, um Ihre Azure AD- und AD FS-Vertrauensstellung zu reparieren.

Wählen Sie in der Liste mit verfügbaren Aufgaben die Option **AAD- und ADFS-Vertrauensstellung reparieren**.

![](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

Geben Sie auf der Seite **Herstellen einer Verbindung mit Azure AD** die Anmeldeinformationen des globalen Administrators für Azure AD an, und klicken Sie auf „Weiter“.

![](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

Geben Sie auf der Seite **Anmeldeinformationen für den Remotezugriff** die Anmeldeinformationen für den Domänenadministrator ein.

![](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

Wenn Sie auf „Weiter“ klicken, überprüft Azure AD Connect die Integrität des Zertifikats und zeigt ggf. Probleme an.

![](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

Auf der Seite **Bereit für Konfiguration** wird die Liste der Aktionen angezeigt, die ausgeführt werden, um die Vertrauensstellung zu reparieren.

![](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

Klicken Sie auf „Installieren“, um mit der Reparatur der Vertrauensstellung fortzufahren.

>[AZURE.NOTE] Azure AD Connect kann nur selbstsignierte Zertifikate reparieren bzw. Maßnahmen dafür ergreifen. Zertifikate von Drittanbietern nicht durch Azure AD Connect repariert werden.

### Hinzufügen eines neuen AD FS-Servers

> [AZURE.NOTE] Azure AD Connect erfordert die PFX-Zertifikatdatei, um einen AD FS-Server hinzuzufügen. Daher können Sie diesen Vorgang nur ausführen, wenn Sie die AD FS-Farm mit Azure AD Connect konfiguriert haben.

Wählen Sie **Weiteren Verbundserver bereitstellen** und klicken Sie auf „Weiter“.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

Geben Sie auf der Seite **Herstellen einer Verbindung mit Azure AD** die Anmeldeinformationen des globalen Administrators für Azure AD an, und klicken Sie auf „Weiter“.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

Geben Sie auf der nächsten Seite die Anmeldeinformationen des Domänenadministrators an.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

Auf der nächsten Seite werden Sie von Azure AD Connect aufgefordert, das Kennwort der PFX-Datei einzugeben, das Sie beim Konfigurieren der neuen AD FS-Farm mit Azure AD Connect angegeben haben. Klicken Sie auf „Kennwort eingeben“, um das Kennwort für die PFX-Datei anzugeben.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

Geben Sie auf der nächsten Seite die Namen oder IP-Adressen von zusätzlichen Servern an, die der AD FS-Farm hinzugefügt werden sollen.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

Klicken Sie auf „Weiter“, und führen Sie die Schritte auf der letzten Konfigurationsseite aus. Nachdem Azure AD Connect die Server der AD FS-Farm hinzugefügt hat, haben Sie die Möglichkeit, die Konnektivität zu überprüfen.

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

![](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Hinzufügen eines neuen AD FS-Webanwendungs-Proxyservers

> [AZURE.NOTE] Azure AD Connect erfordert die PFX-Zertifikatdatei, um einen Webanwendungs-Proxyserver hinzuzufügen. Daher können Sie diesen Vorgang nur ausführen, wenn Sie die AD FS-Farm mit Azure AD Connect konfiguriert haben.

Wählen Sie in der Liste der verfügbaren Aufgaben die Option **Webanwendungsproxy bereitstellen**.

![](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

Geben Sie auf der nächsten Seite die Anmeldeinformationen des globalen Azure-Administrators an.

![](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

Als nächstes wird die Seite **SSL-Zertifikat angeben** angezeigt, auf der Sie das Kennwort für die PFX-Datei angeben müssen, das Sie beim Konfigurieren der AD FS-Farm mit Azure AD Connect angegeben haben.

![](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

![](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

Fügen Sie auf der nächsten Seite den Server als Webanwendungs-Proxyserver hinzu. Da der Webanwendungs-Proxyserver nicht unbedingt der Domäne angehört, werden Sie vom Assistenten zur Eingabe von Administratoranmeldeinformationen für den hinzugefügten Server aufgefordert.

![](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

Geben Sie auf der Seite **Anmeldeinformationen der Proxyvertrauensstellung** Administratoranmeldeinformationen am, um die Proxyvertrauensstellung zu konfigurieren und auf den primären Server in der AD FS-Farm zuzugreifen.

![](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

Auf der Seite **Bereit für Konfiguration** zeigt der Assistent die Liste der Aktionen an, die ausgeführt werden.

![](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

Klicken Sie auf „Installieren“, um die Konfiguration abzuschließen. Nach Abschluss der Konfiguration bietet der Assistent Ihnen die Möglichkeit, die Verbindung mit den Servern zu überprüfen. Klicken Sie auf „Überprüfen“, um die Konnektivität zu überprüfen.

![](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Hinzufügen einer neuen Verbunddomäne

Das Hinzufügen einer neue Domäne, die einen Verbund mit Azure AD bilden soll, ist mithilfe von Azure AD Connect einfach. Azure AD Connect fügt nicht nur die neue Domäne für den Verbund hinzu, sondern ändert auch die Anspruchsregeln, damit der richtige Aussteller angezeigt wird, falls Sie über mehrere Domänen in einem Verbund mit Azure AD verfügen.

Zum Hinzufügen einer neuen Verbunddomäne wählen Sie die Aufgabe **Weitere Azure AD-Domäne hinzufügen**.

![](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

Geben Sie auf der nächsten Seite des Assistenten die Anmeldeinformationen des globalen Administrators für Azure AD an.

![](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

Geben Sie auf der Seite „Anmeldeinformationen für den Remotezugriff“ die Anmeldeinformationen des Domänenadministrators ein.

![](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

Auf der nächsten Seite stellt der Assistent eine Liste der Azure AD-Domänen bereit, die Sie in Ihrem lokalen Verzeichnis einem Verbund hinzufügen möchten. Wählen Sie die Domäne in der Liste aus.

![](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

Nach der Auswahl der Domäne erhalten Sie vom Assistenten die entsprechenden Informationen zu weiteren Aktionen, die er ausführt, sowie zur Auswirkung der Konfiguration. In einigen Fällen – wenn Sie eine Domäne auswählen, die noch nicht in Azure AD überprüft wurde – erhalten Sie vom Assistenten Informationen zur Überprüfung der Domäne. Unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md) finden Sie weitere Informationen zum Überprüfen Ihrer Domäne.

Klicken Sie auf „Weiter“, und auf der Seite **Bereit für Konfiguration** wird die Liste der Aktionen angezeigt, die Azure AD Connect ausführt. Klicken Sie auf „Installieren“, um die Konfiguration abzuschließen.

![](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## AD FS-Anpassung

Die folgenden Abschnitte enthalten detaillierte Informationen zum Ausführen einiger der häufigsten Aufgaben, die zum Anpassen der AD FS-Anmeldeseite erforderlich sind.

### Benutzerdefiniertes Firmenlogo oder Abbildung hinzufügen

Zum Ändern des auf der Anmeldeseite angezeigten Firmenlogos verwenden Sie das folgende Windows PowerShell-Cmdlet und folgende Syntax.

> [AZURE.NOTE] Die empfohlene Größe das Logo ist 260 x 35 bei 96 DPI bei einer Dateigröße von höchstens 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Der TargetName-Parameter ist erforderlich. Das mit AD FS veröffentlichte Standarddesign heißt „Standard“.
 

### Anmeldebeschreibung hinzufügen

Zum Hinzufügen einer Beschreibung für die Anmeldeseite verwenden Sie das folgende Windows PowerShell-Cmdlet und folgende Syntax.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Ändern von AD FS-Anspruchsregeln

AD FS bietet die Möglichkeit, benutzerdefinierte Regeln zum Ausstellen von Ansprüchen anzugeben. Es unterstützt eine umfangreiche Anspruchsprache, die Sie zum Erstellen von benutzerdefinierten Anspruchsregeln verwenden können. Weitere Informationen finden Sie in [diesem Artikel](https://technet.microsoft.com/library/dd807118.aspx).

In den folgenden Abschnitten wird erläutert, wie Sie benutzerdefinierte Regeln für einige Szenarien in Zusammenhang mit dem Azure AD- und AD FS-Verbund schreiben können.

#### Unveränderliche ID hängt vom Wert im Attribut ab

Azure AD Connect ermöglicht Ihnen das Festlegen eines Attributs, das als Quellanker verwendet wird, wenn Objekte mit Azure AD synchronisiert werden. Möglicherweise sollten Sie einen unveränderlichen ID-Anspruch je nach der Bedingung ausgeben, wenn der Wert im benutzerdefinierten Attribut nicht leer ist. Beachten Sie für das Beispiel, dass Sie „ms-ds-consistencyguid“ als Attribut für den Quellanker ausgewählt haben und „ImmutableID“ als „ms-ds-consistencyguid“ ausgeben möchten, wenn das Attribut einen Wert aufweist. Andernfalls geben Sie „objectGuid“ als unveränderliche ID aus. Sie können den Satz von benutzerdefinierten Anspruchsregeln wie unten beschrieben erstellen:

**Regel 1 (Abfrageattribute)**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Mit dieser Regel werden einfach die Werte von „ms-ds-consistencyguid“ und „objectguid“ für den Benutzer aus Active Directory abgefragt. Ändern Sie den Speichernamen je nach Verfügbarkeit zum entsprechenden Speichernamen in Ihrer ADFS-Bereitstellung und den Anspruchstyp zum richtigen Anspruchstyp, den Sie in Ihrem Verbund für „objectGUID“ und „ms-ds-consistencyguid“ definiert haben. Ich habe in meiner Testumgebung benutzerdefinierte Anspruchstypen definiert.

Durch die Verwendung von „hinzufügen“ anstatt „ausgeben“ vermeiden Sie außerdem das Hinzufügen eines ausgehenden Werts für die Entität und verwenden einfach die Werte als Zwischenwerte. Sie geben den Anspruch in einer späteren Regel aus, nachdem Sie festgelegt haben, welcher Wert als unveränderliche ID verwendet wird.

**Regel 2: (Überprüfen, ob „ms-ds-consistencyguid“ für den Benutzer vorhanden ist)**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Diese Regel definiert einfach das temporäre Flag „idflag“, das auf „useguid“ festgelegt ist, wenn „ms-ds-concistencyguid“ für den Benutzer nicht ausgefüllt ist. Die Logik dahinter ist die Tatsache, dass ADFS leere Ansprüche nicht zulasst. Wenn also die Ansprüche http://contoso.com/ws/2016/02/identity/claims/objectguid und http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid in Regel 1 hinzugefügt werden, erhalten Sie den Anspruch „msdsconsistencyguid“ NUR, wenn der Wert für den Benutzer ausgefüllt ist. Ist er nicht ausgefüllt, erkennt ADFS, dass er als leerer Wert vorhanden ist und verwirft ihn umgehend. „ObjectGuid“, wie bekanntermaßen alle Objekte aufweisen, sodass dieser Anspruch immer vorhanden ist, nachdem Regel 1 ausgeführt wurde.

**Regel 3: Ausgeben von „ms-ds-consistencyguid“ als unveränderliche ID, sofern vorhanden**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dies ist eine implizite EXIST-Überprüfung. Wenn der Wert für den Anspruch vorhanden ist, geben Sie ihn als unveränderliche ID aus. Beachten Sie, dass ich den Anspruch „nameidentifier“ ausgebe. Sie müssen dies zum entsprechenden Anspruchstyp für die unveränderliche ID in Ihrer Umgebung ändern.

**Regel 4: Ausgeben der Objekt-GUID als unveränderliche ID, wenn „ms-ds-consistencyGuid“ nicht vorhanden ist**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

Mit dieser Regel wird einfach das temporäre Flag „idflag“ überprüft und basierend auf dem Wert entschieden, ob der Anspruch ausgegeben wird oder nicht.

> [AZURE.NOTE] Die Reihenfolge dieser Regeln ist wichtig.

#### SSO mit Unterdomänen-UPN

Mit Azure AD Connect können Sie mehrere Domänen für den Verbund hinzufügen ([Hinzufügen einer neuen Verbunddomäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)). Der UPN-Anspruch muss geändert werden, damit die Aussteller-ID der Stammdomäne und nicht der Unterdomäne entspricht, weil die Verbundstammdomäne auch das untergeordnete Element abdeckt.

Standardmäßig wird die Anspruchsregel für die Aussteller-ID wie folgt festgelegt:

	c:[Type 
	== “http://schemas.xmlsoap.org/claims/UPN“]

	=> issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standardmäßiger Aussteller-ID-Anspruch](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Die Standardregel verwendet einfach das UPN-Suffix im Anspruch der Aussteller-ID. John ist z.B. ein Benutzer in „sub.contoso.com“, und „contoso.com“ ist mit Azure AD im Verbund. John gibt bei der Anmeldung bei Azure AD john@sub.contoso.com als Benutzernamen ein, und dann behandelt die standardmäßige Aussteller-ID-Anspruchsregel in AD FS ihn folgendermaßen:

c:[Type == „http://schemas.xmlsoap.org/claims/UPN“]

=> issue(Type = „http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, „.+@(?<domain>.+)“, „http://${domain}/adfs/services/trust/“));

**Anspruchswert:** http://sub.contoso.com/adfs/services/trust/

Damit nur die Stammdomäne im Ausstelleranspruchswert enthalten ist, ändern Sie die Anspruchsregel zu:

	c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

	=> issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## Nächste Schritte

Erfahren Sie mehr über [Benutzeranmeldungsoptionen](active-directory-aadconnect-user-signin.md).

<!---HONumber=AcomDC_0511_2016-->