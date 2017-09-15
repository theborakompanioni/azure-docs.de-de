---
title: "Erste Schritte mit der Azure CLI für Batch | Microsoft-Dokumentation"
description: "Hier erhalten Sie eine kurze Einführung in die Batch-Befehle der Azure-Befehlszeilenschnittstelle zum Verwalten von Ressourcen des Azure Batch-Diensts."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 45c611e1c429e8c17c0fb1537577a4cbd037c23a
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="manage-batch-resources-with-azure-cli"></a>Verwalten von Batch-Ressourcen mit der Azure CLI

Azure CLI 2.0 ist die neue Befehlszeilenumgebung von Azure und dient zum Verwalten von Azure-Ressourcen. Sie kann unter MacOS, Linux und Windows verwendet werden. Azure CLI 2.0 ist für die Verwaltung von Azure-Ressourcen über die Befehlszeile optimiert. Sie können die Azure CLI verwenden, um Ihre Azure Batch-Konten sowie Ressourcen wie Pools, Aufträge und Tasks zu verwalten. Mit der Azure CLI können Sie viele der Tasks per Skript ausführen, die Sie auch mit den Batch-APIs, dem Azure-Portal und den Batch PowerShell-Cmdlets durchführen.

Dieser Artikel bietet eine Übersicht über die Verwendung der [Azure CLI, Version 2.0](https://docs.microsoft.com/cli/azure/overview) mit Batch. Eine Übersicht über die Verwendung der Befehlszeilenschnittstelle mit Azure finden Sie unter [Erste Schritte mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

Microsoft empfiehlt die neueste Version der Azure CLI: Version 2.0. Weitere Informationen zu Version 2.0 finden Sie unter [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/) (Azure CLI 2.0 jetzt allgemein verfügbar).

## <a name="set-up-the-azure-cli"></a>Einrichten der Azure CLI

Um die Azure CLI zu installieren, führen Sie die Schritte unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md) aus.

> [!TIP]
> Es empfiehlt sich, die Installation der Azure-Befehlszeilenschnittstelle regelmäßig zu aktualisieren, um von Dienstupdates und Verbesserungen zu profitieren.
> 
> 

## <a name="command-help"></a>Hilfe zu Befehlen

Sie können Hilfetext für jeden Befehl der Azure-Befehlszeilenschnittstelle anzeigen, indem Sie `-h` an den Befehl anhängen. Geben Sie keine weiteren Optionen an. Beispiel:

* Geben Sie Folgendes ein, um Hilfe zum Befehl `az` zu erhalten: `az -h`
* Verwenden Sie Folgendes, um eine Liste mit allen Batch-Befehlen der Befehlszeilenschnittstelle zu erhalten: `az batch -h`
* Geben Sie Folgendes ein, um ein Batch-Konto zu erstellen: `az batch account create -h`

Verwenden Sie im Zweifelsfall die Befehlszeilenoption `-h` , um Hilfe zu einem Befehl der Azure-Befehlszeilenschnittstelle zu erhalten.

> [!NOTE]
> In früheren Versionen der Azure CLI wurde einem CLI-Befehl `azure` vorangestellt. In Version 2.0 wird allen Befehlen `az` vorangestellt. Stellen Sie sicher, dass all Ihre Skripts auf die neue Syntax für Version 2.0 aktualisiert sind.
>
>  

Weitere Informationen zu [Azure CLI-Befehlen für Batch](https://docs.microsoft.com/cli/azure/batch) finden Sie zudem in der Referenzdokumentation der Azure CLI. 

## <a name="log-in-and-authenticate"></a>Anmelden und Authentifizieren

Um die Azure CLI mit Batch zu verwenden, müssen Sie sich anmelden und authentifizieren. Hierfür sind zwei einfache Schritte erforderlich:

1. **Melden Sie sich bei Azure an.** Durch Anmelden bei Azure erhalten Sie Zugriff auf Azure Resource Manager-Befehle, einschließlich der Befehle für den [Batch Management-Dienst](batch-management-dotnet.md).  
2. **Melden Sie sich bei Ihrem Batch-Konto an.** Durch Anmelden bei Ihrem Batch-Konto erhalten Sie Zugriff auf die Befehle des Batch-Diensts.   

### <a name="log-in-to-azure"></a>Anmelden an Azure

Es gibt verschiedenen Möglichkeiten, sich bei Azure anzumelden. Diese werden detailliert unter [Anmelden mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) beschrieben:

1. [Interaktiv anmelden](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#az_authenticate_azure_cli_interactive_log_in). Melden Sie sich interaktiv an, wenn Sie Azure CLI-Befehle selbst an der Befehlszeile ausführen.
2. [Anmelden mit einem Dienstprinzipal](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#az_authenticate_azure_cli_logging_in_with_a_service_principal). Melden Sie sich mit einem Dienstprinzipal an, wenn Sie Azure CLI-Befehle über ein Skript oder aus einer Anwendung heraus ausführen.

Im Rahmen dieses Artikels veranschaulichen wir die interaktive Anmeldung bei Azure. Geben Sie [az login](https://docs.microsoft.com/cli/azure/#login) an der Befehlszeile ein:

```azurecli
# Log in to Azure and authenticate interactively.
az login
```

Der Befehl `az login` gibt, wie hier gezeigt, ein Token zurück, das Sie zur Authentifizierung verwenden. Befolgen Sie die bereitgestellten Anweisungen, um eine Webseite zu öffnen und das Token an Azure zu senden:

![Anmelden an Azure](./media/batch-cli-get-started/az-login.png)

Die im Abschnitt [Beispielskripts für die Shell](#sample-shell-scripts) aufgeführten Beispiele zeigen auch, wie Sie Ihre Azure CLI-Sitzung starten, indem Sie sich interaktiv bei Azure anmelden. Nachdem Sie sich angemeldet haben, können Sie Befehle aufrufen, um mit Batch Management-Ressourcen wie Batch-Konten, -Schlüsseln, -Anwendungspaketen und -Kontingenten zu arbeiten.  

### <a name="log-in-to-your-batch-account"></a>Anmelden bei Ihrem Batch-Konto

Um die Azure CLI zum Verwalten von Batch-Ressourcen wie Pools, Aufträgen und Tasks zu verwenden, müssen Sie sich bei Ihrem Batch-Konto anmelden und authentifizieren. Verwenden Sie den Befehl [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login), um sich beim Batch-Dienst anzumelden. 

Für die Authentifizierung bei Ihrem Batch-Konto stehen Ihnen zwei Optionen zur Verfügung:

- **Durch Verwenden der Azure Active Directory-Authentifizierung.** 

    Die Authentifizierung über Azure AD ist das Standardvorgehen, wenn Sie die Azure CLI mit Batch verwenden, und wird für die meisten Szenarien empfohlen. 
    
    Bei der interaktiven Anmeldung bei Azure – wie im vorherigen Abschnitt beschrieben – werden Ihre Anmeldeinformationen zwischengespeichert, sodass die Azure CLI Sie mit diesen Anmeldeinformationen bei Ihrem Batch-Konto anmelden kann. Wenn Sie sich mithilfe eines Dienstprinzipals bei Azure anmelden, werden diese Anmeldeinformationen ebenfalls verwendet, um Sie bei Ihrem Batch-Konto anzumelden.

    Ein Vorteil von Azure AD ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC). Bei der rollenbasierten Zugriffssteuerung richtet sich der Zugriff eines Benutzers nach der ihm zugewiesenen Rolle, nicht danach, ob der Benutzer über die Kontoschlüssel verfügt oder nicht. Sie verwalten also keine Kontoschlüssel, sondern RBAC-Rollen, und Azure AD verarbeitet Zugriff und Authentifizierung.  

    Die Authentifizierung über Azure AD ist erforderlich, wenn Sie Ihr Azure Batch-Konto im Poolzuweisungsmodus „Benutzerabonnement“ erstellt haben. 

    Um sich über Azure AD bei Ihrem Batch-Konto anzumelden, rufen Sie den Befehl [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) auf: 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Durch Verwenden der Authentifizierung mit gemeinsam verwendetem Schlüssel.**

    Bei der [Authentifizierung mit gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) werden Ihre Kontozugriffsschlüssel verwendet, um Azure CLI-Befehle für den Batch-Dienst zu authentifizieren.

    Wenn Sie Azure CLI-Skripts erstellen, um den Aufruf von Batch-Befehlen zu automatisieren, können Sie entweder die Authentifizierung mit gemeinsam verwendetem Schlüssel oder einen Azure AD-Dienstprinzipal verwenden. In einigen Szenarien ist die Authentifizierung mit gemeinsam verwendetem Schlüssel möglicherweise einfacher als die Erstellung eines Dienstprinzipals.  

    Um sich mithilfe der Authentifizierung mit gemeinsam verwendetem Schlüssel anzumelden, geben Sie die `--shared-key-auth`-Option in der Befehlszeile an:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

Die im Abschnitt [Beispielskripts für die Shell](#sample-shell-scripts) aufgeführten Beispiele zeigen, wie Sie sowohl Azure AD als auch einen gemeinsam verwendeten Schlüssel verwenden, um sich über die Azure CLI bei Ihrem Batch-Konto anzumelden.

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Verwenden von Azure Batch-CLI-Vorlagen und Dateiübertragung (Vorschau)

Mit der Azure-Befehlszeilenschnittstelle können Sie vollständige Batch-Aufträge ausführen, ohne Code zu schreiben. Batch-Vorlagendateien unterstützen die Erstellung von Pools, Aufträgen und Aufgaben über die Azure-Befehlszeilenschnittstelle. Sie können die Azure-Befehlszeilenschnittstelle auch verwenden, um Auftragseingabedateien in das dem Batch-Konto zugeordnete Azure Storage-Konto hochzuladen und Auftragsausgabedateien daraus herunterzuladen. Weitere Informationen finden Sie unter [Verwenden von Azure Batch-CLI-Vorlagen und Dateiübertragung (Vorschau)](batch-cli-templates.md).

## <a name="sample-shell-scripts"></a>Beispielskripts für die Shell

Die in der folgenden Tabelle aufgeführten Beispielskripts zeigen, wie Sie Azure CLI-Befehle mit dem Batch-Dienst und dem Batch Management-Dienst verwenden, um häufig auftretende Tasks auszuführen. Diese Beispielskripts enthalten viele der Befehle, die in der Azure CLI für Batch verfügbar sind. 

| Skript | Hinweise |
|---|---|
| [Erstellen eines Batch-Kontos](./scripts/batch-cli-sample-create-account.md) | Erstellt ein Batch-Konto und verknüpft es mit einem Speicherkonto. |
| [Hinzufügen einer Anwendung](./scripts/batch-cli-sample-add-application.md) | Fügt eine Anwendung hinzu und lädt die gepackten Binärdateien hoch|
| [Verwalten von Batch-Pools](./scripts/batch-cli-sample-manage-pool.md) | Veranschaulicht das Erstellen und Verwalten von Pools sowie das Ändern ihrer Größe. |
| [Ausführen eines Auftrags und von Tasks mit Batch](./scripts/batch-cli-sample-run-job.md) | Veranschaulicht die Ausführung eines Auftrags und das Hinzufügen von Tasks |

## <a name="json-files-for-resource-creation"></a>JSON-Dateien für die Erstellung von Ressourcen

Beim Erstellen von Batch-Ressourcen wie Pools und Aufträgen können Sie eine JSON-Datei mit der Konfiguration der neuen Ressource angeben, anstatt die entsprechenden Parameter als Befehlszeilenoptionen zu übergeben. Beispiel:

```azurecli
az batch pool create my_batch_pool.json
```

Die meisten Batch-Ressourcen können zwar allein mithilfe von Befehlszeilenoptionen erstellt werden, manche Features erfordern jedoch die Angabe einer JSON-Datei mit den Ressourcendetails. Eine JSON-Datei ist beispielsweise erforderlich, wenn Sie Ressourcendateien für eine Startaufgabe angeben möchten.

Sie finden die für die Ressourcenerstellung erforderliche JSON-Syntax in der [Batch REST-API-Referenz][rest_api]. Jedes Thema zum „Hinzufügen eines *Ressourcentyps*“ in der REST API-Referenz enthält JSON-Beispielskripts zum Erstellen dieser Ressource. Sie können diese JSON-Beispielskripts als Vorlage für JSON-Dateien verwenden, die Sie mit der Azure CLI verwenden möchten. Die JSON-Syntax für die Erstellung eines Pools finden Sie beispielsweise unter [Hinzufügen eines Pools zu einem Konto][rest_add_pool].

Ein Beispielskript, das eine JSON-Datei angibt, finden Sie unter [Ausführen eines Auftrags und von Tasks mit Batch](./scripts/batch-cli-sample-run-job.md).

> [!NOTE]
> Wenn Sie bei der Ressourcenerstellung eine JSON-Datei angeben, werden alle anderen Parameter, die Sie an der Befehlszeile für diese Ressource angeben, ignoriert.
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>Effiziente Abfragen für Batch-Ressourcen

Jeder Batch-Ressourcentyp unterstützt einen Befehl vom Typ `list` , um Ihr Batch-Konto abzufragen und Ressourcen des entsprechenden Typs aufzulisten. So können Sie beispielsweise die Pools in Ihrem Konto und die Aufgaben in einem Auftrag auflisten:

```azurecli
az batch pool list
az batch task list --job-id job001
```

Wenn Sie den Batch-Dienst mit einem `list`-Vorgang abfragen, können Sie eine OData-Klausel angeben, um die Menge an zurückgegebenen Daten zu begrenzen. Da sämtliche Filtervorgänge auf dem Server stattfinden, werden nur die von Ihnen angeforderten Daten übermittelt. Verwenden Sie diese Klauseln, um bei Auflistungsvorgängen Bandbreite (und Zeit) zu sparen.

Die folgende Tabelle beschreibt die OData-Klauseln, die vom Batch-Dienst unterstützt werden:

| Klausel | Beschreibung |
|---|---|
| `--select-clause [select-clause]` | Gibt für jede Entität eine Teilmenge der Eigenschaften zurück. |
| `--filter-clause [filter-clause]` | Gibt nur Entitäten zurück, die dem angegebenen OData-Ausdruck entsprechen. |
| `--expand-clause [expand-clause]` | Ruft die Entitätsinformationen in einem einzelnen zugrunde liegenden REST-Aufruf ab. Die expand-Klausel unterstützt nur die `stats`-Eigenschaft. |

Ein Beispielskript zur Verwendung einer OData-Klausel finden Sie unter [Ausführen eines Auftrags und von Tasks mit Batch](./scripts/batch-cli-sample-run-job.md).

Weitere Informationen zur Ausführung effizienter Listenabfragen mit OData-Klauseln finden Sie unter [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md).

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

Die folgenden Hinweise können beim Beheben von Problemen mit der Azure CLI helfen:

* Verwenden Sie `-h` , um **Hilfetext** für Befehle der Befehlszeilenschnittstelle anzuzeigen.
* Verwenden Sie `-v` und `-vv`, um eine **ausführliche** Befehlsausgabe anzuzeigen. Wenn das `-vv`-Flag enthalten ist, zeigt die Azure CLI die tatsächlichen REST-Anforderungen und -Antworten. Mit diesen praktischen Switches können Sie die vollständige Fehlerausgabe anzeigen.
* Mit der Option `--json` können Sie die **Befehlsausgabe als JSON** anzeigen. `az batch pool show pool001 --json` zeigt beispielsweise die Eigenschaften von „pool001“ im JSON-Format an. Diese Ausgabe können Sie dann kopieren und ändern, um sie in einer `--json-file` zu verwenden (weitere Informationen finden Sie weiter oben in diesem Artikel unter [JSON-Dateien](#json-files)).
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting to any location.--->
* Das [Batch-Forum][batch_forum] wird von Mitgliedern des Batch-Teams überwacht. Sie können dort Fragen stellen, wenn Probleme auftreten oder Sie bei einem bestimmten Vorgang Hilfe benötigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).
* Weitere Informationen zu Batch-Ressourcen finden Sie in der [Übersicht über Azure Batch für Entwickler](batch-api-basics.md).
* Weitere Informationen dazu, wie Sie mithilfe von Batch-Vorlagen Pools, Aufträge und Aufgaben ohne Programmieraufwand erstellen, finden Sie unter [Verwenden von Azure Batch-CLI-Vorlagen und Dateiübertragung (Vorschau)](batch-cli-templates.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

