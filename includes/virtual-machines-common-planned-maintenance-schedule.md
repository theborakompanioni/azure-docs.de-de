

## <a name="multi-and-single-instance-vms"></a>Mehrfach- und Einzelinstanz-VMs
Für viele Kunden von Azure ist es entscheidend, dass sie planen können, wann für ihre VMs die geplante Wartung durchgeführt wird. Der Grund ist die Ausfallzeit von ca. 15 Minuten, zu der es bei der Wartung kommt. Sie können Verfügbarkeitsgruppen nutzen, um zu steuern, wann bereitgestellte VMs einer geplanten Wartung unterzogen werden.

Es gibt zwei mögliche Konfigurationen für VMs, die in Azure ausgeführt werden. VMs werden entweder als Mehr- oder Einzelinstanz ausgeführt. Wenn VMs sich in Verfügbarkeitsgruppen befinden, werden sie als Mehrfachinstanz konfiguriert. Beachten Sie, dass auch einzelne VMs in einer Verfügbarkeitsgruppe bereitgestellt werden können, damit sie als Mehrfachinstanz behandelt werden. Wenn VMs sich NICHT in einer Verfügbarkeitsgruppe befinden, werden sie als Einzelinstanz konfiguriert.  Ausführliche Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit Ihrer virtuellen Windows-Computer](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Verwalten der Verfügbarkeit Ihrer virtuellen Linux-Computer](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Geplante Wartungsupdates für Einzelinstanz- und Mehrfachinstanz-VMs treten separat auf. Durch Neukonfiguration Ihrer VMs als Einzelinstanz (wenn sie Mehrfachinstanzen sind) oder Mehrfachinstanz (wenn sie Einzelinstanzen sind) können Sie steuern, wann Ihre VMs der geplanten Wartung unterzogen werden. Ausführliche Informationen zur geplanten Wartung für Azure-VMs finden Sie unter [Geplante Wartung für virtuelle Azure-Computer mit Linux](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Geplante Wartung für virtuelle Azure-Computer mit Windows](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="for-multi-instance-configuration"></a>Für die Mehrfachinstanzkonfiguration
Sie können den Zeitraum auswählen, zu dem die geplante Wartung Ihre VMs beeinflusst, die in einer Verfügbarkeitsgruppenkonfiguration bereitgestellt werden, indem Sie diese VMs aus Verfügbarkeitsgruppen entfernen.

1. Sieben Kalendertage vor der geplanten Wartung Ihrer VMs in einer Mehrfachinstanzkonfiguration erhalten Sie eine E-Mail. Die Abonnement-IDs und Namen der betroffenen Mehrfachinstanz-VMs werden im Text der E-Mail erwähnt.
2. Während dieser sieben Tage können Sie den Zeitraum auswählen, zu dem Ihre Instanzen aktualisiert werden, indem Sie Ihre Mehrfachinstanz-VMs in dieser Region aus ihrer Verfügbarkeitsgruppe entfernen. Diese Änderung in der Konfiguration führt zu einem Neustart, da der virtuelle Computer von einem physischen Host, dem Ziel der Wartung, auf einen anderen physischen Host verschoben wird, der kein Wartungsziel ist.
3. Sie können die VM im Azure-Portal aus ihrer Verfügbarkeitsgruppe entfernen.

   1. Wählen Sie im Portal die VM aus, die aus der Verfügbarkeitsgruppe entfernt werden soll.  

   2. Klicken Sie unter **Einstellungen** auf **Verfügbarkeitsgruppe**.

      ![Verfügbarkeitsgruppenauswahl ](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. Wählen Sie im Dropdownmenü der Verfügbarkeitsgruppe die Option „Nicht Teil einer Verfügbarkeitsgruppe“ aus.

      ![Entfernen aus Gruppe](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Klicken Sie oben auf **Speichern**. Klicken Sie auf **Ja**, um zu bestätigen, dass die VM mit dieser Aktion neu gestartet wird.

   >[!TIP]
   >Sie können die VM später für den Mehrfachinstanzbetrieb neu konfigurieren, indem Sie eine der aufgelisteten Verfügbarkeitsgruppen auswählen.

4. Aus Verfügbarkeitsgruppen entfernte VMs werden auf Einzelinstanzhosts verschoben und während der geplanten Wartung von Verfügbarkeitsgruppenkonfigurationen nicht aktualisiert.
5. Nach Abschluss der Aktualisierung der Verfügbarkeitsgruppen-VMs (gemäß dem in der ursprünglichen E-Mail beschriebenen Zeitplan) sollten Sie die VMs wieder ihren Verfügbarkeitsgruppen hinzufügen. Wenn die VMs Teil einer Verfügbarkeitsgruppe werden, werden sie als Mehrfachinstanz-VMs neu konfiguriert, und es wird ein Neustart durchgeführt. Nachdem alle Mehrfachinstanzupdates für die gesamte Azure-Umgebung abgeschlossen sind, folgt in der Regel die Einzelinstanzwartung.

Sie können zum Entfernen einer VM aus einer Verfügbarkeitsgruppe auch Azure PowerShell verwenden:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Für die Einzelinstanzkonfiguration
Sie können den Zeitraum auswählen, zu dem die geplante Wartung Ihre VMs in einer Einzelinstanzkonfiguration beeinflusst, indem Sie diese VMs Verfügbarkeitsgruppen hinzufügen.

Schrittweise Anleitung

1. Sieben Kalendertage vor der geplanten Wartung Ihrer VMs in einer Einzelinstanzkonfiguration erhalten Sie eine E-Mail. Die Abonnement-IDs und Namen der betroffenen Einzelinstanz-VMs werden im Text der E-Mail erwähnt.
2. Während dieser sieben Tage können Sie den Zeitraum auswählen, in dem Ihre Instanz neu startet, indem Sie Ihre Einzelinstanz-VMs einer Verfügbarkeitsgruppe hinzufügen, die sich in derselben Region befindet. Diese Änderung in der Konfiguration führt zu einem Neustart, da der virtuelle Computer von einem physischen Host, dem Ziel der Wartung, auf einen anderen physischen Host verschoben wird, der kein Wartungsziel ist.
3. Führen Sie die hier angegebenen Schritte aus, um vorhandene VMs mit dem Azure-Portal und Azure PowerShell Verfügbarkeitsgruppen hinzuzufügen. (Siehe Azure PowerShell-Beispiel im Anschluss an diese Schritte.)
4. Sobald diese VMs als Mehrfachinstanzen neu konfiguriert sind, werden sie von der geplanten Wartung für Einzelinstanz-VMs ausgeschlossen.
5. Nachdem das Update der Einzelinstanz-VM abgeschlossen ist (gemäß Zeitplan in der ursprünglichen E-Mail), können Sie die VMs auf den Einzelinstanzbetrieb zurücksetzen, indem Sie sie aus ihren Verfügbarkeitsgruppen entfernen.

Das Hinzufügen einer VM zu einer Verfügbarkeitsgruppe ist auch mit Azure PowerShell möglich:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
