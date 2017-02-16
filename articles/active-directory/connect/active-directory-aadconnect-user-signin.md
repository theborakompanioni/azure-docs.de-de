---
title: 'Azure AD Connect: Benutzeranmeldung | Microsoft Docs'
description: "Azure AD Connect-Benutzeranmeldung für benutzerdefinierte Einstellungen."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: bddd581b5164b03ccba75370cd89a7831101cf37
ms.openlocfilehash: 5b77d16c4a373936dfa5fbba35bc3dc4c9a37e78


---
# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD Connect-Optionen für die Benutzeranmeldung
Mit Azure AD Connect können sich Ihre Benutzer sowohl bei Cloud- als auch bei lokalen Ressourcen mit denselben Kennwörtern anmelden. In diesem Artikel werden die wichtigsten Konzepte für jedes Identitätsmodell beschrieben, um Sie bei der Auswahl der Identität zu unterstützen, die Sie für Ihre Registrierung für Azure AD verwenden möchten.

Wenn Sie mit dem Azure AD-Identitätsmodell bereits vertraut sind und mehr über ein bestimmtes Modell erfahren möchten, klicken Sie unten einfach auf das entsprechende Thema.

* [Kennwortsynchronisierung](#password-synchronization) mit [einmaliger Anmeldung (Single Sign-On (SSO))](active-directory-aadconnect-sso.md)
* [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md)
* [Verbund-SSO (mit AD FS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)





##<a name="choosing-the-user-sign-in-method-for-your-organization"></a>Auswählen einer Benutzeranmeldemethode für Ihre Organisation
Für die meisten Organisationen, die lediglich die Benutzeranmeldung für Office 365, SaaS-Anwendungen und andere Azure AD-basierte Ressourcen aktivieren möchten, empfiehlt sich Standardoption für die Kennwortsynchronisierung. Einige Organisationen haben aber bestimmte Gründe, warum sie diese Option nicht nutzen können, und können dann entweder die Verbundanmeldung, z.B. AD FS, oder die Passthrough-Authentifizierung verwenden. Die folgende Tabelle dient Ihnen als Hilfe beim Treffen der richtigen Entscheidung. 

Ziel | PHS und SSO| PTA mit SSO| AD FS |
 --- | --- | --- | --- |
Synchronisieren neuer Benutzer-, Kontakt- und Gruppenkonten, die in meiner lokalen Active Directory-Instanz automatisch für die Cloud erstellt werden|x|x|x|
Einrichten meines Mandanten für Office 365-Hybridszenarien|x|x|x|
Ermöglichen der Anmeldung und des Zugriffs auf Clouddienste für meine Benutzer mit ihrem lokalen Kennwort|x|x|x|
Implementieren des einmaligen Anmeldens mit Anmeldeinformationen des Unternehmens|x|x|x|
Sicherstellen, dass keine Kennwörter in der Cloud gespeichert werden||x*|x|
Aktivieren der lokalen Multi-Factor Authentication-Lösungen|||x|

*Per einfachem Connector.

>[!NOTE] 
> Für die Passthrough-Authentifizierung gelten in Bezug auf Rich Clients derzeit einige Einschränkungen.  Weitere Informationen finden Sie unter [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md).

### <a name="password-synchronization"></a>Kennwortsynchronisierung
Mit der Kennwortsynchronisierung werden Benutzerkennworthashes aus Ihrem lokalen Active Directory mit Azure AD synchronisiert.  Werden Kennwörter geändert oder lokal zurückgesetzt, werden die neuen Kennwörter sofort mit Azure AD synchronisiert, damit Ihre Benutzer für Cloudressourcen dasselbe Kennwort verwenden können wie in der lokalen Umgebung.  Die Kennwörter werden weder an Azure AD übermittelt noch als Klartext in Azure AD gespeichert.  Die Kennwortsynchronisierung kann mit der Kennwortrückschreibung kombiniert werden, um es den Benutzern zu ermöglichen, ihre Kennwörter in Azure AD selbst zurückzusetzen.

Darüber hinaus können Sie auch das [einmalige Anmelden (SSO)](active-directory-aadconnect-sso.md) für Benutzer auf in die Domäne eingebundenen Computern aktivieren, die sich im Unternehmensnetzwerk befinden. Beim einmaligen Anmelden müssen aktivierte Benutzer nur einen Benutzernamen eingeben, um sicher auf die Cloudressourcen zuzugreifen.

![Cloud](./media/active-directory-aadconnect-user-signin/passwordhash.png)

[Weitere Informationen zur Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md)

### <a name="pass-through-authentication"></a>Passthrough-Authentifizierung
Bei der Passthrough-Authentifizierung wird das Kennwort des Benutzers anhand des lokalen Active Directory-Controllers überprüft, und das Kennwort muss in Azure AD nicht hinterlegt sein. So können lokale Richtlinien während der Authentifizierung für Clouddienste ausgewertet werden, z.B. Einschränkungen der Anmeldestunden. Bei der Passthrough-Authentifizierung wird ein einfacher Agent auf einem in die Domäne eingebundenen Windows Server 2012 R2-Computer in der lokalen Umgebung genutzt, der auf Anforderungen zur Kennwortüberprüfung lauscht. Für diesen Agent müssen keine Ports für eingehenden Datenverkehr aus dem Internet geöffnet sein.

Darüber hinaus können Sie auch das einmalige Anmelden für Benutzer auf in die Domäne eingebundenen Computern aktivieren, die sich im Unternehmensnetzwerk befinden. Beim einmaligen Anmelden müssen aktivierte Benutzer nur einen Benutzernamen eingeben, um sicher auf die Cloudressourcen zuzugreifen.
![Passthrough-Authentifizierung](./media/active-directory-aadconnect-user-signin/pta.png)

[Weitere Informationen zur Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) und zum [einmaligen Anmelden](active-directory-aadconnect-sso.md)

### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Verbund mit einem neuen oder vorhandenen AD FS in einer Windows Server 2012 R2-Farm
Mit der Verbundanmeldung können sich Ihre Benutzer bei Azure AD-basierten Diensten mit ihren lokalen Kennwörtern anmelden. Während sie in ihrem Unternehmensnetzwerk angemeldet sind, können sie sogar auf Cloudressourcen zugreifen, ohne ihre Kennwörter eingeben zu müssen.  Mit der Verbundoption für AD FS können Sie eine neue Windows Server 2012 R2-Farm bereitstellen oder eine Farm angeben.  Wenn Sie eine vorhandene Farm angeben, konfiguriert Azure AD Connect die Vertrauensstellung zwischen der Farm und Azure AD, sodass sich Ihre Benutzer anmelden können.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Voraussetzungen für den Verbund mit AD FS unter Windows Server 2012 R2
Wenn Sie eine neue Farm bereitstellen:

* Ein Windows Server 2012 R2-Server für den Verbundserver.
* Ein Windows Server 2012 R2-Server für den Webanwendungsproxy.
* Eine PFX-Datei mit einem SSL-Zertifikat für den vorgesehenen Verbunddienstnamen, z.B. „fs.contoso.com“.

Wenn Sie eine neue Farm bereitstellen oder eine vorhandene Farm verwenden:

* Lokale Administratorrechte auf Ihren Verbundservern.
* Lokale Administratorrechte für jeden Arbeitsgruppenserver (keiner Domäne angehörend), auf dem Sie die Webanwendungsproxy-Rolle bereitstellen möchten.
* Der Computer, auf dem Sie den Assistenten ausführen, muss eine Verbindung mit allen weiteren Computern herstellen können, auf denen Sie AD FS oder den Webanwendungsproxy über die Windows-Remoteverwaltung installieren möchten.

[Konfigurieren von SSO mit AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Anmelden mit einer früheren Version von AD FS oder einer Drittanbieterlösung
Wenn Sie die Cloudanmeldung bereits mit einer früheren Version von AD FS (z.B. AD FS 2.0) oder einer Drittanbieterlösung konfiguriert haben, können Sie die Konfiguration der Benutzeranmeldung über Azure AD Connect überspringen.  Auf diese Weise profitieren Sie von den neuesten Synchronisierungsfunktionen und anderen Features Azure AD Connect und können gleichzeitig Ihre vorhandene Lösung für die Anmeldung nutzen.

[Azure AD-Verbund – Kompatibilitätsliste für Drittanbieter](active-directory-aadconnect-federation-compatibility.md)


## <a name="user-sign-in-and-user-principal-name-upn"></a>Benutzeranmeldung und Benutzerprinzipalname (UPN)
### <a name="understanding-user-principal-name"></a>Grundlegendes zu Benutzerprinzipalnamen
In Active Directory ist das UPN-Standardsuffix der DNS-Name der Domäne, in der das Benutzerkonto erstellt wurde. In den meisten Fällen ist dies der Domänenname, der als Organisationsdomäne im Internet registriert ist. Allerdings können Sie weitere UPN-Suffixe mithilfe von Active Directory-Domänen und -Vertrauensstellungen hinzufügen.

Der UPN des Benutzers hat das Format username@domain. Der Benutzer John könnte für die Active Directory-Domäne „contoso.com“ beispielsweise den folgenden UPN haben: 'john@contoso.com'.. Der UPN des Benutzers basiert auf RFC 822. Obwohl UPN und E-Mail-Adresse das gleiche Format verwenden, entspricht der Wert des UPN für einen Benutzer möglicherweise nicht der E-Mail-Adresse dieses Benutzers.

### <a name="user-principal-name-in-azure-ad"></a>Benutzerprinzipalnamen in Azure AD
Der Azure AD Connect-Assistent verwendet entweder das userPrincipalName-Attribut, oder Sie können das Attribut (bei der benutzerdefinierten Installation) angeben, das lokal als Benutzerprinzipalname in Azure AD verwendet werden soll. Dies ist der Wert, der zur Anmeldung bei Azure AD verwendet wird. Wenn der Wert des Attributs für den Benutzerprinzipalnamen nicht einer überprüften Domäne in Azure AD entspricht, ersetzt ihn Azure AD durch den Standardwert „.onmicrosoft.com“.

Jedes Verzeichnis in Azure Active Directory verfügt über einen integrierten Domänennamen der Form „contoso.onmicrosoft.com“, den Sie verwenden können, um mit der Nutzung von Azure oder anderen Microsoft-Diensten zu beginnen. Sie können mithilfe von benutzerdefinierten Domänen die Anmeldung vereinfachen und die Benutzerfreundlichkeit erhöhen. Informationen zu benutzerdefinierten Domänennamen in Azure AD und eine Anleitung zur Überprüfung einer Domäne finden Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory-add-domain.md#add-a-custom-domain-name-to-your-directory)

## <a name="azure-ad-sign-in-configuration"></a>Konfiguration der Azure AD-Anmeldung
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Konfigurieren der Azure AD-Anmeldung mit Azure AD Connect
Die Azure AD-Anmeldung hängt davon ab, ob Azure AD das UPN-Suffix eines zu synchronisierenden Benutzers mit einer der überprüften benutzerdefinierten Domänen im Azure AD-Verzeichnis abgleichen kann. Azure AD Connect bietet Unterstützung beim Konfigurieren der Einstellungen für die Azure AD-Anmeldung, damit die Benutzeranmeldung in der Cloud der lokalen Anmeldung ähnelt. Azure AD Connect listet die für die Domäne(n) definierten UPN-Suffixe auf, versucht, diese mit einer benutzerdefinierten Domäne in Azure AD abzugleichen, und unterstützt Sie bei der entsprechenden Aktion, die ausgeführt werden soll.
Die Azure AD-Anmeldeseite listet die UPN-Suffixe auf, die für das lokale Active Directory-Verzeichnis definiert wurden, und zeigt den entsprechenden Status für jedes Suffix. Mögliche Statuswerte:

| Zustand | Beschreibung | Erforderliche Aktion |
|:--- |:--- |:--- |
| Überprüft |Azure AD Connect hat eine übereinstimmende überprüfte Domäne in Azure AD festgestellt. Alle Benutzer dieser Domäne können sich mit ihren lokalen Anmeldeinformationen anmelden. |Es ist keine Aktion erforderlich |
| Nicht überprüft |Azure AD Connect hat eine übereinstimmende benutzerdefinierte Domäne in Azure AD gefunden, die allerdings nicht überprüft ist. Das UPN-Suffix der Benutzer dieser Domäne wird nach der Synchronisierung in das Standardsuffix „.onmicrosoft.com“ geändert, wenn die Domäne nicht überprüft wurde. |Überprüfen Sie die benutzerdefinierte Domäne in Azure AD. [Weitere Informationen](../active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |
| Nicht hinzugefügt |Azure AD Connect konnte keine benutzerdefinierte Domäne finden, die dem UPN-Suffix entspricht. Das UPN-Suffix der Benutzer dieser Domäne wird in das Standardsuffix „.onmicrosoft.com“ geändert, wenn die Domäne nicht in Azure hinzugefügt und überprüft wurde. |Fügen Sie eine benutzerdefinierte Domäne hinzu, die dem UPN-Suffix entspricht, und überprüfen Sie sie. [Weitere Informationen](../active-directory-add-domain.md) |

Die Azure AD-Anmeldeseite listet die für die lokale Active Directory-Instanz definierten UPN-Suffixe und die entsprechende benutzerdefinierte Domäne in Azure AD mit ihrem aktuellen Überprüfungsstatus auf. In der benutzerdefinierten Installation können Sie jetzt das Attribut für Benutzerprinzipalnamen auf der Seite **Azure AD-Anmeldung** auswählen.

![Azure AD-Anmeldeseite](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Sie können auf die Schaltfläche für die Aktualisierung klicken, um den aktuellen Status der benutzerdefinierten Domänen erneut aus Azure AD abzurufen.

### <a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Auswählen des Attributs für den Benutzerprinzipalnamen in Azure AD
UserPrincipalName: Das Atttribut „userPrincipalName“ ist das Attribut, mit dem sich Benutzer bei Azure AD und Office 365 anmelden. Die verwendeten Domänen, auch als UPN-Suffix bezeichnet, sollte in Azure AD überprüft werden, bevor die Benutzer synchronisiert werden. Es wird dringend empfohlen, das Standardattribut „userPrincipalName“ beizubehalten. Wenn dieses Attribut nicht routingfähig ist und nicht überprüft werden kann, können Sie ein anderes Attribut als das Attribut mit der Anmelde-ID auswählen, beispielsweise „email“. Dieser Wert wird als alternative ID bezeichnet. Der Attributwert der alternativen ID muss dem RFC822-Standard entsprechen. Eine alternative ID kann sowohl mit Kennwort-SSO als auch mit Verbund-SSO (Single Sign-On, einmalige Anmeldung) als Anmeldungslösung verwendet werden.

> [!NOTE]
> Die Verwendung einer alternativen ID ist nicht mit allen Office 365-Workloads und der Passthrough-Authentifizierung kompatibel. Weitere Informationen finden Sie unter [Konfigurieren der alternativen Anmelde-ID](https://technet.microsoft.com/library/dn659436.aspx).
> 
> 

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Andere benutzerdefinierte Zustandswerte für Domänen und Auswirkungen auf die Azure-Anmeldung
Es ist wichtig, die Beziehung zwischen den benutzerdefinierten Zustandswerten für Domänen in Ihrem Azure AD-Verzeichnis und den lokal definierten UPN-Suffixen zu verstehen. Gehen wir die verschiedenen Azure-Anmeldemöglichkeiten beim Einrichten der Synchronisierung mit Azure AD Connect durch.

Nehmen wir für die folgenden Informationen einmal an, dass wir mit dem UPN-Suffix „contoso.com“ arbeiten, das zum Beispiel im lokalen Verzeichnis als Teil des UPN verwendet wird. Beispiel: user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express-Einstellungen / Kennwortsynchronisierung
| Zustand | Auswirkung auf die Azure-Benutzeranmeldung |
|:---:|:--- |
| Nicht hinzugefügt |In diesem Fall wurde keine benutzerdefinierte Domäne für „contoso.com“ im Azure AD-Verzeichnis hinzugefügt. Benutzer mit lokalem UPN und dem Suffix „@contoso.com,“ können nicht ihren lokalen UPN zur Azure-Anmeldung verwenden. Sie müssen stattdessen einen neuen, von Azure AD bereitgestellten UPN verwenden, indem Sie das Suffix für das Azure AD-Standardverzeichnis hinzufügen. Wenn Sie beispielsweise Benutzer mit dem Azure AD-Verzeichnis „azurecontoso.onmicrosoft.com“ synchronisieren, erhält der lokale Benutzer user@contoso.com den UPN user@azurecontoso.onmicrosoft.com. |
| Nicht überprüft |In diesem Fall wurde dem Azure AD-Verzeichnis die benutzerdefinierte Domäne „contoso.com“ hinzugefügt, aber diese ist noch nicht überprüft. Wenn Sie mit der Synchronisierung von Benutzern fortfahren, ohne die Domäne zu überprüfen, weist Azure AD den Benutzern wie im Szenario „Nicht hinzugefügt“ neue UPNs zu. |
| Überprüft |In diesem Fall wurde die benutzerdefinierte Domäne „contoso.com“ bereits hinzugefügt, und die Domäne wurde in Azure AD auf das UPN-Suffix überprüft. Benutzer sind nun in der Lage, ihren lokalen Benutzerprinzipalnamen (z.B. „user@contoso.com,“) zu verwenden, um sich bei Azure anzumelden, nachdem sie mit Azure AD synchronisiert wurden. |

###### <a name="ad-fs-federation"></a>AD FS-Verbund
Sie können mit der Standarddomäne „.onmicrosoft.com“ in Azure AD oder einer nicht überprüften benutzerdefinierten Domäne in Azure AD keinen Verbund erstellen. Wenn Sie bei der Ausführung des Azure AD Connect-Assistenten eine nicht überprüfte Domäne auswählen, um einen Verbund zu erstellen, werden Sie von Azure AD Connect aufgefordert, über den DNS-Hostinganbieter für die Domäne zunächst die erforderlichen Datensätze zu erstellen. Weitere Informationen finden Sie [hier](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Wenn Sie die Benutzeranmeldeoption „Verbund mit AD FS“ ausgewählt haben, müssen Sie über eine benutzerdefinierte Domäne verfügen, um mit der Erstellung eines Verbunds in Azure AD fortzufahren. Im vorliegenden Fall bedeutet dies, dass wir dem Azure AD-Verzeichnis die benutzerdefinierte Domäne „contoso.com“ hinzufügen müssen.

| Zustand | Auswirkung auf die Azure-Benutzeranmeldung |
|:---:|:--- |
| Nicht hinzugefügt |In diesem Fall konnte Azure AD Connect im Azure AD-Verzeichnis keine übereinstimmende benutzerdefinierte Domäne für das UPN-Suffix „contoso.com“ finden. Sie müssen die benutzerdefinierte Domäne „contoso.com“ hinzufügen, wenn Benutzer sich mithilfe von AD FS mit ihrem lokalen Benutzerprinzipalnamen (z.B. „user@contoso.com“) anmelden sollen. |
| Nicht überprüft |In diesem Fall zeigt Azure AD Connect entsprechende Angaben an, damit Sie Ihre Domäne zu einem späteren Zeitpunkt überprüfen können. |
| Überprüft |In diesem Fall können Sie ohne weitere Aktionen direkt mit der Konfiguration fortfahren. |

## <a name="changing-user-sign-in-method"></a>Ändern der Benutzeranmeldungsmethode
Nach der Erstkonfiguration von Azure AD Connect durch den Assistenten können Sie mit den verfügbaren Aufgaben in Azure AD Connect die Anmeldemethode für den Benutzer von „Verbund“ in „Kennwortsynchronisierung“ oder „Passthrough-Authentifizierung“ ändern. Führen Sie den Azure AD Connect-Assistenten erneut aus. Es wird eine Liste mit Aufgaben angezeigt, die Sie durchführen können. Wählen Sie **Ändern der Benutzeranmeldung** aus der Liste der Aufgaben.

![Ändern der Benutzeranmeldung](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Auf der nächsten Seite werden Sie aufgefordert, die Anmeldeinformationen für Azure AD anzugeben.

![Mit Azure AD verbinden](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Wählen Sie auf der Seite **Benutzeranmeldung** die gewünschte Benutzeranmeldung aus.

![Mit Azure AD verbinden](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Wenn Sie nur vorübergehend zur Kennwortsynchronisierung wechseln, aktivieren Sie **Benutzerkonten nicht konvertieren**. Ist diese Option nicht aktiviert, werden alle Benutzer zu Verbundbenutzern konvertiert. Dieser Vorgang kann mehrere Stunden dauern.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

Erfahren Sie mehr über [Entwurfskonzepte für Azure AD Connect](active-directory-aadconnect-design-concepts.md)




<!--HONumber=Dec16_HO3-->


