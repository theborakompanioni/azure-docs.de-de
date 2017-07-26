---
title: "Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung in lokalem Active Directory und Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Workday als Quelle von Identitätsdaten für Active Directory und Azure Active Directory verwendet wird."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2017
ms.author: asmalser
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: f9cc94ca1fc44d10af19debab49435b265bf6e7c
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a>Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung in lokalem Active Directory und Azure Active Directory
In diesem Tutorial werden die Schritte vorgestellt, die Sie zum Importieren von Personen aus Workday in sowohl Active Directory als auch Azure Active Directory ausführen müssen, wobei einige Attribute optional in Workday zurückgeschrieben werden. 



## <a name="overview"></a>Übersicht

Der [Azure Active Directory-Benutzerbereitstellungsdienst](active-directory-saas-app-provisioning.md) ist zum Bereitstellen von Benutzerkonten mit der [Workday Human Resources-API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) integriert. Azure AD nutzt diese Verbindung zum Ermöglichen der folgenden Workflows für die Benutzerbereitstellung:

* **Bereitstellung von Benutzern in Active Directory**: Sie können ausgewählte Gruppen von Benutzern aus Workday in eine oder mehrere Active Directory-Gesamtstrukturen synchronisieren. 

* **Bereitstellung reiner Cloudbenutzer in Azure Active Directory**: Hybridbenutzer, die in Active Directory und Azure Active Directory vorhanden sind, können in letzterem mithilfe von [AAD Connect](connect/active-directory-aadconnect.md) bereitgestellt werden. Reine Cloudbenutzer können jedoch mithilfe des Azure AD-Benutzerbereitstellungsdiensts direkt aus Workday in Azure Active Directory bereitgestellt werden.

* **Zurückschreiben von E-Mail-Adressen in Workday**: Der Azure AD-Benutzerbereitstellungsdienst kann ausgewählte Azure AD-Benutzerattribute wie die E-Mail-Adresse in Workday zurückschreiben.

### <a name="scenarios-covered"></a>Behandelte Szenarien

Die vom Azure AD-Benutzerbereitstellungsdienst unterstützten Workday-Workflows zur Benutzerbereitstellung ermöglichen die Automatisierung der folgenden Szenarien im Personalwesen und bei der Verwaltung des Lebenszyklus von Identitäten:

* **Einstellung neuer Mitarbeiter**: Wenn Workday ein neuer Mitarbeiter hinzugefügt wird, erfolgt in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](active-directory-saas-app-provisioning.md) automatisch die Erstellung eines Benutzerkontos, wobei die E-Mail-Adresse in Workday zurückgeschrieben wird.

* **Aktualisierungen von Mitarbeiterattributen und -profil**: Wenn ein Mitarbeiterdatensatz in Workday aktualisiert wird (wie z.B. der Name, Titel oder Vorgesetzte), wird das entsprechende Benutzerkonto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](active-directory-saas-app-provisioning.md) automatisch aktualisiert.

* **Kündigungen von Mitarbeitern** : Wenn einem Mitarbeiter in Workday gekündigt wird, wird das entsprechende Benutzerkonto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](active-directory-saas-app-provisioning.md) automatisch deaktiviert.

* **Erneute Einstellung von Mitarbeitern**: Wenn ein Mitarbeiter in Workday erneut eingestellt wird, kann sein altes Konto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](active-directory-saas-app-provisioning.md) automatisch reaktiviert oder erneut bereitgestellt werden (je nachdem, was Sie bevorzugen).


## <a name="planning-your-solution"></a>Planen der Lösung

Überprüfen Sie vor Beginn der Workday-Integration die folgenden Voraussetzungen, und lesen Sie die folgende Anleitung zum Erfüllen Ihrer aktuellen Anforderungen an die Active Directory-Architektur und -Benutzerbereitstellung mithilfe der von Azure Active Directory gebotenen Lösungen.

### <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Gültiges Azure AD Premium P1-Abonnement mit globalem Administratorzugriff
* Workday-Implementierungsmandant für Test- und Integrationszwecke
* Administratorberechtigungen in Workday zum Erstellen eines Systemintegrationsbenutzers für Testzwecke und Vornehmen von Änderungen zum Testen von Mitarbeiterdaten
* Für die Benutzerbereitstellung in Active Directory einen der Domäne beigetretenen Server mit mindestens Windows Server 2012 zum Hosten des [lokalen Synchronisierungs-Agents](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) für die Synchronisierung zwischen Active Directory und Azure AD

> [!NOTE]
> Wenn sich Ihr Azure AD-Mandant in Europa befindet, lesen Sie den Abschnitt [Bekannte Probleme](#known-issues) weiter unten.


### <a name="solution-architecture"></a>Lösungsarchitektur

Azure AD bietet einen umfangreichen Satz von Bereitstellungsconnectors für das Erfüllen von Anforderungen an die Bereitstellung und Verwaltung des Lebenszyklus von Identitäten zwischen Workday und Active Directory, Azure AD, SaaS-Apps u.a. Die zu verwendenden Features und die Einrichtung der Lösung variieren abhängig von der Umgebung und den Anforderungen Ihrer Organisation. Führen Sie als ersten Schritt eine Bestandsaufnahme durch, welche der folgenden Elemente in Ihrer Organisation vorhanden und bereitgestellt sind:

* Wie viele Active Directory-Gesamtstrukturen werden verwendet?
* Wie viele Active Directory-Domänen werden verwendet?
* Wie viele Active Directory-Organisationseinheiten (OUs) werden verwendet?
* Wie viele Azure Active Directory-Mandanten werden verwendet?
* Gibt es Benutzer, die sowohl in Active Directory als auch Azure Active Directory (als „hybride“ Benutzer) bereitgestellt werden müssen?
* Gibt es Benutzer, die zwar in Azure Active Directory, aber nicht in Active Directory bereitgestellt werden müssen (z.B. reine Cloudbenutzer)?
* Müssen E-Mail-Adressen von Benutzern in Workday zurückgeschrieben werden?

Sobald Sie die Antworten auf diese Fragen kennen, können Sie Ihre Workday-Bereitstellung gemäß der folgenden Anleitung planen.

#### <a name="using-provisioning-connector-apps"></a>Verwenden von Bereitstellungsconnector-Apps

Azure Active Directory unterstützt vorintegrierte Bereitstellungsconnectors für Workday und eine große Anzahl von SaaS-Anwendungen. 

Ein einzelner Bereitstellungsconnector kommuniziert mit der API eines einzelnen Quellsystems und hilft bei der Bereitstellung von Daten in einem einzelnen Zielsystem. Die meisten Bereitstellungsconnectors, die Azure AD unterstützt, sind für ein einzelnes Quell- und Zielsystem gedacht (z.B. Azure AD zu ServiceNow), und können einfach eingerichtet werden, indem die betreffende App (z.B. ServiceNow) aus dem Azure AD-App-Katalog hinzugefügt wird. 

Es gibt eine direkte Beziehung zwischen Bereitstellungsconnectorinstanzen und App-Instanzen in Azure AD:

| Quellsystem | Zielsystem |
| ---------- | ---------- | 
| Azure AD-Mandant | SaaS-Anwendung |


Beim Arbeiten mit Workday und Active Directory müssen jedoch mehrere Quell- und Zielsysteme berücksichtigt werden:

| Quellsystem | Zielsystem | Hinweise |
| ---------- | ---------- | ---------- |
| Workday | Active Directory-Gesamtstruktur | Jede Gesamtstruktur wird als eigenes Zielsystem behandelt. |
| Workday | Azure AD-Mandant | Entsprechend dem Bedarf reiner Cloudbenutzer. |
| Active Directory-Gesamtstruktur | Azure AD-Mandant | Dieser Flow erfolgt derzeit über AAD Connect. |
| Azure AD-Mandant | Workday | Für das Zurückschreiben von E-Mail-Adressen. |

Um diese zahlreichen Workflows zu mehreren Quell- und Zielsystemen zu erleichtern, bietet Azure AD mehrere Bereitstellungsconnector-Apps, die über den Azure AD-App-Katalog hinzugefügt werden können:

![AAD-App-Katalog](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **Workday to Active Directory Provisioning**: Diese App vereinfacht die Bereitstellung von Benutzerkonten aus Workday in eine einzelne Active Directory-Gesamtstruktur. Wenn Sie über mehrere Gesamtstrukturen verfügen, können Sie eine Instanz dieser App aus dem Azure AD-App-Katalog für jede Active Directory-Gesamtstruktur hinzufügen, in der die Bereitstellung erfolgen soll.

* **Workday to Azure AD Provisioning**: Wenngleich AAD Connect das Tool ist, das Sie zum Synchronisieren von Active Directory-Benutzern mit Azure Active Directory verwenden sollten, können Sie auch diese App nutzen, um die Bereitstellung reiner Cloudbenutzer aus Workday in einem einzelnen Azure Active Directory-Mandanten zu erleichtern.

* **Workday Writeback**: Diese App vereinfacht das Zurückschreiben von E-Mail-Adressen von Benutzern aus Azure Active Directory in Workday.

> [!TIP]
> Die reguläre „Workday“-App dient zum Einrichten des einmaligen Anmeldens zwischen Workday und Azure Active Directory. 

Das Einrichten und Konfigurieren dieser spezielle Bereitstellungsconnector-Apps ist das Thema in den verbleibenden Abschnitten dieses Tutorials. Welche Apps Sie konfigurieren, hängt davon ab, in welchen Systemen die Bereitstellung erfolgen soll und wie viele Active Directory-Gesamtstrukturen und Azure AD-Mandanten in Ihrer Umgebung vorhanden sind.

![Übersicht](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Konfigurieren eines Systemintegrationsbenutzers in Workday
Eine gängige Anforderung an alle Workday-Bereitstellungsconnectors sind Anmeldeinformationen für ein Workday-Systemintegrationskonto, mit dem eine Verbindung mit der Workday Human Resources-API hergestellt wird. In diesem Abschnitt wird das Erstellen eines Systemintegrationskontos in Workday beschrieben.

> [!NOTE]
> Sie können diesen Schritt auslassen und stattdessen ein globales Workday-Administratorkonto als Systemintegrationskonto nutzen. Diese Vorgehensweise ist für Demos einwandfrei, wird jedoch für Produktionsbereitstellungen nicht empfohlen.

### <a name="create-an-integration-system-user"></a>Erstellen eines Integrationssystembenutzers

**So erstellen Sie einen Integrationssystembenutzer**

1. Melden Sie sich mithilfe eines Administratorkontos bei Ihrem Workday-Mandanten an. Geben Sie in der **Workday-Workbench** die Suchzeichenfolge „Benutzer erstellen“ in das Suchfeld ein, und klicken Sie dann auf den Link **Integrationssystembenutzer erstellen**. 
   
    ![Benutzer erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Benutzer erstellen")
2. Führen Sie die Aufgabe **Integrationssystembenutzer erstellen** aus, indem Sie einen Benutzernamen und ein Kennwort für einen neuen Integrationssystembenutzer angeben.  
 * Lassen Sie das Kontrollkästchen **Bei der nächsten Anmeldung neues Kennwort anfordern** deaktiviert. Dieser Benutzer meldet sich programmgesteuert an. 
 * Übernehmen Sie für **Sitzungstimeout in Minuten** den Standardwert 0. Diese Einstellung verhindert, dass Sitzungen des Benutzers vorzeitig beendet werden. 
   
    ![Integrationssystembenutzer erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Integrationssystembenutzer erstellen")

### <a name="create-a-security-group"></a>Erstellen einer Sicherheitsgruppe
Sie müssen eine uneingeschränkte Integrationssystem-Sicherheitsgruppe erstellen und den Benutzer dieser Gruppe zuweisen.

**So erstellen Sie eine Sicherheitsgruppe**

1. Geben Sie „Sicherheitsgruppe erstellen“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe erstellen**. 
   
    ![Sicherheitsgruppe erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Sicherheitsgruppe erstellen")
2. Führen Sie die Aufgabe **Sicherheitsgruppe erstellen** aus.  
3. Wählen Sie in der Dropdownliste **Typ der Mandantensicherheitsgruppe** die Option „Integrationssystem-Sicherheitsgruppe – uneingeschränkt“ aus.
4. Erstellen Sie eine Sicherheitsgruppe, der die Mitglieder explizit hinzugefügt werden. 
   
    ![Sicherheitsgruppe erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Sicherheitsgruppe erstellen")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Zuweisen des Integrationssystembenutzers zur Sicherheitsgruppe

**So weisen Sie den Integrationssystembenutzer zu**

1. Geben Sie „Sicherheitsgruppe bearbeiten“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe bearbeiten**. 
   
    ![Sicherheitsgruppe bearbeiten](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Sicherheitsgruppe bearbeiten")
2. Suchen Sie anhand des Namens nach der neuen Integrationssicherheitsgruppe, und wählen Sie sie aus. 
   
    ![Sicherheitsgruppe bearbeiten](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Sicherheitsgruppe bearbeiten")
3. Fügen Sie den neuen Integrationssystembenutzer der neuen Sicherheitsgruppe hinzu. 
   
    ![Systemsicherheitsgruppe](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systemsicherheitsgruppe")  

### <a name="configure-security-group-options"></a>Konfigurieren von Sicherheitsgruppenoptionen
In diesem Schritt gewähren Sie der neuen Sicherheitsgruppe Berechtigungen für **Get**- und **Put**-Vorgänge für die durch die folgenden Domänensicherheitsrichtlinien geschützten Objekte:

* Externe Kontobereitstellung
* Mitarbeiterdaten: öffentliche Mitarbeiterberichte
* Mitarbeiterdaten: alle Positionen
* Mitarbeiterdaten: aktuelle Personalinformationen
* Mitarbeiterdaten: Berufsbezeichnung in Mitarbeiterprofil

**So konfigurieren Sie Sicherheitsgruppenoptionen**

1. Geben Sie „Domänensicherheitsrichtlinien“ in das Suchfeld ein, und klicken Sie dann auf den Link **Domänensicherheitsrichtlinien für Funktionsbereich**.  
   
    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domänensicherheitsrichtlinien")  
2. Suchen Sie nach "System", und wählen Sie den Funktionsbereich **System** aus.  Klicken Sie auf **OK**.  
   
    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domänensicherheitsrichtlinien")  
3. Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Funktionsbereich „System“ den Eintrag **Sicherheitsverwaltung**, und wählen Sie die Domänensicherheitsrichtlinie **Externe Kontobereitstellung** aus.  
   
    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domänensicherheitsrichtlinien")  
4. Klicken Sie auf **Berechtigungen bearbeiten**, und fügen Sie die neue Sicherheitsgruppe auf der Dialogfeldseite **Berechtigungen bearbeiten** zur Liste der Sicherheitsgruppen mit Berechtigungen für die **Get**- und **Put**-Integration hinzu. 
   
    ![Berechtigung bearbeiten](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Berechtigung bearbeiten")  
5. Wiederholen Sie Schritt 1 oben, um zum Bildschirm für die Auswahl der Funktionsbereiche zurückzukehren. Suchen Sie nun nach „Personal“, wählen Sie den Funktionsbereich **Personal** aus, und klicken Sie auf die Schaltfläche **OK**.
   
    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domänensicherheitsrichtlinien")  
6. Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Funktionsbereich „Personal“ den Eintrag **Mitarbeiterdaten: Personal**, und wiederholen Sie Schritt 4 für die folgenden Sicherheitsrichtlinien:

   * Mitarbeiterdaten: öffentliche Mitarbeiterberichte
   * Mitarbeiterdaten: alle Positionen
   * Mitarbeiterdaten: aktuelle Personalinformationen
   * Mitarbeiterdaten: Berufsbezeichnung in Mitarbeiterprofil
   
7. Wiederholen Sie Schritt 1, um zum Bildschirm für die Auswahl von Funktionsbereichen zurückzukehren. Suchen Sie nun nach **Kontaktinformationen**, wählen Sie den Funktionsbereich „Personal“ aus, und klicken Sie auf **OK**.

8.  Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Funktionsbereich „Personal“ den Eintrag **Mitarbeiterdaten: Kontaktinformationen**, und wiederholen Sie Schritt 4 für die folgenden Sicherheitsrichtlinien:

    * Mitarbeiterdaten: E-Mail (geschäftlich)

    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domänensicherheitsrichtlinien")  
    
### <a name="activate-security-policy-changes"></a>Aktivieren von Sicherheitsrichtlinienänderungen

**So aktivieren Sie Sicherheitsrichtlinienänderungen**

1. Geben Sie „aktivieren“ in das Suchfeld ein, und klicken Sie dann auf den Link **Ausstehende Sicherheitsrichtlinienänderungen aktivieren**. 
   
    ![Aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktivieren") 
2. Geben Sie zum Ausführen der Aufgabe „Ausstehende Sicherheitsrichtlinienänderungen aktivieren“ zunächst einen Kommentar für Überwachungszwecke ein, und klicken Sie dann auf die Schaltfläche **OK**. 
   
    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")   
3. Führen Sie die Aufgabe auf dem nächsten Bildschirm aus, indem Sie das Kontrollkästchen **Bestätigen** aktivieren und auf **OK** klicken. 
   
    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Konfiguration der Benutzerbereitstellung aus Workday in Active Directory
Befolgen Sie diese Anweisungen zum Konfigurieren der Bereitstellung von Benutzerkonten aus Workday in den einzelnen Active Directory-Gesamtstrukturen, in denen die Bereitstellung erfolgen soll.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Bereitstellung aus Workday in Active Directory**

1.  Gehen Sie zu <https://portal.azure.com>.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3.  Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4.  Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie die Kategorie **Alle** aus.

5.  Suchen Sie nach **Workday Provisioning to Active Directory**, und fügen Sie die App aus dem Katalog hinzu.

6.  Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7.  Legen Sie **Bereitstellungsmodus****** auf **Automatisch** fest.

8.  Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. **Dies sollte ungefähr wie folgt aussehen: username@contoso4**

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ durch die ordnungsgemäße Umgebungszeichenfolge ersetzt.

   * **Active Directory-Gesamtstruktur**: Der Name der Active Directory-Gesamtstruktur entsprechend der Rückgabe des PowerShell-Cmdlets „Get-ADForest“. Dieser meist eine Zeichenfolge wie: *contoso.com*

   * **Active Directory-Container**: Geben Sie die Zeichenfolge des Containers ein, der alle Benutzer in Ihrer Active Directory-Gesamtstruktur enthält. Beispiel: *OU=Standard Users,OU=Users,DC=contoso,DC=test*

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-Anmeldeinformationen in Workday gültig sind. 

![Azure-Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Active Directory.

1.  Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Workday-Mitarbeiter in lokale Umgebung synchronisieren**.

2.  Im Feld **Quellobjektbereich** können Sie die Benutzergruppen in Workday für die Bereitstellung in Active Directory auswählen, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Workday“. Beispielfilter:

   * Beispiel: Auswählen der Benutzer mit Mitarbeiter-IDs von 1000000 bis 2000000

      * Attribut: WorkerID

      * Operator: REGEX.VERGLEICH

      * Wert: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Beispiel: Nur Mitarbeiter und keine vorübergehend Beschäftigten 

      * Attribut: EmployeeID

      * Operator: IST NICHT NULL

3.  Im Feld **Zielobjektaktionen** können Sie global filtern, welche Aktionen auf Active Directory angewendet werden können. **Erstellen** und **Aktualisieren** erfolgen am häufigsten.

4.  Im Abschnitt **Attributzuordnungen** können Sie definieren, wie einzelne Workday-Attribute Active Directory-Attributen zugeordnet werden.

5. Klicken Sie auf eine vorhandene Attributzuordnung, um sie zu aktualisieren. Oder klicken Sie am unteren Bildschirmrand auf **Neue Zuordnung hinzufügen**, um neue Zuordnungen hinzuzufügen. Eine einzelne Attributzuordnung unterstützt die folgenden Eigenschaften:

      * **Zuordnungstyp**

         * **Direkt**: Schreibt den Wert des Workday-Attributs unverändert in das AD-Attribut.

         * **Konstante**: Schreibt einen statischen, konstanten Zeichenfolgenwert in das AD-Attribut.

         * **Ausdruck**: Ermöglicht das Schreiben eines benutzerdefinierten Werts basierend auf einem oder mehreren Workday-Attributen in das AD-Attribut. [Weitere Informationen finden Sie im Artikel zu Ausdrücken](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Quellattribut**: Das Benutzerattribut aus Workday.

      * **Standardwert**: Optional. Wenn das Quellattribut einen leeren Wert aufweist, wird von der Zuordnung stattdessen dieser Wert geschrieben.
            Die meisten Konfigurationen sehen vor, dieses Feld leer zu lassen.

      * **Zielattribut**: Das Benutzerattribut in Active Directory.

      * **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Workday und Active Directory verwendet werden soll. Diese Einstellung wird meist auf das Feld „Mitarbeiter-ID“ für Workday festgelegt, das in der Regel den „Mitarbeiter-ID“-Attributen in Active Directory zugeordnet wird.

      * **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Falls mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.

      * **Diese Zuordnung anwenden**
       
         * **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.

         * **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.

6. Um Ihre Zuordnungen zu speichern, klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**.

![Azure-Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Nachstehend finden Sie einige Beispiele für Attributzuordnungen zwischen Workday und Active Directory sowie einige häufig verwendete Ausdrücke**

-   Der Ausdruck für die Zuordnung zum AD-Attribut „parentDistinguishedName“ kann zum Bereitstellen eines Benutzers in einer bestimmten Organisationseinheit basierend auf einem oder mehreren Workday-Quellattributen verwendet werden. In diesem Beispiel werden Benutzer abhängig von ihren Standortsdaten in Workday verschiedenen Organisationseinheiten zugewiesen.

-   Mit dem Ausdruck für die Zuordnung zum AD-Attribut „userPrincipalName“ wird der Benutzerprinzipalname firstName.LastName@contoso.com erstellt. Er ersetzt außerdem ungültige Sonderzeichen.

-   [Hier finden Sie Dokumentation zum Schreiben von Ausdrücken](active-directory-saas-writing-expressions-for-attribute-mappings.md).

  
| WORKDAY-ATTRIBUT | ACTIVE DIRECTORY-ATTRIBUT |  ÜBEREINSTIMMENDE ID? | ERSTELLEN/AKTUALISIEREN |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  EmployeeID | **Ja** | Wird nur bei der Erstellung geschrieben | 
|  **Municipality**   |   l   |     | Erstellen und aktualisieren |
|  **Company**         | company   |     |  Erstellen und aktualisieren |
|  **CountryReferenceTwoLetter**      |   co |     |   Erstellen und aktualisieren |
| **CountryReferenceTwoLetter**    |  c  |     |         Erstellen und aktualisieren |
| **SupervisoryOrganization**  | department  |     |  Erstellen und aktualisieren |
|  **PreferredNameData**  |  displayName |     |   Erstellen und aktualisieren |
| **EmployeeID**    |  cn    |   |   Wird nur bei der Erstellung geschrieben |
| **Fax**      | facsimileTelephoneNumber     |     |    Erstellen und aktualisieren |
| **Vorname**   | givenName       |     |    Erstellen und aktualisieren |
| **Switch(\[Active\], , "0", "True", "1",)** |  accountDisabled      |     | Erstellen und aktualisieren |
| **Mobile**  |    mobile       |     |       Wird nur bei der Erstellung geschrieben |
| **EmailAddress**    | mail    |     |     Erstellen und aktualisieren |
| **ManagerReference**   | manager  |     |  Erstellen und aktualisieren |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Erstellen und aktualisieren |
| **PostalCode**  |   postalCode  |     | Erstellen und aktualisieren |
| **LocalReference** |  preferredLanguage  |     |  Erstellen und aktualisieren |
| **Replace(Mid(Replace(\[EmployeeID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Wird nur bei der Erstellung geschrieben |
| **Nachname**   |   sn   |     |  Erstellen und aktualisieren |
| **CountryRegionReference** |  st     |     | Erstellen und aktualisieren |
| **AddressLineData**    |  streetAddress  |     |   Erstellen und aktualisieren |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Wird nur bei der Erstellung geschrieben |
| **BusinessTitle**   |  title     |     |  Erstellen und aktualisieren |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])", , "m", , ), , "([ñńňÑŃŇN])", , "n", , ), , "([öòőõôóÖÒŐÕÔÓO])", , "o", , ), , "([P])", , "p", , ), , "([Q])", , "q", , ), , "([řŘR])", , "r", , ), , "([ßšśŠŚS])", , "s", , ), , "([TŤť])", , "t", , ), , "([üùûúůűÜÙÛÚŮŰU])", , "u", , ), , "([V])", , "v", , ), , "([W])", , "w", , ), , "([ýÿýŸÝY])", , "y", , ), , "([źžżŹŽŻZ])", , "z", , ), " ", , , "", , ), "contoso.com")**   | userPrincipalName     |     | Erstellen und aktualisieren                                                   
| **Switch(\[Municipality\], "OU=Standard Users,OU=Users,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standard Users,OU=Users,OU=Austin,OU=Locations,DC=contoso,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", “London", "OU=Standard Users,OU=Users,OU=London,OU=Locations,DC=contoso,DC=com")**  | parentDistinguishedName     |     |  Erstellen und aktualisieren |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Teil 3: Konfigurieren des lokalen Synchronisierungs-Agents

Um Active Directory lokal bereitzustellen, muss in der gewünschten Active Directory-Gesamtstruktur ein Agent auf einem in die Domäne eingebunden Server installiert werden. Für diesen Vorgang sind die Anmeldeinformationen eines Domänenadministrators oder (Unternehmensadministrators) erforderlich.

**[Sie können den lokalen Synchronisierungs-Agent hier herunterladen.](https://go.microsoft.com/fwlink/?linkid=847801)**

Führen Sie nach der Installation des Agents die folgenden PowerShell-Befehle zum Konfigurieren des Agents für Ihre Umgebung aus.

**Befehl 1**

> cd C:\\Program Files\\Microsoft Azure Active Directory Synchronization Agent\\Modules\\AADSyncAgent

> import-module AADSyncAgent.psd1

**Befehl 2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Eingabe: Geben Sie für „DirectoryName“ (Verzeichnisname) den Namen der Active Directory-Gesamtstruktur entsprechend der Eingabe in Teil \#2 ein.
* Eingabe: Administratorbenutzername und -kennwort für die Active Directory-Gesamtstruktur

**Befehl 3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Eingabe: Benutzername und Kennwort des globalen Administrators für Ihren Azure AD-Mandanten

**Befehl 4**

> Get-AdSyncAgentProvisioningTasks

* Aktion: Bestätigen Sie, dass Daten zurückgegeben werden. Dieser Befehl ermittelt automatisch Workday-Bereitstellungs-Apps in Ihrem Azure AD-Mandanten. Beispielausgabe:

> Name: My AD Forest
>
> Aktiviert: TRUE
>
> DirectoryName: mydomain.contoso.com
>
> Credentialed: False
>
> Bezeichner: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Befehl 5**

> Start-AdSyncAgentSynchronization -Automatic

**Befehl 6**

> net stop aadsyncagent

**Befehl 7**

> net start aadsyncagent

### <a name="part-4-start-the-service"></a>Teil 4: Starten des Diensts
Sobald Sie die Teile 1 bis 3 abgeschlossen haben, können Sie im Azure-Verwaltungsportal den Bereitstellungsdienst starten.

1.  Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Auf der Registerkarte **Überwachungsprotokolle** können einzelne Synchronisierungsereignisse angezeigt werden, z.B. welche Benutzer aus Workday ausgelesen und anschließend Active Directory hinzugefügt oder darin aktualisiert werden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](active-directory-saas-provisioning-reporting.md)**

5.  Das Windows-Protokoll „Anwendung“ auf dem Agent-Computer zeigt alle Vorgänge, die mittels des Agents ausgeführt werden.

6. Nach Abschluss wird auf der Registerkarte  **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben.

![Azure-Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Konfiguration der Benutzerbereitstellung in Azure Active Directory
Die Konfiguration der Bereitstellung in Azure Active Directory hängt von Ihren Bereitstellungsanforderungen ab (siehe die folgende Tabelle).

| Szenario | Lösung |
| -------- | -------- |
| **Benutzer müssen in Active Directory und Azure AD bereitgestellt werden** | **[AAD Connect](connect/active-directory-aadconnect.md)** verwenden |
| **Benutzer müssen nur in Active Directory bereitgestellt werden** | **[AAD Connect](connect/active-directory-aadconnect.md)** verwenden |
| **Benutzer müssen nur in Azure AD (als reine Cloudbenutzer) bereitgestellt werden** | Die App **Workday to Azure Active Directory provisioning** im App-Katalog verwenden |

Anleitungen zum Einrichten von Azure AD Connect finden Sie in der [Dokumentation zu Azure AD Connect](connect/active-directory-aadconnect.md).

In den folgenden Abschnitten wird das Einrichten einer Verbindung zwischen Workday und Azure AD beschrieben, um reine Cloudbenutzer bereitzustellen.

> [!IMPORTANT]
> Führen Sie diese Schritte nur aus, wenn es reine Cloudbenutzer gibt, die in Azure AD und nicht in lokalem Active Directory bereitgestellt werden müssen.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App für Azure AD und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Workday-zu-Azure Active Directory-Bereitstellung für reine Cloudbenutzer**

1.  Navigieren Sie zu <https://portal.azure.com>.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3.  Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4.  Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie dann die Kategorie **Alle** aus.

5.  Suchen Sie nach **Workday to Azure AD provisioning**, und fügen Sie die App aus dem Katalog hinzu.

6.  Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7.  Legen Sie **Bereitstellungsmodus****** auf **Automatisch** fest.

8.  Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. Dies sollte ungefähr wie folgt aussehen: username@contoso4

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ (bei Bedarf) durch die ordnungsgemäße Umgebungszeichenfolge ersetzt.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**.

   * Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-URL und -Anmeldeinformationen in Workday gültig sind.


### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Azure Active Directory für reine Cloudbenutzer.

1.  Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Mitarbeiter in Azure AD synchronisieren**.

2.   Im Feld **Quellobjektbereich** können Sie die Benutzergruppen in Workday für die Bereitstellung in Azure Active Directory auswählen, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Workday“. Beispielfilter:

   * Beispiel: Auswählen der Benutzer mit Mitarbeiter-IDs von 1000000 bis 2000000

      * Attribut: WorkerID

      * Operator: REGEX.VERGLEICH

      * Wert: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Beispiel: Nur vorübergehend Beschäftigte und keine regulären Mitarbeiter

      * Attribut: ContingentID

      * Operator: IST NICHT NULL

3.  Im Feld **Zielobjektaktionen** können Sie global filtern, welche Aktionen auf Azure AD angewendet werden können. **Erstellen** und **Aktualisieren** erfolgen am häufigsten.

4.  Im Abschnitt **Attributzuordnungen** können Sie definieren, wie einzelne Workday-Attribute Active Directory-Attributen zugeordnet werden.

5. Klicken Sie auf eine vorhandene Attributzuordnung, um sie zu aktualisieren. Oder klicken Sie am unteren Bildschirmrand auf **Neue Zuordnung hinzufügen**, um neue Zuordnungen hinzuzufügen. Eine einzelne Attributzuordnung unterstützt die folgenden Eigenschaften:

   * **Zuordnungstyp**

      * **Direkt**: Schreibt den Wert des Workday-Attributs unverändert in das AD-Attribut.

      * **Konstante**: Schreibt einen statischen, konstanten Zeichenfolgenwert in das AD-Attribut.

      * **Ausdruck**: Ermöglicht das Schreiben eines benutzerdefinierten Werts basierend auf einem oder mehreren Workday-Attributen in das AD-Attribut. [Weitere Informationen finden Sie im Artikel zu Ausdrücken](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Quellattribut**: Das Benutzerattribut aus Workday.

   * **Standardwert**: Optional. Wenn das Quellattribut einen leeren Wert aufweist, wird von der Zuordnung stattdessen dieser Wert geschrieben.
            Die meisten Konfigurationen sehen vor, dieses Feld leer zu lassen.

   * **Zielattribut**: Das Benutzerattribut in Azure AD.

   * **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Workday und Azure AD verwendet werden soll. Diese Einstellung wird meist auf das Feld „Mitarbeiter-ID“ für Workday festgelegt, das in der Regel dem „Mitarbeiter-ID“-Attribut (neu) oder einem Erweiterungsattribut in Azure AD zugeordnet wird.

   * **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Falls mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.

   * **Diese Zuordnung anwenden**

     * **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.

     * **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.

6. Um Ihre Zuordnungen zu speichern, klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**.

### <a name="part-3-start-the-service"></a>Teil 3: Starten des Diensts
Sobald Sie die Teile 1 und 2 abgeschlossen haben, können Sie den Bereitstellungsdienst starten.

1.  Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Einzelne Synchronisierungsereignisse können auf der Registerkarte **Überwachungsprotokolle** angezeigt werden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](active-directory-saas-provisioning-reporting.md)**

5. Nach Abschluss wird auf der Registerkarte  **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Konfigurieren des Zurückschreibens von E-Mail-Adressen in Workday
Befolgen Sie diese Anweisungen zum Konfigurieren des Zurückschreibens von E-Mail-Adressen der Benutzer aus Azure Active Directory in Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Bereitstellung aus Workday in Active Directory**

1.  Gehen Sie zu <https://portal.azure.com>.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3.  Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4.  Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie dann die Kategorie **Alle** aus.

5.  Suchen Sie nach **Workday Writeback**, und fügen Sie die App aus dem Katalog hinzu.

6.  Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7.  Legen Sie **Bereitstellungsmodus****** auf **Automatisch** fest.

8.  Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. Dies sollte ungefähr wie folgt aussehen: username@contoso4

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ (bei Bedarf) durch die ordnungsgemäße Umgebungszeichenfolge ersetzt.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-URL und -Anmeldeinformationen in Workday gültig sind.


### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 


In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Active Directory.

1.  Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Azure AD-Benutzer in Workday synchronisieren**.

2.  Im Feld **Quellobjektbereich** können Sie optional filtern, für welche Gruppen von Benutzern in Azure Active Directory E-Mail-Adressen in Workday zurückgeschrieben werden sollen. Die Standardoption ist „Alle Benutzer in Azure AD“. 

3.  Im Abschnitt **Attributzuordnungen** können Sie definieren, wie einzelne Workday-Attribute Active Directory-Attributen zugeordnet werden. Für die E-Mail-Adresse gibt es standardmäßig eine Zuordnung. Die ID für den Abgleich muss jedoch aktualisiert werden, um Benutzer in Azure AD mit ihren entsprechenden Einträgen in Workday abzugleichen. Eine gängige Methode für den Abgleich ist das Synchronisieren der Mitarbeiter-ID von Workday mit „extensionAttribute“ 1-15 in Azure AD und das anschließende Verwenden dieses Attributs in Azure AD, um die Benutzer wieder mit Workday abzugleichen.

4.  Um Ihre Zuordnungen zu speichern, klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**.

### <a name="part-3-start-the-service"></a>Teil 3: Starten des Diensts
Sobald Sie die Teile 1 und 2 abgeschlossen haben, können Sie den Bereitstellungsdienst starten.

1.  Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Einzelne Synchronisierungsereignisse können auf der Registerkarte **Überwachungsprotokolle** angezeigt werden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](active-directory-saas-provisioning-reporting.md)**

5. Nach Abschluss wird auf der Registerkarte  **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben.

## <a name="known-issues"></a>Bekannte Probleme

* **Überwachungsprotokolle in europäischen Gebietsschemas**: Seit Veröffentlichung dieser Technical Preview-Version gibt es ein bekanntes Problem mit den [Überwachungsprotokollen](active-directory-saas-provisioning-reporting.md) für Workday-Connector-Apps, die nicht im [Azure-Portal](https://portal.azure.com) angezeigt werden, wenn sich der Azure AD-Mandant in einem europäischen Rechenzentrum befindet. Eine Lösung für dieses Problem ist in Kürze verfügbar. Überprüfen Sie dieses Thema in naher Zukunft erneut auf neue Informationen dazu. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Tutorial: Konfigurieren des einmaligen Anmeldens zwischen Workday und Azure Active Directory](active-directory-saas-workday-tutorial.md)
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).

