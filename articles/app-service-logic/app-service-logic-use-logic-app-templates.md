<properties
 pageTitle="Logik-App-Vorlagen | Microsoft Azure"
 description="Erfahren Sie, wie Sie zuvor erstellte Logik-App-Vorlagen verwenden, die Ihnen beim Einstieg helfen."
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="klam"/>

# Logik-App-Vorlagen

## Verwenden von Logik-App-Vorlagen

Eine Logik-App-Vorlage ist eine vorgefertigte Logik-App, mit der Sie schnell mit dem Erstellen Ihrer eigenen Workflows beginnen können.

Diese Vorlagen eignen sich gut, um verschiedene Muster kennenzulernen, die mit Logik-Apps erstellt werden können. Sie können diese Vorlagen unverändert nutzen oder Ihrem Szenario anpassen.

## Übersicht über die verfügbaren Vorlagen

Auf der Logik-App-Plattform wurden bereits zahlreiche Vorlagen veröffentlicht. Einige Beispielkategorien sowie die darin aufgeführten Connectorarten sind nachfolgend aufgeführt.

### Enterprise Cloud-Vorlagen
Vorlagen für die Integration von Dynamics CRM, Salesforce, Box, Azure Blob und anderen Connectors für die Anforderungen Ihrer Unternehmenscloud. Mit diesen Vorlagen können Sie beispielsweise Ihre Leads organisieren und Ihre Unternehmensdateidaten sichern.

### Enterprise Integration Pack-Vorlagen
Konfigurationen von VETER-Pipelines (validate, extract, transform, enrich, route: überprüfen, extrahieren, transformieren, erweitern, weiterleiten) die ein X12 EDI-Dokument über AS2 erhalten und in XML umwandeln sowie X12 und AS2-Nachrichten verarbeiten.

### Vorlagen für Protokollmuster
Diese Vorlagen bestehen aus Logik-Apps, die Protokollmuster wie die Anforderungsantwort über HTTP sowie Integrationen über FTP und SFTP enthalten. Verwenden Sie diese im vorhandenen Zustand oder als Grundlage für die Erstellung komplexerer Protokollmuster.

### Persönliche Produktivitätsvorlagen
Vorlagen zum Steigern der persönlichen Produktivität enthalten Vorlagen, mit denen Sie tägliche Erinnerungen festlegen, Arbeitslisten für wichtige Arbeitsaufgaben erstellen und langwierige Aufgaben so automatisieren, dass nur noch die Genehmigung des Benutzers benötigt wird.

### Vorlagen für die Cloud für Consumer
Einfache Vorlagen für die Integration in Social Media Services wie Twitter, Slack und E-Mail, mit denen Sie Marketing-Initiativen in sozialen Medien verstärken können. Dazu zählen auch Vorlagen zum Cloudkopieren, mit denen Sie die Produktivität steigern, da Sie die Zeit für wiederkehrende Aufgaben sparen.

## Erstellen einer Logik-App mithilfe einer Vorlage 

Um mit der Verwendung von Logik-App-Vorlagen zu beginnen, wechseln Sie zum Logik-App-Designer. Wenn Sie den Designer über eine vorhandene Logik-App öffnen, wird die Logik-App automatisch in Ihrer Designeransicht geladen. Wenn Sie jedoch eine neue Logik-App erstellen, sehen Sie den folgenden Bildschirm. ![](../../includes/media/app-service-logic-templates/template7.png)

In diesem Bildschirm können Sie wahlweise mit einer leeren Logik-App oder einer vorgefertigten Vorlage beginnen. Wenn Sie eine der Vorlagen auswählen, erhalten Sie weitere Informationen darüber. In diesem Beispiel verwenden wir die Vorlage *When a new file is created in Dropbox, copy it to OneDrive* (Wenn eine neue Datei in Dropbox erstellt wird, in OneDrive kopieren). ![](../../includes/media/app-service-logic-templates/template2.png)

Wenn Sie die Vorlage verwenden möchten, wählen Sie einfach die Schaltfläche *Verwenden der Vorlage*. Je nachdem, welche Connectors die Vorlage verwendet, werden Sie zur Anmeldung bei Ihren Konten aufgefordert. Wenn Sie zuvor bereits eine Verbindung zu diesen Connectors hergestellt haben, können Sie wie folgt fortfahren. ![](../../includes/media/app-service-logic-templates/template3.png)

Wenn die Verbindung hergestellt wurde und Sie *Weiter* ausgewählt haben, wird die Logik-App in der Designeransicht geöffnet. ![](../../includes/media/app-service-logic-templates/template4.png)

Im obigen Beispiel sind (wie bei vielen Vorlagen) einige der erforderlichen Eigenschaftsfelder in den Connectors ausgefüllt. Jedoch muss möglicherweise noch ein Wert eingegeben werden, bevor die Logik-App ordnungsgemäß bereitgestellt werden kann. Wenn Sie versuchen, sie ohne Ausfüllen der Pflichtfelder bereitzustellen, wird eine Fehlermeldung angezeigt.

Wenn Sie in die Vorlagenansicht zurückkehren möchten, wählen Sie die Schaltfläche *Vorlagen* in der oberen Navigationsleiste. Wenn Sie zur Vorlagenansicht zurückwechseln, werden nicht gespeicherte Fortschritte verworfen. Bevor Sie zur Vorlagenansicht zurückwechseln, werden Sie mit einer Meldung davor gewarnt. ![](../../includes/media/app-service-logic-templates/template5.png)

## Bereitstellen einer Logik-App aus einer Vorlage

Wenn Sie Ihre Vorlage geladen und alle gewünschten Änderungen vorgenommen haben, wählen Sie die Schaltfläche „Speichern“ in der oberen linken Ecke. So wird Ihre Logik-App gespeichert veröffentlicht. ![](../../includes/media/app-service-logic-templates/template6.png)

Wenn Sie weitere Informationen über das Hinzufügen weiterer Schritte zu einer vorhandenen Logik-App-Vorlage oder das Bearbeiten im Allgemeinen wünschen, können Sie unter [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md) mehr erfahren.

<!---HONumber=AcomDC_0831_2016-->