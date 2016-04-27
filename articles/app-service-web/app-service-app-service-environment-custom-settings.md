<properties 
	pageTitle="Benutzerdefinierte Einstellungen für App Service-Umgebungen" 
	description="Benutzerdefinierte Konfigurationseinstellungen für App Service-Umgebungen" 
	services="app-service" 
	documentationCenter="" 
	authors="stefsch" 
	manager="nirma" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/08/2016" 
	ms.author="stefsch"/>

# Benutzerdefinierte Konfigurationseinstellungen für App Service-Umgebungen

## Übersicht ##
Da App Service-Umgebungen isoliert für einen einzelnen Kunden eingerichtet werden, gibt es bestimmte Konfigurationseinstellungen, die exklusiv auf eine App Service-Umgebung angewendet werden können. In diesem Artikel werden die verschiedenen verfügbaren Anpassungen für App Service-Umgebungen dokumentiert.

Anpassungen für App Service-Umgebungen werden in einem Array im neuen Attribut „clusterSettings“ gespeichert, das sich im Properties-Wörterbuch der ARM-Entität *hostingEnvironments* befindet.

Ein kurzer ARM-Vorlagenausschnitt (siehe unten) zeigt das Attribut „clusterSettings“:


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Das Attribut „clusterSettings“ kann in eine ARM-Vorlage eingefügt werden, um die App Service-Umgebung zu aktualisieren.

Alternativ kann der Attributwert über den [Azure Resource Explorer](https://resources.azure.com) aktualisiert werden. Navigieren Sie im Azure Resource Explorer zum Knoten für die App Service-Umgebung (subscriptions > resourceGroups > providers > Microsoft.Web > hostingEnvironments), und klicken Sie auf die App Service-Umgebung, die aktualisiert werden soll.

Klicken Sie im rechten Browserfenster in der oberen Symbolleiste auf „Lesen/Schreiben“, um eine interaktive Bearbeitung im Resource Explorer zuzulassen. Klicken Sie dann auf die blaue Schaltfläche „Bearbeiten“, um die Bearbeitung der ARM-Vorlage zu ermöglichen. Scrollen Sie bis zum Ende des rechten Browserfensters. Das Attribut „clusterSettings“ wird ganz unten angezeigt. Sie können entweder einen Attributwert eingeben oder den vorhandenen Wert aktualisieren.

Geben Sie das Array der Konfigurationswerte ein, die Sie für das Attribut „clusterSettings“ verwenden möchten. Alternativ können Sie die Werte auch kopieren und einfügen. Klicken Sie dann auf die grüne Schaltfläche „PUT“ oben im rechten Browserfenster, um einen Commit für die Änderungen an der App Service-Umgebung auszuführen.

Unabhängig vom gewählten Ansatz zur Aktualisierung der App Service-Umgebung dauert es nach dem Übermitteln der Änderungen etwa 30 Minuten multipliziert mit der Anzahl der Front-Ends in der App Service-Umgebung, bis die Änderungen wirksam werden. Wenn eine App Service-Umgebung beispielsweise über vier Front-Ends verfügt, dauert es etwa zwei Stunden, bis die Konfigurationsaktualisierung abgeschlossen ist. Während die Konfigurationsänderung angewendet wird, können keine weiteren Skalierungsvorgänge oder Konfigurationsänderungen für die App Service-Umgebung ausgeführt werden.

## Deaktivieren von TLS 1.0 ##
Eine Frage, die häufig von Kunden gestellt wird – insbesondere dann, wenn die Kunden PCI-Compliance-Audits durchführen – ist die Möglichkeit zur expliziten Deaktivierung von TLS 1.0 für ihre Apps.

TLS 1.0 kann mit dem folgenden *clusterSettings*-Eintrag deaktiviert werden:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],



## Erste Schritte
Die Azure-Website mit ARM-Schnellstartvorlagen umfasst eine Vorlage mit der Basisdefinition zum [Erstellen einer App Service-Umgebung](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
 

<!---HONumber=AcomDC_0413_2016-->