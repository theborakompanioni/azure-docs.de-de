---
title: Verwalten eines Machine Learning-Arbeitsbereichs | Microsoft Docs
description: Verwalten des Zugriffs auf Azure Machine Learning-Arbeitsbereiche sowie Bereitstellen und Verwalten von ML-API-Webdiensten
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: cf0cd5202288a9da55e0f8409a51ea904eeb2d55
ms.openlocfilehash: 775ef958846ad2a780abb761d1c3934e671d5f1c
ms.lasthandoff: 03/01/2017


---
# <a name="manage-an-azure-machine-learning-workspace"></a>Verwalten eines Azure Machine Learning-Arbeitsbereichs

> [!NOTE]
> Informationen zum Verwalten von Webdiensten im Machine Learning Web Services-Portal finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md).
> 
> 

Machine Learning-Arbeitsbereiche können Sie entweder im Azure-Portal oder im klassischen Azure-Portal verwalten.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwalten Sie einen Arbeitsbereich im Azure-Portal

1. Melden Sie sich über das Administratorkonto eines Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie im Suchfeld oben auf der Seite „Machine Learning-Arbeitsbereiche“ ein, und wählen Sie dann **Machine Learning-Arbeitsbereiche** aus.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Neben den Standardinformationen und -optionen zur Ressourcenverwaltung stehen folgende Optionen zur Verfügung:

- **Eigenschaften anzeigen:** Auf dieser Seite werden Informationen zum Arbeitsbereich und der Ressource angezeigt. Zudem können Sie hier das Abonnement und die Ressourcengruppe ändern, mit denen der Arbeitsbereich verbunden ist.
- **Speicherschlüssel neu synchronisieren:** Im Arbeitsbereich werden die Schlüssel für das Speicherkonto verwaltet. Wenn sich die Schlüssel für das Speicherkonto ändern, können Sie auf **Schlüssel neu synchronisieren** klicken, um die Schlüssel mit dem Arbeitsbereich zu synchronisieren.

Verwenden Sie zum Verwalten der dem Arbeitsbereich zugeordneten Webdienste das Portal für Machine Learning-Webdienste. Ausführliche Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md).

> [!NOTE]
> Zum Bereitstellen oder Verwalten neuer Webdienste muss Ihnen in dem Abonnement, für das der Webdienst bereitgestellt wird, eine Teilnehmer- oder Administratorrolle zugewiesen sein. Wenn Sie andere Benutzer in einen Machine Learning-Arbeitsbereich einladen, müssen Sie ihnen eine Teilnehmer- oder Administratorrolle im Abonnement zuweisen, damit sie Webdienste bereitstellen oder verwalten können. 

## <a name="use-the-azure-classic-portal"></a>Verwenden des klassischen Azure-Portals

Mit dem klassischen Azure-Portal können Sie Ihre Arbeitsbereiche für maschinelles Lernen wie folgt verwalten:

* Kontrollieren der Nutzung des Arbeitsbereichs
* Konfigurieren des Arbeitsbereichs zum Zulassen oder Verweigern des Zugriffs
* Verwalten von Webdiensten, die im Arbeitsbereich erstellt wurden
* Löschen des Arbeitsbereichs

Außerdem finden Sie auf der Dashboard-Registerkarte eine Übersicht über die Nutzung Ihres Arbeitsbereichs und eine Schnellübersicht über Informationen zum Arbeitsbereich.  

> [!TIP]
> In Azure Machine Learning Studio können Sie auf der Registerkarte **WEB SERVICES** Machine Learning-Webdienste hinzufügen, aktualisieren oder löschen.
> 
> 

So verwalten Sie einen Arbeitsbereich im klassischen Azure-Portal

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit Ihrem Microsoft Azure-Konto an. Verwenden Sie das Konto, das dem Azure-Abonnement zugeordnet ist.
2. Klicken Sie im Microsoft Azure-Dienstbereich auf **MACHINE LEARNING**.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Die Workspace-Seite enthält drei Registerkarten:

* **DASHBOARD** – ermöglicht das Anzeigen der Verwendung des Arbeitsbereichs und von Informationen
* **KONFIGURIEREN** – ermöglicht es, den Zugriff auf den Arbeitsbereich zu verwalten
* **WEB SERVICES** – ermöglicht die VerwaItung von Webdiensten, die in diesem Arbeitsbereich veröffentlicht wurden

### <a name="to-monitor-how-the-workspace-is-being-used"></a>So kontrollieren Sie die Nutzung des Arbeitsbereichs
Klicken Sie auf die Registerkarte **DASHBOARD** .

Im Dashboard können Sie die Gesamtnutzung des Arbeitsbereichs ablesen und sich eine schnelle Übersicht über die Informationen des Arbeitsbereichs verschaffen.

* Das Diagramm **SERVER** zeigt die Computerressourcen an, die im Arbeitsbereich verwendet werden. Sie können die Ansicht ändern, um relative oder absolute Werte anzuzeigen, und den im Diagramm angezeigten Zeitrahmen ändern.
* **Verwendungsübersicht** zeigt den vom Arbeitsbereich genutzten Azure-Speicher an.
* **Auf einen Blick** enthält eine Zusammenfassung der Informationen des Arbeitsbereichs und nützliche Links.

> [!NOTE]
> Über den Link **Bei ML Studio anmelden** wird Machine Learning Studio mit dem Microsoft-Konto geöffnet, an dem Sie gerade angemeldet sind. Das Microsoft-Konto, mit dem Sie sich beim klassischen Azure-Portal angemeldet haben, um einen Arbeitsbereich zu erstellen, verfügt nicht automatisch über die Berechtigung zum Öffnen dieses Arbeitsbereichs. Um einen Arbeitsbereich zu öffnen, müssen Sie an dem Microsoft-Konto angemeldet sein, das als Besitzer des Arbeitsbereichs definiert wurde, oder Sie müssen eine Einladung des Besitzers zur Teilnahme am Arbeitsbereich erhalten haben.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>So erteilen oder blockieren Sie den Zugriff für Benutzer
Klicken Sie auf die Registerkarte **KONFIGURIEREN** .

Auf der Registerkarte „Configuration“ haben Sie folgende Möglichkeiten:

* Blockieren des Zugriffs auf den Machine Learning-Arbeitsbereich, indem Sie auf "DENY" klicken. Benutzer können den Arbeitsbereich dann nicht mehr in Machine Learning Studio öffnen. Klicken Sie auf "ALLOW", um den Zugang wiederherzustellen.

Um zusätzliche Konten zu verwalten, die Zugriff auf den Arbeitsbereich in Machine Learning Studio haben, klicken Sie auf der Registerkarte **DASHBOARD** auf **Bei ML Studio anmelden** (siehe vorherigen Hinweis zu **Bei ML Studio anmelden**). Dadurch wird der Arbeitsbereich in Machine Learning Studio geöffnet. Klicken Sie dort auf die Registerkarte **SETTINGS** und dann auf **USERS**. Sie können auf **INVITE MORE USERS** klicken, um Benutzern den Zugriff auf den Arbeitsbereich zu gewähren, oder einen Benutzer auswählen und durch Klicken auf **REMOVE** entfernen.

### <a name="to-manage-web-services-in-this-workspace"></a>So verwalten Sie Webdienste in diesem Arbeitsbereich
Klicken Sie auf die Registerkarte **WEBDIENSTE** .

Hierdurch wird eine Liste von Webdiensten angezeigt, die von diesem Arbeitsbereich veröffentlicht wurden.
Um einen Webdienst zu verwalten, klicken Sie auf den Namen in der Liste, um die Webdienstseite zu öffnen.

Ein Webdienst kann einen oder mehrere definierte Endpunkte haben.

* Sie können zusätzlich zum Endpunkt "Standard" weitere Endpunkte definieren. Um den Endpunkt hinzuzufügen, klicken Sie unten im Dashboard auf **Endpunkte verwalten**, um das Azure Machine Learning Web Services-Portal zu öffnen.
* Aktivieren Sie zum Löschen eines Endpunkts (der Standardendpunkt kann nicht gelöscht werden) das Kontrollkästchen am Anfang der Endpunktzeile, und klicken Sie auf **DELETE**. Dadurch wird der Endpunkt aus dem Webdienst entfernt.
  
  > [!NOTE]
  > Wenn eine Anwendung den Webdienst-Endpunkt gerade verwendet, wenn dieser gelöscht wird, erhält die Anwendung beim nächsten Zugriff auf den Dienst eine Fehlermeldung.
  > 
  > 

Klicken Sie auf den Namen eines Webdienst-Endpunkts, um diesen zu öffnen. 

Im Dashboard können Sie die Gesamtnutzung des Webdiensts über einen Zeitraum anzeigen. In der Dropdownliste „Period“ rechts oben in den Nutzungsdiagrammen können Sie den Zeitraum auswählen. Im Dashboard werden die folgenden Informationen angezeigt:

* **Requests Over Time** zeigt ein Diagramm der Anzahl der Anforderungen im ausgewählten Zeitraum an. Es dient zum Bestimmen von Nutzungsspitzen.
* **Request-Response Requests** zeigt die Gesamtanzahl der „Request-Response“-Aufrufe, die der Dienst im ausgewählten Zeitraum empfangen hat, und die Anzahl der Fehler an.
* **Average Request-Response Compute Time** zeigt die Durchschnittszeit an, die zum Ausführen der empfangenen Anforderungen benötigt wurde.
* **Batch Requests** zeigt die Gesamtanzahl der Batchanforderungen, die der Dienst im ausgewählten Zeitraum empfangen hat, und die Anzahl der Fehler an.
* **Average Job Latency** zeigt die Durchschnittszeit an, die zum Ausführen der empfangenen Anforderungen benötigt wurde.
* **Errors** zeigt die kumulierte Anzahl der Fehler, die bei Aufrufen des Webdiensts aufgetreten sind.
* **Services Costs** zeigt die Kosten für den Abrechnungsplan, der dem Dienst zugeordnet ist.

Auf der Seite „Configure“ können Sie die folgenden Eigenschaften aktualisieren:

* In **Description** können Sie eine Beschreibung des Webdiensts eingeben. „Description“ ist ein Pflichtfeld.
* **Logging** ermöglicht Ihnen das Aktivieren bzw. Deaktivieren der Fehlerprotokollierung für den Endpunkt. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Protokollierung für Machine Learning-Webdienste](machine-learning-web-services-logging.md).
* **Enable Sample data** ermöglicht das Bereitstellen von Beispieldaten, die Sie mit dem Request-Response-Dienst testen können. Wenn Sie den Webdienst in Machine Learning Studio erstellt haben, stammen die Beispieldaten aus den Daten, mit denen Sie Ihr Modell trainiert haben. Wenn Sie den Dienst programmgesteuert erstellt haben, stammen die Daten aus den Beispieldaten, die Sie als Teil des JSON-Pakets bereitgestellt haben.


