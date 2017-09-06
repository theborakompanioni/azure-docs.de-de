---
title: Sicherer Zugriff auf Azure Logic Apps | Microsoft-Dokumentation
description: "Legen Sie die Sicherheit fest für den Schutz des Zugriffs auf Trigger, Eingaben und Ausgaben, Aktionsparameter und Dienste, die mit Workflows in Azure Logic Apps verwendet werden."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 0528d660f590e106f61729f10f8f68da3fe58cb7
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="secure-access-to-your-logic-apps"></a>Sichern des Zugriffs auf Ihre Logik-Apps

Es stehen viele Tools zur Verfügung, um Ihre Logik-App zu sichern.

* Sichern des Zugriffs zum Auslösen einer Logik-App (HTTP-Anforderungstrigger)
* Sichern des Zugriffs zum Verwalten, Bearbeiten oder Lesen einer Logik-App
* Sichern des Zugriffs auf Inhalte von Eingaben und Ausgaben für eine Ausführung
* Sichern von Parametern oder Eingaben in Aktionen in einem Workflow
* Sichern des Zugriffs auf Dienste, die Anforderungen von einem Workflow empfangen

## <a name="secure-access-to-trigger"></a>Sichern des Zugriffs für Trigger

Bei der Arbeit mit einer Logik-App, die durch eine HTTP-Anforderung ([Anforderung](../connectors/connectors-native-reqres.md) oder [Webhook](../connectors/connectors-native-webhook.md)) ausgelöst wird, können Sie den Zugriff einschränken, sodass nur autorisierte Clients die Logik-App auslösen können. Alle Anforderungen an eine Logik-App werden verschlüsselt und mit SSL gesichert.

### <a name="shared-access-signature"></a>Shared Access Signature (SAS)

Jeder Anforderungsendpunkt einer Logik-App enthält als Teil der URL eine [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md). Jede URL enthält einen `sp`-, `sv`- und `sig`-Abfrageparameter. Berechtigungen werden mit `sp` festgelegt und entsprechen zulässigen HTTP-Methoden. `sv` ist die Version, die zum Generieren verwendet wurde. `sig` dient zum Authentifizieren des Zugriffs für Trigger. Die Signatur wird mit dem SHA256-Algorithmus mit einem geheimen Schlüssel für alle URL-Pfade und -Eigenschaften generiert. Der geheime Schlüssel wird nie verfügbar gemacht bzw. veröffentlicht. Er bleibt verschlüsselt und wird als Teil der Logik-App gespeichert. Die Logik-App autorisiert nur Trigger, die eine gültige Signatur enthalten, die mit dem geheimen Schlüssel erstellt wurde.

#### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

Sie können jederzeit über die REST-API oder das Azure-Portal erneut einen neuen sicheren Schlüssel generieren. Alle aktuellen URLs, die zuvor mit dem alten Schlüssel generiert wurden, werden ungültig und sind nicht mehr berechtigt, die Logik-App auszulösen.

1. Öffnen Sie im Azure-Portal die Logik-App, für die Sie erneut einen Schlüssel generieren möchten.
1. Klicken Sie unter **Einstellungen** auf das Menüelement **Zugriffsschlüssel**.
1. Wählen Sie den Schlüssel aus, den Sie erneut generieren möchten, und schließen Sie den Vorgang ab.

URLs, die Sie nach der erneuten Generierung abrufen, werden mithilfe des neuen Zugriffsschlüssels signiert.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Erstellen von Rückruf-URLs mit einem Ablaufdatum

Wenn Sie die URL an andere Parteien weitergeben, können Sie nach Bedarf URLs mit bestimmten Schlüsseln und Ablaufdaten generieren. Sie können dann nahtlos Schlüssel bereitstellen oder sicherstellen, dass der Zugriff zum Auslösen einer App auf eine bestimmte Zeitspanne beschränkt ist. Über die [Logik-Apps-REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) können Sie ein Ablaufdatum für eine URL angeben:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Nehmen Sie in den Text die Eigenschaft `NotAfter` als eine JSON-Datumszeichenfolge auf, die eine Rückruf-URL zurückgibt, die nur bis zum Datums- und Uhrzeitwert `NotAfter` gültig ist.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Erstellen von URLs mit einem primären oder sekundären geheimen Schlüssel

Beim Generieren oder Auflisten von Rückruf-URLs für anforderungsbasierte Trigger können Sie auch angeben, welcher Schlüssel zum Signieren der URL verwendet werden soll.  Sie können mit der [Logik-Apps-REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) wie folgt eine mit einem bestimmten Schlüssel signierte URL generieren:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Nehmen Sie in den Text die Eigenschaft `KeyType` entweder als `Primary` oder `Secondary` auf.  Dadurch wird eine URL zurückgegeben, die mit dem angegebenen sicheren Schlüssel signiert wurde.

### <a name="restrict-incoming-ip-addresses"></a>Beschränken eingehender IP-Adressen

Zusätzlich zur Verwendung der Shared Access Signature möchten Sie möglicherweise das Aufrufen einer Logik-App auf bestimmte Clients beschränken.  Beispiel: Wenn Sie den Endpunkt mithilfe von Azure API Management verwalten, können Sie für die Logik-App festlegen, dass sie eine Anforderung nur annimmt, wenn die Anforderung von der IP-Adresse der API Management-Instanz stammt.

Diese Einstellung kann in den Logik-App-Einstellungen konfiguriert werden:

1. Öffnen Sie im Azure-Portal die Logik-App, der Sie IP-Adressbeschränkungen hinzufügen möchten.
1. Klicken Sie unter **Einstellungen** auf das Menüelement **Konfiguration der Zugriffssteuerung**.
1. Geben Sie die Liste der IP-Adressbereiche an, die vom Trigger akzeptiert werden sollen.

Ein gültiger IP-Adressbereich weist das Format `192.168.1.1/255` auf. Wenn die Logik-App nur als geschachtelte Logik-App ausgelöst werden soll, wählen Sie die Option **Nur andere Logik-Apps** aus. Durch diese Option wird ein leeres Array in die Ressource geschrieben. Dies bedeutet, dass nur Aufrufe vom Dienst selbst (übergeordnete Logik-Apps) erfolgreich ausgelöst werden.

> [!NOTE]
> Sie können weiterhin eine Logik-App mit einem Anforderungstrigger über REST-API/Management `/triggers/{triggerName}/run` ausführen, unabhängig von der IP. In diesem Szenario ist eine Authentifizierung über die Azure-REST-API erforderlich, und alle Ereignisse werden dann im Azure-Überwachungsprotokoll angezeigt. Legen Sie die Richtlinien für die Zugriffssteuerung entsprechend fest.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Festlegen von IP-Adressbereichen in der Ressourcendefinition

Bei Verwendung einer [Bereitstellungsvorlage](logic-apps-create-deploy-template.md) zum Automatisieren Ihrer Bereitstellungen können die IP-Bereichseinstellungen in der Ressourcenvorlage konfiguriert werden.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Hinzufügen von Azure Active Directory, OAuth oder weiteren Sicherheitsfunktionen

Um zusätzlich zu einer Logik-App weitere Autorisierungsprotokolle hinzuzufügen, bietet [Azure API Management](https://azure.microsoft.com/services/api-management/) umfangreiche Überwachungsfunktionen, Sicherheit, Gruppenrichtlinien und Dokumentationen für einen beliebigen Endpunkt mit der Möglichkeit, eine Logik-App als API verfügbar zu machen. Azure API Management kann einen öffentlichen oder privaten Endpunkt für die Logik-App verfügbar machen. Dadurch können Azure Active Directory, Zertifikate, OAuth oder andere Sicherheitsstandards genutzt werden. Wenn eine Anforderung empfangen wird, leitet Azure API Management die Anforderung an die Logik-App weiter (und führt dabei alle erforderlichen Transformationen oder Einschränkungen durch). Sie können die Einstellungen für den eingehenden IP-Bereich der Logik-App verwenden, um vorzuschreiben, dass die Logik-App nur über API Management ausgelöst werden kann.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Sichern des Zugriffs zum Verwalten oder Bearbeiten von Logik-Apps

Damit nur bestimmte Benutzer oder Gruppen Vorgänge für die Ressource ausführen können, können Sie den Zugriff auf Verwaltungsvorgänge für eine Logik-App einschränken. Für Logik-Apps wird die [rollenbasierte Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-configure.md) von Azure verwendet, und sie können mit den gleichen Tools angepasst werden.  Es gibt auch einige integrierte Rollen, die Sie Mitgliedern Ihres Abonnements zuweisen können:

* **Mitwirkender für Logik-Apps**: Bietet Zugriff zum Anzeigen, Bearbeiten und Aktualisieren einer Logik-App.  Das Entfernen der Ressource oder das Ausführen von Verwaltungsvorgängen ist nicht möglich.
* **Logik-App-Operator**: Kann die Logik-App und den Ausführungsverlauf anzeigen und sie aktivieren/deaktivieren.  Das Bearbeiten oder Aktualisieren der Definition ist nicht möglich.

Sie können auch [Azure-Ressourcensperren](../azure-resource-manager/resource-group-lock-resources.md) verwenden, um das Ändern oder Löschen von Logik-Apps zu verhindern. Diese Funktion ist nützlich, um zu verhindern, dass Produktionsressourcen geändert oder gelöscht werden.

## <a name="secure-access-to-contents-of-the-run-history"></a>Sichern des Zugriffs auf Inhalte des Ausführungsverlaufs

Sie können den Zugriff auf Inhalte von Eingaben oder Ausgaben früherer Ausführungen auf bestimmte IP-Adressbereiche beschränken.  

Alle Daten innerhalb einer Workflowausführung werden während der Übertragung und im Ruhezustand verschlüsselt. Wenn der Ausführungsverlauf aufgerufen wird, authentifiziert der Dienst die Anforderung und stellt Links zur Anforderung und zu Antworteingaben und -ausgaben bereit. Dieser Link kann geschützt werden, sodass nur Anforderungen zum Anzeigen des Inhalts von einem bestimmten IP-Adressbereich die Inhalte zurückgibt. Sie können diese Funktion für eine zusätzliche Zugriffssteuerung verwenden. Sie können auch eine IP-Adresse wie `0.0.0.0` angeben, damit niemand auf Eingaben/Ausgaben zugreifen kann. Nur Benutzer mit Administratorberechtigungen könnten diese Einschränkung entfernen, sodass die Möglichkeit für „Just-In-Time-Zugriff“ auf Workflowinhalte besteht.

Diese Einstellung kann in den Ressourceneinstellungen des Azure-Portals konfiguriert werden:

1. Öffnen Sie im Azure-Portal die Logik-App, der Sie IP-Adressbeschränkungen hinzufügen möchten.
1. Klicken Sie unter **Einstellungen** auf das Menüelement **Konfiguration der Zugriffssteuerung**.
1. Geben Sie die Liste der IP-Adressbereiche für den Zugriff auf den Inhalt an.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Festlegen von IP-Adressbereichen in der Ressourcendefinition

Bei Verwendung einer [Bereitstellungsvorlage](logic-apps-create-deploy-template.md) zum Automatisieren Ihrer Bereitstellungen können die IP-Bereichseinstellungen in der Ressourcenvorlage konfiguriert werden.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Sichern von Parametern und Eingaben innerhalb eines Workflows

Sie könnten einige Aspekte einer Workflowdefinition für umgebungsübergreifende Bereitstellungen parametrisieren. Darüber hinaus können einige dieser Parameter sichere Parameter sein, die nicht angezeigt werden sollen, wenn ein Workflow bearbeitet wird, z.B. eine Client-ID und ein geheimer Clientschlüssel für die [Azure Active Directory-Authentifizierung](../connectors/connectors-native-http.md#authentication) einer HTTP-Aktion.

### <a name="using-parameters-and-secure-parameters"></a>Verwenden von Parametern und sicheren Parametern

Für den Zugriff auf den Wert eines Ressourcenparameters zur Laufzeit bietet die [Workflowdefinitionssprache](http://aka.ms/logicappsdocs) einen `@parameters()`-Vorgang. Darüber hinaus können Sie [Parameter in der Ressourcenbereitstellungsvorlage angeben](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Wenn Sie jedoch den Parametertyp als `securestring` angeben, wird der Parameter nicht mit dem Rest der Ressourcendefinition zurückgegeben und auf ihn kann nicht durch Anzeigen der Ressource nach der Bereitstellung zugegriffen werden.

> [!NOTE]
> Wenn der Parameter im Header oder Text einer Anforderung verwendet wird, kann der Parameter durch den Zugriff auf den Ausführungsverlauf und die ausgehende HTTP-Anforderung sichtbar sein. Achten Sie darauf, dass Sie Ihre Richtlinien für den Zugriff auf Inhalte entsprechend festlegen.
> Autorisierungsheader sind nie über Eingaben oder Ausgaben sichtbar. Wenn der geheime Schlüssel also dort verwendet wird, ist der geheime Schlüssel nicht abrufbar.

#### <a name="resource-deployment-template-with-secrets"></a>Ressourcenbereitstellungsvorlage mit geheimen Schlüsseln

Das folgende Beispiel veranschaulicht eine Bereitstellung, die zur Laufzeit auf den sicheren Parameter `secret` verweist. In einer separaten Parameterdatei könnten Sie den Umgebungswert für `secret` angeben oder den [Azure Resource Manager-Schlüsseltresor](../azure-resource-manager/resource-manager-keyvault-parameter.md) nutzen, um geheime Schlüssel zum Zeitpunkt der Bereitstellung abzurufen.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is the request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Sichern des Zugriffs auf Dienste, die Anforderungen von einem Workflow empfangen

Es gibt viele Methoden zum Sichern eines Endpunkts, auf den die Logik-App zugreifen muss.

### <a name="using-authentication-on-outbound-requests"></a>Verwenden der Authentifizierung für ausgehende Anforderungen

Bei der Arbeit mit einer Aktion vom Typ HTTP, HTTP + Swagger (offene API) oder Webhook können Sie der gesendeten Anforderung eine Authentifizierung hinzufügen. Sie könnten eine Standardauthentifizierung, eine Zertifikatauthentifizierung oder eine Azure Active Directory-Authentifizierung einschließen. Informationen zum Konfigurieren dieser Authentifizierung finden Sie [in diesem Artikel](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Einschränken des Zugriffs auf Logik-App-IP-Adressen

Alle Aufrufe von Logik-Apps stammen aus einem bestimmten Satz von IP-Adressen pro Region. Sie können zusätzliche Filter hinzufügen, um nur Anforderungen von diesen IP-Adressen anzunehmen. Eine Liste der IP-Adressen finden Sie unter [Logik-App-Grenzwerte und -Konfiguration](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Lokale Konnektivität

Logik-Apps ermöglichen die Integration in mehrere Dienste, um eine sichere und zuverlässige lokale Kommunikation bereitzustellen.

#### <a name="on-premises-data-gateway"></a>Lokales Datengateway

Viele verwaltete Connectors oder Logik-Apps bieten sichere Verbindungen mit lokalen Systemen, einschließlich des Dateisystems, SQL, SharePoint, DB2 und mehr. Das Gateway überträgt Daten aus lokalen Quellen durch verschlüsselte Kanäle über Azure Service Bus. Der gesamte Datenverkehr stammt als sicherer ausgehender Datenverkehr vom Gateway-Agent. Erfahren Sie mehr darüber, [wie das Datengateway funktioniert](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) bietet lokale Konnektivitätsoptionen, einschließlich Site-to-Site-VPN- und ExpressRoute-Integration, für geschützte Proxys und eine gesicherte Kommunikation mit lokalen Systemen. Im Logik-App-Designer können Sie schnell innerhalb eines Workflows eine API auswählen, die über Azure API Management verfügbar gemacht wird, und so schnellen Zugriff auf lokale Systeme bereitstellen.

#### <a name="hybrid-connections-from-azure-app-service"></a>Hybridverbindungen über Azure App Service

Sie können die Funktion für lokale Hybridverbindungen für Azure-API- und Web-Apps verwenden, um lokal zu kommunizieren.  Informationen zu Hybridverbindungen und zu deren Konfiguration finden Sie [in diesem Artikel](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Bereitstellungsvorlage](logic-apps-create-deploy-template.md)  
[Fehlerbehandlung](logic-apps-exception-handling.md)  
[Überwachen von Logik-Apps](logic-apps-monitor-your-logic-apps.md)  
[Diagnostizieren von Fehlern und Problemen bei Logik-Apps](logic-apps-diagnosing-failures.md)  

