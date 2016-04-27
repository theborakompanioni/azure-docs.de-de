
<properties
   pageTitle="Service Fabric-Clustersicherheit: Clientauthentifizierung mit Azure Active Directory | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Sie einen Service Fabric-Cluster mit Azure Active Directory (AAD) für die Clientauthentifizierung erstellen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/11/2016"
   ms.author="seanmck"/>

# VORSCHAU: Erstellen eines Service Fabric-Clusters mit Azure Active Directory für die Clientauthentifizierung

Sie können den Zugriff auf die Verwaltungsendpunkte eines Service Fabric-Clusters mit Azure Active Directory (AAD) schützen. In diesem Artikel wird beschrieben, wie Sie die erforderlichen AAD-Artefakte erstellen, diese bei der Clustererstellung auffüllen und anschließend eine Verbindung damit herstellen.

>[AZURE.IMPORTANT] Die AAD-Integration in Service Fabric-Cluster befindet sich derzeit in der Vorschauphase. Alle Features, die in diesem Artikel beschrieben werden, sind in der Service Fabric 5.0-Laufzeit verfügbar, aber wir empfehlen Ihnen, sie noch nicht für Produktionscluster einzusetzen.

## Modellieren eines Service Fabric-Clusters in AAD

AAD ermöglicht Organisationen (als Mandanten bezeichnet) die Verwaltung des Benutzerzugriffs auf Anwendungen, die in Anwendungen mit einer webbasierten Anmeldebenutzeroberfläche und Anwendungen mit einer nativen Clientbenutzeroberfläche unterteilt sind. In diesem Dokument wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Falls nicht, sollten Sie sich zuerst den Artikel [Einrichten eines Azure Active Directory-Mandanten](../active-directory/active-directory-howto-tenant.md) durchlesen.

Für Service Fabric-Cluster sind unterschiedliche Einstiegspunkte für die Verwaltungsfunktionalität vorhanden, z.B. der webbasierte [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) und [Visual Studio](service-fabric-manage-application-in-visual-studio.md). Daher erstellen Sie zwei AAD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.

Wir haben einen Satz mit Windows PowerShell-Skripts erstellt, um einige Schritte der Konfiguration von AAD mit einem Service Fabric-Cluster zu vereinfachen.

>[AZURE.NOTE] Sie müssen diese Schritte ausführen, *bevor* Sie den Cluster erstellen. Falls für die Skripts Clusternamen und Endpunkte verwendet werden, sollte es sich dabei also um geplante Werte handeln, nicht um bereits erstellte Werte.

1. [Laden Sie die Skripts herunter][sf-aad-ps-script-download], und extrahieren Sie sie, bevor Sie fortfahren.

2. Führen Sie `SetupApplications.ps1` aus, und geben Sie TenantId, ClusterName und WebApplicationReplyUrl als Parameter an. Beispiel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Sie können die **TenantId** ermitteln, indem Sie sich die URL für den Mandanten im klassischen Azure-Portal ansehen. Die GUID, die in diese URL eingebettet ist, ist die TenantId. Beispiel:

    https://<i></i>manage.windowsazure.com/microsoft.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/**690ec069-8200-4068-9d01-5aaf188e557a**/users

    Der **ClusterName** wird verwendet, um die vom Skript erstellten AAD-Anwendungen mit einem Präfix zu versehen. Dies muss keine exakte Übereinstimmung mit dem tatsächlichen Clusternamen sein. Er dient lediglich als Hilfe, um Ihnen die Zuordnung von AAD-Artefakten zu dem Service Fabric-Cluster zu erleichtern, für den sie verwendet werden.

    Das **WebApplicationReplyUrl**-Element ist der Standardendpunkt, an den Ihre Benutzer von AAD nach Abschluss des Anmeldevorgangs zurückgegeben werden. Sie sollten diesen Wert auf den Service Fabric Explorer-Endpunkt für Ihren Cluster festlegen, also standardmäßig:

    https://&lt;cluster_domain&gt;:19080/Explorer

    Sie werden zur Anmeldung an einem Konto aufgefordert, das über Administratorrechte für den AAD-Mandanten verfügt. Nach der Anmeldung erstellt das Skript die Webanwendung und native Anwendung, um Ihren Service Fabric-Cluster darzustellen. Wenn Sie sich die Anwendungen des Mandanten im [klassischen Azure-Portal][azure-classic-portal] ansehen, sollten zwei neue Einträge angezeigt werden:

    - *ClusterName*\_Cluster
    - *ClusterName*\_Client

    Das Skript gibt den für die Azure Resource Manager-Vorlage (ARM) erforderlichen JSON-Code aus, wenn Sie den Cluster im nächsten Abschnitt erstellen. Lassen Sie das PowerShell-Fenster also geöffnet.

    ![Die Ausgabe des SetupApplication-Skripts enthält den JSON-Code, der für die ARM-Vorlage erforderlich ist.][setupapp-script-output]

## Cluster erstellen

Nachdem Sie nun die AAD-Anwendungen erstellt haben, können Sie den Service Fabric-Cluster erstellen. Derzeit wird die Konfiguration der AAD-Authentifizierung für Service Fabric-Cluster vom Azure-Portal nicht unterstützt, sodass Sie diesen Schritt mit einer ARM-Vorlage in PowerShell oder Visual Studio ausführen müssen.

Beachten Sie, dass AAD nur für die Clientauthentifizierung im Cluster verwendet wird. Zum Erstellen eines sicheren Clusters müssen Sie auch ein Zertifikat angeben. Es wird zum Schützen der Kommunikation zwischen den Knoten im Cluster und zum Ermöglichen der Serverauthentifizierung für die Verwaltungsendpunkte des Clusters verwendet. Sie finden eine [ARM-Vorlage für einen sicheren Cluster im Azure-Schnellstartkatalog][secure-cluster-arm-template], oder Sie können die Anleitung in der Infodatei unter [Service Fabric-Ressourcengruppenprojekt in Visual Studio](service-fabric-cluster-creation-via-visual-studio.md) befolgen.

Fügen Sie die Ausgabe des ARM-Vorlagenausschnitts aus dem `SetupApplication`-Skript als Peer fabricSettings, managementEndpoint usw. hinzu. Der Ausschnitt ist auch hier angegeben, falls Sie das Fenster bereits geschlossen haben:

```json
  "azureActiveDirectory": {
    "tenantId": "<your_tenant_id>",
    "clusterApplication": "<your_cluster_application_client_id>",
    "clientApplication": "<your_native_application_client_id>"
  }
```

clusterApplication bezieht sich auf die Webanwendung, die im vorherigen Abschnitt erstellt wurde. Sie finden die dazugehörige ID in der Ausgabe des SetupApplication-Skripts, wo sie als `WebAppId` bezeichnet wird. clientApplication bezieht sich auf die native Anwendung, und die dazugehörige Client-ID ist in der SetupApplication-Ausgabe als NativeClientAppId enthalten.

## Zuweisen von Benutzern zu Rollen

Nachdem Sie die Anwendungen zum Darstellen Ihres Clusters erstellt haben, müssen Sie Ihre Benutzer den Rollen zuweisen, die von Service Fabric unterstützt werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Hierfür können Sie das [klassische Azure-Portal][azure-classic-portal] verwenden.

1. Navigieren Sie zu Ihrem Mandanten, und wählen Sie „Anwendungen“.
2. Wählen Sie die Webanwendung aus, die hier über einen Namen der Art `myTestCluster_Cluster` verfügt.
3. Klicken Sie auf die Registerkarte „Benutzer“.
4. Wählen Sie einen zuzuweisenden Benutzer aus, und klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Zuweisen**.

    ![Schaltfläche „Benutzer zu Rollen zuweisen“][assign-users-to-roles-button]

5. Wählen Sie die Rolle aus, die dem Benutzer zugewiesen werden soll.

    ![Zuweisen von Benutzern zu Rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).

## Herstellen der Verbindung mit dem Cluster

Wenn Sie zum Service Fabric Explorer in einem Cluster mit AAD-Aktivierung navigieren, werden Sie automatisch auf eine sichere Anmeldeseite umgeleitet.

Zum Herstellen der Verbindung von einem nativen Client wie Windows PowerShell oder Visual Studio müssen Sie AAD als Anmeldemechanismus festlegen und dann einen Fingerabdruck des Serverzertifikats angeben, der zum Überprüfen der Identität von Endpunkten verwendet wird. Die Details für diese beiden Einstiegspunkte werden unten angezeigt.

### Herstellen der Verbindung über Visual Studio

In Visual Studio können Sie das Veröffentlichungsprofil ändern, um die erforderlichen Attribute wie unten gezeigt hinzuzufügen:

```xml
<ClusterConnectionParameters     
    ConnectionEndpoint="<your_cluster_endpoint>:19000"  
    AzureActiveDirectory="true"
    ServerCertThumbprint="<your_cert_thumbprint>"
    />
```

Wenn Sie die Veröffentlichung im Cluster durchführen, wird von Visual Studio ein Anmeldefenster geöffnet, in dem Sie den Cluster authentifizieren können.

![AAD-Anmeldefenster während der Visual Studio-Veröffentlichung][vs-publish-aad-login]

### Herstellen einer Verbindung mit Windows PowerShell

In PowerShell können Sie die erforderlichen Parameter für das Connect-ServiceFabricCluster-Cmdlet angeben. Dies ist unten dargestellt:

```PowerShell
Connect-ServiceFabricCluster -AzureActiveDirectory -ConnectionEndpoint <cluster_endpoint>:19000 -ServerCertThumbprint <server_cert_thumbprint>
```

Wie in Visual Studio auch, wird von PowerShell ein sicheres Anmeldefenster für die Authentifizierung angezeigt.

>[AZURE.NOTE] Standardmäßig wird von PowerShell das Service Fabric-TCP-Gateway verwendet, und Visual Studio lauscht über Port 19000. Wenn Sie einen anderen Port konfiguriert haben, sollten Sie diesen beim Angeben des Verbindungsendpunkts verwenden.

## Bekannte Probleme

### Authentifizierungsfehler für den nativen Client aufgrund einer nicht übereinstimmenden Antwortadresse

Bei der Authentifizierung über einen nativen Client, z.B. Visual Studio oder PowerShell, wird unter Umständen eine Fehlermeldung der folgenden Art angezeigt:

*Antwortadresse http://localhost/ stimmt nicht mit der Antwortadresse überein, die für die Anwendung &lt;GUID der Clusterclientanwendung&gt; konfiguriert ist.*

Zur Problemumgehung fügen Sie der Definition der Clusterclientanwendung in AAD **http://<i></i>localhost** als Umleitungs-URI hinzu, und zwar zusätzlich zur Adresse „urn:ietf:wg:oauth:2.0:oob“, die bereits vorhanden ist.

## Nächste Schritte

- Erfahren Sie mehr über die [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md).
- Erfahren Sie, wie Sie die [Veröffentlichung in einem Remotecluster mit Visual Studio](service-fabric-publish-app-remote-cluster.md) durchführen.

<!-- Links -->
[sf-aad-ps-script-download]: http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[secure-cluster-arm-template]: https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad
[aad-graph-api-docs]: https://msdn.microsoft.com/de-DE/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com

<!-- Images -->
[assign-users-to-roles-button]: ./media/service-fabric-cluster-security-client-auth-with-aad/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-security-client-auth-with-aad/assign-users-to-roles.png
[setupapp-script-output]: ./media/service-fabric-cluster-security-client-auth-with-aad/setupapp-script-arm-json-output.png
[vs-publish-aad-login]: ./media/service-fabric-cluster-security-client-auth-with-aad/vs-login-prompt.png

<!---HONumber=AcomDC_0413_2016-->