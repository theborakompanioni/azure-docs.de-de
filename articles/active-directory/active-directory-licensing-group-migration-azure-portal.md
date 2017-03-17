---

title: Migrieren Ihrer einzelnen lizenzierten Benutzer zu einer Gruppe in Azure Active Directory | Microsoft-Dokumentation
description: Informationen zum Migrieren einzelner Benutzerlizenzen zur gruppenbasierten Lizenzierung mithilfe von Azure Active Directory
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: ac3f563828c5fa379f328392a3f5cf7c7932f534
ms.openlocfilehash: 7582602b16a9a307cc4e5e9a1862653e25ba300f
ms.lasthandoff: 03/01/2017


---

# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a>Migrieren lizenzierter Benutzer zu einer Gruppe für die Lizenzierung in Azure Active Directory

Sie haben möglicherweise einzelnen Benutzern in den Abteilungen bereits per direkter Zuweisung Lizenzen zugewiesen, z.B. mithilfe von PowerShell-Skripts oder anderen Tools. Wenn Sie mit der gruppenbasierten Lizenzierung zum Verwalten von Lizenzen in Ihrer Organisation beginnen möchten, benötigen Sie einen Migrationsplan, um vorhandene Lösungen reibungslos durch die gruppenbasierte Lizenzierung zu ersetzen.

Der wichtigste zu beachtende Aspekt ist, dass Sie unbedingt eine Situation vermeiden müssen, in der die gruppenbasierte Lizenzierung dazu führt, dass Benutzer ihre derzeit zugewiesenen Lizenzen vorübergehend nicht nutzen können. Jeder Vorgang, der zu Änderungen an zugewiesenen Lizenzen führt, muss vermieden werden, damit kein Risiko besteht, dass Benutzer keinen Zugriff mehr auf Dienste und ihre Daten haben.

## <a name="recommended-migration-process"></a>Empfohlener Migrationsprozess

1. Sie haben (z.B. mit PowerShell) die Verwaltung der Zuweisung und Entziehung von Lizenzen für Benutzer automatisiert. Lassen Sie diese Einrichtung unverändert.

2. Erstellen Sie eine neue Lizenzierungsgruppe (oder entscheiden Sie, welche vorhandene Gruppen genutzt werden soll), und stellen Sie sicher, dass alle erforderlichen Benutzer als Mitglieder hinzugefügt werden.

3. Weisen Sie die benötigten Lizenzen diesen Gruppen zu. Ziel muss es sein, denselben Lizenzierungszustand zu erreichen, den die bestehende Automatisierungslösung (z.B. PowerShell) diesen Benutzern zuweist.

4. Stellen Sie sicher, dass Lizenzen allen Benutzer in diesen Gruppen zugewiesen wurden. Überprüfen Sie hierzu den Verarbeitungsstatus der einzelnen Gruppen oder die Überwachungsprotokolle.

  - Sie können einzelne Benutzer stichprobenartig überprüfen, indem Sie sich ihre Lizenzdetails ansehen. Sie erkennen dann, ob ihnen dieselben Lizenzen entweder direkt oder über Gruppen zugewiesen wurden. Etwaige parallele Zuweisungspfade werden offenkundig, und die ursprüngliche direkte Zuweisung kann sicher aufgehoben werden.

  - Sie können ein PowerShell-Skript ausführen, um zu [überprüfen, wie Benutzern Lizenzen zugewiesen sind](active-directory-licensing-group-advanced.md).

  - Wenn die gleichen Produktlizenz dem Benutzer direkt und über eine Gruppe zugewiesen ist, wird vom Benutzer nur eine Lizenz genutzt. Daher sind keine zusätzlichen Lizenzen erforderlich, um die Migration auszuführen.

5. Vergewissern Sie sich, dass Lizenzzuweisungen nicht misslungen sind, indem Sie jede Gruppe auf Benutzer mit dem Status „Fehler“ überprüfen. Weitere Informationen finden Sie unter [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Erwägen Sie die Aufhebung direkter Zuweisungen. Dies kann gestaffelt erfolgen, um zunächst das Ergebnis für eine Teilmenge von Benutzern zu prüfen.

  Sie können die ursprünglichen direkten Zuweisungen für Benutzer intakt lassen. Doch wenn die Benutzer ihre lizenzierte Gruppe verlassen, behalten sie die ursprüngliche Lizenz, was Sie möglicherweise nicht möchten.

## <a name="an-example"></a>Beispiel

Wir sind eine Organisation mit 1.000 Benutzern. Alle Benutzer benötigen EMS-Lizenzen (Enterprise Mobility + Security). 200 Benutzer in der Finanzabteilung benötigen Office 365 Enterprise E3-Lizenzen. Wir haben ein lokal ausgeführtes PowerShell-Skript, mit dem Benutzern den Anforderungen entsprechend Lizenzen zugewiesen und entzogen werden. Wir möchten das Skript durch die gruppenbasierte Lizenzierung ersetzen, damit Lizenzen von Azure AD automatisch verwaltet werden.

Der Migrationsvorgang kann wie folgt aussehen:

1. Weisen Sie im Azure-Portal in Azure AD die EMS-Lizenz der Gruppe **Alle Benutzer** zu. Weisen Sie die E3-Lizenz der Gruppe **Finanzabteilung** zu, die alle erforderlichen Benutzer enthält.

2. Vergewissern Sie sich für jede Gruppe, dass die Lizenzzuweisung für alle Benutzer abgeschlossen wurde. Wechseln Sie zum Blatt jeder Gruppe, wählen Sie **Lizenzen** aus, und überprüfen Sie oben im Blatt **Lizenzen** den Verarbeitungsstatus.

  - Suchen Sie nach „Latest license changes have been applied to all users“ (Die letzten Lizenzänderungen wurden von allen Benutzern übernommen), um zu bestätigen, dass die Verarbeitung abgeschlossen ist.

  - Suchen Sie oben nach einer Benachrichtigung zu Benutzern, denen Lizenzen möglicherweise nicht erfolgreich zugewiesen wurden. Haben wir für einige Benutzer nicht genügend Lizenzen? Wurden einigen Benutzern in Konflikt stehende Lizenz-SKUs zugewiesen, die das Übernehmen der von der Gruppe zugewiesenen Lizenzen verhindern?

3. Überprüfen Sie stichprobenartig einige Benutzer, um zu prüfen, ob ihnen Lizenzen direkt oder per Gruppe zugewiesen wurden. Wechseln Sie zum Blatt eines Benutzers, wählen Sie **Lizenzen** aus, und untersuchen Sie den Status der Lizenzen.

  - Dies ist der erwartete Benutzerzustand während der Migration:

      ![erwarteter Benutzerzustand](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Dies bestätigt, dass der Benutzer sowohl über direkt zugewiesene als auch über von der Gruppe übernommene Lizenzen verfügt. Wir sehen, dass sowohl **EMS** als auch **E3** zugewiesen sind.

  - Wählen Sie die einzelnen Lizenzen aus, um Details zu den aktivierten Diensten einzublenden. Dies dient zum Überprüfen, ob die direkt oder per Gruppe zugewiesenen Lizenzen exakt dieselben Servicepläne für den Benutzer aktivieren.

      ![Überprüfen von Serviceplänen](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. Nachdem Sie sichergestellt haben, dass sowohl die direkt als auch die per Gruppe zugewiesenen Lizenzen äquivalent sind, können Sie beginnen, den Benutzern direkt zugewiesene Lizenzen zu entziehen. Sie können dies testen, indem Sie sie direkt im Portal entziehen und anschließend Automatisierungsskripts ausführen, um sie in einem Massenvorgang zu entziehen. Hier ist ein Beispiel des gleichen Benutzers, dem die direkt zugewiesenen Lizenzen mithilfe des Portals entzogen werden. Beachten Sie, dass der Lizenzstatus unverändert bleibt, aber keine direkten Zuweisungen mehr zu sehen sind.

  ![direkte Lizenzen entzogen](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die Lizenzverwaltung über Gruppen finden Sie in folgenden Artikeln.

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

