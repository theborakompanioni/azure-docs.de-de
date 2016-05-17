<properties
	pageTitle="Erstellen mehrerer virtueller Computer | Microsoft Azure"
	description="Optionen für das Erstellen mehrerer virtueller Computer unter Windows"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="gbowerman"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/02/2016"
	ms.author="guybo"/>

# Erstellen mehrerer virtueller Azure-Computer

Es gibt viele Szenarien, in denen Sie eine große Anzahl ähnlicher virtueller Computer erstellen müssen, z.B. High Performance Computing (HPC), umfangreiche Datenanalysen, skalierbare und häufig zustandslose Server auf mittlerer Ebene oder Back-End-Server (wie Webserver) und verteilte Datenbanken.

In diesem Artikel werden die verfügbaren Optionen zum Erstellen mehrerer virtueller Computer in Azure beschrieben, die über die einfachen Fälle der manuellen Erstellung einer Reihe von virtuellen Computern hinausgehen, die nicht gut skaliert werden können, wenn Sie eine größere Anzahl von virtuellen Computern erstellen müssen.

Eine Möglichkeit zur Optimierung der Erstellung vieler ähnlicher virtueller Computer ist die Verwendung des Azure Resource Manager-Konstrukts _Ressourcenschleifen_.

## Ressourcenschleifen

Ressourcenschleifen sind eine syntaktische Kurzform in Azure Resource Manager-Vorlagen, mit denen Sie eine Reihe von ähnlich konfigurierten Ressourcen in einer Schleife erstellen können. Mit Ressourcenschleifen können Sie beispielsweise mehrere Speicherkonten, Netzwerkschnittstellen und virtuelle Computer erstellen. Weitere Informationen zu Ressourcenschleifen finden Sie unter [Create VMs in Availability Sets using Resource Loops](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/) (Erstellen von virtuellen Computern in Verfügbarkeitsgruppen mithilfe von Ressourcenschleifen).

## Herausforderungen bei der Skalierung

Obwohl Ressourcenschleifen die Erstellung einer umfangreichen Cloudinfrastruktur einfacher gestalten und präzisere Vorlagen ermöglichen, bleiben bestimmte Probleme bestehen. Beim Erstellen von 100 virtuellen Computern mit einer Ressourcenschleife müssten Sie z.B. beim Definieren der virtuellen Computer die Netzwerkschnittstellenkarten (NICs) mit den virtuellen Computern und Speicherkonten korrelieren. Die Anzahl von virtuellen Computern unterscheidet sich wahrscheinlich von der Anzahl von Speicherkonten und somit unterscheiden sich die Größen der Ressourcenschleifen. Dies sind lösbare Probleme, aber die Komplexität nimmt mit zunehmender Größe erheblich zu.

Eine weitere Herausforderung tritt auf, wenn Sie eine Infrastruktur mit flexibler Skalierung benötigen, z.B. eine automatische Skalierung, bei der die Anzahl der virtuellen Computer als Reaktion auf die Workload automatisch erhöht oder verringert wird. Virtuelle Computer bieten keinen integrierten Mechanismus zum Variieren der Anzahl (horizontales Hoch- und Herunterskalieren). Wenn Sie durch das Entfernen von virtuellen Computern horizontal herunterskalieren, ist die Sicherstellung einer hohen Verfügbarkeit durch die gleichmäßige Verteilung der virtuellen Computer über Update- und Fehlerdomänen eine weitere schwierige Aufgabe.

Ressourcenschleifen sind zwar eine syntaktische Kurzform, bei deren Verwendung gehen jedoch mehrere Aufrufe zum Erstellen von Ressourcen an das zugrunde liegende Fabric. Wenn mit mehreren Aufrufen ähnliche Ressourcen erstellt werden, besteht für Azure die implizite Gelegenheit, diesen Entwurf zu verbessern und Optimierungen durchzuführen, um so Zuverlässigkeit und Leistung bei der Bereitstellung zu verbessern. Hier werden _VM-Skalierungsgruppen_ relevant.

## VM-Skalierungsgruppen

VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Gruppe von identischen virtuellen Computern bereitstellen und verwalten können. Wenn alle virtuellen Computer identisch konfiguriert sind, können VM-Skalierungsgruppen leicht horizontal herunter- und hochskaliert werden, indem Sie einfach die Anzahl der virtuellen Computer in der Gruppe ändern. Zudem können sie für die automatische Skalierung basierend auf den Anforderungen der Workload konfiguriert werden.

Bei Anwendungen, für die Computeressourcen horizontal herunter- und hochskaliert werden müssen, werden Skalierungsoperationen implizit über Fehler- und Updatedomänen ausgeglichen.

Statt der Korrelation mehrerer Ressourcen wie z.B. Netzwerkschnittstellenkarten und virtueller Computer verfügt eine Skalierungsgruppe über Eigenschaften für Netzwerk, Speicher, virtuelle Computer und Erweiterungen, die zentral konfiguriert werden können.

Eine Einführung in VM-Skalierungsgruppen finden Sie auf der [Produktseite](https://azure.microsoft.com/services/virtual-machine-scale-sets/), und weitere ausführliche Informationen finden Sie in der [Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).

<!---HONumber=AcomDC_0504_2016-->