<properties
   pageTitle="Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung | Microsoft Azure"
   description="Erfahren Sie, wie Sie unter Verwendung der Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank herstellen."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung

Die Azure Active Directory-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Microsoft Azure SQL-Datenbank und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Mithilfe der Azure Active Directory-Authentifizierung können Sie die Identitäten von Datenbankbenutzern und andere Microsoft-Dienste an einer zentralen Stelle verwalten. Die zentrale ID-Verwaltung ermöglicht eine einheitliche Verwaltung von Datenbankbenutzern und vereinfacht die Berechtigungsverwaltung. Daraus ergeben sich u. a. die folgenden Vorteile:

- Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
- Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
- Es wird eine Kennwortrotation über eine zentrale Stelle ermöglicht.
- Kunden können Datenbankberechtigungen mithilfe von externen Gruppen (AAD) verwalten.
- Durch das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen wird das Speichern von Kennwörtern überflüssig.
- Die Azure Active Directory-Authentifizierung verwendet eigenständige Datenbankbenutzer zum Authentifizieren von Identitäten auf Datenbankebene.
- Azure Active Directory unterstützt die tokenbasierte Authentifizierung für Anwendungen, die sich mit SQL-Datenbank verbinden.
- Die Azure Active Directory-Authentifizierung unterstützt AD FS (Domänenverbund) sowie native Benutzer-/Kennwortauthentifizierung für ein lokales Azure Active Directory ohne Domänensynchronisierung.
- Azure Active Directory unterstützt Verbindungen von SQL Server Management Studio, bei denen universelle Active Directory-Authentifizierungsverfahren verwendet werden, zu denen auch die Multi-Factor Authentication (MFA) gehört. MFA bietet eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

Die Konfigurationsschritte schließen die folgenden Verfahren zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung ein:

1. Erstellen und Auffüllen eines Azure Active Directory-Verzeichnisses
2. Sicherstellen, dass eine Datenbank in Azure SQL-Datenbank V12 vorliegt (Bei SQL Data Warehouse nicht erforderlich.)
3. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
4. Erstellen eines Azure Active Directory-Administrators für Azure SQL Server oder [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)
5. Konfigurieren der Clientcomputer
6. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
7. Herstellen einer Verbindung mit Ihrer Datenbank unter Verwendung von Azure AD-Identitäten


## Architektur von Vertrauensstellungen

Das folgende Diagramm bietet eine Übersicht über die Lösungsarchitektur für die Azure AD-Authentifizierung mit Azure SQL-Datenbank. Die gleichen Konzepte gelten für SQL Data Warehouse. Zur Unterstützung des nativen Benutzerkennworts von Azure Active Directory werden nur der Cloudanteil und Azure AD/Azure SQL-Datenbank berücksichtigt. Zur Unterstützung der Verbundauthentifizierung (oder von Benutzername und Kennwort für Windows-Anmeldeinformationen) ist die Kommunikation mit dem AD FS-Block erforderlich. Die Pfeile zeigen die Kommunikationswege.

![Diagramm zur AAD-Authentifizierung][1]

Das folgende Diagramm zeigt die Verbund-, Vertrauensstellungs- und Hostbeziehungen, die einem Client durch Übermittlung eines Tokens die Verbindungsherstellung mit einer Datenbank ermöglichen. Das Token wird von einem Azure Active Directory authentifiziert, und die Datenbank vertraut dem Token. Bei „Customer1“ kann es sich um ein Azure Active Directory mit nativen Benutzern oder um ein Azure Active Directory mit Verbundbenutzern handeln. „Customer2“ stellt eine mögliche Lösung einschließlich importierter Benutzer dar. In diesem Beispiel stammen diese aus einem dem Verbund angehörenden Azure Active Directory, und AD FS wird mit Azure Active Directory synchronisiert. Wichtig: Für den Zugriff auf eine Datenbank mithilfe der Azure AD-Authentifizierung muss das Abonnement, das als Host fungiert, dem Azure Active Directory zugeordnet sein. Das gleiche Abonnement muss auch verwendet werden, um die SQL Server-Instanz zu erstellen, auf der die Azure SQL-Datenbank oder das SQL Data Warehouse gehostet werden.

![Abonnementbeziehung][2]

## Administratorstruktur

Bei Verwendung der Azure AD-Authentifizierung sind zwei Administratorkonten für den SQL-Datenbank-Server vorhanden: der ursprüngliche SQL Server-Administrator und der Azure AD-Administrator. Die gleichen Konzepte gelten für SQL Data Warehouse. Nur der auf einem Azure AD-Konto basierende Administrator kann den ersten eigenständigen Azure AD-Datenbankbenutzer in einer Benutzerdatenbank erstellen. Das Konto für die Azure AD-Administratoranmeldung kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn es sich bei dem Administrator um ein Gruppenkonto handelt, kann es von einem beliebigen Gruppenmitglied verwendet werden, sodass mehrere Azure AD-Administratoren die SQL Server-Instanz verwalten können. Die Verwendung eines Gruppenkontos für den Administrator ermöglicht es Ihnen, Gruppenmitglieder in Azure AD zentral hinzuzufügen und zu entfernen, ohne die Benutzer oder Berechtigungen in SQL-Datenbank zu ändern. Es kann jeweils nur ein Azure AD-Administrator (ein Benutzer oder eine Gruppe) konfiguriert werden.

![Administratorstruktur][3]

## Berechtigungen

Um neue Benutzer zu erstellen, müssen Sie über die Berechtigung `ALTER ANY USER` in der Datenbank verfügen. Die Berechtigung `ALTER ANY USER` kann jedem Datenbankbenutzer gewährt werden. Die Berechtigung `ALTER ANY USER` haben auch Serveradministratorkonten inne, ebenso wie Datenbankbenutzer mit der Berechtigung `CONTROL ON DATABASE` oder `ALTER ON DATABASE` für diese Datenbank sowie Mitglieder der Datenbankrolle `db_owner`.

Für die Erstellung eines eigenständigen Datenbankbenutzers in Azure SQL-Datenbank oder SQL Data Warehouse müssen Sie unter Verwendung einer Azure AD-Identität eine Verbindung mit der Datenbank herstellen. Für den ersten eigenständigen Datenbankbenutzer muss unter Verwendung eines Azure Active Directory-Administrators (Besitzer der Datenbank) eine Verbindung mit der Datenbank hergestellt werden. Dies wird nachstehend in den Schritten 4 und 5 gezeigt. Eine Azure Active Directory-Authentifizierung ist nur möglich, wenn der Azure Active Directory-Administrator für den Azure SQL-Datenbankserver oder den SQL Data Warehouse-Server erstellt wurde. Falls der Azure Active Directory-Administrator vom Server entfernt wurde, können vorhandene Azure Active Directory-Benutzer, die zuvor in SQL Server erstellt wurden, nicht mehr mithilfe ihrer Azure Active Directory-Anmeldeinformationen auf die Datenbank zugreifen.

## Funktionen und Einschränkungen von Azure AD

Die folgenden Elemente von Azure Active Directory können in Azure SQL Server und SQL Data Warehouse bereitgestellt werden:

- Systemeigene Elemente: ein in Azure AD erstelltes Element in der verwalteten Domäne oder einer Kundendomäne. Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Domänennamen zum Vereinfachen des Anmeldevorgangs für Benutzer](../active-directory/active-directory-add-domain.md).

- Mitglieder der Verbunddomäne: in Azure AD mit einer Verbunddomäne erstellte Mitglieder. Weitere Informationen finden Sie unter [Microsoft Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory).

- Aus anderen Azure Active Directory-Instanzen importierte Mitglieder, die systemeigene Mitglieder oder Mitglieder der Verbunddomäne sind.

- Active Directory-Gruppen, die als Sicherheitsgruppen erstellt wurden.

Microsoft-Konten (beispielsweise "outlook.com", "hotmail.com", "live.com") oder andere Gastkonten (z. B. "gmail.com", "yahoo.com") werden nicht unterstützt. Wenn Sie sich mit dem Konto und Kennwort bei [https://login.live.com](https://login.live.com) anmelden können, verwenden Sie ein Microsoft-Konto. Dies wird für die Azure AD-Authentifizierung für Azure SQL-Datenbank oder Azure SQL Data Warehouse nicht unterstützt.

### Zusätzliche Überlegungen

- Um die Verwaltungsmöglichkeiten zu verbessern, wird empfohlen, eine dedizierte Azure Active Directory-Gruppe als Administrator bereitzustellen.
- Es kann immer nur ein einzelner Azure AD-Administrator (Benutzer oder Gruppe) für eine Azure SQL Server- oder Azure SQL Data Warehouse-Instanz konfiguriert werden.
- Nur ein Azure Active Directory-Administrator für SQL Server kann unter Verwendung eines Azure Active Directory-Kontos erstmals eine Verbindung mit der Azure SQL Server- oder Azure SQL Data Warehouse-Instanz herstellen. Der Active Directory-Administrator kann weitere Azure Active Directory-Datenbankbenutzer konfigurieren.
- Es wird empfohlen, das Verbindungstimeout auf 30 Sekunden festzulegen.
- SQL Server 2016 Management Studio und SQL Server Data Tools für Visual Studio 2015 (ab Version 14.0.60311.1April 2016) unterstützen die Azure Active Directory-Authentifizierung. (Die Azure Active Directory-Authentifizierung wird vom **.NET Framework-Datenanbieter für SqlServer** ab .NET Framework 4.6 unterstützt.) Daher können die neuesten Versionen dieser Tools und Datenschichtanwendungen (DAC und BACPAC) die Azure Active Directory-Authentifizierung verwenden.
- [ODBC Version 13.1](https://www.microsoft.com/download/details.aspx?id=53339) unterstützt die Azure Active Directory-Authentifizierung, aber für `bcp.exe` kann keine Verbindung per Azure Active Directory-Authentifizierung hergestellt werden, da hierfür ein älterer ODBC-Anbieter verwendet wird.
- `sqlcmd` unterstützt die Azure Active Directory-Authentifizierung ab Version 13.1, verfügbar im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).
- SQL Server Data Tools für Visual Studio 2015 erfordert mindestens die Data Tools-Version von April 2016 (Version 14.0.60311.1). Azure Active Directory-Benutzer werden derzeit nicht im SSDT-Objekt-Explorer angezeigt. Die Benutzer können aber in [sys.database\_principals](https://msdn.microsoft.com/library/ms187328.aspx) angezeigt werden.
- [Microsoft JDBC-Treiber 6.0 für SQL Server](https://www.microsoft.com/de-DE/download/details.aspx?id=11774) unterstützt die Azure Active Directory-Authentifizierung. Siehe auch [Einstellen der Verbindungseigenschaften](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase kann sich nicht per Azure Active Directory-Authentifizierung authentifizieren.
- Einige Tools wie BI und Excel werden nicht unterstützt.
- Die Azure Active Directory-Authentifizierung wird für SQL-Datenbank auf den Blättern **Datenbank importieren** und **Datenbank exportieren** des Azure-Portals unterstützt. Import- und Exportvorgänge mit Azure Active Directory-Authentifizierung werden auch per PowerShell-Befehl unterstützt.


## 1\. Erstellen und Auffüllen von Azure AD

Erstellen Sie ein Azure Active Directory-Verzeichnis, und füllen Sie es mit Benutzern und Gruppen. Das Azure Active Directory kann die von Azure AD verwaltete Anfangsdomäne sein. Das Azure Active Directory kann auch ein lokaler Active Directory-Domänendienst im Verbund mit dem Azure Active Directory sein.

Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](../active-directory/active-directory-add-domain.md), [Microsoft Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory), [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx) sowie unter [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## 2\. Sicherstellen, dass Version 12 von SQL-Datenbank verwendet wird

Die Azure Active Directory-Authentifizierung wird in der aktuellen Version unterstützt, SQL-Datenbank V12. Informationen zu SQL-Datenbank V12 und darüber, ob diese Version in Ihrer Region verfügbar ist, finden Sie unter [Neuerungen in SQL-Datenbank V12](sql-database-v12-whats-new.md). Dieser Schritt ist für Azure SQL Data Warehouse nicht erforderlich, da SQL Data Warehouse nur in V12 verfügbar ist.

Wenn Sie über eine vorhandene Datenbank verfügen, prüfen Sie, ob diese in SQL-Datenbank V12 gehostet wird, indem Sie (beispielsweise über SQL Server Management Studio) eine Verbindung mit der Datenbank herstellen und `SELECT @@VERSION;` ausführen. Die erwartete Ausgabe für eine Datenbank in SQL-Datenbank V12 ist mindestens **Microsoft SQL Azure (RTM) – 12.0**. Wenn Ihre Datenbank nicht in SQL-Datenbank V12 gehostet wird, finden Sie weitere Informationen unter [Planen und Vorbereiten des Upgrades auf die SQL-Datenbank V12](sql-database-v12-plan-prepare-upgrade.md). Besuchen Sie dann das klassische Azure-Portal, um die Datenbank nach SQL-Datenbank V12 zu migrieren.

Alternativ können Sie eine neue Datenbank in SQL-Datenbank V12 erstellen, indem Sie die unter [Erstellen einer ersten Azure SQL-Datenbank](sql-database-get-started.md) aufgeführten Schritte ausführen. **Tipp**: Lesen Sie die Informationen im nächsten Schritt, bevor Sie ein Abonnement für Ihre neue Datenbank auswählen.

## 3\. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement

Um Ihre Datenbank dem Azure AD-Verzeichnis für Ihre Organisation zuzuordnen, kennzeichnen Sie das Verzeichnis für das Azure-Abonnement, das die Datenbank hostet, als vertrauenswürdiges Verzeichnis. Weitere Informationen zu Azure-Abonnements finden Sie unter [Wie Azure-Abonnements mit Azure AD verknüpft sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Zusätzliche Informationen:** Jedes Azure-Abonnement weist eine Vertrauensstellung mit einer Azure AD-Instanz auf. Dies bedeutet, dass es diesem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten vertraut. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, ein Abonnement vertraut jedoch nur einem Verzeichnis. Sie können unter [https://manage.windowsazure.com/](https://manage.windowsazure.com/) auf der Registerkarte **Einstellungen** ermitteln, welchem Verzeichnis Ihr Abonnement vertraut. Diese Vertrauensstellung, die ein Abonnement mit einem Verzeichnis aufweist, unterscheidet sich von der Beziehung, die ein Abonnement mit allen anderen Ressourcen in Azure (Websites, Datenbanken usw.) hat. Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind, ebenfalls beendet. Das Verzeichnis verbleibt jedoch in Azure, und Sie können ihm ein anderes Abonnement zuordnen und weiterhin die Benutzer des Verzeichnisses verwalten. Weitere Informationen zu Ressourcen finden Sie unter [Grundlegendes zur Zugriffssteuerung in Azure sowie zur Integration in Azure Active Directory](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Die folgenden Verfahren zeigen Schrittanleitungen dazu, wie Sie das zugeordnete Verzeichnis für ein Abonnement ändern.

1. Melden Sie sich als Azure-Abonnementadministrator beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.
2. Wählen Sie auf dem Banner links die Option **EINSTELLUNGEN** aus.
3. Ihre Abonnements werden im Bildschirm mit den Einstellungen angezeigt. Wenn das gewünschte Abonnement nicht angezeigt wird, klicken Sie oben auf **Abonnements**, klicken Sie auf die Dropdownliste **NACH VERZEICHNIS FILTERN**, und wählen Sie das Verzeichnis aus, das Ihre Abonnements enthält. Klicken Sie dann auf **ANWENDEN**.

	![Abonnement auswählen][4]
4. Klicken Sie im Bereich **Einstellungen** auf Ihr Abonnement, und klicken Sie dann im unteren Seitenbereich auf **VERZEICHNIS BEARBEITEN**.

	![Einstellungen im Portal][5]
5. Wählen Sie im Feld **VERZEICHNIS BEARBEITEN** die Azure Active Directory-Instanz aus, die Ihrer SQL Server- oder SQL Data Warehouse-Instanz zugeordnet ist, und klicken Sie dann auf den Pfeil zum Fortfahren.

	![Verzeichnis auswählen][6]
6. Bestätigen Sie im Dialogfeld **BESTÄTIGEN** für die Verzeichniszuordnung, die Meldung **Alle Co-Administratoren werden entfernt.**

	![Verzeichniszuordnung bestätigen][7]
7. Klicken Sie auf das Häkchen, um das Portal neu zu laden.

> [AZURE.NOTE] Wenn Sie das Verzeichnis ändern, wird der Zugriff für alle Co-Administratoren, Azure AD-Benutzer und -Gruppen und alle verzeichnisgestützten Ressourcenbenutzer entfernt. Ein Zugriff auf dieses Abonnement oder die zugehörigen Ressourcen ist durch diese Benutzer anschließend nicht mehr möglich. Nur Sie als Dienstadministrator können den Zugriff für Prinzipale basierend auf dem neuen Verzeichnis konfigurieren. Es kann längere Zeit dauern, bis diese Änderung an alle Ressourcen weitergegeben wurde. Die Änderung des Verzeichnisses führt auch zu einer Änderung des Azure AD-Administrators für die SQL-Datenbank und SQL Data Warehouse und deaktiviert den Datenbankzugriff für alle bestehenden Azure AD-Benutzer. Der Azure AD-Administrator muss zurückgesetzt werden (siehe Beschreibung unten), und es müssen neue Azure AD-Benutzer erstellt werden.

## 4\. Erstellen eines Azure AD-Administrators für Azure SQL Server

Jede Azure SQL Server-Instanz (mit Hosten von SQL-Datenbank oder SQL Data Warehouse) wird zunächst mit einem einzelnen Serveradministratorkonto konfiguriert, das als Administrator für die gesamte Azure SQL Server-Instanz fungiert. Anschließend muss ein zweiter SQL Server-Administrator erstellt werden, bei dem es sich um ein Azure AD-Konto handelt. Dieser Prinzipal wird als eigenständiger Datenbankbenutzer in der Masterdatenbank erstellt. Als Administratoren sind die Serveradministratorkonten Mitglieder der Rolle **db\_owner** in jeder Benutzerdatenbank. Der Zugriff auf alle Benutzerdatenbanken erfolgt als Benutzer **dbo**. Weitere Informationen zu den Serveradministratorkonten finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md) sowie im Abschnitt **Anmeldungen und Benutzer** unter [Sicherheitsrichtlinien und Einschränkungen von Azure SQL-Datenbank](sql-database-security-guidelines.md).

Bei Verwendung von Azure Active Directory mit Geo-Replikation muss der Azure Active Directory-Administrator sowohl für den primären als auch für die sekundären Server konfiguriert werden. Wenn ein Server über keinen Azure Active Directory-Administrator verfügt, erhalten Benutzer bei der Azure Active Directory-Anmeldung die Fehlermeldung „Keine Verbindung“.

> [AZURE.NOTE] Benutzer, die nicht auf einem Azure AD-Konto basieren (hierzu gehört auch das Azure SQL Server-Administratorkonto) können keine Azure AD-basierten Benutzer erstellen, da sie keine Berechtigung zum Überprüfen der vorgeschlagenen Datenbankbenutzer mit Azure AD besitzen.

### Bereitstellen eines Azure Active Directory-Administrators für Ihre Azure SQL Server-Instanz unter Verwendung des Azure-Portals

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) in der oberen rechten Ecke auf Ihre Verbindung, um eine Dropdownliste mit möglichen Active Directory-Verzeichnissen zu öffnen. Wählen Sie das richtige Active Directory-Verzeichnis als Standardeinstellung für Azure AD aus. Durch diesen Schritt wird das Abonnement mit Active Directory und Azure SQL Server verknüpft. So wird sichergestellt, dass dasselbe Abonnement sowohl für Azure AD als auch für SQL Server verwendet wird. (Der Azure SQL Server kann entweder Azure SQL-Datenbank oder Azure SQL Data Warehouse hosten.)

	![Administrator auswählen][8]
2. Wählen Sie auf dem Banner links **SQL Server**, wählen Sie Ihre SQL Server-Instanz, und klicken Sie dann oben auf dem Blatt **SQL Server** auf **Einstellungen**.

	![Einstellungen][9]
3. Klicken Sie auf dem Blatt **Einstellungen** auf „Active Directory-Administrator“.
4. Klicken Sie auf dem Blatt **Active Directory-Administrator** auf **Active Directory-Administrator**, und klicken Sie dann oben auf **Administrator festlegen**.
5. Suchen Sie auf dem Blatt **Administrator hinzufügen** nach einem Benutzer, wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie als Administrator festlegen möchten, und klicken Sie dann auf **Auswählen**. (Auf dem Blatt „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. Benutzer oder Gruppen, die abgeblendet dargestellt werden, können nicht ausgewählt werden, da sie nicht als Azure AD-Administratoren unterstützt werden. Eine Liste der unterstützten Administratoren finden Sie unter **Features und Einschränkungen von Azure AD** weiter oben.) Die rollenbasierte Zugriffskontrolle (Role-based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben.
6. Klicken Sie oben auf dem Blatt **Active Directory-Administrator** auf **SPEICHERN**. ![Administrator auswählen][10]

	Der Vorgang zum Ändern des Administrators kann einige Minuten in Anspruch nehmen. Anschließend wird der neue Administrator im Feld **Active Directory-Administrator** angezeigt.

> [AZURE.NOTE] Beim Einrichten des Azure AD-Administrators darf der Name des neuen Administrators (Benutzer oder Gruppe) in der virtuellen Masterdatenbank noch nicht als SQL Server-Authentifizierungsbenutzer vorhanden sein. Wenn er vorhanden ist, schlägt die Einrichtung des Azure AD-Administrators fehl, wobei die Erstellung rückgängig gemacht und angegeben wird, dass ein solcher Administrator (Name) bereits vorhanden ist. Da dieser SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, sind alle Versuche erfolglos, unter der Azure AD-Authentifizierung eine Verbindung mit dem Server herzustellen.

Wenn Sie später einen Administrator entfernen möchten, klicken Sie oben auf dem Blatt **Active Directory-Administrator** auf **Administrator entfernen** und anschließend auf **Speichern**.

### Bereitstellen eines Azure AD-Administrators für Azure SQL Server unter Verwendung von PowerShell

Zum Ausführen von PowerShell-Cmdlets muss Azure PowerShell installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Um einen Azure AD-Administrator bereitzustellen, führen Sie die folgenden Azure PowerShell-Befehle aus:

- Add-AzureRmAccount
- Select-AzureRmSubscription


Verwenden Sie folgende Cmdlets zum Bereitstellen und Verwalten des Azure AD-Administrators:

| Cmdlet-Name | Beschreibung |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx) | Stellt einen Azure Active Directory-Administrator für Azure SQL Server oder Azure SQL Data Warehouse bereit. (Muss aus dem aktuellen Abonnement stammen.) |
| [Remove-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Entfernt einen Azure Active Directory-Administrator für Azure SQL Server oder Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx) | Gibt Informationen zu dem Azure Active Directory-Administrator zurück, der aktuell für Azure SQL Server oder Azure SQL Data Warehouse konfiguriert ist. |

Verwenden Sie den PowerShell-Befehl "get-help", um weitere Informationen zu diesen Befehlen anzuzeigen. Verwenden Sie z. B. ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Das folgende Skript stellt eine Azure AD-Administratorengruppe namens **DBA\_Group** (Objekt-ID `40b79501-b343-44ed-9ce7-da4c8cc7353f`) für den Server **demo\_server** in einer Ressourcengruppe mit dem Namen **Group-23** bereit:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Der Eingabeparameter **DisplayName** akzeptiert entweder den Azure AD-Anzeigenamen oder den Benutzerprinzipalnamen (UPN). Beispiel: ``DisplayName="John Smith"`` und ``DisplayName="johns@contoso.com"``. Für Azure AD-Gruppen wird nur der Azure AD-Anzeigename unterstützt.

> [AZURE.NOTE] Der Azure PowerShell-Befehl ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` hindert Sie nicht daran, nicht unterstützte Benutzer als Azure AD-Administratoren bereitzustellen. Ein nicht unterstützter Benutzer kann bereitgestellt werden, jedoch keine Verbindung mit einer Datenbank herstellen. (Eine Liste der unterstützten Administratoren finden Sie unter **Features und Einschränkungen von Azure AD** weiter oben.)

Im folgenden Beispiel wird der optionale Wert für **ObjectID** verwendet:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Ein Wert für **ObjectID** ist in Azure AD erforderlich, wenn **DisplayName** nicht eindeutig ist. Um die Werte für **ObjectID** und **DisplayName** abzurufen, verwenden Sie den Active Directory-Abschnitt im klassischen Azure-Portal, um die Eigenschaften für einen Benutzer oder eine Gruppe anzuzeigen.

Das folgende Beispiel gibt Informationen zum aktuellen Azure AD-Administrator für Azure SQL Server zurück:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Im folgenden Beispiel wird ein Azure AD-Administrator entfernt:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Sie können auch einen Azure Active Directory-Administrator mithilfe der REST-APIs bereitstellen. Weitere Informationen finden Sie in der Referenz zur REST-API der Dienstverwaltung sowie unter [Vorgänge für Azure SQL-Datenbanken](https://msdn.microsoft.com/library/azure/dn505719.aspx).

## 5\. Konfigurieren der Clientcomputer

Auf allen Clientcomputern, über die Ihre Anwendungen oder Benutzer unter Verwendung von Azure AD-Identitäten eine Verbindung mit der Azure SQL-Datenbank oder mit Azure SQL Data Warehouse herstellen, muss die folgende Software installiert werden:

- .NET Framework 4.6 oder höher, erhältlich unter [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Die Azure Active Directory-Authentifizierungsbibliothek für SQL Server (**ADALSQL.DLL**) steht in verschiedenen Sprachen (sowohl x86 als auch amd64) im Downloadcenter unter [Microsoft Active Directory-Authentifizierungsbibliothek für Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742) bereit.

### Tools

- Durch das Installieren von [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) oder [SQL Server Data Tools für Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) wird die Anforderung erfüllt, dass .NET Framework 4.6 vorhanden sein muss.
- SSMS installiert die x86-Version von **ADALSQL.DLL**.
- SSDT installiert die amd64-Version von **ADALSQL.DLL**.
- Die neueste Visual Studio-Version unter [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) erfüllt die .NET Framework 4.6-Anforderung, die erforderliche amd64-Version von **ADALSQL.DLL** wird jedoch nicht installiert.

## 6\. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind

### Grundlegendes zu eigenständigen Datenbankbenutzern

Für die Azure Active Directory-Authentifizierung ist es erforderlich, dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden. Ein eigenständiger Datenbankbenutzer basierend auf einer Azure AD-Identität ist ein Datenbankbenutzer, der über keine Anmeldung für die Masterdatenbank verfügt, und der einer Identität im Azure AD-Verzeichnis zugeordnet ist, das mit der Datenbank verknüpft ist. Bei der Azure AD-Identität kann es sich entweder um ein einzelnes Benutzerkonto oder um eine Gruppe handeln. Weitere Informationen zu eigenständigen Datenbankbenutzern finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht im Portal erstellt werden. RBAC-Rollen werden nicht auf SQL Server, SQL-Datenbank oder SQL Data Warehouse übertragen. Azure RBAC-Rollen dienen zum Verwalten von Azure-Ressourcen und gelten nicht für Datenbankberechtigungen. Beispielsweise gewährt die Rolle **SQL Server-Mitwirkender** keinen Zugriff zum Herstellen einer Verbindung mit der SQL-Datenbank oder SQL Data Warehouse. Die Zugriffsberechtigung muss direkt in der Datenbank mithilfe von Transact-SQL-Anweisungen erteilt werden.

### Herstellen einer Verbindung mit Benutzerdatenbank oder Data Warehouse mithilfe von SQL Server Management Studio oder SQL Server Data Tools

Um zu bestätigen, dass der Azure AD-Administrator ordnungsgemäß eingerichtet ist, stellen Sie mit dem Azure AD-Administratorkonto eine Verbindung mit der Datenbank **master** her. Um einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitzustellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mithilfe einer Azure AD-Identität, die Zugriff auf die Datenbank hat, eine Verbindung mit der Datenbank her.

> [AZURE.IMPORTANT] Unterstützung für die Azure Active Directory-Authentifizierung wird über [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015 bereitgestellt. Die SSMS-Version von August 2016 bietet auch Unterstützung für die universelle Active Directory-Authentifizierung, die es Administratoren ermöglicht, die Multi-Factor Authentication per Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App anzufordern.

#### Herstellen einer Verbindung mithilfe der integrierten Active Directory-Authentifizierung

Verwenden Sie diese Methode, wenn Sie mit Ihren Azure Active Directory-Anmeldeinformationen aus einer Verbunddomäne bei Windows angemeldet sind.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im Feld **Authentifizierung** die Einstellung **Integrierte Active Directory-Authentifizierung** aus. Es ist kein Kennwort erforderlich bzw. keine Kennworteingabe möglich, da bei der Verbindungsherstellung Ihre bereits vorhandenen Anmeldeinformationen verwendet werden. ![Integrierte AD-Authentifizierung auswählen][11]

2. Klicken Sie auf die Schaltfläche **Optionen**, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der die Verbindung hergestellt werden soll.![Datenbanknamen auswählen][13]


#### Herstellen einer Verbindung mithilfe der Active Directory-Kennwortauthentifizierung

Verwenden Sie diese Methode, wenn Sie unter Verwendung der verwalteten Azure AD-Domäne und mit einem Azure AD-Prinzipalnamen eine Verbindung herstellen. Sie können diese Authentifizierungsmethode auch für ein Verbundkonto ohne Zugriff auf die Domäne verwenden, beispielsweise wenn Sie an einem Remotestandort arbeiten.

Verwenden Sie diese Methode, wenn Sie mit Ihren Windows-Anmeldeinformationen aus einer Domäne angemeldet sind, die nicht im Verbund mit Azure konfiguriert ist, oder wenn Sie die Azure AD-Authentifizierung mit Azure AD basierend auf der Anfangs- oder der Clientdomäne nutzen.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbankmodul herstellen**) im Feld **Authentifizierung** die Einstellung **Active Directory-Kennwortauthentifizierung** aus.
2. Geben Sie im Feld **Benutzername** Ihren Azure Active Directory-Benutzernamen im Format **username@domain.com** ein. Es muss sich um ein Konto aus Azure Active Directory oder um ein Konto aus einer Domäne handeln, die im Verbund mit Azure Active Directory konfiguriert ist.
3. Geben Sie im Feld **Kennwort** Ihr Benutzerkennwort für das Azure Active Directory-Konto oder das Verbunddomänenkonto ein. 
![AD-Kennwortauthentifizierung auswählen][12]

4. Klicken Sie auf die Schaltfläche **Optionen**, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der Sie sich verbinden möchten. (Siehe Grafik in der vorherigen Option.)


### Erstellen eines eigenständigen Azure AD-Datenbankbenutzers in einer Benutzerdatenbank

Um einen Azure AD-basierten eigenständigen Datenbankbenutzer zu erstellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mit einer Azure AD-Identität, die mindestens über die Berechtigung **BELIEBIGEN BENUTZER ÄNDERN** verfügt, eine Verbindung mit der Datenbank her. Verwenden Sie anschließend die folgende Transact-SQL-Syntax:

	CREATE USER <Azure_AD_principal_name>
	FROM EXTERNAL PROVIDER;


*Azure\_AD\_principal\_name* kann der Benutzerprinzipalname eines Azure AD-Benutzers oder der Anzeigename einer Azure AD-Gruppe sein.

**Beispiele:** So erstellen Sie einen eigenständigen Datenbankbenutzer, der einen Benutzer der Azure AD-Verbunddomäne oder einen Benutzer der verwalteten Azure AD-Domäne repräsentiert:

	CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
	CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Um einen eigenständigen Datenbankbenutzer zu erstellen, der eine Azure AD-Gruppe oder eine Gruppe der Verbunddomäne repräsentiert, geben Sie den Anzeigenamen einer Sicherheitsgruppe an:

	CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

So erstellen Sie einen eigenständigen Datenbankbenutzer, der eine Anwendung darstellt, die eine Verbindung mit einem Azure AD-Token herstellt:

	CREATE USER [appName] FROM EXTERNAL PROVIDER;

Weitere Informationen zum Erstellen eigenständiger Datenbankbenutzer basierend auf Azure Active Directory-Identitäten finden Sie unter [CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Wenn der Azure Active Directory-Administrator für Azure SQL Server entfernt wird, können Azure AD-Authentifizierungsbenutzer keine Verbindung mehr mit dem Server herstellen. Nicht mehr verwendbare Azure AD-Benutzer können bei Bedarf manuell von einem SQL-Datenbankadministrator gelöscht werden.

Wenn Sie einen Datenbankbenutzer erstellen, erhält dieser Benutzer die Berechtigung **VERBINDEN** und kann sich als Mitglied der Rolle **ÖFFENTLICH** mit dieser Datenbank verbinden. Anfänglich stehen dem Benutzer nur die Berechtigungen zur Verfügung, die der Rolle **ÖFFENTLICH** gewährt wurden, sowie sämtliche Berechtigungen, die einer Windows-Gruppe erteilt wurden, denen der Benutzer angehört. Nachdem Sie einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitgestellt haben, können Sie dem Benutzer zusätzliche Berechtigungen gewähren. Die Vorgehensweise ist hierbei mit der Berechtigungszuweisung für alle weiteren Benutzertypen identisch. Typischerweise gewähren Sie Datenbankrollen die gewünschten Berechtigungen, und weisen diesen Rollen anschließend Benutzer zu. Weitere Informationen finden Sie unter [Database Engine Permission Basics](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx) (in englischer Sprache). Weitere Informationen zu besonderen SQL-Datenbankrollen finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md). Ein Verbunddomänenbenutzer, der in eine verwaltete Domäne importiert wird, muss die Identität der verwalteten Domäne verwenden.

> [AZURE.NOTE] Azure AD-Benutzer werden in den Datenbankmetadaten mit dem Typ E (EXTERNAL\_USER) gekennzeichnet, Gruppen mit dem Typ X (EXTERNAL\_GROUPS). Weitere Informationen finden Sie unter [sys.database\_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## 7\. Herstellen einer Verbindung mit Azure AD-Identitäten

Die Azure Active Directory-Authentifizierung unterstützt die folgenden Methoden der Verbindungsherstellung mit einer Datenbank unter Verwendung von Azure AD-Identitäten:

- Integrierte Windows-Authentifizierung
- Azure AD-Prinzipalname und Kennwort
- Anwendungstokenauthentifizierung

### 7\.1. Verbindungsherstellung über die integrierte Authentifizierung (Windows)

Um die integrierte Windows-Authentifizierung zu verwenden, muss das Active Directory Ihrer Domäne mit Azure Active Directory verbunden sein. Ihre Client-Anwendung (oder ein Dienst), die eine Verbindung mit der Datenbank herstellt, muss unter den Domänenanmeldeinformationen eines Benutzers auf einem Computer ausgeführt werden, der der Domäne beigetreten ist.

Um mithilfe der integrierten Authentifizierung und einer Azure AD-Identität eine Verbindung mit einer Datenbank herzustellen, muss das Authentifizierungsschlüsselwort in der Verbindungszeichenfolge für die Datenbank auf "Active Directory Integrated" festgelegt sein. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

	string ConnectionString =
	@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
	SqlConnection conn = new SqlConnection(ConnectionString);
	conn.Open();

Beachten Sie, dass das Schlüsselwort ``Integrated Security=True`` in der Verbindungszeichenfolge nicht für die Verbindungsherstellung mit Azure SQL-Datenbank unterstützt wird. Beachten Sie beim Herstellen einer ODBC-Verbindung, dass Sie Leerzeichen entfernen und Authentifizierung auf ActiveDirectoryIntegrated festlegen müssen.

### 7\.2. Herstellen einer Verbindung mit einem Azure AD-Prinzipalnamen und einem Kennwort
Um mithilfe der integrierten Authentifizierung und einer Azure AD-Identität eine Verbindung mit einer Datenbank herzustellen, muss als Authentifizierungsschlüsselwort das Kennwort für Active Directory festgelegt sein. Die Verbindungszeichenfolge muss Benutzer-ID/UID und Kennwort/PWD-Schlüsselwörter enthalten. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

	string ConnectionString =
	  @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
	SqlConnection conn = new SqlConnection(ConnectionString);
	conn.Open();

Informieren Sie sich über Azure AD-Authentifizierungsmethoden, indem Sie die Demo-Codebeispiele unter [Azure AD Authentication GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth) (GitHub-Demo zur Azure AD-Authentifizierung) verwenden.


### 7\.3 Herstellen einer Verbindung mit einem Azure AD-Token
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

Weitere Informationen finden Sie im [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/) (Blog zur Sicherheit von SQL Server).

### Herstellen einer Verbindung mit SQLCMD  
Die folgenden Anweisungen stellen eine Verbindung mithilfe von Version 13.1 von SQLCMD her, die im [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643) verfügbar ist.

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## Siehe auch

[Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Eigenständige Datenbankbenutzer](https://msdn.microsoft.com/library/ff929071.aspx)

[CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


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

<!---HONumber=AcomDC_0921_2016-->