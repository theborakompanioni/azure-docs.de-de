 (Sicherungstresore<properties
   pageTitle="Azure Backup limits table"
   description="Beschreibung der Systembegrenzung für Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />

---
{}

ms.service: backup ms.devlang: NA ms.topic: article ms.tgt_pltfrm: NA ms.workload: TBD ms.date: 10/05/2015 ms.author: trinadhk ;"jimpark";: '' "\"aashishr\"": ''

---
Für Azure Backup gelten die folgenden Beschränkungen.

| Bezeichnung | Standardlimit |
| --- | --- |
| Anzahl der Server/Computer, die für jeden Tresor registriert werden können |50 für Windows Server/Windows Client/SCDPM  <br/>  200 für virtuelle IaaS-Computer |
| Größe einer Datenquelle für im Azure-Tresorspeicher gespeicherten Daten |Max. 54.400 GB<sup>1</sup> |
| Anzahl der Sicherungstresore, die in einem Azure-Abonnement erstellt werden können |25 (Sicherungstresore) <br/> 25 Recovery Services-Tresore pro Region |
| Häufigkeit der geplanten Sicherungen pro Tag |3 Sicherungen pro Tag für Windows Server und Windows Client  <br/> 2 pro Tag für SCDPM <br/>  Eine Sicherung pro Tag für virtuelle IaaS-Computer |
| Datenträger, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind |16 |

* <sup>1</sup>Die Beschränkung von  54.400 GB gilt nicht für IaaS-VM-Sicherungen.



<!--HONumber=Nov16_HO3-->


