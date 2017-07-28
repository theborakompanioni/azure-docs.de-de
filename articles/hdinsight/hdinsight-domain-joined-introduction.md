---
title: "Hadoop-Sicherheit – In die Domäne eingebundene HDInsight-Cluster – Azure | Microsoft-Dokumentation"
description: 'Inhalt:'
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
ms.date: 10/31/2016
ms.author: saurinsh
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 9f6de5b516da110b93a1e0cddf835b2bcec77bb7
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>Einführung in die Hadoop-Sicherheit mit in die Domäne eingebundenen HDInsight-Clustern (Vorschau)

Von Azure HDInsight wurde bislang nur ein einzelner lokaler Administratorbenutzer unterstützt. Für kleinere Anwendungsteams oder Abteilungen hat das hervorragend funktioniert. Mit der zunehmenden Verbreitung von Hadoop-basierten Workloads im Unternehmenssektor gewinnen jedoch professionelle Funktionen wie Active Directory-basierte Authentifizierung, Unterstützung mehrerer Benutzer und rollenbasierte Zugriffssteuerung immer mehr an Bedeutung. Mithilfe von in die Domäne eingebundenen HDInsight-Clustern können Sie einen HDInsight-Cluster erstellen, der einer Active Directory-Domäne angehört, und anschließend eine Liste mit Mitarbeitern des Unternehmens konfigurieren, die sich für die Anmeldung bei dem HDInsight-Cluster über Azure Active Directory authentifizieren können. Externe Benutzer können sich nicht bei dem HDInsight-Cluster anmelden oder darauf zugreifen. Der Unternehmensadministrator kann mithilfe von [Apache Ranger](http://hortonworks.com/apache/ranger/) die rollenbasierte Zugriffskontrolle für Hive-Sicherheit konfigurieren und so den Datenzugriff optimal beschränken. Darüber hinaus kann der Administrator den mitarbeiterspezifischen Datenzugriff sowie jegliche Änderung der Zugriffssteuerungsrichtlinien überwachen und so ein hohes Maß an Kontrolle über Unternehmensressourcen erreichen.

> [!NOTE]
> Die neuen Features, die in dieser Vorschau beschrieben werden, stehen nur in Linux-basierten HDInsight-Clustern für die Hive-Workload zur Verfügung. Andere Workloads (etwa HBase, Spark, Storm und Kafka) kommen in späteren Versionen hinzu.

> [!IMPORTANT]
> Oozie ist auf HDInsight mit Domänenverknüpfung nicht aktiviert.

## <a name="benefits"></a>Vorteile
Die Unternehmenssicherheit basiert auf vier zentralen Säulen: Umgebungssicherheit, Authentifizierung, Autorisierung und Verschlüsselung.

![Vorteile von in die Domäne eingebundenen HDInsight-Clustern (Säulen)](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png)zu erstellen und zu verwalten.

### <a name="perimeter-security"></a>Umgebungssicherheit
Die Umgebungssicherheit in HDInsight wird mithilfe von virtuellen Netzwerken und einem Gatewaydienst erreicht. Unternehmensadministratoren können einen HDInsight-Cluster in einem virtuellen Netzwerk erstellen und den Zugriff auf das virtuelle Netzwerk mithilfe von Netzwerksicherheitsgruppen (eingehende oder ausgehende Firewallregeln) beschränken. Zum Schutz der Umgebung können nur IP-Adressen, die in den eingehenden Firewallregeln definiert sind, mit dem HDInsight-Cluster kommunizieren. Durch die Verwendung eines Gatewaydiensts lässt sich die Umgebungssicherheit weiter verbessern. Der Gatewaydienst fungiert als erste Verteidigungslinie für eingehende, an den HDInsight-Cluster gerichtete Anforderungen. Er nimmt die Anforderung entgegen, prüft sie und leitet sie erst dann an die anderen Knoten im Cluster weiter. Dadurch wird die Umgebungssicherheit für andere Namen- und Datenknoten im Cluster gewährleistet.

### <a name="authentication"></a>Authentifizierung
Mit dieser öffentlichen Vorschau kann ein Unternehmensadministrator einen in die Domäne eingebundenen HDInsight-Cluster in einem [virtuellen Netzwerk](https://azure.microsoft.com/services/virtual-network/) bereitstellen. Die Knoten des HDInsight-Clusters werden der vom Unternehmen verwalteten Domäne hinzugefügt. Hierzu wird [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) verwendet. Alle Knoten im Cluster werden einer vom Unternehmen verwalteten Domäne hinzugefügt. In dieser Konfiguration können sich die Mitarbeiter des Unternehmens mit ihren Domänenanmeldeinformationen bei den Clusterknoten anmelden. Darüber hinaus können sie sich mit ihren Domänenanmeldeinformationen auch bei anderen genehmigten Endpunkten wie Hue, Ambari Views, ODBC, JDBC, PowerShell und REST-APIs authentifizieren, um mit dem Cluster zu interagieren. Der Administrator hat uneingeschränkte Kontrolle über die Begrenzung der Anzahl von Benutzern, die über diese Endpunkte mit dem Cluster interagieren.

### <a name="authorization"></a>Autorisierung
In den meisten Unternehmen hat nicht jeder Mitarbeiter Zugriff auf alle Unternehmensressourcen. Analog dazu kann der Administrator in dieser Version für die Clusterressourcen Richtlinien für die rollenbasierte Zugriffssteuerung definieren. So kann der Administrator etwa [Apache Ranger](http://hortonworks.com/apache/ranger/) zum Festlegen von Zugriffssteuerungsrichtlinien für Hive konfigurieren. Dadurch wird sichergestellt, dass Mitarbeiter nur auf Daten zugreifen können, die sie für ihre Arbeit benötigen. SSH-Zugriff auf den Cluster ist allein dem Administrator vorbehalten.

### <a name="auditing"></a>Überwachung
Neben dem Schutz der HDInsight-Clusterressourcen vor nicht autorisierten Benutzern und dem Schutz der Daten müssen auch sämtliche Zugriffe auf die Clusterressourcen und -daten überwacht werden, um nicht autorisierte oder versehentliche Ressourcenzugriffe zu erfassen. Mit dieser Vorschau kann der Administrator sämtliche Zugriffe auf die Ressourcen und Daten des HDInsight-Clusters anzeigen und entsprechende Berichte erstellen. Außerdem kann er sämtliche Änderungen an den Zugriffssteuerungsrichtlinien anzeigen, die an von Apache Ranger unterstützten Endpunkten durchgeführt werden, und entsprechende Berichte erstellen. Ein in die Domäne eingebundener HDInsight-Cluster verwendet für die Suche nach Überwachungsprotokollen die vertraute Apache Ranger-Benutzeroberfläche. Im Back-End werden die Protokolle von Ranger mithilfe von [Apache Solr](http://hortonworks.com/apache/solr/) gespeichert und gesucht.

### <a name="encryption"></a>Verschlüsselung
Datenschutz spielt eine wichtige Rolle bei der Erfüllung der Sicherheits- und Complianceanforderungen von Unternehmen. Neben der Beschränkung des Zugriffs für nicht autorisierte Mitarbeiter sollten die Daten daher auch durch eine Verschlüsselung geschützt werden. Sowohl die Datenspeicher für HDInsight-Cluster als auch Azure Storage Blob und Azure Data Lake Store unterstützen die transparente serverseitige [Verschlüsselung von Daten](../storage/storage-service-encryption.md) im Ruhezustand. Sichere HDInsight-Cluster arbeiten reibungslos mit dieser Funktion zur serverseitigen Verschlüsselung ruhender Daten zusammen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).
* Informationen zum Verwalten eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Manage Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-manage.md) (Verwalten von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Configure Hive policies in Domain-joined HDInsight (Preview)](hdinsight-domain-joined-run-hive.md) (Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster).
* Informationen zum Ausführen von Hive-Abfragen per SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

