---
title: Lizenzbenutzer in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie sich selbst und Ihre Benutzer in Azure Active Directory lizenzieren.
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: c4509cdb003687083d0456c1957b19cf35ee056a
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Schnellstart: Lizenzbenutzer in Azure Active Directory
Bei lizenzbasierten Azure AD-Diensten wird ein Azure Active Directory-Abonnement (Azure AD) in Ihrem Azure-Mandanten aktiviert. Nach Aktivierung des Abonnements werden Dienstfunktionen von Azure AD-Administratoren verwaltet und von lizenzierten Benutzern verwendet. Wenn Sie Enterprise Mobility + Security, Azure AD Premium oder Azure AD Basic erwerben, wird Ihr Mandant mit dem Abonnement aktualisiert, einschließlich der Gültigkeitsdauer und der vorausbezahlten Lizenzen. Ihre Abonnementinformationen (einschließlich der Anzahl zugewiesener oder verfügbarer Lizenzen) stehen über das Azure-Portal unter **Azure Active Directory** zur Verfügung, indem Sie die Kachel **Lizenzen** öffnen. Das Blatt **Lizenzen** ist auch der beste Ort für die Verwaltung Ihrer Lizenzzuweisungen.

Zwar ist nur der Erwerb eines Abonnements erforderlich, um kostenpflichtige Funktionen zu konfigurieren, doch Sie müssen Benutzerlizenzen für kostenpflichtige Azure AD-Features zuweisen. Allen Benutzern, die Zugriff haben sollen oder über kostenpflichtige Azure AD-Features verwaltet werden, muss eine Lizenz zugewiesen werden. Eine Lizenzzuweisung ist eine Zuordnung zwischen einem Benutzer und einem erworbenen Dienst wie Azure AD Premium, Basic oder Enterprise Mobility + Security.

Mit [gruppenbasierter Lizenzzuweisung](active-directory-licensing-whatis-azure-portal.md) können Sie Regeln wie die Folgenden einrichten:
* Alle Benutzer in Ihrem Verzeichnis erhalten automatisch eine Lizenz.
* Alle Benutzer mit der entsprechenden Position erhalten eine Lizenz.
* Sie können die Entscheidung an andere Manager in der Organisation (mit [Self-Service-Gruppen](active-directory-accessmanagement-self-service-group-management.md)) delegieren.

> [!TIP]
> Ausführliche Informationen zur Lizenzzuweisung zu Gruppen (einschließlich erweiterter Szenarien und Office 365-Lizenzierungsszenarien) finden Sie unter [Zuweisen von Lizenzen zu einer Gruppe von Benutzern in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-to-users-and-groups"></a>Zuweisen von Lizenzen zu Benutzern und Gruppen
Sie sollten zunächst bei aktivem Abonnement sich selbst eine Lizenz zuweisen und Ihren Browser aktualisieren, um sicherzustellen, dass Sie alle in Ihrem Abonnement enthaltenen, erwarteten Funktionen sehen können. Im nächsten Schritt weisen Sie den Benutzern Lizenzen zu, die auf kostenpflichtige Azure AD-Features zugreifen müssen. Eine einfache Möglichkeit zum Zuweisen von Lizenzen besteht darin, Benutzergruppen statt einzelnen Benutzern Lizenzen zuzuweisen. Wenn Sie einer Gruppe Lizenzen zuweisen, wird allen Mitgliedern der Gruppe eine Lizenz zugewiesen. Wenn Benutzer der Gruppe hinzugefügt oder aus der Gruppe entfernt werden, wird die entsprechende Lizenz automatisch zugewiesen oder entfernt. 

> [!NOTE]
> Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Verwendungsstandort** für den Benutzer angeben. Sie können diese Eigenschaft im Azure-Portal unter **Benutzer** &gt; **Profil** &gt; **Einstellungen** festlegen. Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer, deren Verwendungsstandort nicht angegeben ist, den Standort des Verzeichnisses.

Um eine Lizenz zuzuweisen, wählen Sie unter **Azure Active Directory** &gt; **Lizenzen** &gt; **Alle Produkte** mindestens ein Produkt aus, und klicken Sie auf der Befehlsleiste auf **Zuweisen**.

![Auswählen einer zuzuweisenden Lizenz](media/license-users-groups/select-license-to-assign.png)

Auf dem Blatt **Benutzer und Gruppen** können Sie mehrere Benutzer oder Gruppen auswählen bzw. Servicepläne im Produkt deaktivieren. Über das Suchfeld am oberen Rand können Sie nach Benutzer- und Gruppennamen suchen.

![Auswählen eines Benutzers oder einer Gruppe für die Lizenzzuweisung](media/license-users-groups/select-user-for-license-assignment.png)

Wenn Sie einer Gruppe Lizenzen zuweisen, dauert es je nach Größe der Gruppe unter Umständen eine Weile, bis alle Benutzer die Lizenz geerbt haben. Sie können den Verarbeitungsstatus auf dem Blatt **Gruppe** auf der Kachel **Lizenzen** verfolgen.

![Lizenzzuweisungsstatus](media/license-users-groups/license-assignment-status.png)

Bei der Azure AD-Lizenzzuweisung können zwar Zuweisungsfehler auftreten, dies ist bei der Verwaltung von Azure AD- und Enterprise Mobility + Security-Produkten allerdings eher selten. Potenzielle Zuweisungsfehler sind beschränkt auf:
- Zuweisungskonflikte: Treten auf, wenn einem Benutzer zuvor eine Lizenz zugewiesen war, die nicht mit der aktuellen Lizenz kompatibel ist. In diesem Fall muss die aktuelle Lizenz entfernt werden, um die neue Lizenz zuweisen zu können.
- Überschreitung der verfügbaren Lizenzanzahl: Wenn die Anzahl der Benutzer in zugewiesenen Gruppen die Anzahl der verfügbaren Lizenzen überschreitet, gibt der Zuweisungsstatus für den Benutzer einen Zuweisungsfehler aufgrund fehlender Lizenzen an.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Lizenzierung von Azure AD B2B-Kollaboration

Mit B2B-Kollaboration können Sie Gastbenutzer in Ihren Azure AD-Mandanten einladen, um den Zugriff auf Azure AD-Dienste und beliebige Azure-Ressourcen, die Sie verfügbar machen, zu ermöglichen.  

Es fallen Kosten für das Einladen von B2B-Benutzern und das Zuweisen zu einer Anwendung in Azure AD an. Bis zu 10 Apps pro Gastbenutzer und 3 einfache Berichte sind für B2B-Kollaborationsbenutzer auch kostenlos. Wenn Ihrem Gastbenutzer entsprechende Lizenzen im Azure AD-Mandanten des Partners zugewiesen wurden, wird er auch in Ihrem lizenziert.

Es ist nicht erforderlich, aber falls Sie den Zugriff auf kostenpflichtige Azure AD-Features ermöglichen möchten, müssen diese B2B-Gastbenutzer mit entsprechenden Azure AD-Lizenzen lizenziert werden. Ein einladender Mandant mit einer kostenpflichtigen Azure AD-Lizenz kann zusätzlichen fünf, zu dem Mandanten eingeladenen Gastbenutzern B2B-Kollaborationsbenutzerrechte zuweisen. Weitere Informationen und Szenarien finden Sie unter [Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Anzeigen zugewiesener Lizenzen

Eine Übersicht über zugewiesene und verfügbare Lizenzen finden Sie unter **Azure Active Directory** &gt; **Lizenzen** &gt; **Alle Produkte**.

![Anzeigen der Lizenzzusammenfassung](media/license-users-groups/view-license-summary.png)

Eine detaillierte Liste mit zugewiesenen Benutzern und Gruppen erhalten Sie, indem Sie ein bestimmtes Produkt auswählen. Die Liste **Lizenzierte Benutzer** zeigt alle Benutzer, die derzeit eine Lizenz nutzen. Hier sehen Sie auch, ob die Lizenz dem Benutzer direkt zugewiesen wurde, oder ob er sie von einer Gruppe geerbt hat.

![Anzeigen der Lizenzdetails](media/license-users-groups/view-license-detail.png)

Analog dazu finden Sie unter **Lizenzierte Gruppen** eine Liste aller Gruppen, denen Lizenzen zugewiesen wurden. Wählen Sie einen Benutzer oder eine Gruppe aus, um das Blatt **Lizenzen** zu öffnen, das alle Lizenzen anzeigt, die dem jeweiligen Objekt zugewiesen wurden.

## <a name="remove-a-license"></a>Entfernen einer Lizenz

Navigieren Sie zum Entfernen einer Lizenz zum gewünschten Benutzer oder zur gewünschten Gruppe, und öffnen Sie die Kachel **Lizenzen**. Wählen Sie die Lizenz aus, und klicken Sie auf **Entfernen**.

![Entfernen einer Lizenz](media/license-users-groups/remove-license.png)

Lizenzen, die der Benutzer von einer Gruppe geerbt hat, können nicht direkt entfernt werden. Entfernen Sie stattdessen den Benutzer aus der Gruppe, von der er die Lizenz erbt.


## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie erfahren, wie Lizenzen für Benutzer und Gruppen im Azure AD-Verzeichnis zugewiesen werden. 

Über den folgenden Link können Sie mit dem Azure-Portal Zuweisungen von Abonnementlizenzen in Azure AD konfigurieren.

> [!div class="nextstepaction"]
> [Zuweisen von Azure AD-Lizenzen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 
