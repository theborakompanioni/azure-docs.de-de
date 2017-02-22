---
title: Konfigurieren sicherer Azure Service Fabric-Clusterverbindungen | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit Visual Studio sichere Verbindungen konfigurieren, die vom Azure Service Fabric-Cluster unterstützt werden."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/08/2015
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: f7edee399717ecb96fb920d0a938da551101c9e1
ms.openlocfilehash: b6705e14e52e98759027389758d7fa57c4e11462


---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Konfigurieren sicherer Verbindungen mit einem Service Fabric-Cluster aus Visual Studio
Erfahren Sie, wie Sie Visual Studio verwenden, um auf sichere Weise auf einen Azure Service Fabric-Cluster mit konfigurierten Zugriffsrichtlinien zuzugreifen.

## <a name="cluster-connection-types"></a>Clusterverbindungstypen
Zwei Arten von Verbindungen werden vom Azure Service Fabric-Cluster unterstützt: **nicht sichere** Verbindungen und sichere Verbindungen auf **X.509-Zertifikatbasis**. (Lokal gehostete Service Fabric-Cluster unterstützen auch **Windows**- und **DSTS**-Authentifizierung.) Sie müssen den Verbindungstyp für den Cluster bei dessen Erstellung konfigurieren. Der Verbindungstyp kann nach der Erstellung nicht mehr geändert werden.

Die Service Fabric-Tools von Visual Studio unterstützen alle Authentifizierungstypen zum Herstellen einer Clusterverbindung für die Veröffentlichung. Anweisungen zum Einrichten eines sicheren Service Fabric-Clusters finden Sie unter [Einrichten eines Service Fabric-Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md) .

## <a name="configure-cluster-connections-in-publish-profiles"></a>Konfigurieren von Clusterverbindungen in Veröffentlichungsprofilen
Wenn Sie ein Service Fabric-Projekt aus Visual Studio veröffentlichen, können Sie das Dialogfeld **Service Fabric-Anwendung veröffentlichen** verwenden, um einen Azure Service Fabric-Cluster durch Klicken auf die Schaltfläche **Auswählen** im Abschnitt **Verbindungsendpunkt** auszuwählen. Sie können sich bei Ihrem Azure-Konto anmelden und dann in Ihren Abonnements einen vorhandenen Cluster auswählen.

![Das Dialogfeld **Service Fabric-Anwendung veröffentlichen** wird verwendet, um eine Service Fabric-Verbindung zu konfigurieren.][publishdialog]

Das Dialogfeld **Service Fabric-Cluster auswählen** überprüft automatisch die Clusterverbindung. Wenn die Überprüfung erfolgreich war, bedeutet dies, dass auf dem System die richtigen Zertifikate für eine sichere Verbindung mit dem Cluster installiert sind oder dass Ihr Cluster nicht sicher ist. Überprüfungsfehler können durch Netzwerkprobleme oder durch eine fehlerhafte Konfiguration des Systems in Bezug auf die Verbindung mit einem sicheren Cluster verursacht werden.

![Im Dialogfeld **Service Fabric-Cluster auswählen** können Sie eine vorhandene Service Fabric-Clusterverbindung konfigurieren oder eine neue Clusterverbindung erstellen und konfigurieren.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>So stellen Sie die Verbindung mit einem sicheren Cluster her
1. Stellen Sie sicher, dass Sie auf eines der Clientzertifikate zugreifen können, das für den Zielcluster vertrauenswürdig ist. Das Zertifikat ist in der Regel als PFX-Datei (privater Informationsaustausch) freigegeben. Weitere Informationen zum Konfigurieren des Servers für den Zugriff auf einen Client finden Sie unter [Einrichten eines Service Fabric-Clusters im Azure-Portal](service-fabric-cluster-creation-via-portal.md) .
2. Installieren Sie das vertrauenswürdige Zertifikat. Zu diesem Zweck doppelklicken Sie auf die PFX-Datei oder verwenden das PowerShell-Skript „Import-PfxCertificate“, um Zertifikate zu importieren. Installieren Sie das Zertifikat unter **Cert:\LocalMachine\My**. Die Standardeinstellungen können beim Importieren des Zertifikats bedenkenlos übernommen werden.
3. Wählen Sie den Befehl **Veröffentlichen...** im Kontextmenü des Projekts aus, um das Dialogfeld **Azure-Anwendung veröffentlichen** zu öffnen, und wählen Sie dann den Zielcluster aus. Das Tool löst die Verbindung automatisch auf und speichert die Parameter für die sichere Verbindung im Veröffentlichungsprofil.
4. [Optional]: You can edit the publish profile to specify a secure cluster connection.
   
   Da Sie die Veröffentlichungsprofil-XML-Datei manuell bearbeiten, um Zertifikatsinformationen anzugeben, achten Sie darauf, dass Sie den Namen des Zertifikatspeichers, den Speicherort und den Zertifikatfingerabdruck notieren. Sie müssen diese Werte für den Namen des Zertifikatspeichers und den Speicherort angeben. Weitere Informationen finden Sie unter [Vorgehensweise: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx).
   
   Sie können die *ClusterConnectionParameters* -Parameter verwenden, um die PowerShell-Parameter für die Herstellung der Verbindung mit dem Service Fabric-Cluster anzugeben. Es werden alle Parameter des Cmdlets Connect-ServiceFabricCluster akzeptiert. Eine Liste der verfügbaren Parameter finden Sie unter [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .
   
   Wenn Sie auf einem Remotecluster veröffentlichen, müssen Sie die entsprechenden Parameter für diesen Cluster angeben. Im Folgenden finden Sie ein Beispiel zum Herstellen einer Verbindung mit einem unsicheren Cluster:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Im Folgenden finden Sie ein Beispiel für die Verbindung mit einem sicheren Cluster auf x509-Zertifikatbasis:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Bearbeiten Sie alle anderen erforderlichen Einstellungen, z.B. Upgradeparameter und Speicherort der Anwendungsparameter, und veröffentlichen Sie die Anwendung über das Dialogfeld **Service Fabric-Anwendung veröffentlichen** in Visual Studio.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Zugreifen auf die Service Fabric-Cluster finden Sie unter [Visualisieren des Clusters mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png



<!--HONumber=Jan17_HO4-->


