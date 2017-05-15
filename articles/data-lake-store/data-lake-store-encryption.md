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
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 20444d368c568ee716ff242e33323b91ffd198eb
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Datenverschlüsselung in Azure Data Lake Store

Mit der Verschlüsselung in Azure Data Lake Store können Sie Ihre Daten schützen, Sicherheitsrichtlinien für Unternehmen implementieren und gesetzliche Vorschriften einhalten. Dieser Artikel enthält eine Übersicht über das Design und behandelt einige technische Aspekte der Implementierung.

Data Lake Store unterstützt die Verschlüsselung ruhender sowie übertragener Daten. Für ruhende Daten unterstützt Data Lake Store die standardmäßig aktivierte, transparente Verschlüsselung. Das bedeutet Folgendes:

* **Standardmäßig aktiviert**: Beim Erstellen eines neuen Data Lake Store-Kontos wird die Verschlüsselung standardmäßig aktiviert. In Data Lake Store gespeicherte Daten werden daraufhin immer verschlüsselt, bevor sie auf persistenten Medien gespeichert werden. Das gilt für alle Daten und kann nach der Kontoerstellung nicht mehr geändert werden.
* **Transparent**: Daten werden von Data Lake Store vor der persistenten Speicherung automatisch verschlüsselt und vor dem Abrufen entschlüsselt. Die Verschlüsselung wird auf der Data Lake Store-Ebene von einem Administrator konfiguriert und verwaltet. Die Datenzugriffs-APIs werden nicht geändert. Folglich sind auch keine verschlüsselungsbedingten Änderungen in Anwendungen und Diensten erforderlich, die mit Data Lake Store interagieren.

Auch übertragene Daten (Daten in Bewegung) werden in Data Lake Store immer verschlüsselt. Zusätzlich zur Verschlüsselung von Daten vor dem Speichern auf persistenten Medien werden Daten auch während der Übertragung immer über HTTPS geschützt. Für die Data Lake Store-REST-Schnittstellen wird ausschließlich das HTTPS-Protokoll unterstützt. Das folgende Diagramm zeigt, wie Daten in Data Lake Store verschlüsselt werden:

![Diagramm der Datenverschlüsselung in Data Lake Store](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-store"></a>Einrichten der Verschlüsselung mit Data Lake Store

Die Verschlüsselung für Data Lake Store wird während der Kontoerstellung eingerichtet und ist standardmäßig immer aktiviert. Sie können die Schlüssel entweder selbst verwalten oder die Verwaltung durch Data Lake Store zulassen (dies ist die Standardeinstellung).

Weitere Informationen finden Sie in den [Ersten Schritten](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="how-encryption-works-in-data-lake-store"></a>Funktionsweise der Verschlüsselung in Data Lake Store

Im Folgenden wird beschrieben, wie Sie Masterverschlüsselungsschlüssel verwalten. Außerdem erhalten Sie eine Beschreibung der drei unterschiedlichen Schlüsseltypen, die Sie bei der Datenverschlüsselung für Data Lake Store verwenden können.

### <a name="master-encryption-keys"></a>Masterverschlüsselungsschlüssel

Data Lake Store bietet zwei Verwaltungsmodi für Masterverschlüsselungsschlüssel (Master Encryption Keys, MEKs). Stellen Sie sich den Masterverschlüsselungsschlüssel vorerst einfach als Schlüssel der obersten Ebene vor. Zugriff auf den Masterverschlüsselungsschlüssel ist erforderlich, um in Data Lake Store gespeicherte Daten entschlüsseln zu können.

Zur Verwaltung des Masterverschlüsselungsschlüssels stehen die beiden folgenden Modi zur Verfügung:

*    Vom Dienst verwaltete Schlüssel
*    Vom Kunden verwaltete Schlüssel

In beiden Modi wird der Masterverschlüsselungsschlüssel geschützt, indem er Azure Key Vault gespeichert wird. Key Vault ist ein vollständig verwalteter, äußerst sicherer Dienst in Azure, der zur sicheren Aufbewahrung kryptografischer Schlüssel verwendet werden kann. Weitere Informationen finden Sie unter [Key Vault](https://azure.microsoft.com/services/key-vault).

Im Anschluss folgt eine kurze Gegenüberstellung der Funktionen, die in den beiden MEK-Verwaltungsmodi zur Verfügung stehen:

|  | Vom Dienst verwaltete Schlüssel | Vom Kunden verwaltete Schlüssel |
| --- | --- | --- |
|Wie werden die Daten gespeichert?|Immer verschlüsselt (vor dem Speichern)|Immer verschlüsselt (vor dem Speichern)|
|Wo wird der Masterverschlüsselungsschlüssel gespeichert?|Key Vault|Key Vault|
|Werden Verschlüsselungsschlüssel unverschlüsselt außerhalb von Key Vault gespeichert? |Nein|Nein|
|Kann der MEK aus Key Vault abgerufen werden?|Nein. Wenn der MEK in Key Vault gespeichert wurde, kann er ausschließlich zur Ver- und Entschlüsselung verwendet werden.|Nein. Wenn der MEK in Key Vault gespeichert wurde, kann er ausschließlich zur Ver- und Entschlüsselung verwendet werden.|
|Wer ist der Eigentümer der Key Vault-Instanz und des MEK?|Der Data Lake Store-Dienst|Sie sind Eigentümer der Key Vault-Instanz, die zu Ihrem Azure-Abonnement gehört. Der MEK in Key Vault kann per Software oder Hardware verwaltet werden.|
|Können Sie den Zugriff auf den MEK für den Data Lake Store-Dienst sperren?|Nein|Ja. Sie können die Zugriffssteuerungslisten in Key Vault verwalten und Zugriffssteuerungseinträge für die Dienstidentität für den Data Lake Store-Dienst entfernen.|
|Können Sie den MEK endgültig löschen?|Nein|Ja. Wenn Sie den MEK aus Key Vault löschen, können die Daten im Data Lake Store-Konto von niemandem mehr entschlüsselt werden (auch nicht vom Data Lake Store-Dienst). <br><br> Wenn Sie den MEK vor dem Löschen aus Key Vault explizit gesichert haben, ist eine Wiederherstellung des MEK und der Daten möglich. Wenn Sie den MEK allerdings vor dem Löschen aus Key Vault nicht gesichert haben, können die Daten im Data Lake Store-Konto anschließend nicht mehr entschlüsselt werden.|


Abgesehen von dem Unterschied, wer den MEK und die Key Vault-Instanz verwaltet, in der er sich befindet, ist das Design bei beiden Modi gleich.

Beachten Sie beim Auswählen des Modus für die Masterverschlüsselungsschlüssel Folgendes:

*    Beim Bereitstellen eines Data Lake Store-Kontos haben Sie die Wahl zwischen vom Kunden und vom Dienst verwalteten Schlüsseln.
*    Nach der Bereitstellung eines Data Lake Store-Kontos kann der Modus nicht mehr geändert werden.

### <a name="encryption-and-decryption-of-data"></a>Ver- und Entschlüsselung von Daten

Im Rahmen der Datenverschlüsselung gibt es drei Arten von Schlüsseln. In der folgenden Tabelle erhalten Sie einen Überblick:

| Schlüssel                   | Abkürzung | Zugeordnet zu | Speicherort                             | Typ       | Hinweise                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Masterverschlüsselungsschlüssel | MEK          | Data Lake Store-Konto | Key Vault                              | Asymmetrisch | Kann von Data Lake Store oder von Ihnen verwaltet werden.                                                              |
| Datenverschlüsselungsschlüssel   | DEK          | Data Lake Store-Konto | Persistenter Speicher (vom Data Lake Store-Dienst verwaltet) | Symmetrisch  | Der DEK wird vom MEK verschlüsselt. Der verschlüsselte DEK ist das, was auf einem persistenten Medium gespeichert wird. |
| Blockverschlüsselungsschlüssel  | BEK          | Einem Datenblock | Keine                                         | Symmetrisch  | Der BEK wird vom DEK und dem Datenblock abgeleitet.                                                      |

Das folgende Diagramm veranschaulicht dieses Konzepte:

![Schlüssel bei der Datenverschlüsselung](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudoalgorithmus, wenn eine Datei entschlüsselt werden soll:
1.    Überprüfen Sie, ob der DEK für das Data Lake Store-Konto zwischengespeichert und zur Verwendung bereit ist.
    - Falls nicht, lesen Sie den verschlüsselten DEK aus dem persistenten Speicher, und senden Sie ihn zur Entschlüsselung an Key Vault. Der verschlüsselte DEK wird im Arbeitsspeicher zwischengespeichert. Er kann nun verwendet werden.
2.    Gehen Sie für jeden Datenblock in der Datei wie folgt vor:
    - Lesen Sie den verschlüsselten Datenblock aus dem persistenten Speicher.
    - Generieren Sie den BEK auf der Grundlage des DEK und des verschlüsselten Datenblocks.
    - Entschlüsseln Sie die Daten mithilfe des BEK.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudoalgorithmus, wenn ein Datenblock verschlüsselt werden soll:
1.    Überprüfen Sie, ob der DEK für das Data Lake Store-Konto zwischengespeichert und zur Verwendung bereit ist.
    - Falls nicht, lesen Sie den verschlüsselten DEK aus dem persistenten Speicher, und senden Sie ihn zur Entschlüsselung an Key Vault. Der verschlüsselte DEK wird im Arbeitsspeicher zwischengespeichert. Er kann nun verwendet werden.
2.    Generieren Sie auf der Grundlage des DEKs einen eindeutigen BEK für den Datenblock.
3.    Verschlüsseln Sie den Datenblock mit dem BEK mithilfe der AES-256-Verschlüsselung.
4.    Speichern Sie den verschlüsselten Datenblock im persistenten Speicher.

> [!NOTE] 
> Im Hinblick auf die Leistung wird der DEK unverschlüsselt für kurze Zeit im Arbeitsspeicher zwischengespeichert und anschließend sofort gelöscht. Auf einem persistenten Medium wird er vor dem Speichern immer durch den MEK verschlüsselt.

## <a name="key-rotation"></a>Schlüsselrotation

Wenn Sie vom Kunden verwaltete Schlüssel verwenden, ist eine Rotation des MEK möglich. Informationen zum Einrichten eines Data Lake Store-Kontos mit vom Kunden verwalteten Schlüsseln finden Sie in den [Ersten Schritten](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Voraussetzungen

Beim Einrichten des Data Lake Store-Kontos haben Sie ausgewählt, dass Sie Ihre eigenen Schlüssel verwenden. Diese Option kann nach der Kontoerstellung nicht mehr geändert werden. Bei den folgenden Schritten wird davon ausgegangen, dass Sie vom Kunden verwaltete Schlüssel verwenden (d.h. Sie haben Ihre eigenen Schlüssel aus Key Vault ausgewählt).

Wenn Sie die Standardverschlüsselungsoptionen verwenden, werden Ihre Daten immer durch von Data Lake Store verwaltete Schlüssel verschlüsselt. Bei dieser Option besteht die Möglichkeit der Schlüsselrotation nicht, da diese von Data Lake Store verwaltet werden.

### <a name="how-to-rotate-the-mek-in-data-lake-store"></a>Rotieren des MEK in Data Lake Store

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Navigieren Sie zu der Key Vault-Instanz mit den Schlüsseln, die Ihrem Data Lake Store-Konto zugeordnet sind. Wählen Sie **Schlüssel** aus.

    ![Screenshot von Key Vault](./media/data-lake-store-encryption/keyvault.png)

3.    Wählen Sie den Schlüssel aus, der Ihrem Data Lake Store-Konto zugeordnet ist, und erstellen Sie eine neue Version dieses Schlüssels. Data Lake Store unterstützt derzeit nur die Schlüsselrotation zu einer neuen Version eines Schlüssels. Die Rotation zu einem anderen Schlüssel wird nicht unterstützt.

   ![Screenshot des Fensters „Schlüssel“, „Neue Version“ hervorgehoben](./media/data-lake-store-encryption/keynewversion.png)

4.    Navigieren Sie zum Data Lake Store-Konto, und wählen Sie **Verschlüsselung** aus.

    ![Screenshot des Fensters des Data Lake Store-Speicherkontos, „Verschlüsselung“ hervorgehoben](./media/data-lake-store-encryption/select-encryption.png)

5.    Sie werden darüber benachrichtigt, dass eine neue Version des Schlüssels verfügbar ist. Klicken Sie auf **Schlüssel rotieren**, um den Schlüssel auf die neue Version zu aktualisieren.

    ![Screenshot des Data Lake Store-Fensters mit der Benachrichtigung, „Schlüssel rotieren“ hervorgehoben](./media/data-lake-store-encryption/rotatekey.png)

Der Vorgang dauert in der Regel weniger als zwei Minuten, und bei der Schlüsselrotation sind keine Ausfallzeiten zu erwarten. Nach Abschluss des Vorgangs wird die neue Version des Schlüssels verwendet.

