---
title: "Installieren von Aufträgen für die elastische Datenbank | Microsoft Docs"
description: "Schrittweise Anleitung für die Installation der Funktion für elastische Aufträge"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 960e5e311571d1054a2ef5c15c33291de27b0878
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="installing-elastic-database-jobs-overview"></a>Installieren von Aufträgen für die elastische Datenbank – Übersicht
[**Aufträge für die elastische Datenbank**](sql-database-elastic-jobs-overview.md) können mithilfe von PowerShell oder über das klassische Azure-Portal installiert werden. Der Zugriff zum Erstellen und Verwalten von Aufträgen mithilfe der PowerShell-API ist jedoch nur möglich, wenn Sie das PowerShell-Paket installieren. Darüber hinaus stellen die PowerShell-APIs zurzeit erheblich mehr Funktionen bereit als das Portal.

Wenn Sie **Aufträge für die elastische Datenbank** bereits über das Portal aus einem vorhandenen **Pool für elastische Datenbanken** installiert haben, finden Sie in der neuesten PowerShell-Vorschau Skripts, mit denen Sie ein Upgrade der vorhandenen Installation vornehmen können. Es wird dringend empfohlen, dass Sie ein Upgrade Ihrer Installation auf die neuesten Komponenten der **Aufträge für die elastische Datenbank** durchführen, um die über die PowerShell-APIs verfügbar gemachten neuen Funktionen nutzen zu können.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Eine kostenlose Testversion finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Installieren Sie die neueste Version mithilfe des [Webplattform-Installers](http://go.microsoft.com/fwlink/p/?linkid=320376). Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
* [NuGet-Befehlszeilenhilfsprogramm](https://nuget.org/nuget.exe) verwendet. Weitere Informationen finden Sie hier: http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>Herunterladen und Importieren des PowerShell-Pakets der Aufträge für die elastische Datenbank
1. Öffnen Sie das Microsoft Azure PowerShell-Befehlsfenster, und navigieren Sie zu dem Verzeichnis, in das Sie das NuGet-Befehlszeilenhilfsprogramm („nuget.exe“) heruntergeladen haben.
2. Mit dem folgenden Befehl laden Sie das Paket der **Aufträge für die elastische Datenbank** in das aktuelle Verzeichnis herunter und importieren es:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    Die Dateien der **Aufträge für die elastische Datenbank** werden im lokalen Verzeichnis in einem Ordner mit dem Namen **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** platziert, wobei *x.x.xxxx.x* für die Versionsnummer steht. Die PowerShell-Cmdlets (einschließlich der erforderlichen Client-DLLs) befinden sich im Unterverzeichnis **tools\ElasticDatabaseJobs**, und die PowerShell-Skripts für Installation, Upgrade und Deinstallation befinden sich ebenfalls im Unterverzeichnis **tools**.
3. Navigieren Sie zum Unterverzeichnis „tools“ unter dem Ordner „Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x“, indem Sie „cd tools“ eingeben, z. B. in dieser Weise:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Führen Sie das Skript „.\InstallElasticDatabaseJobsCmdlets.ps1“ aus, um das Verzeichnis „ElasticDatabaseJobs“ in „$home\Documents\WindowsPowerShell\Modules“ zu kopieren. Dadurch wird das Modul außerdem automatisch zur Verwendung importiert, beispielsweise:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>Installieren der Komponenten der Aufträge für die elastische Datenbank mithilfe von PowerShell
1. Öffnen Sie ein Microsoft Azure PowerShell-Befehlsfenster, und navigieren Sie zum Unterverzeichnis „\tools“ unter dem Ordner „Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x“: Geben „cd \tools“ ein.
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. Führen Sie das Skript „.\InstallElasticDatabaseJobs.ps1“ aus, und geben Sie Werte für die erforderlichen Variablen an. Dieses Skript erstellt die in [Aufträge für die elastische Datenbank – Komponenten und Preise](sql-database-elastic-jobs-overview.md#components-and-pricing) beschriebenen Komponenten und konfiguriert den Azure Cloud Service passend für die Verwendung der abhängigen Komponenten.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Wenn Sie den Befehl ausführen, wird ein Fenster geöffnet, das Sie zur Eingabe von **Benutzername** und **Kennwort** auffordert. Dabei handelt es sich nicht um Ihre Azure-Anmeldeinformationen. Geben Sie den Benutzernamen und das Kennwort ein, die Sie als Anmeldeinformationen für das Administratorkonto des neuen Servers erstellen möchten.

Die für diesen Beispielaufruf angegebenen Parameter können nach Ihren Wünschen angepasst werden. Im folgenden finden Sie weitere Informationen zum Verhalten der einzelnen Parameter:

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Beschreibung</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Gibt den Azure-Ressourcengruppennamen an, der für die Aufnahme der neu erstellten Azure-Komponenten erstellt wird. Dieser Parameter ist standardmäßig auf den Wert „__ElasticDatabaseJob“ festgelegt. Das Ändern dieses Werts wird nicht empfohlen.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Gibt den Azure-Standort an, der für die neu erstellten Azure-Komponenten verwendet werden soll. Dieser Parameter ist standardmäßig auf den Central US-Standort festgelegt.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Gibt die Anzahl der zu installierenden Arbeitsprozesse an. Dieser Parameter weist den Standardwert „1“ auf. Es kann eine höhere Anzahl Arbeitsprozesse verwendet werden, um den Dienst zu skalieren und höhere Verfügbarkeit bereitzustellen. Für Bereitstellungen, die eine hohe Verfügbarkeit des Diensts benötigen, wird der Wert „2“ empfohlen.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Gibt die Größe der VM für die Verwendung innerhalb des Clouddiensts an. Dieser Parameter ist standardmäßig auf A0 festgelegt. Die möglichen Parameterwerte sind „A0/A1/A2/A3“ und legen die Größe „Extraklein/Klein/Mittel/Groß“ für die Workerrolle fest. Weitere Informationen zu Workerrollengrößen finden Sie unter [Aufträge für die elastische Datenbank – Komponenten und Preise](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Gibt das Service Level-Ziel für die Standard-Edition an. Dieser Parameter ist standardmäßig „S0“. Die Parameterwerte „S0/S1/S2/S3/S4/S6/S9/S12“ sind zulässig und bewirken die Verwendung des entsprechenden SLO für Azure SQL-Datenbank. Weitere Informationen zu SLOs für SQL-Datenbank finden Sie unter [Aufträge für die elastische Datenbank – Komponenten und Preise](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Gibt den Namen des Administratorbenutzers für den neu erstellten Azure SQL-Datenbankserver an. Wenn die Angabe nicht erfolgt, wird ein PowerShell-Anmeldeinformationsfenster geöffnet und fordert zur Eingabe der Anmeldeinformationen auf.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Gibt das Administratorkennwort für den neu erstellten Azure SQL-Datenbankserver an. Wenn keine Angabe erfolgt, wird ein PowerShell-Anmeldeinformationsfenster geöffnet und fordert zur Eingabe der Anmeldeinformationen auf.</td>
</tr>
</table>

Für Systeme, die viele Aufträge parallel auf einer großen Anzahl von Datenbanken ausführen sollen, werden Angaben in dieser Art empfohlen: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Aktualisieren einer vorhandenen Installation von Komponenten der Aufträge für die elastische Datenbank mithilfe von PowerShell
**Aufträge für die elastische Datenbank** kann innerhalb einer vorhandenen Installation im Hinblick auf Skalierbarkeit und Hochverfügbarkeit aktualisiert werden. Dieser Prozess ermöglicht zukünftige Upgrade des Dienstcodes, ohne dass die Steuerdatenbank verworfen und neu erstellt werden muss. Dieser Prozess kann auch innerhalb der gleichen Version verwendet werden, um die Größe der Dienst-VM oder die Workeranzahl auf dem Server zu ändern.

Zum Aktualisieren der Größe der VM einer Installation führen Sie das folgende Skript mit Parameterwerten Ihrer Wahl aus.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parameter</th>
  <th>Beschreibung</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>Bezeichnet den Namen der Azure-Ressourcengruppe, die bei der ursprünglichen Installation der Komponenten der Aufträge für die elastische Datenbank verwendet wurde. Dieser Parameter ist standardmäßig auf den Wert „__ElasticDatabaseJob“ festgelegt. Da eine Änderung dieses Werts nicht empfohlen wird, sollten Sie diesen Parameter nicht anzugeben brauchen.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Gibt die Anzahl der zu installierenden Arbeitsprozesse an.  Dieser Parameter weist den Standardwert „1“ auf.  Es kann eine höhere Anzahl Arbeitsprozesse verwendet werden, um den Dienst zu skalieren und höhere Verfügbarkeit bereitzustellen.  Für Bereitstellungen, die eine hohe Verfügbarkeit des Diensts benötigen, wird der Wert „2“ empfohlen.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Gibt die Größe der VM für die Verwendung innerhalb des Clouddiensts an. Dieser Parameter ist standardmäßig auf A0 festgelegt. Die möglichen Parameterwerte sind „A0/A1/A2/A3“ und legen die Größe „Extraklein/Klein/Mittel/Groß“ für die Workerrolle fest. Weitere Informationen zu Workerrollengrößen finden Sie unter [Aufträge für die elastische Datenbank – Komponenten und Preise](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>Installieren der Komponenten der Aufträge für die elastische Datenbank mithilfe des Portals
Nachdem Sie einen [Pool für elastische Datenbanken erstellt](sql-database-elastic-pool-manage-portal.md) haben, können Sie die Komponenten der **Aufträge für die elastische Datenbank** installieren, um die Ausführung von Verwaltungsaufgaben für jede der Datenbanken im Pool für elastische Datenbanken zu ermöglichen. Anders als bei der Verwendung der PowerShell-APIs für die **Aufträge für die elastische Datenbank** , ist die Portalschnittstelle aktuell auf die Ausführung auf einem vorhandenen Pool beschränkt.

**Geschätzter Zeitaufwand:** 10 Minuten.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/#) in der Dashboardansicht des Pools für elastische Datenbanken auf **Auftrag erstellen**.
2. Wenn Sie zum ersten Mal einen Auftrag erstellen, müssen Sie **Aufträge für die elastische Datenbank** installieren, indem Sie auf **VORSCHAUBESTIMMUNGEN** klicken.
3. Akzeptieren Sie die Bedingungen, indem Sie das Kontrollkästchen aktivieren.
4. Klicken Sie in der Ansicht "Dienste installieren" auf **JOB CREDENTIALS**.
   
    ![Installieren der Dienste][1]
5. Geben Sie einen Benutzernamen und ein Kennwort für einen Datenbankadministrator ein. Im Rahmen der Installation wird ein neuer Azure SQL-Datenbank-Server erstellt. Auf diesem neuen Server wird eine neue Datenbank erstellt, die als Steuerdatenbank bezeichnet wird und die Metadaten für Aufträge für die elastische Datenbank enthält. Der Benutzername und das Kennwort, die hier erstellt werden, werden für die Anmeldung bei der Steuerdatenbank verwendet. Für die Skriptausführung für die Datenbanken innerhalb des Pools werden getrennte Anmeldeinformationen verwendet.
   
    ![Erstellen von Benutzername und Kennwort][2]
6. Klicken Sie auf die Schaltfläche "OK". Die Komponenten werden in wenigen Minuten in einer neuen [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellt. Die neue Ressourcengruppe wird an Start angeheftet, wie unten dargestellt. Nach der Erstellung werden alle Aufträge für die elastische Datenbank (Cloud Service, SQL-Datenbank, Service Bus und Storage) in der Gruppe erstellt.
   
    ![Ressourcengruppe unter Start][3]
7. Wenn Sie versuchen, einen Auftrag zu erstellen oder zu verwalten, während die Aufträge für die elastische Datenbank installiert werden, wird bei der Eingabe der **Anmeldeinformationen** die folgende Meldung angezeigt.
   
    ![Bereitstellung noch nicht abgeschlossen][4]

Wenn die Deinstallation erforderlich ist, löschen Sie die Ressourcengruppe. Weitere Informationen finden Sie unter [Deinstallieren der Komponenten der Aufträge für die elastische Datenbank](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Nächste Schritte
Stellen Sie sicher, dass für jede Datenbank in der Gruppe Anmeldeinformationen mit den geeigneten Rechten für die Skriptausführung erstellt werden. Weitere Informationen dazu finden Sie unter [Sichern der SQL-Datenbank](sql-database-manage-logins.md).
Informationen zum Einstieg finden Sie unter [Erstellen und Verwalten von Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-create-and-manage.md).

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png

