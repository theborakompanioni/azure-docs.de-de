<properties
   pageTitle="SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch | Microsoft Azure"
   description="SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="juergent"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch

[767598]: https://service.sap.com/sap/support/notes/767598
[773830]: https://service.sap.com/sap/support/notes/773830
[826037]: https://service.sap.com/sap/support/notes/826037
[965908]: https://service.sap.com/sap/support/notes/965908
[1031096]: https://service.sap.com/sap/support/notes/1031096
[1139904]: https://service.sap.com/sap/support/notes/1139904
[1173395]: https://service.sap.com/sap/support/notes/1173395
[1245200]: https://service.sap.com/sap/support/notes/1245200
[1409604]: https://service.sap.com/sap/support/notes/1409604
[1558958]: https://service.sap.com/sap/support/notes/1558958
[1585981]: https://service.sap.com/sap/support/notes/1585981
[1588316]: https://service.sap.com/sap/support/notes/1588316
[1590719]: https://service.sap.com/sap/support/notes/1590719
[1597355]: https://service.sap.com/sap/support/notes/1597355
[1605680]: https://service.sap.com/sap/support/notes/1605680
[1619720]: https://service.sap.com/sap/support/notes/1619720
[1619726]: https://service.sap.com/sap/support/notes/1619726
[1619967]: https://service.sap.com/sap/support/notes/1619967
[1750510]: https://service.sap.com/sap/support/notes/1750510
[1752266]: https://service.sap.com/sap/support/notes/1752266
[1757924]: https://service.sap.com/sap/support/notes/1757924
[1757928]: https://service.sap.com/sap/support/notes/1757928
[1758182]: https://service.sap.com/sap/support/notes/1758182
[1758496]: https://service.sap.com/sap/support/notes/1758496
[1772688]: https://service.sap.com/sap/support/notes/1772688
[1814258]: https://service.sap.com/sap/support/notes/1814258
[1882376]: https://service.sap.com/sap/support/notes/1882376
[1909114]: https://service.sap.com/sap/support/notes/1909114
[1922555]: https://service.sap.com/sap/support/notes/1922555
[1928533]: https://service.sap.com/sap/support/notes/1928533
[1941500]: https://service.sap.com/sap/support/notes/1941500
[1956005]: https://service.sap.com/sap/support/notes/1956005
[1973241]: https://service.sap.com/sap/support/notes/1973241
[1984787]: https://service.sap.com/sap/support/notes/1984787
[1999351]: https://service.sap.com/sap/support/notes/1999351
[2002167]: https://service.sap.com/sap/support/notes/2002167
[2015553]: https://service.sap.com/sap/support/notes/2015553
[2039619]: https://service.sap.com/sap/support/notes/2039619
[2121797]: https://service.sap.com/sap/support/notes/2121797
[2134316]: https://service.sap.com/sap/support/notes/2134316
[2178632]: https://service.sap.com/sap/support/notes/2178632
[2191498]: https://service.sap.com/sap/support/notes/2191498
[2233094]: https://service.sap.com/sap/support/notes/2233094
[2243692]: https://service.sap.com/sap/support/notes/2243692

[azure-cli]: ../xplat-cli-install.md
[azure-portal]: https://portal.azure.com
[azure-ps]: ../powershell-install-configure.md
[azure-quickstart-templates-github]: https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]: https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]: ../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]: ../azure-subscription-service-limits.md#subscription

[dbms-guide]: virtual-machines-windows-sap-dbms-guide.md "SAP NetWeaver auf virtuellen Windows-Computern (VMs) – DBMS-Bereitstellungshandbuch"
[dbms-guide-2.1]: virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f "Caching für VMs und VHDs"
[dbms-guide-2.2]: virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 "Software-RAID"
[dbms-guide-2.3]: virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 "Microsoft Azure Storage"
[dbms-guide-2]: virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 "Struktur einer RDBMS-Bereitstellung"
[dbms-guide-3]: virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 "Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern"
[dbms-guide-5.5.1]: virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 "SQL Server 2012 SP1 CU4 und höher"
[dbms-guide-5.5.2]: virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b "SQL Server 2012 SP1 CU3 und frühere Versionen"
[dbms-guide-5.6]: virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 "Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace"
[dbms-guide-5.8]: virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 "SQL Server für SAP in Azure – Allgemeine Zusammenfassung"
[dbms-guide-5]: virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 "Besonderheiten bei SQL Server-RDBMS"
[dbms-guide-8.4.1]: virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 "Speicherkonfiguration"
[dbms-guide-8.4.2]: virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d "Sichern und Wiederherstellen"
[dbms-guide-8.4.3]: virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c "Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung"
[dbms-guide-8.4.4]: virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 "Andere"
[dbms-guide-900-sap-cache-server-on-premises]: virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]: ./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]: ./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]: ./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]: ./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]: ./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]: ./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]: ./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]: ./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]: ./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]: virtual-machines-windows-sap-deployment-guide.md "SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Bereitstellungshandbuch"
[deployment-guide-2.2]: virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 "SAP-Ressourcen"
[deployment-guide-3.1.2]: virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab "Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image"
[deployment-guide-3.2]: virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 "Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP"
[deployment-guide-3.3]: virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 "Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP"
[deployment-guide-3.4]: virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 "Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP"
[deployment-guide-3]: virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e "Bereitstellungsszenarios für virtuelle Computer für SAP in Microsoft Azure"
[deployment-guide-4.1]: virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 "Bereitstellen von Azure PowerShell-Cmdlets"
[deployment-guide-4.2]: virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e "Herunterladen und Importieren von SAP-relevanten PowerShell-Cmdlets"
[deployment-guide-4.3]: virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc "Einbinden virtueller Computer in die lokale Domäne – nur Windows"
[deployment-guide-4.4.2]: virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 "Linux"
[deployment-guide-4.4]: virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d "Herunterladen, Installieren und Aktivieren von Azure VM-Agent"
[deployment-guide-4.5.1]: virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 "Azure PowerShell"
[deployment-guide-4.5.2]: virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f "Azure-Befehlszeilenschnittstelle"
[deployment-guide-4.5]: virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca "Konfigurieren der erweiterten Azure-Überwachung für SAP"
[deployment-guide-5.1]: virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 "Bereitschaftsüberprüfung für die erweiterte Azure-Überwachung für SAP"
[deployment-guide-5.2]: virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 "Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur"
[deployment-guide-5.3]: virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 "Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP"

[deployment-guide-configure-monitoring-scenario-1]: virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b "Konfigurieren der Überwachung"
[deployment-guide-configure-proxy]: virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d "Konfigurieren von Proxys"
[deployment-guide-figure-100]: ./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]: ./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]: virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]: ./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]: ./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]: ./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]: virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]: ./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]: ./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]: ./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]: virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]: ./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]: ./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]: virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]: ./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]: virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]: ./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]: ./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]: ./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]: virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]: virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]: virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]: virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b "Überprüfungen und Problembehandlung für die Einrichtung der End-to-End-Überwachung für SAP in Azure"

[deploy-template-cli]: ../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]: ../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]: ../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]: http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]: virtual-machines-windows-sap-get-started.md
[getting-started-dbms]: virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]: virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]: virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]: virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]: virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]: virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]: virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]: virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]: virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]: http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]: virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]: virtual-machines-linux-enable-aem.md

[Logo_Linux]: ./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]: ./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]: https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]: virtual-machines-windows-sap-planning-guide.md "SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch"
[planning-guide-1.2]: virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff "Ressourcen"
[planning-guide-11.4]: virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 "Hohe Verfügbarkeit (HA) und Disaster Recovery (DR) für SAP NetWeaver auf virtuellen Azure-Computern"
[planning-guide-11.4.1]: virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 "Hohe Verfügbarkeit für SAP-Anwendungsserver"
[planning-guide-11.5]: virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f "Verwenden von Autostart für SAP-Instanzen"
[planning-guide-2.1]: virtual-machines-windows-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 "Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk"
[planning-guide-2.2]: virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 "Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk"
[planning-guide-3.1]: virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a "Azure-Regionen"
[planning-guide-3.2.1]: virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 "Fehlerdomänen"
[planning-guide-3.2.2]: virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 "Upgradedomänen"
[planning-guide-3.2.3]: virtual-machines-windows-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 "Azure-Verfügbarkeitsgruppen"
[planning-guide-3.2]: virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 "Das Konzept der virtuellen Microsoft Azure-Computer"
[planning-guide-3.3.2]: virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Azure Premium-Speicher"
[planning-guide-5.1.1]: virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 "Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure"
[planning-guide-5.1.2]: virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c "Bereitstellen eines virtuellen Computers mit einem kundenspezifischen Image"
[planning-guide-5.2.1]: virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef "Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure"
[planning-guide-5.2.2]: virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 "Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP"
[planning-guide-5.2]: virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 "Vorbereiten virtueller Computer mit SAP für Azure"
[planning-guide-5.3.1]: virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 "Unterschied zwischen einem Azure-Datenträger und einem Azure-Image"
[planning-guide-5.3.2]: virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a "Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure"
[planning-guide-5.4.2]: virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 "Kopieren von Datenträgern zwischen Azure-Speicherkonten"
[planning-guide-5.5.1]: virtual-machines-windows-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 "VM-/VHD-Struktur für SAP-Bereitstellungen"
[planning-guide-5.5.3]: virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d "Festlegen der automatischen Bereitstellung für angefügte Datenträger"
[planning-guide-7.1]: virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 "Einzelner virtueller Computer mit SAP NetWeaver-Demo/Schulungsszenario"
[planning-guide-7]: virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 "Konzepte für Nur-Cloud-Bereitstellung von SAP-Instanzen"
[planning-guide-9.1]: virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 "Azure-Überwachungslösung für SAP"
[planning-guide-azure-premium-storage]: virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Azure Premium-Speicher"

[planning-guide-figure-100]: ./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]: ./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]: ./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]: ./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]: ./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]: ./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]: ./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]: ./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]: ./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]: ./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]: ./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]: ./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]: ./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]: ./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]: ./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]: ./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]: ./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]: ./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]: ./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]: ./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]: ./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]: ./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]: ./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]: virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd "Microsoft Azure-Netzwerk"
[planning-guide-storage-microsoft-azure-storage-and-data-disks]: virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f "Speicher: Microsoft Azure Storage und Datenträger"

[powershell-install-configure]: ../powershell-install-configure.md
[resource-group-authoring-templates]: ../resource-group-authoring-templates.md
[resource-group-overview]: ../resource-group-overview.md
[resource-groups-networking]: ../virtual-network/resource-groups-networking.md
[sap-pam]: https://support.sap.com/pam "SAP-Produktverfügbarkeitsmatrix"
[sap-templates-2-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]: ../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]: ../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]: ../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]: ../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]: ../storage/storage-premium-storage.md
[storage-redundancy]: ../storage/storage-redundancy.md
[storage-scalability-targets]: ../storage/storage-scalability-targets.md
[storage-use-azcopy]: ../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]: virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]: virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]: ../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]: virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]: virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]: virtual-machines-linux-cli-deploy-templates.md "Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI"
[virtual-machines-deploy-rmtemplates-powershell]: virtual-machines-windows-ps-manage.md "Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager und PowerShell"
[virtual-machines-linux-agent-user-guide]: virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]: virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]: virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]: virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]: virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]: virtual-machines-windows-capture-image.md#prepare-the-vm-for-image-capture
[virtual-machines-linux-configure-lvm]: virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]: virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]: virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]: virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]: virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]: virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]: virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]: virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]: virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]: virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]: virtual-machines-windows-ps-create.md
[virtual-machines-sizes]: virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]: virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]: virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]: virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]: virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]: virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]: virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]: virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]: https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]: ../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]: ../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]: ../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]: ../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]: ../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]: ../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]: ../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]: ../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]: ../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]: ../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]: ../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]: ../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]: ../xplat-cli-install.md
[xplat-cli-azure-resource-manager]: ../xplat-cli-azure-resource-manager.md

Microsoft Azure ermöglicht Unternehmen das Abrufen von Server- und Speicherressourcen in kürzester Zeit ohne langwierige Beschaffungszyklen. Virtuelle Azure-Computer ermöglichen Unternehmen das Bereitstellen klassischer Anwendungen, wie z.B. SAP NetWeaver-basierte Anwendungen, in Azure, und das Erweitern deren Zuverlässigkeit und Verfügbarkeit, ohne dass weitere Ressourcen lokal verfügbar sind. Microsoft Azure Virtual Machines-Dienste unterstützen auch standortübergreifende Konnektivität, wodurch Unternehmen virtuelle Azure-Computer aktiv in ihren lokalen Domänen, privaten Clouds und ihrer SAP-Systemlandschaft integrieren können. Dieses Whitepaper beschreibt die Grundlagen von Microsoft Azure Virtual Machines und bietet eine schrittweise Anleitung für die Planungs- und Implementierungsaspekte für SAP NetWeaver-Installationen in Azure. Daher sollten Sie dieses Dokument lesen, bevor Sie die eigentlichen Bereitstellungen von SAP NetWeaver in Azure beginnen. Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## Zusammenfassung
Cloud Computing ist ein häufig verwendeter Begriff, der in der IT-Branche mehr und mehr an Bedeutung gewinnt – in kleinen Unternehmen ebenso wie in großen und multinationalen Konzernen.

Microsoft Azure ist die Plattform für Clouddienste von Microsoft, die ein umfangreiches Spektrum an neuen Möglichkeiten bietet. Nun können Kunden Anwendungen schnell als Dienste in der Cloud bereitstellen bzw. die Bereitstellung aufheben und sind nicht mehr durch technische oder budgetbezogene Aspekte eingeschränkt. Anstatt Zeit und Geld in die Hardwareinfrastruktur zu investieren, können sich Unternehmen auf die Anwendung, Geschäftsprozesse und ihre Vorteile für Kunden und Benutzer konzentrieren.

Mit den Microsoft Azure Virtual Machines-Diensten bietet Microsoft eine umfassende IaaS-Plattform (Infrastructure-as-a-Service). Auf SAP NetWeaver basierende Anwendungen werden auf Azure Virtual Machines (IaaS) unterstützt. Dieses Whitepaper beschreibt, wie auf SAP NetWeaver basierende Anwendungen in Microsoft Azure als Plattform der Wahl geplant und implementiert werden.

Das Dokument selbst konzentriert sich auf zwei Hauptaspekte:

* Im erste Teil werden zwei unterstützte Bereitstellungsmuster für SAP NetWeaver-basierte Anwendungen in Azure beschrieben. Es wird auch die allgemeine Handhabung von Azure bei SAP-Bereitstellungen beschrieben.
* Der zweite Teil enthält ausführliche Informationen zum Implementieren der zwei unterschiedlichen Szenarios, die im ersten Teil beschrieben werden.

Zusätzliche Ressourcen finden Sie im Kapitel [Ressourcen][planning-guide-1.2] in diesem Dokument.

### Definitionen
Im gesamten Dokument werden die folgenden Begriffe verwendet:

* IaaS: Infrastructure as a Service.
* PaaS: Platform as a Service.
* SaaS: Software as a Service.
* ARM: Azure Resource Manager.
* SAP-Komponente: eine einzelne SAP-Anwendung wie ECC, BW, Solution Manager oder EP. SAP-Komponenten können auf herkömmlichen ABAP- oder Java-Technologien oder auf einer Nicht-NetWeaver-basierten Anwendung wie Business Objects basieren.
* SAP-Umgebung: eine oder mehrere SAP-Komponenten, die logisch gruppiert sind, um eine Geschäftsfunktion wie Entwicklung, QAS, Schulung, DR oder Produktion auszuführen.
* SAP-Landschaft: Dies bezieht sich auf alle SAP-Assets in der IT-Landschaft eines Kunden. Die SAP-Landschaft umfasst alle Produktions- und anderen Umgebungen.
* SAP-System: die Kombination aus DBMS-Ebene und Anwendungsebene, z.B. in einem SAP-ERP-Entwicklungssystem, SAP BW-Testsystem, SAP CRM-Produktionssystem usw. In Azure-Bereitstellungen wird die Aufteilung dieser beiden Ebenen zwischen lokalen Systemen und Azure nicht unterstützt. Das bedeutet, dass ein SAP-System entweder lokal oder in Azure bereitgestellt wird. Allerdings können Sie die verschiedenen Systeme einer SAP-Landschaft in Azure oder lokal bereitstellen. Sie konnte z.B. die SAP CRM-Entwicklungs- und Testsysteme in Azure und die SAP CRM-Produktionssysteme lokal bereitstellen.
* Nur-Cloud-Bereitstellung: Eine Bereitstellung, in der das Azure-Abonnement nicht über eine Site-to-Site- oder ExpressRoute-Verbindung mit der lokalen Netzwerkinfrastruktur verbunden ist. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als "Cloud-Only"-Bereitstellungen bezeichnet. Der Zugriff auf mit dieser Methode bereitgestellte virtuelle Computer erfolgt über das Internet und eine öffentliche IP-Adresse und/oder einen öffentlichen DNS-Namen, die bzw. der den virtuellen Computern in Azure zugewiesen wird. Für Microsoft Windows werden das lokale Active Directory (AD) und DNS in diesen Bereitstellungen nicht auf Azure ausgeweitet. Daher sind die virtuellen Computer nicht Teil des lokalen Active Directory. Dasselbe gilt für Linux-Implementierungen, z.B. mit OpenLDAP und Kerberos.

> [AZURE.NOTE] Nur-Cloud-Bereitstellungen in diesem Dokument sind als vollständige SAP-Landschaften definiert, die ausschließlich in Azure ohne Erweiterung von Active Directory/OpenLDAP oder Namensauflösung vom lokalen System in die öffentliche Cloud ausgeführt werden. Nur-Cloud-Konfigurationen werden für die SAP-Produktionssysteme oder Konfigurationen nicht unterstützt, bei denen SAP STMS oder andere lokale Ressourcen zwischen in Azure gehosteten SAP-Systemen und lokalen Ressourcen verwendet werden müssen.

* Standortübergreifend: Beschreibt ein Szenario, in dem virtuelle Computer mit einem Azure-Abonnement bereitgestellt werden, das Site-to-Site-, Multisite- oder ExpressRoute-Verbindungen zwischen den lokalen Rechenzentren und Azure umfasst. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als "Cross-Premises"-Szenarien bezeichnet. Durch die Verbindung sollen lokale Domänen, das lokale Active Directory/OpenLDAP und lokales DNS auf Azure erweitert werden. Die lokale Landschaft wird auf die Azure-Ressourcen des Abonnements erweitert. Durch diese Erweiterung können die virtuellen Computer Teil der lokalen Domäne sein. Domänenbenutzer der lokalen Domäne können auf die Server zugreifen und Dienste auf diesen virtuellen Computern ausführen (z.B. DBMS-Dienste). Die Kommunikation und Namensauflösung zwischen lokal bereitgestellten virtuellen Computern und in Azure bereitgestellten virtuellen Computern ist möglich. In diesem Szenario werden wahrscheinlich die meisten SAP-Assets bereitgestellt. In [diesem Artikel][vpn-gateway-cross-premises-options] und [diesem Artikel][vpn-gateway-site-to-site-create] finden Sie weitere Informationen.

> [AZURE.NOTE] Standortübergreifende Bereitstellungen von SAP-Systemen, in denen virtuelle Azure-Computer mit SAP-Systemen Mitglieder einer lokalen Domäne sind, werden für SAP-Produktionssysteme unterstützt. Standortübergreifende Konfigurationen werden für die Bereitstellung von Teilen von SAP-Landschaften oder vollständigen SAP-Landschaften in Azure unterstützt. Auch für das Ausführen von vollständigen SAP-Landschaften in Azure müssen diese virtuellen Computer Teil der lokalen Domäne und ADS/OpenLDAP sein. In früheren Versionen der Dokumentation wurden Hybrid-IT-Szenarien beschrieben, wobei sich der "Hybrid" darauf bezieht, dass es eine standortübergreifende Verbindung zwischen lokalen Systemen und Azure gibt. Außerdem ist es wichtig, dass die virtuellen Computer in Azure Teil des lokalen Active Directory/OpenLDAP sind.

In einigen Microsoft-Dokumentationen werden standortübergreifende Szenarien etwas anders beschrieben, insbesondere bei DBMS-HA-Konfigurationen. Im Fall von SAP-bezogenen Dokumenten bezieht sich ein standortübergreifendes Szenario einfach nur auf eine Standort-zu-Standort- oder private (ExpressRoute) Verbindung und die Tatsache, dass die SAP-Landschaft zwischen lokalen Systemen und Azure verteilt ist.

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Ressourcen
Die folgenden zusätzlichen Handbücher zum Thema SAP-Bereitstellungen in Azure sind verfügbar:

* [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch (dieses Dokument)][planning-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Bereitstellungshandbuch][deployment-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – DBMS-Bereitstellungshandbuch][dbms-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch für Bereitstellungen mit hoher Verfügbarkeit][ha-guide]

> [AZURE.IMPORTANT] Sofern möglich, wird eine Verknüpfung mit dem entsprechenden SAP-Installationshandbuch verwendet (Referenz InstGuide-01, siehe <http://service.sap.com/instguides>). Wenn es um die Voraussetzungen und den Installationsvorgang geht, sollten Sie die SAP NetWeaver-Installationshandbücher immer sorgfältig lesen, da dieses Dokument nur spezielle Aufgaben für SAP NetWeaver-Systeme, die auf einem virtuellen Microsoft Azure-Computer installiert sind, abdeckt.

Die folgenden SAP-Hinweise beziehen sich auf das Thema SAP in Azure:

| Hinweisnummer | Titel |
|--------------|-------|
| [1928533] | SAP-Anwendungen in Azure: unterstützte Produkte und Größen |
| [2015553] | SAP on Microsoft Azure: Support Prerequisites (SAP in Microsoft Azure: Voraussetzungen für die Unterstützung) |
| [1999351] | Problembehandlung für die erweiterte Azure-Überwachung für SAP |
| [2178632] | Wichtige Überwachungsmetriken für SAP in Microsoft Azure |
| [1409604] | Virtualisierung unter Windows: erweiterte Überwachung |
| [2191498] | SAP unter Linux mit Azure: erweiterte Überwachung
| [2243692] | Linux auf Microsoft Azure-VMs (IaaS): SAP-Lizenzprobleme
| [1984787] | SUSE LINUX Enterprise Server 12: Installationshinweise
| [2002167] | Red Hat Enterprise Linux 7.x: Installation und Upgrade

Lesen Sie bitte auch das [SCN-Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes), das alle SAP-Hinweise für Linux enthält.

Allgemeine standardmäßige und maximale Einschränkungen von Azure-Abonnements finden Sie in [diesem Artikel][azure-subscription-service-limits-subscription].

## Mögliche Szenarien
SAP wird in Unternehmen häufig als eine der Anwendungen angesehen, die am wichtigsten für den Erfolg sind. Die Architektur und Abläufe dieser Anwendungen ist in der Regel sehr komplex, und es ist sehr wichtig, dass Sie die Verfügbarkeits- und Leistungsanforderungen erfüllen.

Daher müssen sich Unternehmen sorgfältig überlegen, welche Anwendungen in einer öffentlichen Cloudumgebung unabhängig vom ausgewählten Cloudanbieter ausgeführt werden können.

Mögliche Systemtypen für die Bereitstellung von SAP NetWeaver-basierten Anwendungen innerhalb öffentlicher Cloudumgebungen sind unten aufgeführt:

1. Produktionssysteme mittlerer Größe
1. Entwicklungssysteme
1. Testsysteme
1. Prototypsysteme
1. Schulungs-/Demonstrationssysteme

Um SAP-Systeme erfolgreich in Azure IaaS oder IaaS bereitzustellen, müssen Sie die wichtigsten Unterschiede zwischen den Angeboten herkömmlicher Outsourcer oder Hoster und IaaS-Angeboten kennen. Während herkömmliche Hoster oder Outsourcer die Infrastruktur (Netzwerk-, Speicher- und Servertyp) auf den Workload anpassen, den ein Kunde hosten möchte, liegt es in der Verantwortung des Kunden, den richtigen Workload für IaaS-Bereitstellungen auszuwählen.

Als ersten Schritt müssen Kunden die folgenden Punkte überprüfen:

* die von SAP unterstützten Typen virtueller Azure-Computer
* die von SAP unterstützten Produkte/Versionen in Azure
* die unterstützten Betriebssystem- und DBMS-Versionen für die entsprechenden SAP-Versionen in Azure
* den von verschiedenen Azure-SKUs bereitgestellten SAPS-Durchsatz

Die Antworten auf diese Fragen finden Sie im SAP-Hinweis [1928533].

Als zweiten Schritt müssen Azure-Ressourcen und Bandbreiteneinschränkungen mit dem tatsächlichen Verbrauch von lokalen Systemen verglichen werden. Daher müssen Kunden mit den verschiedenen Funktionen der Azure-Typen vertraut sein, die mit SAP in folgenden Bereichen unterstützt werden:

* CPU und Speicherressourcen von verschiedenen Typen virtueller Computer und
* IOPS-Bandbreite verschiedener Typen virtueller Computer und
* Netzwerkfunktionen der verschiedenen Typen virtueller Computer.

Die meisten dieser Daten finden Sie [hier][virtual-machines-sizes].

Denken Sie daran, dass die im oben stehenden Link aufgeführten Grenzwerte die Obergrenzen darstellen. Das bedeutet nicht, dass die Grenzwerte für die Ressourcen, z.B. IOPS, unter allen Umständen bereitgestellt werden können. Ausnahmen sind jedoch die CPU- und Speicherressourcen eines ausgewählten Typs virtueller Computer. Für die Typen virtueller Computer, die von SAP unterstützt werden, sind die CPU- und Speicherressourcen reserviert und stehen somit jederzeit zur Nutzung auf dem virtuellen Computer zur Verfügung.

Die Microsoft Azure-Plattform ist wie andere IaaS-Plattformen eine Plattform mit mehreren Mandanten. Das bedeutet, dass Speicher-, Netzwerk- und andere Ressourcen von Mandanten gemeinsam genutzt werden. Intelligente Drosselungs- und Kontingentlogik wird verwendet, um zu verhindern, dass ein Mandant die Leistung eines anderen Mandanten deutlich beeinträchtigt ("Noisy Neighbor"). Obwohl durch Logik in Azure versucht wird, Bandbreitenabweichungen klein zu halten, tendieren Plattformen mit umfangreicher gemeinsamer Nutzung zu größeren Abweichungen bei Ressourcen-/Bandbreitenverfügbarkeit, als viele Kunden von ihren lokalen Bereitstellungen gewohnt sind. Daher erleben Sie möglicherweise im Hinblick auf Netzwerk- oder Speicher-E/A (Volumen und Latenz) minütlich variierende Bandbreiten. Die Wahrscheinlichkeit, dass ein SAP-System in Azure stärkere Schwankungen als ein lokales System aufweist, muss berücksichtigt werden.

Der letzte Schritt ist das Auswerten von Verfügbarkeitsanforderungen. Es kann vorkommen, dass die zugrunde liegenden Azure-Infrastruktur aktualisiert werden muss und dass die Hosts, auf denen virtuelle Computer ausgeführt werden, neu gestartet werden müssen. In diesen Fällen werden die auf diesen Hosts ausgeführten virtuellen Computer heruntergefahren und ebenfalls neu gestartet. Solche Wartungsarbeiten werden außerhalb der Hauptgeschäftszeiten für eine bestimmte Region durchgeführt, aber das potenzielle Fenster (einige Stunden) für einen Neustart ist relativ groß. Es gibt verschiedene Technologien innerhalb der Azure-Plattform, die konfiguriert werden können, um einige oder alle der Auswirkungen dieser Aktualisierungen zu verringern. Zukünftige Verbesserungen der Azure-Plattform, der DBMS- und SAP-Anwendungen werden entwickelt, um die Auswirkungen solcher Neustarts zu minimieren.

Um erfolgreich ein SAP-System auf Azure bereitzustellen, müssen das lokale SAP-System, das Betriebssystem, Datenbank- und SAP-Anwendungen in der SAP-Azure-Support-Matrix angezeigt werden, in die Ressourcen passen, die die Azure-Infrastruktur bereitstellen kann und die mit den Verfügbarkeits-SLAs, die Microsoft Azure bietet, funktionieren können. Wenn diese Systeme identifiziert werden, müssen Sie sich für eines der folgenden zwei Bereitstellungsszenarios entscheiden.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk
 
![Einzelner virtueller Computer mit SAP-Demo- oder -Schulungsszenario in Azure bereitgestellt][planning-guide-figure-100]

Dieses Szenario ist typisch für Schulungs- oder Demosysteme, in denen alle Komponenten von SAP- und Nicht-SAP-Software auf einem einzelnen virtuellen Computer installiert werden. SAP-Produktionssysteme werden in diesem Bereitstellungsszenario nicht unterstützt. Dieses Szenario erfüllt allgemein die folgenden Anforderungen:

* Der Zugriff auf die virtuellen Computer erfolgt über das öffentliche Netzwerk. Eine direkte Netzwerkkonnektivität für die Anwendungen, die auf den virtuellen Computern ausgeführt werden, zum lokalen Netzwerk der Firma, die die Demo- oder Schulungsinhalte besitzt, oder zum Kunden ist nicht erforderlich.
* Im Falle von mehreren virtuellen Computern in einem Schulungs- und Demoszenario muss die Netzwerkkommunikation und die Namensauflösung zwischen den virtuellen Computern funktionieren. Die Kommunikation zwischen den Gruppen von virtuellen Computern muss aber isoliert werden, damit mehrere Gruppen von virtuellen Computern nebeneinander ohne Störung bereitgestellt werden können.
* Internetkonnektivität ist erforderlich, damit sich der Endbenutzer remote bei den in Azure gehosteten virtuellen Computern anmelden kann. Je nach Gastbetriebssystem wird Terminal Services/RDS oder VNC/ssh verwendet, um auf den virtuellen Computer zuzugreifen und die Schulungsaufgaben oder die Demos auszuführen. Wenn SAP-Ports wie 3200, 3300 und 3600 bereitgestellt werden können, ist der Zugriff auf die SAP-Anwendungsinstanz von jedem mit dem Internet verbundenen Desktop aus möglich.
* Die SAP-Systeme (und die virtuellen Computer) stellen ein eigenständiges Szenario in Azure dar, das nur eine öffentliche Internetverbindung für den Endbenutzerzugriff, aber keine Verbindung mit anderen virtuellen Computern in Azure erfordert.
* SAPGUI und ein Browser werden installiert und direkt auf dem virtuellen Computer ausgeführt.
* Ein schnelles Zurücksetzen eines virtuellen Computers auf den ursprünglichen Zustand und eine neue Bereitstellung dieses ursprünglichen Zustands ist erneut erforderlich.
* Im Falle von Demo- und Schulungsszenarios, die auf mehreren virtuellen Computern realisiert werden, ist pro Gruppe von virtuellen Computern ein Active Directory/OpenLDAP und/oder DNS-Dienst erforderlich.


![Gruppe virtueller Computer als Demo- oder Schulungsszenario in einem Azure-Cloud-Dienst][planning-guide-figure-200]

Es ist wichtig, zu beachten, dass die virtuellen Computer in jeder Gruppe parallel bereitgestellt werden müssen, wobei die Namen der virtuellen Computer in jeder Gruppe identisch sind.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk
 
![VPN mit Site-to-Site-Konnektivität (standortübergreifende)][planning-guide-figure-300]

Dieses Szenario ist ein standortübergreifendes Szenario mit zahlreichen möglichen Bereitstellungsmustern. Es kann einfach als das Ausführen einiger Teile der SAP-Landschaft lokal und anderer Teile der SAP-Landschaft in Azure beschrieben werden. Alle Aspekte der Tatsache, dass ein Teil der SAP-Komponenten in Azure ausgeführt wird, sollten für Endbenutzer ersichtlich sein. Daher funktionieren SAP Transport Correction System (STMS), RFC-Kommunikation, Drucken, Sicherheit (wie SSO) usw. problemlos für SAP-Systeme, die unter Microsoft Azure ausgeführt werden. Das standortübergreifende Szenario beschreibt aber auch ein Szenario, in dem die vollständige SAP-Landschaft in Azure ausgeführt wird, wobei die Domäne und der DNS-Dienst des Kunden in Azure erweitert werden.

> [AZURE.NOTE] Dies ist das Bereitstellungsszenario, das für die Ausführung produktiver SAP-Systeme unterstützt wird.

[In diesem Artikel][vpn-gateway-create-site-to-site-rm-powershell] erhalten Sie weitere Informationen zur Verbindung Ihres lokalen Netzwerks mit Microsoft Azure.

> [AZURE.IMPORTANT] Wenn wir über standortübergreifende Szenarios zwischen Azure- und lokalen Kundenbereitstellungen sprechen, geht es um die Granularität ganzer SAP-Systeme. Folgende Szenarien werden für standortübergreifende Szenarien _nicht unterstützt_:
> 
> * Ausführen von unterschiedlichen SAP-Anwendungsebenen in unterschiedlichen Bereitstellungsmethoden. Zum Beispiel wird die DBMS-Ebene lokal ausgeführt, aber die SAP-Anwendungsebene auf virtuellen Computern wird als virtuelle Azure-Computer bereitgestellt oder umgekehrt.
> * Einige Komponenten einer SAP-Ebene in Azure und einige lokal. Zum Beispiel werden Instanzen der SAP-Anwendungsebene zwischen lokalen und virtuellen Azure-Computern aufgeteilt.
> * Die Verteilung der virtuellen Computer, auf denen SAP-Instanzen eines Systems ausgeführt werden, auf mehrere Azure-Regionen wird nicht unterstützt.
> 
> Der Grund für diese Einschränkungen ist die Anforderung für ein Hochleistungsnetzwerk mit einer sehr niedrigen Latenz innerhalb eines SAP-Systems, insbesondere zwischen den Anwendungsinstanzen und der DBMS-Ebene eines SAP-Systems.



### Unterstützte Betriebssystem- und Datenbankversionen

* Die für Dienste virtueller Azure-Computer unterstützte Microsoft-Serversoftware wird in diesem Artikel aufgeführt: <http://support.microsoft.com/kb/2721672>
* Unterstützte Betriebssystemversionen und Datenbanksystemversionen, die von Diensten virtueller Azure-Computer zusammen mit SAP-Software unterstützt werden, sind im SAP-Hinweis [1928533] dokumentiert.
* SAP-Anwendungen und -Versionen, die von Diensten virtueller Azure-Computer unterstützt werden, sind im SAP-Hinweis [1928533] dokumentiert.
* Nur 64-Bit-Images werden für die Ausführung als virtuelle Gastcomputer in Azure für SAP-Szenarios unterstützt. Das bedeutet auch, dass nur 64-Bit-SAP-Anwendungen und -Datenbanken unterstützt werden.

## Microsoft Azure Virtual Machine-Dienste
Die Microsoft Azure-Plattform ist eine internetweite Clouddiensteplattform, die in Microsoft-Rechenzentren gehostet und betrieben wird. Die Plattform umfasst die Microsoft Azure Virtual Machine-Dienste (Infrastructure as a Service oder IaaS) und eine Reihe von umfassenden PaaS-Funktionen (Platform as a Service)-Funktionen.

Die Azure-Plattform verringert die Notwendigkeit für den Vorabkauf von Technologie und Infrastruktur. Sie vereinfacht die Wartung und den Betrieb von Anwendungen durch die Bereitstellung von bedarfsgesteuerten Server- und Speicherressourcen zum Hosten, Skalieren und Verwalten von Webanwendungen und verbundenen Anwendungen. Die Infrastrukturverwaltung wird mit einer Plattform automatisiert, die für hohe Verfügbarkeit und dynamische Skalierung konzipiert wurde, um den Nutzungsanforderungen mit der Option eines nutzungsbasierten Preismodells zu entsprechen.


 
![Positionieren von Microsoft Azure Virtual Machine-Dienste][planning-guide-figure-400]

Mit Azure Virtual Machine-Diensten ermöglicht Ihnen Microsoft das Bereitstellen benutzerdefinierter Images in Azure als IaaS-Instanzen (siehe Abbildung 4). Die virtuellen Computer in Azure basieren auf virtuellen Hyper-V-Festplatten (VHD) und können unterschiedliche Betriebssysteme als Gastbetriebssysteme ausführen.

Aus Betriebsperspektive bietet der Azure Virtual Machine-Dienst ähnliche Funktionen wie lokal bereitgestellte virtuellen Computer. Er hat jedoch den wichtigen Vorteil, dass Sie keine Infrastruktur beschaffen, verwalten und managen müssen. Entwickler und Administratoren haben Vollzugriff auf das Betriebssystemimage auf diesen virtuellen Computern. Administratoren können sich remote bei diesen virtuellen Computern anmelden, um Wartungs- und Problembehandlungsaufgaben sowie Softwarebereitstellungsaufgaben auszuführen. Hinsichtlich der Bereitstellung sind die einzigen Einschränkungen die Größen und Funktionen von virtuellen Azure-Computern. Diese sind möglicherweise nicht so präzise konfigurierbar, wie es lokal möglich wäre. Es gibt eine Auswahl von virtuellen Computertypen, die eine Kombination aus Folgendem darstellen:

* Anzahl der vCPUs,
* Arbeitsspeicher,
* Anzahl der virtuellen Festplatten, die angefügt werden können,
* Netzwerk- und Speicherbandbreiten.

Die Größen und die Einschränkungen verschiedener angebotener virtueller Computer sehen Sie in einer Tabelle in [diesem Artikel][virtual-machines-sizes].

Wie Sie sehen, gibt es verschiedene Familien oder Serien virtueller Computer. Seit Dezember 2015 können Sie zwischen den folgenden Familien virtueller Computer unterscheiden:

* A0-A7-VM-Typen: Nicht alle sind für SAP zertifiziert. Erste VM-Reihe, mit der Azure IaaS eingeführt wurde.
* A8-A11-VM-Typen: Hochleistungs-Serverinstanzen. Werden auf anderen Serverhosts mit höherer Leistung als andere virtuelle Computer der A-Serie ausgeführt.
* D-Serie-VM-Typen: bessere Leistung als A0-A7. Nicht alle Typen von virtuellen Computern sind SAP-zertifiziert.
* DS-Serie-VM-Typen: Diese verwenden die gleichen Hosts wie die D-Serie, können aber eine Verbindung zu Azure Storage Premium herstellen (Informationen finden Sie im Kapitel [Azure Storage Premium][planning-guide-3.3.2] dieses Dokuments). Auch hier sind nicht alle Typen von virtuellen Computern SAP-zertifiziert.
* G-Serie-VM-Typen: virtuelle Computer mit viel Arbeitsspeicher.
* GS-Serie-VM-Typen: wie die G-Serie, aber mit der Option zur Verwendung von Azure Storage Premium (Informationen finden Sie im Kapitel [Azure Storage Premium][planning-guide-3.3.2] dieses Dokuments). Bei Verwendung von virtuellen Computern der GS-Serie als Datenbankserver müssen Sie Storage Premium für Datenbankdaten und Transaktionsprotokolldateien verwenden.


Die gleichen CPU- und Speicherkonfigurationen können Sie auch in anderen VM-Serien finden. Wenn Sie sich die Durchsatzleistung dieser virtuellen Computer aus den verschiedenen Serien ansehen, stellen Sie möglicherweise fest, dass sie sich deutlich unterscheidet, obwohl die gleiche CPU- und Arbeitsspeicherkonfiguration vorliegt. Die Ursache liegt darin, dass die zugrunde liegende Hostserverhardware bei der Einführung der unterschiedlichen Typen virtueller Computer unterschiedliche Durchsatzmerkmale hatte. In der Regel spiegelt sich der Unterschied bei der Durchsatzleistung auch im Preis der unterschiedlichen virtuellen Computer wider.

Beachten Sie, dass möglicherweise nicht alle VM-Serien in jeder der Azure-Regionen (Informationen zu Azure-Regionen finden Sie im nächsten Kapitel) angeboten werden. Beachten Sie auch, dass nicht alle virtuellen Computer oder VM-Serie für SAP zertifiziert sind.

> [AZURE.IMPORTANT] Für die Verwendung von SAP NetWeaver-basierten Anwendungen werden nur die Typen virtueller Computer, die im SAP-Hinweis [1928533] aufgeführt werden, unterstützt.

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-Regionen
Microsoft ermöglicht die Bereitstellung virtueller Computer in so genannten "Azure-Regionen". Eine Azure-Region kann ein oder mehrere Rechenzentren umfassen, die sich nahe beieinander befinden. Für die meisten geopolitischen Regionen der Welt verfügt Microsoft über mindestens zwei Azure-Regionen. In Europa gibt es z.B. die Azure-Regionen "Nordeuropa" und "Westeuropa". Diese zwei Azure-Regionen in einer geopolitischen Region haben genug Abstand zueinander, sodass sich Naturkatastrophen oder technische Notfälle nicht auf beide Azure-Regionen in der gleichen geopolitischen Region auswirken können. Da Microsoft kontinuierlich neue Azure-Regionen in verschiedenen geopolitischen Regionen in aller Welt aufbaut, werden diese Regionen immer zahlreicher. Im Dezember 2015 gab es bereits 20 Azure-Regionen, und zusätzliche Regionen wurden bereits angekündigt. Als Kunde können Sie SAP-Systeme in allen diesen Regionen bereitstellen, auch in den beiden Azure-Regionen in China. Aktuelle Informationen zu Azure-Regionen finden Sie auf dieser Website: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Das Konzept der virtuellen Microsoft Azure-Computer
Microsoft Azure bietet eine IaaS-Lösung (Infrastruktur as a Service) zum Hosten von virtuellen Computern mit ähnlichen Funktionen wie eine lokale Virtualisierungslösung. Sie können virtuelle Computer innerhalb des Azure-Portals, der PowerShell und der CLI erstellen, die ebenfalls Bereitstellungs- und Verwaltungsfunktionen bieten.

Mit dem Azure-Ressourcen-Manager können Sie Ihre Anwendungen mit einer deklarativen Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung in allen Phasen des Anwendungslebenszyklus wiederholt bereitzustellen.

Weitere Informationen zu ARM-Vorlagen Gruppen finden Sie hier:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI][virtual-machines-linux-cli-deploy-templates]
* [Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager und PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://azure.microsoft.com/documentation/templates/>

Ein weiteres interessantes Feature ist die Möglichkeit zum Erstellen von Images von virtuellen Computern, die es Ihnen ermöglichen, bestimmte Repositorys vorzubereiten, von denen Sie schnell virtuelle Computerinstanzen bereitstellen können, die Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Images von virtuellen Computern finden Sie in [diesem Artikel (Windows)][virtual-machines-windows-capture-image] oder in [diesem Artikel (Linux)][virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Fehlerdomänen
Fehlerdomänen stellen eine physische Fehlereinheit dar, die sehr eng mit der physischen Infrastruktur in Rechenzentren verwandt ist, und obwohl ein physisches Blade oder Rack als Fehlerdomäne betrachtet werden kann, gibt es keine direkte 1: 1-Zuordnung zwischen den beiden.

Wenn Sie mehrere virtuelle Computer als Teil eines SAP-Systems in Microsoft Azure Virtual Machine-Diensten bereitstellen, können Sie die Azure Fabric Controller zum Bereitstellen der Anwendung in verschiedenen Fehlerdomänen einsetzen und so die Anforderungen des Microsoft Azure-SLA erfüllen. Allerdings ist die Verteilung von Fehlerdomänen über eine Azure-Skalierungseinheit (Sammlung von Hunderten von Serverknoten oder Speicherknoten und Netzwerken) oder die Zuweisung von virtuellen Computern zu einer bestimmten Fehlerdomäne etwas, das Sie nicht direkt steuern können. Um Azure Fabric Controller anzuweisen, eine Gruppe aus virtuellen Computern in verschiedenen Fehlerdomänen bereitzustellen, müssen Sie den virtuellen Computern zum Zeitpunkt der Bereitstellung eine Azure-Verfügbarkeitsgruppe zuweisen. Weitere Informationen zu Azure-Verfügbarkeitsgruppen finden Sie im Kapitel [Azure-Verfügbarkeitsgruppen][planning-guide-3.2.3] in diesem Dokument.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Upgradedomänen
Upgradedomänen stellen eine logische Einheit dar, mit deren Hilfe Sie bestimmen können, wie ein virtueller Computer innerhalb eines SAP-Systems, das aus SAP-Instanzen besteht, die auf mehreren virtuellen Computern ausgeführt werden, aktualisiert wird. Wenn ein Upgrade durchgeführt wird, durchläuft Microsoft Azure nacheinander den Aktualisierungsprozess für diese Upgradedomänen. Durch das Verteilen von virtuellen Computern zum Zeitpunkt der Bereitstellung über verschiedene Upgradedomänen können Sie Ihr SAP-System teilweise vor potenziellen Ausfallzeiten schützen. Um zu erzwingen, dass Azure die virtuellen Computer eines SAP-Systems bereitstellt, das über verschiedene Upgradedomänen verteilt ist, müssen Sie ein bestimmtes Attribut zum Zeitpunkt der Bereitstellung der einzelnen virtuellen Computer festlegen. Ähnlich wie Fehlerdomänen ist auch eine Azure-Skalierungseinheit in mehrere Upgradedomänen unterteilt. Um Azure Fabric Controller anzuweisen, eine Gruppe aus virtuellen Computern in verschiedenen Upgradedomänen bereitzustellen, müssen Sie den virtuellen Computern zum Zeitpunkt der Bereitstellung eine Azure-Verfügbarkeitsgruppe zuweisen. Weitere Informationen zu Azure-Verfügbarkeitsgruppen finden Sie im Kapitel [Azure-Verfügbarkeitsgruppen][planning-guide-3.2.3] weiter unten.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure-Verfügbarkeitsgruppen
Virtuelle Azure-Computer innerhalb einer Azure-Verfügbarkeitsgruppe werden vom Azure Fabric Controller auf verschiedene Fehler- und Upgradedomänen verteilt. Der Zweck der Verteilung auf verschiedene Fehler- und Upgradedomänen besteht darin, zu verhindern, dass alle virtuellen Computer eines SAP-Systems im Falle von Infrastrukturwartungsarbeiten oder eines Fehler innerhalb einer Fehlerdomäne heruntergefahren werden. Standardmäßig sind virtuelle Computer nicht Teil einer Verfügbarkeitsgruppe. Die Teilnahme eines virtuellen Computers in einer Verfügbarkeitsgruppe wird zum Zeitpunkt der Bereitstellung oder später durch eine Neukonfiguration und erneute Bereitstellung eines virtuellen Computers definiert.

Lesen Sie zum besseren Verständnis des Konzepts der Azure-Verfügbarkeitsgruppen und der Beziehung von Verfügbarkeitsgruppen zu Fehler- und Upgradedomänen [diesen Artikel][virtual-machines-manage-availability].

Informationen zum Definieren von Verfügbarkeitsgruppen für ARM über eine JSON-Vorlage finden Sie in den [Rest-API-Spezifikationen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json). Suchen Sie dort nach „Verfügbarkeit“.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Speicher: Microsoft Azure Storage und Datenträger
Microsoft Azure Virtual Machines nutzt verschiedene Speichertypen. Bei der Implementierung von SAP auf Azure Virtual Machine-Diensten ist es wichtig, die Unterschiede zwischen diesen zwei Speicherhaupttypen zu verstehen:

* Nicht permanenter, temporärer Speicher.
* Permanenter Speicher.

Der nicht permanente Speicher ist direkt mit dem ausgeführten virtuellen Computer verknüpft und befindet sich auf den Serverknoten – der lokale Instanzspeicher (temporärer Speicher). Die Größe hängt von der Größe des virtuellen Computers ab, die beim Start der Bereitstellung ausgewählt wird. Dieser Speichertyp ist temporär. Daher wird der Datenträger initialisiert, wenn eine Instanz eines virtuellen Computers neu gestartet wird. In der Regel befindet sich die Auslagerungsdatei des Betriebssystems auf diesem temporären Datenträger.

___

> ![Windows][Logo_Windows] Windows
>
> Auf virtuellen Windows-Computern wird das temporäre Laufwerk als Laufwerk D:\\ auf einem vorhandenen virtuellen Computer bereitgestellt.
>
> ![Linux][Logo_Linux] Linux
> 
> Auf virtuellen Linux-Computern wird es als "/mnt/resource" oder "/mnt" bereitgestellt. Ausführlichere Informationen finden Sie hier:
> 
> * [Vorgehensweise: Anfügen eines Datenträgers an einen virtuellen Linux-Computer][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/archive/2013/12/07/understanding-the-temporary-drive-on-windows-azure-virtual-machines.aspx>

___

Das eigentliche Laufwerk ist temporär, da es auf dem Hostserver gespeichert wird. Wenn der virtuelle Computer in eine erneute Bereitstellung verschoben wird (z.B. aufgrund von Wartungsarbeiten auf dem Host oder durch Herunterfahren und Neustart), geht der Inhalt des Laufwerks verloren. Aus diesem Grund dürfen Sie auf diesem Laufwerk keine wichtigen Daten speichern. Der für diese Art von Speicher verwendete Medientyp unterscheidet sich je nach VM-Serie durch sehr unterschiedliche Leistungsmerkmale, die seit Juni 2015 folgendermaßen aussehen:

* A5-A7: sehr eingeschränkte Leistung. Nur für Auslagerungsdatei empfohlen.
* A8-A11: sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz.
* D-Serie: sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz.
* DS-Serie: sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz.
* G-Serie: sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz.
* GS-Serie: sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz.

Die Angaben oben beziehen sich auf die für SAP zertifzierten VM-Typen. Die VM-Serie mit hervorragenden IOPS und Durchsatz können für einige DBMS-Funktionen genutzt werden. Weitere Informationen finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide].

Microsoft Azure Storage stellt permanenten Speicher und die normalen Ebenen für Schutz und Redundanz, die auch SAN-Speicher bietet, bereit. Auf Azure Storage basierende Datenträger sind virtuelle Festplatten (VHD), die sich in den Azure Storage-Diensten befinden. Der lokale Betriebssystem-Datenträger (Windows C:\\, Linux / ( /dev/sda1 )) wird in Azure Storage gespeichert, und auch zusätzliche Volumes/Datenträger, die auf dem virtuellen Computer bereitgestellt werden, sind dort gespeichert.

Es ist möglich, eine vorhandene virtuelle lokale Festplatte hochzuladen oder eine leere innerhalb von Azure zu erstellen und diese bereitgestellten virtuellen Computern anzufügen. Diese VHDs werden als Azure-Datenträger bezeichnet.

Nach dem Erstellen oder Hochladen einer virtuellen Festplatte in Azure Storage ist es möglich, diese in einem vorhandenen virtuellen Computer bereitzustellen und anzufügen und vorhandene (nicht bereitgestellte) VHDs zu kopieren.

Da diese VHDs permanent sind, sind beim Neustart und erneuten Erstellen einer virtuellen Computerinstanz alle Daten und Änderungen sicher. Auch wenn eine Instanz gelöscht wird, sind diese VHDs weiterhin sicher und können erneut bereitgestellt oder im Falle von Datenträgern ohne Betriebssystem anderen virtuellen Computern bereitgestellt werden.

Innerhalb des Netzwerks von Azure Storage können verschiedene Redundanzebenen konfiguriert werden:

* Die Mindestebene, die ausgewählt werden kann, ist die „lokale Redundanz“, die drei Replikaten der Daten innerhalb des gleichen Rechenzentrums einer Azure-Region entspricht (Informationen finden Sie im Kapitel [Azure-Regionen][planning-guide-3.1]).
* Zonenredundanter Speicher, der die drei Images über verschiedene Rechenzentren innerhalb derselben Azure-Region verteilt.
* Standardredundanzebene ist geografische Redundanz, die den Inhalt asynchron in drei weitere Images der Daten in einer anderen Azure-Region repliziert, die in der gleichen geopolitischen Region gehostet wird.

Informationen zu den verschiedenen Speicherredundanzoptionen finden Sie auch in der Tabelle oben in diesem Artikel: <https://azure.microsoft.com/pricing/details/storage/>

Weitere Informationen zur Azure Storage finden Sie hier:

* <https://azure.microsoft.com/documentation/services/storage/>
* <https://azure.microsoft.com/services/site-recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview.aspx>


#### Azure-Standardspeicher
Azure-Standard-BLOB-Speicher war der verfügbare Speichertyp, als Azure IaaS veröffentlicht wurde. IOPS-Kontingente wurden für jede einzelne VHD erzwungen. Die Latenz unterschied sich grundlegend von SAN/NAS-Geräten, wie sie in der Regel für lokal gehostete High-End-SAP-Systeme bereitgestellt werden. Dennoch erwies sich der Azure-Standardspeicher für mehrere Hunderte von SAP-Systemen, die in der Zwischenzeit in Azure bereitgestellt wurden, als ausreichend.

Der Azure-Standardspeicher wird basierend auf den tatsächlich gespeicherten Daten, der Menge der Speichertransaktionen, den ausgehenden Datenübertragungen und der ausgewählten Redundanzoption in Rechnung gestellt. Viele VHDs können mit der Maximalgröße von 1 TB erstellt werden, aber solange sie leer bleiben, werden sie nicht in Rechnung gestellt. Wenn Sie dann eine virtuelle Festplatte mit 100 GB füllen, wird Ihnen das Speichern von 100 GB in Rechnung gestellt, nicht die nominale Größe, mit der die virtuelle Festplatte erstellt wurde.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Storage Premium
Im April 2015 stellte Microsoft Azure Storage Premium vor. Storage Premium wurde mit dem Ziel, Folgendes bereitzustellen, eingeführt:

* Bessere E/A-Latenz.
* Besserer Durchsatz.
* Weniger variierende E/A-Latenz.

Zu diesem Zweck wurden viele Änderungen eingeführt. Die beiden wichtigsten sind folgende:

* Verwendung von SSD-Laufwerken in Azure Storage-Knoten
* Ein neuer Lesecache, der durch die lokale SSD eines Azure-Serverknotens unterstützt wird

Im Gegensatz zum standardmäßigen Speicher, bei dem sich Funktionen nicht abhängig von der Größe der Festplatte (oder der VHD) änderten, verfügt Storage Premium derzeit über drei unterschiedliche Datenträgerkategorien, die Sie am Ende dieses Artikels vor dem FAQ-Abschnitt sehen können: <https://azure.microsoft.com/pricing/details/storage/>

Sie sehen, dass IOPS-/VHD- und Datenträgerdurchsatz von der Größenkategorie der Datenträger abhängen.

Kostenbasis im Falle von Storage Premium ist nicht der tatsächliche Datenumfang, der in diesen VHDs gespeichert wird, sondern die Größenkategorie einer solchen virtuellen Festplatte, unabhängig von der Datenmenge, die auf der virtuellen Festplatte gespeichert wird.

Sie können auch virtuelle Festplatten in Storage Premium erstellen, die den unten gezeigten Größenkategorien nicht direkt zugeordnet sind. Insbesondere beim Kopieren von VHDs aus dem Standardspeicher zu Storage Premium kann dies der Fall sein. Hier erfolgt eine Zuordnung zur nächstgrößeren Storage Premium-Datenträgeroption.

Beachten Sie, dass nur bestimmte VM-Serien von Azure Storage Premium profitieren können. Stand Dezember 2015 sind das die DS- und die GS-Serien. Die DS-Serie entspricht im Grunde der D-Serie, mit der Ausnahme, dass die DS-Serie die Möglichkeit hat, Storage Premium-basierte virtuelle Computer zusätzlich zu virtuellen Festplatten, die im Azure-Standardspeicher gehostet werden, bereitzustellen. Dasselbe gilt für die G-Serie im Vergleich zur GS-Serie.

Wenn Sie sich den Teil zu virtuellen Computern der DS-Serie [in diesem Artikel][virtual-machines-sizes] ansehen, erkennen Sie auch, dass es Datenmengeneinschränkungen für Storage Premium-VHDs bezüglich der Granularität der VM-Stufe gibt. Unterschiedliche virtuelle Computer der DS-Serie oder der GS-Serie verfügen auch über andere Einschränkungen im Hinblick auf die Anzahl der virtuellen Festplatten, die bereitgestellt werden können. Diese Einschränkungen werden ebenfalls im oben erwähnten Artikel dokumentiert. Aber im Wesentlichen bedeutet das Folgendes: Wenn Sie z.B. 32 P30-Datenträger/-VHDs auf einem einzelnen virtuellen DS14-Computer bereitstellen, erhalten Sie NICHT das 32-fache des maximalen Durchsatzes eines P30-Datenträgers. Stattdessen beschränkt der maximale Durchsatz auf VM-Ebene, wie im Artikel dokumentiert, den Datendurchsatz.

Weitere Informationen zu Storage Premium finden Sie hier: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### Azure-Speicherkonten
Wenn Sie Dienste oder virtuelle Computer in Azure bereitstellen, muss die Bereitstellung von VHDs und VM-Images in so genannten Azure-Speicherkonten organisiert werden. Wenn Sie eine Azure-Bereitstellung planen, müssen Sie die Einschränkungen von Azure sorgfältig berücksichtigen. Einerseits gibt es eine begrenzte Anzahl von Speicherkonten pro Azure-Abonnement. Obwohl jedes Azure-Speicherkonto eine große Anzahl von VHD-Dateien enthalten kann, besteht eine feste Obergrenze für die Gesamt-IOPS pro Speicherkonto. Bei der Bereitstellung von Hunderten von SAP-VMs mit DBMS-Systemen, die umfassende E/A-Aufrufe erzeugen, wird empfohlen, virtuelle Computer mit hoher IOPS DBMS auf mehrere Azure-Speicherkonten zu verteilen. Achten Sie darauf, das aktuelle Limit für Azure-Speicherkonten pro Abonnement nicht zu überschreiten. Da der Speicher ein wesentlicher Teil der Datenbankbereitstellung für ein SAP-System ist, wird dieses Konzept detaillierter im bereits erwähnten [DBMS-Bereitstellungshandbuch][dbms-guide] erläutert.

Weitere Informationen zu Azure-Speicherkonten finden Sie in [diesem Artikel][storage-scalability-targets]. In diesem Artikel werden Sie feststellen, dass es Unterschiede hinsichtlich der Einschränkungen zwischen Azure-Standardspeicherkonten und Storage Premium-Konten gibt. Hauptunterschied ist die Datenmenge, die innerhalb eines solchen Speicherkontos gespeichert werden kann. Beim Standardspeicher ist die Menge wesentlich größer als bei Storage Premium. Auf der anderen Seite ist das Standardspeicherkonto hinsichtlich IOPS stark eingeschränkt (siehe Spalte "Total Request Rate"), während das Azure Storage Premium-Konto keine solche Einschränkung aufweist. Bei der Erörterung der Bereitstellung von SAP-Systemen, insbesondere der DBMS-Server, werden Details und Ergebnisse dieser Unterschiede erläutert.

Innerhalb eines Speicherkontos haben Sie die Möglichkeit, unterschiedliche Container für die Organisation und Kategorisierung verschiedener VHDs zu erstellen. Diese Container werden in der Regel verwendet, um z.B. virtuelle Festplatten unterschiedlicher virtueller Computer zu trennen. Es gibt keine Leistungseinbußen bei der Verwendung von nur einem Container oder mehrerer Container unter einem einzelnen Azure-Speicherkonto.

Innerhalb von Azure folgt ein VHD-Name der folgenden Benennungskonvention, die einen eindeutigen Namen für die virtuelle Festplatte in Azure bereitstellen muss:

	http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Wie bereits erwähnt, muss die oben genannten Zeichenfolge die virtuellen Festplatte, die in Azure Storage gespeichert ist, eindeutig identifizieren.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure-Netzwerk
Microsoft Azure stellt eine Netzwerkinfrastruktur bereit, die die Zuordnung aller Szenarios ermöglicht, die wir mit SAP-Software umsetzen möchten. Die Funktionen sind:

* Zugriff von außen direkt auf die virtuellen Computer über Windows Terminal Services oder ssh/VNC
* Zugriff auf Dienste und bestimmte Ports, die von Anwendungen auf den virtuellen Computern verwendet werden
* Interne Kommunikation und Namensauflösung zwischen einer Gruppe von virtuellen Computern, die als virtuelle Azure-Computer bereitgestellt werden
* Standortübergreifende Konnektivität zwischen einem lokalen Kundennetzwerk und dem Azure-Netzwerk
* Azure-Region-übergreifende Konnektivität oder Rechenzentrumskonnektivität zwischen Azure-Standorten

Weitere Informationen finden Sie hier: <https://azure.microsoft.com/documentation/services/virtual-network/>

Es gibt viele verschiedene Möglichkeiten zum Konfigurieren der Namens- und IP-Auflösung in Azure. In diesem Dokument benötigen Nur-Cloud-Szenarios die standardmäßige Verwendung von Azure DNS (im Gegensatz zum Definieren eines eigenen DNS-Diensts). Es gibt auch einen neueren Azure DNS-Dienst, der verwendet werden kann, anstatt einen eigenen DNS-Server einzurichten. Weitere Informationen finden Sie in [diesem Artikel][virtual-networks-manage-dns-in-vnet] und auf [dieser Seite](https://azure.microsoft.com/services/dns/).

Für standortübergreifende Szenarios verlassen wir uns auf die Tatsache, dass das lokale AD/OpenLDAP/DNS über VPN- oder private Verbindungen zu Azure erweitert wurde. Für bestimmte hier dokumentierte Szenarios kann es notwendig sein, ein AD/OpenLDAP-Replikat in Azure zu installieren.

Da Netzwerk und Namensauflösung wesentliche Teile der Datenbankbereitstellung für ein SAP-System sind, wird dieses Konzept detaillierter im [DBMS-Bereitstellungshandbuch][dbms-guide] erläutert.


##### Virtuelle Azure-Netzwerke

Durch das Einrichten eines virtuellen Azure-Netzwerk können Sie den Adressbereich der privaten IP-Adressen definieren, die durch die Azure-DHCP-Funktionen zugeordnet werden. In standortübergreifenden Szenarios wird der definierte IP-Adressbereich weiterhin mithilfe von DHCP von Azure zugeordnet. Allerdings erfolgt die Auflösung von Domänennamen lokal (vorausgesetzt, dass die virtuellen Computer einer lokalen Domäne angehören). Darum können auch Adressen außerhalb der verschiedenen Azure-Clouddienste aufgelöst werden.

[comment]: <> (MSSedusch still needed? TODO Ursprünglich wurde ein virtuelles Azure-Netzwerk an eine Affinitätsgruppe gebunden. Damit war ein virtuelles Netzwerk in Azure auf die Azure-Skalierungseinheit beschränkt, die der Affinitätsgruppe zugewiesen wurde. Das bedeutete im Endeffekt, dass das virtuelle Netzwerk auf die Ressourcen beschränkt war, die in der Azure-Skalierungseinheit verfügbar waren. Dies wurde seitdem geändert, und jetzt können sich virtuelle Azure-Netzwerke über mehrere Azure-Skalierungseinheiten erstrecken. Das erfordert jedoch, dass virtuelle Azure-Netzwerke bei der Erstellung **KEINEN** Affinitätsgruppen mehr zugeordnet werden. Wie bereits erwähnt, sollten Sie entgegen den Empfehlungen von vor einem Jahr **KEINE Azure-Affinitätsgruppen mehr nutzen**. Weitere Informationen finden Sie unter: <https://azure.microsoft.com/blog/regional-virtual-networks/>)

Alle virtuellen Computer in Azure müssen mit einem virtuellen Netzwerk verbunden sein.

Weitere Informationen finden Sie in [diesem Artikel][resource-groups-networking] und auf [dieser Seite](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd TODO Couldn't find an article which includes the OpenLDAP topic + ARM; ) 
[comment]: <> (MSSedusch <https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [AZURE.NOTE] Standardmäßig können Sie nach der Bereitstellung eines virtuellen Computers die Konfiguration des virtuellen Netzwerks nicht mehr ändern. Die TCP/IP-Einstellungen müssen dem Azure-DHCP-Server überlassen werden. Das Standardverhalten ist die dynamische IP-Zuweisung.

Die MAC-Adresse der virtuellen Netzwerkkarte kann sich ändern, z.B. nach einer Größenanpassung, und das Windows- oder Linux-Gastbetriebssystem erkennt die neue Netzwerkkarte und verwendet automatisch DHCP zum Zuweisen der IP- und DNS-Adressen.

##### Statische IP-Zuweisung
Es ist möglich, virtuellen Computern in einem virtuellen Azure-Netzwerk feste oder reservierte IP-Adressen zuzuweisen. Das Ausführen der virtuellen Computer in einem virtuellen Azure-Netzwerk eröffnet eine gute Möglichkeit, um diese Funktion für einige Szenarios im Bedarfsfall zu nutzen. Die IP-Zuweisung bleibt während des gesamten Lebenszyklus des virtuellen Computers gültig, unabhängig davon, ob der virtuelle Computer ausgeführt oder heruntergefahren wird. Daher müssen Sie die Gesamtzahl der virtuellen Computer (ausgeführte und beendete virtuelle Computer) berücksichtigen, wenn Sie den IP-Adressbereich für das virtuelle Netzwerk definieren. Die IP-Adresse bleibt zugewiesen, bis der virtuelle Computer und seine Netzwerkschnittstelle gelöscht werden oder bis die Zuweisung der IP-Adresse wieder aufgehoben wird. Ausführliche Informationen finden Sie in [diesem Artikel][virtual-networks-static-private-ip-arm-pportal].

##### Mehrere NICs pro virtuellen Computer
Sie können mehrere virtuelle Netzwerkschnittstellenkarten (vNIC) für virtuelle Azure-Computer definieren. Dank der Möglichkeit, mehrere vNICs einzusetzen, können Sie mit der Einrichtung der Netzwerkdatenverkehrstrennung beginnen. Dabei wird z.B. der Clientdatenverkehr über eine vNIC und der Back-End-Datenverkehr über eine zweite vNIC weitergeleitet. Je nach Typ des virtuellen Computers gibt es unterschiedliche Einschränkungen im Hinblick auf die Anzahl von vNICs. Informationen zu Details, Funktionen und Einschränkungen finden Sie in den folgenden Artikeln:
 
* [Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)][virtual-networks-multiple-nics]
* [Bereitstellen von Multi-NIC-VMs mithilfe einer Vorlage][virtual-network-deploy-multinic-arm-template]
* [Bereitstellen von Multi-NIC-VMs mit PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Bereitstellen von Multi-NIC-VMs mithilfe der Azure-CLI][virtual-network-deploy-multinic-arm-cli]

#### Standort-zu-Standort-Konnektivität
"Standortübergreifend" bezieht sich darauf, dass virtuelle Azure-Computer und lokale Computer über eine transparente und permanente VPN-Verbindung verknüpft sind. Dieses Konzept wird wahrscheinlich zum häufigsten SAP-Bereitstellungsmuster in Azure werden. Die Annahme ist, dass betriebliche Verfahren und Prozesse mit SAP-Instanzen in Azure transparent arbeiten sollten. Das bedeutet, dass Sie in der Lage sein sollten, aus diesen Systemen heraus zu drucken und das SAP Transport Management System (TMS) zu verwenden, um Änderungen aus einem Entwicklungssystem in Azure in ein lokal bereitgestelltes Testsystem zu übertragen. Weitere Dokumentationen zu „Standort-zu-Standort“ finden Sie in [diesem Artikel][vpn-gateway-create-site-to-site-rm-powershell].

##### VPN-Tunnelgerät
Um eine Standort-zu-Standort-Verbindung (lokales Rechenzentrum zu Azure-Rechenzentrum) zu erstellen, müssen Sie ein VPN-Gerät abrufen und konfigurieren oder RRAS (Routing and Remote Access Service) verwenden, der als Softwarekomponente mit Windows Server 2012 eingeführt wurde.

* [Erstellen eines virtuellen Netzwerks mit einer Standort-zu-Standort-VPN-Verbindung mit PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten][vpn-gateway-about-vpn-devices]
* [Häufig gestellte Fragen zum VPN-Gateway][vpn-gateway-vpn-faq]

![Site-to-Site-Verbindung zwischen lokalem System und Azure][planning-guide-figure-600]

Die obige Abbildung zeigt zwei Azure-Abonnements mit IP-Adressunterbereichen, die für die Verwendung in virtuellen Netzwerken in Azure reserviert sind. Die Verbindung vom lokalen Netzwerk zu Azure wird mittels VPN hergestellt.

#### Punkt-zu-Standort-VPN
Punkt-zu-Standort-VPN erfordert, dass jeder Clientcomputer eine Verbindung mit dem eigenen VPN in Azure herstellt. Für die SAP-Szenarios, die wir betrachten, ist die Punkt-zu-Standort-Konnektivität nicht geeignet. Aus diesem Grund werden keine weiteren Angaben zur Punkt-zu-Standort-VPN-Konnektivität gemacht.

[comment]: <> (MSSedusch -- More information can be found here) 
[comment]: <> (MShermannd TODO Link no longer valid; But ARM is anyway not supported - see next link below) 
[comment]: <> (MSSedusch -- <http://msdn.microsoft.com/library/azure/dn133798.aspx>.) 
[comment]: <> (MShermannd TODO Point to Site not supported yet with ARM ) 
[comment]: <> (MSSedusch -- <https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### VPN mit mehreren Standorten
Azure bietet jetzt auch die Möglichkeit zum Erstellen von VPN-Konnektivität mit mehreren Standorten für ein Azure-Abonnement. Zuvor war ein einzelnes Abonnement auf eine Standort-zu-Standort-VPN-Verbindung beschränkt. Diese Einschränkung wurde mit VPN-Verbindungen mit mehreren Standorten für ein einzelnes Abonnement aufgehoben. Dadurch können mehrere Azure-Regionen für ein bestimmtes Abonnement über standortübergreifende Konfigurationen genutzt werden.

Weitere Dokumentationen finden Sie in [diesem Artikel][vpn-gateway-create-site-to-site-rm-powershell]. 
[comment]: <> (MShermannd TODO found no ARM docu link)

#### VNet-zu-VNet-Verbindung
Bei Verwendung von VPN mit mehreren Standorten müssen Sie ein separates virtuelles Azure-Netzwerk in jeder einzelnen Region konfigurieren. Jedoch besteht sehr häufig die Anforderung, dass die Komponenten in den verschiedenen Regionen miteinander kommunizieren sollen. Im Idealfall sollte diese Kommunikation nicht von einer Azure-Region zu lokalen Standorten und von dort aus in die andere Azure-Region weitergeleitet werden. Azure bietet zur Vereinfachung die Möglichkeit, eine Verbindung von einem virtuellen Azure-Netzwerk in einer Region mit einem anderen virtuellen Azure-Netzwerk, das in einer anderen Region gehostet wird, zu konfigurieren. Diese Funktionalität wird als VNet-zu-VNet-Verbindung bezeichnet. Weitere Informationen zu dieser Funktionalität finden Sie hier: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### Private Verbindung mit Azure – ExpressRoute
Microsoft Azure ExpressRoute ermöglicht das Erstellen privater Verbindungen zwischen Azure-Rechenzentren und der lokalen Infrastruktur des Kunden oder in einer Umgebung mit mehreren Standorten. ExpressRoute wird von verschiedenen MPLS-VPN-Anbietern (paketvermittelt) oder anderen Netzwerkdienstanbietern angeboten. ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. ExpressRoute-Verbindungen bieten eine größere Sicherheit, höhere Zuverlässigkeit durch mehrere parallele Leitungen, höhere Geschwindigkeit und weniger Latenz als herkömmliche Verbindungen über das Internet.

Weitere Informationen zu Azure ExpressRoute und den Angeboten finden Sie hier:

* <https://azure.microsoft.com/documentation/services/expressroute/>
* <https://azure.microsoft.com/pricing/details/expressroute/>
* <https://azure.microsoft.com/documentation/articles/expressroute-faqs/>

Express Route ermöglicht mehrere Azure-Abonnements über eine ExpressRoute-Verbindung, wie hier beschrieben wird.

* <https://azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://azure.microsoft.com/documentation/articles/expressroute-howto-circuit-arm/>


#### Tunnelerzwingung in standortübergreifenden Szenarios
Für virtuelle Computer, die lokalen Domänen über Standort-zu-Standort-, Punkt-zu-Standort- oder ExpressRoute-Verbindungen beitreten, müssen Sie sicherstellen, dass auch die Internetproxyeinstellungen für alle Benutzer auf diesen virtuellen Computern bereitgestellt werden. Standardmäßig würde Software, die auf diesen virtuellen Computer ausgeführt wird, und würden Benutzer, die mit einem Browser auf das Internet zugreifen, nicht durch den Unternehmensproxy geleitet, sondern über Azure eine direkte Verbindung mit dem Internet herstellen. Aber auch die Proxyeinstellung ist keine hundertprozentige Lösung, um den Datenverkehr über den Unternehmensproxy zu leiten, da es in der Verantwortung der Software und der Dienste liegt, den Proxy zu suchen. Wenn die auf dem virtuellen Computer ausgeführte Software dies nicht leistet oder wenn ein Administrator die Einstellungen ändert, kann der Datenverkehr ins Internet wieder direkt durch Azure ins Internet umgeleitet werden.

Um dies zu vermeiden, können Sie erzwungenes Tunneling mit Standort-zu-Standort-Konnektivität zwischen lokalen Standorten und Azure konfigurieren. Eine detaillierte Beschreibung der Funktion des erzwungenen Tunnelings finden Sie hier: <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Erzwungenes Tunneling mit ExpressRoute wird von Kunden aktiviert, die eine Standardroute über die ExpressRoute-BGP-Peeringsitzungen anfordern.

#### Zusammenfassung der Azure-Netzwerke
In diesem Kapitel haben viele wichtige Informationen zu Azure-Netzwerken erhalten. Es folgt eine Zusammenfassung der wichtigsten Punkte:


* Mit virtuellen Azure-Netzwerken können Sie das Netzwerk gemäß Ihren Anforderungen einrichten.
* Virtuelle Azure-Netzwerke können genutzt werden, um virtuellen Computern IP-Adressbereiche oder feste IP-Adressen zuzuweisen.
* Zum Einrichten einer Standort-zu-Standort- oder Punkt-zu-Standort-Verbindung müssen Sie zunächst ein virtuelles Azure-Netzwerk erstellen.
* Nach der Bereitstellung eines virtuellen Computers ist es nicht mehr möglich, das virtuelle Netzwerk, das dem virtuellen Computer zugewiesen wurde, zu ändern.

### Kontingente in Azure Virtual Machine-Diensten
Wir müssen klar sagen, dass die Speicher- und Netzwerkinfrastruktur von mehreren virtuellen Computern, auf denen verschiedene Dienste in der Azure-Infrastruktur ausgeführt werden, gemeinsam genutzt wird. Und genau wie in den Rechenzentren des Kunden findet die übermäßige Bereitstellung einiger Infrastrukturressourcen zu einem gewissen Grad statt. Die Microsoft Azure-Plattform verwendet Datenträger-, CPU-, Netzwerk- und andere Kontingente, um den Ressourcenverbrauch zu begrenzen und eine konsistente sowie vorhersagbare Leistung beizubehalten. Die verschiedenen VM-Typen (A5, A6 usw.) haben unterschiedliche Kontingente für die Anzahl der Datenträger, für die CPU, das RAM und das Netzwerk.

> [AZURE.NOTE] CPU- und Arbeitsspeicherressourcen der von SAP unterstützten Typen virtueller Computer werden auf den Hostknoten vorab zugeordnet. Dies bedeutet, dass nach der Bereitstellung des virtuellen Computers die Ressourcen auf dem Host gemäß der Definition durch den Typ des virtuellen Computers zur Verfügung stehen.

Bei der Planung und Größenanpassung von SAP-Lösungen in Azure müssen die Kontingente für virtuelle Computer jeder Größe berücksichtigt werden. Die Kontingente für virtuelle Computer werden [hier][virtual-machines-sizes] beschrieben.

Die beschriebenen Kontingente stellen die theoretischen Höchstwerte dar. Das IOPS-Limit pro VHD kann mit kleinen IOs (8 KB) erreicht werden, aber möglicherweise nicht mit großen IOs (1 MB). Das IOPS-Limit wird für die Granularität einzelner VHDs erzwungen.

Nachfolgende Entscheidungsstruktur hilft Ihnen zu ermitteln, ob sich Ihr vorhandenes SAP-System in Azure Virtual Machines Services und seine Features einpasst oder ob zu dessen Implementierung in Azure Änderungen an seiner Konfiguration erforderlich sind:
 
![Entscheidungsbaum für die Bereitstellung Von SAP in Azure][planning-guide-figure-700]

**Schritt 1**: Die wichtigste Frage, mit der Sie beginnen müssen, ist, inwieweit für Ihr SAPS ein bestimmtes SAP-System erforderlich ist. Die SAPS-Anforderungen müssen in eine DBMS-Komponente und eine SAP-Anwendungskomponente unterteilt werden, selbst wenn das SAP-System bereits lokal in einer 2-Tier-Konfiguration bereitgestellt ist. Bei vorhandenen Systemen können die mit der derzeitigen Hardware verwendeten SAPS meist mithilfe vorhandener SAP-Benchmarks bestimmt bzw. geschätzt werden. Die Ergebnisse finden Sie hier: <http://global.sap.com/campaigns/benchmark/index.epx>. Für neu bereitgestellte SAP-Systeme sollten Sie Dimensionierungstests durchführen, durch die Sie die SAPS-Anforderungen des Systems bestimmen. Lesen Sie hierzu auch den folgenden Blog sowie das angefügte Dokument für die Dimensionierung von SAP in Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Schritt 2**: Bei vorhandenen Systemen sollten das E/A-Volumen sowie die E/A-Vorgänge pro Sekunde auf dem DBMS-Server gemessen werden. Bei neu geplanten Systemen sollten die Dimensionierungstests für das neue System auch grob Aufschluss über die E/A-Anforderungen auf DBMS-Seite geben. Falls Sie sich unsicher sind, sollten Sie eventuell eine Machbarkeitsstudie durchführen.

**Schritt 3**: Vergleichen Sie die SAPS-Anforderungen für den DBMS-Server mit den SAPS der verschiedenen von Azure bereitgestellten VM-Typen. Informationen zu den SAPS der verschiedenen Azure-VM-Typen finden Sie in SAP-Hinweis [1928533]. Hauptaugenmerk sollten Sie zunächst auf die DBMS-VM legen, da die Datenbankebene die Ebene in einem SAP-NetWeaver-System ist, die sich in den meisten Bereitstellungen nicht verteilen lässt. Im Gegensatz dazu lässt sich die SAP-Anwendungsebene durchaus verteilen. Wenn keiner der von SAP unterstützten VM-Typen von Azure die erforderlichen SAPS bereitstellt, kann die Arbeitslast des geplanten SAP-Systems nicht in Azure ausgeführt werden. Sie müssen das System entweder lokal bereitstellen oder das Volumen der Arbeitslast für das System ändern.

**Schritt 4**: Wie [hier][virtual-machines-sizes] dokumentiert, erzwingt Azure ein IOPS-Kontingent pro VHD, und zwar unabhängig davon, ob Sie Standardspeicher oder Storage Premium verwenden. Abhängig vom VM-Typ variiert die Anzahl der VHDs, die bereitgestellt werden können. Daher können Sie eine maximale Anzahl von IOPS berechnen, die mit den verschiedenen VM-Typen erreicht werden kann. Abhängig vom Layout der Datenbankdatei können Sie VHDs so aufteilen, dass sie ein Volumen des Gastbetriebssystems werden. Wenn allerdings das aktuelle IOPS-Volumen eines bereitgestellten SAP-Systems die berechneten Grenzen des größten VM-Typs von Azure überschreitet und keine Möglichkeit besteht, dies mit zusätzlichem Speicher zu kompensieren, kann dies die Arbeitslast des SAP-Systems erheblich beeinträchtigen. In solchen Fällen kann ein Punkt erreicht werden, an dem das System nicht mehr in Azure bereitgestellt werden sollte.

**Schritt 5**: Gerade bei lokal in 2-Ebenen-Konfigurationen bereitgestellten SAP-Systemen ist die Wahrscheinlichkeit groß, dass das System in Azure in einer 3-Ebenen-Konfiguration konfiguriert werden muss. In diesem Schritt prüfen Sie, ob die SAP-Anwendungsebene Komponenten enthält, die nicht verteilt werden können und sich daher nicht in die von den verschiedenen Azure-VM-Typen bereitgestellten CPU- und Arbeitsspeicherressourcen einpassen. Sollte es tatsächlich eine solche Komponente geben, können das SAP-System und seine Arbeitslast nicht in Azure bereitgestellt werden. Lassen sich die SAP-Anwendungskomponenten jedoch auf mehrere Azure-VMs verteilen, so kann das System in Azure bereitgestellt werden.

**Schritt 6**: Wenn die Komponenten der DBMS- und SAP-Anwendungsebene auf Azure-VMs ausgeführt werden können, müssen bei der Konfiguration die folgenden Punkte berücksichtigt werden:

* Anzahl der Azure-VMs
* VM-Typen für die einzelnen Komponenten
* Anzahl der VHDs einer DBMS-VM zur Bereitstellung ausreichender IOPS

## Verwalten von Azure-Assets

### Azure-Portal
Das Azure-Portal ist eine von drei Schnittstellen für die Verwaltung von Azure-VM-Bereitstellungen. Die grundlegenden Verwaltungsaufgaben, wie das Bereitstellen von VMs aus Images, können über das Azure-Portal ausgeführt werden. Darüber hinaus können im Azure-Portal auch Speicherkonten, virtuelle Netzwerke und andere Azure-Komponenten erstellt werden. Funktionen wie das Hochladen von VHDs vom lokalen Standort nach Azure oder das Kopieren von VHDs innerhalb von Azure sind allerdings Aufgaben, für die Tools von Drittanbietern bzw. Verwaltungstools wie PowerShell oder die Befehlszeilenschnittstelle erforderlich sind.
 
![Übersicht der virtuellen Computer im Microsoft Azure-Portal][planning-guide-figure-800]

[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>) 
[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Verwaltung und Konfiguration einer VM-Instanz können im Azure-Portal erfolgen.

Sie können hierbei VMs nicht nur neu starten und herunterfahren, sondern auch Datenträger für VM-Instanzen anfügen, trennen und erstellen, um die Instanzen zur Imagevorbereitung zu erfassen und die Größe der Instanzen zu konfigurieren.

Das Azure-Portal bietet grundlegende Funktionen für die Bereitstellung und Konfiguration von VMs und zahlreiche weitere Azure-Dienste. Jedoch stellt das Azure-Portal nicht alle verfügbaren Funktionen bereit. So können im Azure-Portal Aufgaben wie die Folgenden nicht ausgeführt werden:

* Hochladen von VHDs in Azure
* Kopieren von VMs

[comment]: <> (MShermannd TODO what about automation service for SAP VMs ? ) 
[comment]: <> (MSSedusch deployment of multiple VMs os meanwhile possible) 
[comment]: <> (MSSedusch Also any type of automation regarding deployment is not possible with the Azure portal. Tasks such as scripted deployment of multiple VMs is not possible via the Azure Portal.)

### Verwaltung mit Microsoft Azure PowerShell-Cmdlets
Windows PowerShell ist ein leistungsfähiges und erweiterbares Framework, das gerade von Kunden, die eine größere Anzahl von Systemen in Azure bereitstellen, sehr geschätzt wird. Nach der Installation der PowerShell-Cmdlets auf einem Desktop, Laptop oder einer dedizierten Verwaltungsstation können die Cmdlets remote ausgeführt werden.

Der Vorgang zum Aktivieren eines lokalen Desktops/Laptops für die Verwendung der Azure PowerShell-Cmdlets und deren Konfiguration für die Verwendung mit Azure-Abonnements wird in [diesem Artikel][powershell-install-configure] beschrieben.

Ausführlichere Schritte zum Installieren, Aktualisieren und Konfigurieren der Azure PowerShell-Cmdlets finden Sie auch in [diesem Kapitel des Bereitstellungshandbuchs][deployment-guide-4.1].

Bisherige Erfahrungen der Kunden zeigen PowerShell (PS) mit Sicherheit als das leistungsstärkere Tool für die Bereitstellung von VMs und die Erstellung benutzerdefinierter Schritte innerhalb der Bereitstellung von VMs. Alle Kunden, die SAP-Instanzen in Azure ausführen, verwenden PS-Cmdlets zur Ergänzung Ihrer Verwaltungsaufgaben im Azure-Portal bzw. sogar ausschließlich zur Verwaltung ihrer Bereitstellungen in Azure. Da für die Azure-spezifischen Cmdlets dieselben Namenskonventionen gelten wie für die mehr als 2000 Windows-spezifischen Cmdlets, fällt Windows-Administratoren die Übernahme dieser Cmdlets nicht schwer.

Einige Beispiele finden Sie hier: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO describe new CLI command when tested) 
Die Bereitstellung der Azure Monitoring Extension for SAP (siehe Kapitel [Azure-Überwachungslösung für SAP][planning-guide-9.1] in diesem Dokument) ist nur über PowerShell oder die Befehlszeilenschnittstelle möglich. Daher muss PowerShell oder die Befehlszeilenschnittstelle bei der Bereitstellung oder Verwaltung eines SAP NetWeaver-Systems in Azure installiert und konfiguriert werden.

Da Azure eine erweiterte Funktionalität bietet, werden neue PS-Cmdlets hinzugefügt, weshalb ein Update der Cmdlets erforderlich ist. Es ist daher durchaus sinnvoll, die Azure-Download-Website mindestens einmal pro Monat <https://azure.microsoft.com/downloads/> zu überprüfen, um nachzusehen, ob neue Cmdlet-Versionen verfügbar sind. Die neue Version wird einfach über die ältere Version installiert.

Eine allgemeine Liste der Azure PowerShell-Befehle finden Sie hier: <https://msdn.microsoft.com/library/azure/dn708514.aspx>.

### Verwaltung mit den Befehlen der Microsoft Azure-Befehlszeilenschnittstelle

Für Kunden, die Linux verwenden und Azure-Ressourcen verwalten möchten, steht PowerShell womöglich nicht zur Debatte. Als Alternative bietet Microsoft die Azure-Befehlszeilenschnittstelle. Die Azure-Befehlszeilenschnittstelle stellt eine Reihe von plattformübergreifenden Open Source-Befehlen für die Arbeit mit der Azure-Plattform bereit. Die Azure-Befehlszeilenschnittstelle bietet im Wesentlichen die gleiche Funktionalität wie das Azure-Portal.

Informationen zur Installation, Konfiguration und Verwendung der Befehle der Befehlszeilenschnittstelle für die Ausführung von Azure-Aufgaben finden Sie unter den folgenden Themen:

* [Installieren der Azure-Befehlszeilenschnittstelle][xplat-cli]
* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI][virtual-machines-linux-cli-deploy-templates]
* [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure-Ressourcen-Manager][xplat-cli-azure-resource-manager]

Weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle zum Bereitstellen der Azure Monitoring Extension for SAP finden Sie im [Bereitstellungshandbuch][planning-guide] im Kapitel [Azure-Befehlszeilenschnittstelle für Linux-VMs][deployment-guide-4.5.2].

## Verschiedene Möglichkeiten der Bereitstellung von VMs für SAP in Azure
In diesem Kapitel lernen Sie verschiedene Methoden der Bereitstellung von VMs in Azure kennen. Zudem werden weitere Vorbereitungsverfahren wie auch die Behandlung von VHDs und VMs in Azure beschrieben.

### Bereitstellung von VMs für SAP
Microsoft Azure bietet verschiedene Methoden der Bereitstellung von VMs und der zugehörigen Datenträger. Daher ist es wichtig, die Unterschiede zu kennen, da die Vorbereitung der VMs abhängig von der Bereitstellungsmethode ist. Insbesondere werfen wir einen Blick auf die folgenden Szenarien:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure
Sie planen, ein bestimmtes SAP-System vom lokalen Standort nach Azure zu verschieben. Dies kann erfolgen, indem Sie die VHD mit dem Betriebssystem und den SAP- und DBMS-Binärdateien sowie die VHDs mit den Daten- und Protokolldateien des DBMS in Azure hochladen. Im Gegensatz zu [Szenario 2 unten][planning-guide-5.1.2] werden Hostname, SAP-SID und SAP-Benutzerkonten auf der Azure-VM entsprechend der Konfiguration in der lokalen Umgebung beibehalten. Daher ist eine Generalisierung des Images nicht erforderlich. Lokale Vorbereitungsschritte sowie Informationen zum Hochladen nicht generalisierter VMs oder VHDs in Azure finden Sie in diesem Dokument im Kapitel [Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure][planning-guide-5.2.1]. Ausführliche Schritte zur Bereitstellung eines solchen Images in Azure finden Sie im [Bereitstellungshandbuch][deployment-guide] im Kapitel [Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP][deployment-guide-3.4].

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Bereitstellen eines virtuellen Computers mit einem kundenspezifischen Image
Aufgrund der spezifischen Patch-Anforderungen Ihrer Betriebssystem- bzw. DBMS-Version sind die im Azure Marketplace erhältlichen Images möglicherweise nicht für Ihren Bedarf geeignet. Daher muss möglicherweise anhand eines eigenen, benutzerdefinierten OS/DBMS-VM-Images ein virtueller Computer erstellt werden, der anschließend mehrmals bereitgestellt werden kann. Bei der Vorbereitung eines solchen „privaten“ Image für die Duplizierung müssen folgende Punkte berücksichtigt werden:

___

> ![Windows][Logo_Windows] Windows
>
> Die Windows-Einstellungen (wie die Windows-SID und der Hostname) müssen auf der lokalen VM mit dem folgenden sysprep-Befehl abstrahiert/generalisiert werden. 
[comment]: <> (MSSedusch > See more details here :) 
[comment]: <> (MShermannd TODO first link is about classic model. Didn't find an Azure docu article) 
[comment]: <> (MSSedusch > <https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>) 
[comment]: <> (MSSedusch > <http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
>
> ![Linux][Logo_Linux] Linux
>
> Führen Sie die in den folgenden Artikeln beschriebenen Schritte für [SUSE][virtual-machines-linux-create-upload-vhd-suse] oder [Red Hat][virtual-machines-linux-redhat-create-upload-vhd] aus, um eine VHD zum Hochladen in Azure vorzubereiten.

___

Wenn Sie auf Ihrer lokalen VM bereits SAP-Inhalte installiert haben (insbesondere bei zweischichtigen Systemen), können Sie die SAP-Systemeinstellungen nach der Bereitstellung der Azure-VM über das vom SAP Software Provisioning Manager unterstützte Verfahren zum Umbenennen von Instanzen anpassen (SAP-Hinweis [1619720]). Die lokalen Vorbereitungsschritte sowie Informationen zum Hochladen einer generalisierten VM in Azure finden Sie in diesem Dokument in den Kapiteln [Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP][planning-guide-5.2.2] und [Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure][planning-guide-5.3.2]. Eine ausführliche Beschreibung der Bereitstellungsschritte für ein solches Image in Azure finden Sie im [Bereitstellungshandbuch][deployment-guide] im Kapitel [Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP][deployment-guide-3.3].

#### Bereitstellen eines virtuellen Computers aus Azure Marketplace
Zur Bereitstellung Ihrer VM möchten Sie ein von Microsoft oder einem Drittanbieter bereitgestelltes VM-Image aus Azure Marketplace verwenden. Nachdem Sie Ihre VM in Azure bereitgestellt haben, verwenden Sie dieselben Richtlinien und Tools zum Installieren der SAP-Software und/oder des DBMS auf der VM wie in einer lokalen Umgebung. Eine ausführlichere Beschreibung der Bereitstellung finden Sie im [Bereitstellungshandbuch][deployment-guide] im Kapitel [Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP][deployment-guide-3.2].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Vorbereiten virtueller Computer mit SAP für Azure
Vor dem Hochladen von VMs in Azure müssen Sie sicherstellen, dass die VMs und VHDs bestimmte Anforderungen erfüllen. Abhängig von der Bereitstellungsmethode gibt es hierbei geringfügige Unterschiede.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure
Eine gängige Bereitstellungsmethode ist das Verschieben einer vorhandenen VM mit einem SAP-System vom lokalen Standort nach Azure. Diese VM und das SAP-System auf der VM sollten in Azure mit dem gleichen Hostnamen und vermutlich auch derselben SAP-SID ausgeführt werden. Das Gastbetriebssystem der VM sollte in diesem Fall nicht für mehrere Bereitstellungen generalisiert werden. Wenn das lokale Netzwerk in Azure erweitert wurde (siehe Kapitel [Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk][planning-guide-2.2] in diesem Dokument), können innerhalb der VM sogar die gleichen Domänenkonten verwendet werden wie zuvor lokal vor der Verschiebung.

Für die Vorbereitung Ihres eigenen Azure-VM-Datenträgers gelten folgende Anforderungen:

* Bislang war die VHD mit dem Betriebssystem auf eine Größe von maximal 127 GB beschränkt. Diese Einschränkung gilt seit Ende März 2015 nicht mehr. Die VHD mit dem Betriebssystem kann nun wie jede andere im Azure-Speicher bereitgestellte VHD bis zu 1 TB groß sein kann. 
[comment]: <> (MShermannd TODO have to check if CLI also converts to static )
* Das VHD-Format muss jedoch statisch sein. Dynamische VHDs oder VHDs im VHDx-Format werden von Azure noch nicht unterstützt. Dynamische VHDs werden beim Hochladen mit PowerShell-Cmdlets bzw. der Befehlszeilenschnittstelle in statische VHDs konvertiert.
* Auch auf der VM bereitgestellte VHDs, die in Azure weiterhin bereitgestellt werden sollen, müssen ein statisches VHD-Format aufweisen. Die Größeneinschränkung für Betriebssystem-Datenträger gilt auch für Datenträger mit ganz normalen Daten. Die Größe von VHDs ist auf 1 TB beschränkt. Dynamische VHDs werden beim Hochladen mit PowerShell-Cmdlets bzw. der Befehlszeilenschnittstelle in statische VHDs konvertiert.
* Solange die VM noch nicht vollständig bereitgestellt ist und keine geeigneteren Benutzerkonten zur Verfügung stehen, sollten Sie ein neues lokales Konto mit Administratorrechten hinzufügen, das vom Microsoft-Support verwendet bzw. als Kontext für die Ausführung von Diensten und Anwendungen zugewiesen werden kann, bis der virtuelle Computer bereitgestellt ist und geeignetere Benutzer verwendet werden können.
* In einem reinen Cloud-Bereitstellungsszenario (siehe Kapitel [Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk][planning-guide-2.1] in diesem Dokument) in Kombination mit dieser Bereitstellungsmethode funktionieren Domänenkonten möglicherweise nicht mehr, sobald der Azure-Datenträger in Azure bereitgestellt ist. Dies gilt insbesondere für Konten, die zum Ausführen von Diensten wie DBMS- oder SAP-Anwendungen verwendet werden. Daher müssen Sie solche Domänenkonten durch lokale VM-Konten ersetzen und die lokalen Domänenkonten auf der VM löschen. Die Beibehaltung der lokalen Domänenbenutzer im VM-Image ist kein Problem, wenn die VM im standortübergreifenden Szenario bereitgestellt wird, das in diesem Dokument in Kapitel [Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk][planning-guide-2.2] beschrieben wird.
* Wenn die Domänenkonten bei der lokalen Ausführung des Systems als DBMS-Anmeldungen bzw. -Benutzer verwendet wurden und diese VMs nun in reinen Cloud-Szenarien bereitgestellt werden sollen, müssen diese Domänenbenutzer hingegen gelöscht werden. Stellen Sie sicher, dass der lokale Administrator sowie ein weiterer lokaler VM-Benutzer im DBMS als Anmeldung/Benutzer mit Administratorrechten hinzugefügt werden.
* Fügen Sie weitere lokale Konten hinzu, wie sie für die jeweiligen Bereitstellungsszenarien erforderlich sind.

___

> ![Windows][Logo_Windows] Windows
>
> In diesem Szenario ist für das Upload und die Bereitstellung der VM in Azure keine Generalisierung (sysprep) der VM erforderlich. Stellen Sie sicher, dass Laufwerk D:\\ nicht verwendet wird. Legen Sie dann die automatische Bereitstellung für angefügte Datenträger fest, wie in diesem Dokument in Kapitel [Festlegen der automatischen Bereitstellung für angefügte Datenträger][planning-guide-5.5.3] beschrieben.
> 
> ![Linux][Logo_Linux] Linux
>
> In diesem Szenario ist für das Upload und die Bereitstellung der VM in Azure keine Generalisierung (waagent -deprovision) der VM erforderlich. Stellen Sie sicher, dass /mnt/resource nicht verwendet wird und alle Datenträger über uuid bereitgestellt wurden. Stellen Sie für den Betriebssystem-Datenträger sicher, dass im Bootloader-Eintrag auch die Uuid-basierte Bereitstellung enthalten ist.

___

#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP
VHD-Dateien, die ein generalisiertes Betriebssystem enthalten, werden in Azure-Speicherkonten auch in Containern gespeichert. Von einer solchen Image-VHD können Sie eine neue VM bereitstellen, indem Sie in Ihren Vorlagendateien für die Bereitstellung als Quellen-VHD auf die VHD verweisen, wie im [Bereitstellungshandbuch][deployment-guide] im Kapitel [Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP][deployment-guide-3.3] beschrieben.

Für die Vorbereitung Ihres eigenen Azure-VM-Image gelten folgende Anforderungen:

* Bislang war die VHD mit dem Betriebssystem auf eine Größe von maximal 127 GB beschränkt. Diese Einschränkung gilt seit Ende März 2015 nicht mehr. Die VHD mit dem Betriebssystem kann nun wie jede andere im Azure-Speicher bereitgestellte VHD bis zu 1 TB groß sein kann. 
[comment]: <> (MShermannd TODO have to check if CLI also converts to static )
* Das VHD-Format muss jedoch statisch sein. Dynamische VHDs oder VHDs im VHDx-Format werden von Azure noch nicht unterstützt. Dynamische VHDs werden beim Hochladen mit PowerShell-Cmdlets bzw. der Befehlszeilenschnittstelle in statische VHDs konvertiert.
* Auch auf der VM bereitgestellte VHDs, die in Azure weiterhin bereitgestellt werden sollen, müssen ein statisches VHD-Format aufweisen. Die Größeneinschränkung für Betriebssystem-Datenträger gilt auch für Datenträger mit ganz normalen Daten. Die Größe von VHDs ist auf 1 TB beschränkt. Dynamische VHDs werden beim Hochladen mit PowerShell-Cmdlets bzw. der Befehlszeilenschnittstelle in statische VHDs konvertiert.
* Da die auf der VM als Benutzer registrierten Domänenbenutzer in einem reinen Cloud-Bereitstellungsszenario (siehe Kapitel [Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk][planning-guide-2.1] in diesem Dokument) nicht vorhanden sind, funktionieren Dienste, die diese Domänenkonten nutzen, möglicherweise nicht mehr, sobald das Image in Azure bereitgestellt ist. Dies gilt insbesondere für Konten, die zum Ausführen von Diensten wie DBMS- oder SAP-Anwendungen verwendet werden. Daher müssen Sie solche Domänenkonten durch lokale VM-Konten ersetzen und die lokalen Domänenkonten auf der VM löschen. Die Beibehaltung der lokalen Domänenbenutzer im VM-Image stellt vermutlich kein Problem dar, wenn die VM im standortübergreifenden Szenario bereitgestellt wird, das in diesem Dokument im Kapitel [Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk][planning-guide-2.2] beschrieben wird.
* Solange die VM noch nicht vollständig bereitgestellt ist und keine geeigneteren Benutzerkonten zur Verfügung stehen, sollten Sie ein neues lokales Konto mit Administratorrechten hinzufügen, das im Falle von Problemen vom Microsoft-Support zur Ermittlung der Fehlerursache verwendet bzw. als Kontext für die Ausführung von Diensten und Anwendungen zugewiesen werden kann, bis der virtuelle Computer bereitgestellt ist und geeignetere Benutzer verwendet werden können.
* Bei reinen Cloud-Bereitstellungen bzw. wenn die Domänenkonten bei der lokalen Ausführung des Systems als DBMS-Anmeldungen bzw. -Benutzer verwendet wurden, müssen diese Domänenbenutzer hingegen gelöscht werden. Stellen Sie sicher, dass der lokale Administrator sowie ein weiterer lokaler VM-Benutzer als Anmeldung/Benutzer des DBMS mit Administratorrechten hinzugefügt werden.
* Fügen Sie weitere lokale Konten hinzu, wie sie für die jeweiligen Bereitstellungsszenarien erforderlich sind.
* Wenn das Image eine Installation von SAP NetWeaver enthält und es wahrscheinlich ist, dass der Hostname bei der Bereitstellung in Azure geändert wird, empfiehlt es sich, die neuesten Versionen der SAP Software Provisioning Manager-DVD in die Vorlage zu kopieren. Sie können dann problemlos auf die von SAP bereitgestellte Umbenennungsfunktion zurückgreifen, um den geänderten Hostnamen anzupassen und/oder die SID des SAP-Systems im bereitgestellten VM-Image zu ändern, sobald eine neue Kopie gestartet wird.

___

> ![Windows][Logo_Windows] Windows
>
> Stellen Sie sicher, dass Laufwerk D:\\ nicht verwendet wird. Legen Sie dann die automatische Bereitstellung für angefügte Datenträger fest, wie in diesem Dokument in Kapitel [Festlegen der automatischen Bereitstellung für angefügte Datenträger][planning-guide-5.5.3] beschrieben.
> 
> ![Linux][Logo_Linux] Linux
>
> Stellen Sie sicher, dass /mnt/resource nicht verwendet wird und alle Datenträger über uuid bereitgestellt wurden. Stellen Sie für den Betriebssystem-Datenträger sicher, dass im Bootloader-Eintrag auch die Uuid-basierte Bereitstellung enthalten ist.

___

* Die SAP-GUI (für Verwaltungs- und Setup-Aufgaben) kann in einer solchen Vorlage vorinstalliert sein.
* Auch andere Programme, die für die erfolgreiche Ausführung der VMs in standortübergreifenden Szenarien erforderlich sind, können installiert werden, solange diese die Umbenennung der VM unterstützen.

Wenn die VM ausreichend vorbereitet ist, um generisch und letztendlich unabhängig von Konten bzw. Benutzern zu sein, die im anvisierten Azure-Bereitstellungsszenario nicht verfügbar sind, kann der letzte Vorbereitungsschritt für die Generalisierung eines solchen Image durchgeführt werden.

##### Generalisieren eines virtuellen Computers 

___

[comment]: <> (MShermannd TODO have to find better articles / docu about generalizing the VMs for ARM )
> ![Windows][Logo_Windows] Windows
>
> Der letzte Schritt ist die Anmeldung bei einer VM mit einem Administratorkonto. Öffnen Sie als Administrator ein Windows-Befehlsfenster. Wechseln Sie zu „…\\windows\\system32\\sysprep“, und führen Sie „sysprep.exe“ aus. Daraufhin wird ein kleines Fenster geöffnet. Aktivieren Sie hier das Kontrollkästchen „Verallgemeinern“ (das standardmäßig deaktiviert ist), und ändern Sie die Option für das Herunterfahren von der Standardeinstellung „Neu starten“ in „Herunterfahren“ Dieses Verfahren setzt voraus, dass der sysprep-Prozess lokal auf dem Gastbetriebssystem einer VM ausgeführt wird. Wenn Sie diesen Vorgang für eine bereits in Azure ausgeführte VM durchführen möchten, folgen Sie den in [diesem Artikel][virtual-machines-windows-capture-image] beschriebenen Schritten.
> 
> ![Linux][Logo_Linux] Linux
>
> [Erfassen eines virtuellen Linux-Computers zur Verwendung als Ressourcen-Manager-Vorlage][virtual-machines-linux-capture-image]

___

### Übertragen von virtuellen Computern und VHDs vom lokalen Standort nach Azure
Zum Hochladen von VM-Images und -Datenträgern in Azure ist das Azure-Portal ungeeignet. Stattdessen verwenden Sie hierzu die PowerShell-Cmdlets bzw. die Befehlszeilenschnittstelle von Azure. Ebenso können Sie das Tool „AzCopy“ verwenden. Das Tool kann VHDs in beiden Richtungen zwischen lokalem Standort und Azure kopieren. Mit ihm können Sie auch VHDs zwischen Azure-Regionen kopieren. Download und Verwendung von AzCopy werden in [dieser Dokumentation][storage-use-azcopy] beschrieben.

Eine weitere Alternative sind die verschiedenen auf dem Markt erhältlichen GUI-ähnlichen Tools von Drittanbietern. Vor der Verwendung eines solchen Tools sollten Sie jedoch sicherstellen, dass das Tool Azure-Seitenblobs unterstützt. Für unsere Zwecke benötigen wir Azure-Seitenblobspeicher (eine Beschreibung der Unterschiede finden Sie hier: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Mit den von Azure bereitgestellten Tools lassen sich die VMs und VHDs vor dem Upload auch sehr effizient komprimieren. Dadurch reduziert sich die zum Hochladen benötigte Zeit (die ohnehin schon je nach Upload-Link zum Internet zwischen dem lokalen Standort und der gewünschten Azure-Bereitstellungsregion variiert). Es ist leicht nachzuvollziehen, dass das Hochladen von VMs oder VHDs von einem europäischen Standort in ein Azure-Rechenzentrum in den USA länger dauert als das Hochladen derselben VMs/VHDs in ein Azure-Rechenzentrum in Europa.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure
Eine vorhandene VM oder VHD, die Sie aus dem lokalen Netzwerk hochladen möchten, muss die Anforderungen erfüllen, die in diesem Dokument im Kapitel [Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure][planning-guide-5.2.1] beschrieben werden.

Eine solche VM muss nicht generalisiert werden und kann in dem Zustand hochgeladen werden, den sie nach dem Herunterfahren am lokalen Standort hat. Das Gleiche gilt auch für zusätzliche VHDs, die kein Betriebssystem enthalten.

##### Hochladen einer VHD und Konvertieren in einen Azure-Datenträger
In diesem Fall möchten wir eine VHD mit oder ohne Betriebssystem hochladen und sie als Datenträger für Daten oder das Betriebssystem auf einer VM bereitstellen. Dies ist ein mehrstufiger Prozess.

__PowerShell__

* Melden Sie sich mit _Login-AzureRmAccount_ bei Ihrem Abonnement an.
* Legen Sie das Abonnement Ihres Kontexts mit _Set-AzureRmContext_ und dem Parameter SubscriptionId oder SubscriptionName fest (siehe <https://msdn.microsoft.com/library/mt619263.aspx>).
* Laden Sie die VHD mit _Add-AzureRmVhd_ in ein Azure-Speicherkonto hoch (siehe <https://msdn.microsoft.com/library/mt603554.aspx>).
* Legen Sie die VHD mit _Set-AzureRmVMOSDisk_ als Betriebssystem-Datenträger einer neuen VM-Konfiguration fest (siehe <https://msdn.microsoft.com/library/mt603746.aspx>).
* Erstellen Sie aus der VM-Konfiguration mit _New-AzureRmVM_ eine neue VM (siehe <https://msdn.microsoft.com/library/mt603754.aspx>).
* Fügen Sie einen Datenträger für Daten mit _Add-AzureRmVMDataDisk_ einer neuen VM hinzu (siehe <https://msdn.microsoft.com/library/mt603673.aspx>).

__Azure-Befehlszeilenschnittstelle__

* Wechseln Sie mit _azure config mode arm_ in den Azure Resource Manager-Modus.
* Melden Sie sich mit _azure login_ bei Ihrem Abonnement an.
* Wählen Sie Ihr Abonnement mit _azure account set `<subscription name or id`>_ aus.
* Laden Sie die VHD mit _azure storage blob upload_ hoch (siehe [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher][storage-azure-cli]).
* Erstellen Sie mit _azure vm create_ und dem Parameter „-d“ eine neue VM, wobei Sie die hochgeladene VHD als Betriebssystem-Datenträger angeben.
* Fügen Sie einen Datenträger für Daten mit _vm disk attach-new_ einer neuen VM hinzu.

__Vorlage__

* Hochladen der VHD mit PowerShell oder der Azure-Befehlszeilenschnittstelle
* Stellen Sie den virtuellen Computer, wie in [dieser Beispiel-JSON-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json) dargestellt, mit einer JSON-Vorlage bereit, die auf die VHD verweist.

#### Bereitstellen eines VM-Image
Eine vorhandene VM oder VHD, die Sie aus dem lokalen Netzwerk hochladen möchten, um sie als Azure-VM-Image zu verwenden, muss die Anforderungen erfüllen, die in diesem Dokument im Kapitel [Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP][planning-guide-5.2.2] beschrieben werden.

* Verwenden Sie _sysprep_ (unter Windows) bzw. _waagent -deprovision_ (unter Linux), um die VM zu generalisieren (siehe [Erfassen eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell][virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] bzw. [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage][virtual-machines-linux-capture-image-capture]).
* Melden Sie sich mit _Login-AzureRmAccount_ bei Ihrem Abonnement an.
* Legen Sie das Abonnement Ihres Kontexts mit _Set-AzureRmContext_ und dem Parameter SubscriptionId oder SubscriptionName fest (siehe <https://msdn.microsoft.com/library/mt619263.aspx>).
* Laden Sie die VHD mit _Add-AzureRmVhd_ in ein Azure-Speicherkonto hoch (siehe <https://msdn.microsoft.com/library/mt603554.aspx>).
* Legen Sie die VHD mit _Set-AzureRmVMOSDisk -SourceImageUri -CreateOption fromImage_ als Betriebssystem-Datenträger einer neuen VM-Konfiguration fest (siehe <https://msdn.microsoft.com/library/mt603746.aspx>).
* Erstellen Sie aus der VM-Konfiguration mit _New-AzureRmVM_ eine neue VM (siehe <https://msdn.microsoft.com/library/mt603754.aspx>).

__Azure-Befehlszeilenschnittstelle__

* Verwenden Sie _sysprep_ (unter Windows) bzw. _waagent -deprovision_ (unter Linux), um die VM zu generalisieren (siehe [Erfassen eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell][virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] bzw. [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage][virtual-machines-linux-capture-image-capture] für Linux).
* Wechseln Sie mit _azure config mode arm_ in den Azure Resource Manager-Modus.
* Melden Sie sich mit _azure login_ bei Ihrem Abonnement an.
* Wählen Sie Ihr Abonnement mit _azure account set `<subscription name or id`>_ aus.
* Laden Sie die VHD mit _azure storage blob upload_ hoch (siehe [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher][storage-azure-cli]).
* Erstellen Sie mit _azure vm create_ und dem Parameter „-Q“ eine neue VM, wobei Sie die hochgeladene VHD als Betriebssystem-Datenträger angeben.

__Vorlage__

* Verwenden Sie _sysprep_ (unter Windows) bzw. _waagent -deprovision_ (unter Linux), um die VM zu generalisieren (siehe [Erfassen eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell][virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] bzw. [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage][virtual-machines-linux-capture-image-capture] für Linux).
* Hochladen der VHD mit PowerShell oder der Azure-Befehlszeilenschnittstelle
* Stellen Sie die VM, wie in [dieser Beispiel-JSON-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json) gezeigt, mit einer JSON-Vorlage bereit, die die Image-VHD referenziert.

#### Herunterladen von VHDs an den lokalen Standort
Azure Infrastructure as a Service (IaaS) ist keine Einbahnstraße, auf der Sie VHDs und SAP-Systeme nur in eine Richtung hochladen können. Vielmehr können Sie Ihre SAP-Systeme auch wieder aus Azure zurück an Ihren lokalen Standort übertragen.

Während des Downloads dürfen die VHDs allerdings nicht aktiv sein. Selbst wenn Sie VHDs herunterladen, die auf VMs bereitgestellt werden, müssen Sie diese VMs herunterfahren. Wenn Sie nur den Inhalt der Datenbank herunterladen möchten, um daraus lokal ein neues System einzurichten, und es akzeptabel ist, dass das System während des Downloads und der Einrichtung des neuen Systems in Azure betriebsbereit bleibt, können Sie eine lange Ausfallzeit vermeiden, indem Sie eine komprimierte Datenbanksicherung auf einer VHD erstellen und anschließend anstatt der VM mit der Betriebssystembasis nur diese VHD herunterladen.

#### PowerShell

Wenn das SAP-System beendet und die VM heruntergefahren sind, können Sie die VHD mit dem PowerShell-Cmdlet „Save-AzureRmVhd“ (ausgeführt auf dem lokalen Ziel) zurück in die lokale Umgebung herunterladen. Sie benötigen dazu die URL der VHD, die Sie im Abschnitt „Speicher“ des Azure-Portal finden (navigieren Sie zum Speicherkonto und dann zu dem Speichercontainer, in dem die VHD erstellt wurde). Außerdem müssen Sie angeben, wohin die VHD kopiert werden soll.

Sie geben dann den Befehl einfach mit den Parametern „SourceUri“ (URL der herunterzuladenden VHD) und „LocalFilePath“ (physischer Zielspeicherort der VHD auf dem lokalen System einschließlich ihres Namens) ein. Der Befehl kann wie folgt aussehen:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Weitere Informationen zum Cmdlet Save-AzureRmVhd finden Sie hier: <https://msdn.microsoft.com/library/mt622705.aspx>.

#### Befehlszeilenschnittstelle (CLI)

Wenn das SAP-System beendet und die VM heruntergefahren sind, können Sie die VHD mit dem Befehl „azure storage blob download“ der Azure-Befehlszeilenschnittstelle (ausgeführt auf dem lokalen Ziel) zurück in die lokale Umgebung herunterladen. Sie benötigen dazu den Namen und den Container der VHD, die Sie im Abschnitt „Speicher“ des Azure-Portal finden (navigieren Sie zum Speicherkonto und dann zu dem Speichercontainer, in dem die VHD erstellt wurde). Außerdem müssen Sie angeben, wohin die VHD kopiert werden soll.

Sie geben dann den Befehl einfach mit den Parametern „blob“ und „container“ der herunterzuladenden VHD sowie „destination“ (physischer Zielspeicherort der VHD auf dem lokalen System einschließlich ihres Namens) ein. Der Befehl kann wie folgt aussehen:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download> 
```

### Übertragen von VMs und VHDs innerhalb von Azure

#### Kopieren von SAP-Systemen innerhalb von Azure

Ein SAP-System, ja selbst ein dedizierter DBMS-Server, der eine SAP-Anwendungsebene unterstützt, besteht vermutlich aus mehreren VHDs, die das Betriebssystem mit den Binärdateien sowie die Daten- und Protokolldateien der SAP-Datenbank enthalten. Weder die Azure-Funktionalität des Kopierens von VHDs noch die Azure-Funktionalität des Speicherns von VHDs auf Festplatte verfügen jedoch über einen Synchronisierungsmechanismus, der mehrere VHDs synchron abbilden kann. Daher hätten die kopierten oder gespeicherten VHDs, selbst wenn diese auf derselben VM bereitgestellt werden, wohl kaum einen identischen Status. Wenn aber die verschiedenen VHDs unterschiedliche Daten- und Protokolldateien enthalten, wäre die Datenbank letztendlich inkonsistent.

**Zusammenfassung: Zum Kopieren oder Speichern der VHDs einer SAP-Systemkonfiguration müssen Sie das SAP-System beenden und die für das SAP-System bereitgestellte VM herunterfahren. Erst danach können Sie den VHD-Satz mit den Systemdateien kopieren oder herunterladen, um eine Kopie des SAP-Systems in Azure oder am lokalen Standort zu erstellen.**

Datenträger mit Daten werden als VHD-Dateien in einem Azure-Speicherkonto gespeichert und können direkt an eine VM angefügt oder als Image verwendet werden. Die VHD wird in diesem Fall vor dem Anfügen an die VM an einen anderen Speicherort kopiert. Der vollständige Name der VHD-Datei muss innerhalb von Azure eindeutig sein. Wie bereits erwähnt besteht der Name wie folgt aus drei Teilen:

	http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### PowerShell
Wie in [diesem Artikel][storage-powershell-guide-full-copy-vhd] beschrieben, können Sie VHDs mit den Azure PowerShell-Cmdlets kopieren.

##### Befehlszeilenschnittstelle (CLI)
Wie in [diesem Artikel][storage-azure-cli-copy-blobs] beschrieben, können Sie VHDs über die Befehlszeilenschnittstelle von Azure kopieren.

##### Azure Storage-Tools

* <http://azurestorageexplorer.codeplex.com/releases/view/125870>

Professionelle Editions von Azure Storage Explorer finden Sie hier:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>


Das Kopieren einer VHD innerhalb eines Speicherkontos ist ein rascher Prozess, der (wie das Erstellen eines Snapshots mit „lazy copy“ und „copy on write“) nur wenige Sekunden dauert. Sobald Sie eine solche Kopie erstellt haben, können Sie sie direkt an eine VM anfügen bzw. als Image verwenden, um sie in dieser Form als Kopie an VMs anzufügen.

##### PowerShell

```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### Befehlszeilenschnittstelle (CLI)
```
azure config mode arm 

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopieren von Datenträgern zwischen Azure-Speicherkonten
Diese Aufgabe kann nicht im Azure-Portal ausgeführt werden. Verwenden Sie stattdessen die Azure-spezifischen PowerShell-Cmdlets, die Befehlszeilenschnittstelle von Azure oder einen Speicher-Browser eines Drittanbieters. Mit den PowerShell-Cmdlets bzw. den Befehlen der Befehlszeilenschnittstelle können Sie Blobs erstellen und verwalten. Dies schließt auch die Möglichkeit des asynchronen Kopierens von Blobs zwischen Speicherkonten und Regionen innerhalb des Azure-Abonnements ein.

##### PowerShell 

Hier können VHDs auch zwischen verschiedenen Abonnements kopiert werden. Weitere Informationen finden Sie [in diesem Artikel][storage-powershell-guide-full-copy-vhd].

Die grundlegende Abfolge der Logik des PS-Cmdlets sieht wie folgt aus:

* Erstellen Sie mit _New-AzureStorageContext_ einen Speicherkontokontext für das Quellspeicherkonto (siehe <https://msdn.microsoft.com/library/dn806380.aspx>).
* Erstellen Sie mit _New-AzureStorageContext_ einen Speicherkontokontext für das Zielspeicherkonto (siehe <https://msdn.microsoft.com/library/dn806380.aspx>).
* Starten Sie die Kopie mit:

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Überprüfen Sie den Status der Kopie in einer Schleife mit:
 
```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Fügen Sie die neue VHD an eine VM an, wie oben beschrieben.

Beispiele finden Sie in [diesem Artikel][storage-powershell-guide-full-copy-vhd].

##### Befehlszeilenschnittstelle (CLI)
* Starten Sie die Kopie mit:

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Überprüfen Sie den Status der Kopie in einer Schleife mit:

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```
  
* Fügen Sie die neue VHD an eine VM an, wie oben beschrieben.

Beispiele finden Sie in [diesem Artikel][storage-azure-cli-copy-blobs].

### Behandlung von Datenträgern
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>VM-/VHD-Struktur für SAP-Bereitstellungen
Im Idealfall sollten die Verfahren in Verbindung mit VM-Strukturen und den zugehörigen VHDs einfach sein. In lokalen Installationen finden die verschiedensten kundenspezifischen Strukturierungskonzepte für Serverinstallationen Anwendung.

* Eine Basis-VHD mit dem Betriebssystem und allen Binärdateien des DBMS- und/oder SAP-Systems. Seit März 2015 gilt für diese VHD ein Größenlimit von 1 TB (statt bislang 127 GB).
* Eine oder mehrere VHDs mit der DBMS-Protokolldatei der SAP-Datenbank sowie der Protokolldatei des temporären DBMS-Speicherbereichs (sofern vom DBMS unterstützt). Wenn die Anforderungen an die Datenbankprotokoll-IOPS hoch sind, müssen Sie das System auf mehrere VHDs aufteilen, um das erforderliche IOPS-Volumen zu erreichen.
* Mehrere VHDs mit jeweils einer oder zwei Datenbankdateien der SAP-Datenbank sowie den temporären DBMS-Datendateien (sofern vom DBMS unterstützt).

![Verweiskonfiguration der Azure-IaaS-VM für SAP][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO describe Linux structure )

___

> ![Windows][Logo_Windows] Windows
>
> Bei vielen Kunden haben wir Konfigurationen gesehen, in denen beispielsweise die SAP- und DBMS-Binärdateien nicht auf Laufwerk C:\\ mit dem Betriebssystem installiert waren. Hierfür wurden die verschiedensten Gründe genannt, gingen wir der Sache jedoch auf den Grund, stellte sich meist heraus, dass die Laufwerke vor 10 oder 15 Jahren zu klein wurden und Betriebssystemupdates einfach mehr Speicherplatz erforderten. Diese Bedingungen treffen heute aber kaum mehr zu. Heute kann Laufwerk C:\\ Festplatten oder VMs mit großen Kapazitäten zugeordnet werden. Um die Bereitstellungsstruktur möglichst einfach zu halten, empfehlen wir für SAP NetWeaver-Systeme in Azure das folgende Bereitstellungsmuster:
>
> Die Auslagerungsdatei für das Betriebssystem Windows sollte auf Laufwerk D: (nicht persistenter Datenträger) eingerichtet werden.
> 
> ![Linux][Logo_Linux] Linux
>
> Die Auslagerungsdatei für Linux sollte unter „/mnt /mnt/resource“ eingerichtet werden, wie in [diesem Artikel][virtual-machines-linux-agent-user-guide] beschrieben. Die Auslagerungsdatei kann in der Konfigurationsdatei „/etc/waagent.conf“ des Linux-Agenten konfiguriert werden. Nehmen Sie die folgenden Einstellungen vor:

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Zur Aktivierung der Änderungen müssen Sie den Linux-Agenten neu starten:

```
sudo service waagent restart
```

Informationen zur empfohlenen Größe der Auslagerungsdatei finden Sie in SAP-Hinweis [1597355].

___

Wie viele VHDs Sie für Ihre DBMS-Datendateien benötigen und der Typ des Azure-Speichers, auf denen diese VHDs bereitgestellt werden, richtet sich nach den IOPS-Anforderungen und der benötigten Latenz. Die genauen Zahlen werden in [diesem Artikel][virtual-machines-sizes] genannt.

Aus den letzten zwei Jahren der SAP-Bereitstellung ziehen wir einige Rückschlüsse, die wir Ihnen wie folgt zusammenfassen möchten:

* Der IOPS-Datenverkehr mit den verschiedenen Datendateien ist nicht immer identisch, da die Datendateien der bestehenden Kundensysteme je nach SAP-Datenbank(en) völlig unterschiedlich dimensioniert sein können. Für die Platzierung der aus den Datendateien gebildeten LUNs erwies sich daher eine RAID-Konfiguration gegenüber mehreren VHDs als wesentlich sinnvoller. Es gab Situationen, insbesondere bei Verwendung des Azure-Standardspeichers, in denen allein die IOPS-Rate das Kontingent einer einzelnen VHD für das DBMS-Transaktionsprotokoll sprengte. In solchen Szenarien empfiehlt sich die Verwendung des Premiumspeichers oder auch mehrerer Standardspeicher-VHDs mit Software-RAID.

___

> ![Windows][Logo_Windows] Windows
>
> * [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure][virtual-machines-sql-server-performance-best-practices]
> 
> ![Linux][Logo_Linux] Linux
>
> * [Konfigurieren von Software-RAID unter Linux][virtual-machines-linux-configure-raid]
> * [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure][virtual-machines-linux-configure-lvm]
> * [Geheime Schlüssel für Azure Storage und E/A-Optimierungen unter Linux](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)

___

* Der Premiumspeicher zeigt eine wesentlich bessere Leistung, insbesondere bei kritischen Schreibzugriffen auf Transaktionsprotokolle. In SAP-Szenarios, von denen eine produktionsähnliche Leistung erwartet wird, empfiehlt sich die Verwendung von VM-Series, das Azure-Premiumspeicher nutzen kann.

Beachten Sie auch, dass die VHD mit dem Betriebssystem und, wie wir empfehlen, mit den SAP-Binärdateien und möglichst auch der Datenbank (Basis-VM) nicht mehr auf 127 GB begrenzt ist. Diese VHD kann nun bis zu 1 TB groß sein. Dies sollte als Speicher für alle erforderlichen Dateien, einschließlich z.B. der SAP-Batchjobprotokolle, ausreichen.

Weitere Empfehlungen und Details, insbesondere für DBMS-VMs, finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide].


#### Behandlung von Datenträgern
In den meisten Szenarien müssen Sie zusätzliche Datenträger erstellen, um die SAP-Datenbank auf der VM bereitzustellen. Über die Anzahl der VHDs haben wir bereits im Kapitel [VM-/VHD-Struktur für SAP-Bereitstellungen][planning-guide-5.5.1] dieses Dokuments gesprochen. Nach der Bereitstellung der Basis-VM können Sie über das Azure-Portal weitere Datenträger anfügen und trennen. Das Anfügen und Trennen der Datenträger kann unabhängig vom Betriebszustand der VM erfolgen, also auch dann, wenn die VM heruntergefahren ist. Im Azure-Portal haben Sie sowohl die Möglichkeit, einen leeren Datenträger anzufügen, als auch einen bereits bestehenden, der zu diesem Zeitpunkt allerdings an keiner anderen VM angefügt sein darf.

**Hinweis**: VHDs können jeweils immer nur einer VM angefügt sein.
 
![Verbinden/Trennen von Datenträgern und dem Azure-Standardspeicher][planning-guide-figure-1400]

Sie müssen also entscheiden, ob Sie eine neue und leere VHD erstellen wollen (unter dem gleichen Speicherkonto wie die Basis-VM) oder ob Sie eine vorhandene VHD verwenden möchten, die bereits hochgeladen wurde und nun an die VM angefügt werden soll.

**WICHTIG**: Bei Verwendung des Azure-Standardspeichers können Sie **KEINE** Hostzwischenspeicherung verwenden. Übernehmen Sie daher unverändert den Standardwert NONE der Host-Cache-Einstellung. Bei Verwendung des Azure-Premiumspeichers sollten Sie den Lese-Cache aktivieren, wenn Ihre E/As in erster Linie aus Lesezugriffen bestehen, wie es für den E/A-Datenverkehr bei den Datendateien einer Datenbank üblich ist. Im Falle der Transaktionsprotokolldatei einer Datenbank wird von jeder Art des Caching abgeraten.

___

> ![Windows][Logo_Windows] Windows
>
> [Anfügen eines Datenträgers für Daten im Azure-Portal][virtual-machines-windows-attach-disk-portal]
>
> Falls der VM bereits Datenträger angefügt sind, müssen Sie sich an der VM anmelden, um den Windows Disk Manager zu öffnen. Wenn die automatische Bereitstellung für angefügte Datenträger nicht aktiviert ist, wie im Kapitel [Festlegen der automatischen Bereitstellung für angefügte Datenträger][planning-guide-5.5.3] empfohlen, muss das neu angefügte Volume online geschaltet und initialisiert werden.
>
> ![Linux][Logo_Linux] Linux
>
> Falls der VM bereits Datenträger angefügt sind, müssen Sie sich bei der VM anmelden und die Datenträger initialisieren, wie in [diesem Artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux] beschrieben.

___

Außerdem müssen Sie den neuen Datenträger, wenn er noch leer ist, formatieren. Für die Formatierung gelten insbesondere für DBMS-Daten- und Protokolldateien dieselben Empfehlungen wie für Bare-Metal-Bereitstellungen des DBMS.

Wie bereits im Kapitel [Das Konzept der virtuellen Microsoft Azure-Computer][planning-guide-3.2] erwähnt, kann ein Azure-Speicherkonto nicht unbegrenzt Ressourcen in puncto E/A-Volumen, IOPS und Datenvolumen bereitstellen. DBMS-VMs sind in der Regel am meisten davon betroffen. Falls Sie nur wenige VMs mit großem E/A-Volumen bereitstellen müssen, sollten Sie möglichst für jede VM ein eigenes Speicherkonto einrichten, damit Sie innerhalb des Größenlimits für Azure-Speicherkonten bleiben. Andernfalls müssen Sie versuchen, diese VMs auf die verschiedenen Speicherkonten aufzuteilen, ohne die Grenzen einzelner Speicherkonten zu sprengen. Nähere Einzelheiten hierzu finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide]. Diese Einschränkungen sollten Sie auch bei reinen SAP-Anwendungsserver-VMs oder anderen VMs bedenken, denen vermutlich irgendwann einmal weitere VHDs hinzugefügt werden müssen.

Ein weiterer wichtiger Punkt, der bei Speicherkonten bedacht werden sollte, ist, ob die VHDs des Speicherkontos geografisch repliziert werden. Die Georeplikation wird auf Ebene des Speicherkontos, nicht auf VM-Ebene aktiviert oder deaktiviert. Wenn die Georeplikation aktiviert ist, werden die VHDs eines Speicherkontos an ein anderes Azure-Rechenzentrum innerhalb derselben Region repliziert. Bei Ihrer Entscheidung sollten Sie die folgende Einschränkung berücksichtigen:

Die Georeplikation von Azure erfolgt lokal und individuell für jede VHD einer VM, d.h., die E/As werden nicht in chronologischer Reihenfolge über mehrere VHDs einer VM repliziert. Das bedeutet, dass die VHD mit der Basis-VM sowie alle weiteren VHDs, die an der VM angefügt sind, unabhängig voneinander repliziert werden. Es findet also keine Synchronisierung zwischen den Änderungen innerhalb der verschiedenen VHDs statt. Da die E/As unabhängig von der Reihenfolge repliziert werden, in der sie geschrieben wurden, ist die Georeplikation für Datenbankserver, deren Datenbanken über mehrere VHDs verteilt sind, völlig ungeeignet. Neben DBMS gibt es auch andere Anwendungen, deren Prozesse Daten auf mehreren VHDs schreiben oder bearbeiten, wobei die Reihenfolge der Änderungen erhalten bleiben muss. Falls dies eine Voraussetzung ist, sollte die Georeplikation in Azure nicht aktiviert werden. Wenn Sie wissen, dass Sie für bestimmte VMs Georeplikation benötigen, für andere hingegen nicht, können Sie diese VMs und deren VHDs bereits unter verschiedenen Speicherkonten einrichten, für die Sie dann nach Bedarf Georeplikation aktivieren oder deaktivieren.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Festlegen der automatischen Bereitstellung für angefügte Datenträger

___


> ![Windows][Logo_Windows] Windows
> 
> Für VMs, die aus eigenen Images oder Datenträgern erstellt werden, sollten Sie den Parameter „automount“ überprüfen und gegebenenfalls aktivieren. Wenn die automatische Bereitstellung mit diesem Parameter aktiviert wurde, kann die VM nach einem Neustart oder einer erneuten Bereitstellung in Azure die angefügten (d.h. bereitgestellten) Laufwerke automatisch wieder bereitstellen. Für die von Microsoft bereitgestellten Images wird der Parameter in Azure Marketplace aktiviert.
>
> Wie Sie die automatische Bereitstellung für Ihre eigenen Images oder Datenträger aktivieren, entnehmen Sie bitte der Dokumentation des Befehlszeilentools diskpart.exe:
> 
> * [Befehlszeilenoptionen für DiskPart](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automatische Bereitstellung](http://technet.microsoft.com/library/cc753703.aspx)
> 
> Öffnen Sie das Windows-Befehlszeilenfenster als Administrator.
> 
> Falls der VM bereits Datenträger angefügt sind, müssen Sie sich an der VM anmelden, um den Windows Disk Manager zu öffnen. Wenn die automatische Bereitstellung für angefügte Datenträger nicht aktiviert ist, wie im Kapitel [Festlegen der automatischen Bereitstellung für angefügte Datenträger][planning-guide-5.5.3] empfohlen, muss das neu angefügte Volume online geschaltet und initialisiert werden.
>
> ![Linux][Logo_Linux] Linux
>
> Neu angefügte leere Datenträger müssen initialisiert werden, wie in [diesem Artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux] beschrieben. Außerdem müssen neue Datenträger „/etc/fstab“ hinzugefügt werden.

___


### Die eigentliche Bereitstellung
Das Verfahren der eigentlichen Bereitstellung, insbesondere die Bereitstellung von SAP Extended Monitoring, wird ausführlich und mit einer detaillierten Beschreibung der einzelnen Schritte im [Bereitstellungshandbuch][deployment-guide] behandelt.

## Zugriff auf SAP-Systeme auf Azure-VMs
In reinen Cloud-Szenarien möchten Sie eventuell über die SAP-GUI eine Internetverbindung mit diesen SAP-Systemen herstellen. In diesen Fällen gehen Sie am besten nach dem im Folgenden beschriebenen Verfahren vor.

Später gehen wir in diesem Dokument noch auf ein zweites gängiges Szenario ein, nämlich den Zugriff auf SAP-Systeme bei standortübergreifenden Bereitstellungen über eine Standort-zu-Standort-Verbindung (VPN-Tunnel) bzw. eine Azure ExpressRoute-Verbindung zwischen den lokalen und den Azure-Systemen.


### Remotezugriff auf SAP-Systeme

Azure Resource Manager (ARM) gibt keine Standardendpunkte mehr vor, wie dies bislang im klassischen Modell der Fall war. Alle Ports einer ARM-VM sind geöffnet, solange Folgendes zutrifft:

1. Für das Subnetz oder die Netzwerkschnittstelle ist keine Netzwerksicherheitsgruppe definiert. Der Netzwerkverkehr zu Azure-VMs kann über Netzwerksicherheitsgruppen geschützt werden. Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?][virtual-networks-nsg]
1. Für die Netzwerkschnittstelle ist kein Azure Load Balancer definiert.
 
Die Architekturunterschiede zwischen dem klassischen Modell und ARM sind in [diesem Artikel][virtual-machines-azure-resource-manager-architecture] beschrieben.
 
#### Konfiguration des SAP-Systems und SAP-GUI-Konnektivität für reine Cloud-Szenarien
Dieses Thema wird im folgenden Artikel ausführlich behandelt: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### Ändern der Firewalleinstellungen in der VM
Möglicherweise müssen Sie die Firewall Ihrer VMs konfigurieren, um auf Ihrem SAP-System eingehenden Datenverkehr zuzulassen.

___

> ![Windows][Logo_Windows] Windows
>
> Die Windows-Firewall ist auf einer in Azure bereitgestellten VM standardmäßig aktiviert. Sie müssen die Firewall daher so einstellen, dass der SAP-Port geöffnet ist. Andernfalls kann über die SAP-GUI keine Verbindung hergestellt werden. Gehen Sie dazu folgendermaßen vor:
>
>  * Öffnen Sie „Systemsteuerung“ > „System und Sicherheit“ > „Windows-Firewall“, und klicken Sie dann auf „Erweiterte Einstellungen“.
>  * Klicken Sie mit der rechten Maustaste auf „Eingehende Regeln“, und wählen Sie „Neue Regel“ aus.
>  * Geben Sie im daraufhin geöffneten Assistenten an, dass Sie eine neue Port-Regel erstellen möchten.
>  * Übernehmen Sie im nächsten Schritt des Assistenten die Einstellung TCP, und geben Sie die Nummer des Ports ein, den Sie öffnen möchten. Da unsere SAP-Instanznummer 00 ist, haben wir hier 3200 eingegeben. Wenn Ihre Instanz eine andere Nummer hat, sollten Sie hier nun den Port öffnen, den Sie zuvor auf Basis Ihrer Instanznummer festgelegt haben.
>  * Lassen Sie im nächsten Fenster des Assistenten die Option "Verbindung zulassen" aktiviert.
>  * Legen Sie im nächsten Schritt fest, ob die Regel auf die Domäne, auf das private und/oder auf das öffentliche Netzwerk angewendet werden soll. Stellen Sie diese Optionen nach Bedarf ein. Die Regel muss jedoch für das öffentliche Netzwerk gelten, wenn über die SAP-GUI Verbindungen von außerhalb über das öffentliche Netzwerk möglich sein sollen.
>  * Geben Sie der Regel im letzten Schritt des Assistenten einen Namen, und speichern Sie die Regel dann, indem Sie auf „Fertig stellen“ klicken.
>
>  Die Regel wird sofort wirksam.
>
> ![Portregeldefinition][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Durch die Linux-Images aus Azure Marketplace wird die iptables-Firewall standardmäßig nicht aktiviert, weshalb die Verbindung mit Ihrem SAP-System funktionieren sollte. Wenn Sie „iptables“ oder eine andere Firewall aktiviert haben, informieren Sie sich in der Dokumentation der betreffenden Firewall, wie eingehender TCP-Datenverkehr an Port 32xx zugelassen wird (xx ist hierbei die Systemnummer Ihres SAP-Systems).

___

#### Sicherheitsempfehlungen

Die SAP-GUI stellt zu keiner der aktiven SAP-Instanzen (Port 32xx) eine direkte Verbindung her, sondern verbindet sich zunächst über den Port, der für den SAP-Nachrichtenserverprozess (Port 36xx) geöffnet ist. Früher wurde derselbe Port vom Nachrichtenserver für die interne Kommunikation mit den Anwendungsinstanzen verwendet. Um zu verhindern, dass lokale Anwendungsserver versehentlich mit einem Azure-Nachrichtenserver kommunizieren, können die internen Kommunikationsports geändert werden. Bei Systemen, die von lokalen Systemen geklont wurden, beispielsweise einem Klon der Entwicklung für einen Projekttest, wird dringend empfohlen, die interne Kommunikation zwischen dem SAP-Nachrichtenserver und seinen Anwendungsinstanzen auf einen anderen Port zu verlegen. Dies kann mit dem Standardprofilparameter erfolgen:

>	rdisp/msserv_internal

Eine Beschreibung finden Sie in: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm>

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Konzepte für Nur-Cloud-Bereitstellung von SAP-Instanzen

### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Einzelner virtueller Computer mit SAP NetWeaver-Demo/Schulungsszenario
 
![Ausführung von SAP-Demo-Systemen mit einem einzelnen virtuellen Computer mit denselben VM-Namen, isoliert in Azure Cloud Services][planning-guide-figure-1700]

In diesem Szenario (siehe Kapitel [Nur Cloud][planning-guide-2.1] in diesem Dokument) implementieren wir ein typisches Schulungs-/Demosystem, wobei sich das Material für das vollständige Schulungs-/Demoszenario auf einer einzigen VM befindet. Wir gehen davon aus, dass die Bereitstellung über VM-Imagevorlagen erfolgt. Außerdem gehen wir davon aus, dass mehrere dieser Demo-/Schulungs-VMs bereitgestellt werden müssen, wobei für alle VMs der gleiche Name verwendet wird.

Es wird vorausgesetzt, dass Sie ein VM-Image erstellt haben, wie in einigen Abschnitten des Kapitels [Vorbereiten virtueller Computer mit SAP für Azure][planning-guide-5.2] in diesem Dokument beschrieben.

Die Abfolge der Ereignisse zur Implementierung dieses Szenarios sieht wie folgt aus:

[comment]: <> (MShermannd TODO have to provide ARM sample / description using json template + clarification regarding unique VM name within ARM virtual network )
##### PowerShell

* Erstellen einer neuen Ressourcengruppe für jede Schulungs-/Demoumgebung

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Erstellen eines neuen Speicherkontos

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Erstellen Sie für jede Schulungs-/Demoumgebung ein neues virtuelles Netzwerk, so dass der gleiche Hostname und die gleichen IP-Adressen verwendet werden können. Das virtuelle Netzwerk wird durch eine Netzwerksicherheitsgruppe geschützt, die nur Datenverkehr an Port 3389 (für Remotedesktopzugriff) und Port 22 (für SSH) zulässt.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Erstellen Sie eine neue öffentliche IP-Adresse, über die Zugriff auf die VM über das Internet möglich ist.

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Erstellen Sie eine neue Netzwerkschnittstelle für die VM.

```powershell 
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip 
```

* Erstellen Sie eine VM. Für das reine Cloud-Szenario erhält jede VM den gleichen Namen. Auch die SAP-SIDs der SAP NetWeaver-Instanzen dieser VMs sind identisch. Innerhalb einer Azure-Ressourcengruppe müssen die Namen von VMs eindeutig sein, aber in verschiedenen Azure-Ressourcengruppen können VMs mit identischen Namen ausgeführt werden. Die Standardkonten „Administrator“ (Windows) und „root“ (Linux) sind nicht gültig. Daher muss ein neuer Benutzername mit Administratorrechten samt Kennwort eingerichtet werden. Auch die Größe der VM muss definiert werden.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Fügen Sie optional weitere Datenträger hinzu, und stellen Sie erforderliche Inhalte wieder her. Beachten Sie, dass innerhalb von Azure alle Blobnamen (URLs auf die Blobs) eindeutig sein müssen.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### Befehlszeilenschnittstelle (CLI)

Der folgende Beispielcode ist für Linux vorgesehen. Für Windows verwenden Sie entweder PowerShell, wie zuvor beschrieben, oder Sie geben in diesem Beispiel „%rgName%“ statt „$rgName“ ein und legen die Umgebungsvariable mit dem Windows-Befehl _set_ fest.

* Erstellen einer neuen Ressourcengruppe für jede Schulungs-/Demoumgebung

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Erstellen eines neuen Speicherkontos

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Erstellen Sie für jede Schulungs-/Demoumgebung ein neues virtuelles Netzwerk, so dass der gleiche Hostname und die gleichen IP-Adressen verwendet werden können. Das virtuelle Netzwerk wird durch eine Netzwerksicherheitsgruppe geschützt, die nur Datenverkehr an Port 3389 (für Remotedesktopzugriff) und Port 22 (für SSH) zulässt.

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol * --source-address-prefix * --source-port-range * --destination-address-prefix * --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol * --source-address-prefix * --source-port-range * --destination-address-prefix * --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Erstellen Sie eine neue öffentliche IP-Adresse, über die Zugriff auf die VM über das Internet möglich ist.

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Erstellen Sie eine neue Netzwerkschnittstelle für die VM.

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet 
```

* Erstellen Sie eine VM. Für das reine Cloud-Szenario erhält jede VM den gleichen Namen. Auch die SAP-SIDs der SAP NetWeaver-Instanzen dieser VMs sind identisch. Innerhalb einer Azure-Ressourcengruppe müssen die Namen von VMs eindeutig sein, aber in verschiedenen Azure-Ressourcengruppen können VMs mit identischen Namen ausgeführt werden. Die Standardkonten „Administrator“ (Windows) und „root“ (Linux) sind nicht gültig. Daher muss ein neuer Benutzername mit Administratorrechten samt Kennwort eingerichtet werden. Auch die Größe der VM muss definiert werden.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Fügen Sie optional weitere Datenträger hinzu, und stellen Sie erforderliche Inhalte wieder her. Beachten Sie, dass innerhalb von Azure alle Blobnamen (URLs auf die Blobs) eindeutig sein müssen.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### Vorlage
Sie können die Beispielvorlagen aus dem GitHub-Repository „azure-quickstart-templates“ verwenden.

* [Simple Linux VM (Einfache Linux-VM)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Simple Windows VM (Einfache Windows-VM)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [VM from image (VM aus Image)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### Implementieren mehrerer VMs, die innerhalb von Azure miteinander kommunizieren müssen
Dieses reine Cloud-Szenario ist ein typisches Szenario für Schulungs- und Demozwecke, wobei die Software für dieses Szenario auf mehrere VMs verteilt ist. Die auf den verschiedenen VMs installierten Komponenten müssen miteinander kommunizieren können. Auch für dieses Szenario ist weder eine lokale Netzwerkkommunikation noch ein standortübergreifendes Szenario erforderlich.

Dieses Szenario ist eine Erweiterung der Installation, die in diesem Dokument im Kapitel [Schulungs-/Demoszenario mit einem einzelnen virtuellen Computer mit SAP NetWeaver][planning-guide-7.1] beschrieben wird. In diesem Szenario werden einer vorhandenen Ressourcengruppe weitere VMs hinzugefügt. Im folgenden Beispiel besteht die Schulungsumgebung aus einer SAP ASCS/SCS-VM, einer VM mit einem DBMS und einer VM mit einer SAP-Anwendungsserverinstanz.

Vor der Einrichtung dieses Szenarios sollten Sie sich wie im vorherigen Szenario Gedanken über die grundlegenden Einstellungen machen.

#### Benennung von Ressourcengruppen und virtuellen Computern
Alle Namen von Ressourcengruppen müssen eindeutig sein. Entwickeln Sie Ihr eigenes Benennungsschema für Ihre Ressourcen, z.B. `<rg-name`>-Suffix.

Der Name des virtuellen Computers muss innerhalb der Ressourcengruppe eindeutig sein.

#### Einrichten des Netzwerks für die Kommunikation zwischen den verschiedenen VMs
 
![Gruppe von virtuellen Computern in Azure Virtual Network][planning-guide-figure-1900]

Um Namenskonflikte mit Klonen der gleiche Schulungs- bzw Demo-Landschaften zu verhindern, erstellen Sie für jedes Querformat ein Azure Virtual Network. Die DNS-Namensauflösung erfolgt durch Azure. Sie können außerhalb von Azure aber auch einen eigenen DNS-Server konfigurieren (was hier nicht näher erörtert wird). In diesem Szenario konfigurieren wir keinen eigenen DNS-Server. Für alle virtuellen Computer in einem Azure Virtual Network wird die Kommunikation über Hostnamen aktiviert.

Schulungs- oder Demo-Landschaften können aus folgenden Gründen durch virtuelle Netzwerke anstatt nur durch Ressourcengruppen getrennt werden:

* Die SAP-Landschaft erfordert in der gegebenen Einrichtung ein eigenes AD/OpenLDAP. Zudem muss jede der Landschaften über einen Domänenserver verfügen.
* Die SAP-Landschaft verfügt in der gegebenen Einrichtung über Komponenten, die feste IP-Adressen erfordern.

Weitere Informationen zu virtuellen Azure-Netzwerken und deren Definition finden Sie [in diesem Artikel][virtual-networks-create-vnet-arm-pportal].

## Bereitstellen von SAP-VMs mit (standortübergreifender) Verbindung zum Unternehmensnetzwerk

Sie führen eine SAP-Landschaft aus und möchten die Bereitstellung auf Bare-Metal-Umgebungen für High-End-DBMS-Server, lokale virtualisierte Umgebungen für Anwendungsschichten und kleinere SAP-Systemen mit 2-Tier-Konfiguration sowie Azure IaaS aufteilen. Die Grundannahme ist, dass SAP-Systeme innerhalb einer SAP-Landschaft ungeachtet der Bereitstellungsform untereinander und mit zahlreichen anderen Softwarekomponenten innerhalb des Unternehmens kommunizieren müssen. Es sollte auch nicht zu Abweichungen aufgrund der Bereitstellungsform für Endbenutzer kommen, die über SAP GUI oder andere Schnittstellen auf SAP-Systeme zugreifen. Diese Bedingung können nur erfüllt werden, wenn wir das lokale Active Directory/OpenLDAP und DNS-Dienste über Site-to-Site-/Multi-Site- oder private Verbindungen wie Azure ExpressRoute erweitern.

Um weitere Hintergrundinformationen zu den Implementierungsdetails von SAP in Azure zu erhalten, empfehlen wir das Kapitel [Konzepte der Nur-Cloud-Bereitstellung von SAP-Instanzen][planning-guide-7] in diesem Dokument. Darin werden einige der grundlegenden Konstrukte von Azure und deren Verwendung mit SAP-Anwendungen in Azure erläutert.

### Szenario einer SAP-Landschaft

Das standortübergreifende Szenario kann in etwa wie in der folgenden Grafik beschrieben werden:
 
![Site-to-Site-Verbindung zwischen lokale und Azure-Ressourcen][planning-guide-figure-2100]

Die oben dargestellte Grafik beschreibt ein Szenario, in dem das lokale AD/OpenLDAP und DNS auf Azure erweitert wurden. Auf der lokalen Seite ist pro Azure-Abonnement ein bestimmter IP-Adressbereich reserviert. Der IP-Adressbereich wird auf der Seite von Azure einem Azure Virtual Network zugewiesen.

#### Sicherheitshinweise

Als Mindestanforderung ist die Verwendung von sicheren Kommunikationsprotokollen wie SSL/TLS für den Browserzugriff oder von VPN-basierten Verbindungen für den Systemzugriff auf die Azure-Dienste erforderlich. Es wird davon ausgegangen, dass Unternehmen die VPN-Verbindung zwischen ihrem Unternehmensnetzwerk und Azure sehr unterschiedlich verwalten. Einige Unternehmen öffnen möglicherweise einfach alle Ports. Einige andere Unternehmen möchten möglicherweise genauer spezifizieren, welche Ports geöffnet werden müssen usw.

In der nachfolgenden Tabelle sind typische SAP-Kommunikationsports aufgeführt. Im Grunde reicht es aus, den SAP-Gatewayport zu öffnen.

| Dienst | Portname | Beispiel: `<nn`> = 01 | Standardbereich (min.–max.) | Kommentar |
|---------|-----------|-------------------|-------------------------|---------|
| Verteiler | sapdp`<nn>` siehe * | 3201 | 3200 – 3299 | SAP Dispatcher, wird von der SAP-Benutzeroberfläche für Windows und Java verwendet |
| Nachrichtenserver | sapms`<sid`> siehe ** | 3600 | frei sapms`<anySID`> | sid = SAP-System-ID |
| Gateway | sapgw`<nn`> siehe * | 3301 | frei | SAP-Gateway für die CPIC- und RFC-Kommunikation |
| SAP-Router | sapdp99 | 3299 | frei | Nach der Installation können einem beliebigen Wert nur CI-Dienstnamen (Central Instance) in /etc/services erneut zugewiesen werden. |

*) nn = SAP-Instanznummer

**) sid = SAP-System-ID

Ausführlichere Informationen zu den für unterschiedliche SAP-Produkte oder Dienste von SAP-Produkten erforderlichen Ports finden Sie hier: <http://scn.sap.com/docs/DOC-17124>. Mit diesem Dokument sollten Sie auf dem VPN-Gerät dedizierte Ports für bestimmte SAP-Produkte und -Szenarien öffnen können.

Als weitere Sicherheitsmaßnahme bei der Bereitstellung von virtuellen Computern in einem solchen Szenario können Sie eine [Netzwerksicherheitsgruppe][virtual-networks-nsg] erstellen, um Zugriffsregeln zu definieren.

### Umgang mit anderen virtuellen Computerserien

Microsoft hat in den vergangenen 12 Monaten viele weitere virtuelle Computertypen hinzugefügt, die sich entweder hinsichtlich der Anzahl der vCPUs, des Speichers oder (noch wichtiger) der Hardware unterscheiden, auf der sie ausgeführt werden. Nicht alle diese virtuellen Computer werden von SAP unterstützt (unterstützte virtuelle Computertypen finden Sie im SAP-Hinweis [1928533]). Einige dieser virtuellen Computer werden auf anderen Host-Hardwaregenerationen ausgeführt. Die Bereitstellung dieser Host-Hardwaregenerationen erfolgt in der Granularität einer Azure-Skalierungseinheit. Somit können Fällen auftreten, in denen die verschiedenen von Ihnen ausgewählten VM-Größen nicht mit derselben Skalierungseinheit ausgeführt werden können. Eine Verfügbarkeitsgruppe kann nur Skalierungseinheiten für unterschiedliche Hardware umfassen. Beispiel: Wenn Sie das DBMS auf A5-A11-VMs und die SAP-Anwendungsebene auf virtuellen Computern der G-Serie ausführen möchten, erzwingen Sie die Bereitstellung eines einzelnen SAP-Systems oder anderer SAP-Systeme in unterschiedlichen Verfügbarkeitsgruppen erzwungen.


#### Drucken auf einem lokalen Netzwerkdrucker von einer SAP-Instanz in Azure
##### Drucken über TCP/IP in standortübergreifendem Szenario


Das Einrichten Ihre lokalen TCP/IP-basierten Netzwerkdrucker auf einem virtuellen Azure-Computer erfolgt generell auf die gleiche Weise wie in Ihrem Unternehmensnetzwerk. Es wird vorausgesetzt, dass Sie über einen Site-to-Site-VPN-Tunnel oder eine ExpressRoute-Verbindung verfügen.

___

> ![Windows][Logo_Windows] Windows
>
> Gehen Sie dazu folgendermaßen vor:
> - Einige Netzwerkdrucker verfügen über einen Konfigurations-Assistenten, mit dem Sie den Drucker auf einem virtuellen Azure-Computer einrichten können. Wenn der Drucker keine Assistenten-Software besitzt, richten Sie ihn "manuell" ein, indem Sie einen neuen TCP/IP-Druckeranschluss erstellen.
> - Öffnen Sie "Systemsteuerung -> Geräte und Drucker -> Drucker hinzufügen".
> - Wählen Sie "Drucker unter Verwendung einer TCP/IP-Adresse oder eines Hostnamens hinzufügen" aus.
> - Geben Sie die IP-Adresse des Druckers ein.
> - Standard-Druckeranschluss 9100
> - Installieren Sie bei Bedarf den entsprechenden Druckertreiber manuell.
> 
> ![Linux][Logo_Linux] Linux
>
> - Befolgen Sie wie für Windows einfach das Standardverfahren zum Installieren eines Netzwerkdruckers.
> - Beachten Sie zum Hinzufügen eines Druckers die Anweisungen in den öffentlichen Linux-Handbüchern für [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) oder [Red Hat](https://access.redhat.com/documentation/de-DE/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html).

___

 
![Drucken im Netzwerk][planning-guide-figure-2200]



##### Hostbasierter Drucker über SMB (freigegebener Drucker) in standortübergreifendem Szenario

Hostbasierte Drucker sind nicht von Haus aus mit dem Netzwerk kompatibel. Sie können einen hostbasierten Drucker jedoch auf Computern in einem Netzwerk freigegeben, vorausgesetzt, er ist an einen eingeschalteten Computer angeschlossen. Stellen Sie eine Site-to-Site- oder ExpressRoute-Verbindung zu Ihrem Unternehmensnetzwerk her, und geben Sie den lokalen Drucker frei. Das SMB-Protokoll verwendet als Namensdienst NetBIOS anstelle von DNS. Der NetBIOS-Hostname kann vom DNS-Hostnamen abweichen. Normalerweise sind der NetBIOS-Hostname und der DNS-Hostname identisch. Die DNS-Domäne wird im NetBIOS-Namespace nicht unterstützt. Folglich kann der vollständig qualifizierte DNS-Hostname bestehend aus dem DNS-Hostnamen und der DNS-Domäne nicht im NetBIOS-Namespace verwendet werden.

Die Druckerfreigabe wird durch einen eindeutigen Namen im Netzwerk identifiziert:

* Hostname des SMB-Hosts (immer erforderlich)
* Name der Freigabe (immer erforderlich)
* Name der Domäne, wenn sich die Druckerfreigabe nicht in derselben Domäne wie das SAP-System befindet
* Darüber hinaus sind für den Zugriff auf die Druckerfreigabe möglicherweise ein Benutzernamen und ein Kennwort erforderlich.

Gewusst wie:

___

> ![Windows][Logo_Windows] Windows
>
> Geben Sie Ihre lokalen Drucker frei. Öffnen Sie auf dem virtuellen Azure-Computer Windows Explorer, und geben Sie den Freigabenamen des Druckers ein. Ein Druckerinstallations-Assistent führt Sie durch den Installationsprozess.
>
> ![Linux][Logo_Linux] Linux
>
> Es folgen einige Beispiele der Dokumentation zur Konfiguration von Netzwerkdruckern unter Linux einschließlich eines Kapitels zum Drucken unter Linux. Dies funktioniert auf einem virtuellen Azure Linux-Computer auf gleiche Weise, sofern der virtuelle Computer Teil eines VPN ist:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_(Samba)_Share_or_Windows_Share>
> * RHEL <https://access.redhat.com/documentation/de-DE/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>

___


##### USB-Drucker (Druckerweiterleitung) 

In Azure kann Benutzern in einer Remotesitzung mithilfe der Remotedesktopdienste kein Zugriff auf ihre lokalen Druckergeräte gewährt werden.

___

> ![Windows][Logo_Windows] Windows
>
> Weitere Informationen zum Drucken unter Windows finden Sie hier: <http://technet.microsoft.com/library/jj590748.aspx>.

___

 
#### Integration von SAP Azure-Systemen in das Change and Transport System (CTS) in standortübergreifenden Umgebungen

Das SAP Change and Transport System (CTS) muss für den Export und Import von Transportanforderungen innerhalb Ihrer Landschaft systemübergreifend konfiguriert werden. Es wird davon ausgegangen, dass sich die Entwicklungsinstanzen eines SAP-Systems (DEV) in Azure befinden, während die Qualitätssicherung (QA) sowie die produktiven Systeme (PRD) lokal vorhanden sind. Darüber hinaus wird erwartet, dass ein zentrales Transportverzeichnis vorhanden ist.

##### Konfigurieren der Transportdomäne
Informationen zum Konfigurieren der Transportdomäne auf dem System, das Sie als Transport Domain Controller festgelegt haben, finden Sie unter [ Configuring the Transport Domain Controller](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm) (Konfigurieren des Transport Domain Controllers). Der Systembenutzer TMSADM wird erstellt und das erforderliche RFC-Ziel wird generiert. Sie können diese RFC-Verbindungen mithilfe der Transaktion SM59 überprüfen. Die Hostnamensauflösung muss innerhalb Ihrer gesamten Transportdomäne aktiviert sein.

Gewusst wie:

* In diesem Szenario haben wir das lokale QAS-System als CTS-Domänencontroller festgelegt. Rufen Sie das Transaktions-STMS auf. Das Dialogfeld für TMS wird angezeigt. Das Dialogfeld "Transportdomäne konfigurieren" wird angezeigt. (Dieses Dialogfeld wird nur angezeigt, wenn Sie noch keine Transportdomäne konfiguriert haben.)
* Stellen Sie unter „SM59 > ABAP-Verbindung > TMSADM@E61.DOMAIN\_E61 > Details > Dienstprogramme (M) > Autorisierungstest“ sicher, dass der automatisch erstellte Benutzer TMSADM autorisiert ist. Auf dem Startbildschirm des Transaktions-STMS sollte angezeigt werden, dass dieses SAP-System jetzt wie hier dargestellt als Controller der Transportdomäne fungiert:
 
![Startbildschirm für Transaktions-STMS auf dem Domänencontroller][planning-guide-figure-2300]

#### Einbeziehen von SAP-Systemen in die Transportdomäne

Schließen Sie mit den folgenden Schritten ein SAP-System in eine Transportdomäne ein:

* Rufen Sie auf dem DEV-System in Azure das Transportsystem (Client 000) und anschließend das Transaktion-STMS auf. Wählen Sie im Dialogfeld die Option "Andere Konfiguration" aus, und fahren Sie mit "Domäne in System einbeziehen" fort. Geben Sie den Domänencontroller als Zielhost an ([Einbeziehen von SAP-Systemen in die Transportdomäne](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Das System wartet jetzt auf die Einbeziehung in die Transportdomäne.
* Aus Sicherheitsgründen müssen Sie anschließend zum Domänencontroller zurückkehren, um Ihre Anforderung zu bestätigen. Wählen Sie auf dem wartenden System "Systemübersicht" und "Genehmigen" aus. Bestätigen Sie die Eingabeaufforderung, woraufhin die Konfiguration verteilt wird.

Dieses SAP-System enthält jetzt die erforderlichen Informationen über alle anderen SAP-Systeme in der Transportdomäne. Gleichzeitig werden die Adressdaten des neuen SAP-Systems an alle anderen SAP-Systeme gesendet, und das SAP-System wird in das Transportprofil des Transportverwaltungsprogramms eingegeben. Überprüfen Sie, ob RFCs und der Zugriff auf das Transportverzeichnis der Domäne funktionieren.

Fahren Sie wie gewohnt mit der Konfiguration Ihres Transportsystems fort. Eine Beschreibung finden Sie in der Dokumentation [Change and Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm) (Änderungs- und Transportsystem).

Gewusst wie:

* Stellen Sie sicher, dass das lokale STMS ordnungsgemäß konfiguriert ist.
* Stellen Sie sicher, dass der Hostname des Transport Domain Controllers vom virtuellen Computer in Azure aufgelöst werden kann und umgekehrt.
* Wählen Sie "Transaktions-STMS -> Andere Konfiguration -> System in Domäne einbeziehen".
* Bestätigen Sie die Verbindung im lokale TMS-System.
* Konfigurieren Sie Transportrouten, Gruppen und Ebenen wie gewohnt.

In standortübergreifenden Szenarien mit Site-to-Site-Verbindung kann zwischen dem lokalen System und Azure eine beträchtlich Latenz vorliegen. Wenn wir die Schritte des Objekttransports von der Entwicklung über Testsysteme bis hin zur Produktion verfolgen oder die Anwendung von Transportfunktionen oder Supportpaketen auf die unterschiedlichen Systeme erwägen, fällt auf, dass je nach Speicherort im zentralen Transportverzeichnis bei manchen Systemen eine hohe Latenz beim Lesen und Schreiben von Daten im zentralen Transportverzeichnis auftritt. Die Situation ähnelt der Konfiguration von SAP-Landschaften, in denen die verschiedenen Systeme auf unterschiedliche Rechenzentren verteilt sind, die in beträchtlichem Abstand zueinander liegen.

Um solche Latenzen zu umgehen und schnelle Lese- oder Schreibvorgänge in oder aus dem Transportverzeichnis zu ermöglichen, können Sie zwei STMS-Transportdomänen einrichten (eine für das lokale System und eine für die Systeme in Azure), und diese anschließend verbinden. Lesen Sie in der folgenden Dokumentation mehr über die Prinzipien hinter diesem Konzept im SAP-TMS: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Gewusst wie:

* Richten Sie an jedem Standort (lokal und in Azure) mithilfe des Transaktion-STMS eine Transportdomäne ein: <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Verknüpfen Sie die Domänen mit einem Domänenlink, und bestätigen Sie die Verknüpfung zwischen den beiden Domänen: <http://help.sap.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/content.htm>
* Verteilen Sie die Konfiguration auf das verknüpfte System.

#### RFC-Datenverkehr zwischen SAP-Instanzen in Azure und auf dem lokalen System (standortübergreifend)

Die RFC-Datenübertragung zwischen lokalen und Azure-Systemen muss funktionieren. Rufen Sie zum Einrichten einer Verbindung die Transaktion "SM59" in einem Quellsystem auf, in dem Sie eine RFC-Verbindung mit dem Zielsystem definieren müssen. Die Konfiguration ähnelt der Standardeinrichtung einer RFC-Verbindung.

Es wird davon ausgegangen, dass sich die virtuellen Computer im standortübergreifenden Szenario, die SAP-Systeme ausführen, die miteinander kommunizieren müssen, in derselben Domäne befinden. Daher unterscheidet sich die Einrichtung einer RFC-Verbindung zwischen SAP-Systeme nicht von den Einrichtungsschritten und Eingaben in lokalen Szenarios.

#### Zugriff auf "lokale" Dateifreigaben von SAP-Instanzen in Azure oder umgekehrt

SAP-Instanzen in Azure müssen auf Dateifreigaben innerhalb der lokalen Systeme des Unternehmens zugreifen. Darüber hinaus benötigen lokale SAP-Instanzen Zugriff auf Dateifreigaben in Azure. Um die Dateifreigaben zu aktivieren, konfigurieren Sie die Berechtigungen und Freigabeoptionen auf dem lokalen System. Stellen Sie sicher, dass die Ports für die VPN- oder ExpressRoute-Verbindung zwischen Azure und Ihrem Rechenzentrum geöffnet sind.

## Unterstützungsmöglichkeiten
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure-Überwachungslösung für SAP
Um die Überwachung unternehmenskritischer SAP-Systeme in Azure zu aktivieren, ruft das SAP-Überwachungstool SAPOSCOL bzw. der SAP-Host-Agent über eine Azure-Überwachungserweiterung für SAP Daten vom Host des Azure Virtual Machine-Diensts ab. Da bei SAP sehr spezifische Anforderungen hinsichtlich SAP-Anwendungen gelten, hat sich Microsoft entschieden, die erforderliche Funktionalität nicht generisch in Azure zu implementiert. Die Kunden sollten die erforderlichen Überwachungskomponenten und Konfigurationen selbst auf ihren virtuellen Computer in Azure bereitstellen. Die Bereitstellung und das Lebenszyklusmanagement der Überwachungskomponenten wird jedoch zum Großteil von Azure automatisiert.

#### Lösungsentwurf

Die Architektur des Azure-VM-Agenten und des Erweiterungs-Frameworks bildet die Grundlage für die zur SAP-Überwachung entwickelte Lösung. Der Azure-VM-Agent und das Erweiterungs-Framework sollen die Installation der im Azure-VM-Erweiterungskatalog auf einem virtuellen Computer enthaltenen Software-Anwendungen zulassen. Die grundlegende Idee hinter diesem Konzept besteht darin, die Bereitstellung spezieller Funktionen auf einem virtuellen Computer (in Fällen wie der Azure-Überwachungserweiterung für SAP) und die Konfiguration dieser Software zum Zeitpunkt der Bereitstellung zuzulassen.

Seit Februar 2014 wird der "Azure-VM-Agent", der die Verarbeitung bestimmter Azure-VM-Erweiterungen auf dem virtuellen Computer ermöglicht, bei der Erstellung von virtuellen Computern im Azure-Portal standardmäßig in virtuelle Windows-Computer integriert. Bei SUSE oder Red Hat Linux ist der Agent des virtuellen Computers bereits Teil des Azure Marketplace-Images. Wenn ein virtueller Linux-Computer aus dem lokalen System in Azure hochgeladen wird, muss der Agent des virtuellen Computers manuell installiert werden.


Dies sind die grundlegenden Bausteine der Überwachungslösung in Azure für SAP:
 
![Microsoft Azure-Erweiterungskomponenten][planning-guide-figure-2400]

Wie in dem oben abgebildeten Blockdiagramm dargestellt, wird ein Teil der Überwachungslösung für SAP im Azure-VM-Image und im Azure-Erweiterungskatalog gehostet. Hierbei handelt es sich um ein weltweit repliziertes Repository, das von Azure-Vorgängen verwaltet wird. Es ist die Aufgabe des gemeinsamen SAP/MS-Teams, das für die Zusammenarbeit der Azure-Implementierung von SAP mit Azure-Vorgängen zuständig ist, neue Versionen der Azure-Überwachungserweiterung für SAP zu veröffentlichen. Diese Azure-Überwachungserweiterung für SAP ruft die erforderlichen Informationen mithilfe der Erweiterungen Microsoft Azure-Diagnose (WAD) oder Linux Azure-Diagnose (LAD) ab.

Wenn Sie einen neuen virtuellen Windows-Computer bereitstellen, wird der Azure-VM-Agent dem virtuellen Computer automatisch hinzugefügt. Dieser Agent hat die Aufgabe, das Laden und die Konfiguration der Azure-Erweiterungen für die Überwachung von SAP NetWeaver-Systemen zu koordinieren. Bei virtuellen Linux-Computern ist der Azure-VM-Agent bereits Teil des Azure Marketplace-Betriebssystemimages.

Ein Schritt muss jedoch noch vom Kunden ausgeführt werden. Dies ist die Aktivierung und Konfiguration der Leistungserfassung. Der mit der Konfiguration verbundene Vorgang wird durch ein PowerShell-Skript oder einen CLI-Befehl automatisiert. Das PowerShell-Skript kann wie im [Bereitstellungshandbuch][deployment-guide] beschrieben in das Microsoft Azure Script Center heruntergeladen werden.

Die Gesamtarchitektur der Azure-Überwachungslösung für SAP sieht wie folgt aus:
 
![Azure-Überwachungslösung für SAP NetWeaver][planning-guide-figure-2500]

**Anweisungen zur exakten Vorgehensweise sowie detaillierte Schritte zur Verwendung dieser PowerShell-Cmdlets oder CLI-Befehle während der Bereitstellung finden Sie im [Bereitstellungshandbuch][deployment-guide].**

### Integration von in Azure befindlichen SAP-Instanzen in SAProuter

Die in Azure ausgeführten SAP-Instanzen müssen auch von SAProuter aus zugänglich sein.
 
![Netzwerkverbindung zu SAP-Router][planning-guide-figure-2600]

Ein SAProuter ermöglicht die TCP/IP-Kommunikation zwischen teilnehmenden Systemen, wenn keine direkte IP-Verbindung vorhanden ist. Dies bietet den Vorteil, dass auf Netzwerkebene keine End-to-End-Verbindung zwischen den Kommunikationspartnern erforderlich ist. Der SAProuter hört standardmäßig Port 3299 ab. Um SAP-Instanzen über einen SAProuter miteinander zu verbinden, müssen Sie bei jedem Verbindungsversuch die SAProuter-Zeichenfolge und Hostnamen eingeben.

## SAP NetWeaver AS Java

Bisher wurden in diesem Dokument schwerpunktmäßig SAP NetWeaver im allgemeinen bzw. der SAP NetWeaver ABAP-Stapel behandelt. In diesem kurzen Abschnitt sind spezielle Aspekte des SAP Java-Stapels aufgeführt. Eines der wichtigsten ausschließlich auf SAP NetWeaver Java basierten Anwendungen ist das SAP Enterprise Portal. Andere SAP NetWeaver-basierte Anwendungen wie SAP PI und SAP Solution Manager verwenden sowohl die SAP NetWeaver ABAP- als auch die Java-Stapel. Daher sind sicherlich bestimmten Aspekte im Zusammenhang mit dem SAP NetWeaver Java-Stapel zu berücksichtigen.

### SAP Enterprise Portal

Bei standortübergreifenden Szenarien ähnelt das Einrichten eines SAP-Portals auf einem virtuellen Azure-Computer einer lokalen Installation. Da DNS lokal ausgeführt wird, können die Porteinstellungen der einzelnen Instanzen lokal wie bei einer lokalen Konfiguration festgelegt werden. Die bisher in diesem Dokument beschriebenen Empfehlungen und Einschränkungen gelten im Allgemeinen für eine Anwendung wie das SAP Enterprise Portal oder den SAP NetWeaver Java-Stapel im Allgemeinen.

![Bereitgestelltes SAP-Portal][planning-guide-figure-2700]

In einem speziellen Bereitstellungsszenario einiger Kunden wird das SAP Enterprise Portal direkt über das Internet verfügbar gemacht, während der Host des virtuellen Computers über einen Site-to-Site-VPN-Tunnel oder ExpressRoute mit dem Unternehmensnetzwerk verbunden ist. Bei einem solchen Szenario ist sicherzustellen, dass bestimmte Ports geöffnet und nicht durch eine Firewall oder eine Netzwerksicherheitsgruppe blockiert sind. Derselbe Mechanismus muss angewendet werden, wenn Sie in einem reinen Cloud-Szenario vom lokalen System aus eine Verbindung mit einer SAP Java-Instanz herstellen möchten.

Der anfängliche Portal-URI ist http(s):`<Portalserver`>: 5XX00/Irj. Der Port wird dabei durch 50.000 + (Systemnummer × 100) gebildet. Der Standard-Portal-URI des SAP-Systems 00 ist `<dns name`>.`<azure region`>.Cloudapp.azure.com:PublicPort/irj. Weitere Informationen erhalten Sie unter <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.
 
![Endpunktkonfiguration][planning-guide-figure-2800]

Informationen zum Anpassen der URL und/oder Ports des SAP Enterprise Portal finden Sie in dieser Dokumentation:

* [Change Portal URL (Ändern der Portal-URL)](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Change Default port numbers, Portal port numbers (Ändern der Standard- und Portal-Portnummern)](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)


## Hohe Verfügbarkeit (HA) und Disaster Recovery (DR) für SAP NetWeaver auf virtuellen Azure-Computern
### Definition der Terminologie

Der Begriff **hohe Verfügbarkeit (HA)** bezieht sich generell auf eine Reihe von Technologien, die IT-Störungen minimieren, indem sie die Geschäftskontinuität von IT-Diensten durch redundante, fehlertolerante oder Failover-geschützte Komponenten innerhalb des **gleichen** Rechenzentrums bereitstellen. In unserem Fall erfolgt diese innerhalb einer einzelnen Azure-Region.

Die **Notfallwiederherstellung (Disaster Recovery, DR)** zielt ebenfalls auf die Minimierung von IT-Dienstunterbrechungen und deren Wiederherstellung ab, aber in **anderen** Rechenzentren, die sich in der Regel Hunderte von Kilometern entfernt befinden. In unserem Fall erfolgt dies in der Regel zwischen verschiedenen Azure-Regionen innerhalb der geopolitischen Region oder entsprechend Ihrer Konfiguration als Kunde.

### Übersicht über hohe Verfügbarkeit
Die Erörterung der hohen SAP-Verfügbarkeit in Azure kann in zwei Bereiche untergliedert werden:

* **Hohe Verfügbarkeit der Azure-Infrastruktur**, z.B. die hohe Verfügbarkeit des Computings (VMs), Netzwerks, Speichers usw. und dessen Vorteile beim Erhöhen der Verfügbarkeit der SAP-Anwendung.
* **Hohe Verfügbarkeit der SAP-Anwendung**, z.B. die hohe Verfügbarkeit von SAP-Software-Komponenten:
	* SAP-Anwendungsserver
	* SAP ASCS/SCS-Instanz
	* DB-Server

Hinzu kommt die Kombinationsmöglichkeit mit der hohen Verfügbarkeit der Azure-Infrastruktur.

Die hohe Verfügbarkeit von SAP in Azure unterscheidet sich in einigen Punkten von der hohen Verfügbarkeit von SAP in einer lokalen physischen oder virtuellen Umgebung. Das folgende Dokument von SAP beschreibt Standardkonfigurationen für hohe Verfügbarkeit von SAP in virtualisierten Umgebungen unter Windows: <http://scn.sap.com/docs/DOC-44415>. Es gibt keine in SAPinst integrierte SAP HA-Konfiguration für Linux, wie dies für Windows der Fall ist. Weitere Informationen zur SAP HA für Linux auf lokalen Systemen erhalten Sie hier: <http://scn.sap.com/docs/DOC-8541>.

### Hohe Verfügbarkeit der Azure-Infrastruktur
Derzeit gibt es auf Azure Virtual Machines keinen SLA für einen einzelnen virtuellen Computer. Um einen Eindruck der Verfügbarkeit eines einzelnen virtuellen Computers zu erhalten, können Sie einfach das Produkt aus den verschiedenen verfügbaren Azure-SLAs erstellen: <https://azure.microsoft.com/support/legal/sla/>.

Als Grundlage für die Berechnung werden 30 Tage pro Monat bzw. 43200 Minuten verwendet. Eine Ausfallzeit von 0,05 % entspricht somit 21,6 Minuten. Wie üblich multipliziert sich die Verfügbarkeit der verschiedenen Diensten auf folgende Weise:

(Verfügbarkeitsdienst 1/100) * (Verfügbarkeitsdienst 2/100) * (Verfügbarkeitsdienst 3/100) *...

Beispiel:

(99,95/100) * (99,9/100) * (99,9/100) = 0,9975 bzw. eine allgemeine Verfügbarkeit von 99,75%.

#### Hohe Verfügbarkeit von virtuellen Computern (VM)

Es gibt zwei Arten von Azure-Plattformereignissen, die sich auf die Verfügbarkeit von virtuellen Computern auswirken können: geplante und ungeplante Wartung.

* Geplante Wartungsereignisse sind regelmäßige Updates, die von Microsoft für die zugrunde liegende Azure-Plattform ausgeführt werden können, um die gesamte Verfügbarkeit, Leistung und Sicherheit der Plattforminfrastruktur zu verbessern, unter der die virtuellen Computer ausgeführt werden.
* Ungeplante Wartungsereignisse treten auf, wenn die Hardware oder physische Infrastruktur, die dem virtuellen Computer zugrunde liegt, einen Ausfall verursacht. Diese können Ausfälle des lokalen Netzwerks, Datenträgers oder andere Fehler auf Rackebene umfassen. Wenn ein solcher Fehler festgestellt wird, migriert die Azure-Plattform Ihren virtuellen Computer automatisch vom fehlerhaften physischen Server, der den virtuellen Computer hostet, zu einem fehlerfreien physischen Server. Diese Ereignisse treten selten auf, verursachen jedoch u. U. den Neustart des virtuellen Computers.

Weitere Einzelheiten finden Sie in dieser Dokumentation: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### Azure-Speicher-Redundanz

Die Daten in Ihrem Microsoft Azure Storage-Konto werden stets repliziert, um die Beständigkeit und die hohe Verfügbarkeit sicherzustellen und den Azure Storage-SLA auch bei vorübergehend auftretenden Hardwareausfällen zu erfüllen.

Da Azure Storage standardmäßig drei Images der Daten aufbewahrt, ist auf mehreren Azure-Datenträgern weder RAID5 noch RAID1 erforderlich.

Weitere Einzelheiten finden Sie in diesem Artikel: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### Neustart des virtuellen Computers der Azure-Infrastruktur für eine "höhere Verfügbarkeit" von SAP-Anwendung

Wenn Sie Funktionen wie das Windows Server-Failoverclustering (WSFC) oder ein Linux-Äquivalent nicht verwenden möchten (Letzteres wird in Azure noch nicht in Verbindung mit SAP-Software unterstützt), werden SAP-Systeme mithilfe der Neustartfunktion für virtuelle Azure-Computer vor geplanten und ungeplante Ausfallzeiten der physischen Serverinfrastruktur von Azure und der gesamten zugrundeliegenden Azure-Plattform geschützt.
 
> [AZURE.NOTE] Wichtig ist dabei, dass die Neustartfunktion für virtuelle Azure-Computer in erster Linie die virtuellen Computer und NICHT die Anwendungen schützt. Die Neustartfunktion für virtuelle Computer bietet keine hohe Verfügbarkeit für SAP-Anwendungen. Aufgrund des gewissen Maßes an Infrastrukturverfügbarkeit, das dadurch bereitgestellt wird, bietet sie jedoch indirekt eine "höhere Verfügbarkeit von SAP-Systemen. Es gibt auch keinen SLA für die Zeit, die es dauert, einen virtuellen Computer nach einem geplanten oder ungeplanten Hostausfall neu starten. Aus diesem Grund eignet sich diese Methode der "hohen Verfügbarkeit" nicht für wichtige Komponenten eines SAP-Systems wie (A)SCS oder DBMS.

Eine weiteres wichtiges Infrastrukturelement für hohe Verfügbarkeit ist der Speicher. Beispiel: Der Azure Storage-SLA bietet 99,9%ige Verfügbarkeit. Wenn alle virtuellen Computer samt Datenträgern in einem einzelnen Azure Storage-Konto bereitgestellt werden, würde ein Ausfall von Azure Storage zu einem Ausfall aller in diesem Azure Storage-Konto vorhandenen virtuellen Computer sowie aller SAP-Komponenten innerhalb dieser virtuellen Computer führen.

Anstatt alle virtuellen Computer in einem einzelnen Azure Storage-Konto bereitzustellen, können Sie auch für jeden virtuellen Speicher dedizierten Speicherkonten verwenden. Sie erhöhen durch die Verwendung mehrerer unabhängiger Azure Storage-Konten die allgemeine Verfügbarkeit der virtuellen Computer und der SAP-Anwendung.

Hier ein Beispiel der Architektur eines SAP NetWeaver-Systems, das eine Azure-Infrastruktur nutzt:
 
![Nutzung der hochverfügbaren Azure-Infrastruktur für eine "höhere Verfügbarkeit" von SAP-Anwendungen][planning-guide-figure-2900]

Für wichtige SAP-Komponenten haben wir bisher Folgendes erreicht:

* Hohe Verfügbarkeit für SAP-Anwendungsserver (AS)

SAP-Anwendungsserverinstanzen sind redundante Komponenten. Jede SAP-AS-Instanz wird auf ihrem eigenen virtuellen Computer bereitgestellt, der in einer anderen Fehler- und Upgradedomäne von Azure ausgeführt wird (siehe Kapitel [Fehlerdomänen][planning-guide-3.2.1] und [Upgradedomänen][planning-guide-3.2.2]). Dies wird durch die Verwendung von Azure-Verfügbarkeitsgruppen sichergestellt (siehe Kapitel [Azure-Verfügbarkeitsgruppen][planning-guide-3.2.3]). Infolge von potenziellen geplanten oder ungeplanten Ausfällen einer Fehler- oder Upgradedomäne fallen eine begrenzte Anzahl von virtuellen Computern mit ihren SAP-Instanzen aus. Jede SAP-AS-Instanz befindet sich in ihrem eigenen Azure Storage-Konto. Der Ausfall eines Azure Storage-Kontos würde nur zum Ausfall eines virtuellen Speichers in dessen SAP-AS-Instanz führen. Zu berücksichtigen ist jedoch, dass mit einem Azure-Abonnement nur eine begrenzte Anzahl von Azure Storage-Konten verfügbar ist. Um nach dem Neustart des virtuellen Computers den automatischen Start der (A)SCS-Instanz zu gewährleisten, legen Sie den im Kapitel [Verwenden von Autostart für SAP-Instanzen][planning-guide-11.5] beschriebenen Autostart-Parameter im Startprofil der (A)SCS-Instanz fest. Weitere Details finden Sie auch im Kapitel [Hohe Verfügbarkeit für SAP-Anwendungsserver][planning-guide-11.4.1].

* _Höhere_ Verfügbarkeit der SAP-(A)SCS-Instanz
 
Hier verwenden wir die Neustartfunktion für virtuelle Azure-Computer, um den virtuellen Computer mit der installierten SAP-(A)SCS-Instanz zu schützen. Bei geplanten oder ungeplanten Ausfallzeiten von Azure-Servern werden die virtuellen Computer auf einem anderen verfügbaren Server neu gestartet. Wie bereits erwähnt, schützt die Neustartfunktion für virtuelle Azure-Computer in erster Linie virtuelle Computer und NICHT Anwendungen, in diesem Fall die (A)SCS-Instanz. Mit der Neustartfunktion für virtuelle Computer erreichen wir indirekt eine "höhere Verfügbarkeit der SAP-(A)SCS-Instanz. Um nach dem Neustart des virtuellen Computers den automatischen Start der (A)SCS-Instanz zu gewährleisten, legen Sie den im Kapitel [Verwenden von Autostart für SAP-Instanzen beschriebenen][planning-guide-11.5] Autostart-Parameter im Startprofil der (A)SCS-Instanz fest. Dies bedeutet, dass die (A)SCS-Instanz als einzelne Fehlerquelle (Single Point of Failure, SPOF), die auf einem einzelnen virtuellen Computer ausgeführt wird, der ausschlaggebende Faktor für die Verfügbarkeit der gesamten SAP-Landschaft ist.

* _Höhere_ Verfügbarkeit des DBMS-Servers

Hier verwenden wir ähnlich wie beim Anwendungsfall der SAP-(A)SCS-Instanz die Neustartfunktion für virtuelle Azure-Computer, um den virtuellen Computer mit der installierten DBMS-Software zu schützen. Zudem erreichen wir durch den Neustart des virtuellen Computers eine "höhere Verfügbarkeit" der DBMS-Software. Ein auf einem einzelnen virtuellen Computer ausgeführtes DBMS ist ebenfalls ein SPOF und damit der ausschlaggebende Faktor für die Verfügbarkeit der gesamten SAP-Landschaft.

### Hohe Verfügbarkeit von SAP-Anwendungen in Azure IaaS
Um eine hohe Verfügbarkeit des gesamten SAP-Systems zu erreichen, müssen alle kritische SAP-Systemkomponenten wie redundante SAP-Anwendungsserver und einzigartige Komponenten (z.B. eine einzelne Fehlerquelle) wie eine SAP-(A)SCS-Instanz und DBMS schützen.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hohe Verfügbarkeit für SAP-Anwendungsserver
Für die SAP-Anwendungsserver/Dialogfeldinstanzen ist keine spezielle hoch Verfügbare Lösung erforderlich. Die hohe Verfügbarkeit wird einfach durch Redundanz erzielt, die durch die verschiedenen virtuellen Computer reichlich gegeben ist. Die virtuellen Computer sollten alle in der gleichen Azure-Verfügbarkeitsgruppe platziert werden, um zu vermeiden, dass sie während geplanter Wartungsausfallzeiten alle zur gleichen Zeit aktualisiert werden. Die grundlegende Funktion, die auf unterschiedlichen Upgrade- und Fehlerdomänen innerhalb einer Azure-Skalierungseinheit basiert, wurde bereits im Kapitel [Upgradedomänen][planning-guide-3.2.2] vorgestellt. Azure-Verfügbarkeitsgruppen wurden im Kapitel [Azure-Verfügbarkeitsgruppen][planning-guide-3.2.3] dieses Dokuments präsentiert.

Die Anzahl der von einer Azure-Verfügbarkeitsgruppe innerhalb einer Azure-Skalierungseinheit verwendbaren Fehler- und Upgradedomänen ist begrenzt. Wenn Sie einer Verfügbarkeitsgruppe eine Reihe von virtuellen Maschinen hinzufügen, werden früher oder später mehrere virtuelle Computer innerhalb derselben Fehler- oder Upgradedomäne enthalten sein.

Durch die Bereitstellung mehrerer SAP-Anwendungsserverinstanzen auf ihren dedizierten virtuellen Computern entsteht bei fünf Upgradedomänen am Ende das folgende Bild. Die aktuelle maximale Anzahl von Fehler- und Updatedomänen innerhalb einer Verfügbarkeitsgruppe kann sich im Lauf der Zeit ändern:
 
![Hohe Verfügbarkeit der SAP-Anwendungsserver in Azure][planning-guide-figure-3000]

Weitere Einzelheiten finden Sie in dieser Dokumentation: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>


#### Hohe Verfügbarkeit für die SAP-(A)SCS-Instanz unter Windows

Windows Server-Failovercluster (WSFC) ist eine häufig verwendete Lösung zum Schutz der SAP-(A)SCS-Instanz. Es ist auch in SAPinst in Form einer "hoch verfügbaren Installation" integriert. Derzeit bietet die Azure-Infrastruktur nicht die Funktionalität, um das erforderliche Windows Server-Failovercluster auf die gleiche Weise wie auf dem lokalen System einzurichten.

Ab Januar 2016 bietet die Azure-Cloud-Plattform, auf der das Windows-Betriebssystem ausgeführt wird, keine Möglichkeit mehr, um ein freigegebenes Clustervolume auf einem Datenträger zu verwenden, der von zwei virtuellen Azure-Computern genutzt wird.

Sie können jedoch Software von Drittanbietern verwenden, die durch die synchrone und transparente Datenträgerreplikation ein Freigabevolume bietet, das in WSFC integriert werden kann. Dieser Ansatz setzt voraus, dass jeweils nur der aktive Clusterknoten auf eine der Datenträgerkopien zugreifen kann. Diese hoch verfügbare Konfiguration wird seit Januar 2016 unterstützt, um die SAP-(A)SCS-Instanz in Windows-Gastbetriebssystemen auf virtuellen Azure-Computern in Verbindung mit der Drittanbietersoftware SIOS DataKeeper zu schützen.

Die SIOS DataKeeper-Lösung bietet eine freigegebene Cluster-Datenträgerressource für Windows-Failovercluster. Diese beinhaltet Folgendes:

* Eine weitere Azure-VHD, die jedem der in einer Windows-Clusterkonfiguration enthaltenen virtuellen Computer (VMs) angehängt ist
* Die auf beiden VM-Knoten ausgeführte SIOS DataKeeper Cluster Edition
* Die Konfiguration der SIOS DataKeeper Cluster Edition in einer Weise, die synchron den Inhalt des zusätzlich angehängten VHD-Volumes von virtuellen Quellcomputern im zusätzlich angehängten VHD-Volume des virtuellen Zielcomputers widerspiegelt.
* SIOS DataKeeper abstrahiert die lokalen Quell- und Zielvolumes und präsentiert diese dem Windows-Failovercluster als einen einzelnen freigegebenen Datenträger.
 
Alle Details zum Installieren eines Windows-Failoverclusters mit SIOS DataKeeper und SAP finden Sie im Whitepaper [Clustering SAP ASCS Instance using Windows Server Failover Cluster on Azure with SIOS DataKeeper][ha-guide-classic] \(Clustering der SAP-ASCS-Instanz mithilfe des Windows Server-Failoverclusters in Azure mit SIOS DataKeeper).

#### Hohe Verfügbarkeit für die SAP-(A)SCS-Instanz unter Linux
 
Seit Dezember 2015 gibt es auch keine Entsprechung mehr für den freigegebenen WSFC-Datenträger für virtuellen Linux-Computer in Azure. Alternativlösungen mit Drittanbietersoftware wie SIOS für Windows wurden noch nicht für die Ausführung von SAP unter Linux in Azure geprüft.



#### Hohe Verfügbarkeit für die SAP-Datenbankinstanz
Normalerweise basiert die SAP HA-Einrichtung für DBMS auf zwei virtuellen DBMS-Computern, wobei mit den Hochverfügbarkeitsfunktionen des DBMS Daten aus der aktiven DBMS-Instanz auf dem zweiten virtuellen Computer in eine passive DBMS-Instanz repliziert werden.

Eine Erläuterung der hohen Verfügbarkeit sowie der Funktionen zur Notfallwiederherstellung für ein allgemeines oder spezielles DBMS finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide].


#### Hohe End-to-End-Verfügbarkeit für das vollständige SAP-System

Hier sind zwei Beispiele für eine vollständige SAP NetWeaver HA-Architektur in Azure – einmal für Windows und einmal für Linux. Bei den nachfolgend erläuterten Konzepten müssen Sie möglicherweise gewisse Kompromisse eingehen, wenn Sie viele SAP-Systeme bereitstellen und die Anzahl der bereitgestellten virtuellen Computer die maximale Anzahl von Speicherkonten pro Abonnement überschreitet. In diesen Fällen müssen die VHDs virtueller Computer in einem Speicherkonto kombiniert werden. Normalerweise würden Sie zu diesem Zweck VHDs der SAP-Anwendungsebene mit virtuellen Computern verschiedener SAP-Systeme kombinieren. Wir haben zudem in einem Azure Storage-Konto verschiedene VHDs von unterschiedlichen virtuellen DBMS-Computern der verschiedenen SAP-Systemen kombiniert. Dabei wurden die IOPS-Grenzwerte für Azure Storage-Konten berücksichtigt (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>).

##### ![Windows][Logo_Windows] Hohe Verfügbarkeit unter Windows

![Hochverfügbare Anwendungsarchitektur für SAP NetWeaver mit SQL Server in Azure IaaS][planning-guide-figure-3200]

Die folgenden Azure-Konstrukte werden für das SAP NetWeaver-System verwendet, um die Auswirkungen von Infrastrukturproblemen und Host-Patches zu minimieren:

* Das vollständige System wird in Azure bereitgestellt (erforderlich – die DBMS-Ebene, die (A)SCS-Instanz und die vollständige Anwendungsebene müssen am selben Ort ausgeführt werden).
* Das vollständige System wird im Rahmen eines Azure-Abonnements (erforderlich) ausgeführt.
* Das vollständige System wird auf einem einzelnen Azure Virtual Network (erforderlich) ausgeführt.
* Die virtuellen Computer eines SAP-Systems können auch dann in drei Verfügbarkeitsgruppen aufgeteilt werden, wenn alle VMs demselben virtuellen Netzwerk angehören.
* Alle virtuellen Computer, die DBMS-Instanzen eines bestimmten SAP-Systems ausführen, sind in einer Verfügbarkeitsgruppe enthalten. Es wird davon ausgegangen, dass pro System mehrere virtuelle Computer vorhanden sind, auf denen DBMS-Instanzen ausgeführt werden, da systemeigene Hochverfügbarkeitsfunktionen für das DBMS, wie SQL Server AlwaysOn oder Oracle Data Guard, verwendet werden.
* Alle virtuellen Computer, auf denen DBMS-Instanzen ausgeführt werden, verwenden ihr eigenes Speicherkonto. Daten- und Protokolldateien des DBMS werden mit den Hochverfügbarkeitsfunktionen des DBMS, die zur Datensynchronisierung dienen, von einem Speicherkonto zum nächsten repliziert. Bei Nichtverfügbarkeit eines Speicherkontos fällt ein SQL-Windows-Clusterknoten aber nicht der gesamte SQL Server-Dienst aus.
* Alle virtuellen Computer, die (A)SCS-Instanzen eines bestimmten SAP-Systems ausführen, sind in einer Verfügbarkeitsgruppe enthalten. In diesen virtuellen Computern ist zum Schutz der (A)SCS-Instanz das Windows Server-Failovercluster (WSFC) konfiguriert.
* Alle virtuellen Computer, auf denen (A)SCS-Instanzen ausgeführt werden, verwenden ihr eigenes Speicherkonto. (A)SCS-Instanzdateien und globale SAP-Ordner werden mithilfe der SIOS DataKeeper-Replikation von einem Speicherkonto zum nächsten repliziert. Bei Nichtverfügbarkeit eines Speicherkontos fällt ein (A)SCS-Windows-Clusterknoten aber nicht der gesamte (A)SCS-Dienst aus.
* ALLE virtuellen Computer, welche die SAP-Anwendungsserverebene repräsentieren, sind in einer dritten Verfügbarkeitsgruppe enthalten.
* ALLE virtuellen Computer, auf denen SAP-Anwendungsserver ausgeführt werden, verwenden ihr eigenes Speicherkonto. Bei Nichtverfügbarkeit eines Speicherkontos fällt ein SAP-Anwendungsserver aus, während die anderen SAP-AS weiter ausgeführt werden.

##### ![Linux][Logo_Linux] Hohe Verfügbarkeit unter Linux

Die Architektur für SAP HA unter Linux in Azure ist im Grunde mit der oben für Windows beschriebenen Architektur identisch. Seit Januar 2016 gelten jedoch zwei Einschränkungen:

* Derzeit wird nur SAP ASE 16 unter Linux in Azure ohne jegliche ASE-Replikationsfeatures unterstützt.
* Es wird noch keine SAP (A)SCS HA-Lösung unter Linux in Azure unterstützt.

Infolgedessen können SAP-Linux-Azure-Systeme seit Januar 2016 aufgrund der fehlenden (A)SCS-Instanz und der SAP ASE-Einzelinstanzdatenbank nicht die gleiche Verfügbarkeit wie SAP-Windows-Azure-Systeme bieten.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Verwenden von Autostart für SAP-Instanzen

SAP bot diese Funktion zum Starten von SAP-Instanzen unmittelbar nach dem Start des Betriebssystems auf dem virtuellen Computer an. Die genauen Schritte finden Sie im SAP Knowledge Base-Artikel [1909114]\: „How to start SAP instances automatically using parameter Autostart“ (Automatisches Starten von SAP-Instanzen mit dem Autostart-Parameter). SAP rät jetzt jedoch von der Verwendung dieser Einstellung ab, da die Reihenfolge der Instanzneustarts nicht gesteuert wird, in der Annahme, dass mehrere virtuelle Computer betroffen sind oder mehrere Instanzen pro virtuellem Computer ausgeführt wurden. Bei einem typischen Azure-Szenario mit einer SAP-Anwendungsserverinstanz auf einem virtuellen Computer, bei dem ein einzelner virtueller Computer neu gestartet wird, ist der Autostart-Parameter nicht wirklich wichtig und kann durch Hinzufügen des Parameters aktiviert werden:

	Autostart = 1

In das Startprofil der SAP ABAP- und/oder Java-Instanz.

> [AZURE.NOTE] 
Der Autostart-Parameter kann auch Nachteile haben. Der Parameter löst den Start einer SAP ABAP- oder Java-Instanz aus, wenn der mit der Instanz verbundene Windows-/Linux-Dienst gestartet wird. Das erfolgt in jedem Fall beim Hochfahren der Betriebssysteme. Neustarts von SAP-Diensten sind jedoch auch für die Lebenszyklusmanagementfunktionen der SAP-Software wie SUM oder andere Updates bzw. Upgrades gängig. Bei diesen Funktionen wird kein automatischer Neustart einer Instanz erwartet. Aus diesem Grund sollte der Autostart-Parameter vor dem Ausführen dieser Aufgaben deaktiviert werden. Die Autostart-Parameter sollte auch nicht für geclusterte SAP-Instanzen wie ASCS/SCS/CI verwendet werden.

Weitere Informationen zur Autostart-Funktion für SAP-Instanzen finden Sie hier:

* [Start/Stop SAP along with your Unix Server Start/Stop (Starten/Beenden von SAP zusammen mit Ihrem Unix-Server)](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starting and Stopping SAP NetWeaver Management Agents (Starten und Beenden von SAP NetWeaver-Verwaltungsagents)](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [How to enable auto Start of HANA Database (Aktivieren des automatischen Starts der HANA-Datenbank)](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)


### Größere SAP-Systeme mit drei Ebenen
Die Aspekte der hohen Verfügbarkeit von SAP-Konfigurationen mit 3 Ebenen wurden bereits in früheren Abschnitten erläutert. Was ist jedoch mit Systemen, auf denen die Anforderungen der DBMS-Server zu groß sind, um die in Azure zu platzieren, während die SAP-Anwendungsebene in Azure bereitgestellt werden könnte?

#### Speicherort von SAP-Konfigurationen mit drei Ebenen

Weder die Anwendungsebene an sich noch die Anwendung und die DBMS-Ebene können auf das lokale System und Azure aufgeteilt werden. SAP-Systeme werden entweder vollständig lokal oder in Azure bereitgestellt. Zudem ist es nicht möglich, einige der Anwendungsserver lokal und andere in Azure auszuführen. Das ist der Ausgangspunkt der Erörterung. Des Weiteren können die DBMS-Komponenten eines SAP-Systems und die SAP-Server-Anwendungsserverebene nicht in zwei verschiedenen Azure-Regionen bereitgestellt werden. Beispiel: DBMS in USA (Westen) und die SAP-Anwendungsebene in USA (Mitte). Diese Konfigurationen werden aufgrund der Empfindlichkeit der SAP NetWeaver-Architektur gegenüber Latenzen nicht unterstützt.

Rechenzentrumspartner haben jedoch im vergangenen Jahr weitere Standorte (Kolokationen) für Azure-Regionen entwickelt. Diese Kolokationen befinden sich häufig in der Nähe der physischen Azure-Rechenzentren innerhalb einer Azure-Region. Durch die kurze Entfernung und die Verbindung von Ressourcen in der Kolokation über ExpressRoute mit Azure kann die Latenz auf unter 2 ms sinken. In diesen Fällen kann die DBMS-Ebene (einschließlich SAN/NAS) an einer solchen Kolokation und die SAP-Anwendungsebene in Azure platziert werden. Es bestehen seit Dezember 2015 noch keinerlei derartige Bereitstellungen. Diverse Kunden mit Nicht-SAP-Anwendungsbereitstellungen nutzen solche Ansätze jedoch bereits.

### Offline-Sicherung von SAP-Systemen

Je nach SAP-Konfiguration (2 oder 3 Ebenen) ist möglicherweise eine Sicherung erforderlich. Dabei sollte der Inhalt des virtuellen Computers an sich sowie die Datenbank gesichert werden. Die DBMS-bezogene Sicherungen sollten mit Datenbankmethoden durchgeführt werden. Eine ausführliche Beschreibung der unterschiedlichen Datenbanken finden Sie im [DBMS-Handbuch][dbms-guide]. Andererseits können SAP-Daten (einschließlich des Datenbankinhalts) auch wie in diesem Abschnitt beschrieben offline gesichert werden. Die Online-Sicherung wird im nächsten Abschnitt erläutert.

Für die Offline-Sicherung muss der virtuelle Computer im Grunde über das Azure-Portal heruntergefahren werden. Anschließend sollte ein Kopie des VM-Basisdatenträgers einschließlich aller mit dem virtuellen Computer verbundenen VHDs erstellt werden. Dadurch wird ein Image des virtuellen Computers und des damit verbundenen Datenträgers zu einem bestimmten Zeitpunkt erstellt. Es wird empfohlen, die Sicherungen in ein anderes Azure Storage-Konto zu kopieren. Gehen Sie hierfür wie im Kapitel [Kopieren von Datenträgern zwischen Azure Storage-Konten][planning-guide-5.4.2] dieses Dokuments vor. Das Herunterfahren kann nicht nur über das Azure-Portal, sondern auch über PowerShell oder CLI erfolgen. Eine Beschreibung finden Sie hier: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Bei einer Wiederherstellung dieses Zustands werden sowohl der virtuelle Basiscomputer als auch dessen Originaldatenträger sowie die hinzugefügten VHDs gelöscht. Die gespeicherten VHDs werden in das ursprüngliche Azure-Konto zurück kopiert. Anschließend wird das System erneut bereitgestellt. Dieser Artikel erläutert anhand eines Beispiels, wie dieser Prozess in PowerShell geschrieben wird: <http://www.westerndevs.com/azure-snapshots/>

Wichtig ist, dass Sie eine neue SAP-Lizenz installieren, da beim Wiederherstellen einer VM-Sicherung wie oben beschrieben ein neuer Hardwareschlüssel erstellt wird.

### Online-Sicherung von SAP-Systemen

Das DBMS wird mit speziellen DBMS-Methoden gesichert. Eine Beschreibung finden Sie im [DBMS-Handbuch][dbms-guide].

Andere virtuelle Computer im SAP-System können mit der Sicherungsfunktion für virtuelle Azure-Computer gesichert werden. Die Sicherung virtueller Azure-Computer wurde Anfang 2015 eingeführt und ist mittlerweile eine Standardmethode zum Sichern eines vollständigen virtuellen Computers in Azure. Azure Backup speichert die Sicherungen in Azure und ermöglicht eine erneute Wiederherstellung von einem virtuellen Computer.

> [AZURE.NOTE] 
Seit Dezember 2015 wird bei der Sicherung virtueller Computer NICHT mehr die eindeutige VM-ID beibehalten, die für die SAP-Lizenzierung erforderlich ist. Dies bedeutet, dass bei der Wiederherstellung von einer VM-Sicherung ein neuer SAP-Lizenzschlüssel installiert werden muss. Der wiederhergestellte virtuelle Computer gilt als neue VM und nicht als Ersatz der zuvor gespeicherten VM. Seit Januar 2016 unterstützt die Sicherungsfunktion für virtuelle Azure-Computer keine VMs mehr, die noch mit dem Azure-Ressourcen-Manager bereitgestellt werden.

> ![Windows][Logo_Windows] Windows
>
> Virtuelle Computer, auf denen Datenbanken ausgeführt werden, können theoretisch auch konsistent gesichert werden, wenn die DBMS-Systeme Windows VSS (Volume Shadow Copy Service, Volumenschattenkopie-Dienst – <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>) ebenso wie zum Beispiel SQL Server unterstützen. Bedenken Sie jedoch, dass auf Basis der Sicherungen virtueller Azure-Computer keine Zeitpunktwiederherstellung von Datenbanken möglich ist. Daher wird empfohlen, die Sicherung von Datenbanken mit DBMS-Funktionen durchzuführen und nicht die Sicherung virtueller Azure-Computer zu verwenden.
>
> Hier haben Sie Gelegenheit, sich mit der Sicherungsfunktion für virtuelle Azure-Computer vertraut zu machen: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Zudem können Sie Datenbanken mit einer Kombination aus Microsoft Data Protection Manager, der auf einem virtuellen Azure-Computer installiert ist, und der Azure-Sicherung sichern/wiederherstellen. Weitere Informationen finden Sie hier: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.


> ![Linux][Logo_Linux] Linux
> 
> Es gibt unter Linux keine Entsprechung zu Windows VSS. Daher sind nur dateikonsistente Sicherungen aber keine anwendungskonsistenten Sicherungen möglich. Die SAP-DBMS-Sicherung sollte mit DBMS-Funktionen erfolgen. Das Dateisystem mit den SAP-bezogenen Daten kann beispielsweise mit „tar“ gespeichert werden. Eine Beschreibung finden Sie hier: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>


### Azure als DR-Standort für die SAP-Produktionslandschaften

Seit Mitte 2014 ermöglichen Erweiterungen verschiedener Komponenten zu Hyper-V, System Center und Azure die Verwendung von Azure als DR-Standort für lokal auf der Basis von Hyper-V ausgeführte virtuelle Computer.

Einen Blog mit Informationen zum Bereitstellen dieser Lösung finden Sie hier: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## Zusammenfassung
Hier die wichtigsten Punkte der hohen Verfügbarkeit für SAP-Systeme in Azure:

* Derzeit können die einzelnen SAP-Fehlerquelle nicht exakt auf die gleiche Weise wie in lokalen Bereitstellungen geschützt werden. Der Grund dafür ist, dass Cluster mit freigegebenen Datenträgern noch nicht ohne die Verwendung von Drittanbietersoftware in Azur erstellt werden können.
* Für die DBMS-Ebene müssen DBMS-Funktionen verwendet werden, die von der Clustertechnologie für freigegebene Datenträger unabhängig sind. Details finden Sie im [DBMS-Handbuch][dbms-guide].
* Um die Auswirkungen von Problemen in Fehlerdomänen innerhalb der Azure-Infrastruktur oder bei der Hostwartung zu minimieren, sollten Sie Azure-Verfügbarkeitsgruppen verwenden:
	* Es wird empfohlen, eine Verfügbarkeitsgruppe für die SAP-Anwendungsebene zu verwenden.
	* Es wird empfohlen, eine separate Verfügbarkeitsgruppe für die SAP-DBMS-Ebene zu verwenden.
	* Es wird NICHT empfohlen, dieselbe Verfügbarkeitsgruppe auf virtuelle Computer unterschiedlicher SAP-Systeme anzuwenden.
* Informationen zum Sichern der SAP-DBMS-Ebene finden Sie im [DBMS-Handbuch][dbms-guide].
* Das Sichern von SAP-Dialoginstanzen ist nicht sinnvoll, da es bei einfachen Dialoginstanzen in der Regel schneller geht, sie erneut bereitzustellen.
* Es ist empfehlenswert, den virtuellen Computer, der das globale Verzeichnis des SAP-Systems enthält, einschließlich aller Profile der unterschiedlichen Instanzen zu sichern. Dies sollte beispielsweise mit der Windows-Sicherung oder unter Linux mit "tar" durchgeführt werden. Aufgrund von Unterschieden zwischen Windows Server 2008 (R2) und Windows Server 2012 (R2) ist die Sicherung mit den neueren Windows Server-Versionen einfacher. Es wird daher empfohlen, Windows Server 2012 (R2) als Windows-Gastbetriebssystem ausgeführt.

<!---HONumber=AcomDC_0907_2016-->