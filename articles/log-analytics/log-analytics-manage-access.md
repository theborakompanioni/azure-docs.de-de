---
title: Verwalten des Zugriffs auf Log Analytics | Microsoft-Dokumentation
description: "Zum Verwalten des Zugriffs auf Log Analytics werden verschiedene Verwaltungsaufgaben für Benutzer, Konten, OMS-Arbeitsbereiche und Azure-Konten verwendet."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fbd35f9fdd5165a2dd4bc7dd47bd70c890539e38


---
# <a name="manage-access-to-log-analytics"></a>Verwalten des Zugriffs auf Log Analytics
Die Verwaltung des Zugriffs auf Log Analytics umfasst verschiedene Verwaltungsaufgaben für Benutzer, Konten, OMS-Arbeitsbereiche und Azure-Konten. Zum Erstellen eines Arbeitsbereichs in der Operations Management Suite (OMS) wählen Sie einen Arbeitsbereichsnamen aus, ordnen ihn Ihrem Konto zu und wählen einen geografischen Standort aus. Ein Arbeitsbereich ist im Wesentlichen ein Container, der Kontoinformationen und einfache Konfigurationsinformationen für das Konto enthält. Sie oder andere Mitglieder Ihrer Organisation können mehrere OMS-Arbeitsbereiche nutzen, um unterschiedliche Mengen von Daten zu verwalten, die in Ihrer gesamten IT-Infrastruktur oder Teilen davon erfasst werden.

Informationen für einen schnellen Einstieg finden Sie im Artikel [Erste Schritte mit Log Analytics](log-analytics-get-started.md). Im weiteren Verlauf dieses Artikels werden häufig verwendete Aufgaben beschrieben:

* Bestimmen der benötigten Anzahl von Arbeitsbereichen
* Verwalten von Konten und Benutzern
* Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich
* Verknüpfen eines vorhandenen Arbeitsbereichs mit einem Azure-Abonnement
* Upgrade eines Arbeitsbereichs auf einen kostenpflichtigen Datenplan
* Ändern des Datenplantyps
* Hinzufügen einer Azure Active Directory-Organisation zu einem vorhandenen Arbeitsbereich
* Löschen Ihres OMS-Arbeitsbereichs

## <a name="determine-the-number-of-workspaces-you-need"></a>Bestimmen der benötigten Anzahl von Arbeitsbereichen
Ein Arbeitsbereich ist eine Azure-Ressource. Es handelt sich hierbei um einen Container, in dem Daten gesammelt, aggregiert, analysiert und im OMS-Portal angezeigt werden.

Es ist möglich, mehrere OMS Log Analytics-Arbeitsbereiche zu erstellen, und Benutzer können Zugriff auf einen oder mehrere Arbeitsbereiche haben. Durch Minimieren der Anzahl von Arbeitsbereichen können Sie die meisten Daten übergreifend abfragen und korrelieren. In diesem Abschnitt wird beschrieben, wann es hilfreich sein kann, mehr als einen Arbeitsbereich zu erstellen.

Ein Log Analytics-Arbeitsbereich bietet derzeit Folgendes:

* Einen geografischen Standort für die Speicherung von Daten
* Granularität für die Abrechnung
* Datenisolation

Basierend auf den obigen Merkmalen sollten Sie mehrere Arbeitsbereiche erstellen, wenn Folgendes gilt:

* Sie sind ein globales Unternehmen und müssen Daten aus Gründen der Datensouveränität bzw. aus Compliancegründen in bestimmten Regionen speichern.
* Sie nutzen Azure und möchten Gebühren für ausgehende Datenübertragungen vermeiden, indem Sie einen Log Analytics-Arbeitsbereich in derselben Region wie die verwalteten Azure-Ressourcen nutzen.
* Sie möchten Gebühren basierend auf der Nutzung unterschiedlichen Abteilungen bzw. Geschäftseinheiten zuordnen. Wenn Sie einen Arbeitsbereich für jede Abteilung bzw. Geschäftseinheit erstellen, werden die Gebühren in Ihrer Azure-Rechnung und Nutzungsaufstellung für jeden Arbeitsbereich separat aufgeführt.
* Sie sind ein Service Provider mit Verwaltung und müssen die Log Analytics-Daten für jeden Kunden, den Sie verwalten, von den Daten der anderen Kunden isolieren.
* Sie verwalten mehrere Kunden und möchten, dass den einzelnen Kunden/Abteilungen/Geschäftseinheiten jeweils nur die eigenen Daten angezeigt werden.

Wenn Sie Agents zum Sammeln von Daten verwenden, können Sie jeden Agent so konfigurieren, dass er Daten an den erforderlichen Arbeitsbereich liefert.

Bei Verwendung von System Center Operations Manager kann jede Operations Manager-Verwaltungsgruppe mit nur einem Arbeitsbereich verbunden werden. Sie können den Microsoft Monitoring Agent auf Computern installieren, die mit Operations Manager verwaltet werden, und den Agent so einrichten, dass er sowohl Daten an Operations Manager als auch an einen anderen Log Analytics-Arbeitsbereich liefert.

### <a name="workspace-information"></a>Informationen zum Arbeitsbereich
Im OMS-Portal können Sie Ihre Informationen zum Arbeitsbereich anzeigen und angeben, ob Sie Informationen von Microsoft erhalten möchten.

#### <a name="view-workspace-information"></a>Anzeigen von Informationen zum Arbeitsbereich
1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf Registerkarte **Konten** .
3. Klicken Sie auf die Registerkarte **Arbeitsbereichsinformationen**.  
   ![Informationen zum Arbeitsbereich](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Verwalten von Konten und Benutzern
Jedem Arbeitsbereich können mehrere Benutzerkonten zugeordnet sein, wobei jedes Benutzerkonto (Microsoft- oder Organisationskonto) Zugriff auf mehrere OMS-Arbeitsbereiche hat.

Standardmäßig wird der Besitzer des Microsoft- oder Organisationskontos, das zum Erstellen des Arbeitsbereichs verwendet wird, zum Administrator des Arbeitsbereichs. Der Administrator kann anschließend zusätzliche Microsoft-Konten einladen oder Benutzer in Azure Active Directory auswählen.

Der Zugriff auf den OMS-Arbeitsbereich wird an zwei Stellen gesteuert:

* In Azure können Sie die rollenbasierte Zugriffssteuerung verwenden, um den Zugriff auf das Azure-Abonnement und die dazugehörigen Azure-Ressourcen bereitzustellen. Diese Berechtigungen werden auch für den PowerShell- und den REST-API-Zugriff verwendet.
* Im OMS-Portal besteht nur Zugriff auf das OMS-Portal, nicht auf das zugeordnete Azure-Abonnement.

Wenn Sie Benutzern nur Zugriff auf das OMS-Portal gewähren, aber nicht auf das Azure-Abonnement, mit dem es verknüpft ist, werden den Benutzern auf den Kacheln der Backup- und der Site Recovery-Lösung keine Daten angezeigt.
Damit die Daten in diesen Lösungen allen Benutzern angezeigt werden, benötigen sie mindestens **Lesezugriff** auf den Backup-Tresor und den mit dem OMS-Arbeitsbereich verknüpften Site Recovery-Tresor.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Verwalten des Zugriffs auf Log Analytics mit dem Azure-Portal
Wenn Sie Benutzern Zugriff auf den Log Analytics-Arbeitsbereich gewähren, indem Sie Azure-Berechtigungen verwenden (z.B. im Azure-Portal), können diese Benutzer auch auf das Log Analytics-Portal zugreifen. Wenn Benutzer sich im Azure-Portal befinden, können sie zum OMS-Portal navigieren, indem sie beim Anzeigen der Log Analytics-Arbeitsbereichressource auf die Aufgabe **OMS-Portal** klicken.

Wichtige Punkte zum Azure-Portal:

* Dies ist keine *rollenbasierte Zugriffssteuerung*. Wenn Sie im Azure-Portal für den Log Analytics-Arbeitsbereich Zugriffsberechtigungen vom Typ *Leser* haben, können Sie Änderungen über das OMS-Portal vornehmen. Für das OMS-Portal wird das Konzept „Administrator“, „Mitwirkender“ und „ReadOnly-Benutzer“ (nur Lesezugriff) verwendet. Wenn sich das Konto, mit dem Sie sich angemeldet haben, in der mit dem Arbeitsbereich verknüpften Azure Active Directory-Instanz befindet, sind Sie Administrator im OMS-Portal. Andernfalls sind Sie ein Mitwirkender.
* Wenn Sie sich über „http://mms.microsoft.com“ beim OMS-Portal anmelden, wird standardmäßig die Liste **Arbeitsbereich auswählen** angezeigt. Sie enthält nur die Arbeitsbereiche, die mit dem OMS-Portal hinzugefügt wurden. Zum Anzeigen der Arbeitsbereiche, auf die Sie mit Azure-Abonnements zugreifen können, müssen Sie als Teil der URL einen Mandanten angeben. Beispiel:
  
  `mms.microsoft.com/?tenant=contoso.com` Die Mandanten-ID ist häufig dieser letzte Teil der E-Mail-Adresse, die Sie bei der Anmeldung verwenden.
* Wenn das Konto, mit dem Sie sich anmelden, ein Konto in der Azure Active Directory-Mandanteninstanz ist, sind Sie *Administrator* im OMS-Portal. Solange Sie sich nicht als CSP anmelden, ist dies in der Regel der Fall.  Falls sich Ihr Konto nicht in der Azure Active Directory-Mandanteninstanz befindet, sind Sie *Benutzer* im OMS-Portal.
* Wenn Sie direkt zu einem Portal navigieren möchten, auf das Sie mit Azure-Berechtigungen Zugriff haben, müssen Sie die Ressource als Teil der URL angeben. Es ist möglich, diese URL mit PowerShell abzurufen.
  
  Beispiel: `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.
  
  Die URL sieht wie folgt aus: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Verwalten von Benutzern im OMS-Portal
Sie verwalten Benutzer und Gruppen auf der Registerkarte **Benutzer verwalten** unter der Registerkarte **Konten** auf der Seite „Einstellungen“.   

![Verwalten von Benutzern](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich
Gehen Sie folgendermaßen vor, um einen Benutzer oder eine Gruppe zu einem OMS-Arbeitsbereich hinzuzufügen.

1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Wählen Sie im Abschnitt **Benutzer verwalten** den hinzuzufügenden Kontotyp aus: **Unternehmenskonto**, **Microsoft-Konto** oder **Microsoft-Support**.
   
   * Wenn Sie „Microsoft-Konto“ auswählen, geben Sie die E-Mail-Adresse des Benutzers ein, der dem Microsoft-Konto zugeordnet ist.
   * Wenn Sie „Unternehmenskonto“ auswählen, können Sie einen Teil des Benutzer- oder Gruppennamens bzw. des E-Mail-Alias eingeben. Anschließend wird in einem Dropdownfeld eine Liste mit passenden Benutzern und Gruppen angezeigt. Wählen Sie einen Benutzer oder eine Gruppe aus.
   * Verwenden Sie „Microsoft-Support“, um einem Microsoft-Supporttechniker oder einem anderen Microsoft-Mitarbeiter vorübergehenden Zugriff auf Ihren Arbeitsbereich zu gewähren, damit er bei der Problembehandlung behilflich sein kann.
     
     > [!NOTE]
     > Damit Sie eine optimale Leistung erzielen, sollten Sie die Anzahl von Active Directory-Gruppen, die einem einzelnen OMS-Konto zugeordnet sind, auf drei begrenzen – eines für Administratoren, eines für Mitwirkende und eines für ReadOnly-Benutzer. Mehr Gruppen können sich auf die Leistung von Log Analytics auswirken.
     > 
     > 
4. Wählen Sie den hinzuzufügenden Benutzer- oder Gruppentyp aus: **Administrator**, **Mitwirkender** oder **ReadOnly-Benutzer**.  
5. Klicken Sie auf **Hinzufügen**.
   
   Wenn Sie ein Microsoft-Konto hinzufügen, wird eine Einladung zur Teilnahme am Arbeitsbereich an die angegebene E-Mail-Adresse gesendet. Nachdem der Benutzer die Anweisungen in der Einladung zur Teilnahme an OMS befolgt hat, kann er auf diesen OMS-Arbeitsbereich zugreifen.
   Wenn Sie ein Organisationskonto hinzufügen, kann der Benutzer sofort auf Log Analytics zugreifen.  
   ![Einladungs-E-Mail](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Bearbeiten eines vorhandenen Benutzertyps
Sie können die Kontorolle eines Benutzers ändern, der Ihrem OMS-Konto zugeordnet ist. Sie haben die folgenden Rollenoptionen:

* *Administrator*: Kann Benutzer verwalten, alle Warnungen anzeigen und darauf reagieren sowie Server hinzufügen und entfernen
* *Mitwirkender*: Kann alle Warnungen anzeigen und darauf reagieren sowie Server hinzufügen und entfernen
* *ReadOnly-Benutzer*: Als schreibgeschützt gekennzeichnete Benutzer können folgende Aktionen nicht ausführen:
  
  1. Lösungen hinzufügen/entfernen Der Lösungskatalog ist ausgeblendet
  2. Kacheln unter **Mein Dashboard** hinzufügen/ändern/entfernen
  3. Seiten unter **Einstellungen** anzeigen Die Seiten sind ausgeblendet.
  4. In der Suchansicht: PowerBI-Konfiguration, gespeicherte Suchen und Warnungsaufgaben sind ausgeblendet

#### <a name="to-edit-an-account"></a>So bearbeiten Sie ein Konto
1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Wählen Sie die Rolle für den Benutzer, den Sie ändern möchten.
4. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

### <a name="remove-a-user-from-an-oms-workspace"></a>Entfernen eines Benutzers aus einem OMS-Arbeitsbereich
Gehen Sie folgendermaßen vor, um einen Benutzer aus einem OMS-Arbeitsbereich zu entfernen. Durch Entfernen des Benutzers wird der Arbeitsbereich nicht geschlossen. Stattdessen wird die Zuordnung zwischen diesem Benutzer und dem Arbeitsbereich aufgehoben. Wenn ein Benutzer mehreren Arbeitsbereichen zugeordnet ist, kann er sich weiterhin bei OMS anmelden und seine anderen Arbeitsbereiche anzeigen.

1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Benutzer verwalten**.
3. Klicken Sie neben dem Namen des Benutzers, den Sie entfernen möchten, auf **Entfernen**.
4. Klicken Sie im Bestätigungsdialogfeld auf **Ja**.

### <a name="add-a-group-to-an-existing-workspace"></a>Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich
1. Führen Sie die oben unter „So fügen Sie einen Benutzer einem vorhandenen Arbeitsbereich hinzu“ beschriebenen Schritte 1–4 aus.
2. Wählen Sie unter **Benutzer/Gruppe auswählen** die Option **Gruppe**.
   ![add a group to an existing workspace](./media/log-analytics-manage-access/add-group.png)
3. Geben Sie den Anzeigenamen oder die E-Mail-Adresse für die Gruppe ein, die Sie hinzufügen möchten.
4. Wählen Sie in der Liste die gewünschte Gruppe aus, und klicken Sie auf **Hinzufügen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Verknüpfen eines vorhandenen Arbeitsbereichs mit einem Azure-Abonnement
Ab dem 26. September 2016 müssen alle Arbeitsbereiche bei der Erstellung mit einem Azure-Abonnement verknüpft werden. Vor diesem Datum erstellte Arbeitsbereiche müssen bei der nächsten Anmeldung mit einem Abonnement verknüpft werden. 

> [!IMPORTANT]
> Damit Sie einen Arbeitsbereich verknüpfen können, muss Ihr Azure-Konto bereits Zugriff auf den zu verknüpfenden Arbeitsbereich haben.  Anders ausgedrückt: Das Konto, das Sie für den Zugriff auf das Azure-Portal verwenden, muss **identisch** mit dem Konto sein, mit dem Sie auf Ihren OMS-Arbeitsbereich zugreifen. Ist dies nicht der Fall, lesen Sie unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace) weiter.
> 
> 

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>So verknüpfen Sie einen Arbeitsbereich mit einem Azure-Abonnement im OMS-Portal
Um einen Arbeitsbereich mit einem Azure-Abonnement im OMS-Portal zu verknüpfen, muss der angemeldete Benutzer bereits über ein kostenpflichtiges Azure-Konto verfügen.

1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf die Registerkarte **Konten****Azure Subscription & Data Plan** (Azure-Abonnement und -Datentarif).
3. Klicken Sie auf den Datentarif, den Sie verwenden möchten.
4. Klicken Sie auf **Speichern**.  
   ![Abonnement und Datentarife](./media/log-analytics-manage-access/subscription-tab.png)

Der neue Datentarif wird oben auf der Webseite im Menüband des OMS-Portals angezeigt.

![OMS-Menüband](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>So verknüpfen Sie einen Arbeitsbereich mit einem Azure-Abonnement im Azure-Portal
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Suchen Sie nach **Log Analytics (OMS)** , und wählen Sie diese Option aus.
3. Ihre Liste mit den vorhandenen Arbeitsbereichen wird angezeigt. Klicken Sie auf **Hinzufügen**.  
   ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Klicken Sie unter **OMS Workspace** (OMS-Arbeitsbereich) auf **Or link existing** (Oder vorhandenen einfügen).  
   ![Vorhandene verknüpfen](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klicken Sie auf **Erforderliche Einstellungen konfigurieren**.  
   ![configure required settings](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Daraufhin wird eine Liste mit Arbeitsbereichen angezeigt, die noch nicht mit Ihrem Azure-Konto verknüpft sind. Wählen Sie einen Arbeitsbereich aus.  
   ![Auswählen von Arbeitsbereichen](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Bei Bedarf können Sie die Werte für die folgenden Elemente ändern:
   * Abonnement
   * Ressourcengruppe
   * Ort
   * Tarif   
     ![Ändern von Werten](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klicken Sie auf **Erstellen**. Der Arbeitsbereich ist jetzt mit Ihrem Azure-Konto verknüpft.

> [!NOTE]
> Wenn der Arbeitsbereich, den Sie verknüpfen möchten, hier nicht angezeigt wird, hat Ihr Azure-Abonnement keinen Zugriff auf den OMS-Arbeitsbereich, den Sie auf der OMS-Webseite erstellt haben.  Der Zugriff auf dieses Konto muss über das OMS-Portal gewährt werden. Informationen hierzu finden Sie unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace).
> 
> 

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Upgraden eines Arbeitsbereichs auf einen kostenpflichtigen Plan
Für OMS stehen drei Arten von Arbeitsbereichsplänen zur Verfügung: **Free**, **Eigenständig** und **OMS**.  Bei der Planoption *Free* ist das an Log Analytics gesendete Datenvolumen auf 500 MB pro Tag beschränkt.  Wenn Sie diese Menge überschreiten, müssen Sie Ihren Arbeitsbereich auf einen kostenpflichtigen Plan upgraden, um zu vermeiden, dass die über diesen Grenzwert hinausgehenden Daten nicht erfasst werden. Der Plantyp kann jederzeit geändert werden.  Weitere Informationen zu den Preisen für OMS finden Sie unter [Preise](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Verwenden von Berechtigungen aus einem OMS-Abonnement
Wenn Sie die Berechtigungen nutzen möchten, die Sie durch den Kauf von OMS E1, OMS E2 oder des OMS-Add-Ons für System Center erwerben, wählen Sie die Planoption *OMS* von OMS Log Analytics aus.

Beim Kauf eines OMS-Abonnements werden die Berechtigungen Ihrem Enterprise Agreement hinzugefügt. Die Berechtigungen stehen für alle Azure-Abonnements zur Verfügung, die im Rahmen dieses Vertrags erstellt werden. Dadurch können Sie zum Beispiel mehrere OMS-Arbeitsbereiche verwenden, die die Berechtigung der OMS-Abonnements verwenden.

Gehen Sie wie folgt vor, um sicherzustellen, dass die Verwendung eines OMS-Arbeitsbereichs auf Ihre Berechtigungen des OMS-Abonnements angewendet wird:

1. Erstellen Sie Ihren OMS-Arbeitsbereich in einem Azure-Abonnement, das zu dem Enterprise Agreement mit dem OMS-Abonnement gehört.
2. Wählen Sie für den Arbeitsbereich die Planoption *OMS* aus.

> [!NOTE]
> Wenn Ihr Arbeitsbereich vor dem 26. September 2016 erstellt wurde und Ihr Log Analytics-Preisplan *Premium* lautet, verwendet dieser Arbeitsbereich Berechtigungen aus dem OMS-Add-On für System Center. Die Berechtigungen können auch durch einen Wechsel zum Tarif *OMS* genutzt werden. 
> 
> 

Die OMS-Abonnementberechtigungen werden im Azure- oder im OMS-Portal nicht angezeigt. Die Berechtigungen und die Nutzung können im Enterprise Portal angezeigt werden.  

Wenn Sie das Azure-Abonnement ändern möchten, mit dem Ihr OMS-Arbeitsbereich verknüpft ist, können Sie das Azure PowerShell-Cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) verwenden.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Verwenden von Azure Commitment von einem Enterprise Agreement
Wenn Sie über kein OMS-Abonnement verfügen, wird jede OMS-Komponente separat abgerechnet, und die Nutzung wird auf Ihrer Azure-Rechnung ausgewiesen.

Wenn Sie über einen monetären Azure-Verpflichtungsbetrag für die Unternehmensanmeldung verfügen, mit der Ihre Azure-Abonnements verknüpft sind, wird jegliche Nutzung von Log Analytics automatisch mit dem verbleibenden monetären Verpflichtungsbetrag verrechnet.

Zum Ändern des Azure-Abonnements, mit dem der OMS-Arbeitsbereich verknüpft ist, können Sie das Azure PowerShell-Cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) verwenden.  

### <a name="change-a-workspace-to-a-paid-data-plan"></a>Ändern eines Arbeitsbereichs in einen kostenpflichtigen Datenplan
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Suchen Sie nach **Log Analytics**, und wählen Sie diese Option aus.
3. Ihre Liste mit den vorhandenen Arbeitsbereichen wird angezeigt. Wählen Sie einen Arbeitsbereich aus.  
   ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4. Klicken Sie unter **Einstellungen** auf **Tarif**.  
   ![pricing tier](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5. Wählen Sie unter **Tarif** einen Datentarif aus, und klicken Sie dann auf **Auswählen**.  
   ![Plan auswählen](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Wenn Sie die Ansicht im Azure-Portal aktualisieren, sehen Sie, dass der **Tarif** mit dem ausgewählten Plan aktualisiert wurde.  
   ![Aktualisieren des Tarifs](./media/log-analytics-manage-access/manage-access-change-plan04.png)

## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Hinzufügen einer Azure Active Directory-Organisation zu einem vorhandenen Arbeitsbereich
Sie können Ihren Log Analytics-Arbeitsbereich einer Azure Active Directory-Domäne zuordnen und Ihrem OMS-Arbeitsbereich Benutzer aus Ihrem Verzeichnis hinzufügen.

Wenn Sie den Arbeitsbereich über das Azure-Portal erstellen oder den Arbeitsbereich mit einem Azure-Abonnement verknüpfen, wird Azure Active Directory als Organisationskonto verknüpft.

Falls Sie den Arbeitsbereich über das OMS-Portal erstellen, werden Sie aufgefordert, eine Verknüpfung mit einem Azure-Abonnement zu erstellen, und ein Organisationskonto für das Azure-Abonnement wird dem Log Analytics-Arbeitsbereich zugeordnet.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>So fügen Sie eine Azure Active Directory-Organisation einem vorhandenen Arbeitsbereich hinzu
Durch Hinzufügen einer Azure Active Directory-Organisation können Sie dem Arbeitsbereich Benutzer und Gruppen aus diesem Verzeichnis hinzufügen.

1. Klicken Sie in OMS auf der Seite mit den Einstellungen auf **Konten** und anschließend auf die Registerkarte **Benutzer verwalten**.  
2. Überprüfen Sie die Informationen zu Organisationskonten, und klicken Sie dann auf **Organisation hinzufügen**.  
    ![Hinzufügen einer Organisation](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Geben Sie die Identitätsinformationen des Administrators Ihrer Azure Active Directory-Domäne ein. Anschließend wird bestätigt, dass Ihr Arbeitsbereich mit Ihrer Azure Active Directory-Domäne verknüpft ist.
    ![Bestätigung der Arbeitsbereichsverknüpfung](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

Wenn Sie Benutzer aus einem anderen Verzeichnis hinzufügen möchten, können Sie auf die Schaltfläche *Organisation ändern* klicken, um den Arbeitsbereich einem anderen Verzeichnis zuzuordnen.

## <a name="delete-your-log-analytics-workspace"></a>Löschen Ihres Azure Log Analytics-Arbeitsbereichs
Wenn Sie einen Log Analytics-Arbeitsbereich löschen, werden alle damit zusammenhängenden Daten innerhalb von 30 Tagen aus dem OMS-Dienst gelöscht.

Wenn Sie Administrator sind und mehrere Benutzer mit dem Arbeitsbereich verknüpft sind, wird die Zuordnung zwischen den Benutzern und dem Arbeitsbereich aufgehoben. Wenn die Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie OMS mit diesen Arbeitsbereichen weiter nutzen. Wenn sie jedoch keinen anderen Arbeitsbereichen zugeordnet sind, müssen sie einen Arbeitsbereich erstellen, um OMS verwenden zu können.

### <a name="to-delete-an-oms-workspace"></a>So löschen Sie einen OMS-Arbeitsbereich
1. Klicken Sie in OMS auf die Kachel **Einstellungen** .
2. Klicken Sie auf die Registerkarte **Konten** und dann auf die Registerkarte **Workspace Information** (Arbeitsbereichsinformationen).
3. Klicken Sie auf **Arbeitsbereich schließen**.
4. Wählen Sie einen der Gründe für das Schließen des Arbeitsbereichs aus, oder geben Sie einen anderen Grund in das Textfeld ein.
5. Klicken Sie auf **Arbeitsbereich schließen**.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Hinzufügen von Agents und zum Erfassen von Daten finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md) .
* [Fügen Sie Log Analytics-Lösungen aus dem Lösungskatalog hinzu](log-analytics-add-solutions.md) , um Funktionen hinzuzufügen und Daten zu sammeln.
* [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](log-analytics-proxy-firewall.md) beschreibt, wie Sie vorgehen müssen, damit Agents mit dem Log Analytics-Dienst kommunizieren können, wenn Ihre Organisation einen Proxyserver oder eine Firewall verwendet.




<!--HONumber=Nov16_HO2-->


