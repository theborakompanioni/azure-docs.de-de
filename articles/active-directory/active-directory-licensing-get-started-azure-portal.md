---

title: Erste Schritte mit der Lizenzierung in Azure Active Directory | Microsoft-Dokumentation
description: "Beschreibung der Azure Active Directory-Lizenzierung, der Funktionsweise, der ersten Schritte und der bewährten Methoden – einschließlich Office 365, Microsoft Intune und Azure Active Directory Premium und Basic"
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
ms.date: 05/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 8e9d6900910f45b5f17600fc5608191871c5ab21
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---

# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Lizenzieren Sie sich selbst und Ihre Benutzer in Azure Active Directory

> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-licensing-get-started-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) ist die IDaaS-Lösung (Identity as a Service) und -Plattform von Microsoft. Azure AD wird in einer Reihe von funktionalen und technischen Versionen angeboten. Diese reichen von Azure AD Free, das mit anderen Microsoft-Diensten wie Office 365, Dynamics, Microsoft Intune und Azure (Azure AD generiert keine Nutzungsgebühren in diesem Modus) verfügbar ist, bis hin zu kostenpflichtigen Versionen von Azure AD wie Enterprise Mobility Suite (EMS), Azure AD Premium (P1 und P2) und Azure AD Basic sowie Azure Multi-Factor Authentication (MFA). Wie bei vielen der Microsoft-Onlinedienste werden die meisten kostenpflichtigen Versionen von Azure AD über Berechtigungen pro Benutzer bereitgestellt, wie auch in Office 365, Microsoft Intune und Azure AD. In diesen Fällen entspricht der Erwerb des Diensts einem oder mehreren Abonnements, und jedes Abonnement umfasst eine im Voraus bezahlte Anzahl von Lizenzen in Ihrem Mandanten. Berechtigungen pro Benutzer werden durch Lizenzzuweisung erreicht und erstellen eine Verknüpfung zwischen dem Benutzer und dem Produkt, sodass die Komponenten des Diensts für den Benutzer aktiviert werden und eine der vorausbezahlten Lizenzen nutzt wird.

[Testen Sie Azure AD Premium jetzt.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Einen umfassenden Überblick über die Azure AD-Dienstfunktionen finden Sie unter [Was ist Azure AD?](https://azure.microsoft.com/en-us/documentation/articles/active-directory-whatis/). Weitere Informationen finden Sie auf unserer Seite zu Vereinbarungen zum Servicelevel.

> [!NOTE]
> Nutzungsbasierte Abonnements von Azure sind anders: Sie werden zwar auch in Ihrem Verzeichnis dargestellt, diese Abonnements ermöglichen jedoch das Erstellen von Azure-Ressourcen und deren Abstimmung auf Ihre Zahlungsweise. In diesem Fall ist KEINE Lizenzanzahl mit dem Abonnement verknüpft. Die Zuordnung von Benutzern zum Abonnement und der Benutzerzugriff zum Verwalten von Abonnementressourcen erfolgt durch die Erteilung von Berechtigungen zum Verarbeiten von Azure-Ressourcen, die dem Abonnement zugeordnet sind.

## <a name="how-does-azure-ad-licensing-work"></a>Wie funktioniert die Lizenzierung von Azure AD?

Bei lizenzbasierten Azure AD-Diensten wird ein Abonnement im Azure AD-Verzeichnis-/Dienstmandanten aktiviert. Nach Aktivierung des Abonnements können Dienstfunktionen von Verzeichnis-/Dienstadministratoren verwaltet und von lizenzierten Benutzern verwendet werden.

Wenn Sie Enterprise Mobility Suite, Azure AD Premium oder Azure AD Basic erwerben oder aktivieren, wird das Verzeichnis mit dem Abonnement aktualisiert, einschließlich der Gültigkeitsdauer und der vorausbezahlten Lizenzen. Ihre Abonnementinformationen (einschließlich der Anzahl zugewiesener oder verfügbarer Lizenzen) stehen über das Azure-Portal unter „Azure Active Directory“ &gt; **Lizenzen** für das jeweilige Verzeichnis zur Verfügung. Dies ist auch der beste Ort für die Verwaltung Ihrer Lizenzzuweisungen.

Jedes Abonnement umfasst einen oder mehrere Dienstpläne, die jeweils die enthaltene Funktionsebene des Diensttyps abbilden, beispielsweise Azure AD, Azure MFA, Microsoft Intune, Exchange Online und SharePoint Online.  Für die Azure AD-Lizenzverwaltung ist keine Verwaltung auf Dienstplanebene erforderlich. Dies ist ein Unterschied zu Office 365, für das dieser erweiterte Konfigurationsmodus zum Verwalten des Zugriffs auf die enthaltenen Dienste benötigt wird. Bei Azure AD erfolgt die Konfiguration im Dienst, um Funktionen zu aktivieren und einzelne Berechtigungen zu verwalten.

> [!IMPORTANT]
> Azure AD Premium- und Basic-Abonnements sowie Enterprise Mobility Suite-Abonnements sind auf ihre bereitgestellten Verzeichnisse/Mandanten beschränkt. Abonnements können nicht zwischen Verzeichnissen aufgeteilt oder verwendet werden, um Benutzern Berechtigungen in anderen Verzeichnissen zu erteilen. Das Verschieben eines Abonnements zwischen Verzeichnissen ist möglich, erfordert jedoch das Einreichen eines Supporttickets oder im Fall von direkten Käufen die Kündigung und den erneuten Erwerb.
>
> Beim Kauf von Azure AD oder Enterprise Mobility Suite über ein Abonnement mit Volumenlizenzierung erfolgt die Aktivierung automatisch, wenn die Vereinbarung andere Microsoft Online-Dienste wie etwa Office 365 enthält.

### <a name="assigning-licenses"></a>Zuweisen von Lizenzen

Zwar ist nur der Erwerb eines Abonnements erforderlich, um kostenpflichtige Funktionen zu konfigurieren, die Verwendung Ihrer kostenpflichtigen Azure AD-Funktionen erfordert jedoch die Verteilung von Lizenzen an die richtigen Personen. Im Allgemeinen muss allen Benutzern, die Zugriff haben sollen oder die über kostenpflichtige Azure AD-Funktionen verwaltet werden, eine Lizenz zugewiesen werden. Die Lizenzzuweisung ist eine Zuordnung zwischen einem Benutzer und einem erworbenen Dienst wie Azure AD Premium, Basic oder Enterprise Mobility Suite.

Es ist einfach zu verwalten, welche Benutzer im Verzeichnis eine Lizenz erhalten sollen. Hierzu können Sie Lizenzen entweder Gruppen im Azure-Portal zuweisen (dieses Feature befindet sich in der öffentlichen Vorschau) oder sie über das Portal, mithilfe von PowerShell oder über APIs direkt, den richtigen Personen zuweisen. Wenn einer Gruppe Lizenzen zugewiesen werden, wird allen Mitgliedern der Gruppe eine Lizenz zugewiesen. Wenn Benutzer der Gruppe hinzugefügt oder aus der Gruppe entfernt werden, werden sie der entsprechenden Lizenz zugewiesen oder davon entfernt. Für die Gruppenzuweisung kann jede verfügbare Gruppenverwaltung genutzt werden, die Ihnen zur Verfügung steht und die mit der gruppenbasierten Zuweisung zu Anwendungen konsistent ist. Durch diesen Ansatz können Sie Regeln so einrichten, dass alle Benutzer in Ihrem Verzeichnis automatisch zugewiesen werden, dass sichergestellt ist, dass jeder in der entsprechenden Position über eine Lizenz verfügt, oder dass die Entscheidung an andere Manager in der Organisation delegiert wird. 

Ausführliche Informationen zur Lizenzzuweisung zu Gruppen (einschließlich erweiterter Szenarien und Office 365-Lizenzierungsszenarien) finden Sie in [diesem Artikel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-assignment-azure-portal).

## <a name="getting-started-with-azure-ad-licensing"></a>Erste Schritte mit der Azure AD-Lizenzierung

Die ersten Schritte mit Azure AD sind einfach. Sie können Ihr Verzeichnis im Rahmen der Registrierung für eine kostenlose Testversion von Azure erstellen. [Erfahren Sie mehr über die Anmeldung als Organisation](https://azure.microsoft.com/en-us/documentation/articles/sign-up-organization/). Mit Folgendem können Sie sicherstellen, dass das Verzeichnis optimal auf andere Microsoft-Dienste, die Sie möglicherweise nutzen oder zu nutzen planen, und auf Ihre Ziele beim Erwerb des Diensts abgestimmt ist.

Dies sind einige bewährte Methoden:

- Wenn Sie bereits Organisationsdienste von Microsoft verwenden, verfügen Sie schon über ein Azure AD-Verzeichnis. In diesem Fall sollten Sie das gleiche Verzeichnis für andere Dienste verwenden, damit die zentrale Identitätsverwaltung (einschließlich Bereitstellung und Hybrid-SSO (Single Sign-On; einmaliges Anmelden)) von den Diensten genutzt werden kann. Ihre Benutzer müssen sich nur einmal anmelden und können umfangreichere Funktionen der Dienste nutzen. Wenn Sie also einen kostenpflichtigen Azure AD-Dienst für Ihre Mitarbeiter erwerben möchten, empfiehlt es sich, dafür das gleiche Verzeichnis zu verwenden.

- Falls Sie Azure AD für andere Benutzer (Partner, Kunden usw.) verwenden, Azure AD-Dienste isoliert vom Produktionsdienst evaluieren oder eine Sandbox-Umgebung für Ihre Dienste einrichten möchten, empfiehlt es sich, zunächst über das klassische Azure-Portal ein neues Verzeichnis zu erstellen. Informieren Sie sich ausführlicher über das Erstellen eines neuen Azure AD-Verzeichnisses im klassischen Azure-Portal. Das neue Verzeichnis wird mit Ihrem Konto als externer Benutzer mit globalen Administratorberechtigungen erstellt. Wenn Sie sich mit diesem Konto beim Azure-Portal anmelden, wird dieses Verzeichnis angezeigt, und Sie haben Zugriff auf alle Verwaltungsaufgaben für das Verzeichnis.

> [!NOTE]
> Azure AD unterstützt "externe Benutzer". Dies sind Benutzerkonten in einer Instanz von Azure AD, die mit einem Microsoft-Konto (MSA) oder einer Azure AD-Identität aus einem anderen Verzeichnis erstellt wurden. Während diese Funktion in alle Organisationsdienste von Microsoft implementiert werden soll, werden diese Konten noch nicht in allen Diensten unterstützt. Beispielsweise unterstützt das Office 365-Verwaltungsportal derzeit diese Benutzer nicht. Daher können externe Benutzer mit Microsoft-Konten nicht auf das Office 365-Verwaltungsportal zugreifen, während externe Benutzer aus anderen Azure AD-Verzeichnissen ignoriert werden. Im letzteren Fall kann über diese Umgebungen nur auf das lokale Konto des Benutzers (das Azure AD- oder Office 365-Verzeichnis, in dem der Benutzer ursprünglich erstellt wurde) zugegriffen werden.

Wie erwähnt, gibt es für Azure AD verschiedene kostenpflichtige Versionen. Diese Versionen weisen geringfügige Unterschiede in ihrer Kaufverfügbarkeit auf:

|  Produkt       |              EA/VL  | Öffnen  | CSP |  Nutzungsrechte für MPN  | Direktkauf |  Testversion |
|  -------------- | ------------- | ------- | ------ | ----- | ---------------- | ----------------- | ------- |
|  Enterprise Mobility Suite  | X   |    X  |    X  |     X | &nbsp;  | X |
|  Azure AD Premium P2     |    X    |   X   |   X   | &nbsp;  |  X  |   X  
|  Azure AD Premium P1     |    X   |    X    |  X   |  &nbsp; |  X  |  &nbsp; |             
|  Azure AD Basic          |    X   |    X   |  X  |  X  | &nbsp; | &nbsp;  |

### <a name="select-one-or-more-license-trials"></a>Auswählen von mindestens einer Lizenztestversion

Unter „Azure Active Directory“ &gt; **Schnellstart** können Sie ein Testabonnement für Azure AD Premium oder Enterprise Mobility Suite aktivieren.

![Auswählen einer Testlizenz](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Die Testlizenzen stehen nun auf dem Blatt **Lizenzen** zur Verfügung.

### <a name="assign-licenses-to-users-and-groups"></a>Zuweisen von Lizenzen zu Benutzern und Gruppen

Sobald das Abonnement aktiv ist, sollten Sie sich selbst eine Lizenz zuweisen und den Browser aktualisieren, um sicherzustellen, dass alle Funktionen angezeigt werden. Der nächste Schritt ist, den Benutzern Lizenzen zuzuweisen, die auf kostenpflichtige Azure AD-Funktionen zugreifen oder darin enthalten sein müssen. Wie bereits weiter oben unter [Zuweisen von Lizenzen](#assigning-licenses) erwähnt, ermitteln Sie hierzu am besten die Gruppe, die die gewünschte Zielgruppe darstellt, und weisen ihr die Lizenz zu. Auf diese Weise wird Benutzern, die im Laufe des Lebenszyklus der Gruppe hinzugefügt oder daraus entfernt werden, die Lizenz zugewiesen bzw. entzogen.

> [!NOTE]
> Einige Microsoft-Dienste sind nicht in an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator daher für den Benutzer die Eigenschaft „Verwendungsstandort“ angeben. Dies ist im Azure-Portal unter „Benutzer“ &gt; „Profil“ &gt; „Einstellungen“ möglich. Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses.

Wenn Sie einer Gruppe oder einzelnen Benutzern eine Lizenz zuweisen möchten, wählen Sie unter „Azure Active Directory“ &gt; „Lizenzen“ &gt; „Alle Produkte“ mindestens ein Produkt aus, und klicken Sie auf der Befehlsleiste auf die Schaltfläche **Zuweisen**.

![Auswählen einer zuzuweisenden Lizenz](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Daraufhin wird ein neues Blatt geöffnet, auf dem Sie mehrere Benutzer oder eine Gruppe auswählen und optional Dienstpläne innerhalb des Produkts deaktivieren können. Über die Suchleiste am oberen Rand können Sie nach Benutzer- und Gruppennamen suchen.

![Auswählen eines Benutzers oder einer Gruppe für die Lizenzzuweisung](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Wenn Sie der Gruppe eine Lizenz zuweisen, dauert es je nach Anzahl der Benutzer in der Gruppe unter Umständen eine Weile, bis alle Benutzer die Lizenz geerbt haben. Der Verarbeitungsstatus wird auf dem Gruppenblatt unter der Kachel **Lizenzen** angezeigt.

![Lizenzzuweisungsstatus](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Bei der Azure AD-Lizenzzuweisung können zwar Zuweisungsfehler auftreten, dies ist bei der Verwaltung von Azure AD- und EMS-Produkten allerdings eher selten. Potenzielle Zuweisungsfehler sind beschränkt auf:
- Zuweisungskonflikte: Treten auf, wenn einem Benutzer zuvor eine Lizenz zugewiesen war, die nicht mit der aktuellen Lizenz kompatibel ist. In diesem Fall muss die aktuelle Lizenz entfernt werden, um die neue Lizenz zuweisen zu können.
- Überschreitung der Anzahl verfügbarer Lizenzen: Wenn die Anzahl von Benutzern in zugewiesenen Gruppen die Anzahl verfügbarer Lizenzen überschreitet, wird für den Zuweisungsstatus der Benutzer ein Zuweisungsfehler aufgrund fehlender Lizenzen angegeben.

Ausführliche Informationen zum Zuweisen von Gruppenlizenzen finden Sie in diesem Artikel.

### <a name="view-assigned-licenses"></a>Anzeigen zugewiesener Lizenzen

Eine Übersicht über zugewiesene und verfügbare Lizenzen finden Sie unter „Azure Active Directory“ &gt; **Lizenzen** &gt; **Alle Produkte**.

![Anzeigen der Lizenzzusammenfassung](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Eine detaillierte Liste mit zugewiesenen Benutzern und Gruppen erhalten Sie, indem Sie auf ein bestimmtes Produkt klicken. **Lizenzierte Benutzer** zeigt alle Benutzer, die derzeit eine Lizenz nutzen. Hier sehen Sie auch, ob die Lizenz dem Benutzer direkt zugewiesen wurde oder ob er sie von einer Gruppe geerbt hat.

![Anzeigen der Lizenzdetails](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Analog dazu finden Sie unter **Lizenzierte Gruppen** alle Gruppen, denen Lizenzen zugewiesen wurden. Wenn Sie in diesen Ansichten auf einen Benutzer oder auf eine Gruppe klicken, wird das Blatt **Lizenzen** mit allen Lizenzen geöffnet, die dem entsprechenden Objekt zugewiesen sind.

### <a name="removing-a-license"></a>Entfernen einer Lizenz

Navigieren Sie zum Entfernen einer Lizenz zum gewünschten Benutzer oder zur gewünschten Gruppe, und öffnen Sie die Kachel **Lizenzen**. Wählen Sie die Lizenz aus, und klicken Sie auf **Entfernen**.

![Entfernen einer Lizenz](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Lizenzen, die der Benutzer von einer Gruppe geerbt hat, können nicht direkt entfernt werden. Entfernen Sie stattdessen den Benutzer aus der Gruppe, von der er die Lizenz erbt.

### <a name="extending-trials"></a>Erweitern von Testversionen

Erweiterungen von Testversionen für Kunden sind als Self-Service über das Office 365-Portal verfügbar. Ein Kundenadministrator kann zum Office-Portal navigieren (der Zugriff hängt von Berechtigungen für das Office-Portal ab) und die Azure AD Premium-Testversion auswählen. Der Verlängerungsprozess wird durch Klicken auf den Link **Testzeitraum verlängern** gestartet. Eine Kreditkarte ist zwar erforderlich, sie wird jedoch nicht belastet.

![Verlängern eines Testzeitraums über das Azure-Portal](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu erweiterten Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie in diesem Artikel.

Jetzt sind Sie möglicherweise bereit, einige Funktionen von Azure AD Premium zu konfigurieren und zu verwenden.

* [Self-Service-Kennwortzurücksetzung](active-directory-manage-passwords.md)
* [Self-Service-Gruppenverwaltung](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect-Zustand](active-directory-aadconnect-health.md)
* [Gruppenzuweisung zu Anwendungen](active-directory-manage-groups.md)
* [Zuweisen von Lizenzen zu einer Gruppe](active-directory-licensing-group-assignment-azure-portal.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Direkter Erwerb von Azure AD Premium-Lizenzen](http://aka.ms/buyaadp)

