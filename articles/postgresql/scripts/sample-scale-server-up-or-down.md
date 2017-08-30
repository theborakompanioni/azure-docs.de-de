---
title: "Azure CLI-Skript: Skalieren von Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Skalieren eines Azure-Datenbank für PostgreSQL-Servers auf eine andere Leistungsstufe nach Abfragen der Metriken."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: b847abb336cce5dd5516469dca58002d3ba265f0
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Überwachen und Skalieren eines einzelnen PostgreSQL-Servers mit der Azure CLI
Dieses CLI-Beispielskript skaliert einen einzelnen Azure-Datenbank für PostgreSQL-Server nach Abfragen der Metriken auf eine andere Leistungsstufe. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die Abonnement-ID, die in den az monitor-Befehlen verwendet wird, durch Ihre eigene Abonnement-ID. [!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Erstellen und skalieren Sie Azure-Datenbank für PostgreSQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Löschen Sie die Ressourcengruppe.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az postgres server create](/cli/azure/postgres/server#create) | Erstellt einen PostgreSQL-Server, der die Datenbanken hostet. |
| [az monitor metrics list](/cli/azure/monitor/metrics#list) | Listet den Metrikwert für die Ressourcen auf. |
| [az group delete](/cli/azure/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure/overview)
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md)
- Lesen Sie weitere Informationen zur Skalierung: [Dienstebenen](../concepts-service-tiers.md) und [Compute-Einheiten und Speichereinheiten](../concepts-compute-unit-and-storage.md)

