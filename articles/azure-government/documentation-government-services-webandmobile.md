---
title: "Azure Government – Web, Mobile und API | Microsoft-Dokumentation"
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: brendalee
manager: zakramer
ms.assetid: a1e173a9-996a-4091-a2e3-6b1e36da9ae1
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/5/2016
ms.author: brendal
translationtype: Human Translation
ms.sourcegitcommit: 6b77e338e1c7f0f79ea3c25b0b073296f7de0dcf
ms.openlocfilehash: f0ba3e50c4281a0e8c365be075ae45ab72f6e82d

---
# <a name="azure-government-web--mobile"></a>Azure Government Web und Mobile
## <a name="app-services"></a>App-Dienste
### <a name="variations"></a>Variationen
Azure App Service ist allgemein in Azure Government verfügbar.

Die Adresse, die in Azure Government für Azure App Service-Apps erstellt wird, unterscheidet sich von den Adressen, die in der öffentlichen Cloud erstellt werden:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| App Service |*.azurewebsites.net |*.azurewebsites.us|

Einige der in der öffentlichen Cloud verfügbaren App Service-Features stehen in Azure Government noch nicht zur Verfügung:

- App Service-Umgebungen
- App-Bereitstellung
    - Continuous Delivery (Vorschau)
- Einstellungen
    - VNet-Integration und Hybridverbindungen
    - Sicherheitsüberprüfung
- Entwicklungstools
    - Leistungstest
    - Ressourcen-Explorer
    - PHP-Debuggen
- Überwachung
    - Application Insights
    - Instanzenspezifische Metriken
    - HTTP-Livedatenverkehr
    - Anwendungsereignisse
    - FREB-Protokolle
- Support und Problembehandlung
    - App Service-Ratgeber
    - Fehlerverlauf
    - Diagnostics-as-a-Service
    - Auswirkungen des Problems minimieren


### <a name="considerations"></a>Überlegungen
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für App Service:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die im Rahmen von Azure App Service eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegen. Binärdateien, die in Azure App Service ausgeführt werden. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. SQL-Verbindungszeichenfolgen. Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten Ihrer Azure App Service-Instanz eingegeben werden. Geben Sie in die folgenden Felder keine regulierten/kontrollierten Daten ein: Ressourcengruppen, Ressourcennamen, Ressourcentags|

## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/), um weitere Informationen und Aktualisierungen zu erhalten.




<!--HONumber=Dec16_HO2-->


