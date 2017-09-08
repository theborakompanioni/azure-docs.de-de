---
title: Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Data Lake Analytics-Konten, -Datenquellen, -Benutzer und -Aufträge verwalten."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: e49d1a0e0ccc6567d0a6841817667717ff5dba76
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a>Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Erfahren Sie, wie Sie Azure Data Lake Analytics-Konten, -Kontodatenquellen, -Benutzer und -Aufträge mithilfe des Azure-Portals verwalten. Um Verwaltungsthemen zur Verwendung anderer Tools anzuzeigen, klicken Sie auf eine Registerkarte oben auf der Seite.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Verwalten von Data Lake Analytics-Konten

### <a name="create-an-account"></a>Erstellen eines Kontos

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Intelligence + Analyse** > **Data Lake Analytics**.
3. Wählen Sie Werte für die folgenden Elemente: 
   1. **Name**: Der Name des Data Lake Analytics-Kontos.
   2. **Abonnement**: Das für das Konto verwendete Azure-Abonnement.
   3. **Ressourcengruppe**: Die Azure-Ressourcengruppe, in der das Konto erstellt werden soll. 
   4. **Standort**: Das Azure-Rechenzentrum für das Data Lake Analytics-Konto. 
   5. **Data Lake Store**: Der Standardspeicher, der für das Data Lake Analytics-Konto verwendet werden soll. Das Data Lake Store-Konto und das Data Lake Analytics-Konto müssen sich am selben Ort befinden.
4. Klicken Sie auf **Erstellen**. 

### <a name="delete-a-data-lake-analytics-account"></a>Löschen eines Data Lake Analytics-Kontos

Bevor Sie ein Data Lake Analytics-Konto löschen, löschen Sie dessen Data Lake Store-Konto.

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Löschen**.
3. Geben Sie den Kontonamen ein.
4. Klicken Sie auf **Löschen**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Verwalten von Datenquellen

Data Lake Analytics unterstützt die folgenden Datenquellen:

* Data Lake Store
* Azure Storage

Sie können den Daten-Explorer zum Durchsuchen von Datenquellen und zum Durchführen von grundlegenden Dateiverwaltungsvorgängen verwenden. 

### <a name="add-a-data-source"></a>Hinzufügen einer Datenquelle

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Datenquellen**.
3. Klicken Sie auf **Datenquelle hinzufügen**.
    
   * Zum Hinzufügen eines Data Lake Store-Kontos benötigen Sie den Kontonamen und Zugriff auf das Konto, um eine Abfrage durchführen zu können.
   * Zum Hinzufügen eines Azure Blob-Speichers benötigen Sie das Speicherkonto und den Kontoschlüssel. Diese finden Sie im Speicherkonto im Portal.

## <a name="set-up-firewall-rules"></a>Einrichten von Firewallregeln

Sie können Data Lake Analytics verwenden, um den Zugriff auf Ihr Data Lake Analytics-Konto auf Netzwerkebene weiter zu sperren. Sie können eine Firewall aktivieren, eine IP-Adresse angeben oder einen IP-Adressbereich für Ihre vertrauenswürdigen Clients definieren. Nachdem Sie diese Measures aktiviert haben, können nur Clients, die über die IP-Adressen innerhalb des angegebenen Bereichs verfügen, eine Verbindung mit dem Speicher herstellen.

Wenn andere Azure-Dienste wie Azure Data Factory oder VMs eine Verbindung mit dem Data Lake Analytics-Konto herstellen, stellen Sie sicher, dass **Allow Azure Services** (Azure-Dienste zulassen) **aktiviert** ist. 

### <a name="set-up-a-firewall-rule"></a>Einrichten einer Firewallregel

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie im Menü auf der linken Seite auf **Firewall**.

## <a name="add-a-new-user"></a>Hinzufügen eines neuen Benutzers

Sie können den **Assistent für das Hinzufügen von Benutzern** verwenden, um einfach neue Data Lake-Benutzer bereitzustellen.

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf der linken Seite unter **Erste Schritte** auf **Assistent zum Hinzufügen von Benutzern**.
3. Wählen Sie einen Benutzer aus, und klicken Sie dann auf **Auswählen**.
4. Wählen Sie eine Rolle aus, und klicken Sie dann auf **Auswählen**. Um einen neuen Entwickler für die Verwendung von Azure Data Lake einzurichten, wählen Sie die Rolle **Data Lake Analytics-Entwickler** aus.
5. Wählen Sie die Zugriffssteuerungslisten (ACLs) für die U-SQL-Datenbanken aus. Wenn Sie mit Ihrer Auswahl zufrieden sind, klicken Sie auf **Auswählen**.
6. Wählen Sie die ACLs für Dateien aus. Ändern Sie für den Standardspeicher nicht die ACLs für das Stammverzeichnis „/“ und für den Systemordner. Klicken Sie auf **Auswählen**.
7. Überprüfen Sie alle ausgewählten Änderungen, und klicken Sie dann auf **Ausführen**.
8. Wenn der Assistent abgeschlossen ist, klicken Sie auf **Fertig**.

## <a name="manage-role-based-access-control"></a>Verwalten der rollenbasierten Zugriffssteuerung

Wie andere Azure-Dienste können Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwenden, um zu steuern, wie Benutzer mit dem Dienst interagieren.

Die standardmäßigen RBAC-Rollen haben die folgenden Funktionen:
* **Besitzer**: Kann Aufträge übermitteln, Aufträge überwachen, Aufträge von allen Benutzern abbrechen und das Konto konfigurieren.
* **Mitwirkender**: Kann Aufträge übermitteln, Aufträge überwachen, Aufträge von allen Benutzern abbrechen und das Konto konfigurieren.
* **Leser**: Kann Aufträge überwachen.

Verwenden Sie die Rolle „Data Lake Analytics-Entwickler“, um U-SQL-Entwicklern das Verwenden des Data Lake Analytics-Diensts zu ermöglichen. Sie können die Rolle „Data Lake Analytics-Entwickler“ für Folgendes verwenden:
* Übermitteln von Aufträgen.
* Überwachen des Auftragsstatus und des Fortschritts von Aufträgen, die von jedem Benutzer übermittelt werden.
* Anzeigen der U-SQL-Skripts von Aufträgen, die von einem beliebigen Benutzer übermittelt wurden.
* Abbrechen ausschließlich Ihrer eigenen Aufträge.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Hinzufügen von Benutzern oder Sicherheitsgruppen zu einem Data Lake Analytics-Konto

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Zugriffssteuerung (IAM)** > **Hinzufügen**.
3. Wählen Sie eine Rolle aus.
4. Fügen Sie einen Benutzer hinzu.
5. Klicken Sie auf **OK**.

>[!NOTE]
>Wenn ein Benutzer oder eine Sicherheitsgruppe Aufträge übermitteln muss, benötigt er oder sie auch die Berechtigung für das Speicherkonto. Weitere Informationen finden Sie unter [Schützen von Daten, die im Azure Data Lake Store gespeichert sind](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Verwalten von Aufträgen

### <a name="submit-a-job"></a>Übermitteln eines Auftrags

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.

2. Klicken Sie auf **Neuer Auftrag**. Konfigurieren Sie für jeden Auftrag Folgendes:

    1. **Auftragsname**: Der Name des Auftrags.
    2. **Priorität**: Niedrigere Zahlen haben eine höhere Priorität. Wenn sich zwei Aufträge in der Warteschlange befinden, wird zuerst der Auftrag mit dem niedrigeren Prioritätswert ausgeführt.
    3. **Parallelität**: Die maximale Anzahl von Compute-Vorgängen, die für diesen Auftrag reserviert werden sollen.

3. Klicken Sie auf **Auftrag senden**.

### <a name="monitor-jobs"></a>Überwachen von Aufträgen

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Alle Aufträge anzeigen**. Eine Liste mit allen aktiven und kürzlich abgeschlossenen Aufträgen im Konto wird angezeigt.
3. Klicken Sie optional auf **Filtern**, um die Aufträge nach den Werten **Zeitbereich**, **Auftragsname** und **Ersteller** zu suchen. 

### <a name="monitoring-pipeline-jobs"></a>Überwachen von Pipelineaufträgen
Aufträge, die Teil einer Pipeline sind, arbeiten zusammen – in der Regel sequenziell –, um ein bestimmtes Szenario zu erfüllen. Sie können beispielsweise eine Pipeline für Bereinigung, Extraktion, Transformation und Aggregation des Verbrauchs verwenden, um Einblicke in das Kundenverhalten zu gewinnen. Pipelineaufträge können über die Pipeline-Eigenschaft identifiziert werden, wenn der Auftrag übermittelt wurde. Mit ADF V2 geplanten Aufträgen wird diese Eigenschaft automatisch hinzugefügt. 

So zeigen Sie eine Liste der U-SQL-Aufträge an, die zu Pipelines gehören 

1. Navigieren Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Einblick in Aufträge**. Standardmäßig wird die Registerkarte „Alle Aufträge“ angezeigt, die eine Liste der Aufträge enthält, die ausgeführt werden, sich in der Warteschlange befinden oder beendet wurden.
3. Klicken Sie auf die Registerkarte **Pipelineaufträge**. Eine Liste der Pipelineaufträge wird zusammen mit aggregierten Statistiken für jede Pipeline angezeigt.

### <a name="monitoring-recurring-jobs"></a>Überwachen von wiederkehrenden Aufträgen
Ein wiederkehrender Auftrag besitzt die gleiche Geschäftslogik, verwendet jedoch bei jeder Ausführung unterschiedliche Eingabedaten. Im Idealfall sollten wiederkehrende Aufträge immer erfolgreich und in einer relativ einheitlichen Ausführungszeit ausgeführt werden. Durch die Überwachung dieses Verhaltens stellen Sie sicher, dass der Auftrag fehlerfrei ist. Wiederkehrende Aufträge können mithilfe der Recurrence-Eigenschaft identifiziert werden. Mit ADF V2 geplanten Aufträgen wird diese Eigenschaft automatisch hinzugefügt.

So zeigen Sie eine Liste der wiederkehrenden U-SQL-Aufträge an 

1. Navigieren Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Einblick in Aufträge**. Standardmäßig wird die Registerkarte „Alle Aufträge“ angezeigt, die eine Liste der Aufträge enthält, die ausgeführt werden, sich in der Warteschlange befinden oder beendet wurden.
3. Klicken Sie auf die **Wiederholte Aufträge**. Eine Liste der wiederkehrenden Aufträge wird zusammen mit aggregierten Statistiken zu jedem wiederkehrenden Auftrag angezeigt.

## <a name="manage-policies"></a>Verwalten von Richtlinien

### <a name="account-level-policies"></a>Richtlinien auf Kontoebene

Diese Richtlinien gelten für alle Aufträge in einem Data Lake Analytics-Konto.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximale Anzahl von AUs in einem Data Lake Analytics-Konto
Eine Richtlinie steuert die Gesamtanzahl der Analytics-Einheiten (Analytics Units, AUs), die Ihr Data Lake Analytics-Konto verwenden kann. Standardmäßig ist der Wert auf 250 festgelegt. Wenn dieser Wert beispielsweise auf 250 AUs festgelegt ist, können Sie einen Auftrag haben, der mit 250 zugewiesenen AUs ausgeführt wird, oder 10 Aufträge, die jeweils mit 25 AUs ausgeführt werden. Zusätzliche Aufträge, die übermittelt werden, bleiben in der Warteschlange, bis die ausgeführten Aufträge abgeschlossen sind. Wenn ausgeführte Aufträge abgeschlossen sind, werden die AUs für die auszuführenden Aufträge in der Warteschlange freigegeben.

So ändern Sie die Anzahl der AUs für Ihr Data Lake Analytics-Konto:

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Eigenschaften**.
3. Bewegen Sie unter **Maximal zulässige AUs** den Schieberegler, um einen Wert auszuwählen, oder geben Sie den Wert in das Textfeld ein. 
4. Klicken Sie auf **Speichern**.

> [!NOTE]
> Wenn Sie mehr als die Standard-AUs (250) benötigen, klicken Sie im Portal auf **Hilfe und Support**, um eine Supportanfrage zu übermitteln. Die Anzahl der in Ihrem Data Lake Analytics-Konto verfügbaren AUs kann erhöht werden.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximale Anzahl der Aufträge, die gleichzeitig ausgeführt werden können
Eine Richtlinie steuert, wie viele Aufträge gleichzeitig ausgeführt werden können. Standardmäßig ist dieser Wert auf 20 festgelegt. Wenn bei Ihrem Data Lake Analytics AUs verfügbar sind, werden neue Aufträge sofort ausgeführt, bis die Gesamtanzahl der ausgeführten Aufträge den Wert dieser Richtlinie erreicht. Wenn Sie die maximale Anzahl der Aufträge erreicht haben, die gleichzeitig ausgeführt werden können, werden nachfolgende Aufträge in der Reihenfolge der Priorität in die Warteschlange aufgenommen, bis mindestens ein ausgeführter Auftrag abgeschlossen ist (je nach AU-Verfügbarkeit).

So ändern Sie die Anzahl der Aufträge, die gleichzeitig ausgeführt werden können:

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Eigenschaften**.
3. Bewegen Sie unter **Höchstzahl ausgeführter Aufträge** den Schieberegler, um einen Wert auszuwählen, oder geben Sie den Wert in das Textfeld ein. 
4. Klicken Sie auf **Speichern**.

> [!NOTE]
> Wenn Sie mehr als die Standardanzahl von Aufträgen (20) ausführen müssen, klicken Sie im Portal auf **Hilfe und Support**, um eine Supportanfrage zu übermitteln. Die Anzahl der Aufträge, die in Ihrem Data Lake Analytics-Konto gleichzeitig ausgeführt werden können, kann erhöht werden.
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a>Aufbewahrungsdauer von Auftragsmetadaten und -ressourcen 
Wenn Ihre Benutzer U-SQL-Aufträge ausführen, bewahrt der Data Lake Analytics-Dienst alle zugehörigen Dateien auf. Zugehörige Dateien umfassen das U-SQL-Skript, die DLL-Dateien, auf die im U-SQL-Skript verwiesen wird, die kompilierten Ressourcen und die Statistiken. Die Dateien befinden sich im Ordner „/system/“ des Standard-Azure Data Lake Storage-Kontos. Diese Richtlinie steuert, wie lange diese Ressourcen gespeichert werden, bis sie automatisch gelöscht werden (die Standardzeit beträgt 30 Tage). Sie können diese Dateien zum Debuggen und zur Leistungsoptimierung von Aufträgen verwenden, die Sie künftig erneut ausführen werden.

So ändern Sie die Aufbewahrungsdauer von Auftragsmetadaten und -ressourcen:

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Eigenschaften**.
3. Bewegen Sie unter **Abfrageaufbewahrungsdauer in Tagen** den Schieberegler, um einen Wert auszuwählen, oder geben Sie den Wert in das Textfeld ein.  
4. Klicken Sie auf **Speichern**.

### <a name="job-level-policies"></a>Richtlinien auf Auftragsebene
Mit Richtlinien auf Auftragsebene können Sie die maximale Anzahl von AUs und die maximale Priorität steuern, die einzelne Benutzer (oder Mitglieder bestimmter Sicherheitsgruppen) für Aufträge festlegen können, die sie übermitteln. Dadurch können Sie die Kosten kontrollieren, die durch jeden Benutzer entstehen. Darüber hinaus können Sie kontrollieren, welchen Effekt geplante Aufträge auf Produktionsaufträge mit hoher Priorität haben könnten, die im selben Data Lake Analytics-Konto ausgeführt werden.

Data Lake Analytics verfügt über zwei Richtlinien, die Sie auf Auftragsebene festlegen können:

* **AU-Grenzwert pro Auftrag**: Benutzer können nur Aufträge übermitteln, die über bis zu dieser Anzahl von AUs verfügen. Standardmäßig stimmt dieser Grenzwert mit dem Höchstwert für AUs für das Konto überein.
* **Priorität**: Benutzer können nur Aufträge übermitteln, deren Priorität niedriger als oder gleich wie dieser Wert ist. Beachten Sie, dass eine höhere Anzahl eine niedrigere Priorität bedeutet. Standardmäßig ist dieser Wert auf 1 festgelegt, die höchstmögliche Priorität.

Für jedes Konto ist eine Standardrichtlinie festgelegt. Die Standardrichtlinie gilt für alle Benutzer des Kontos. Sie können zusätzliche Richtlinien für bestimmte Benutzer und Gruppen festlegen. 

> [!NOTE]
> Richtlinien auf Kontoebene und Richtlinien auf Auftragsebene gelten gleichzeitig.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Hinzufügen einer Richtlinie für einen bestimmten Benutzer oder eine bestimmte Gruppe

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Eigenschaften**.
3. Klicken Sie unter **Grenzwerte bei der Auftragsübermittlung** auf die Schaltfläche **Richtlinie hinzufügen**. Wählen Sie dann die folgenden Einstellungen aus, oder geben Sie sie ein:
    1. **Richtlinienname berechnen**: Geben Sie einen Richtliniennamen ein, der den Zweck der Richtlinie benennt.
    2. **Benutzer oder Gruppe auswählen**: Wählen Sie den Benutzer oder die Gruppe aus, für den oder die diese Richtlinie gilt.
    3. **Den AU-Grenzwert für den Auftrag festlegen**: Legen Sie den AU-Grenzwert fest, der für den ausgewählten Benutzer oder die ausgewählte Gruppe gilt.
    4. **Maximale Priorität festlegen**: Legen Sie die maximale Priorität fest, die für den ausgewählten Benutzer oder die ausgewählte Gruppe gilt.

4. Klicken Sie auf **OK**.

5. Die neue Richtlinie wird in der **Standardrichtlinientabelle** unter **Grenzwerte bei der Auftragsübermittlung** aufgeführt. 

#### <a name="delete-or-edit-an-existing-policy"></a>Löschen oder Bearbeiten einer vorhandenen Richtlinie

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Klicken Sie auf **Eigenschaften**.
3. Suchen Sie unter **Grenzwerte bei der Auftragsübermittlung** nach der Richtlinie, die Sie bearbeiten möchten.
4.  Um die Optionen **Löschen** und **Bearbeiten** in der Spalte ganz rechts in der Tabelle anzuzeigen, klicken Sie auf **...**.

### <a name="additional-resources-for-job-policies"></a>Zusätzliche Ressourcen für Auftragsrichtlinien
* [Blogbeitrag zur Richtlinienübersicht](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Blogbeitrag zu Richtlinien auf Kontoebene](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Blogbeitrag zu Richtlinien auf Auftragsebene](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Nächste Schritte

* [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
* [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)


