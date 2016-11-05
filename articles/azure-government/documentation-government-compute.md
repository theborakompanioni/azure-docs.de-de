---
title: Dokumentation zu Azure Government | Microsoft Docs
description: Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government
services: Azure-Government
cloud: gov
documentationcenter: ''
author: ryansoc
manager: zakramer
editor: ''

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 09/29/2016
ms.author: ryansoc

---
# <a name="azure-government-compute"></a>Azure Government Compute
## <a name="virtual-machines"></a>Virtual Machines
Ausführliche Informationen zu diesem Dienst und seiner Verwendungsweise finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variationen
In Azure Government sind die folgenden VM-SKUs allgemein verfügbar:

| VM-SKU | US Gov VA | US Gov IA | Hinweise |
| --- | --- | --- | --- |
| A |Allgemein verfügbar |Allgemein verfügbar |Keine |
| Dv1 |Allgemein verfügbar |- |Keine |
| DSv1 |Allgemein verfügbar |- |Keine |
| Dv2 |Allgemein verfügbar |Allgemein verfügbar |15 in Kürze verfügbar |
| F |Allgemein verfügbar |Allgemein verfügbar |Keine |
| G |Geplant |- |Keine |

### <a name="data-considerations"></a>Überlegungen zu Daten
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Virtual Machines:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die auf einem virtuellen Computer eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegende Daten enthalten. Binärdateien, die in Azure Virtual Machines ausgeführt werden. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. SQL-Verbindungszeichenfolgen.  Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Azure Virtual Machine-Instanz eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Tenant role names (Namen von Mandantenrollen), Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!--HONumber=Oct16_HO2-->


