---
title: "Azure Multi-Factor Authentication – Funktionsweise"
description: "Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache Überprüfungsoptionen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="how-azure-multi-factor-authentication-works"></a>Funktionsweise von Azure Multi-Factor Authentication
Die Sicherheit der Überprüfung in zwei Schritten liegt im Ebenenansatz. Die Faktoren der mehrfachen Authentifizierung zu überwinden stellt eine große Herausforderung für Angreifer dar. Selbst wenn ein Angreifer das Kennwort des Benutzers herausfinden kann, ist dies nutzlos, wenn er das vertraute Gerät nicht besitzt. 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren.  Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache Überprüfungsoptionen.


## <a name="methods-available-for-two-step-verification"></a>Verfügbare Methoden für die Überprüfung in zwei Schritten
Wenn ein Benutzer sich anmeldet, wird eine zusätzliche Bestätigung an den Benutzer gesendet.  In der folgenden Liste sind die verschiedenen Methoden zusammengefasst, die für diese zweite Überprüfung verwendet werden können.

| Überprüfungsmethode | Beschreibung |
| --- | --- |
| Telefonanruf |Auf dem registrierten Telefon des Benutzers erfolgt ein Anruf. Der Benutzer gibt ggf. eine PIN ein und drückt dann die #-Taste. |
| Textnachricht |Eine Textnachricht mit einem sechsstelligen Code wird an das Mobiltelefon des Benutzers gesendet. Der Benutzer gibt diesen Code auf der Anmeldeseite ein. |
| Benachrichtigung über eine mobile App |Eine Überprüfungsanforderung wird an das Smartphone eines Benutzers gesendet. Der Benutzer gibt in der mobilen App ggf. eine PIN ein und wählt dann **Überprüfen**. |
| Prüfcode in der mobilen App |Die mobile App, die auf dem Smartphone eines Benutzers ausgeführt wird, zeigt einen Prüfcode an, der sich alle 30 Sekunden ändert. Der Benutzer sucht nach den neuesten Code und gibt ihn auf der Anmeldeseite. |
| OATH-Token von Drittanbietern | Azure Multi-Factor Authentication Server kann so konfiguriert werden, dass Prüfmethoden von Drittanbietern akzeptiert werden. |

Azure Multi-Factor Authentication umfasst auswählbare Überprüfungsmethoden für die Cloud und für Server. Sie können auswählen, welche Methoden für Ihre Benutzer verfügbar sein sollen: Anruf, SMS, App-Benachrichtigung oder App-Codes. Weitere Informationen finden Sie unter [Auswählbare Verifizierungsmethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich zu den verschiedenen [Versionen und Verbrauchsmethoden für Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

- Wählen Sie, ob Azure MFA [in der Cloud oder lokal](multi-factor-authentication-get-started.md) bereitgestellt werden soll.

- Lesen Sie Antworten auf [Häufig gestellte Fragen](multi-factor-authentication-faq.md).
