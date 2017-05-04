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
ms.date: 03/19/2017
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 284b239860481cf76f647d78f6a7b5e2b7cf9a3b
ms.lasthandoff: 04/26/2017


---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Häufig gestellte Fragen zu Azure-IaaS-VM-Datenträgern sowie zu verwalteten und nicht verwalteten Premium-Datenträgern

In diesem Artikel gehen wir auf einige häufig gestellte Fragen zu Managed Disks und Storage Premium ein.

## <a name="managed-disks"></a>Managed Disks

**Was ist Azure Managed Disks?**

Das Managed Disks-Feature nimmt Ihnen die Speicherkontoverwaltung ab und vereinfacht so die Datenträgerverwaltung für virtuelle Azure-IaaS-Computer. Weitere Informationen finden Sie in der [Übersicht über Azure Managed Disks](storage-managed-disks-overview.md).

**Welche Kosten fallen für mich an, wenn ich auf der Grundlage einer vorhandenen virtuellen Festplatte mit einer Größe von 80 GB einen verwalteten Standarddatenträger erstelle?**

Ein standardmäßiger verwalteter Datenträger, der auf Grundlage einer virtuellen Festplatte mit einer Größe von 80 GB erstellt wird, wird nach der nächsten verfügbaren Größe für Standard-Datenträger abgerechnet (in diesem Fall also als S10-Datenträger). Somit wird Ihnen der Preis für einen S10-Datenträger in Rechnung gestellt. Ausführliche Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Fallen bei verwalteten Standarddatenträgern Transaktionskosten an?**

Ja, jede Transaktion wird Ihnen in Rechnung gestellt. Ausführliche Informationen finden Sie in der [Preisübersicht] (https://azure.microsoft.com/pricing/details/storage).

**Wird bei einem verwalteten Standarddatenträger die tatsächliche Größe der Daten auf dem Datenträger oder die bereitgestellte Kapazität des Datenträgers abgerechnet?**

Die Abrechnung erfolgt auf der Grundlage der bereitgestellten Kapazität des Datenträgers. Ausführliche Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Inwiefern unterscheiden sich die Preise für verwaltete Premium-Datenträger von den Preisen für nicht verwaltete Datenträger?**

Die Preise für verwaltete Premium-Datenträger unterscheiden sich nicht von den Preisen für nicht verwaltete Premium-Datenträger.

**Kann ich den Speicherkontotyp (Standard/Premium) meiner verwalteten Datenträger ändern?**

Ja. Der Speicherkontotyp Ihrer verwalteten Datenträger kann über das Azure-Portal, mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle geändert werden.

**Kann ich einen verwalteten Datenträger in ein privates Speicherkonto kopieren oder exportieren?**

Ja. Verwaltete Datenträger können über das Azure-Portal, mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle exportiert werden.

**Kann ich mithilfe einer VHD-Datei in einem Azure-Speicherkonto einen verwalteten Datenträger in einem anderen Abonnement erstellen?**

Nein.

**Kann ich mithilfe einer VHD-Datei in einem Azure-Speicherkonto einen verwalteten Datenträger in einer anderen Region erstellen?**

Nein.

**Gelten für Kunden, die verwaltete Datenträger verwenden, Einschränkungen bei der Skalierung?**

Managed Disks beseitigt die Grenzwerte im Zusammenhang mit Speicherkonten. Die Anzahl verwalteter Datenträger pro Abonnement ist allerdings standardmäßig auf 2000 beschränkt. Dieser Grenzwert kann auf Wunsch erhöht werden. Wenden Sie sich hierzu an den Support.

**Kann ich eine inkrementelle Momentaufnahme eines verwalteten Datenträgers erstellen?**

Nein. Die aktuelle Momentaufnahmefunktion erstellt eine vollständige Kopie eines verwalteten Datenträgers. Die Unterstützung inkrementeller Momentaufnahmen ist jedoch bereits geplant.

**Können virtuelle Computer in einer Verfügbarkeitsgruppe eine Mischung aus verwalteten und nicht verwalteten Datenträgern besitzen?**

Nein. Die virtuellen Computer in einer Verfügbarkeitsgruppe müssen entweder alle über verwaltete oder alle über nicht verwaltete Datenträger verfügen. Den gewünschten Datenträgertyp können Sie bei der Erstellung einer Verfügbarkeitsgruppe auswählen.

**Ist Managed Disks die Standardoption im Azure-Portal?**

Noch nicht, dies ist aber für die Zukunft geplant.

**Kann ich einen leeren verwalteten Datenträger erstellen?**

Ja, Sie können einen leeren Datenträger erstellen. Ein verwalteter Datenträger kann unabhängig von einem virtuellen Computer erstellt werden (also ohne ihn an einen virtuellen Computer anzufügen).

**Wie viele Fehlerdomänen werden bei Verwendung von Managed Disks standardmäßig für Verfügbarkeitsgruppen unterstützt?**

Bei Verwendung von Managed Disks werden für Verfügbarkeitsgruppen abhängig von der Region entweder zwei oder drei Fehlerdomänen unterstützt.

**Wie wird das Standardspeicherkonto für die Diagnose eingerichtet?**

Sie richten ein privates Speicherkonto für die VM-Diagnose ein. Es ist jedoch geplant, die Diagnose ebenfalls auf Managed Disks umzustellen.

**Welche Art von RBAC-Unterstützung steht für Managed Disks zur Verfügung?**

Managed Disks unterstützt drei zentrale Standardrollen:

1.  Besitzer: Kann alles verwalten (einschließlich des Zugriffs).

2.  Mitwirkender: Kann alles verwalten (mit Ausnahme des Zugriffs).

3.  Leser: Kann alles anzeigen, aber keine Änderungen vornehmen.

**Kann ich einen verwalteten Datenträger in ein privates Speicherkonto kopieren oder exportieren?**

Sie können mithilfe eines schreibgeschützten SAS-URIs (Shared Access Signature) für den verwalteten Datenträger den Inhalt in ein privates Speicherkonto oder in einen lokalen Speicher kopieren.

**Kann ich eine Kopie meines verwalteten Datenträgers erstellen?**

Kunden können eine Momentaufnahme ihrer verwalteten Datenträger erstellen und anschließend auf der Grundlage der Momentaufnahme einen weiteren verwalteten Datenträger erstellen.

**Werden nicht verwaltete Datenträger weiterhin unterstützt?**

Ja. Wir unterstützen sowohl verwaltete als auch nicht verwaltete Datenträger. Allerdings empfehlen wir, für neue Workloads verwaltete Datenträger zu verwenden und aktuelle Workloads zu Managed Disks zu migrieren.

**Wenn ich einen Datenträger mit einer Größe von 128 GB erstelle und die Größe anschließend auf 130 GB erhöhe, wird mir dann die nächsthöhere Datenträgergröße (512 GB) in Rechnung gestellt?**

Ja.

**Kann ich verwaltete Datenträger vom Typ „LRS“, „GRS“ und „ZRS“ erstellen?**

Azure Managed Disks unterstützt momentan nur lokal redundanten Speicher (LRS).

**Kann ich meine verwalteten Datenträger verkleinern?**
Nein. Dies wird derzeit nicht unterstützt. 

**Kann ich die Eigenschaft „Name“ des Computers ändern, wenn ich einen speziellen (nicht mit Sysprep vorbereiteten oder generalisierten) Betriebssystem-Datenträger zum Bereitstellen einer VM verwende?** Nein. Sie können die Eigenschaft „Name“ des Computers nicht ändern. Die neue VM übernimmt diese von der übergeordneten VM, mit der der Betriebssystem-Datenträger erstellt wurde. 

## <a name="managed-disks-and-port-8443"></a>Managed Disks und Port 8443

**Warum müssen Kunden die Sperre für ausgehenden Datenverkehr an Port 8443 für virtuelle Computer aufheben, die Azure Managed Disks verwenden?**

Der Azure-VM-Agent verwendet Port 8443, um den Status jeder VM-Erweiterung an die Azure-Plattform zu melden. Wenn die Sperre dieses Ports nicht aufgehoben ist, kann der VM-Agent diese Status nicht melden. Weitere Informationen zum VM-Agent finden Sie unter [Übersicht über den Agent für virtuelle Azure-Computer](../virtual-machines/windows/agent-user-guide.md).

**Was passiert, wenn ein virtueller Computer mit Erweiterungen bereitgestellt wird und die Sperre für den Port nicht aufgehoben wurde?**

Die Bereitstellung führt zu einem Fehler. 

**Was passiert, wenn ein virtueller Computer ohne Erweiterungen bereitgestellt und die Sperre für den Port nicht aufgehoben wurde?**

Dies wirkt sich nicht auf die Bereitstellung aus. 

**Was passiert, wenn eine Erweiterung auf einem virtuellen Computer installiert wird, der bereits bereitgestellt ist und ausgeführt wird, und die Sperre für Port 8443 auf diesem virtuellen Computer nicht aufgehoben wurde?**

Die Erweiterung wird nicht erfolgreich bereitgestellt. Der Status der Erweiterung ist unbekannt. 

**Was passiert, wenn eine ARM-Vorlage zum Bereitstellen mehrerer virtueller Computer verwendet wird, auf denen Port 8443 gesperrt ist – einer der virtuellen Computer verfügt über Erweiterungen und ein zweiter ist vom ersten abhängig?**

Der erste virtuelle Computer wird als fehlerhafte Bereitstellung angezeigt, da die Erweiterungen nicht erfolgreich bereitgestellt wurden. Der zweite virtuelle Computer wird nicht bereitgestellt. 

**Gilt die Anforderung, dass die Sperre des Ports aufgehoben sein muss, für alle VM-Erweiterungen?**

Ja.

**Muss die Sperre für eingehende und ausgehende Verbindungen an Port 8443 aufgehoben werden?**

Nein. Es muss nur die Sperre für ausgehende Verbindungen an Port 8443 aufgehoben werden. 

**Ist die Aufhebung der Sperre für ausgehende Verbindungen an Port 8443 für die gesamte Lebensdauer des virtuellen Computers erforderlich?**

Ja.

**Wirkt sich die Aufhebung der Sperre für diesen Port auf die Leistung des virtuellen Computers aus?**

Nein.

**Gibt es ein Datum, an dem dieses Problem voraussichtlich behoben sein wird, damit ich die Sperre für Port 8443 nicht mehr aufheben muss?**

Ja, dies ist bis Ende Mai 2017 geplant.

## <a name="premium-disks--both-managed-and-unmanaged"></a>Premium-Datenträger (verwaltet und nicht verwaltet)

**Kann ich sowohl Premium- als auch Standarddatenträger anfügen, wenn ein virtueller Computer eine Größenserie mit Storage Premium-Unterstützung (beispielsweise DSv2) verwendet?** 

Ja.

**Kann ich an eine Größenserie ohne Storage Premium-Unterstützung (also beispielsweise an die D-, Dv2-, G- oder F-Serie) sowohl Premium- als auch Standarddatenträger anfügen?**

Nein. An virtuelle Computer, die keine Größenserie mit Storage Premium-Unterstützung verwenden, können nur Standarddatenträger angefügt werden.

**Welche Kosten fallen für mich an, wenn ich einen Storage Premium-Datenträger von einer vorhandenen virtuellen Festplatte mit einer Größe von 80 GB erstelle?**

Ein Premiumdatenträger, der auf der Grundlage einer virtuellen Festplatte mit einer Größe von 80 GB erstellt wird, wird nach der nächsten verfügbaren Größe für Premiumdatenträger abgerechnet (in diesem Fall also als P10-Datenträger). Für Sie fallen die Preise für einen P10-Datenträger an.

**Fallen bei Verwendung von Storage Premium Transaktionskosten an?**

Es fallen feste Kosten für die Datenträgergröße an, und es gelten bestimmte Grenzwerte für IOPS und Durchsatz. Weitere Kosten fallen gegebenenfalls für die ausgehende Bandbreite und die Momentaufnahmekapazität an. Ausführliche Informationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage).

**Welche IOPS- und Durchsatzgrenzwerte gelten für den Datenträgercache?**

Die kombinierte Beschränkung für den Cache und die lokale SSD für virtuelle Computer der DS-Serie liegt bei 4.000 IOPS pro Kern und 33 MB pro Sekunde und Kern. Die GS-Serie bietet 5.000 IOPS pro Kern und 50 MB pro Sekunde und Kern.

**Wird für virtuelle Computer mit verwalteten Datenträgern die lokale SSD unterstützt?**

Bei der lokalen SSD handelt es sich um einen temporären Speicher, der in einem virtuellen Computer mit verwalteten Datenträgern enthalten ist. Für diesen temporären Speicher fallen keine zusätzlichen Kosten an. Es empfiehlt sich, auf dieser lokalen SSD keine Anwendungsdaten zu speichern, da die Daten nicht dauerhaft in Azure Blob Storage gespeichert werden.

**Welche Auswirkungen hat das Anwenden des Befehls TRIM auf Premium-Datenträger?**

Es gibt keinen Nachteil bei Anwendung von TRIM auf Azure-Datenträger auf Premium- oder Standard-Datenträgern.

## <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?

Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, und wir helfen Ihnen dabei, eine Antwort zu finden. Sie können in den Kommentaren am Ende dieses Artikels oder im [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata) auf MSDN eine Frage stellen und sich mit dem Azure Storage-Team und anderen Communitymitgliedern über den Artikel austauschen.

Featurevorschläge und Ideen können über das [Azure Storage-Feedbackforum](https://feedback.azure.com/forums/217298-storage) eingereicht werden.

