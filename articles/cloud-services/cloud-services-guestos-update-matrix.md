---
title: Informationen zu den neuesten Azure-Gastbetriebssystemversionen | Microsoft-Dokumentation
description: "Die neueste Releaseneuigkeiten und SDK-Kompatibilität für das Azure Cloud Services-Gastbetriebssystem."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 3/8/2017
ms.author: raiye
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 182b03d7075c3161f09f27f265b624897cb7a9cc
ms.lasthandoff: 03/09/2017


---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix
Bietet Ihnen aktuelle Informationen zu den neuesten Azure-Gastbetriebssystemreleases für Cloud Services. Anhand dieser Informationen können Sie Ihren Upgradepfad planen, bevor ein Gastbetriebssystem deaktiviert wird. Wenn Sie die Rollen so konfigurieren, dass die *automatischen* Gast-BS-Updates, wie unter [Updateeinstellungen für Azure-Gast-BS][Azure Guest OS Update Settings] beschrieben, verwendet werden, müssen Sie diese Seite nicht unbedingt lesen.

> [!IMPORTANT]
> Diese Seite bezieht sich auf die Cloud Services-Web- und Workerrollen, die zusätzlich zu einem Gastbetriebssystem ausgeführt werden. Sie **gilt nicht** für IaaS Virtual Machines. 
> 
> 

<!-- -->

> [!TIP]
> Abonnieren Sie den [RSS-Feed zu Gastbetriebssystemupdates][rss], um immer sofort über alle Gastbetriebssystemänderungen benachrichtigt zu werden.
> 
> 

Sind Sie unsicher, was das Gast-BS ist oder wie Gast-BS-Releases funktionieren? Lesen Sie [diesen](#how-it-works) Abschnitt.

## <a name="news-updates"></a>Neuigkeiten
###### <a name="january-10-2017"></a>**10. Januar 2017**
Das Januar-Gastbetriebssystem enthält Patches, die nur die Betriebssystemfamilie 2 betreffen (Windows 2008 Server R2). Wir haben daher nur das Image für die Betriebssystemfamilie 2 (WA-GUEST-OS-2.59_201701-01) für diesen Monat veröffentlicht. Für alle anderen Betriebssystemfamilien, bleibt das Betriebssystem vom Dezember (201612-01) das aktuellste.

###### <a name="december-14-2016"></a>**14. Dezember 2016**
Die Bereitstellung des Gastbetriebssystems beginnt am 14. Dezember 2016 und soll voraussichtlich am 13. Januar 2017 freigegeben werden.

###### <a name="november-20-2016"></a>**20. November 2016**
Die Bereitstellung des Gastbetriebssystems beginnt am 8. November 2016 und soll voraussichtlich am 8. Dezember 2016 freigegeben werden.

###### <a name="october-23-2016"></a>**23. Oktober 2016**
Windows Server 2016 wird am 1. November 2016 als Gastbetriebssystemfamilie 5 veröffentlicht und bietet .NET 4.6-Unterstützung .


## <a name="releases"></a>Releases
## <a name="family-5-releases"></a>Releases von Familie 5
**Windows Server 2016**

Installierte .NET Framework-Versionen: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> Änderungen bei Datumsangaben mit einem * bleiben vorbehalten. 
> 
> Das RDP-Kennwort für Betriebssystemfamilie 5 muss mindestens zehn Zeichen umfassen.
>

| Konfigurationszeichenfolge | Herausgabedatum | Deaktivierungsdatum | Abgelaufenes Datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.4_201612-01 |10. Januar 2017 |Post 5.6 |TBD | 
| WA-GUEST-OS-5.3_201611-01 |14. Dezember 2016 |Nach Version 5.5 |TBD |
| WA-GUEST-OS-5.2_201610-02 |1. November 2016 |Nach Version 5.4 |TBD |

## <a name="family-4-releases"></a>Releases von Familie 4
**Windows Server 2012 R2**

Unterstützt .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Änderungen bei Datumsangaben mit einem * bleiben vorbehalten
> 
> 

| Konfigurationszeichenfolge | Herausgabedatum | Deaktivierungsdatum | Abgelaufenes Datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.39_201612-01 |10. Januar 2017 |Post 4.41 |TBD |
| WA-GUEST-OS-4.38_201611-01 |14. Dezember 2016 |Post 4.40 |TBD |
| WA-GUEST-OS-4.37_201610-02 |16. November 2016 |Nach Version 4.39 |TBD |
|~~WA-GUEST-OS-4.36_201609-01~~ |13. Oktober 2016 |14. Januar 2017 |TBD |
|~~WA-GUEST-OS-4.35_201608-01~~ |13. September 2016 |16. Dezember 2016 |TBD |
|~~WA-GUEST-OS-4.34_201607-01~~ |8. August 2016 |13. November 2016 |TBD |
|~~WA-GUEST-OS-4.33_201606-01~~ |13. Juli 2016 |13. Oktober 2016 |TBD |
|~~WA-GUEST-OS-4.32_201605-01~~ |10. Juni 2016 |8. September 2016 |TBD |
|~~WA-GUEST-OS-4.31_201604-01~~ |2. Mai 2016 |13. August 2016 |TBD |
|~~WA-GUEST-OS-4.30_201603-01~~ |7. April 2016 |10. Juli 2016 |TBD |
|~~WA-GUEST-OS-4.29_201602-02~~ |12. März 2016 |2. Juni 2016 |TBD |
|~~WA-GUEST-OS-4.28_201601-01~~ |12. Februar 2016 |7. Mai 2016 |TBD |
|~~WA-GUEST-OS-4.27_201512-01~~ |12. Januar 2016 |12. April 2016 |TBD |

## <a name="family-3-releases"></a>Releases von Familie 3
**Windows Server 2012**

Unterstützt .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Änderungen bei Datumsangaben mit einem * bleiben vorbehalten
> 
> 

| Konfigurationszeichenfolge | Herausgabedatum | Deaktivierungsdatum | Abgelaufenes Datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.46_201612-01 |10. Januar 2017 |Post 3.48 |TBD |
| WA-GUEST-OS-3.45_201611-01 |14. Dezember 2016 |Nach Version 3.47 |TBD |
| WA-GUEST-OS-3.44_201610-01 |16. November 2016 |Nach Version 3.46 |TBD |
| ~~WA-GUEST-OS-3.43_201609-01~~ |13. Oktober 2016 |14. Januar 2017 |TBD |
| ~~WA-GUEST-OS-3.42_201608-01~~ |13. September 2016 |16. Dezember 2016 |TBD |
| ~~WA-GUEST-OS-3.41_201607-01~~ |8. August 2016 |13. November 2016 |TBD |
| ~~WA-GUEST-OS-3.40_201606-01~~ |13. Juli 2016 |13. Oktober 2016 |TBD |
| ~~WA-GUEST-OS-3.39_201605-01~~ |10. Juni 2016 |8. September 2016 |TBD |
| ~~WA-GUEST-OS-3.38_201604-01~~ |2. Mai 2016 |13. August 2016 |TBD |
| ~~WA-GUEST-OS-3.37_201603-01~~ |7. April 2016 |10. Juli 2016 |TBD |
| ~~WA-GUEST-OS-3.36_201602-02~~ |12. März 2016 |2. Juni 2016 |TBD |
| ~~WA-GUEST-OS-3.35_201601-01~~ |12. Februar 2016 |7. Mai 2016 |TBD |
| ~~WA-GUEST-OS-3.34_201512-01~~ |12. Januar 2016 |12. April 2016 |TBD |

## <a name="family-2-releases"></a>Releases von Familie 2
**Windows Server 2008 R2 SP1**

Unterstützt .NET 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Änderungen bei Datumsangaben mit einem * bleiben vorbehalten
> 
> 

| Konfigurationszeichenfolge | Herausgabedatum | Deaktivierungsdatum | Abgelaufenes Datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.59_201701-01 |10. Januar 2017 |Post 2.61 |TBD |
| WA-GUEST-OS-2.58_201612-01 |10. Januar 2017 |Post 2.60 |TBD |
| WA-GUEST-OS-2.57_201611-01 |14. Dezember 2016 |Nach Version 2.59 |TBD |
| WA-GUEST-OS-2.56_201610-01 |16. November 2016 |10. Februar 2017 |TBD |
|~~WA-GUEST-OS-2.55_201609-01~~ |13. Oktober 2016 |14. Januar 2017 |TBD |
|~~WA-GUEST-OS-2.54_201608-01~~ |13. September 2016 |16. Dezember 2016 |TBD |
|~~WA-GUEST-OS-2.53_201607-01~~ |8. August 2016 |13. November 2016 |TBD |
|~~WA-GUEST-OS-2.52_201606-01~~ |13. Juli 2016 |13. Oktober 2016 |TBD |
|~~WA-GUEST-OS-2.51_201605-01~~ |10. Juni 2016 |8. September 2016 |TBD |
|~~WA-GUEST-OS-2.50_201604-01~~ |2. Mai 2016 |13. August 2016 |TBD |
|~~WA-GUEST-OS-2.49_201603-01~~ |7. April 2016 |10. Juli 2016 |TBD |
|~~WA-GUEST-OS-2.48_201602-02~~|12. März 2016 |2. Juni 2016 |TBD |
|~~WA-GUEST-OS-2.47_201601-01~~|12. Februar 2016 |7. Mai 2016 |TBD |
|~~WA-GUEST-OS-2.46_201512-01~~|12. Januar 2016 |12. April 2016 |TBD |

## <a name="msrc-patch-updates"></a>MSRC-Patch-Updates
Die Liste der Patches, die in den einzelnen monatlichen Gastbetriebssystemreleases enthalten sind, steht [hier][patches] zur Verfügung.

## <a name="sdk-support"></a>SDK-Unterstützung
Obwohl die [Deaktivierungsrichtlinie für das Azure SDK][retire policy sdk] angibt, dass nur Versionen ab 2.2 unterstützt werden, können bei bestimmten Gastbetriebssystem-Familien frühere Versionen verwendet werden. Sie sollten immer das neueste unterstützte SDK verwenden. 

| Gastbetriebssystemfamilie | Kompatible SDK-Versionen |
| --- | --- |
| 5 |Version 2.9.5.1+ |
| 4 |Version 2.1+ |
| 3 |Version 1.8+ |
| 2 |Version 1.3+ |
| 1 |Version 1.0+ |

## <a name="guest-os-release-information"></a>Informationen zu den Gastbetriebssystemreleases
Es gibt drei wichtige Daten für Gastbetriebssystemreleases: **Freigabedatum**, **Deaktivierungsdatum** und **Ablaufdatum**. Ein Gast-BS wird als verfügbar betrachtet, wenn es im Portal verfügbar ist und als Zielgastbetriebssystem ausgewählt werden kann. Wenn das **Deaktivierungsdatum** für ein Gastbetriebssystem erreicht wird, wird es aus Azure entfernt. Jedoch funktioniert jeder Clouddienst, der auf dieses Gast-BS abzielt, wie gewohnt. 

Das Fenster zwischen dem **Deaktivierungsdatum** und dem **Ablaufdatum** bietet Ihnen einen Puffer, um problemlos von einem Gastbetriebssystem zu einem neueren zu wechseln. Wenn Sie die Option *automatisch* für Ihr Gastbetriebssystem festgelegt haben, verwenden Sie immer die neueste Version. In diesem Fall müssen Sie sich keine Gedanken über das Ablaufdatum machen. 

Wenn das **Ablaufdatum** überschritten ist und das Gastbetriebssystem weiterhin von einem Clouddienst verwendet wird, wird das Gastbetriebssystem angehalten oder gelöscht bzw. es wird ein Upgrade erzwungen. Erfahren Sie [hier][retirepolicy] mehr über die Deaktivierungsrichtlinie.

## <a name="guest-os-family-version-explanation"></a>Erläuterung zu den Versionen der Gast-BS-Familie
Die Gastbetriebssystemfamilien basieren auf veröffentlichten Versionen von Microsoft Windows Server. Das Gastbetriebssystem ist das zugrunde liegende Betriebssystem, auf dem Azure Cloud Services ausgeführt werden. Jedes Gastbetriebssystem verfügt über eine Familien-, eine Versions- und eine Releasenummer. 

* **Guest OS family**  
  entspricht einem Windows Server-Betriebssystemrelease, auf dem ein Gastbetriebssystem basiert. Die *Familie 3* basiert beispielsweise auf Windows Server 2012.
* **Gastbetriebssystem-Version**  
  Gilt für das Image einer Gastbetriebssystemfamilie sowie relevante [Microsoft Security Response Center-Patches (MSRC)][msrc], die zum Zeitpunkt der Herstellung der neuen Gastbetriebssystemversion verfügbar sind. Möglicherweise sind nicht alle Patches enthalten. 
  
    Die Zahlen beginnen bei 0 und werden jedes Mal, wenn ein neuer Satz von Updates hinzugefügt wird, um 1 erhöht. Nachfolgende Nullen werden nur angezeigt, wenn sie von Bedeutung sind. Version 2.10 ist beispielsweise eine andere, viel höhere Version als Version 2.1.
* **Gastbetriebssystemrelease**  
  Eine Neuveröffentlichung einer Gastbetriebssystemversion. Eine Neuveröffentlichung kommt vor, wenn Microsoft beim Testen Probleme feststellt, die Änderungen erfordern. Das neueste Release ersetzt immer alle vorherigen Releases, unabhängig davon, ob diese veröffentlicht wurden oder nicht. Das klassischen Azure-Portal lässt Benutzer nur das neueste Release für eine bestimmte Version auswählen. Bereitstellungen, die auf einem früheren Release ausgeführt werden, werden normalerweise nicht zwangsweise aktualisiert. Dies ist abhängig vom Schweregrad des Fehlers. 

Im folgenden Beispiel steht 2 für die Familie, 12 für die Version, und "rel2" für das Release.

**Gastbetriebssystemrelease** – 2.12 rel2

**Konfigurationszeichenfolge für dieses Release** – WA-GUEST-OS-2.12_201208-02

In der Konfigurationszeichenfolge für ein Gastbetriebssystem sind dieselben Informationen eingebettet sowie ein Datum, das zeigt, welche MSRC-Patches in diesem Release berücksichtigt wurden. In diesem Beispiel wurden MSRC-Patches für Windows Server 2008 R2 bis einschließlich August 2012 integriert. Es werden nur Patches einbezogen, die ausdrücklich für diese Version von Windows Server gelten. Wenn beispielsweise ein MSRC-Patch für Microsoft Office gilt, wird er nicht berücksichtigt, da dieses Produkt kein Bestandteil des Windows Server-Basisimage ist. 

## <a name="guest-os-system-update-process"></a>Updateprozess des Gastbetriebssystems
Diese Seite enthält Informationen zu anstehenden Gastbetriebssystemreleases. Kunden haben uns mitgeteilt, dass sie wissen möchten, wann ein Release stattfindet, da ihre Clouddienstrollen neu gestartet werden, wenn sie auf "Automatisches Update" festgelegt sind. Gastbetriebssystemreleases werden in der Regel mindestens 5 Tage nach dem MSRC-Updaterelease veröffentlicht, das am zweiten Dienstag jedes Monats auftritt. Neue Releases enthalten alle relevanten MSRC-Patches für jede Gastbetriebssystemfamilie. 

Für Microsoft Azure werden ständig Updates veröffentlicht. Das Gastbetriebssystem ist nur ein solches Update in der Pipeline. Ein Release kann durch eine Reihe von Faktoren beeinflusst werden, die zu zahlreich sind, um hier aufgeführt zu werden. Darüber hinaus wird Azure auf Hunderttausenden von Computern ausgeführt. Daher ist es unmöglich, ein genaues Datum und eine Uhrzeit anzugeben, zu der Ihre Rollen neu gestartet werden. Wir aktualisieren den [RSS-Feed zu Gastbetriebssystemupdates][rss] mit den neuesten Informationen, die uns vorliegen. Dabei handelt es sich jedoch um ungefähre Zeitfenster. Uns ist bekannt, dass dies für Kunden problematisch ist, und arbeiten an einem Plan zur Begrenzung oder zeitlichen Planung von Neustarts. 

Wenn eine neue Version des Gastbetriebssystems veröffentlicht wird, kann die vollständige Verteilung über Azure einige Zeit in Anspruch nehmen. Während Dienste auf das neue Gastbetriebssystem aktualisiert werden, werden sie den Updatedomänen entsprechend neu gestartet. Dienste, für die automatische Updates festgelegt wurden, erhalten zuerst ein Release. Nach dem Update wird die neue Gastbetriebssystemversion für Ihren Dienst im klassischen Azure-Portal aufgeführt. Erneute Releases können während dieses Zeitraums auftreten. Einige Versionen können über einen längeren Zeitraum bereitgestellt werden, und es finden möglicherweise für viele Wochen nach dem offiziellen Veröffentlichungsdatum keine automatischen Upgradeneustarts statt. Sobald ein Gastbetriebssystem verfügbar ist, können Sie diese Version explizit im Portal oder in der Konfigurationsdatei auswählen. 

Viele wertvolle Informationen zu Neustarts und Verweise auf weitere technische Informationen zu Gast- und Hostbetriebssystemupdates finden Sie im MSDN-Blogbeitrag [Role Instance Restarts Due to OS Upgrades][restarts] (Neustarts von Rolleninstanzen aufgrund von Betriebssystemupgrades).

Wenn Sie Ihr Gastbetriebssystem manuell aktualisieren, lesen Sie bitte die [Deaktivierungsrichtlinie für Gastbetriebssysteme][retirepolicy].

## <a name="guest-os-supportability-and-retirement-policy"></a>Unterstützungs- und Deaktivierungsrichtlinie für Gastbetriebssysteme
Die Unterstützungs- und Deaktivierungsrichtlinie für Gastbetriebssysteme wird [hier][retirepolicy] erläutert.

[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure.md
[rss]: http://sxp.microsoft.com/feeds/3.0/msdntn/WindowsAzureOSUpdates
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md


