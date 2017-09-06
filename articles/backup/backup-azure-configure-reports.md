---
title: "Konfigurieren von Berichten für Azure Backups"
description: "In diesem Artikel wird das Konfigurieren von Power BI-Berichten für Azure Backup mithilfe von Recovery Services-Tresor behandelt."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 4629665e6fbe26c26eb45af7509de338367c4e18
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="configure-azure-backup-reports"></a>Konfigurieren von Azure Backup-Berichten
In diesem Artikel werden die Schritte für das Konfigurieren von Berichten für Azure Backup mithilfe von Recovery Services-Tresor beschrieben und wie Sie auf diese mithilfe von PowerBI zugreifen können. Nachdem Sie diese Schritte ausgeführt haben, können Sie direkt in Power BI alle Berichte anzeigen, Berichte anpassen und erstellen. 

## <a name="supported-scenarios"></a>Unterstützte Szenarien
1. Azure Backup-Berichte werden für die Sicherung von virtuellen Azure-Computern und für die Sicherung von Dateien/Ordnern in der Cloud mit dem Azure Recovery Services-Agent unterstützt.
2. Berichte über Azure SQL, DPM und Azure Backup Server werden derzeit nicht unterstützt.
3. Sie können die Berichte in allen Tresoren mit allen Abonnements anzeigen, sofern für alle Tresore dasselbe Speicherkonto verwendet wird. Das ausgewählte Speicherkonto muss sich in derselben Region wie der Recovery Services-Tresor befinden.
4. Die Häufigkeit der geplanten Aktualisierungen für die Berichte beträgt in Power BI 24 Stunden. In Power BI können Sie die Berichte auch ad hoc aktualisieren. In diesem Fall werden die aktuellsten Daten im Kundenspeicherkonto zum Rendern der Berichte verwendet. 

## <a name="prerequisites"></a>Voraussetzungen
1. Erstellen Sie ein [Azure-Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account), um Berichte zu konfigurieren. Dieses Speicherkonto wird zum Speichern von berichtsbezogenen Daten verwendet.
2. [Erstellen Sie ein Power BI-Konto](https://powerbi.microsoft.com/landing/signin/), um mit dem Power BI-Portal Ihre eigenen Berichte anzuzeigen, anzupassen und zu erstellen.
3. Wenn noch keine Registrierung vorhanden ist, registrieren Sie als Ressourcenanbieter **Microsoft.insights** mit einem Abonnement für ein Speicherkonto und für Recovery Services-Tresor, um die Übertragung von Berichtsdaten an das Speicherkonto zu aktivieren. Um dies zu tun, wechseln Sie zu Azure-Portal > Abonnement > Ressourcenanbieter, und markieren Sie den Anbieter, für den die Registrierung durchgeführt werden soll. 

## <a name="configure-storage-account-for-reports"></a>Konfigurieren des Speicherkontos für Berichte
Mit den folgenden Schritten konfigurieren Sie das Speicherkonto für Recovery Services-Tresore mithilfe von Azure Portal. Es handelt sich dabei um eine nur einmal durchzuführende Konfiguration. Nachdem das Speicherkonto konfiguriert ist, können sie direkt in Power BI Berichte über das Inhaltspaket und die Nutzung anzeigen.
1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit dem nächsten Schritt fortfahren. Wenn Sie keinen Recovery Services-Tresor geöffnet haben und sich im Azure-Portal befinden, klicken Sie im Hub-Menü auf **Durchsuchen**.

   * Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.
   * Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wenn **Recovery Services-Tresore**angezeigt wird, klicken Sie auf den Eintrag.

      ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

     Das ausgewählte Tresor-Dashboard wird geöffnet.
2. Klicken Sie in der unter dem Tresor angezeigten Liste im Abschnitt „Überwachung und Berichte“ auf das Element **Sicherungsberichte**, um das Speicherkonto für Berichte zu konfigurieren.

      ![Wählen Sie in „Sicherungsberichte“ das Menüelement „Schritt 2“](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Klicken Sie auf dem Blatt „Sicherungsberichte“ auf die Schaltfläche **Konfigurieren**. Es wird das Blatt „Azure Application Insights“ geöffnet, das für das Übertragen der Daten per Push an das Speicherkonto des Kunden verwendet wird.

      ![Schritt 3: Konfigurieren des Speicherkontos](./media/backup-azure-configure-reports/configure-storage-account.PNG)
4. Legen Sie die Umschaltfläche für den Status auf **Ein** fest, und aktivieren Sie das Kontrollkästchen **In einem Speicherkonto archivieren**, damit die Berichtsdaten an das Speicherkonto übermittelt werden können.

      ![Schritt 4: Aktivieren der Diagnose](./media/backup-azure-configure-reports/set-status-on.png)
5. Klicken Sie in der Speicherkontoauswahl in der Liste für das Speichern der Berichtsdaten auf das Speicherkonto, und klicken Sie auf **OK**.

      ![Schritt 5: Auswählen des Speicherkontos](./media/backup-azure-configure-reports/select-storage-account.png)
6. Aktivieren Sie das Kontrollkästchen **AzureBackupReport**, und wählen Sie mit dem Schieberegler die Aufbewahrungsdauer für diese Berichtsdaten aus. Die Berichtsdaten werden für den mit diesem Schieberegler ausgewählten Zeitraum im Speicherkonto beibehalten.

      ![Schritt 6: Auswählen des Speicherkontos](./media/backup-azure-configure-reports/save-configuration.png)
7. Überprüfen Sie alle Änderungen, und klicken Sie oben auf die Schaltfläche **Speichern**, wie in der obigen Abbildung gezeigt. Dadurch wird sichergestellt, dass alle Änderungen gespeichert werden und das Speicherkonto nun für das Speichern von Berichtsdaten konfiguriert ist.

> [!NOTE]
> Nachdem Sie die Speicherung der Berichte im Konto konfiguriert haben **warten Sie 24 Stunden**, bis die erste Pushübertragung von Daten abgeschlossen ist. Importieren Sie das Azure Backup-Inhaltspaket erst nach diesem Zeitraum in Power BI. Weitere Informationen finden Sie im [Abschnitt mit häufig gestellten Fragen](#frequently-asked-questions). 
>
>

## <a name="view-reports-in-power-bi"></a>Anzeigen von Berichten in Power BI 
Nachdem das Speicherkonto mit dem Recovery Services-Tresor für Berichte konfiguriert wurde, dauert es etwa 24 Stunden, bis die ersten Daten an das Speicherkonto übermittelt werden. Nach Ablauf von 24 Stunden nach dem Konfigurieren des Speicherkontos können Sie mit den folgenden Schritten Berichte in Power BI anzeigen:
1. Melden Sie sich bei Power BI [an](https://powerbi.microsoft.com/landing/signin/).
2. Klicken Sie auf **Abrufen von Daten**, und klicken Sie in der Inhaltspaket-Bibliothek unter **Dienste** auf „Abrufen“. Folgen Sie den Schritten, die in der [Power BI-Dokumentation für den Zugriff auf das Inhaltspaket](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/) beschrieben werden.

     ![Importieren eines Inhaltspakets](./media/backup-azure-configure-reports/content-pack-import.png)
3. Geben Sie **Azure Backup** in die Suchleiste ein, und klicken Sie auf **Jetzt abrufen**.

      ![Abrufen eines Inhaltspakets](./media/backup-azure-configure-reports/content-pack-get.png)
4. Geben Sie den Namen des in Schritt 5 konfigurierten Speicherkontos ein, und klicken Sie auf die Schaltfläche **Weiter**.

    ![Den Namen des Speicherkontos erfassen](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Geben Sie den Speicherkontoschlüssel für dieses Speicherkonto ein. Sie können [die Speicherzugriffsschlüssel anzeigen und kopieren](../storage/common/storage-create-storage-account.md#manage-your-storage-account), indem Sie im Azure-Portal zu Ihrem Speicherkonto navigieren. 

     ![Speicherkonto erfassen](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Klicken Sie auf die Schaltfläche **Anmelden**. Nach der erfolgreichen Anmeldung erhalten Sie eine Benachrichtigung über den **Datenimport**.

    ![Importieren eines Inhaltspakets](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Nach einiger Zeit ist der Import abgeschlossen, und Sie erhalten eine **Erfolgsbenachrichtigung**. Wenn viele Daten im Speicherkonto vorhanden sind, kann das Importieren des Inhaltspakets eine gewisse Zeit beanspruchen.
    
    ![Erfolgreicher Import des Inhaltspakets](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Nachdem die Daten erfolgreich importiert wurden, wird das **Azure Backup**-Inhaltspaket im Navigationsbereich in **Apps** angezeigt. In der Liste werden jetzt das Dashboard, die Berichte und das Dataset von Azure Backup angezeigt, wobei die neu importierten Berichte mit einem Stern gekennzeichnet sind. 

     ![Azure Backup-Inhaltspaket](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Klicken Sie unter „Dashboards“ auf **Azure Backup**. Es wird eine Reihe angehefteter Schlüsselberichte angezeigt.

      ![Azure Backup-Dashboard](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Um alle Berichte anzuzeigen, klicken Sie auf einen beliebigen Bericht im Dashboard.

      ![Azure Backup-Auftragsintegrität](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Klicken Sie auf die Registerkarten in den Berichten, um die Berichte in diesem Bereich anzuzeigen.

      ![Azure Backup-Berichtsregisterkarten](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
1. **Wie überprüfe ich, ob bereits Berichtsdaten an das Speicherkonto übermittelt werden?**
    
    Sie können zum konfigurierten Speicherkonto wechseln und Container auswählen. Wenn der Container einen Eintrag für „insights-logs-azurebackupreport“ aufweist, bedeutet dies, dass Berichtsdaten übermittelt werden.

2. **Wie häufig werden die Daten an das Speicherkonto und das Azure Backup-Inhaltspaket in Power BI per Push übermittelt?**

   Nach dem Aktivieren dauert es etwa 24 Stunden, bis Daten an das Speicherkonto per Push übermittelt werden. Die Häufigkeit, mit der die Daten nach dem ersten Übermitteln aktualisiert werden, ist der folgenden Tabelle zu entnehmen. 
      * Daten bezüglich **Aufträgen, Warnungen, Sicherungselementen, Tresoren, geschützten Servern und Richtlinien** werden während des Protokollierens per Push an das Speicherkonto des Kunden übermittelt.
      * Daten bezüglich des **Speichers** werden alle 24 Stunden per Push an das Speicherkonto des Kunden übermittelt.
   
    ![Übermittlungshäufigkeit von Azure Backup-Berichtsdaten](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Power BI verfügt über eine [täglich geplante Aktualisierung](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Sie können die Daten in Power BI für das Inhaltspaket manuell aktualisieren.

3. **Wie lange kann ich die Berichte aufbewahren?** 

   Beim Konfigurieren des Speicherkontos können Sie den Aufbewahrungszeitraum für die Berichtsdaten im Speicherkonto auswählen (siehe Schritt 6 im obigen Abschnitt zum Konfigurieren des Speicherkontos für Berichte). Darüber hinaus können Sie die [Berichte in Excel analysieren](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) und sie gemäß Ihren Anforderungen über einen längeren Zeitraum hinweg aufbewahren. 

4. **Sehe ich nach dem Konfigurieren des Speicherkontos alle meine Daten in Berichten?**

   Alle Daten, die nach dem **Konfigurieren des Speicherkontos** generiert wurden, werden per Push an das Speicherkonto übermittelt und stehen in den Berichten zur Verfügung. Die **in Arbeit befindlichen Aufträge werden nicht per Push übermittelt** an die Berichterstellung. Nach dem Abschluss oder dem Scheitern des Auftrags wird dieser an die Berichte gesendet.

5. **Ich habe bereits das Speicherkonto konfiguriert, sodass es Berichte anzeigt. Kann ich die Konfiguration ändern, um ein anderes Speicherkonto zu verwenden?** 

   Ja, Sie können die Konfiguration ändern und auf ein anderes Speicherkonto zeigen. Sie müssen während der Verbindung mit dem Azure Backup-Inhaltspaket das neu konfigurierte Speicherkonto verwenden. Sobald ein anderes Speicherkonto konfiguriert ist, werden neue Daten an dieses Speicherkonto übermittelt. Die älteren Daten (von vor der Konfigurationsänderung) verbleiben jedoch auf dem älteren Speicherkonto.

6. **Kann ich Berichte über alle Tresore und Abonnements anzeigen?** 

   Ja, Sie können dasselbe Speicherkonto für verschiedene Tresore konfigurieren, um tresorübergreifende Berichte anzuzeigen. Sie können auch dasselbe Speicherkonto für Tresore in verschiedenen Abonnements verwenden. Dieses Speicherkonto können Sie dann bei der Verbindung mit dem Azure Backup-Inhaltspaket in Power BI verwenden, um die Berichte anzuzeigen. Allerdings muss sich das ausgewählte Speicherkonto in derselben Region befinden wie der Recovery Services-Tresor.
   
## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das Speicherkonto konfiguriert und das Azure Backup-Inhaltspaket importiert haben, sind die nächsten Schritte das Anpassen dieser Berichte und das Verwenden des Reportingdatenmodells für das Erstellen von Berichten. Weitere Informationen finden Sie in den folgenden Artikeln.

* [Using Azure Backup reporting data model (Verwenden des Reportingdatenmodells von Azure Backup)](backup-azure-reports-data-model.md)
* [Filtering reports in Power BI (Filtern von Berichten in Power BI)](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Erstellen von Berichten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)


