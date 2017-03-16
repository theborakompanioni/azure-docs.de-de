---
title: "Azure Multi-Factor Authentication – Funktionsweise"
description: "Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache Überprüfungsoptionen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtland
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: e028130569a1e55834f68bde87f21f6f7505d41f
ms.openlocfilehash: 9132e9bde7403e2868b92cb1149904d926ff5d8e
ms.lasthandoff: 02/23/2017


---
# <a name="how-azure-multi-factor-authentication-works"></a>Funktionsweise von Azure Multi-Factor Authentication
Die Sicherheit der Multi-Factor Authentication liegt im Ebenenansatz. Die Faktoren der mehrfachen Authentifizierung zu überwinden stellt eine große Herausforderung für Angreifer dar. Selbst wenn ein Angreifer das Kennwort des Benutzers herausfinden kann, ist dies nutzlos, wenn er das vertraute Gerät nicht besitzt. Auch wenn der Benutzer das Gerät verliert, kann es der Finder des Geräts nicht verwenden, wenn er nicht gleichzeitig auch das Kennwort des Benutzers kennt.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren.  Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache Überprüfungsoptionen.

Detaillierte Informationen zur Funktionsweise erhalten Sie im folgenden Video:

> [!VIDEO https://channel9.msdn.com/Events/TechEd/Europe/2014/EM-B313/player]

## <a name="methods-available-for-two-step-verification"></a>Verfügbare Methoden für die Überprüfung in zwei Schritten
Wenn ein Benutzer sich anmeldet, wird eine zusätzliche Bestätigung an den Benutzer gesendet.  In der folgenden Liste sind die verschiedenen Methoden zusammengefasst, die für diese zweite Überprüfung verwendet werden können.

| Überprüfungsmethode | Beschreibung |
| --- | --- |
| Telefonanruf |Es wird ein automatisierter Anruf an das registrierte Telefon des Benutzers ausgelöst. Der Benutzer bestätigt den Anmeldevorgang durch Drücken der Rautetaste oder durch Eingabe einer PIN. |
| Textnachricht |Eine Textnachricht mit einem sechsstelligen Code wird an das Mobiltelefon des Benutzers gesendet.  Dieser Code muss eingegeben werden, um die Überprüfung abzuschließen. |
| Benachrichtigung über mobile App |An das Smartphone des Benutzers wird eine Überprüfungsanforderung gesendet. Darin wird der Benutzer aufgefordert, in der mobilen App die Option „Überprüfen“ auszuwählen, um die Überprüfung abzuschließen. Diese Variante wird verwendet, wenn Benutzer das Senden einer App-Benachrichtigung als primäre Überprüfungsmethode ausgewählt haben.  Wenn Benutzer eine Anforderung erhalten, obwohl sie derzeit keinen Anmeldevorgang durchführen, können sie diese als Betrugsversuch melden. |
| Prüfcode in der mobilen App |Die mobile App, die auf dem Smartphone eines Benutzers ausgeführt wird, zeigt einen sechsstelligen Prüfcode an, der sich alle 30 Sekunden ändert. Der Benutzer sucht nach den neuesten Code und gibt ihn auf der Anmeldeseite ein, um die Überprüfung abzuschließen. Diese Variante wird verwendet, wenn Benutzer das Senden eines Überprüfungscodes als primäre Überprüfungsmethode ausgewählt haben. |
| Drittanbieter-OATH-Tokens | Azure Multi-Factor Authentication kann so konfiguriert werden, dass Prüfmethoden von Drittanbietern akzeptiert werden. |

Azure Multi-Factor Authentication umfasst auswählbare Überprüfungsmethoden für die Cloud und für Server. Das bedeutet, dass Sie auswählen können, welche Methoden für Ihre Benutzer verfügbar sein sollen: Anruf, SMS, App-Benachrichtigung oder App-Codes. Weitere Informationen finden Sie unter [Auswählbare Verifizierungsmethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich zu den verschiedenen [Versionen und Verbrauchsmethoden für Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

- Wählen Sie, ob Azure MFA [in der Cloud oder lokal](multi-factor-authentication-get-started.md) bereitgestellt werden soll.
