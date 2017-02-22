---
title: "Verwalten von in die Domäne eingebundenen HDInsight-Clustern | Microsoft-Dokumentation"
description: "Informationen zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 78382cf83ed5e94507854d41293a95fd529964c1


---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Verwalten von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)
Hier erhalten Sie Informationen zu den Benutzern und den Rollen in HDInsight mit Domänenverknüpfung sowie zum Verwalten von in die Domäne eingebundenen HDInsight-Clustern.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Benutzer von in die Domäne eingebundenen HDInsight-Clustern
Ein HDInsight-Cluster, der nicht in die Domäne eingebunden ist, verfügt über zwei Benutzerkonten, die während der Clustererstellung erstellt werden:

* **Ambari-Administrator:** Dieses Konto wird auch als *Hadoop-Benutzer* oder *HTTP-Benutzer* bezeichnet. Dieses Konto kann für die Anmeldung bei Ambari unter https://&lt;Clustername>.azurehdinsight.net verwendet werden. Es kann auch zum Ausführen von Abfragen für Ambari-Ansichten, zum Ausführen von Aufträgen über externe Tools (z.B. PowerShell, Templeton, Visual Studio) und für die Authentifizierung mit dem Hive ODBC-Treiber und BI-Tools (z.B. Excel, Power BI oder Tableau) verwendet werden.
* **SSH-Benutzer:** Dieses Konto kann mit SSH und zum Ausführen von sudo-Befehlen verwendet werden. Es verfügt über Rootberechtigungen für die virtuellen Linux-Computer.

Ein in die Domäne eingebundener HDInsight-Cluster verfügt neben Ambari-Administrator und SSH-Benutzer über drei neue Benutzer.

* **Ranger-Administrator:** Dieses Konto ist das lokale Apache Ranger-Administratorkonto. Es handelt sich um keinen Active Directory-Domänenbenutzer. Dieses Konto kann verwendet werden, um Richtlinien einzurichten und andere Benutzer als Administratoren oder delegierte Administratoren festzulegen (sodass diese Benutzer Richtlinien verwalten können). Der Benutzername lautet standardmäßig *admin*, und das Kennwort ist identisch mit dem Ambari-Administratorkennwort. Das Kennwort kann auf der Seite „Settings“ (Einstellungen) in Ranger aktualisiert werden.
* **Domänenbenutzer des Clusteradministrators:** Dieses Konto ist ein Active Directory-Domänenbenutzer, der als Hadoop-Clusteradministrator (einschließlich Ambari und Ranger) festgelegt ist. Sie müssen die Anmeldeinformationen dieses Benutzers während der Clustererstellung angeben. Dieser Benutzer verfügt über die folgenden Berechtigungen:

  * Einbinden von Computern in die Domäne und Platzieren dieser Computer in der Organisationseinheit, die Sie während der Clustererstellung angeben.
  * Erstellen von Dienstprinzipalen in der Organisationseinheit, die Sie während der Clustererstellung angeben.
  * Erstellen von Reverse-DNS-Einträgen.

    Beachten Sie, dass die anderen AD-Benutzer auch über diese Berechtigungen verfügen.

    Es gibt einige Endpunkte innerhalb des Clusters (z.B. Templeton), die nicht von Ranger verwaltet werden und daher nicht sicher sind. Diese Endpunkte sind für alle Benutzer außer dem Domänenbenutzer des Clusteradministrators gesperrt.
* **Normal:** Während der Clustererstellung können Sie mehrere Active Directory-Gruppen angeben. Die Benutzer in diesen Gruppen werden mit Ranger und Ambari synchronisiert. Diese Benutzer sind Domänenbenutzer und haben nur Zugriff auf über Ranger verwaltete Endpunkte (z.B. Hiveserver2). Alle RBAC-Richtlinien und die Überwachung gelten für diese Benutzer.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Rollen von in die Domäne eingebundenen HDInsight-Clustern
In die Domäne eingebundene HDInsight-Cluster weisen die folgenden Rollen auf:

* Clusteradministrator
* Clusteroperator
* Dienstadministrator
* Dienstoperator
* Clusterbenutzer

**So zeigen Sie die Berechtigungen dieser Rollen an**

1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Roles** (Rollen).
3. Klicken Sie auf das blaue Fragezeichen, um die Berechtigungen anzuzeigen:

    ![Berechtigungen für Rollen von in die Domäne eingebundenen HDInsight-Clustern](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Öffnen der Ambari-Verwaltungsoberfläche
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie Ihren HDInsight-Cluster auf einem Blatt. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klicken Sie im Hauptmenü auf **Dashboard**, um Ambari zu öffnen.
4. Melden Sie sich mit dem Domänenbenutzernamen und dem Kennwort des Clusteradministrators bei Ambari an.
5. Klicken Sie oben rechts auf das Dropdownmenü **Admin** und dann auf **Manage Ambari** (Ambari verwalten).

    ![HDInsight mit Domänenverknüpfung – Verwalten von Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Die Benutzeroberfläche sieht wie folgt aus:

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Auflisten der über Active Directory synchronisierten Domänenbenutzer
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Benutzer**. Es werden alle Benutzer angezeigt, die über Active Directory mit dem HDInsight-Cluster synchronisiert wurden.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Auflisten der Benutzer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Auflisten der über Active Directory synchronisierten Domänengruppen
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Groups** (Gruppen). Es werden alle Gruppen angezeigt, die über Active Directory mit dem HDInsight-Cluster synchronisiert wurden.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Auflisten der Gruppen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurieren von Berechtigungen für Hive-Ansichten
1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Views** (Ansichten).
3. Klicken Sie auf **HIVE**, um die Details anzuzeigen.

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Hive-Ansichten](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klicken Sie auf den Link **Hive View** (Hive-Ansicht), um Hive-Ansichten zu konfigurieren.
5. Scrollen Sie nach unten zum Abschnitt **Permissions** (Berechtigungen).

    ![HDInsight mit Domänenverknüpfung – Ambari-Verwaltungsoberfläche – Hive-Ansichten – Konfigurieren von Berechtigungen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klicken Sie auf **Add User** (Benutzer hinzufügen) oder **Add Group** (Gruppe hinzufügen), und geben Sie dann die Benutzer oder Gruppen an, die Hive-Ansichten verwenden können.

## <a name="configure-users-for-the-roles"></a>Konfigurieren von Benutzern für die Rollen
 Eine Aufstellung der Rollen und der zugehörigen Berechtigungen finden Sie unter [Rollen von in die Domäne eingebundenen HDInsight-Clustern](#roles-of-domain---joined-hdinsight-clusters).

1. Öffnen Sie die Ambari-Verwaltungsoberfläche.  Informationen hierzu finden Sie unter [Öffnen der Ambari-Verwaltungsoberfläche](#open-the-ambari-management-ui).
2. Klicken Sie im linken Menü auf **Roles** (Rollen).
3. Klicken Sie auf **Add User** (Benutzer hinzufügen) oder **Add Group** (Gruppe hinzufügen), um Benutzer und Gruppen zu verschiedenen Rollen zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)](hdinsight-domain-joined-configure.md).
* Informationen zum Konfigurieren von Hive-Richtlinien und zum Ausführen von Hive-Abfragen finden Sie unter [Konfigurieren von Hive-Richtlinien für in die Domäne eingebundene HDInsight-Cluster (Vorschau)](hdinsight-domain-joined-run-hive.md).
* Informationen zum Ausführen von Hive-Abfragen mit SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).



<!--HONumber=Dec16_HO2-->


