<properties
   pageTitle="SAP NetWeaver auf virtuellen Linux-Computern – DBMS-Bereitstellungshandbuch | Microsoft Azure"
   description="SAP NetWeaver auf virtuellen Linux-Computern (VMs) – DBMS-Bereitstellungshandbuch"
   services="virtual-machines-linux,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="juergent"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch

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

[dbms-guide]: virtual-machines-linux-sap-dbms-guide.md "SAP NetWeaver auf virtuellen Linux-Computern (VMs) – DBMS-Bereitstellungshandbuch"
[dbms-guide-2.1]: virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f "Caching für VMs und VHDs"
[dbms-guide-2.2]: virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 "Software-RAID"
[dbms-guide-2.3]: virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 "Microsoft Azure Storage"
[dbms-guide-2]: virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 "Struktur einer RDBMS-Bereitstellung"
[dbms-guide-3]: virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 "Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern"
[dbms-guide-5.5.1]: virtual-machines-linux-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 "SQL Server 2012 SP1 CU4 und höher"
[dbms-guide-5.5.2]: virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b "SQL Server 2012 SP1 CU3 und frühere Versionen"
[dbms-guide-5.6]: virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 "Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace"
[dbms-guide-5.8]: virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 "SQL Server für SAP in Azure – Allgemeine Zusammenfassung"
[dbms-guide-5]: virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 "Besonderheiten bei SQL Server-RDBMS"
[dbms-guide-8.4.1]: virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 "Speicherkonfiguration"
[dbms-guide-8.4.2]: virtual-machines-linux-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d "Sichern und Wiederherstellen"
[dbms-guide-8.4.3]: virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c "Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung"
[dbms-guide-8.4.4]: virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 "Andere"
[dbms-guide-900-sap-cache-server-on-premises]: virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]: ./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]: ./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]: ./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]: ./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]: ./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]: ./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]: ./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]: ./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]: ./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]: virtual-machines-linux-sap-deployment-guide.md "SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Bereitstellungshandbuch"
[deployment-guide-2.2]: virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 "SAP-Ressourcen"
[deployment-guide-3.1.2]: virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab "Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image"
[deployment-guide-3.2]: virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 "Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP"
[deployment-guide-3.3]: virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 "Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP"
[deployment-guide-3.4]: virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 "Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP"
[deployment-guide-3]: virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e "Bereitstellungsszenarios für virtuelle Computer für SAP in Microsoft Azure"
[deployment-guide-4.1]: virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 "Bereitstellen von Azure PowerShell-Cmdlets"
[deployment-guide-4.2]: virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e "Herunterladen und Importieren von SAP-relevanten PowerShell-Cmdlets"
[deployment-guide-4.3]: virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc "Einbinden virtueller Computer in die lokale Domäne – nur Windows"
[deployment-guide-4.4.2]: virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 "Linux"
[deployment-guide-4.4]: virtual-machines-linux-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d "Herunterladen, Installieren und Aktivieren von Azure VM-Agent"
[deployment-guide-4.5.1]: virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 "Azure PowerShell"
[deployment-guide-4.5.2]: virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f "Azure-Befehlszeilenschnittstelle"
[deployment-guide-4.5]: virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca "Konfigurieren der erweiterten Azure-Überwachung für SAP"
[deployment-guide-5.1]: virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 "Bereitschaftsüberprüfung für die erweiterte Azure-Überwachung für SAP"
[deployment-guide-5.2]: virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 "Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur"
[deployment-guide-5.3]: virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 "Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP"

[deployment-guide-configure-monitoring-scenario-1]: virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b "Konfigurieren der Überwachung"
[deployment-guide-configure-proxy]: virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d "Konfigurieren von Proxys"
[deployment-guide-figure-100]: ./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]: ./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]: virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]: ./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]: ./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]: ./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]: virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]: ./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]: ./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]: ./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]: virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]: ./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]: ./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]: virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]: ./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]: virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]: ./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]: ./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]: ./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]: virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]: virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]: virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]: virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b "Überprüfungen und Problembehandlung für die Einrichtung der End-to-End-Überwachung für SAP in Azure"

[deploy-template-cli]: ../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]: ../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]: ../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]: http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]: virtual-machines-linux-sap-get-started.md
[getting-started-dbms]: virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]: virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]: virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]: virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]: virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]: virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]: virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]: virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]: virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]: http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]: virtual-machines-linux-enable-aem.md

[Logo_Linux]: ./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]: ./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]: https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]: virtual-machines-linux-sap-planning-guide.md "SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Planungs- und Implementierungshandbuch"
[planning-guide-1.2]: virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff "Ressourcen"
[planning-guide-11]: virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 "Hohe Verfügbarkeit (HA) und Disaster Recovery (DR) für SAP NetWeaver auf virtuellen Azure-Computern"
[planning-guide-11.4.1]: virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 "Hohe Verfügbarkeit für SAP-Anwendungsserver"
[planning-guide-11.5]: virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f "Verwenden von Autostart für SAP-Instanzen"
[planning-guide-2.1]: virtual-machines-linux-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 "Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk"
[planning-guide-2.2]: virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 "Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk"
[planning-guide-3.1]: virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a "Azure-Regionen"
[planning-guide-3.2.1]: virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 "Fehlerdomänen"
[planning-guide-3.2.2]: virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 "Upgradedomänen"
[planning-guide-3.2.3]: virtual-machines-linux-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 "Azure-Verfügbarkeitsgruppen"
[planning-guide-3.2]: virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 "Das Konzept der virtuellen Microsoft Azure-Computer"
[planning-guide-3.3.2]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Azure Premium-Speicher"
[planning-guide-5.1.1]: virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 "Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure"
[planning-guide-5.1.2]: virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c "Bereitstellen eines virtuellen Computers mit einem kundenspezifischen Image"
[planning-guide-5.2.1]: virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef "Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure"
[planning-guide-5.2.2]: virtual-machines-linux-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 "Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP"
[planning-guide-5.2]: virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 "Vorbereiten virtueller Computer mit SAP für Azure"
[planning-guide-5.3.1]: virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 "Unterschied zwischen einem Azure-Datenträger und einem Azure-Image"
[planning-guide-5.3.2]: virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a "Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure"
[planning-guide-5.4.2]: virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 "Kopieren von Datenträgern zwischen Azure-Speicherkonten"
[planning-guide-5.5.1]: virtual-machines-linux-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 "VM-/VHD-Struktur für SAP-Bereitstellungen"
[planning-guide-5.5.3]: virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d "Festlegen der automatischen Bereitstellung für angefügte Datenträger"
[planning-guide-7.1]: virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 "Einzelner virtueller Computer mit SAP NetWeaver-Demo/Schulungsszenario"
[planning-guide-7]: virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 "Konzepte für Nur-Cloud-Bereitstellung von SAP-Instanzen"
[planning-guide-9.1]: virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 "Azure-Überwachungslösung für SAP"
[planning-guide-azure-premium-storage]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Azure Premium-Speicher"

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
[planning-guide-microsoft-azure-networking]: virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd "Microsoft Azure-Netzwerk"
[planning-guide-storage-microsoft-azure-storage-and-data-disks]: virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f "Speicher: Microsoft Azure Storage und Datenträger"

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
[virtual-machines-azure-resource-manager-architecture]: ../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]: virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]: virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]: virtual-machines-linux-cli-deploy-templates.md "Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI"
[virtual-machines-deploy-rmtemplates-powershell]: virtual-machines-windows-ps-manage.md "Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager und PowerShell"
[virtual-machines-linux-agent-user-guide]: virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]: virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]: virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]: virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]: virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]: virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]: virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]: virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]: virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]: virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]: virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]: virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]: virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]: virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]: virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]: virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]: virtual-machines-linux-sizes.md
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
[vpn-gateway-site-to-site-create]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]: ../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]: ../xplat-cli-install.md
[xplat-cli-azure-resource-manager]: ../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] klassisches Bereitstellungsmodell.

Dieses Handbuch ist Teil der Dokumentation zur Implementierung und Bereitstellung der SAP-Software in Microsoft Azure. Lesen Sie vor diesem Handbuch das [Planungs- und Implementierungshandbuch][planning-guide]. In diesem Dokument wird die Bereitstellung von verschiedenen Managementsystemen für relationale Datenbanken (Relational Database Management System, RDBMS) und verwandten Produkten mit SAP auf virtuellen Microsoft Azure-Computern mit den Azure-IaaS-Funktionen (Infrastructure as a Service) erläutert.

Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## Allgemeine Hinweise
In diesem Kapitel werden Überlegungen zum Ausführen von DBMS-Systemen für SAP auf virtuellen Azure-Computern vorgestellt. Das Kapitel enthält nur wenige Verweise auf spezifische DBMS-Systeme. Stattdessen werden die spezifischen DBMS-Systeme weiter unten in diesem Artikel behandelt.

### Definitionen
Im gesamten Dokument werden die folgenden Begriffe verwendet:

* IaaS: Infrastructure as a Service.
* PaaS: Platform as a Service.
* SaaS: Software as a Service.
* SAP-Komponente: Eine einzelne SAP-Anwendung wie ECC, BW, Solution Manager oder EP. SAP-Komponenten können auf herkömmlichen ABAP- oder Java-Technologien oder auf einer Nicht-NetWeaver-basierten Anwendung wie Business Objects basieren.
* SAP-Umgebung: eine oder mehrere SAP-Komponenten, die logisch gruppiert sind, um eine Geschäftsfunktion wie Entwicklung, QAS, Schulung, DR oder Produktion auszuführen.
* SAP-Landschaft: Dies bezieht sich auf alle SAP-Assets in der IT-Landschaft eines Kunden. Die SAP-Landschaft umfasst alle Produktions- und anderen Umgebungen.
* SAP-System: Die Kombination aus DBMS-Ebene und Anwendungsebene, z.B. in einem SAP-ERP-Entwicklungssystem, SAP BW-Testsystem, SAP CRM-Produktionssystem o.ä. In Azure-Bereitstellungen wird die Aufteilung dieser beiden Ebenen zwischen lokalen Systemen und Azure nicht unterstützt. Dies bedeutet, die ein SAP-System entweder lokal oder in Azure bereitgestellt wird. Allerdings können Sie die verschiedenen Systeme einer SAP-Landschaft in Azure oder lokal bereitstellen. Sie konnte z.B. die SAP CRM-Entwicklungs- und Testsysteme in Azure und die SAP CRM-Produktionssysteme lokal bereitstellen.
* Nur-Cloud-Bereitstellung: Eine Bereitstellung, in der das Azure-Abonnement nicht über eine Site-to-Site- oder ExpressRoute-Verbindung mit der lokalen Netzwerkinfrastruktur verbunden ist. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als „Cloud-Only“-Bereitstellungen bezeichnet. Der Zugriff auf mit dieser Methode bereitgestellte virtuelle Computer erfolgt über das Internet und öffentliche Internet-Endpunkte, die den virtuellen Computern in Azure zugewiesen sind. Das lokale Active Directory (AD) und DNS werden in diesen Bereitstellungen nicht auf Azure ausgeweitet. Daher sind die virtuellen Computer nicht Teil des lokalen Active Directory. Hinweis: Nur-Cloud-Bereitstellungen in diesem Dokument sind als vollständige SAP-Landschaften definiert, die ausschließlich in Azure ohne Erweiterung von Active Directory oder Namensauflösung vom lokalen System in die Public Cloud ausgeführt werden. Nur-Cloud-Konfigurationen werden für SAP-Produktionssysteme oder Konfigurationen nicht unterstützt, bei denen SAP STMS oder andere lokale Ressourcen zwischen in Azure gehosteten SAP-Systemen und lokalen Ressourcen verwendet werden müssen.
* Standortübergreifend: Beschreibt ein Szenario, in dem virtuelle Computer mit einem Azure-Abonnement bereitgestellt werden, das Site-to-Site-, Multisite- oder ExpressRoute-Verbindungen zwischen den lokalen Rechenzentren und Azure umfasst. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als „Cross-Premises“-Szenarien bezeichnet. Durch die Verbindung sollen lokale Domänen, das lokale Active Directory und lokales DNS auf Azure ausgeweitet werden. Die lokale Landschaft wird auf die Azure-Ressourcen des Abonnements erweitert. Durch diese Erweiterung können die virtuellen Computer Teil der lokalen Domäne sein. Domänenbenutzer der lokalen Domäne können auf die Server zugreifen und Dienste auf diesen virtuellen Computern ausführen (z.B. DBMS-Dienste). Die Kommunikation und Namensauflösung zwischen lokal bereitgestellten virtuellen Computern und in Azure bereitgestellten virtuellen Computern ist möglich. Wir erwarten, dass dies das häufigste Szenario für die Bereitstellung von SAP-Assets in Azure sein wird. In [diesem Artikel][vpn-gateway-cross-premises-options] und [diesem Artikel][vpn-gateway-site-to-site-create] finden Sie weitere Informationen.

> [AZURE.NOTE] Standortübergreifende Bereitstellungen von SAP-Systemen, in denen virtuelle Azure-Computer mit SAP-Systemen Mitglieder einer lokalen Domäne sind, werden für SAP-Produktionssysteme unterstützt. Standortübergreifende Konfigurationen werden für die Bereitstellung von Teilen von SAP-Landschaften oder vollständigen SAP-Landschaften in Azure unterstützt. Auch für das Ausführen von vollständigen SAP-Landschaften in Azure müssen diese virtuellen Computer Teil der lokalen Domäne und ADS sein. In früheren Versionen der Dokumentation wurden Hybrid-IT-Szenarien beschrieben, wobei sich „Hybrid“ darauf bezieht, dass es eine standortübergreifende Verbindung zwischen lokalen Systemen und Azure gibt. In diesem Fall bedeutet „Hybrid“ auch, dass die virtuellen Computer in Azure Teil des lokalen Active Directory sind.

In einigen Microsoft-Dokumentationen werden standortübergreifende Szenarien etwas anders beschrieben, insbesondere bei DBMS-HA-Konfigurationen. In Dokumenten zu SAP bezieht sich ein standortübergreifendes Szenario einfach nur auf eine Site-to-Site- oder private Verbindung (ExpressRoute) und die Tatsache, dass die SAP-Landschaft zwischen lokalen Systemen und Azure verteilt ist.

### Ressourcen
Die folgenden Handbücher zum Thema SAP-Bereitstellungen in Azure sind verfügbar:

* [SAP NetWeaver in Azure Virtual Machines – Planungs- und Implementierungshandbuch][planning-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – Bereitstellungshandbuch][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch (dieses Dokument)][dbms-guide]

Die folgenden SAP-Hinweise beziehen sich auf das Thema SAP in Azure:

| Hinweisnummer | Titel
|------------|--------
| [1928533] | SAP Applications on Azure: Supported Products and Azure VM types (SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen)
| [2015553] | SAP on Microsoft Azure: Support Prerequisites (SAP in Microsoft Azure: Voraussetzungen für die Unterstützung)
| [1999351] | Problembehandlung für die erweiterte Azure-Überwachung für SAP
| [2178632] | Wichtige Überwachungsmetriken für SAP in Microsoft Azure
| [1409604] | Virtualisierung unter Windows: erweiterte Überwachung
| [2191498] | SAP on Linux with Azure: Enhanced Monitoring (SAP unter Linux mit Azure: Erweiterte Überwachung)
| [2039619] | SAP Applications on Microsoft Azure using the Oracle Database: Supported Products and Versions (SAP-Anwendungen in Microsoft Azure mit der Oracle-Datenbank: Unterstützte Produkte und Versionen)
| [2233094] | DB6: SAP Applications on Azure Using IBM DB2 for Linux, UNIX, and Windows - Additional Information (DB6: SAP-Anwendungen in Azure mit IBM DB2 für Linux, UNIX und Windows – Weitere Informationen)
| [2243692] | Linux auf Microsoft Azure-VMs (IaaS): SAP-Lizenzprobleme
| [1984787] | SUSE LINUX Enterprise Server 12: Installationshinweise
| [2002167] | Red Hat Enterprise Linux 7.x: Installation und Upgrade

Lesen Sie bitte auch das [SCN-Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes), das alle SAP-Hinweise für Linux enthält.

Sie sollten über Kenntnisse zur Microsoft Azure-Architektur sowie zur Bereitstellung und Verwendung von virtuellen Microsoft Azure-Computern verfügen. Weitere Informationen finden Sie hier: <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Hier werden **nicht** die Microsoft Azure-PaaS-Angebote (Platform as a Service) der Microsoft Azure Platform erläutert. In diesem Dokument geht es darum, ein Datenbankverwaltungssystem (Database Management System, DBMS) auf virtuellen Microsoft Azure-Computern (IaaS) so wie in Ihrer lokalen Umgebung auszuführen. Die Datenbankfunktionen und -features für diese beiden Angebote unterscheiden sich stark und sollten nicht verwechselt werden. Siehe auch: <https://azure.microsoft.com/services/sql-database/>

Da es um IaaS geht, sind Installation und Konfiguration von Windows, Linux und DBMS im Allgemeinen weitgehend identisch mit allen virtuellen Computern oder Bare-Metal-Computern, die Sie lokal installieren. Es gibt jedoch einige Entscheidungen bei der Implementierung der Architektur und Systemverwaltung, die bei der Nutzung von IaaS anders sind. In diesem Dokument sollen die spezifischen Unterschiede bei Architektur und Systemverwaltung beschrieben werden, auf die Sie bei der Verwendung von IaaS vorbereitet sein müssen.

Die allgemeinen Unterschiede werden für die folgenden Bereiche beschrieben:

* Planen des richtigen VM-/VHD-Layouts der SAP-Systeme, um sicherzustellen, dass Sie das richtige Datendateilayout verwenden und ausreichende IOPS für Ihre Workloads erzielen.
* Überlegungen zur Netzwerknutzung bei Verwendung von IaaS.
* Spezifische Datenbankfunktionen, die für die Optimierung des Datenbanklayouts verwendet werden.
* Überlegungen zu Sicherung und Wiederherstellung in IaaS.
* Verwenden von verschiedenen Imagetypen für die Bereitstellung.
* Hohe Verfügbarkeit in Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktur einer RDBMS-Bereitstellung
Damit Sie die Informationen in diesem Kapitel nachvollziehen können, müssen Sie [dieses Kapitel][deployment-guide-3] im [Bereitstellungshandbuch][deployment-guide] verstanden haben. Sie sollten Kenntnisse über die verschiedenen VM-Serien und ihre Unterschiede sowie zu den Unterschieden zwischen Azure-Standardspeicher und Storage Premium haben, um dieses Kapitel zu verstehen.

Bis März 2015 waren Azure-VHDs, die ein Betriebssystem enthalten, auf eine Größe von 127 GB beschränkt. Diese Einschränkung wurde im März 2015 aufgehoben (weitere Informationen finden Sie unter <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Seitdem können VHDs mit dem Betriebssystem die gleiche Größe wie jede andere VHD haben. Trotzdem wird eine Bereitstellungsstruktur bevorzugt, bei der das Betriebssystem, DBMS und später SAP-Binärdateien von den Datenbankdateien getrennt sind. Daher gehen wir davon aus, dass bei SAP-Systemen, die auf virtuellen Azure-Computern ausgeführt werden, der virtuelle Basiscomputer (oder die Basis-VHD) mit dem Betriebssystem, den ausführbaren Dateien für das Datenbankverwaltungssystem und den ausführbaren SAP-Dateien installiert wird. Die DBMS-Datendateien und -Protokolldateien werden in Azure-Speicher (Standardspeicher oder Storage Premium) in separaten VHD-Dateien gespeichert und als logische Datenträger an den ursprünglichen virtuellen Azure-Computer mit dem Betriebssystemimage angefügt.

Abhängig davon, ob Azure-Standardspeicher oder Storage Premium verwendet wird (z.B. bei virtuellen Computern der DS- oder GS-Serie), gibt es verschiedene Kontingente in Azure. Diese werden [hier][virtual-machines-sizes] dokumentiert. Bei der Planung Ihrer Azure-VHDs müssen Sie den besten Kompromiss zwischen den Kontingenten im Hinblick auf folgende Aspekte ermitteln:

* Die Anzahl der Datendateien.
* Die Anzahl von VHDs, die die Dateien enthalten.
* Die IOPS-Kontingente einer einzelnen VHD.
* Der Datendurchsatz pro VHD.
* Die Anzahl der für die jeweilige Größe des virtuellen Computers möglichen zusätzlichen VHDs.
* Der Gesamtspeicherdurchsatz, den ein virtueller Computer bereitstellen kann.
 
Azure erzwingt ein IOPS-Kontingent pro VHD. Diese Kontingente sind für VHDs, die in Azure-Standardspeicher und Storage Premium gehostet werden, unterschiedlich. Die E/A-Latenzen sind bei den beiden Speichertypen ebenfalls sehr unterschiedlich, wobei Storage Premium um Größenordnungen bessere E/A-Latenzen bietet. Für jeden der verschiedenen Typen von virtuellen Computern können Sie eine begrenzte Anzahl von VHDs anfügen. Eine weitere Einschränkung besteht darin, dass nur bestimmte Typen von virtuellen Computern Azure Storage Premium nutzen können. Dies bedeutet, dass die Entscheidung für einen bestimmten virtuellen Computertyp nicht nur durch die Anforderungen an CPU und Arbeitsspeicher bestimmt wird, sondern auch durch IOPS-Wert, Latenz und Anforderungen an den Datenträgerdurchsatz, die normalerweise abhängig von der Anzahl der VHDs und dem Typ der Storage Premium-Datenträger skaliert werden. Vor allem bei Storage Premium wird die Größe einer virtuellen Festplatte möglicherweise auch von der Anzahl von IOPS und dem Durchsatz diktiert, der von den einzelnen VHD erzielt werden muss.

Die Tatsache, dass die IOPS-Gesamtrate, die Anzahl der bereitgestellten virtuellen Festplatten und die Größe des virtuellen Computers zusammenhängen, kann dazu führen, dass die Azure-Konfiguration eines SAP-Systems von der in einer lokalen Bereitstellung abweicht. Die IOPS-Grenzwerte pro LUN können bei lokalen Bereitstellungen normalerweise konfiguriert werden. Bei Azure Storage sind diese Grenzwerte fest bzw. bei Storage Premium abhängig vom Datenträgertyp. Daher gibt es bei lokalen Bereitstellungen Kundenkonfigurationen von Datenbankservern, in denen viele verschiedene Volumes für spezielle ausführbare Dateien wie SAP und das DBMS oder spezielle Volumes für temporäre Datenbanken oder Tablespaces verwendet werden. Wenn ein solches lokales System in Azure verschoben wird, kann dies zu einer Verschwendung potenzieller IOPS-Bandbreite führen, indem eine VHD für ausführbare Dateien oder Datenbanken verwendet wird, die keine oder wenige IOPS ausführen. Aus diesem Grund wird bei virtuellen Azure-Computern empfohlen, das DBMS und ausführbare SAP-Dateien möglichst auf dem Datenträger mit dem Betriebssystem zu installieren.

Die Platzierung der Datenbankdateien und Protokolldateien und der Typ des verwendeten Azure-Speichers sollten durch die Anforderungen an IOPS, Latenz und Durchsatz bestimmt werden. Damit ausreichende IOPS für das Transaktionsprotokoll verfügbar sind, müssen Sie möglicherweise mehrere VHDs für die Transaktionsprotokolldatei nutzen oder einen größeren Storage Premium-Datenträger verwenden. In solchen Fällen würden Sie einfach ein Software-RAID (z.B. Windows Storage Pool für Windows oder MDADM und LVM [Logical Volume Manager] für Linux) mit den VHDs erstellen, die das Transaktionsprotokoll enthalten.

___

> ![Windows][Logo_Windows] Windows
>
> Laufwerk „D:\\“ auf einem virtuellen Azure-Computer ist ein nicht permanentes Laufwerk, das durch einige lokale Datenträger im Azure-Serverknoten gesichert wird. Da es nicht permanent ist, bedeutet dies, dass alle Änderungen am Inhalt auf dem Laufwerk „D:\\“ verloren gehen, wenn der virtuelle Computer neu gestartet wird. Änderungen sind hierbei gespeicherte Dateien, erstellte Verzeichnisse, installierte Anwendungen usw.
>
> ![Linux][Logo_Linux] Linux
>
> Virtuelle Azure-Computer unter Linux stellen ein Laufwerk automatisch in „/mnt/resource“ bereit. Dies ist ein nicht permanentes Laufwerk, das durch lokale Datenträger im Azure-Serverknoten gesichert wird. Da es nicht permanent ist, bedeutet dies, dass alle Änderungen am Inhalt von „/mnt/resource“ verloren gehen, wenn der virtuelle Computer neu gestartet wird. Änderungen sind hierbei gespeicherte Dateien, erstellte Verzeichnisse, installierte Anwendungen usw.

___

Abhängig von der Azure VM-Serie weisen die lokalen Datenträger im Computeknoten unterschiedliche Leistungen auf, die wie folgt kategorisiert werden können:

* A0-A7: Sehr eingeschränkte Leistung. Nur für die Windows-Auslagerungsdatei geeignet.
* A8-A11: Sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz
* D-Serie: Sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz
* DS-Serie: Sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz
* G-Serie: Sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz
* GS-Serie: Sehr gute Leistungsmerkmale mit mehreren Zehntausend IOPS und > 1 GB/s Durchsatz

Die Angaben oben beziehen sich auf die für SAP zertifizierten VM-Typen. Die VM-Serien mit hervorragenden Werten für IOPS und Durchsatz können für einige DBMS-Funktionen genutzt werden, z.B. tempdb oder temporärer Tablespace.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Caching für VMs und VHDs
Wenn diese Datenträger/VHDs über das Portal erstellt oder hochgeladene VHDs auf virtuellen Computern bereitgestellt werden, kann ausgewählt werden, ob der E/A-Datenverkehr zwischen dem virtuellen Computer und diesen VHDs im Azure-Speicher zwischengespeichert wird. Azure Standard und Storage Premium verwenden zwei unterschiedliche Technologien für diese Art von Cache. In beiden Fällen wird der Cache selbst auf den gleichen Datenträgern gesichert, die vom temporären Datenträger des virtuellen Computers verwendet werden („D:\\“ unter Windows oder „/mnt/resource“ unter Linux).
 
Für Azure-Standardspeicher lauten die möglichen Cachetypen wie folgt:

* Kein Zwischenspeichern
* Zwischenspeichern von Lesevorgängen
* Zwischenspeichern von Lese- und Schreibvorgängen

Um eine konsistente und planbare Leistung zu erzielen, sollten Sie das Zwischenspeichern beim Azure-Standardspeicher für alle virtuellen Festplatten mit **DBMS-bezogenen Datendateien, Protokolldateien und Tablespaces auf „NONE“** festlegen. Für das Zwischenspeichern des virtuellen Computers kann die Standardeinstellung übernommen werden.

Für Azure Storage Premium stehen die folgenden Optionen zum Zwischenspeichern zur Verfügung:

* Kein Zwischenspeichern
* Zwischenspeichern von Lesevorgängen

Für Azure Storage Premium wird empfohlen, das **Zwischenspeichern von Lesevorgängen für Datendateien** in der SAP-Datenbank zu nutzen und **Kein Zwischenspeichern für die VHD(s) von Protokolldatei(en)** festzulegen.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Software-RAID
Wie bereits oben erwähnt, müssen Sie einen Kompromiss zwischen dem benötigten IOPS-Wert für die Datenbankdateien auf der Menge von virtuellen Festplatten, die Sie konfigurieren können, und der maximalen Anzahl von IOPS finden, die ein virtueller Azure-Computer pro VHD beim Storage Premium-Speichertyp bereitstellt. Die einfachste Möglichkeit zum Verwalten der IOPS-Auslastung auf verschiedenen VHDs ist das Erstellen eines Software-RAIDs mit den verschiedenen VHDs. Fügen Sie dann eine Anzahl von Datendateien des SAP-DBMS zu den LUNs im Software-RAID hinzu. Abhängig von den jeweiligen Anforderungen sollten Sie die Verwendung von Storage Premium erwägen, da zwei der drei verschiedenen Storage Premium-Datenträger ein höheres IOPS-Kontingent als VHDs mit Standardspeicher bereitstellen. Dazu kommt die erheblich bessere E/A-Latenz bei Azure Storage Premium.

Dasselbe gilt für das Transaktionsprotokoll der unterschiedlichen DBMS-Systeme. Bei vielen Systemen ist es nicht hilfreich, einfach mehr Tlog-Dateien hinzuzufügen, da die DBMS-Systeme jeweils nur in eine der Dateien schreiben. Wenn höhere IOPS-Raten benötigt werden, als durch eine einfache VHD mit Standardspeicher bereitgestellt werden kann, können Sie ein Stripeset über mehrere VHDs mit Standardspeicher erstellen, oder Sie verwenden einen größeren Storage Premium-Datenträgertyp, der neben höheren IOPS-Raten auch eine um Größenordnungen geringere Latenz für die E/A-Vorgänge im Transaktionsprotokoll bereitstellt.
 
In folgenden Situationen sollte bei Azure-Bereitstellungen ein Software-RAID verwendet werden:

* Das Transaktions-/Wiederholen-Protokoll erfordert mehr IOPS als Azure für eine einzelne VHD bietet. Wie bereits erwähnt, kann dieses Problem behoben werden, indem mit einem Software-RAID eine LUN über mehrere virtuelle Festplatten erstellt wird.
* Ungleichmäßige Verteilung der E/A-Workload über die verschiedenen Datendateien der SAP-Datenbank. In solchen Fällen kann es vorkommen, dass eine Datendatei das Kontingent recht häufig ausschöpft. Gleichzeitig nutzen andere Datendateien das IOPS-Kontingent einer einzelnen VHD nicht annähernd aus. In solchen Fällen besteht die einfachste Lösung darin, eine LUN über mehrere virtuelle Festplatten mit einem Software-RAID zu erstellen.
* Sie kennen die genaue E/A-Workload pro Datendatei nicht und kennen die IOPS-Gesamtworkload für das DBMS nur ungefähr. Die einfachste Lösung ist das Erstellen einer LUN mithilfe eines Software-RAIDs. Die Summe der Kontingente der verschiedenen VHDs hinter dieser LUN sollte dann die bekannte IOPS-Rate erfüllen.

___

> ![Windows][Logo_Windows] Windows
>
> Die Verwendung von Speicherplätzen in Windows Server 2012 oder höher ist vorzuziehen, da dies effizienter als Windows-Striping in früheren Windows-Versionen ist. Beachten Sie dabei, dass Sie möglicherweise die Windows-Speicherpools und Speicherplätze mit PowerShell-Befehlen erstellen müssen, wenn Sie Windows Server 2012 als Betriebssystem verwenden. Die PowerShell-Befehle finden Sie hier: <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Nur MDADM und LVM (Logical Volume Manager) werden unterstützt, um ein Software-RAID unter Linux zu erstellen. Weitere Informationen finden Sie in den folgenden Artikeln:
>
> * [Konfigurieren von Software-RAID unter Linux][virtual-machines-linux-configure-raid] \(für MDADM)
> * [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure][virtual-machines-linux-configure-lvm]


___

Für die Nutzung von VM-Serien, die mit Azure Storage Premium verwendet werden können, gelten normalerweise folgende Überlegungen:

* Anforderungen für E/A-Latenzen, die der von den SAN-/NAS-Geräten bereitgestellten Leistung möglichst entsprechen.
* Anforderungen nach einer um Größenordnungen besseren E/A-Latenz als vom Azure-Standardspeicher bereitgestellt werden kann.
* Mehr IOPS pro virtuellen Computer als mit mehreren Standardspeicher-VHDs für einen bestimmten virtuellen Computertyp erreicht werden könnten.

Da der zugrunde liegende Azure-Speicher jede VHD in mindestens drei Speicherknoten repliziert, kann einfaches RAID 0-Striping verwendet werden. Es ist nicht erforderlich, RAID5 oder RAID1 zu implementieren.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage
In Microsoft Azure Storage werden der virtuelle Basiscomputer (mit dem Betriebssystem) und VHDs oder BLOBs in mindestens 3 getrennten Speicherknoten gespeichert. Beim Erstellen eines Speicherkontos können verschiedene Schutzoptionen ausgewählt werden:

![Georeplikation für Azure-Speicherkonto aktiviert][dbms-guide-figure-100]

Lokale Azure-Speicherreplikation (lokal redundant) bietet einen Schutz vor Datenverlust durch Ausfall der Infrastruktur, dessen Bereitstellung wenige Kunden leisten könnten. Wie oben gezeigt, gibt es vier verschiedene Optionen und eine fünfte Option, die eine Variation der ersten drei Optionen darstellt. Bei einer genaueren Betrachtung kann wie folgt unterschieden werden:

* **Lokal redundanter Storage Premium-Speicher (LRS)**: Azure Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Es gibt 3 Replikate der Daten innerhalb des gleichen Azure-Rechenzentrums in einer Azure-Region. Die Kopien befinden sich in verschiedenen Fehler- und Upgradedomänen (die Konzepte werden in [diesem Kapitel][planning-guide-3.2] des [Planungshandbuchs][planning-guide] erläutert). Wenn ein Replikat der Daten aufgrund des Ausfalls eines Speicherknotens oder Datenträgers nicht mehr verwendet werden kann, wird automatisch ein neues Replikat generiert.
* **Lokal redundanter Speicher (LRS)**: Es gibt in diesem Fall 3 Replikate der Daten innerhalb des gleichen Azure-Rechenzentrums in einer Azure-Region. Die Kopien befinden sich in verschiedenen Fehler- und Upgradedomänen (die Konzepte werden in [diesem Kapitel][planning-guide-3.2] des [Planungshandbuchs][planning-guide] erläutert). Wenn ein Replikat der Daten aufgrund des Ausfalls eines Speicherknotens oder Datenträgers nicht mehr verwendet werden kann, wird automatisch ein neues Replikat generiert.
* **Georedundanter Speicher (GRS)**: In diesem Fall erfolgt eine asynchrone Replikation, die zusätzlich 3 Replikate der Daten in einer anderen Azure-Region speichert. Diese befindet sich in den meisten Fällen in der gleichen geografischen Region (z.B. „Europa, Norden“ und „Europa, Westen“). Dies führt zu drei zusätzlichen Replikaten, sodass insgesamt sechs Replikate vorhanden sind. Eine Variante hiervon ist eine Ergänzung, bei der die Daten in der georeplizierten Azure-Region für Lesezwecke verwendet werden können (georedundant mit Lesezugriff).
* **Zonenredundanter Speicher (ZRS)**: In diesem Fall bleiben die 3 Replikate der Daten in der gleichen Azure-Region. Wie in [diesem Kapitel][planning-guide-3.1] des [Planungshandbuchs][planning-guide] erläutert, kann es sich bei einer Azure-Region um eine Reihe von Rechenzentren in unmittelbarer Nähe zueinander handeln. Bei LRS werden die Replikate über die verschiedenen Rechenzentren verteilt, die eine Azure-Region darstellen.

Weitere Informationen finden Sie [hier][storage-redundancy].
 
> [AZURE.NOTE] Bei DBMS-Bereitstellungen wird die Verwendung von georedundantem Speicher nicht empfohlen.
>
> Die Georeplikation von Azure-Speicher ist asynchron. Die Replikation von einzelnen virtuellen Festplatten, die auf einem einzelnen virtuellen Computer bereitgestellt werden, wird nicht gleichzeitig synchronisiert. Es ist daher nicht möglich, DBMS-Dateien zu replizieren, die über verschiedene VHDs verteilt oder in einem Software-RAID bereitgestellt werden, das auf mehreren virtuellen Festplatten basiert. Für DBMS-Software muss der permanente Datenträgerspeicher in verschiedenen LUNs und zugrunde liegenden Datenträgern/VHDs/Spindeln exakt synchronisiert werden. DBMS-Software verwendet verschiedene Mechanismen zum Sequenzieren von E/A-Schreibvorgängen, und ein DBMS meldet bereits eine Beschädigung des Zieldatenträgerspeichers für die Replikation, wenn die Vorgänge um wenige Millisekunden abweichen. Wenn also eine Datenbankkonfiguration mit einer Datenbank auf mehreren VHDs georepliziert werden soll, muss eine solche Replikation mit Funktionen der Datenbank erfolgen. Sie sollten sich für diese Aufgabe nicht auf die Georeplikation von Azure-Speicher verlassen.
>
> Das Problem kann am einfachsten anhand eines Beispielsystems erläutert werden. Angenommen, Sie haben ein SAP-System in Azure hochgeladen, das 8 VHDs mit Datendateien des DBMS sowie eine VHD mit der Transaktionsprotokolldatei umfasst. Auf jede dieser neun VHDs werden Daten in konsistenter Weise gemäß dem DBMS geschrieben, unabhängig davon, ob sie in die Daten- oder Transaktionsprotokolldateien geschrieben werden.
>
> Damit die Daten ordnungsgemäß georepliziert werden und ein konsistentes Datenbankimage beibehalten wird, muss der Inhalt aller neun VHDs in genau der Reihenfolge georepliziert werden, in der die E/A-Vorgänge auf den neun unterschiedlichen VHDs ausgeführt wurden. Allerdings können bei der Georeplikation von Azure-Speicher keine Abhängigkeiten zwischen VHDs deklariert werden. Dies bedeutet, dass bei der Georeplikation von Microsoft Azure Storage nicht bekannt ist, dass die Inhalte dieser neun verschiedenen VHDs zusammenhängen und dass die Datenänderungen nur dann konsistent sind, wenn sie in der Reihenfolge repliziert werden, in der die E/A-Vorgänge auf den neun VHDs ausgeführt wurden.
>
> Neben dem hohen Risiko, dass die georeplizierten Images in diesem Szenario kein konsistentes Datenbankimage bieten, tritt auch eine Leistungsbeeinträchtigung beim georedundanten Speicher auf, die die Leistung erheblich senken kann. Zusammengefasst bedeutet dies, dass Sie diese Art der Speicherredundanz für DBMS-Workloads nicht verwenden sollten.
 
#### Zuordnen von VHDs in Dienstspeicherkonten von virtuellen Azure-Computern
Ein Azure-Speicherkonto ist nicht nur ein Konzept für Verwaltungsaufgaben, sondern stellt auch eine Gruppe von Einschränkungen dar. Diese Einschränkungen unterscheiden sich abhängig davon, ob sie sich auf ein Konto mit Azure-Standardspeicher oder Azure Storage Premium beziehen. Die genauen Funktionen und Einschränkungen werden [hier][storage-scalability-targets] aufgeführt.
 
Es ist daher für Azure-Standardspeicher wichtig festzustellen, dass die IOPS pro Speicherkonto beschränkt sind (siehe Zeile „Gesamtanfragerate “ in [diesem Artikel][storage-scalability-targets]). Es gibt außerdem eine anfängliche Einschränkung von maximal 100 Speicherkonten pro Azure-Abonnement (Stand Juli 2015). Aus diesem Grund wird empfohlen, bei Verwendung von Azure-Standardspeicher die IOPS von virtuellen Computern zwischen mehreren Speicherkonten aufzuteilen. Für einen einzelnen virtuellen Computer wird im Idealfall ein Speicherkonto verwendet. In DBMS-Bereitstellungen, in denen jede virtuelle Festplatte, die in Azure-Standardspeicher gehostet wird, die Kontingentgrenze erreichen kann, sollten Sie nur 30 bis 40 VHDs pro Azure-Speicherkonto bereitstellen, das Azure-Standardspeicher verwendet. Wenn Sie jedoch Azure Storage Premium nutzen und große Datenbankmengen gespeichert werden sollen, sollte es im Hinblick auf IOPS keine Probleme geben. Für ein Azure Storage Premium-Speicherkonto gelten jedoch mehr Einschränkungen für die Datenmenge als für Konten mit Azure-Standardspeicher. Daher können Sie bei einem Konto mit Azure Storage Premium nur eine begrenzte Anzahl von VHDs bereitstellen, bevor die Grenze für das Datenvolumen erreicht wird. Betrachten Sie als Fazit ein Azure-Speicherkonto als „virtuelles SAN“ mit eingeschränkten Funktionen für IOPS und/oder Kapazität. Die Aufgabe besteht daher, wie bei lokalen Bereitstellungen, darin, das Layout der VHDs der unterschiedlichen SAP-Systeme auf den verschiedenen „imaginären SAN-Geräten“ oder Azure-Speicherkonten zu definieren.
 
Für Azure-Standardspeicher wird empfohlen, möglichst keinen Speicher aus verschiedenen Speicherkonten auf einem virtuellen Computer zu präsentieren.

Mit der DS- oder GS-Serie von virtuellen Azure-Computern ist es jedoch möglich, VHDs aus Azure-Standardspeicherkonten und Storage Premium-Konten bereitzustellen. In Anwendungsfällen wie dem Schreiben von Sicherungen auf VHDs mit Standardspeicher, wobei DBMS-Daten- und Protokolldateien in Storage Premium gespeichert sind, könnte eine solche heterogene Speicherung genutzt werden.

Basierend auf Kundenbereitstellungen und Tests können etwa 30 bis 40 VHDs mit Datenbankdateien und Protokolldateien in einem einzelnen Azure-Standardspeicherkonto bereitgestellt und dabei eine akzeptable Leistung erzielt werden. Wie bereits erwähnt, ergeben sich die Einschränkungen bei Konten mit Azure Storage Premium mit hoher Wahrscheinlichkeit aus der Datenkapazität und nicht aus IOPS.

Wie bei lokalen SAN-Geräten ist für die Freigabe eine Überwachung erforderlich, um Engpässe in einem Azure-Speicherkonto zu erkennen. Mit der Azure-Überwachungserweiterung für SAP und dem Azure-Portal können Azure-Speicherkonten mit hoher Auslastung ermittelt werden, die eine suboptimale E/A-Leistung aufweisen können. Wenn diese Situation erkannt wird, sollten Sie stark ausgelastete virtuelle Computer in ein anderes Azure-Speicherkonto verschieben. Im [Bereitstellungshandbuch][deployment-guide] finden Sie weitere Informationen zum Aktivieren der Funktionen für die SAP-Hostüberwachung.

Einen anderen Artikel mit einer Zusammenfassung bewährter Methoden zu Azure-Standardspeicher und Azure-Standardspeicherkonten finden Sie hier: <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### Verschieben von bereitgestellten virtuellen DBMS-Computern aus dem Azure-Standardspeicher in Azure Storage Premium
Es gibt einige Szenarien, in denen Sie als Kunde einen bereitgestellten virtuellen Computer aus Azure-Standardspeicher in Azure Storage Premium verschieben möchten. Dies ist nur möglich, wenn die Daten physisch verschoben werden. Es gibt mehrere Möglichkeiten, um dieses Ziel zu erreichen.

* Sie können einfach alle virtuellen Festplatten, die Basis-VHD sowie die Daten-VHDs in ein neues Konto mit Azure Storage Premium kopieren. Sehr häufig wird die Anzahl von VHDs in Azure-Standardspeicher nicht aufgrund der Tatsache ausgewählt, dass das Datenvolumen benötigt wird. Die Anzahl der virtuellen Festplatten war vielmehr für den gewünschten IOPS-Wert erforderlich. Bei einem Wechsel zu Azure Storage Premium benötigen Sie deutlich weniger VHDs, um den gleichen IOPS-Durchsatz zu erzielen. Angesichts der Tatsache, dass Sie beim Azure-Standardspeicher für die Datennutzung und nicht für die nominale Datenträgergröße bezahlen, ist die Anzahl der virtuellen Festplatten im Hinblick auf die Kosten nicht ausschlaggebend. Bei Azure Storage Premium bemessen sich die Kosten allerdings nach der nominalen Datenträgergröße. Die meisten Kunden versuchen daher, die Anzahl der Azure-VHDs in Storage Premium auf die Anzahl zu beschränken, die für den erforderlichen IOPS-Durchsatz benötigt wird. Daher entscheiden sie sich in den meisten Fällen dagegen, einfach 1:1 zu kopieren.
* Wenn noch keine Bereitstellung erfolgt ist, stellen Sie eine einzelne VHD mit einer Datenbanksicherung Ihrer SAP-Datenbank bereit. Nach der Sicherung heben Sie die Bereitstellung aller virtuellen Festplatten auf, einschließlich der virtuellen Festplatte mit der Sicherung, und kopieren die Basis-VHD und die VHD mit der Sicherung in ein Azure Storage Premium-Konto. Dann stellen Sie den virtuellen Computer auf Grundlage der Basis-VHD und die virtuelle Festplatte mit der Sicherung bereit. Jetzt erstellen Sie zusätzliche leere Storage Premium-Datenträger für den virtuellen Computer, die zum Wiederherstellen der Datenbank verwendet werden. Dabei wird davon ausgegangen, dass das DBMS das Ändern von Pfaden für Daten- und Protokolldateien im Rahmen der Wiederherstellung zulässt.
* Eine andere Möglichkeit ist eine Variante des ersten Verfahrens, wobei Sie nur die Sicherungs-VHD in Azure Storage Premium kopieren und dann an einen virtuellen Computer anfügen, den Sie neu bereitgestellt und installiert haben.
* Die vierte Möglichkeit wählen Sie, wenn Sie die Anzahl der Datendateien in der Datenbank ändern müssen. In diesem Fall führen Sie eine homogene SAP-Systemkopie mit Export/Import aus. Speichern Sie diese Exportdateien auf einer VHD, die in ein Azure Storage Premium-Konto kopiert wird, und fügen Sie sie an einen virtuellen Computer an, mit dem Sie die Importvorgänge ausführen. Kunden nutzen diese Möglichkeit vor allem dann, wenn die Anzahl der Datendateien verringert werden soll.

### Bereitstellung von virtuellen Computern für SAP in Azure
Microsoft Azure bietet mehrere Methoden zum Bereitstellen virtueller Computer und zugehöriger Datenträger. Daher ist es wichtig, die Unterschiede zu kennen, da die Vorbereitung der virtuellen Computer abhängig von der Bereitstellungsart ist. Allgemein werden die in den folgenden Kapiteln beschriebenen Szenarien betrachtet.

#### Bereitstellen eines virtuellen Computers aus dem Azure Marketplace
Sie möchten ein Image aus dem Azure Marketplace für die Bereitstellung des virtuellen Computers verwenden, das von Microsoft oder einem Drittanbieter stammt. Nachdem Sie Ihren virtuellen Computer in Azure bereitgestellt haben, verwenden Sie dieselben Richtlinien und Tools zum Installieren der SAP-Software auf dem virtuellen Computer wie in einer lokalen Umgebung. Für die Installation der SAP-Software auf dem virtuellen Azure-Computer empfehlen SAP und Microsoft das Hochladen und Speichern der SAP-Installationsmedien in Azure-VHDs oder das Erstellen eines virtuellen Azure-Computers, der als „Dateiserver“ fungiert und die erforderlichen SAP-Installationsmedien enthält.

#### Bereitstellen eines virtuellen Computers mit einem kundenspezifischen generalisierten Image
Aufgrund spezifischer Patch-Erfordernisse im Hinblick auf die Betriebssystem- oder DBMS-Version sind die im Azure Marketplace erhältlichen Images möglicherweise nicht für Ihren Bedarf geeignet. Daher muss möglicherweise anhand eines eigenen, benutzerdefinierten OS-/DBMS-Images ein virtueller Computer erstellt werden, der anschließend mehrmals bereitgestellt werden kann. Um ein solches „privates“ Image für die Duplizierung vorzubereiten, muss das Betriebssystem auf dem lokalen virtuellen Computer generalisiert werden. Im [Bereitstellungshandbuch][deployment-guide] finden Sie ausführliche Informationen zum Generalisieren eines virtuellen Computers.

Wenn Sie auf Ihrer lokalen VM bereits SAP-Inhalte installiert haben (insbesondere bei zweischichtigen Systemen), können Sie die SAP-Systemeinstellungen nach der Bereitstellung der Azure-VM über das vom SAP Software Provisioning Manager unterstützte Verfahren zum Umbenennen von Instanzen anpassen (SAP-Hinweis [1619720]). Andernfalls können Sie die SAP-Software auch später, nach der Bereitstellung des virtuellen Azure-Computers installieren.
 
Bezüglich des von der SAP-Anwendung verwendeten Datenbankinhalts können Sie den Inhalt entweder durch eine SAP-Installation neu generieren, oder Sie können die Inhalte in Azure importieren, indem Sie eine VHD mit einer DBMS-Datenbanksicherung oder Funktionen des DBMS verwenden, um eine Sicherung direkt in Microsoft Azure Storage durchzuführen. In diesem Fall könnten Sie auch VHDs mit den DBMS Daten- und Protokolldateien lokal vorbereiten und diese dann als Datenträger in Azure importieren. Die Übertragung von DBMS-Daten, die aus einer lokalen Bereitstellung in Azure geladen werden, müsste jedoch über VHDs erfolgen, die lokal vorbereitet werden müssen.

#### Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure
Sie planen, ein bestimmtes lokales SAP-System in Azure zu verschieben („Lift and Shift“). Dies kann erfolgen, indem Sie die VHD mit dem Betriebssystem, SAP- und DBMS-Binärdateien sowie die VHDs mit den Daten- und Protokolldateien des DBMS in Azure hochladen. Im Gegensatz zu Szenario 2 oben werden Hostname, SAP SID und SAP-Benutzerkonten auf dem virtuellen Azure Computer entsprechend der Konfiguration in der lokalen Umgebung beibehalten. Daher ist eine Generalisierung des Images nicht erforderlich. Dieser Anwendungsfall gilt hauptsächlich für standortübergreifende Szenarien, bei denen ein Teil der SAP-Landschaft lokal und Teile in Azure ausgeführt werden.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern
Azure bietet die folgenden Funktionen für hohe Verfügbarkeit (High Availability, HA) und Notfallwiederherstellung (Disaster Recovery, DR), die sich auf verschiedene, in SAP- und DBMS-Bereitstellungen verwendete Komponenten beziehen.

### In Azure-Knoten bereitgestellte virtuelle Computer
Der Azure-Plattform bietet keine Features wie die Livemigration für bereitgestellte virtuelle Computer. Dies bedeutet, dabei bei Wartungen an einem Servercluster, auf dem ein virtueller Computer bereitgestellt wird, der virtuelle Computer beendet und neu gestartet werden muss. Die Wartung in Azure erfolgt über sogenannte Upgradedomänen innerhalb von Serverclustern. Es wird jeweils nur eine Upgradedomäne gewartet. Bei einem solchen Neustart tritt eine Betriebsunterbrechung auf, während der virtuelle Computer heruntergefahren, die Wartung durchgeführt und der virtuelle Computer neu gestartet wird. Die meisten DBMS-Hersteller bieten jedoch Funktionen für Hochverfügbarkeit und Notfallwiederherstellung, mit denen die DBMS-Dienste schnell auf einem anderen Knoten neu gestartet werden, wenn der primäre Knoten nicht verfügbar ist. Die Azure-Plattform bietet Funktionen zum Verteilen von virtuellen Computern, Speicher und anderen Azure-Diensten auf Upgradedomänen, um sicherzustellen, dass geplante Wartungsarbeiten oder Infrastrukturausfälle nur eine kleine Teilmenge von virtuellen Computern oder Diensten betreffen. Bei sorgfältiger Planung ist es möglich, Verfügbarkeitsniveaus zu erreichen, die mit lokalen Infrastrukturen vergleichbar sind.

Bei Microsoft Azure-Verfügbarkeitsgruppen handelt es sich um eine logische Gruppierung von virtuellen Computern oder Diensten, mit der sichergestellt wird, dass virtuelle Computer und andere Dienste auf verschiedene Fehler- und Upgradedomänen innerhalb eines Clusters verteilt werden, damit zu jedem Zeitpunkt nur ein Knoten heruntergefahren wird (in [diesem Artikel][virtual-machines-manage-availability] finden Sie weitere Informationen).

Die Konfiguration muss beim Einführen von virtuellen Computern entsprechend dem Zweck erfolgen, wie im Folgenden gezeigt:

![Definition einer Verfügbarkeitsgruppe für DBMS-HA-Konfigurationen][dbms-guide-figure-200]

Wenn hochverfügbare Konfigurationen von DBMS-Bereitstellungen erstellt werden sollen (unabhängig von den einzelnen benötigten DBMS-HA-Funktionen), müssen die virtuellen DBMS-Computer Folgendes erfüllen:

* Hinzufügen der virtuellen Computer zu dem gleichen virtuellen Azure-Netzwerk (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* Die virtuellen Computer der HA-Konfiguration sollten sich auch im gleichen Subnetz befinden. Die Namensauflösung zwischen den verschiedenen Subnetzen ist in Nur-Cloud-Bereitstellungen nicht möglich, nur die IP-Auflösung. Bei der Verwendung von Site-to-Site- oder ExpressRoute-Verbindungen für standortübergreifende Bereitstellungen ist ein Netzwerk mit mindestens einem Subnetz bereits eingerichtet. Die Auflösung erfolgt gemäß den lokalen AD-Richtlinien und der Netzwerkinfrastruktur. 
[Kommentar]: <> (MSSedusch TODO Prüfen, ob weiterhin für ARM zutreffend)

#### IP-Adressen
Es wird dringend empfohlen, die virtuellen Computer für HA-Konfigurationen robust einzurichten. Die Adressierung der HA-Partner mit IP-Adressen innerhalb der HA-Konfiguration ist in Azure nur dann zuverlässig, wenn statische IP-Adressen verwendet werden. Es gibt zwei Konzepte von „Herunterfahren“ in Azure:

* Das Herunterfahren über das Azure-Portal oder das Azure PowerShell-Cmdlet „Stop-AzureRmVM“: In diesem Fall wird der virtuelle Computer heruntergefahren, und die Zuordnung wird aufgehoben. Für Ihr Azure-Konto wird dieser virtuelle Computer nicht mehr in Rechnung gestellt, es fallen daher lediglich Kosten für den verwendeten Speicher an. Wenn jedoch die private IP-Adresse der Netzwerkschnittstelle nicht statisch war, wird die IP-Adresse freigegeben, und es ist nicht sichergestellt, dass der Netzwerkschnittstelle nach einem Neustart des virtuellen Computers wieder die alte IP-Adresse zugewiesen wird . Das Herunterfahren über das Azure-Portal oder durch Aufrufen von „Stop-AzureRmVM“ führt automatisch zur Aufhebung der Zuordnung. Wenn die Zuordnung für den Computer nicht aufgehoben werden soll, verwenden Sie „Stop-AzureRmVM -StayProvisioned“.
* Wenn Sie den virtuellen Computer auf Betriebssystemebene herunterfahren, wird er heruntergefahren und die Zuordnung wird NICHT aufgehoben. Allerdings wird der virtuelle Computer weiter für Ihr Azure-Konto in Rechnung gestellt, auch wenn er heruntergefahren wurde. In einem solchen Fall bleibt die Zuweisung der IP-Adresse zu einem angehaltenen virtuellen Computer unverändert. Das interne Herunterfahren des virtuellen Computers erzwingt nicht automatisch die Aufhebung der Zuordnung.

Auch bei standortübergreifenden Szenarien bedeuten das Herunterfahren und Aufheben der Zuordnung standardmäßig, dass die Zuweisung von IP-Adressen auf dem virtuellen Computer aufgehoben wird, auch wenn dies von den lokalen Richtlinien in den DHCP-Einstellungen abweicht.

* Eine Ausnahme stellt die Situation dar, wenn eine statische IP-Adresse einer Netzwerkschnittstelle zugewiesen wird, wie [hier][virtual-networks-reserved-private-ip] beschrieben.
* In diesem Fall bleibt die IP-Adresse konstant, solange die Netzwerkschnittstelle nicht gelöscht wird.

> [AZURE.IMPORTANT] Damit die gesamte Bereitstellung übersichtlich und leicht zu verwalten bleibt, sollten Sie die virtuellen Partnercomputer in einer DBMS-HA- oder DR-Konfiguration in Azure auf jeden Fall so einrichten, dass eine funktionierende Namensauflösung zwischen den verschiedenen beteiligten virtuellen Computern besteht.
 
## Bereitstellen der Hostüberwachung
Für die produktive Verwendung von SAP-Anwendungen auf virtuellen Azure-Computern benötigt SAP die Möglichkeit, Hostüberwachungsdaten von den physischen Hosts abzurufen, auf denen die virtuellen Azure-Computer ausgeführt werden. Es wird eine bestimmte SAP-HostAgent-Patchebene benötigt, die diese Funktion in SAPOSCOL und SAP HostAgent ermöglicht. Die Patchebenen sind in SAP-Hinweis [1409604] genau dokumentiert.

Weitere Informationen zur Bereitstellung von Komponenten, die Hostdaten für SAPOSCOL und SAPHostAgent bereitstellen, sowie zur Verwaltung des Lebenszyklus dieser Komponenten finden Sie im [Bereitstellungshandbuch][deployment-guide].

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Besonderheiten bei Microsoft SQL Server

### SQL Server-IaaS
Ab Microsoft Azure lassen sich bestehende SQL Server-Anwendungen auf Grundlage der Windows Server-Plattform einfach zu Azure-VMs migrieren. Wenn Sie SQL Server auf einem virtuellen Computer verwenden, können Sie die Gesamtbetriebskosten der Bereitstellung, Verwaltung und Wartung von unternehmensweiten Anwendungen senken, indem Sie diese Anwendungen einfach zu Microsoft Azure migrieren. Mit SQL Server auf einem virtuellen Azure-Computer stehen Administratoren und Entwicklern die gleichen Entwicklungs- und Verwaltungstools wie in der lokalen Umgebung zur Verfügung.

> [AZURE.IMPORTANT] Hier wird nicht auf Microsoft Azure SQL-Datenbank eingegangen. Dabei handelt es sich um ein PaaS-Angebot (Plattform as a Service) der Microsoft Azure Platform. In diesem Dokument geht es um das Ausführen des bekannten SQL Server-Produkts für lokale Bereitstellungen auf virtuellen Azure-Computern, wobei die IaaS-Funktion (Infrastructure as a Service) von Azure genutzt wird. Die Datenbankfunktionen und -Features für diese beiden Angebote unterscheiden sich und dürfen nicht verwechselt werden. Weitere Informationen: <https://azure.microsoft.com/services/sql-database/>
 
Sie sollten vor dem Fortfahren [diese Dokumentation][virtual-machines-sql-server-infrastructure-services] lesen.

In den folgenden Abschnitten werden Inhalte aus dieser Dokumentation zusammengefasst bzw. Bezug darauf genommen. Außerdem werden Besonderheiten in Bezug auf SAP erwähnt, und einige Konzepte werden ausführlicher beschrieben. Es wird jedoch dringend empfohlen, zuerst die oben genannte Dokumentation durchzuarbeiten, bevor Sie die SQL Server-spezifische Dokumentation lesen.

Einige spezifische Informationen für SQL Server in IaaS sollten Sie vor dem Fortfahren kennen:

* **SLA für virtuelle Computer**: Es gibt eine SLA für in Azure ausgeführte virtuelle Computer. Diese finden Sie unter folgendem Link: <https://azure.microsoft.com/support/legal/sla/>
* **SQL-Versionsunterstützung**: Für SAP-Kunden wird SQL Server 2008 R2 und höher auf virtuellen Microsoft Azure-Computern unterstützt. Frühere Versionen werden nicht unterstützt. Ausführliche Informationen finden Sie in der allgemeinen [Supporterklärung](https://support.microsoft.com/kb/956893). Beachten Sie, dass im Allgemeinen auch SQL Server 2008 von Microsoft unterstützt wird. Aufgrund spezieller Funktionen für SAP, die in SQL Server 2008 R2 eingeführt wurden, ist die Mindestversion für SAP jedoch SQL Server 2008 R2. Bedenken Sie, dass SQL Server 2012 und 2014 mit einer stärkeren Integration in das IaaS-Szenario erweitert wurden (z.B. direktes Sichern in Azure Storage). Daher beschränkt sich dieses Dokument auf SQL Server 2012 und 2014, d.h. die aktuelle Patchebene für Azure.
* **Unterstützung für SQL-Features**: Die meisten SQL Server-Features werden von Microsoft Azure Virtual Machines unterstützt. Dabei gelten jedoch einige Ausnahmen. **SQL Server-Failovercluster mit freigegebenen Datenträgern werden nicht unterstützt**. Verteilte Technologien wie Datenbankspiegelung, AlwaysOn-Verfügbarkeitsgruppen, Replikation, Protokollversand und Service Broker werden einer einzelnen Azure-Region unterstützt. SQL Server Always On wird auch zwischen verschiedenen Azure-Regionen unterstützt, wie hier beschrieben: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>. Ausführliche Informationen finden Sie in der [Supporterklärung](https://support.microsoft.com/kb/956893). Ein Beispiel für die Bereitstellung einer Always On-Konfiguration finden Sie in [diesem Artikel][virtual-machines-workload-template-sql-alwayson]. Darüber hinaus finden Sie [hier][virtual-machines-sql-server-infrastructure-services] bewährte Methoden.
* **SQL-Leistung**: Wir sind davon überzeugt, dass unter Microsoft Azure gehostete virtuelle Computer im Vergleich zu anderen Virtualisierungsangeboten in der öffentlichen Cloud sehr gute Leistung erbringen. Die einzelnen Ergebnisse können allerdings variieren. Dieser [Artikel][virtual-machines-sql-server-performance-best-practices] enthält hierzu Informationen.
* **Verwenden von Images aus Azure Marketplace**: Die schnellste Möglichkeit zum Bereitstellen eines neuen virtuellen Microsoft Azure-Computers besteht darin, ein Image aus dem Azure Marketplace zu verwenden. Einige Images in Azure Marketplace beinhalten SQL Server. Die Images, bei denen SQL Server bereits installiert ist, können nicht sofort für SAP NetWeaver-Anwendungen verwendet werden. Dies liegt daran, dass innerhalb dieser Images die standardmäßige SQL Server-Sortierung und nicht die für SAP NetWeaver-Systeme erforderliche Sortierung installiert ist. Um solche Images zu verwenden, lesen Sie sich die Schritte im Kapitel [Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace][dbms-guide-5.6] durch.
* Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/). Das [SQL Server 2012-Lizenzierungshandbuch](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) und das [SQL Server 2014-Lizenzierungshandbuch](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) sind ebenfalls wichtige Ressourcen.
 
### SQL Server-Konfigurationsrichtlinien für SAP-bezogene SQL Server-Installationen auf Azure-VMs

#### Empfehlungen für die VM-/VHD-Struktur für SAP-bezogene SQL Server-Bereitstellungen
In Übereinstimmung mit der allgemeinen Beschreibung sollten ausführbare SQL Server-Dateien sich im Systemverzeichnis der Basis-VHD (Laufwerk „C:“) des virtuellen Computers befinden bzw. bei der Installation dort abgelegt werden. Üblicherweise werden die meisten SQL Server-Systemdatenbanken durch die SAP NetWeaver-Workload nicht stark genutzt. Daher können die Systemdatenbanken von SQL Server („master“, „msdb“ und „model“) auf dem Laufwerk „C:\\“ verbleiben. Als Ausnahme kommt tempdb in Betracht, die im Falle einiger SAP ERP- sowie aller BW-Workloads entweder ein höheres Volumen für Daten oder für E/A-Vorgänge erfordern kann, wodurch die Kapazität der ursprünglichen VM übertroffen wird. Trifft dies auf Ihr System zu, sollten Sie die folgenden Schritte ausführen:

* Verschieben Sie die primären tempdb-Datendateien auf das logische Laufwerk, auf dem sich die primären Datendateien der SAP-Datenbank befinden.
* Fügen Sie alle zusätzlichen tempdb-Datendateien den jeweiligen logischen Laufwerken hinzu, die eine Datendatei der SAP-Benutzerdatenbank enthalten.
* Fügen Sie die tempdb-Protokolldatei dem logischen Laufwerk hinzu, das die Protokolldatei der Benutzerdatenbank enthält.
* **Ausschließlich bei VM-Typen mit Verwendung lokaler SSDs** auf Serverknoten können tempdb-Datendateien und -Protokolldateien auf dem Laufwerk „D:\\“ platziert werden. Dennoch kann es sinnvoll sein, mehrere tempdb-Datendateien zu verwenden. Beachten Sie, dass das Laufwerkvolumen bei „D:\\“ je nach VM-Typ variieren kann.
 
Mithilfe dieser Konfigurationen kann tempdb mehr Speicherplatz nutzen, als auf dem Systemlaufwerk vorhanden ist. Um die richtige tempdb-Größe zu bestimmen, können Sie die tempdb-Größen auf vorhandenen Systemen überprüfen, die lokal ausgeführt werden. Darüber hinaus ermöglicht eine solche Konfiguration IOPS-Werte für tempdb, die nicht durch das Systemlaufwerk zur Verfügung gestellt werden können. Auch hier können lokal ausgeführte Systeme zur Überwachung des E/A-Workloads für tempdb verwendet werden, sodass Sie die IOPS-Werte ableiten können, die Sie für tempdb erwarten.

Eine VM-Konfiguration, die SQL Server mit einer SAP-Datenbank ausführt und bei der die tempdb-Daten und die tempdb-Protokolldatei auf dem Laufwerk „D:\\“ platziert sind, würde wie folgt aussehen:
 
![Referenzkonfiguration der Azure-IaaS-VM für SAP][dbms-guide-figure-300]

Beachten Sie, dass die Größe des Laufwerk „D:\\“ je nach VM-Typ variieren kann. Abhängig von der erforderlichen Größe von tempdb sind Sie unter Umständen gezwungen, tempdb-Daten und -Protokolldateien mit den Daten und Protokolldateien der SAP-Datenbank zu koppeln, falls das Laufwerk „D:\\“ nicht ausreicht.

#### Formatieren der VHDs
Für SQL Server sollte die NTFS-Blockgröße für VHDs, die SQL Server-Daten und -Protokolldateien enthalten, 64 KB betragen. Es ist nicht erforderlich, Laufwerk „D:\\“ zu formatieren. Dieses Laufwerk ist bereits vorformatiert.

Um sicherzustellen, dass die Wiederherstellung oder Erstellung von Datenbanken die Datendateien nicht initialisiert, indem der Inhalt der Dateien gelöscht wird, sollten Sie sicherstellen, dass der Benutzerkontext, in dem der SQL Server-Dienst ausgeführt wird, über eine bestimmte Berechtigung verfügt. Normalerweise verfügen die Benutzer in der Windows-Administratorengruppe über diese Berechtigungen. Wenn der SQL Server-Dienst im Benutzerkontext eines Benutzers ausgeführt wird, der kein Windows-Administrator ist, müssen Sie diesem Benutzer das Benutzerrecht „Durchführen von Volumewartungsaufgaben“ zuweisen. Ausführliche Informationen finden Sie im Microsoft Knowledge Base-Artikel: <https://support.microsoft.com/kb/2574695>
 
#### Auswirkungen der Datenbankkomprimierung
Bei Konfigurationen, bei denen die E/A-Bandbreite zum begrenzenden Faktor werden könnte, sollte jede Möglichkeit ergriffen werden, die IOPS zu senken. Dadurch kann die ausführbare Workload in einem IaaS-Szenario wie Azure besser verteilt werden. Falls noch nicht erfolgt, wird sowohl von SAP als auch von Microsoft dringend empfohlen, vor dem Hochladen vorhandener SAP-Datenbanken in Azure die SQL Server-Seitenkomprimierung anzuwenden.
 
Der Empfehlung zur Durchführung der Datenbankkomprimierung vor dem Hochladen in Azure liegen zwei Gründe zugrunde:

* Die hochzuladende Datenmenge ist geringer.
* Die Ausführungsdauer der Komprimierung ist geringer (unter der Annahme, dass lokal leistungsfähigere Hardware mit mehr CPUs, einer höheren E/A-Bandbreite oder einer geringeren E/A-Latenz zur Verfügung steht).
* Durch kleinere Datenbanken werden u. U. die Kosten für die Datenträgerzuordnung gesenkt.

Die Datenbankkomprimierung funktioniert auf virtuellen Azure-Computern genauso wie in der lokalen Umgebung. Weitere Informationen zum Komprimieren einer vorhandenen SAP SQL Server-Datenbank finden Sie unter: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### SQL Server 2014 – Direktes Speichern von Datenbankdateien im Azure-Blobspeicher
SQL Server 2014 bietet die Möglichkeit, Datenbankdateien direkt im Azure-Blobspeicher ohne den umschließenden „Wrapper“ einer virtuellen Festplatte zu speichern. Insbesondere bei der Verwendung von standardmäßigem Azure-Speicher oder kleinerer VM-Typen werden hierdurch Szenarien möglich, in denen Sie die IOPS-Einschränkungen überwinden können, die durch eine begrenzte Anzahl von VHDs erzwungen werden können, die auf einigen kleineren VM-Typen bereitgestellt werden können. Dies funktioniert für Benutzerdatenbanken, jedoch nicht für die Systemdatenbanken von SQL Server. Es funktioniert auch für Daten- und Protokolldateien von SQL Server. Wenn Sie eine SAP SQL Server-Datenbank auf diese Weise bereitstellen möchten, statt sie in virtuelle Festplatten „einzuschließen“, bedenken Sie Folgendes:

* Das verwendete Speicherkonto muss sich in derselben Azure-Region befinden wie das Speicherkonto, das zur Bereitstellung des virtuellen Computers verwendet wird, auf dem SQL Server ausgeführt wird.
* Die bereits genannten Überlegungen im Hinblick auf die Verteilung virtueller Festplatten auf verschiedene Azure-Speicherkonten gelten auch für diese Bereitstellungsmethode. Dies bedeutet, dass die E/A-Vorgänge für die Grenzwerte des Azure-Speicherkontos eingerechnet werden.
[comment]: <> (MSSedusch TODO Dabei wird aber nur die Netzwerkbandbreite und keine Speicherbandbreite genutzt, richtig?)

Ausführliche Informationen zu dieser Art der Bereitstellung finden Sie hier: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Um SQL Server-Datendateien direkt auf Azure Storage Premium zu speichern, müssen Sie über eine minimale SQL Server 2014-Patchversion verfügen, die hier dokumentiert ist: <https://support.microsoft.com/kb/3063054> Das Speichern von SQL Server-Datendateien im Azure-Standardspeicher funktioniert mit der veröffentlichten Version von SQL Server 2014. Diese Patches enthalten jedoch weitere Fixes, durch die die direkte Verwendung von Azure-Blobspeicher für SQL Server-Datendateien und Sicherungen zuverlässiger wird. Wir empfehlen daher, diese Patches im Allgemeinen zu verwenden.

### SQL Server 2014-Pufferpoolerweiterung
In SQL Server 2014 wurde ein neues Feature eingeführt, die sogenannte Pufferpoolerweiterung. Diese Funktion erweitert den Pufferpool von SQL Server, der im Arbeitsspeicher mit einem Cache zweiter Ebene gespeichert wird, der von lokalen SSDs eines Servers oder des virtuellen Computers unterstützt wird. Auf diese Weise kann ein umfangreicherer Arbeitssatz der Daten „im Arbeitsspeicher“ beibehalten werden. Im Vergleich zum Zugriff auf den Azure-Standardspeicher ist der Zugriff auf die Erweiterung des Pufferpools, der auf lokalen SSDs eines virtuellen Azure-Computers gespeichert wird, um viele Faktoren schneller. Daher kann es sinnvoll sein, das lokale Laufwerk „D:\\“ der VM-Typen mit hervorragenden IOPS und Durchsatz zu verwenden, um die IOPS-Auslastung für den Azure-Speicher zu verringern und die Antwortzeiten von Abfragen deutlich zu verbessern. Dies trifft insbesondere dann zu, wenn nicht Storage Premium verwendet wird. Falls Storage Premium und der Premium-Lesecache von Azure auf dem Computeknoten verwendet werden (wie für Datendateien empfohlen), sind keine großen Unterschiede zu erwarten. Der Grund dafür ist, dass beide Caches (SQL Server-Pufferpoolerweiterung und Lesecache für Storage Premium) die lokalen Datenträger der Computeknoten verwenden. Weitere Informationen zu dieser Funktion finden Sie in der folgenden Dokumentation: <https://msdn.microsoft.com/library/dn133176.aspx>

### Überlegungen zur Sicherung und Wiederherstellung für SQL Server
Bei der Bereitstellung von SQL Server in Azure ist es erforderlich, die Sicherungsmethode zu überprüfen. Auch wenn es sich nicht um ein Produktivsystem handelt, muss die durch SQL Server gehostete SAP-Datenbank regelmäßig gesichert werden. Da Azure Storage drei Images vorhält, ist die Sicherung, um einen etwaigen Speicherabsturz kompensieren zu können, weniger dringlich. Der wichtigste Grund, einen ordnungsgemäßen Sicherungs- und Wiederherstellungsplan zu verfolgen, ist vielmehr, dass Sie mithilfe der Point-in-Time-Wiederherstellung logische und manuelle Fehler beheben können. Das Ziel ist also entweder, Sicherungen für die Wiederherstellung des Zustands der Datenbank zu einem bestimmten Zeitpunkt zu verwenden, oder, mithilfe einer Sicherung in Azure durch Seeding ein anderes System aufzusetzen, indem die bestehende Datenbank kopiert wird. Beispielsweise ließe sich eine Umstellung von einer 2-stufigen SAP-Konfiguration auf ein 3-stufiges Systemsetup desselben Systems durch Wiederherstellung einer Sicherung umsetzen.

Zum Sichern von SQL Server im Azure-Speicher gibt es drei verschiedene Verfahren:
 
1. SQL Server 2012 CU4 und höher können Datenbanken systemintern unter einer URL sichern. Eine detaillierte Beschreibung finden Sie im Blog [New functionality in SQL Server 2014 – Part 5 – Backup/Restore Enhancements](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx) (Neue Funktionen in SQL Server 2014 – Teil 5 – Verbesserungen bei Sicherung und Wiederherstellung). Weitere Informationen finden Sie im Kapitel [SQL Server 2012 SP1 CU4 und höher][dbms-guide-5.5.1].
1. SQL Server-Versionen vor SQL 2012 CU4 können für die Sicherung auf einer virtuellen Festplatte eine Umleitungsfunktion verwenden und den Schreibdatenstrom an einen konfigurierten Azure-Speicherort verschieben. Weitere Informationen finden Sie im Kapitel [SQL Server 2012 SP1 CU3 und frühere Versionen][dbms-guide-5.5.2].
1. Die letzte Methode besteht darin, auf einem VHD-Datenträgergerät eine herkömmliche SQL Server-Datenträgersicherung durchzuführen. Dies ist mit dem lokalen Bereitstellungsmuster identisch und wird in diesem Dokument nicht ausführlich erläutert.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 und höher
Diese Funktion ermöglicht Ihnen das direkte Sichern im Azure-BLOB-Speicher. Ohne diese Methode müssen Sie die Sicherungen auf anderen Azure-VHDs durchführen. Dabei werden VHD- und IOPS-Kapazitäten beansprucht. Der grundlegende Ansatz lautet wie folgt:
 
 ![Verwenden der SQL Server 2012-Sicherung für Microsoft Azure Storage-BLOB][dbms-guide-figure-400]

Der Vorteil besteht in diesem Fall darin, dass zum Speichern von SQL Server-Sicherungen keine virtuelle Festplatten genutzt werden müssen. Somit sind weniger virtuelle Festplatten zugeordnet, und die gesamte VHD-IOPS-Bandbreite kann für Daten-und Protokolldateien verwendet werden. Beachten Sie, dass die maximale Größe einer Sicherung auf 1TB beschränkt ist, wie im Abschnitt „Einschränkungen“ dieses Artikels dokumentiert: <https://msdn.microsoft.com/library/dn435916.aspx#limitations> Wenn die Sicherungsgröße trotz der Verwendung der SQL Server-Sicherungskomprimierung eine Größe von 1TB übersteigen würde, muss die im Kapitel [SQL Server 2012 SP1 CU3 und frühere Versionen][dbms-guide-5.5.2] dieses Dokuments beschriebene Funktion verwendet werden.

[Verwandte Dokumentation](https://msdn.microsoft.com/library/dn449492.aspx): Hier wird die Wiederherstellung von Datenbanken aus Sicherungen im Azure-Blobspeicher beschrieben. Die direkte Wiederherstellung aus dem Azure-Blobspeicher wird nicht empfohlen, wenn die Sicherungen eine Größe von 25GB übersteigen. Die Empfehlung in diesem Artikel basiert auf Leistungsüberlegungen und beruht nicht auf funktionalen Einschränkungen. Daher können von Fall zu Fall verschiedene Bedingungen zutreffen.

Eine Dokumentation darüber, wie dieser Sicherungstyp eingerichtet und verwendet wird, finden Sie in [diesem](https://msdn.microsoft.com/library/dn466438.aspx) Tutorial.
 
Ein Beispiel für die Abfolge der Schritte finden Sie [hier](https://msdn.microsoft.com/library/dn435916.aspx).

Bei der Automatisierung von Sicherungen muss unbedingt sichergestellt werden, dass die BLOBs für jede Sicherung unterschiedlich benannt werden. Andernfalls werden sie überschrieben, und die Wiederherstellungskette wird unterbrochen.
 
Um bei den drei verschiedenen Sicherungsarten keine Überlappungen zu erzeugen, ist es ratsam, in dem für die Sicherung verwendeten Speicherkonto mehrere Container zu erstellen. Die Container könnten für jeweils nur einen virtuellen Computer oder für virtuelle Computer und Sicherungstyp erstellt werden. Das Schema könnte wie folgt aussehen:
 
 ![Verwenden der SQL Server 2012-Sicherung für Microsoft Azure Storage-BLOB – verschiedene Container unter verschiedenen Speicherkonten][dbms-guide-figure-500]

Im obigen Beispiel würden die Sicherungen nicht unter dem Speicherkonto ausgeführt werden, in dem die virtuellen Computer bereitgestellt werden. Stattdessen gäbe es ein neues Speicherkonto speziell für die Sicherungen. Die Speicherkonten würden unterschiedliche Container enthalten, die mit einer Matrix des Sicherungstyps und des Namens des virtuellen Computers erstellt werden. Eine solche Segmentierung erleichtert die Verwaltung der Sicherungen verschiedener virtueller Computer.

Die BLOBs, in die die Sicherungen direkt geschrieben werden, werden für die Anzahl der virtuellen Festplatten eines virtuellen Computers nicht berücksichtigt. Daher ist es möglich, die maximale Anzahl der bereitgestellten VHDs für die spezifische VM-SKU für die Daten- und Transaktionsprotokolldatei zu erhöhen und weiterhin eine Sicherung mit einem Speichercontainer auszuführen.

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 und frühere Versionen
Um eine Sicherung direkt im Azure-Speicher durchzuführen, müssen Sie im ersten Schritt die MSI-Datei herunterladen, die mit [diesem](https://www.microsoft.com/download/details.aspx?id=40740) KBA-Artikel verknüpft ist.
 
Laden Sie die X64-Installationsdatei und die Dokumentation herunter. Die Datei installiert ein Programm namens „Microsoft SQL Server Backup to Microsoft Azure Tool“. Lesen Sie sich die Dokumentation des Produkts sorgfältig durch. Das Tool funktioniert grundsätzlich wie folgt:

* SQL Server-seitig wird ein Speicherort für die SQL Server-Sicherung definiert (verwenden Sie nicht das Laufwerk „D:\\“).
* Das Tool ermöglicht Ihnen die Definition von Regeln, die verwendet werden können, um verschiedene Sicherungsarten unterschiedlichen Azure-Speichercontainern zuzuordnen.
* Nach der Erstellung der Regeln leitet das Tool den Schreibdatenstrom der Sicherung zu einer der VHDs/einem der Datenträger an dem zuvor definierten Azure-Speicherort.
* Das Tool hinterlässt eine kleine Stubdatei mit einer Größe von wenigen KB auf der VHD/dem Datenträger, die bzw. der für die SQL Server-Sicherung definiert wurde. **Diese Datei sollte an dem Speicherort verbleiben, da sie für Wiederherstellungen aus dem Azure-Speicher benötigt wird.**
	* Wenn die Stubdatei verloren gegangen ist (z.B. durch den Verlust von Speichermedien, in denen die Stubdatei enthalten war) und Sie die Sicherung in einem Microsoft Azure-Speicherkonto ausgewählt haben, können Sie die Stubdatei über Microsoft Azure Storage wiederherstellen, indem Sie sie aus dem Speichercontainer herunterladen, in dem sie abgelegt wurde. Anschließend sollten Sie die Stubdatei in einem Ordner auf dem lokalen Computer platzieren, auf dem das Tool so konfiguriert ist, dass dieser Container erkannt wird und zum Hochladen verwendet wird. Dabei muss dasselbe Verschlüsselungskennwort verwendet werden, sofern Verschlüsselung für die ursprüngliche Regel verwendet wurde.

Dies bedeutet, dass das weiter oben für neuere Versionen von SQL Server beschriebene Schema auch für SQL Server-Versionen platziert werden kann, die keine direkte Adressierung eines Azure-Speicherorts zulassen.
 
Diese Methode sollte nicht für neuere Versionen von SQL Server verwendet werden, die eine systeminterne Sicherung im Azure-Speicher unterstützen. Ausnahmen gelten in Fällen, in denen die Einschränkungen für systemeigene Sicherungen in Azure die Ausführung einer systemeigenen Sicherung in Azure blockieren.

#### Weitere Möglichkeiten zum Sichern von SQL Server-Datenbanken
Eine weitere Möglichkeit zum Sichern von Datenbanken besteht darin, zusätzliche VHDs an einen virtuellen Computer anzufügen, auf dem Sicherungen gespeichert werden. In diesem Fall müssen Sie sicherstellen, dass die virtuellen Festplatten nicht mit voller Auslastung ausgeführt werden. Sollte dies der Fall sein, müssen Sie die Bereitstellung der virtuellen Festplatte aufheben, diese „archivieren“ und durch eine neue leere virtuelle Festplatte ersetzen. Bei diesem Verfahren sollten diese virtuellen Festplatten in Azure-Speicherkonten platziert werden, die von den Speicherkonten mit den Datenbankdateien getrennt sind.

Eine zweite Möglichkeit besteht darin, einen großen virtuellen Computer zu verwenden, dem viele VHDs angefügt werden können. Beispiel: D14 mit 32 VHDs. Verwenden Sie Speicherplätze zum Erstellen einer flexiblen Umgebung, in der Sie Freigaben erstellen können, die dann als Sicherungsziele für die verschiedenen DBMS-Server verwendet werden.
 
Weitere bewährte Methoden finden Sie [hier](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx).

#### Einfluss der Sicherung und Wiederherstellung auf die Leistung
Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit nur bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen Datendateien gespeichert sind, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je geringer die Anzahl der CPU-Threads auf dem virtuellen Computer, desto größere Auswirkungen hat die Sicherungskomprimierung.
* Je weniger Sicherungsziele (BLOBs oder VHDs), desto geringer der Durchsatz.
* Je kleiner die Größe des virtuellen Computers, desto geringer das Speicherdurchsatzkontingent beim Schreiben und Lesen aus dem Azure-Speicher. Dies gilt unabhängig davon, ob die Sicherungen direkt im Azure-Blob oder auf virtuellen Festplatten gespeichert werden, die wiederum in Azure-Blobs gespeichert werden.

Wenn Sie in neueren Versionen ein Microsoft Azure Storage-BLOB als Sicherungsziel verwenden, können Sie nur ein URL-Ziel für jede Sicherung festlegen.
 
Bei Verwendung von „Microsoft SQL Server Backup to Microsoft Azure Tool“ in älteren Versionen können Sie hingegen mehrere Dateiziele definieren. Bei mehreren Zielen kann die Sicherung skaliert werden, und der Durchsatz der Sicherung ist höher. In Folge enthält das Azure-Speicherkonto dann ebenfalls mehrere Dateien. Unsere Tests haben ergeben, dass bei der Verwendung mehrerer Dateiziele ein Durchsatz erzielt werden kann, der auch mit den ab SQL Server 2012 SP1 CU4 implementierten Sicherungserweiterungen erreicht wird. Darüber hinaus gilt hier nicht die Einschränkung von 1 TB wie in der systemeigenen Sicherung in Azure.

Bedenken Sie jedoch, dass der Durchsatz auch vom Standort des Azure-Speicherkontos abhängig ist, das Sie für die Sicherung verwenden. Möglich wäre es, das Speicherkonto in einer anderen Region als der Region zu platzieren, in der die virtuellen Computer ausgeführt werden. Beispielsweise können Sie die VM-Konfiguration in Westeuropa ausführen, aber das für die Sicherung verwendete Speicherkonto in Nordeuropa platzieren. Dies wirkt sich definitiv auf den Sicherungsdurchsatz aus und generiert höchstwahrscheinlich keinen Durchsatz von 150 MB/s, wie es in Fällen möglich zu sein scheint, in denen der Zielspeicher und die virtuellen Computer in demselben regionalen Datencenter ausgeführt werden.

#### Verwalten von Sicherungs-BLOBs
Es gibt die Anforderung, Sicherungen selbst zu verwalten. Da bei der regelmäßigen Ausführung von Transaktionsprotokollsicherungen erwartungsgemäß viele Blobs erstellt werden, kann die Verwaltung dieser Blobs das Azure-Portal schnell überlasten. Daher wird empfohlen, einen Azure-Speicher-Explorer zu verwenden. Es sind mehrere gute Azure-Speicher-Explorer verfügbar, die dabei helfen können, ein Azure-Speicherkonto zu verwalten.

* Microsoft Visual Studio mit installiertem Azure-SDK (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure-Speicher-Explorer (<https://azure.microsoft.com/downloads/>)
* Drittanbietertools

[comment]: <> (Noch nicht in ARM unterstützt) 
[comment]: <> (#### Azure-VM-Sicherung 
[comment]: <> (VMs im SAP-System können mit der Funktion für die Sicherung virtueller Azure-Computer gesichert werden. Die Sicherung virtueller Azure-Computer wurde Anfang 2015 eingeführt und ist mittlerweile eine Standardmethode zum Sichern eines vollständigen virtuellen Computers in Azure. Bei der Azure-Sicherung werden die Sicherungen in Azure gespeichert, und die Wiederherstellung eines virtuellen Computers ist möglich.) 
[comment]: <> (VMs mit Datenbanken können auch konsistent gesichert werden, wenn die DBMS-Systeme Windows VSS (Volume Shadow Copy Service, Volumenschattenkopie-Dienst – <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>) ebenso wie zum Beispiel SQL Server unterstützen. Die Verwendung der Sicherung virtueller Azure-Computer kann also eine Möglichkeit sein, eine wiederherstellbare Sicherung einer SAP-Datenbank zu erhalten. Bedenken Sie jedoch, dass auf Basis der Sicherungen virtueller Azure-Computer keine Point-in-Time-Wiederherstellung von Datenbanken möglich ist. Daher wird empfohlen, die Sicherung von Datenbanken mit DBMS-Funktionen durchzuführen und nicht die Sicherung virtueller Azure-Computer zu verwenden. 
[comment]: <> (Um sich mit der Sicherung virtueller Azure-Computer vertraut zu machen, besuchen Sie die folgende Webseite: <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace
Microsoft bietet im Azure Marketplace virtuelle Computer an, die bereits Versionen von SQL Server enthalten. Für SAP-Kunden, die Lizenzen für SQL Server und Windows benötigen, ist dies möglicherweise eine Gelegenheit, diese Lizenzen durch Einrichten virtueller Computer abzudecken, auf denen SQL Server bereits installiert ist. Um solche Images für SAP zu verwenden, müssen die folgenden Aspekte berücksichtigt werden:

* Die Nicht-Evaluierungsversionen von SQL Server verursachen höhere Kosten als ein virtueller Computer mit einer „Nur-Windows-Bereitstellung“ aus dem Azure Marketplace. Einen Preisvergleich finden Sie in den folgenden Artikeln: <https://azure.microsoft.com/pricing/details/virtual-machines/> und <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>.
* Sie können nur SQL Server-Versionen verwenden, die von SAP unterstützt werden, z.B. SQL Server 2012.
* Die Sortierung der SQL Server-Instanz, die auf den im Azure Marketplace verfügbaren virtuellen Computern installiert ist, ist nicht die Sortierung, die SAP NetWeaver zum Ausführen der SQL Server-Instanz benötigt. Sie können die Sortierung aber mithilfe der Anleitung im folgenden Abschnitt ändern.

#### Ändern der SQL Server-Sortierung eines virtuellen Microsoft Windows-/SQL Server-Computers
Da die SQL Server-Images im Azure Marketplace nicht die Sortierung verwenden, die für SAP NetWeaver-Clientanwendungen erforderlich ist, muss diese sofort nach der Bereitstellung geändert werden. Für SQL Server 2012 können Sie hierzu die folgenden Schritte durchführen, sobald der virtuelle Computer bereitgestellt wurde und ein Administrator sich am bereitgestellten virtuellen Computer anmelden kann:

* Öffnen Sie ein Windows-Befehlsfenster als Administrator.
* Ändern Sie das Verzeichnis in „C:\\Program Files\\Microsoft SQL Server\\110\\Setup Bootstrap\\SQLServer2012“.
* Führen Sie den folgenden Befehl aus: Setup.exe /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS=`<local_admin_account_name`> /SQLCOLLATION=SQL\_Latin1\_General\_Cp850\_BIN2
	* `<local_admin_account_name`> ist das Konto, das als Administratorkonto definiert wurde, als der virtuelle Computer zum ersten Mal über den Katalog bereitgestellt wurde.

Dieser Vorgang sollte nur wenige Minuten dauern. Um sicherzustellen, dass dieser Schritt zum richtigen Ergebnis geführt hat, führen Sie die folgenden Schritte aus:

* Öffnen Sie SQL Server Management Studio.
* Öffnen Sie ein Abfragefenster.
* Führen Sie den Befehl „sp\_helpsort“ in der SQL Server-Masterdatenbank aus.

Das Ergebnis sollte wie folgt aussehen:

	Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Wenn dies nicht das Ergebnis ist, BEENDEN Sie die SAP-Bereitstellung, und untersuchen Sie, warum der setup-Befehl nicht wie erwartet funktioniert hat. Die Bereitstellung von SAP NetWeaver-Anwendungen auf SQL Server-Instanzen mit anderen SQL Server-Codeseiten als den oben genannten wird **NICHT** unterstützt.

### SQL Server-Hochverfügbarkeit für SAP in Azure
Wie weiter oben in diesem Artikel erwähnt, besteht keine Möglichkeit zum Erstellen von freigegebenem Speicher, der erforderlich ist, um die älteste Hochverfügbarkeitsfunktion von SQL Server zu verwenden. Mit dieser Funktion würden zwei oder mehr SQL Server-Instanzen in einem Windows Server-Failovercluster (WSFC) mit einem freigegebenen Datenträger für die Benutzerdatenbanken (und u.U. tempdb) installiert werden. Dies ist die seit langer Zeit verwendete standardmäßige Hochverfügbarkeitsmethode, die auch von SAP unterstützt wird. Da Azure keinen freigegebenen Speicher unterstützt, ist die Konfiguration von SQL Server-Hochverfügbarkeit mit einer Clusterkonfiguration mit freigegebenen Datenträgern nicht möglich. Es können jedoch viele andere Hochverfügbarkeitsmethoden verwendet werden. Diese werden in den folgenden Abschnitten beschrieben.

[comment]: <> (Artikel verweist noch auf ASM) 
[comment]: <> (Bevor der Benutzer die Beschreibung der verschiedenen Technologien für Hochverfügbarkeit für SQL Server in Azure liest, sollte er das empfehlenswerte Dokument [hier][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions] mit weiteren Details und Hinweisen lesen.)

#### SQL Server-Protokollversand
Eine der Methoden für hohe Verfügbarkeit (HA) ist der SQL Server-Protokollversand. Wenn die an der Hochverfügbarkeitskonfiguration teilnehmenden VMs über eine funktionierende Namensauflösung verfügen, besteht kein Problem, und die Einrichtung in Azure unterscheidet sich nicht von anderen, lokal ausgeführten Einrichtungen. Es wird nicht empfohlen, ausschließlich die IP-Auflösung zugrunde zu legen. Informationen zur Einrichtung des Protokollversands und zu den Prinzipien des Protokollversands finden Sie in dieser Dokumentation:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Um die Hochverfügbarkeit tatsächlich zu erreichen, müssen die VMs mit einer Protokollversandkonfiguration innerhalb derselben Azure-Verfügbarkeitsgruppe bereitgestellt werden.

#### Datenbankspiegelung
Die von SAP unterstützte Datenbankspiegelung (siehe SAP-Hinweis [965908]) basiert auf der Definition eines Failoverpartners in der SAP-Verbindungszeichenfolge. Bei standortübergreifenden Fällen wird davon ausgegangen, dass sich die beiden virtuellen Computer in derselben Domäne befinden und dass es sich bei dem Benutzerkontext, unter dem die beiden SQL Server-Instanzen ausgeführt werden, ebenfalls um Domänenbenutzer handelt, die über ausreichende Berechtigungen für die zwei beteiligten SQL Server-Instanzen verfügen. Daher unterscheidet sich die Einrichtung der Datenbankspiegelung in Azure nicht von einer typischen lokalen Einrichtung/Konfiguration.

Für Nur-Cloud-Bereitstellungen besteht die einfachste Methode darin, eine weitere Domäne in Azure einzurichten, damit sich die DBMS-VMs (und im Idealfall die dedizierten SAP-VMs) innerhalb einer Domäne befinden.

Wenn eine weitere Domäne nicht möglich ist, können Sie auch Zertifikate für die Endpunkte der Datenbankspiegelung verwenden, wie hier beschrieben: <https://technet.microsoft.com/library/ms191477.aspx>

Ein Tutorial zum Einrichten der Datenbankspiegelung in Azure finden Sie hier: <https://technet.microsoft.com/library/ms189852.aspx>

#### AlwaysOn
Da Always On für lokale SAP-Bereitstellungen unterstützt wird (siehe SAP-Hinweis [1772688]), wird Always On auch in Kombination mit SAP in Azure unterstützt. Die Tatsache, dass Sie keine freigegebenen Datenträger in Azure erstellen können, bedeutet nicht, dass Sie keine AlwaysOn-WSFC-Konfiguration (Windows Server-Failovercluster) zwischen verschiedenen VMs erstellen können. Es bedeutet lediglich, dass Sie keine Möglichkeit haben, einen freigegebenen Datenträger als Quorum in der Clusterkonfiguration zu verwenden. Folglich können Sie eine AlwaysOn-WSFC-Konfiguration in Azure erstellen. Wählen Sie dabei nicht den Quorumtyp aus, der freigegebene Datenträger verwendet. Der Azure-Umgebung, in der diese VMs bereitgestellt werden, sollte die VMs nach Namen auflösen, und die VMs sollten sich in derselben Domäne befinden. Dies gilt nur für Azure und für standortübergreifende Bereitstellungen. Bei der Bereitstellung des SQL Server-Verfügbarkeitsgruppenlisteners (nicht zu verwechseln mit der Azure-Verfügbarkeitsgruppe) gibt es einige Besonderheiten zu beachten, da Azure derzeit nicht die Erstellung eines AD-/DNS-Objekts zulässt, wie es lokal möglich ist. Daher sind einige abweichende Installationsschritte notwendig, um das spezifische Verhalten von Azure zu umgehen.

Im Folgenden finden Sie einige Überlegungen zur Verwendung eines Verfügbarkeitsgruppenlisteners:

* Die Verwendung eines Verfügbarkeitsgruppenlisteners ist nur mit Windows Server 2012 oder Windows Server 2012 R2 als Gastbetriebssystem des virtuellen Computers möglich. Für Windows Server 2012 müssen Sie sicherstellen, dass der folgende Patch angewendet wird: <https://support.microsoft.com/kb/2854082>
* Dieser Patch ist für Windows Server 2008 R2 nicht vorhanden, und Always On müsste auf die gleiche Weise wie die Datenbankspiegelung verwendet werden, indem ein Failoverpartner in der Verbindungszeichenfolge angegeben wird (über den SAP-default.pfl-Parameter „dbs/mss/server“ – siehe SAP-Hinweis [965908]).
* Bei der Verwendung eines Verfügbarkeitsgruppenlisteners müssen die Datenbank-VMs mit einem dedizierten Lastenausgleich verbunden sein. Die Namensauflösung in Nur-Cloud-Bereitstellungen erfordert entweder, dass sich alle VMs eines SAP-Systems (Anwendungsserver, DBMS-Server und (A)SCS-Server) in demselben virtuellen Netzwerk befinden, oder die Wartung der Datei „etc\\host“ durch eine SAP-Anwendungsebene ist erforderlich, um die VM-Namen der SQL Server-VMs aufzulösen. Um zu vermeiden, dass Azure neue IP-Adressen in Fällen zuweist, in denen beide virtuelle Computer durch Zufall heruntergefahren werden, sollten Sie den Netzwerkschnittstellen dieser VMs in der AlwaysOn-Konfiguration statische IP-Adressen zuweisen (die Definition einer statischen IP-Adresse wird in [diesem][virtual-networks-reserved-private-ip] Artikel beschrieben).
[comment]: <> (Alte Blogs)
[comment]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>)
* Beim Erstellen der WSFC-Clusterkonfiguration, in der dem Cluster eine spezielle IP-Adresse zugewiesen muss, sind spezielle Schritte notwendig, da Azure dem Clusternamen derzeit dieselbe IP-Adresse zuweisen würde wie dem Knoten, auf dem der Cluster erstellt wird. Das bedeutet, dass ein manueller Schritt ausgeführt werden muss, um dem Cluster eine andere IP-Adresse zuzuweisen.
* Der Verfügbarkeitsgruppenlistener wird in Azure mit TCP/IP-Endpunkten erstellt, die den virtuellen Computern zugewiesen werden, auf denen die primären und sekundären Replikate der Verfügbarkeitsgruppe ausgeführt werden.
* Möglicherweise müssen diese Endpunkte mit Zugriffssteuerungslisten (ACLs) gesichert werden.

[comment]: <> (TODO alter Bog) 
[comment]: <> (Die einzelnen Schritte und Anforderungen bei der Installation einer Always On-Konfiguration in Azure werden am besten durch das Tutorial veranschaulicht, das [hier][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups] verfügbar ist.) 
[comment]: <> (Vorkonfigurierte Always On-Einrichtung über den Azure-Katalog <https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>) 
[comment]: <> (Die Erstellung eines Verfügbarkeitsgruppenlisteners wird am besten in [diesem][virtual-machines-windows-classic-ps-sql-int-listener] Tutorial beschrieben.) 
[comment]: <> (Die Sicherung von Netzwerkendpunkten mit ACLs wird am besten hier erklärt:) [comment]: <> (* <https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>) 
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>) 
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>) 
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>)

Es ist auch möglich, eine SQL Server-AlwaysOn-Verfügbarkeitsgruppe über verschiedene Azure-Regionen hinweg bereitzustellen. Bei dieser Funktion wird die Azure-VNet-zu-Vnet-Konnektivität verwendet ([weitere Details][virtual-networks-configure-vnet-to-vnet-connection]). 
[comment]: <> (TODO alter Blog) 
[comment]: <> (Die Einrichtung von SQL Server-AlwaysOn-Verfügbarkeitsgruppen in einem solchen Szenario wird hier beschrieben: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.)

#### SQL Server-Hochverfügbarkeit in Azure – Zusammenfassung
Da der Azure-Speicher Inhalte schützt, gibt es einen Grund weniger, der für ein Hot-Standby-Image spricht. Dies bedeutet, dass Ihr Hochverfügbarkeitsszenario nur in den folgenden Fällen einen Schutz bieten muss:

* Nichtverfügbarkeit des virtuellen Computers insgesamt aufgrund einer Wartung auf dem Servercluster in Azure oder aus anderen Gründen
* Softwareprobleme bei der SQL Server-Instanz
* Schutz vor manuellen Fehlern, wenn Daten gelöscht werden und eine Point-in-Time-Wiederherstellung erforderlich ist

Bei der Betrachtung entsprechender Technologien wäre die Aussage möglich, dass die ersten beiden Fälle durch die Datenbankspiegelung oder AlwaysOn und der dritte Fall nur durch den Protokollversand abgedeckt werden können.

Sie sollten die im Vergleich zur Datenbankspiegelung komplexere Einrichtung von AlwaysOn mit den durch AlwaysOn gebotenen Vorteilen abwägen. Zu diesen Vorteilen zählen:

*	Lesbare sekundäre Replikate
*	Sicherungen aus sekundären Replikaten
*	Bessere Skalierbarkeit
*	Mehrere sekundäre Replikate

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>SQL Server für SAP in Azure – Allgemeine Zusammenfassung
In diesem Handbuch finden Sie eine Reihe von Empfehlungen. Es ist ratsam, das Handbuch mehrmals zu lesen, bevor Sie Ihre Azure-Bereitstellung planen. Im Allgemeinen jedoch sollten Sie die zehn wichtigsten Punkte für DBMS in Azure befolgen:

[comment]: <> (2.3 hat einen höheren Durchsatz als was? Als eine virtuelle Festplatte?)
1. Verwenden Sie die neueste DBMS-Version wie SQL Server 2014, die die meisten Vorteile in Azure bietet. Bei SQL Server ist dies SQL Server 2012 SP1 CU4, worin auch das Feature zur Sicherung im Azure-Speicher enthalten ist. Gemeinsam mit SAP empfehlen wir jedoch, mindestens SQL Server 2014 SP1 CU1 oder SQL Server 2012 SP2 und die neueste CU zu verwenden.
1. Planen Sie Ihre SAP-Systemlandschaft in Azure sorgfältig, um das Layout von Datendateien und die Einschränkungen von Azure gegeneinander abzuwägen:
	* Verwenden Sie nicht zu viele VHDs, aber ausreichend viele, um sicherzustellen, dass die erforderlichen IOPS erzielt werden können.
	* Denken Sie daran, dass IOPS pro Azure-Speicherkonto beschränkt sind und dass Speicherkonten innerhalb jedes Azure-Abonnements beschränkt sind ([weitere Details][azure-subscription-service-limits]).
	* Erstellen Sie nur Stripesets für VHDs, wenn Sie einen höheren Durchsatz erzielen müssen.
1. Auf dem Laufwerk „D:\\“ sollten Sie nie Software installieren oder Dateien speichern, die Persistenz erfordern, da dieses Laufwerk nicht permanent ist und somit sämtliche Inhalte bei einem Windows-Neustart verloren gehen.
1. Verwenden Sie kein Azure-VHD-Caching für den Azure-Standardspeicher.
1. Verwenden Sie keine georeplizierten Azure-Speicherkonten. Verwenden Sie die Option „Lokal redundant“ für DBMS-Workloads.
1. Verwenden Sie die Hochverfügbarkeits- und Notfallwiederherstellungslösung Ihres DBMS-Herstellers, um Datenbankdaten zu replizieren.
1. Verwenden Sie immer die Namensauflösung, und verlassen Sie sich nicht auf IP-Adressen.
1. Verwenden Sie die höchstmögliche Datenbankkomprimierung. Für SQL Server ist dies die Seitenkomprimierung.
1. Verwenden Sie SQL Server-Images aus dem Azure Marketplace mit großer Sorgfalt. Wenn Sie SQL Server verwenden, müssen Sie die Instanzensortierung vor der Installation eines SAP NetWeaver-Systems ändern.
1. Installieren und konfigurieren Sie die SAP-Hostüberwachung für Azure wie im [Bereitstellungshandbuch][deployment-guide] beschrieben.

## Besonderheiten bei SAP ASE unter Windows
Ab Microsoft Azure lassen sich bestehende SAP ASE-Anwendungen einfach zu virtuellen Azure-Computern migrieren. Wenn Sie SAP ASE auf einem virtuellen Computer betreiben, können Sie die Gesamtbetriebskosten der Bereitstellung, Verwaltung und Wartung von unternehmensweiten Anwendungen senken, indem Sie diese Anwendungen einfach zu Microsoft Azure migrieren. Bei SAP ASE auf einem virtuellen Azure-Computer stehen Administratoren und Entwicklern die vertrauten Entwicklungs- und Verwaltungstools aus der lokalen Umgebung zur Verfügung.

Eine SLA für die Azure-VMs finden Sie hier: <https://azure.microsoft.com/support/legal/sla>

Wir sind davon überzeugt, dass unter Microsoft Azure gehostete virtuelle Computer im Vergleich zu anderen Virtualisierungsangeboten in der Public Cloud sehr gute Leistung erbringen. Die einzelnen Ergebnisse können allerdings variieren. Informationen zur SAP-Größe für SAPS-Zahlen der verschiedenen SAP-zertifizierten VM-SKUs finden Sie im separaten SAP-Hinweis [1928533].

Aussagen und Empfehlungen betreffend die Verwendung von Azure Storage, die Bereitstellung von SAP-VMs oder SAP Monitoring gelten auch für Bereitstellungen von SAP ASE in Verbindung mit SAP-Anwendungen (siehe die ersten vier Kapitel dieses Dokuments).

### Versionsunterstützung für SAP ASE 
Zur Verwendung mit Produkten der SAP Business Suite wird in SAP momentan SAP ASE Version 16.0 unterstützt. Jegliche Updates für SAP ASE-Server und JDBC- sowie ODBC-Treiber für die Verwendung mit Produkten der SAP Business Suite werden ausschließlich über den SAP Service Marketplace bereitgestellt: <https://support.sap.com/swdc>

Wie für alle lokalen Installationen gilt: Laden Sie Updates für SAP ASE-Server und für JDBC- sowie ODBC-Treiber nicht direkt von den Sybase-Websites herunter. Detaillierte Informationen zu Patches, die für die lokale Verwendung mit Produkten der SAP Business Suite und die Verwendung mit virtuellen Azure-Computern unterstützt werden, erhalten Sie in den folgenden SAP-Hinweisen:

* [1590719]
* [1973241]
 
Allgemeine Informationen zum Ausführen der SAP Business Suite in SAP ASE finden Sie im [SCN](https://scn.sap.com/community/ase).

### SAP ASE-Konfigurationsrichtlinien für SAP-bezogene SAP ASE-Installationen auf Azure-VMs

#### Struktur der SAP ASE-Bereitstellung
In Übereinstimmung mit der allgemeinen Beschreibung sollten ausführbare SAP ASE-Dateien sich im Systemverzeichnis der Basis-VHD (Laufwerk „C:“) des virtuellen Computers befinden bzw. bei der Installation dort abgelegt werden. Üblicherweise werden die meisten System- und Tooldatenbanken von SAP ASE durch die SAP NetWeaver-Workload nicht stark in Anspruch genommen. Daher können auch die System- und Tooldatenbanken („master“, „model“, „saptools“, „sybmgmtdb“, „sybsystemdb“) auf dem Laufwerk „C:\\“ verbleiben.

Als Ausnahme kommt die temporäre Datenbank in Betracht, die sämtliche Arbeitstabellen und temporären Tabellen enthält, die in SAP ASE erstellt werden. Im Falle einiger SAP ERP- sowie aller BW-Workloads kann die temporäre Datenbank entweder ein höheres Volumen für Daten oder für E/A-Vorgänge erfordern, wodurch die Speicherkapazität der Basis-VHD (Laufwerk „C:\\“) der ursprünglichen VM übertroffen wird.
 
Abhängig von der Version von SAPInst/SWPM, die für die Installation des Systems verwendet wurde, kann die Datenbank Folgendes enthalten:

* Eine einzelne SAP ASE-tempdb, die bei der Installation von SAP ASE erstellt wird
* Eine bei der Installation von SAP ASE erstellte SAP ASE-tempdb sowie eine zusätzliche saptempdb, die durch die SAP-Installationsroutine erstellt wird
* Eine SAP ASE-tempdb, die bei der Installation von SAP ASE erstellt wird, sowie eine zusätzliche, (z.B. durch Befolgen der Schritte in SAP-Hinweis [1752266]) manuell erstellte tempdb, um bestimmte ERP/BW-spezifische Anforderungen an tempdb zu erfüllen

Im Falle von bestimmten ERP- oder bei allen BW-Workloads ist es hinsichtlich der Leistung sinnvoll, die tempdb-Geräte der zusätzlich erstellten tempdb (durch SWPM oder manuell erstellt) auf einem anderen Laufwerk als „C:“ zu speichern. Wenn keine zusätzliche tempdb vorhanden ist, sollten Sie diese erstellen (siehe SAP-Hinweis [1752266]).

Trifft dies auf Ihr System zu, sollten Sie für die zusätzlich erstellte tempdb die folgenden Schritte ausführen:

* Verschieben Sie das erste tempdb-Gerät auf das erste Gerät der SAP-Datenbank.
* Fügen Sie tempdb-Geräte zu jeder der VHDs hinzu, die ein Gerät der SAP-Datenbank enthält.

Mithilfe dieser Konfiguration kann tempdb einerseits mehr Speicherplatz nutzen, als auf dem Systemlaufwerk vorhanden ist. (Als Vergleichsmaßstab können Sie die tempdb-Verzeichnisgrößen bestehender lokaler Systeme heranziehen.) Eine solche Konfiguration ermöglicht andererseits IOPS-Größenordnungen für tempdb, die das Systemlaufwerk nicht erzielen kann. Auch hier können lokal ausgeführte Systeme zur Überwachung des E/A-Workloads für tempdb verwendet werden.

Platzieren Sie niemals SAP ASE-Geräte auf dem Laufwerk „D:\\“ des virtuellen Computers. Dies gilt auch für die tempdb, auch wenn die Objekte in der tempdb nur temporär sind.

#### Auswirkungen der Datenbankkomprimierung
Bei Konfigurationen, bei denen die E/A-Bandbreite zum begrenzenden Faktor werden könnte, sollte jede Möglichkeit ergriffen werden, die IOPS zu senken. Dadurch kann die ausführbare Workload in einem IaaS-Szenario wie Azure besser verteilt werden. Verwenden Sie daher unbedingt die SAP ASE-Komprimierung, bevor Sie eine bestehende SAP-Datenbank in Azure hochladen.

Vor dem Hochladen in Azure die Komprimierung auszuführen, wenn dies nicht bereits implementiert ist, wird aus den folgenden Gründen empfohlen:

* Die in Azure hochzuladende Datenmenge ist geringer.
* Die Ausführungsdauer der Komprimierung ist geringer (unter der Annahme, dass lokal leistungsfähigere Hardware mit mehr CPUs, einer höheren E/A-Bandbreite oder einer geringeren E/A-Latenz zur Verfügung steht).
* Durch kleinere Datenbanken werden u. U. die Kosten für die Datenträgerzuordnung gesenkt.

Die Daten- und LOB-Komprimierung funktioniert auf einer VM, die auf einem virtuellen Azure-Computer gehostet wird, genauso wie in der lokalen Umgebung. Weitere Informationen zur Überprüfung, ob die Komprimierung bei einer bestehenden SAP ASE-Datenbank bereits in Verwendung ist, erhalten Sie in SAP-Hinweis [1750510].

#### Verwenden von DBACockpit für die Überwachung von Datenbankinstanzen
Bei SAP-Systemen, bei denen SAP ASE als Datenbankplattform verwendet wird, erfolgt der Zugriff auf das DBACockpit über eingebettete Browserfenster im Transaktions-DBACockpit oder über Webdynpro. Die vollständige Funktionalität für die Überwachung und Verwaltung der Datenbank steht allerdings nur über die Webdynpro-Implementierung von DBACockpit zur Verfügung.

Wie bei lokalen Systemen sind einige Schritte erforderlich, um die gesamte SAP NetWeaver-Funktionalität zu aktivieren, die durch die Webdynpro-Implementierung von DBACockpit genutzt wird. Befolgen Sie die Schritte in SAP-Hinweis [1245200], um die Verwendung von Webdynpros zu aktivieren und die erforderlichen Webdynpros zu generieren. Beim Befolgen der Anweisungen in den obigen Hinweisen werden Sie auch den Internet Communication Manager (ICM) zusammen mit den Ports konfigurieren, die für HTTP- und HTTPS-Verbindungen verwendet werden. Die Standardeinstellung für HTTP sieht wie folgt aus:

> icm/server\_port\_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
>
> icm/server\_port\_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600

Die im Transaktions-DBACockpit generierten Links sehen so aus:

> https://`<vollqualifizierter\_Hostname`>:44300/sap/bc/webdynpro/sap/dba\_cockpit
> 
> http://`<vollqualifizierter\_Hostname`>:8000/sap/bc/webdynpro/sap/dba\_cockpit

Der virtuelle Azure-Computer, auf dem das SAP-System gehostet wird, kann auf verschiedene Arten verbunden sein: Site-to-Site, mehrere Standorte oder ExpressRoute (standortübergreifende Bereitstellung). In Abhängigkeit davon muss ICM einen vollqualifizierten Hostnamen verwenden, der von dem Computer, von dem aus DBACockpit geöffnet werden soll, aufgelöst werden kann. In SAP-Hinweis [773830] erfahren Sie, wie ICM den vollqualifizierten Hostnamen in Abhängigkeit von Profilparametern feststellt. Setzen Sie explizit den Parameter „icm/host\_name\_full“, falls erforderlich.

Wird die VM in einem Nur-Cloud-Szenario ohne standortübergreifende Konnektivität zwischen der lokalen Umgebung und Azure bereitgestellt, müssen eine öffentliche IP-Adresse und eine Domänenbezeichnung festgelegt werden. Das Format des öffentlichen DNS-Namens der VM sieht dann so aus:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com

Einzelheiten zum DNS-Namen finden Sie [hier][virtual-machines-azurerm-versus-azuresm].

Wenn Sie den SAP-Profilparameter „icm/host\_name\_full“ auf den DNS-Namen der Azure-VM einstellen, erhalten Sie einen Link, der ähnlich wie dieser aussieht:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

Stellen Sie in diesem Fall Folgendes sicher:

* Fügen Sie der Netzwerksicherheitsgruppe im Azure-Portal für die TCP/IP-Ports, die für die Kommunikation mit ICM verwendet werden, eingehende Regeln hinzu.
* Fügen Sie der Windows-Firewall-Konfiguration für die TCP/IP-Ports, die für die Kommunikation mit ICM verwendet werden, eingehende Regeln hinzu.

Für den automatischen Import aller zur Verfügung stehenden Korrekturen wird empfohlen, regelmäßig den Ihre Version betreffenden SAP-Hinweis zur Korrektursammlung zu konsultieren:

* [1558958]
* [1619967]
* [1882376]

Weitere Informationen zu DBA Cockpit für SAP ASE erhalten Sie in den folgenden SAP-Hinweisen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]
* [1814258]
* [1922555]
* [1956005]

#### Überlegungen zur Sicherung und Wiederherstellung für SAP ASE
Bei der Bereitstellung von SAP ASE in Azure ist es erforderlich, die Sicherungsmethode zu überprüfen. Auch wenn es sich bei dem System nicht um ein Produktionssystem handelt, muss die durch SAP ASE gehostete SAP-Datenbank regelmäßig gesichert werden. Da Azure Storage drei Images vorhält, ist die Sicherung, um einen etwaigen Speicherabsturz kompensieren zu können, weniger dringlich. Der wichtigste Grund, einen ordnungsgemäßen Sicherungs- und Wiederherstellungsplan zu verfolgen, ist vielmehr, dass Sie mithilfe der Point-in-Time-Wiederherstellung logische und manuelle Fehler beheben können. Das Ziel ist also entweder, Sicherungen für die Wiederherstellung des Zustands der Datenbank zu einem bestimmten Zeitpunkt zu verwenden, oder, mithilfe einer Sicherung in Azure durch Seeding ein anderes System aufzusetzen, indem die bestehende Datenbank kopiert wird. Beispielsweise ließe sich eine Umstellung von einer 2-stufigen SAP-Konfiguration auf ein 3-stufiges Systemsetup desselben Systems durch Wiederherstellung einer Sicherung umsetzen.

Sicherung und Wiederherstellung einer Datenbank in Azure funktionieren wie bei lokalen Umgebungen. Weitere Informationen finden Sie in diesen SAP-Hinweisen:

* [1588316]
* [1585981]

Darin wird erläutert, wie Abbildkonfigurationen erstellt und Sicherungen geplant werden. Je nach Ihrer Strategie und Ihren Anforderungen können Sie Datenbank- und Protokollabbilder für Datenträger konfigurieren und auf einer der vorhandenen VHDs speichern. Außerdem lässt sich eine zusätzliche VHD zur Sicherung hinzufügen. Um die Gefahr von Datenverlusten bei einem Fehler zu reduzieren, wird empfohlen, eine VHD zu verwenden, auf der sich kein Datenbankgerät befindet.

Neben der Daten- und LOB-Komprimierung lässt sich in SAP ASE auch die Sicherung komprimieren. Um den Platzbedarf der Datenbank- und Protokollabbilder zu senken, empfiehlt es sich, die Sicherungskomprimierung zu verwenden. Weitere Informationen finden Sie in SAP-Hinweis [1588316]. Für den Fall, dass Sie zukünftig Sicherungen oder VHDs mit Sicherungsabbildungen vom virtuellen Azure-Computer auf lokale Rechner herunterladen möchten, wird durch Komprimieren der Sicherung auch die zu übertragende Datenmenge gesenkt.

Verwenden Sie das Laufwerk „D:\\“ nicht als Speicherziel für Datenbank- oder Protokolldateien.

#### Einfluss der Sicherung und Wiederherstellung auf die Leistung
Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit nur bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen sich Datenbankgeräte befinden, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Ziele (Stripeset-Verzeichnisse, VHDs), auf die Sicherungen geschrieben werden, desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Striping des Zielvolumes der Sicherung auf mehrere bereitgestellte VHDs, wodurch der IOPS-Durchsatz auf den betreffenden Volumes verbessert wird
* Erstellen einer Abbildkonfiguration auf SAP ASE-Ebene, bei der mehr als ein Zielverzeichnis für das Speichern des Abbilds angegeben wird

Informationen zum Striping eines Volumes auf mehrere bereitgestellte VHDs finden Sie oben in dieser Anleitung. Einzelheiten zur Verwendung mehrerer Verzeichnisse in der SAP ASE-Abbildkonfiguration finden Sie in der Dokumentation zur gespeicherten Prozedur „sp\_config\_dump“. Diese Funktion wird verwendet, um im [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp) die Abbildkonfiguration zu erstellen.

### Notfallwiederherstellung mit Azure-VMs

#### Datenreplikation mit SAP Sybase Replication Server
Mit dem SAP Sybase Replication Server (SRS) bietet SAP ASE eine Lösung für den betriebsbereiten Standbymodus, mit der Datenbanktransaktionen asynchron an einen anderen Standort übertragen werden können.

Installation und Betrieb von SRS funktionieren in einer VM, die in Azure Virtual Machine Services gehostet wird, genauso gut wie bei einer lokalen Implementierung.

ASE-HADR per SAP Replication Server ist bei einer zukünftigen Version vorgesehen. Dieses wird für Microsoft Azure-Plattformen getestet und sobald verfügbar für diese veröffentlicht.

## Besonderheiten bei SAP ASE unter Linux

Ab Microsoft Azure lassen sich bestehende SAP ASE-Anwendungen einfach zu virtuellen Azure-Computern migrieren. Wenn Sie SAP ASE auf einem virtuellen Computer betreiben, können Sie die Gesamtbetriebskosten der Bereitstellung, Verwaltung und Wartung von unternehmensweiten Anwendungen senken, indem Sie diese Anwendungen einfach zu Microsoft Azure migrieren. Bei SAP ASE auf einem virtuellen Azure-Computer stehen Administratoren und Entwicklern die vertrauten Entwicklungs- und Verwaltungstools aus der lokalen Umgebung zur Verfügung.

Wichtig bei der Bereitstellung von Azure-VMs ist die Kenntnis der offiziellen SLAs. Diese finden Sie hier: <https://azure.microsoft.com/support/legal/sla>

Informationen zur jeweils empfohlenen SAP-Größe sowie eine Liste mit SAP-zertifizierten VM-SKUs erhalten Sie demnächst in SAP-Hinweis [1928533]. Zusätzliche Dokumente zur SAP-Größe für Azure-VMs finden Sie hier <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> und hier <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>.

Aussagen und Empfehlungen betreffend die Verwendung von Azure Storage, die Bereitstellung von SAP-VMs oder SAP Monitoring gelten auch für Bereitstellungen von SAP ASE in Verbindung mit SAP-Anwendungen (siehe die ersten vier Kapitel dieses Dokuments).

Die beiden folgenden SAP-Hinweise beinhalten allgemeine Informationen zu ASE unter Linux und ASE in der Cloud:

* [2134316]
* [1941500]

### Versionsunterstützung für SAP ASE 
Zur Verwendung mit Produkten der SAP Business Suite wird in SAP momentan SAP ASE Version 16.0 unterstützt. Jegliche Updates für SAP ASE-Server und JDBC- sowie ODBC-Treiber für die Verwendung mit Produkten der SAP Business Suite werden ausschließlich über den SAP Service Marketplace bereitgestellt: <https://support.sap.com/swdc>

Wie für alle lokalen Installationen gilt: Laden Sie Updates für SAP ASE-Server und für JDBC- sowie ODBC-Treiber nicht direkt von den Sybase-Websites herunter. Detaillierte Informationen zu Patches, die für die lokale Verwendung mit Produkten der SAP Business Suite und die Verwendung mit virtuellen Azure-Computern unterstützt werden, erhalten Sie in den folgenden SAP-Hinweisen:

* [1590719]
* [1973241]
 
Allgemeine Informationen zum Ausführen der SAP Business Suite in SAP ASE finden Sie im [SCN](https://scn.sap.com/community/ase).

### SAP ASE-Konfigurationsrichtlinien für SAP-bezogene SAP ASE-Installationen auf Azure-VMs

#### Struktur der SAP ASE-Bereitstellung
In Übereinstimmung mit der allgemeinen Beschreibung sollten ausführbare SAP ASE-Dateien sich im Stammdateisystem der VM ( /sybase ) befinden bzw. bei der Installation dort abgelegt werden. Üblicherweise werden die meisten System- und Tooldatenbanken von SAP ASE durch die SAP NetWeaver-Workload nicht stark in Anspruch genommen. Daher können auch die System- und Tooldatenbanken („master“, „model“, „saptools“, „sybmgmtdb“, „sybsystemdb“) im Stammdateisystem verbleiben.

Als Ausnahme kommt die temporäre Datenbank in Betracht, die sämtliche Arbeitstabellen und temporären Tabellen enthält, die in SAP ASE erstellt werden. Im Falle einiger SAP ERP- sowie aller BW-Workloads kann die temporäre Datenbank entweder ein höheres Volumen für Daten oder für E/A-Vorgänge erfordern, wodurch die Speicherkapazität des Betriebssystemdatenträgers der ursprünglichen VM übertroffen wird.
 
Abhängig von der Version von SAPInst/SWPM, die für die Installation des Systems verwendet wurde, kann die Datenbank Folgendes enthalten:

* Eine einzelne SAP ASE-tempdb, die bei der Installation von SAP ASE erstellt wird
* Eine bei der Installation von SAP ASE erstellte SAP ASE-tempdb sowie eine zusätzliche saptempdb, die durch die SAP-Installationsroutine erstellt wird
* Eine SAP ASE-tempdb, die bei der Installation von SAP ASE erstellt wird, sowie eine zusätzliche, (z.B. durch Befolgen der Schritte in SAP-Hinweis [1752266]) manuell erstellte tempdb, um bestimmte ERP/BW-spezifische Anforderungen an tempdb zu erfüllen

Was den Fall der spezifischen ERP- bzw. der BW-Workloads insgesamt betrifft, ist es aus Gründen der Leistung sinnvoll, die tempdb-Geräte der (durch SWPM oder manuell) zusätzlich erstellten tempdb einem gesonderten Dateisystem zuzuweisen. Dieses kann durch einen einzelnen Azure-Datenträger oder durch Linux-RAID mit mehreren Azure-Datenträgern dargestellt werden. Wenn keine zusätzliche tempdb vorhanden ist, sollten Sie diese erstellen (siehe SAP-Hinweis [1752266]).

Trifft dies auf Ihr System zu, sollten Sie für die zusätzlich erstellte tempdb die folgenden Schritte ausführen:

* Verschieben Sie das Verzeichnis der ersten tempdb in das erste Dateisystem der SAP-Datenbank.
* Fügen Sie ein tempdb-Verzeichnis zu jeder der VHDs hinzu, die ein Dateisystem der SAP-Datenbank enthält.

Mithilfe dieser Konfiguration kann tempdb einerseits mehr Speicherplatz nutzen, als auf dem Systemlaufwerk vorhanden ist. (Als Vergleichsmaßstab können Sie die tempdb-Verzeichnisgrößen bestehender lokaler Systeme heranziehen.) Eine solche Konfiguration ermöglicht andererseits IOPS-Größenordnungen für tempdb, die das Systemlaufwerk nicht erzielen kann. Auch hier können lokal ausgeführte Systeme zur Überwachung des E/A-Workloads für tempdb verwendet werden.

SAP ASE-Verzeichnisse dürfen sich unter keinen Umständen auf „/mnt“ oder „/mnt/resource“ der VM befinden. Dies gilt auch für tempdb, auch wenn die in tempdb enthaltenen Objekte sich nur temporär darin befinden, da es sich bei „/mnt“ und „/mnt/resource“ um standardmäßige temporäre Speicherplätze für Azure-VMs handelt, die nicht persistent sind. Weitere Informationen zu temporären Speicherplätzen von Azure-VMs erhalten Sie in [diesem Artikel][virtual-machines-linux-how-to-attach-disk].

#### Auswirkungen der Datenbankkomprimierung
Bei Konfigurationen, bei denen die E/A-Bandbreite zum begrenzenden Faktor werden könnte, sollte jede Möglichkeit ergriffen werden, die IOPS zu senken. Dadurch kann die ausführbare Workload in einem IaaS-Szenario wie Azure besser verteilt werden. Verwenden Sie daher unbedingt die SAP ASE-Komprimierung, bevor Sie eine bestehende SAP-Datenbank in Azure hochladen.

Vor dem Hochladen in Azure die Komprimierung auszuführen, wenn dies nicht bereits implementiert ist, wird aus den folgenden Gründen empfohlen:

* Die in Azure hochzuladende Datenmenge ist geringer.
* Die Ausführungsdauer der Komprimierung ist geringer (unter der Annahme, dass lokal leistungsfähigere Hardware mit mehr CPUs, einer höheren E/A-Bandbreite oder einer geringeren E/A-Latenz zur Verfügung steht).
* Durch kleinere Datenbanken werden u. U. die Kosten für die Datenträgerzuordnung gesenkt.

Die Daten- und LOB-Komprimierung funktioniert auf einer VM, die auf einem virtuellen Azure-Computer gehostet wird, genauso wie in der lokalen Umgebung. Weitere Informationen zur Überprüfung, ob die Komprimierung bei einer bestehenden SAP ASE-Datenbank bereits in Verwendung ist, erhalten Sie in SAP-Hinweis [1750510]. Außerdem finden Sie in SAP-Hinweis [2121797] weitere Informationen zur Datenbankkomprimierung.

#### Verwenden von DBACockpit für die Überwachung von Datenbankinstanzen
Bei SAP-Systemen, bei denen SAP ASE als Datenbankplattform verwendet wird, erfolgt der Zugriff auf das DBACockpit über eingebettete Browserfenster im Transaktions-DBACockpit oder über Webdynpro. Die vollständige Funktionalität für die Überwachung und Verwaltung der Datenbank steht allerdings nur über die Webdynpro-Implementierung von DBACockpit zur Verfügung.

Wie bei lokalen Systemen sind einige Schritte erforderlich, um die gesamte SAP NetWeaver-Funktionalität zu aktivieren, die durch die Webdynpro-Implementierung von DBACockpit genutzt wird. Befolgen Sie die Schritte in SAP-Hinweis [1245200], um die Verwendung von Webdynpros zu aktivieren und die erforderlichen Webdynpros zu generieren. Beim Befolgen der Anweisungen in den obigen Hinweisen werden Sie auch den Internet Communication Manager (ICM) zusammen mit den Ports konfigurieren, die für HTTP- und HTTPS-Verbindungen verwendet werden. Die Standardeinstellung für HTTP sieht wie folgt aus:

> icm/server\_port\_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
>
> icm/server\_port\_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600

Die im Transaktions-DBACockpit generierten Links sehen so aus:

> https://`<vollqualifizierter\_Hostname`>:44300/sap/bc/webdynpro/sap/dba\_cockpit
> 
> http://`<vollqualifizierter\_Hostname`>:8000/sap/bc/webdynpro/sap/dba\_cockpit

Der virtuelle Azure-Computer, auf dem das SAP-System gehostet wird, kann auf verschiedene Arten verbunden sein: Site-to-Site, mehrere Standorte oder ExpressRoute (standortübergreifende Bereitstellung). In Abhängigkeit davon muss ICM einen vollqualifizierten Hostnamen verwenden, der von dem Computer, von dem aus DBACockpit geöffnet werden soll, aufgelöst werden kann. In SAP-Hinweis [773830] erfahren Sie, wie ICM den vollqualifizierten Hostnamen in Abhängigkeit von Profilparametern feststellt. Setzen Sie explizit den Parameter „icm/host\_name\_full“, falls erforderlich.

Wird die VM in einem Nur-Cloud-Szenario ohne standortübergreifende Konnektivität zwischen der lokalen Umgebung und Azure bereitgestellt, müssen eine öffentliche IP-Adresse und eine Domänenbezeichnung festgelegt werden. Das Format des öffentlichen DNS-Namens der VM sieht dann so aus:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com

Einzelheiten zum DNS-Namen finden Sie [hier][virtual-machines-azurerm-versus-azuresm].

Wenn Sie den SAP-Profilparameter „icm/host\_name\_full“ auf den DNS-Namen der Azure-VM einstellen, erhalten Sie einen Link, der ähnlich wie dieser aussieht:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit

Stellen Sie in diesem Fall Folgendes sicher:

* Fügen Sie der Netzwerksicherheitsgruppe im Azure-Portal für die TCP/IP-Ports, die für die Kommunikation mit ICM verwendet werden, eingehende Regeln hinzu.
* Fügen Sie der Windows-Firewall-Konfiguration für die TCP/IP-Ports, die für die Kommunikation mit ICM verwendet werden, eingehende Regeln hinzu.

Für den automatischen Import aller zur Verfügung stehenden Korrekturen wird empfohlen, regelmäßig den Ihre Version betreffenden SAP-Hinweis zur Korrektursammlung zu konsultieren:

* [1558958]
* [1619967]
* [1882376]

Weitere Informationen zu DBA Cockpit für SAP ASE erhalten Sie in den folgenden SAP-Hinweisen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]
* [1814258]
* [1922555]
* [1956005]

#### Überlegungen zur Sicherung und Wiederherstellung für SAP ASE
Bei der Bereitstellung von SAP ASE in Azure ist es erforderlich, die Sicherungsmethode zu überprüfen. Auch wenn es sich bei dem System nicht um ein Produktionssystem handelt, muss die durch SAP ASE gehostete SAP-Datenbank regelmäßig gesichert werden. Da Azure Storage drei Images vorhält, ist die Sicherung, um einen etwaigen Speicherabsturz kompensieren zu können, weniger dringlich. Der wichtigste Grund, einen ordnungsgemäßen Sicherungs- und Wiederherstellungsplan zu verfolgen, ist vielmehr, dass Sie mithilfe der Point-in-Time-Wiederherstellung logische und manuelle Fehler beheben können. Das Ziel ist also entweder, Sicherungen für die Wiederherstellung des Zustands der Datenbank zu einem bestimmten Zeitpunkt zu verwenden, oder, mithilfe einer Sicherung in Azure durch Seeding ein anderes System aufzusetzen, indem die bestehende Datenbank kopiert wird. Beispielsweise ließe sich eine Umstellung von einer 2-stufigen SAP-Konfiguration auf ein 3-stufiges Systemsetup desselben Systems durch Wiederherstellung einer Sicherung umsetzen.

Sicherung und Wiederherstellung einer Datenbank in Azure funktionieren wie bei lokalen Umgebungen. Weitere Informationen finden Sie in diesen SAP-Hinweisen:

* [1588316]
* [1585981]

Darin wird erläutert, wie Abbildkonfigurationen erstellt und Sicherungen geplant werden. Je nach Ihrer Strategie und Ihren Anforderungen können Sie Datenbank- und Protokollabbilder für Datenträger konfigurieren und auf einer der vorhandenen VHDs speichern. Außerdem lässt sich eine zusätzliche VHD zur Sicherung hinzufügen. Um die Gefahr von Datenverlusten bei einem Fehler zu reduzieren, wird empfohlen, eine VHD zu verwenden, auf der sich kein Datenbankverzeichnis bzw. keine Datenbankdatei befindet.

Neben der Daten- und LOB-Komprimierung lässt sich in SAP ASE auch die Sicherung komprimieren. Um den Platzbedarf der Datenbank- und Protokollabbilder zu senken, empfiehlt es sich, die Sicherungskomprimierung zu verwenden. Weitere Informationen finden Sie in SAP-Hinweis [1588316]. Für den Fall, dass Sie zukünftig Sicherungen oder VHDs mit Sicherungsabbildungen vom virtuellen Azure-Computer auf lokale Rechner herunterladen möchten, wird durch Komprimieren der Sicherung auch die zu übertragende Datenmenge gesenkt.

Verwenden Sie nicht die von der Azure-VM verwendeten temporären Speicherorte „/mnt“ oder „/mnt/resource“ als Ziel für Datenbank- oder Protokollabbilder.

#### Einfluss der Sicherung und Wiederherstellung auf die Leistung
Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit nur bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen sich Datenbankgeräte befinden, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Sicherungsziele (Linux-Software-RAID, VHDs), desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Striping des Zielvolumes der Sicherung auf mehrere bereitgestellte VHDs, wodurch der IOPS-Durchsatz auf den betreffenden Volumes verbessert wird
* Erstellen einer Abbildkonfiguration auf SAP ASE-Ebene, bei der mehr als ein Zielverzeichnis für das Speichern des Abbilds angegeben wird

Informationen zum Striping eines Volumes auf mehrere bereitgestellte VHDs finden Sie oben in dieser Anleitung. Einzelheiten zur Verwendung mehrerer Verzeichnisse in der SAP ASE-Abbildkonfiguration finden Sie in der Dokumentation zur gespeicherten Prozedur „sp\_config\_dump“. Diese Funktion wird verwendet, um im [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp) die Abbildkonfiguration zu erstellen.

### Notfallwiederherstellung mit Azure-VMs

#### Datenreplikation mit SAP Sybase Replication Server
Mit dem SAP Sybase Replication Server (SRS) bietet SAP ASE eine Lösung für den betriebsbereiten Standbymodus, mit der Datenbanktransaktionen asynchron an einen anderen Standort übertragen werden können.

Installation und Betrieb von SRS funktionieren in einer VM, die in Azure Virtual Machine Services gehostet wird, genauso gut wie bei einer lokalen Implementierung.

ASE HADR per SAP Replication Server wird derzeit nicht unterstützt. Gegebenenfalls wird dieses zu einem späteren Zeitpunkt für Microsoft Azure-Plattformen getestet und veröffentlicht.

## Besonderheiten bei Oracle Database unter Windows
Seit Mitte 2013 wird die Oracle-Software von Oracle für die Verwendung mit Microsoft Windows Hyper-V und Azure unterstützt. In diesem Artikel erfahren Sie Einzelheiten zur allgemeinen Unterstützung von Windows Hyper-V und Azure durch Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces>

Neben der allgemeinen Unterstützung wird auch das spezielle Szenario der gemeinsamen Verwendung von SAP-Anwendungen und Oracle Database unterstützt. Details finden Sie in diesem Teil des Dokumentes.

### Versionsunterstützung für Oracle
Eine detaillierte Übersicht über die verschiedenen Oracle-Versionen und die entsprechenden Betriebssystemversionen, die für den Betrieb von SAP unter Oracle auf einem virtuellen Azure-Computer unterstützt werden, erhalten Sie in SAP-Hinweis [2039619].

Allgemeine Informationen zum Ausführen der SAP Business Suite unter Oracle finden Sie im SCN: <https://scn.sap.com/community/oracle>

### Oracle-Konfigurationsrichtlinien für SAP-Installationen auf Azure-VMs

#### Speicherkonfiguration
Unterstützt wird ausschließlich eine einzige Instanz von Oracle unter Verwendung von NTFS-formatierten Datenträgern. Sämtliche Datenbankdateien müssen auf dem NTFS-Dateisystem auf VHD-Datenträgern gespeichert werden. Diese VHDs werden in eine Azure-VM eingebunden. Sie basieren auf Azure-Seitenblobspeicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Jegliche Arten von Netzlaufwerken und Remotefreigaben wie Azure-Dateidienste
 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>
 
werden **NICHT** für Oracle-Datenbankdateien unterstützt!

Die Aussagen, die in diesem Dokument in den Kapiteln [Caching für VMs und VHDs][dbms-guide-2.1] und [Microsoft Azure Storage][dbms-guide-2.3] gemacht werden, gelten auch für Bereitstellungen mit der Oracle Database, wenn Azure-VHDs auf Basis von Azure-Seitenblobspeicher verwendet werden.

Wie weiter oben im allgemeinen Teil dieses Dokuments erläutert wurde, gelten für Azure-VHDs Kontingente für den IOPS-Durchsatz. Die jeweilige Größe der Kontingente hängt vom Typ der verwendeten VM ab. Eine Liste mit VM-Typen und den entsprechenden Kontingenten finden Sie [hier][virtual-machines-sizes].

Die unterstützten Typen der Azure-VM werden in SAP-Hinweis [1928533] aufgeführt.

Solange das bestehende IOPS-Kontingent pro Datenträger den Anforderungen entspricht, ist es möglich, alle Datenbankdateien auf einer einzelnen eingebundenen Azure-VHD zu speichern.

Wenn mehr IOPS erforderlich sind, wird ausdrücklich empfohlen, Windows-Speicherpools (nur verfügbar unter Windows Server 2012 und höher) oder Windows-Striping für Windows 2008 R2 zu verwenden, um ein einzelnes großes, logisches Gerät aus mehreren eingebundenen VHD-Datenträgern zu erstellen. Weitere Informationen finden Sie in diesem Dokument im Kapitel [Software-RAID][dbms-guide-2.2]. Durch diese Herangehensweise wird der Aufwand verringert, der zur Verwaltung des Speicherplatzes notwendig ist. Außerdem müssen Dateien nicht mehr manuell auf mehrere eingebundene VHDs verteilt werden.

#### Sichern und Wiederherstellen
Die Funktionen zum Sichern und Wiederherstellen werden für die SAP BR-Tools für Oracle genauso unterstützt wie auf standardmäßigen Windows Server-Betriebssystemen und Hyper-V. Auch Oracle Recovery Manager (RMAN) wird für die Sicherung auf einen Datenträger und die Wiederherstellung von einem Datenträger unterstützt.

#### Hohe Verfügbarkeit
[comment]: <> (Link bezieht sich auf ASM.)
Oracle Data Guard wird aus Gründen der hohen Verfügbarkeit und der Notfallwiederherstellung unterstützt. Einzelheiten finden Sie in [dieser][virtual-machines-windows-classic-configure-oracle-data-guard] Dokumentation.

#### Andere
Bezüglich aller weiteren allgemeinen Themen wie Azure-Verfügbarkeitsgruppen oder SAP-Überwachung gelten die Angaben in den ersten drei Kapiteln dieses Dokuments auch für Bereitstellungen von VMs mit Oracle Database.

## Einzelheiten zur SAP MaxDB-Datenbank unter Windows

### Versionsunterstützung für SAP MaxDB
SAP unterstützt derzeit SAP MaxDB Version 7.9 für die Verwendung mit SAP NetWeaver-basierten Produkten in Azure. Jegliche Updates für SAP MaxDB-Server und JDBC- sowie ODBC-Treiber für die Verwendung mit SAP NetWeaver-basierten Produkten werden ausschließlich über den SAP Service Marketplace bereitgestellt: <https://support.sap.com/swdc> Allgemeine Informationen zur gemeinsamen Verwendung von SAP NetWeaver und SAP MaxDB erhalten Sie unter <https://scn.sap.com/community/maxdb>.

### Unterstützte Microsoft Windows-Versionen und Azure-VM-Typen für SAP MaxDB-DBMS
Die unterstützten Microsoft Windows-Versionen für SAP MaxDB-DBMS auf Azure finden Sie unter:

* [SAP-Produktverfügbarkeitsmatrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird ausdrücklich empfohlen, stets die neueste Version des Betriebssystems Microsoft Windows zu verwenden. Derzeit ist dies Windows 2012 R2.

### Verfügbare SAP MaxDB-Dokumentation
Die aktualisierte Liste zur SAP MaxDB-Dokumentation finden Sie in SAP-Hinweis [767598].
	
### SAP MaxDB-Konfigurationsrichtlinien für SAP-Installationen in Azure-VMs

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Speicherkonfiguration
Die bewährten Methoden für Azure-Speicher mit SAP MaxDB orientieren sich an den allgemeinen Empfehlungen, die im Kapitel [Struktur einer RDBMS-Bereitstellung][dbms-guide-2] ausgesprochen werden.

> [AZURE.IMPORTANT] Wie andere Datenbanken verfügt auch SAP MaxDB über Daten- und Protokolldateien. Die korrekte Terminologie bei SAP MaxDB lautet allerdings „Volume“ (nicht „Datei“). Bei SAP MaxDB sprechen wir also z.B. von Datenvolumes und Protokollvolumes. Diese sind nicht zu verwechseln mit den Datenträgervolumes des Betriebssystems.

Folgende Schritte müssen ausgeführt werden:

* Stellen Sie das Azure-Speicherkonto mit den Daten- und Protokollvolumes (d.h. Dateien) von SAP MaxDB auf **Local Redundant Storage (LRS)** ein. Eine Anleitung hierfür finden Sie im Kapitel [Microsoft Azure Storage][dbms-guide-2.3].
* Grenzen Sie den E/A-Pfad für SAP MaxDB-Datenvolumes (also Dateien) vom E/A-Pfad für Protokollvolumes (d. h. Dateien) ab. Dies bedeutet, dass SAP MaxDB-Datenvolumes (Dateien) auf einem und SAP MaxDB-Protokollvolumes (Dateien) auf einem anderen logischen Laufwerk installiert werden müssen.
* Stellen Sie, wie im Kapitel [Caching für VMs][dbms-guide-2.1] erläutert wird, für jedes Azure-Blob die Dateizwischenspeicherung entsprechend ein, je nachdem, ob Sie SAP MaxDB-Daten- oder Protokollvolumes (d.h. Dateien) verwenden, und abhängig davon, ob Sie Azure Standard oder Azure Storage Premium nutzen.
* Solange die bestehenden IOPS-Kontingente pro Datenträger den Anforderungen genügen, ist es möglich, alle Datenvolumes auf einer einzigen, eingebundenen Azure-VHD und alle Protokollvolumes der Datenbank auf einer einzigen anderen, eingebundenen Azure-VHD zu speichern.
* Wenn mehr IOPS und oder Speicherplatz erforderlich sind, wird ausdrücklich empfohlen, Microsoft Windows-Speicherpools (nur verfügbar unter Microsoft Windows Server 2012 und höher) oder Microsoft Windows-Striping für Microsoft Windows 2008 R2 zu verwenden, um ein einzelnes großes, logisches Gerät aus mehreren eingebundenen VHD-Datenträgern zu erstellen. Weitere Informationen finden Sie in diesem Dokument im Kapitel [Software-RAID][dbms-guide-2.2]. Durch diese Herangehensweise wird der Aufwand verringert, der zur Verwaltung des Speicherplatzes notwendig ist. Außerdem müssen Dateien nicht mehr manuell auf mehrere eingebundene VHDs verteilt werden.
* Für besonders hohe IOPS-Anforderungen steht Azure Storage Premium zur Verfügung, das für VMs der DS-Serie und der GS-Serie angeboten wird.

![Referenzkonfiguration der Azure-IaaS-VM für SAP MaxDB-DBMS][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Sichern und Wiederherstellen
Bei der Bereitstellung von SAP MaxDB in Azure ist es erforderlich, die Sicherungsmethode zu überprüfen. Auch wenn es sich bei dem System nicht um ein Produktionssystem handelt, muss die durch SAP MaxDB gehostete SAP-Datenbank regelmäßig gesichert werden. Da Azure Storage drei Images vorhält, ist die Sicherung, um einen etwaigen Speicherabsturz und vor allem Betriebsausfälle und Verwaltungsfehler kompensieren zu können, weniger dringlich. Der wichtigste Grund, einen ordnungsgemäßen Sicherungs- und Wiederherstellungsplan zu verfolgen, besteht darin, dass Sie mithilfe der Point-in-Time-Wiederherstellung logische und manuelle Fehler beheben können. Das Ziel ist also entweder, Sicherungen für die Wiederherstellung des Zustands der Datenbank zu einem bestimmten Zeitpunkt zu erstellen, oder, mithilfe einer Sicherung in Azure durch Seeding ein anderes System aufzusetzen, indem die bestehende Datenbank kopiert wird. Beispielsweise ließe sich eine Umstellung von einer 2-stufigen SAP-Konfiguration auf ein 3-stufiges Systemsetup desselben Systems durch Wiederherstellung einer Sicherung umsetzen.

Sicherung und Wiederherstellung einer Datenbank in Azure funktionieren wie bei lokalen Umgebungen. Sie können also die standardmäßigen Tools in SAP MaxDB zum Sichern und Wiederherstellen verwenden. Diese werden in einer SAP MaxDB-Dokumentation beschrieben, die Sie in SAP-Hinweis [767598] aufgeführt finden.

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung
Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen Datenbankgeräte gespeichert sind, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Ziele (Stripeset-Verzeichnisse, VHDs), auf die die Sicherung geschrieben wird, desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Getrennte, dedizierte Volumes für das Sichern
* Striping des Zielvolumes der Sicherung auf mehrere bereitgestellte VHDs, wodurch der IOPS-Durchsatz auf den betreffenden Datenträgervolumes verbessert wird
* Abgegrenzte, dedizierte logische Datenträger für:
    * SAP MaxDB-Sicherungsvolumes (also Dateien)
    * SAP MaxDB-Datenvolumes (d. h. Dateien)
    * SAP MaxDB-Protokollvolumes (gleich Dateien)

Informationen zum Striping eines Volumes auf mehrere bereitgestellte VHDs finden Sie in diesem Dokument im Kapitel [Software-RAID][dbms-guide-2.2].

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Sonstige
Bezüglich aller weiteren allgemeinen Themen wie Azure-Verfügbarkeitsgruppen oder SAP-Überwachung gelten die Angaben in den ersten drei Kapiteln dieses Dokuments auch für Bereitstellungen von VMs mit der SAP MaxDB-Datenbank. Andere SAP MaxDB-spezifische Einstellungen sind für Azure-VMs transparent. Diese sind in verschiedenen Dokumenten beschrieben, die in SAP-Hinweis [767598] und den folgenden SAP-Hinweisen aufgelistet sind:

* [826037]
* [1139904]
* [1173395]

## Einzelheiten zu SAP liveCache unter Windows

### Versionsunterstützung für SAP liveCache
SAP liveCache wird in Azure Virtual Machines ab der Version **SAP LC/LCAPPS 10.0 SP 25** unterstützt. Dies schließt **liveCache 7.9.08.31** und **LCA-Build 25** ein, veröffentlicht für **EhP 2 für SAP SCM 7.0** und höher.

### Unterstützte Microsoft Windows-Versionen und Azure-VM-Typen für SAP liveCache-DBMS
Die unterstützten Microsoft Windows-Versionen für SAP liveCache auf Azure finden Sie unter:

* [SAP-Produktverfügbarkeitsmatrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird ausdrücklich empfohlen, stets die neueste Version des Betriebssystems Microsoft Windows zu verwenden. Derzeit ist dies Windows 2012 R2.

### SAP liveCache-Konfigurationsrichtlinien für SAP-Installationen in Azure-VMs

#### Empfohlene Typen der Azure-VM
Da es sich bei SAP liveCache um eine Anwendung handelt, die für umfangreiche Berechnungen vorgesehen ist, wird die Leistung von SAP liveCache maßgeblich durch die Geschwindigkeit von RAM und CPU bestimmt.

Bei den von SAP unterstützten Typen der Azure-VM (siehe SAP-Hinweis [1928533]) werden alle virtuellen CPU-Ressourcen von dedizierten physischen CPU-Ressourcen des Hypervisors gestützt. Eine Überversorgung (und damit eine Konkurrenz um CPU-Ressourcen) findet nicht statt.

Analog wird bei den von SAP unterstützten Typen der Azure-VM der gesamte VM-Arbeitsspeicher dem physischen Arbeitsspeicher zugeordnet. Es erfolgt z.B. keine Überbereitstellung (Over-Commitment).

Aus dieser Perspektive ist besonders empfehlenswert, die neue D-Serie oder DS-Serie (in Verbindung mit Azure Storage Premium) der Azure-VM zu verwenden, da die Prozessoren 60 % schneller als die der A-Serie sind. Für die höchste RAM- und CPU-Last stehen die virtuellen Computer der G-Serie oder GS-Serie (in Kombination mit Azure Storage Premium) bereit, die über die aktuelle Intel® Xeon®-Prozessorfamilie (E5 v3) verfügen und die im Vergleich zu den D/DS-Serien das Doppelte an Arbeitsspeicher und das Vierfache an SSD-Speicher (Festkörperlaufwerk) bieten.

#### Speicherkonfiguration
Da SAP liveCache auf SAP MaxDB-Technologie basiert, gelten alle bewährten Methoden hinsichtlich Azure-Speicher, wie sie im Kapitel [Speicherkonfiguration][dbms-guide-8.4.1] für SAP MaxDB dargestellt werden, auch für SAP liveCache.

#### Dedizierte Azure-VM für liveCache
Da SAP liveCache viel an Rechenleistung erfordert, wird im Sinne des produktiven Nutzens nachdrücklich empfohlen, einen dedizierten, virtuellen Azure-Computer bereitzustellen.
 
![Dedizierte Azure-VM für liveCache im Anwendungsfall für produktiven Nutzen][dbms-guide-figure-700]

#### Sichern und Wiederherstellen
Das Sichern und Wiederherstellen wurde, Überlegungen zur Leistung eingeschlossen, bereits in den entsprechenden SAP MaxDB-Kapiteln [Sichern und Wiederherstellen][dbms-guide-8.4.2] und [Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung][dbms-guide-8.4.3] beschrieben.

#### Andere
Alle anderen, allgemeinen Themen wurden bereits in [diesem][dbms-guide-8.4.4] SAP MaxDB-Kapitel besprochen.

## Einzelheiten zum SAP Content Server unter Windows
Der SAP Content Server ist eine separate, serverbasierte Komponente zum Speichern von Inhalt wie elektronischen Dokumenten in verschiedenen Formaten. Der SAP Content Server ist eine technologische Errungenschaft und kann für alle SAP-Anwendungen verwendet werden. Er wird auf einem gesonderten System installiert. Typischer Inhalt sind z.B. Schulungsmaterialien und Dokumentationen des Knowledge Warehouse oder technische Zeichnungen aus dem mySAP PLM Document Management System.

### Versionsunterstützung für SAP Content Server
Momentan unterstützt SAP:

* **SAP Content Server** mit Version **6.50 (und höher)**
* **SAP MaxDB Version 7.9**
* **Microsoft IIS (Internet Information Server) Version 8.0 (und höher)**

Es wird nachdrücklich empfohlen, den SAP Content Server stets in der neuesten Version zu verwenden. Zum Zeitpunkt des Abfassens dieses Dokuments ist dies **6.50 SP4** mit der neuesten Version von **Microsoft IIS 8.5**.

Die neuesten unterstützten Versionen von SAP Content Server und Microsoft IIS sind in der [SAP-Produktverfügbarkeitsmatrix (Product Availability Matrix, PAM)][sap-pam] aufgelistet.

### Unterstützte Microsoft Windows-Versionen und Azure-VM-Typen für SAP Content Server
Die für den SAP Content Server auf Azure unterstützten Windows-Versionen finden Sie hier:

* [SAP-Produktverfügbarkeitsmatrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird nachdrücklich empfohlen, Microsoft Windows stets in der neuesten Version zu verwenden. Zum Zeitpunkt des Abfassens dieses Dokuments ist dies **Windows Server 2012 R2**.

### SAP Content Server-Konfigurationsrichtlinien für SAP-Installationen in Azure-VMs

#### Speicherkonfiguration 
Wenn Sie SAP Content Server so konfigurieren, dass Dateien in der SAP MaxDB-Datenbank gespeichert werden, gelten alle bewährten Methoden für Azure-Speicher, die im Kapitel [Speicherkonfiguration][dbms-guide-8.4.1] für SAP MaxDB beschrieben wurden, auch für das SAP Content Server-Szenario.

Konfigurieren Sie SAP Content Server so, dass Dateien im Dateisystem gespeichert werden, empfiehlt es sich, ein dediziertes logisches Laufwerk zu verwenden. Wenn Sie Speicherplätze nutzen, können Sie auch die logische Datenträgergröße und den IOPS-Durchsatz erhöhen, wie im Kapitel [Software-RAID][dbms-guide-2.2] beschrieben wird.

#### Speicherort für den SAP Content Server
SAP Content Server muss in derselben Azure-Region und im selben Azure-VNET wie das SAP-System bereitgestellt werden. Sie haben die Möglichkeit, die SAP Content Server-Komponenten auf einer dedizierten Azure-VM oder auf derselben VM wie das SAP-System zu installieren.
 
![Dedizierte Azure-VM für SAP Content Server][dbms-guide-figure-800]

#### Speicherort für den SAP Cache Server
Der SAP Cache Server ist eine zusätzliche, serverbasierte Komponente für den lokalen Zugriff auf (zwischengespeicherte) Dokumente. Der SAP Cache Server speichert die Dokumente auf einem SAP Content Server zwischen. Dies geschieht, um für den Fall, dass Dokumente mehr als einmal von verschiedenen Speicherorten abgerufen werden müssen, den Netzwerkdatenverkehr zu optimieren. Allgemein gilt, dass sich der SAP Cache Server in physischer Nähe zu dem Client befinden muss, der auf den SAP Cache Server zugreift.

Hier haben Sie zwei Möglichkeiten:

1. **Client ist ein Back-End-SAP-System** Falls ein Back-End-SAP-System für den Zugriff auf den SAP Content Server konfiguriert wurde, ist dieses SAP-System ein Client. Da sowohl das SAP-System als auch der SAP Content Server in derselben Azure-Region –im selben Datencenter – bereitgestellt werden, befinden sie sich in physischer Nähe. Es besteht also keine Veranlassung für einen dedizierten SAP Cache Server. SAP UI-Clients (SAP-GUI oder Webbrowser) haben direkten Zugriff auf das SAP-System, während das SAP-System Dokumente vom SAP Content Server abruft.
1. **Client ist ein lokaler Webbrowser** Der SAP Content Server lässt sich so konfigurieren, dass der Zugriff direkt über den Webbrowser erfolgen kann. In diesem Fall ist ein lokal ausgeführter Webbrowser ein Client des SAP Content Server. Das lokale Datencenter und das Azure-Datencenter befinden sich an unterschiedlichen physischen Standorten (idealerweise nicht weit voneinander entfernt). Ihr lokales Datencenter ist per Azure-Site-to-Site-VPN oder ExpressRoute mit Azure verbunden. Beide Optionen bieten eine sichere VPN-Netzwerkverbindung mit Azure. Die Site-to-Site-Verbindung verfügt allerdings nicht über Netzwerkbandbreite und Latenz-SLA zwischen dem lokalen Datencenter und dem Azure-Datencenter. Um den Zugriff auf Dokumente zu beschleunigen, können Sie eine der folgenden Maßnahmen ergreifen:
    1. Lokale Installation des SAP Cache Server in der Nähe zum lokalen Webbrowser (Option in [dieser][dbms-guide-900-sap-cache-server-on-premises] Abbildung)
    1. Konfigurieren von Azure ExpressRoute, das hohe Geschwindigkeit und niedrige Wartezeit mit einer dedizierten Netzwerkverbindung zwischen dem lokalen Datencenter und dem Azure-Datencenter bietet
 
![Option zur lokalen Installation des SAP Cache Server][dbms-guide-figure-900] <a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### Sichern und Wiederherstellen
Wenn Sie den SAP Content Server für das Speichern von Dateien in der SAP MaxDB-Datenbank konfigurieren, gelten die Angaben zum Speichern und Wiederherstellen sowie die Leistungsüberlegungen aus den Kapiteln zu SAP MaxDB [Sichern und Wiederherstellen][dbms-guide-8.4.2] und [Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung][dbms-guide-8.4.3].

Konfigurieren Sie den SAP Content Server so, dass Dateien im Dateisystem gespeichert werden, haben Sie die Option, das Sichern und Wiederherstellen der gesamten Dateistruktur am Speicherort der Dokumente auszuführen. Ähnlich dem Sichern und Wiederherstellen bei SAP MaxDB wird empfohlen, für Sicherungszwecke ein dediziertes Datenträgervolume zur Verfügung zu halten.

#### Andere
Andere, für den SAP Content Server spezifische Einstellungen sind für Azure-VMs transparent. Sie werden in verschiedenen Dokumenten und SAP-Hinweisen beschrieben:

* <https://service.sap.com/contentserver>
* SAP-Hinweis [1619726]

## Besonderheiten bei IBM DB2 für LUW unter Windows
Mit Microsoft Azure können Sie Ihre bestehende SAP-Anwendung unter IBM DB2 für Linux, UNIX und Windows (LUW) einfach zu virtuellen Azure-Computern migrieren. Bei SAP unter IBM DB2 für LUW stehen Administratoren und Entwicklern die vertrauten Entwicklungs- und Verwaltungstools aus der lokalen Umgebung zur Verfügung. Allgemeine Informationen zum Ausführen der SAP Business Suite unter IBM DB2 für LUW finden Sie im SAP Community Network (SCN) unter <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Weitere Informationen und Updates zu SAP unter DB2 für LUW auf Azure erhalten Sie in SAP-Hinweis [2233094].

### Versionsunterstützung für IBM DB2 für Linux, UNIX und Windows
SAP unter IBM DB2 für LUW auf Microsoft Azure Virtual Machine Services wird seit DB2 Version 10.5 unterstützt.

Informationen zu den unterstützten SAP-Produkten und Typen der Azure-VM erhalten Sie in SAP-Hinweis [1928533].

### Konfigurationsrichtlinien für IBM DB2 für Linux, UNIX und Windows für SAP-Installationen in Azure-VMs

#### Speicherkonfiguration
Sämtliche Datenbankdateien müssen auf dem NTFS-Dateisystem auf VHD-Datenträgern gespeichert werden. Diese VHDs werden in eine Azure-VM eingebunden. Sie basieren auf Azure-Seitenblobspeicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Jegliche Arten von Netzlaufwerken und Remotefreigaben wie die folgenden Azure-Dateidienste werden für Datenbankdateien **NICHT** unterstützt:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>
 
Die Aussagen, die in diesem Dokument im Kapitel [Struktur einer RDBMS-Bereitstellung][dbms-guide-2] gemacht wurden, gelten auch für Bereitstellungen mit Datenbanken basierend auf IBM DB2 für LUW, wenn Azure-VHDs auf Basis von Azure-Seitenblobspeicher verwendet werden.

Wie weiter oben im allgemeinen Teil dieses Dokuments erläutert wurde, gelten für Azure-VHDs Kontingente für den IOPS-Durchsatz. Die jeweilige Größe der Kontingente hängt vom Typ der verwendeten VM ab. Eine Liste mit VM-Typen und den entsprechenden Kontingenten finden Sie [hier][virtual-machines-sizes].

Solange das bestehende IOPS-Kontingent pro Datenträger ausreicht, ist es möglich, alle Datenbankdateien auf einer einzelnen eingebundenen Azure-VHD zu speichern.

Überlegungen zur Leistung finden Sie auch im Kapitel „Data Safety and Performance Considerations for Database Directories“ (Aspekte der Datensicherheit und Leistung bei Datenbankverzeichnissen) in den SAP-Installationshandbüchern.

Alternativ können Sie Windows-Speicherpools (nur verfügbar unter Windows Server 2012 und höher) oder Windows-Striping für Windows 2008 R2 verwenden, wie im Kapitel [Software-RAID][dbms-guide-2.2] in diesem Dokument beschrieben wird, um ein einzelnes großes, logisches Gerät aus mehreren eingebundenen VHD-Datenträgern zu erstellen. Legen Sie für die Datenträger, die die DB2-Speicherpfade für die Verzeichnisse „sapdata“ und „saptmp“ enthalten, eine physische Datenträgersektorgröße von 512 KB fest. Wenn Sie Windows-Speicherpools verwenden, müssen die Speicherpools über die Befehlszeilenschnittstelle manuell erstellt werden. Der Parameter hierfür lautet „-LogicalSectorSizeDefault“. Einzelheiten finden Sie hier: <https://technet.microsoft.com/library/hh848689.aspx>

#### Sichern und Wiederherstellen
Die Funktionen zum Sichern und Wiederherstellen werden für IBM DB2 für LUW genauso unterstützt wie auf standardmäßigen Windows Server-Betriebssystemen und Hyper-V.

Eine gültige Sicherungsstrategie für die Datenbank ist unerlässlich.

Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit nur bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen sich Datenbankgeräte befinden, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Ziele (Stripeset-Verzeichnisse, VHDs), auf die die Sicherung geschrieben wird, desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Striping des Zielvolumes der Sicherung auf mehrere bereitgestellte VHDs, wodurch der IOPS-Durchsatz auf den betreffenden Volumes verbessert wird
* Verwenden von mehr als einem Zielverzeichnis für das Sichern

#### Hohe Verfügbarkeit und Notfallwiederherstellung
Microsoft Cluster Server (MSCS) wird nicht unterstützt.

Hohe Verfügbarkeit und Notfallwiederherstellung (HADR) in DB2 wird unterstützt. Wenn die an der Hochverfügbarkeitskonfiguration teilnehmenden virtuellen Computer über eine funktionierende Namensauflösung verfügen, unterscheidet sich die Einrichtung in Azure nicht von anderen, lokal ausgeführten Einrichtungen. Es wird nicht empfohlen, ausschließlich die IP-Auflösung zugrunde zu legen.

Verwenden Sie nicht die Azure Store-Georeplikation. Weitere Informationen erhalten Sie in den Kapiteln [Microsoft Azure Storage][dbms-guide-2.3] und [Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern][dbms-guide-3].

#### Andere
Bezüglich aller weiteren allgemeinen Themen wie Azure-Verfügbarkeitsgruppen oder SAP-Überwachung gelten die Angaben in den ersten drei Kapiteln dieses Dokuments auch für Bereitstellungen von VMs mit IBM DB2 für LUW.

Weitere Informationen erhalten Sie auch im Kapitel [SQL Server für SAP in Azure – Allgemeine Zusammenfassung][dbms-guide-5.8].

## Besonderheiten bei IBM DB2 für LUW unter Linux
Mit Microsoft Azure können Sie Ihre bestehende SAP-Anwendung unter IBM DB2 für Linux, UNIX und Windows (LUW) einfach zu virtuellen Azure-Computern migrieren. Bei SAP unter IBM DB2 für LUW stehen Administratoren und Entwicklern die vertrauten Entwicklungs- und Verwaltungstools aus der lokalen Umgebung zur Verfügung. Allgemeine Informationen zum Ausführen der SAP Business Suite unter IBM DB2 für LUW finden Sie im SAP Community Network (SCN) unter <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Weitere Informationen und Updates zu SAP unter DB2 für LUW auf Azure erhalten Sie in SAP-Hinweis [2233094].

### Versionsunterstützung für IBM DB2 für Linux, UNIX und Windows
SAP unter IBM DB2 für LUW auf Microsoft Azure Virtual Machine Services wird seit DB2 Version 10.5 unterstützt.

Informationen zu den unterstützten SAP-Produkten und Typen der Azure-VM erhalten Sie in SAP-Hinweis [1928533].

### Konfigurationsrichtlinien für IBM DB2 für Linux, UNIX und Windows für SAP-Installationen in Azure-VMs

#### Speicherkonfiguration
Sämtliche Datenbankdateien müssen auf einem Dateisystem auf VHD-Datenträgern gespeichert werden. Diese VHDs werden in eine Azure-VM eingebunden. Sie basieren auf Azure-Seitenblobspeicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Jegliche Arten von Netzlaufwerken und Remotefreigaben wie die folgenden Azure-Dateidienste werden für Datenbankdateien **NICHT** unterstützt:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Die Aussagen, die in diesem Dokument im Kapitel [Struktur einer RDBMS-Bereitstellung][dbms-guide-2] gemacht wurden, gelten auch für Bereitstellungen mit Datenbanken basierend auf IBM DB2 für LUW, wenn Azure-VHDs auf Basis von Azure-Seitenblobspeicher verwendet werden.

Wie weiter oben im allgemeinen Teil dieses Dokuments erläutert wurde, gelten für Azure-VHDs Kontingente für den IOPS-Durchsatz. Die jeweilige Größe der Kontingente hängt vom Typ der verwendeten VM ab. Eine Liste mit VM-Typen und den entsprechenden Kontingenten finden Sie [hier][virtual-machines-sizes].

Solange das bestehende IOPS-Kontingent pro Datenträger ausreicht, ist es möglich, alle Datenbankdateien auf einer einzelnen eingebundenen Azure-VHD zu speichern.

Überlegungen zur Leistung finden Sie auch im Kapitel „Data Safety and Performance Considerations for Database Directories“ (Aspekte der Datensicherheit und Leistung bei Datenbankverzeichnissen) in den SAP-Installationshandbüchern.

Alternativ können Sie LVM (Logical Volume Manager) oder MDADM verwenden, wie im Kapitel [Software-RAID-][dbms-guide-2.2] dieses Dokuments beschrieben, um ein großes logisches Gerät aus mehreren bereitgestellten VHD-Datenträgern zu erstellen. Legen Sie für die Datenträger, die die DB2-Speicherpfade für die Verzeichnisse „sapdata“ und „saptmp“ enthalten, eine physische Datenträgersektorgröße von 512 KB fest.

#### Sichern und Wiederherstellen
Die Funktionen zum Sichern und Wiederherstellen werden für IBM DB2 für LUW genauso unterstützt wie auf standardmäßigen lokalen Linux-Installationen.

Eine gültige Sicherungsstrategie für die Datenbank ist unerlässlich.

Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit nur bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an VHDs, auf denen sich Datenbankgeräte befinden, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Ziele (Stripeset-Verzeichnisse, VHDs), auf die die Sicherung geschrieben wird, desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Striping des Zielvolumes der Sicherung auf mehrere bereitgestellte VHDs, wodurch der IOPS-Durchsatz auf den betreffenden Volumes verbessert wird
* Verwenden von mehr als einem Zielverzeichnis für das Sichern

#### Hohe Verfügbarkeit und Notfallwiederherstellung
Hohe Verfügbarkeit und Notfallwiederherstellung (HADR) in DB2 wird unterstützt. Wenn die an der Hochverfügbarkeitskonfiguration teilnehmenden virtuellen Computer über eine funktionierende Namensauflösung verfügen, unterscheidet sich die Einrichtung in Azure nicht von anderen, lokal ausgeführten Einrichtungen. Es wird nicht empfohlen, ausschließlich die IP-Auflösung zugrunde zu legen.

Verwenden Sie nicht die Azure Store-Georeplikation. Weitere Informationen erhalten Sie in den Kapiteln [Microsoft Azure Storage][dbms-guide-2.3] und [Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern][dbms-guide-3].

#### Andere
Bezüglich aller weiteren allgemeinen Themen wie Azure-Verfügbarkeitsgruppen oder SAP-Überwachung gelten die Angaben in den ersten drei Kapiteln dieses Dokuments auch für Bereitstellungen von VMs mit IBM DB2 für LUW.

Weitere Informationen erhalten Sie auch im Kapitel [SQL Server für SAP in Azure – Allgemeine Zusammenfassung][dbms-guide-5.8].

<!---HONumber=AcomDC_0907_2016-->
