---
title: "Allgemeine Verwaltungsaufgaben für Clouddienste (klassisch) | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über die Verwaltung von Clouddiensten im klassischen Azure-Vorschauportal."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: c2a9a14891f197ae442c41668229d4a7610ba248
ms.openlocfilehash: 17270815b6278ec6b36a134c313cd543622dd9f4


---
# <a name="how-to-manage-cloud-services"></a>Verwalten von Clouddiensten
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-how-to-manage-portal.md)
> * [Klassisches Azure-Portal](cloud-services-how-to-manage.md)
>
>

Im Bereich **Cloud Services** des klassischen Azure-Portals können Sie eine Dienstrolle oder eine Bereitstellung aktualisieren, eine Bereitstellung zur Produktion heraufstufen, Ressourcen mit Ihrem Clouddienst verknüpfen, sodass Sie die Ressourcenabhängigkeiten sehen und die Ressourcen zusammen skalieren können, und einen Clouddienst oder eine Bereitstellung löschen.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Aktualisieren einer Clouddienstrolle oder -bereitstellung
Wenn Sie den Anwendungscode für Ihren Clouddienst aktualisieren müssen, verwenden Sie **Aktualisieren** auf dem Dashboard, auf der Seite **Cloud Services** oder auf der Seite **Instanzen**. Sie können eine oder alle Rollen aktualisieren. Sie müssen ein neues Dienstpaket und eine neue Dienstkonfigurationsdatei hochladen.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf dem Dashboard auf der Seite **Cloud Services** oder auf der Seite **Instanzen** auf **Aktualisieren**.

    ![Bereitstellung aktualisieren](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. Geben Sie in **Bereitstellungsbezeichnung** einen Namen ein, um die Bereitstellung zu kennzeichnen (z.B. „meinclouddienstv4“). Sie finden den Bereitstellungsnamen auf dem Dashboard unter **Schnellstart**.
3. Klicken Sie unter **Paket** auf **Durchsuchen**, um die Dienstpaketdatei (.cspkg) hochzuladen.
4. Klicken Sie unter **Konfiguration** auf **Durchsuchen**, um die Dienstkonfigurationsdatei (.cscfg) hochzuladen.
5. Wählen Sie unter **Rolle** die Option **Alle**, wenn Sie alle Rollen im Clouddienst aktualisieren möchten. Wählen Sie zum Ausführen einer einzelnen Rollenaktualisierung die zu aktualisierende Rolle aus. Auch wenn Sie eine bestimmte Rolle zum Aktualisieren auswählen, werden die Updates in der Dienstkonfigurationsdatei auf alle Rollen angewendet.
6. Wenn durch das Update die Anzahl der Rollen oder die Größe einer Rolle geändert wird, aktivieren Sie das Kontrollkästchen **Allow update if role sizes or number of roles changes** , um die Fortsetzung des Updates zu ermöglichen.

    Wenn Sie die Größe einer Rolle (d. h. die Größe eines virtuellen Computers, der eine Rolleninstanz hostet) oder die Anzahl der Rollen ändern, denken Sie daran, dass für jede Rolleninstanz (virtueller Computer) ein neues Image erstellt werden muss und dass lokale Daten verloren gehen.

7. Wenn Dienstrollen nur eine Rolleninstanz haben, aktivieren Sie das Kontrollkästchen **Auch dann aktualisieren, wenn für eine oder mehrere Rollen nur eine Instanz vorhanden ist** , damit das Update fortgesetzt werden kann.

    Azure kann während des Updates eines Clouddiensts nur dann eine Dienstverfügbarkeit von 99,95 Prozent garantieren, wenn jede Rolle mindestens zwei Rolleninstanzen (virtuelle Computer) hat. In diesem Fall kann ein virtueller Computer Clientanforderungen verarbeiten, während der andere aktualisiert wird.

8. Klicken Sie auf **OK** (Häkchen), um mit dem Update des Diensts zu beginnen.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Austauschen von Bereitstellungen zum Heraufstufen einer Bereitstellung zur Produktion
Verwenden Sie **Austauschen** , um eine Stagingbereitstellung eines Cloud-Diensts zur Produktion heraufzustufen. Wenn Sie beschließen, eine neue Version eines Cloud-Diensts bereitzustellen, können Sie die neue Version in der Cloud-Dienst-Stagingumgebung bereitstellen und testen, während Ihre Kunden die aktuelle Version in der Produktion einsetzen. Wenn Sie bereit sind, die neue Version zur Produktion heraufzustufen, können Sie mit **Austauschen** die URLs austauschen, mit denen die beiden Bereitstellungen adressiert werden.

Sie können Bereitstellungen über die Seite **Cloud-Dienste** oder über das Dashboard austauschen.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Cloud Services**.
2. Klicken Sie in der Liste der Cloud-Dienste auf den Cloud-Dienst, um ihn auszuwählen.
3. Klicken Sie auf **Austauschen**.

    Die folgende Bestätigungsaufforderung wird geöffnet.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Klicken Sie nach der Verifizierung der Bereitstellungsinformationen auf **Ja** , um die Bereitstellungen auszutauschen.

    Der Austausch der Bereitstellungen erfolgt schnell, da sich nur die virtuellen IP-Adressen für die Bereitstellungen ändern.

    Um Rechenkosten zu sparen, können Sie die Bereitstellung in der Stagingumgebung löschen, wenn Sie sicher sind, dass die neue Produktionsbereitstellung wie erwartet funktioniert.

### <a name="common-questions-about-swapping-deployments"></a>Häufig gestellte Fragen zum Austauschen der Bereitstellungen

**Was sind die Voraussetzungen zum Austauschen von Bereitstellungen?**

Es gibt zwei wichtige Voraussetzungen für einen erfolgreichen Austausch von Bereitstellungen:

- Wenn Sie eine statische IP-Adresse für Ihren Produktionsslot verwenden möchten, müssen Sie auch eine für den Stagingslot reservieren. Andernfalls tritt bei dem Austausch ein Fehler auf.

- Alle Instanzen Ihrer Rollen müssen ausgeführt werden, bevor Sie den Austausch durchführen können. Sie können den Status Ihrer Instanzen im klassischen Azure-Portal oder mit dem [Befehl Get-AzureRole in Windows PowerShell](https://docs.microsoft.com/en-us/powershell/servicemanagement/azure.service/v3.1.0/get-azurerole) überprüfen.

Beachten Sie, dass Updates von Gastbetriebssystemen und Dienstreparaturvorgänge auch dazu führen können, dass beim Austausch von Bereitstellungen ein Fehler auftritt. Weitere Informationen finden Sie unter [Behandeln von Problemen mit der Clouddienstbereitstellung](cloud-services-troubleshoot-deployment-problems.md).

**Führt ein Austausch zu einer Ausfallzeit für die Anwendung? Wie sollte ich dabei vorgehen?**

Wie im vorherigen Abschnitt beschrieben, erfolgt der Austausch von Bereitstellungen in der Regel sehr schnell, da es sich nur um eine Konfigurationsänderung im Azure Load Balancer handelt. In einigen Fällen kann er jedoch zehn oder mehr Sekunden dauern und zu vorübergehenden Verbindungsausfällen führen. Um die Auswirkungen auf Ihre Kunden zu minimieren, ist es empfehlenswert, [Clientwiederholungslogik](../best-practices-retry-general.md) zu implementieren.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Verknüpfen einer Ressource mit einem Clouddienst
Um die Abhängigkeiten des Cloud-Diensts von anderen Ressourcen anzuzeigen, können Sie eine Azure-SQL-Datenbankinstanz oder ein Speicherkonto mit dem Cloud-Dienst verknüpfen. Auf der Seite **Verknüpfte Ressourcen** können Sie Ressourcen verknüpfen und Verknüpfungen aufheben und dann deren Verwendung im Clouddienst-Dashboard überwachen. Wenn für ein verknüpftes Speicherkonto die Überwachung aktiviert ist, können Sie die gesamten Anforderungen im Cloud-Dienst-Dashboard überwachen.

Verwenden Sie **Verknüpfen**, um eine neue oder vorhandene SQL-Datenbankinstanz oder ein Speicherkonto mit Ihrem Clouddienst zu verknüpfen. Anschließend können Sie die Datenbank sowie die Clouddienstrolle, die diese verwendet, auf der Seite **Skalieren** skalieren. (Ein Speicherkonto wird mit zunehmender Nutzung automatisch skaliert.) Weitere Informationen finden Sie unter [Automatisches Skalieren eines Clouddiensts](cloud-services-how-to-scale.md).

Sie können die Datenbank außerdem im Knoten **Datenbanken** des klassischen Azure-Portals überwachen, verwalten und skalieren.

Das "Verknüpfen" einer Ressource bedeutet in diesem Fall nicht, dass Ihre App mit der Ressource verbunden wird. Wenn Sie mithilfe von **Verknüpfen**eine neue Datenbank erstellen, müssen Sie dem Anwendungscode die Verbindungszeichenfolgen hinzufügen und den Cloud-Dienst anschließend aktualisieren. Verbindungszeichenfolgen müssen Sie auch hinzufügen, wenn Ihre App Ressourcen in einem verknüpften Speicherkonto verwendet.

Im Folgenden wird beschrieben, wie eine neue SQL-Datenbankinstanz, die auf einem neuen SQL-Datenbankserver bereitgestellt ist, mit einem Cloud-Dienst verknüpft wird.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Verknüpfen einer SQL-Datenbankinstanz mit einem Cloud-Dienst
1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Cloud Services**. Klicken Sie anschließend auf den Namen des Cloud-Diensts, um das Dashboard zu öffnen.
2. Klicken Sie auf **Linked Resources**.

    Die Seite **Linked Resources** wird geöffnet.

    ![Seite "Linked Resources"](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klicken Sie auf **Eine Ressource verknüpfen** oder auf **Verknüpfen**.

    Der Assistent **Link Resource** wird gestartet.

    ![Verknüpfen, Seite&1;](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klicken Sie auf **Neue Ressource erstellen** oder auf **Vorhandene Ressource verknüpfen**.
5. Wählen Sie den Typ der zu verknüpfenden Ressource. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **SQL-Datenbank**. (Das klassische Azure-Portal unterstützt nicht das Verknüpfen eines Speicherkontos mit einem Cloud-Dienst.)
6. Um die Datenbankkonfiguration abzuschließen, folgen Sie den Anweisungen in der Hilfe für den Bereich **SQL-Datenbanken** im klassischen Azure-Portal.

    Sie können den Fortschritt der Verknüpfungsoperation im Nachrichtenbereich verfolgen.

    Wenn das Verknüpfen abgeschlossen ist, können Sie den Status der verknüpften Ressource im Cloud-Dienst-Dashboard überwachen. Informationen zum Skalieren einer verknüpften SQL-Datenbank finden Sie unter [Skalieren eines Clouddiensts und verknüpfter Ressourcen](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Aufheben der Verknüpfung einer verknüpften Ressource
1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Cloud Services**. Klicken Sie anschließend auf den Namen des Cloud-Diensts, um das Dashboard zu öffnen.
2. Klicken Sie auf **Linked Resources**, und wählen Sie die Ressource anschließend aus.
3. Klicken Sie auf **Unlink**. Klicken Sie dann an der Bestätigungsaufforderung auf **Ja** .

    Das Aufheben der Verknüpfung einer SQL-Datenbank hat keine Auswirkungen auf die Datenbank oder die Verbindungen der Anwendung mit der Datenbank. Sie können die Datenbank weiterhin im Bereich **SQL-Datenbanken** des klassischen Azure-Portals verwalten.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Löschen von Bereitstellungen und eines Clouddiensts
Bevor Sie einen Cloud-Dienst löschen können, müssen Sie die einzelnen bestehenden Bereitstellungen löschen.

Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert. Rechenkosten für Rolleninstanzen fallen auch dann an, wenn ein Cloud-Dienst nicht ausgeführt wird.

Gehen Sie folgendermaßen vor, um eine Bereitstellung oder Ihren Cloud-Dienst zu löschen.

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Cloud Services**.
2. Wählen Sie den Cloud-Dienst aus, und klicken Sie dann auf **Löschen**. (Um einen Cloud-Dienst auszuwählen, ohne das Dashboard zu öffnen, klicken Sie im Cloud-Diensteintrag auf eine beliebige Stelle außerhalb des Namens.)

    Wenn Sie eine Bereitstellung in Staging oder Produktion haben, wird unten im Fenster ein Auswahlmenü ähnlich dem folgenden angezeigt. Bevor Sie den Cloud-Dienst löschen können, müssen Sie bestehende Bereitstellungen löschen.

    ![Menü "Löschen"](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. Klicken Sie zum Löschen einer Bereitstellung auf **Delete production deployment** (Produktionsbereitstellung löschen) oder auf **Delete staging deployment** (Stagingbereitstellung löschen). Klicken Sie dann an der Bestätigungsaufforderung auf **Ja**.
4. Wenn Sie den Cloud-Dienst löschen möchten, wiederholen Sie Schritt 3 bei Bedarf, um die andere Bereitstellung zu löschen.
5. Klicken Sie zum Löschen des Cloud-Diensts auf **Cloud-Dienst löschen**. Klicken Sie dann an der Bestätigungsaufforderung auf **Ja**.

> [!NOTE]
> Wenn für den Cloud-Dienst die ausführliche Überwachung konfiguriert ist, löscht Azure die Überwachungsdaten aus Ihrem Speicherkonto nicht, wenn Sie den Cloud-Dienst löschen. Sie müssen die Daten manuell löschen. Informationen zum Speicherort der Metriktabellen finden Sie unter „Zugreifen auf ausführliche Überwachungsdaten außerhalb des klassischen Azure-Portals“ in [Überwachen von Cloud Services](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Nächste Schritte
* [Allgemeine Konfiguration Ihres Clouddiensts](cloud-services-how-to-configure.md)
* Weitere Informationen zum [Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name.md)
* Konfigurieren von [SSL-Zertifikaten](cloud-services-configure-ssl-certificate.md)



<!--HONumber=Jan17_HO1-->


