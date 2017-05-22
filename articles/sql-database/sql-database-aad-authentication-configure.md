---
title: "Konfigurieren der Azure Active Directory-Authentifizierung – SQL| Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie unter Verwendung der Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank und SQL Data Warehouse herstellen.
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 05/05/2017
ms.author: rickbyh
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: f018e265d008cc06631034cea417109c71e7786f
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse

In diesem Artikel erfahren Sie, wie Sie ein Azure AD erstellen und auffüllen und dann dieses Azure AD mit Azure SQL-Datenbank und SQL Data Warehouse verwenden. Eine Übersicht finden Sie unter [Authentifizierung über Azure Active Directory](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Das Herstellen einer Verbindung mit einer SQL Server-Instanz, die auf einer Azure-VM ausgeführt wird, wird für ein Azure Active Directory-Konto nicht unterstützt. Verwenden Sie stattdessen ein Active Directory-Domänenkonto.

## <a name="create-and-populate-an-azure-ad"></a>Erstellen und Auffüllen von Azure AD
Erstellen Sie ein Azure Active Directory-Verzeichnis, und füllen Sie es mit Benutzern und Gruppen. Erstellen der anfänglichen, von Azure AD verwalteten Domäne Azure AD kann auch ein lokaler Active Directory Domain Services im Verbund mit Azure AD sein.

Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx)[ sowie unter Verwalten von Azure AD mit Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0) und [erforderliche Ports und Protokolle für die Hybrid-Identität](../active-directory/active-directory-aadconnect-ports.md).

## <a name="optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
Um Ihre Datenbank dem Azure AD-Verzeichnis für Ihre Organisation zuzuordnen, kennzeichnen Sie das Verzeichnis für das Azure-Abonnement, das die Datenbank hostet, als vertrauenswürdiges Verzeichnis. Weitere Informationen zu Azure-Abonnements finden Sie unter [Wie Azure-Abonnements mit Azure AD verknüpft sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Zusätzliche Informationen:** Jedes Azure-Abonnement weist eine Vertrauensstellung mit einer Azure AD-Instanz auf. Dies bedeutet, dass es diesem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten vertraut. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, ein Abonnement vertraut jedoch nur einem Verzeichnis. Sie können unter [https://manage.windowsazure.com/](https://manage.windowsazure.com/) auf der Registerkarte **Einstellungen** ermitteln, welchem Verzeichnis Ihr Abonnement vertraut. Diese Vertrauensstellung, die ein Abonnement mit einem Verzeichnis aufweist, unterscheidet sich von der Beziehung, die ein Abonnement mit allen anderen Ressourcen in Azure (Websites, Datenbanken usw.) hat. Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind, ebenfalls beendet. Das Verzeichnis verbleibt jedoch in Azure, und Sie können ihm ein anderes Abonnement zuordnen und weiterhin die Benutzer des Verzeichnisses verwalten. Weitere Informationen zu Ressourcen finden Sie unter [Grundlegendes zur Zugriffssteuerung in Azure sowie zur Integration in Azure Active Directory](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Die folgenden Verfahren zeigen Ihnen, wie Sie das zugeordnete Verzeichnis für ein Abonnement ändern.
1. Melden Sie sich als Azure-Abonnementadministrator beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.
2. Wählen Sie auf dem Banner links die Option **EINSTELLUNGEN**aus.
3. Ihre Abonnements werden im Bildschirm mit den Einstellungen angezeigt. Wenn das gewünschte Abonnement nicht angezeigt wird, klicken Sie oben auf **Abonnements**, anschließend auf der Dropdownliste **NACH VERZEICHNIS FILTERN**, und wählen das Verzeichnis aus, das Ihre Abonnements enthält. Klicken Sie dann auf **ANWENDEN**.
   
    ![Abonnement auswählen][4]
4. Klicken Sie im Bereich **Einstellungen** auf Ihr Abonnement, und klicken Sie dann im unteren Seitenbereich auf **VERZEICHNIS BEARBEITEN**.
   
    ![Einstellungen im Portal][5]
5. Wählen Sie im Feld **VERZEICHNIS BEARBEITEN** die Azure Active Directory-Instanz aus, die Ihrer SQL Server- oder SQL Data Warehouse-Instanz zugeordnet ist, und klicken Sie dann auf den Pfeil zum Fortfahren.
   
    ![Verzeichnis auswählen][6]
6. Bestätigen Sie im Dialogfeld **BESTÄTIGEN** für die Verzeichniszuordnung, die Meldung **Alle Co-Administratoren werden entfernt**.
   
    ![Verzeichniszuordnung bestätigen][7]
7. Klicken Sie auf das Häkchen, um das Portal neu zu laden.

   > [!NOTE]
   > Wenn Sie das Verzeichnis ändern, wird der Zugriff für alle Co-Administratoren, Azure AD-Benutzer und -Gruppen und alle verzeichnisgestützten Ressourcenbenutzer entfernt. Ein Zugriff auf dieses Abonnement oder die zugehörigen Ressourcen ist durch diese Benutzer anschließend nicht mehr möglich. Nur Sie als Dienstadministrator können den Zugriff für Prinzipale basierend auf dem neuen Verzeichnis konfigurieren. Es kann längere Zeit dauern, bis diese Änderung an alle Ressourcen weitergegeben wurde. Die Änderung des Verzeichnisses führt auch zu einer Änderung des Azure AD-Administrators für die SQL-Datenbank und SQL Data Warehouse und deaktiviert den Datenbankzugriff für alle bestehenden Azure AD-Benutzer. Der Azure AD-Administrator muss zurückgesetzt werden (siehe Beschreibung unten), und es müssen neue Azure AD-Benutzer erstellt werden.
   >  

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Erstellen eines Azure AD-Administrators für die Azure SQL Server-Instanz
Jede Azure SQL Server-Instanz (mit Hosten von SQL-Datenbank oder SQL Data Warehouse) wird zunächst mit einem einzelnen Serveradministratorkonto konfiguriert, das als Administrator für die gesamte Azure SQL Server-Instanz fungiert. Anschließend muss ein zweiter SQL Server-Administrator erstellt werden, bei dem es sich um ein Azure AD-Konto handelt. Dieser Prinzipal wird als eigenständiger Datenbankbenutzer in der Masterdatenbank erstellt. Als Administratoren sind die Serveradministratorkonten Mitglieder der Rolle **db_owner** in jeder Benutzerdatenbank. Der Zugriff auf alle Benutzerdatenbanken erfolgt als Benutzer **dbo**. Weitere Informationen zu den Serveradministratorkonten finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md).

Bei Verwendung von Azure Active Directory mit Georeplikation muss der Azure Active Directory-Administrator sowohl für den primären als auch für die sekundären Server konfiguriert werden. Wenn ein Server über keinen Azure Active Directory-Administrator verfügt, erhalten Benutzer bei der Azure Active Directory-Anmeldung die Fehlermeldung „Keine Verbindung“.

> [!NOTE]
> Benutzer, die nicht auf einem Azure AD-Konto basieren (hierzu gehört auch das Administratorkonto für die Azure SQL Server-Instanz) können keine Azure AD-basierten Benutzer erstellen, da sie keine Berechtigung zum Überprüfen der vorgeschlagenen Datenbankbenutzer mit Azure AD besitzen.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Bereitstellen eines Azure Active Directory-Administrators für Ihre Azure SQL Server-Instanz

Die beiden folgenden Verfahren zeigen Ihnen, wie Sie einen Azure Active Directory-Administrator für Ihre Azure SQL Server-Instanz im Azure-Portal und mithilfe von PowerShell bereitstellen.

### <a name="azure-portal"></a>Azure-Portal
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) in der oberen rechten Ecke auf Ihre Verbindung, um eine Dropdownliste mit möglichen Active Directory-Verzeichnissen zu öffnen. Wählen Sie das richtige Active Directory-Verzeichnis als Standardeinstellung für Azure AD aus. Durch diesen Schritt wird das Abonnement mit Active Directory und der Azure SQL Server-Instanz verknüpft. So wird sichergestellt, dass das gleiche Abonnement sowohl für Azure AD als auch für SQL Server verwendet wird. (Die Azure SQL Server-Instanz kann entweder Azure SQL-Datenbank oder Azure SQL Data Warehouse hosten.)
   
    ![Administrator auswählen][8]
2. Wählen Sie auf dem Banner links **SQL Server** aus, wählen Sie Ihre **SQL Server-Instanz** aus, und klicken Sie dann oben auf dem Blatt **SQL Server** auf **Einstellungen**.
   
    ![Einstellungen][9]
3. Klicken Sie auf dem Blatt **Einstellungen** auf **Active Directory-Administrator**.
4. Klicken Sie auf dem Blatt **Active Directory-Administrator** auf **Active Directory-Administrator** und klicken Sie anschließend oben auf **Administrator festlegen**.
5. Suchen Sie auf dem Blatt **Administrator hinzufügen**nach einem Benutzer, wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie als Administrator festlegen möchten, und klicken Sie dann auf **Auswählen**. (Auf dem Blatt „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Auf dem Blatt „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Eine Liste der unterstützten Administratoren finden Sie unter **Features und Einschränkungen von Azure AD** weiter oben.) Die rollenbasierte Zugriffskontrolle (Role-based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben.
6. Klicken Sie oben auf dem Blatt **Active Directory-Administrator** auf **SPEICHERN**.

    ![Administrator auswählen][10]
   
    Der Vorgang zum Ändern des Administrators kann einige Minuten in Anspruch nehmen. Anschließend wird der neue Administrator im Feld **Active Directory-Administrator** angezeigt.

   > [!NOTE]
   > Beim Einrichten des Azure AD-Administrators darf der Name des neuen Administrators (Benutzer oder Gruppe) in der virtuellen Masterdatenbank noch nicht als SQL Server-Authentifizierungsbenutzer vorhanden sein. Wenn er vorhanden ist, schlägt die Einrichtung des Azure AD-Administrators fehl, wobei die Erstellung rückgängig gemacht und angegeben wird, dass ein solcher Administrator (Name) bereits vorhanden ist. Da dieser SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, sind alle Versuche erfolglos, unter der Azure AD-Authentifizierung eine Verbindung mit dem Server herzustellen.
   > 


Wenn Sie später einen Administrator entfernen möchten, klicken Sie oben auf dem Blatt **Active Directory-Administrator** auf **Administrator entfernen** und anschließend auf **Speichern**.

### <a name="powershell"></a>PowerShell
Zum Ausführen von PowerShell-Cmdlets muss Azure PowerShell installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

Um einen Azure AD-Administrator bereitzustellen, führen Sie die folgenden Azure PowerShell-Befehle aus:

* Add-AzureRmAccount
* Select-AzureRmSubscription

Verwenden Sie folgende Cmdlets zum Bereitstellen und Verwalten des Azure AD-Administrators:

| Cmdlet-Name | Beschreibung |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Stellt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure SQL Data Warehouse bereit. (Muss aus dem aktuellen Abonnement stammen.) |
| [Remove-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Entfernt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |Gibt Informationen zu dem Azure Active Directory-Administrator zurück, der aktuell für die Azure SQL Server-Instanz oder Azure SQL Data Warehouse konfiguriert ist. |

Verwenden Sie den PowerShell-Befehl "get-help", um weitere Informationen zu diesen Befehlen anzuzeigen. Verwenden Sie z. B. ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Das folgende Skript stellt eine Azure AD-Administratorengruppe namens **DBA_Group** (Objekt-ID `40b79501-b343-44ed-9ce7-da4c8cc7353f`) für den Server **demo_server** in einer Ressourcengruppe mit dem Namen **Group-23** bereit:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

Der Eingabeparameter **DisplayName** akzeptiert entweder den Azure AD-Anzeigenamen oder den Benutzerprinzipalnamen (UPN). Beispiel: ``DisplayName="John Smith"`` und ``DisplayName="johns@contoso.com"``. Für Azure AD-Gruppen wird nur der Azure AD-Anzeigename unterstützt.

> [!NOTE]
> Der Azure PowerShell-Befehl ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` hindert Sie nicht daran, nicht unterstützte Benutzer als Azure AD-Administratoren bereitzustellen. Ein nicht unterstützter Benutzer kann bereitgestellt werden, jedoch keine Verbindung mit einer Datenbank herstellen. 
> 

Im folgenden Beispiel wird der optionale Wert für **ObjectID**verwendet:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Ein Wert für **ObjectID** ist in Azure AD erforderlich, wenn **DisplayName**nicht eindeutig ist. Verwenden Sie den Active Directory-Abschnitt im klassischen Azure-Portal, um die Werte für **ObjectID** und **DisplayName** abzurufen um die Eigenschaften für einen Benutzer oder eine Gruppe anzuzeigen.
> 

Das folgende Beispiel gibt Informationen zum aktuellen Azure AD-Administrator für die Azure SQL Server-Instanz zurück:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Im folgenden Beispiel wird ein Azure AD-Administrator entfernt: 

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Sie können auch einen Azure Active Directory-Administrator mithilfe der REST-APIs bereitstellen. Weitere Informationen finden Sie unter [Vorgänge für Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="configure-your-client-computers"></a>Konfigurieren der Clientcomputer
Auf allen Clientcomputern, über die Ihre Anwendungen oder Benutzer unter Verwendung von Azure AD-Identitäten eine Verbindung mit der Azure SQL-Datenbank oder mit Azure SQL Data Warehouse herstellen, muss die folgende Software installiert werden:

* .NET Framework 4.6 oder höher, erhältlich unter [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* Die Azure Active Directory-Authentifizierungsbibliothek für SQL Server (**ADALSQL.DLL**) steht in verschiedenen Sprachen (sowohl x86 als auch amd64) im Downloadcenter unter [Microsoft Active Directory-Authentifizierungsbibliothek für Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742)bereit.

Sie können diese Anforderungen folgendermaßen erfüllen:

* Durch das Installieren von [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) oder [SQL Server Data Tools für Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) wird die Anforderung erfüllt, dass .NET Framework 4.6 vorhanden sein muss.
* SSMS installiert die x86-Version von **ADALSQL.DLL**.
* SSDT installiert die amd64-Version von **ADALSQL.DLL**.
* Die neueste Visual Studio-Version unter [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) erfüllt die .NET Framework 4.6-Anforderung, die erforderliche amd64-Version von **ADALSQL.DLL**wird jedoch nicht installiert.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind

Für die Azure Active Directory-Authentifizierung ist es erforderlich, dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden. Ein eigenständiger Datenbankbenutzer basierend auf einer Azure AD-Identität ist ein Datenbankbenutzer, der über keine Anmeldung für die Masterdatenbank verfügt, und der einer Identität im Azure AD-Verzeichnis zugeordnet ist, das mit der Datenbank verknüpft ist. Bei der Azure AD-Identität kann es sich entweder um ein einzelnes Benutzerkonto oder um eine Gruppe handeln. Weitere Informationen zu eigenständigen Datenbankbenutzern finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht im Portal erstellt werden. RBAC-Rollen werden nicht auf SQL Server, SQL-Datenbank oder SQL Data Warehouse übertragen. Azure RBAC-Rollen dienen zum Verwalten von Azure-Ressourcen und gelten nicht für Datenbankberechtigungen. Beispielsweise gewährt die Rolle **SQL Server-Mitwirkender** keinen Zugriff zum Herstellen einer Verbindung mit der SQL-Datenbank oder SQL Data Warehouse. Die Zugriffsberechtigung muss direkt in der Datenbank mithilfe von Transact-SQL-Anweisungen erteilt werden.
>

Um einen Azure AD-basierten eigenständigen Datenbankbenutzer zu erstellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mit einer Azure AD-Identität, die mindestens über die Berechtigung **BELIEBIGEN BENUTZER ÄNDERN** verfügt, eine Verbindung mit der Datenbank her. Verwenden Sie anschließend die folgende Transact-SQL-Syntax:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* kann der Benutzerprinzipalname (UPN) eines Azure AD-Benutzers oder der Anzeigename einer Azure AD-Gruppe sein.

**Beispiele:** So erstellen Sie einen eigenständigen Datenbankbenutzer, der einen Benutzer der Azure AD-Verbunddomäne oder einen Benutzer der verwalteten Azure AD-Domäne repräsentiert:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Um einen eigenständigen Datenbankbenutzer zu erstellen, der eine Azure AD-Gruppe oder eine Gruppe der Verbunddomäne repräsentiert, geben Sie den Anzeigenamen einer Sicherheitsgruppe an:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

So erstellen Sie einen eigenständigen Datenbankbenutzer, der eine Anwendung darstellt, die eine Verbindung mit einem Azure AD-Token herstellt:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  Sie können einen Benutzer nur direkt aus der Azure Active Directory-Instanz erstellen, die Ihrem Azure-Abonnement zugeordnet ist. Mitglieder anderer Active Directory-Instanzen, die importierte Benutzer der zugeordneten Active Directory-Instanz sind (bekannt als externe Benutzer) können jedoch einer Active Directory-Gruppe des Active Directory-Mandanten hinzugefügt werden. Indem Sie einen eigenständigen Datenbankbenutzer für diese AD-Gruppe erstellen, können die Benutzer aus der externen Active Directory-Instanz auf die SQL-Datenbank zugreifen.   

Weitere Informationen zum Erstellen eigenständiger Datenbankbenutzer basierend auf Azure Active Directory-Identitäten finden Sie unter [CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Wenn der Azure Active Directory-Administrator für die Azure SQL Server-Instanz entfernt wird, können Azure AD-Authentifizierungsbenutzer keine Verbindung mehr mit dem Server herstellen. Nicht mehr verwendbare Azure AD-Benutzer können bei Bedarf manuell von einem SQL-Datenbankadministrator gelöscht werden.   

>  [!NOTE]
>  Wenn Sie die Benachrichtigung **Das Verbindungstimeout ist abgelaufen.** erhalten, müssen Sie möglicherweise den Parameter `TransparentNetworkIPResolution` der Verbindungszeichenfolge auf FALSE festlegen. Weitere Informationen finden Sie unter [Connection timeout issue with .NET Framework 4.6.1 – TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/) (Verbindungstimeout-Problem mit .NET Framework 4.6.1 – TransparentNetworkIPResolution).   

   
Wenn Sie einen Datenbankbenutzer erstellen, erhält dieser Benutzer die Berechtigung **VERBINDEN** und kann sich als Mitglied der Rolle **ÖFFENTLICH** mit dieser Datenbank verbinden. Anfänglich stehen dem Benutzer nur die Berechtigungen zur Verfügung, die der Rolle **ÖFFENTLICH** gewährt wurden, sowie sämtliche Berechtigungen, die einer Windows-Gruppe erteilt wurden, denen der Benutzer angehört. Nachdem Sie einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitgestellt haben, können Sie dem Benutzer zusätzliche Berechtigungen gewähren. Die Vorgehensweise ist hierbei mit der Berechtigungszuweisung für alle weiteren Benutzertypen identisch. Typischerweise gewähren Sie Datenbankrollen die gewünschten Berechtigungen, und weisen diesen Rollen anschließend Benutzer zu. Weitere Informationen finden Sie unter [Database Engine Permission Basics (in englischer Sprache)](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Weitere Informationen zu besonderen SQL-Datenbankrollen finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md).
Ein Verbunddomänenbenutzer, der in eine verwaltete Domäne importiert wird, muss die Identität der verwalteten Domäne verwenden.

> [!NOTE]
> Azure AD-Benutzer werden in den Datenbankmetadaten mit dem Typ E (EXTERNAL_USER) gekennzeichnet, Gruppen mit dem Typ X (EXTERNAL_GROUPS). Weitere Informationen finden Sie unter [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Herstellen einer Verbindung mit Benutzerdatenbank oder Data Warehouse mithilfe von SQL Server Management Studio oder SQL Server Data Tools
Um zu bestätigen, dass der Azure AD-Administrator ordnungsgemäß eingerichtet ist, stellen Sie mit dem Azure AD-Administratorkonto eine Verbindung mit der Datenbank **master** her.
Um einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitzustellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mithilfe einer Azure AD-Identität, die Zugriff auf die Datenbank hat, eine Verbindung mit der Datenbank her.

> [!IMPORTANT]
> Unterstützung für die Azure Active Directory-Authentifizierung wird über [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015 bereitgestellt. Die SSMS-Version von August 2016 bietet auch Unterstützung für die universelle Active Directory-Authentifizierung, die es Administratoren ermöglicht, die Multi-Factor Authentication per Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App anzufordern.
 
## <a name="using-an-azure-ad-identity-to-connect-using-sql-server-management-studio-or-sql-server-database-tools"></a>Verwenden einer Azure AD-Identität zum Herstellen einer Verbindung mithilfe von SQL Server Management Studio oder SQL Server-Datenbanktools

Die folgenden Verfahren zeigen Ihnen, wie Sie mithilfe von SQL Server Management Studio oder SQL Server-Datenbanktools die Verbindung einer SQL-­Datenbank mit einer Azure AD-Identität herstellen.

### <a name="active-directory-integrated-authentication"></a>Integrierte Active Directory-Authentifizierung

Verwenden Sie diese Methode, wenn Sie mit Ihren Azure Active Directory-Anmeldeinformationen aus einer Verbunddomäne bei Windows angemeldet sind.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im Feld **Authentifizierung** die Einstellung **Integrierte Active Directory-Authentifizierung** aus. Es ist kein Kennwort erforderlich bzw. es kann kein Kennwort eingegeben werden, weil Ihre vorhandenen Anmeldeinformationen zur Verbindungsherstellung verwendet werden.   

    ![Integrierte AD-Authentifizierung auswählen][11]
2. Klicken Sie auf die Schaltfläche **Optionen**, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld  **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der die Verbindung hergestellt werden soll.   

    ![Datenbanknamen auswählen][13]

## <a name="active-directory-password-authentication"></a>Active Directory-Kennwortauthentifizierung

Verwenden Sie diese Methode, wenn Sie unter Verwendung der verwalteten Azure AD-Domäne und mit einem Azure AD-Prinzipalnamen eine Verbindung herstellen. Sie können diese Authentifizierungsmethode auch für ein Verbundkonto ohne Zugriff auf die Domäne verwenden, beispielsweise wenn Sie an einem Remotestandort arbeiten.

Verwenden Sie diese Methode, wenn Sie mit Ihren Windows-Anmeldeinformationen aus einer Domäne angemeldet sind, die nicht im Verbund mit Azure konfiguriert ist, oder wenn Sie die Azure AD-Authentifizierung mit Azure AD basierend auf der Anfangs- oder der Clientdomäne nutzen.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im Feld **Authentifizierung** die Einstellung **Active Directory-Kennwortauthentifizierung** aus.
2. Geben Sie im Feld **Benutzername** Ihren Azure Active Directory-Benutzernamen im Format **username@domain.com** ein. Es muss sich um ein Konto aus Azure Active Directory oder um ein Konto aus einer Domäne handeln, die im Verbund mit Azure Active Directory konfiguriert ist.
3. Geben Sie im Feld **Kennwort** Ihr Benutzerkennwort für das Azure Active Directory-Konto oder das Verbunddomänenkonto ein.

    ![AD-Kennwortauthentifizierung auswählen][12]
4. Klicken Sie auf die Schaltfläche **Optionen**, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld  **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der die Verbindung hergestellt werden soll. (Siehe Grafik in der vorherigen Option.)

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Verwenden einer Azure AD-Identität, um von einer Clientanwendung aus eine Verbindung herzustellen

Die folgenden Verfahren zeigen Ihnen, wie Sie mit einer Azure AD-Identität von einer Clientanwendung aus eine Verbindung mit einer SQL-Datenbank herstellen.

###  <a name="active-directory-integrated-authentication"></a>Integrierte Active Directory-Authentifizierung

Um die integrierte Windows-Authentifizierung zu verwenden, muss das Active Directory Ihrer Domäne mit Azure Active Directory verbunden sein. Ihre Client-Anwendung (oder ein Dienst), die eine Verbindung mit der Datenbank herstellt, muss unter den Domänenanmeldeinformationen eines Benutzers auf einem Computer ausgeführt werden, der der Domäne beigetreten ist.

Um mithilfe der integrierten Authentifizierung und einer Azure AD-Identität eine Verbindung mit einer Datenbank herzustellen, muss das Authentifizierungsschlüsselwort in der Verbindungszeichenfolge für die Datenbank auf "Active Directory Integrated" festgelegt sein. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Das Schlüsselwort ``Integrated Security=True`` in der Verbindungszeichenfolge wird nicht für die Verbindungsherstellung mit Azure SQL-Datenbank unterstützt. Beim Herstellen einer ODBC-Verbindung müssen Sie Leerzeichen entfernen und Authentifizierung auf ActiveDirectoryIntegrated festlegen.

### <a name="active-directory-password-authentication"></a>Active Directory-Kennwortauthentifizierung

Um mithilfe der integrierten Authentifizierung und einer Azure AD-Identität eine Verbindung mit einer Datenbank herzustellen, muss als Authentifizierungsschlüsselwort das Kennwort für Active Directory festgelegt sein. Die Verbindungszeichenfolge muss Benutzer-ID/UID und Kennwort/PWD-Schlüsselwörter enthalten. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Informieren Sie sich über Azure AD-Authentifizierungsmethoden, indem Sie die Demo-Codebeispiele unter [Azure AD Authentication GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)(GitHub-Demo zur Azure AD-Authentifizierung) verwenden.

## <a name="azure-ad-token"></a>Azure AD-Token
Diese Authentifizierungsmethode ermöglicht Diensten der mittleren Ebene, die Verbindung mit der Azure SQL-Datenbank oder Azure SQL Data Warehouse durch Abrufen eines Tokens von Azure Active Directory (AAD) herzustellen. So sind komplexe Szenarien einschließlich der zertifikatbasierten Authentifizierung möglich. Sie müssen vier grundlegende Schritte zum Verwenden der Azure AD-Token-Authentifizierung ausführen:

1. Registrieren Sie Ihre Anwendung in Azure Active Directory, und rufen Sie die Client-ID für den Code ab. 
2. Erstellen Sie einen Datenbankbenutzer, der die Anwendung darstellt. (Bereits in Schritt 6 abgeschlossen.)
3. Erstellen Sie ein Zertifikat auf dem Clientcomputer, auf dem die Anwendung ausgeführt wird.
4. Fügen Sie das Zertifikat als Schlüssel für die Anwendung hinzu.

Beispiel-Verbindungszeichenfolge:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Weitere Informationen finden Sie im [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)(Blog zur Sicherheit von SQL Server).

### <a name="sqlcmd"></a>sqlcmd

Die folgenden Anweisungen stellen eine Verbindung mithilfe von Version 13.1 von SQLCMD her, die im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643)verfügbar ist.

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Nächste Schritte
- Eine Übersicht über den Zugriff und die Steuerung in SQL-Datenbank finden Sie unter [Azure SQL Database access control](sql-database-control-access.md) (SQL-Datenbank – Zugriffssteuerung).
- Eine Übersicht über Anmeldungen, Benutzer und Datenbankrollen in SQL-Datenbank finden Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md).
- Weitere Informationen zu Datenbankprinzipalen finden Sie unter [Prinzipale](https://msdn.microsoft.com/library/ms181127.aspx).
- Weitere Informationen zu Datenbankrollen finden Sie unter [Datenbankrollen](https://msdn.microsoft.com/library/ms189121.aspx).
- Weitere Informationen zu Firewallregeln in SQL-Datenbank finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).
- Ein Tutorial mit SQL Server-Authentifizierung finden Sie unter [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md).
- Ein Tutorial mit Azure Active Directory-Authentifizierung finden Sie unter [Azure AD-Authentifizierung und -Autorisierung](sql-database-control-access-aad-authentication-get-started.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png


