---
title: IIS-Protokolle in Log Analytics | Microsoft Docs
description: "IIS (Internet Information Services, Internetinformationsdienste) speichern Benutzeraktivitäten in Protokolldateien, die von Log Analytics gesammelt werden können.  Dieser Artikel beschreibt die Konfiguration der Sammlung von IIS-Protokollen sowie Details zu den Datensätzen, die im OMS-Repository erstellt werden."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 2114bdafb3b9fe2eb0632271840b8b70a76d10f1
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="iis-logs-in-log-analytics"></a>IIS-Protokolle in Log Analytics
IIS (Internet Information Services, Internetinformationsdienste) speichern Benutzeraktivitäten in Protokolldateien, die von Log Analytics gesammelt werden können.  

![IIS-Protokolle](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurieren von IIS-Protokollen
Log Analytics sammelt Einträge aus von IIS erstellten Protokolldateien. Daher müssen Sie [IIS für die Protokollierung konfigurieren](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics unterstützt nur IIS-Protokolldateien im W3C-Format, aber keine benutzerdefinierten Felder oder die erweiterte IIS-Protokollierung.  
Daten aus Protokollen im NCSA- oder nativen IIS-Format werden von Log Analytics nicht gesammelt.

Konfigurieren Sie die IIS-Protokolle in Log Analytics über das [Menü „Daten“ in den Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).  Abgesehen von der Auswahl der Option **IIS-Protokolldateien im W3C-Format sammeln**ist keine Konfiguration erforderlich.

Wenn Sie die IIS-Protokollsammlung aktivieren, sollten Sie für jeden Server die Einstellung für das IIS-Protokollrollover konfigurieren.

## <a name="data-collection"></a>Datensammlung
Log Analytics sammelt etwa alle 15 Minuten IIS-Protokolleinträge aus jeder verbundenen Quelle.  Der Agent zeichnet seine Position in jedem Ereignisprotokoll auf, aus dem er Daten sammelt.  Wenn der Agent offline geht, sammelt Log Analytics Ereignisse ab dem Zeitpunkt der letzten Sammlung, unabhängig davon, ob die Ereignisse erstellt wurden, während der Agent offline war.

## <a name="iis-log-record-properties"></a>Eigenschaften der IIS-Protokolldatensätze
IIS-Protokolldatensätze weisen den Typ **W3CIISLog** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften:

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Computer |Name des Computers, auf dem das Ereignis gesammelt wurde. |
| cIP |IP-Adresse des Clients. |
| csMethod |Methode der Anforderung, beispielsweise GET oder POST. |
| csReferer |Website, von der der Benutzer über einen Link auf die aktuelle Website gelangt ist. |
| csUserAgent |Browsertyp des Clients. |
| csUserName |Name des authentifizierten Benutzers, der auf den Server zugegriffen hat. Anonyme Benutzer werden durch einen Bindestrich gekennzeichnet. |
| csUriStem |Ziel der Anforderung, beispielsweise eine Webseite. |
| csUriQuery |Abfrage, die der Client versucht hat auszuführen. |
| ManagementGroupName |Name der Verwaltungsgruppe für Operations Manager-Agents.  Bei anderen Agents lautet dieser „AOI-\<Arbeitsbereich-ID\>“. |
| RemoteIPCountry |Land der IP-Adresse des Clients. |
| RemoteIPLatitude |Breitengrad der Client-IP-Adresse. |
| RemoteIPLongitude |Längengrad der Client-IP-Adresse. |
| scStatus |HTTP-Statuscode. |
| scSubStatus |Unterstatus-Fehlercode. |
| scWin32Status |Windows-Statuscode. |
| sIP |IP-Adresse des Webservers. |
| SourceSystem |Operations Manager |
| sPort |Port auf dem Server, mit dem der Client verbunden ist. |
| sSiteName |Name der IIS-Website. |
| TimeGenerated |Datum und Uhrzeit, zu der der Eintrag protokolliert wurde. |
| TimeTaken |Verarbeitungsdauer der Anforderung in Millisekunden. |

## <a name="log-searches-with-iis-logs"></a>Protokollsuchvorgänge mit IIS-Protokollen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollabfragen, die IIS-Protokolldatensätze abrufen.

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=W3CIISLog |Alle IIS-Protokolldatensätze. |
| Type=W3CIISLog scStatus=500 |Alle IIS-Protokolleinträge mit dem Rückgabestatus 500 |
| Type=W3CIISLog &#124; Measure count() by cIP |Anzahl der IIS-Protokolleinträge nach Client-IP-Adresse. |
| Type=W3CIISLog csHost="www.contoso.com" &#124; Measure count() by csUriStem |Anzahl der IIS-Protokolleinträge nach URL für den Host www.contoso.com. |
| Type=W3CIISLog &#124; Measure Sum(csBytes) by Computer &#124; top 500000 |Gesamtzahl an Bytes, die von jedem IIS-Computer empfangen wurden. |

>[!NOTE]
> Falls für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) durchgeführt wurde, müssen die obigen Abfragen wie folgt geändert werden.

> | Abfrage | Beschreibung |
|:--- |:--- |
| W3CIISLog |Alle IIS-Protokolldatensätze. |
| W3CIISLog &#124; where scStatus==500 |Alle IIS-Protokolleinträge mit dem Rückgabestatus 500 |
| W3CIISLog &#124; summarize count() by cIP |Anzahl der IIS-Protokolleinträge nach Client-IP-Adresse. |
| W3CIISLog &#124; where csHost=="www.contoso.com" &#124; summarize count() by csUriStem |Anzahl der IIS-Protokolleinträge nach URL für den Host www.contoso.com. |
| W3CIISLog &#124; summarize sum(csBytes) by Computer &#124; take 500000 |Gesamtzahl an Bytes, die von jedem IIS-Computer empfangen wurden. |

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Log Analytics für die Sammlung von Daten aus anderen [Datenquellen](log-analytics-data-sources.md) zur Analyse.
* Informieren Sie sich über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.
* Konfigurieren Sie Warnungen in Log Analytics, sodass Sie proaktiv benachrichtigt werden, wenn in IIS-Protokollen wichtige Probleme gefunden werden.

