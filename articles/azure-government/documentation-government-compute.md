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
ms.date: 11/18/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: ca01baebd755e92aff1ef080af1fcd4606b9a2ec
ms.openlocfilehash: 200caac4b480495b0bbb8f10cc89fdd73c42dbe2


---
# <a name="azure-government-compute"></a>Azure Government Compute
## <a name="virtual-machines"></a>Virtual Machines
Ausführliche Informationen zu diesem Dienst und seiner Verwendungsweise finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="variations"></a>Variationen
Die Produkte (VMs) sind in Azure Government verfügbar:

| Produkte (VMs) | US Gov VA | US Gov IA |
| --- | --- | --- |
| A0–A7 |J |J |
| D-Serie |J |N |
| Dv2-Serie |J |J |
| DS-Serie |J |N |
| F-Serie |J |J |
| FS-Serie |N |N |
| G-Serie |J |N |
| GS-Serie |J |N |

### <a name="data-considerations"></a>Überlegungen zu Daten
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Virtual Machines:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die auf einem virtuellen Computer eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegende Daten enthalten. Binärdateien, die in Azure Virtual Machines ausgeführt werden. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. SQL-Verbindungszeichenfolgen.  Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Azure Virtual Machine-Instanz eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Tenant role names (Namen von Mandantenrollen), Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Dec16_HO1-->


