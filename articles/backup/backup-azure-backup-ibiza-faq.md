<properties
   pageTitle="Öffentliche Vorschauversion von Azure Backup – Häufig gestellte Fragen | Microsoft Azure"
   description="Diese Version der häufig gestellten Fragen dient als Unterstützung für die öffentliche Vorschauversion des Azure Backup-Diensts. Hier finden Sie Antworten auf häufig gestellte Fragen zum Backup-Agent, zu Sicherung und Aufbewahrung, Wiederherstellung, Sicherheit und auf andere allgemeine Fragen zur Azure Backup-Lösung."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="Backup-Lösung; Backup-Dienst"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
	 ms.tgt_pltfrm="na"
	 ms.devlang="na"
	 ms.topic="get-started-article"
	 ms.date="03/30/2016"
	 ms.author="trinadhk; markgal; jimpark;"/>

# Öffentliche Vorschauversion des Azure Backup-Diensts – Häufig gestellte Fragen

> [AZURE.SELECTOR]
- [Häufig gestellte Fragen zu Backup für den klassischen Modus](backup-azure-backup-faq.md)
- [Häufig gestellte Fragen zu Backup für den ARM-Modus](backup-azure-backup-ibiza-faq.md)

Dieser Artikel enthält spezielle Informationen zur öffentlichen Vorschauversion des Azure Backup-Diensts. Der Artikel wird aktualisiert, wenn neue häufig gestellte Fragen eintreffen, und er dient als Zusatz zum Artikel [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq). Der Artikel „Azure Backup – Häufig gestellte Fragen“ enthält alle Fragen und Antworten zum Azure Backup-Dienst.

Sie können im Disqus-Abschnitt dieses Artikels oder eines verwandten Artikels Fragen zu Azure Backup stellen. Außerdem können Sie Fragen zum Azure Backup-Dienst im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) stellen.

## Was umfasst die öffentliche Vorschauversion?
Mit der öffentlichen Vorschauversion werden der Recovery Services-Tresor und die ARM-Unterstützung beim Schützen von Azure-VMs eingeführt. Der Recovery Services-Tresor ist die nächste Generation der Tresore. Dieser Tresor wird von den Diensten Azure Backup und Azure Site Recovery (ASR) genutzt. Sie können sich dies als „Tresor v.2“ vorstellen.

## Recovery Services- und Backup-Tresore

**F1. Da Version 2 (v.2) von Recovery Services-Tresoren verfügbar ist, stellt sich die Frage, ob Backup-Tresore (v.1) noch unterstützt werden?** <br/> A1. Ja. Backup-Tresore werden weiterhin unterstützt. Erstellen Sie Backup-Tresore im klassischen Portal. Erstellen Sie Recovery Services-Tresore im Azure-Portal.

**F2. Kann ich einen Backup-Tresor zu einem Recovery Services-Tresor migrieren?** <br/> A2. Nein, leider nicht. Derzeit ist es nicht möglich, den Inhalt eines Backup-Tresors zu einem Recovery Services-Tresor zu migrieren. Wir arbeiten daran, diese Funktion hinzuzufügen, aber sie steht im Rahmen der öffentlichen Vorschau noch nicht zur Verfügung.

**F3. Unterstützen Recovery Services-Tresore VMs der v.1 oder v.2?** <br/> A3. Recovery Services-Tresore unterstützen VMs der v.1 und v.2. Sie können eine im klassischen Portal erstellte VM (v.1) oder eine im Azure-Portal erstellte VM (v.2) in einem Recovery Services-Tresor sichern.


## ARM-Unterstützung für Azure-VMs

**F1. Gelten in Bezug auf die ARM-Unterstützung für Azure-VMs Einschränkungen?** <br/> A1. Die PowerShell-Cmdlets für ARM sind derzeit nicht verfügbar. Sie müssen die Benutzeroberfläche des Azure-Portals verwenden, um einer Ressourcengruppe Ressourcen hinzuzufügen.

<!---HONumber=AcomDC_0406_2016-->