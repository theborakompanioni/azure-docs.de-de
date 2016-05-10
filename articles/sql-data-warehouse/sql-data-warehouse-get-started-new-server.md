<properties
   pageTitle="Erstellen einer SQL Data Warehouse-Datenbank im Azure-Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein Azure SQL Data Warehouse im Azure-Portal erstellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/03/2016"
   ms.author="lodipalm;"/>

# Erstellen eines neuen logischen Servers

In Azure SQL-Datenbank und SQL Data Warehouse wird jede Datenbank einem Server und jeder Server einem geografischen Standort zugewiesen. Der Server wird als logischer SQL Server bezeichnet.

> [AZURE.NOTE] <a name="note"></a>Ein logischer SQL Server:
  >
  > + Bietet ein einheitliches Verfahren zum Konfigurieren mehrerer Datenbanken innerhalb des gleichen geografischen Standorts.
  > + Ist keine physische Hardware wie bei einem lokalen Server, sondern Teil der Dienstsoftware. Aus diesem Grund wird er als *logischer Server* bezeichnet.
  > + Kann mehrere Datenbanken ohne Beeinträchtigung der Leistung hosten.
  > + Enthält im Namen ein kleingeschriebenes *s*. "SQL **s**erver" ist ein logischer Azure-Server, während "SQL **S**erver" der Name eines lokalen Datenbankprodukts von Microsoft ist.

1. Klicken Sie auf **Server** > **Neuen Server erstellen**. Es fallen keine Gebühren für den Server ein. Wenn Sie einen bereits vorhandenen logischen V12-Server verwenden möchten, wählen Sie den vorhandenen Server aus und fahren mit dem nächsten Schritt fort.

    ![Erstellen eines neuen Servers](./media/sql-data-warehouse-get-started-provision/create-server.png)

2. Geben Sie die Informationen in **Neuer Server** ein.

	- **Servername**: Geben Sie einen Namen für den logischen Server ein. Dieser ist für jeden geografischen Standort eindeutig.
	- **Name des Serveradministrators**: Geben Sie einen Benutzernamen für das Serveradministratorkonto ein.
	- **Kennwort**: Geben Sie das Administratorkennwort für den Server ein.
	- **Standort**: Wählen Sie einen geografischen Standort für den Server. Um die Datenübertragungszeit zu verkürzen, empfiehlt es sich, den Server in geografischer Nähe zu anderen Datenressourcen zu platzieren, auf die diese Datenbank zugreift.
	- **V12-Server erstellen**: „JA“ ist für SQL Data Warehouse die einzige Option.
	- **Azure-Diensten Zugriff auf den Server erlauben**: Diese Option ist für SQL Data Warehouse immer aktiviert.

    >[AZURE.NOTE] Stellen Sie sicher, dass Sie den Servernamen, den Serveradministratornamen und das Kennwort notieren und aufbewahren. Sie benötigen diese Informationen, um sich beim Server anzumelden.

3. Klicken Sie auf **OK**, um die Einstellungen zu speichern und zum Blatt "SQL Data Warehouse" zurückzukehren.

    ![Konfigurieren des neuen Servers](./media/sql-data-warehouse-get-started-provision/configure-server.png)

<!---HONumber=AcomDC_0504_2016-->