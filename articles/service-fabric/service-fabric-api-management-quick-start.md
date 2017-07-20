---
title: Azure Service Fabric mit API Management-Schnellstart | Microsoft-Dokumentation
description: Diese Anleitung beschreibt die ersten Schritte mit Azure API Management und Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 2160e2e65de5c65df8a13248bad4f626def86e49
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---

# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric mit Azure API Management-Schnellstart

Diese Anleitung veranschaulicht das Einrichten des Azure-API-Managements mit Service Fabric und die Konfiguration Ihres ersten API-Vorgangs zum Senden von Datenverkehr an die Back-End-Dienste in Service Fabric. Weitere Informationen zu den Azure API Management-Szenarios mit Service Fabric finden Sie im Artikel [Übersicht](service-fabric-api-management-overview.md). 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a>Bereitstellen von API Management und-Service Fabric in Azure

Der erste Schritt besteht aus der Bereitstellung von API Management und einem Service Fabric-Cluster in Azure in einem gemeinsamen virtuellen Netzwerk. Dies ermöglicht dem API-Management direkt mit Service Fabric zu kommunizieren, damit dieses die Dienstermittlung und die Auflösung der Dienstpartition ausführen und Datenverkehr direkt an einen Back-End-Dienst weiterleiten kann.

### <a name="topology"></a>Topologie

Dieses Handbuch stellt die folgende Topologie in Azure bereit, in welcher API Management und Service Fabric Subnetze im gleichen virtuellen Netzwerk sind:

 ![Bildunterschrift][sf-apim-topology-overview]

Um schnell zu beginnen, werden für jeden Schritt der Bereitstellung Resource Manager-Vorlagen zur Verfügung gestellt:

 - Netzwerktopologie:
    - [network.json][network-arm]
    - [network.parameters.json][network-parameters-arm]
 - Service Fabric-Cluster:
    - [cluster.json][cluster-arm]
    - [cluster.parameters.json][cluster-parameters-arm]
 - API Management:
    - [apim.json][apim-arm]
    - [apim.parameters.json][apim-parameters-arm]

### <a name="sign-in-to-azure-and-select-your-subscription"></a>Melden Sie sich bei Azure an, und wählen Sie Ihr Abonnement aus

In diesem Leitfaden wird [Azure PowerShell][azure-powershell] verwendet. Wenn Sie eine neue PowerShell-Sitzung starten, melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.
 
Melden Sie sich bei Ihrem Azure-Konto an:

```powershell
PS > Login-AzureRmAccount
```

Wählen Sie Ihr Abonnement aus:

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine neue Ressourcengruppe für die Bereitstellung. Geben Sie einen Namen und einen Speicherort ein.

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a>Stellen Sie die Netzwerktopologie bereit

Der erste Schritt ist das Einrichten der Netzwerktopologie, für welche API Management und Service Fabric-Cluster bereitgestellt werden. Die [network.json][network-arm]-Vorlage von Resource Manager ist so konfiguriert, dass ein virtuelles Netzwerk (VNET) mit zwei Subnetzen und zwei Netzwerksicherheitsgruppen (NSG) erstellt wird. 

Die [network.parameters.json][network-parameters-arm]-Parameterdatei enthält die Namen der Subnetze und NSGs, für die API Management und-Service Fabric bereitgestellt werden. Für diese Anleitung müssen die Parameterwerte nicht geändert werden. Die API Management- und Service Fabric-Resource Manager-Vorlagen verwenden diese Werte, weshalb diese, wenn sie hier geändert werden, in den anderen Resource Manager-Vorlagen entsprechend geändert werden müssen. 

 1. Laden Sie die folgenden Resource Manager-Vorlage und Parameterdatei herunter:

    - [network.json][network-arm]
    - [network.parameters.json][network-parameters-arm]

 2. Verwenden Sie den folgenden PowerShell-Befehl, um die Resource-Manager Vorlagen- und Parameterdateien für die Netzwerkinstallation bereitzustellen:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a>Bereitstellen des Service Fabric-Clusters

Nachdem die Bereitstellung der Netzwerkressourcen abgeschlossen ist, liegt der nächste Schritt in der Bereitstellung von Service Fabric-Clustern für das VNET in den für den Service Fabric-Cluster festgelegten Subnetz und NSG. Für dieses Tutorial ist die Service Fabric-Resource Manager-Vorlage für die Verwendung der im vorherigen Schritt eingerichteten Namen des VNET, Subnetzes und NSG vorkonfiguriert. 

Die Resource Manager-Vorlage des Service Fabric-Clusters ist für die Erstellung eines sicheren Cluster mit Zertifikatssicherheit konfiguriert. Das Zertifikat wird zum Sichern der Knoten-zu-Knoten-Kommunikation für Ihren Cluster und zur Verwaltung des Benutzerzugriffs auf Ihre Service Fabric-Cluster verwendet. API Management verwendet dieses Zertifikat, um auf den Service Fabric Naming Service für die Dienstermittlung zuzugreifen.

Dieser Schritt erfordert das Vorhandensein eines Zertifikats in Key Vault für die Clustersicherheit. Weitere Informationen zum Einrichten eines sicheren Clusters mit Key Vault finden Sie in [dieser Anleitung zum Erstellen eines Clusters in Azure mithilfe des Resource Managers](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Sie können die Azure Active Directory-Authentifizierung zusätzlich zu dem für den Clusterzugriff verwendeten Zertifikat hinzufügen. Azure Active Directory ist die empfohlene Vorgehensweise für die Verwaltung des Benutzerzugriffs auf Ihre Service Fabric-Cluster, aber es ist nicht notwendig, dieses Tutorial abzuschließen. Ein Zertifikat wird in beiden Fällen benötigt, sowohl für die Knoten-zu-Knoten-Sicherheit als auch für die Azure API Management-Authentifizierung, die derzeit die Authentifizierung mit Azure Active Directory für ein Service Fabric-Back-End nicht unterstützt.

 1. Laden Sie die folgenden Resource Manager-Vorlage und Parameterdatei herunter:
 
    - [cluster.json][cluster-arm]
    - [cluster.parameters.json][cluster-parameters-arm]

 2. Füllen Sie für Ihre Bereitstellung die leeren Parameter in der `cluster.parameters.json`-Datei, einschließlich der [Key Vault-Informationen](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) für das Cluster-Zertifikat aus.

 3. Verwenden Sie den folgenden PowerShell-Befehl, um die Resource Manager-Vorlagen- und Parameterdateien zum Erstellen der Service Fabric-Cluster bereitzustellen:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Bereitstellen von API Management

Stellen Sie abschließend das API Management im VNET in dem für das API Management festgelegten Subnetz und NSG bereit. Sie müssen nicht warten, bis die Bereitstellung des Service Fabric-Clusters abgeschlossen ist, bevor Sie das API Management bereitstellen. 

Für dieses Tutorial ist die API Management-Resource Manager-Vorlage für die Verwendung der im vorherigen Schritt eingerichteten Namen für VNET, Subnetz und NSG vorkonfiguriert. 

 1. Laden Sie die folgenden Resource Manager-Vorlage und Parameterdatei herunter:
 
    - [apim.json][apim-arm]
    - [apim.parameters.json][apim-parameters-arm]

 2. Füllen Sie die leeren Parameter in der `apim.parameters.json` für Ihre Bereitstellung aus.

 3. Verwenden Sie den folgenden PowerShell-Befehl, um die Resource Manager-Vorlagen und Parameterdateien für das API Management bereitzustellen:

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>Konfigurieren des API Managements

Sobald Ihr API Management- und Service Fabric-Cluster bereitgestellt wurden, können Sie ein Service Fabric-Back-End im API Management konfigurieren. Dadurch können Sie eine Back-End-Dienst-Richtlinie erstellen, die Datenverkehr an Ihr Service Fabric-Cluster sendet.

### <a name="api-management-security"></a>API Management-Sicherheit

Zum Konfigurieren des Service Fabric-Back-Ends müssen Sie zuerst die API Management-Sicherheitseinstellungen konfigurieren. Wechseln Sie zum Konfigurieren der Sicherheitseinstellungen zu Ihrem API Management-Dienst im Azure-Portal.

#### <a name="enable-the-api-management-rest-api"></a>Aktivieren der API Management-REST-API

Die API Management-REST-API ist derzeit die einzige Möglichkeit zum Konfigurieren eines Back-End-Diensts. Der erste Schritt ist das Aktivieren und Sichern der API Management-REST-API.

 1. Wählen Sie im API Management-Dienst **Management API – VORSCHAU** unter **Sicherheit** aus.
 2. Aktivieren Sie das Kontrollkästchen **API Management-REST-API aktivieren**.
 3. Notieren Sie die URL der Verwaltungs-API – dies ist die URL, die wir später zur Einrichtung des Service Fabric-Back-End verwenden werden.
 4. Generieren Sie einen **Zugriffstoken**, indem Sie ein Ablaufdatum und einen Schlüssel auswählen, und klicken Sie dann auf die Schaltfläche **Generieren** am unteren Rand der Seite.
 5. Kopieren Sie den **Zugriffstoken** und speichern Sie ihn – wir werden ihn in den folgenden Schritten benötigen. Beachten Sie, dass sich dies vom primären und sekundären Schlüssel unterscheidet.

#### <a name="upload-a-service-fabric-client-certificate"></a>Hochladen eines Service Fabric-Client-Zertifikats

API Management muss mit Ihrem Service Fabric-Cluster für die Dienstermittlung mithilfe von Clientzertifikaten, die über Zugriff auf den Cluster verfügen, authentifiziert werden. Der Einfachheit halber verwendet dieses Tutorial das gleiche Zertifikat, das beim Erstellen des Service Fabric-Clusters angegeben wird, welches standardmäßig für den Zugriff auf Ihr Cluster verwendet werden kann.

 1. Wählen Sie im API Management-Dienst **Clientzertifikate – VORSCHAU** unter **Sicherheit** aus.
 2. Klicken Sie auf die Schaltfläche **+Hinzufügen**
 2. Wählen Sie die private Schlüsseldatei (PFX) des Clusterzertifikats aus, das Sie beim Erstellen ihres Service Fabric-Clusters angegeben haben, und geben Sie einen Namen und das Kennwort für den privaten Schlüssel ein.

> [!NOTE]
> Dieses Tutorial verwendet das gleiche Zertifikat zur Clientauthentifizierung und Knoten-zu-Knoten-Sicherheit. Sie können ein separates Clientzertifikat verwenden, wenn Sie den Zugriff auf Ihr Service Fabric-Cluster konfiguriert haben.

### <a name="configure-the-backend"></a>Konfigurieren des Back-End

Nachdem die API Management-Sicherheit konfiguriert wurde, können Sie nun das Service Fabric-Back-End konfigurieren. Bei Service Fabric-Back-Ends ist nicht ein bestimmter Service Fabric-Dienst sondern der Service Fabric-Cluster das Back-End. Dies ermöglicht einer einzelnen Richtlinie das Weiterleiten an mehr als einen Dienst im Cluster.

Dieser Schritt erfordert das von Ihnen zuvor erstellte Zugriffstoken und den Fingerabdruck für ihr Cluster-Zertifikat, das Sie im vorherigen Schritt in API Management hochgeladen haben.

> [!NOTE]
> Wenn Sie im vorherigen Schritt ein separates Clientzertifikat für das API Management verwendet haben, benötigen Sie in diesem Schritt zusätzlich zu dem Fingerabdruck des Clusterzertifikats den Fingerabdruck für das Clientzertifikat.

Senden Sie die folgende HTTP-PUT-Anforderung an die API Management-API-URL, die Sie vorher bei der Aktivierung der API Management REST-API zum Konfigurieren des Service Fabric-Back-End-Diensts notiert haben. Wenn der Befehl erfolgreich abgeschlossen wurde, sollte eine `HTTP 201 Created` Antwort angezeigt werden. Weitere Informationen zu jedem Feld finden Sie unter API Management [Back-End-API-Referenzdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

HTTP-Befehl und URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Anforderungsheader:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Anforderungstext:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

Der hier angegebene **url**-Parameter ist ein vollqualifizierter Dienstname eines Diensts in Ihrem Cluster, an den alle Anforderungen standardmäßig weitergeleitet werden, sollte in einer Back-End-Richtlinie kein Dienstname angegeben sein. Sie können einen Fake-Dienstnamen, wie z.B. „fabric:/fake/service“ verwenden, wenn Sie keinen Fallbackdienst wünschen.

In der API Management [Back-End-API-Referenzdokumentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) finden Sie detaillierte Informationen für jedes Feld.

#### <a name="example"></a>Beispiel

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Bereitstellen eines Service Fabric-Back-End-Diensts

Nachdem Sie nun das Service Fabric als ein Back-End für das API Management konfiguriert haben, können Sie für Ihre APIs Back-End-Richtlinien erstellen, die Datenverkehr an Ihre Service Fabric-Dienste senden. Zunächst benötigen Sie jedoch einen Dienst, der in Service Fabric ausgeführt wird, um Anforderungen zu akzeptieren.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Erstellen Sie einen Service Fabric-Dienst mit einem HTTP-Endpunkt

In diesem Tutorial erstellen wir einen einfachen zustandslosen ASP.NET Core-Dienst mithilfe der Projektvorlage für Standard-Web-API. Dadurch wird ein HTTP-Endpunkt für den Dienst erstellt, den Sie über Azure API Management verfügbar machen:

```
/api/values
```

Beginnen Sie mit dem [Einrichten Ihrer Entwicklungsumgebung für die Entwicklung von ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Nachdem Sie Ihre Entwicklungsumgebung eingerichtet haben, starten Sie Visual Studio als Administrator, und erstellen Sie einen ASP.NET Core-Dienst:

 1. Wählen Sie in Visual Studio "Datei -> Neues Projekt" aus.
 2. Wählen Sie die Service Fabric-Anwendungsvorlage unter Cloud aus, und nennen Sie sie **„ApiApplication“**.
 3. Wählen Sie die ASP.NET Core-Dienstvorlage aus, und nennen Sie das Projekt **„WebApiService“**.
 4. Wählen Sie die Web-API ASP.NET Core 1.1-Projektvorlage aus.
 5. Nachdem das Projekt erstellt wurde, öffnen Sie die `PackageRoot\ServiceManifest.xml`, und entfernen Sie das `Port`-Attribut aus der Endpunktressourcenkonfiguration:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Dies ermöglicht Service Fabric, dynamisch einen Port aus dem Portbereich für Anwendungen anzugeben, den wir über die Netzwerksicherheitsgruppe in der Cluster-Resource Manager-Vorlage geöffnet haben, und ermöglicht so den Fluss von Datenverkehr aus dem API Management.
 
 6. Drücken Sie F5 in Visual Studio, um zu überprüfen, ob die Web-API lokal verfügbar ist. 

    Öffnen Sie den Service Fabric Explorer und führen Sie einen Drilldown zu einer bestimmten Instanz des ASP.NET Core-Diensts aus, um die Basisadresse herauszufinden, die durch den Dienst überwacht wird. Fügen Sie `/api/values` an die Basisadresse hinzu, und öffnen Sie sie in einem Browser. Dadurch wird die Get-Methode für den ValuesController in der Web-API-Vorlage aufgerufen. Dieser gibt die standardmäßige Antwort, die von der Vorlage bereitgestellt wird zurück – ein JSON-Array, das zwei Zeichenfolgen enthält:

    ```json
    ["value1", "value2"]`
    ```

    Dies ist der Endpunkt, den Sie über das API Management in Azure verfügbar machen müssen.

 7. Stellen Sie schließlich die Anwendung auf Ihren Cluster in Azure bereit. Klicken Sie [in Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box) mit der rechten Maustaste auf das Anwendungs-Projekt, und wählen Sie **Veröffentlichen** aus. Geben Sie Ihren Clusterendpunkt (z.B. `mycluster.westus.cloudapp.azure.com:19000`) an, um die Anwendung in Ihrem Service Fabric-Cluster in Azure bereitzustellen.

Ein zustandsloser ASP.NET Core-Dienst mit dem Namen `fabric:/ApiApplication/WebApiService` sollte jetzt in Ihrem Service Fabric-Cluster in Azure ausgeführt werden.

## <a name="create-an-api-operation"></a>Erstellen eines API-Vorgangs

Nun sind wir bereit, einen Vorgang im API Management zu erstellen, welchen externe Clients zur Kommunikation mit dem im Service Fabric-Cluster ausgeführten zustandslosen ASP.NET Core-Dienst verwenden.

 1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrer API Management-Dienst-Bereitstellung.
 2. Wählen Sie im API Management-Dienstblatt **APIs – Vorschau** aus
 3. Fügen Sie eine neue API hinzu, indem Sie auf das Feld **leere API** klicken und das Dialogfeld ausfüllen:

     - **Webdienst-URL**: für Service Fabric-Back-Ends wird dieser URL-Wert nicht verwendet. Sie können hier einen beliebigen Wert einfügen. Verwenden Sie für dieses Tutorial `http://servicefabric`.
     - **Name**: Geben Sie einen Namen für Ihre API ein. Verwenden Sie für dieses Tutorial `Service Fabric App`.
     - **URL-Schema**: Wählen Sie HTTP, HTTPS oder beides aus. Verwenden Sie für dieses Tutorial `both`.
     - **API-URL-Suffix**: Geben Sie ein Suffix für unsere API ein. Verwenden Sie für dieses Tutorial `myapp`.
 
 4. Klicken Sie nachdem die API erstellt wurde auf **+ Vorgang hinzufügen**, um einen Front-End-API-Vorgang hinzuzufügen. Füllen Sie die Werte aus:
    
     - **URL**: Wählen Sie `GET` aus und geben Sie einen URL-Pfad für die API an. Verwenden Sie für dieses Tutorial `/api/values`.
     
       Standardmäßig ist der hier angegebene URL-Pfad der URL-Pfad, der an den Back-End-Service Fabric-Dienst gesendet wird. Sollten Sie hier den gleichen URL-Pfad verwenden, den Ihr Dienst verwendet, in diesem Fall `/api/values`, funktioniert der Vorgang ohne weitere Änderung. Sie können hier auch einen URL-Pfad angeben, der sich von dem von Ihrem Back-End-Service Fabric-Dienst verwendeten URL-Pfad unterscheidet. In diesem Fall müssen Sie auch später in Ihrer Vorgangsrichtlinie einen Pfad-Rewrite angeben.
     - **Anzeigename**: Geben Sie einen Namen für die API ein. Verwenden Sie für dieses Tutorial `Values`.

## <a name="configure-a-backend-policy"></a>Konfigurieren Sie eine Back-End-Richtlinie

Die Back-End-Richtlinie verbindet alles. Hier konfigurieren Sie den Back-End-Service Fabric-Dienst, an den Anforderungen weitergeleitet werden. Sie können diese Richtlinie auf alle API-Vorgänge anwenden. Die [Back-End-Konfiguration für Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) bietet die folgenden Anforderungsrouting-Steuerelemente: 
 - Dienstinstanzauswahl durch Angabe eines Service Fabric-Dienst-Instanznamens entweder hartcodiert (z.B. `"fabric:/myapp/myservice"`) oder aus der HTTP-Anforderung generiert (z.B. `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Lösung der Partition durch Generieren eines Partitionsschlüssels mithilfe eines Service Fabric-Partitionierungsschemas.
 - Replikat-Auswahl für zustandsbehaftete Dienste.
 - Retry-Bedingungen der Lösung, mit denen Sie die Bedingungen für das erneute Beheben eines Dienstspeicherorts und erneutes Senden einer Anforderung festlegen können.

Erstellen Sie für dieses Tutorial eine Back-End-Richtlinie, die Anforderungen direkt an den zuvor bereitgestellten zustandslosen ASP.NET Core-Dienst weiterleitet:

 1. Wählen Sie die **eingehenden Richtlinien** für den `Values` Vorgang aus und bearbeiten Sie diese, indem Sie auf das Symbol „Bearbeiten“ klicken und anschließend **Codeansicht** auswählen.
 2. Fügen Sie, wie hier dargestellt, im Gruppenrichtlinien-Code-Editor eine `set-backend-service` Richtlinie unter eingehende Richtlinien hinzu, und klicken Sie auf die Schaltfläche **Speichern**:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

Einen vollständigen Satz von Attributen für die Service Fabric-Back-End-Richtlinie finden Sie in der [API Management-Back-End-Dokumentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

### <a name="add-the-api-to-a-product"></a>Fügen Sie die API einem Produkt hinzu. 

Bevor Sie die API aufrufen können, müssen Sie sie einem Produkt hinzufügen, bei dem Sie Benutzern Zugriff gewähren können. 

 1. Wählen Sie im API Management-Dienst **Produkte – VORSCHAU** aus.
 2. Standardmäßig bietet API Management zwei Produkte an: Starter und Unbegrenzt. Wählen Sie das Produkt Unbegrenzt.
 3. APIs auswählen.
 4. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.
 5. Wählen Sie die in den vorherigen Schritten erstellte `Service Fabric App`-API aus, und klicken Sie auf die Schaltfläche **Auswählen**.

### <a name="test-it"></a>Testen

Sie können jetzt versuchen, eine Anforderung an den Back-End-Dienst in Service Fabric über API Management direkt vom Azure Portal aus zu senden.

 1. Wählen Sie im API Management-Dienst **API – PREVIEW**.
 2. Wählen Sie in der `Service Fabric App` API, die Sie in den vorherigen Schritten erstellt haben, die Registerkarte **Test** aus.
 3. Klicken Sie auf die Schaltfläche **Senden**, um eine Testanforderung an den Back-End-Dienst zu senden.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt sollten Sie über ein einfaches Setup mit Service Fabric und API Management verfügen.

Um eine schnelle Einrichtung zu ermöglichen, verwendet dieses Tutorial eine einfache zertifikatbasierte Benutzerauthentifizierung für Ihr Service Fabric-Cluster. Eine erweiterte Benutzerauthentifizierung für Ihr Service Fabric-Cluster ist mit [Azure Active Directory-Authentifizierung](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication) vorzuziehen. 

[Erstellen und Konfigurieren Sie anschließend erweiterte Produkteinstellungen im Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules), um Ihre Anwendung für den Datenverkehr der realen Welt vorzubereiten.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png

