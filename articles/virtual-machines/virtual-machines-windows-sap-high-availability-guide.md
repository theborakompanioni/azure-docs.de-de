<properties
   pageTitle="SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“ | Microsoft Azure"
   description="SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="goraco"
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
   ms.author="goraco"/>

# SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“

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

[sap-installation-guides]: http://service.sap.com/instguides

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
[planning-guide-11]: virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 "Hohe Verfügbarkeit (HA) und Disaster Recovery (DR) für SAP NetWeaver auf virtuellen Azure-Computern"
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

[sap-ha-guide]: virtual-machines-windows-sap-high-availability-guide.md "SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“"
[sap-ha-guide-1]: virtual-machines-windows-sap-high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c "Voraussetzungen"
[sap-ha-guide-2]: virtual-machines-windows-sap-high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08 "Ressourcen"
[sap-ha-guide-3]: virtual-machines-windows-sap-high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1 "Unterschiede bei der hohen Verfügbarkeit von SAP zwischen Azure Resource Manager und klassischem Bereitstellungsmodell"
[sap-ha-guide-3.1]: virtual-machines-windows-sap-high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686 "Ressourcengruppen"
[sap-ha-guide-3.2]: virtual-machines-windows-sap-high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860 "Erstellen eines Clusters mit dem Azure Resource Manager im Vergleich zum klassischen Bereitstellungsmodell"
[sap-ha-guide-4]: virtual-machines-windows-sap-high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7 "Erstellen eines Windows Server-Failoverclusters (WSFC)"
[sap-ha-guide-4.1]: virtual-machines-windows-sap-high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2 "Quorummodi"
[sap-ha-guide-5]: virtual-machines-windows-sap-high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275 "Lokaler Windows-Failovercluster"
[sap-ha-guide-5.1]: virtual-machines-windows-sap-high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592 "Freigegebener Speicher"
[sap-ha-guide-5.2]: virtual-machines-windows-sap-high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd "Netzwerk/Namensauflösung"
[sap-ha-guide-6]: virtual-machines-windows-sap-high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a "Windows-Failovercluster mit Microsoft Azure"
[sap-ha-guide-6.1]: virtual-machines-windows-sap-high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341 "Freigegebener Datenträger in Microsoft Azure mit SIOS DataKeeper"
[sap-ha-guide-6.2]: virtual-machines-windows-sap-high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb "Namensauflösung in Microsoft Azure"
[sap-ha-guide-7]: virtual-machines-windows-sap-high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa "Hohe Verfügbarkeit von SAP NetWeaver in Azure IaaS"
[sap-ha-guide-7.1]: virtual-machines-windows-sap-high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e "Hohe Verfügbarkeit für SAP-Anwendungsserver"
[sap-ha-guide-7.2]: virtual-machines-windows-sap-high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db "Hohe Verfügbarkeit für die SAP (A)SCS-Instanzen"
[sap-ha-guide-7.2.1]: virtual-machines-windows-sap-high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51 "Hohe Verfügbarkeit für SAP (A)SCS-Instanz mit Windows-Failovercluster in Azure"
[sap-ha-guide-7.3]: virtual-machines-windows-sap-high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027 "Hohe Verfügbarkeit für die DBMS-Instanz"
[sap-ha-guide-7.4]: virtual-machines-windows-sap-high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816 "Mögliche End-to-End-Bereitstellungsszenarien für hohe Verfügbarkeit"
[sap-ha-guide-8]: virtual-machines-windows-sap-high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf "Vorbereitung der Infrastruktur"
[sap-ha-guide-8.1]: virtual-machines-windows-sap-high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489 "Bereitstellen von VMs mit (standortübergreifender) Verbindung mit dem Unternehmensnetzwerk für den Produktiveinsatz"
[sap-ha-guide-8.2]: virtual-machines-windows-sap-high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310 "Reine Cloudbereitstellung von SAP-Instanzen zu Test-/Demozwecken"
[sap-ha-guide-8.3]: virtual-machines-windows-sap-high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a "Azure Virtual Network"
[sap-ha-guide-8.4]: virtual-machines-windows-sap-high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e "DNS-IP-Adressen"
[sap-ha-guide-8.5]: virtual-machines-windows-sap-high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f "Hostnamen und statische IP-Adressen für die SAP ASCS/SCS-Clusterinstanz und DBMS-Clusterinstanz"
[sap-ha-guide-8.6]: virtual-machines-windows-sap-high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30 "Einrichten statischer IP-Adressen für die SAP-VMs"
[sap-ha-guide-8.7]: virtual-machines-windows-sap-high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e "Einrichten der statischen IP-Adresse für internen Load Balancer (ILB)"
[sap-ha-guide-8.8]: virtual-machines-windows-sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c "Standardmäßige ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure"
[sap-ha-guide-8.9]: virtual-machines-windows-sap-high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716 "Ändern der standardmäßigen ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure"
[sap-ha-guide-8.10]: virtual-machines-windows-sap-high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c "Hinzufügen von Windows-Computern zur Domäne"
[sap-ha-guide-8.11]: virtual-machines-windows-sap-high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158 "Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten für eine SAP ASCS/SCS-Instanz"
[sap-ha-guide-8.12]: virtual-machines-windows-sap-high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab "Einrichten des Windows Server-Failoverclusters für SAP ASCS/SCS-Instanz"
[sap-ha-guide-8.12.1]: virtual-machines-windows-sap-high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79 "Erfassen von Clusterknoten in der Clusterkonfiguration"
[sap-ha-guide-8.12.2]: virtual-machines-windows-sap-high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca "Konfigurieren eines Cluster-Dateifreigabenzeugen"
[sap-ha-guide-8.12.2.1]: virtual-machines-windows-sap-high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855 "Erstellen einer Dateifreigabe"
[sap-ha-guide-8.12.2.2]: virtual-machines-windows-sap-high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d "Konfigurieren des Dateifreigabenzeugen-Quorums im Failovercluster-Manager"
[sap-ha-guide-8.12.3]: virtual-machines-windows-sap-high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868 "Installieren von SIOS DataKeeper Cluster Edition für SAP ASCS/SCS-Clusterfreigabe-Datenträger"
[sap-ha-guide-8.12.3.1]: virtual-machines-windows-sap-high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3 "Hinzufügen von Microsoft .NET Framework 3.5"
[sap-ha-guide-8.12.3.2]: virtual-machines-windows-sap-high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102 "Installieren von SIOS DataKeeper"
[sap-ha-guide-8.12.3.3]: virtual-machines-windows-sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 "Einrichten von SIOS DataKeeper"
[sap-ha-guide-9]: virtual-machines-windows-sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b "Installation des SAP NetWeaver-Systems"
[sap-ha-guide-9.1]: virtual-machines-windows-sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 "SAP-Installation mit hoch verfügbarer ASCS/SCS-Instanz"
[sap-ha-guide-9.1.1]: virtual-machines-windows-sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 "Erstellen eines virtuellen Hostnamens für die SAP ASCS/SCS-Clusterinstanz"
[sap-ha-guide-9.1.2]: virtual-machines-windows-sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 "Installieren des ersten SAP-Knotens des Clusters"
[sap-ha-guide-9.1.3]: virtual-machines-windows-sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 "Ändern des SAP-Profils der ASCS/SCS-Instanz"
[sap-ha-guide-9.1.4]: virtual-machines-windows-sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 "Hinzufügen eines Testports"
[sap-ha-guide-9.2]: virtual-machines-windows-sap-high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7 "Installieren der Datenbankinstanz"
[sap-ha-guide-9.3]: virtual-machines-windows-sap-high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748 "Installation des zweiten Clusterknotens"
[sap-ha-guide-9.4]: virtual-machines-windows-sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a "Ändern des Starttyps des Windows-Diensts der SAP ERS-Instanz"
[sap-ha-guide-9.5]: virtual-machines-windows-sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 "Installation des primären Anwendungsservers von SAP"
[sap-ha-guide-9.6]: virtual-machines-windows-sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 "Installation des zusätzlichen Anwendungsservers von SAP"
[sap-ha-guide-10]: virtual-machines-windows-sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9 "Testen des Failovers der SAP ASCS/SCS-Instanz und der Replikation von SIOS"
[sap-ha-guide-10.1]: virtual-machines-windows-sap-high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445 "Ausgangspunkt – Die SAP ASCS/SCS-Instanz wird auf Clusterknoten A ausgeführt"
[sap-ha-guide-10.2]: virtual-machines-windows-sap-high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916 "Failoverprozess von Knoten A auf Knoten B"
[sap-ha-guide-10.3]: virtual-machines-windows-sap-high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5 "Endergebnis – Die SAP ASCS/SCS-Instanz wird auf Clusterknoten B ausgeführt"


[sap-ha-guide-figure-1000]: ./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]: ./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]: ./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]: ./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]: ./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]: ./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]: ./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-3000]: ./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]: ./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]: ./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]: ./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]: ./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]: ./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]: ./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]: ./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]: ./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]: ./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]: ./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]: ./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]: ./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]: ./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]: ./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]: ./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]: ./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]: ./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]: ./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]: ./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]: ./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]: ./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]: ./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]: ./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]: ./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]: ./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]: ./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]: ./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]: ./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]: ./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]: ./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]: ./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]: ./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]: ./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]: ./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]: ./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]: ./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]: ./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]: ./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]: ./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]: ./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]: ./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]: ./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]: ./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]: ./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]: ./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]: ./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]: ./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]: ./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]: ./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]: ./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]: ./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]: ./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]: ./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]: ./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png


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
[virtual-machines-azure-resource-manager-architecture-benefits-arm]: ../resource-manager-deployment-model.md#benefits-of-using-resource-manager-and-resource-groups
[virtual-machines-azurerm-versus-azuresm]: virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]: virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]: virtual-machines-linux-cli-deploy-templates.md "Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI"
[virtual-machines-deploy-rmtemplates-powershell]: virtual-machines-windows-ps-manage.md "Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager und PowerShell"
[virtual-machines-linux-agent-user-guide]: virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]: virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]: virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]: virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]: virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]: virtual-machines-windows-capture-image.md#capture-the-vm
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
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]: virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]: virtual-machines-windows-portal-sql-alwayson-int-listener.md
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
 

Microsoft Azure ermöglicht Unternehmen das Abrufen von Compute-, Speicher- und Netzwerkressourcen in kürzester Zeit ohne langwierige Beschaffungszyklen. Virtuelle Azure-Computer ermöglichen Unternehmen das Bereitstellen klassischer Anwendungen, wie z.B. SAP NetWeaver-basierter Anwendungen (ABAP-, Java- und ABAP+Java-Stack), in Azure und das Erweitern ihrer Zuverlässigkeit und Verfügbarkeit, ohne dass weitere Ressourcen lokal verfügbar sind. Virtuelle Azure-Computer unterstützen auch standortübergreifende Verbindungen, wodurch Unternehmen virtuelle Azure-Computer aktiv in ihre lokalen Domänen, privaten Clouds und ihre SAP-Systemlandschaft integrieren können.


In diesem Dokument werden alle Schritte detailliert beschrieben, die zum Bereitstellen hoch verfügbarer SAP-Systeme in Azure mithilfe unserer neuen Methode mit dem neuen Azure Resource Manager-Bereitstellungsmodell erforderlich sind. Wir gehen in diesem Handbuch die wichtigsten Schritte gemeinsam durch:


- Auffinden der entsprechenden SAP-Installationshandbücher und Hinweise, die weiter unten im Abschnitt [Ressourcen][sap-ha-guide-2] aufgelistet werden.  
  Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

- Grundlegendes zum Unterschied zwischen dem aktuellen klassischen Azure-Bereitstellungsmodell und dem neuen Azure Resource Manager-Bereitstellungsmodell

- Grundlegendes zu den WSFC-Quorummodi (Windows Server-Failovercluster), damit Sie das für Ihre Azure-Bereitstellung geeignete Modell wählen können

- Grundlegendes zu freigegebenem WSFC-Speicher in Azure

- Grundlegendes dazu, wie Single Point of Failure-Komponenten von SAP wie SAP ASCS/SCS und DBMS sowie redundante Komponenten wie SAP-Anwendungsserver in Azure geschützt werden können

- Detaillierte Vorgehensweise zum Installieren und Konfigurieren eines hoch verfügbaren SAP-Systems in einem Windows Server-Failovercluster (WSFC) mit Microsoft Azure als Plattform und dem neuen Azure Resource Manager

- Zusätzliche für ein WSFC in Azure erforderliche Schritte, die in lokalen Bereitstellungen nicht erforderlich sind


Um die Bereitstellung und Konfiguration zu vereinfachen, verwenden wir die neuen Vorlagen des Typs „SAP 3 Tier HA Azure Resource Manager“, die die Bereitstellung der gesamten Infrastruktur automatisieren. Diese Infrastruktur ist für hoch verfügbare SAP-Systeme erforderlich und unterstützt die gewünschte SAPS-Größenanpassung Ihres SAP-Systems.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

##  <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Voraussetzungen

Achten Sie, ehe Sie beginnen, darauf, dass alle in den folgenden Kapiteln beschriebenen Voraussetzungen erfüllt sind und Sie alle im Kapitel „Ressourcen“ aufgelisteten Ressourcen überprüft haben.

Wir verwenden Azure Resource Manager-Vorlagen für SAP NetWeaver mit 3 Schichten: [https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/)

Einen Überblick über die Azure Resource Manager-Vorlagen für SAP finden Sie hier: [https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)


##  <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Ressourcen

Die folgenden zusätzlichen Handbücher zum Thema SAP-Bereitstellungen in Azure sind verfügbar:

- [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch][planning-guide]
- [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Bereitstellungshandbuch][deployment-guide]
- [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – DBMS-Bereitstellungshandbuch][dbms-guide]
- [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“ (dieses Handbuch)][sap-ha-guide]


> [AZURE.NOTE] Sofern möglich, wird ein Link zum entsprechenden SAP-Installationshandbuch verwendet (siehe [SAP-Installationshandbücher][sap-installation-guides]). Wenn es um die Voraussetzungen und den Installationsvorgang geht, sollten Sie die SAP NetWeaver-Installationshandbücher immer sorgfältig lesen, da dieses Dokument nur spezielle Aufgaben für SAP NetWeaver-Systeme abdeckt, die auf einem virtuellen Microsoft Azure-Computer installiert sind.

Die folgenden SAP-Hinweise beziehen sich auf das Thema SAP in Azure:


| Hinweisnummer | Titel                                                    
| ------------- |----------------------------------------------------------
| [1928533] | SAP-Anwendungen in Azure: unterstützte Produkte und Größen 
| [2015553] | SAP on Microsoft Azure: Support Prerequisites (SAP in Microsoft Azure: Voraussetzungen für die Unterstützung)         
| [1999351] | Enhanced Azure Monitoring for SAP (Erweiterte Azure-Überwachung für SAP)                        
| [2178632] | Wichtige Überwachungsmetriken für SAP in Microsoft Azure        
| [1999351] | Virtualisierung unter Windows: erweiterte Überwachung           


Allgemeine standardmäßige und maximale Einschränkungen von Azure-Abonnements finden Sie in [diesem Artikel][azure-subscription-service-limits-subscription].

##  <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a> Unterschiede bei der hohen Verfügbarkeit von SAP zwischen Azure Resource Manager und klassischem Bereitstellungsmodell 

> [AZURE.NOTE] Das klassische Bereitstellungsmodell ist auch als Azure Service Management-Modell (ASM) bekannt.

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Ressourcengruppen
Ressourcengruppen sind ein neues Konzept. Sie enthalten jeweils alle Ressourcen mit dem gleichen Lebenszyklus, da sie z.B. zur gleichen Zeit erstellt und gelöscht werden. Weitere Informationen über Ressourcengruppen finden Sie in diesem Artikel.

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Erstellen eines Clusters mit dem Azure Resource Manager im Vergleich zum klassischen Bereitstellungsmodell 

Das neue Azure Resource Manager-Modell weist im Vergleich mit dem klassischen Bereitstellungsmodell für hohe Verfügbarkeit die folgenden Änderungen auf:

- Es ist kein Clouddienst erforderlich, der einen internen Load Balancer (ILB) von Azure verwendet.

Wenn Sie weiterhin das alte klassische Azure-Modell verwenden möchten, müssen Sie die Schritte ausführen, die in diesem Artikel beschrieben werden: [SAP NetWeaver in Azure – Clustering von SAP ASCS/SCS-Instanzen mithilfe von Windows Server-Failoverclustern in Azure mit SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [AZURE.NOTE] Es wird ausdrücklich empfohlen, das neue Azure Resource Manager-Bereitstellungsmodell für Ihre SAP-Installationen zu verwenden, da es im Vergleich mit dem klassischen Bereitstellungsmodell viele Vorteile bietet. Weitere Informationen finden Sie in [diesem Artikel][virtual-machines-azure-resource-manager-architecture-benefits-arm].


## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Erstellen eines Windows Server-Failoverclusters (WSFC) 

Ein Microsoft WSFC ist die technische Grundlage einer hoch verfügbaren SAP ASCS/SCS-Installation samt DBMS unter Windows.

Bei einem Failovercluster handelt es sich um eine Gruppe von 1+n unabhängigen Servern, die zur Steigerung der Verfügbarkeit von Anwendungen und Diensten zusammenarbeiten. Bei einem Knotenausfall muss WSFC die Anzahl von Ausfällen bestimmen, die erfolgen darf, um weiter einen funktionierenden Cluster zur Verfügung zu stellen, der die angegebenen Anwendungen und/oder Dienste bereitstellen kann. Verschiedene Quorummodi sind verfügbar, um dies zu erreichen.
 

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorummodi

WSFC bietet verschiedene Quorummodi:

- **Knotenmehrheit:** Jeder Knoten kann abstimmen. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte. Diese Option wird bei einer ungeraden Anzahl von Knoten empfohlen. Beispiel: Wenn in einem Cluster mit 7 Knoten 3 Knoten ausfallen, kann der Cluster weiterhin die Mehrheit erreichen und ausgeführt werden.

- **Knoten- und Datenträgermehrheit:** Alle Knoten plus ein zugewiesener Datenträger im Clusterspeicher (der sog. „Datenträgerzeuge“) können abstimmen, solange sie verfügbar sind und kommunizieren. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte. Dieser Modus ist in einer Clusterumgebung mit einer geraden Anzahl von Knoten sinnvoll. Solange die Hälfte der Knoten sowie der Datenträgerzeuge online sind, verbleibt der Cluster in einem ordnungsgemäßen Zustand.

- **Knoten- und Dateifreigabemehrheit:** Alle Knoten sowie eine vom Administrator erstellte zugewiesene Dateifreigabe (der „Dateifreigabenzeuge“) können abstimmen, sofern sie verfügbar sind und kommunizieren. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte. Dieser Modus ist in einer Clusterumgebung mit einer geraden Anzahl von Knoten sinnvoll und vergleichbar mit dem Modus „Knoten- und Datenträgermehrheit“, wobei eine Zeugendateifreigabe anstelle eines Zeugendatenträgers verwendet wird. Die Implementierung ist einfach, doch wenn die Dateifreigabe selbst nicht hoch verfügbar ist, kann diese Lösung ein Single Point of Failure (SPOF) sein.

- **Keine Mehrheit – nur Datenträger:** Der Cluster besitzt das Quorum, wenn ein Knoten verfügbar ist und mit einem bestimmten Datenträger im Clusterspeicher kommuniziert. Nur die Knoten, die auch mit diesem Datenträger kommunizieren, können dem Cluster beizutreten. Dieser Modus wird nicht empfohlen.  

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Lokaler Windows-Failovercluster

Bei diesem Beispiel haben wir einen Cluster mit zwei Knoten. Wenn die Netzwerkverbindung zwischen den Knoten ausfällt, während beide Knoten in Betrieb bleiben, muss geklärt werden, welcher Knoten dem Cluster die Anwendungen und Dienste zur Verfügung stellen soll. Ein Quorumdatenträger oder eine Quorumdateifreigabe erfüllen diesen Zweck. Der Knoten, der Zugriff auf den Quorumdatenträger oder die Quorumdateifreigabe hat, ist derjenige, der den Zugriff auf die Dienste gewährleistet.

Bei diesem Beispiel wird ein Cluster mit zwei Knoten verwendet. Aus diesem Grund haben wir uns für den Quorummodus „Knoten- und Dateifreigabemehrheit“ entschieden. Der Modus „Knoten- und Datenträgermehrheit“ ist auch eine mögliche Option. In einer Produktivumgebung wird empfohlen, mit einem Quorumdatenträger zu arbeiten und Netzwerk- und Speichersystemtechnologie einzusetzen, um für hohe Verfügbarkeit zu sorgen.

![Abbildung 1: Vorgeschlagene Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure][sap-ha-guide-figure-1000]

_**Abbildung 1:** Vorgeschlagene Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Freigegebener Speicher

Die obige Abbildung zeigt einen Cluster mit freigegebenem Speicher mit zwei Knoten. In einem lokalen Cluster mit freigegebenem Speicher gibt es freigegebenen Speicher, der für alle Knoten im Cluster sichtbar ist. Mithilfe eines Sperrmechanismus werden die Daten vor Beschädigung geschützt. Darüber hinaus können alle Knoten erkennen, wenn ein anderer Knoten ausfällt. Wenn ein Knoten ausfällt, übernehmen die restlichen den Besitz der Speicherressourcen und stellen die Verfügbarkeit der Dienste sicher.

> [AZURE.NOTE] Zum Erzielen von hoher Verfügbarkeit mit verschiedenen DBMS, wie z.B. SQL Server, sind keine freigegebenen Datenträger erforderlich. SQL Server Always On führt die Replikation von DBMS-Daten- und -Protokolldateien vom lokalen Datenträger eines Clusterknotens auf den lokalen Datenträger eines anderen Clusterknotens durch. Daher ist bei der Windows-Clusterkonfiguration kein freigegebener Datenträger erforderlich.

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Netzwerk/Namensauflösung

Der Cluster ist über eine virtuelle IP-Adresse und einen virtuellen Hostnamen erreichbar, der vom DNS-Server bereitgestellt wird. Die lokalen Knoten und der DNS-Server können mehrere IP-Adressen verwenden.

Bei einer normalen Installation werden zwei oder mehr Netzwerkverbindungen verwendet:

- Eine dedizierte Verbindung mit dem Speicher und
- Eine clusterinterne Netzwerkverbindung für den Takt und
- Ein von den Clients verwendetes öffentliches Netzwerk zum Herstellen der Verbindung mit dem Cluster



## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows-Failovercluster mit Microsoft Azure

Im Vergleich mit Bare-Metal- oder privaten Cloudbereitstellungen sind für virtuelle Microsoft Azure-Computer zusätzliche Schritte erforderlich, um einen WSFC zu konfigurieren. Um einen freigegebenen Clusterdatenträger zu erstellen, sind mehrere IP-Adressen und virtuelle Hostnamen für die SAP ASCS/SCS-Instanz erforderlich.

Nachstehend werden die zusätzlichen Konzepte und Schritte erörtert, die erforderlich sind, wenn in Microsoft Azure ein SAP HA Central Services-Cluster erstellt wird. In den folgenden Schritten wird gezeigt, wie das Drittanbietertool SIOS DataKeeper eingerichtet und der interne Load Balancer von Azure konfiguriert werden. Diese Tools geben uns die Möglichkeit, einen Windows-Failovercluster mit Dateifreigabenzeuge in Microsoft Azure zu erstellen.


![Abbildung 2: Schema einer Windows Server-Failoverclusterkonfiguration in Azure ohne freigegebenen Datenträger][sap-ha-guide-figure-1001]

_**Abbildung 2:** Schema einer Windows Server-Failoverclusterkonfiguration in Azure ohne freigegebenen Datenträger_


### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Freigegebener Datenträger in Microsoft Azure mit SIOS DataKeeper

Ab September 2016 bietet Microsoft Azure keinen freigegebenen Speicher zum Erstellen eines Clusters mit freigegebenem Speicher mehr. Für eine hoch verfügbare SAP ASCS /SCS-Instanz ist jedoch freigegebener Clusterspeicher erforderlich.

Die Drittanbietersoftware SIOS DataKeeper Cluster Edition ermöglicht es, einen gespiegelten Speicher zu erstellen, der freigegebenen Clusterspeicher simuliert. Die SIOS-Lösung bietet eine synchrone Datenreplikation in Echtzeit. Eine freigegebene Datenträgerressource für einen Cluster wird folgendermaßen erstellt:

- An alle virtuellen Computer (VMs) in einer Windows-Clusterkonfiguration muss eine zusätzliche virtuelle Azure-Festplatte (VHD) angehängt werden.
- SIOS DataKeeper Cluster Edition muss auf beiden VM-Knoten ausgeführt werden.
- SIOS DataKeeper Cluster Edition muss in einer Weise konfiguriert sein, die synchron den Inhalt des zusätzlich angehängten VHD-Volumes von virtuellen Quellcomputern im zusätzlich angehängten VHD-Volume des virtuellen Zielcomputers widerspiegelt. SIOS DataKeeper abstrahiert die lokalen Quell- und Zielvolumes und präsentiert diese dem Windows-Failovercluster als einzelnen freigegebenen Datenträger.

Weitere Informationen zu SIOS DataKeeper finden Sie hier: [http://us.sios.com/products/datakeeper-cluster/](http://us.sios.com/products/datakeeper-cluster/)

 ![Abbildung 3: Schema einer Windows Server-Failoverclusterkonfiguration in Azure bei Verwenden von SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Abbildung 3:** Schema einer Windows Server-Failoverclusterkonfiguration in Azure bei Verwenden von SIOS DataKeeper_

> [AZURE.NOTE] Zum Erzielen von hoher Verfügbarkeit mit verschiedenen DBMS, wie z.B. SQL Server, sind keine freigegebenen Datenträger erforderlich. SQL Server Always On führt die Replikation von DBMS-Daten- und -Protokolldateien vom lokalen Datenträger eines Clusterknotens auf den lokalen Datenträger eines anderen Clusterknotens durch. Daher ist bei der Windows-Clusterkonfiguration kein freigegebener Datenträger erforderlich.

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Namensauflösung in Microsoft Azure

Die Cloudplattform von Microsoft Azure ermöglicht nicht die Konfiguration virtueller IP-Adressen, z.B. von Floating IPs. Aus diesem Grund benötigen Sie eine alternative Lösung zum Einrichten einer virtuellen IP-Adresse, um die Clusterressource in der Cloud zu erreichen. Azure bietet den internen Load Balancer (ILB). Mithilfe des ILB kann der Cluster über die virtuelle IP-Adresse des Clusters erreicht werden. Daher müssen Sie den ILB in der Ressourcengruppe bereitstellen, die die Clusterknoten enthält. Anschließend müssen Sie alle erforderlichen Portweiterleitungsregeln mithilfe der Testports des ILB konfigurieren. Die Clients können über den virtuellen Hostnamen eine Verbindung herstellen. Der DNS-Server löst die IP-Adresse des Clusters auf. Der ILB übernimmt die Weiterleitung an den aktiven Knoten des Clusters.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> Hohe Verfügbarkeit von SAP NetWeaver in Azure IaaS

Wie bereits im Kapitel zur hohen Verfügbarkeit unter [„SAP NetWeaver auf Azure Virtual Machines (VMs)“ im Planungs- und Implementierungshandbuch für SAP NetWeaver auf Windows Virtual Machines (VMs)][planning-guide-11] erläutert, müssen zum Erreichen von hoher Verfügbarkeit für SAP-Anwendungen (z.B. von SAP-Softwarekomponenten) die folgenden Komponenten geschützt werden:

- SAP-Anwendungsserver
- SAP ASCS/SCS-Instanz
- DBMS-Server

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> Hohe Verfügbarkeit für SAP-Anwendungsserver

Für die SAP-Anwendungsserver/-Dialoginstanzen ist meist keine spezielle hoch verfügbare Lösung erforderlich. Sie erreichen hohe Verfügbarkeit durch Redundanz und verfügen demnach über mehrere Dialoginstanzen, die auf verschiedenen virtuellen Azure-Computern konfiguriert sind. Es müssen mindestens zwei SAP-Anwendungsinstanzen auf zwei virtuellen Azure-Computern installiert sein.

![Abbildung 4: SAP-Anwendungsserver mit hoher Verfügbarkeit][sap-ha-guide-figure-2000]

_**Abbildung 4:** SAP-Anwendungsserver mit hoher Verfügbarkeit_


Alle virtuellen Computer, auf denen SAP-Anwendungsserver gehostet werden, müssen in der gleichen **Azure-Verfügbarkeitsgruppe** platziert werden. Die Azure-Verfügbarkeitsgruppe stellt Folgendes sicher:

- Alle virtuellen Computer gehören zu denselben Upgradedomänen. Das heißt, dass vermieden wird, dass die VMs während einer geplanten Wartungsausfallzeit zeitgleich aktualisiert werden.
- Alle virtuellen Computer gehören zu denselben Fehlerdomänen. Dadurch wird z.B. sichergestellt, dass VMs so bereitgestellt werden, dass ein Single Point of Failure vermieden wird, der sich auf die Verfügbarkeit aller VMs auswirken könnte.

Weitere Informationen finden Sie im Artikel [Verwalten der Verfügbarkeit virtueller Computer][virtual-machines-manage-availability].

Da das Azure-Speicherkonto ein potenzieller Single Point of Failure sein kann, benötigen Sie unbedingt mindestens zwei Azure-Speicherkonten, auf die mindestens zwei VMs verteilt sind. Im Idealfall sollte jede VM, auf der SAP-Dialoginstanzen ausgeführt werden, in einem anderen Speicherkonto bereitgestellt werden.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> Hohe Verfügbarkeit für die SAP (A)SCS-Instanzen 

![Abbildung 5: Hoch verfügbare SAP ASCS-/SCS-Instanz][sap-ha-guide-figure-2001]

_**Abbildung 5:** Hoch verfügbare SAP ASCS-/SCS-Instanz_


#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> Hohe Verfügbarkeit für SAP (A)SCS-Instanz mit Windows-Failovercluster in Azure

Im Vergleich mit Bare-Metal- oder privaten Cloudbereitstellungen sind für virtuelle Microsoft Azure-Computer zusätzliche Schritte erforderlich, um einen WSFC zu konfigurieren. Zum Erstellen eines Windows-Failoverclusters sind zum Einrichten eines Clusters von SAP ASCS/SCS-Instanzen ein freigegebener Clusterdatenträger, mehrere IP-Adressen und virtuelle Hostnamen und ein interner Load Balancer (ILB) von Azure erforderlich.

Dies wird im weiteren Verlauf des Dokuments detailliert erläutert.

![Abbildung 6: Schema einer Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure bei Verwenden von SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Abbildung 6:** Schema einer Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure bei Verwenden von SIOS DataKeeper_


### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a> Hohe Verfügbarkeit für die DBMS-Instanz

Das DBMS ist in einem SAP-System ebenfalls ein SPOF und muss deshalb mit einer hoch verfügbaren Lösung geschützt werden. Es folgt ein Beispiel für eine SQL Server Always On-Lösung mit hoher Verfügbarkeit in Azure mithilfe eines Windows Server-Failoverclusters und internen Load Balancers von Azure. SQL Server Always On repliziert DBMS-Daten- und -Protokolldateien mithilfe seiner eigenen DBMS-Replikation. Daher benötigen wir keine freigegebenen Clusterdatenträger, was die gesamte Einrichtung vereinfacht.


![Abbildung 7: SAP DBMS-Server mit hoher Verfügbarkeit – Beispiel einer SQL Server Always On-Einstellung für hohe Verfügbarkeit][sap-ha-guide-figure-2003]

_**Abbildung 7:** SAP DBMS-Server mit hoher Verfügbarkeit – Beispiel einer SQL Server Always On-Einstellung für hohe Verfügbarkeit_


Das Clustering des DBMS wird in diesem Dokument nicht behandelt.

Weitere Informationen zum Clustering von SQL Server in Azure mithilfe des Azure Resource Manager-Bereitstellungsmodells finden Sie in den folgenden Artikeln:

- [Manuelles Konfigurieren der Always On-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
- [Konfigurieren eines internen Load Balancers für eine Always On-Verfügbarkeitsgruppe in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

### <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> Mögliche End-to-End-Bereitstellungsszenarien für hohe Verfügbarkeit

Hier ist ein Beispiel einer vollständigen SAP NetWeaver-Architektur für hohe Verfügbarkeit in Azure, bei der wir einen dedizierten Cluster für die SAP ASCS/SCS-Instanz und einen anderen für das DBMS verwenden.

![Abbildung 8: SAP-Architekturvorlage 1 für hohe Verfügbarkeit – mit dediziertem Cluster für ASCS/SCS und dediziertem Cluster für DBMS-Instanz][sap-ha-guide-figure-2004]

_**Abbildung 8:** SAP-Architekturvorlage 1 für hohe Verfügbarkeit – mit dediziertem Cluster für ASCS/SCS und dediziertem Cluster für DBMS-Instanz_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Vorbereitung der Infrastruktur

Um die Bereitstellung der erforderlichen Ressourcen für SAP zu vereinfachen, haben wir Azure Resource Manager-Vorlagen für SAP entwickelt.

Diese Vorlagen mit 3 Ebenen unterstützen auch Szenarien mit hoher Verfügbarkeit wie z.B.:

- **Architekturvorlage 1**: Mit zwei Clustern für Single Points of Failures von SAP ASCS/SCS und DBMS

Azure Resource Manager-Vorlagen für Szenario 1 sind hier verfügbar:

- Azure Marketplace-Image: [https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)
- Benutzerdefiniertes Image: [https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Wenn Sie auf „sap-3-tier-marketplace-image“ klicken, erhalten Sie im Azure-Portal die folgende Benutzeroberfläche:

![Abbildung 9: Angeben von Azure Resource Manager-Parametern für hohe Verfügbarkeit von SAP][sap-ha-guide-figure-3000]

_**Abbildung 9:** Angeben von Azure Resource Manager-Parametern für hohe Verfügbarkeit von SAP_


Wählen Sie für die Option SYSTEMAVAILABILITY unbedingt **HA** (High Availability, hohe Verfügbarkeit) aus.

Mit den Vorlagen wird Folgendes erstellt:

- Alle erforderlichen **VMs** für:
    - SAP-Anwendungsserver-VMs: `<SAPSystemSID>-di-<Number>`
    - ASCS/SCS-Cluster-VMs: `<SAPSystemSID>-ascs-<Number>`
    - DBMS-Cluster: `<SAPSystemSID>-db-<Number>`
- **Netzwerkkarten für alle VMs mit zugeordneten IP-Adressen**:
    - `<SAPSystemSID>-nic-di-<Number>`
    - `<SAPSystemSID>-nic-ascs-<Number>`
    - `<SAPSystemSID>-nic-db-<Number>`
- **Azure-Speicherkonten**
- **Verfügbarkeitsgruppen** für:
    - SAP-Anwendungsserver-VMs: `<SAPSystemSID>-avset-di`
    - SAP ASCS/SCS-Cluster-VMs: `<SAPSystemSID>-avset-ascs`
    - DBMS-Cluster-VMs: `<SAPSystemSID>-avset-db`
- **Interner Load Balancer (ILB) von Azure** mit allen Ports für ASCS/SCS-Instanz und IP-Adresse `<SAPSystemSID>-lb-ascs`
-	**Interner Load Balancer (ILB) von Azure** mit allen Ports für SQL Server DBMS und IP-Adresse `<SAPSystemSID>-lb-db`
- **Netzwerksicherheitsgruppe**: `<SAPSystemSID>-nsg-ascs-0` Mit offenem externen RDP-Port zur VM `<SAPSystemSID>-ascs-0`


> [AZURE.NOTE]  Alle IP-Adressen der Netzwerkkarten und Azure ILBs werden anfänglich als **dynamisch** erstellt. Sie müssen Sie in **statische** IP-Adressen ändern, was weiter unten in diesem Dokument beschrieben wird.


### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Bereitstellen von VMs mit (standortübergreifender) Verbindung mit dem Unternehmensnetzwerk für den Produktiveinsatz

Für SAP-Produktivsysteme müssen Sie Azure-VMs mit einer [(standortübergreifenden) Verbindung mit dem Unternehmensnetzwerk][planning-guide-2.2] bereitstellen, wozu ein Azure-Standort-zu-Standort-VPN oder Azure ExpressRoute verwendet wird.

> [AZURE.NOTE]  In diesem Fall sind Ihr Azure-VNET und -Subnetz bereits erstellt und vorbereitet.


Wählen Sie für das Feld **NEWOREXISTINGSUBNET** die Option „Existing“ aus.

In das Textfeld **SUBNETID** müssen Sie die vollständige Zeichenfolge Ihrer vorbereiteten Subnetz-ID für das Azure-Netzwerk eingeben, in dem Sie Ihre Azure-VMs bereitstellen möchten.

Mit dem folgenden PowerShell-Befehl können Sie eine Liste aller Azure-Netzwerksubnetze abrufen:

```powershell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
```


Die **SUBNETZ-ID** wird im Feld „ID“ angezeigt.

Eine Liste aller **SUBNETZ-IDs** kann mit dem folgenden PowerShell-Befehl abgerufen werden:

```PowerShell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
```

Die **SUBNETZID** sieht in etwa wie folgt aus:

```
/subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Reine Cloudbereitstellung von SAP-Instanzen zu Test-/Demozwecken

Sie können Ihr hoch verfügbares SAP-System auch in einem sog. reinen Cloudbereitstellungsmodell bereitstellen.

Diese Bereitstellung ist für die meisten Demozwecke, jedoch nicht für Produktivanwendungsfälle geeignet.

Wählen Sie für das Feld _NEWOREXISTINGSUBNET_ die Option **New** aus. Lassen Sie das Feld **SUBNETID** leer.

Das Azure-VNET und -Subnetz werden von der Azure Resource Manager-Vorlage für SAP automatisch erstellt.

> [AZURE.NOTE] Darüber hinaus müssen Sie mindestens einen dedizierten virtuellen Computer für AD/DNS im selben VNET bereitstellen. Diese virtuellen Computer werden nicht von der Vorlage erstellt.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Virtuelles Azure-Netzwerk (Azure VNET)

Bei unserem Beispiel ist der Adressraum des Azure VNET 10.0.0.0/16. Es gibt ein Subnetz mit dem Namen _**Subnet**_ mit dem Adressbereich 10.0.0.0/24. Alle virtuellen Computer (VMs) und ILBs werden in diesem VNET bereitgestellt.
  
> [AZURE.NOTE] Nehmen Sie im Gastbetriebssystem keine Änderungen an den Netzwerkeinstellungen (z.B. IP-Adresse, DNS-Server, Subnetz usw.) vor. Alle Netzwerkeinstellungen erfolgen durch Azure und werden vom DHCP-Dienst verteilt.

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS-IP-Adressen

Stellen Sie sicher, dass Ihr VNET-Option **DNS-Server** auf **Benutzerdefiniertes DNS** festgelegt ist. Im Fall von:

- **[(Standortübergreifender) Verbindung mit dem Unternehmensnetzwerk][planning-guide-2.2]**: Fügen Sie die IP-Adressen der lokalen DNS-Server hinzu. Lokale DNS-Server können auf die virtuellen Computer erweitert werden, die in Azure ausgeführt werden. In diesem Fall können Sie die IP-Adressen der Azure-VMs hinzufügen, die zum Ausführen des DNS-Diensts konfiguriert sind.

-	**[Reiner Cloudbereitstellung][planning-guide-2.1]**: Stellen Sie eine zusätzliche VM im selben VNET bereit, die als DNS-Server fungiert. Fügen Sie die IP-Adressen der Azure-VMs hinzu, die zum Ausführen des DNS-Diensts konfiguriert sind.


![Abbildung 10: Konfigurieren von DNS-Servern für Azure-VNET][sap-ha-guide-figure-3001]

_**Abbildung 10:** Konfigurieren von DNS-Servern für Azure-VNET_

> [AZURE.NOTE] Wenn Sie die IP-Adressen der DNS-Server ändern, müssen Sie die Azure-VMs neu starten, damit die Änderung übernommen und an die neuen DNS-Server übertragen wird. In unserem Beispiel ist der DNS-Dienst auf den folgenden Windows-VMs installiert und konfiguriert:



| VM-Rolle | VM-Hostname | Name der Netzwerkkarte | Statische IP-Adresse  
| ---------------|-------------|--------------------|-------------------
| 1\. DNS-Server | domcontr-0 | pr1-nic-domcontr-0 | 10\.0.0.10         
| 2\. DNS-Server | domcontr-1 | pr1-nic-domcontr-1 | 10\.0.0.11         


### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Hostnamen und statische IP-Adressen für die SAP ASCS/SCS-Clusterinstanz und DBMS-Clusterinstanz

Ähnlich wie in lokalen Umgebungen benötigen wir die folgenden reservierten Hostnamen bzw. IP-Adressen:

| Rolle des virtuellen Hostnamens | Virtueller Hostname | Virtuelle statische IP-Adresse 
| ----------------------------------------------------------------------------|------------------|---------------------------
| SAP ASCS/SCS – 1. virtueller Hostname im Cluster (wird für die Clusterverwaltung verwendet) | pr1-ascs-vir | 10\.0.0.42                 
| SAP ASCS/SCS – **Instanz** – Virtueller Hostname | pr1-ascs-sap | `10.0.0.43`             
| SAP DBMS – 2. virtueller Hostname im Cluster (wird für die Clusterverwaltung verwendet) | pr1-dbms-vir | 10\.0.0.32                 
 

Die virtuellen Hostnamen _**pr1-ascs-vir**_ und _**pr1-dbms-vir**_ und zugehörigen IP-Adressen, die zum Verwalten des Clusters selbst verwendet werden, werden während der Erstellung des Clusters generiert. Siehe dazu das Kapitel [Erfassen von Clusterknoten in der Clusterkonfiguration][sap-ha-guide-8.12.1].

Die anderen beiden virtuellen Hostnamen _**pr1-ascs-sap**_ und _**pr1-dbms-sap**_ und zugehörigen IP-Adressen, die von der SAP ASCS/SCS-Clusterinstanz und DBMS-Clusterinstanz verwendet werden, können manuell auf dem DNS-Server erstellt werden. Siehe dazu das Kapitel [Erstellen virtueller Hostnamen für SAP ASCS/SCS-Cluster][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Einrichten statischer IP-Adressen für die SAP-VMs

Nach der Bereitstellung der virtuellen Computer für das Clustering müssen wir statische IP-Adressen für alle virtuellen Computer einrichten. Dies darf nicht innerhalb des Gastbetriebssystems, sondern muss in der Konfiguration des virtuellen Azure-Netzwerks erfolgen.

Hierfür bietet sich das Azure-Portal an. Navigieren Sie im Azure-Vorschauportal zu:

```
<Resource Group> -> <Network Card> -> Settings -> IP Address
```

Ändern Sie das Feld **Zuweisung** von **Dynamisch** in **Statisch**, und geben Sie die gewünschte IP-Adresse ein.

> [AZURE.NOTE] Wenn Sie die IP-Adressen der Netzwerkkarte ändern, müssen Sie die Azure-VMs neu starten, damit die Änderung übernommen wird.


![Abbildung 11: Konfigurieren der statischen IP-Adresse der Netzwerkkarte für die einzelnen virtuellen Computer][sap-ha-guide-figure-3002]

_**Abbildung 11:** Konfigurieren der statischen IP-Adresse der Netzwerkkarte für die einzelnen virtuellen Computer_

Wiederholen Sie diesen Schritt für alle Netzwerkschnittstellen, d.h. für alle VMs einschließlich derjenigen, die Sie für den AD-/DNS-Dienst nutzen möchten.

In unserem Beispiel haben wir die folgenden VMs und statischen IP-Adressen:

| VM-Rolle | VM-Hostname | Name der Netzwerkkarte | Statische IP-Adresse  
| ----------------------------------------|--------------|--------------------|-------------------
| 1\. SAP-Anwendungsserver | pr1-di-0 | pr1-nic-di-0 | 10\.0.0.50         
| 2\. SAP-Anwendungsserver | pr1-di-1 | pr1-nic-di-1 | 10\.0.0.51         
| ... | ... | ... | ...               
| Letzter SAP-Anwendungsserver | pr1-di-5 | pr1-nic-di-5 | 10\.0.0.55         
| 1\. Clusterknoten für ASCS/SCS-Instanz | pr1-ascs-0 | pr1-nic-ascs-0 | 10\.0.0.40         
| 2\. Clusterknoten für ASCS/SCS-Instanz | pr1-ascs-1 | pr1-nic-ascs-1 | 10\.0.0.41         
| 1\. Clusterknoten für DBMS-Instanz | pr1-db-0 | pr1-nic-db-0 | 10\.0.0.30         
| 2\. Clusterknoten für DBMS-Instanz | pr1-db-1 | pr1-nic-db-1 | 10\.0.0.31         

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Einrichten der statischen IP-Adresse für internen Load Balancer (ILB)

Die Azure Resource Manager-Vorlage für SAP erstellt einen internen Azure Load Balancer (ILB), der für die SAP ASCS/SCS-Clusterinstanz und die DBMS-Clusterinstanz verwendet wird.

Bei der ersten Bereitstellung wird die IP-Adresse des ILB auf **Dynamisch** festgelegt. Die IP-Adresse muss in **Statisch** geändert werden.

In unserem Beispiel haben wir zwei Azure ILBs mit den folgenden statischen IP-Adressen:

| Rolle des Azure ILB | Name des Azure ILB | Statische IP-Adresse 
| ---------------------------|----------------|-------------------
| ILB der SAP ASCS/SCS-Instanz | pr1-lb-ascs | `10.0.0.43`         
| ILB des SAP-DBMS | pr1-lb-dbms | 10\.0.0.33         


> [AZURE.NOTE]  
**IP-Adresse des virtuellen Hostnamens der SAP ASCS/SCS-Instanz = IP-Adresse des Azure Load Balancers von SAP ASCS/SCS: pr1-lb-ascs** **IP-Adresse des virtuellen Namens des DBMS = IP-Adresse des Azure Load Balancers des DBMS: pr1-lb-dbms**

Legen Sie in unserem Beispiel die IP-Adresse des internen Load Balancers _pr1-lb-ascs_ auf die IP-Adresse des virtuellen Hostnamens der SAP ASCS/SCS-Instanz (`10.0.0.43`) fest.

![Abbildung 12: Einrichten der statischen IP-Adresse für internen Load Balancer (ILB) für SAP ASCS/SCS-Instanz][sap-ha-guide-figure-3003]

_**Abbildung 12:** Einrichten der statischen IP-Adresse für internen Load Balancer (ILB) für SAP ASCS/SCS-Instanz_

Legen Sie auf dieselbe Weise die IP-Adresse des Load Balancers _pr1-lb-dbms_ auf die IP-Adresse des virtuellen Hostnamens der DBMS-Instanz (in unserem Fall 10.0.0.33) fest.

### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Standardmäßige ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure

Die Azure Resource Manager-Vorlage für SAP erstellt standardmäßig alle erforderlichen Ports für:

- ABAP ASCS-Instanz mit der Standardinstanznummer **00**
- Java ASCS-Instanz mit der Standardinstanznummer **01**

Aus diesem Grund müssen Sie während der Installation Ihrer SAP ASCS/SCS-Instanz die Standardinstanznummern 00 und 01 für Ihre ABAP ASCS- und/oder Java SCS-Instanz verwenden.

Die folgenden Azure ILB-Endpunkte sind erforderlich und werden für die SAP NetWeaver ABAP ASCS-Ports erstellt:


| Dienst/Name der Lastenausgleichsregel | Standardportnummern | Konkrete Ports für (ASCS-Instanz mit Instanznummer 00) (ERS mit 10)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Enqueue-Server/_lbrule3200_ | 32`<InstanceNumber>` | 3200                                                                     
| ABAP-Message-Server/_lbrule3600_ | 32`<InstanceNumber>` | 3600                                                                     
| Interne ABAP-Nachricht/_lbrule3900_ | 39`<InstanceNumber>` | 3900                                                                     
| Message-Server HTTP/_Lbrule8100_ | 81`<InstanceNumber>` | 8100                                                                     
| SAP Start Service ASCS HTTP/_Lbrule50013_ | 5`<InstanceNumber>`13 | 50013                                                                    
| SAP Start Service ASCS HTTPS/_Lbrule50014_ | 5`<InstanceNumber>`14 | 50014                                                                    
| Enqueue-Replikation/_Lbrule50016_ | 5`<InstanceNumber>`16 | 50016                                                                    
| SAP Start Service ERS HTTP _Lbrule51013_ | 5`<InstanceNumber>`13 | 51013                                                                    
| SAP Start Service ERS HTTP _Lbrule51014_ | 5`<InstanceNumber>`14 | 51014                                                                    
| Win RM _Lbrule5985_ | | 5985                                                                     
| Dateifreigabe _Lbrule445_ | | 445                                                                      

_**Tabelle 1:** Portnummern von SAP NetWeaver ABAP ASCS-Instanzen_


Die folgenden Azure ILB-Endpunkte sind erforderlich und müssen für die SAP NetWeaver Java SCS-Ports erstellt werden:

| Dienst/Name der Lastenausgleichsregel | Standardportnummern | Konkrete Ports für (SCS-Instanz mit Instanznummer 01) (ERS mit 11)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Enqueue-Server/_lbrule3201_ | 32`<InstanceNumber>` | 3201                                                                     
| Gateway-Server/_lbrule3301_ | 33`<InstanceNumber>` | 3301                                                                     
| Java-Message-Server/_lbrule3900_ | 39`<InstanceNumber>` | 3901                                                                     
| Message-Server-HTTP/_Lbrule8101_ | 81`<InstanceNumber>` | 8101                                                                     
| SAP Start Service SCS HTTP/_Lbrule50113_ | 5`<InstanceNumber>`13 | 50113                                                                    
| SAP Start Service SCS HTTPS/_Lbrule50114_ | 5`<InstanceNumber>`14 | 50114                                                                    
| Enqueue-Replikation/_Lbrule50116_ | 5`<InstanceNumber>`16 | 50116                                                                    
| SAP Start Service ERS HTTP _Lbrule51113_ | 5`<InstanceNumber>`13 | 51113                                                                    
| SAP Start Service ERS HTTP _Lbrule51114_ | 5`<InstanceNumber>`14 | 51114                                                                    
| Win RM _Lbrule5985_ | | 5985                                                                     
| Dateifreigabe _Lbrule445_ | | 445                                                                      

_**Tabelle 2:** Portnummern von SAP NetWeaver Java SCS-Instanzen_


![Abbildung 13: Standardmäßige ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure][sap-ha-guide-figure-3004]

_**Abbildung 13:** Standardmäßige ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure_

Legen Sie auf dieselbe Weise die IP-Adresse des Load Balancers _**pr1-lb-dbms**_ auf die IP-Adresse des virtuellen Hostnamens der DBMS-Instanz (in unserem Fall _**10.0.0.33**_) fest.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Ändern der standardmäßigen ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure

Wenn Sie eine andere Instanznummern für die SAP ASCS- oder SCS-Instanz verwenden möchten, müssen Sie die Namen und Werte dieser Ports ändern.

Hierfür bietet sich z.B. das Azure-Portal an.

Besuchen Sie `<SID>-lb-ascs load balancer -> Load Balancing Rules`.

Ändern Sie für alle zur SAP ASCS- oder SCS-Instanz gehörigen Lastenausgleichsregeln Folgendes:

- Name
- Port
- Back-End-Port

Wenn wir beispielsweise die Standardnummer der ASCS-Instanz von 00 in 31 ändern möchten, müssen Sie die Änderungen für alle Ports ausführen, die in _**Tabelle 1:** Portnummern von SAP NetWeaver ABAP ASCS-Instanzen_ aufgeführt sind.

Es folgt ein Beispiel einer Änderung für Port _lbrule3200_.

![Abbildung 14: Ändern der standardmäßigen ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure][sap-ha-guide-figure-3005]

_**Abbildung 14:** Ändern der standardmäßigen ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure_


### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Hinzufügen von Windows-Computern zur Domäne

Nachdem Sie den virtuellen Computern eine statische IP-Adresse zugewiesen haben, fügen Sie sie der Domäne hinzu.

![Abbildung 15: Hinzufügen eines virtuellen Computers zu einer Domäne][sap-ha-guide-figure-3006]

_**Abbildung 15:** Hinzufügen eines virtuellen Computers zu einer Domäne_


### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten für eine SAP ASCS/SCS-Instanz

Azure Load Balancers und auch der interne Azure Load Balancer (ILB) schließen Verbindungen, wenn diese Verbindungen für einen bestimmten Zeitraum im Leerlauf sind. Auf der anderen Seite öffnen SAP-Workprozesse in Dialoginstanzen Verbindungen mit dem SAP-Enqueue-Prozess, sobald die erste Enqueue-/Dequeue-Anforderung gesendet werden muss. Diese Verbindungen bleiben in der Regel bestehen, bis der Workprozess oder Enqueue-Prozess neu gestartet wird. Wenn die Verbindung allerdings für einige Zeit im Leerlauf ist, wird sie vom Azure ILB geschlossen. Das ist nicht wirklich ein Problem, da der SAP-Workprozess die Verbindung mit dem Enqueue-Prozess wiederherstellt, sollte diese nicht mehr bestehen. Allerdings werden diese Aktivitäten in den Entwickler-Traces von SAP-Prozessen dokumentiert, wobei ohne wirklich guten Grund sehr viele Informationen angehäuft werden. Daher empfehlen wir, die TCP/IP-Einstellungen `KeepAliveTime` und `KeepAliveInterval` auf beiden Clusterknoten zu ändern. Die Änderungen der TCP/IP-Parameter müssen mit SAP-Profilparametern kombiniert werden, die weiter unten in diesem Dokument beschrieben werden.

Fügen Sie die folgenden Windows-Registrierungseinträge auf beiden Windows-Clusterknoten für SAP ASCS/SCS hinzu:

| Pfad | HKLM\\SYSTEM\\CurrentControlSet\\Services\\Tcpip\\Parameters   
| ----------------------|-----------------------------------------------------------
| Variablenname | `KeepAliveTime`                                              
| Variablentyp | REG\_DWORD-Wert (dezimal)                                        
| Wert | 120000                                                     
| Link zur Dokumentation | [https://technet.microsoft.com/de-DE/library/cc957549.aspx](https://technet.microsoft.com/de-DE/library/cc957549.aspx) 


_**Tabelle 3:** Erster zu ändernder TCP/IP-Parameter_


| Pfad | HKLM\\SYSTEM\\CurrentControlSet\\Services\\Tcpip\\Parameters   
| ----------------------|-----------------------------------------------------------
| Variablenname | `KeepAliveInterval`                                          
| Variablentyp | REG\_DWORD-Wert (dezimal)                                        
| Wert | 120000                                                     
| Link zur Dokumentation | [https://technet.microsoft.com/de-DE/library/cc957548.aspx](https://technet.microsoft.com/de-DE/library/cc957548.aspx)


_**Tabelle 4:** Zweiter zu ändernder TCP/IP-Parameter_

**Starten Sie dann beide Clusterknoten neu**, um die Änderungen zu übernehmen.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Einrichten des Windows Server-Failoverclusters für SAP ASCS/SCS-Instanz

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Erfassen von Clusterknoten in der Clusterkonfiguration

Der erste Schritt ist das Hinzufügen des Features „Failoverclustering“ zu beiden Clusterknoten. Dies erfolgt mit dem **Assistenten zum Hinzufügen von Rollen und Features**, was eigentlich keiner weiteren Erklärung bedarf.

Der zweite Schritt ist das Einrichten des Failoverclusters mithilfe des Failovercluster-Managers von Windows.

Klicken Sie in der MMC des Failovercluster-Managers auf „Cluster erstellen“, und fügen nur den Namen des ersten Clusterknotens A hinzu, z.B. _**pr1-ascs-0**_. Fügen Sie den zweiten Knoten noch nicht hinzu. Dieser wird in einem späteren Schritt hinzugefügt.

![Abbildung 16: Erster Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Hinzufügen des Server-/VM-Namens des ersten Knotens des Clusters][sap-ha-guide-figure-3007]

_**Abbildung 16:** Erster Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Hinzufügen des Server-/VM-Namens des ersten Knotens des Clusters_

In den nächsten Schritten müssen Sie den Netzwerknamen (virtuellen Hostnamen) des Clusters angeben.

![Abbildung 17: Zweiter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Festlegen des Namens des Clusters][sap-ha-guide-figure-3008]

_**Abbildung 17:** Zweiter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Festlegen des Namens des Clusters_


Nach dem Erstellen des Clusters erfolgt ein Test zur Überprüfung des Clusters.

![Abbildung 18: Letzter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Ausführen der Clusterüberprüfung][sap-ha-guide-figure-3009]

_**Abbildung 18:** Letzter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Ausführen der Clusterüberprüfung_


![Abbildung 19: Letzter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Bei der Clusterüberprüfung wird die Warnung angezeigt, dass kein Quorumdatenträger gefunden wurde][sap-ha-guide-figure-3010]

_**Abbildung 19:** Letzter Schritt zum Hinzufügen einer Failoverclusterkonfiguration: Bei der Clusterüberprüfung wird die Warnung angezeigt, dass kein Quorumdatenträger gefunden wurde_

In dieser Phase können alle Warnungen zu Datenträgern ignoriert werden. Ein Dateifreigabenzeuge wird später zusammen mit den freigegebenen SIOS-Datenträgern hinzugefügt. In dieser Phase interessieren wir uns nicht für ein Quorum.

![Abbildung 20: Clusterkernressource mit IP-Adresse wird definiert: Eine neue IP-Adresse ist jedoch erforderlich][sap-ha-guide-figure-3011]

_**Abbildung 20:** Clusterkernressource mit IP-Adresse wird definiert: Eine neue IP-Adresse ist jedoch erforderlich_


Da die IP-Adresse des Servers auf einen der VM-Knoten zeigt, kann nicht der Cluster starten. Wir müssen nun die IP-Adresse des Hauptclusterdiensts ändern.

Das bedeutet, dass wir dem virtuellen Hostnamen des Clusters_**pr1-ascs-vir**_ eine IP-Adresse (in unserem Beispiel _**10.0.0.42**_) zuweisen müssen. Dies erfolgt über die Eigenschaftenseite der IP-Adressressource des Hauptclusterdiensts (siehe unten).

![Abbildung 21: Festlegen der ordnungsgemäßen IP-Adresse im Feld „Eigenschaften“][sap-ha-guide-figure-3012]

_**Abbildung 21:** Festlegen der ordnungsgemäßen IP-Adresse im Feld „Eigenschaften“_


![Abbildung 22: Zuweisen der für den Cluster reservierten IP-Adresse][sap-ha-guide-figure-3013]

_**Abbildung 22:** Zuweisen der für den Cluster reservierten IP-Adresse_

Schalten Sie nach dem Ändern der IP-Adresse den virtuellen Hostnamen des Clusters online.

![Abbildung 23: Hauptclusterdienst im laufenden Betrieb mit ordnungsgemäßer IP-Adresse][sap-ha-guide-figure-3014]

_**Abbildung 23:** Hauptclusterdienst im laufenden Betrieb mit ordnungsgemäßer IP-Adresse_

Nun da der Hauptclusterdienst in Betrieb ist, können Sie den zweiten Clusterknoten hinzufügen.

![Abbildung 24: Hinzufügen des zweiten Clusterknotens][sap-ha-guide-figure-3015]

_**Abbildung 24:** Hinzufügen des zweiten Clusterknotens_

![Abbildung 25: Hinzufügen des Hostnamens des zweiten Clusterknotens. Beispiel: pr1-ascs-1][sap-ha-guide-figure-3016]

_**Abbildung 25:** Hinzufügen des Hostnamens des zweiten Clusterknotens. Beispiel: pr1-ascs-1_

![Abbildung 26: : Kontrollkästchen NICHT aktivieren!][sap-ha-guide-figure-3017]

_**Abbildung 26:** : Kontrollkästchen NICHT aktivieren!_

> [AZURE.NOTE]  
Stellen Sie sicher, dass das Kontrollkästchen „Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden“ **NICHT** aktiviert wird!

![Abbildung 27: Warnung zum Datenträgerquorum wiederum ignorieren][sap-ha-guide-figure-3018]

_**Abbildung 27:** Warnung zum Datenträgerquorum wiederum ignorieren_

Sie können Warnungen zu Quorum und Datenträgern ignorieren. Das Quorum und die Einrichtung der Freigabedatenträger werden später konfiguriert (siehe das Kapitel **[Installieren von SIOS DataKeeper Cluster Edition für SAP ASCS/SCS-Clusterfreigabe-Datenträger][sap-ha-guide-8.12.3]**).

#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Konfigurieren eines Cluster-Dateifreigabenzeugen

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Erstellen einer Dateifreigabe

Wir wählen einen Dateifreigabenzeugen und keinen Quorumdatenträger. Diese Option wird von SIOS DataKeeper unterstützt.

In der Konfiguration, die wir in diesem Dokument zur Veranschaulichung nutzen, ist der Dateifreigabenzeuge auf dem AD-/DNS-Server konfiguriert, der in Azure ausgeführt wird und _**domcontr-0**_ heißt. Da Sie eine VPN-Verbindung mit Azure (Standort-zu-Standort oder ExpressRoute) konfiguriert haben, befindet sich Ihr AD/DNS am lokalen Standort und ist deshalb nicht zur Ausführung als Dateifreigabenzeuge geeignet.

> [AZURE.NOTE] Falls Ihr AD/DNS nur lokal ausgeführt wird, konfigurieren Sie Ihren Dateifreigabenzeugen nicht in einem AD/DNS, das lokal in einem Windows-Betriebssystem ausgeführt wird, da die Netzwerklatenz zwischen in Azure ausgeführten Clusterknoten und lokal ausgeführtem AD/DNS zu groß sein kann, was Verbindungsprobleme verursachen kann. Konfigurieren Sie den Dateifreigabenzeugen in einer in Azure ausgeführten Windows-VM, die in der Nähe des Clusterknotens betrieben wird.

Das Quorumlaufwerk benötigt mindestens 1.024 MB freien Speicherplatz. Empfohlen werden 2.048 MB.

Der erste Schritt ist das Hinzufügen des Clusternamenobjekts.

![Abbildung 28: Zuweisen der Berechtigungen für die Freigabe für das Clusternamenobjekt][sap-ha-guide-figure-3019]

_**Abbildung 28:** Zuweisen der Berechtigungen für die Freigabe für das Clusternamenobjekt_

Vergewissern Sie sich, dass die Berechtigungen das Ändern von Daten in der Freigabe für das Clusternamenobjekt zulassen (in unserem Beispiel _**pr1-ascs-vir$**_). Um das Clusternamenobjekt der oben gezeigten Liste hinzuzufügen, klicken Sie auf **Hinzufügen**. Ändern Sie dann den Filter, um das Suchen nach Computerobjekten zu erlauben (siehe unten).

![Abbildung 29: Ändern des Objekttyps, um Computerobjekte ebenfalls einzuschließen][sap-ha-guide-figure-3020]

_**Abbildung 29:** Ändern des Objekttyps, um Computerobjekte ebenfalls einzuschließen_

![Abbildung 30: Aktivieren des Kontrollkästchens für Computerobjekte][sap-ha-guide-figure-3021]

_**Abbildung 30:** Aktivieren des Kontrollkästchens für Computerobjekte_

Geben Sie anschließend das Clusternamenobjekt ein, wie in Abbildung 29 dargestellt. Da der Datensatz jetzt erstellt sein sollte, können Sie die Berechtigungen ändern (siehe Abbildung 28).

Legen Sie als Nächstes auf der Registerkarte „Sicherheit“ der Freigabe die feiner abgestuften Berechtigungen für das Clusternamenobjekt fest.

![Abbildung 31: Festlegen von Sicherheitsattributen für das Clusternamenobjekt für das Dateifreigabequorum][sap-ha-guide-figure-3022]

_**Abbildung 31:** Festlegen von Sicherheitsattributen für das Clusternamenobjekt für das Dateifreigabequorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Konfigurieren des Dateifreigabenzeugen-Quorums im Failovercluster-Manager

Der nächste Schritt besteht im Ändern der Clusterkonfiguration in einen Dateifreigabenzeugen mithilfe des Failovercluster-Managers.

![Abbildung 32: Aufrufen des Assistenten zum Konfigurieren des Clusterquorums (wie hier gezeigt)][sap-ha-guide-figure-3023]

_**Abbildung 32:** Aufrufen des Assistenten zum Konfigurieren des Clusterquorums (wie hier gezeigt)_

![Abbildung 33: Auswahlbildschirm mit verschiedenen Quorumkonfigurationen][sap-ha-guide-figure-3024]

_**Abbildung 33:** Auswahlbildschirm mit verschiedenen Quorumkonfigurationen_

Bei dieser Option müssen Sie _**Quorumzeugen auswählen **_ wählen.

![Abbildung 34: Auswahlbildschirm des Dateifreigabenzeugen][sap-ha-guide-figure-3025]

_**Abbildung 34:** Auswahlbildschirm des Dateifreigabenzeugen_

In unserem Fall müssen Sie _**Dateifreigabenzeugen konfigurieren**_ auswählen.

![Abbildung 35: Festlegen des Speicherorts der Dateifreigabe für die Zeugenfreigabe][sap-ha-guide-figure-3026]

_**Abbildung 35:** Festlegen des Speicherorts der Dateifreigabe für die Zeugenfreigabe_


Geben Sie den UNC-Pfad zur Dateifreigabe ein (in unserem Beispiel `\\domcontr-0\FSW`).

Klicken Sie auf „Weiter“, woraufhin eine Liste der Änderungen eingeblendet wird, die Sie vornehmen möchten. Überprüfen Sie sie, und klicken Sie erneut auf „Weiter“, um die Clusterkonfiguration zu ändern.

![Abbildung 36: Bildschirm mit Meldung zur erfolgreichen Neukonfiguration des Clusters][sap-ha-guide-figure-3027]

_**Abbildung 36:** Bildschirm mit Meldung zur erfolgreichen Neukonfiguration des Clusters_

In diesem letzten Schritt sollte die Clusterkonfiguration erfolgreich neu konfiguriert werden.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Installieren von SIOS DataKeeper Cluster Edition für SAP ASCS/SCS-Clusterfreigabe-Datenträger

Unser aktueller Status ist, dass wir über eine funktionierende Windows Server-Failoverclusterkonfiguration in Azure verfügen. Allerdings hat diese Clusterkonfiguration noch keine freigegebenen Datenträgerressourcen. Um eine SAP ASCS/SCS-Instanz installieren zu können, benötigen wir freigegebene Datenträgerressourcen. Hier kommt SIOS DataKeeper Cluster Edition ins Spiel. Da Azure uns nicht erlaubt, freigegebene Datenträgerressourcen mit der erforderlichen Funktionalität zu erstellen, müssen wir z.B. auf SIOS DataKeeper zurückgreifen, um uns diese Funktionalität zu sichern.

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Hinzufügen von Microsoft .NET Framework 3.5

Bei den neuesten Versionen von Windows Server ist Microsoft .NET Framework 3.5 nicht automatisch aktiviert bzw. installiert. SIOS DataKeeper erfordert jedoch .NET Framework auf allen Knoten, auf denen das Produkt installiert wird. Aus diesem Grund ist es erforderlich, .NET Framework 3.5 in allen Gastbetriebssystemen der verschiedenen virtuellen Computer zu installieren.

Es gibt zwei Möglichkeiten zum Hinzufügen von .NET Framework 3.5. Die erste Möglichkeit ist die Verwendung von **Rollen und Features hinzufügen** unter Windows (wie unten dargestellt):

![Abbildung 37: Installieren von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features][sap-ha-guide-figure-3028]

_**Abbildung 37:** Installieren von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features_

![Abbildung 38: Statusanzeige bei der Installation von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features][sap-ha-guide-figure-3029]

_**Abbildung 38:** Statusanzeige bei der Installation von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features_

Die zweite Möglichkeit zum Aktivieren von .NET Framework 3.5 ist das Befehlszeilentool _**dism.exe**_. Für diese Art von Installation benötigen Sie Zugriff das Verzeichnis „Sxs“ auf dem Windows-Installationsmedium. Der folgende Befehl muss an einer Befehlszeile mit erhöhten Rechten ausgeführt werden:

```
Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> Installieren von SIOS DataKeeper

Lassen Sie uns die Installation von SIOS DataKeeper Cluster Edition durchlaufen. Das Tool muss auf beiden Knoten in unserem Cluster installiert werden. SIOS DataKeeper ermöglicht die Erstellung von virtuellem freigegebenem Speicher durch Erstellen eines synchronisierten Spiegels und Simulieren von freigegebenem Clusterspeicher.

Vor der Installation der SIOS-Software müssen Sie den Domänenbenutzer _**DataKeeperSvc**_ erstellen.

> [AZURE.NOTE] Fügen Sie diesen Benutzer _**DataKeeperSvc**_ der Gruppe **Lokale Administratoren** auf beiden Clusterknoten hinzu.
  
Installieren der SIOS-Software auf beiden Clusterknoten

![SIOS-Installationsprogramm][sap-ha-guide-figure-3030]

![Abbildung 39: Erster Bildschirm der SIOS DataKeeper-Installation][sap-ha-guide-figure-3031]

_**Abbildung 39:** Erster Bildschirm der SIOS DataKeeper-Installation_

![Abbildung 40: DataKeeper informiert, dass ein Dienst deaktiviert werden soll][sap-ha-guide-figure-3032]

_**Abbildung 40:** DataKeeper informiert, dass ein Dienst deaktiviert werden soll_

Wenn das Popupfenster in Abbildung 40 angezeigt wird, wählen Sie _**Yes**_.

![Abbildung 41: Benutzerauswahl für SIOS DataKeeper][sap-ha-guide-figure-3033]

_**Abbildung 41:** Benutzerauswahl für SIOS DataKeeper_


Im obigen Bildschirm wird die Wahl von _**Domain or Server account**_ empfohlen.

![Abbildung 42: Angeben des Domänenbenutzers und Kennworts bei der Installation von SIOS DataKeeper][sap-ha-guide-figure-3034]

_**Abbildung 42:** Angeben des Domänenbenutzers und Kennworts bei der Installation von SIOS DataKeeper_

Geben Sie das Domänenkonto, das Sie für SIOS DataKeeper erstellt haben, und die Kennwörter für dieses Konto an.

![Abbildung 43: Angeben Ihrer SIOS DataKeeper-Lizenz][sap-ha-guide-figure-3035]

_**Abbildung 43:** Angeben Ihrer SIOS DataKeeper-Lizenz_

Installieren Sie den Lizenzschlüssel für SIOS DataKeeper, wie in Abbildung 43 gezeigt. Am Ende der Installation werden Sie aufgefordert, **den virtuellen Computer neu zu starten**.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Einrichten von SIOS DataKeeper

Nach der Installation von SIOS DataKeeper auf beiden Knoten müssen wir mit der Konfiguration beginnen. Ziel der Konfiguration ist eine synchrone Datenreplikation zwischen den zusätzlichen virtuellen Festplatten, die Sie an jeden virtuellen Computer angefügt haben. In den folgenden Schritten wird die Konfiguration auf beiden Knoten gezeigt.

![Abbildung 44: DataKeeper-Tool für Verwaltung und Konfiguration][sap-ha-guide-figure-3036]

_**Abbildung 44:** DataKeeper-Tool für Verwaltung und Konfiguration_


Starten Sie das DataKeeper-Tool für Verwaltung und Konfiguration, und klicken Sie auf den Link _**Connect Server**_ (oben rot markiert).

![Abbildung 45: Einfügen des Namens oder der TCP/IP-Adresse des ersten Knotens und in einem zweiten Schritt des zweiten Knotens, mit denen das Verwaltungstool eine Verbindung herstellen soll][sap-ha-guide-figure-3037]

_**Abbildung 45:** Einfügen des Namens oder der TCP/IP-Adresse des ersten Knotens und in einem zweiten Schritt des zweiten Knotens, mit denen das Verwaltungstool eine Verbindung herstellen soll_

Der nächste Schritt ist das Erstellen des Replikationsauftrags zwischen den beiden Knoten.

![Abbildung 46: Erstellen eines Replikationsauftrags][sap-ha-guide-figure-3038]

_**Abbildung 46:** Erstellen eines Replikationsauftrags_

Ein Assistent begleitet Sie durch den Prozess.

![Abbildung 47: Festlegen des Namens des Replikationsauftrags][sap-ha-guide-figure-3039]

_**Abbildung 47:** Festlegen des Namens des Replikationsauftrags_

![Abbildung 48: Definieren der Basisdaten für den Knoten, der der aktuelle Quellknoten sein soll][sap-ha-guide-figure-3040]

_**Abbildung 48:** Definieren der Basisdaten für den Knoten, der der aktuelle Quellknoten sein soll_

Im ersten Schritt müssen der Name, die TCP/IP-Adresse und das Datenträgervolume des Quellknotens definiert werden. Im zweiten Schritt wird der Zielknoten festgelegt. Wiederum müssen der Name, die TCP/IP-Adresse und das Datenträgervolume des Zielknotens definiert werden.

![Abbildung 49: Definieren der Basisdaten für den Knoten, der der aktuelle Zielknoten sein soll][sap-ha-guide-figure-3041]

_**Abbildung 49:** Definieren der Basisdaten für den Knoten, der der aktuelle Zielknoten sein soll_

Der nächste Schritt ist das Bestimmen der Komprimierungsalgorithmen, die angewendet werden sollen. Für unsere Zwecke wird das Komprimieren des Replikationsdatenstroms empfohlen. Insbesondere in Situationen mit erneuter Synchronisierung wird die dafür erforderliche Dauer durch die Komprimierung des Replikationsdatenstroms erheblich verkürzt. Bedenken Sie, dass die Komprimierung CPU- und Arbeitsspeicherressourcen eines virtuellen Computers in Anspruch nimmt. Je höher also die Komprimierungsrate, desto mehr CPU-Nutzung. Sie können diese Einstellung später anpassen und ändern.

Eine andere Einstellung, die Sie überprüfen müssen, ist, ob die Replikation synchron oder asynchron erfolgt. **Zum Schutz von SAP ASCS/SCS-Konfigurationen muss die Replikationseinstellung auf „Synchronous“ festgelegt werden**.

![Abbildung 50: Festlegen von Replikationsdetails][sap-ha-guide-figure-3042]

_**Abbildung 50:** Festlegen von Replikationsdetails_

Der letzte Schritt besteht im Bestimmen, ob das Volume, das vom Replikationsauftrag repliziert wird, einer WSFC-Clusterkonfiguration als freigegebener Datenträger dargestellt werden sollte. Für die SAP ASCS/SCS-Konfiguration muss „Yes“ ausgewählt werden, damit der Windows-Cluster das replizierte Volume als freigegebenen Datenträger erkennt, das als Clustervolume verwendet werden kann.

![Abbildung 51: Auf „Yes“ klicken, um das replizierte Volume als Clustervolume zu aktivieren][sap-ha-guide-figure-3043]

_**Abbildung 51:** Auf „Yes“ klicken, um das replizierte Volume als Clustervolume zu aktivieren_

Nachdem die Erstellung abgeschlossen ist, zeigt das DataKeeper-Verwaltungstool den Replikationsauftrag als aktiv an.

![Abbildung 52: Die synchrone Spiegelung von DataKeeper des freigegebenen SAP ASCS/SCS-Datenträgers ist aktiv][sap-ha-guide-figure-3044]

_**Abbildung 52:** Die synchrone Spiegelung von DataKeeper des freigegebenen SAP ASCS/SCS-Datenträgers ist aktiv_

Daher wird der Datenträger jetzt im Failovercluster-Manager von Windows als DataKeeper-Datenträger angezeigt (siehe unten).

![Abbildung 53: Der von DataKeeper replizierte Datenträger wird im Failovercluster-Manager angezeigt][sap-ha-guide-figure-3045]

_**Abbildung 53:** Der von DataKeeper replizierte Datenträger wird im Failovercluster-Manager angezeigt_


## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Installation des SAP NetWeaver-Systems

Wir verzichten auf eine Beschreibung der Einrichtung des DBMS, da diese vom verwendeten DBMS abhängig ist. Allerdings wird davon ausgegangen, dass auf hohe Verfügbarkeit bezogene Aspekte des DBMS mit den Funktionen verwaltet werden, die verschiedene DBMS-Hersteller für Azure unterstützen. Beispiele: Always On oder Datenbankspiegelung für SQL Server und Oracle Data Guard für Oracle. In unserem Beispielszenario in dieser Dokumentation haben wir das DBMS nicht zusätzlich geschützt.

Es wurden auch keine besonderen Überlegungen hinsichtlich der verschiedenen DBMS und ihrer Interaktion mit solchen SAP ASCS/SCS-Clusterkonfigurationen in Azure angestellt.

> [AZURE.NOTE]  
Der Installationsvorgang von SAP NetWeaver ABAP-, Java- und ABAP+Java-Systemen ist nahezu identisch. Der größte Unterschied ist, dass ein SAP ABAP-System eine ASCS-Instanz hat. Das SAP Java-System hat eine SCS-Instanz. Und ein SAP ABAP+Java-System hat eine ASCS- und eine SCS-Instanz, die in der gleichen Microsoft-Failoverclustergruppe ausgeführt werden. Unterschiede bei der Installation der einzelnen SAP NetWeaver-Installationsstacks werden explizit angegeben. Bei allen anderen Schritten wird angenommen, dass sie identisch sind.

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> SAP-Installation mit hoch verfügbarer ASCS/SCS-Instanz

> [AZURE.NOTE]  
Platzieren Sie die Auslagerungsdatei nicht auf gespiegelten DataKeeper-Volumes, da dies von DataKeeper nicht unterstützt wird. Sie können die Auslagerungsdatei auf dem temporären Laufwerk D:\\ einer Azure-VM belassen, wo sie sich bereits befindet, wenn eine VM in Azure bereitgestellt wird. Sollte dies nicht der Fall sein, korrigieren Sie den Speicherort, und legen Sie die Windows-Auslagerungsdatei auf dem Laufwerk D:\\ Ihrer Azure-VM ab.

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Erstellen eines virtuellen Hostnamens für die SAP ASCS/SCS-Clusterinstanz

Zunächst müssen Sie den erforderlichen DNS-Eintrag für den virtuellen Hostnamen der ASCS/SCS-Instanz erstellen. Das Tool hierfür ist der Windows DNS-Manager. Neben dem virtuellen Hostnamen muss auch die IP-Adresse definiert werden, die dem virtuellen Hostnamen zugewiesen wird.

> [AZURE.NOTE]  
**Beachten Sie, dass die IP-Adresse, die wir dem virtuellen Hostnamen der ASCS/SCS-Instanz zuweisen, identisch mit der IP-Adresse sein muss, die wir dem Azure Load Balancer zugewiesen haben (`<sid>-lb-ascs`) IP-Adresse des virtuellen SAP ASCS/SCS-Hostnamen `(pr1-ascs-sap)` = IP-Adresse des Azure Load Balancers `(pr1-lb-ascs)`**

Dies bedeutet auch, dass in einem Windows Server-Failovercluster in Azure nur eine SAP-Failoverclusterrolle ausgeführt werden kann, z.B. für ein ABAP-System eine ASC-Instanz, für ein Java-System eine SCS-Instanz und für ein ABAP+Java-System eine ASC-Instanz und eine SCS-Instanz.

> [AZURE.NOTE] Das Bilden eines Clusters mit mehreren SIDs, wie im SAP-Installationshandbuch beschrieben (siehe [SAP-Installationshandbücher][sap-installation-guides]), ist derzeit in Azure nicht möglich.

![Abbildung 54: Definieren des DNS-Eintrags für den virtuellen SAP ASCS/SCS-Clusternamen und der TCP/IP-Adresse][sap-ha-guide-figure-3046]

_**Abbildung 54:** Definieren des DNS-Eintrags für den virtuellen SAP ASCS/SCS-Clusternamen und der TCP/IP-Adresse_

Der Eintrag wird im DNS-Manager unter der Domäne angezeigt (siehe die folgende Abbildung).

![Abbildung 55: Neuer virtueller Name und TCP/IP-Adresse für die SAP ASCS/SCS-Clusterkonfiguration][sap-ha-guide-figure-3047]

_**Abbildung 55:** Neuer virtueller Name und TCP/IP-Adresse für die SAP ASCS/SCS-Clusterkonfiguration_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Installieren des ersten SAP-Knotens des Clusters

Die Installation des ersten ASCS/SCS-Clusterknotens unterscheidet sich in keiner Weise von der Dokumentation der SAP-Installation:
- Führen Sie die Option „Erster Clusterknoten“ auf Clusterknoten A aus, z.B. dem Host _**pr1-ascs-0**_ wie in unserem Beispiel.

Wenn Sie für den internen Load Balancer von Azure die Standardports beibehalten möchten, wählen Sie:

- Für ein **ABAP-System**: **ASCS** Instanznummer **00**
- Für ein **Java-System**: **SCS** Instanznummer **01**
- Für ein **ABAP+JAVA-System**: **ASCS** Instanznummer**00** und **SCS** Instanznummer **01**

Wenn Sie andere Instanznummern als 00 für eine ABAP ASC-Instanz und 01 für eine Java SCS-Instanz verwenden möchten, müssen Sie zunächst die standardmäßigen Lastenausgleichsregeln für den Azure ILB ändern. Siehe dazu **[Ändern der standardmäßigen ASCS/SCS-Lastenausgleichsregeln für internen Load Balancer (ILB) von Azure][sap-ha-guide-8.9]**.

Nach diesem Schritt müssen Sie einige Schritte ausführen, die in der üblichen SAP-Installationsdokumentation nicht beschrieben sind.

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Ändern des SAP-Profils der ASCS/SCS-Instanz

Ein neuer Profilparameter muss hinzugefügt werden. Dieser Profilparameter verhindert das Schließen von Verbindungen zwischen SAP-Workprozessen und dem Enqueue-Server, wenn diese zu lange im Leerlauf sind. Dieses Problemszenario wurde bereits in diesem Dokument im Kapitel **[Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten für eine SAP ASCS/SCS-Instanz][sap-ha-guide-8.11]** erwähnt. In diesem Abschnitt haben wir auch zwei Änderungen an einigen grundlegenden TCP/IP-Verbindungsparametern erläutert. In einem zweiten Schritt müssen wir den Enqueue-Server für das Senden eines **keep\_alive**-Signals konfigurieren, damit die Verbindungen nicht den Schwellenwert für den Leerlauf des Azure ILB erreichen. Fügen Sie zu diesem Zweck diesen Profilparameter:

```
enque/encni/set_so_keepalive = true
```

dem Profil der SAP ASCS/SCS-Instanz hinzu. Bei unserem Beispiel lautet der Pfad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

z.B. zum Profil der SAP SCS-Instanz und dem zugehörigen Pfad

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`


#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Hinzufügen eines Testports

Damit die gesamte Clusterkonfiguration mit einem Azure Load Balancer funktioniert, müssen wir die Testfunktionalität des internen Load Balancers nutzen. Ein interner Load Balancer von Azure verteilt in der Regel den eingehenden Workload gleichmäßig auf die beteiligten virtuellen Computer. Dies funktioniert jedoch in einer solchen Clusterkonfiguration nicht, da nur eine der Instanzen aktiv und die andere passiv ist und keine Workloads akzeptiert. Um Konfigurationen zu ermöglichen, bei denen der interne Load Balancer von Azure nur den aktiven Instanzen Workloads zuweist, wurde eine Testfunktionalität eingerichtet. Dank dieser Funktionalität kann der interne Load Balancer prüfen, welche der Instanzen aktiv ist, und anschließend nur dieser Instanz den Workload zuweisen. Lassen Sie uns zunächst die aktuelle _**ProbePort**_-Einstellung mit diesem PowerShell-Befehl überprüfen, der auf einem an der Clusterkonfiguration beteiligten virtuellen Computer ausgeführt wird:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter 
```

![Abbildung 56: Testport der Clusterkonfiguration ist standardmäßig 0][sap-ha-guide-figure-3048]

_**Abbildung 56:** Testport der Clusterkonfiguration ist standardmäßig 0_

Standardmäßig ist die Testportnummer auf 0 festgelegt. Damit die Konfiguration funktioniert, muss ein Port definiert werden. In diesem Fall müssen wir den Testport _**62300**_ verwenden, da diese Portnummer in den Azure Resource Manager-Vorlagen für SAP festgelegt ist. Das Zuweisen dieser Portnummer kann mit den beiden folgenden Befehlen erfolgen:

Rufen Sie zuerst den virtuellen SAP-Hostnamen der Clusterressource _**SAP WAC IP**_ ab.

```PowerShell
$var = Get-ClusterResource | Where-Object {  $_.name -eq "SAP PR1 IP"  } 
```

Legen Sie dann den Testport auf 62300 fest.

```PowerShell
$var | Set-ClusterParameter -Multiple @{"Address"="10.0.0.43";"ProbePort"=62300;"Subnetmask"="255.255.255.0";"Network"="Cluster Network 1";"OverrideAddressMatch"=1;"EnableDhcp"=0}  
```

Sie müssen die Clusterrolle _**SAP PR1**_ beenden und neu starten, damit die Änderungen übernommen werden.

Prüfen Sie, nachdem Sie die Clusterrolle _**SAP PR1**_ online geschaltet haben, ob _**ProbePort**_ auf den neuen Wert festgelegt ist:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter 
```

![Abbildung 57: Testport des Clusters nach der Änderung][sap-ha-guide-figure-3049]

_**Abbildung 57:** Testport des Clusters nach der Änderung_

Wie Sie sehen, ist _**ProbePort**_ jetzt auf _**62300**_ festgelegt. Nun können Sie auf die Dateifreigabe _**\\\ascsha-clsap\\sapmnt**_ von anderen Hosts wie _**ascsha-dbas**_ zugreifen.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Installieren der Datenbankinstanz

Die Installation der Datenbankinstanz unterscheidet sich in keiner Weise von der SAP-Installationsdokumentation. Daher wird sie hier nicht dokumentiert.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Installation des zweiten Clusterknotens

Wiederum unterscheidet sich die Installation des zweiten Clusterknotens nicht von der SAP-Installationsdokumentation. Führen Sie daher zum Installieren des zweiten Clusterknotens die Schritte im Installationshandbuch aus.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Ändern des Starttyps des Windows-Diensts der SAP ERS-Instanz

Ändern Sie den Starttyp der Windows-Dienste für SAP ERS auf beiden Clusterknoten in _**Automatisch (Verzögerter Start)**_.

![Abbildung 58: Ändern des Starttyps des Windows-Diensts für die ERS SAP-Instanz in „Automatisch, verzögert“][sap-ha-guide-figure-3050]

_**Abbildung 58:** Ändern des Starttyps des Windows-Diensts für die ERS SAP-Instanz in „Automatisch, verzögert“_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Installation des primären Anwendungsservers von SAP

Führen Sie die Installation der primären Anwendungsserverinstanz `<sid>-di-0` auf dem virtuellen Computer aus, der den primären Anwendungsserver hosten soll. Es bestehen keine Abhängigkeiten mit Azure oder DataKeeper.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Installation des zusätzlichen Anwendungsservers von SAP

Führen Sie die Installation eines zusätzlichen SAP-Anwendungsservers auf allen virtuellen Computern aus, die für das Hosten eines SAP-Anwendungsservers vorgesehen sind, z.B. `<sid>-di-1` bis `<sid>-di-<n>`.

## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Testen des Failovers der SAP ASCS/SCS-Instanz und der Replikation von SIOS

Mithilfe des _**Failovercluster-Managers**_ und der SIOS DataKeeper-Benutzeroberfläche können Sie ein Failover einer SAP ASCS/SCS-Instanz und die SIOS-Datenträgerreplikation problemlos testen und überwachen.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> Ausgangspunkt – Die SAP ASCS/SCS-Instanz wird auf Clusterknoten A ausgeführt

Die Clustergruppe _**SAP WAC**_ wird auf Clusterknoten A ausgeführt, z.B. auf _**ascsha-clna**_. Der freigegebene Datenträger S:, der Teil der Clustergruppe _**SAP WAC**_ ist und von der ASCS/SCS-Instanz verwendet wird, ist Clusterknoten A zugewiesen.

![Abbildung 59: : Failovercluster-Manager: Die SAP-Clustergruppe <SID> wird auf Clusterknoten A ausgeführt][sap-ha-guide-figure-5000]

_**Abbildung 59:** : Failovercluster-Manager: Die SAP-Clustergruppe <SID> wird auf Clusterknoten A ausgeführt_

Auf der Benutzeroberfläche von SIOS DataKeeper erkennen Sie, dass die Daten auf dem freigegebenen Datenträger synchron vom Quellvolume S: auf Clusterknoten A (z.B. _**ascsha-clna [10.0.0.41]**_) auf das Zielvolume _**S:**_ auf Clusterknoten B (z.B. _**ascsha-clnb [10.0.0.42]**_) repliziert werden.

![Abbildung 60: SIOS DataKeeper: Replizieren des lokalen Volumes von Clusterknoten A auf Clusterknoten B][sap-ha-guide-figure-5001]

_**Abbildung 60:** SIOS DataKeeper: Replizieren des lokalen Volumes von Clusterknoten A auf Clusterknoten B_


### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failoverprozess von Knoten A auf Knoten B

Sie können ein Failover der SAP-Clustergruppe <SID> von Clusterknoten A auf Clusterknoten B wie folgt auslösen:

- Mithilfe des Failovercluster-Managers

- Mithilfe der Failovercluster-PowerShell

  ```powershell
  Move-ClusterGroup -Name "SAP WAC" 
  ```

- Durch einen Neustart von Clusterknoten A im Windows-Gastbetriebssystem (dadurch wird ein automatisches Failover der SAP-Clustergruppe <SID> von Knoten A auf Knoten B ausgelöst)

- Durch einen Neustart von Clusterknoten A im Azure-Portal (dadurch wird ein automatisches Failover der SAP-Clustergruppe <SID> von Knoten A auf Knoten B ausgelöst)

- Durch einen Neustart von Clusterknoten A mithilfe von Azure PowerShell (dadurch wird ein automatisches Failover der SAP-Clustergruppe <SID> von Knoten A auf Knoten B ausgelöst)

  ```powershell
  Restart-AzureVM -Name ascsha-clna -ServiceName ascsha-cluster
  ```

### <a name="755a6b93-0099-4533-9f6d-5c9a613878b5"></a> Endergebnis – Die SAP ASCS/SCS-Instanz wird auf Clusterknoten B ausgeführt

Nach dem Failover wird die Clustergruppe `SAP <SID>` auf Clusterknoten B ausgeführt, z.B. auf _**ascsha-clnb**_.

![Abbildung 61: : Failovercluster-Manager: Die SAP-Clustergruppe <SID> wird auf Clusterknoten B ausgeführt][sap-ha-guide-figure-5002]

_**Abbildung 61:** : Failovercluster-Manager: Die SAP-Clustergruppe <SID> wird auf Clusterknoten B ausgeführt_

Der freigegebene Datenträger ist jetzt auf Knoten B bereitgestellt. SIOS DataKeeper repliziert Daten vom Quellvolume S: auf Clusterknoten B (z.B. _**ascsha-clnb [10.0.0.42]**_) auf das Zielvolume S: auf Clusterknoten A (z.B. _**ascsha-clna [10.0.0.41]**_).

![Abbildung 62: SIOS DataKeeper: Replizieren des lokalen Volumes von Clusterknoten B auf Clusterknoten A][sap-ha-guide-figure-5003]

_**Abbildung 62:** SIOS DataKeeper: Replizieren des lokalen Volumes von Clusterknoten B auf Clusterknoten A_

<!---HONumber=AcomDC_0907_2016-->
