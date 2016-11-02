<properties
    pageTitle="System Update Assessment-Lösung in Log Analytics | Microsoft Azure"
    description="Mithilfe der Lösung „System Updates“ in Log Analytics können Sie fehlende Updates auf Server in Ihrer Infrastruktur anwenden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="system-update-assessment-solution-in-log-analytics"></a>System Update Assessment-Lösung in Log Analytics

Mithilfe der Lösung „System Updates“ in Log Analytics können Sie fehlende Updates auf Server in Ihrer Infrastruktur anwenden. Nach der Installation der Lösung können Sie die Updates, die auf den überwachten Servern fehlen, in Operational Insights über die Kachel **System Update Assessment** auf der Seite **Übersicht** in OMS anzeigen.

Wenn ermittelt wird, dass Updates fehlen, werden ausführliche Informationen im Dashboard **Updates** angezeigt. Mithilfe des Dashboards **Updates** können Sie mit fehlenden Updates arbeiten und einen Plan für deren Anwendung auf die betreffenden Server entwickeln.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Prozesses die System Update Assessment-Lösung zu Ihrem OMS-Arbeitsbereich hinzu.  Es ist keine weitere Konfiguration erforderlich.

## <a name="system-update-data-collection-details"></a>Details zur System Update-Datensammlung

System Update Assessment sammelt Metadaten und Statusdaten mit den Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für System Update Assessment gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Nein](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Nein](./media/log-analytics-system-update/oms-bullet-red.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)| Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates|

Die folgende Tabelle zeigt Beispiele für Datentypen, die von System Update Assessment erfasst werden:

|**Datentyp**|**Felder**|
|---|---|
|Metadaten|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAddress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Zustand|StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>So arbeiten Sie mit Updates

1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **System Update Assessment**.  
    ![Kachel „System Update Assessment“](./media/log-analytics-system-update/sys-update-tile.png)
2. Zeigen Sie auf dem Dashboard **Updates** die Updatekategorien an.  
    ![Dashboard „Updates“](./media/log-analytics-system-update/sys-updates02.png)
3. Scrollen Sie zum rechten Rand der Seite, um das Blatt **Kritische Windows-Updates oder -Sicherheitsupdates** anzuzeigen, und klicken Sie dann unter **Klassifizierung** auf **Sicherheitsupdates**.  
    ![Sicherheitsupdates](./media/log-analytics-system-update/sys-updates03.png)
4. Auf der Seite „Protokollsuche“ wird eine Vielzahl an Informationen zu Sicherheitsupdates angezeigt, die für die Server Ihrer Infrastruktur als fehlend ermittelt wurden. Klicken Sie auf **Liste**, um ausführliche Informationen zu den Updates anzuzeigen.  
    ![Suchergebnisse – Liste](./media/log-analytics-system-update/sys-updates04.png)
5. Auf der Seite „Protokollsuche“ werden ausführliche Informationen zu den einzelnen Updates angezeigt. Klicken Sie neben der KBID-Nummer auf **Ansicht**, um den entsprechenden Artikel auf der Microsoft Support-Website anzuzeigen.  
    ![Ergebnisse der Protokollsuche – KB anzeigen](./media/log-analytics-system-update/sys-updates05.png)
6. Der Webbrowser öffnet die Microsoft Support-Webseite für das Update auf einer neuen Registerkarte. Sehen Sie sich die Informationen zu dem Update an, das nicht vorhanden ist.  
    ![Microsoft Support-Website](./media/log-analytics-system-update/sys-updates06.png)
7. Mit den Informationen, die Sie gefunden haben, können Sie einen Plan zum manuellen Anwenden der fehlenden Updates erstellen, oder Sie können die verbleibenden Schritte ausführen, um das Update automatisch anzuwenden.
8. Wenn Sie die fehlenden Updates automatisch anwenden möchten, gehen Sie zurück zum Dashboard **Updates**, und klicken Sie dann unter **Updateausführungen** auf **Klicken Sie, um eine Updateausführung zu planen**.  
    ![Updateausführungen – Registerkarte „Geplant“](./media/log-analytics-system-update/sys-updates07.png)
9. Klicken Sie auf der Seite **Updateausführungen** auf der Registerkarte **Geplant** auf **Hinzufügen**, um eine neue Updateausführung zu erstellen.  
    ![Registerkarte „Geplant“ – Hinzufügen](./media/log-analytics-system-update/sys-updates08.png)
10. Geben Sie auf der Seite **Neue Updateausführung** einen Namen für die Updateausführung ein, fügen Sie einzelne Computer oder Computergruppen hinzu, definieren Sie einen Zeitplan, und klicken Sie dann auf **Speichern**.  
    ![Neue Updateausführung](./media/log-analytics-system-update/sys-updates09.png)
11. Die Registerkarte **Geplant** der Seite **Updateausführungen** zeigt die neue Updateausführung, die Sie geplant haben.  
    ![Registerkarte „Geplant“](./media/log-analytics-system-update/sys-updates10.png)
12. Wenn die Updateausführung gestartet wird, sehen Sie entsprechende Informationen auf der Registerkarte **Wird ausgeführt**.  
    ![Registerkarte „Wird ausgeführt“](./media/log-analytics-system-update/sys-updates11.png)
13. Nach Abschluss der Updateausführung zeigt die Registerkarte **Abgeschlossen** den Status an.
14. Wenn Updates während der Updateausführung angewendet wurden, wird auf dem Blatt **Kritische Windows-Updates oder -Sicherheitsupdates** die Anzahl der Updates reduziert.  
    ![Blatt „Kritische Windows-Updates oder -Sicherheitsupdates“ – Anzahl der Updates reduziert](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Nächste Schritte

- [Durchsuchen von Protokollen](log-analytics-log-searches.md) , um detaillierte System Update-Daten anzuzeigen



<!--HONumber=Oct16_HO2-->


