<properties
 pageTitle="Pläne und Abrechnung in Azure Scheduler"
 description="Pläne und Abrechnung in Azure Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="06/30/2016"
 ms.author="krisragh"/>

# Pläne und Abrechnung in Azure Scheduler

## Pläne für Auftragssammlungen

In Azure Scheduler wird nach Auftragssammlungen abgerechnet. Auftragssammlungen enthalten eine Reihe von Aufträgen und sind in drei Plänen (Free, Standard und Premium) verfügbar.

|**Auftragssammlungsplan**|**Max. Anzahl von Aufträgen pro Auftragssammlung**|**Max. Wiederholungen**|**Max. Auftragssammlungen pro Abonnement**|**Einschränkungen**|
|:---|:---|:---|:---|:---|
|**Free**|Fünf Aufträge pro Auftragssammlung|Einmal pro Stunde. Aufträge können maximal einmal pro Stunde ausgeführt werden.|Ein Abonnement kann maximal eine Auftragssammlung vom Typ „Free“ enthalten.|Das [HTTP-Objekt für die ausgehende Autorisierung](scheduler-outbound-authentication.md) kann nicht verwendet werden.
|**Standard**|50 Aufträge pro Auftragssammlung|Einmal pro Minute. Aufträge können maximal einmal pro Minute ausgeführt werden.|Ein Abonnement kann maximal 100 Auftragssammlungen vom Typ „Standard“ enthalten.|Zugriff auf sämtliche Scheduler-Features|
|**P10 Premium**|50 Aufträge pro Auftragssammlung|Einmal pro Minute. Aufträge können maximal einmal pro Minute ausgeführt werden.|Ein Abonnement kann maximal 10.000 Auftragssammlungen vom Typ „P10 Premium“ enthalten. <a href="mailto:wapteams@microsoft.com">Setzen Sie sich mit uns in Verbindung</a>, um weitere Informationen zu erhalten.|Zugriff auf sämtliche Scheduler-Features|
|**P20 Premium**|1\.000 Aufträge pro Auftragssammlung|Einmal pro Minute. Aufträge können maximal einmal pro Minute ausgeführt werden.|Ein Abonnement kann maximal 10.000 Auftragssammlungen vom Typ „P20 Premium“ enthalten. <a href="mailto:wapteams@microsoft.com">Setzen Sie sich mit uns in Verbindung</a>, um weitere Informationen zu erhalten.|Zugriff auf sämtliche Scheduler-Features|

## Upgrades und Downgrades für Auftragssammlungspläne

Sie können bei Ihren Auftragssammlungsplänen jederzeit zwischen „Free“, „Standard“ und „Premium“ wechseln. Beim Herabstufen einer Auftragssammlung zu „Free“ kann jedoch aus folgenden Gründen ein Fehler auftreten:

- Im Abonnement ist bereits eine Auftragssammlung vom Typ „Free“ vorhanden.
- Ein Auftrag in der Auftragssammlung hat eine höhere Wiederholungsrate als für Aufträge in Auftragssammlungen vom Typ „Free“ zulässig. In Auftragssammlungen vom Typ „Free“ ist maximal eine Wiederholung pro Stunde zulässig.
- Die Auftragssammlung enthält mehr als fünf Aufträge.
- Ein Auftrag in der Auftragssammlung besitzt eine HTTP- oder HTTPS-Aktion, die ein [HTTP-Objekt für die ausgehende Autorisierung](scheduler-outbound-authentication.md) verwendet.

## Abrechnung und Azure-Pläne

Für Auftragssammlungen vom Typ „Free“ fallen in Abonnements keine Kosten an. Bei mehr als 100 Standard-Auftragssammlungen (entspricht zehn Standard-Abrechnungseinheiten) lohnt sich die Verlagerung der Auftragssammlungen in der Premium-Plan.

Wenn Sie eine Auftragssammlung vom Typ "Standard" und eine vom Typ "Premium" besitzen, werden Ihnen eine Standard-Abrechnungseinheit _und_ eine Premium-Abrechnungseinheit in Rechnung gestellt. Die Abrechnung des Scheduler-Diensts basiert auf der Anzahl aktiver Auftragssammlungen vom Typ „Standard“ oder „Premium“. Dies wird in den beiden folgenden Abschnitten ausführlicher erläutert.

## Standard-Abrechnungseinheiten

Eine Standard-Abrechnungseinheit kann bis zu zehn Standard-Auftragssammlungen enthalten. Da eine Standard-Auftragssammlung bis zu 50 Aufträge enthalten kann, kann ein Abonnement mit einer Standard-Abrechnungseinheit bis zu 500 Aufträge (und nahezu 22 Millionen Auftragsausführungen pro Monat) umfassen.

Bis zu einer Anzahl von zehn Standard-Auftragssammlungen wird eine einzelne Standard-Abrechnungseinheit berechnet. Wenn Sie zwischen elf und 20 Standard-Auftragssammlungen besitzen, werden zwei Standard-Abrechnungseinheiten berechnet. Wenn Sie zwischen 21 und 30 Standard-Auftragssammlungen besitzen, werden drei Standard-Abrechnungseinheiten berechnet, usw.

## P10 Premium-Abrechnungseinheiten

Eine P10 Premium-Abrechnungseinheit kann bis zu 10.000 P10 Premium-Auftragssammlungen enthalten. Da eine P10 Premium-Auftragssammlung bis zu 50 Aufträge enthalten kann, kann ein Abonnement mit einer Premium-Abrechnungseinheit bis zu 500.000 Aufträge (und nahezu 22 Milliarden Auftragsausführungen pro Monat) umfassen.

Wenn Sie bis zu 10.000 Premium-Auftragssammlungen besitzen, wird eine P10 Premium-Abrechnungseinheit berechnet. Wenn Sie zwischen 10.001 und 20.000 Premium-Auftragssammlungen besitzen, werden zwei P10 Premium-Abrechnungseinheiten berechnet usw.

P10 Premium-Auftragssammlungen haben den gleichen Funktionsumfang wie Standard-Auftragssammlungen, sind preislich aber interessanter, wenn Ihre Anwendung besonders viele Auftragssammlungen benötigt.

## P20 Premium-Abrechnungseinheiten

Eine P20 Premium-Abrechnungseinheit kann bis zu 5.000 P20 Premium-Auftragssammlungen enthalten. Da eine P20 Premium-Auftragssammlung bis zu 1.000 Aufträge enthalten kann, kann ein Abonnement mit einer Premium-Abrechnungseinheit bis zu 5.000.000 Aufträge (und nahezu 220 Milliarden Auftragsausführungen pro Monat) umfassen.

P20 Premium-Auftragssammlungen bieten den gleichen Funktionsumfang wie P10 Premium-Auftragssammlungen, unterstützen aber eine größere Anzahl von Aufträgen pro Auftragssammlung und eine größere Anzahl von Aufträgen insgesamt als P10 Premium, sodass Sie von größerer Skalierbarkeit profitieren.

## Abrechnung und aktiver Status

Auftragssammlungen sind immer aktiv, es sei denn, Ihr gesamtes Abonnement wurde aufgrund von Problemen bei der Abrechnung vorübergehend deaktiviert. Wenn Sie sichergehen möchten, dass eine Auftragssammlung nicht in Rechnung gestellt wird, müssen Sie sie entweder mit dem Plan _Free_ versehen oder die Auftragssammlung löschen.

Sie können zwar mit einem einzelnen Vorgang alle Aufträge innerhalb einer Auftragssammlung deaktivieren, dies hat jedoch keine Auswirkungen auf deren Abrechnungsstatus. Mit anderen Worten: Die Auftragssammlung wird _weiterhin_ in Rechnung gestellt. Darüber hinaus gelten auch leere Auftragssammlungen als aktiv und werden berechnet.

## Preise

Ausführliche Informationen finden Sie unter [Scheduler Preise](https://azure.microsoft.com/pricing/details/scheduler/).

## Siehe auch


 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)

<!---HONumber=AcomDC_0803_2016-->