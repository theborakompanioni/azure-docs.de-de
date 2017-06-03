---
title: "Azure-Portal: Erstellen und Verwalten eines SQL-Datenbank-Pools für elastische Datenbanken | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe der im Azure-Portal und in SQL-Datenbank integrierten Intelligenz einen skalierbaren Pool für elastische Datenbanken verwalten, überwachen und skalieren, um die Leistung der Datenbank und die Kostenverwaltung zu optimieren."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 05/08/2017
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 66f7cc63d311b6b5cd223fe0d8a9bf9f636abef1
ms.contentlocale: de-de
ms.lasthandoff: 04/20/2017


---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a>Erstellen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal
In diesem Thema erfahren Sie, wie Sie skalierbare [Pools für elastische Datenbanken](sql-database-elastic-pool.md) über das Azure-Portal erstellen und verwalten. Sie können einen Azure-Pool für elastische Datenbanken auch mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), der REST-API oder [C#](sql-database-elastic-pool-manage-csharp.md) erstellen und verwalten. Mithilfe von [Transact-SQL](sql-database-elastic-pool-manage-tsql.md) können Sie Datenbanken auch erstellen und in und aus Pools für elastische Datenbanken verschieben.

## <a name="create-an-elastic-pool"></a>Erstellen eines elastischen Pools 

Bei der Erstellung eines Pools für elastische Datenbanken gibt es zwei Möglichkeiten: Sie können einen Pool von Grund auf neu erstellen, wenn Sie genau wissen, wie der Pool aussehen soll. Sie können aber auch mit einer Empfehlung des Diensts beginnen. SQL-Datenbank verfügt über integrierte Intelligenz, die auf der Grundlage der Telemetriedaten Ihrer bisherigen Datenbanknutzung ein Setup für einen Pool für elastische Datenbanken empfiehlt, falls dieses für Sie kostengünstiger ist.

Auf einem Server können mehrere Pools erstellt werden, es ist jedoch nicht möglich, Datenbanken von verschiedenen Servern im gleichen Pool zusammenzufassen.

> [!NOTE]
> Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase.  Die allgemeine Verfügbarkeit von elastischen Pools in dieser Region wird so bald wie möglich bereitgestellt.
>
>

### <a name="step-1-create-an-elastic-pool"></a>Schritt 1: Erstellen eines Pools für elastische Datenbanken

Am einfachsten verschieben Sie vorhandene Datenbanken in einen Pool für elastische Datenbanken, indem Sie im Portal auf der Grundlage eines bereits vorhandenen **Serverblatts** einen Pool für elastische Datenbanken erstellen.

> [!NOTE]
> Sie können einen Pool für elastische Datenbanken aber auch erstellen, indem Sie im **Marketplace** nach **Elastischer SQL-Pool** suchen oder auf dem Navigationsblatt **Elastische SQL-Pools** auf **+Hinzufügen** klicken. Im Rahmen dieses Poolbereitstellungsworkflows können Sie einen neuen oder einen bereits vorhandenen Server angeben.
>
>

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) auf **Weitere Dienste** **>** **SQL Server** und dann auf den Server mit den Datenbanken, die Sie einem Pool für elastische Datenbanken hinzufügen möchten.
2. Klicken Sie auf **Neuer Pool**.

    ![Hinzufügen eines Pools zu einem Server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **ODER**

    Unter Umständen wird eine Meldung angezeigt, die darauf hinweist, dass empfohlene elastische Pools für den Server vorhanden sind. Klicken Sie auf die Meldung, um die Pools anzuzeigen, die basierend auf den Telemetriedaten zur bisherigen Datenbanknutzung empfohlen werden. Klicken Sie anschließend auf die Ebene, um weitere Details anzuzeigen und den Pool anzupassen. Informationen zur Bereitstellung der Empfehlung finden Sie weiter unten in diesem Thema unter [Grundlagen von Poolempfehlungen](#understand-pool-recommendations).

    ![Empfohlener Pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Das Blatt **Elastischer Pool** wird angezeigt. Hier werden die Einstellungen für Ihren Pool festgelegt. Wenn Sie im vorherigen Schritt auf **Neuer Pool** geklickt haben, ist der Tarif standardmäßig auf **Standard** festgelegt, und es sind keine Datenbanken ausgewählt. Sie können einen leeren Pool erstellen oder einen Satz bestehender Datenbanken vom vorher ausgewählten Server angeben, der in den Pool verschoben werden soll. Wenn Sie einen empfohlenen Pool erstellen, sind der empfohlene Tarif, die Leistungseinstellungen und eine Liste mit Datenbanken bereits ausgefüllt. Diese Angaben können jedoch noch geändert werden.

    ![Konfigurieren eines elastischen Pools](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Geben Sie einen Namen für den elastischen Pool an, oder behalten Sie den Standardnamen bei.

### <a name="step-2-choose-a-pricing-tier"></a>Schritt 2: Auswählen eines Tarifs

Der Tarif für den Pool bestimmt die Features für die elastischen Datenbanken im Pool, die maximale Anzahl von eDTUs (eDTU MAX) und den für jede Datenbank verfügbaren Speicher (GB). Weitere Informationen finden Sie unter den Dienstebenen.

Klicken Sie auf **Tarif**, wählen sie den gewünschten Tarif aus, und klicken Sie anschließend auf **Auswählen**, um den Tarif für den Pools zu ändern.

> [!IMPORTANT]
> Nachdem Sie den Tarif ausgewählt und Ihre Änderungen übernommen haben, indem Sie im letzten Schritt auf **OK** geklickt haben, können Sie den Tarif des Pools nicht mehr ändern. Wenn Sie den Tarif eines vorhandenen Pools für elastische Datenbanken ändern möchten, müssen Sie einen Pool für elastische Datenbanken unter dem gewünschten Tarif erstellen und die Datenbanken dann in diesen neuen Pool migrieren.
>
>

![Auswählen einer Preisstufe](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a>Schritt 3: Konfigurieren des Pools

Klicken Sie nach dem Festlegen des Tarifs auf „Pool konfigurieren“. Fügen Sie nun Datenbanken hinzu, legen Sie Pool-eDTUs und den Poolspeicher (in GB) fest, und geben Sie die eDTU-Mindestwerte und -Maximalwerte für die elastischen Datenbanken im Pool an.

1. Klicken Sie auf **Pool konfigurieren**
2. Wählen Sie die Datenbanken aus, die Sie dem Pool hinzufügen möchten. Beim Erstellen des Pools ist dies ein optionaler Schritt. Nachdem der Pool erstellt wurde, können Datenbanken hinzugefügt werden.
    Klicken Sie zum Hinzufügen von Datenbanken auf **Datenbank hinzufügen**, wählen Sie die Datenbanken aus, die Sie hinzufügen möchten, und klicken Sie anschließend auf die Schaltfläche **Auswählen**.

    ![Datenbanken hinzufügen](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Wenn die von Ihnen verwendeten Datenbanken über ausreichend Telemetriedaten zur bisherigen Nutzung verfügen, werden der Graph **Estimated eDTU and GB usage** (Geschätzte eDTU- und GB-Nutzung) und das Balkendiagramm **Actual eDTU usage** (Tatsächliche eDTU-Nutzung) aktualisiert. Diese helfen Ihnen, Konfigurationsentscheidungen zu treffen. Außerdem erhalten Sie vom Dienst unter Umständen eine Empfehlungsnachricht zur richtigen Dimensionierung des Pools. Weitere Informationen finden Sie unter [Dynamic Recommendations](#dynamic-recommendations)weiter.

3. Verwenden Sie die Steuerelemente auf der Seite **Pool konfigurieren** , um die Einstellungen zu untersuchen und den Pool zu konfigurieren. Ausführlichere Informationen zu den Grenzwerten für die einzelnen Dienstebenen finden Sie in den [Beschränkungen für Pools für elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools). Eine ausführliche Anleitung zur Wahl der passenden Poolgröße finden Sie unter [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool.md). Weitere Informationen zu Pooleinstellungen finden Sie unter [Eigenschaften von Pools für elastische Datenbanken](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![Konfigurieren eines elastischen Pools](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Klicken Sie auf **Auswählen** in the **Configure Pool** .
5. Klicken Sie auf **OK** , um den Pool zu erstellen.

## <a name="understand-elastic-pool-recommendations"></a>Grundlegendes zu Empfehlungen für Pools für elastische Datenbanken

Der SQL-Datenbankdienst bewertet den Nutzungsverlauf und empfiehlt einen oder mehrere Pools, wenn dies kostengünstiger als die Verwendung von Einzeldatenbanken ist. Jede Empfehlung ist mit einer eindeutigen Teilmenge der Datenbanken des Servers konfiguriert, die optimal zum Pool passen.

![Empfohlener Pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Die Poolempfehlung umfasst Folgendes:

- Tarif für den Pool (Basic, Standard, Premium oder Premium RS)
- Passende **POOL-eDTUs** (auch als maximale eDTUs pro Pool bezeichnet)
- **eDTU maximal** und **eDTU minimal** pro Datenbank
- Liste der empfohlenen Datenbanken für den Pool

> [!IMPORTANT]
> Der Dienst berücksichtigt bei der Empfehlung von Pools die Telemetrie der letzten 30 Tage. Damit eine Datenbank für einen Pool für elastische Datenbanken infrage kommt, muss sie seit mindestens sieben Tagen vorhanden sein. Datenbanken, die sich bereits in einem elastischen Pool befinden, werden nicht als Kandidaten für elastische Pools empfohlen.
>

Der Dienst bewertet den Ressourcenbedarf und die Kosteneffizienz beim Verschieben der Einzeldatenbanken auf jeder Dienstebene in Pools auf der gleichen Ebene. Beispielsweise werden alle Datenbanken der Standard-Edition auf einem Server entsprechend ihrer Eignung für einen elastischen Pool der Standard-Edition bewertet. Dies bedeutet, dass der Dienst keine tarifübergreifenden Empfehlungen trifft, also etwa eine Standard-Datenbank nicht in einen Premium-Pool verschiebt.

Nach dem Hinzufügen von Datenbanken zum Pool werden auf der Grundlage des Nutzungsverlaufs der von Ihnen ausgewählten Datenbanken dynamisch Empfehlungen generiert. Diese Empfehlungen werden im Diagramm zur eDTU- und GB-Nutzung sowie im Empfehlungsbanner oben auf dem Blatt **Pool konfigurieren** angezeigt. Die Empfehlungen unterstützen Sie bei der Erstellung eines Pools für elastische Datenbanken, der speziell für Ihre Datenbanken optimiert ist.

![Dynamische Empfehlungen](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a>Verwalten und Überwachen eines Pools für elastische Datenbanken

Mithilfe des Azure-Portals können Sie einen Pool für elastische Datenbanken sowie die darin enthaltenen Datenbanken überwachen und verwalten. Über das Portal können Sie die Verwendung eines elastischen Pools sowie der darin enthaltenen Datenbanken überwachen. Außerdem können Sie einen Änderungssatz für Ihren elastischen Pool erstellen und alle Änderungen gleichzeitig übermitteln. So können Sie etwa Datenbanken hinzufügen oder entfernen, die Einstellungen des elastischen Pools ändern oder Ihre Datenbankeinstellungen anpassen.

Die Abbildung weiter unten zeigt ein Beispiel für einen Pool für elastische Datenbanken. Die Ansicht enthält Folgendes:

*  Diagramme zur Überwachung der Ressourcenverwendung durch den elastischen Pool und die im Pool enthaltenen Datenbanken.
*  Die Schaltfläche **Pool konfigurieren** zum Vornehmen von Änderungen an dem elastischen Pool.
*  Die Schaltfläche **Datenbank erstellen** zum Erstellen einer Datenbank und Hinzufügen der Datenbank zum aktuellen Pool für elastische Datenbanken.
*  Elastische Aufträge zum Verwalten einer großen Anzahl von Datenbanken durch Ausführen von Transact-SQL-Skripts für alle Datenbanken in einer Liste.

![Poolansicht][2]

Navigieren Sie zu einem bestimmten Pool, um dessen Ressourcenverwendung anzuzeigen. In der Standardkonfiguration wird für den Pool die Speicher- und eDTU-Verwendung der letzten Stunde angezeigt. Das Diagramm kann mit anderen Metriken und Zeitfenstern konfiguriert werden.

1. Wählen Sie einen Pool für elastische Datenbanken aus.
2. Unter **Überwachung des elastischen Pools** befindet sich ein Diagramm namens **Ressourcenverwendung**. Klicken Sie auf das Diagramm.

    ![Überwachung des elastischen Pools][3]

    Das Blatt **Metrik** wird geöffnet und zeigt eine detaillierte Ansicht der angegebenen Metriken für das angegebene Zeitfenster.   

    ![Blatt "Metrik"][9]

### <a name="to-customize-the-chart-display"></a>So passen Sie die Darstellung des Diagramms an

Sie können das Diagramm und das Metrikblatt bearbeiten, um andere Metriken wie etwa die prozentuale Verwendung von CPU, Daten-E/A und Protokoll-E/A anzuzeigen.

1. Klicken Sie auf dem Metrikblatt auf **Bearbeiten**.

    ![Klicken auf „Bearbeiten“][6]

2. Wählen Sie auf dem Blatt **Diagramm bearbeiten** einen Zeitbereich („Letzte Stunde“, „Heute“ oder „Letzte Woche“) aus, oder klicken Sie auf **Benutzerdefiniert**, um einen beliebigen Datumsbereich innerhalb der letzten beiden Wochen auszuwählen. Wählen Sie den Diagrammtyp („Balken“ oder „Linie“) und dann die zu überwachenden Ressourcen aus.

   > [!Note]
   > Hinweis: Nur Metriken mit der gleichen Maßeinheit können gleichzeitig im Diagramm angezeigt werden. Wenn Sie also beispielsweise die Option für den eDTU-Prozentsatz auswählen, können nur andere prozentbasierte Metriken ausgewählt werden.
   >

    ![Klicken auf „Bearbeiten“](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)



3. Klicken Sie dann auf **OK**.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Verwalten und Überwachen von Datenbanken in einem Pool für elastische Datenbanken

Einzelne Datenbanken können auch auf potenzielle Probleme überwacht werden.

1. Unter **Überwachung der elastischen Datenbank**befindet sich ein Diagramm mit Metriken für fünf Datenbanken. Standardmäßig zeigt das Diagramm die fünf wichtigsten Datenbanken des Pools (gemessen an der durchschnittlichen eDTU-Verwendung innerhalb der letzten Stunde). Klicken Sie auf das Diagramm.

    ![Überwachung des elastischen Pools][4]

2. Das Blatt **Datenbank-Ressourcennutzung** wird angezeigt. Hier finden Sie eine detaillierte Ansicht der Datenbankverwendung im Pool. Über das Raster im unteren Teil des Blatts können Sie bis zu fünf beliebige Datenbanken im Pool auswählen und deren Verwendung im Diagramm anzeigen. Sie können auch auf **Diagramm bearbeiten** klicken, um die Metriken und das Zeitfenster für das Diagramm anzupassen.

    ![Blatt mit der Ressourcenverwendung der Datenbank][8]

### <a name="to-customize-the-view"></a>So passen Sie die Ansicht an

1. Klicken Sie auf dem Blatt **Datenbank-Ressourcennutzung** auf **Diagramm bearbeiten**.

    ![Klicken auf „Diagramm bearbeiten“](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Wählen Sie auf dem Blatt **Diagramm bearbeiten** einen Zeitbereich („Letzte Stunde“ oder „Letzte 24 Stunden“) aus, oder klicken Sie auf **Benutzerdefiniert**, um einen anderen Tag innerhalb der letzten beiden Wochen auszuwählen.

    ![Klicken auf „Benutzerdefiniert“](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klicken Sie auf das Dropdownfeld für den Datenbankvergleich **** , um eine andere Metrik auszuwählen.

    ![Bearbeiten des Diagramms](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>So wählen Sie die zu überwachenden Datenbanken aus

In der Datenbankliste auf dem Blatt mit der Ressourcenverwendung der Datenbank **** können Sie nach bestimmten Datenbanken suchen, indem Sie die einzelnen Listenseiten durchgehen oder den Namen einer Datenbank eingeben. Aktivieren Sie das Kontrollkästchen, um die Datenbank auszuwählen.

![Suchen nach zu überwachenden Datenbanken][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Hinzufügen einer Warnung zu einer Poolressource für elastische Datenbanken

Sie können einem Pool für elastische Datenbanken Regeln hinzufügen, um E-Mails an Personen oder Warnzeichenfolgen an URL-Endpunkte zu senden, wenn der Pool für elastische Datenbanken einen bestimmten Verwendungsschwellenwert erreicht.

> [!IMPORTANT]
> Bei der Überwachung der Ressourcenverwendung für Pools für elastische Datenbanken gibt es eine Verzögerung von mindestens 20 Minuten. Für Pools für elastische Datenbanken können derzeit keine Warnungen mit weniger als 30 Minuten festgelegt werden. Warnungen für elastische Pools mit einem Zeitraum (Parameter namens „-WindowSize“ in der PowerShell-API) von weniger als 30 Minuten werden möglicherweise nicht ausgelöst. Stellen Sie sicher, dass alle Warnungen, die Sie für Pools für elastische Datenbanken definieren, einen Zeitraum (WindowSize) von mindestens 30 Minuten besitzen.
>
>

**So fügen Sie eine Warnung zu einer Ressource hinzu:**

1. Klicken Sie auf das Diagramm **Ressourcennutzung**, um das Blatt **Metrik** zu öffnen. Klicken Sie auf **Warnung hinzufügen**, und füllen Sie dann die Informationen auf dem Blatt **Warnungsregel hinzufügen** aus (**Ressource** wird automatisch auf den Pool festgelegt, mit dem Sie arbeiten).
2. Geben Sie einen aussagekräftigen **Namen** und eine aussagekräftige **Beschreibung** für die Warnung ein.
3. Wählen Sie in der Liste eine **Metrik** aus, für die die Warnung ausgegeben werden soll.

    Das Diagramm zeigt dynamisch die Ressourcenverwendung für diese Metrik an, um Ihnen die Festlegung eines Schwellenwerts zu erleichtern.

4. Wählen Sie eine **Bedingung** (größer als, kleiner als usw.) und einen **Schwellenwert** aus.
5. Klicken Sie auf **OK**.

## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in einen elastischen Pool

Sie können Datenbanken in einem vorhandenen Pool hinzufügen oder entfernen. Datenbanken können sich in verschiedenen Pools befinden. Sie können jedoch nur Datenbanken hinzufügen, die sich auf demselben logischen Server befinden.

1. Klicken Sie auf dem Blatt für den Pool unter **Elastische Datenbanken** auf **Pool konfigurieren**.

    ![Klicken auf „Pool konfigurieren“][1]

2. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Zu Pool hinzufügen**.

    ![Klicken auf „Zu Pool hinzufügen“](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Wählen Sie auf dem Blatt **Datenbanken hinzufügen** die Datenbanken aus, die dem Pool hinzugefügt werden sollen. Klicken Sie dann auf **Auswählen**.

    ![Auswählen hinzuzufügender Datenbanken](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Auf dem Blatt **Pool konfigurieren** wird nun die hinzuzufügende Datenbank mit dem Status **Ausstehend** angezeigt.

    ![Ausstehende Hinzufügungen zum Pool](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einem elastischen Pool

1. Wählen Sie auf dem Blatt **Pool konfigurieren** die zu entfernenden Datenbanken aus.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicken Sie auf **Aus Pool entfernen**.

    ![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Auf dem Blatt **Pool konfigurieren** wird nun die zu entfernende Datenbank mit dem Status **Ausstehend** angezeigt.

    ![Vorschau-Datenbank hinzufügen und entfernen](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. Klicken Sie auf dem Blatt **Pool konfigurieren** auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Ändern der Leistungseinstellungen eines Pools für elastische Datenbanken

Im Zuge der Überwachung der Ressourcenverwendung eines Pools für elastische Datenbanken stellen Sie unter Umständen fest, dass gewisse Anpassungen erforderlich sind. Möglicherweise muss die Leistung oder Speicherbegrenzung des Pools geändert werden. Gegebenenfalls möchten Sie auch die Datenbankeinstellungen im Pool ändern. Sie können das Setup des Pools jederzeit ändern, um die beste Leistungsverteilung und Kosteneffizienz zu erzielen. Weitere Informationen finden Sie unter [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool.md).

So ändern Sie die eDTUs oder Speicherbegrenzungen pro Pool und die eDTUs pro Datenbank:

1. Öffnen Sie das Blatt **Pool konfigurieren** .

    Ändern Sie mithilfe der Schieberegler für die Einstellungen des Pools für elastische Datenbanken**** die Pooleinstellungen.

    ![Ressourcenverwendung eines elastischen Pools](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Wenn sich die Einstellung ändert, werden in der Anzeige die geschätzten monatlichen Kosten der Änderung angezeigt.

    ![Aktualisieren eines Pools für elastische Datenbanken und neue monatliche Kosten](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>Latenzzeit der elastischen Poolvorgänge
* Änderungen der minimalen oder maximalen Anzahl der eDTUs pro Datenbank werden in der Regel in maximal 5 Minuten durchgeführt.
* Änderungen der eDTUs pro Pool hängen von der Gesamtmenge des Speicherplatzes aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

## <a name="next-steps"></a>Nächste Schritte

- Das Konzept eines Pools für elastische Datenbanken wird unter [Was ist ein Azure SQL-Pool für elastische Datenbanken?](sql-database-elastic-pool.md) erläutert.
- Informationen zur Verwendung von Pools für elastische Datenbanken finden Sie unter [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool.md).
- Informationen zur Verwendung elastischer Aufträge, um Transact-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool auszuführen, finden Sie in der [Übersicht über elastische Aufträge](sql-database-elastic-jobs-overview.md).
- Informationen zum Durchführen übergreifender Abfragen für eine beliebige Anzahl von Datenbanken im Pool finden Sie in der [Übersicht über elastische Abfragen](sql-database-elastic-query-overview.md).
- Informationen zum Durchführen von Transaktionen für eine beliebige Anzahl von Datenbanken im Pool finden Sie in der [Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png

