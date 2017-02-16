---
title: Sichern einer Azure-Logik-App | Microsoft-Dokumentation
description: Sichern von Eingaben, Zugriff und Steuerung einer Azure-Logik-App
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
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: b5545e089a67a16d20cac6bb1d8067a6ff9fcb17
ms.openlocfilehash: e27e4f42722cd819f03f7271e72463b5781d2725


---

# <a name="securing-a-logic-app"></a>Sichern einer Logik-App

Es stehen viele Tools zur Verfügung, um Ihre Logik-App zu sichern.

* Sichern des Zugriffs zum Auslösen einer Logik-App (HTTP-Anforderungstrigger)
* Sichern des Zugriffs zum Verwalten, Bearbeiten oder Lesen einer Logik-App
* Sichern des Zugriffs auf Inhalte von Eingaben und Ausgaben für eine Ausführung
* Sichern von Parametern oder Eingaben in Aktionen in einem Workflow
* Sichern des Zugriffs auf Dienste, die Anforderungen von einem Workflow empfangen

## <a name="secure-access-to-trigger"></a>Sichern des Zugriffs für Trigger

Bei der Arbeit mit einer Logik-App, die durch eine HTTP-Anforderung ([Anforderung](../connectors/connectors-native-reqres.md) oder [Webhook](../connectors/connectors-native-webhook.md)) ausgelöst wird, können Sie den Zugriff einschränken, sodass nur autorisierte Clients die Logik-App auslösen können.  Alle Anforderungen an eine Logik-App werden verschlüsselt und mit SSL gesichert.

### <a name="shared-access-signature"></a>Shared Access Signature (SAS)

Jeder Anforderungsendpunkt einer Logik-App enthält als Teil der URL eine [Shared Access Signature](../storage/storage-dotnet-shared-access-signature-part-1.md) (SAS).  Jede URL enthält einen `sp`-, `sv`- und `sig`-Abfrageparameter.  Berechtigungen werden mit `sp` festgelegt und entsprechen zulässigen HTTP-Methoden. `sv` ist die Version, die zum Generieren verwendet wurde. `sig` dient zum Authentifizieren des Zugriffs für Trigger.  Er wird mit dem SHA256-Algorithmus mit einem geheimen Schlüssel für alle URL-Pfade und -Eigenschaften generiert.  Der geheime Schlüssel wird nie verfügbar gemacht bzw. veröffentlicht. Er bleibt verschlüsselt und wird als Teil der Logik-App gespeichert.  Die Logik-App autorisiert nur Trigger, die eine gültige Signatur enthalten, die mit dem geheimen Schlüssel erstellt wurde.

#### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

Sie können jederzeit über die REST-API oder das Azure-Portal erneut einen neuen sicheren Schlüssel generieren.  Alle aktuellen URLs, die zuvor mit dem alten Schlüssel generiert wurden, werden ungültig und sind nicht mehr berechtigt, die Logik-App auszulösen.

1. Öffnen Sie im Azure-Portal die Logik-App, für die Sie erneut einen Schlüssel generieren möchten.
1. Klicken Sie unter **Einstellungen** auf das Menüelement **Zugriffsschlüssel**.
1. Wählen Sie den Schlüssel aus, den Sie erneut generieren möchten, und schließen Sie den Vorgang ab.

URLs, die Sie nach der erneuten Generierung abrufen, werden mithilfe des neuen Zugriffsschlüssels signiert.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Erstellen von Rückruf-URLs mit einem Ablaufdatum

Wenn Sie die URL an andere Parteien weitergeben, können Sie nach Bedarf URLs mit bestimmten Schlüsseln und Ablaufdaten generieren.  So können Sie nahtlos Schlüssel bereitstellen oder sicherstellen, dass der Zugriff zum Auslösen einer App auf eine bestimmte Zeitspanne beschränkt ist.  Über die [Logik-Apps-REST-API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) können Sie wie folgt ein Ablaufdatum für eine URL angeben:

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

Ein gültiger IP-Adressbereich weist das Format `192.168.1.1/255` auf.  Wenn die Logik-App nur als geschachtelte Logik-App ausgelöst werden soll, wählen Sie die Option **Nur andere Logik-Apps** aus.  Dadurch wird ein leeres Array in die Ressource geschrieben. Dies bedeutet, dass nur Aufrufe vom Dienst selbst (übergeordnete Logik-Apps) erfolgreich ausgelöst werden.

> [!NOTE]
> Eine Logik-App mit einem Anforderungstrigger könnte weiterhin über REST-API/Management `/triggers/{triggerName}/run` ausgeführt werden, unabhängig von der IP.  In diesem Fall wäre eine Authentifizierung über die Azure-REST-API erforderlich, und alle Ereignisse werden dann im Azure-Überwachungsprotokoll angezeigt.  Legen Sie die Richtlinien für die Zugriffssteuerung entsprechend fest.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Festlegen von IP-Adressbereichen in der Ressourcendefinition

Bei Verwendung einer [Bereitstellungsvorlage](app-service-logic-create-deploy-template.md) zum Automatisieren Ihrer Bereitstellungen können die IP-Bereichseinstellungen in der Ressourcenvorlage konfiguriert werden.  

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

Wenn Sie zusätzliche Autorisierungsprotokolle zur Logik-App hinzufügen möchten, verwenden Sie [Azure API Management](https://azure.microsoft.com/services/api-management/).  Dies ermöglicht umfangreiche Funktionen für Überwachung, Sicherheit, Richtlinien und Dokumentation für einen beliebigen Endpunkt, und die Logik-App kann als eine API verfügbar gemacht werden.  Azure API Management kann einen öffentlichen oder privaten Endpunkt für die Logik-App verfügbar machen. Dadurch können Azure Active Directory, Zertifikate, OAuth oder andere Sicherheitsstandards genutzt werden.  Wenn eine Anforderung empfangen wird, leitet Azure API Management die Anforderung an die Logik-App weiter (und führt dabei alle erforderlichen Transformationen oder Einschränkungen durch).  Sie können die Einstellungen für den eingehenden IP-Bereich der Logik-App verwenden, um vorzuschreiben, dass die Logik-App nur über API Management ausgelöst werden kann.

## <a name="secure-access-to-manage-or-edit-a-logic-app"></a>Sichern des Zugriffs zum Verwalten oder Bearbeiten einer Logik-App

Damit nur bestimmte Benutzer oder Gruppen Vorgänge für die Ressource ausführen können, können Sie den Zugriff auf Verwaltungsvorgänge für eine Logik-App einschränken.  Für Logik-Apps wird die [rollenbasierte Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-configure.md) von Azure verwendet, und sie können mit den gleichen Tools angepasst werden.  Es gibt auch einige integrierte Rollen, die Sie Mitgliedern Ihres Abonnements zuweisen können:

* **Mitwirkender für Logik-Apps**: Bietet Zugriff zum Anzeigen, Bearbeiten und Aktualisieren einer Logik-App.  Das Entfernen der Ressource oder das Ausführen von Verwaltungsvorgängen ist nicht möglich.
* **Logik-App-Operator**: Kann die Logik-App und den Ausführungsverlauf anzeigen und sie aktivieren/deaktivieren.  Das Bearbeiten oder Aktualisieren der Definition ist nicht möglich.

Sie können auch die [Azure-Ressourcensperre](../azure-resource-manager/resource-group-lock-resources.md) nutzen, um das Ändern oder Löschen einer Logik-App zu verhindern.  Dies ist nützlich, um zu verhindern, dass Produktionsressourcen geändert oder gelöscht werden.

## <a name="secure-access-to-contents-of-the-run-history"></a>Sichern des Zugriffs auf Inhalte des Ausführungsverlaufs

Sie können den Zugriff auf Inhalte von Eingaben oder Ausgaben früherer Ausführungen auf bestimmte IP-Adressbereiche beschränken.  

Alle Daten innerhalb einer Workflowausführung werden während der Übertragung und im Ruhezustand verschlüsselt.  Wenn der Ausführungsverlauf aufgerufen wird, authentifiziert der Dienst die Anforderung und stellt Links zur Anforderung und zu Antworteingaben und -ausgaben bereit.  Dieser Link kann geschützt werden, sodass nur Anforderungen zum Anzeigen des Inhalts von einem bestimmten IP-Adressbereich die Inhalte zurückgibt.  Dies kann für eine zusätzliche Zugriffssteuerung verwendet werden.  Sie können auch eine IP-Adresse wie `0.0.0.0` angeben, damit niemand auf Eingaben/Ausgaben zugreifen kann.  Nur Benutzer mit Administratorberechtigungen könnten diese Einschränkung entfernen, sodass die Möglichkeit für „Just-In-Time-Zugriff“ auf Workflowinhalte besteht.

Diese Einstellung kann in den Ressourceneinstellungen des Azure-Portals konfiguriert werden:

1. Öffnen Sie im Azure-Portal die Logik-App, der Sie IP-Adressbeschränkungen hinzufügen möchten.
1. Klicken Sie unter **Einstellungen** auf das Menüelement **Konfiguration der Zugriffssteuerung**.
1. Geben Sie die Liste der IP-Adressbereiche für den Zugriff auf den Inhalt an.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Festlegen von IP-Adressbereichen in der Ressourcendefinition

Bei Verwendung einer [Bereitstellungsvorlage](app-service-logic-create-deploy-template.md) zum Automatisieren Ihrer Bereitstellungen können die IP-Bereichseinstellungen in der Ressourcenvorlage konfiguriert werden.  

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

Es gibt einige Aspekte einer Workflowdefinition, die Sie für umgebungsübergreifende Bereitstellungen parametrisieren könnten.  Darüber hinaus können einige dieser Parameter sichere Parameter sein, die nicht angezeigt werden sollen, wenn ein Workflow bearbeitet wird, z.B. eine Client-ID und ein geheimer Clientschlüssel für die [Azure Active Directory-Authentifizierung](../connectors/connectors-native-http.md#authentication) einer HTTP-Aktion.

### <a name="using-parameters-and-secure-parameters"></a>Verwenden von Parametern und sicheren Parametern

Die [Workflowdefinitionssprache](http://aka.ms/logicappsdocs) bietet einen `@parameters()`-Vorgang für den Zugriff auf den Wert eines Ressourcenparameters zur Laufzeit.  Darüber hinaus können Sie [Parameter in der Ressourcenbereitstellungsvorlage angeben](../azure-resource-manager/resource-group-authoring-templates.md#parameters).  Wenn Sie den Parametertyp als `securestring` angeben, wird er nicht mit dem Rest der Ressourcendefinition zurückgegeben, d.h., auf ihn kann nicht durch Anzeigen der Ressource nach der Bereitstellung zugegriffen werden.

> [!NOTE]
> Wenn der Parameter im Header oder Text der Anforderung verwendet wird, kann er durch den Zugriff auf den Ausführungsverlauf und die ausgehende HTTP-Anforderung sichtbar sein.  Achten Sie darauf, dass Sie Ihre Richtlinien für den Zugriff auf Inhalte entsprechend festlegen.
> Autorisierungsheader sind nie über Eingaben oder Ausgaben sichtbar. Wenn also der geheime Schlüssel verwendet wird, kann er nicht abgerufen werden.

#### <a name="resource-deployment-template-with-secrets"></a>Ressourcenbereitstellungsvorlage mit geheimen Schlüsseln

Im Folgenden sehen Sie ein Beispiel für eine Bereitstellung, die zur Laufzeit auf den sicheren Parameter `secret` verweist.  In einer separaten Parameterdatei könnten Sie den Umgebungswert für `secret` angeben oder den [Azure Resource Manager-Schlüsseltresor](../azure-resource-manager/resource-manager-keyvault-parameter.md) nutzen, um die geheimen Schlüssel zum Zeitpunkt der Bereitstellung abzurufen.

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

Bei der Arbeit mit einer Aktion vom Typ HTTP, HTTP + Swagger (offene API) oder Webhook können Sie der gesendeten Anforderung eine Authentifizierung hinzufügen.  Dies könnte eine Standardauthentifizierung, eine Zertifikatauthentifizierung oder eine Azure Active Directory-Authentifizierung sein.  Informationen zum Konfigurieren dieser Authentifizierung finden Sie [in diesem Artikel](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Einschränken des Zugriffs auf Logik-App-IP-Adressen

Alle Aufrufe von Logik-Apps stammen aus einem bestimmten Satz von IP-Adressen pro Region.  Sie können zusätzliche Filter hinzufügen, um nur Anforderungen von diesen IP-Adressen anzunehmen.  Eine Liste dieser IP-Adressen finden Sie [in diesem Artikel](app-service-logic-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Lokale Konnektivität

Logik-Apps ermöglichen die Integration in eine Reihe von Diensten, um eine sichere und zuverlässige lokale Kommunikation bereitzustellen.

#### <a name="on-premises-data-gateway"></a>Lokales Datengateway

Viele der verwalteten Connectors von Logik-Apps bieten sichere Verbindungen mit lokalen Systemen, einschließlich des Dateisystems, SQL, SharePoint, DB2 und mehr.  Das Gateway nutzt verschlüsselte Kanäle über Azure Service Bus, um Daten lokal weiterzuleiten, und der gesamte Datenverkehr stammt aus dem sicheren ausgehenden Datenverkehr vom Gateway-Agent.  Weitere Informationen zur Funktionsweise des Gateways finden Sie [in diesem Artikel](app-service-logic-gateway-install.md#how-the-gateway-works).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) bietet eine Reihe von lokalen Konnektivitätsoptionen, einschließlich Site-to-Site-VPN- und ExpressRoute-Integration, für geschützte Proxys und eine gesicherte Kommunikation mit lokalen Systemen.  Im Logik-App-Designer können Sie schnell innerhalb eines Workflows eine API auswählen, die über Azure API Management verfügbar gemacht wird, und so schnellen Zugriff auf lokale Systeme bereitstellen.

#### <a name="hybrid-connections-from-azure-app-services"></a>Hybridverbindungen über Azure App Services

Sie können die Funktion für lokale Hybridverbindungen für Azure-API- und Web-Apps verwenden, um lokal zu kommunizieren.  Informationen zu Hybridverbindungen und zu deren Konfiguration finden Sie [in diesem Artikel](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Bereitstellungsvorlage](app-service-logic-create-deploy-template.md)  
[Fehlerbehandlung](app-service-logic-exception-handling.md)  
[Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)  
[Diagnostizieren von Fehlern und Problemen bei Logik-Apps](app-service-logic-diagnosing-failures.md)  



<!--HONumber=Jan17_HO1-->


