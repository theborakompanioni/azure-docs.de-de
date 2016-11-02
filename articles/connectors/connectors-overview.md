<properties
	pageTitle="Übersicht über Logic Apps-Connectors | Microsoft Azure"
	description="Übersicht über Connectors, die in einer Logik-App verwendet werden können"
	services=""
	documentationCenter="" 
	authors="jeffhollan"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# Verwenden von Connectors in einer Logik-App

Connectors ermöglichen den schnellen dienst-, protokoll- und plattformübergreifenden Zugriff auf Ereignisse, Daten und Aktionen. Eine umfassende Liste mit von Logic Apps unterstützten Connectors finden Sie [hier](apis-list.md). Connectors können als Trigger oder Aktion in einer Logik-App verwendet werden und benötigen unter Umständen eine konfigurierte *Verbindung* (beispielsweise bei der Autorisierung eines Twitter-Kontos für den Zugriff oder die Beitragsveröffentlichung in Ihrem Namen).

## Grundlagen

Connectors sind gehostete Dienste, auf die Sie im Rahmen einer Logik-App zugreifen können, um eine Integration mit anderen Diensten wie Dynamics, Azure, Salesforce [und anderen](apis-list.md) zu erreichen. Sie werden von Microsoft bereitgestellt und verwaltet, sodass Sie Ihre Integrationsworkflows erstellen können, ohne sich um Aspekte wie Skalierung, Durchsatz und Sicherheit kümmern zu müssen. Sie können einer Logik-App einen Connector hinzufügen, indem Sie unter **Verwaltete Microsoft-APIs anzeigen** nach einer Connector-Aktion oder nach einem Connector-Trigger suchen und sie bzw. ihn auswählen.

![Aktionsmenü für die Triggerauswahl][1]

Connector-Aktionen und -Trigger verfügen jeweils über einen eigenen Satz an konfigurierbaren Eigenschaften. Über die Informationsschaltflächen erhalten Sie weitere Informationen zur jeweiligen Aktion. Alternativ können Sie sich auch in der Referenzdokumentation [ausführlicher informieren](apis-list.md).

Falls Sie eine Dienst- oder API-Integration implementieren möchten und es sich bei dem Dienst oder der API noch nicht um einen Connector handelt, können Sie Logik-Apps auch durch einen [benutzerdefinierten Connector](../app-service-logic/app-service-logic-create-api-app.md) erweitern oder den Dienst direkt über ein Protokoll wie HTTP aufrufen.

## Trigger

Einige Connectors verfügen über einen Trigger. Das bedeutet, dass ein Ereignis dieses Connectors eine Logik-App auslöst und im Rahmen des Triggers ggf. Daten übergibt. Ein Trigger ist immer der erste Schritt in einer Logik-App. Beispiele für beliebte Trigger wären etwa:
 
 * Wiederholung – stündlich ausführen
 * Wenn eine HTTP-Anforderung empfangen wird
 * Wenn ein Element einer Warteschlange hinzugefügt wird
 * Wenn eine E-Mail empfangen wird
 
Einige Trigger werden bei Eintritt eines Ereignisses umgehend über eine Benachrichtigung der Logik-App ausgelöst. Für andere muss ein Wiederholungsintervall konfiguriert werden, um anzugeben, wie oft die Logik-App den Dienst auf ein Ereignis überprüfen soll (bis zu alle 15 Sekunden).

Sobald ein Ereignis empfangen wird, wird die ausgeführte Logik-App ausgelöst, und die Aktionen im Workflow werden gestartet. Über den Trigger können Sie auch auf beliebige Daten im gesamten Workflow zugreifen. So übergibt beispielsweise der Trigger „On a new tweet“ (Bei neuem Tweet) den Tweet an die Ausführung.

## Aktionen

Die meisten Connectors verfügen über mindestens eine Aktion, die im Rahmen des Workflows ausgeführt werden kann. Aktionen sind Schritte, die auftreten, nachdem die Ausführung durch einen Trigger ausgelöst wurde. Klicken Sie zum Hinzufügen einer Aktion auf die Schaltfläche **Neuer Schritt**, und suchen Sie nach dem gewünschten Connector. Nach dem Auswählen des Connectors (und dem Konfigurieren der ggf. erforderlichen [Verbindungen](#connections)) wird die konfigurierbare Aktionskarte angezeigt. Sie können Daten aus vorherigen Schritten auswählen, indem Sie auf eines der Token für Ausgaben klicken, oder nach Bedarf andere Konfigurationen eingeben.

![Konfigurieren eine Connector-Aktion][2]

## Verbindungen

Die meisten Connectors können erst nach dem Konfigurieren einer *Verbindung* verwendet werden. Eine *Verbindung* ist eine beliebige Anmeldungs- oder Verbindungskonfiguration, die für den Zugriff auf den Connector benötigt wird. Für Connectors, die OAuth verwenden, bedeutet das Erstellen einer Verbindung die Anmeldung beim Dienst (wie Office 365, Salesforce oder GitHub). Das Zugriffstoken kann hierbei verschlüsselt und sicher in einem geheimen Azure-Speicher gespeichert werden. Andere Connectors (wie etwa FTP und SQL) erfordern eine Verbindung mit Konfigurationsangaben wie Serveradresse, Benutzername und Kennwort. Diese Verbindungskonfigurationsdetails werden ebenfalls verschlüsselt und sicher gespeichert. Verbindungen können so lange auf den Dienst zugreifen, wie der Dienst dies zulässt. Bei Azure Active Directory-OAuth-Verbindungen (wie Office 365 und Dynamics) können wir das Zugriffstoken weiterhin unbefristet aktualisieren. Bei anderen Diensten ist unter Umständen nach einer gewissen Zeit eine Aktualisierung des Tokens erforderlich. Durch bestimmte Aktionen wie das Ändern eines Kennworts werden im Allgemeinen sämtliche Zugriffstoken ungültig.

Verbindungen können in Azure unter **Durchsuchen** > **API Connections** (API-Verbindungen) angezeigt und verwaltet werden. Über die Ressource „API Connections“ (API-Verbindungen) können Sie sämtliche von Ihnen erstellte Verbindungen anzeigen, bearbeiten, aktualisieren und erneut autorisieren.

## Nächste Schritte

- [Erstellen Ihrer ersten Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Kennenlernen allgemeiner Szenarien und Beispiele für Logik-Apps](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Erste Schritte mit Triggern und Aktionen für die Unternehmensintegration](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png

<!---HONumber=AcomDC_0727_2016-->