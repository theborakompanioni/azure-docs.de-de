<properties
	pageTitle="Erstellen eines neuen elastischen Pools mit dem Azure-Portal | Microsoft Azure"
	description="Enthält Informationen zum Hinzufügen eines skalierbaren Pools für elastische Datenbanken zu Ihrer SQL-Datenbankkonfiguration, um eine einfachere Verwaltung und Ressourcenfreigabe zwischen zahlreichen Datenbanken zu ermöglichen."
	keywords="Skalierbare Datenbank, Datenbankkonfiguration"
	services="sql-database"
	documentationCenter=""
	authors="ninarn"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="04/28/2016"
	ms.author="ninarn"
	ms.workload="data-management"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="NA"/>


# Erstellen eines neuen Pools für elastische Datenbanken mit dem Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)

Es gibt zwei Möglichkeiten, um einen [Pool](sql-database-elastic-pool.md) zu erstellen.

1. Erstellen Sie ihn selbstständig, v.a. wenn Sie wissen, welches Pool-Setup Sie benötigen.
2. Oder Sie erstellen ihn ausgehend von einer Empfehlung vom SQL-Datenbankdienst. Der Dienst untersucht Ihre bisheriger Nutzung anhand von Telemetriedaten. Anschließen empfiehlt er eine kostengünstige Konfiguration für Ihre Datenbanken.

Sie können einem Server mehrere Pools hinzufügen, aber es ist nicht möglich, Datenbanken von verschiedenen Servern in demselben Pool zusammenzufassen. Zum Erstellen eines Pools benötigen Sie mindestens eine Datenbank auf einem V12-Server. Falls Sie keinen V12-Server besitzen,finden Sie unter [SQL-Datenbank-Tutorial: Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md) weitere Informationen. Sie können einen Pool mit nur einer Datenbank erstellen, aber Pools sind erst mit mehreren Datenbanken kosteneffizient. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md).

Pools sind nur mit SQL-Datenbank-V12-Servern verfügbar. Wenn Sie Datenbanken auf einem V11-Server installiert haben, können Sie auf einem V12-Server [mit einem PowerShell-Skript die für einen Pool geeigneten Kandidaten ermitteln](sql-database-elastic-pool-database-assessment-powershell.md) und dann in einem Schritt [mit PowerShell ein Upgrade auf V12 ausführen und einen Pool erstellen](sql-database-upgrade-server-powershell.md).

## Erstellen eines neuen Pools

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) auf **SQL-Server** und dann auf den Server mit den Datenbanken, die Sie einem Pool hinzufügen möchten.
2. Klicken Sie auf **Neuer Pool**.

    ![Hinzufügen eines Pools zu einem Server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **ODER**

    Klicken Sie auf die Meldung, um die Pools anzuzeigen, die basierend auf den Telemetriedaten zur bisherigen Datenbanknutzung empfohlen werden. Klicken Sie anschließend auf die Ebene, um weitere Details anzuzeigen und den Pool anzupassen. Informationen zur Bereitstellung der Empfehlung finden Sie weiter unten in diesem Thema unter [Grundlagen von Poolempfehlungen](#understand-pool-recommendations).

    ![Empfohlener Pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

    Das Blatt **Pool für elastische Datenbanken**, in dem Sie den Pool einrichten, wird angezeigt. Wenn Sie im vorherigen Schritt auf **Neuer Pool** geklickt haben, werden vom Portal unter **Tarif** ein **Standard-Pool**, ein eindeutiger **Name** für den Pool und eine Standardkonfiguration für den Pool ausgewählt. Wenn Sie einen empfohlenen Pool ausgewählt haben, sind die empfohlene Ebene und die Konfiguration des Pools bereits ausgewählt, aber Sie können diese Angaben immer noch ändern.

    ![Konfigurieren eines elastischen Pools](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

3. Um den Tarif für den Pool zu ändern, klicken Sie auf die Option **Tarif**, auf den gewünschten Tarif und dann auf **Auswählen**.

    > [AZURE.IMPORTANT] Nachdem Sie den Tarif ausgewählt und Ihre Änderungen übernommen haben, indem Sie im letzten Schritt auf **OK** geklickt haben, können Sie den Tarif des Pools nicht mehr ändern.

4. Klicken Sie auf **Pool konfigurieren**, um Datenbanken hinzuzufügen und Ressourceneinstellungen für den Pool auszuwählen.
5. Klicken Sie zum Hinzufügen von Datenbanken auf **Datenbank hinzufügen**, wählen Sie die Datenbanken aus, die Sie hinzufügen möchten, und klicken Sie anschließend auf die Schaltfläche **Auswählen**.

    ![Hinzufügen von Datenbanken](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Wenn die von Ihnen verwendeten Datenbanken über ausreichend Telemetriedaten zur bisherigen Nutzung verfügen, werden der Graph **Geschätzte eDTU- und GB-Nutzung** und das Balkendiagramm **Tatsächliche eDTU-Nutzung** aktualisiert, um für Sie als Hilfe beim Treffen von Konfigurationsentscheidungen zu dienen.

    ![Dynamische Empfehlungen](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

6. Verwenden Sie die Steuerelemente auf der Seite **Pool konfigurieren**, um die Einstellungen zu untersuchen und den Pool zu konfigurieren. Weitere Details zu den Grenzwerten für die einzelnen Dienstebenen finden Sie unter [Beschränkungen für elastische Pools](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases), und eine ausführliche Anleitung zur richtigen Dimensionierung eines Pools finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md).
7. Klicken Sie auf **Auswählen**, wenn Sie fertig sind, und anschließend auf **OK**, um den Pool zu erstellen.

## Grundlagen von Poolempfehlungen

Der SQL-Datenbankdienst bewertet den Nutzungsverlauf und empfiehlt einen oder mehrere Pools, wenn dies kostengünstiger als die Verwendung von Einzeldatenbanken ist. Jede Empfehlung ist mit einer eindeutigen Teilmenge der Datenbanken des Servers konfiguriert, die optimal zum Pool passen. Die Poolempfehlung umfasst Folgendes:

- Tarif für den Pool (Basic, Standard oder Premium)
- Passende **POOL-eDTUs** (auch als maximale eDTUs pro Pool bezeichnet)
- **eDTU maximal** und **eDTU minimal** pro Datenbank
- Liste der empfohlenen Datenbanken für den Pool

Der Dienst berücksichtigt bei der Empfehlung von Pools die Telemetrie der letzten 30 Tage. Damit eine Datenbank als Kandidat für einen elastischen Datenbankpool berücksichtigt wird, muss sie seit mindestens 7 Tagen existieren. Datenbanken, die sich bereits in einem elastischen Datenbankpool befinden, werden nicht als Empfehlungskandidaten für elastische Datenbankpools angesehen.

Der Dienst bewertet den Ressourcenbedarf und die Kosteneffizienz beim Verschieben der Einzeldatenbanken auf jeder Dienstebene in Pools auf der gleichen Ebene. Beispielsweise werden alle Datenbanken der Standard-Edition auf einem Server entsprechend ihrer Eignung für einen elastischen Pool der Standard-Edition bewertet. Dies bedeutet, dass der Dienst keine tarifübergreifenden Empfehlungen trifft, also etwa eine Standard-Datenbank nicht in einen Premium-Pool verschiebt.

## Zusätzliche Ressourcen

- [Verwalten eines elastischen SQL-Datenbankpools mit dem Portal](sql-database-elastic-pool-manage-portal.md)
- [Verwalten eines elastischen SQL-Datenbankpools mit PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [Verwalten eines elastischen SQL-Datenbankpools mit C#](sql-database-elastic-pool-manage-csharp.md)
- [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md) 

<!---HONumber=AcomDC_0504_2016-->