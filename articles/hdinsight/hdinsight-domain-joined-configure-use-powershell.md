---
title: "Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit PowerShell – Azure| Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Einrichten und Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure PowerShell."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d31ad53525ef75bdb61c42409dc07bba4138fc25
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau) mit Azure PowerShell
Es wird beschrieben, wie Sie einen Azure HDInsight-Cluster mit Azure Active Directory (Azure AD) und [Apache Ranger](http://hortonworks.com/apache/ranger/) per Azure PowerShell einrichten. Ein Azure PowerShell-Skript wird bereitgestellt, um die Konfiguration schneller und weniger fehleranfällig zu machen. In die Domäne eingebundenes HDInsight kann nur unter Linux-basierten Clustern konfiguriert werden. Weitere Informationen finden Sie unter [Introduce Domain-joined HDInsight clusters (Einführung in HDInsight-Cluster mit Domänenverknüpfung)](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie ist auf HDInsight mit Domänenverknüpfung nicht aktiviert.

Die Konfiguration eines typischen in die Domäne eingebundenen HDInsight-Clusters umfasst die folgenden Schritte:

1. Erstellen Sie ein klassisches Azure-VNet für Azure AD.  
2. Erstellen und konfigurieren Sie Azure AD und Azure AD DS.
3. Fügen Sie einen virtuellen Computer zum klassischen VNet hinzu, um eine Organisationseinheit zu erstellen. 
4. Erstellen Sie eine Organisationseinheit für Azure AD DS.
5. Erstellen Sie ein HDInsight-VNet im Ressourcenverwaltungsmodus von Azure.
6. Richten Sie Reverse-DNS-Zonen für Azure AD DS ein.
7. Führen Sie das Peering der beiden VNets durch.
8. Erstellen Sie ein HDInsight-Cluster.

Mit dem bereitgestellten PowerShell-Skript werden die Schritte 3 bis 7 ausgeführt. Sie müssen die Schritte 1 und 2 manuell ausführen.  Falls Sie Azure PowerShell nicht verwenden möchten, helfen Ihnen die Informationen unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)) weiter. 

Ein Beispiel für die fertige Topologie sehen Sie hier:

![In die Domäne eingebundene HDInsight-Topologie](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Azure AD unterstützt zur Zeit nur klassische virtuelle Netzwerke (VNets), und Linux-basierte HDInsight-Cluster nur Azure Resource Manager-basierte VNets. Daher erfordert die Integration von HDInsight und Azure AD zwei VNets und Peering zwischen ihnen. Informationen über den Vergleich der beiden Bereitstellungsmodelle finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md). Die beiden VNets müssen sich in der gleichen Region wie die Azure AD DS befinden.

> [!NOTE]
> In diesem Lernprogramm wird davon ausgegangen, dass Sie noch nicht über Azure AD verfügen. Wenn Sie bereits darüber verfügen, können Sie diesen Teil in Schritt 2 überspringen.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Machen Sie sich mit [Azure AD-Domänendiensten](https://azure.microsoft.com/services/active-directory-ds/) und deren [Preis](https://azure.microsoft.com/pricing/details/active-directory-ds/)-Struktur vertraut.
* Stellen Sie sicher, dass sich Ihre Abonnement auf der Zulassungsliste befindet. Dazu können Sie eine E-Mail mit Ihrer Abonnement-ID an hdipreview@microsoft.com senden.
* Ein SSL-Zertifikat, das von einer Signaturstelle für Ihre Domäne signiert ist. Es wird ein Zertifikat für sicheres LDAP benötigt. Selbstsignierte Zertifikate können nicht verwendet werden.
* Azure PowerShell.  Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Erstellen Sie ein klassisches Azure-VNet für Azure AD.
Eine Anleitung finden Sie [hier](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Erstellen und konfigurieren Sie Azure AD und Azure AD DS.
Eine Anleitung finden Sie [hier](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-the-powershell-script"></a>Ausführen des PowerShell-Skripts
Sie können das PowerShell-Skript von [GitHub](https://github.com/hdinsight/DomainJoinedHDInsight) herunterladen. Extrahieren Sie die ZIP-Datei, und speichern Sie die Dateien lokal.

**So bearbeiten Sie das PowerShell-Skript**

1. Öffnen Sie „run.ps1“ mit der Windows PowerShell ISE oder einem beliebigen Text-Editor.
2. Geben Sie die Werte für die folgenden Variablen ein:
   
   * **$SubscriptionName**: Der Name des Azure-Abonnements, unter dem Sie den HDInsight-Cluster erstellen möchten. Sie haben unter diesem Abonnement bereits ein klassisches virtuelles Netzwerk erstellt und erstellen zusätzlich ein virtuelles Azure Resource Manager-Netzwerk für den HDInsight-Cluster im Abonnement.
   * **$ClassicVNetName**: Das klassische virtuelle Netzwerk mit Azure AD DS. Dieses virtuelle Netzwerk muss sich in demselben Abonnement befinden, das oben angegeben ist. Dieses virtuelle Netzwerk muss mit dem Azure-Portal erstellt werden, nicht mit dem klassischen Portal. Wenn Sie die Anleitung unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic) verwenden, lautet der Standardname „contosoaadvnet“.
   * **$ClassicResourceGroupName**: Der Resource Manager-Gruppenname für das klassische virtuelle Netzwerk, der oben erwähnt wird. Beispiel: contosoaadrg. 
   * **$ArmResourceGroupName**: Der Name der Ressourcengruppe, in der Sie den HDInsight-Cluster erstellen möchten. Sie können dieselbe Ressourcengruppe „$ArmResourceGroupName“ verwenden.  Falls die Ressourcengruppe nicht vorhanden ist, erstellt das Skript die Ressourcengruppe für Sie.
   * **$ArmVNetName**: Der Name des virtuellen Resource Manager-Netzwerks, in dem Sie den HDInsight-Cluster erstellen möchten. Dieses virtuelle Netzwerk wird in „$ArmResourceGroupName“ angeordnet.  Falls das VNet nicht vorhanden ist, wird es mit dem PowerShell-Skript erstellt. Wenn es vorhanden ist, sollte es Teil der oben angegebenen Ressourcengruppe sein.
   * **$AddressVnetAddressSpace**: Der Netzwerkadressraum für das virtuelle Resource Manager-Netzwerk. Stellen Sie sicher, dass dieser Adressraum verfügbar ist. Dieser Adressraum darf sich nicht mit dem Adressraum des klassischen virtuellen Netzwerks überschneiden. Beispiel: „10.1.0.0/16“.
   * **$ArmVnetSubnetName**: Der Name für das Subnetz des virtuellen Resource Manager-Netzwerks, in dem Sie die HDInsight-Cluster-VMs anordnen möchten. Falls das Subnetz nicht vorhanden ist, wird es mit dem PowerShell-Skript erstellt. Wenn es vorhanden ist, sollte es Teil des oben angegebenen virtuellen Netzwerks sein.
   * **$AddressSubnetAddressSpace**: Der Netzwerkadressbereich für das Subnetz des virtuellen Resource Manager-Netzwerks. Die IP-Adressen der HDInsight-Cluster-VMs stammen aus diesem Subnetz-Adressbereich. Beispiel: „10.1.0.0/24“.
   * **$ActiveDirectoryDomainName**: Der Name der Azure AD-Domäne, in die Sie die HDInsight-Cluster-VMs einbinden möchten. Beispiel: „contoso.onmicrosoft.com“.
   * **$ClusterUsersGroups**: Der allgemeine Name der Sicherheitsgruppen aus Ihrer AD-Instanz, die Sie mit dem HDInsight-Cluster synchronisieren möchten. Die Benutzer in dieser Sicherheitsgruppe können sich mit ihren Anmeldeinformationen für die Active Directory-Domäne am Cluster-Dashboard anmelden. Diese Sicherheitsgruppen müssen in der Active Directory-Instanz vorhanden sein. Beispiele: „hiveusers“ oder „clusteroperatorusers“.
   * **$OrganizationalUnitName**: Die Organisationseinheit in der Domäne, in der Sie die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale anordnen möchten. Diese Organisationseinheit wird vom PowerShell-Skript erstellt, falls sie nicht vorhanden ist. Beispiel: „HDInsightOU“.
3. Speichern Sie die Änderungen.

**So führen Sie das Skript aus**

1. Führen Sie **Windows PowerShell** als Administrator aus.
2. Navigieren Sie zum Ordner von „run.ps1“. 
3. Führen Sie das Skript aus, indem Sie den Dateinamen eingeben und die **EINGABETASTE** drücken.  Drei Dialogfelder für die Anmeldung werden angezeigt:
   
   1. **Anmeldung am klassischen Azure-Portal**: Geben Sie Ihre Anmeldeinformationen ein, die Sie zum Anmelden am klassischen Azure-Portal verwenden. Sie müssen die Erstellung von Azure AD und Azure AD DS mit diesen Anmeldeinformationen durchgeführt haben.
   2. **Anmeldung am Azure Resource Manager-Portal**: Geben Sie Ihre Anmeldeinformationen ein, die Sie zum Anmelden am Azure Resource Manager-Portal verwenden.
   3. **Domänenbenutzername**: Geben Sie die Anmeldeinformationen des Domänenbenutzernamens ein, der als Administrator für den HDInsight-Cluster fungieren soll. Wenn Sie eine Azure AD-Instanz von Grund auf neu erstellt haben, müssen Sie für die Erstellung dieses Benutzers diese Dokumentation verwendet haben. 
      
      > [!IMPORTANT]
      > Geben Sie den Benutzernamen in diesem Format ein: 
      > 
      > domänenname\benutzername (z.B. „contoso.onmicrosoft.com\clusteradmin“)
      > 
      > 
      
      Dieser Benutzer muss über drei Berechtigungen für folgende Vorgänge verfügen: Einbinden von Computern in die bereitgestellte Active Directory-Domäne, Erstellen von Dienstprinzipalen und Computerobjekten in der bereitgestellten Organisationseinheit und Hinzufügen von DNS-Reverseproxyregeln.

Beim Erstellen von Reverse-DNS-Zonen werden Sie vom Skript aufgefordert, eine Netzwerk-ID einzugeben. Diese Netzwerk-ID muss das Adresspräfix des virtuellen Resource Manager-Netzwerks sein. Wenn der Adressraum für das Subnetz des virtuellen Resource Manager-Netzwerks beispielsweise „10.2.0.0/24“ lautet, geben Sie „10.2.0.0/24“ ein, wenn Sie vom Tool zur Eingabe der Netzwerk-ID aufgefordert werden. 

## <a name="create-hdinsight-cluster"></a>Erstellen des HDInsight-Clusters
In diesem Abschnitt erstellen Sie einen Linux-basierten Hadoop-Cluster in HDInsight, entweder mithilfe des Azure-Portals oder einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md). Andere Methoden zur Erstellung von Clustern und Informationen zu den Einstellungen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zur Verwendung einer Resource Manager-Vorlage zum Erstellen von Hadoop-Clustern in HDInsight finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight mithilfe von Resource Manager-Vorlagen](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe des Azure-Portals**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu**, **Intelligence + Analyse**, und klicken Sie dann auf **HDInsight**.
3. Geben Sie auf dem Blatt **Neuer HDInsight-Cluster** die folgenden Werte ein, oder wählen Sie diese aus:
   
   * **Clustername**: Geben Sie einen neuen Namen für den in die Domäne eingebundenen HDInsight-Cluster ein.
   * **Abonnement** : Wählen Sie ein Azure-Abonnement aus, das für die Erstellung dieses Clusters verwendet wird.
   * **Clusterkonfiguration**:
     
     * **Clustertyp**: Hadoop. HDInsight mit Domänenverknüpfung wird derzeit nur auf Hadoop-Clustern unterstützt.
     * **Betriebssystem**: Linux.  HDInsight mit Domänenverknüpfung wird derzeit nur auf Linux-basierten HDInsight-Clustern unterstützt.
     * **Version**: Hadoop 2.7.3 (HDI 3.5). HDInsight mit Domänenverknüpfung wird derzeit nur auf HDInsight-Clustern, Version 3.5 unterstützt.
     * **Clustertyp**: PREMIUM
       
       Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Anmeldeinformationen**: Konfigurieren Sie die Anmeldeinformationen für den Benutzer des Clusters und den Benutzer von SSH.
   * **Datenquelle**: Erstellen Sie ein neues Speicherkonto oder verwenden Sie ein vorhandenes Speicherkonto, das als Standardspeicherkonto für den HDInsight-Cluster verwendet werden kann. Der Standort muss dem Standort der beiden VNets entsprechen.  Dieser Standort muss auch dem Standort des HDInsight-Clusters entsprechen.
   * **Preise**: Wählen Sie die Anzahl der Workerknoten Ihres Clusters aus.
   * **Erweiterte Konfiguration**: 
     
     * **Domain-joining & Vnet/Subnet (Domänenbeitritt & VNET-Subnetz)**: 
       
       * **Domäneneinstellungen**: 
         
         * **Domänenname**: contoso.onmicrosoft.com
         * **Domänenbenutzername**: Geben Sie einen Domänenbenutzernamen ein. Diese Domäne muss über die folgenden Berechtigungen verfügen: Hinzufügen von Computern zur Domäne, und Platzieren dieser Computer in der konfigurierten Organisationseinheit; Erstellen von Dienstprinzipalen innerhalb der konfigurierten Organisationseinheit; Erstellen von Reverse-DNS-Einträgen. Dieser Benutzer der Domäne wird der Administrator dieses in die Domäne eingebundenen HDInsight-Clusters.
         * **Domänenkennwort**: Geben Sie das Benutzerkennwort für die Domäne ein.
         * **Organisationseinheit**: Geben Sie den Distinguished Name der zuvor konfigurierten OE ein. Beispiel: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Access user group (Zugriff auf die Benutzergruppe)**: Geben Sie die Sicherheitsgruppe an, deren Benutzer Sie mit dem Cluster synchronisieren möchten. Beispiel: HiveUsers.
           
           Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
           
           ![Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuelles Netzwerk**: contosohdivnet
       * **Subnetz**: Subnet1
         
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.        
         Klicken Sie zum Speichern der Änderungen auf **Auswählen**.
   * **Ressourcengruppe**: Wählen Sie die Ressourcengruppe aus, die für das HDInsight VNet verwendet wird (contosohdirg).
4. Klicken Sie auf **Erstellen**.  

Eine weitere Möglichkeit in die Domäne eingebundene HDInsight-Cluster zu erstellen ist es, eine Azure Resource Management-Vorlage zu verwenden. Im Folgenden sehen Sie, wie das funktioniert:

**Erstellen eines in die Domäne eingebundenen HDInsight-Clusters mithilfe einer Resource Management-Vorlage**

1. Klicken Sie auf das folgende Bild, um eine Resource Manager-Vorlage im Azure-Portal zu öffnen. Die Resource Manager-Vorlage befindet sich in einem öffentlichen Blobcontainer. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Geben Sie auf dem Blatt **Parameter** folgende Werte ein:
   
   * **Abonnement**: (Wählen Sie Ihr Azure-Abonnement aus).
   * **Ressourcengruppe**: Klicken Sie auf **Vorhandene Verwenden**, und geben Sie die gleiche Ressourcengruppe an wie die, die Sie verwendet haben.  Beispiel: contosohdirg. 
   * **Standort**: Geben Sie den Standort einer Ressourcengruppe an.
   * **Clustername**: Geben Sie einen Namen für den Hadoop-Cluster ein, den Sie erstellen möchten. Beispiel: contosohdicluster.
   * **Clustertyp**: Wählen Sie einen Clustertyp aus.  Der Standardwert lautet **hadoop**.
   * **Standort**: Wählen Sie einen Standort für den Cluster aus.  Das standardmäßige Speicherkonto verwendet den gleichen Speicherort.
   * **Cluster Worker Node count (Anzahl von Workerknoten im Cluster)**: Wählen Sie die Anzahl der Workerknoten aus.
   * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **admin**.
   * **SSH-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **sshuser**.  Sie können auch einen anderen Namen festlegen. 
   * **Virtual Network-ID**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>
   * **Virtuelles Netzwerk/Subnetz**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>/subnets/Subnet1
   * **Domänenname**: contoso.onmicrosoft.com
   * **Organization Unit DN (Organisationseinheit DN)**: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
   * **Cluster Users Group D Ns (Nutzergruppe Cluster D Ns)**: "\"CN=HiveUsers,OU=AADDC Users,DC=<DomainName>,DC=onmicrosoft,DC=com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (Geben Sie den Benutzernamen des Administrators der Domäne ein)
   * **DomainAdminPassword**: (Geben Sie das Kennwort des Administrators der Domäne ein)
   * **Ich stimme den oben genannten Geschäftsbedingungen zu**: (aktivieren)
   * **An Dashboard anheften**: (aktivieren)
3. Klicken Sie auf **Kaufen**. Es wird eine neue Kachel mit dem Titel **Deploying Template deployment (Vorlagenbereitstellung bereitstellen)** angezeigt. Das Erstellen eines Clusters dauert ca. 20 Minuten. Wenn der Cluster erstellt wurde, öffnen Sie ihn, indem Sie im Portal auf das Clusterblatt klicken.

Löschen Sie den Cluster, wenn Sie das Tutorial beendet haben. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](hdinsight-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Verwenden von SSH für das Herstellen von Verbindungen mit in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


