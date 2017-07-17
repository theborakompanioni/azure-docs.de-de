---
title: "Problembehandlung: Fehlende Daten im Azure Active Directory-Aktivitätsprotokoll | Microsoft-Dokumentation"
description: "Enthält eine Liste mit den verschiedenen verfügbaren Berichten für Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
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
ms.openlocfilehash: 18af51e95a283a5cd33688484a0d7477eb4b957d
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# Ich kann einige Aktionen nicht finden, die ich im Aktivitätsprotokoll für Azure Active Directory ausgeführt habe.
<a id="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log" class="xliff"></a>


## Symptome
<a id="symptoms" class="xliff"></a>

Ich habe einige Aktionen im Azure-Portal ausgeführt und erwartet, die Überwachungsprotokolle für diese Aktionen auf dem Blatt `Activity logs > Audit Logs` zu finden. Ich kann sie jedoch nicht finden.

 ![Berichterstellung](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## Ursache
<a id="cause" class="xliff"></a>

Aktionen werden nicht sofort im Aktivitätsüberwachungsprotokoll angezeigt. Ab der Ausführung des Vorgangs kann es zwischen 15 Minuten und eine Stunde dauern, bis die Überwachungsprotokolle im Portal angezeigt werden.

## Lösung
<a id="resolution" class="xliff"></a>

Warten Sie 15 Minuten bis eine Stunde, und überprüfen Sie dann, ob die Aktionen im Protokoll angezeigt werden. Wenn sie weiterhin nicht vorhanden sind, erstellen Sie ein Supportticket, und wir nehmen uns der Sache an.


## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Active Directory-Berichten](active-directory-reporting-faq.md).


