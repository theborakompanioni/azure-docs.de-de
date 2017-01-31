---
title: "Verwenden von Azure Storage für die SQL Server-Sicherung und -Wiederherstellung | Microsoft Docs"
description: "Erfahren Sie, wie Sie SQL Server in Azure Storage sichern. Erläutert die Vorteile der Sicherung von SQL-Datenbanken in Azure Storage."
services: virtual-machines-windows
documentationcenter: 
author: MikeRayMSFT
manager: jhubbard
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/22/2016
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: 3cfeed9d10cba8e51e8609fe2a0a2a3228681850


---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Verwenden von Azure Storage für SQL Server-Sicherung und -Wiederherstellung
## <a name="overview"></a>Übersicht
Seit SQL Server 2012 SP1 CU2 können Sie SQL Server-Sicherungen direkt in den Azure-Blobspeicherdienst schreiben. Mithilfe dieser Funktion können Sie Daten vom Azure Blob-Dienst mit einer lokalen SQL Server-Datenbank oder einer SQL Server-Datenbank auf einem virtuellen Azure-Computer sichern und daraus wiederherstellen. Die Sicherung in der Cloud bietet zahlreiche Vorteile: Verfügbarkeit, unbegrenzter georeplizierter Offsitespeicher und problemlose Migration von Daten in die und aus der Cloud. Sie können BACKUP- und RESTORE-Anweisungen mithilfe von T-SQL oder SMO aufrufen.

In SQL Server 2016 wurden neue Funktionen eingeführt: Sie können die [Dateimomentaufnahme-Sicherung](http://msdn.microsoft.com/library/mt169363.aspx) verwenden, um nahezu sofortige Sicherungen und unglaublich schnelle Wiederherstellungen durchzuführen.

In diesem Thema werden die Vorteile des Azure-Speichers für SQL-Sicherungen erläutert. Anschließend werden die notwendigen Komponenten beschrieben. Die Ressourcen am Ende des Artikels bieten exemplarische Vorgehensweisen und zusätzliche Informationen zur Verwendung dieses Diensts mit Ihren SQL Server-Sicherungen.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Vorteile der Verwendung des Azure-Blob-Diensts für SQL Server-Sicherungen
Bei der Sicherung von SQL Server gibt es verschiedene Herausforderungen. Hierzu gehören die Speicherverwaltung, das Risiko eines Speicherausfalls, der Zugriff auf Offsitespeicher und die Konfiguration der Hardware. Viele dieser Herausforderungen lassen sich durch Verwendung des Azure-Blobspeicherdiensts für SQL Server-Sicherungen meistern. Betrachten Sie die folgenden Vorteile:

* **Einfache Bedienung:** Das Speichern von Sicherungen in Azure-Blobs ist eine praktische, flexible und leicht zugängliche Option für die Offsitespeicherung. Die Erstellung von Offsitespeicher für SQL Server-Sicherungen erfordert ggf. lediglich das Ändern vorhandener Skripts/Aufträge für die Verwendung der **BACKUP TO URL**-Syntax. Offsitespeicher sollte normalerweise weit genug vom Standort der Produktionsdatenbank entfernt sein, um zu verhindern, dass ein einziger Notfall sich auf beide Standorte (den des Offsitespeichers und den der Produktionsdatenbank) auswirkt. Die [Georeplikation Ihrer Azure-Blobs](../../../storage/storage-redundancy.md) bietet zusätzlichen Schutz bei einem Notfall, der sich auf die gesamte Region auswirkt.
* **Sicherungsarchiv:** Der Azure Blob Storage-Dienst bietet eine bessere Alternative als die sonst übliche Archivierung von Sicherungen auf Band. Bandspeicher muss ggf. physisch an einen Offsitestandort transportiert werden und erfordert Schutzmaßnahmen für die Medien. Die Speicherung von Sicherungen im Azure-Blob-Speicher stellt eine sofortige, hochverfügbare und dauerhafte Archivierungsoption dar.
* **Verwaltete Hardware**: Bei Azure-Diensten fällt kein Mehraufwand für Hardwareverwaltung an. Azure-Dienste verwalten die Hardware und stellen geografische Replikation für Redundanz und Schutz vor Hardwareausfällen zur Verfügung.
* **Unbegrenzter Speicherplatz**: Durch eine direkte Sicherung in Azure-Blobs haben Sie Zugriff auf praktisch unbegrenzten Speicher. Die Sicherung in einem virtuellen Azure-Computer dagegen weist Einschränkungen aufgrund der Computergröße auf. Die Anzahl von Datenträgern, die für Sicherungen an einen virtuellen Azure-Computer angefügt werden können, ist begrenzt. Bei einer sehr großen Instanz beträgt die maximale Anzahl 16 Datenträger, während kleinere Instanzen weniger Datenträger unterstützen.
* **Verfügbarkeit der Sicherungen:** In Azure-Blobs gespeicherte Sicherungen sind jederzeit von überall aus erreichbar. So können Sie Daten ganz einfach auf einer lokalen SQL Server-Instanz oder einer anderen, auf einem virtuellen Azure-Computer ausgeführten SQL Server-Instanz wiederherstellen, ohne dass Sie Datenbanken anfügen/trennen oder die VHD herunterladen und anfügen müssen.
* **Kosten**: Sie zahlen nur für den tatsächlich genutzten Dienst. Die Option kann genauso kosteneffizient wie eine Offsitesicherungs-/-archivierungslösung sein. Weitere Informationen finden Sie im [Azure-Preisrechner](http://go.microsoft.com/fwlink/?LinkId=277060 "Preisrechner") sowie in der [Azure-Preisübersicht](http://go.microsoft.com/fwlink/?LinkId=277059 "Preisübersicht").
* **Speichermomentaufnahmen**: Wenn Datenbankdateien in einem Azure-Blob gespeichert werden und Sie SQL Server 2016 verwenden, können Sie die [Dateimomentaufnahme-Sicherung](http://msdn.microsoft.com/library/mt169363.aspx) für nahezu sofortige Sicherungen und unglaublich schnelle Wiederherstellungen verwenden.

Weitere Details finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure-BLOB-Speicherdienst](http://go.microsoft.com/fwlink/?LinkId=271617).

Die beiden folgenden Abschnitte bieten eine Einführung in den Azure-Blobspeicherdienst, einschließlich der erforderlichen SQL Server-Komponenten. Es ist wichtig, die Komponenten zu kennen und zu wissen, wie sie interagieren, um erfolgreich Daten im Azure-Blobspeicherdienst zu sichern und daraus wiederherzustellen.

## <a name="azure-blob-storage-service-components"></a>Komponenten des Azure-Blob-Speicherdiensts
Die folgenden Azure-Komponenten werden bei einer Sicherung im Azure-Blobspeicherdienst verwendet.

| Komponente | Beschreibung |
| --- | --- |
| **Speicherkonto** |Das Speicherkonto ist der Ausgangspunkt für alle Speicherdienste. Um auf einen Azure-Blob-Speicherdienst zuzugreifen, erstellen Sie als Erstes ein Azure-Speicherkonto. Weitere Informationen zum Azure-Blob-Speicherdienst finden Sie unter [Verwenden des Azure-Blob-Speicherdiensts](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** |Ein Container stellt einen Satz Blobs als Gruppe bereit und kann eine unbegrenzte Anzahl von Blobs enthalten. Um eine SQL Server-Sicherung in einen Azure-Blob-Dienst zu speichern, muss mindestens ein Stammcontainer erstellt worden sein. |
| **Blob** |Eine Datei von beliebiger Art und Größe. Blobs sind über das folgende URL-Format adressierbar: **https://[Speicherkonto].blob.core.windows.net/[Container]/[Blob]**. Weitere Informationen über Seitenblobs finden Sie unter [Grundlegendes zu Block- und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-Komponenten
Die folgenden SQL Server-Komponenten werden bei einer Sicherung im Azure-Blobspeicherdienst verwendet.

| Komponente | Beschreibung |
| --- | --- |
| **URL** |Eine URL gibt einen Uniform Resource Identifier (URI) zu einer eindeutigen Sicherungsdatei an. Die URL dient zur Angabe des Speicherorts und Namens der SQL Server-Sicherungsdatei. Die URL darf nicht lediglich auf einen Container, sondern muss auf einen tatsächlichen Blob verweisen. Wenn das Blob nicht vorhanden ist, wird es erstellt. Das Angeben eines vorhandenen Blobs führt zu einem Fehler bei BACKUP – es sei denn, die Option > WITH FORMAT wird angegeben. Hier sehen Sie ein Beispiel einer URL, die Sie im BACKUP-Befehl angeben können: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wird empfohlen, ist jedoch nicht erforderlich. |
| **Credential** |Die Informationen, die zum Herstellen einer Verbindung zum Azure-Blob-Speicherdienst und zum Authentifizieren bei diesem erforderlich sind, werden als Anmeldeinformationen gespeichert.  Damit SQL Server Sicherungen in einen Azure-Blob schreiben oder daraus wiederherstellen kann, müssen SQL Server-Anmeldeinformationen erstellt werden. Weitere Informationen finden Sie unter [CREATE CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Wenn Sie eine Sicherungsdatei kopieren und in den Azure-Blob-Speicherdienst hochladen, müssen Sie einen Seiten-Blob als Speichertyp verwenden, falls Sie diese Datei für Wiederherstellungen nutzen möchten. Ein RESTORE aus einem Block-Blob schlägt mit einer Fehlermeldung fehl.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
1. Erstellen Sie ein Azure-Konto, falls Sie noch keins besitzen. Wenn Sie Azure evaluieren, können Sie die [kostenlose Testversion](https://azure.microsoft.com/free/)verwenden.
2. Arbeiten Sie dann eines der folgenden Tutorials durch, in denen Sie Anleitungen zum Erstellen eines Speicherkontos und zum Durchführen einer Wiederherstellung erhalten.
   
   * **SQL Server 2014**: [Tutorial: SQL Server 2014-Sicherung und -Wiederherstellung im Microsoft Azure Blob Storage-Dienst](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [Tutorial: Using the Microsoft Azure Blob storage service with SQL Server 2016 databases](https://msdn.microsoft.com/library/dn466438.aspx)
3. Lesen Sie weitere Artikel zu diesem Thema, und beginnen Sie mit [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148.aspx).

Wenn Probleme auftreten, lesen Sie das Thema [SQL Server-URL-Sicherung – bewährte Methoden und Problembehandlung](https://msdn.microsoft.com/library/jj919149.aspx).

Weitere Sicherungs- und Wiederherstellungsoptionen für SQL Server finden Sie unter [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).




<!--HONumber=Jan17_HO2-->


