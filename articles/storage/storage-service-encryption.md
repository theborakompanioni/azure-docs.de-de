<properties
	pageTitle="Azure Storage Service Encryption für ruhende Daten | Microsoft Azure"
	description="Verwenden Sie Azure Storage Service Encryption, um Ihren Azure-Blobspeicher auf der Dienstseite zu verschlüsseln, wenn die Daten gespeichert werden, und beim Abrufen der Daten zu entschlüsseln."
	services="storage"
	documentationCenter=".net"
	authors="robinsh"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/08/2016"
	ms.author="robinsh"/>

# Azure Storage Service Encryption für ruhende Daten

Azure Storage Service Encryption (SSE) für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Verschlüsselung, Entschlüsselung und Schlüsselverwaltung sind für Benutzer vollständig transparent.

Die folgenden Abschnitte enthalten eine ausführliche Anleitung zur Verwendung der Features von Storage Service Encryption sowie die unterstützten Szenarien und Benutzererfahrungen.

## Übersicht


Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Daten können während der Übertragung zwischen einer Anwendung und Azure mit [clientseitiger Verschlüsselung](storage-client-side-encryption.md), HTTPS oder SMB 3.0 geschützt werden. Storage Service Encryption (SSE) bietet eine Verschlüsselung ruhender Daten, die Verarbeitung der Ver- und Entschlüsselung sowie die Schlüsselverwaltung auf vollständig transparente Weise. Sämtliche Daten werden mittels 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

SSE verschlüsselt die Daten, wenn sie in Azure Storage geschrieben werden, und kann für Blockblobs, Seitenblobs und Anfügeblobs verwendet werden. Das Feature funktioniert wie folgt:

-   Allgemeine Speicherkonten und Blobspeicherkonten
-   Standardspeicher und Primärspeicher
-   Alle Redundanzebenen (LRS, ZRS, GRS, RA-GRS)
-   Azure Resource Manager-Speicherkonten (nicht jedoch klassische Konten)
-   Alle Regionen

Dieser Screenshot zeigt, wo Sie die Storage Service Encryption-Einstellung mit dem [Azure-Portal](https://azure.portal.com) finden. Klicken Sie auf diesem Bildschirm auf „Verschlüsselung“, um fortzufahren.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)

Nach Klicken auf die Einstellung „Verschlüsselung“ können Sie Storage Service Encryption aktivieren oder deaktivieren.

![Portal-Screenshot mit Verschlüsselungseigenschaften](./media/storage-service-encryption/image2.png)

##Verschlüsselungsszenarien

Storage Service Encryption kann auf einer Speicherkontoebene aktiviert werden. Folgende Kundenszenarien werden unterstützt:

-   Verschlüsselung von Block-, Anfüge- und Seitenblobs.

-   Verschlüsselung von lokalen archivierten VHDs und Vorlagen, die in Azure eingebracht werden.

-   Verschlüsselung der zu Grunde liegenden Betriebssystemdatenträger und sonstigen Datenträger für IaaS-VMs, die Sie mit Ihren VHDs erstellt haben.

Für SSE gelten die folgenden Einschränkungen:

-   Verschlüsselung von klassischen Speicherkonten wird nicht unterstützt.

-   Verschlüsselung von klassischen, zu Resource Manager-Speicherkonten migrierten Speicherkonten wird nicht unterstützt.

-   Vorhandene Daten – SSE verschlüsselt nur nach Aktivierung der Verschlüsselung neu erstellte Daten. Wenn Sie z.B. ein neues Resource Manager-Speicherkonto erstellen, jedoch keine Verschlüsselung aktivieren, und dann Blobs oder archivierte VHDs in das Speicherkonto hochladen und SSE aktivieren, werden diese Blobs nicht verschlüsselt, sofern sie nicht neu geschrieben oder kopiert werden.

-   Unterstützung für Marketplace – Aktivieren der Verschlüsselung von virtuellen Computern, die über den Marketplace mittels [Azure-Portal](https://portal.azure.com), PowerShell und Azure-Befehlszeilenschnittstelle erstellt werden. Das VHD-Basisimage bleibt unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

-   Tabellen, Warteschlangen und Dateidaten werden nicht verschlüsselt.

##Erste Schritte

###Schritt 1: [Erstellen eines neuen Speicherkontos](storage-create-storage-account.md)

###Schritt 2: Aktivieren der Verschlüsselung

Sie können die Verschlüsselung im [Azure-Portal](https://portal.azure.com) aktivieren.

> [AZURE.NOTE] Wenn Sie SSE programmgesteuert für ein Speicherkonto aktivieren oder deaktivieren möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://msdn.microsoft.com/library/azure/mt163683.aspx), die [Clientbibliothek des Speicherressourcenanbieters für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md) oder die [Azure-CLI](storage-azure-cli.md) verwenden.

###Schritt 3: Kopieren von Daten in das Speicherkonto

Wenn Sie SSE für ein Speicherkonto aktivieren und dann Blobs in dieses Speicherkonto schreiben, werden die Blobs verschlüsselt. Alle Blobs, die sich bereits in diesem Speicherkonto befinden, werden erst verschlüsselt, nachdem sie erneut geschrieben wurden. Sie können die Daten aus einem Speicherkonto in ein mit SSE verschlüsseltes Konto kopieren und sogar SSE aktivieren und die Blobs aus einem Container in einen anderen kopieren, um sicherzustellen, dass ältere Daten verschlüsselt werden. Sie können dazu eines der folgenden Tools verwenden.

#### Verwenden von AzCopy

AzCopy ist ein Windows Befehlszeilenprogramm, das zum Kopieren von Daten zu und von Microsoft Azure Blob, File und Table Storage entwickelt wurde, wobei durch einfache Befehle optimale Leistung erzielt wird. Mit diesem Tool können Sie Ihre Blobs aus einem Speicherkonto in ein anderes kopieren, für das SSE aktiviert ist.

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### Verwenden von Speicherclientbibliotheken

Sie können mithilfe unseres umfangreichen Angebots an Speicherclientbibliotheken, einschließlich .NET, C++, Java, Android, Node.js, PHP, Python und Ruby, Blobdaten in und aus Blobspeicher oder zwischen Speicherkonten kopieren.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

#### Verwenden eines Storage-Explorers

Mit einem Storage-Explorer können Sie Speicherkonten erstellen, Daten hoch- und herunterladen, Inhalte von Blobs anzeigen und durch Verzeichnisse navigieren. Sie können eines dieser Konten zum Hochladen von Blobs in Ihr Speicherkonto mit aktivierter Verschlüsselung verwenden. Mit einigen Storage-Explorern können Sie auch Daten aus Ihrem vorhandenen Blobspeicher in einen anderen Container in Ihrem Speicherkonto oder ein neues Speicherkonto kopieren, für das SSE aktiviert ist.

Weitere Informationen finden Sie unter [Microsoft Azure-Speicher-Explorer](storage-explorers.md).

###Schritt 4: Abfragen des Status der verschlüsselten Daten

Eine aktualisierte Version der Speicherclientbibliotheken wurde bereitgestellt, mit denen Sie den Status eines Objekts ermitteln können, um zu bestimmen, ob es verschlüsselt ist oder nicht. In naher Zukunft werden diesem Dokument Beispiele hinzugefügt.

Rufen Sie in der Zwischenzeit [Kontoeigenschaften abrufen](https://msdn.microsoft.com/library/azure/mt163553.aspx) auf, um zu überprüfen, ob die Verschlüsselung für das Speicherkonto aktiviert wurde, oder zeigen Sie die Eigenschaften des Speicherkontos im Azure-Portal an.

##Verschlüsselungs- und Entschlüsselungsworkflow

Hier ist eine kurze Beschreibung des Verschlüsselungs-/Entschlüsselungsworkflows:

-   Der Kunde aktiviert die Verschlüsselung für das Speicherkonto.

-   Wenn der Kunde neue Daten (PUT Blob, PUT Block, PUT Page usw.) in den Blobspeicher schreibt, wird jeder Schreibvorgang mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

-   Wenn der Kunde auf Daten zugreifen muss (GET Blob usw.), werden die Daten vor der Rückgabe an den Benutzer automatisch entschlüsselt.

-   Wenn die Verschlüsselung deaktiviert ist, werden neue Schreibvorgänge nicht mehr verschlüsselt, und vorhandene verschlüsselte Daten bleiben verschlüsselt, bis sie vom Benutzer neu geschrieben werden. Während die Verschlüsselung aktiviert ist, werden Schreibvorgänge in den Blobspeicher verschlüsselt. Der Zustand der Daten ändert sich nicht, wenn der Benutzer zwischen Aktivieren und Deaktivieren der Verschlüsselung für das Speicherkonto umschaltet.

-   Alle Verschlüsselungsschlüssel werden von Microsoft gespeichert, verschlüsselt und verwaltet.

##Häufig gestellte Fragen zu Storage Service Encryption für ruhende Daten

**F: Ich habe ein klassisches Speicherkonto. Kann ich dafür SSE aktivieren?**

A: Nein, SSE wird nur für Resource Manager-Speicherkonten unterstützt.

**F: Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**

A: Sie können ein neues Resource Manager-Speicherkonto erstellen und Ihre Daten mit [AzCopy](storage-use-azcopy.md) aus Ihrem vorhandenen klassischen Speicherkonto in Ihr neu erstelltes Resource Manager-Speicherkonto kopieren.

**F: Ich habe ein vorhandenes Resource Manager-Speicherkonto. Kann ich dafür SSE aktivieren?**

A: Ja, aber nur neu geschriebene Blobs werden verschlüsselt. Ältere, bereits vorhandene Daten werden nicht verschlüsselt.

**F: Kann ich die aktuellen Daten in einem vorhandenen Resource Manager-Speicherkonto verschlüsseln?**

A: Sie können SSE jederzeit in einem Resource Manager-Speicherkonto aktivieren. Blobs, die bereits vorhanden waren, werden jedoch nicht verschlüsselt. Um diese Blobs zu verschlüsseln, können Sie sie unter einem anderen Namen oder in einen anderen Container kopieren und anschließend die nicht verschlüsselten Versionen entfernen.

**F: Ich verwende Storage Premium; kann ich SSE verwenden?**

A: Ja, SSE wird von Standard Storage und Premium Storage unterstützt.

**F: Wenn ich ein neues Speicherkonto erstelle, SSE aktiviere und dann eine neue VM erstelle, die dieses Speicherkonto verwendet, bedeutet dies, dass mein virtueller Computer verschlüsselt ist?**

A: Ja. Alle erstellten Datenträger, die das neue Speicherkonto verwenden, werden verschlüsselt, sofern sie nach der SSE-Aktivierung erstellt wurden. Wurde die VM mit Azure Market Place erstellt, bleibt das VHD-Basisimage unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

**F: Kann ich neue Speicherkonten mithilfe von Azure PowerShell und Azure-CLI erstellen, wenn SSE aktiviert ist?**

A: Ja.

**F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?**

A: Es fallen keine zusätzlichen Kosten an.

**F: Wer verwaltet die Verschlüsselungsschlüssel?**

A: Die Schlüssel werden von Microsoft verwaltet.

**F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?**

A: Wir arbeiten darauf hin, dass Kunden ihre eigenen Verschlüsselungsschlüssel verwenden können.

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Derzeit nicht; die Schlüssel werden vollständig von Microsoft verwaltet.

**F: Ist SSE standardmäßig aktiviert, wenn ich ein neues Speicherkonto erstelle?**

A: SSE ist nicht standardmäßig aktiviert. Sie können dieses Feature im Azure-Portal aktivieren. Außerdem können Sie dieses Feature programmgesteuert mit der Speicherressourcenanbieter-REST-API aktivieren.

**F: Wo liegt der Unterschied zu Azure Drive Encryption?**

A: Dieses Feature wird zum Verschlüsseln von Daten im Azure-Blobspeicher verwendet. Azure Disk Encryption wird zum Verschlüsseln von Betriebssystemdatenträgern und sonstigen Datenträgern in virtuellen IaaS-Computern verwendet. Weitere Informationen finden Sie in unserem [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

**F: Was geschieht, wenn ich SSE aktiviere, und dann Azure Disk Encryption auf den Datenträgern aktiviere?**

A: Dies funktioniert nahtlos. Ihre Daten werden durch beide Methoden verschlüsselt.

**F: Mein Speicherkonto ist so eingerichtet, dass es georedundant repliziert wird. Werden meine redundanten Kopien ebenfalls verschlüsselt, wenn ich SSE aktiviere?**

A: Ja, alle Kopien des Speicherkontos werden verschlüsselt, und alle Redundanzoptionen – lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS), georedundanter Speicher (GRS) und lesezugriff-georedundanter Speicher (RA-GRS, Read-Access Geo Redundant) werden unterstützt.

**F: Ich kann die Verschlüsselung nicht für mein Speicherkonto aktivieren.**

A: Handelt es sich um ein Resource Manager-Speicherkonto? Klassische Speicherkonten werden nicht unterstützt.

**F: Ist SSE nur in bestimmten Regionen möglich?**

A: SSE ist in allen Regionen verfügbar.

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Bitte wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

##Nächste Schritte

Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

<!---HONumber=AcomDC_0921_2016-->