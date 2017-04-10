---
title: "Häufig gestellte Fragen zu Azure IoT Suite | Microsoft Azure | Microsoft Docs"
description: "Häufig gestellte Fragen zu IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: corywink
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: 8fa08ca6cefc10c1e1b47a7a498f69aca7de882e
ms.lasthandoff: 03/23/2017


---
# <a name="frequently-asked-questions-for-iot-suite"></a>Häufig gestellte Fragen zu IoT Suite

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Wo kann ich den Quellcode für die vorkonfigurierte Lösung finden?
Der Quellcode wird in den folgenden GitHub-Repositorys gespeichert:
* [Azure IoT Remote Monitoring preconfigured solution][lnk-remote-monitoring-github] (Vorkonfigurierte Lösung zur Remoteüberwachung in Azure IoT)
* [Azure IoT Predictive Maintenance preconfigured solution][lnk-predictive-maintenance-github] (Vorkonfigurierte Lösung zur vorausschauenden Wartung in Azure IoT)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a>Wie aktualisiere ich auf die neueste Version der vorkonfigurierten Lösung zur Remoteüberwachung, die die IoT Hub Geräteverwaltungsfeatures verwendet?
* Wenn Sie eine vorkonfigurierte Lösung von der Website „https://www.azureiotsuite.com/“ bereitstellen, wird immer eine neue Instanz der neuesten Version der Lösung bereitgestellt.
* Wenn Sie eine vorkonfigurierte Lösung über die Befehlszeile bereitstellen, können Sie eine vorhandene Bereitstellung mit neuem Code aktualisieren. Siehe [Cloud deployment][lnk-cloud-deployment] (Cloudbereitstellung) im GitHub-[Repository][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a>Wie kann ich der vorkonfigurierten Lösung zur Remoteüberwachung Unterstützung für eine neue Gerätemethode hinzufügen?
Siehe Abschnitt [Hinzufügen von Unterstützung für eine neue Methode zum Simulator][lnk-add-method] im Artikel [Anpassen vorkonfigurierter Lösungen][lnk-customize].

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a>Das simulierte Gerät ignoriert meine gewünschten Eigenschaftsänderungen – warum?
In der vorkonfigurierten Lösung zur Remoteüberwachung verwendet der Code des simulierten Geräts nur die gewünschten Eigenschaften **Desired.Config.TemperatureMeanValue** und **Desired.Config.TelemetryInterval** zum Aktualisieren der gemeldeten Eigenschaften. Alle anderen gewünschten Eigenschaftenänderungen werden ignoriert.

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a>Mein Gerät wird nicht in der Geräteliste im Lösungsdashboard angezeigt – warum?
Die Liste der Geräte im Lösungsdashboard verwendet eine Abfrage, um die Liste der Geräte zurückzugeben. Derzeit kann eine Abfrage nicht mehr als 10.000 Geräte zurückgeben. Versuchen Sie, die Suchkriterien für die Abfrage einzuschränken.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Was ist der Unterschied zwischen dem Löschen einer Ressourcengruppe im Azure-Portal und dem Klicken auf "Löschen" für eine vorkonfigurierte Lösung in "azureiotsuite.com"?
* Wenn Sie in [azureiotsuite.com][lnk-azureiotsuite] eine vorkonfigurierte Lösung löschen, werden alle Ressourcen gelöscht, die beim Erstellen der vorkonfigurierten Lösung bereitgestellt waren. Wenn Sie dieser Ressourcengruppe weitere Ressourcen hinzugefügt haben, werden diese ebenfalls gelöscht. 
* Wenn Sie die Ressourcengruppe im [Azure-Portal][lnk-azure-portal] löschen, werden nur die Ressourcen in dieser Ressourcengruppe gelöscht. Sie müssen auch die zugeordnete Azure Active Directory-Anwendung löschen, die der vorkonfigurierten Lösung im [klassischen Azure-Portal][lnk-classic-portal] zugeordnet ist.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Wie viele IoT Hub-Instanzen kann ich in einem Abonnement bereitstellen?
Standardmäßig können Sie [10 IoT Hubs pro Abonnement][link-azuresublimits] bereitstellen. Sie können ein [Azure-Supportticket][link-azuresupportticket] erstellen, um dieses Limit zu erhöhen. Da jede vorkonfigurierte Lösung einen neuen IoT Hub bereitstellt, können Sie somit in einem Abonnement nur bis zu zehn vorkonfigurierte Lösungen bereitstellen. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Wie viele Instanzen von DocumentDB kann ich in einem Abonnement bereitstellen?
50. Sie können ein [Azure-Supportticket][link-azuresupportticket] erstellen, um dieses Limit zu erhöhen. Standardmäßig können Sie jedoch nur 50 DocumentDB-Instanzen pro Abonnement bereitstellen. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele Bing Maps-APIs im Tarif „Free“ kann ich in einem Abonnement bereitstellen?
Zwei. In einem Azure-Abonnement können für Bing Karten für Unternehmen nur zwei Tarife für die erste interne Transaktionsebene erstellt werden. Die Remoteüberwachungslösung wird standardmäßig mit dem Tarif für die erste interne Transaktionsebene bereitgestellt. Daher können Sie in einem Abonnement ohne Modifikationen nur bis zu zwei vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Ich habe eine Lösungsbereitstellung zur Remoteüberwachung mit einer statischen Karte. Wie kann ich eine interaktive Bing-Karte hinzufügen?
1. Rufen Sie Ihren Bing Maps-API for Enterprise QueryKey aus dem [Azure-Portal][lnk-azure-portal] ab: 
   
   1. Navigieren Sie zu der Ressourcengruppe, in der sich Bing Maps API for Enterprise im [Azure-Portal][lnk-azure-portal] befindet.
   2. Klicken Sie auf **Alle Einstellungen** und dann auf **Schlüsselverwaltung**. 
   3. Sie sehen zwei Schlüssel: **MasterKey** und **QueryKey**. Kopieren Sie den Wert für **QueryKey**.
      
      > [!NOTE]
      > Sie haben kein Bing Maps API for Enterprise-Konto? Erstellen Sie ein Konto im [Azure-Portal][lnk-azure-portal], indem Sie auf „+ Neu“ klicken, nach Bing Maps API for Enterprise suchen und die Anweisungen für die Erstellung befolgen.
      > 
      > 
2. Ziehen Sie den aktuellen Code aus [Azure IoT Remote Monitoring][lnk-remote-monitoring-github] nach unten.
3. Führen Sie eine lokale oder Cloud-Bereitstellung aus, indem Sie die Anleitung zur Befehlszeilenbereitstellung im Ordner „/docs/“ im Repository befolgen. 
4. Wenn Sie eine lokale oder Cloud-Bereitstellung ausgeführt haben, suchen Sie im Stammordner nach der Datei „*.user.config“, die während der Bereitstellung erstellt wurde. Öffnen Sie diese Datei in einem Texteditor. 
5. Ändern Sie die folgende Zeile, sodass sie den Wert enthält, den Sie von Ihrem **QueryKey** kopiert haben: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich Microsoft Azure für DreamSpark besitze?
Zu diesem Zeitpunkt können Sie keine vorkonfigurierte Lösung mit einem Konto für [Microsoft Azure für DreamSpark][lnk-dreamspark] erstellen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure][lnk-30daytrial] einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfüge?
Derzeit können Sie keine vorkonfigurierte Lösung erstellen, wenn Sie über ein CSP-Abonnement (Cloud Solution Provider, Cloudlösungsanbieter) verfügen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure][lnk-30daytrial] einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### <a name="how-do-i-delete-an-aad-tenant"></a>Wie lösche ich einen AAD-Mandanten?
Informationen hierzu finden Sie im Blogbeitrag von Eric Golpe: [Exemplarische Vorgehensweise zum Löschen eines Azure AD-Mandanten][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Nächste Schritte
Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Übersicht über die vorkonfigurierte Lösung für vorhersagbaren Wartungsbedarf][lnk-predictive-overview]
* [Sicherheit im Internet der Dinge von Anfang an][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
