---
title: "Datenklassifizierung für Azure | Microsoft Docs"
description: "Dieser Artikel enthält eine Einführung in die Grundlagen der Datenklassifizierung und hebt ihren Wert hervor, insbesondere im Kontext von Cloud Computing und der Verwendung von Microsoft Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 91d4afed-b80f-4a26-b526-b52b9c858cfb
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 354648461adf77ccf753cf24721ab7dd82b34d3c
ms.openlocfilehash: 1f64478b900e5120253da42fcd1777540737b22c


---
# <a name="data-classification-for-azure"></a>Datenklassifizierung für Azure
Dieser Artikel enthält eine Einführung in die Grundlagen der Datenklassifizierung und hebt ihren Wert hervor, insbesondere im Kontext von Cloud Computing und der Verwendung von Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Grundlegendes zur Datenklassifizierung
Für die erfolgreiche Datenklassifizierung in einer Organisation sind ein starkes Bewusstsein für die Anforderungen der Organisation sowie gute Kenntnisse der Speicherorte von Datenassets erforderlich.  

Daten sind in einem von drei Grundzuständen vorhanden: 

* Im Ruhezustand 
* In Bearbeitung 
* Während der Übertragung 

Für alle drei Zustände sind eindeutige technische Lösungen für die Datenklassifizierung erforderlich, aber die angewendeten Prinzipien für die Datenklassifizierung sollten jeweils gleich sein. Daten, die als vertraulich klassifiziert werden, müssen auch im Ruhezustand, während der Bearbeitung und während der Übertragung vertraulich bleiben. 

Außerdem können Daten entweder strukturiert oder unstrukturiert sein. Typische Klassifizierungsprozesse für die strukturierten Daten in Datenbanken und Tabellen sind, was die Verwaltung betrifft, weniger komplex und zeitaufwändig als die Prozesse für unstrukturierte Daten, z.B. Dokumente, Quellcode und E-Mails. 

> [!TIP]
> Weitere Informationen zu Azure-Funktionen und bewährten Methoden zur Datenverschlüsselung finden Sie unter [Azure-Datenverschlüsselung – Bewährte Methoden](azure-security-data-encryption-best-practices.md).
> 
> 

Meist verfügen Organisationen über eine größere Menge an unstrukturierten Daten als an strukturierten Daten. Unabhängig davon, ob Daten strukturiert oder unstrukturiert sind, ist es wichtig, dass Sie die Vertraulichkeit der Daten verwalten. Bei korrekter Implementierung trägt die Datenklassifizierung dazu bei, dass sensible bzw. vertrauliche Datenassets besser als Datenassets verwaltet werden, die als öffentlich oder frei verteilbar angesehen werden. 

### <a name="controlling-access-to-data"></a>Steuern des Zugriffs auf Daten
Authentifizierung und Autorisierung werden häufig miteinander verwechselt, und ihre jeweilige Rolle wird falsch verstanden. In Wirklichkeit besteht ein deutlicher Unterschied, der in der folgenden Abbildung dargestellt ist:  

![Datenzugriff und -steuerung](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Authentifizierung
Die Authentifizierung besteht in der Regel aus mindestens zwei Teilen: einem Benutzernamen oder einer Benutzer-ID zur Identifizierung eines Benutzers und einem Token, z.B. einem Kennwort, um zu bestätigen, dass die Anmeldeinformation „Benutzername“ gültig ist. Bei diesem Prozess wird dem authentifizierten Benutzer kein Zugriff auf Elemente oder Dienste gewährt. Es wird überprüft, ob der Benutzer die Person ist, als die er sich ausgibt.   

> [!TIP]
> [Azure Active Directory](../active-directory/active-directory-whatis.md) bietet cloudbasierte Identitätsdienste, mit denen Sie Benutzer authentifizieren und autorisieren können. 
> 
> 

### <a name="authorization"></a>Autorisierung
Die Autorisierung ist der Prozess, bei dem einem authentifizierten Benutzer der Zugriff auf eine Anwendung, ein Dataset, eine Datendatei oder ein anderes Objekt gewährt wird. Für die Zuweisung von Berechtigungen zum Verwenden, Ändern oder Löschen von Elementen, auf die zugegriffen werden kann, für authentifizierte Benutzer muss die Datenklassifizierung beachtet werden. 

Für die erfolgreiche Autorisierung ist die Implementierung eines Mechanismus erforderlich, mit dem die Anforderungen einzelner Benutzer zum Zugreifen auf Dateien und Informationen basierend auf einer Kombination aus Rollen-, Sicherheitsrichtlinien- und Risikorichtlinienaspekten überprüft werden. Auf Daten aus bestimmten Branchenanwendungen muss ggf. nicht von allen Mitarbeitern zugegriffen werden können, und nur eine kleine Gruppe der Mitarbeiter benötigt vermutlich Zugriff auf die Dateien der Personalabteilung. Damit Organisationen steuern können, wer auf Daten zugreift und wann und wie dies geschieht, muss ein effektives System zum Authentifizieren von Benutzern vorhanden sein. 

> [!TIP]
> Stellen Sie in Microsoft Azure sicher, dass Sie nur die rollenbasierte Access Control (RBAC) von Azure verwenden, um jeweils nur den Zugriff zu gewähren, den Benutzer zum Erledigen ihrer Arbeit benötigen. Weitere Informationen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure Active Directory-Ressourcen](../active-directory/role-based-access-control-configure.md). 
> 
> 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Rollen und Zuständigkeiten beim Cloud Computing
Cloudanbieter können beim Verwalten von Risiken zwar Hilfe leisten, aber Kunden müssen sicherstellen, dass die Verwaltung und Durchsetzung der Datenklassifizierung richtig implementiert ist. So kann der richtige Grad an Datenverwaltungsdiensten bereitgestellt werden.  

Die Zuständigkeiten in Bezug auf die Datenklassifizierung variieren basierend darauf, welches Clouddienstmodell verwendet wird. Dies ist in der folgenden Abbildung dargestellt. Die drei Hauptmodelle für Clouddienste sind Infrastructure as a Service (IaaS), Platform as a Service (PaaS) und Software as a Service (SaaS). Die Implementierung von Mechanismen der Datenklassifizierung variiert auch basierend auf der Abhängigkeit vom Cloudanbieter bzw. von den jeweiligen Erwartungen. 

![Rollen](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Auch wenn Sie selbst für die Klassifizierung Ihrer Daten verantwortlich sind, sollten Cloudanbieter schriftlich darlegen, wie der Datenschutz für die in der Cloud gespeicherten Kundendaten sichergestellt wird.  

* Die Anforderungen an **IaaS-Anbieter** beschränken sich darauf sicherzustellen, dass die virtuelle Umgebung Funktionen zur Datenklassifizierung bietet und die Complianceanforderungen der Kunden erfüllt. IaaS-Anbieter spielen eine kleinere Rolle bei der Datenklassifizierung, da sie nur dafür sorgen müssen, dass für Kundendaten die Complianceanforderungen erfüllt werden. Trotzdem müssen die Anbieter aber sicherstellen, dass in ihren virtuellen Umgebungen die Anforderungen an die Datenklassifizierung erfüllt werden und die Rechenzentren ausreichend geschützt sind.
* Die Zuständigkeiten von **PaaS**-Anbietern lassen sich nicht eindeutig festlegen, da die Plattform auch für einen Ansatz mit Schichten verwendet werden kann, um Sicherheit für ein Klassifizierungstool zu bieten. PaaS-Anbieter sind für die Authentifizierung und ggf. einige Autorisierungsregeln zuständig und müssen Funktionen für die Sicherheit und Datenklassifizierung für ihre Anwendungsebene bereitstellen. Ähnlich wie IaaS-Anbieter auch, müssen PaaS-Anbieter sicherstellen, dass ihre Plattform alle relevanten Anforderungen an die Datenklassifizierung erfüllt.
* **SaaS-Anbieter** werden häufig als Teil einer Autorisierungskette angesehen und müssen sicherstellen, dass die in der SaaS-Anwendung gespeicherten Daten nach dem Klassifizierungstyp gesteuert werden können. SaaS-Anwendungen können für Branchenanwendungen verwendet werden und müssen gemäß ihrer Bestimmung die Mittel zum Authentifizieren und Autorisieren von Daten bereitstellen, die verwendet und gespeichert werden. 

## <a name="classification-process"></a>Klassifizierungsprozess
Viele Organisationen, die die Wichtigkeit der Datenklassifizierung verstanden haben und mit der Implementierung beginnen möchten, stehen vor der folgenden Herausforderung: Womit soll begonnen werden?

Eine effektive und einfache Möglichkeit zum Implementieren der Datenklassifizierung ist die Verwendung des Modells „PLAN, DO, CHECK, ACT“ unter [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). In der folgenden Abbildung sind die Aufgaben dargestellt, die zum erfolgreichen Implementieren der Datenklassifizierung bei diesem Modell durchgeführt werden müssen.  

1. **PLAN (PLANEN)**. Hier werden Datenassets und ein Datenverwalter zum Bereitstellen des Klassifizierungsprogramms identifiziert und Schutzprofile entwickelt. 
2. **DO (DURCHFÜHREN)**. Nachdem die Richtlinien für die Datenklassifizierung vereinbart wurden, stellen Sie das Programm bereit und implementieren nach Bedarf Durchsetzungstechnologien für vertrauliche Daten.  
3. **CHECK (PRÜFEN)**. Überprüfen und validieren Sie Berichte, um sicherzustellen, dass die verwendeten Tools und Methoden wirklich für die Klassifizierungsrichtlinien geeignet sind. 
4. **ACT (HANDELN)**. Überprüfen Sie den Status des Datenzugriffs sowie Dateien und Daten, für die dies erforderlich ist, indem Sie eine Methodik für die Neuklassifizierung und Überprüfung verwenden, um Änderungen zu übernehmen und neuen Risiken zu begegnen.  

![Plan, Do, Check, Act](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)

### <a name="select-a-terminology-model-that-addresses-your-needs"></a>Auswählen eines für Ihre Anforderungen geeigneten Terminologiemodells
Es gibt mehrere Arten von Prozessen für die Klassifizierung von Daten, z.B. manuelle Prozesse, standortbasierte Prozesse, bei denen Daten anhand des Benutzer- oder Systemstandorts klassifiziert werden, anwendungsbasierte Prozesse wie die datenbankspezifische Klassifizierung und automatisierte Prozesse, die für unterschiedliche Technologien eingesetzt werden. Einige davon sind weiter unten in diesem Artikel im Abschnitt „Schützen vertraulicher Daten“ beschrieben.  

In diesem Artikel werden zwei verallgemeinerte Terminologiemodelle eingeführt, die auf gängigen und in der Branche akzeptierten Modellen basieren. Diese Terminologiemodelle, die jeweils drei Ebenen der Klassifizierungsvertraulichkeit bieten, sind in der folgenden Tabelle aufgeführt.  

> [!NOTE]
> Beim Klassifizieren einer Datei oder Ressource, in der Daten kombiniert sind, die normalerweise auf unterschiedlichen Ebenen klassifiziert werden, sollte sich die allgemeine Klassifizierung nach der höchsten vorhandenen Klassifizierungsebene richten. Beispielsweise sollte eine Datei, die sensible und mit Zugriffsbeschränkungen versehene Daten enthält, als „Eingeschränkt“ klassifiziert werden.  
> 
> 

| **Vertraulichkeit** | **Terminologiemodell 1** | **Terminologiemodell 2** |
| --- | --- | --- |
| Hoch |Vertraulich |Eingeschränkt |
| Mittel |Nur zur internen Verwendung |Sensibel |
| Niedrig |Öffentlich |Nicht eingeschränkt |

#### <a name="confidential-restricted"></a>Vertraulich (eingeschränkt)
Informationen, die als vertraulich oder eingeschränkt klassifiziert sind, sind beispielsweise Daten, bei denen sich katastrophale Folgen für Personen bzw. Organisationen ergeben, wenn sie kompromittiert werden oder verloren gehen. Informationen dieser Art werden häufig nach dem Schema „Need to know“ bereitgestellt und können Folgendes umfassen: 

* Persönliche Daten, z.B. personenbezogene Informationen wie Sozialversicherungsnummer oder Personalausweisnummer, Reisepassnummer, Kreditkartennummer, Führerscheinnummer, medizinische Daten und Nummer der Krankenversicherungskarte.  
* Finanzdaten, z.B. Kontonummern von Giro- oder Sparkonten. 
* Geschäftliche Daten, z.B. Dokumente oder Daten, bei denen es sich um Originale oder spezielles geistiges Eigentum handelt.  
* Rechtliche Daten, z.B. anwaltsvertrauliche Unterlagen. 
* Authentifizierungsdaten, z.B. private Verschlüsselungsschlüssel, Benutzername/Kennwort-Paare oder andere Identifizierungssequenzen wie Dateien mit privaten biometrischen Schlüsseln. 

Für Daten, die als vertraulich klassifiziert sind, gelten in Bezug auf die Handhabung häufig regulatorische und auf die Compliance bezogene Anforderungen. 

#### <a name="for-internal-use-only-sensitive"></a>Nur für die interne Verwendung (sensibel)
Informationen, die als Daten mittlerer Vertraulichkeit klassifiziert werden, sind Dateien und Daten, bei denen sich für Personen bzw. Organisationen keine schwerwiegenden Auswirkungen ergeben, wenn sie verloren gehen oder zerstört werden. Beispiele für Informationen dieser Art sind: 

* E-Mails, die meist gelöscht oder verteilt werden können, ohne dass dies zu kritischen Situationen führt (mit Ausnahme von Postfächern oder E-Mails von Personen, die im Rahmen der vertraulichen Klassifizierung angegeben sind).  
* Dokumente und Dateien, die keine vertraulichen Daten enthalten.

In der Regel umfasst diese Klassifizierung alle Daten, die nicht vertraulich sind. Diese Klassifizierung kann den Großteil der geschäftlichen Daten umfassen, da die meisten Dateien, die täglich verwaltet oder genutzt werden, als sensibel klassifiziert werden können. Mit Ausnahme von Daten, die öffentlich zugänglich oder vertraulich sind, können alle Daten eines Unternehmens standardmäßig als „sensibel“ klassifiziert werden. 

#### <a name="public-unrestricted"></a>Öffentlich (nicht eingeschränkt)
Zu den Informationen, die als „Öffentlich“ klassifiziert sind, gehören Daten und Dateien, die für geschäftliche Anforderungen oder Vorgänge nicht kritisch sind. Zu dieser Klassifizierung können auch Daten gehören, die absichtlich für die Nutzung öffentlich gemacht wurden, z.B. Marketingmaterial oder Pressemitteilungen. Außerdem kann diese Klassifizierung beispielsweise von einem E-Mail-Dienst gespeicherte Spam-E-Mails umfassen. 

### <a name="define-data-ownership"></a>Definieren der Dateneigentümerschaft
Es ist wichtig, für alle Datenassets eine eindeutige Besitzerkette einzurichten. In der folgenden Tabelle sind verschiedene Rollen der Dateneigentümerschaft in Bezug auf die Datenklassifizierung und die dazugehörigen Rechte angegeben.  

| **Rolle** | **Erstellen** | **Ändern/Löschen** | **Delegieren** | **Lesen** | **Archivieren/Wiederherstellen** |
| --- | --- | --- | --- | --- | --- |
| Besitzer |X |X |X |X |X |
| Verwalter | | |X | | |
| Administrator | | | | |X |
| Benutzer\* | |X | |X | |

**Benutzern können von einem Verwalter zusätzliche Rechte gewährt werden, z.B. Bearbeiten und Löschen.* 

> [!NOTE]
> Diese Tabelle enthält keine vollständige Liste der Rollen und Rechte, sondern nur einen repräsentativen Ausschnitt. 
> 
> 

Der **Datenassetbesitzer** ist der ursprüngliche Ersteller der Daten, der die Eigentümerschaft delegieren und einen Verwalter zuweisen kann. Wenn eine Datei erstellt wird, sollte der Besitzer eine Klassifizierung zuweisen können. Dies bedeutet, dass Benutzer sich im Klaren darüber sein müssen, welche Daten basierend auf den Richtlinien der Organisation als vertraulich klassifiziert werden müssen. Alle Daten des Datenassetbesitzers können automatisch als „Nur für die interne Verwendung (sensibel)“ klassifiziert werden, es sei denn, dieser ist dafür verantwortlich, vertrauliche (eingeschränkte) Daten zu besitzen oder zu erstellen. In vielen Fällen ändert sich die Rolle des Besitzers, nachdem die Daten klassifiziert wurden. Beispielsweise kann der Besitzer eine Datenbank mit klassifizierten Informationen erstellen und die Rechte dafür an den Datenverwalter abgeben.  

> [!NOTE]
> Datenassetbesitzer nutzen häufig eine Mischung aus Diensten, Geräten und Medien, von denen einige persönlicher Art sind und einige dem Unternehmen gehören. Mit einer eindeutigen Unternehmensrichtlinie kann sichergestellt werden, dass die Nutzung von Geräten, z.B. Laptops und Smartphones oder ähnlichen Geräten, gemäß den Richtlinien für die Datenklassifizierung erfolgt.  
> 
> 

Der **Datenassetverwalter** wird vom Assetbesitzer (oder einer Person, an die diese Aufgabe delegiert wurde) zugewiesen, damit das Asset gemäß der Vereinbarung mit dem Assetbesitzer oder gemäß den geltenden Richtlinienanforderungen verwaltet werden kann. Idealerweise kann die Verwalterrolle in einem automatisierten System implementiert werden. Durch einen Assetverwalter wird sichergestellt, dass die erforderlichen Zugriffssteuerungen vorhanden sind, und er ist dafür verantwortlich, an ihn delegierte Assets zu verwalten und zu schützen. Für den Assetverwalter können folgende Zuständigkeiten gelten:  

* Schützen des Assets gemäß Anweisung des Assetbesitzers bzw. laut Vereinbarung mit dem Assetbesitzer 
* Sicherstellen der Einhaltung von Klassifizierungsrichtlinien 
* Informieren von Assetbesitzern über Änderungen an vereinbarten Kontrollen oder Schutzmaßnahmen, bevor diese Änderungen wirksam werden 
* Melden von Änderungen oder des Wegfalls der Zuständigkeiten des Assetverwalters an den Assetbesitzer 
* Ein **Administrator** ist ein Benutzer, der sicherstellen muss, dass die Integrität gewahrt wird, aber er ist kein Datenassetbesitzer, -verwalter oder -benutzer. Bei vielen Administratorrollen werden Datencontainer-Verwaltungsdienste bereitgestellt, ohne dass Zugriff auf die Daten besteht. Die Administratorrolle umfasst die Sicherung und Wiederherstellung von Daten, das Verwalten der Datensätze für die Assets und das Auswählen, Beschaffen und Betreiben der Geräte und des Speichers mit den Assets. 
* Als Assetbenutzer gelten alle Personen, denen der Zugriff auf die Daten oder eine Datei gewährt wird. Die Zugriffszuweisung wird vom Besitzer häufig an den Datenverwalter delegiert.  

### <a name="implementation"></a>Implementierung
Verwaltungsaspekte gelten für alle Klassifizierungsmethodiken. Diese Aspekte müssen Details (wer, was, wo, wann und warum) zu einem Datenasset und Informationen dazu enthalten, wie es verwendet, geändert oder gelöscht und wie darauf zugegriffen wird. Die gesamte Assetverwaltung muss mit dem Wissen durchgeführt werden, wie eine Organisation mit Risiken umgeht. Hierfür kann, wie im Prozess der Datenklassifizierung definiert, aber eine einfache Methodik angewendet werden. Weitere Aspekte der Datenklassifizierung umfassen die Einführung neuer Anwendungen und Tools sowie die Verwaltung von Änderungen nach der Implementierung einer Klassifizierungsmethode.  

### <a name="reclassification"></a>Neuklassifizierung
Die Neuklassifizierung oder Änderung des Klassifizierungszustands eines Datenassets muss durchgeführt werden, wenn ein Benutzer oder System ermittelt, dass sich die Wichtigkeit oder das Risikoprofil des Datenassets geändert hat. Dies ist wichtig, um sicherzustellen, dass der Klassifizierungsstatus weiterhin aktuell und gültig ist. Der Großteil des Inhalts, der nicht manuell klassifiziert wird, kann automatisch oder basierend auf der Nutzung durch einen Datenverwalter oder -besitzer klassifiziert werden. 

### <a name="manual-data-reclassification"></a>Manuelle Neuklassifizierung von Daten
Idealerweise wird hierbei sichergestellt, dass die Details einer Änderung erfasst und überprüft werden. Die wahrscheinlichsten Gründe für die manuelle Neuklassifizierung sind die Vertraulichkeit, das Vorhandensein von Datensätzen im Papierformat oder eine Anforderung zum Überprüfen von Daten, die ursprünglich falsch klassifiziert wurden. Da in diesem Artikel um die Datenklassifizierung und das Verschieben von Daten in die Cloud geht, müssten manuelle Neuklassifizierungen von Fall zu Fall geprüft werden. Außerdem wäre eine Überprüfung des Risikomanagements ideal, damit alle Anforderungen an die Klassifizierung erfüllt werden. Im Allgemeinen werden hierbei die Richtlinie der Organisation zum Klassifizierungsumfang, der standardmäßige Klassifizierungszustand (alle Daten und Dateien, die sensibel aber nicht vertraulich sind) und die Ausnahmeregelung für Hochrisikodaten herangezogen. 

### <a name="automatic-data-reclassification"></a>Automatische Neuklassifizierung der Daten
Bei der automatischen Neuklassifizierung der Daten wird die gleiche allgemeine Regel wie bei der manuellen Klassifizierung verwendet. Der Unterschied besteht darin, dass mit automatisierten Lösungen sichergestellt werden kann, dass Regeln eingehalten und bei Bedarf angewendet werden können. Die Datenklassifizierung kann im Rahmen einer Richtlinie zur Durchsetzung der Datenklassifizierung erfolgen. Sie kann angewendet werden, wenn Daten gespeichert, verwendet und übertragen werden, indem Autorisierungstechnologie eingesetzt wird.

* Anwendungsbasiert: Wenn standardmäßig bestimmte Anwendungen genutzt werden, wird eine Klassifizierungsebene festgelegt. Daten, die aus einer CRM-Software (Customer Relationship Management), Personaldaten-Software und aus Datenverwaltungstools stammen, sind standardmäßig vertraulich. 
* Standortbasiert: Der Datenstandort kann zur Identifizierung der Vertraulichkeit von Daten herangezogen werden. Daten, die von einer Personalabteilung oder Finanzabteilung gespeichert werden, sind eher vertraulicher Art.  

### <a name="data-retention-recovery-and-disposal"></a>Aufbewahrung, Wiederherstellung und Ausmusterung von Daten
Die Wiederherstellung und Ausmusterung von Daten ist, wie die Neuklassifizierung von Daten, ein wesentlicher Aspekt der Verwaltung von Datenassets. Die Prinzipien der Datenwiederherstellung und -entfernung werden durch eine Richtlinie zur Datenaufbewahrung definiert und genauso wie bei der Neuklassifizierung der Daten durchgesetzt. Diese Aufgabe wird von der Verwalter- und Administratorrolle gemeinsam übernommen.  

Wenn keine Richtlinie zur Datenaufbewahrung vorhanden ist, kann dies zu Datenverlusten oder zur Nichterfüllung von regulatorischen und rechtlichen Ermittlungsanforderungen führen. Die meisten Organisationen, die nicht über eine eindeutig definierte Richtlinie zur Datenaufbewahrung verfügen, nutzen häufig eine standardmäßige Aufbewahrungsrichtlinie der Art „alles wird aufbewahrt“. Eine Aufbewahrungsrichtlinie dieser Art ist bei Clouddienstszenarien aber mit zusätzlichen Risiken verbunden. 

Eine Richtlinie zur Datenaufbewahrung für Anbieter von Clouddiensten kann beispielsweise so angesehen werden, dass sie „für die Dauer des Abonnements“ gilt (solange für den Dienst bezahlt wird, werden die Daten aufbewahrt). Bei dieser Vereinbarung der Art „Bezahlung für Aufbewahrung“ werden Aufbewahrungsrichtlinien des Unternehmens oder regulatorische Aufbewahrungsrichtlinien ggf. nicht berücksichtigt. Durch das Definieren einer Richtlinie für vertrauliche Daten kann sichergestellt werden, dass Daten basierend auf bewährten Methoden gespeichert und entfernt werden. Außerdem kann eine Archivierungsrichtlinie erstellt werden, um formell festzulegen, welche Daten zu welchem Zeitpunkt entfernt werden sollen. 

Mit der Richtlinie für die Datenaufbewahrung sollten die erforderlichen regulatorischen und auf die Compliance bezogenen Anforderungen sowie die rechtlichen Aufbewahrungsanforderungen des Unternehmens abgedeckt werden. Bei klassifizierten Daten können sich Fragen zur Aufbewahrungsdauer und zu Ausnahmen für Daten ergeben, die bei einem Anbieter gespeichert sind. Fragen dieser Art werden häufiger bei Daten gestellt, die nicht richtig klassifiziert wurden. 

> [!TIP]
> Weitere Informationen zu Aufbewahrungsrichtlinien für Azure-Daten und weiteren Punkten finden Sie unter [Microsoft Online-Abonnementvertrag](https://azure.microsoft.com/support/legal/subscription-agreement/).
> 
> 

## <a name="protecting-confidential-data"></a>Schützen vertraulicher Daten
Nach dem Klassifizieren von Daten ist das Suchen nach und Implementieren von Wegen zum Schützen vertraulicher Daten ein integraler Bestandteil jeder Strategie zum Schutz von Daten. In Bezug auf den Schutz vertraulicher Daten muss auch beachtet werden, wie Daten in herkömmlichen Architekturen und in der Cloud gespeichert und übertragen werden. 

Dieser Abschnitt enthält grundlegende Informationen zu einigen Technologien, mit denen die Durchsetzung automatisiert werden kann, um als „Vertraulich“ klassifizierte Daten zu schützen. 

Wie in der folgenden Abbildung zu sehen ist, können diese Technologien als lokale oder cloudbasierte Lösungen bereitgestellt werden – oder als Hybrid-Lösung, bei der die Bereitstellung teilweise lokal und teilweise in der Cloud erfolgt. (Einige Technologien, z.B. die Verschlüsselung und Rechteverwaltung, erstrecken sich auch auf Benutzergeräte.)  

![Technologien](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Rights Management-Software
Eine Lösung zur Verhinderung von Datenverlusten ist die Rights Management-Software. Im Gegensatz zu Ansätzen, bei denen versucht wird, den Informationsfluss an Ausgangspunkten in einer Organisation zu unterbrechen, agiert die Rights Management-Software auf tieferen Ebenen von Technologien für die Datenspeicherung. Dokumente werden verschlüsselt, und bei der Steuerung, von wem sie entschlüsselt werden können, werden Zugriffssteuerelemente verwendet. Diese werden in einer Lösung für die Authentifizierungssteuerung definiert, z.B. einem Verzeichnisdienst.  

> [!TIP]
> Sie können Azure Rights Management (Azure RMS) als Lösung für den Schutz von Informationen verwenden, um Daten in unterschiedlichen Szenarien zu schützen. Weitere Informationen zu dieser Azure-Lösung finden Sie unter [Was ist Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms).
> 
> 

Beispiele für Vorteile der Rights Management-Software: 

* Sensible Informationen sind geschützt. Benutzer können ihre Daten direkt mit Anwendungen schützen, die für Rights Management geeignet sind. Es sind keine zusätzlichen Schritte erforderlich: Die Erstellung von Dokumenten, das Senden von E-Mails und das Veröffentlichen von Daten ist in einer einheitlichen Schutzumgebung möglich. 
* Der Schutz wird quasi mit den Daten übertragen. Kunden behalten die Kontrolle darüber, wer Zugriff auf die Daten hat – ob in der Cloud, der vorhandenen IT-Infrastruktur oder auf dem Desktop des Benutzers. Organisationen können ihre Daten verschlüsseln und den Zugriff darauf gemäß ihren geschäftlichen Anforderungen beschränken. 
* Standardmäßige Richtlinien für den Schutz von Informationen. Administratoren und Benutzer können Standardrichtlinien für viele gängige Geschäftsszenarien nutzen, z.B. „Firma (vertraulich) – Schreibgeschützt“ und „Nicht weiterleiten“. Es werden umfassende Nutzungsrechte unterstützt, z.B. Lesen, Kopieren, Drucken, Speichern, Bearbeiten und Weiterleiten, um für Flexibilität beim Definieren von benutzerdefinierten Nutzungsrechten zu sorgen. 

> [!TIP]
> Sie können Daten in Azure Storage schützen, indem Sie [Azure Storage Service Encryption](../storage/storage-service-encryption.md) für ruhende Daten verwenden. Außerdem können Sie [Azure Disk Encryption](azure-security-disk-encryption.md) verwenden, um Daten zu schützen, die auf virtuellen Datenträgern für Azure Virtual Machines gespeichert sind.
> 
> 

### <a name="encryption-gateways"></a>Verschlüsselungsgateways
Verschlüsselungsgateways werden auf eigenen Schichten betrieben, um Verschlüsselungsdienste bereitzustellen, indem der gesamte Zugriff auf cloudbasierte Daten umgeleitet wird. Dieser Ansatz sollte nicht mit einem virtuellen privaten Netzwerk (Virtual Private Network, VPN) verwechselt werden. Verschlüsselungsgateways sind so konzipiert, dass sie eine transparente Schicht für cloudbasierte Lösungen bieten.   

Verschlüsselungsgateways können ein Mittel zum Verwalten und Schützen von Daten sein, die als „Vertraulich“ klassifiziert wurden, indem sowohl Daten während der Übertragung als auch ruhende Daten verschlüsselt werden.  

Verschlüsselungsgateways werden im Datenfluss zwischen Benutzergeräten und Anwendungsrechenzentren angeordnet, um Dienste für die Ver- und Entschlüsselung bereitzustellen. Bei diesen Lösungen handelt es sich, wie bei VPNs auch, hauptsächlich um lokale Lösungen. Sie sind dafür ausgelegt, Dritten die Kontrolle über Verschlüsselungsschlüssel zu geben. So wird das Risiko verringert, das entsteht, wenn ein Anbieter sowohl mit der Verwaltung der Daten als auch mit der Verwaltung der Schlüssel betraut wird. Lösungen dieser Art sind, ähnlich wie die Verschlüsselung, so konzipiert, dass sie zwischen Benutzern und dem Dienst nahtlos und transparent funktionieren. 

> [!TIP]
> Sie können Azure ExpressRoute verwenden, um Ihre lokalen Netzwerke über eine dedizierte private Verbindung auf die Microsoft Cloud auszudehnen. Weitere Informationen zu dieser Funktion finden Sie unter [ExpressRoute – Technische Übersicht](../expressroute/expressroute-introduction.md). Eine andere Option für die plattformübergreifende Verbindung zwischen Ihrem lokalen Netzwerk und Azure ist eine [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
> 
> 

### <a name="data-loss-prevention"></a>Verhindern von Datenverlusten
Es ist wichtig, den Datenverlustaspekt (auch als Datenleck bezeichnet) zu berücksichtigen. Die Verhinderung von externen Datenverlusten aufgrund von böswilligen oder versehentlich handelnden Insidern ist für viele Unternehmen von großer Bedeutung.  

Mit Technologien zur Verhinderung von Datenverlusten (Data Loss Prevention, DLP) kann sichergestellt werden, dass Lösungen wie E-Mail-Dienste keine Daten übertragen, die als „Vertraulich“ klassifiziert wurden. Organisationen können DLP-Funktionen in vorhandenen Produkten nutzen, um Datenverlust zu verhindern. Für diese Funktionen werden Richtlinien verwendet, die leicht von Grund auf neu erstellt werden können, oder Sie können eine vom Softwareanbieter bereitgestellte Vorlage nutzen.  

Mit DLP-Technologien können eingehende Inhaltsanalysen per Schlüsselwortabgleich, Wörterbuchabgleich, Auswertung regulärer Ausdrücke und anderen Inhaltsuntersuchungen durchgeführt werden, um Inhalte zu erkennen, die zu Verletzungen der DLP-Richtlinien des Unternehmens führen. Beispielsweise kann mit DLP dazu beigetragen werden, dass der Verlust der folgenden Datentypen verhindert wird: 

* Sozialversicherungsnummer und Personalausweisnummer 
* Bankdaten 
* Kreditkartennummer  
* IP-Adressen 

Bei einigen DLP-Technologien besteht auch die Möglichkeit, die DLP-Konfiguration außer Kraft zu setzen (wenn eine Organisation beispielsweise die US-Sozialversicherungsnummer an ein Unternehmen übertragen muss, das mit der Gehaltsabrechnung beauftragt ist). Außerdem ist es möglich, DLP so zu konfigurieren, dass Benutzer schon vor dem Versuch benachrichtigt werden, sensible Informationen zu senden, die nicht übertragen werden dürfen. 

> [!TIP]
> Sie können DLP-Funktionen von Office 365 nutzen, um Ihre Dokumente zu schützen. Weitere Informationen finden Sie unter [Office 365 compliance controls: Data Loss Prevention (in englischer Sprache)](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/).
> 
> 

## <a name="see-also"></a>Siehe auch
* [Azure-Datenverschlüsselung – Bewährte Methoden](azure-security-data-encryption-best-practices.md)
* [Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung – Bewährte Methoden](azure-security-identity-management-best-practices.md)
* [Blog des Azure-Sicherheitsteams](http://blogs.msdn.com/b/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)




<!--HONumber=Nov16_HO3-->


