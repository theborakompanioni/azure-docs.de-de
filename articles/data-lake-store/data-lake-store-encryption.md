---
title: "Verschlüsselung in Azure Data Lake Store | Microsoft-Dokumentation"
description: "Grundlegendes zur Funktionsweise der Verschlüsselung und der Schlüsselrotation in Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 9bd9996a4a22b2f57510b6f3b6625e22b3183b1c
ms.contentlocale: de-de
ms.lasthandoff: 04/29/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Datenverschlüsselung in Azure Data Lake Store

## <a name="overview-of-encryption-in-azure-data-lake-store"></a>Übersicht über die Verschlüsselung in Azure Data Lake Store

Mit der Verschlüsselung in Azure Data Lake Store (ADLS) können Sie Ihre Daten schützen, Sicherheitsrichtlinien für Unternehmen implementieren und gesetzliche Vorschriften einhalten. Dieser Artikel enthält eine Übersicht über das Design und behandelt technische Aspekte der Datenverschlüsselungsimplementierung in Data Lake Store.

ADLS unterstützt die standardmäßig aktivierte, transparente Verschlüsselung ruhender Daten. Das bedeutet Folgendes:

* Standardmäßig aktiviert: Bei der Erstellung eines neuen Azure Data Lake Store-Kontos wird die Verschlüsselung standardmäßig aktiviert. In Data Lake Store gespeicherte Daten werden daraufhin immer verschlüsselt, bevor sie auf persistenten Medien gespeichert werden. Das gilt für alle Daten und kann nach der Kontoerstellung nicht mehr geändert werden.
* Transparent: Daten werden von ADLS vor der persistenten Speicherung automatisch verschlüsselt und vor dem Abrufen entschlüsselt. Die Verschlüsselung wird auf der Data Lake Store-Ebene von einem Administrator konfiguriert und verwaltet. Die Datenzugriffs-APIs werden nicht geändert. Folglich sind auch keine verschlüsselungsbedingten Änderungen in Anwendungen und Diensten erforderlich, die mit Data Lake Store interagieren.

Auch übertragene Daten (Daten in Bewegung) werden in Data Lake Store immer verschlüsselt. Neben der Verschlüsselung von Daten vor der Speicherung auf persistenten Medien werden Daten auch während der Übertragung oder in Bewegung immer über HTTPS (HTTP über Secure Sockets Layer) geschützt. Für die Data Lake Store-REST-Schnittstellen wird ausschließlich das HTTPS-Protokoll unterstützt.

![Abbildung 1](./media/data-lake-store-encryption/fig1.png)


## <a name="setting-up-encryption-with-azure-data-lake-store"></a>Einrichtung der Verschlüsselung mit Azure Data Lake Store

Die Verschlüsselung für Azure Data Lake Store wird im Rahmen der Kontoerstellung eingerichtet und ist standardmäßig immer aktiviert. Kunden können Schlüssel entweder selbst verwalten oder die Schlüsselverwaltung Azure Data Lake Store überlassen (Standardeinstellung).

Informationen zum Einrichten der Verschlüsselung mit Azure Data Lake Store finden Sie in den [ersten Schritten](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="under-the-hood--how-encryption-works-in-azure-data-lake-store"></a>Hintergrundinformationen zur Funktionsweise der Verschlüsselung in Azure Data Lake Store

### <a name="master-encryption-keys"></a>Masterverschlüsselungsschlüssel

Azure Data Lake Store bietet zwei Verwaltungsmodi für Masterverschlüsselungsschlüssel (Master Encryption Keys, MEKs). Die Verwendung von Masterverschlüsselungsschlüsseln wird weiter unten ausführlicher erläutert. Stellen Sie sich den Masterverschlüsselungsschlüssel vorerst einfach als Schlüssel der obersten Ebene vor. Zugriff auf den Masterverschlüsselungsschlüssel ist erforderlich, um in Data Lake Store gespeicherte Daten entschlüsseln zu können.

Zur Verwaltung des Masterverschlüsselungsschlüssels stehen die beiden folgenden Modi zur Verfügung:

1.    Vom Dienst verwaltete Schlüssel
2.    Vom Kunden verwaltete Schlüssel

In beiden Modi wird der Masterverschlüsselungsschlüssel zu seinem Schutz in Azure Key Vault gespeichert. Azure Key Vault ist ein vollständig verwalteter, äußerst sicherer Dienst in Azure, der zur sicheren Aufbewahrung kryptografischer Schlüssel verwendet werden kann. Weitere Informationen zu Azure Key Vault finden Sie [hier](https://azure.microsoft.com/services/key-vault).

Im Anschluss folgt eine kurze Gegenüberstellung der Funktionen, die in den beiden MEK-Verwaltungsmodi zur Verfügung stehen:

|  | Vom Dienst verwaltete Schlüssel | Vom Kunden verwaltete Schlüssel |
| --- | --- | --- |
|Wie werden die Daten gespeichert?|Immer verschlüsselt (vor dem Speichern)|Immer verschlüsselt (vor dem Speichern)|
|Wo wird der Masterverschlüsselungsschlüssel gespeichert?|Azure-Schlüsseltresor|Azure-Schlüsseltresor|
|Werden Verschlüsselungsschlüssel unverschlüsselt außerhalb von Azure Key Vault gespeichert?|Nein|Nein|
|Kann der MEK aus Azure Key Vault abgerufen werden?|Nein. Nach der Speicherung im Schlüsseltresor kann der Schlüssel ausschließlich zur Ver- und Entschlüsselung verwendet werden.|Nein. Nach der Speicherung im Schlüsseltresor kann der Schlüssel ausschließlich zur Ver- und Entschlüsselung verwendet werden.|
|Wer ist für Azure Key Vault und den MEK zuständig?|Der Azure Data Lake Store-Dienst.|Der Kunde ist für Azure Key Vault zuständig (gehört zum eigenen Azure-Abonnement). Der MEK im Schlüsseltresor kann per Software oder Hardware (HSM) verwaltet werden.|
|Kann der Kunde den Zugriff auf den MEK für den Azure Data Lake Store-Dienst sperren?|Nein|Ja. Er kann die Zugriffssteuerungslisten in Azure Key Vault verwalten und Zugriffssteuerungseinträge für die Dienstidentität für den Azure Data Lake Store-Dienst entfernen.|
|Kann der Kunde den MEK endgültig löschen?|Nein|Ja. Wenn der Kunde den MEK aus Azure Key Vault löscht, können die Daten im ADLS-Konto von niemandem mehr entschlüsselt werden (auch nicht vom Azure Data Lake Store-Dienst). <br><br> Falls der MEK vor dem Löschen aus Azure Key Vault explizit vom Kunden gesichert wurde, können der Schlüssel und anschließend auch die Daten wiederhergestellt werden. Wurde der MEK allerdings vor dem Löschen aus Azure Key Vault nicht vom Kunden gesichert, können die Daten im ADLS-Konto nicht mehr entschlüsselt werden.|


Abgesehen von dem grundlegenden Unterschied, wer den MEK und den Schlüsseltresor verwaltet, in dem er sich befindet, ist das Design bei beiden Modi gleich.

Bei der Wahl des Modus für Masterverschlüsselungsschlüssel sind einige wichtige Aspekte zu beachten.

1.    Beim Bereitstellen eines ADLS-Kontos haben Sie die Wahl zwischen vom Kunden verwalteten Schlüsseln und von ADLS verwalteten Schlüsseln.
2.    Nach der Bereitstellung eines ADLS-Kontos kann der Modus nicht mehr geändert werden.

### <a name="encryption-and-decryption-of-data"></a>Ver- und Entschlüsselung von Daten

Im Rahmen der Datenverschlüsselung für Azure Data Lake gibt es drei Arten von Schlüsseln. Die folgende Tabelle gibt Aufschluss über deren jeweilige Rolle:

| Schlüssel                   | Abkürzung | Verknüpft mit | Speicherort                             | Typ       | Hinweise                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Masterverschlüsselungsschlüssel | MEK          | Einem ADLS-Konto | Azure-Schlüsseltresor                              | Asymmetrisch | Kann von ADLS oder vom Kunden verwaltet werden                                                              |
| Datenverschlüsselungsschlüssel   | DEK          | Einem ADLS-Konto | Persistenter Speicher (vom ADLS-Dienst verwaltet) | Symmetrisch  | Der DEK wird durch den MEK verschlüsselt und vom Dienst auf persistenten Medien gespeichert. |
| Blockverschlüsselungsschlüssel  | BEK          | Einem Datenblock | Keine                                         | Symmetrisch  | Der BEK wird vom DEK und dem Datenblock abgeleitet.                                                      |

Das folgende Diagramm veranschaulicht dieses Konzepte:

![Abbildung 2](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudoalgorithmus, wenn eine Datei entschlüsselt werden soll:
1.    Überprüfen Sie, ob der DEK für das ADLS-Konto zwischengespeichert und verwendungsbereit ist.
    * Falls nicht, lesen Sie den verschlüsselten DEK aus dem persistenten Speicher, und senden Sie ihn zur Entschlüsselung an Azure Key Vault. Speichern Sie den entschlüsselten DEK im Arbeitsspeicher zwischen. Daraufhin kann er verwendet werden.
2.    Gehen Sie für jeden Datenblock in der Datei wie folgt vor:
    * Lesen Sie den verschlüsselten Datenblock aus dem persistenten Speicher.
    * Generieren Sie den BEK auf der Grundlage des DEKs und des verschlüsselten Datenblocks.
    * Entschlüsseln Sie die Daten mithilfe des BEKs.
#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudoalgorithmus, wenn ein Datenblock verschlüsselt werden soll:
1.    Überprüfen Sie, ob der DEK für das ADLS-Konto zwischengespeichert und verwendungsbereit ist.
    * Falls nicht, lesen Sie den verschlüsselten DEK aus dem persistenten Speicher, und senden Sie ihn zur Entschlüsselung an Azure Key Vault. Speichern Sie den entschlüsselten DEK im Arbeitsspeicher zwischen. Daraufhin kann er verwendet werden.
2.    Generieren Sie auf der Grundlage des DEKs einen eindeutigen BEK für den Datenblock.
3.    Verschlüsseln Sie den Datenblock mit dem BEK und einer AES-256-Verschlüsselung.
4.    Speichern Sie den verschlüsselten Datenblock im persistenten Speicher.

> [!NOTE] 
> Zur Verbesserung der Leistung wird der Klartext-Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) kurzzeitig flüchtig im Arbeitsspeicher zwischengespeichert und anschließend sofort wieder gelöscht. Auf einem persistenten Medium wird er vor dem Speichern immer durch den Masterverschlüsselungsschlüssel (Master Encryption Key, MEK) verschlüsselt.

## <a name="key-rotation"></a>Schlüsselrotation

Bei vom Kunden verwalteten Schlüsseln ermöglicht Azure Data Lake Store die Rotation des Masterverschlüsselungsschlüssels (Master Encryption Key, MEK). Informationen zum Einrichten eines ADLS-Kontos mit vom Kunden verwalteten Schlüsseln finden Sie auf der [Seite mit den ersten Schritten](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="pre-requisites"></a>Voraussetzungen

Bei der Einrichtung des Azure Data Lake-Kontos haben sich die Kunden für die Verwendung ihrer eigenen Schlüssel entschieden. Diese Option kann nach der Kontoerstellung nicht mehr geändert werden. Wenn Sie die Standardoptionen für die Verschlüsselung verwenden, werden Ihre Daten immer unter Verwendung von Schlüsseln verschlüsselt, die von Azure Data Lake verwaltetet werden. Bei dieser Option steht dem Kunden keine Schlüsselrotation zur Verfügung, da die Schlüssel von Azure Data Lake verwaltet werden. Bei den folgenden Schritten wird davon ausgegangen, dass Sie vom Kunden verwaltete Schlüssel verwenden (also Ihre eigenen Schlüssel aus Ihrem Schlüsseltresor ausgewählt haben).

### <a name="how-to-rotate-the-key-mek-in-azure-data-lake-store"></a>Rotieren des Schlüssels (MEK) in Azure Data Lake Store

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com/) an.
2. Navigieren Sie zu dem Schlüsseltresor mit den Schlüsseln, die Ihrem Azure Data Lake Store-Konto zugeordnet sind, und wählen Sie „Schlüssel“ aus.

    ![Schlüssel](./media/data-lake-store-encryption/keyvault.png)

3.    Wählen Sie den Schlüssel aus, der Ihrem Azure Data Lake Store-Konto zugeordnet ist, und erstellen Sie eine neue Version dieses Schlüssels.
  
   Bislang unterstützt Azure Data Lake nur die Schlüsselrotation zu einer neuen Version eines Schlüssels. Die Rotation zu einem anderen Schlüssel wird nicht unterstützt.

   ![Neue Version](./media/data-lake-store-encryption/keynewversion.png)

4.    Navigieren Sie zu dem Azure Data Lake Store-Konto, und wählen Sie „Verschlüsselung“ aus.

    ![Neue Version](./media/data-lake-store-encryption/select-encryption.png)

5.    Sie werden darauf hingewiesen, dass eine neue Schlüsselversion des Schlüssels verfügbar ist. Außerdem steht eine Schaltfläche zur Verfügung, über die Sie die Schlüsselrotation zu dieser neuen Version ausführen können. Klicken Sie auf „Schlüssel rotieren“, um den Schlüssel auf die neue Version zu aktualisieren.

    ![Fertig](./media/data-lake-store-encryption/rotatekey.png)

6. Der Vorgang dauert in der Regel weniger als zwei Minuten, und bei der Schlüsselrotation sind keine Ausfallzeiten zu erwarten. Nach Abschluss des Vorgangs wird die neue Version des Schlüssels verwendet.

