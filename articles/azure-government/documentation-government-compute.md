---
title: "Azure Government – Compute | Microsoft Docs"
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: fb11f60c-5a70-46a9-82a0-abb2a4f4239b
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 3/13/2017
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 146fcd9788bab55e4abd9e70dcfe25741f041d34
ms.lasthandoff: 03/15/2017


---
# <a name="azure-government-compute"></a>Azure Government Compute
## <a name="virtual-machines"></a>Virtual Machines
Ausführliche Informationen zu diesem Dienst und seiner Verwendungsweise finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="variations"></a>Variationen
Die Produkte (VMs) sind in Azure Government verfügbar:

| Produkte (VMs) | US Gov VA | US Gov IA | US DoD, Osten | US DoD, Westen
| --- | --- | --- |--- |--- |
| A0–A7 |J |J |J |J |J |
| Av2 |J |J |J |J |J |
| D-Serie |J |N |N |N |
| Dv2-Serie |J |J |J |J |
| DS-Serie |J |N |N |N |
| DSv2-Serie |J |N |J |J |
| F-Serie |J |J |J |J |
| FS-Serie |J |N |J |J |
| G-Serie |J |N |N |N |
| GS-Serie |J |N |N |N |

### <a name="data-considerations"></a>Überlegungen zu Daten
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Virtual Machines:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die auf einem virtuellen Computer eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegende Daten enthalten. Binärdateien, die in Azure Virtual Machines ausgeführt werden. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. SQL-Verbindungszeichenfolgen.  Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Azure Virtual Machine-Instanz eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Tenant role names (Namen von Mandantenrollen), Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.


