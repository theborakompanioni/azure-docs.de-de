<properties
  pageTitle="Häufig gestellte Fragen zu Azure IoT Suite | Microsoft Azure | Microsoft Azure"
  description="Häufig gestellte Fragen zu IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="05/09/2016"
  ms.author="araguila"/>
   
# Häufig gestellte Fragen zu IoT Suite

### Was ist der Unterschied zwischen dem Löschen einer Ressourcengruppe im Azure-Portal und dem Klicken auf "Löschen" für eine vorkonfigurierte Lösung in "azureiotsuite.com"?

- Wenn Sie in [azureiotsuite.com][lnk-azureiotsuite] eine vorkonfigurierte Lösung löschen, werden alle Ressourcen gelöscht, die bereitgestellt waren, als Sie die vorkonfigurierte Lösung erstellt haben. Wenn Sie dieser Ressourcengruppe weitere Ressourcen hinzugefügt haben, werden diese ebenfalls gelöscht. 

- Wenn Sie die Ressourcengruppe im [Azure-Portal][lnk-azure-portal] löschen, werden nur die Ressourcen in dieser Ressourcengruppe gelöscht. Sie müssen im klassischen [Azure-Portal][lnk-classic-portal] außerdem die Azure Active Directory-Anwendung löschen, die der vorkonfigurierten Lösung zugeordnet ist.

### Wie viele Instanzen von DocumentDB kann ich in einem Abonnement bereitstellen?

Fünf. Sie können ein [Azure-Support-Ticket][link-azuresupportticket] erstellen, um dieses Limit zu erhöhen. Doch standardmäßig können Sie nur fünf DocumentDB-Instanzen pro Abonnement bereitstellen. Daher können Sie in einem bestimmten Abonnement nur bis zu fünf vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### Wie viele Bing Maps-APIs im Tarif „Free“ kann ich in einem Abonnement bereitstellen?

Zwei. Sie können nur zwei Bing Maps-APIs im Tarif „Free“ in einem Abonnement erstellen. Die Remoteüberwachungslösung wird standardmäßig mit einer Bing Maps-API im Tarif „Free“ bereitgestellt. Daher können Sie in einem Abonnement ohne Modifikationen nur bis zu zwei vorkonfigurierte Remoteüberwachungslösungen bereitstellen.

### Ich habe eine Lösungsbereitstellung zur Remoteüberwachung mit einer statischen Karte. Wie kann ich eine interaktive Bing-Karte hinzufügen? 
1. Rufen Sie Ihren Bing Maps-API for Enterprise QueryKey aus dem [Azure-Portal][lnk-azure-portal] ab: 
 1. Navigieren Sie zur Ressourcengruppe, in der Sich Bing Maps API for Enterprise im [Azure-Portal][lnk-azure-portal] befindet.
 2. Klicken Sie auf „Alle Einstellungen“ und anschließend auf „Schlüsselverwaltung“. 
 3. Sie sehen zwei Schlüssel: MasterKey und QueryKey. Kopieren Sie den Wert für QueryKey.

     > [AZURE.NOTE] Sie haben kein Bing Maps API for Enterprise-Konto? Erstellen Sie ein Konto im [Azure-Portal][lnk-azure-portal], indem Sie auf „+ Neu“ klicken, nach Bing Maps API for Enterprise suchen, und die Anweisungen für die Erstellung befolgen.

2. Ziehen Sie den aktuellen Code aus [Azure IoT Remote Monitoring][lnk-remote-monitoring-github] nach unten.

3. Führen Sie eine lokale oder Cloud-Bereitstellung aus, indem Sie die Anleitung zur Befehlszeilenbereitstellung im Ordner /docs/ im Repository befolgen.

4. Wenn Sie eine lokale oder Cloud-Bereitstellung ausgeführt haben, suchen Sie im Stammordner nach der Datei „*.user.config“, die während der Bereitstellung erstellt wurde. Öffnen Sie diese Datei in einem Texteditor.

5. Ändern Sie die folgende Zeile, sodass Sie den Wert enthält, den Sie für Ihren QueryKey kopiert haben:
   
  `<setting name="MapApiQueryKey" value="" />`

### Kann ich eine vorkonfigurierte Lösung erstellen, wenn ich Microsoft Azure für DreamSpark besitze?
Zu diesem Zeitpunkt können Sie keine vorkonfigurierte Lösung mit einem Konto für [Microsoft Azure für DreamSpark][lnk-dreamspark] erstellen. Sie können jedoch in wenigen Minuten ein [kostenloses Testkonto für Azure][lnk-30daytrial] einrichten, mit dem Sie eine vorkonfigurierte Lösung erstellen können.

### Wie lösche ich einen AAD-Mandanten?

Siehe den Blogbeitrag von Eric Golpe [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx

<!---HONumber=AcomDC_0511_2016-->