---
title: Verwendung des Azure-Ressourcen-Connectors in Logik-Apps | Microsoft-Dokumentation
description: Erstellen und Konfigurieren des Azure-Ressourcen-Connectors oder einer API-App und Verwenden in einer Logik-App in Azure App Service
services: logic-apps
documentationcenter: .net,nodejs,java
author: stepsic-microsoft-com
manager: erikre
editor: 
ms.assetid: a32e5a5c-34d7-4422-b0f7-c5cf7b8abffa
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/01/2016
ms.author: stepsic
translationtype: Human Translation
ms.sourcegitcommit: 5b1b65e3d1066bea6958fa6461a157ee39fbe7dc
ms.openlocfilehash: d230450535613e85c607ef120929ea61bc2085bc
ms.lasthandoff: 02/15/2017


---
# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit dem Azure-Ressourcen-Connector und das Hinzufügen zur Logik-App
> [!NOTE]
> Diese Version des Artikels gilt für die Logik-Apps-Schemaversion 2014-12-01-preview.
> 
> 

Der Azure-Ressourcen-Connector bietet eine einfache Möglichkeit zum Verwalten von Azure-Ressourcen in Ihrer Logik-App.

## <a name="create-the-azure-resource-connector"></a>Erstellen des Azure-Ressourcen-Connectors
Um die API-App für den Azure-Ressourcen-Connector verwenden zu können, müssen Sie zuerst eine Instanz der App erstellen. Dies kann entweder inline beim Erstellen einer Logik-App oder durch Auswählen der API-App für den Azure-Ressourcen-Manager-Connector aus dem Azure Marketplace erfolgen.

Um sie zu konfigurieren, müssen Sie einen Dienstprinzipal mit Berechtigungen für alles, was Sie in Azure ausführen möchten, einrichten. Alle Aufrufe erfolgen im Namen dieses Dienstprinzipals, den Sie eingerichtet haben. Dadurch können Sie für den Connector festlegen, dass er nur genau die gewünschten Bereiche verwendet, und nicht mehr.

David Ebbo hat [einen hervorragenden Blogbeitrag](http://blog.davidebbo.com/2014/12/azure-service-principal.html) mit einer Anleitung dieser Einrichtung geschrieben. Folgen Sie den Anweisungen dieses Beitrags, und rufen Sie Ihre **Mandanten-ID**, **Client-ID** und den **geheimen Schlüssel** ab. Diese drei Felder sowie die **Abonnement-ID**sind zum Konfigurieren des Connectors erforderlich.

## <a name="using-the-azure-resource-connector-in-logic-app-designer"></a>Verwenden des Azure-Ressourcen-Connectors im Logik-App-Designer
### <a name="trigger"></a>Trigger
Es gibt zwei Trigger, die im Connector unterstützt werden:

| Name | Beschreibung |
| --- | --- |
| Ereignis tritt auf |Trigger, wenn ein Ereignis für eine Ressource in Ihrem Abonnement auftritt. |
| Metrik überschreitet Schwellenwert |Trigger, wenn eine Metrik einen bestimmten Schwellenwert erreicht. |

### <a name="action"></a>Aktion
Ebenso können Sie eine große Anzahl von Aktionen innerhalb Ihres Azure-Abonnements bereitstellen:

Aktionen für **Ressourcengruppen** :

| Name | Beschreibung |
| --- | --- |
| Ressourcengruppen auflisten |Listen Sie alle Ressourcengruppen im Abonnement auf. |
| Ressourcengruppe abrufen |Rufen Sie eine Ressourcengruppe mit der ID ab. |
| Ressourcengruppe erstellen |Erstellen oder aktualisieren Sie eine Ressourcengruppe. |
| Ressourcengruppe löschen |Löschen Sie eine Ressourcengruppe. |

Aktionen für **Ressourcen** :

| Name | Beschreibung |
| --- | --- |
| Ressourcen auflisten |Listen Sie Ressourcen in Ihrem Abonnement mit verschiedenen Arten von Filtern auf. |
| Ressource abrufen |Rufen Sie eine einzelne Ressource mit der Ressourcen-ID ab. |
| Ressource erstellen oder aktualisieren |Erstellen Sie eine Ressource, oder aktualisieren Sie eine vorhandene Ressource. Sie müssen alle Eigenschaften für diese Ressource angeben. |
| Ressourcenaktion |Führen Sie eine andere Aktion für eine Ressource aus. Sie müssen den Namen der Aktion und die etwaige Nutzlast dieser Aktion  kennen. |
| Ressource löschen |Löschen Sie eine Ressource. |

Aktionen für **Ressourcenanbieter** :

| Name | Beschreibung |
| --- | --- |
| Ressourcenanbieter auflisten |Listen Sie alle verfügbaren Ressourcenanbieter im Abonnement auf. |

Aktionen für **Ressourcengruppenbereitstellungen** :

| Name | Beschreibung |
| --- | --- |
| Bereitstellungen auflisten |Listen Sie alle Bereitstellungen in einer Ressourcengruppe auf. |
| Bereitstellung abrufen |Rufen Sie eine Vorlagenbereitstellung mit der ID ab. |
| Bereitstellung erstellen |Erstellen Sie eine neue Ressourcengruppenbereitstellung durch Bereitstellen einer Vorlage. |

Aktionen für **Ereignisse** zu Ressourcen:

| Name | Beschreibung |
| --- | --- |
| Ereignisse abrufen |Rufen Sie Ereignisse in einem Abonnement für eine Ressource ab. |

Aktionen für **Metriken** :

| Name | Beschreibung |
| --- | --- |
| Metriken abrufen |Rufen Sie eine Metrik für eine Ressourcen-ID ab. |

## <a name="do-more-with-your-connector"></a>Mehr mit Ihrem Connector machen
Nachdem der Connector nun erstellt ist, können Sie ihn mit einer Logik-App in einem Geschäftsworkflow hinzufügen. Informationen finden Sie unter [Was sind Logik-Apps?](../logic-apps/logic-apps-what-are-logic-apps.md).

> [!NOTE]
> Wenn Sie Azure Logic Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [Logik-App testen](https://azure.microsoft.com/try/app-service/logic/)sofort kostenlos eine kurzlebige Starter-Logik-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

Anzeigen der Swagger-REST-API-Referenz unter [Referenz zu Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md

