---
title: "Einführung in Azure Stack Key Vault | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure Stack Key Vault Schlüssel und Geheimnisse verwaltet"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/04/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 0fa9038cd4707408fd45fea7ea92cfe815585adf
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Einführung in Key Vault in Azure Stack

## <a name="before-you-start"></a>Vorbereitung
In diesem Artikel wird Folgendes vorausgesetzt:

* Azure Stack-Bediener müssen [ein Angebot erstellt](azure-stack-create-offer.md) haben, das den Key Vault-Dienst umfasst.  
* Benutzer müssen [ein Angebot abonnieren](azure-stack-subscribe-plan-provision-vm.md), das den Key Vault-Dienst umfasst.  
* [PowerShell ist für die Verwendung mit Azure Stack konfiguriert.](azure-stack-powershell-configure-user.md) 
 
## <a name="key-vault-basics"></a>Key Vault-Grundlagen
Key Vault in Azure Stack unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Durch die Verwendung von Key Vault können Sie Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln.

Der Schlüsseltresor optimiert die Schlüsselverwaltung und ermöglicht es Ihnen, die Kontrolle über Schlüssel zu behalten, die für den Datenzugriff und die Verschlüsselung Ihrer Daten verwendet werden. Entwickler können Schlüssel für Tests und Entwicklung innerhalb von Minuten erstellen und diese später nahtlos in Schlüssel für die Produktion migrieren. Sicherheitsadministratoren können nach Bedarf Berechtigungen für Schlüssel erteilen (und widerrufen).

Jeder Benutzer mit einem Azure Stack-Abonnement kann Schlüsseltresore erstellen und verwenden. Wenngleich Key Vault insbesondere Entwicklern und Sicherheitsadministratoren Vorteile bietet, kann die Lösung durch den Betreiber implementiert und verwaltet werden, der andere Azure Stack-Dienste für eine Organisation verwaltet. So kann sich der Azure Stack-Administrator beispielsweise mit einem Azure Stack-Abonnement anmelden, einen Tresor für die Schlüsselspeicherung der Organisation erstellen und dann diese operativen Aufgaben übernehmen:

* Erstellen oder Importieren eines Schlüssels oder geheimer Informationen
* Widerrufen oder Löschen eines Schlüssels oder geheimer Informationen
* Autorisieren von Benutzern oder Anwendungen für den Zugriff auf den Schlüsseltresor, sodass sie die Schlüssel und geheimen Schlüssel verwalten oder verwenden können
* Konfigurieren der Schlüsselverwendung (z. B. Anmelden oder Verschlüsseln)

Der Betreiber kann anschließend den Entwicklern die URIs für den Aufruf der Schlüssel aus ihren Anwendungen und den Sicherheitsadministratoren Protokollinformationen zur Schlüsselverwendung bereitstellen.

Entwickler können die Schlüssel außerdem durch Verwendung von APIs direkt verwalten. Weitere Informationen finden Sie im Key Vault-Entwicklerhandbuch.

## <a name="scenarios"></a>Szenarien
Die folgende Tabelle stellt einige der Szenarien dar, in denen Sie mithilfe von Key Vault die Anforderungen von Entwicklern und Sicherheitsadministratoren erfüllen können:

### <a name="developer-for-an-azure-stack-application"></a>Entwickler einer Azure Stack-Anwendung
**Problem:** Ich möchte eine Anwendung für Azure Stack schreiben, die Schlüssel für Anmeldung und Verschlüsselung verwendet, aber ich möchte diese Schlüssel außerhalb der Anwendung verwalten, damit die Lösung auch für eine geografisch verteilte Anwendung geeignet ist.

**Stellungnahme:** Schlüssel werden in einem Tresor gespeichert und bei Bedarf über einen URI aufgerufen.

### <a name="developer-for-software-as-a-service-saas"></a>Entwickler von SaaS-Lösungen (Software-as-a-Service)
**Problem:** Ich möchte weder die Verantwortung noch die Haftung für Schlüssel und geheime Schlüssel meiner Kunden übernehmen.

**Stellungnahme:** Kunden können eigene Schlüssel in Azure Stack importieren und diese verwalten. Ich möchte, dass Kunden sowohl die Verantwortung als auch die Verwaltung für ihre Schlüssel übernehmen, sodass ich mich auf meine eigentlichen Aufgaben konzentrieren kann – die Bereitstellung der Softwarefunktionen.

### <a name="chief-security-officer-cso"></a>Sicherheitsbeauftragter (Chief Security Officer, CSO)
**Problem:** Ich möchte sicherstellen, dass meine Organisation die Kontrolle über den gesamten Lebenszyklus der Schlüssel behält und die Schlüsselverwendung überwachen kann.

**Stellungnahme:** Mit Key Vault sind Ihre Schlüssel für Microsoft nicht sichtbar und können auch nicht extrahiert werden.  Wenn eine Anwendung kryptografische Vorgänge unter Verwendung von Kundenschlüsseln ausführen muss, führt Key Vault diese Vorgänge im Auftrag der Anwendung aus. Die Anwendung hat keine Informationen über die Kundenschlüssel.  Auch wenn wir verschiedene Azure Stack-Dienste und -Ressourcen nutzen, möchte ich die Schlüssel über einen einzigen Speicherort in Azure Stack verwalten. Der Tresor stellt eine einzige Benutzeroberfläche bereit – unabhängig davon, über wie viele Tresore Sie in Azure Stack verfügen, welche Regionen diese unterstützen und von welchen Anwendungen die Tresore verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Key Vault in Azure Stack über das Portal](azure-stack-kv-manage-portal.md)  
* [Verwalten von Key Vault in Azure Stack mithilfe von PowerShell](azure-stack-kv-manage-powershell.md)

