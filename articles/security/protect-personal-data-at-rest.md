Cu--- title: Schützen ruhender personenbezogener Daten durch Verschlüsselung in Azure | Microsoft-Dokumentation description: Dieser Artikel ist Teil einer Reihe von Artikeln zur Verwendung von Azure zum Schützen personenbezogener Daten services: security documentationcenter: na author: Barclayn manager: MBaldwin editor: TomSh

ms.assetid: ms.service: security ms.devlang: na ms.topic: article ms.tgt_pltfrm: na ms.workload: na ms.date: 08/22/2017 ms.author: barclayn ms.custom: 

---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Azure-Verschlüsselungstechnologien: Schützen ruhender personenbezogener Daten durch Verschlüsselung

Dieser Artikel enthält grundlegende Informationen zur Verwendung von Azure-Verschlüsselungstechnologien zum Schützen ruhender Daten.

Die Verschlüsselung ruhender Daten eine wichtige bewährte Methode zum Schutz sensibler oder personenbezogener Daten und zur Erfüllung von Compliance- und Datenschutzanforderungen.
Die Verschlüsselung ruhender Daten wurde entwickelt, um dem Angreifer den Zugriff auf die unverschlüsselten Daten zu verwehren, indem die Daten auf der Festplatte verschlüsselt sind.

## <a name="scenario"></a>Szenario 

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben.

Das Unternehmen verwendet Microsoft Azure zum Speichern von Unternehmensdaten in der Cloud. Dies kann z.B. folgende Kunden- und/oder Mitarbeiterinformationen umfassen:

- Adressen
- Telefonnummern
- Steueridentifikationsnummern
- Kreditkarteninformationen

Das Unternehmen muss den Schutz der Kunden- und Mitarbeiterdaten gewährleisten und gleichzeitig den jeweiligen Abteilungen Zugriff auf die Daten ermöglichen (z.B. der Lohnbuchhaltung oder der Reservierungsabteilung).

Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms, die personenbezogene Informationen enthält, um Beziehungen zu aktuellen und früheren Kunden nachzuverfolgen.

### <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen muss den Schutz der personenbezogenen Daten von Kunden und Mitarbeitern gewährleisten und gleichzeitig den jeweiligen Abteilungen Zugriff auf die Daten ermöglichen (z.B. der Lohnbuchhaltung oder der Reservierungsabteilung). Diese personenbezogenen Daten werden außerhalb des durch das Unternehmen gesteuerten Rechenzentrums gespeichert und stehen nicht unter der physischen Kontrolle des Unternehmens.

### <a name="company-goal"></a>Unternehmensziel

Als Teil einer mehrschichtigen Defense-in-Depth-Sicherheitsstrategie ist ein Unternehmensziel die Sicherstellung, dass alle Datenquellen, die personenbezogene Daten enthalten, einschließlich der Daten im Cloudspeicher, verschlüsselt werden. Wenn nicht autorisierte Personen Zugriff auf die personenbezogenen Daten erhalten, müssen diese in einer nicht lesbaren Form vorliegen. Die Anwendung der Verschlüsselung muss einfach sein und im Hintergrund erfolgen – für die Benutzer und die Administratoren.

## <a name="solutions"></a>Lösungen

Azure-Dienste umfassen mehrere Tools und Technologien, mit denen Sie ruhende personenbezogene Daten durch Verschlüsselung schützen können.

### <a name="azure-key-vault"></a>Azure Key Vault

[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) ermöglicht die sichere Speicherung der Schlüssel, die zum Verschlüsseln ruhender Daten in Azure-Diensten verwendet werden, und ist die empfohlene Lösung zur Schlüsselspeicherung und -verwaltung. Die Verwaltung der Verschlüsselungsschlüssel ist für die Sicherung gespeicherter Daten unerlässlich.

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a>Wie verwende ich Azure Key Vault zum Schützen von Schlüsseln, mit denen personenbezogene Daten verschlüsselt werden?

Zur Verwendung von Azure Key Vault benötigen Sie ein Abonnement für ein Azure-Konto. Zudem muss Azure PowerShell installiert sein. Die erforderlichen Schritte umfassen die Verwendung von PowerShell-Cmdlets für folgende Vorgänge:

1. Verbindungsherstellung mit Ihren Abonnements

2. Erstellen eines Schlüsseltresors

3. Hinzufügen eines Schlüssels oder geheimen Schlüssels zum Schlüsseltresor

4. Registrieren von Anwendungen, die den Schlüsseltresor mit Azure Active Directory verwenden

5. Autorisieren der Anwendungen zum Verwenden des Schlüssels oder Geheimnisses

Verwenden Sie zum Erstellen eines Schlüsseltresors das PowerShell-Cmdlet „New-AzureRmKeyVault“. Sie weisen einen Tresornamen, einen Ressourcengruppennamen und einen geografischen Standort zu. Den Tresornamen verwenden Sie beim Verwalten von Schlüsseln über andere Cmdlets. Anwendungen, die den Tresor über die REST-API verwenden, nutzen den URI des Tresors.

Azure Key Vault kann einen softwaregeschützten Schlüssel für Sie bereitstellen. Oder Sie können einen vorhandenen Schlüssel in einer PFX-Datei importieren. Sie können auch Geheimnisse (Kennwörter) im Tresor speichern.

Zudem können Sie einen Schlüssel in Ihrem lokalen HSM generieren und in HSMs im Key Vault-Dienst übertragen, ohne dass der Schlüssel die HSM-Grenzen verlässt.

Ausführliche Anweisungen zur Verwendung von Azure Key Vault finden Sie unter [Erste Schritte mit Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started).

Eine Liste der mit Azure Key Vault verwendeten PowerShell-Cmdlets finden Sie unter [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>Azure Disk Encryption für Windows

[Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) schützt ruhende personenbezogene Daten auf virtuellen Azure-Computern und lässt sich in Azure Key Vault integrieren. Azure Disk Encryption verwendet [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) unter Windows und [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) unter Linux zum Verschlüsseln der Betriebssystem- und Datenlaufwerke. Azure Disk Encryption wird auf Clients unter Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 sowie Windows 8 und Windows 10 unterstützt.

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a>Wie verwende ich Azure Disk Encryption zum Schützen personenbezogener Daten?

Zur Verwendung von Azure Disk Encryption benötigen Sie ein Abonnement für ein Azure-Konto. Führen Sie folgende Schritte aus, um Azure Disk Encryption für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Verwenden Sie die Resource Manager-Vorlage für Azure Disk Encryption, PowerShell oder die Befehlszeilenschnittstelle (CLI), um die Datenverschlüsselung zu aktivieren und die Verschlüsselungskonfiguration anzugeben. 

2. Gewähren Sie Zugriff auf die Azure-Plattform zum Lesen des Verschlüsselungsmaterials aus Ihrem Schlüsseltresor.

3. Geben Sie die Identität einer Azure Active Directory-Anwendung (AAD) an, um das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor zu schreiben.

In Azure werden die Konfiguration des virtuellen Computers und des Schlüsseltresors aktualisiert sowie Ihr verschlüsselter virtueller Computer eingerichtet.

Wenn Sie Ihren Schlüsseltresor zur Unterstützung von Azure Disk Encryption einrichten, können Sie einen Schlüsselverschlüsselungsschlüssel (KEK, Key Encryption Key) zur erhöhten Sicherheit und zur Unterstützung der Sicherung verschlüsselter virtueller Computer hinzufügen.

![](media/protect-personal-data-at-rest/create-key.png)

Ausführliche Anweisungen zu bestimmten Bereitstellungsszenarien und Benutzeroberflächen finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption).

### <a name="azure-storage-service-encryption"></a>Azure Storage Service Encryption

[Azure Storage Service Encryption (SSE) für ruhende Daten](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Azure Storage verschlüsselt Ihre Daten automatisch mittels 256-Bit-AES-Verschlüsselung, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Dieser Dienst ist für Azure-Blobs und -Dateien verfügbar.

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a>Wie verwende ich Storage Service Encryption zum Schützen personenbezogener Daten?

Führen Sie folgende Schritte aus, um Storage Service Encryption zu aktivieren:

1. Melden Sie sich im Azure-Portal an.

2. Wählen Sie ein Speicherkonto aus.

3. Wählen Sie unter „Einstellungen“ im Abschnitt „Blob-Dienst“ die Option „Verschlüsselung“ aus.

4. Wählen Sie im Abschnitt „Dateidienst“ die Option „Verschlüsselung“ aus.

Nach Klicken auf die Einstellung „Verschlüsselung“ können Sie Storage Service Encryption aktivieren oder deaktivieren.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Neue Daten werden verschlüsselt. Daten in vorhandenen Dateien in diesem Speicherkonto bleiben unverschlüsselt.

Kopieren Sie nach dem Aktivieren der Verschlüsselung Daten in das Speicherkonto. Verwenden Sie dazu eine der folgenden Methoden:

1. Kopieren Sie Blobs oder Dateien mit dem [Befehlszeilenprogramm AzCopy](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).

2. [Binden Sie eine Dateifreigabe mit SMB ein](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows), damit Sie ein Hilfsprogramm wie Robocopy zum Kopieren von Dateien verwenden können.

3. Kopieren Sie Blob- oder Dateidaten mithilfe von [Storage-Clientbibliotheken wie .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs) in den oder aus dem Blobspeicher oder zwischen Speicherkonten.

4.  Verwenden Sie einen [Speicher-Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) zum Hochladen von Blobs in Ihr Speicherkonto mit aktivierter Verschlüsselung.

### <a name="transparent-data-encryption"></a>Transparent Data Encryption

Transparent Data Encryption (TDE) ist eine Funktion in SQL Azure, über die Sie Daten auf Datenbank- sowie auf Serverebene verschlüsseln können. TDE ist jetzt bei allen neu erstellten Datenbanken standardmäßig aktiviert. TDE führt die E/A-Verschlüsselung und -Entschlüsselung der Daten- und Protokolldateien in Echtzeit durch.

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a>Wie verwende ich TDE zum Schützen personenbezogener Daten?

Sie können TDE über das Azure-Portal, mithilfe der REST-API oder mithilfe von PowerShell konfigurieren. Führen Sie folgende Schritte aus, um TDE für eine vorhandene Datenbank über das Azure-Portal zu aktivieren:

1. Öffnen Sie das Azure-Portal unter <https://portal.azure.com>, und melden Sie sich mit Ihrem Azure-Administrator- oder Mitwirkendenkonto an.

2. Klicken Sie auf dem linken Banner auf DURCHSUCHEN und dann auf „SQL-Datenbanken“.

3. Klicken Sie bei ausgewählter Option „SQL-Datenbanken“ im linken Bereich auf Ihre Benutzerdatenbank.

4. Klicken Sie auf dem Blatt der Datenbank auf „Alle Einstellungen“.

5. Klicken Sie auf dem Blatt „Einstellungen“ auf den Bereich „Transparent Data Encryption“, um das Blatt „Transparent Data Encryption“ zu öffnen.

6. Schieben Sie auf dem Blatt „Datenverschlüsselung“ die Schaltfläche „Datenverschlüsselung“ auf „Ein“, und klicken Sie dann auf „Speichern“ (oben auf der Seite), um die Einstellung zu übernehmen. Der Verschlüsselungsstatus zeigt annähernd den Fortschritt der Transparent Data Encryption an.

![Aktivieren der Datenverschlüsselung](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

Anweisungen zum Aktivieren von TDE und Informationen zum Entschlüsseln von TDE-geschützten Datenbanken sowie weitere Informationen finden Sie im Artikel [Transparent Data Encryption mit Azure SQL-Datenbank](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database).

## <a name="summary"></a>Zusammenfassung

Das Unternehmen kann das Ziel der Verschlüsselung von in der Azure-Cloud gespeicherten personenbezogenen Daten umsetzen. Dazu kann durch Verwendung von Azure Disk Encryption ein Schutz vollständiger Volumes erfolgen. Dies kann die Betriebssystemdateien sowie die Datendateien umfassen, die personenbezogene und andere sensible Daten enthalten. Mithilfe von Azure Storage Service Encryption können personenbezogene Daten geschützt werden, die in Blobs und Dateien gespeichert sind. Für in Azure SQL-Datenbanken gespeicherte Daten bietet Transparent Data Encryption Schutz vor nicht autorisierter Offenlegung personenbezogener Informationen.

Zum Schützen der Schlüssel, mit denen Daten in Azure verschlüsselt werden, kann das Unternehmen Azure Key Vault verwenden. Azure Key Vault optimiert die Schlüsselverwaltung und ermöglicht dem Unternehmen, die Kontrolle über Schlüssel zu behalten, die für den Datenzugriff und die Verschlüsselung von personenbezogenen Daten verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

- [Leitfaden zur Azure Disk Encryption-Problembehandlung](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [Verschlüsseln eines virtuellen Azure-Computers](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Datenverschlüsselung in Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [Verschlüsselung ruhender Daten von Datenbanken mit Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
