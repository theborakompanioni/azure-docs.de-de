---
title: "Problembehandlung: Fehlende Daten in den heruntergeladenen Azure Active Directory-Aktivitätsprotokollen | Microsoft-Dokumentation"
description: "Bietet Ihnen eine Lösung für fehlende Daten in heruntergeladenen Azure Active Directory-Aktivitätsprotokollen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9109c698e4e8b43eeb7534c338adc99476012a3f
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# Ich kann keine Daten in den Azure Active Directory-Aktivitätsprotokollen finden, die ich heruntergeladen habe.
<a id="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded" class="xliff"></a>


## Symptome
<a id="symptoms" class="xliff"></a>

Ich habe die Aktivitätsprotokolle (Überwachung oder Anmeldungen) heruntergeladen, und für den ausgewählten Zeitraum werden nicht alle Datensätze angezeigt. Warum? 

 ![Berichterstellung](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## Ursache
<a id="cause" class="xliff"></a>

Beim Herunterladen von Aktivitätsprotokollen im Azure-Portal gilt eine Obergrenze von 120.000 Datensätzen, die nach Aktualität sortiert werden. 

## Lösung
<a id="resolution" class="xliff"></a>

Mithilfe von [Azure AD-Berichterstellungs-APIs](active-directory-reporting-api-getting-started.md) können jederzeit bis zu einer Millionen Datensätze abgerufen werden. Unsere empfohlene Vorgehensweise ist das Ausführen eines Skripts auf Basis eines Zeitplans, der die Berichterstellungs-APIs zum inkrementellen Abrufen von Datensätzen über eine bestimmte Zeitspanne (z.B. täglich oder wöchentlich) aufruft.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Active Directory-Berichten](active-directory-reporting-faq.md).


