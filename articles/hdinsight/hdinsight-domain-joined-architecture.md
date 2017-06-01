---
title: "In die Domäne eingebundene Azure HDInsight-Architektur| Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie für HDInsight eine Einbindung in die Domäne planen."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: saurinsh
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: d365446b7eafd373b3d1bde2ed0a407f1e917b86
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planen von in die Azure-Domäne eingebundenen Hadoop-Clustern in HDInsight

Bei einem herkömmlichen Hadoop-Cluster handelt es sich um einen Einzelbenutzercluster. Dieser ist für die meisten Unternehmen geeignet, in denen kleinere Anwendungsteams Big Data-Workloads erstellen. Hadoop wird immer beliebter, und viele Unternehmen führen die Umstellung auf ein Modell durch, bei dem Cluster von IT-Teams verwaltet werden und mehrere Anwendungsteams Cluster gemeinsam nutzen. Cluster für mehrere Benutzer zählen daher zu den am meisten geforderten Funktionen in Azure HDInsight.

Anstatt die Authentifizierung und Autorisierung für mehrere Benutzer selbst zu erstellen, wird für HDInsight der beliebteste Identitätsanbieter verwendet: Azure Active Directory (Azure AD). Die leistungsstarke Sicherheitsfunktion in Azure AD kann zum Verwalten der Autorisierung für mehrere Benutzer in HDInsight verwendet werden. Wenn Sie HDInsight in Active Directory integrieren, können Sie über Ihre Active Directory-Anmeldeinformationen mit den Clustern kommunizieren. In HDInsight wird ein Azure AD-Benutzer einem lokalen Hadoop-Benutzer zugeordnet, sodass alle Dienste, die unter HDInsight ausgeführt werden (Ambari, Hive-Server, Ranger, Spark Thrift-Server usw.) für den authentifizierten Benutzer nahtlos funktionieren.

## <a name="integrate-hdinsight-with-azure-ad"></a>Integrieren von HDInsight in Azure AD

Wenn Sie HDInsight in Active Directory integrieren, werden die HDInsight-Clusterknoten in die Active Directory-Domäne eingebunden. In HDInsight werden Dienstprinzipale für die im Cluster ausgeführten Hadoop-Dienste erstellt und in einer angegebenen Organisationseinheit (OE) in Azure AD angeordnet. Außerdem werden in HDInsight umgekehrte DNS-Zuordnungen in der Azure AD-Domäne für die IP-Adresse der Knoten erstellt, die in die Domäne eingebunden werden.

Sie können mehrere Architekturen verwenden, um diese Einrichtung umzusetzen. Sie können zwischen den folgenden Architekturen wählen.

**Integration von HDInsight in Azure AD unter Azure IaaS**

Dies ist die einfachste Architektur für die Integration von HDInsight in Azure AD. Der Azure AD-Domänencontroller wird auf mindestens einem virtuellen Computer (VM) in Azure ausgeführt. Diese virtuellen Computer befinden sich in der Regel in einem virtuellen Netzwerk. Sie richten ein anderes virtuelles Netzwerk für den HDInsight-Cluster ein. Zum Herstellen einer „Sichtverbindung“ von HDInsight mit Azure AD müssen Sie diese virtuellen Netzwerke per [VNet-zu-VNet-Peering](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md) verknüpfen.

![Topologie mit in die Domäne eingebundenem HDInsight-Cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> In dieser Architektur können Sie Azure Data Lake Store nicht mit dem HDInsight-Cluster nutzen.


Voraussetzungen für Azure AD:

* Eine [Organisationseinheit](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) muss erstellt werden, in der die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platziert werden.
* Für die Kommunikation mit der Active Directory-Instanz muss [Lightweight Directory Access Protocol](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPS) eingerichtet werden. Das Zertifikat zum Einrichten von LDAPS muss ein reales Zertifikat (kein selbst signiertes Zertifikat) sein.
* In der Domäne müssen für den IP-Adressbereich des HDInsight-Subnetzes (z.B. 10.2.0.0/24 in der obigen Abbildung) Reverse-DNS-Zonen erstellt werden.
* Ein Dienstkonto oder ein Benutzerkonto wird benötigt. Mit diesem Konto erstellen Sie den HDInsight-Cluster. Dieses Konto muss die folgenden Berechtigungen besitzen:

    - Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
    - Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
    - Berechtigungen zum Einbinden von Computern in die Active Directory-Domäne

**Integration von HDInsight in eine auf die Cloud beschränkte Azure AD-Instanz**

Bei einer auf die Cloud beschränkten Azure AD-Instanz müssen Sie einen Domänencontroller konfigurieren, damit HDInsight in Azure AD integriert werden kann. Dazu verwenden Sie [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Mit AD DS werden Domänencontrollercomputer in der Cloud erstellt und entsprechende IP-Adressen bereitgestellt. Es werden zwei Domänencontroller erstellt, um für eine hohe Verfügbarkeit zu sorgen.

Derzeit ist Azure AD DS nur in klassischen virtuellen Netzwerken vorhanden. Der Zugriff darauf ist nur über das klassische Azure-Portal möglich. Das virtuelle HDInsight-Netzwerk ist im Azure-Portal vorhanden, und das Peering mit dem klassischen virtuellen Netzwerk muss per VNet-zu-VNet-Peering hergestellt werden.

> [!NOTE]
> Für das Peering zwischen einem klassischen virtuellen Netzwerk und einem virtuellen Azure Resource Manager-Netzwerk müssen sich beide virtuellen Netzwerke in derselben Region und unter demselben Azure-Abonnement befinden.

![Topologie mit in die Domäne eingebundenem HDInsight-Cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Voraussetzungen für Azure AD:

* Eine [Organisationseinheit](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) muss erstellt werden, in der die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platziert werden.
* Für die Konfiguration von Azure AD DS müssen Sie [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) einrichten. Das Zertifikat zum Einrichten von LDAPS muss ein reales Zertifikat (kein selbst signiertes Zertifikat) sein.
* In der Domäne müssen für den IP-Adressbereich des HDInsight-Subnetzes (z.B. 10.2.0.0/24 in der obigen Abbildung) Reverse-DNS-Zonen erstellt werden.
* [Kennworthashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) müssen zwischen Azure AD und Azure AD DS synchronisiert werden.
* Ein Dienstkonto oder ein Benutzerkonto wird benötigt. Mit diesem Konto erstellen Sie den HDInsight-Cluster. Dieses Konto muss die folgenden Berechtigungen besitzen:

    - Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
    - Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
    - Berechtigungen zum Einbinden von Computern in die Azure AD-Domäne

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](hdinsight-domain-joined-configure.md).
* Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern finden Sie unter [Verwalten von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](hdinsight-domain-joined-manage.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung (Vorschau)](hdinsight-domain-joined-run-hive.md).
* Informationen zum Ausführen von Hive-Abfragen mit SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

