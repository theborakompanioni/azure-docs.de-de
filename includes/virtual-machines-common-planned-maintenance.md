Azure führt regelmäßig Updates durch, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur für virtuelle Computer zu verbessern. Diese Updates reichen von Patches für Softwarekomponenten in der Hostumgebung (wie etwa Betriebssystem, Hypervisor und diverse auf dem Host bereitgestellte Agents) über Upgrades für Netzwerkkomponenten bis hin zur Außerbetriebsetzung von Hardware. Die meisten dieser Updates werden ohne Auswirkungen auf die gehosteten virtuellen Computer durchgeführt. Es gibt jedoch Ausnahmen:

- Wenn die Wartung keinen Neustart erfordert, verwendet Azure eine direkte Migration, um den virtuellen Computer anzuhalten, während der Host aktualisiert wird.

- Wenn die Wartung einen Neustart erfordert, werden Sie in einer Benachrichtigung über den geplanten Wartungstermin informiert. In diesen Fällen erhalten Sie auch ein Zeitfenster, in dem Sie die Wartung zu einem für Sie günstigen Zeitpunkt selbst starten können.

Auf dieser Seite erfahren Sie, wie Microsoft Azure die beiden Wartungsarten durchführt. Weitere Informationen zu ungeplanten Ereignissen (Ausfällen) finden Sie im Artikel zur Verwaltung der Verfügbarkeit virtueller Computer unter [Windows] (../articles/virtual-machines/windows/manage-availability.md) bzw. unter [Linux](../articles/virtual-machines/linux/manage-availability.md).

Auf einem virtuellen Computer ausgeführte Anwendungen können Informationen zu anstehenden Updates mithilfe des Azure-Metadatendiensts für [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) oder [Linux] (../articles/virtual-machines/linux/instance-metadata-service.md) sammeln.

## <a name="in-place-vm-migration"></a>Direkte Migration virtueller Computer

Wenn Updates keinen vollständigen Neustart erfordern, wird eine direkte Livemigration verwendet. Während des Updates wird der virtuelle Computer für etwa 30 Sekunden angehalten. Dabei bleibt der Speicher im Arbeitsspeicher erhalten, während in der Hostumgebung die erforderlichen Updates und Patches angewendet werden. Danach wird die Ausführung des virtuellen Computers fortgesetzt, und die Uhr des virtuellen Computers wird automatisch synchronisiert.

Für virtuelle Computer in Verfügbarkeitsgruppen werden Updatedomänen einzeln nacheinander aktualisiert. Alle virtuellen Computer in einer Updatedomäne (UD) werden angehalten, aktualisiert und fortgesetzt, bevor die geplante Wartung mit der nächsten UD fortfährt.

Einige Anwendungen werden durch Updates dieser Art unter Umständen beeinträchtigt. Anwendungen, die Ereignisse in Echtzeit verarbeiten (beispielsweise Medienstreaming oder Transcodierung), sowie Szenarien mit hohem Netzwerkdurchsatz können unter Umständen keine 30-sekündige Pause tolerieren. <!-- sooooo, what should they do? --> 


## <a name="maintenance-requiring-a-reboot"></a>Wartungsmaßnahmen, die einen Neustart erfordern

Wenn virtuelle Computer für eine geplante Wartung neu gestartet werden müssen, werden Sie darüber im Voraus informiert. Die geplante Wartung hat zwei Phasen: das Self-Service-Zeitfenster und ein Zeitfenster für die geplante Wartung.

Das **Self-Service-Zeitfenster** ermöglicht es Ihnen, die Wartung auf Ihren virtuellen Computern zu initiieren. Innerhalb dieses Zeitfensters können Sie die einzelnen virtuellen Computer abfragen, um ihren Status zu ermitteln und die Ergebnisse Ihrer letzten Wartungsanforderung zu prüfen.

Wenn Sie die Self-Service-Wartung starten, wird Ihr virtueller Computer auf einen bereits aktualisierten Knoten verschoben und wieder hochgefahren. Aufgrund des Neustarts des virtuellen Computers geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert.

Sollte im Rahmen der Self-Service-Wartung ein Fehler auftreten, wird der Vorgang beendet. In diesem Fall wird der virtuelle Computer nicht aktualisiert und aus der Iteration der geplanten Wartung entfernt. Sie werden zu einem späteren Zeitpunkt mit einem neuen Zeitplan kontaktiert und erhalten erneut die Möglichkeit, eine Self-Service-Wartung durchzuführen. 

Nach Ablauf des Self-Service-Zeitfensters beginnt das **Zeitfenster für die geplante Wartung**. Innerhalb dieses Zeitfensters können Sie zwar weiterhin das Wartungszeitfenster abfragen, die Wartung aber nicht mehr selbst starten.

## <a name="availability-considerations-during-planned-maintenance"></a>Überlegungen zur Verfügbarkeit während einer geplanten Wartung 

Wenn Sie bis zum Zeitfenster für die geplante Wartung warten möchten, sollten Sie ein paar Dinge berücksichtigen, um eine möglichst hohe Verfügbarkeit Ihrer virtuellen Computer zu gewährleisten. 

### <a name="paired-regions"></a>Regionspaare

Jeder Azure-Region ist innerhalb der gleichen geografischen Region eine andere Region als Regionspartner zugeordnet. Während einer geplanten Wartung aktualisiert Azure nur die virtuellen Computer in einer einzelnen Region eines Regionspaars. Wenn z. B. die virtuellen Computer (Virtual Machines, VMs) in der Region „USA, Norden-Mitte“ aktualisiert werden, aktualisiert Azure nicht gleichzeitig die virtuellen Computer in der Region „USA, Süden-Mitte“. Andere Regionen wie Nordeuropa können jedoch gleichzeitig mit USA (Ost) gewartet werden. Sie sollten mit der Funktionsweise von Regionspaaren vertraut sein, um Ihre virtuellen Computer besser auf Regionen verteilen zu können. Weitere Informationen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Verfügbarkeitsgruppen und Skalierungsgruppen

Wenn Sie eine Workload auf virtuellen Azure-Computern bereitstellen, können Sie die virtuellen Computer in einer Verfügbarkeitsgruppe erstellen, um eine hohe Verfügbarkeit der Anwendung zu erreichen. Dadurch wird sichergestellt, dass während eines Ausfalls oder Wartungsereignisses mindestens ein virtueller Computer verfügbar ist.

Innerhalb einer Verfügbarkeitsgruppe werden einzelne virtuelle Computer auf bis zu 20 Updatedomänen (UDs) verteilt. Während einer geplanten Wartung ist zum jeweiligen Zeitpunkt immer nur eine Updatedomäne betroffen. Die Reihenfolge der betroffenen Updatedomänen ist jedoch nicht unbedingt sequenziell. 

VM-Skalierungsgruppen sind eine Azure-Computerressource, mit der Sie eine Gruppe identischer virtueller Computer als Einzelressource bereitstellen und verwalten können. Die Skalierungsgruppe wird automatisch über Updatedomänen hinweg bereitgestellt – genau wie virtuelle Computer in einer Verfügbarkeitsgruppe. Und genau wie bei Verfügbarkeitsgruppen ist auch bei Skalierungsgruppen immer nur jeweils eine Updatedomäne betroffen.

Weitere Informationen zum Konfigurieren Ihrer virtuellen Computer für hohe Verfügbarkeit finden Sie im Artikel zur Verwaltung der Verfügbarkeit Ihrer virtuellen Computer für [Windows] (../articles/virtual-machines/windows/manage-availability.md) bzw. für [Linux](../articles/virtual-machines/linux/manage-availability.md).