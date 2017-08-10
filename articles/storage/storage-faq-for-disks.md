---
title: "Häufig gestellte Fragen (FAQ) zu Azure-IaaS-VM-Datenträgern | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure-IaaS-VM-Datenträgern und Premium-Datenträgern (verwaltet und nicht verwaltet)"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 9e5eed35334f1b95441b8181c8e90645be78b389
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Häufig gestellte Fragen zu Azure-IaaS-VM-Datenträgern sowie zu verwalteten und nicht verwalteten Premium-Datenträgern

In diesem Artikel gehen wir auf einige häufig gestellte Fragen zu Azure Managed Disks und Storage Premium ein.

## <a name="managed-disks"></a>Verwaltete Datenträger

**Was ist Azure Managed Disks?**

Das Managed Disks-Feature nimmt Ihnen die Speicherkontoverwaltung ab und vereinfacht so die Datenträgerverwaltung für virtuelle Azure-IaaS-Computer. Weitere Informationen finden Sie in der [Übersicht über Azure Managed Disks](storage-managed-disks-overview.md).

**Welche Kosten fallen für mich an, wenn ich auf der Grundlage einer vorhandenen virtuellen Festplatte mit einer Größe von 80 GB einen verwalteten Standarddatenträger erstelle?**

Ein verwalteter Standarddatenträger, der auf Grundlage einer virtuellen Festplatte mit einer Größe von 80 GB erstellt wird, wird nach der nächsten verfügbaren Größe für Standarddatenträger abgerechnet (in diesem Fall also als S10-Datenträger). Ihnen wird der Preis für einen S10-Datenträger in Rechnung gestellt. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Fallen bei verwalteten Standarddatenträgern Transaktionskosten an?**

Ja. Jede Transaktion wird Ihnen in Rechnung gestellt. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Wird bei einem verwalteten Standarddatenträger die tatsächliche Größe der Daten auf dem Datenträger oder die bereitgestellte Kapazität des Datenträgers abgerechnet?**

Die Abrechnung erfolgt auf der Grundlage der bereitgestellten Kapazität des Datenträgers. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Inwiefern unterscheiden sich die Preise für verwaltete Premium-Datenträger von den Preisen für nicht verwaltete Datenträger?**

Die Preise für verwaltete Premium-Datenträger unterscheiden sich nicht von den Preisen für nicht verwaltete Premium-Datenträger.

**Kann ich den Speicherkontotyp (Standard oder Premium) meiner verwalteten Datenträger ändern?**

Ja. Der Speicherkontotyp Ihrer verwalteten Datenträger kann über das Azure-Portal mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle geändert werden.

**Kann ich einen verwalteten Datenträger in ein privates Speicherkonto kopieren oder exportieren?**

Ja. Sie können verwaltete Datenträger können über das Azure-Portal mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle exportieren.

**Kann ich mithilfe einer VHD-Datei in einem Azure-Speicherkonto einen verwalteten Datenträger mit einem anderen Abonnement erstellen?**

Nein.

**Kann ich mithilfe einer VHD-Datei in einem Azure-Speicherkonto einen verwalteten Datenträger in einer anderen Region erstellen?**

Nein.

**Gelten für Kunden, die verwaltete Datenträger verwenden, Einschränkungen bei der Skalierung?**

Managed Disks beseitigt die Grenzwerte im Zusammenhang mit Speicherkonten. Die Anzahl verwalteter Datenträger pro Abonnement ist allerdings standardmäßig auf 2000 beschränkt. Wenden Sie sich an den Support, um diese Zahl zu erhöhen.

**Kann ich eine inkrementelle Momentaufnahme eines verwalteten Datenträgers erstellen?**

Nein. Die aktuelle Momentaufnahmefunktion erstellt eine vollständige Kopie eines verwalteten Datenträgers. Die Unterstützung inkrementeller Momentaufnahmen ist jedoch bereits geplant.

**Können virtuelle Computer in einer Verfügbarkeitsgruppe eine Kombination aus verwalteten und nicht verwalteten Datenträgern besitzen?**

Nein. Die virtuellen Computer in einer Verfügbarkeitsgruppe müssen entweder alle über verwaltete oder alle über nicht verwaltete Datenträger verfügen. Den gewünschten Datenträgertyp können Sie bei der Erstellung einer Verfügbarkeitsgruppe auswählen.

**Ist Managed Disks die Standardoption im Azure-Portal?**

Noch nicht, dies ist aber für die Zukunft geplant.

**Kann ich einen leeren verwalteten Datenträger erstellen?**

Ja. Sie können einen leeren Datenträger erstellen. Ein verwalteter Datenträger kann unabhängig von einem virtuellen Computer erstellt werden, also ohne ihn z.B. an einen virtuellen Computer anzufügen.

**Wie viele Fehlerdomänen werden bei Verwendung von verwalteten Datenträgern standardmäßig für Verfügbarkeitsgruppen unterstützt?**

Bei Verwendung von verwalteten Datenträgern werden für Verfügbarkeitsgruppen abhängig von der Region entweder zwei oder drei Fehlerdomänen unterstützt.

**Wie wird das Standardspeicherkonto für die Diagnose eingerichtet?**

Sie richten ein privates Speicherkonto für die VM-Diagnose ein. Es ist jedoch geplant, die Diagnose ebenfalls auf Managed Disks umzustellen.

**Welcher Support für rollenbasierte Zugriffssteuerung steht für verwaltete Datenträger zur Verfügung?**

Managed Disks unterstützt drei zentrale Standardrollen:

* Besitzer: Kann alles verwalten, einschließlich des Zugriffs
* Mitwirkende: Kann alles mit Ausnahme des Zugriffs verwalten
* Leser: Kann alles anzeigen, aber keine Änderungen vornehmen

**Kann ich einen verwalteten Datenträger in ein privates Speicherkonto kopieren oder exportieren?**

Sie können mithilfe eines schreibgeschützten SAS-URIs für den verwalteten Datenträger den Inhalt in ein privates Speicherkonto oder in einen lokalen Speicher kopieren.

**Kann ich eine Kopie meines verwalteten Datenträgers erstellen?**

Kunden können eine Momentaufnahme ihrer verwalteten Datenträger erstellen und anschließend auf der Grundlage der Momentaufnahme einen weiteren verwalteten Datenträger erstellen.

**Werden nicht verwaltete Datenträger weiterhin unterstützt?**

Ja. Es werden verwaltete und nicht verwaltete Datenträger unterstützt. Allerdings wird empfohlen, für neue Workloads verwaltete Datenträger zu verwenden und aktuelle Workloads zu verwalteten Datenträgern zu migrieren.


**Wenn ich einen Datenträger mit einer Größe von 128 GB erstelle und die Größe anschließend auf 130 GB erhöhe, wird mir dann die nächsthöhere Datenträgergröße (512 GB) in Rechnung gestellt?**

Ja.

**Kann ich verwaltete Datenträger für lokal redundanten Speicher, georedundanten Speicher und zonenredundanten Speicher erstellen?**

Azure Managed Disks unterstützt momentan nur lokal redundanten Speicher (LRS).

**Kann ich meine verwalteten Datenträger verkleinern?**

Nein. Dies wird derzeit nicht unterstützt. 

**Kann ich die Eigenschaft „Computername“ ändern, wenn ein spezialisierter Betriebssystemdatenträger (der nicht mit dem Systemvorbereitungstool erstellt wurde und nicht generalisiert ist) zur Bereitstellung einer VM verwendet wird?**

Nein. Sie können die Eigenschaft „Computername“ nicht aktualisieren. Die neue VM erbt diese von der übergeordneten VM, mit der der Betriebssystem-Datenträger erstellt wurde. 

**Wo finde ich Beispielvorlagen von Azure Resource Manager, um virtuelle Computer mit verwalteten Datenträgern zu erstellen?**
* [Liste der Vorlagen die Managed Disks verwenden](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

## <a name="managed-disks-and-storage-service-encryption"></a>Managed Disks und Storage Service Encryption 

**Ist Azure Storage Service Encryption standardmäßig beim Erstellen eines verwalteten Datenträgers aktiviert?**

Ja.

**Wer verwaltet die Verschlüsselungsschlüssel?**

Microsoft verwaltet die Verschlüsselungsschlüssel.

**Kann ich für meine verwalteten Datenträger Storage Service Encryption deaktivieren?**

Nein.

**Ist Storage Service Encryption nur in bestimmten Regionen verfügbar?**

Nein. Es ist in allen Regionen verfügbar, in denen Managed Disks verfügbar ist. Managed Disks ist in allen öffentlichen Regionen und Deutschland verfügbar.

**Wie finde ich heraus, ob mein verwalteter Datenträger verschlüsselt ist?**

Sie können im Azure-Portal, der Azure-CLI und in PowerShell herausfinden, wann ein verwaltetet Datenträger erstellt wurde. Wenn der Zeitpunkt nach dem 9. Juni 2017 liegt, ist Ihr Datenträger verschlüsselt. 

**Wie kann ich meine vorhandenen Laufwerke verschlüsseln, die vor dem 10. Juni 2017 erstellt wurden?**

Ab dem 10. Juni 2017 werden neue Daten, die in vorhandene Datenträger geschrieben werden, automatisch verschlüsselt. Darüber hinaus ist geplant, dass vorhandene Daten verschlüsselt werden, wobei die Verschlüsselung asynchron im Hintergrund abläuft. Wenn Sie jetzt schon vorhandene Daten verschlüsseln müssen, erstellen Sie eine Kopie Ihres Datenträgers. Neue Datenträger werden verschlüsselt.

* [Kopieren von verwalteten Datenträger mithilfe der Azure-CLI](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [Kopieren von verwalteten Datenträger mithilfe von PowerShell](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

**Werden verwaltete Momentaufnahmen und Bilder verschlüsselt?**

Ja. Alle verwalteten Momentaufnahmen und Bilder, die nach dem 9. Juni 2017 erstellt werden, werden automatisch verschlüsselt. 

**Kann ich VMs mit nicht verwalteten Datenträgern, die sich auf Speicherkonten befinden, die verschlüsselt sind oder dies waren, in verwaltete Datenträger konvertieren?**

Ja

**Wird eine exportierte VHD-Datei von einem verwalteten Datenträger oder eine Momentaufnahme auch verschlüsselt?**

Nein. Wenn Sie allerdings eine VHD-Datei von einem verschlüsselten, verwalteten Datenträger oder einer Momentaufnahme in ein verschlüsseltes Speicherkonto exportieren, ist sie verschlüsselt. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium-Datenträger (verwaltet und nicht verwaltet)

**Kann ich sowohl Premium- als auch Standarddatenträger anfügen, wenn ein virtueller Computer eine Größenserie mit Storage Premium-Unterstützung (beispielsweise DSv2) verwendet?** 

Ja.

**Kann ich an eine Größenserie ohne Storage Premium-Unterstützung (also beispielsweise an die D-, Dv2-, G- oder F-Serie) sowohl Premium- als auch Standarddatenträger anfügen?**

Nein. An virtuelle Computer, die keine Größenserie mit Storage Premium-Unterstützung verwenden, können nur Standarddatenträger angefügt werden.

**Welche Kosten fallen an, wenn ich einen Storage Premium-Datenträger von einer vorhandenen virtuellen Festplatte mit einer Größe von 80 GB erstelle?**

Ein Premiumdatenträger, der auf der Grundlage einer virtuellen Festplatte mit einer Größe von 80 GB erstellt wird, wird nach der nächsten verfügbaren Größe für Premiumdatenträger abgerechnet (in diesem Fall also als P10-Datenträger). Ihnen wird der Preis für einen P10-Datenträger in Rechnung gestellt.

**Fallen bei Verwendung von Storage Premium Transaktionskosten an?**

Es fallen feste Kosten für die Datenträgergröße an, und es gelten bestimmte Grenzwerte für IOPS und Durchsatz. Weitere Kosten fallen gegebenenfalls für die ausgehende Bandbreite und die Momentaufnahmekapazität an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Welche IOPS- und Durchsatzgrenzwerte gelten für den Datenträgercache?**

Die kombinierte Beschränkung für den Cache und die lokale SSD für virtuelle Computer der DS-Serie liegt bei 4000 IOPS pro Kern und 33 MB pro Sekunde und Kern. Die GS-Serie bietet 5000 IOPS pro Kern und 50 MB pro Sekunde und Kern.

**Wird für virtuelle Computer mit Managed Disks die lokale SSD unterstützt?**

Bei der lokalen SSD handelt es sich um einen temporären Speicher, der in einem virtuellen Computer mit Managed Disks enthalten ist. Für diesen temporären Speicher fallen keine zusätzlichen Kosten an. Es empfiehlt sich, auf dieser lokalen SSD keine Anwendungsdaten zu speichern, da die Daten nicht dauerhaft in Azure Blob Storage gespeichert werden.

**Gibt es Konsequenzen für die Verwendung von TRIM auf Premium-Datenträgern?**

Es gibt keinen Nachteil bei der Verwendung von TRIM auf Azure-Datenträger auf Premium- oder Standard-Datenträgern.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Neue Datenträgergrößen: verwaltet und nicht verwaltet

**Was ist die größte Datenträgergröße, die für Betriebssystem- und Datenträger unterstützt wird?**

Der Partitionstyp, den Azure für einen Betriebssystemdatenträger unterstützt, ist der Master Boot Record (MBR). Das MBR-Format unterstützt eine Datenträgergröße bis zu 2 TB. Die maximale Größe, die Azure für einen Betriebssystemdatenträger unterstützt beträgt 2 TB. Azure unterstützt für Datenträger bis zu 4 TB. 

**Was ist die größte Seitenblobgröße, die unterstützt wird?**

Die größte Seitenblobgröße, die von Azure unterstützt wird, ist 8 TB (8.191 GB). Seitenblobs größer als 4 TB (4.095 GB), die an einen virtuellen Computer als Daten oder als Betriebssystemdatenträger angefügt wurden, werden nicht unterstützt.

**Benötige ich eine neue Version der Azure-Tools, um Datenträger, die größer als 1TB sind, erstellen, anfügen, anpassen und hochladen zu können?**

Sie müssen Ihrer vorhandenen Azure-Tools nicht aktualisieren, um Datenträger, die größer als 1 TB sind, erstellen, anfügen oder anpassen zu können. Um eine VHD-Datei von einem lokalen Speicherort als Seitenblob oder als nicht verwalteten Datenträger direkt in Azure hochladen zu können, müssen Sie die neuesten Tools verwenden:

|Azure-Tools      | Unterstützte Versionen                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Versionsnummer 4.1.0: Version Juni 2017 oder höher|
|Azure-CLI V1     | Versionsnummer 0.10.13: Version Mai 2017 oder höher|
|AzCopy           | Versionsnummer 6.1.0: Version Juni 2017 oder höher|

Die Unterstützung für Azure-CLI V2 und Azure-Speicher-Explorer ist bald verfügbar. 

**Werden P4- und P6-Datenträgergrößen für nicht verwaltete Datenträger oder Seitenblobs unterstützt?**

Nein. P4-Datenträger (32 GB) und P6-Datenträgergrößen (64 GB) werden nur für verwaltete Datenträger unterstützt. Die Unterstützung für nicht verwaltete Datenträger und Seitenblobs ist bald verfügbar.

**Wenn mein vorhandener verwalteter Premium-Datenträger mit weniger als 64 GB erstellt wurde, bevor der kleine Datenträger aktiviert wurde (circa 15. Juni 2017), wie wird dies berechnet?**

Vorhandene kleinen Premium-Datenträger mit weniger als 64 GB werden weiterhin gemäß dem P10-Tarif in Rechnung gestellt. 

**Wie kann ich den Datenträgertarif von kleinen Premium-Datenträgern mit weniger als 64 GB von P10 in P4 oder P6 ändern?**

Machen Sie eine Momentaufnahme Ihres kleinen Datenträgers, und erstellen Sie dann einen Datenträger, damit der Tarif automatisch in P4 oder P6 basierend auf der bereitgestellten Größe geändert wird. 


## <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?

Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, und wir helfen Ihnen dabei, eine Antwort zu finden. Sie können in den Kommentaren am Ende dieses Artikels Fragen stellen. Im [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata) von MSDN können Sie sich mit dem Azure Storage-Team und anderen Mitgliedern der Community über diesen Artikel austauschen.

Funktionsvorschläge und -ideen können über das [Azure Storage-Feedbackforum](https://feedback.azure.com/forums/217298-storage) eingereicht werden.

