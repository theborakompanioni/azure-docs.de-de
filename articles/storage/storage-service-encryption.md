<properties
	pageTitle="Azure Storage Service Encryption für ruhende Daten (Vorschau) | Microsoft Azure"
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
	ms.date="07/11/2016"
	ms.author="robinsh"/>

# Azure Storage Service Encryption für ruhende Daten (Vorschau)

Azure Storage Service Encryption (SSE) für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Verschlüsselung, Entschlüsselung und Schlüsselverwaltung sind für Benutzer vollständig transparent.

Die folgenden Abschnitte enthalten eine ausführliche Anleitung zur Verwendung der Features von Storage Service Encryption sowie die unterstützten Szenarien und Benutzererfahrungen.

## Übersicht


Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Daten können während der Übertragung zwischen einer Anwendung und Azure mit [clientseitiger Verschlüsselung](storage-client-side-encryption.md), HTTPS oder SMB 3.0 geschützt werden. Storage Service Encryption ist ein neues Feature von Azure Storage, das Daten verschlüsselt, wenn sie in Azure Storage geschrieben werden, mit Unterstützung von Blockblobs, Seitenblobs und Anfügeblobs. Dieses Feature kann für neue Speicherkonten aktiviert werden, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt werden, und ist für alle Redundanzebenen (LRS, ZRS, GRS und RA-GRS) verfügbar. Storage Service Encryption ist für Standard Storage und Premium Storage, Verschlüsselung, Entschlüsselung und Schlüsselverwaltung auf vollständig transparente Weise verfügbar. Sämtliche Daten werden mittels 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren. Der Abschnitt „Vorschau“ im Folgenden beschreibt im Detail, wie Sie in das Vorschauprogramm für Storage Service Encryption einsteigen können.

Dieser Screenshot zeigt, wo Sie die Storage Service Encryption-Einstellung mit dem [Azure-Portal](https://azure.portal.com) finden. Klicken Sie auf diesem Bildschirm auf „Verschlüsselung“, um fortzufahren.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)

Nach Klicken auf die Einstellung „Verschlüsselung“ können Sie Storage Service Encryption aktivieren oder deaktivieren.

![Portal-Screenshot mit Verschlüsselungseigenschaften](./media/storage-service-encryption/image2.png)

##Availability

Für Storage Standard ist dieses Feature derzeit in den Regionen „Australien, Südosten“, „USA, Mitte“, „Asien, Osten“ „USA, Osten 2“, „Asien, Südosten“, „Europa, Westen“ und „USA, Westen“ verfügbar.

Für Storage Premium ist dieses Feature derzeit in den Regionen „Australien, Südosten“, „USA, Mitte“, „Asien, Osten“ „USA, Osten 2“, „Asien, Südosten“, „Japan, Osten“ und „USA, Westen“ verfügbar.

Wenn wir dieses Feature in weiteren Regionen einführen, werden wir dieses Dokument entsprechend aktualisieren.

##Verschlüsselungsszenarien

Storage Service Encryption kann auf einer Speicherkontoebene aktiviert werden. Folgende Kundenszenarien werden unterstützt:

-   Verschlüsselung von Block-, Anfüge- und Seitenblobs.

-   Verschlüsselung von lokalen archivierten VHDs und Vorlagen, die in Azure eingebracht werden.

-   Verschlüsselung der zu Grunde liegenden Betriebssystemdatenträger und sonstigen Datenträger für IaaS-VMs, die Sie mit Ihren VHDs erstellt haben.

Die öffentliche Vorschau hat folgende Einschränkungen:

-   Verschlüsselung von klassischen Speicherkonten wird nicht unterstützt.

-   Verschlüsselung von klassischen, zu Resource Manager-Speicherkonten migrierten Speicherkonten wird nicht unterstützt.

-   Vorhandene Daten – SSE verschlüsselt nur nach Aktivierung der Verschlüsselung neu erstellte Daten. Wenn Sie z.B. ein neues Resource Manager-Speicherkonto erstellen, jedoch keine Verschlüsselung aktivieren, und dann Blobs oder archivierte VHDs in das Speicherkonto hochladen und SSE aktivieren, werden diese Blobs nicht verschlüsselt, sofern sie nicht neu geschrieben oder kopiert werden.

-   Unterstützung für Marketplace – Aktivieren der Verschlüsselung von virtuellen Computern, die über den Marketplace mittels (Azure-Portal) [https://portal.azure.com], PowerShell und Azure-Befehlszeilenschnittstelle erstellt werden. Das VHD-Basisimage bleibt unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

-   Tabellen, Warteschlangen und Dateidaten werden nicht verschlüsselt.

##Vorschau

Dieses Feature wird nur für neu erstellte Resource Manager-Speicherkonten unterstützt. Klassische Speicherkonten werden nicht unterstützt. Um dieses neue Feature verwenden zu können, müssen Sie Ihr Abonnement mithilfe von PowerShell-Cmdlets registrieren. Nach Genehmigung Ihres Abonnements können Sie SSE für Ihr Speicherkonto unter dem zugelassenen Abonnement aktivieren. Wie bei fast jeder Vorschau gilt: Die Verwendung in der Produktion sollte erst dann beginnen, wenn das Feature allgemein verfügbar ist. Sie können unserer Storage Service Encryption-Vorschaugruppe auf Yammer beitreten, um über Ihre Erfahrungen zu berichten.

### Registrierung für die Vorschau

-   [Installieren Sie die Azure PowerShell-Cmdlets](../powershell-install-configure.md).

-   Führen Sie PowerShell unter Windows 10 als Administrator aus.

-   Registrieren Sie sich mit dem Speicherressourcenanbieter-Namespace. Dies ist nur für ein Abonnement erforderlich, das noch nicht mit dem Speicherressourcenanbieter registriert ist.

    `PS E:> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage" `

-   Um sich für die Funktion zu registrieren, können Sie das Register-AzureRmProviderFeature-PowerShell-Cmdlet verwenden.

    `Register-AzureRmProviderFeature -FeatureName "EncryptionAtRest" -ProviderNamespace "Microsoft.Storage"`

-   Um den Status Ihrer Registrierung abzufragen, um festzustellen, ob das Abonnement genehmigt wurde, können Sie das Get-AzureRmProviderFeature-PowerShell-Cmdlet verwenden.

    `Get-AzureRmProviderFeature -FeatureName "EncryptionAtRest" -ProviderNamespace "Microsoft.Storage"`

Wenn der Registrierungsstatus „Registriert“ zurückgegeben wird, wurde Ihr Abonnement genehmigt. Besuchen Sie auch unsere Azure Storage Service Encryption-Vorschaugruppe auf Yammer.

##Erste Schritte

###Schritt 1: [Anmelden für die Vorschau](#registering-for-preview)

###Schritt 2: [Erstellen eines neuen Speicherkontos](storage-create-storage-account.md)

###Schritt 3: Aktivieren der Verschlüsselung

Sie können die Verschlüsselung im [Azure-Portal](https://portal.azure.com) aktivieren.

> [AZURE.NOTE] Wenn Sie Storage Service Encryption programmgesteuert für ein Speicherkonto aktivieren oder deaktivieren möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://msdn.microsoft.com/library/azure/mt163683.aspx) verwenden. Wir fügen diese Möglichkeit der [Speicherressourcenanbieter-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), Azure PowerShell und Azure-CLI bald hinzu.

###Schritt 4: Kopieren von Daten in das Speicherkonto

#### Verwenden von AzCopy

AzCopy ist ein Windows Befehlszeilenprogramm, das zum Kopieren von Daten zu und von Microsoft Azure Blob, File und Table Storage entwickelt wurde, wobei durch einfache Befehle optimale Leistung erzielt wird. Mit diesem Dienstprogramm können Sie Daten aus einem vorhandenen Blobspeicherkonto in Ihr neues Speicherkonto mit aktivierter Verschlüsselungsfunktion kopieren.

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### Verwenden von Speicherclientbibliotheken

Sie können mit unserem umfangreichen Satz von Speicherclientbibliotheken einschließlich .NET, C++, Java, Android, Node.js, PHP, Python und Ruby Daten in den Blobspeicher hochladen und daraus herunterladen.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

#### Verwenden eines Storage-Explorers

Mit einem Storage-Explorer können Sie Speicherkonten erstellen, Daten hoch- und herunterladen, Inhalt von Blobs anzeigen und durch Verzeichnisse navigieren. Viele unterstützen klassische und Resource Manager-Speicherkonten.

Sie können eines dieser Konten zum Hochladen von Blobs in Ihr Speicherkonto mit aktivierter Verschlüsselung verwenden. Mit einigen Storage-Explorern können Sie auch Daten aus Ihrem vorhandenen Speicherkonto in Ihr neues Speicherkonto kopieren, in dem SSE aktiviert ist.

Weitere Informationen finden Sie unter [Microsoft Azure-Speicher-Explorer](storage-explorers.md).

###Schritt 5: Abfragen des Status der verschlüsselten Daten

Sobald SSE allgemein verfügbar ist, wird eine aktualisierte Version der Speicherclientbibliotheken bereitgestellt, mit denen Sie den Status eines Objekts ermitteln können, um zu bestimmen, ob es verschlüsselt ist oder nicht.

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

A: Nein, SSE wird in der Vorschau nur für neu erstellte Resource Manager-Speicherkonten unterstützt.

**F: Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**

A: Sie können ein neues Resource Manager-Speicherkonto erstellen und Ihre Daten mit [AzCopy](storage-use-azcopy.md) aus Ihrem vorhandenen klassischen Speicherkonto in Ihr neu erstelltes Resource Manager-Speicherkonto kopieren.

**F: Ich habe ein vorhandenes Resource Manager-Speicherkonto. Kann ich dafür SSE aktivieren?**

A: Während der SSE-Vorschau müssen Sie ein neues Konto erstellen, um auf die neue SSE-Funktion zuzugreifen.

**F: Kann ich die aktuellen Daten in einem vorhandenen Resource Manager-Speicherkonto verschlüsseln?**

A: Wenn Ihr vorhandenes Resource Manager-Speicherkonto vor dieser Vorschauankündigung erstellt wurde, können Sie ein neues Resource Manager-Speicherkonto erstellen und die Verschlüsselung aktivieren. Sie können dann die Daten aus dem vorherigen Speicherkonto kopieren, und sie werden automatisch verschlüsselt. Wenn Ihr vorhandenes Resource Manager-Speicherkonto jedoch nach dieser Vorschauankündigung erstellt wurde und Sie sich entschieden haben, die Verschlüsselung zu einem späteren Zeitpunkt zu aktivieren, können Sie die Verschlüsselung für dieses Speicherkonto mithilfe des Azure-Portals aktivieren und unverschlüsselte Daten wieder in das Speicherkonto schreiben.

**F: Ich verwende Storage Premium; kann ich SSE verwenden?**

A: Ja, SSE wird von Standard Storage und Premium Storage unterstützt.

**F: Wenn ich ein neues Speicherkonto erstelle, SSE aktiviere und dann eine neue VM erstelle, die dieses Speicherkonto verwendet, bedeutet dies, dass mein virtueller Computer verschlüsselt ist?**

A: Ja. Alle erstellten Datenträger, die das neue Speicherkonto verwenden, werden verschlüsselt, sofern sie nach der SSE-Aktivierung erstellt wurden. Wurde die VM mit Azure Market Place erstellt, bleibt das VHD-Basisimage unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

**F: Kann ich neue Speicherkonten mithilfe von Azure PowerShell und Azure-CLI erstellen, wenn SSE aktiviert ist?**

A: Wir werden diese Funktion in der bevorstehenden Veröffentlichung von Azure PowerShell und Azure-CLI bereitstellen, voraussichtlich Ende April.

**F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?**

A: Es fallen keine zusätzlichen Kosten an.

**F: Wie melde ich mich für die Vorschau an?**

A: Sie können sich für den Zugriff auf die Vorschau mit PowerShell registrieren. Nachdem Ihr Abonnement für das Feature genehmigt wurde, können Sie mit PowerShell die Verschlüsselung für ruhende Daten aktivieren.

**F: Welchen Funktionsnamen benötige ich zur Registrierung, wenn ich mich mit PowerShell für die Vorschau anmelde?**

A: EncryptionAtRest.

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

**F: Ich kann die Verschlüsselung nicht auf meinem Speicherkonto aktivieren.**

A: Wann haben Sie das Speicherkonto erstellt? Bei der Vorschau müssen Sie Ihr Abonnement registrieren und auch ein neues Speicherkonto erstellen, um SSE verwenden zu können. Sie können SSE nicht auf Speicherkonten aktivieren, die vor der Vorschau erstellt wurden.

**F: Ist die SSE-Vorschau nur in bestimmten Regionen möglich?**

A: Die SSE-Vorschau ist derzeit in den Regionen Asien, Osten und Europa, Westen für Standard Storage und in der Region Japan, Osten für Storage Premium verfügbar. Wenn wir dieses Feature in den folgenden Monaten in zusätzlichen Regionen einführen, wird dieses Dokument aktualisiert.

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Bitte wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

##Nächste Schritte

Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

<!---HONumber=AcomDC_0713_2016-->