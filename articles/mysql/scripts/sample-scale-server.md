---
title: "Azure CLI-Skriptbeispiele zum Skalieren eines Azure-Datenbank für MySQL-Servers | Microsoft-Dokumentation"
description: "Dieses CLI-Beispielskript skaliert einen Azure-Datenbank für MySQL-Server nach Abfragen der Metriken auf eine andere Leistungsstufe."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 05/31/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: 33316ff3db382d25a444d55772c6ee4d7b7ac418
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Überwachen und Skalieren eines Azure-Datenbank für MySQL-Servers mithilfe der Azure CLI
Dieses CLI-Beispielskript skaliert einen einzelnen Azure-Datenbank für MySQL-Server nach Abfragen der Metriken auf eine andere Leistungsstufe.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die Abonnement-ID, die in den az monitor-Befehlen verwendet wird, durch Ihre eigene Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Erstellen und skalieren Sie Azure-Datenbank für MySQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Löschen Sie die Ressourcengruppe.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az mysql server create](/cli/azure/mysql/server#create) | Erstellt einen MySQL-Server, der die Datenbanken hostet. |
| [az monitor metrics list](/cli/azure/monitor/metrics#list) | Listet den Metrikwert für die Ressourcen auf. |
| [az group delete](/cli/azure/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie weitere Informationen zur Azure CLI: [Azure CLI-Dokumentation](/cli/azure/overview).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für MySQL](../sample-scripts-azure-cli.md)
- Weitere Informationen zur Skalierung finden Sie unter [Dienstebenen](../concepts-service-tiers.md) und [Compute-Einheiten und Speichereinheiten](../concepts-compute-unit-and-storage.md).

