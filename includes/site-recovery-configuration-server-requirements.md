| **Hardware** | |
| --- |---|
| Anzahl von CPU-Kernen| 8 Kerne |
| RAM| 12 GB|
| Anzahl von Datenträgern | **3 Datenträger** <br> - Betriebssystemdatenträger<br> - Prozessservercache-Datenträger<br> - Aufbewahrungslaufwerk (für Failback)|
| Freier Speicherplatz (Prozessservercache) | 600 GB
| Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB|
| **Software** | |
| Betriebssystemversion | Windows Server 2012 R2 |
| Gebietsschema des Betriebssystems | Englisch (en-us)|
| VMware vSphere PowerCLI-Version | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Windows Server-Rollen | Aktivieren Sie die folgenden Rollen nicht: <br> **Active Directory Domain Services** <br>**Internetinformationsdienste** <br> **Hyper-V** |
| **Netzwerk** | |
| Typ der Netzwerkschnittstellenkarte | VMXNET3 |
| IP-Adresstyp | Statisch |
| Zugriff auf das Internet | Für den Server sollte es möglich sein, entweder direkt oder über einen Proxyserver auf die folgende URL zuzugreifen. <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi <br> - time.nist.gov <br> - time.windows.com |
| Ports | 443 (Steuerkanalorchestrierung)<br>9443 (Datentransport)|


<!--HONumber=Jan17_HO3-->


