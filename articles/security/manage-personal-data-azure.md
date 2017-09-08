---
title: Verwalten personenbezogener Daten in Microsoft Azure | Microsoft-Dokumentation
description: "Anleitung zum Korrigieren, Aktualisieren, Löschen und Exportieren von personenbezogenen Daten in Azure Active Directory und Azure SQL-Datenbank"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 3b57c92bd744644ea81878712b4272ed3ece4e2e
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="manage-personal-data-in-microsoft-azure"></a>Verwalten personenbezogener Daten in Microsoft Azure

Dieser Artikel enthält Anleitungen zum Korrigieren, Aktualisieren, Löschen und Exportieren von personenbezogenen Daten in Azure Active Directory und Azure SQL-Datenbank.

## <a name="scenario"></a>Szenario

Ein in Dublin ansässiges Unternehmen bietet ein umfangreiches Angebot für luxuriöse Hochzeitsziele in Irland und auf der ganzen Welt für einheimische und internationale Kunden an. Es verfügt über Niederlassungen, Kunden, Mitarbeiter und Lieferanten auf der ganzen Welt, um alle Angebote umfassend bedienen zu können.

Neben vielen anderen Dingen verfolgt das Unternehmen auch Antworten auf Einladungen, die Informationen zu Nahrungsmittelallergien und Ernährungsbesonderheiten enthalten. Die Hochzeitsgäste können sich für verschiedene Aktivitäten anmelden, wie z.B. Reiten, Surfen, Bootstouren usw. Auch ist während der Monate vor dem Ereignis eine Kommunikation unter den Teilnehmern über eine zentrale Webseite möglich. Das Unternehmen sammelt personenbezogene Daten von Mitarbeitern, Lieferanten, Kunden und Hochzeitsgästen. Aufgrund der internationalen Ausrichtung seiner Aktivitäten muss das Unternehmen mehrere Ebenen von Vorschriften einhalten.

## <a name="problem-statement"></a>Problembeschreibung

- Datenadministratoren müssen die Möglichkeit haben, fehlerhafte personenbezogene Daten zu korrigieren und unvollständige oder geänderte personenbezogene Daten zu aktualisieren.

- Datenadministratoren müssen die Möglichkeit haben, personenbezogene Daten bei einer Anforderung durch die jeweilige Person zu löschen.

- Datenadministratoren müssen Daten exportieren und einer Person auf Anfrage in einem allgemein verständlichen, strukturierten Format zur Verfügung stellen können.

## <a name="company-goals"></a>Unternehmensziele

- Falsche oder unvollständige personenbezogene Daten von Kunden, Hochzeitsgästen, Mitarbeitern und Lieferanten müssen in Azure Active Directory und Azure SQL-Datenbank korrigiert oder aktualisiert werden.

- Personenbezogene Daten müssen auf Anfrage in Azure Active Directory und Azure SQL-Datenbank gelöscht werden.

- Personenbezogene Daten müssen auf Anfrage für Personen in einem allgemein verständlichen, strukturierten Format exportiert werden.

## <a name="solutions"></a>Lösungen

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Azure Active Directory: Korrigieren falscher oder unvollständiger personenbezogener Daten und Löschen von personenbezogenen Daten/Benutzerprofilen

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.
Sie können im [Azure-Portal](https://portal.azure.com/) Benutzerprofile von Kunden und Mitarbeitern sowie Arbeitsinformationen von Benutzern in Ihrer [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)-Umgebung (AAD) korrigieren, aktualisieren oder löschen, die personenbezogene Daten enthalten. Dazu gehören z.B. Benutzername, Arbeitstitel, Adresse oder Telefonnummer.

Dazu müssen Sie sich mit einem Konto anmelden, das als globaler Administrator für das Verzeichnis konfiguriert ist.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Wie korrigiere oder aktualisiere ich Benutzerprofile und Arbeitsinformationen in Azure Active Directory?

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE** .

    ![media/image1.png](media/manage-personal-data-azure/image001.png)

3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzer** aus.

    ![media/image2.png](media/manage-personal-data-azure/image003.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Benutzer** einen Benutzer in der Liste aus, und wählen Sie dann auf dem Blatt für den ausgewählten Benutzer **Profil** aus, um Benutzerprofilinformationen anzuzeigen, die korrigiert oder aktualisiert werden müssen.

    ![media/image3.png](media/manage-personal-data-azure/image005.png)

5. Korrigieren oder aktualisieren Sie die Daten, und wählen Sie anschließend auf der Befehlsleiste **Speichern** aus.

6.  Wählen Sie auf dem Blatt für den ausgewählten Benutzer **Arbeitsinformationen** aus, um Arbeitsinformationen für den Benutzer anzuzeigen, die möglicherweise korrigiert oder aktualisiert werden müssen.

    ![media/image4.png](media/manage-personal-data-azure/image007.png)

7. Korrigieren oder aktualisieren Sie die Arbeitsinformationen, und wählen Sie anschließend auf der Befehlsleiste **Speichern** aus.

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Wie lösche ich ein Benutzerprofil in Azure Active Directory?

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE** .

    ![](media/manage-personal-data-azure/image001.png)

3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzer** aus.

    ![media/image2.png](media/manage-personal-data-azure/image003.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Benutzer** einen Benutzer aus der Liste aus.

    ![media/image3.png](media/manage-personal-data-azure/image007.png)

5. Wählen Sie auf dem Blatt für den ausgewählten Benutzer die Option **Übersicht** aus, und klicken Sie auf der Befehlsleiste auf **Löschen**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>SQL-Datenbank: Korrigieren falscher oder unvollständiger personenbezogener Daten; Löschen von personenbezogenen Daten; Exportieren von personenbezogenen Daten 

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50) ist eine Clouddatenbank, die Entwickler bei der Erstellung und Verwaltung von Anwendungen unterstützt.

In [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50) können Sie mit SQL-Standardabfragen personenbezogene Daten aktualisieren und löschen. Darüber hinaus können personenbezogene Daten über verschiedene Verfahren wie den Import/Export-Assistenten von Azure SQL Server aus SQL-Datenbank exportiert werden. Der Export kann in unterschiedliche Formate erfolgen, z.B. eine BACPAC-Datei.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Wie korrigiere, aktualisiere oder lösche ich personenbezogene Daten in SQL-Datenbank?

Weitere Informationen zum Korrigieren oder Aktualisieren personenbezogener Daten in SQL-Datenbank finden Sie in der Transact-SQL-Dokumentation unter [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [UPDATETEXT](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [WITH <allgemeiner Tabellenausdruck>](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql) oder [WRITETEXT](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql).

Weitere Informationen zum Löschen von personenbezogenen Daten in SQL-Datenbank finden Sie in der Transact-SQL-Dokumentation unter [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql).

#### <a name="how-do-i-export-personal-data-to-a-bacpac-file-in-sql-database"></a>Wie exportiere ich personenbezogene Daten in eine BACPAC-Datei in SQL-Datenbank?

Eine BACPAC-Datei enthält die Daten und Metadaten aus SQL-Datenbank. Es handelt sich um eine ZIP-Datei mit der Erweiterung BACPAC. Sie können dazu das [Azure-Portal](https://portal.azure.com/), das Befehlszeilen-Hilfsprogramm SQLPackage, SQL Server Management Studio (SSMS) oder PowerShell verwenden.

Weitere Informationen zum Exportieren von Daten in eine BACPAC-Datei finden Sie auf der Seite [Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei](https://docs.microsoft.com/azure/sql-database/sql-database-export). Dort erhalten Sie auch detaillierte Anweisungen für die einzelnen oben aufgeführten Methoden.

Wie exportiere ich personenbezogene Daten aus SQL-Datenbank mit dem Import/Export-Assistenten von SQL Server?

Dieser Assistent unterstützt Sie beim Kopieren von Daten aus einer Quelle an ein Ziel. Eine Einführung in den Assistenten, einschließlich Informationen zum Abrufen, sowie Informationen zu den erforderlichen Berechtigungen und zum Abrufen von Hilfe zu dem Tool finden Sie auf der Webseite [Importieren und Exportieren von Daten mit dem SQL Server-Import/Export-Assistenten](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard).

Eine Übersicht über die Schritte im Assistenten finden Sie auf der Webseite [Steps in the SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) (Schritte im SQL Server-Import/Export-Assistenten).

## <a name="next-steps"></a>Nächste Schritte:

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)


