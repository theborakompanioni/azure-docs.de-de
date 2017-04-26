---
title: "Tutorial: Konfigurieren von Workday für die eingehende Synchronisierung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Workday als Quelle von Identitätsdaten für Azure Active Directory verwendet wird."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: c18c92a1ff7d98b61c09138c492aa78dc99ec1cf
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-configure-workday-for-inbound-synchronization"></a>Tutorial: Konfigurieren von Workday für die eingehende Synchronisierung
In diesem Tutorial werden die Schritte erläutert, die Sie in Workday und Azure AD ausführen müssen, um Personen aus Workday in Azure AD zu importieren. 

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure AD Premium-Abonnement
* Einen Mandanten in Workday

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Workday 
2. Erstellen eines Integrationssystembenutzers 
3. Erstellen einer Sicherheitsgruppe 
4. Zuweisen des Integrationssystembenutzers zur Sicherheitsgruppe 
5. Konfigurieren von Sicherheitsgruppenoptionen 
6. Aktivieren von Sicherheitsrichtlinienänderungen 
7. Konfigurieren des Benutzerimports in Azure AD 

## <a name="enable-the-application-integration-for-workday"></a>Aktivieren der Anwendungsintegration für Workday
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Workday aktivieren.

**So aktivieren Sie die Anwendungsintegration für Workday**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Anwendung hinzufügen")
5. Geben Sie im Suchfeld das Wort **Workday**ein.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Anwendung aus dem Katalog hinzufügen")
6. Wählen Sie im Ergebnisbereich "Workday" aus, und klicken Sie dann auf "Abschließen", um die Anwendung hinzuzufügen.
   
   ![Anwendungskatalog](./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Anwendungskatalog")

## <a name="create-an-integration-system-user"></a>Erstellen eines Integrationssystembenutzers

**So erstellen Sie einen Integrationssystembenutzer**

1. Geben Sie in der **Workday-Workbench** die Suchzeichenfolge „Benutzer erstellen“ in das Suchfeld ein, und klicken Sie dann auf den Link **Integrationssystembenutzer erstellen**. 
   
    ![Benutzer erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Benutzer erstellen")
2. Führen Sie die Aufgabe **Integrationssystembenutzer erstellen** aus, indem Sie einen Benutzernamen und ein Kennwort für einen neuen Integrationssystembenutzer angeben.  
 * Lassen Sie das Kontrollkästchen **Bei der nächsten Anmeldung neues Kennwort anfordern** deaktiviert. Dieser Benutzer meldet sich programmgesteuert an. 
 * Übernehmen Sie für **Sitzungstimeout in Minuten** den Standardwert 0. Diese Einstellung verhindert, dass Sitzungen des Benutzers vorzeitig beendet werden. 
   
    ![Integrationssystembenutzer erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Integrationssystembenutzer erstellen")

## <a name="create-a-security-group"></a>Erstellen einer Sicherheitsgruppe
Für das in diesem Tutorial beschriebene Szenario müssen Sie eine uneingeschränkte Integrationssystem-Sicherheitsgruppe erstellen und den Benutzer dieser Gruppe zuweisen.

**So erstellen Sie eine Sicherheitsgruppe**

1. Geben Sie „Sicherheitsgruppe erstellen“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe erstellen**. 
   
    ![Sicherheitsgruppe erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Sicherheitsgruppe erstellen")
2. Führen Sie die Aufgabe **Sicherheitsgruppe erstellen** aus.  
3. Wählen Sie in der Dropdownliste **Typ der Mandantensicherheitsgruppe** die Option „Integrationssystem-Sicherheitsgruppe – uneingeschränkt“ aus.
4. Erstellen Sie eine Sicherheitsgruppe, der die Mitglieder explizit hinzugefügt werden. 
   
    ![Sicherheitsgruppe erstellen](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Sicherheitsgruppe erstellen")

## <a name="assign-the-integration-system-user-to-the-security-group"></a>Zuweisen des Integrationssystembenutzers zur Sicherheitsgruppe

**So weisen Sie den Integrationssystembenutzer zu**

1. Geben Sie „Sicherheitsgruppe bearbeiten“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe bearbeiten**. 
   
    ![Sicherheitsgruppe bearbeiten](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Sicherheitsgruppe bearbeiten")
2. Suchen Sie anhand des Namens nach der neuen Integrationssicherheitsgruppe, und wählen Sie sie aus. 
   
    ![Sicherheitsgruppe bearbeiten](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Sicherheitsgruppe bearbeiten")
3. Fügen Sie den neuen Integrationssystembenutzer der neuen Sicherheitsgruppe hinzu. 
   
    ![Systemsicherheitsgruppe](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systemsicherheitsgruppe")  

## <a name="configure-security-group-options"></a>Konfigurieren von Sicherheitsgruppenoptionen
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

    ![Domänensicherheitsrichtlinien](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domänensicherheitsrichtlinien")  
    
## <a name="activate-security-policy-changes"></a>Aktivieren von Sicherheitsrichtlinienänderungen

**So aktivieren Sie Sicherheitsrichtlinienänderungen**

1. Geben Sie „aktivieren“ in das Suchfeld ein, und klicken Sie dann auf den Link **Ausstehende Sicherheitsrichtlinienänderungen aktivieren**. 
   
    ![Aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktivieren") 
2. Geben Sie zum Ausführen der Aufgabe „Ausstehende Sicherheitsrichtlinienänderungen aktivieren“ zunächst einen Kommentar für Überwachungszwecke ein, und klicken Sie dann auf die Schaltfläche **OK**. 
   
    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")   
3. Führen Sie die Aufgabe auf dem nächsten Bildschirm aus, indem Sie das Kontrollkästchen **Bestätigen** aktivieren und auf **OK** klicken. 
   
    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")  

## <a name="configure-user-import-in-azure-ad"></a>Konfigurieren des Benutzerimports in Azure AD
In diesem Abschnitt wird beschrieben, wie Sie Azure AD zum Importieren von Personen aus Workday konfigurieren.

**So konfigurieren Sie den Benutzerimport**

1. Klicken Sie auf der Anwendungsintegrationsseite für **Workday** auf **Benutzerimport konfigurieren**, um das Dialogfeld **Bereitstellung konfigurieren** zu öffnen.
2. Führen Sie auf der Seite **Einstellungen und Administrator-Anmeldeinformationen** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**: 
   
    ![Einstellungen und Administratoranmeldeinformationen](./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Einstellungen und Administratoranmeldeinformationen")    
  1. Geben Sie im Textfeld für den **Workday-Admin-Benutzernamen** den Namen des Benutzers ein, den Sie im Abschnitt „Erstellen eines Integrationssystembenutzers“ erstellt haben.
  2. Geben Sie im Textfeld für das **Workday-Administratorkennwort** das Kennwort des Benutzers ein, den Sie im Abschnitt „Erstellen eines Integrationssystembenutzers“ erstellt haben.
  3. Geben Sie im Textfeld für die **Workday-Mandanten-URL** die URL Ihres Workday-Mandanten ein.
3. Klicken Sie auf der Seite **Verbindung testen** auf **Test starten**, um die Konnektivität zu überprüfen, und klicken Sie anschließend auf **Weiter**. 
   
    ![Verbindung testen](./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Verbindung testen")  
4. Klicken Sie auf der Seite **Bereitstellungsoptionen** auf **Weiter**. 
   
    ![Bereitstellungsoptionen](./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Bereitstellungsoptionen")
5. Klicken Sie im Dialogfeld **Bereitstellung beginnen** auf **Abschließen**. 
   
    ![Bereitstellung beginnen](./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Bereitstellung beginnen")

Jetzt können Sie zum Abschnitt **Benutzer** wechseln und überprüfen, ob der Workday-Benutzer importiert wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


