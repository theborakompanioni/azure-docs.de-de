---
title: "Überwachen von Änderungen an einem virtuellen Computer – Azure Event Grid und Logic Apps | Microsoft-Dokumentation"
description: "Überprüfen auf Konfigurationsänderungen an virtuellen Computern (VMs) mit Azure Event Grid und Logic Apps"
keywords: logic apps, event grids, virtual machine, VM
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 4d4c16860dbec10162797a13c8f9f57106abd17f
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---

# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Überwachen von Änderungen an einem virtuellen Computer mit Azure Event Grid und Logic Apps

Wenn bestimmte Ereignisse in Azure- oder Drittanbieterressourcen auftreten, können Sie einen automatisierten [Logik-App-Workflow](../logic-apps/logic-apps-what-are-logic-apps.md) starten. Diese Ressourcen können derartige Ereignisse in einem [Azure-Ereignisraster](../event-grid/overview.md) veröffentlichen. Das Ereignisraster überträgt diese Ereignisse wiederum mithilfe von Push an Abonnenten, die als Endpunkte Warteschlangen, Webhooks, oder [Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) aufweisen. Als Abonnent kann Ihre Logik-App auf diese Ereignisse vom Ereignisraster warten, bevor automatisierte Workflows zur Durchführung von Aufgaben ausgeführt werden – ohne dass Sie entsprechenden Code schreiben müssen.

Im Folgenden werden einige Beispiele für Ereignisse aufgeführt, die Herausgeber mit dem Azure Event Grid-Dienst an Abonnenten senden können:

* Eine Ressource wird erstellt, gelesen, aktualisiert oder gelöscht. Beispielsweise können Sie Änderungen überwachen, bei denen möglicherweise Gebühren für Ihr Azure-Abonnement anfallen und die sich auf Ihre Abrechnung auswirken könnten. 
* Eine Person wird zu einem Azure-Abonnement hinzugefügt oder aus dieser entfernt.
* Ihre App führt eine bestimmte Aktion durch.
* Eine neue Meldung wird in einer Warteschlange angezeigt.

In diesem Tutorial wird eine Logik-App erstellt, die Änderungen an einer VM überwacht und E-Mails über diese Änderungen sendet. Wenn Sie eine Logik-App mit einem Ereignisabonnement für eine Azure-Ressource erstellen, werden Ereignisse aus dieser Ressource über ein Ereignisraster an die Logik-App weitergeleitet. Das Tutorial führt Sie durch das Erstellen dieser Logik-App:

![Übersicht – Überwachen von virtuellen Computern mit Event Grid und Logic Apps](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie eine Logik-App zur Überwachung von Ereignissen über ein Ereignisraster.
> * Fügen Sie eine Bedingung zur Prüfung auf Änderungen an einem virtuellen Computer hinzu.
> * Senden Sie bei Änderungen an Ihrem virtuellen Computer E-Mails.

## <a name="prerequisites"></a>Voraussetzungen

* Ein E-Mail-Konto bei [einem beliebigen von Azure Logic Apps unterstützten E-Mail-Anbieter](../connectors/apis-list.md) wie Office 365 Outlook, Outlook.com oder Gmail zum Senden von Benachrichtigungen. Dieses Tutorial verwendet Office 365 Outlook.

* Ein [virtueller Computer](https://azure.microsoft.com/services/virtual-machines). Erstellen Sie eine VM über eine [Dokumentation zu virtuellen Computern](https://docs.microsoft.com/azure/virtual-machines/), wenn Sie dies nicht bereits getan haben. Zur Veröffentlichung von Ereignissen auf dem virtuellen Computer [müssen Sie keine weiteren Aktionen durchführen](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Erstellen einer Logik-App zur Überwachung von Ereignissen über ein Ereignisraster

Erstellen Sie zuerst eine Logik-App, und fügen Sie einen Ereignisrastertrigger hinzu, der die Ressourcengruppe für Ihren virtuellen Computer überwacht. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 

2. Wählen Sie im Azure-Hauptmenü in der linken oberen Ecke **Neu** > **Unternehmensintegration** > **Logik-App**.

   ![Erstellen einer Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Erstellen Sie Ihre Logik-App mit den in der folgenden Tabelle angegebenen Einstellungen:

   ![Angeben von Details zur Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Name** | *{Name-Ihrer-Logik-App}* | Geben Sie einen eindeutigen Namen für die Logik-App an. | 
   | **Abonnement** | *{Ihr-Azure-Abonnement}* | Wählen Sie für alle Dienste in diesem Tutorial das gleiche Azure-Abonnement. | 
   | **Ressourcengruppe** | *{Ihre-Azure-Ressourcengruppe}* | Wählen Sie für alle Dienste in diesem Tutorial die gleiche Azure-Ressourcengruppe. | 
   | **Location** | *{Ihre-Azure-Region}* | Wählen Sie für alle Dienste in diesem Tutorial die gleiche Azure-Region. | 
   | | | 

4. Wählen Sie dann **An Dashboard anheften** und **Erstellen** aus.

   Sie haben nun eine Azure-Ressource für Ihre Logik-App erstellt. 
   Nachdem Azure Ihre Logik-App bereitgestellt hat, zeigt der Designer für Logik-Apps Vorlagen für allgemeine Muster an, die Ihnen einen schnelleren Einstieg ermöglichen.

   > [!NOTE] 
   > Wenn Sie **An Dashboard anheften** auswählen, wird automatisch Ihre Logik-App im Designer für Logik-Apps geöffnet. Alternativ können Sie manuell nach der Logik-App suchen und diese öffnen.

5. Wählen Sie nun eine Logik-App-Vorlage. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App** aus, um Ihre Logik-App neu zu erstellen.

   ![Auswählen der Logik-App-Vorlage](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Der Designer für Logik-Apps zeigt Ihnen jetzt [*Connectors*](../connectors/apis-list.md) und [*Trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts), mit denen Sie Ihre Logik-App sowie Aktionen starten können, die Sie nach einem Trigger zur Durchführung von Aufgaben hinzufügen können. Ein Trigger ist ein Ereignis, bei dem eine Logik-App-Instanz erstellt und der Logik-App-Workflow gestartet wird. 
   Für Ihre Logik-App ist ein Trigger als erstes Element erforderlich.

6. Geben Sie im Suchfeld „Event Grid“ als Filter ein. Wählen Sie den folgenden Trigger aus: **Azure Event Grid – Für ein Ressourcenereignis**.

   ![Auswählen des Triggers „Azure Event Grid – Für ein Ressourcenereignis“](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Wenn Sie aufgefordert werden, melden Sie sich mit Ihrem Azure-Anmeldeinformationen bei Azure Event Grid an.

   ![Anmelden mit Ihren Azure-Anmeldeinformationen](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Wenn Sie mit einem persönlichen Microsoft-Konto wie @outlook.com oder @hotmail.com angemeldet sind, wird der Event Grid-Trigger möglicherweise nicht ordnungsgemäß angezeigt. Um dieses Problem zu umgehen, wählen Sie [Verbindung über Dienstprinzipal herstellen](/azure-resource-manager/resource-group-create-service-principal-portal.md), oder authentifizieren Sie sich als Mitglied des Azure Active Directory, dass Ihrem Azure-Abonnement zugeordnet ist, z.B. *Benutzername*@emailoutlook.onmicrosoft.com.

8. Abonnieren Sie jetzt für Ihre Logik-App Ereignisse eines Herausgebers. Legen Sie die Details für Ihr Ereignisabonnement wie in der folgenden Tabelle angegeben fest:

   ![Angeben von Details für das Ereignisabonnement](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **Abonnement** | *{VM-Azure-Abonnement}* | Wählen Sie das Azure-Abonnement des Ereignisherausgebers. Wählen Sie für dieses Tutorial das Azure-Abonnement für Ihren virtuellen Computer aus. | 
   | **Ressourcentyp** | Microsoft.Resources.resourceGroups | Wählen Sie den Ressourcentyp des Ereignisherausgebers aus. Wählen Sie für dieses Tutorial den angegebenen Wert, damit Ihre Logik-App nur Ressourcengruppen überwacht. | 
   | **Ressourcenname** | *{VM-Ressourcengruppenname}* | Wählen Sie den Ressourcennamen des Herausgebers. Wählen Sie für dieses Tutorial den Namen der Ressourcengruppe für Ihren virtuellen Computer aus. | 
   | Wählen Sie für optionale Einstellungen **Erweiterte Optionen anzeigen**. | *{siehe Beschreibungen}* | * **Präfixfilter**: Lassen Sie diese Einstellung für dieses Tutorial leer. Das Standardverhalten entspricht allen Werten. Allerdings können Sie als Filter eine Präfixzeichenfolge wie einen Pfad und einen Parameter für eine bestimmte Ressource angeben. <p>* **Suffixfilter**: Lassen Sie diese Einstellung für dieses Tutorial leer. Das Standardverhalten entspricht allen Werten. Wenn nur bestimmte Dateitypen angezeigt werden sollen, können Sie als Filter jedoch eine Suffixzeichenfolge wie eine Dateinamenerweiterung angeben.<p>* **Abonnementname**: Geben Sie einen eindeutigen Namen für Ihr Ereignisabonnement an. |
   | | | 

   Anschließend sieht Ihr Ereignisrastertrigger etwa wie im folgenden Beispiel aus:
   
   ![Details zu einem Beispiel-Ereignisrastertrigger](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**. Um die Details einer Aktion in Ihrer Logik-App zu reduzieren und auszublenden, wählen Sie die Titelleiste der Aktion aus.

   ![Speichern Ihrer Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Wenn Sie Ihre Logik-App mit einen Ereignisrastertrigger speichern, erstellt Azure automatisch ein Ereignisabonnement für Ihre Logik-App in Ihrer ausgewählten Ressource. Veröffentlicht die Ressource nun ein Ereignis im Ereignisraster, überträgt dieses Ereignisraster das Ereignis mithilfe von Push automatisch an Ihre Logik-App. Dieses Ereignis löst Ihre Logik-App aus, erstellt dann eine Instanz des in den folgenden Schritten definierten Workflows und startet deren Ausführung.

Ihre Logik-App ist nun live geschaltet und lauscht auf Ereignisse im Ereignisraster, führt allerdings erst Aktionen durch, wenn Sie diese dem Workflow hinzufügen. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Hinzufügen einer Bedingung zur Prüfung auf Änderungen an einem virtuellen Computer

Um den Workflow Ihrer Logik-App nur bei einem bestimmten Ereignis auszuführen, fügen Sie eine Bedingung hinzu, die auf Schreibvorgänge des virtuellen Computers überprüft. Wenn diese Bedingung erfüllt ist, sendet Ihre Logik-App Ihnen per E-Mail Details zum aktualisierten virtuellen Computer.

1. Wählen Sie im Designer für Logik-Apps unter dem Ereignisrastertrigger **Neuer Schritt** > **Bedingung hinzufügen** aus.

   ![Hinzufügen einer Bedingung zu Ihrer Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Der Designer für Logik-Apps fügt Ihrem Workflow eine leere Bedingung einschließlich der abhängig davon, ob die Bedingung „true“ oder „false“ ist, zu befolgenden Aktionspfade hinzu.

   ![Leere Bedingung](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. Wählen Sie im Feld **Bedingung** **Im erweiterten Modus bearbeiten**.
Geben Sie folgenden Ausdruck ein:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Nun sieht Ihre Bedingung wie im folgendem Beispiel aus:

   ![Leere Bedingung](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Dieser Ausdruck überprüft das `body`-Ereignis auf ein `data`-Objekt, bei dem die Eigenschaft `operationName` der Vorgang `Microsoft.Compute/virtualMachines/write` ist. 
   Weitere Informationen finden Sie unter [Event Grid-Ereignisschema](../event-grid/event-schema.md).

3. Um eine Beschreibung für die Bedingung anzugeben, wählen Sie in der Form „Bedingung“ die Schaltfläche **Ellipsen** (**...** ) und dann **Umbenennen**.

   > [!NOTE] 
   > Die späteren Beispiele in diesem Tutorial bieten auch Beschreibungen für Schritte im Logik-App-Workflow.

4. Wählen Sie jetzt **Im Standardmodus bearbeiten**, damit der Ausdruck automatisch folgendermaßen aufgelöst wird:

   ![Bedingung der Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Speichern Sie Ihre Logik-App.

## <a name="send-email-when-your-virtual-machine-changes"></a>Senden von E-Mails bei Änderungen an Ihrem virtuellen Computer

Fügen Sie nun eine [*Aktion*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) hinzu, um eine E-Mail zu erhalten, wenn die angegebene Bedingung erfüllt ist.

1. Wählen Sie im Feld **Falls erfüllt** der Bedingung die Option **Aktion hinzufügen**.

   ![Hinzufügen einer Aktion bei Erfüllung der Bedingung](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Geben Sie im Suchfeld den Begriff „E-Mail“ als Filter ein. Suchen Sie abhängig von Ihrem E-Mail-Anbieter nach dem entsprechenden Connector, und wählen Sie diesen aus. Wählen Sie anschließend die Aktion „E-Mail senden“ für Ihren Connector aus. Beispiel: 

   * Wählen Sie für ein Geschäfts-, Schul- oder Unikonto in Azure den Office 365 Outlook-Connector aus. 
   * Wählen Sie für persönliche Microsoft-Konten den Outlook.com-Connector aus. 
   * Wählen Sie für Gmail-Konten den Gmail-Connector aus. 

   Wir fahren mit dem Office 365 Outlook-Connector fort. 
   Wenn Sie einen anderen Anbieter verwenden, werden dieselben Schritte durchgeführt, allerdings kann das Erscheinungsbild der Benutzeroberfläche eventuell abweichen. 

   ![Auswählen der Aktion „E-Mail senden“](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Wenn noch keine Verbindung für Ihren E-Mail-Anbieter besteht, melden Sie sich nach Aufforderung zur Authentifizierung bei Ihrem E-Mail-Konto an.

4. Legen Sie Details für die E-Mail entsprechend den Angaben in der folgenden Tabelle fest:

   ![Leere E-Mail-Aktion](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Um aus Feldern in Ihrem Workflow auszuwählen, klicken Sie in ein Bearbeitungsfeld, sodass die Liste **Dynamischer Inhalt** geöffnet wird, oder wählen Sie **Dynamischen Inhalt hinzufügen**. Wählen Sie für weitere Felder **Weitere Informationen** für jeden Abschnitt in der Liste. Wählen Sie zum Schließen der Liste **Dynamischer Inhalt** **Dynamischen Inhalt hinzufügen**.

   | Einstellung | Empfohlener Wert | Beschreibung | 
   | ------- | --------------- | ----------- | 
   | **To** | *{Empfänger-E-Mail-Adresse}* |Geben Sie die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. | 
   | **Betreff** | Ressource aktualisiert: **Betreff**| Geben Sie den Inhalt des Betreffs der E-Mail ein. Geben Sie für dieses Tutorial den vorgeschlagenen Text ein, und wählen Sie das Feld **Betreff** des Ereignisses. Hier enthält Ihr E-Mail-Betreff den Namen für die aktualisierte Ressource (virtueller Computer). | 
   | **Text** | Ressourcengruppe: **Thema** <p>Ereignistyp: **Ereignistyp**<p>Ereignis-ID: **ID**<p>Zeit: **Ereigniszeit** | Geben Sie den Inhalt des Texts der E-Mail ein. Geben Sie für dieses Tutorial den vorgeschlagenen Text ein, und wählen Sie die Felder **Thema**, **Ereignistyp**, **ID** und **Ereigniszeit** des Ereignisses, damit Ihre E-Mail den Namen der Ressourcengruppe, den Ereignistyp, den Ereigniszeitstempel und die Ereignis-ID für das Update einschließt. <p>Um Ihrem Inhalt leere Zeilen hinzuzufügen, drücken Sie UMSCHALT + EINGABETASTE. | 
   | | | 

   > [!NOTE] 
   > Wenn Sie ein Feld auswählen, das ein Array darstellt, fügt der Designer automatisch eine **ForEach**-Schleife für die Aktion hinzu, die auf das Array verweist. Auf diese Weise führt Ihre Logik-App diese Aktion für jedes Arrayelement durch.

   Ihre E-Mail-Aktion könnte nun folgendem Beispiel entsprechen:

   ![Auswählen der in einer E-Mail einzuschließenden Ausgaben](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Anschließend sieht Ihre Logik-App in etwa wie im folgenden Beispiel aus:

   ![Fertiggestellte Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Speichern Sie Ihre Logik-App. Um Details zu den einzelnen Aktionen in Ihrer Logik-App zu reduzieren und auszublenden, wählen Sie die Titelleiste der Aktion aus.

   ![Speichern Ihrer Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Die Logik-App ist nun live geschaltet, wartet jedoch vor der Durchführung von Aktionen auf Änderungen an Ihrem virtuellen Computer. 
   Um nun Ihre Logik-App zu testen, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="test-your-logic-app-workflow"></a>Testen Ihres Logik-App-Workflows

1. Um zu überprüfen, ob Ihre Logik-App die angegebenen Ereignisse abruft, aktualisieren Sie Ihren virtuellen Computer. 

   Beispielsweise können Sie die Größe Ihrer VM im Azure-Portal oder [mit Azure PowerShell ändern](../virtual-machines/windows/resize-vm.md). 

   Nach nur wenigen Minuten sollten Sie eine E-Mail erhalten. Beispiel:

   ![E-Mail zum Update eines virtuellen Computers](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Um den Ausführungs- und Triggerverlauf für Ihre Logik-App zu überprüfen, wählen Sie in Ihrem Logik-App-Menü die Option **Übersicht**. Um weitere Details einer Ausführung anzuzeigen, wählen Sie die Zeile dieser Ausführung.

   ![Verlauf von Logik-App-Ausführungen](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Um die Eingaben und Ausgaben für die einzelnen Schritte anzuzeigen, erweitern Sie den Schritt, den Sie überprüfen möchten. Diese Informationen helfen Ihnen bei der Diagnose und beim Debuggen von Problemen in Ihrer Logik-App.
 
   ![Details des Verlaufs von Logik-App-Ausführungen](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Herzlichen Glückwunsch! Sie haben eine Logik-App erstellt und ausgeführt, die Ressourcenereignisse durch einen Ereignisraster überwacht und Ihnen eine E-Mail sendet, wenn derartige Ereignisse auftreten sollten. Außerdem haben Sie erfahren, wie einfach Sie Workflows zur Automatisierung von Prozessen und Integration von Systemen sowie Clouddiensten erstellen können.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden Ressourcen verwendet und Aktionen durchgeführt, für die Gebühren in Ihrem Azure-Abonnement anfallen. Wenn Sie mit der Durchführung des Tutorials und dem Testen fertig sind, deaktivieren oder löschen Sie unbedingt alle Ressourcen, für deren Gebühren Sie nicht aufkommen möchten.

Sie können die Ausführung Ihrer Logik-App und das Senden von E-Mail durch die Logik-App stoppen, ohne die App zu löschen. Wählen Sie in Ihrem Logik-App-Menü **Übersicht**. Wählen Sie auf der Symbolleiste **Deaktivieren** aus.

![Deaktivieren der Logik-App](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Häufig gestellte Fragen

**F**: Welche anderen Aufgaben zur Überwachung virtueller Computer kann ich mithilfe von Ereignisrastern und Logik-Apps durchführen? </br>
**A**: Sie können andere Konfigurationsänderungen überwachen wie etwa Folgende:

* Ein virtueller Computer erhält rollenbasierte Zugriffssteuerungsrechte (Role-Based Access Control, RBAC).
* Es werden Änderungen an einer Netzwerksicherheitsgruppe (Network Security Group, NSG) in einer Netzwerkschnittstelle (Network Interface, NIC) vorgenommen.
* Datenträger für einen virtuellen Computer werden hinzugefügt oder entfernt.
* Der NIC eines virtuellen Computers wird eine öffentliche IP-Adresse zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

* [Event Grid – Übersicht](../event-grid/overview.md)
* [Event Grid – Begriffe](../event-grid/concepts.md)
* [Schnellstart: Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Event Grid](../event-grid/custom-event-quickstart.md)
* [Event Grid-Ereignisschema](../event-grid/event-schema.md)
* [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
* [Erstellen von Logik-App-Workflows mit vordefinierten Vorlagen](../logic-apps/logic-apps-use-logic-app-templates.md)
