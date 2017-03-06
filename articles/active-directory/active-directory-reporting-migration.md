---
title: "Speicherorte von Aktivitätsberichten im Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wo Aktivitätsberichte im Azure-Portal gespeichert werden"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: 800397efdf2e3e3cb95c77bc90f45bb8852e1e42
ms.openlocfilehash: d05cf7dc62518ea5519f319a2502e1d261b148f3
ms.lasthandoff: 02/17/2017


---
# <a name="how-to-find-activity-reports-in-the-azure-portal"></a>Speicherorte von Aktivitätsberichten im Azure-Portal

Mit der Migration von klassischen Azure-Portal zum Azure-Portal werfen wir einen neuen Blick auf die Azure Active Directory-Aktivitätsprotokolle. Wir haben vor einigen Monaten einen [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/) veröffentlicht, aus dem hervorgeht, wie wir Aktivitätsprotokolle im Kontext der Ressource bieten, die Sie nutzen. In diesem Artikel wird beschrieben, wie wir die im klassischen Azure-Portal vorhandenen Berichte in das neue Azure-Portal übertragen haben.

## <a name="what-is-new"></a>Was ist neu?

Die Berichte im klassischen Azure-Portal sind in verschiedene Kategorien unterteilt:

1.    Sicherheitsberichte
2.    Aktivitätsberichte
3.    Berichte zu integrierten Apps

### <a name="activity-and-integrated-app-reports"></a>Aktivitätsberichte und Berichte zu integrierten Apps

Im neuen Azure-Portal sind wir zur kontextbasierten Berichterstellung gewechselt und haben die vorhandenen Berichte in einer zentralen Ansicht zusammengeführt, wobei eine einzelne zugrunde liegende API die Daten in der Ansicht bereitstellt. Sie finden diese Ansicht im Azure-Portal (im linken Navigationsbereich) unter „*Aktivitäten* > Überwachungsprotokolle“. Es folgen die Berichte, die in dieser Ansicht zusammengeführt wurden.

-    Überwachungsbericht

-     Kennwortzurücksetzungsaktivität

-     Aktivität "Registrierung für Zurücksetzen des Kennworts"

-     Self-Service-Gruppenaktivität

-     Namensänderungen für Office&365;-Gruppen

-     Kontobereitstellungsaktivität

-     Status des Kennwortrollovers
-     Kontobereitstellungsfehler


Der Bericht „Anwendungsnutzung“ wurde verbessert und einer Ansicht mit dem Namen „*Aktivitäten* -> Anmeldungen“ (im linken Navigationsbereich) hinzugefügt. Dieser Bericht enthält alle Anmeldevorgänge des Benutzers, anhand derer wiederum die Informationen zur „Anwendungsnutzung“ abgeleitet werden können. Auf dem Bildschirm mit der Übersicht über die Unternehmensanwendungen finden Sie ebenfalls Informationen zur App-Nutzung.

## <a name="how-can-i-access-a-specific-report-in-this-single-view"></a>Wie kann ich in dieser zentralen Ansicht auf einen bestimmten Bericht zugreifen?

### <a name="audit-logs"></a>Überwachungsprotokolle

Eine der häufigsten Anforderungen vieler Kunden ist die Möglichkeit, in Azure AD mittels mehrerer Filteroptionen auf Aktivitätsprotokolle zuzugreifen. An deren Stelle bieten wir nun einen erweiterten Filtermechanismus zum Filtern der gewünschten Daten. Einer der Filter, den wir bereitgestellt haben, heißt *Aktivitätskategorie*. Er listet die verschiedenen Typen von Aktivitätsprotokollen auf, die Azure AD bietet. Indem Sie die gewünschte Kategorie auswählen, können Sie die Ergebnisse der Aktivitätsprotokolle auf diejenigen eingrenzen, an denen Sie interessiert sind. 

Wenn Sie beispielsweise nur an Aktivitäten im Zusammenhang mit der **Self-Service-Kennwortzurücksetzung** interessiert sind, können Sie die Kategorie**Self-Service-Kennwortverwaltung** wählen. Die angezeigten Kategorien sind im Kontext der Ressource, mit der Sie arbeiten.  


![Überwachungsprotokolle](./media/active-directory-reporting-migration/06.png "Überwachungsprotokolle")

 
Derzeit gibt es die folgenden Kategorien:

- Kernverzeichnis

- Self-Service-Kennwortverwaltung

- Self-Service-Gruppenverwaltung

- Kontobereitstellung

### <a name="application-usage"></a>Anwendungsnutzung

In der Ansicht „Aktivitäten -> Anmeldungen“ können Sie die Anwendungsnutzung für alle oder eine einzelne App anzeigen. Wie unten dargestellt, ist diese Ansicht für alle Anwendungen oder eine einzelne Anwendung vorhanden. Wenn Sie die Ergebnisse eingrenzen möchten, können Sie anhand des Benutzer- oder Anwendungsnamen filtern.
 

![Überwachungsprotokolle](./media/active-directory-reporting-migration/07.png "Überwachungsprotokolle")


### <a name="security-reports"></a>Sicherheitsberichte

Die Sicherheitsberichte wurden zusammengeführt, um eine Ansicht aller Risikosignale bereitzustellen, die Azure Active Directory erkennen und melden kann.

Diese zusammengefasste Ansicht enthält Daten über Folgendes:

- Gefährdete Benutzer
- Riskante Anmeldungen 


![Überwachungsprotokolle](./media/active-directory-reporting-migration/04.png "Überwachungsprotokolle")



## <a name="activity-reports-in-azure-classic-portal-versus-azure-portal"></a>Aktivitätsberichte im klassischen Azure-Portal im Vergleich zum Azure-Portal

Dieser Abschnitt enthält die vorhandenen Berichte im klassischen Azure-Portal und erläutert, wie Sie diese Informationen in das Azure-Portal übertragen können.

**Überwachungsprotokolle:**

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Kernverzeichnis**. 

**Kennwortzurücksetzungsaktivität:**

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**. 

**Aktivität „Registrierung für Zurücksetzen des Kennworts“:**    

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**.

**Self-Service-Gruppenaktivität:**    

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Gruppenverwaltung**.

**Namensänderungen für Office&365;-Gruppen:**

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**.
3. Wählen Sie **Aktivitätsressourcentyp** die Option **Gruppe**. 
4. Wählen Sie als **Aktivitätsquelle** die Option **Office&365;-Gruppen**. 

**Kontobereitstellungsaktivität:**    

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung**.

**Status des Kennwortrollovers:**    

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Automatisches Rollover von App-Kennwörtern**.

**Kontobereitstellungsfehler:**

1. Klicken Sie im linken Navigationsbereich auf **Aktivitäten** und dann auf **Überwachungsprotokolle**.
2. Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung**.

**Anwendungsnutzung:**

- Klicken Sie im linken Navigationsbereich auf **Unternehmensanwendungen** und dann auf **Anmeldungen**.



