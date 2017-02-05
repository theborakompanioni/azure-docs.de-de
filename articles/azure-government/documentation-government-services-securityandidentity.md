---
title: "Azure Government – Sicherheit und Identität | Microsoft Docs"
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: e2fe7983-5870-43e9-ae01-2d45d3102c8a
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/14/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: cd01170c3c0a3f62024de3357d342af1f4f90c6c
ms.openlocfilehash: 27d447e8e3c336bbce2e1ca81d2c7c413b0360fc


---
# <a name="azure-government-security--identity"></a>Azure Government – Sicherheit und Identität
## <a name="key-vault"></a>Key Vault
Einzelheiten zu diesem Dienst und seiner Verwendung finden Sie in der [öffentlichen Dokumentation zu Azure Key Vault](../key-vault/index.md).

### <a name="data-considerations"></a>Überlegungen zu Daten
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Key Vault:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Alle mit einem Azure Key Vault-Schlüssel verschlüsselten Daten können regulierte/kontrollierte Daten enthalten. |Azure Key Vault-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Key Vault-Instanz eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Resource group names (Ressourcengruppennamen), Key Vault names (Key Vault-Namen), Abonnementname. |

Key Vault ist allgemein in Azure Government verfügbar. Wie auch bei der öffentlichen Variante gibt es keine Erweiterung. Key Vault ist daher nur über PowerShell und die CLI verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Nov16_HO3-->


