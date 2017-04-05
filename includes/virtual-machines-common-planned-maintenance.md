

## <a name="memory-preserving-updates"></a>Speichererhaltende Updates
Bei einer Klasse von Updates in Microsoft Azure ist für die Kunden keinerlei Beeinträchtigung der ausgeführten virtuellen Computer spürbar. Viele dieser Updates betreffen Komponenten oder Dienste, die ohne Beeinträchtigung der ausgeführten Instanz aktualisiert werden können. Bei einigen dieser Updates handelt es sich um Plattforminfrastrukturaktualisierungen für das Hostbetriebssystem, die ohne vollständigen Neustart des virtuellen Computers angewendet werden können.

Diese Updates werden mit Techniken durchgeführt, die eine direkte Live-Migration ermöglichen (auch als „speichererhaltende“ Aktualisierung bezeichnet). Bei der Aktualisierung wird der virtuelle Computer in einen pausierten Zustand versetzt. Dadurch bleibt der Speicher im Arbeitsspeicher erhalten, während das zugrunde liegende Hostbetriebssystem die erforderlichen Updates und Patches erhält. Der virtuelle Computer wird innerhalb von 30 Sekunden Pause wieder fortgesetzt. Die Uhr des fortgesetzten virtuellen Computers wird automatisch synchronisiert.

Nicht alle Updates können mit diesem Mechanismus bereitgestellt werden. Dank der Kürze der Pausierung verringert diese Art der Updatebereitstellung jedoch deutlich die Auswirkungen auf virtuelle Computer.

Updates für mehrere Instanzen (für virtuelle Computer in einer Verfügbarkeitsgruppe) werden nach und nach auf die einzelnen Updatedomänen angewendet.  

## <a name="virtual-machine-configurations"></a>Konfigurationen für virtuelle Computer
Es gibt zwei Arten der Konfiguration virtueller Computer: mehrere Instanzen und eine Instanz. In einer Konfiguration mit mehreren Instanzen werden ähnliche virtuelle Computer in eine Verfügbarkeitsgruppe platziert.

Die Konfiguration mit mehreren Instanzen sorgt für Redundanz über mehrere physische Computer hinweg, für Leistungskraft und für optimale Vernetzung. Sie wird empfohlen, um die Verfügbarkeit Ihrer Anwendung sicherzustellen. Alle virtuellen Computer in der Verfügbarkeitsgruppe sollten denselben Zweck für Ihre Anwendung erfüllen.

Weitere Informationen zum Konfigurieren Ihrer virtuellen Computer für hohe Verfügbarkeit finden Sie unter [Verwalten der Verfügbarkeit Ihrer virtuellen Windows-Computer](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder unter [Verwalten der Verfügbarkeit Ihrer virtuellen Linux-Computer](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Im Gegensatz dazu wird eine Einzelinstanz-Konfiguration für eigenständige virtuelle Computer verwendet, die nicht in einer Verfügbarkeitsgruppe platziert wurden. Diese virtuellen Computer fallen nicht unter die Vereinbarung zum Servicelevel (SLA). Diese erfordert nämlich, dass mindestens zwei virtuelle Computer in der gleichen Verfügbarkeitsgruppe bereitgestellt werden.

Weitere Informationen zu SLAs finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/) in den Abschnitten „Cloud Services“ und „Virtual Machines“.

## <a name="multi-instance-configuration-updates"></a>Konfigurationsupdates bei mehreren Instanzen
Während der geplanten Wartung aktualisiert die Azure-Plattform zuerst den Satz von virtuellen Computern, die in einer Konfiguration mit mehreren Instanzen gehostet werden. Im Rahmen des Updates wird ein Neustart dieser virtuellen Computer ausgeführt, was mit einer Downtime von etwa 15 Minuten verbunden ist.

Bei einem Konfigurationsupdate mit mehreren Instanzen wird davon ausgegangen, dass jeder virtuelle Computer in der Verfügbarkeitsgruppe eine ähnliche Funktion erfüllt. Mit dieser Einstellung werden virtuelle Computer so aktualisiert, dass die Verfügbarkeit während des Prozesses erhalten bleibt.

Jeder virtuelle Computer in einer Verfügbarkeitsgruppe wird durch die zugrunde liegende Azure-Plattform einer Updatedomäne (UD) und einer Fehlerdomäne (FD) zugewiesen. Jede Updatedomäne ist eine Gruppe virtueller Computer, die im selben Zeitfenster neu gestartet wird. Jede Fehlerdomäne ist eine Gruppe virtueller Computer, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen.


Weitere Informationen zu Updatedomänen und Fehlerdomänen finden Sie unter [Konfigurieren mehrerer virtueller Computer in einer Verfügbarkeitsgruppe für höhere Redundanz](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).

Damit die Verfügbarkeit während eines Updates erhalten bleibt, führt Azure die Wartung nacheinander für die einzelnen Updatedomänen durch. Die Wartung in einer Updatedomäne umfasst Folgendes: Herunterfahren jedes virtuellen Computers in der Domäne, Anwenden des Updates auf die Hostcomputer und Neustarten der virtuellen Computer. Nach Abschluss der Wartung in der Domäne wiederholt Azure den Prozess für die nächste Updatedomäne. Dies wird so lange fortgesetzt, bis alle Domänen aktualisiert sind.

Die Reihenfolge der Updatedomänen, die neu gestartet werden, muss bei der Wartung nicht unbedingt in Folge bearbeitet werden, es befindet sich jedoch nur jeweils eine Updatedomäne im Neustartvorgang. Aktuell wird bei Azure eine Woche vorher über geplante Wartungsarbeiten für virtuelle Computer in einer Konfiguration mit mehreren Instanzen informiert.

Nach der Wiederherstellung eines virtuellen Computers ist in der Windows-Ereignisanzeige beispielsweise folgender Inhalt zu sehen:

<!--Image reference-->
![][image2]


Ermitteln Sie mithilfe der Anzeige die virtuellen Computer, die in einer Konfiguration mit mehreren Instanzen konfiguriert sind (über das Azure-Portal, über Azure PowerShell oder über die Azure-Befehlszeilenschnittstelle). Über das Azure-Portal können Sie beispielsweise die _Verfügbarkeitsgruppe_ dem Dialogfeld **virtuelle Computer (klassisch)** hinzufügen. Die virtuellen Computer mit der gleichen Verfügbarkeitsgruppe sind Teil einer Konfiguration mit mehreren Instanzen. Im folgenden Beispiel besteht die Konfiguration mit mehreren Instanzen aus den virtuellen Computern „SQLContoso01“ und „SQLContoso02“.

<!--Image reference-->
  ![Ansicht „Virtuelle Computer (klassisch)“ im Azure-Portal][image4]

## <a name="single-instance-configuration-updates"></a>Konfigurationsupdates bei einzelnen Instanzen
Nach Abschluss der Updates für Konfigurationen mit mehreren Instanzen führt Azure Updates für Konfigurationen mit einer einzelnen Instanz durch. Bei diesen Updates werden auch virtuelle Computer neu gestartet, die nicht in Verfügbarkeitsgruppen ausgeführt werden.

> [!NOTE]
> Falls in einer Verfügbarkeitsgruppe nur eine einzelne Instanz eines virtuellen Computers ausgeführt wird, wird dies von der Azure-Plattform als Update einer Konfiguration mit mehreren Instanzen behandelt.
>

Die Wartung einer Einzelinstanzkonfiguration umfasst Folgendes: Herunterfahren aller virtuellen Computer auf einem Hostcomputer, Aktualisieren des Hostcomputers und Neustarten der virtuellen Computer. Durch die Wartung entsteht eine Ausfallzeit von etwa 15 Minuten. Das geplante Wartungsereignis wird auf allen virtuellen Computern in einer Region in einem einzelnen Wartungsfenster ausgeführt.


Bei Einzelinstanzkonfigurationen wird die Verfügbarkeit Ihrer Anwendung durch geplante Wartungsereignisse beeinträchtigt. Azure informiert Sie eine Woche im Voraus über geplante Wartungsarbeiten für virtuelle Computer in einer Einzelinstanzkonfiguration.

## <a name="email-notification"></a>E-Mail-Benachrichtigung
Bei geplanten Wartungsarbeiten für virtuelle Computer mit einer Einzelinstanzkonfiguration oder mit einer Konfiguration mit mehreren Instanzen sendet Azure eine Woche im Voraus eine E-Mail-Benachrichtigung. Diese E-Mail wird an die E-Mail-Konten des Abonnementadministrators und des Co-Administrators gesendet. Hier ist ein Beispiel für diese Art von E-Mail:

<!--Image reference-->
![][image1]

## <a name="region-pairs"></a>Regionspaare

Beim Ausführen der Wartung aktualisiert Azure nur die VM-Instanzen in einer Region des jeweiligen Regionspaars. Wenn z. B. die virtuellen Computer (Virtual Machines, VMs) in der Region „USA, Norden-Mitte“ aktualisiert werden, aktualisiert Azure nicht gleichzeitig die virtuellen Computer in der Region „USA, Süden-Mitte“. Die Aktualisierung für diese Region wird zu einem anderen Zeitpunkt geplant. Dies ermöglicht ein Failover oder einen Lastenausgleich zwischen den Regionen. Andere Regionen wie Nordeuropa können jedoch gleichzeitig mit USA (Ost) gewartet werden.

Aktuelle Regionspaare finden Sie in der folgenden Tabelle:

| Region 1 | Region 2 |
|:--- | ---:|
| USA (Ost) |USA (West) |
| USA (Ost) 2 |USA (Mitte) |
| USA Nord Mitte |USA (Mitte/Süden) |
| USA, Westen-Mitte |USA, Westen 2 |
| Kanada, Osten |Kanada, Mitte |
| Brasilien Süd |USA (Mitte/Süden) |
| US Government, Iowa |US Government, Virginia |
| US DoD, Osten |US DoD, Mitte |
| Nordeuropa |Westeuropa |
| UK, Westen |UK, Süden |
| Deutschland, Mitte |Deutschland, Nordosten |
| Südostasien |Ostasien |
| Australien (Südost) |Australien (Ost) |
| Indien, Mitte |Indien, Süden |
| Indien, Westen |Indien, Süden |
| Japan Ost |Japan (Westen) |
| Korea, Mitte |Korea, Süden |
| Ostchina |Nordchina |


<!--Anchors-->
[image1]: ./media/virtual-machines-common-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-common-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-common-planned-maintenance/availabilitysetexample.png


<!--Link references-->
[Virtual Machines Manage Availability]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md

[Understand planned versus unplanned maintenance]: ../articles/virtual-machines/windows/manage-availability.md#Understand-planned-versus-unplanned-maintenance/
