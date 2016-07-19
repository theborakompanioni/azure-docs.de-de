<properties
	pageTitle="Erste Schritte mit Data Catalog | Microsoft Azure"
	description="Umfassendes Tutorial zu den Szenarios und Funktionen von Azure Data Catalog"
	documentationCenter=""
	services="data-catalog"
	authors="steelanddata"
	manager=""
	editor=""
	tags=""/>
<tags
	ms.service="data-catalog"
	ms.devlang="NA"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="NA"
	ms.workload="data-catalog"
	ms.date="07/06/2016"
	ms.author="spelluru"/>

# Tutorial: Erste Schritte mit Azure Data Catalog
Azure Data Catalog ist ein vollständig verwalteter Clouddienst, der als Registrierungs- und Ermittlungssystem für Datenassets von Unternehmen dient. Eine ausführliche Übersicht finden Sie unter [Was ist Azure Data Catalog?](data-catalog-what-is-data-catalog.md).

Dieses Tutorial dient als Starthilfe für Azure Data Catalog. In diesem Tutorial führen Sie die folgenden Schritte aus:

| Schritt | Beschreibung |
| :--- | :---------- |
| [Bereitstellen des Datenkatalogs](#provision-data-catalog) | In diesem Schritt stellen Sie Azure Data Catalog bereit und nehmen die Einrichtung vor. Führen Sie diesen Schritt nur aus, wenn der Katalog nicht bereits eingerichtet wurde. Sie können nur einen Datenkatalog pro Organisation (Azure Active Directory-Domäne) verwenden. Dies gilt auch, wenn Sie Ihrem Azure-Konto mehrere Abonnements zugeordnet haben. | 
| [Registrieren von Datenassets](#register-data-assets) | In diesem Schritt registrieren Sie Datenassets aus der AdventureWorks2014-Beispieldatenbank für den Datenkatalog. Die Registrierung ist der Prozess des Extrahierens wichtiger struktureller Metadaten, z.B. Namen, Typen und Orte, aus der Datenquelle und des Kopierens dieser Metadaten in den Katalog. Die Datenquelle und die dazugehörigen Datenassets bleiben an ihrem Ort, aber die Metadaten werden vom Katalog verwendet, um sie leichter ermittelbar und verständlich zu machen. |
| [Ermitteln von Datenassets](#discover-data-assets) | In diesem Schritt verwenden Sie das Azure Data Catalog-Portal, um Datenassets zu ermitteln, die im vorherigen Schritt registriert wurden. Nachdem eine Datenquelle in Azure Data Catalog registriert wurde, werden ihre Metadaten vom Dienst indiziert, sodass Benutzer benötigte Daten mühelos ermitteln können. |
| [Versehen von Datenassets mit Anmerkungen](#annotate-data-assets) | In diesem Schritt geben Sie Anmerkungen (Beschreibungen, Tags, Dokumentation, Experten usw.) für die Datenassets an, um die aus der Datenquelle extrahierten Metadaten zu erweitern und die Datenquelle für eine größere Zahl von Benutzern besser verständlich zu machen. | 
| [Herstellen einer Verbindung mit Datenassets](#connect-to-data-assets) | In diesem Schritt öffnen Sie Datenassets in integrierten Clienttools wie Excel und SQL Server Data Tools und ein nicht integriertes Tool (SQL Server Management Studio), indem Sie Verbindungsinformationen verwenden. |
| [Verwalten von Datenassets](#manage-data-assets) | In diesem Schritt erfahren Sie, wie Sie die Sicherheit für Ihre Datenassets einrichten. Mit Data Catalog erhalten Benutzer keinen Zugriff auf die eigentlichen Daten. Der Datenzugriff wird vom Besitzer der Datenquelle kontrolliert. <br/><br/>Mit Data Catalog können Benutzer Datenquellen ermitteln und die **Metadaten** anzeigen, die zu den im Katalog registrierten Quellen gehören. Es kann aber zu Situationen kommen, in denen Datenquellen nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar sein sollen. Für diese Szenarien ist es in Data Catalog möglich, dass Benutzer den Besitz von im Katalog registrierten Datenassets übernehmen und dann die Sichtbarkeit ihrer Assets kontrollieren. | 
| [Entfernen von Datenassets](#remove-data-assets) | In diesem Schritt erfahren Sie, wie Sie Datenassets aus dem Datenkatalog entfernen. |  
 
## Voraussetzungen für das Lernprogramm

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

### Azure-Abonnement
Um Azure Data Catalog einzurichten, müssen Sie **Besitzer oder Mitbesitzer** eines Azure-Abonnements sein.

Mit Azure-Abonnements können Sie den Zugriff auf die Clouddienstressourcen wie Azure Data Catalog organisieren. Mit diesen Abonnements können Sie darüber hinaus steuern, wie die Ressourcennutzung gemeldet, abgerechnet und bezahlt wird. Jedes Abonnement kann in Bezug auf Abrechnung und Zahlung unterschiedlich eingerichtet sein, daher können Sie je nach Abteilung, Projekt, Niederlassung usw. verschiedene Abonnements und verschiedene Pläne haben. Jeder Clouddienst gehört zu einem Abonnement, und Sie benötigen ein Abonnement vor dem Einrichten von Azure Data Catalog. Weitere Informationen finden Sie unter [Verwalten von Konten, Abonnements und Administratorrollen](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Wenn Sie über kein Abonnement verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Details hierzu finden Sie unter [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

### Azure Active Directory
Zum Einrichten von Azure Data Catalog müssen Sie mit einem **Azure Active Directory-Benutzerkonto** angemeldet sein, und der Benutzer muss der Besitzer oder Mitbesitzer eines Azure-Abonnements sein.

Azure Active Directory (Azure AD) bietet Ihrem Unternehmen eine einfache Möglichkeit für die Identitäts- und Zugriffsverwaltung – in der Cloud und lokal. Benutzer können ein einzelnes Arbeits- oder Schulkonto für das einmalige Anmelden bei Webanwendungen in der Cloud oder lokalen Webanwendung verwenden. Azure Data Catalog verwendet Azure AD für die Authentifizierung bei der Anmeldung. Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](../active-directory/active-directory-whatis.md).

### Active Directory-Richtlinienkonfiguration

Es gibt Situationen, in denen Benutzer sich beim Azure Data Catalog-Portal anmelden können, jedoch bei dem Versuch, sich beim Tool zum Registrieren von Datenquellen anzumelden, eine Fehlermeldung erhalten, die die Anmeldung verhindert. Dieses Problem tritt möglicherweise nur dann auf, wenn der Benutzer über das Unternehmensnetzwerk arbeitet oder wenn der Benutzer von außerhalb des Unternehmensnetzwerks eine Verbindung herstellt.

Das Registrierungstool verwendet die **Formularauthentifizierung**, um Benutzeranmeldungen für Active Directory zu überprüfen. Für eine erfolgreiche Anmeldung muss die Formularauthentifizierung durch einen Active Directory-Administrator in der **globalen Authentifizierungsrichtlinie** aktiviert sein.

Mit der globalen Authentifizierungsrichtlinie können separate Authentifizierungsmethoden für Intranet- und Extranetverbindungen aktiviert werden, wie nachstehend veranschaulicht. Anmeldefehler können auftreten, wenn die Formularauthentifizierung nicht für das Netzwerk aktiviert ist, aus dem der Benutzer eine Verbindung herstellt.

 ![Globale Authentifizierungsrichtlinie für Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

## Bereitstellen des Datenkatalogs
Sie können nur einen Datenkatalog pro Organisation (Azure Active Directory-Domäne) bereitstellen. Falls der Besitzer oder Mitbesitzer eines Azure-Abonnements, der dieser Active Directory-Domäne angehört, bereits einen Katalog erstellt hat, können Sie keinen Katalog mehr erstellen. Dies gilt auch, wenn Sie über mehrere Azure-Abonnements verfügen. Um zu testen, ob ein Datenkatalog von einem Benutzer in Ihrer Active Directory-Domäne erstellt wurde, navigieren Sie zu http://azuredatacatalog.com und überprüfen, ob der Katalog angezeigt wird. Überspringen Sie das folgende Verfahren, und fahren Sie mit dem nächsten Abschnitt fort, wenn bereits ein Katalog für Sie erstellt wurde.

1. Navigieren Sie zu [https://azure.microsoft.com/services/data-catalog](https://azure.microsoft.com/services/data-catalog),

	![Azure Data Catalog – Marketing-Startseite](media/data-catalog-get-started/data-catalog-marketing-landing-page.png) und klicken Sie auf **Erste Schritte**.
2. Melden Sie sich mit einem Benutzerkonto an, bei dem es sich um den **Besitzer oder Mitbesitzer** eines Azure-Abonnements handelt. Nach dem erfolgreichen Anmelden sollte die folgende Seite angezeigt werden:

	![Azure Data Catalog – Datenkatalog bereitstellen](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
2. Geben Sie einen **Namen** für den Datenkatalog, das zu verwendende **Abonnement** und den **Speicherort** für den Katalog an.
3. Erweitern Sie die Option **Preise**, und geben Sie die Azure Data Catalog-**Edition** (Free und Standard) an. ![Azure Data Catalog – Edition auswählen](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
4. Erweitern Sie **Katalogbenutzer**, und klicken Sie auf **Hinzufügen**, um Benutzer für den Datenkatalog hinzuzufügen. Sie werden dieser Gruppe automatisch hinzugefügt. ![Azure Data Catalog – Benutzer](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
5. Erweitern Sie **Katalogadministratoren**, und klicken Sie auf **Hinzufügen**, um weitere Administratoren für den Datenkatalog hinzuzufügen. Sie werden dieser Gruppe automatisch hinzugefügt. ![Azure Data Catalog – Administratoren](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
6. Klicken Sie auf die Schaltfläche **Katalog erstellen**, um den Datenkatalog für Ihre Organisation zu erstellen. Nach der erfolgreichen Erstellung wird die Startseite für den Datenkatalog angezeigt. ![Azure Data Catalog – Erstellt](media/data-catalog-get-started/data-catalog-created.png)

### Suchen nach dem Datenkatalog im Azure-Portal
1. Navigieren Sie auf einer separaten Registerkarte im Webbrowser oder in einem separaten Webbrowserfenster zu [https://portal.azure.com](https://portal.azure.com), und melden Sie sich mit demselben Konto an, das Sie im vorherigen Schritt zum Erstellen des Datenkatalogs verwendet haben.
2. Klicken Sie auf **Durchsuchen** und dann auf **Datenkatalog**.

	![Azure Data Catalog – Azure-Portal durchsuchen](media/data-catalog-get-started/data-catalog-browse-azure-portal.png)
3. Der erstellte Datenkatalog wird angezeigt.

	![Azure Data Catalog – Katalog in Liste anzeigen](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klicken Sie auf den Katalog, den Sie erstellt haben. Im Portal sollte nun das Blatt **Datenkatalog** angezeigt werden.

	![Azure Data Catalog – Blatt im Portal](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Sie können die Eigenschaften des Datenkatalogs anzeigen und aktualisieren. Klicken Sie beispielsweise auf **Tarif**, und ändern Sie die Edition.

	![Azure Data Catalog – Tarif](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### Adventure Works-Beispieldatenbank 
In diesem Tutorial registrieren Sie Datenassets (Tabellen) aus der AdventureWorks2014-Beispieldatenbank für das SQL Server-Datenbankmodul. Sie können aber eine beliebige unterstützte Datenquelle verwenden, wenn Sie lieber mit Daten arbeiten möchten, die vertraut und für Ihre Rolle relevant sind. Eine Liste mit den unterstützten Datenquellen finden Sie unter [Unterstützte Datenquellen](data-catalog-dsr.md).

### Installieren der Adventure Works 2014 OLTP-Datenbank
Die Adventure Works-Datenbank unterstützt standardmäßige Onlinetransaktionsverarbeitung-Szenarien für einen fiktiven Fahrradhersteller (Adventure Works Cycles), z. B. für die Bereiche Produkte, Vertrieb und Einkauf. In diesem Tutorial registrieren Sie Informationen zu Produkten in **Azure Data Catalog**.

Hier wird nun beschrieben, wie Sie die Adventure Works-Beispieldatenbank installieren:

1. Laden Sie [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) von CodePlex herunter.
2. Führen Sie die Schritte im Artikel [Wiederherstellen einer Datenbanksicherung (SQL Server Management Studio)](http://msdn.microsoft.com/library/ms177429.aspx) aus, um die Datenbank auf Ihrem Computer wiederherzustellen. **Kurzanleitung**:
	1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem SQL Server-Datenbankmodul her.
	2. Klicken Sie mit der rechten Maustaste auf **Datenbanken**, und wählen Sie **Datenbank wiederherstellen**.
	3. Wählen Sie im Dialogfeld **Datenbank wiederherstellen** für **Quelle** die Option **Gerät**, und klicken Sie auf **Durchsuchen (...)**.
	4. Klicken Sie im Dialogfeld **Sicherungsmedien auswählen** auf **Hinzufügen**.
	5. Navigieren Sie zum Ordner mit der Datei **AdventureWorks2014.bak**, wählen Sie die Datei aus, und klicken Sie auf **OK**, um das Dialogfeld **Sicherungsdatei suchen** zu schließen.
	6. Klicken Sie auf **OK**, um das Dialogfeld **Sicherungsmedien auswählen** zu schließen.
	7. Klicken Sie auf **OK**, um das Dialogfeld **Datenbank wiederherstellen** zu schließen.

Als Nächstes wird beschrieben, wie Sie Datenassets aus der Adventure Works-Beispieldatenbank in **Azure Data Catalog** registrieren.

## Registrieren von Datenassets

In dieser Übung verwenden Sie das Registrierungstool, um Datenassets aus der Adventure Works-Datenbank im Katalog zu registrieren. Die Registrierung ist der Prozess zum Extrahieren wichtiger struktureller Metadaten – z. B. Namen, Typen und Orte – aus der Datenquelle und des darin enthaltenen Assets sowie zum Kopieren dieser Metadaten in den Katalog. Die Datenquellen und ihre Daten bleiben an ihrem Ort, aber die Metadaten werden vom Katalog verwendet, um sie leichter ermittelbar und verständlich zu machen.

### So registrieren Sie eine Datenquelle

1.	Navigieren Sie zu [https://azuredatacatalog.com](https://azuredatacatlog.com), und klicken Sie auf der Startseite auf **Daten veröffentlichen**.

    ![Azure Data Catalog – Schaltfläche „Daten veröffentlichen“](media/data-catalog-get-started/data-catalog-publish-data.png)

3.	Klicken Sie auf **Anwendung starten**, um das **Registrierungstool** auf Ihren Computer herunterzuladen und zu installieren und auszuführen.

    ![Azure Data Catalog – Schaltfläche „Starten“](media/data-catalog-get-started/data-catalog-launch-application.png)

4. Klicken Sie auf der Seite **Willkommen** auf **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein.

	![Azure Data Catalog – Dialogfeld „Willkommen“](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

5. Doppelklicken Sie auf der Seite **Microsoft Azure Data Catalog** auf **SQL Server**, oder klicken Sie auf **SQL Server** und dann auf **Weiter**.

    ![Azure Data Catalog – Datenquellen](media/data-catalog-get-started/data-catalog-data-sources.png)

6.	Geben Sie die SQL Server-Verbindungseigenschaften für **AdventureWorks2014** ein (siehe Beispiel unten), und klicken Sie auf **VERBINDEN**.

    ![Azure Data Catalog – Einstellungen für SQL Server-Verbindung](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

7.	Auf der nächsten Seite registrieren Sie die Metadaten des Datenassets. In diesem Beispiel registrieren Sie **Production/Product**-Objekte aus dem Production-Namespace von AdventureWorks. Gehen Sie wie folgt vor:
    
	1. Erweitern Sie in der **Serverhierarchie** die Option **AdventureWorks2014**, und klicken Sie auf **Production**.
	2. Führen Sie STRG+Klicken auf **Product**, **ProductCategory**, **ProductDescription** und **ProductPhoto** aus.
	3. Klicken Sie auf den **Pfeil zum Verschieben der ausgewählten Elemente** (**>**). Alle ausgewählten Product-Objekte werden in die Liste **Zu registrierende Objekte** verschoben.
			
    	![Azure Data Catalog-Tutorial – Objekte durchsuchen und auswählen](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
	4. Wählen Sie **Vorschau einschließen**, um eine Momentaufnahme der Daten einzuschließen. Die Momentaufnahme enthält bis zu 20 Datensätze aus jeder Tabelle und wird in den Katalog kopiert.
	5. Wählen Sie **Datenprofil einschließen**, um eine Momentaufnahme der Objektstatistik für das Datenprofil einzuschließen (z.B. Mindestwert, Höchstwert und Mittelwert für eine Spalte, Anzahl von Zeilen usw.).
	5. Geben Sie unter **Tags hinzufügen** den Text **adventure works, cycles** ein. Hiermit werden Suchtags für diese Datenassets hinzugefügt. Tags sind eine hervorragende Möglichkeit, Benutzer beim Suchen nach einer registrierten Datenquelle zu unterstützen.
	6. (Optional) Geben Sie den Namen eines **Experten** für diese Daten an.
	
    	![Azure Data Catalog-Tutorial – Zu registrierende Objekte](media/data-catalog-get-started/data-catalog-objects-register.png)
    
	7. Klicken Sie auf **REGISTRIEREN**. Azure Data Catalog registriert Ihre ausgewählten Objekte. In dieser Übung werden die ausgewählten Objekte von Adventure Works registriert. Das Registrierungstool extrahiert Metadaten aus dem Datenasset und kopiert diese Daten in den Azure Data Catalog-Dienst. Die Daten verbleiben, wo sie sich derzeit befinden, und unter der Kontrolle der Administratoren und Richtlinien des aktuellen Systems.
	
		![Azure Data Catalog – Registrierte Objekte](media/data-catalog-get-started/data-catalog-registered-objects.png)

	8. Klicken Sie zum Anzeigen der registrierten Datenquellenobjekte auf **Portal anzeigen**. Vergewissern Sie sich im **Azure Data Catalog**-Portal, dass alle vier Tabellen und die Datenbank in der Rasteransicht angezeigt werden.
 
    	![Objekte im Azure Data Catalog-Portal](media/data-catalog-get-started/data-catalog-view-portal.png)
  
	
In dieser Übung haben Sie Objekte aus der Adventure Works-Beispieldatenbank registriert, damit sie von Benutzern in Ihrem Unternehmen leicht ermittelt werden können. In der nächsten Übung erfahren Sie, wie Sie registrierte Datenassets ermitteln.

## Ermitteln von Datenassets
Für die Ermittlung in Azure Data Catalog werden zwei primäre Mechanismen verwendet: Suchen und Filtern.

Die **Suchfunktion** ist intuitiv und leistungsstark. Suchbegriffe werden standardmäßig mit jeder Eigenschaft im Katalog abgeglichen, auch mit den von Benutzern hinzugefügten Anmerkungen.

Die **Filterfunktion** dient als Ergänzung der Suchfunktion. Sie können bestimmte Merkmale wie Experten, Datenquellentyp, Objekttyp und Tags auswählen, um nur übereinstimmende Datenressourcen anzuzeigen und Suchergebnisse auf übereinstimmende Ressourcen zu begrenzen.

Durch eine Kombination von Suchen und Filtern können Sie schnell durch die in Azure Data Catalog registrierten Datenquellen navigieren, um die gewünschten Datenassets zu ermitteln.

In dieser Übung verwenden Sie das **Azure Data Catalog**-Portal, um Datenassets zu ermitteln, die Sie in der vorherigen Übung registriert haben. Details zur Suchsyntax finden Sie unter [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Referenz zur Data Catalog-Suchsyntax).

Wir sehen uns nun einige Beispiele für die Ermittlung von Datenassets im Katalog an.

### Standardsuche
Bei der Standardsuche können Sie den Katalog mit einem oder mehreren Suchbegriffen durchsuchen. Als Ergebnisse werden alle Ressourcen zurückgegeben, bei denen eine beliebige Eigenschaft mit den angegebenen Begriffen übereinstimmt.

1. Klicken Sie im Azure Data Catalog-Portal auf die Schaltfläche **Home**. Navigieren Sie zu [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com), wenn Sie den Webbrowser geschlossen haben.
2. Geben Sie oben im Suchfeld den Text **cycles** ein, und klicken Sie auf das Symbol **Suche**, oder drücken Sie die **EINGABETASTE**.
	
	![Azure Data Catalog – Standardsuche nach Text](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Vergewissern Sie sich, dass alle vier Tabellen und die Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt werden. Sie können zwischen der **Rasteransicht** und der **Listenansicht** umschalten, indem Sie oben auf die Schaltflächen klicken. Dies ist in der folgenden Abbildung dargestellt. Beachten Sie, dass das Suchschlüsselwort in den Suchergebnissen hervorgehoben ist, weil oben die Option **Hervorheben** auf **EIN** festgelegt ist. Sie können auch die Anzahl von **Ergebnissen pro Seite** in den Suchergebnissen angeben.

	![Azure Data Catalog – Ergebnisse der Standardsuche nach Text](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
	
	Links ist der Bereich **Suchen** und rechts der Bereich **Eigenschaften** zu sehen. Im Suchbereich können Sie die Suchkriterien ändern und die Ergebnisse filtern. Im Bereich „Eigenschaften“ werden die Eigenschaften eines ausgewählten Objekts im Raster bzw. in der Liste angezeigt.

4. Klicken Sie in den Suchergebnissen auf **Product**. Klicken Sie auf die Registerkarten mit der Bezeichnung **Vorschau**, **Spalten**, **Datenprofil** und **Dokumentation**, oder verwenden Sie den Pfeil nach oben, um den unteren Bereich in der Mitte zu erweitern.
 
	![Azure Data Catalog – Unterer Bereich](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
	
	Auf der Registerkarte **Vorschau** sollte die Vorschau der Daten in der Tabelle „Product“ angezeigt werden.
5. Klicken Sie auf die Registerkarte **Spalten**, um Details zu den Spalten (z.B. **Name** und **Datentyp**) im Datenasset anzuzeigen.
6. Klicken Sie auf die Registerkarte **Datenprofil**, um die Profilerstellung der Daten (z.B. Anzahl von Zeilen, Größe der Daten, Mindestwert in einer Spalte usw.) im Datenasset anzuzeigen.
6. Filtern Sie die Ergebnisse, indem Sie links die Option **Filter** verwenden. Wenn Sie beispielsweise für **Objekttyp** auf **Tabelle** klicken, sollten nur die vier Tabellen angezeigt werden, und nicht die Datenbank.

	![Azure Data Catalog – Suchergebnisse filtern](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### Eigenschaftsbasierte Suche
Bei der eigenschaftsbasierten Suche können Sie Datenassets ermitteln, für die der Suchbegriff mit der angegebenen Eigenschaft übereinstimmt.

3. Deaktivieren Sie den Filter **Tabelle** für **Objekttyp** unter **Filter**.
4. Geben Sie im Suchfeld den Text **tags:cycles** ein, und klicken Sie auf das Symbol **Suche**, oder drücken Sie die **EINGABETASTE**. Alle Eigenschaften, die Sie zum Durchsuchen des Datenkatalogs verwenden können, finden Sie unter [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Referenz zur Data Catalog-Suchsyntax).
3. Vergewissern Sie sich, dass alle vier Tabellen und die Datenbank (AdventureWorks2014) in den Ergebnissen angezeigt werden.

	![Data Catalog – Suchergebnisse der eigenschaftsbasierten Suche](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### Speichern der Suche 
1. Klicken Sie links im Suchbereich im Abschnitt „Aktuelle Suche“ auf „Speichern“, um die aktuellen Suchkriterien zu speichern. Geben Sie einen Namen für die Suche ein, und klicken Sie auf „Speichern“.
	
	![Azure Data Catalog – Suche speichern](media/data-catalog-get-started/data-catalog-save-search.png)
2. Vergewissern Sie sich, dass die gespeicherte Suche unter „Gespeicherte Suchvorgänge“ angezeigt wird.

	![Azure Data Catalog – Gespeicherte Suchen](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Klicken Sie auf den Pfeil nach unten, um die Aktionen anzuzeigen, die Sie für die gespeicherte Suche durchführen können (Umbenennen, Löschen, Als Standardsuche festlegen). ![Azure Data Catalog – Optionen für gespeicherte Suchen](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### Boolesche Operatoren
Mit booleschen Operatoren können Sie eine Suche erweitern oder einschränken.

2. Geben Sie im Suchfeld den Text **tags:cycles AND objectType:table** ein, und klicken Sie auf das Symbol **Suche**, oder drücken Sie die **EINGABETASTE**.
3. Vergewissern Sie sich, dass in den Ergebnissen nur Tabellen angezeigt werden, und keine Datenbank.

	![Azure Data Catalog – Boolescher Operator der Suche](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### Gruppierung mit Klammern
Die Gruppierung mit Klammern ermöglicht es Ihnen, Teile der Abfrage mit Klammern zu gruppieren, um eine logische Isolierung zu erzielen (insbesondere in Verbindung mit booleschen Operatoren).

1. Geben Sie im Suchfeld den Text **name:product AND (tags:cycles AND objectType:table)** ein, und klicken Sie auf das Symbol **Suche**, oder drücken Sie die **EINGABETASTE**.
2. Vergewissern Sie sich, dass in den Suchergebnissen jetzt nur die Tabelle **Product** angezeigt wird.

	![Azure Data Catalog – Suche gruppieren](media/data-catalog-get-started/data-catalog-grouping-search.png)

### Vergleichsoperatoren
Mit Vergleichsoperatoren können Sie für Eigenschaften mit numerischen Datentypen und Datumsdatentypen andere Vergleiche als „Gleichheit“ verwenden.

1. Geben Sie im Suchfeld **lastRegisteredTime:>"06/09/2016"** ein.
2. Deaktivieren Sie den Filter **Tabelle** für **Objekttyp** auf der linken Seite.
3. Klicken Sie auf das Symbol **Suche**, oder drücken Sie die **EINGABETASTE**.
2. Vergewissern Sie sich, dass die Tabellen „Product“, „ProductCategory“, „ProductDescription“ und „ProductPhoto“ und die registrierte AdventureWorks2014-Datenbank in den Suchergebnissen angezeigt werden.

	![Azure Data Catalog – Vergleichssuchergebnisse](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Ausführliche Informationen zur Ermittlung von Datenassets finden Sie unter [Ermitteln von Datenassets](data-catalog-how-to-discover.md), und Informationen zur Suchsyntax finden Sie unter [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Referenz zur Data Catalog-Suchsyntax).

## Versehen von Datenassets mit Anmerkungen
In dieser Übung verwenden Sie das **Azure Data Catalog**-Portal, um Datenassets, die Sie zuvor im Katalog registriert haben, mit Anmerkungen zu versehen (Hinzufügen von Beschreibungen, Tags, Experten usw.). Die von Ihnen angegebenen Anmerkungen dienen als Ergänzung und Erweiterung der strukturellen Metadaten, die während der Registrierung aus der Datenquelle extrahiert wurden, und führen dazu, dass die Datenassets viel besser ermittelbar und verständlich sind.

### So versehen Sie Datenassets mit Anmerkungen
In diesem Schritt versehen Sie ein einzelnes Dataset (ProductPhoto) mit Anmerkungen. Sie fügen dem ProductPhoto-Datenasset einen Anzeigenamen, eine Beschreibung usw. hinzu.

1.  Navigieren Sie nach dem Schließen des Browsers zu [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com), und führen Sie eine Suche mit **tags:cycles** durch, um nach den registrierten Datenassets zu suchen.
2. Klicken Sie in den Suchergebnissen auf **ProductPhoto**.
3. Geben Sie als **Anzeigename** den Text **Produktbilder** und im Feld **Beschreibung** den Text **Produktfotos für Marketingmaterial** ein.

	![Azure Data Catalog – ProductPhoto-Beschreibung](media/data-catalog-get-started/data-catalog-productphoto-description.png)

	Mit der **Beschreibung** können andere Benutzer die Daten besser ermitteln und verstehen, warum und wie das ausgewählte Datenasset verwendet werden kann. Sie können auch mehrere Tags hinzufügen und Spalten anzeigen. Jetzt können Sie das Suchen und Filtern ausprobieren, um Datenassets anhand der beschreibenden Metadaten zu ermitteln, die Sie dem Katalog hinzugefügt haben.

Auf dieser Seite können Sie auch Folgendes durchführen:

- Fügen Sie Experten für das Datenasset hinzu. Klicken Sie im rechten Bereich unter **Experten:** auf **Hinzufügen...**.
- Fügen Sie Tags auf Datasetebene hinzu. Klicken Sie im rechten Bereich unter **Tags:** auf **Hinzufügen...**. Bei einem Tag kann es sich um ein Benutzertag oder ein Glossartag handeln. Die Standard Edition von Data Catalog umfasst ein Unternehmensglossar, mit dem Katalogadministratoren eine globale Unternehmenstaxonomie definieren können. Katalogbenutzer können Datenassets dann mit Glossarbegriffen versehen. Weitere Informationen finden Sie unter [Einrichten des Unternehmensglossars für gesteuertes Markieren](data-catalog-how-to-business-glossary.md).
- Fügen Sie Tags auf Spaltenebene hinzu. Klicken Sie im unteren Bereich in der Mitte für die Spalte, die Sie mit Anmerkungen versehen möchten, unter **Tags** auf **Hinzufügen…**.
- Fügen Sie eine Beschreibung auf Spaltenebene hinzu. Geben Sie im unteren Bereich in der Mitte für eine Spalte eine **Beschreibung** ein. Sie können auch die Beschreibungsmetadaten anzeigen, die aus der Datenquelle extrahiert wurden.
- Fügen Sie Informationen für **Zugriff anfordern** hinzu, um für Benutzer anzugeben, wie der Zugriff auf das Datenasset angefordert werden kann.

	![Azure Data Catalog – Tags, Beschreibungen hinzufügen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
  
- Klicken Sie im unteren Bereich in der Mitte auf die Registerkarte **Dokumentation**, und geben Sie eine Dokumentation für das Datenasset an. Mit der Azure Data Catalog-Dokumentation können Sie Ihren Data Catalog als Inhaltsrepository zum Erstellen einer vollständigen Textbeschreibung Ihrer Datenassets verwenden.

	![Azure Data Catalog – Registerkarte „Dokumentation“](media/data-catalog-get-started/data-catalog-documentation.png)


Sie können auch eine Mehrfachauswahl treffen oder alle Einträge auswählen und mehrere bzw. alle Datenassets mit einer Anmerkung versehen. Beispielsweise können Sie alle Datenassets auswählen, die Sie registriert haben, und einen Experten dafür angeben.

![Azure Data Catalog – Mehrere Datenassets mit Anmerkungen versehen](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Beim Crowdsourcing-Ansatz von Azure Data Catalog für Anmerkungen können Data Catalog-Benutzer Tags (Benutzer oder Glossar), Beschreibungen und andere Metadaten hinzufügen, damit alle an einem Datenasset und seiner Nutzung beteiligten Benutzer ihre Sichtweise erfassen und anderen Personen zur Verfügung stellen können.

Ausführliche Informationen zum Versehen von Datenassets mit Anmerkungen finden Sie unter [Versehen von Datenassets mit Anmerkungen](data-catalog-how-to-annotate.md).
 
## Herstellen einer Verbindung mit Datenassets
In dieser Übung öffnen Sie Datenressourcen in einem integrierten Clienttool (Excel) und in einem nicht integrierten Tool (SQL Server Management Studio), indem Sie die Verbindungsinformationen verwenden.

> [AZURE.NOTE] Es ist wichtig zu beachten, dass **Azure Data Catalog** Benutzern keinen Zugriff auf die eigentliche Datenquelle ermöglicht – es erleichtert Benutzern lediglich, sie zu ermitteln und zu verstehen. Wenn Benutzer eine Verbindung mit einer Datenquelle herstellen, werden von der gewählten Clientanwendung deren Windows-Anmeldeinformationen verwendet, oder Benutzer werden bei Bedarf zur Eingabe ihrer Anmeldeinformationen aufgefordert. Falls dem Benutzer bisher noch kein Zugriff auf die Datenquelle gewährt wurde, muss dies erfolgen, bevor er die Verbindung herstellen kann.

### So stellen Sie über Excel eine Verbindung mit einem Datenasset her

1. Wählen Sie in den Suchergebnissen den Eintrag **Product** aus. Klicken Sie in der Symbolleiste auf **Öffnen in**, und wählen Sie **Excel**.
 
    ![Azure Data Catalog – Verbindung mit Datenasset herstellen](media/data-catalog-get-started/data-catalog-connect1.png)
5. Klicken Sie unten im Download-Popupfenster auf **Öffnen** (kann je nach Browser variieren).

	![Azure Data Catalog – Heruntergeladene Excel-Verbindungsdatei](media/data-catalog-get-started/data-catalog-download-open.png)
6. Klicken Sie im Fenster **Microsoft Excel-Sicherheitshinweis** auf **Aktivieren**.

	![Azure Data Catalog – Sicherheitspopup für Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
7. Behalten Sie im Dialogfeld **Daten importieren** die Standardwerte bei, und klicken Sie auf **OK**.

	![Azure Data Catalog – Excel, Daten importieren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
8. Die Datenquelle wird in Excel geöffnet.

    ![Azure Data Catalog – Produkttabelle in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In dieser Übung haben Sie Verbindungen mit Datenassets hergestellt, die per **Azure Data Catalog** ermittelt wurden. Mit dem **Azure Data Catalog**-Portal können Benutzer Verbindungen direkt mit Clientanwendungen herstellen, die in das Menü **Öffnen in…** integriert sind. Außerdem können Benutzer eine Verbindung mit einer beliebigen Anwendung herstellen, indem sie die Speicherortinformationen der Verbindung in den Assetmetadaten verwenden. Beispiel: Sie können SQL Server Management Studio verwenden, um eine Verbindung mit der AdventureWorks2014-Datenbank herzustellen und auf die Daten in den Datenassets zuzugreifen, die für dieses Tutorial registriert wurden.

1. Starten Sie **SQL Server Management Studio**.
2. Geben Sie im Dialogfeld **Mit Server verbinden** im Azure Data Catalog-Portal unter **Eigenschaften** den **Servernamen** ein.
3. Verwenden Sie die richtigen Daten für die **Authentifizierung** und die **Anmeldeinformationen**, um auf das Datenasset zuzugreifen, wenn Sie bereits über Zugriff auf das Datenasset verfügen. Falls nicht, können Sie die Informationen im Feld „Zugriff anfordern“ verwenden, um Zugriff zu erhalten.

	![Azure Data Catalog – Zugriff anfordern](media/data-catalog-get-started/data-catalog-request-access.png)

Klicken Sie auf **Verbindungszeichenfolgen anzeigen**, um die ADF.NET-, ODBC- und OLEDB-Verbindungszeichenfolgen anzuzeigen und zur Verwendung in Ihrer Anwendung in die Zwischenablage zu kopieren.

## Verwalten von Datenassets
In diesem Schritt erfahren Sie, wie Sie die Sicherheit für Ihre Datenassets einrichten. Mit Data Catalog erhalten Benutzer keinen Zugriff auf die eigentlichen Daten. Der Datenzugriff wird vom Besitzer der Datenquelle kontrolliert.

Mit Data Catalog können Benutzer Datenquellen ermitteln und die **Metadaten** anzeigen, die zu den im Katalog registrierten Quellen gehören. Es kann aber zu Situationen kommen, in denen Datenquellen nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar sein sollen. Für diese Szenarien ist es in Data Catalog möglich, dass Benutzer den Besitz von im Katalog registrierten Datenassets übernehmen und dann die Sichtbarkeit ihrer Assets kontrollieren.

> [AZURE.NOTE] Die in dieser Übung beschriebenen Verwaltungsfunktionen sind nur in der **Standard Edition von Azure Data Catalog** verfügbar, nicht in der **Free Edition**. In **Azure Data Catalog** können Sie Datenassets in Besitz nehmen, Datenassets Mitbesitzer hinzufügen und die Sichtbarkeit von Datenassets festlegen.

### So nehmen Sie Datenassets in Besitz und schränken die Sichtbarkeit ein

1. Navigieren Sie zu [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com), wenn Sie den Webbrowser geschlossen haben. Geben Sie im Feld für den Suchtext **tags:cycles** ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie in der Ergebnisliste einen Eintrag aus (z.B. **Product**), indem Sie oben rechts auf das Kontrollkästchen klicken, und klicken Sie dann in der Symbolleiste auf **Besitz übernehmen**. Dies ist in der folgenden Abbildung dargestellt.
4. Klicken Sie im Bereich **Eigenschaften** rechts im Abschnitt **Verwaltung** auf **Besitz übernehmen**.

	![Azure Data Catalog – Besitz übernehmen](media/data-catalog-get-started/data-catalog-take-ownership.png)
5. Klicken Sie zum Einschränken der Sichtbarkeit im Abschnitt **Sichtbarkeit** auf **Besitzer und folgende Benutzer** und dann auf **Hinzufügen**. Geben Sie die E-Mail-Adresse des Benutzers in das Textfeld ein, und drücken Sie die EINGABETASTE.

    ![Azure Data Catalog – Zugriff beschränken](media/data-catalog-get-started/data-catalog-ownership.png)

## Entfernen von Datenassets

In dieser Übung verwenden Sie das **Azure Data Catalog**-Portal, um Vorschaudaten aus registrierten Datenassets zu entfernen und Datenassets aus dem Katalog zu löschen.

In **Azure Data Catalog** können Sie ein einzelnes Asset oder mehrere Assets löschen.

### So löschen Sie Datenassets

1. Navigieren Sie zu [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com), wenn Sie den Webbrowser geschlossen haben.
2. Geben Sie im Feld für den Suchtext **tags:cycles** ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie in der Ergebnisliste einen Eintrag aus (z.B. **ProductDescription**), indem Sie oben rechts auf das Kontrollkästchen klicken, und klicken Sie dann in der Symbolleiste auf **Löschen**. Dies ist in der folgenden Abbildung dargestellt.

	![Azure Data Catalog – Rasterelement löschen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
	
	Wenn Sie die Listenansicht verwenden (anstelle der Rasteransicht), befindet sich das Kontrollkästchen wie unten gezeigt links vom Eintrag.

	![Azure Data Catalog – Listenelement löschen](media/data-catalog-get-started/data-catalog-delete-list-item.png)

	Sie können wie unten gezeigt auch mehrere Datenassets auswählen und löschen:

	![Azure Data Catalog – Mehrere Datenassets löschen](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Das Standardverhalten des Katalogs besteht darin, das Registrieren beliebiger Datenquellen für alle Benutzer zuzulassen und Benutzern auch das Löschen aller registrierten Datenassets zu ermöglichen. Die Verwaltungsfunktionen in der **Standard Edition von Azure Data Catalog** umfassen zusätzliche Optionen zum Festlegen des Besitzes von Assets, Einschränken der Ermittlung von Assets und Einschränken des Löschens von Assets.


## Zusammenfassung

In diesem Tutorial haben Sie wichtige Funktionen von **Azure Data Catalog** kennengelernt, z.B. das Registrieren, Ermitteln und Verwalten der Datenassets von Unternehmen sowie das Versehen mit Anmerkungen. Nachdem Sie das Lernprogramm abgeschlossen haben, ist es an der Zeit, mit der Verwendung zu beginnen. Sie können noch heute starten, indem Sie die Datenquellen registrieren, die Sie und Ihr Team verwenden, und indem Sie Kollegen zur Nutzung des Katalogs einladen.

## Referenzen

- [Registrieren von Datenassets](data-catalog-how-to-register.md)
- [Ermitteln von Datenassets](data-catalog-how-to-discover.md)
- [Versehen von Datenassets mit Anmerkungen](data-catalog-how-to-annotate.md)
- [Dokumentieren von Datenassets](data-catalog-how-to-documentation.md)
- [Herstellen einer Verbindung mit Datenassets](data-catalog-how-to-connect.md)
- [Verwalten von Datenassets](data-catalog-how-to-manage.md)

<!---HONumber=AcomDC_0713_2016-->