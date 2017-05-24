---
title: "Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung | Microsoft Docs"
description: 'Inhalt:'
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 213b6b5274ebde8cc460829ff1edc96a66eef994
ms.contentlocale: de-de
ms.lasthandoff: 03/25/2017


---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a>Konfigurieren von Hive-Richtlinien in HDInsight mit Domänenverknüpfung (Vorschau)
Hier erfahren Sie, wie Sie Apache Ranger-Richtlinien für Hive konfigurieren. In diesem Artikel erstellen Sie zwei Ranger-Richtlinien, um den Zugriff auf die Hive-Beispieltabelle „hivesampletable“ einzuschränken. Die Hive-Beispieltabelle „hivesampletable“ ist in HDInsight-Clustern enthalten. Nach dem Konfigurieren der Richtlinien stellen Sie unter Verwendung von Excel und ODBC-Treiber eine Verbindung mit Hive-Tabellen in HDInsight her.

## <a name="prerequisites"></a>Voraussetzungen
* In die Domäne eingebundener HDInsight-Cluster. Weitere Informationen finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).
* Arbeitsstation mit Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 (eigenständige Version) oder Office 2010 Professional Plus.

## <a name="connect-to-apache-ranger-admin-ui"></a>Herstellen einer Verbindung mit der Apache Ranger-Administratoroberfläche
**So stellen Sie eine Verbindung mit der Ranger-Administratoroberfläche her**

1. Stellen Sie über einen Browser eine Verbindung mit der Ranger-Administratoroberfläche her. Die URL lautet „https://&lt;Clustername>.azurehdinsight.net/Ranger/“.

   > [!NOTE]
   > Ranger verwendet andere Anmeldeinformationen als der Hadoop-Cluster. Stellen Sie die Verbindung mit der Ranger-Administratoroberfläche in einem neuen InPrivate-Browserfenster her, um zu verhindern, dass der Browser zwischengespeicherte Hadoop-Anmeldeinformationen verwendet.
   >
   >
2. Melden Sie sich mit dem Domänenbenutzernamen und dem Kennwort des Clusteradministrators an:

    ![HDInsight mit Domänenverknüpfung – Ranger – Startseite](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Ranger kann derzeit nur mit Yarn und Hive verwendet werden.

## <a name="create-domain-users"></a>Erstellen von Domänenbenutzern
In [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]) haben Sie „hiveuser1“ und „hiveuser2“ erstellt. Diese beiden Benutzerkonten werden in diesem Tutorial verwendet.

## <a name="create-ranger-policies"></a>Erstellen von Ranger-Richtlinien
In diesem Abschnitt erstellen Sie zwei Ranger-Richtlinien für den Zugriff auf „hivesampletable“. Sie erteilen für bestimmte Spaltengruppen die Berechtigung „Auswählen“. Beide Benutzer wurden in [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]) erstellt.  Im nächsten Abschnitt testen Sie die beiden Richtlinien in Excel.

**So erstellen Sie Ranger-Richtlinien**

1. Öffnen Sie die Ranger-Administratoroberfläche. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der Apache Ranger-Administratoroberfläche](#connect-to-apache-ranager-admin-ui).
2. Klicken Sie unter **Hive** auf **&lt;Clustername>_hive**. Zwei vorkonfigurierte Richtlinien werden angezeigt.
3. Klicken Sie auf **Neue Richtlinie hinzufügen**, und geben Sie die folgenden Werte ein:

   * Richtlinienname: read-hivesampletable-all
   * Hive-Datenbank: Standard
   * Tabelle: hivesampletable
   * Hive-Spalte: *
   * Benutzer auswählen: hiveuser1
   * Berechtigungen: Auswählen

     ![HDInsight mit Domänenverknüpfung – Ranger – Konfigurieren der Hive-Richtlinie](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)zu erstellen und zu verwalten.

     > [!NOTE]
     > Wenn unter „Benutzer auswählen“ kein Domänenbenutzer eingetragen wird, warten Sie kurz, bis Ranger mit AAD synchronisiert wurde.
     >
     >
4. Klicken Sie auf **Hinzufügen**, um die Richtlinie zu speichern.
5. Wiederholen Sie die letzten beiden Schritte, um eine weitere Richtlinie mit folgenden Eigenschaften zu erstellen:

   * Richtlinienname: read-hivesampletable-devicemake
   * Hive-Datenbank: Standard
   * Tabelle: hivesampletable
   * Hive-Spalte: clientid, devicemake
   * Benutzer auswählen: hiveuser2
   * Berechtigungen: Auswählen

## <a name="create-hive-odbc-data-source"></a>Erstellen einer Hive ODBC-Datenquelle
Die Anleitung finden Sie unter [Erstellen einer Hive ODBC-Datenquelle](hdinsight-connect-excel-hive-odbc-driver.md).  

    Eigenschaft|Beschreibung
    ---|---
    Name der Datenquelle|Geben Sie einen Namen für die Datenquelle an.
    Host|Geben Sie „&lt;HDInsightClusterName>.azurehdinsight.net“ ein. Beispiel: myHDICluster.azurehdinsight.net
    Port|Verwenden Sie <strong>443</strong>. (Dieser Port wurde von 563 in 443 geändert.)
    Datenbank|Verwenden Sie <strong>Standard</strong>.
    Hive-Servertyp|Wählen Sie <strong>Hive Server 2</strong> aus.
    Mechanismus|Wählen Sie <strong>Azure HDInsight Service</strong> aus.
    HTTP-Pfad|Lassen Sie dieses Feld leer.
    Benutzername|Geben Sie hiveuser1@contoso158.onmicrosoft.com ein. Aktualisieren Sie ggf. den Domänennamen, falls er sich unterscheidet.
    Kennwort|Geben Sie das Kennwort für „hiveuser1“ ein.
    </table>

Klicken Sie vor dem Speichern der Datenquelle auf **Testen**.

## <a name="import-data-into-excel-from-hdinsight"></a>Importieren von Daten aus HDInsight in Excel
Im letzten Abschnitt haben Sie zwei Richtlinien konfiguriert:  „hiveuser1“ verfügt über die Auswahlberechtigung für alle Spalten, „hiveuser2“ über die Auswahlberechtigung für zwei Spalten. In diesem Abschnitt nehmen Sie die Identität der beiden Benutzer an, um Daten in Excel zu importieren.

1. Öffnen Sie eine neue oder bereits vorhandene Arbeitsmappe in Excel.
2. Klicken Sie auf der Registerkarte **Daten** auf **Aus anderen Quellen**. Klicken Sie dann auf **Aus dem Datenverbindungs-Assistenten**, um den **Datenverbindungs-Assistenten**zu starten.

    ![Öffnen des Datenverbindungs-Assistenten][img-hdi-simbahiveodbc.excel.dataconnection]
3. Wählen Sie als Datenquelle **ODBC DSN** aus, und klicken Sie dann auf **Weiter**.
4. Wählen Sie in den ODBC-Datenquellen den Namen der Datenquelle aus, den Sie im vorherigen Schritt erstellt haben, und klicken Sie auf **Weiter**.
5. Geben Sie im Assistenten erneut das Kennwort für den Cluster ein, und klicken Sie dann auf **OK**. Warten Sie, bis sich der Dialog **Datenbank und Tabelle wählen** öffnet. Dies kann einige Zeit dauern.
6. Wählen Sie **hivesampletable** aus, und klicken Sie dann auf **Weiter**.
7. Klicken Sie auf **Fertig stellen**.
8. Im Dialog **Daten importieren** können Sie die Abfrage ändern oder spezifizieren. Klicken Sie hierfür auf **Eigenschaften**. Dies kann einige Zeit dauern.
9. Klicken Sie auf die Registerkarte **Definition**. Befehlstext:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Gemäß den definierten Ranger-Richtlinien verfügt „hiveuser1“ über die Auswahlberechtigung für alle Spalten.  Diese Abfrage kann daher mit den Anmeldeinformationen von „hiveuser1“ verwendet werden, nicht aber mit den Anmeldeinformationen von „hiveuser2“.

   ![Verbindungseigenschaften][img-hdi-simbahiveodbc-excel-connectionproperties]
10. Klicken Sie auf **OK** , um den Dialog Verbindungseigenschaften zu schließen.
11. Klicken Sie auf **OK**, um das Dialogfeld **Daten importieren** zu schließen.  
12. Geben Sie erneut das Kennwort für „hiveuser1“ ein, und klicken Sie auf **OK**. Es dauert einige Zeit, bis die Daten in Excel importiert werden. Nach Abschluss des Vorgangs werden 11 Datenspalten angezeigt.

So testen Sie die zweite Richtlinie (read-hivesampletable-devicemake), die Sie im vorherigen Abschnitt erstellt haben

1. Fügen Sie in Excel ein neues Arbeitsblatt ein.
2. Führen Sie das letzte Verfahren durch, um die Daten zu importieren.  Geben Sie dabei allerdings anstelle der Anmeldeinformationen von „hiveuser1“ die Anmeldeinformationen von „hiveuser2“ an. Das funktioniert jedoch nicht, da „hiveuser2“ nur zum Anzeigen von zwei Spalten berechtigt ist. Sie erhalten den folgenden Fehler:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Führen Sie das gleiche Datenimportverfahren durch. Verwenden Sie diesmal die Anmeldeinformationen von „hiveuser2“, und ändern Sie auch die select-Anweisung von:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    in:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Nach Abschluss des Vorgangs wurden zwei Datenspalten importiert.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Konfigurieren eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).
* Informationen zum Verwalten eines in die Domäne eingebundenen HDInsight-Clusters finden Sie unter [Manage Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-manage.md) (Verwalten von in die Domäne eingebundenen HDInsight-Clustern [Vorschau]).
* Informationen zum Ausführen von Hive-Abfragen per SSH für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Informationen zum Verbinden von Hive unter Verwendung von Hive JDBC finden Sie unter [Herstellen einer Verbindung mit Hive unter Azure HDInsight per Hive-JDBC-Treiber](hdinsight-connect-hive-jdbc-driver.md).
* Informationen zum Verbinden von Excel mit Hadoop unter Verwendung von Hive ODBC finden Sie unter [Verbinden von Excel über den Microsoft Hive ODBC-Treiber mit Hadoop](hdinsight-connect-excel-hive-odbc-driver.md).
* Informationen zum Verbinden von Excel mit Hadoop unter Verwendung von Power Query finden Sie unter [Verbinden von Excel mit Hadoop mithilfe von Power Query](hdinsight-connect-excel-power-query.md).

