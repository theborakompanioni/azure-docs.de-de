---
title: Erste Schritte mit der Lizenzierung in Azure Active Directory | Microsoft-Dokumentation
description: "Wie die Azure Active Directory-Lizenzierung funktioniert, und wie Sie mit bewährten Methoden beginnen."
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
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: b62571e446aa0680c653d0a9d109207af26ad786
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Lizenzieren Sie sich selbst und Ihre Benutzer in Azure Active Directory

> [!div class="op_single_selector"]
> * [Azure-Portal-Anweisungen](active-directory-licensing-get-started-azure-portal.md)
> * [Informationen zum klassischen Azure-Portal abrufen](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) ist eine IDaaS-Lösung (Identity as a Service) und Plattform von Microsoft. Azure AD wird in verschiedenen Diensteditionen angeboten:

* Azure Active Directory Free, die mit einem beliebigen Microsoft-Dienst wie Office 365, Dynamics, Microsoft Intune oder Azure verfügbar ist. Azure AD berechnet in diesem Modus keine Verbrauchsgebühren.

* Bezahlte Azure AD-Editionen, z.B.:
  - Enterprise Mobility + Security 
  - Azure AD Premium (P1 und P2)
  - Azure AD Basic
  - Azure Multi-Factor Authentication

Wie bei vielen der Microsoft-Onlinedienste werden die meisten kostenpflichtigen Versionen von Azure AD über Berechtigungen pro Benutzer bereitgestellt, wie auch in Office 365, Microsoft Intune und Azure AD. In diesen Fällen entspricht der Erwerb des Diensts einem oder mehreren Abonnements, und jedes Abonnement umfasst einige im Voraus bezahlte Lizenzen in Ihrem Mandanten. Benutzerspezifische Berechtigungen werden erreicht durch:

* Zuweisen einer Lizenz. 
* Erstellen einer Verknüpfung zwischen Benutzer und Produkt.
* Aktivieren der Dienstkomponenten für den Benutzer.
* Verwenden einer der im Voraus bezahlten Lizenzen.

[Testen Sie Azure AD Premium jetzt.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Einen umfassenden Überblick über die Azure AD-Dienstfunktionen finden Sie unter [Was ist Azure AD?](active-directory-whatis.md). Weitere Informationen finden Sie auf unserer [Seite zu Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Nutzungsbasierte Azure-Abonnements ermöglichen das Erstellen von Azure-Ressourcen und deren Zuordnung zu Ihrer Zahlungsmethode. Mit dem Abonnement ist keine Lizenzanzahl verknüpft. Wenn Sie einem Benutzer die Berechtigung erteilen, mit Azure-Ressourcen zu arbeiten, die dem Abonnement zugeordnet sind, wird er dem Abonnement zugeordnet und kann Abonnementressourcen verwalten.

## <a name="how-does-azure-active-directory-licensing-work"></a>Wie funktioniert die Azure Active Directory-Lizenzierung?

Bei lizenzbasierten Azure AD-Diensten wird ein Abonnement im Azure AD-Dienstmandanten aktiviert. Nach Aktivierung des Abonnements werden die Dienstfunktionen von Azure AD-Administratoren verwaltet und von lizenzierten Benutzern verwendet.

### <a name="manage-subscription-information"></a>Verwalten der Abonnementinformationen

Wenn Sie Enterprise Mobility + Security, Azure AD Premium oder Azure AD Basic erwerben, wird Ihr Mandant mit dem Abonnement aktualisiert, einschließlich der Gültigkeitsdauer und der vorausbezahlten Lizenzen. Ihre Abonnementinformationen (einschließlich der Anzahl zugewiesener oder verfügbarer Lizenzen) stehen über das Azure-Portal zur Verfügung: Öffnen Sie unter **Azure Active Directory** die Kachel **Lizenzen** für das jeweilige Verzeichnis. Das Blatt **Lizenzen** ist auch der beste Ort für die Verwaltung Ihrer Lizenzzuweisungen.

Jedes Abonnement besteht aus mindestens einem Serviceplan, wie z.B. Azure AD, Multi-Factor Authentication, Intune, Exchange Online oder SharePoint Online.  Für die Azure AD-Lizenzverwaltung ist *keine* Verwaltung auf Serviceplanebene erforderlich. Office 365 ist anders, da hier zum Verwalten des Zugriffs auf die enthaltenen Dienste dieser erweiterte Konfigurationsmodus benötigt wird. Bei Azure AD erfolgt die Konfiguration im Dienst, um Features zu aktivieren und einzelne Berechtigungen zu verwalten.

> [!IMPORTANT]
> Azure AD Premium-, Azure AD Basic- sowie Enterprise Mobility + Security-Abonnements sind auf ihre bereitgestellten Verzeichnisse/Mandanten beschränkt. Abonnements können nicht zwischen Verzeichnissen aufgeteilt oder verwendet werden, um Benutzern Berechtigungen in anderen Verzeichnissen zu erteilen. Das Verschieben eines Abonnements zwischen Verzeichnissen ist möglich, erfordert jedoch das Einreichen eines Supporttickets oder im Fall von direkten Käufen die Kündigung und den erneuten Erwerb.
>
> Beim Kauf von Azure AD oder Enterprise Mobility + Security über ein Abonnement mit Volumenlizenzierung, und wenn die Vereinbarung andere Microsoft Online-Dienste (z.B. Office 365) enthält, erfolgt die Aktivierung automatisch. 

### <a name="assign-licenses"></a>Zuweisen von Lizenzen

Zwar ist nur der Erwerb eines Abonnements erforderlich, um kostenpflichtige Funktionen zu konfigurieren, doch Sie müssen Lizenzen für kostenpflichtige Azure AD-Features an Benutzer verteilen. Allen Benutzern, die Zugriff haben sollen oder über kostenpflichtige Azure AD-Features verwaltet werden, muss eine Lizenz zugewiesen werden. Eine Lizenzzuweisung ist eine Zuordnung zwischen einem Benutzer und einem erworbenen Dienst wie Azure AD Premium, Basic oder Enterprise Mobility + Security.


Welche Benutzer im Verzeichnis eine Lizenz erhalten sollen, können Sie verwalten durch: 

* Zuweisen von Lizenzen zu Gruppen im [Azure-Portal](active-directory-licensing-whatis-azure-portal.md).
* Direktes Zuweisen von Lizenzen für Benutzer über Portal, PowerShell oder APIs. 

Wenn Sie einer Gruppe Lizenzen zuweisen, wird allen Mitgliedern der Gruppe eine Lizenz zugewiesen. Wenn Benutzer der Gruppe hinzugefügt oder aus der Gruppe entfernt werden, wird die entsprechende Lizenz zugewiesen oder entfernt. Für die Gruppenzuweisung kann jede verfügbare Gruppenverwaltung genutzt werden, die Ihnen zur Verfügung steht, und sie ist mit der gruppenbasierten Zuweisung zu Anwendungen konsistent.

Mit [gruppenbasierter Lizenzzuweisung](active-directory-licensing-whatis-azure-portal.md) können Sie Regeln wie die Folgenden einrichten:
* Alle Benutzer in Ihrem Verzeichnis erhalten automatisch eine Lizenz.
* Alle Benutzer mit der entsprechenden Position erhalten eine Lizenz.
* Sie können die Entscheidung an andere Manager in der Organisation (mit [Self-Service-Gruppen](active-directory-accessmanagement-self-service-group-management.md)) delegieren.

Ausführliche Informationen zur Lizenzzuweisung zu Gruppen (einschließlich erweiterter Szenarien und Office 365-Lizenzierungsszenarien) finden Sie unter [Zuweisen von Lizenzen zu einer Gruppe von Benutzern in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Erste Schritte mit der Azure AD-Lizenzierung

Die ersten Schritte mit Azure AD sind leicht. Sie können immer ein Verzeichnis im Rahmen der Anmeldung für eine [kostenlose Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/) erstellen.

Mithilfe der folgenden bewährten Methoden können Sie sicherstellen, dass Ihr Mandant auf andere Microsoft-Dienste, die Sie verwenden könnten, und Ihre Ziele für den Dienst ausgerichtet ist:

- Wenn Sie bereits Organisationsdienste von Microsoft verwenden, verfügen Sie schon über einen Azure AD-Mandanten. Sie sollten den gleichen Mandanten für andere Dienste verwenden, damit die zentrale Identitätsverwaltung (einschließlich Bereitstellung und Hybrid-SSO (Single Sign-On; einmaliges Anmelden)) von allen Diensten genutzt werden kann. Mit einmaligem Anmelden profitieren Ihre Benutzer dienstübergreifend von den umfassenden Funktionen. Wenn Sie also einen kostenpflichtigen Azure AD-Dienst für Ihre Mitarbeiter erwerben möchten, sollten Sie den gleichen Mandanten erneut verwenden.

- Wenn Sie Folgendes planen, sollten Sie einen neuen Mandanten im Azure-Portal verwenden:
  - Verwenden von Azure AD für eine andere Gruppe von Benutzern (z.B. Partner oder Kunden).
  - Von Ihrem Produktionsdienst unabhängiges Auswerten der Azure AD-Dienste.
  - Einrichten einer Sandkastenumgebung für Ihre Dienste.

  Das neue Verzeichnis wird mit Ihrem Konto als externer Benutzer mit globalen Administratorberechtigungen erstellt. Wenn Sie sich mit diesem Konto beim Azure-Portal anmelden, wird dieser Mandant angezeigt, und Sie haben Zugriff auf alle Verwaltungsaufgaben.

> [!NOTE]
> Azure AD unterstützt „Gastbenutzer“. Dies sind Benutzerkonten in einem Azure AD-Mandanten, die mit einem Microsoft-Konto oder einer Azure AD-Identität aus einem anderen Mandanten erstellt wurden. Das Office 365-Verwaltungsportal unterstützt dies Benutzer derzeit nicht. Gastbenutzer mit Microsoft-Konten können überhaupt nicht auf das Office 365-Verwaltungsportal zugreifen, während Gastbenutzer aus anderen Azure AD-Mandanten ignoriert werden. Im letzteren Fall kann nur auf das lokale Konto des Benutzers im Azure AD- oder Office 365-Mandanten, wo der Benutzer ursprünglich erstellt wurde, zugegriffen werden.

### <a name="select-one-or-more-license-trials"></a>Auswählen von mindestens einer Lizenztestversion

Unter **Azure Active Directory** &gt; **Schnellstart** können Sie ein Testabonnement für Azure AD Premium oder Enterprise Mobility + Security aktivieren.

![Auswählen einer Testlizenz](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Testlizenzen stehen auf dem Blatt **Lizenzen** zur Verfügung.

### <a name="assign-licenses-to-users-and-groups"></a>Zuweisen von Lizenzen zu Benutzern und Gruppen

Sobald das Abonnement aktiv ist, sollten Sie sich selbst eine Lizenz zuweisen. Aktualisieren Sie dann den Browser, um sicherzustellen, dass Ihnen alle Features angezeigt werden. Im nächsten Schritt weisen Sie den Benutzern Lizenzen zu, die auf kostenpflichtige Azure AD-Features zugreifen müssen. Wie in [Zuweisen von Lizenzen](#assign-licenses) beschrieben, ist eine einfache Möglichkeit zum Zuweisen von Lizenzen, die Gruppe zu identifizieren, die die gewünschte Zielgruppe darstellt, und ihr die Lizenz zuzuweisen. So werden Benutzer, die während des Lebenszyklus der Gruppe dieser hinzugefügt oder aus ihr entfernt werden, der Lizenz zugewiesen bzw. von ihr entfernt.

> [!NOTE]
> Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Verwendungsstandort** für den Benutzer angeben. Sie können diese Eigenschaft im Azure-Portal unter **Benutzer** &gt; **Profil** &gt; **Einstellungen** festlegen. Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer, deren Verwendungsstandort nicht angegeben ist, den Standort des Verzeichnisses.

Um eine Lizenz zuzuweisen, wählen Sie unter **Azure Active Directory** &gt; **Lizenzen** &gt; **Alle Produkte** mindestens ein Produkt aus, und klicken Sie auf der Befehlsleiste auf **Zuweisen**.

![Auswählen einer zuzuweisenden Lizenz](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Auf dem Blatt **Benutzer und Gruppen** können Sie mehrere Benutzer oder Gruppen auswählen bzw. Servicepläne im Produkt deaktivieren. Über das Suchfeld am oberen Rand können Sie nach Benutzer- und Gruppennamen suchen.

![Auswählen eines Benutzers oder einer Gruppe für die Lizenzzuweisung](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Wenn Sie der Gruppe eine Lizenz zuweisen, dauert es je nach Anzahl der Benutzer in der Gruppe unter Umständen eine Weile, bis alle Benutzer die Lizenz geerbt haben. Sie können den Verarbeitungsstatus auf dem Blatt **Gruppe** auf der Kachel **Lizenzen** verfolgen.

![Lizenzzuweisungsstatus](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Bei der Azure AD-Lizenzzuweisung können zwar Zuweisungsfehler auftreten, dies ist bei der Verwaltung von Azure AD- und Enterprise Mobility + Security-Produkten allerdings eher selten. Potenzielle Zuweisungsfehler sind beschränkt auf:
- Zuweisungskonflikte: Treten auf, wenn einem Benutzer zuvor eine Lizenz zugewiesen war, die nicht mit der aktuellen Lizenz kompatibel ist. In diesem Fall muss die aktuelle Lizenz entfernt werden, um die neue Lizenz zuweisen zu können.
- Überschreitung der verfügbaren Lizenzanzahl: Wenn die Anzahl der Benutzer in zugewiesenen Gruppen die Anzahl der verfügbaren Lizenzen überschreitet, gibt der Zuweisungsstatus für den Benutzer einen Zuweisungsfehler aufgrund fehlender Lizenzen an.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Lizenzierung von Azure AD B2B-Kollaboration

Mit B2B-Kollaboration können Sie Gastbenutzer in Ihren Azure AD-Mandanten einladen, um den Zugriff auf Azure AD-Dienste und beliebige Azure-Ressourcen, die Sie verfügbar machen, zu ermöglichen.  

Es fallen Kosten für das Einladen von B2B-Benutzern und das Zuweisen zu einer Anwendung in Azure AD an. Bis zu 10 Apps pro Gastbenutzer und 3 einfache Berichte sind für B2B-Kollaborationsbenutzer auch kostenlos. Wenn Ihrem Gastbenutzer entsprechende Lizenzen im Azure AD-Mandanten des Partners zugewiesen wurden, wird er auch in Ihrem lizenziert.

Es ist nicht erforderlich, aber falls Sie den Zugriff auf kostenpflichtige Azure AD-Features ermöglichen möchten, müssen diese B2B-Gastbenutzer mit entsprechenden Azure AD-Lizenzen lizenziert werden. Ein einladender Mandant mit einer kostenpflichtigen Azure AD-Lizenz kann zusätzlichen fünf, zu dem Mandanten eingeladenen Gastbenutzern B2B-Kollaborationsbenutzerrechte zuweisen. Weitere Informationen und Szenarien finden Sie unter [Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Anzeigen zugewiesener Lizenzen

Eine Übersicht über zugewiesene und verfügbare Lizenzen finden Sie unter **Azure Active Directory** &gt; **Lizenzen** &gt; **Alle Produkte**.

![Anzeigen der Lizenzzusammenfassung](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Eine detaillierte Liste mit zugewiesenen Benutzern und Gruppen erhalten Sie, indem Sie ein bestimmtes Produkt auswählen. Die Liste **Lizenzierte Benutzer** zeigt alle Benutzer, die derzeit eine Lizenz nutzen. Hier sehen Sie auch, ob die Lizenz dem Benutzer direkt zugewiesen wurde, oder ob er sie von einer Gruppe geerbt hat.

![Anzeigen der Lizenzdetails](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Analog dazu finden Sie unter **Lizenzierte Gruppen** eine Liste aller Gruppen, denen Lizenzen zugewiesen wurden. Wählen Sie einen Benutzer oder eine Gruppe aus, um das Blatt **Lizenzen** zu öffnen, das alle Lizenzen anzeigt, die dem jeweiligen Objekt zugewiesen wurden.

### <a name="remove-a-license"></a>Entfernen einer Lizenz

Navigieren Sie zum Entfernen einer Lizenz zum gewünschten Benutzer oder zur gewünschten Gruppe, und öffnen Sie die Kachel **Lizenzen**. Wählen Sie die Lizenz aus, und klicken Sie auf **Entfernen**.

![Entfernen einer Lizenz](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

Lizenzen, die der Benutzer von einer Gruppe geerbt hat, können nicht direkt entfernt werden. Entfernen Sie stattdessen den Benutzer aus der Gruppe, von der er die Lizenz erbt.

### <a name="extend-trials"></a>Erweitern von Testzeiträumen

Erweiterungen von Testversionen für Kunden sind als Self-Service-Registrierung über das Office 365-Portal verfügbar. Ein Kundenadministrator kann zum Office-Portal navigieren (der Zugriff hängt von Berechtigungen für das Office-Portal ab) und die Azure AD Premium-Testversion auswählen. Der Verlängerungsprozess wird durch Klicken auf den Link **Testzeitraum verlängern** gestartet. Eine Kreditkarte ist zwar erforderlich, sie wird jedoch nicht belastet.

![Erweitern eines Testzeitraums über das Azure-Portal](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu erweiterten Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie im Artikel [Zuweisen von Lizenzen zu einer Gruppe von Benutzern in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

Hier finden Sie Informationen zum Konfigurieren und Verwenden anderer kostenpflichtiger Azure AD-Features:

* [Self-Service-Kennwortzurücksetzung](active-directory-manage-passwords.md)
* [Self-Service-Gruppenverwaltung](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Direkter Erwerb von Azure AD Premium-Lizenzen](http://aka.ms/buyaadp)

