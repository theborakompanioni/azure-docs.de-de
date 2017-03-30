---
title: "Schützen von PaaS-Bereitstellungen | Microsoft-Dokumentation"
description: " Informieren Sie sich über die Sicherheitsvorteile von PaaS im Vergleich zu anderen Clouddienstmodellen, und machen Sie sich mit empfohlenen Vorgehensweisen für den Schutz Ihrer Azure-PaaS-Bereitstellung vertraut. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: f218fe7e59e46683b544fd83bfea505b7cbe2d59
ms.lasthandoff: 03/22/2017


---
# <a name="securing-paas-deployments"></a>Schützen von PaaS-Bereitstellungen

Dieser Artikel beschäftigt sich mit Folgendem:

- Sicherheitsvorteile von in der Cloud gehosteten Anwendungen
- Bewertung der Sicherheitsvorteile von PaaS (Platform-as-a-Service) im Vergleich zu anderen Clouddienstmodellen
- Verlagerung des Sicherheitsfokus von netzwerkorientierter zu identitätsorientierter Bereichssicherheit
- Implementierung allgemeiner Sicherheitsempfehlungen für PaaS

## <a name="cloud-security-advantages"></a>Sicherheitsvorteile der Cloud
Die Nutzung der Cloud bringt einige Sicherheitsvorteile mit sich. In einer lokalen Umgebung werden in Organisationen häufig nicht alle Zuständigkeitsbereiche abgedeckt, und für die Sicherheit stehen nur begrenzte Ressourcen zur Verfügung. So entsteht eine Umgebung, in der Angreifer Sicherheitslücken auf allen Ebenen ausnutzen können.

![Sicherheitsvorteile des Cloudzeitalters][1]

Mithilfe der cloudbasierten Sicherheitsfunktionen und Clouddaten eines Anbieters können Organisationen ihre Bedrohungserkennung und ihre Reaktionszeiten verbessern.  Durch die Übertragung von Aufgaben an den Cloudanbieter profitieren Organisationen von einer höheren Sicherheit und können Sicherheitsressourcen und entsprechende Finanzmittel für andere geschäftliche Prioritäten nutzen.

## <a name="division-of-responsibility"></a>Geteilte Zuständigkeit
Es ist wichtig, die Aufteilung der Zuständigkeit zwischen Ihnen und Microsoft zu verstehen. In einer lokalen Umgebung sind Sie für den gesamten Stapel zuständig. Bei einem Wechsel in die Cloud wird dagegen ein Teil der Zuständigkeit auf Microsoft übertragen. Die folgende Zuständigkeitsmatrix zeigt, für welche Bereiche des Stapels in einer SaaS-, PaaS- und IaaS-Bereitstellung Sie selbst zuständig sind und für welche Bereiche Microsoft zuständig ist.

![Zuständigkeitszonen][2]

Ihre Daten und Identitäten gehören bei jeder Art von Cloudbereitstellung Ihnen. Sie sind für den Schutz der Sicherheit Ihrer Daten und Identitäten, lokalen Ressourcen und der von Ihnen gesteuerten Cloudkomponenten zuständig (abhängig von der Art des Diensts).

Für folgende Bereiche sind immer Sie selbst zuständig (unabhängig von der Art der Bereitstellung):

- Daten
- Endpunkte
- Konto
- Zugriffsverwaltung

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Sicherheitsvorteile eines PaaS-Clouddienstmodells
Sehen wir uns anhand der gleichen Zuständigkeitsmatrix die Sicherheitsvorteile einer Azure-PaaS-Bereitstellung im Vergleich zu einer lokalen Bereitstellung an.

![Sicherheitsvorteile von PaaS][3]

Beginnen wir ganz unten im Stapel: bei der physischen Infrastruktur. Hier kümmert sich Microsoft um allgemeine Risiken und Zuständigkeiten. Die Microsoft-Cloud wird kontinuierlich von Microsoft überwacht und lässt sich daher nur schwer angreifen. Für einen Angreifer stellt die Microsoft-Cloud somit kein sinnvolles Ziel dar. Sofern der Angreifer nicht über sehr viel Geld und Ressourcen verfügt, wird er sich wahrscheinlich ein anderes Ziel suchen.  

In der Mitte des Stapels gibt es keinen Unterschied zwischen einer PaaS-Bereitstellung und einer lokalen Bereitstellung. Auf der Anwendungsebene und der Konto- und Zugriffsverwaltungsebene sind die Risiken ähnlich. Bewährte Methoden für die Beseitigung oder Minimierung dieser Risiken finden Sie in diesem Artikel im Abschnitt „Nächste Schritte“.

An der Spitze des Stapels (Datengovernance und Rechteverwaltung) können Sie sich mit einem einzelnen Risiko auseinandersetzen, das mittels Schlüsselverwaltung behandelt werden kann. (Die Schlüsselverwaltung wird in den bewährten Methoden behandelt.) Bei der Schlüsselverwaltung handelt es sich zwar um eine zusätzliche Aufgabe, dafür gibt es in einer PaaS-Bereitstellung jedoch Bereiche, um die Sie sich nicht mehr kümmern müssen, sodass Sie frei gewordene Ressourcen für die Schlüsselverwaltung nutzen können.

Dank verschiedener netzwerkbasierter Technologien bietet die Azure-Plattform außerdem starken DDoS-Schutz. Bei allen Arten von netzwerkbasierten DDoS-Schutzmethoden gelten allerdings gewisse Einschränkungen für Links und Datencenter. Zur Vermeidung der Auswirkungen groß angelegter DDoS-Angriffe können Sie dank der Kernfunktion der Azure-Cloud schnell und automatisch horizontal hochskalieren, um DDoS-Angriffe abzuwehren. Ausführlichere Informationen zur entsprechenden Vorgehensweise finden Sie in den Artikeln zu empfohlenen Vorgehensweisen.

## <a name="modernizing-the-defenders-mindset"></a>Eine neue Mentalität für Verteidiger
Mit PaaS-Bereitstellungen verändert sich der gesamte Sicherheitsansatz. Sie müssen sich nun nicht mehr um alles selbst kümmern, sondern können einen Teil der Verantwortung an Microsoft abgeben.

Ein weiterer wichtiger Unterschied zwischen PaaS und herkömmlichen lokalen Bereitstellungen ist die Neuauslegung des primären Sicherheitsbereichs. In der Vergangenheit war Ihr Netzwerk der primäre lokale Sicherheitsbereich, und bei den meisten lokalen Sicherheitskonzepten wird das Netzwerk als Dreh- und Angelpunkt für die Sicherheit verwendet. Bei PaaS-Bereitstellungen empfiehlt es sich dagegen, die Identität als primären Sicherheitsbereich zu betrachten.

## <a name="identity-as-the-primary-security-perimeter"></a>Identität als primärer Sicherheitsbereich
Eines der fünf grundlegenden Merkmale von Cloud Computing ist umfassender Netzwerkzugriff, was eine netzwerkorientierte Betrachtung weniger relevant macht. Cloud Computing dient zu einem großen Teil dazu, Benutzern unabhängig von ihrem Standort Zugriff auf Ressourcen zu ermöglichen. Die meisten Benutzer befinden sich irgendwo im Internet.

Die folgende Abbildung zeigt die Entwicklung von einem netzwerkorientierten zu einem identitätsorientierten Sicherheitsbereich. Bei der Sicherheit geht es immer weniger um die Verteidigung Ihres Netzwerks und immer mehr um die Verteidigung Ihrer Daten sowie um die Verwaltung der Sicherheit Ihrer Apps und Benutzer. Der wesentliche Unterschied besteht darin, dass Sie die Sicherheit stärker dem annähern möchten, was für Ihr Unternehmen wichtig ist.

![Identität als neuer Sicherheitsbereich][4]

Zu Anfang verfügten Azure PaaS-Dienste wie etwa Webrollen und Azure SQL nur über eine rudimentäre oder über gar keine traditionelle Netzwerkbereichsverteidigung. Es wurde allgemein angenommen, dass das Element für das Internet verfügbar gemacht wird (Webrolle) und dass die Authentifizierung den neuen Sicherheitsbereich bildet (beispielsweise Blob oder Azure SQL).

Bei modernen Sicherheitsmaßnahmen wird davon ausgegangen, dass der Angreifer in den Netzwerkbereich eingedrungen ist. Aus diesem Grund ist bei modernen Verteidigungsmethoden die Identität in den Fokus gerückt. Organisationen müssen einen identitätsbasierten Sicherheitsbereich mit starker Authentifizierung und Autorisierung (bzw. mit entsprechenden bewährten Methoden) einrichten.

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Empfehlungen für die Verwaltung des Identitätsbereichs

Prinzipien und Muster für den Netzwerkbereich sind bereits seit Jahrzehnten verfügbar. Im Gegensatz dazu hat die Branche relativ wenig Erfahrung mit der Identität als primärer Sicherheitsbereich. Wir haben jedoch genug Erfahrung gesammelt, um einige allgemeine Empfehlungen abzugeben, die sich in der Praxis bewährt haben und für nahezu alle PaaS-Dienste gelten.

Der folgende Abschnitt enthält eine allgemeine Zusammenfassung bewährter Methoden für die Verwaltung Ihres Identitätsbereichs.

- **Verlieren Sie Ihre Schlüssel oder Anmeldeinformationen nicht.** Schlüssel und Anmeldeinformationen müssen unbedingt geschützt werden, um die Sicherheit von PaaS-Bereitstellungen zu gewährleisten. Der Verlust von Schlüsseln und Anmeldeinformationen ist ein verbreitetes Problem. Eine empfehlenswerte Methode ist die Verwendung einer zentralen Lösung, bei der Schlüssel und Geheimnisse in Hardwaresicherheitsmodulen (HSM) gespeichert werden. Azure bietet mit [Azure Key Vault](../key-vault/key-vault-whatis.md) ein HSM in der Cloud.
- **Platzieren Sie Anmeldeinformationen und andere Geheimnisse nicht in Quellcode oder auf GitHub.** Noch schlimmer als der Verlust von Schlüsseln und Anmeldeinformationen ist es, wenn nicht autorisierte Personen an Ihre Schlüssel und Anmeldeinformationen gelangen. Angreifer können mithilfe von Bottechnologien Coderepositorys wie GitHub nach darin gespeicherten Schlüsseln und Geheimnissen durchsuchen. Speichern Sie daher keine Schlüssel oder Geheimnisse in diesen öffentlichen Quellcoderepositorys.
- **Schützen Sie Ihre VM-Verwaltungsschnittstellen in PaaS- und IaaS-Hybriddiensten.** IaaS- und PaaS-Dienste werden auf virtuellen Computern ausgeführt. Für die direkte Remoteverwaltung dieser virtuellen Computer stehen je nach Art des Diensts mehrere Verwaltungsschnittstellen zur Verfügung. Dabei können Remoteverwaltungsprotokolle wie das [SSH-Protokoll (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell), das [Remotedesktopprotokoll (RDP)](https://support.microsoft.com/kb/186607) und [Remote PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) verwendet werden. Im Allgemeinen empfiehlt es sich, über das Internet keinen direkten Remotezugriff auf virtuelle Computer zu ermöglichen. Verwenden Sie nach Möglichkeit alternative Methoden – beispielsweise eine VPN-Verbindung (virtuelles privates Netzwerk) mit einem virtuellen Azure-Netzwerk. Sollten keine Alternativen verfügbar sein, verwenden Sie unbedingt komplexe Passphrasen und nach Möglichkeit eine zweistufige Authentifizierung (beispielsweise [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)).
- **Verwenden Sie sichere Authentifizierungs- und Autorisierungsplattformen.**

  - Verwenden Sie Verbundidentitäten in Azure AD anstelle von benutzerdefinierten Benutzerspeichern. Bei Verbundidentitäten profitieren Sie von einem plattformbasierten Konzept und delegieren die Verwaltung autorisierter Identitäten an Ihre Partner. Das Verbundidentitätskonzept ist besonders in Szenarien wichtig, in denen Mitarbeiter gekündigt werden und diese Information in mehreren Identitäts- und Autorisierungssystemen berücksichtigt werden muss.
  - Verwenden Sie von der Plattform bereitgestellte Authentifizierungs- und Autorisierungsmechanismen anstelle von benutzerdefiniertem Code. Der Grund: Bei der Entwicklung von benutzerdefiniertem Authentifizierungscode können Ihnen unter Umständen Fehler unterlaufen. Die meisten Ihrer Entwickler sind keine Sicherheitsexperten und deshalb wahrscheinlich nicht mit den Feinheiten und neuesten Entwicklungen im Authentifizierungs- und Autorisierungsbereich vertraut. Die Sicherheit von kommerziellem Code (etwa von Microsoft) wird häufig intensiv geprüft.
  - Verwenden Sie eine mehrstufige Authentifizierung. Die mehrstufige Authentifizierung ist der aktuelle Standard für die Authentifizierung und Autorisierung, da sie die Sicherheitsprobleme eliminiert, die für Authentifizierungsmethoden mit Benutzername und Kennwort typisch sind. Der Zugriff auf Azure-Verwaltungsschnittstellen (Portal/Remote-PowerShell) und auf kundenorientierte Dienste sollte für die Verwendung von [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) konzipiert und konfiguriert sein.
  - Verwenden Sie standardmäßige Authentifizierungsprotokolle wie OAuth2 und Kerberos. Diese Protokolle wurden umfassend überprüft und sind wahrscheinlich Teil Ihrer Plattformbibliotheken für die Authentifizierung und Autorisierung.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel standen die Sicherheitsvorteile einer Azure-PaaS-Bereitstellung im Vordergrund. Als Nächstes gehen wir auf bewährte Vorgehensweisen zum Schutz Ihrer webbasierten und mobilen PaaS-Lösungen ein. Wir beginnen mit Azure App Service, Azure SQL-Datenbank und Azure SQL Data Warehouse. Sobald Artikel zu empfohlenen Vorgehensweisen für andere Azure-Dienste verfügbar sind, werden in der folgenden Liste entsprechende Links bereitgestellt:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL-Datenbank und Azure SQL Data Warehouse](security-paas-applications-using-sql.md)
- Azure Storage
- Azure Redis Cache
- Azure-Servicebus
- Web Application Firewalls

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png

