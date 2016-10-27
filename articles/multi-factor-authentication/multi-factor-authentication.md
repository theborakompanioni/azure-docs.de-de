<properties
    pageTitle="Übersicht über Azure MFA | Microsoft Azure"
    description="Was ist Azure Multi-Factor Authentication (MFA), warum wird MFA verwendet, weitere Informationen über den Client für die Multi-Factor Authentication sowie die verschiedenen verfügbaren Methoden und Versionen. "
    keywords="Einführung in die MFA, MFA-Übersicht, Was ist MFA"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>


# <a name="what-is-azure-multi-factor-authentication?"></a>Was ist Azure Multi-Factor Authentication?
Die Überprüfung in zwei Schritten ist eine Authentifizierungsmethode, die die Verwendung mehrerer Überprüfungsmethoden erfordert und eine wichtige zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen bietet. Dies funktioniert durch das Anfordern von zwei oder mehr der folgenden Verifizierungsmethoden:

- Etwas, das Sie wissen (normalerweise ein Kennwort)
- Etwas, das Sie haben (ein vertrautes Gerät, das nicht leicht dupliziert werden kann, wie ein Telefon)
- Etwas, das Sie sind (biometrisch)

<center>![Benutzername und Kennwort](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Zertifikate](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smartphone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smartcard](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtuelle Smartcard](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Benutzername und Kennwort](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) ist die Lösung von Microsoft für die Überprüfung in zwei Schritten. Azure MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine sichere Authentifizierung mit verschiedenen Überprüfungsmethoden, einschließlich Telefonanruf, SMS oder mobile App.

>[AZURE.VIDEO multi-factor-authentication-overview]

## <a name="why-use-azure-multi-factor-authentication?"></a>Warum sollten Sie Azure Multi-Factor Authentication verwenden?

Heute sind mehr Menschen immer häufiger verbunden als je zuvor. Mit Smartphones, Tablet-PCs, Laptops und PCs verfügen Benutzer über mehrere unterschiedliche Verbindungsoptionen, die es ermöglichen, ständig verbunden zu bleiben. Benutzer können von überall auf ihre Konten und Anwendungen zugreifen, was zu einer produktiveren Arbeitsweise und besserem Kundenservice führt.

Azure Multi-Factor Authentication ist eine benutzerfreundliche, skalierbare und zuverlässige Lösung, die eine zweite Methode zur Authentifizierung bereitstellt, sodass die Benutzer immer geschützt sind.

![Benutzerfreundlich](./media/multi-factor-authentication/simple.png)| ![Skalierbar](./media/multi-factor-authentication/scalable.png)| ![Immer geschützt](./media/multi-factor-authentication/protected.png)|![Zuverlässig](./media/multi-factor-authentication/reliable.png)
:-------------: | :-------------: | :-------------: | :-------------: |
**Benutzerfreundlich**|**Skalierbar**|**Immer geschützt**|**Zuverlässig**

- **Benutzerfreundlich** : Azure Multi-Factor Authentication ist einfach einzurichten und zu verwenden. Dank des zusätzlichen Schutzes von Azure Multi-Factor Authentication können Benutzer ihre eigenen Geräte verwalten. Und das Beste: Azure Multi-Factor Authentication kann in der Regel ganz einfach mit wenigen Klicks eingerichtet werden.
- **Skalierbar** : Azure Multi-Factor Authentication nutzt die Vorteile der Cloud und wird nahtlos in Ihr lokales Active Directory und in benutzerdefinierte Apps integriert. Von diesem Schutz profitieren sogar Ihre umfangreichen unternehmenskritischen Szenarien.
- **Immer geschützt** : Azure Multi-Factor Authentication bietet sichere Authentifizierung unter Verwendung der strengsten Branchenstandards.
- **Zuverlässig** : Wir garantieren 99,9 % Verfügbarkeit für Azure Multi-Factor Authentication. Der Dienst gilt als nicht verfügbar, wenn keine Überprüfungsanforderungen für die Überprüfung in zwei Schritten empfangen oder verarbeitet werden können.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]


## <a name="how-azure-multi-factor-authentication-works"></a>Funktionsweise von Azure Multi-Factor Authentication

Die Sicherheit der Überprüfung in zwei Schritten liegt im Ebenenansatz. Die Methoden der mehrfachen Überprüfung zu überwinden stellt eine große Herausforderung für Angreifer dar. Sollte ein Angreifer Ihr Kennwort herausfinden, hilft ihm das nicht weiter, wenn er nicht auch über das vertrauenswürdige Gerät verfügt. Und sollten Sie das Gerät verlieren, kann der Finder es ohne Ihr Kennwort nicht verwenden.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

## <a name="methods-available-for-multi-factor-authentication"></a>Verfügbare Methoden für Multi-Factor Authentication
Wenn ein Benutzer sich anmeldet, wird eine zusätzliche Überprüfungsanforderung an den Benutzer gesendet. In der folgenden Liste sind die verschiedenen Methoden zusammengefasst, die für diese zweite Überprüfung verwendet werden können.

Überprüfungsmethode | Beschreibung
------------- | ------------- |
Telefonanruf | Der Benutzer wird telefonisch kontaktiert und gebeten, die Anmeldung zu bestätigen. Zum Abschließen der Überprüfung muss die #-Taste gedrückt werden. Statt der #-Taste kann bei dieser Option wahlweise die Eingabe eines vorher festgelegten Codes verlangt werden.
Textnachricht | An das Smartphone des Benutzers wird eine SMS mit einem sechsstelligen Code gesendet. Dieser Code muss eingegeben werden, um die Überprüfung abzuschließen.
Benachrichtigung über eine mobile App | An das Smartphone des Benutzers wird eine Überprüfungsanforderung gesendet. Darin wird der Benutzer aufgefordert, in der mobilen App die Option **Überprüfen** auszuwählen, um die Überprüfung abzuschließen. Dieses Verfahren wird verwendet, wenn die App-Benachrichtigung als primäre Überprüfungsmethode festgelegt ist. Wenn Benutzer diese Benachrichtigung erhalten, obwohl sie gar keinen Anmeldevorgang durchführen, können sie dies als Betrugsversuch melden.
Überprüfungscode in der mobilen App | Die mobile App auf dem Gerät des Benutzers generiert einen Überprüfungscode. Dieses Verfahren wird verwendet, wenn Sie als primäre Überprüfungsmethode das Senden eines Überprüfungscodes ausgewählt haben.

Für die Überprüfung in mobilen Apps kann Azure Multi-Factor Authentication mit Drittanbieter-Authentifizierungsapps für Smartphones verwendet werden. Es wird jedoch empfohlen, die Microsoft Authenticator-App zu verwenden, die für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar ist.

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Verfügbare Versionen von Azure Multi-Factor Authentication
Azure Multi-Factor Authentication ist in drei verschiedenen Versionen verfügbar. 

Version | Beschreibung
------------- | ------------- |
Multi-Factor Authentication für Office 365 | Diese Version arbeitet ausschließlich mit Office 365-Anwendungen und wird über das Office 365-Portal verwaltet. Dadurch können Administratoren jetzt ihre Office 365-Ressourcen mit der Überprüfung in zwei Schritten sichern. Diese Version wird zusammen mit einem Office 365-Abonnement bereitgestellt.
Multi-Factor Authentication für Azure-Administratoren | Dieselbe Teilmenge von Funktionen der Überprüfung in zwei Schritten für Office 365 ist kostenlos für alle Azure-Administratoren verfügbar. In jedem Administratorkonto eines Azure-Abonnements kann diese Funktion für zusätzlichen Schutz aktiviert werden. Ein Administrator, der auf das Azure-Portal zugreifen möchte, um einen virtuellen Computer oder eine Website zu erstellen, Speicher zu verwalten oder einen anderen Azure-Dienst zu verwenden, kann MFA zu seinem Administratorkonto hinzufügen.
Azure Multi-Factor Authentication | Azure Multi-Factor Authentication bietet von allen Versionen den größten Funktionsumfang. Über das [klassische Azure-Portal](http://manage.windowsazure.com) sind zusätzliche Konfigurationsoptionen verfügbar. Außerdem stehen erweiterte Funktionen für die Berichterstellung zur Verfügung, und es werden verschiedene lokale Anwendungen und Cloudanwendungen unterstützt. Azure Multi-Factor Authentication ist im Lieferumfang von Azure Active Directory Premium und Enterprise Mobility Suite enthalten und kann in der Cloud oder lokal bereitgestellt werden.

## <a name="feature-comparison-of-versions"></a>Funktionsvergleich der Versionen
In der folgenden Tabelle werden die Features aufgeführt, die in den verschiedenen Versionen von Azure Multi-Factor Authentication verfügbar sind:

>[AZURE.NOTE] In dieser Vergleichstabelle werden die Funktionen erläutert, die in den einzelnen Abonnements enthalten sind. Wenn Sie über Azure AD Premium oder die Enterprise Mobility Suite verfügen, sind möglicherweise einige Funktionen nicht verfügbar, abhängig davon, ob Sie [MFA in der Cloud oder lokal](multi-factor-authentication-get-started.md) verwenden.

Feature | Multi-Factor Authentication für Office 365 (in Office 365-SKUs enthalten)|Multi-Factor Authentication für Azure-Administratoren (enthalten in Azure-Abonnement) | Azure Multi-Factor Authentication (enthalten in Azure AD Premium und Enterprise Mobility Suite)
------------- | :-------------: |:-------------: |:-------------: |
Administratoren können Konten mit MFA schützen.| ● | ● (nur für Azure-Administratorkonten verfügbar)|●
Mobile App als zweiter Faktor|● | ● | ●
Telefonanruf als zweiter Faktor|● | ● | ●
SMS als zweiter Faktor|● | ● | ●
App-Kennwörter für Clients, die MFA nicht unterstützen|● | ● | ●
Administrative Kontrolle über Authentifizierungsmethoden| ● | ● | ●
PIN-Modus| | | ●
Betrugswarnung| | | ●
MFA-Berichte| | | ●
Einmalumgehung| | | ●
Benutzerdefinierte Begrüßungen für Telefonanrufe| | | ●
Benutzerdefinierte Anrufer-ID für Telefonanrufe| | | ●
Bestätigung von Ereignissen| | | ●
Vertrauenswürdige IP-Adressen| | | ●
Speichern der MFA für vertrauenswürdige Geräte |● | ● | ●
MFA-SDK | | | ● erfordert einen MFA-Anbieter und ein vollständiges Azure-Abonnement
MFA für lokale Anwendungen mit MFA-Server| | | ●

## <a name="how-to-get-azure-multi-factor-authentication"></a>Beziehen von Azure Multi-Factor Authentication

Wenn Sie alle Funktionen von Azure Multi-Factor Authentication nutzen möchten, stehen mehrere Optionen zur Verfügung:

1.  Erwerben Sie Multi-Factor Authentication-Lizenzen, und weisen Sie diese Ihren Benutzern zu.
2.  Kaufen Sie Lizenzen, in denen Azure Multi-Factor Authentication enthalten ist, und weisen Sie sie Ihren Benutzern zu. Beispiele wären etwa Azure Active Directory Premium, Enterprise Mobility Suite und Enterprise Cloud Suite.
3.  Erstellen Sie einen Azure Multi-Factor Authentication-Anbieter in einem Azure-Abonnement. Bei Verwendung eines Azure Multi-Factor Authentication-Anbieters stehen zwei Nutzungsmodelle zur Verfügung, die über Ihr Azure-Abonnement abgerechnet werden:  
    - **Pro Benutzer**. Diese Option ist für Unternehmen geeignet, die die Überprüfung in zwei Schritten für eine feste Anzahl von Mitarbeitern aktivieren möchten, die die Authentifizierung regelmäßig benötigen.  
    - **Pro Authentifizierung**. Diese Option ist für Unternehmen geeignet, die die Überprüfung in zwei Schritten für eine große Gruppe externer Benutzer aktivieren möchten, die die Authentifizierung seltener oder unregelmäßig benötigen.  

Azure Multi-Factor Authentication umfasst auswählbare Überprüfungsmethoden für die Cloud und für Server. Dies bedeutet, dass Sie auswählen können, welche Methoden für Ihre Benutzer verfügbar sind. Diese Funktion befindet sich derzeit in der öffentlichen Vorschau für die Cloudversion von Multi-Factor Authentication. Weitere Informationen finden Sie unter [Auswählbare Verifizierungsmethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods).

Ausführliche Preisinformationen finden Sie unter [Preise für Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Azure Multi-Factor Authentication verwenden möchten, müssen Sie sich zunächst [zwischen der cloudbasierten und der lokalen Variante von MFA entscheiden](multi-factor-authentication-get-started.md)



<!--HONumber=Oct16_HO2-->


