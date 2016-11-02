<properties
   pageTitle="Anzeigen von Bereitstellungsvorgängen mit dem Portal | Microsoft Azure"
   description="Beschreibt, wie Sie mithilfe des Azure-Portals Probleme in der Resource Manager-Bereitstellung erkennen können."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure-Befehlszeilenschnittstelle](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Sie können die Vorgänge für eine Bereitstellung im Azure-Portal anzeigen. Die Anzeige der Vorgänge ist wahrscheinlich dann am interessantesten, wenn während der Bereitstellung ein Fehler auftritt. Daher konzentriert sich dieser Artikel auf das Anzeigen von fehlerhaften Vorgängen. Das Portal bietet eine Schnittstelle, mit der Sie die Fehler leicht finden und potenzielle Korrekturen ermitteln können.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## Verwenden von Bereitstellungsvorgängen zur Problembehandlung

Um die Bereitstellungsvorgänge anzuzeigen, gehen Sie folgendermaßen vor:

1. Beachten Sie den Status der letzten Bereitstellung für die an der Bereitstellung beteiligte Ressourcengruppe. Sie können diesen Status auswählen, um weitere Details anzuzeigen.

    ![Bereitstellungsstatus](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Der Verlauf mit den letzten Bereitstellungen wird angezeigt. Wählen Sie die fehlerhafte Bereitstellung aus.

    ![Bereitstellungsstatus](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Wählen Sie **Fehler. Klicken Sie hier, um Details anzuzeigen.**, um eine Beschreibung der Ursache für die fehlerhafte Bereitstellung anzuzeigen. In der folgenden Abbildung ist der DNS-Eintrag nicht eindeutig.

    ![Bereitstellungsfehler anzeigen](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Diese Fehlermeldung sollte ausreichen, damit Sie mit der Problembehandlung beginnen können. Wenn Sie jedoch weitere Details zu den abgeschlossenen Tasks benötigen, können Sie die Vorgänge wie in den folgenden Schritten erläutert anzeigen.

4. Sie können alle Bereitstellungsvorgänge auf dem Blatt **Bereitstellung** anzeigen. Wählen Sie einen Vorgang, um weitere Details anzuzeigen.

    ![Vorgänge anzeigen](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    In diesem Fall sehen Sie, dass das Speicherkonto, das virtuelle Netzwerk und die Verfügbarkeitsgruppe erfolgreich erstellt wurden. Bei der öffentlichen IP-Adresse ist ein Fehler aufgetreten, und für andere Ressourcen wurden keine Versuche unternommen.

5. Sie können Ereignisse für die Bereitstellung anzeigen, indem Sie **Ereignisse** wählen.

    ![Ereignisse anzeigen](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Sie sehen alle Ereignisse für die Bereitstellung und können zum Anzeigen weiterer Einzelheiten ein Ereignis auswählen.

    ![Ereignisse anzeigen](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## Verwenden von Überwachungsprotokollen zur Problembehandlung

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Gehen Sie wie folgt vor, um Fehler für eine Bereitstellung anzuzeigen:

1. Zeigen Sie die Überwachungsprotokolle für eine Ressourcengruppe an, indem Sie **Überwachungsprotokolle** wählen.

    ![Überwachungsprotokolle auswählen](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. Auf dem Blatt **Überwachungsprotokolle** wird eine Zusammenfassung der aktuellen Vorgänge für alle Ressourcengruppen in Ihrem Abonnement angezeigt. Dazu gehören eine grafische Darstellung der Zeit und des Status der Vorgänge sowie eine Liste der Vorgänge.

    ![Aktionen anzeigen](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Sie können die Ansicht der Überwachungsprotokolle filtern, um sich auf bestimmte Bedingungen zu konzentrieren. Wählen Sie oben auf dem Blatt **Überwachungsprotokolle** die Option **Filtern**.

    ![Protokolle filtern](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Wählen Sie auf dem Blatt **Filter** Bedingungen aus, um die Ansicht der Überwachungsprotokolle auf die Vorgänge zu beschränken, die Sie anzeigen möchten. Beispielsweise können Sie Vorgänge so filtern, dass nur Fehler für die Ressourcengruppe angezeigt werden.

    ![Filteroptionen festlegen](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Durch Festlegen eines Zeitraums können Sie die Vorgänge weiter filtern. In der folgenden Abbildung wird die Ansicht auf einen bestimmten 20-minütigen Zeitraum gefiltert.

    ![Zeit festlegen](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Sie können beliebige Vorgänge in der Liste auswählen. Wählen Sie den Vorgang mit dem Fehler aus, den Sie untersuchen möchten.

    ![Vorgang auswählen](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Alle Ereignisse für diesen Vorgang werden angezeigt. Beachten Sie die **Korrelations-IDs** in der Zusammenfassung. Anhand dieser ID werden verwandte Ereignisse überwacht. Sie kann hilfreich sein, wenn Sie bei der Problembehandlung mit dem technischen Support zusammenarbeiten. Sie können ein beliebiges Ereignis auswählen, um Details zu dem Ereignis anzuzeigen.

    ![Ereignis auswählen](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Die Details zum Ereignis werden angezeigt. Achten Sie insbesondere auf die **Eigenschaften**, um Informationen über den Fehler zu erhalten.

    ![Details für Überwachungsprotokoll anzeigen](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Der auf die Überwachungsprotokolle angewendete Filter wird bis zur nächsten Anzeige der Überwachungsprotokolle beibehalten, daher müssen Sie die Werte möglicherweise ändern, um die Ansicht der Vorgänge wieder zu erweitern.

## Nächste Schritte

- Unterstützung beim Beheben bestimmter Bereitstellungsfehler finden Sie unter [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
- Informationen zur Überwachung anderer Arten von Aktionen anhand der Überwachungsprotokolle finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
- Informationen zum Überprüfen der Bereitstellung vor der Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe mit Azure Resource Manager-Vorlagen](resource-group-template-deploy.md).

<!---HONumber=AcomDC_0622_2016-->