<properties
   pageTitle="Azure SQL-Datenbank – Azure-Fallstudie: SnelStart | Microsoft Azure"
   description="Erfahren Sie, wie SnelStart seine Businessdienste dank SQL-Datenbank schnell um 1.000 neue Azure SQL-Datenbanken pro Monat erweiterte."
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>

# Dank Azure erweiterte SnelStart seine Businessdienste schnell um 1.000 neue Azure SQL-Datenbanken pro Monat

![Logo](./media/sql-database-case-study-snelstart/snelstartlogo.png)

SnelStart entwickelt beliebte Software für das Finanz- und Businessmanagement für kleine und mittelgroße Unternehmen in den Niederlanden. 110 Mitarbeiter (einschließlich 35 IT-Mitarbeitern) kümmern sich um 55.000 Kunden. Durch die Umstellung von Desktopsoftware zu einem auf Azure aufgebauten SaaS-Angebot (Software-as-a-Service) kann SnelStart die integrierten Dienste optimal nutzen. Die Verwaltung wird mit einer vertrauten Umgebung in C# automatisiert, und dank elastischer Datenbankpools lassen sich Leistung und Skalierbarkeit optimieren, ohne Über- oder Unterdimensionierung von Ressourcen für die Kunden. Azure bietet SnelStart die Flexibilität und Agilität, Kundendaten zwischen lokalen Systemen und der Cloud zu verschieben.

> [AZURE.VIDEO azure-sql-database-case-study-snelstart]

##Warum SnelStart seine Dienste vom Desktop in die Cloud erweiterte

> „Durch die Arbeit mit Azure können wird unsere Software schneller bereitstellen, umgehend auf Kundenanforderungen reagieren und Lösungen skalieren, wenn die Anforderungen steigen.“

> – Henry Been, Software Architect

SnelStart ist seit Jahren ein erfolgreiches Softwareunternehmen, das ein herkömmliches Entwicklungsmodell einsetzte: Code, Verpacken, Liefern – und dann das Ganze wieder von vorn. Im Lauf der Zeit traten Veränderungen immer schneller ein. Vorschriften änderten sich häufig, und Kunden benötigten einfachere Möglichkeiten, Finanzdaten zu verarbeiten und mit der Buchhaltung und den Behörden zusammenzuarbeiten, um mit diesen Veränderungen Schritt zu halten.

„Die Auslieferung der Software an die Kunden war teuer und schränkte uns ein“, erklärt Henry Been, Software Architect bei SnelStart. „Aufgrund der Kosten für Produktion, Verpackung und Lieferung konnten wir Softwarereleases nicht so häufig ausliefern, wie wir wollten. Wir mussten Updates für regelmäßige Lieferungen packen, dadurch wurde es aber schwierig, die wechselnden Anforderungen unserer Kunden zu erfüllen. Wir konnten nie sicher sein, dass unsere Kunden die Upgrades auf die neueste Produktversion tatsächlich ausführten. Daher mussten wir mehrere Versionen unterstützen, was den Support erschwerte.“

Been fügt hinzu: „Wir brauchten eine Möglichkeit, Updates schneller zu planen und freizugeben, um Innovationen schneller auf den Markt zu bringen und neue Dienste für unsere Kunden entwickeln zu können. Wir wollten auch eine größere Anzahl von Prozessen automatisieren, um die Businessadministration für unsere Kunden zu vereinfachen.“

Für SnelStart bestand die Lösung darin, ein cloudbasierter SaaS-Anbieter zu werden, um die Dienste des Unternehmens zu erweitern. Dieses Ziel konnte SnelStart mit der Azure SQL-Datenbankplattform erreichen, ohne den großen IT-Mehraufwand, den eine IaaS-Lösung (Infrastructure-as-a-Service) erfordert hätte.

Dank des Cloudmodells kann SnelStart auch schnell Fehler beheben und neue Funktionen bereitstellen, und die Kunden müssen keine Software herunterladen oder aktualisieren. Henry Been führt weiter aus: „Mit den Azure-Clouddiensten können wir innerhalb kürzester Zeit auf veränderte Anforderungen anderer Parteien reagieren. Anstatt eine neue Version an Tausende Kunden liefern zu müssen, können wir Informationen, die von unserer Desktopanwendung gesendet werden, an neue, von anderen Parteien geforderte Formate anpassen.“

##Ein modernes Unternehmen mit traditionellen Wurzeln

SnelStart ist ein modernes, agiles High-Tech-Unternehmen, das 1964 als Hersteller von Teilen für Musikinstrumente einen eher bescheidenen Anfang nahm. Das Softwarebusiness von SnelStart begann erst in den 1980er-Jahren richtig Fahrt aufzunehmen, als PCs immer größere Verbreitung fanden. Das Unternehmen benötigte eine bessere Alternative zu der Buchhaltungssoftware, die zu dieser Zeit erhältlich war, und nahm die Sache einfach selbst in die Hand. 1982 legte das Unternehmen den Grundstein zu dem, was am Ende die SnelStart-Buchhaltungssoftware werden sollte. Die Software wurde von Anfang an für ihre Einfachheit und Schnelligkeit gelobt – in einem solchen Maß, dass SnelStart schließlich seinen Schwerpunkt verlagerte und sich als Softwareunternehmen neu erfand.

1995 veröffentlichte SnelStart seine erste Buchhaltungsanwendung für Windows. Die auf Microsoft Visual Basic 1.0 und Microsoft Access-Datenbanken aufgebaute Anwendung trug maßgeblich dazu bei, dass der Kundenstamm auf mehr als 5.000 Benutzer anwuchs.

Heute ist SnelStart ein wichtiger Anbieter einer Line-of-Business- und Businessadministrationssoftware für kleine und mittelgroße Unternehmen und Selbstständige in den Niederlanden. Carlo Kuip, IT Architect, sagt: „Unser Ziel ist eine 100-prozentige Automatisierung der Businessadministrationsdienste für unsere Kunden.“

##Optimieren von Leistung und Kosten mit elastischen Pools

SnelStart war eines der ersten Unternehmen, die in großem Umfang elastische Datenbankpools eingesetzt haben. Mit elastischen Pools kann das Unternehmen Kosten senken und die Leistungsanforderungen effizienter verwalten. Wie Henry Been sagt: „Durch den Einsatz von elastischen Datenbankpools können wir die Leistung je nach Anforderungen unserer Kunden ohne Überdimensionierung optimieren. Es wäre ziemlich teuer, wenn wir Ressourcen basierend auf Spitzenlasten bereitstellen müssten. Stattdessen können wir dank der Option, dass Ressourcen durch mehrere Datenbanken mit geringer Auslastung gemeinsam genutzt werden, eine Lösung erstellen, die gut funktioniert und kostengünstig ist.“

##Azure SQL-Datenbanken unterstützen das Packen von Daten in Container zu Isolations- und Sicherheitszwecken 

Mit Azure SQL-Datenbank kann SnelStart die lokalen Businessadministrationsdaten der Kunden schnell und transparent nach Azure verschieben. Eine Azure SQL-Datenbank ist ein praktischer Container, der für Isolation sorgt, eine Grenze für die Authentifizierung und Autorisierung bereitstellt und einfache Sicherungs- und Wiederherstellungsoptionen bietet. Datenbanken stellen ein geeignetes konzeptionelles Modell für die Businessadministration bereit. Carlo Kuip, IT Architect, erläutert: „Elemente innerhalb der Containergrenze enthalten vertrauliche Daten, die von entscheidender Bedeutung für das Business sind. Das Speichern dieser Elemente in einer isolierten Datenbank bietet einen hohen Schutz. Wir können die Autorisierung auf Datenbankebene verwalten und die Verwaltung und Skalierung dieser Datenbanken sogar automatisieren, ohne dass wir dafür Datenbankadministratoren einstellen müssten.“

Azure SQL Data Warehouse spielt für die Sicherheit und Verwaltung bei SnelStart ebenfalls eine wichtige Rolle, denn damit kann das Unternehmen Telemetriedaten erfassen, wie beispielsweise zur Angriffserkennung, zur Protokollierung von Benutzeraktivitäten und zur Erfassung der Konnektivität.

##Azure reduziert den Aufwand, damit Entwickler sich auf das Wesentliche konzentrieren können 

Das Azure-Plattformmodell eliminierte unnötigen Infrastrukturaufwand und versetzte SnelStart in die Lage, Bereitstellungen mithilfe von C#-Verwaltungsbibliotheken zu automatisieren. Kuip führt weiter aus: „Wir konnten unseren aktuellen operativen Betrieb mit einer sehr geringen Anzahl von Mitarbeitern erweitern und gleichzeitig Skalierbarkeit, Geschwindigkeit und Notfallwiederherstellungsoptionen für unsere Kunden verbessern. Durch den Umstieg auf ein dienstbasiertes Modell können sich unsere Ressourcen vermehrt auf neue Dienste und Funktionen konzentrieren und müssen sich nicht mehr damit beschäftigen, vorhandenen Code zu aktualisieren, um neue Vorschriften oder Steuerschlüssel einzupflegen.“ Er ergänzt: „Durch die Automatisierung der Verwaltung und die Nutzung des SaaS-Angebots können wir unseren Kunden mehr Nutzen bieten – ohne umfangreiche Investitionen in Betriebspersonal.“ Durch Einsatz von Azure und elastischen Datenbankpools konnte SnelStart eine Vielzahl neuer Funktionen anbieten, beispielsweise eine stabilere Integration der Kundendaten in Banksysteme, neue Abrechnungsdienste, Backgroundchecks für Kleinunternehmen und E-Mail-Dienste.

> „Allein in den ersten Monaten des Jahrs 2016 konnten wir unsere Azure SQL-Datenbankbereitstellungen von etwa 5.500 auf über 12.000 erweitern, und zurzeit fügen wir jeden Monat ca. 1.000 Datenbanken hinzu.“

> – Henry Been, Software Architect

Die Datenbankverwaltung wird durch die Funktion für elastische Aufträge weiter automatisiert. Kuip erläutert: „Wir wissen die automatische Ermittlung von Datenbanken auf einer SQL-Datenbank-Serverinstanz sehr zu schätzen.“ So kann SnelStart Verwaltungsvorgänge ohne zusätzlichen Aufwand über den gesamten, dynamisch wachsenden Kundenstamm hinweg ausführen.

SnelStart entwickelt auch eine API, die als Broker zwischen Kundendaten und von Drittanbieterpartnern entwickelten Apps fungiert. Kuip führt weiter aus: „Mit dieser API können andere Anbieter Funktionen zu unserer Software hinzufügen, beispielsweise um die mühsame Dateneingabe für Rechnungen und andere Dokumente zu vermeiden.“ Kunden müssen Rechnungen nicht mehr manuell in die Buchhaltungssoftware ihres Kleinunternehmens eingeben – die SnelStart-Software sorgt für den direkten Austausch der notwendigen Informationen. So können Kunden ihre Businessadministrationsaufgaben mit dem Ökosystem aus Informationen verknüpfen, das durch die digitale Transformation der Branche entsteht.

##So ermöglichen Azure-Dienste SaaS für SnelStart

Dank Azure kann SnelStart seine Kunden und ihre Buchhaltungsabteilungen in der Cloud nahtloser bedienen. Ein Beispiel: Sowohl Kunden als auch die Buchhaltungsabteilungen können Informationen austauschen, indem sie direkt auf die Client-API von SnelStart zugreifen, die in Azure gehostet wird. Carlo Kuip beschreibt es so: „Diese wiederverwendbaren Dienste sind in unseren kundenorientierten Web-Apps verfügbar und stellen eine gemeinsame Infrastruktur und gemeinsame Funktionen bereit, um den Kunden Zugriff auf die Businessadministration und auf Drittanbietersoftware zu gewähren, die von unseren Kunden verwendet wird. Einige Beispiele hierfür sind Funktionen für die Produktkonfiguration, die Verwaltung von Firewallregeln und die Verwaltung von langfristigen Prozessen wie etwa Sicherungen.“

> „Unser Ziel ist eine 100-prozentige Automatisierung der Businessadministrationsdienste für unsere Kunden.“

> – Carlo Kuip, IT Architect

Darüber hinaus ermöglichen die Webdienste von SnelStart Kunden und Buchhaltungsabteilungen den einfachen Zugriff auf Daten in den elastischen Pools von Azure SQL-Datenbank. Dieses SaaS-Modell in Verbindung mit Datenbankelastizität und Azure Resource Manager bietet SnelStart Skalierbarkeitsfunktionen, die jede Azure-Bereitstellung ergänzen. Die Implementierung wird mithilfe von C#-Verwaltungsbibliotheken vollständig automatisiert.

![Abbildung 1](./media/sql-database-case-study-snelstart/figure1.png)

Abbildung 1. Seit Juni 2016 verwaltet SnelStart mehr als 11.000 Datenbanken und über 50 elastische Datenbankpools
 
##Einfachheit dank Cloud

Seit dem Wechsel zu einer auf der Azure-Cloud basierenden Lösung konnte SnelStart das schnelle Kundenwachstum problemlos unterstützen und gleichzeitig innovative Funktionen und Dienste anbieten. Henry Been sagt: „Dank Azure können wir Updates für unsere Kunden nahezu kontinuierlich bereitstellen, ohne mehr Mitarbeiter für den Betrieb zu benötigen. Und wir bekommen all die anderen tollen Azure-Funktionen – wie z.B. Skalierbarkeit und Notfallwiederherstellung – direkt im Paket mitgeliefert.“

> „Als wir drüben in Redmond waren, rief mich einer der Entwickler in den Niederlanden wegen eines bestimmten Problems an. Microsoft konnte innerhalb von 48 Stunden eine Änderung an der Microsoft-Produktionsumgebung vornehmen , um unser Problem zu lösen.“

> – Henry Been, Software Architect

SnelStart schätzt die enge Partnerschaft, die das Unternehmen mit dem Microsoft Azure SQL-Datenbank-Team aufgebaut hat. Kuip formuliert es so: „Wir diskutieren über Funktionen und wie wir Technologie einsetzen – diese Gespräche sind für beide Seiten wertvoll.“ Das unmittelbare Ziel für SnelStart ist es, den Stamm an zufriedenen Kunden ständig zu erweitern. Been sagt: „Ohne die technischen und ressourcenbezogenen Einschränkungen, mit denen wir als unabhängiger Softwarehersteller zu kämpfen hatten, steht unserem Wachstum jetzt nichts mehr im Wege.“


## Weitere Informationen

- Weitere Informationen zu elastischen Azure-Datenbankpools finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

- Weitere Informationen zu Webrollen und Workerrollen finden Sie unter [Workerrollen](../fundamentals-introduction-to-azure.md#compute).

- Weitere Informationen zu Azure SQL Data Warehouse finden Sie unter [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/).

- Weitere Informationen zu SnelStart finden Sie unter [SnelStart](http://www.snelstart.nl).

<!---HONumber=AcomDC_0921_2016-->