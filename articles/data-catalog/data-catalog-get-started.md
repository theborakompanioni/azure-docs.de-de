---
title: Erste Schritte mit Data Catalog | Microsoft Docs
description: Umfassendes Tutorial zu den Szenarios und Funktionen von Azure Data Catalog
documentationcenter: 
services: data-catalog
author: steelanddata
manager: jhubbard
editor: 
tags: 
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/19/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 2b128e2771ea89d1c0d6b8d52357d1bcc993115a
ms.openlocfilehash: a9629d0a4bfc44f5d035a29f8e97d76922cc0060


---
# <a name="get-started-with-azure-data-catalog"></a>Erste Schritte mit Azure Data Catalog
Azure Data Catalog ist ein vollständig verwalteter Clouddienst, der als Registrierungs- und Ermittlungssystem für Datenassets von Unternehmen dient. Eine ausführliche Übersicht finden Sie unter [Was ist Azure Data Catalog?](data-catalog-what-is-data-catalog.md).

Dieses Tutorial dient als Starthilfe für Azure Data Catalog. In diesem Tutorial führen Sie die folgenden Verfahren durch:

| Vorgehensweise | Beschreibung |
|:--- |:--- |
| [Bereitstellen des Datenkatalogs](#provision-data-catalog) |In diesem Verfahren stellen Sie Azure Data Catalog bereit bzw. führen die Einrichtung dafür durch. Führen Sie diesen Schritt nur aus, wenn der Katalog nicht bereits eingerichtet wurde. Pro Organisation (Microsoft Azure Active Directory-Domäne) kann nur ein Datenkatalog bereitgestellt werden. Dies gilt auch, wenn Ihrem Azure-Konto mehrere Abonnements zugeordnet sind. |
| [Registrieren von Datenassets](#register-data-assets) |In diesem Verfahren registrieren Sie Datenassets aus der AdventureWorks2014-Beispieldatenbank im Datenkatalog. Die Registrierung ist der Prozess des Extrahierens wichtiger struktureller Metadaten, z.B. Namen, Typen und Orte, aus der Datenquelle und des Kopierens dieser Metadaten in den Katalog. Die Datenquellen und ihre Daten bleiben an ihrem Ort, aber die Metadaten werden vom Katalog verwendet, um sie leichter ermittelbar und verständlich zu machen. |
| [Ermitteln von Datenassets](#discover-data-assets) |In diesem Verfahren verwenden Sie das Azure Data Catalog-Portal, um Datenassets zu ermitteln, die im vorherigen Schritt registriert wurden. Nachdem eine Datenquelle in Azure Data Catalog registriert wurde, werden ihre Metadaten vom Dienst indiziert, sodass Benutzer mühelos nach benötigten Daten suchen können. |
| [Versehen von Datenassets mit Anmerkungen](#annotate-data-assets) |In diesem Verfahren geben Sie Anmerkungen (Informationen wie Beschreibungen, Tags, Dokumentation oder Experten) für die Datenassets an. Diese Informationen dienen als Erweiterung der Metadaten, die aus der Datenquelle extrahiert wurden, sowie dem besseren allgemeinen Verständnis der Datenquelle. |
| [Herstellen einer Verbindung mit Datenassets](#connect-to-data-assets) |In diesem Verfahren öffnen Sie Datenassets in integrierten Clienttools (z.B. Excel und SQL Server Data Tools) und einem nicht integrierten Tool (SQL Server Management Studio). |
| [Verwalten von Datenassets](#manage-data-assets) |In diesem Verfahren richten Sie die Sicherheit für Ihre Datenassets ein. Mit Data Catalog erhalten Benutzer keinen Zugriff auf die eigentlichen Daten. Der Besitzer der Datenquelle steuert den Datenzugriff. <br/><br/> Mit Data Catalog können Sie Datenquellen ermitteln und die **Metadaten** der im Katalog registrierten Quellen anzeigen. Mitunter sollen Datenquellen jedoch nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar sein. In solchen Fällen können Sie in Data Catalog den Besitz von im Katalog registrierten Datenassets übernehmen und die Sichtbarkeit Ihrer Assets steuern. |
| [Entfernen von Datenassets](#remove-data-assets) |In diesem Verfahren wird beschrieben, wie Sie Datenassets aus dem Datenkatalog entfernen. |

## <a name="tutorial-prerequisites"></a>Voraussetzungen für das Lernprogramm
### <a name="azure-subscription"></a>Azure-Abonnement
Um den Azure Data Catalog einzurichten, müssen Sie Besitzer oder Mitbesitzer eines Azure-Abonnements sein.

Mit Azure-Abonnements können Sie den Zugriff auf die Clouddienstressourcen wie Azure Data Catalog organisieren. Mit diesen Abonnements können Sie darüber hinaus steuern, wie die Ressourcennutzung gemeldet, abgerechnet und bezahlt wird. Jedes Abonnement kann in Bezug auf Abrechnung und Zahlung unterschiedlich eingerichtet sein, daher können Sie je nach Abteilung, Projekt, Niederlassung usw. verschiedene Abonnements und verschiedene Pläne haben. Jeder Clouddienst gehört zu einem Abonnement, und Sie benötigen ein Abonnement vor dem Einrichten von Azure Data Catalog. Weitere Informationen finden Sie unter [Verwalten von Konten, Abonnements und Administratorrollen](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Wenn Sie über kein Abonnement verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Details hierzu finden Sie in den Informationen zur [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Zum Einrichten von Azure Data Catalog müssen Sie sich mit einem Azure Active Directory (Azure AD)-Benutzerkonto anmelden. Sie müssen der Besitzer oder Mitbesitzer eines Azure-Abonnements sein.  

Azure AD bietet Ihrem Unternehmen sowohl in der Cloud als auch lokal eine einfache Möglichkeit zur Identitäts- und Zugriffsverwaltung. Sie können sich mit einem einzelnen Geschäfts- oder Schulkonto bei beliebigen cloudbasierten oder lokalen Webanwendungen anmelden. Azure Data Catalog verwendet Azure AD für die Authentifizierung bei der Anmeldung. Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-Richtlinienkonfiguration
Es kann vorkommen, dass Sie sich beim Azure Data Catalog-Portal anmelden können, jedoch eine Fehlermeldung erhalten, wenn Sie versuchen, sich beim Tool zum Registrieren von Datenquellen anzumelden. Dieser Fehler kann auftreten, wenn Sie sich im Unternehmensnetzwerk befinden oder von außerhalb des Unternehmensnetzwerks eine Verbindung herstellen.

Das Registrierungstool verwendet die *Formularauthentifizierung* , um Benutzeranmeldungen mit Azure Active Directory zu überprüfen. Um die Anmeldung zu ermöglichen, muss der Azure Active Directory-Administrator die Formularauthentifizierung in der *globalen Authentifizierungsrichtlinie*aktivieren.

Wie im folgenden Screenshot gezeigt, kann die Authentifizierung in der globalen Authentifizierungsrichtlinie separat für Intranet- und Extranetverbindungen aktiviert werden. Anmeldefehler können auftreten, wenn die Formularauthentifizierung für das Netzwerk, über das Sie eine Verbindung herstellen, nicht aktiviert ist.

 ![Globale Authentifizierungsrichtlinie für Azure Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Bereitstellen des Datenkatalogs
Sie können nur einen Datenkatalog pro Organisation (Azure Active Directory-Domäne) bereitstellen. Wenn der Besitzer oder Mitbesitzer eines Azure-Abonnements, der dieser Azure Active Directory-Domäne angehört, bereits einen Katalog erstellt hat, können Sie daher keinen weiteren Katalog erstellen. Dies gilt auch, wenn Sie über mehrere Azure-Abonnements verfügen. Um festzustellen, ob von einem Benutzer in Ihrer Azure Active Directory-Domäne ein Datenkatalog erstellt wurde, navigieren Sie zur [Azure Data Catalog-Startseite](http://azuredatacatalog.com) und überprüfen, ob ein Katalog angezeigt wird. Wenn bereits ein Katalog für Sie erstellt wurde, überspringen Sie das folgende Verfahren, und fahren Sie mit dem nächsten Abschnitt fort.    

1. Navigieren Sie zur [Seite des Data Catalog-Diensts](https://azure.microsoft.com/services/data-catalog) , und klicken Sie auf **Erste Schritte**.
   
    ![Azure Data Catalog – Angebotsseite](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Melden Sie sich mit einem Benutzerkonto an, bei dem es sich um den Besitzer oder Mitbesitzer eines Azure-Abonnements handelt. Nach der Anmeldung wird die folgende Seite angezeigt:
   
    ![Azure Data Catalog – Datenkatalog bereitstellen](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Geben Sie einen **Namen** für den Datenkatalog, das zu verwendende **Abonnement** und den **Speicherort** für den Katalog an.
4. Erweitern Sie **Preise**, und wählen Sie eine Azure Data Catalog-**Edition** (Free oder Standard) aus.
    ![Azure Data Catalog – Edition auswählen](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Erweitern Sie **Katalogbenutzer**, und klicken Sie auf **Hinzufügen**, um Benutzer für den Datenkatalog hinzuzufügen. Sie werden dieser Gruppe automatisch hinzugefügt.
    ![Azure Data Catalog – Benutzer](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Erweitern Sie **Katalogadministratoren**, und klicken Sie auf **Hinzufügen**, um weitere Administratoren für den Datenkatalog hinzuzufügen. Sie werden dieser Gruppe automatisch hinzugefügt.
    ![Azure Data Catalog – Administratoren](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klicken Sie auf **Katalog erstellen** , um den Datenkatalog für Ihre Organisation zu erstellen. Nachdem der Datenkatalog erstellt wurde, wird die Startseite für den Datenkatalog angezeigt.
    ![Azure Data Catalog – Startseite nach dem Erstellen des Katalogs](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Suchen nach einem Datenkatalog im Azure-Portal
1. Navigieren Sie auf einer separaten Registerkarte im Webbrowser oder in einem separaten Webbrowserfenster zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit dem Konto an, das Sie im vorherigen Schritt zum Erstellen des Datenkatalogs verwendet haben.
2. Klicken Sie auf **Durchsuchen** und dann auf **Datenkatalog**.
   
    ![Azure Data Catalog – Durchsuchen von Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) Der von Ihnen erstellte Datenkatalog wird angezeigt.
   
    ![Azure Data Catalog – Katalog in der Liste anzeigen](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Klicken Sie auf den Katalog, den Sie erstellt haben. Im Portal wird das Blatt **Datenkatalog** angezeigt.
   
   ![Azure Data Catalog – Blatt im Portal ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. Sie können die Eigenschaften des Datenkatalogs anzeigen und aktualisieren. Klicken Sie beispielsweise auf **Tarif** , und ändern Sie die Edition.
   
    ![Azure Data Catalog – Tarif](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-Beispieldatenbank
In diesem Tutorial registrieren Sie Datenassets (Tabellen) aus der AdventureWorks2014-Beispieldatenbank für das SQL Server-Datenbankmodul. Sie können aber jede unterstützte Datenquelle verwenden, wenn Sie lieber mit Daten arbeiten möchten, die Ihnen vertraut und für Ihre Rolle relevant sind. Eine Liste der unterstützten Datenquellen finden Sie unter [Von Azure Data Catalog unterstützte Datenquellen](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Installieren der Adventure Works 2014 OLTP-Datenbank
Die Adventure Works-Datenbank unterstützt Standardszenarien zur Onlinetransaktionsverarbeitung für einen fiktiven Fahrradhersteller (Adventure Works Cycles), z.B. für die Bereiche Produkte, Vertrieb und Einkauf. In diesem Tutorial registrieren Sie Informationen zu Produkten in Azure Data Catalog.

So installieren Sie die Adventure Works-Beispieldatenbank:

1. Laden Sie [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) von CodePlex herunter.
2. Folgen Sie zum Wiederherstellen der Datenbank auf Ihrem Computer den Anweisungen unter [Wiederherstellen einer Datenbanksicherung (SQL Server Management Studio)](http://msdn.microsoft.com/library/ms177429.aspx), oder führen Sie die folgenden Schritte aus:
   1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem SQL Server-Datenbankmodul her.
   2. Klicken Sie mit der rechten Maustaste auf **Datenbanken**, und klicken Sie anschließend auf **Datenbank wiederherstellen**.
   3. Klicken Sie unter **Datenbank wiederherstellen** für **Quelle** auf die Option **Gerät** und dann auf **Durchsuchen**.
   4. Klicken Sie unter **Sicherungsmedien auswählen** auf **Hinzufügen**.
   5. Navigieren Sie zum Ordner mit der Datei **AdventureWorks2014.bak**, wählen Sie die Datei aus, und klicken Sie auf **OK**, um das Dialogfeld **Sicherungsdatei suchen** zu schließen.
   6. Klicken Sie auf **OK**, um das Dialogfeld **Sicherungsmedien auswählen** zu schließen.    
   7. Klicken Sie auf **OK**, um das Dialogfeld **Datenbank wiederherstellen** zu schließen.

Jetzt können Sie Datenassets aus der Adventure Works-Beispieldatenbank mithilfe von Azure Data Catalog registrieren.

## <a name="register-data-assets"></a>Registrieren von Datenassets
In dieser Übung verwenden Sie das Registrierungstool, um Datenassets aus der Adventure Works-Datenbank im Katalog zu registrieren. Bei der Registrierung werden wichtige strukturelle Metadaten (z.B. Namen, Typen und Speicherorte) aus der Datenquelle und des darin enthaltenen Assets extrahiert und in den Katalog kopiert. Die Datenquellen und ihre Daten bleiben an ihrem Ort, aber die Metadaten werden vom Katalog verwendet, um sie leichter ermittelbar und verständlich zu machen.

### <a name="register-a-data-source"></a>Registrieren einer Datenquelle
1. Navigieren Sie zur [Azure Data Catalog-Startseite](http://azuredatacatalog.com) , und klicken Sie auf **Daten veröffentlichen**.
   
   ![Azure Data Catalog – Schaltfläche „Daten veröffentlichen“](media/data-catalog-get-started/data-catalog-publish-data.png)
2. Klicken Sie auf **Anwendung starten** , um das Registrierungstool auf Ihren Computer herunterzuladen, zu installieren und auszuführen.
   
   ![Azure Data Catalog – Schaltfläche „Starten“](media/data-catalog-get-started/data-catalog-launch-application.png)
3. Klicken Sie auf der Seite **Willkommen** auf **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein.     
   
    ![Azure Data Catalog – Willkommensseite](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. Klicken Sie auf der Seite **Microsoft Azure Data Catalog** auf **SQL Server** und dann auf **Weiter**.
   
    ![Azure Data Catalog – Datenquellen](media/data-catalog-get-started/data-catalog-data-sources.png)
5. Geben Sie die SQL Server-Verbindungseigenschaften für **AdventureWorks2014** ein (siehe folgendes Beispiel), und klicken Sie auf **VERBINDEN**.
   
   ![Azure Data Catalog – Einstellungen für SQL Server-Verbindung](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Registrieren Sie die Metadaten Ihres Datenassets. In diesem Beispiel registrieren Sie **Production/Product** -Objekte aus dem Namespace „Production“ von AdventureWorks:
   
   1. Erweitern Sie in der **Serverhierarchie** den Knoten **AdventureWorks2014**, und klicken Sie auf **Production**.
   2. Klicken Sie mit gedrückter STRG-Taste nacheinander auf **Product**, **ProductCategory**, **ProductDescription** und **ProductPhoto**, um diese Elemente auszuwählen.
   3. Klicken Sie auf den **Pfeil zum Verschieben der ausgewählten Elemente** (**>**). Mit dieser Aktion werden alle ausgewählten Objekte in die Liste **Zu registrierende Objekte** verschoben.
      
      ![Azure Data Catalog-Tutorial – Objekte durchsuchen und auswählen](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Wählen Sie **Vorschau einschließen** aus, um eine Momentaufnahme der Daten einzubinden. Die Momentaufnahme enthält bis zu 20 Datensätze aus jeder Tabelle und wird in den Katalog kopiert.
   5. Wählen Sie **Datenprofil einschließen** aus, um eine Momentaufnahme der Objektstatistik für das Datenprofil einzubinden (z. B. Mindest-, Höchst- und Mittelwerte für eine Spalte, Anzahl von Zeilen).
   6. Geben Sie **adventure works, cycles** in das Feld **Tags hinzufügen** ein. Mit dieser Aktion werden Suchtags für diese Datenassets hinzugefügt. Tags sind eine hervorragende Möglichkeit, Benutzer beim Suchen nach einer registrierten Datenquelle zu unterstützen.
   7. Geben Sie den Namen eines **Experten** für diese Daten an (optional).
      
      ![Azure Data Catalog-Tutorial – zu registrierende Objekte](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. Klicken Sie auf **REGISTRIEREN**. Azure Data Catalog registriert Ihre ausgewählten Objekte. In dieser Übung werden die ausgewählten Objekte von Adventure Works registriert. Das Registrierungstool extrahiert Metadaten aus dem Datenasset und kopiert diese Daten in den Azure Data Catalog-Dienst. Die Daten verbleiben, wo sie sich derzeit befinden, und unter der Kontrolle der Administratoren und Richtlinien des aktuellen Systems.
      
      ![Azure Data Catalog – registrierte Objekte](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. Klicken Sie zum Anzeigen der registrierten Datenquellenobjekte auf **Portal anzeigen**. Vergewissern Sie sich im Azure Data Catalog-Portal, dass alle vier Tabellen und die Datenbank in der Rasteransicht angezeigt werden.
      
      ![Objekte im Azure Data Catalog-Portal ](media/data-catalog-get-started/data-catalog-view-portal.png)

In dieser Übung haben Sie Objekte aus der Adventure Works-Beispieldatenbank registriert, sodass sie leicht von Benutzern in Ihrer Organisation ermittelt werden können. In der nächsten Übung erfahren Sie, wie Sie registrierte Datenassets ermitteln.

## <a name="discover-data-assets"></a>Ermitteln von Datenassets
Für die Ermittlung in Azure Data Catalog werden zwei primäre Mechanismen verwendet: Suchen und Filtern.

Die Suchfunktion ist intuitiv und leistungsstark. Suchbegriffe werden standardmäßig mit jeder Eigenschaft im Katalog abgeglichen, auch mit den von Benutzern hinzugefügten Anmerkungen.

Die Filterfunktion dient als Ergänzung der Suchfunktion. Sie können bestimmte Merkmale wie Experten, Datenquellentyp, Objekttyp und Tags auswählen, um übereinstimmende Datenassets anzuzeigen und Suchergebnisse auf übereinstimmende Assets zu begrenzen.

Durch eine Kombination von Suchen und Filtern können Sie schnell durch die in Azure Data Catalog registrierten Datenquellen navigieren, um die gewünschten Datenassets zu ermitteln.

In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um Datenassets zu ermitteln, die Sie in der vorherigen Übung registriert haben. Details zur Suchsyntax finden Sie unter [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Referenz zur Data Catalog-Suchsyntax).

Im Folgenden wird die Ermittlung von Datenassets im Katalog anhand einiger Beispiele erläutert.  

### <a name="discover-data-assets-with-basic-search"></a>Ermitteln von Datenassets mit der Standardsuche
Die Standardsuche ermöglicht Ihnen das Durchsuchen des Katalogs anhand eines oder mehrerer Suchbegriffe. Als Ergebnisse werden alle Ressourcen zurückgegeben, bei denen eine beliebige Eigenschaft mit den angegebenen Begriffen übereinstimmt.

1. Klicken Sie im Azure Data Catalog-Portal auf **Start** . Falls Sie den Webbrowser geschlossen haben, wechseln Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com).
2. Geben Sie `cycles` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
   
    ![Azure Data Catalog – Standardtextsuche](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Vergewissern Sie sich, dass alle vier Tabellen und die Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt werden. Sie können wie im folgenden Screenshot dargestellt mit den Schaltflächen auf der Symbolleiste zwischen der **Rasteransicht** und der **Listenansicht** wechseln. Das Suchschlüsselwort wird in den Suchergebnissen hervorgehoben, weil die Option **Hervorheben** **aktiviert** ist. Sie können auch die Anzahl von **Ergebnissen pro Seite** in den Suchergebnissen angeben.
   
    ![Azure Data Catalog – Ergebnisse der Standardtextsuche](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    Links sehen Sie den Bereich **Suchvorgänge** und rechts den Bereich **Eigenschaften**. Im Bereich **Suchvorgänge** können Sie die Suchkriterien ändern und die Ergebnisse filtern. Im Bereich **Eigenschaften** werden die Eigenschaften eines ausgewählten Objekts im Raster oder in der Liste angezeigt.
4. Klicken Sie in den Suchergebnissen auf **Product** . Klicken Sie auf die Registerkarten **Vorschau**, **Spalten**, **Datenprofil** und **Dokumentation**, oder klicken Sie auf den Pfeil, um den unteren Bereich zu erweitern.  
   
    ![Azure Data Catalog – unterer Bereich](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    Auf der Registerkarte **Vorschau** wird eine Vorschau der Daten in der Tabelle **Product** angezeigt.  
5. Klicken Sie auf die Registerkarte **Spalten**, um Details zu Spalten (z. B. **Name** und **Datentyp**) im Datenasset anzuzeigen.
6. Klicken Sie auf die Registerkarte **Datenprofil**, um das Profil von Daten (z. B. Anzahl von Zeilen, Größe der Daten, Mindestwert in einer Spalte) im Datenasset anzuzeigen.
7. Filtern Sie die Ergebnisse mit der Option **Filter** auf der linken Seite. Wenn Sie beispielsweise für **Objekttyp** auf **Tabelle** klicken, werden nur die vier Tabellen angezeigt, und nicht die Datenbank.
   
    ![Azure Data Catalog – Suchergebnisse filtern](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Ermitteln von Datenassets mit der eigenschaftsbasierten Suche
Die eigenschaftsbasierte Suche ermöglicht Ihnen das Ermitteln von Datenassets, bei denen der Suchbegriff mit der angegebenen Eigenschaft übereinstimmt.

1. Deaktivieren Sie unter **Filter** für **Objekttyp** den Filter **Tabelle**.  
2. Geben Sie `tags:cycles` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**. Alle Eigenschaften, die zum Durchsuchen des Datenkatalogs verwendet werden können, finden Sie unter [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Referenz zur Data Catalog-Suchsyntax).
3. Vergewissern Sie sich, dass alle vier Tabellen und die Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt werden.  
   
    ![Data Catalog – Suchergebnisse der eigenschaftsbasierten Suche](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Speichern der Suche
1. Geben Sie im Bereich **Suchvorgänge** des Abschnitts **Aktuelle Suche** einen Namen für die Suche ein, und klicken Sie auf **Speichern**.
   
    ![Azure Data Catalog – Suche speichern](media/data-catalog-get-started/data-catalog-save-search.png)
2. Vergewissern Sie sich, dass die gespeicherte Suche unter **Gespeicherte Suchvorgänge**angezeigt wird.
   
    ![Azure Data Catalog – gespeicherte Suchvorgänge](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Wählen Sie eine der Aktionen aus, die Sie für den gespeicherten Suchvorgang ausführen können (**Umbenennen**, **Löschen**, **Als Standard speichern**).
   
    ![Azure Data Catalog – Optionen für gespeicherte Suchvorgänge](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Boolesche Operatoren
Sie können die Suche mit booleschen Operatoren erweitern oder eingrenzen.

1. Geben Sie `tags:cycles AND objectType:table`in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
2. Vergewissern Sie sich, dass in den Ergebnissen nur Tabellen angezeigt werden (nicht die Datenbank).  
   
    ![Azure Data Catalog – Boolescher Operator der Suche](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Gruppierung mit Klammern
Durch die Gruppierung mit Klammern können Sie Teile der Abfrage gruppieren, um eine logische Isolierung zu erzielen (insbesondere bei booleschen Operatoren).

1. Geben Sie `name:product AND (tags:cycles AND objectType:table)` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
2. Vergewissern Sie sich, dass in den Suchergebnissen nur die Tabelle **Product** angezeigt wird.
   
    ![Azure Data Catalog – Suchvorgänge gruppieren](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vergleichsoperatoren
Mit Vergleichsoperatoren können Sie für Eigenschaften mit numerischen Datentypen und Datumsdatentypen andere Vergleiche als „Gleichheit“ ausführen.

1. Geben Sie im Suchfeld `lastRegisteredTime:>"06/09/2016"`ein.
2. Deaktivieren Sie unter **Objekttyp** den Filter **Tabelle**.
3. Betätigen Sie die **EINGABETASTE**.
4. Vergewissern Sie sich, dass die Tabellen **Product**, **ProductCategory**, **ProductDescription** und **ProductPhoto** sowie die von Ihnen registrierte AdventureWorks2014-Datenbank in den Suchergebnissen angezeigt werden.
   
    ![Azure Data Catalog – Vergleichssuchergebnisse](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Ausführliche Informationen zur Ermittlung von Datenassets finden Sie unter [Ermitteln von Datenquellen](data-catalog-how-to-discover.md). Informationen zur Suchsyntax finden Sie unter [Referenz zur Data Catalog-Suchsyntax](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="annotate-data-assets"></a>Versehen von Datenassets mit Anmerkungen
In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um Datenassets, die Sie zuvor im Katalog registriert haben, mit Anmerkungen zu versehen (d.h. Informationen wie Beschreibungen, Tags oder Experten hinzufügen). Die Anmerkungen dienen als Ergänzung und Erweiterung der strukturellen Metadaten, die während der Registrierung aus der Datenquelle extrahiert wurden, und führen dazu, dass die Datenassets viel besser ermittelbar und verständlich sind.

In dieser Übung versehen Sie ein Datenasset (ProductPhoto) mit Anmerkungen. Sie fügen dem Datenasset „ProductPhoto“ einen Anzeigenamen und eine Beschreibung hinzu.  

1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com), und führen Sie eine Suche mit `tags:cycles` durch, um nach den von Ihnen registrierten Datenassets zu suchen.  
2. Klicken Sie in den Suchergebnissen auf **ProductPhoto** .  
3. Geben Sie für **Anzeigename** den Text **Produktbilder** und für **Beschreibung** den Text **Produktfotos für Marketingmaterial** ein.
   
    ![Azure Data Catalog – ProductPhoto-Beschreibung](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    Die **Beschreibung** hilft anderen Benutzern, das ausgewählte Datenasset zu finden und zu verstehen, warum und wie es verwendet werden sollte. Sie können auch weitere Tags hinzufügen und Spalten anzeigen. Jetzt können Sie versuchen, Datenassets durch Suchen und Filtern der beschreibenden Metadaten zu ermitteln, die Sie dem Katalog hinzugefügt haben.

Auf dieser Seite können Sie auch Folgendes durchführen:

* Fügen Sie Experten für das Datenasset hinzu. Klicken Sie auf **Hinzufügen** in the **Hinzufügen** .
* Fügen Sie Tags auf Datasetebene hinzu. Klicken Sie auf **Hinzufügen** in the **Hinzufügen** . Bei einem Tag kann es sich um ein Benutzertag oder ein Glossartag handeln. Die Standard Edition von Data Catalog enthält ein Unternehmensglossar, mit dem Katalogadministratoren eine globale Unternehmenstaxonomie definieren können. Katalogbenutzer können Datenassets dann mit Glossarbegriffen versehen. Weitere Informationen finden Sie unter [Einrichten des Unternehmensglossars für gesteuertes Markieren](data-catalog-how-to-business-glossary.md)
* Fügen Sie Tags auf Spaltenebene hinzu. Klicken Sie auf **Hinzufügen** under **Hinzufügen** .
* Fügen Sie eine Beschreibung auf Spaltenebene hinzu. Geben Sie eine **Beschreibung** für eine Spalte ein. Sie können auch die Beschreibungsmetadaten anzeigen, die aus der Datenquelle extrahiert wurden.
* Fügen Sie für **Zugriff anfordern** Informationen hinzu, denen Benutzer entnehmen können, wie der Zugriff auf das Datenasset angefordert wird.
  
    ![Azure Data Catalog – Tags, Beschreibungen hinzufügen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* Klicken Sie auf die Registerkarte **Dokumentation** , und geben Sie eine Dokumentation für das Datenasset an. Mit Azure Data Catalog-Dokumentation können Sie Ihren Datenkatalog als Inhaltsrepository zum Erstellen einer vollständigen Textbeschreibung Ihrer Datenassets verwenden.
  
    ![Azure Data Catalog – Registerkarte „Dokumentation“](media/data-catalog-get-started/data-catalog-documentation.png)

Sie können auch Anmerkungen für mehrere Datenassets hinzufügen. Beispielsweise können Sie alle Datenassets auswählen, die Sie registriert haben, und einen Experten dafür angeben.

![Azure Data Catalog – mehrere Datenassets mit Anmerkungen versehen](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Data Catalog unterstützt einen Crowdsourcing-Ansatz für Anmerkungen. Jeder Data Catalog-Benutzer kann Tags (Benutzer oder Glossar), Beschreibungen und andere Metadaten hinzufügen, sodass alle an einem Datenasset und seiner Nutzung beteiligten Benutzer ihre Sichtweise erfassen und anderen Personen zur Verfügung stellen können.

Ausführliche Informationen zum Versehen von Datenassets mit Anmerkungen finden Sie unter [Hinzufügen von Anmerkungen zu Datenquellen](data-catalog-how-to-annotate.md) .

## <a name="connect-to-data-assets"></a>Herstellen einer Verbindung mit Datenassets
In dieser Übung öffnen Sie Datenassets mithilfe von Verbindungsinformationen in einem integrierten Clienttool (Excel) und einem nicht integrierten Tool (SQL Server Management Studio).

> [!NOTE]
> Bedenken Sie, dass Azure Data Catalog Ihnen keinen Zugriff auf die eigentliche Datenquelle bietet – es erleichtert es Ihnen lediglich, die Datenquelle zu ermitteln und zu verstehen. Wenn Sie eine Verbindung mit einer Datenquelle herstellen, verwendet die dafür ausgewählte Clientanwendung Ihre Windows-Anmeldeinformationen, oder Sie werden zur Eingabe Ihrer Anmeldeinformationen aufgefordert. Um auf eine Datenquelle zugreifen zu können, muss Ihnen der Zugriff gewährt werden (sofern noch nicht geschehen).
> 
> 

### <a name="connect-to-a-data-asset-from-excel"></a>Herstellen einer Verbindung mit einem Datenasset über Excel
1. Wählen Sie in den Suchergebnissen den Eintrag **Product** aus. Klicken Sie auf der Symbolleiste auf **Öffnen in** und dann auf **Excel**.
   
    ![Azure Data Catalog – Verbindung mit Datenasset herstellen](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klicken Sie im Downloadpopupfenster auf **Öffnen** . Das angezeigte Fenster kann je nach Browser variieren.
   
    ![Azure Data Catalog – heruntergeladene Excel-Verbindungsdatei](media/data-catalog-get-started/data-catalog-download-open.png)
3. Klicken Sie im Fenster **Microsoft Excel-Sicherheitshinweis** auf **Aktivieren**.
   
    ![Azure Data Catalog – Excel-Sicherheitspopup](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Übernehmen Sie im Dialogfeld **Daten importieren** die Standardeinstellungen, und klicken Sie auf **OK**.
   
    ![Azure Data Catalog – Excel, Daten importieren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Zeigen Sie die Datenquelle in Excel an.
   
    ![Azure Data Catalog – Produkttabelle in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In dieser Übung haben Sie eine Verbindung mit Datenassets hergestellt, die mit Azure Data Catalog ermittelt wurden. Im Azure Data Catalog-Portal können Sie direkt mithilfe der integrierten Clientanwendungen im Menü **Öffnen in** eine Verbindung herstellen. Anhand der Speicherortinformationen in den Assetmetadaten können Sie auch mit jeder anderen Anwendung Ihrer Wahl eine Verbindung herstellen. Beispielsweise können Sie SQL Server Management Studio verwenden, um eine Verbindung mit der AdventureWorks2014-Datenbank herzustellen und auf die Daten in den Datenassets zuzugreifen, die Sie in diesem Tutorial registriert haben.

1. Öffnen Sie **SQL Server Management Studio**.
2. Geben Sie im Dialogfeld **Mit Server verbinden** den Servernamen aus dem Bereich **Eigenschaften** des Azure Data Catalog-Portals ein.
3. Verwenden Sie eine geeignete Authentifizierungsmethode und die entsprechenden Anmeldeinformationen, um auf das Datenasset zuzugreifen. Falls Sie keinen Zugriff haben, fordern Sie ihn anhand der Informationen im Feld **Zugriff anfordern** an.
   
    ![Azure Data Catalog – Zugriff anfordern](media/data-catalog-get-started/data-catalog-request-access.png)

Klicken Sie auf **Verbindungszeichenfolgen anzeigen** , um die ADF.NET-, ODBC- und OLEDB-Verbindungszeichenfolgen anzuzeigen und zur Verwendung in Ihrer Anwendung in die Zwischenablage zu kopieren.

## <a name="manage-data-assets"></a>Verwalten von Datenassets
In diesem Schritt erfahren Sie, wie Sie die Sicherheit für Ihre Datenassets einrichten. Mit Data Catalog erhalten Benutzer keinen Zugriff auf die eigentlichen Daten. Der Besitzer der Datenquelle steuert den Datenzugriff.

Mit Data Catalog können Sie Datenquellen ermitteln und die Metadaten der im Katalog registrierten Quellen anzeigen. Mitunter sollen Datenquellen jedoch nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar sein. In solchen Fällen können Sie in Data Catalog den Besitz von im Katalog registrierten Datenassets übernehmen und dann die Sichtbarkeit Ihrer Assets steuern.

> [!NOTE]
> Die in dieser Übung beschriebenen Verwaltungsfunktionen sind nur in der Standard Edition von Azure Data Catalog verfügbar, nicht in der kostenlosen Edition.
> In Azure Data Catalog können Sie den Besitz von Datenassets übernehmen, Datenassets Mitbesitzer hinzufügen und die Sichtbarkeit von Datenassets festlegen.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Übernehmen des Besitzes von Datenassets und Einschränken der Sichtbarkeit
1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com). Geben Sie `tags:cycles` in das Textfeld **Suche** ein, und drücken Sie die **EINGABETASTE**.
2. Klicken Sie auf ein Element in der Ergebnisliste und dann in der Symbolleiste auf **Besitz übernehmen** .
3. Klicken Sie im Bereich **Eigenschaften** unter **Verwaltung** auf **Besitz übernehmen**.
   
    ![Azure Data Catalog – Besitz übernehmen](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Klicken Sie zum Einschränken der Sichtbarkeit im Abschnitt **Sichtbarkeit** auf **Besitzer und folgende Benutzer** und dann auf **Hinzufügen**. Geben Sie die E-Mail-Adressen der Benutzer in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
   
    ![Azure Data Catalog – Zugriff beschränken](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Entfernen von Datenassets
In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um Vorschaudaten aus registrierten Datenassets zu entfernen und Datenassets aus dem Katalog zu löschen.

In Azure Data Catalog können Sie ein einzelnes Asset oder mehrere Assets löschen.

1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com).
2. Geben Sie `tags:cycles` in das Textfeld **Suche** ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie wie im folgenden Screenshot dargestellt in der Ergebnisliste ein Element aus, und klicken Sie auf der Symbolleiste auf **Löschen** :
   
    ![Azure Data Catalog – Rasterelement löschen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Wenn Sie die Listenansicht verwenden, befindet sich das Kontrollkästchen wie unten gezeigt links neben dem Element:
   
    ![Azure Data Catalog – Listenelement löschen](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    Sie können wie im folgenden Screenshot gezeigt auch mehrere Datenassets auswählen und löschen:
   
    ![Azure Data Catalog – Mehrere Datenassets löschen](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> Das Standardverhalten des Katalogs besteht darin, das Registrieren beliebiger Datenquellen für alle Benutzer zuzulassen und Benutzern auch das Löschen aller registrierten Datenassets zu ermöglichen. Die Verwaltungsfunktionen in der Standard Edition von Azure Data Catalog enthalten zusätzliche Optionen zum Übernehmen des Besitzes von Assets und Einschränken der Benutzer, die Assets ermitteln und löschen können.
> 
> 

## <a name="summary"></a>Zusammenfassung
In diesem Tutorial haben Sie wichtige Funktionen von Azure Data Catalog kennen gelernt, z.B. das Registrieren, Ermitteln und Verwalten von Unternehmensdatenassets sowie das Hinzufügen von Anmerkungen. Nachdem Sie das Lernprogramm abgeschlossen haben, ist es an der Zeit, mit der Verwendung zu beginnen. Sie können noch heute starten, indem Sie die Datenquellen registrieren, die Sie und Ihr Team verwenden, und indem Sie Kollegen zur Nutzung des Katalogs einladen.

## <a name="references"></a>Referenzen
* [Registrieren von Datenassets](data-catalog-how-to-register.md)
* [Ermitteln von Datenassets](data-catalog-how-to-discover.md)
* [Versehen von Datenassets mit Anmerkungen](data-catalog-how-to-annotate.md)
* [Dokumentieren von Datenassets](data-catalog-how-to-documentation.md)
* [Herstellen einer Verbindung mit Datenassets](data-catalog-how-to-connect.md)
* [Verwalten von Datenassets](data-catalog-how-to-manage.md)




<!--HONumber=Jan17_HO3-->


