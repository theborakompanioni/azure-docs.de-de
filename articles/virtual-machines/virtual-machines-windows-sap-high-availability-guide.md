---
title: SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“ | Microsoft Docs
description: Handbuch zum Thema „Hohe Verfügbarkeit“ für SAP NetWeaver auf virtuellen Windows-Computern
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''

ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/18/2016
ms.author: goraco

---
# <a name="sap-netweaver-on-windows-virtual-machines-vms-highavailability-guide"></a>SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Handbuch zum Thema „Hohe Verfügbarkeit“
[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver auf virtuellen Windows-Computern (VMs) – DBMS-Bereitstellungshandbuch)
[dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching für VMs und VHDs)
[dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID)
[dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Struktur einer RDBMS-Bereitstellung)
[dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern)
[dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 und höher)
[dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 und frühere Versionen)
[dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace)
[dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (SQL Server für SAP in Azure – Allgemeine Zusammenfassung)
[dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Besonderheiten bei SQL Server-RDBMS)
[dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Speicherkonfiguration)
[dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Sichern und Wiederherstellen)
[dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung)
[dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Sonstiges)
[dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Bereitstellungshandbuch)
[deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP-Ressourcen)
[deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image)
[deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP)
[deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP)
[deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP)
[deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Bereitstellungsszenarios für virtuelle Computer für SAP in Microsoft Azure)
[deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Bereitstellen von Azure PowerShell-Cmdlets)
[deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Herunterladen und Importieren von SAP-relevanten PowerShell-Cmdlets)
[deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Einbinden virtueller Computer in die lokale Domäne – nur Windows)
[deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Herunterladen, Installieren und Aktivieren von Azure-VM-Agent)
[deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure-Befehlszeilenschnittstelle)
[deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurieren der erweiterten Azure-Überwachung für SAP)
[deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Bereitschaftsüberprüfung für die erweiterte Azure-Überwachung für SAP)
[deployment-guide-5.2]:virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur)
[deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch)
[planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Ressourcen)
[planning-guide-11]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Hohe Verfügbarkeit (High Availability, HA) und Notfallwiederherstellung (Disaster Recovery, DR) für SAP NetWeaver auf virtuellen Azure-Computern)
[planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (Hohe Verfügbarkeit für SAP-Anwendungsserver)
[planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Verwenden von Autostart für SAP-Instanzen)
[planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk)
[planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk)
[planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure-Regionen)
[planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fehlerdomänen)
[planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgradedomänen)
[planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure-Verfügbarkeitsgruppen)
[planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Das Konzept der virtuellen Microsoft Azure-Computer)
[planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Storage Premium)
[planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure)
[planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bereitstellen eines virtuellen Computers mit einem kundenspezifischen Image)
[planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure)
[planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP)
[planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Vorbereiten virtueller Computer mit SAP für Azure)
[planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Unterschied zwischen einem Azure-Datenträger und einem Azure-Image)
[planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure)
[planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopieren von Datenträgern zwischen Azure-Speicherkonten)
[planning-guide-5.5.1]:virtual-machines-windows-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM-/VHD-Struktur für SAP-Bereitstellungen)
[planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Festlegen der automatischen Bereitstellung für angefügte Datenträger)
[planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Einzelner virtueller Computer mit SAP NetWeaver-Demo/Schulungsszenario)
[planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Konzepte für Nur-Cloud-Bereitstellung von SAP-Instanzen)
[planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure-Überwachungslösung für SAP)
[planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Storage Premium)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[sap-ha-guide]:virtual-machines-windows-sap-high-availability-guide.md (High-availability SAP NetWeaver on Windows virtual machines)
[sap-ha-guide-1]:virtual-machines-windows-sap-high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c (Prerequisites)
[sap-ha-guide-2]:virtual-machines-windows-sap-high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08 (Resources)
[sap-ha-guide-3]:virtual-machines-windows-sap-high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1 (High-availability SAP with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-3.1]:virtual-machines-windows-sap-high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686 (Resource groups)
[sap-ha-guide-3.2]:virtual-machines-windows-sap-high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860 (Clustering with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-4]:virtual-machines-windows-sap-high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7 (Windows Server Failover Clustering)
[sap-ha-guide-4.1]:virtual-machines-windows-sap-high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2 (Quorum modes)
[sap-ha-guide-5]:virtual-machines-windows-sap-high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275 (Windows Server Failover Clustering on-premises)
[sap-ha-guide-5.1]:virtual-machines-windows-sap-high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592 (Shared storage)
[sap-ha-guide-5.2]:virtual-machines-windows-sap-high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd (Networking and name resolution)
[sap-ha-guide-6]:virtual-machines-windows-sap-high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a (Windows Server Failover Clustering in Azure)
[sap-ha-guide-6.1]:virtual-machines-windows-sap-high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341 (Shared disk in Azure with SIOS DataKeeper)
[sap-ha-guide-6.2]:virtual-machines-windows-sap-high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb (Name resolution in Azure)
[sap-ha-guide-7]:virtual-machines-windows-sap-high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa (Hohe Verfügbarkeit für SAP NetWeaver in Azure-IaaS (Infrastructure-as-a-Service))
[sap-ha-guide-7.1]:virtual-machines-windows-sap-high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e (Hoch verfügbare SAP-Anwendungsserver)
[sap-ha-guide-7.2]:virtual-machines-windows-sap-high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db (Hoch verfügbare SAP ASCS/SCS-Instanz)
[sap-ha-guide-7.2.1]:virtual-machines-windows-sap-high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51 (Hoch verfügbare SAP ASCS/SCS-Instanz mit Windows Server-Failoverclustering in Azure)
[sap-ha-guide-7.3]:virtual-machines-windows-sap-high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027 (Hoch verfügbare DBMS-Instanz)
[sap-ha-guide-7.4]:virtual-machines-windows-sap-high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816 (End-to-End-Bereitstellungsszenarios mit hoher Verfügbarkeit)
[sap-ha-guide-8]:virtual-machines-windows-sap-high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf (Vorbereiten der Infrastruktur)
[sap-ha-guide-8.1]:virtual-machines-windows-sap-high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489 (Bereitstellen virtueller Computer mit Verbindungen mit einem Unternehmensnetzwerk (standortübergreifend) für die Verwendung in der Produktion)
[sap-ha-guide-8.2]:virtual-machines-windows-sap-high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310 (Reine Cloudbereitstellung von SAP-Instanzen für Tests und Demonstrationen)
[sap-ha-guide-8.3]:virtual-machines-windows-sap-high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a (Azure Virtual Network)
[sap-ha-guide-8.4]:virtual-machines-windows-sap-high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e (DNS-IP-Adressen)
[sap-ha-guide-8.5]:virtual-machines-windows-sap-high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f (Hostname und statische IP-Adressen für die SAP ASCS/SCS-Clusterinstanz und DBMS-Clusterinstanz)
[sap-ha-guide-8.6]:virtual-machines-windows-sap-high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30 (Festlegen statischer IP-Adressen für virtuelle SAP-Computer)
[sap-ha-guide-8.7]:virtual-machines-windows-sap-high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e (Erstellen einer statischen IP-Adresse für den internen Load Balancer)
[sap-ha-guide-8.8]:virtual-machines-windows-sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c (Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure-Load Balancer)
[sap-ha-guide-8.9]:virtual-machines-windows-sap-high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716 (Ändern der Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure-Load Balancer)
[sap-ha-guide-8.10]:virtual-machines-windows-sap-high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c (Hinzufügen von virtuellen Windows-Computern zur Domäne)
[sap-ha-guide-8.11]:virtual-machines-windows-sap-high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158 (Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten der SAP ASCS/SCS-Instanz)
[sap-ha-guide-8.12]:virtual-machines-windows-sap-high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab (Einrichten eines Clusters mit Windows Server-Failoverclustering für eine SAP ASCS/SCS-Instanz)
[sap-ha-guide-8.12.1]:virtual-machines-windows-sap-high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79 (Erfassen der Clusterknoten in einer Clusterkonfiguration)
[sap-ha-guide-8.12.2]:virtual-machines-windows-sap-high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca (Konfigurieren eines Cluster-Dateifreigabenzeugen)
[sap-ha-guide-8.12.2.1]:virtual-machines-windows-sap-high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855 (Erstellen einer Dateifreigabe share)
[sap-ha-guide-8.12.2.2]:virtual-machines-windows-sap-high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d (Festlegen des Quorums für den Dateifreigabenzeugen im Failovercluster-Manager)
[sap-ha-guide-8.12.3]:virtual-machines-windows-sap-high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868 (Installieren von SIOS DataKeeper Cluster Edition für SAP ASCS/SCS-Clusterfreigabe-Datenträger)
[sap-ha-guide-8.12.3.1]:virtual-machines-windows-sap-high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3 (Hinzufügen von .NET Framework 3.5)
[sap-ha-guide-8.12.3.2]:virtual-machines-windows-sap-high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102 (Installieren von SIOS DataKeeper)
[sap-ha-guide-8.12.3.3]:virtual-machines-windows-sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 (Einrichten von SIOS DataKeeper)
[sap-ha-guide-9]:virtual-machines-windows-sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b (Installieren des SAP NetWeaver-Systems)
[sap-ha-guide-9.1]:virtual-machines-windows-sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 (Installieren von SAP mit einer hoch verfügbaren ASCS/SCS-Instanz)
[sap-ha-guide-9.1.1]:virtual-machines-windows-sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 (Erstellen eines Namens für den virtuellen Host der SAP ASCS/SCS-Clusterinstanz)
[sap-ha-guide-9.1.2]:virtual-machines-windows-sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 (Installieren des ersten SAP-Clusterknotens)
[sap-ha-guide-9.1.3]:virtual-machines-windows-sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 (Ändern des SAP-Profils der ASCS/SCS-Instanz)
[sap-ha-guide-9.1.4]:virtual-machines-windows-sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 (Hinzufügen eines Testports)
[sap-ha-guide-9.2]:virtual-machines-windows-sap-high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7 (Installieren der Datenbankinstanz)
[sap-ha-guide-9.3]:virtual-machines-windows-sap-high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748 (Installieren des zweiten Clusterknotens)
[sap-ha-guide-9.4]:virtual-machines-windows-sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a (Ändern des Starttyps der Windows-Dienstinstanz für SAP ERS)
[sap-ha-guide-9.5]:virtual-machines-windows-sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 (Installieren des primären SAP-Anwendungsservers)
[sap-ha-guide-9.6]:virtual-machines-windows-sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 (Installieren des zusätzlichen SAP-Anwendungsservers)
[sap-ha-guide-10]:virtual-machines-windows-sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9 (Testen des Failovers der SAP ASCS/SCS-Instanz und der Replikation von SIOS)
[sap-ha-guide-10.1]:virtual-machines-windows-sap-high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445 (Die SAP ASCS/SCS-Instanz wird auf Clusterknoten A ausgeführt)
[sap-ha-guide-10.2]:virtual-machines-windows-sap-high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916 (Failover von Knoten A nach Knoten B)
[sap-ha-guide-10.3]:virtual-machines-windows-sap-high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5 (Die SAP ASCS/SCS-Instanz wird auf Clusterknoten B ausgeführt))


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png


[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../resource-manager-deployment-model.md#benefits-of-using-resource-manager-and-resource-groups
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:virtual-machines-windows-portal-sql-alwayson-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md


Azure Virtual Machines eignet sich als Lösung für Organisationen, die Compute-, Speicher- und Netzwerkressourcen in kürzester Zeit und ohne langwierige Beschaffungszyklen benötigen. Sie können Azure Virtual Machines verwenden, um klassische Anwendungen wie SAP NetWeaver-basiertes ABAP und Java sowie einen ABAP+Java-Stapel bereitzustellen. Erweitern Sie die Zuverlässigkeit und Verfügbarkeit ohne zusätzliche lokale Ressourcen. Da Azure Virtual Machines standortübergreifende Konnektivität unterstützt, kann Ihre Organisation Azure Virtual Machines in Ihre lokalen Domänen, privaten Clouds und die SAP-Systemumgebung integrieren.

In diesem Artikel werden die Schritte beschrieben, mit denen Sie hoch verfügbare SAP-Systeme in Azure mit dem neuen Azure Resource Manager-Bereitstellungsmodell bereitstellen. Dabei werden die folgenden Hauptaufgaben behandelt:

* Suchen Sie die richtigen SAP-Installationshandbücher und -hinweise im Abschnitt [Ressourcen][sap-ha-guide-2]. Dieser Artikel ergänzt die SAP-Dokumentation und -Hinweise zur Installation. Diese stellen die primären Ressourcen für das Installieren und Bereitstellen von SAP-Software auf bestimmten Plattformen dar.
* Informieren Sie sich über die Unterschiede zwischen dem klassischen Azure-Bereitstellungsmodell und dem Azure Resource Manager-Bereitstellungsmodell.
* Informieren Sie sich über die Quorummodi für das Windows Server-Failoverclustering, damit Sie das für Ihre Azure-Bereitstellung geeignete Modell auswählen können.
* Informieren Sie sich über freigegebenen Speicher für das Windows Server-Failoverclustering in Azure-Diensten.
* Informieren Sie sich darüber, wie Sie Single Point of Failure-Komponenten wie ABAP SAP Central Services (ASCS)/SAP Central Services (SCS) und Datenbank-Managementsysteme (DBMS) sowie redundante Komponenten wie SAP-Anwendungsserver in Azure schützen.
* Führen Sie ein ausführliches Beispiel einer Installation und Konfiguration eines SAP-Systems mit hoher Verfügbarkeit in einem Windows Server-Failoverclustering-Cluster mithilfe von Azure als Plattform mit dem Azure Resource Manager durch.
* Informieren Sie sie über die weiteren erforderlichen Schritte zum Verwenden von Windows Server-Failoverclustering in Azure, die bei einer lokalen Bereitstellung nicht erforderlich sind.

Zur Vereinfachung der Bereitstellung und Konfiguration verwenden wir in diesem Artikel die neuen Resource Manager-Vorlagen für hohe Verfügbarkeit bei SAP mit drei Ebenen. Die Vorlagen automatisieren die Bereitstellung der gesamten Infrastruktur, die Sie für ein SAP-System mit hoher Verfügbarkeit benötigen. Die Infrastruktur unterstützt auch SAPS-Größenanpassungen für das SAP-System.

[!INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="a-name217c547955954cd8870d15ab00d4f84ca-prerequisites"></a><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Voraussetzungen
Achten Sie vor Beginn darauf, dass die in den folgenden Abschnitten beschriebenen Voraussetzungen erfüllt sind. Überprüfen Sie darüber hinaus alle Ressourcen im Abschnitt [Ressourcen][sap-ha-guide-2].

In diesem Artikel verwenden wir Azure Resource Manager-Vorlagen für [SAP NetWeaver mit drei Ebenen](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Eine hilfreiche Übersicht über die Vorlagen finden Sie unter [Azure Resource Manager-Vorlagen für SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="a-name42b8f6007ba34606b8a553c4f026da08a-resources"></a><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Ressourcen
Diese Anleitungen decken auch SAP-Bereitstellungen in Azure ab:

* [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch][planning-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern – Bereitstellungshandbuch][deployment-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]
* [SAP NetWeaver auf virtuellen Windows-Computern – Handbuch zum Thema „Hohe Verfügbarkeit“ [dieses Handbuch] ][sap-ha-guide]

> [!NOTE]
> Sofern möglich, wird ein Link zum entsprechenden SAP-Installationshandbuch bereitgestellt (siehe [SAP-Installationshandbücher][sap-installation-guides]). Wenn Sie Informationen zu den Voraussetzungen und zum Installationsvorgang benötigen, sollten Sie die SAP NetWeaver-Installationshandbücher sorgfältig lesen. Dieser Artikel behandelt nur die spezifischen Aufgaben für SAP NetWeaver-basierte Systeme, die Sie mit Azure Virtual Machines verwenden können.
> 
> 

Diese SAP-Hinweise beziehen sich auf das Thema SAP in Azure:

| Hinweisnummer | Titel |
| --- | --- |
| [1928533] |SAP-Anwendungen in Azure: unterstützte Produkte und Größen |
| [2015553] |SAP on Microsoft Azure: Support Prerequisites (SAP in Microsoft Azure: Voraussetzungen für die Unterstützung) |
| [1999351] |Enhanced Azure Monitoring for SAP (Erweiterte Azure-Überwachung für SAP) |
| [2178632] |Wichtige Überwachungsmetriken für SAP in Microsoft Azure |
| [1999351] |Virtualisierung unter Windows: erweiterte Überwachung |

Informieren Sie sich auch über die [Einschränkungen von Azure-Abonnements][azure-subscription-service-limits-subscription], einschließlich der allgemeinen Standard- und Maximaleinschränkungen.

## <a name="a-name42156640c601cf45a9b2254baa678b24f1ahighavailability-sap-with-azure-resource-manager-vs-the-classic-deployment-model"></a><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Vergleich zwischen dem Azure Resource Manager- und dem klassischen Bereitstellungsmodell für SAP mit hoher Verfügbarkeit
Das Azure Resource Manager-Bereitstellungsmodell und das klassische Bereitstellungsmodell unterscheiden sich in zwei Punkten:

* Ressourcengruppen
* Voraussetzungen für das Clustering

### <a name="a-namef76af27319934d83b12d65deeae23686a-resource-groups"></a><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Ressourcengruppen
In Azure Resource Manager können Sie mit Ressourcengruppen alle Anwendungsressourcen im Azure-Abonnement verwalten. Bei einem integrierten Ansatz haben alle Ressourcen in einer Ressourcengruppe denselben Lebenszyklus. So werden z.B. alle Ressourcen zur gleichen Zeit erstellt und gelöscht. Weitere Informationen über Ressourcengruppen finden Sie in [diesem Artikel](../resource-group-overview.md#resource-groups).

### <a name="a-name3e85fbe084b1489287afd9b65ff91860a-clustering-with-azure-resource-manager-vs-the-classic-deployment-model"></a><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Vergleich zwischen dem Azure Resource Manager- und dem klassischen Bereitstellungsmodell beim Clustering
Im Azure Resource Manager-Modell benötigen Sie keinen Clouddienst, um den internen Lastenausgleich von Azure für die hohe Verfügbarkeit zu verwenden.

Wenn Sie das klassische Azure-Modell verwenden möchten, führen Sie die Schritte in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056) (SAP NetWeaver in Azure – Clustering von SAP ASCS/SCS-Instanzen mit dem Windows Server-Failoverclustering in Azure mit SIOS DataKeeper) aus.

> [!NOTE]
> Es wird dringend empfohlen, das Azure Resource Manager-Bereitstellungsmodell für Ihre SAP-Installationen zu verwenden. Es bietet viele Vorteile, die beim klassischen Bereitstellungsmodell nicht zur Verfügung stehen. Informieren Sie sich über die Azure-[Bereitstellungsmodelle][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
> 
> 

## <a name="a-name8ecf3ba067c044959c14feec1a2255b7a-windows-server-failover-clustering"></a><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server-Failoverclustering
Windows Server-Failoverclustering ist die Grundlage für eine SAP ASCS/SCS-Installation und ein DBMS in Windows mit hoher Verfügbarkeit.

Bei einem Failovercluster handelt es sich um eine Gruppe von 1+n unabhängigen Servern, die zur Steigerung der Verfügbarkeit von Anwendungen und Diensten zusammenarbeiten. Wenn ein Knotenfehler auftritt, berechnet das Windows Server-Failoverclustering die Anzahl der Fehler, die auftreten können, um trotzdem einen fehlerfreien Cluster für die Bereitstellung der definierten Anwendungen und Dienste zu erhalten. Sie können dazu zwischen verschiedenen Quorummodi auswählen.

### <a name="a-name1a3c5408b16846d699f54219ad1b1ff2a-quorum-modes"></a><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorummodi
Sie können zwischen vier Quorummodi auswählen, wenn Sie das Windows Server-Failoverclustering verwenden:

* **Knotenmehrheit:** Jeder Knoten des Clusters kann abstimmen. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte der Stimmen. Diese Option wird für Cluster mit einer ungeraden Anzahl von Knoten empfohlen. So können z.B. drei Knoten in einem Cluster mit sieben Knoten einen Ausfall verursachen. Der Cluster erreicht immer noch eine Mehrheit und wird weiterhin ausgeführt.  
* **Knoten- und Datenträgermehrheit:** Alle Knoten plus ein festgelegter Datenträger im Clusterspeicher (der Datenträgerzeuge) können abstimmen, solange sie verfügbar sind und kommunizieren. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte der Stimmen. Dieser Modus ist in einer Clusterumgebung mit einer geraden Anzahl von Knoten sinnvoll. Solange die Hälfte der Knoten sowie der Datenträgerzeuge online sind, verbleibt der Cluster in einem ordnungsgemäßen Zustand.
* **Knoten- und Dateifreigabemehrheit:** Alle Knoten sowie eine vom Administrator erstellte zugewiesene Dateifreigabe (der Dateifreigabenzeuge) können abstimmen – unabhängig davon, ob sie verfügbar sind und kommunizieren. Der Cluster funktioniert nur mit der Mehrheit der Stimmen, d.h. mit mehr als der Hälfte der Stimmen. Dieser Modus ist in einer Clusterumgebung mit einer geraden Anzahl von Knoten sinnvoll. Er ähnelt dem Modus mit Knoten- und Datenträgermehrheit, verwendet jedoch einen Dateifreigabenzeugen anstelle eines Datenträgerzeugen. Die Implementierung dieses Modus ist einfach, doch wenn die Dateifreigabe selbst nicht hoch verfügbar ist, kann diese Lösung ein Single Point of Failure (SPOF) sein.
* **Keine Mehrheit – nur Datenträger:** Der Cluster hat ein Quorum, wenn ein Knoten verfügbar ist und mit einem bestimmten Datenträger im Clusterspeicher kommuniziert. Nur die Knoten, die auch mit diesem Datenträger kommunizieren, können dem Cluster beizutreten. Die Verwendung dieses Modus wird nicht empfohlen.
   
  
  ## <a name="a-namefdfee8756e66483aa34314bbaee33275a-windows-server-failover-clustering-onpremises"></a><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Lokales Windows Server-Failoverclustering
  Das Beispiel in Abbildung 1 zeigt einen Cluster mit zwei Knoten. Wenn die Netzwerkverbindung zwischen den Knoten unterbrochen wird, aber beide Knoten weiter ausgeführt werden, bestimmt ein Quorumdatenträger oder eine Quorumdateifreigabe, welcher Knoten die Clusteranwendungen und -dienste weiter bereitstellt. Der Knoten, der Zugriff auf den Quorumdatenträger oder die Quorumdateifreigabe hat, ist derjenige, der die weitere Ausführung der Dienste gewährleistet.

Da in diesem Beispiel ein Cluster mit zwei Knoten verwendet wird, verwenden wir den Quorummodus „Knoten- und Dateifreigabemehrheit“. Der Modus „Knoten- und Datenträgermehrheit“ ist auch eine mögliche Option. In einer Produktionsumgebung wird die Verwendung eines Quorumdatenträgers empfohlen. Sie können die Technologien für die Netzwerk- und Speichersysteme verwenden, um diese hoch verfügbar zu machen.

![Abbildung 1: Beispiel für eine Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure][sap-ha-guide-figure-1000]

***Abbildung 1:** Beispiel für eine Windows Server-Failoverclusterkonfiguration für SAP ASCS/SCS in Azure*

### <a name="a-namebe21cf3efb01402b995554fbecf66592a-shared-storage"></a><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Freigegebener Speicher
Abbildung 1 zeigt auch einen Cluster mit zwei Knoten und freigegebenem Speicher. In einem lokalen Cluster mit freigegebenem Speicher erkennen alle Knoten im Cluster den freigegebenen Speicher. Mithilfe eines Sperrmechanismus werden die Daten vor Beschädigung geschützt. Alle Knoten können erkennen, wenn ein anderer Knoten ausfällt. Wenn ein Knoten ausfällt, übernehmen die restlichen den Besitz der Speicherressourcen und stellen die Verfügbarkeit der Dienste sicher.

> [!NOTE]
> Sie benötigen bei einigen DBMS-Anwendungen wie SQL Server für eine hohe Verfügbarkeit keine freigegebenen Datenträger. SQL Server Always On führt die Replikation von DBMS-Daten- und -Protokolldateien vom lokalen Datenträger eines Clusterknotens auf den lokalen Datenträger eines anderen Clusterknotens durch. In diesem Fall ist bei der Windows-Clusterkonfiguration kein freigegebener Datenträger erforderlich.
> 
> 

### <a name="a-nameff7a9a062bc54b20860a46cdb44669cda-networking-and-name-resolution"></a><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Netzwerk und Namensauflösung
Clientcomputer erreichen den Cluster über eine virtuelle IP-Adresse und den virtuellen Hostnamen, der vom DNS-Server bereitgestellt wird. Die lokalen Knoten und der DNS-Server können mehrere IP-Adressen verwenden.

Bei einer normalen Konfiguration können Sie zwei oder mehr Netzwerkverbindungen verwenden:

* Eine dedizierte Verbindung mit dem Speicher
* Eine clusterinterne Netzwerkverbindung für den Takt
* Ein von den Clients verwendetes öffentliches Netzwerk zum Herstellen der Verbindung mit dem Cluster

## <a name="a-name2ddba413a7f54e4e9a5187908879c10aa-windows-server-failover-clustering-in-azure"></a><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server-Failoverclustering in Azure
Im Vergleich mit Bare-Metal- oder privaten Cloudbereitstellungen sind für virtuelle Azure-Computer zusätzliche Schritte erforderlich, um Windows Server-Failoverclustering zu konfigurieren. Wenn Sie einen freigegebenen Clusterdatenträger erstellen, müssen Sie mehrere IP-Adressen und virtuelle Hostnamen für die SAP ASCS/SCS-Instanz festlegen.

In diesem Artikel werden die wichtigsten Konzepte und die zusätzlichen Schritte besprochen, die erforderlich sind, wenn Sie einen SAP Central Services-Cluster mit hoher Verfügbarkeit in Azure erstellen. Wir zeigen Ihnen, wie Sie das Drittanbietertool SIOS DataKeeper einrichten und den internen Load Balancer von Azure konfigurieren. Mit diesen Tools können Sie einen Windows-Failovercluster mit einem Dateifreigabenzeugen in Azure erstellen.

![Abbildung 2: Windows Server-Failoverclusteringkonfiguration in Azure ohne freigegebenen Datenträger][sap-ha-guide-figure-1001]

***Abbildung 2:** Windows Server-Failoverclusteringkonfiguration in Azure ohne freigegebenen Datenträger*

### <a name="a-name1a464091922b48d79d087cecf757f341a-shared-disk-in-azure-with-sios-datakeeper"></a><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Freigegebener Datenträger in Azure mit SIOS DataKeeper
Sie benötigen freigegebenen Clusterspeicher für eine hoch verfügbare SAP ASCS/SCS-Instanz. Ab September 2016 bietet Azure keinen freigegebenen Speicher zum Erstellen eines Clusters mit freigegebenem Speicher mehr an. Die Drittanbietersoftware SIOS DataKeeper Cluster Edition ermöglicht es Ihnen, einen gespiegelten Speicher zu erstellen, der freigegebenen Clusterspeicher simuliert. Die SIOS-Lösung bietet eine synchrone Datenreplikation in Echtzeit. Eine freigegebene Datenträgerressource für einen Cluster wird folgendermaßen erstellt:

1. Fügen Sie eine zusätzliche Azure-VHD an sämtliche virtuelle Computer (VMs) in einer Windows-Clusterkonfiguration an.
2. Führen Sie SIOS DataKeeper Cluster Edition auf beiden virtuellen Computerknoten aus.
3. Konfigurieren Sie SIOS DataKeeper Cluster Edition so, dass synchron der Inhalt des zusätzlich angehängten VHD-Volumes vom virtuellen Quellcomputer im zusätzlich angehängten VHD-Volume des virtuellen Zielcomputers gespiegelt wird. SIOS DataKeeper abstrahiert die lokalen Quell- und Zielvolumes und präsentiert diese beim Windows Server-Failoverclustering als einen freigegebenen Datenträger.

Weitere Informationen zu [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

 ![Abbildung 3: Windows Server-Failoverclusteringkonfiguration in Azure mit SIOS DataKeeper][sap-ha-guide-figure-1002]

***Abbildung 3:** Windows Server-Failoverclusteringkonfiguration in Azure mit SIOS DataKeeper*

> [!NOTE]
> Sie benötigen bei einigen DBMS-Produkten wie SQL Server für eine hohe Verfügbarkeit keine freigegebenen Datenträger. SQL Server Always On führt die Replikation von DBMS-Daten- und -Protokolldateien vom lokalen Datenträger eines Clusterknotens auf den lokalen Datenträger eines anderen Clusterknotens durch. In diesem Fall ist bei der Windows-Clusterkonfiguration kein freigegebener Datenträger erforderlich.
> 
> 

### <a name="a-name44641e18a94e431f95ff303ab65e0bcba-name-resolution-in-azure"></a><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Namensauflösung in Azure
 Die Azure-Cloudplattform bietet keine Möglichkeit, virtuelle IP-Adressen wie z.B. Floating IP-Adressen zu konfigurieren. Aus diesem Grund benötigen Sie eine alternative Lösung für die Einrichtung einer virtuellen IP-Adresse, um die Clusterressource in der Cloud zu erreichen.
Azure stellt über den Azure Load Balancer-Dienst einen internen Lastenausgleich zur Verfügung. Mit dem internen Load Balancer erreichen Clients den Cluster über die virtuelle IP-Adresse des Clusters.
Sie müssen den internen Load Balancer in der Ressourcengruppe bereitstellen, die die Clusterknoten enthält. Anschließend konfigurieren Sie alle erforderlichen Portweiterleitungsregeln mithilfe der Testports des internen Load Balancers.
Die Clients können über den virtuellen Hostnamen eine Verbindung herstellen. Der DNS-Server löst die IP-Adresse des Clusters auf. Der interne Load Balancer übernimmt die Weiterleitung an den aktiven Knoten des Clusters.

## <a name="a-name2e3fec50241e441b87080b1864f66dfaa-highavailability-sap-netweaver-in-azure-infrastructureasaservice-iaas"></a><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> Hohe Verfügbarkeit von SAP NetWeaver in Azure IaaS (Infrastructure-as-a-Service)
Um hohe Verfügbarkeit für SAP-Anwendungen zu erreichen, z.B. für SAP-Softwarekomponenten, müssen Sie die folgenden Komponenten schützen. Ausführlichere Informationen dazu finden Sie unter [SAP NetWeaver auf virtuellen Windows-Computern (VMs) – Planungs- und Implementierungshandbuch][planning-guide-11].

* SAP-Anwendungsserver
* SAP ASCS/SCS-Instanz
* DBMS-Server

### <a name="a-name93faa747907e440ab00a1ae0a89b1c0ea-highavailability-sap-application-servers"></a><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> Hohe Verfügbarkeit für SAP-Anwendungsserver
Für die SAP-Anwendungsserver und -Dialoginstanzen ist meist keine spezielle hoch verfügbare Lösung erforderlich. Sie erreichen hohe Verfügbarkeit durch Redundanz und indem Sie mehrere Dialoginstanzen in verschiedenen Instanzen von virtuellen Azure-Computern konfigurieren. Es müssen mindestens zwei SAP-Anwendungsinstanzen auf zwei Instanzen von virtuellen Azure-Computern installiert sein.

![Abbildung 4: Hohe Verfügbarkeit für SAP-Anwendungsserver][sap-ha-guide-figure-2000]

***Abbildung 4:** Hohe Verfügbarkeit für SAP-Anwendungsserver*

Alle virtuellen Computer, auf denen SAP-Anwendungsserver gehostet werden, müssen in derselben Azure-Verfügbarkeitsgruppe platziert werden. Eine Azure-Verfügbarkeitsgruppe stellt Folgendes sicher:

* Alle virtuellen Computer sind Teil derselben Upgradedomäne. Eine Upgradedomäne stellt beispielsweise sicher, dass die virtuellen Computer nicht gleichzeitig mit geplanten Wartungsausfallzeiten aktualisiert werden.
* Alle virtuellen Computer sind Teil derselben Fehlerdomäne. Eine Fehlerdomäne stellt beispielsweise bei der Bereitstellung virtueller Computer sicher, dass kein Single Point of Failure Auswirkungen auf die Verfügbarkeit aller virtuellen Computer hat.

Informieren Sie sich über das [Verwalten der Verfügbarkeit virtueller Computer][virtual-machines-manage-availability].

Da das Azure-Speicherkonto ein potenzieller Single Point of Failure sein kann, benötigen Sie unbedingt mindestens zwei Azure-Speicherkonten, auf die mindestens zwei virtuelle Computer verteilt sind. In einer idealen Konfiguration würde jeder virtuelle Computer, auf dem eine SAP-Dialoginstanz ausgeführt wird, in einem anderen Speicherkonto bereitgestellt werden.

### <a name="a-namef559c285ee684eecadd1f60fe7b978dba-highavailability-sap-ascsscs-instance"></a><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> Hohe Verfügbarkeit für SAP ASCS/SCS-Instanzen
![Abbildung 5: Hohe Verfügbarkeit für SAP ASCS/SCS-Instanzen][sap-ha-guide-figure-2001]

***Abbildung 5:** Hohe Verfügbarkeit für SAP ASCS/SCS-Instanzen*

#### <a name="a-nameb5b1fd0b1db44d499162de07a0132a51a-highavailability-sap-ascsscs-instance-with-windows-server-failover-clustering-in-azure"></a><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>Hohe Verfügbarkeit für SAP ASCS/SCS-Instanzen mit Windows-Failoverclustering in Azure
Im Vergleich mit Bare-Metal- oder privaten Cloudbereitstellungen sind für virtuelle Azure-Computer zusätzliche Schritte erforderlich, um Windows Server-Failoverclustering zu konfigurieren. Zum Erstellen eines Windows-Failoverclusters sind für das Clustern einer SAP ASCS/SCS-Instanz ein freigegebener Clusterdatenträger, mehrere IP-Adressen und virtuelle Hostnamen und ein interner Azure Load Balancer erforderlich.

Dies wird später in diesem Artikel ausführlicher erläutert.

![Abbildung 6: Windows Server-Failoverclusteringkonfiguration für eine SAP ASCS/SCS-Instanz in Azure mit SIOS DataKeeper][sap-ha-guide-figure-1002]

***Abbildung 6:** Windows Server-Failoverclusteringkonfiguration für eine SAP ASCS/SCS-Instanz in Azure mit SIOS DataKeeper*

### <a name="a-nameddd878a09c2f4b8e896826ce60be1027a-highavailability-dbms-instance"></a><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a> Hohe Verfügbarkeit für DBMS-Instanzen
Das DBMS ist auch eine zentrale Kontaktstelle für ein SAP-System. Sie sollten es mit einer Lösung mit hoher Verfügbarkeit schützen. Abbildung 7 zeigt ein Beispiel für eine SQL Server Always On-Lösung mit hoher Verfügbarkeit in Azure unter Verwendung von Windows Server-Failoverclustering und des internen Azure Load Balancers. SQL Server Always On repliziert DBMS-Daten- und -Protokolldateien mithilfe seiner eigenen DBMS-Replikation. In diesem Fall benötigen Sie keine freigegebenen Clusterdatenträger, sodass die gesamte Einrichtung vereinfacht wird.

![Abbildung 7: Beispiel für ein SAP-DBMS mit hoher Verfügbarkeit: SQL Server Always On][sap-ha-guide-figure-2003]

***Abbildung 7:** Beispiel für ein SAP-DBMS mit hoher Verfügbarkeit: SQL Server Always On*

Weitere Informationen zum Clustering von SQL Server in Azure mithilfe des Azure Resource Manager-Bereitstellungsmodells finden Sie in den folgenden Artikeln:

* [Manuelles Konfigurieren von Always On-Verfügbarkeitsgruppen in Azure Virtual Machines mit dem Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Konfigurieren eines internen Load Balancers für eine Always On-Verfügbarkeitsgruppe in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

### <a name="a-name045252ed02774fc88f46c5a29694a816a-endtoend-highavailability-deployment-scenarios"></a><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-End-Bereitstellungsszenarios mit hoher Verfügbarkeit
Abbildung 8 zeigt ein Beispiel für eine SAP NetWeaver-Architektur mit hoher Verfügbarkeit in Azure. In diesem Szenario verwenden wir einen dedizierten Cluster für die SAP ASCS/SCS-Instanz und eine weitere für das DBMS.

![Abbildung 8: SAP-Architekturvorlage 1 für hohe Verfügbarkeit mit dediziertem Cluster für ASCS/SCS und dediziertem Cluster für die DBMS-Instanz][sap-ha-guide-figure-2004]

***Abbildung 8:** SAP-Architekturvorlage 1 für hohe Verfügbarkeit: dedizierte Cluster für ASCS/SCS und DBMS*

## <a name="a-name78092dbe165b454c92f54972bdbef9bfa-prepare-the-infrastructure"></a><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Vorbereiten der Infrastruktur
Azure Resource Manager-Vorlagen für SAP vereinfachen die Bereitstellung der erforderlichen Ressourcen.

Die Vorlagen mit drei Ebenen unterstützen auch Szenarios mit hoher Verfügbarkeit, z.B. die Architekturvorlage 1, die über zwei Cluster verfügt. Jeder Cluster ist ein SAP-Single Point of Failure für SAP ASCS/SCS und das DBMS.

Azure Resource Manager-Vorlagen für Szenario 1 sind hier verfügbar:

* [Azure Marketplace-Image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Benutzerdefiniertes Image](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Wenn Sie das SAP-Marketplace-Image mit drei Ebenen auswählen, wird im Azure-Portal dieser Bildschirm angezeigt:

![Abbildung 9: Angeben von Azure Resource Manager-Parametern für hohe Verfügbarkeit von SAP][sap-ha-guide-figure-3000]

***Abbildung 9:** Angeben von Azure Resource Manager-Parametern für hohe Verfügbarkeit von SAP*

Wählen Sie in **SYSTEMAVAILABILITY** die Option **HA** aus.

Mit den Vorlagen wird Folgendes erstellt:

* **Virtuelle Computer:**
  * Virtuelle Computer für SAP-Anwendungsserver: <*SAP-System-SID*>-di-<*Anzahl*>
  * Virtuelle Computer für den ASCS/SCS-Cluster: <*SAP-System-SID*>-ascs-<*Anzahl*>
  * Ein DBMS-Cluster: <*SAP-System-SID*>-db-<*Anzahl*>
* **Netzwerkkarten für alle virtuellen Computer mit zugeordneten IP-Adressen:**
  * <*SAP-System-SID*>-nic-di-<*Anzahl*>
  * <*SAP-System-SID*>-nic-ascs-<*Anzahl*>
  * <*SAP-System-SID*>-nic-db-<*Anzahl*>
* **Azure-Speicherkonten**
* **Verfügbarkeitsgruppen** für:
  * Virtuelle Computer für SAP-Anwendungsserver: <*SAP-System-SID*>-avset-di
  * Virtuelle Computer für den SAP ASCS/SCS-Cluster: <*SAP-System-SID*>-avset-ascs
  * Virtuelle Computer für das DBMS: <*SAP-System-SID*>-avset-db
* **Interner Azure Load Balancer:**
  * Mit allen Ports für die ASCS/SCS-Instanz und IP-Adresse <*SAP-System-SID*>-lb-ascs
  * Mit allen Ports für das SQL Server-DBMS und IP-Adresse <*SAP-System-SID*>-lb-db
* **Netzwerksicherheitsgruppe:** <*SAP-System-SID*>-nsg-ascs-0  
  * Mit einem offenen externen RDP-Port (Remote Desktop Protocol) zum virtuellen Computer <*SAP-System-SID*>-ascs-0

> [!NOTE]
> Sämtliche IP-Adressen für die Netzwerkkarten und den internen Azure Load Balancer sind standardmäßig **dynamisch**. Ändern Sie diese in **statische** IP-Adressen. Dies wird weiter unten in diesem Artikel beschrieben.
> 
> 

### <a name="a-namec87a8d3fb1dc4d2fb23cda4b72977489a-deploy-virtual-machines-with-corporate-network-connectivity-crosspremises-to-use-in-production"></a><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Bereitstellen virtueller Computer mit Verbindungen mit dem Unternehmensnetzwerk (standortübergreifend) für die Verwendung in der Produktion
Stellen Sie für SAP-Produktionssysteme virtuelle Azure-Computer mit einer [(standortübergreifenden) Verbindung mit dem Unternehmensnetzwerk][planning-guide-2.2] bereit. Verwenden Sie dazu ein Azure-Site-to-Site-VPN oder Azure ExpressRoute.

> [!NOTE]
> Sie können Ihre Azure Virtual Network-Instanz verwenden. Das virtuelle Netzwerk und das Subnetz wurden bereits erstellt und vorbereitet.
> 
> 

Wählen Sie in **NEWOREXISTINGSUBNET** die Option **existing** aus.

Fügen Sie in **SUBNETID** die vollständige Zeichenfolge Ihrer vorbereiteten Subnetz-ID für das Azure-Netzwerk ein, in dem Sie Ihre virtuellen Azure-Computer bereitstellen möchten.

Führen Sie den folgenden PowerShell-Befehl aus, um eine Liste aller Azure-Netzwerksubnetze abzurufen:

```powershell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
```

Das Feld **ID** zeigt die **SUBNETID** an.

Mit dem folgenden PowerShell-Befehl können Sie eine Liste aller **SUBNETID**-Werte abrufen:

```PowerShell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
```

Die **SUBNETID** sieht in etwa wie folgt aus:

```
/subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
```

### <a name="a-name7fe9af0e3cce495ba5ecdcb4d8e0a310a-cloudonly-deployment-of-sap-instances-for-test-and-demo"></a><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Reine Cloudbereitstellung von SAP-Instanzen zu Test- und Demonstrationszwecken
Sie können Ihr SAP-System mit hoher Verfügbarkeit auch in einem reinen Cloudbereitstellungsmodell bereitstellen.

Diese Art der Bereitstellung eignet sich hauptsächlich für Demonstrations- oder Testzwecke. Es eignet sich nicht für den Einsatz in Produktionsumgebungen.

Wählen Sie in **NEWOREXISTINGSUBNET** die Option **new** aus. Lassen Sie das Feld **SUBNETID** leer.

Das virtuelle Azure-Netzwerk und das Subnetz werden von der Azure Resource Manager-Vorlage für SAP automatisch erstellt.

> [!NOTE]
> Außerdem müssen Sie mindestens einen dedizierten virtuellen Computer für Active Directory und DNS in der gleichen Instanz von Azure Virtual Network bereitstellen. Die Vorlage erstellt diese virtuellen Computer nicht.
> 
> 

### <a name="a-name47d5300aa83041d483dd1a0d1ffdbe6aa-azure-virtual-network"></a><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Virtuelles Azure-Netzwerk (Azure VNET)
Bei unserem Beispiel ist der Adressbereich des virtuellen Azure-Netzwerks 10.0.0.0/16. Es gibt ein Subnetz mit dem Namen **Subnet** und dem Adressbereich 10.0.0.0/24. Alle virtuellen Computer und internen Load Balancer werden in diesem virtuellen Netzwerk bereitgestellt.

> [!NOTE]
> Nehmen Sie keine Änderungen an den Netzwerkeinstellungen innerhalb des Gastbetriebssystems vor. Dies schließt IP-Adressen, DNS-Server und Subnetz ein. Konfigurieren Sie alle Einstellungen für Ihr Netzwerk in Azure. Der DHCP-Dienst (Dynamic Host Configuration Protocol) übermittelt diese Einstellungen.
> 
> 

### <a name="a-nameb22d7b3b434340ffa319097e13f62f9ea-dns-ip-addresses"></a><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS-IP-Adressen
Stellen Sie sicher, dass die Option **DNS-Server** für Ihr virtuelles Netzwerk auf **Benutzerdefiniertes DNS** festgelegt ist.
Wählen Sie dann die Einstellungen basierend auf dem Typ des Netzwerks aus:

* [(Standortübergreifende) Verbindung mit dem Unternehmensnetzwerk][planning-guide-2.2]: Fügen Sie die IP-Adressen der lokalen DNS-Server hinzu.  
  
    Sie können lokale DNS-Server auf die virtuellen Computer erweitern, die in Azure ausgeführt werden. In diesem Szenario können Sie die IP-Adressen der virtuellen Azure-Computer hinzufügen, auf denen den DNS-Dienst ausgeführt wird.
* [Nur-Cloud-Bereitstellung][planning-guide-2.1]: Stellen Sie einen zusätzlichen virtuellen Computer in derselben Virtual Network-Instanz bereit, die auch als DNS-Server fungiert. Fügen Sie die IP-Adressen der virtuellen Azure-Computer hinzu, die Sie zum Ausführen des DNS-Diensts eingerichtet haben.

![Abbildung 10: Konfigurieren von DNS-Servern für Azure Virtual Network][sap-ha-guide-figure-3001]

***Abbildung 10:** Konfigurieren von DNS-Servern für Azure Virtual Network*

> [!NOTE]
> Wenn Sie die IP-Adressen der DNS-Server ändern, müssen Sie die virtuellen Azure-Computer neu starten, damit die Änderung übernommen und an die neuen DNS-Server übertragen wird.
> 
> 

In unserem Beispiel ist der DNS-Dienst auf den folgenden virtuellen Windows-Computern installiert und konfiguriert:

| Rolle für virtuellen Computer | Hostname für virtuellen Computer | Name der Netzwerkkarte | Statische IP-Adresse |
| --- | --- | --- | --- |
| Erster DNS-Server |domcontr-0 |pr1-nic-domcontr-0 |10.0.0.10 |
| Zweiter DNS-Server |domcontr-1 |pr1-nic-domcontr-1 |10.0.0.11 |

### <a name="a-name9fbd43c05850496597262a921d85d73fa-host-names-and-static-ip-addresses-for-the-sap-ascsscs-clustered-instance-and-dbms-clustered-instance"></a><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Hostnamen und statische IP-Adressen für die SAP ASCS/SCS-Clusterinstanz und -DBMS-Clusterinstanz
Für lokale Bereitstellungen benötigen Sie diese reservierten Hostnamen und IP-Adressen:

| Rolle des virtuellen Hosts | Name des virtuellen Hosts | Virtuelle statische IP-Adresse |
| --- | --- | --- |
| SAP ASCS/SCS – erster virtueller Host im Cluster (für die Clusterverwaltung) |pr1-ascs-vir |10.0.0.42 |
| SAP ASCS/SCS-Instanz – virtueller Hostname |pr1-ascs-sap |10.0.0.43 |
| SAP-DBMS – zweiter virtueller Host im Cluster (Clusterverwaltung) |pr1-dbms-vir |10.0.0.32 |

Bei der Erstellung des Clusters erstellen Sie die virtuellen Hostnamen **pr1-ascs-vir** und **pr1-dbms-vir** und die zugehörigen IP-Adressen, die den Cluster selbst verwalten. Der Prozess wird in [Erfassen von Clusterknoten in der Clusterkonfiguration][sap-ha-guide-8.12.1] beschrieben.

Sie können die anderen beiden virtuellen Hostnamen **pr1-ascs-sap** und **pr1-dbms-sap** und die zugeordneten IP-Adressen manuell auf dem DNS-Server erstellen. Die SAP ASCS/SCS-Clusterinstanz und die DBMS-Clusterinstanz verwenden diese Ressourcen. Eine Beschreibung finden Sie in [Erstellen eines virtuellen Hostnamens für SAP ASCS/SCS-Clusterinstanzen][sap-ha-guide-9.1.1].

### <a name="a-name84c019fe8c584dac9e54173efd4b2c30a-set-static-ip-addresses-for-the-sap-virtual-machines"></a><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Festlegen der statischen IP-Adressen für die beiden virtuellen SAP-Computer
Nach der Bereitstellung der virtuellen Computer für Ihren Cluster müssen Sie statische IP-Adressen für alle virtuellen Computer festlegen. Sie führen diesen Schritt in der Azure Virtual Network-Konfiguration und nicht im Gastbetriebssystem durch.

Das Azure-Portal bietet die Möglichkeit zum Festlegen einer statischen IP-Adresse. Wechseln Sie im Azure-Portal zu **Ressourcengruppe** > **Netzwerkkarte** > **Einstellungen** > **IP-Adresse**.

Wählen Sie unter **Zuweisung** die Option **Statisch** aus. Geben Sie im Feld **IP-Adresse** die IP-Adresse ein, die Sie verwenden möchten.

> [!NOTE]
> Wenn Sie die IP-Adresse der Netzwerkkarte ändern, müssen Sie die virtuellen Azure-Computer neu starten, um die Änderung zu übernehmen.  
> 
> 

![Abbildung 11: Festlegen der statischen IP-Adressen für die Netzwerkkarten der einzelnen virtuellen Computer][sap-ha-guide-figure-3002]

***Abbildung 11:** Festlegen der statischen IP-Adressen für die Netzwerkkarten der einzelnen virtuellen Computer*

Wiederholen Sie diesen Schritt für alle Netzwerkschnittstellen, d.h. für alle virtuellen Computer, einschließlich der virtuellen Computer, die Sie für den Active Directory-/DNS-Dienst verwenden möchten.

In unserem Beispiel verwenden wir die folgenden virtuellen Computer und statischen IP-Adressen:

| Rolle für virtuellen Computer | Hostname für virtuellen Computer | Name der Netzwerkkarte | Statische IP-Adresse |
| --- | --- | --- | --- |
| Erster SAP-Anwendungsserver |pr1-di-0 |pr1-nic-di-0 |10.0.0.50 |
| Zweiter SAP-Anwendungsserver |pr1-di-1 |pr1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Letzter SAP-Anwendungsserver |pr1-di-5 |pr1-nic-di-5 |10.0.0.55 |
| Erster Clusterknoten für die ASCS/SCS-Instanz |pr1-ascs-0 |pr1-nic-ascs-0 |10.0.0.40 |
| Zweiter Clusterknoten für die ASCS/SCS-Instanz |pr1-ascs-1 |pr1-nic-ascs-1 |10.0.0.41 |
| Erster Clusterknoten für die DBMS-Instanz |pr1-db-0 |pr1-nic-db-0 |10.0.0.30 |
| Zweiter Clusterknoten für die DBMS-Instanz |pr1-db-1 |pr1-nic-db-1 |10.0.0.31 |

### <a name="a-name7a8f3e9b062440519e41b73fff816a9ea-set-a-static-ip-address-for-the-internal-load-balancer"></a><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Festlegen der statischen IP-Adresse für den internen Load Balancer
Die Azure Resource Manager-Vorlage für SAP erstellt einen internen Azure Load Balancer, der für den Cluster der SAP ASCS/SCS-Instanz und den DBMS-Cluster verwendet wird.

Bei der ersten Bereitstellung wird die IP-Adresse des internen Load Balancers auf **Dynamisch** festgelegt. Die IP-Adresse muss in **Statisch** geändert werden.

In unserem Beispiel verwenden wir zwei interne Azure Load Balancer mit diesen statischen IP-Adressen:

| Rolle des internen Azure Load Balancers | Name des internen Azure Load Balancers | Statische IP-Adresse |
| --- | --- | --- |
| Interner Load Balancer für die SAP ASCS/SCS-Instanz |pr1-lb-ascs |10.0.0.43 |
| Interner Load Balancer für das SAP-DBMS |pr1-lb-dbms |10.0.0.33 |

> [!NOTE]
> Die IP-Adresse des virtuellen Hostnamens von SAP ASCS/SCS ist mit der IP-Adresse des internen Load Balancers „pr1-lb-ascs“ für die SAP ASCS/SCS-Instanz identisch.
> Die IP-Adresse des virtuellen Namens des DBMS ist mit der IP-Adresse des internen Load Balancers „pr1-lb-dbms“ für das DBMS identisch.
> 
> 

Legen Sie in unserem Beispiel die IP-Adresse des internen Load Balancers **pr1-lb-ascs** auf die IP-Adresse des virtuellen Hostnamens der SAP ASCS/SCS-Instanz (im Beispiel **10.0.0.43**) fest.

![Abbildung 12: Festlegen der statischen IP-Adressen für den internen Load Balancer der SAP ASCS/SCS-Instanz][sap-ha-guide-figure-3003]

***Abbildung 12:** Festlegen der statischen IP-Adressen für den internen Load Balancer der SAP ASCS/SCS-Instanz*

Legen Sie die IP-Adresse des internen Load Balancers **pr1-lb-dbms** auf die IP-Adresse des virtuellen Hostnamens der DBMS-Instanz (im Beispiel **10.0.0.33**) fest.

### <a name="a-namef19bd997154d4583a46e7f5a69d0153ca-default-ascsscs-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer
Die Azure Resource Manager-Vorlage für SAP erstellt alle erforderlichen Ports:

* Eine ABAP ASCS-Instanz mit der Standardinstanznummer **00**
* Eine Java SCS-Instanz mit der Standardinstanznummer **01**

Wenn Sie Ihre SAP ASCS/SCS-Instanz installieren, müssen Sie die Standardinstanznummer 00 für Ihre ABAP ASCS-Instanz und die Standardinstanznummer 01 für Ihre Java SCS-Instanz verwenden.

Als Nächstes erstellen Sie diese erforderlichen Endpunkte für den internen Lastenausgleich für die SAP NetWeaver ABAP ASCS-Ports:

| Dienst/Name der Lastenausgleichsregel | Standardportnummern | Konkrete Ports für (ASCS-Instanz mit Instanznummer 00) (ERS mit 10) |
| --- | --- | --- |
| Enqueue-Server/ *lbrule3200* |32<*Instanznummer*> |3200 |
| ABAP-Message-Server/ *lbrule3600* |36<*Instanznummer*> |3600 |
| Interne ABAP-Nachricht/ *lbrule3900* |39<*Instanznummer*> |3900 |
| Message-Server HTTP/ *Lbrule8100* |81<*Instanznummer*> |8100 |
| SAP Start Service ASCS HTTP/ *Lbrule50013* |5<*Instanznummer*>13 |50013 |
| SAP Start Service ASCS HTTPS/ *Lbrule50014* |5<*Instanznummer*>14 |50014 |
| Enqueue-Replikation/ *Lbrule50016* |5<*Instanznummer*>16 |50016 |
| SAP Start Service ERS HTTP *Lbrule51013* |5<*Instanznummer*>13 |51013 |
| SAP Start Service ERS HTTP *Lbrule51014* |5<*Instanznummer*>14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Dateifreigabe *Lbrule445* | |445 |

***Tabelle 1:** Portnummern der SAP NetWeaver ABAP ASCS-Instanzen*

Danach erstellen Sie diese erforderlichen Endpunkte für den internen Lastenausgleich für die SAP NetWeaver Java SCS-Ports:

| Dienst/Name der Lastenausgleichsregel | Standardportnummern | Konkrete Ports für (SCS-Instanz mit Instanznummer 01) (ERS mit 11) |
| --- | --- | --- |
| Enqueue-Server/ *lbrule3201* |32<*Instanznummer*> |3201 |
| Gateway-Server/ *lbrule3301* |33<*Instanznummer*> |3301 |
| Java-Message-Server/ *lbrule3900* |39<*Instanznummer*> |3901 |
| Message-Server-HTTP/ *Lbrule8101* |81<*Instanznummer*> |8101 |
| SAP Start Service SCS HTTP/ *Lbrule50113* |5<*Instanznummer*>13 |50113 |
| SAP Start Service SCS HTTPS/ *Lbrule50114* |5<*Instanznummer*>14 |50114 |
| Enqueue-Replikation/ *Lbrule50116* |5<*Instanznummer*>16 |50116 |
| SAP Start Service ERS HTTP *Lbrule51113* |5<*Instanznummer*>13 |51113 |
| SAP Start Service ERS HTTP *Lbrule51114* |5<*Instanznummer*>14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Dateifreigabe *Lbrule445* | |445 |

***Tabelle 2:** Portnummern der SAP NetWeaver Java SCS-Instanzen*

![Abbildung 13: Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer][sap-ha-guide-figure-3004]

***Abbildung 13:** Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer*

Legen Sie die IP-Adresse des Load Balancers **pr1-lb-dbms** auf die IP-Adresse des virtuellen Hostnamens der DBMS-Instanz (im Beispiel **10.0.0.33**) fest.

### <a name="a-namefe0bd8b52b4345e3829580bee5415716a-change-the-ascsscs-default-load-balancing-rules-for-the-azure-internal-load-balancer"></a><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Ändern der Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer
Wenn Sie andere Nummern für die SAP ASCS- oder SCS-Instanzen verwenden möchten, müssen Sie die Namen und Werte dieser Ports ändern.

Das Azure-Portal bietet die Möglichkeit zum Ändern der Instanznummern:

Wechseln Sie zu **<*SID*>-lb-ascs Load Balancer** > **Lastenausgleichsregeln**.

Ändern Sie für alle zur SAP ASCS- oder SCS-Instanz gehörenden Lastenausgleichsregeln die folgenden Werte:

* Name
* Port
* Back-End-Port

Wenn Sie z.B. die Standardnummer der ASCS-Instanz von 00 in 31 ändern möchten, müssen Sie die Änderungen für alle in Tabelle 1 aufgeführten Ports durchführen.

Dies ist ein Beispiel einer Änderung für den Port *lbrule3200*.

![Abbildung 14: Ändern der Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer][sap-ha-guide-figure-3005]

***Abbildung 14:** Ändern der Standardregeln für den ASCS/SCS-Lastenausgleich für den internen Azure Load Balancer*

### <a name="a-namee69e9a34460147a3a41cd2e11c626c0ca-add-windows-virtual-machines-to-the-domain"></a><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Hinzufügen virtueller Windows-Computer zur Domäne
Nachdem Sie den virtuellen Computern eine statische IP-Adresse zugewiesen haben, fügen Sie die virtuellen Computer der Domäne hinzu.

![Abbildung 15: Hinzufügen eines virtuellen Computers zu einer Domäne][sap-ha-guide-figure-3006]

***Abbildung 15:** Hinzufügen eines virtuellen Computers zu einer Domäne*

### <a name="a-name661035b24d0f4d3186f8dc0a50d78158a-add-registry-entries-on-both-cluster-nodes-of-the-sap-ascsscs-instance"></a><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten für eine SAP ASCS/SCS-Instanz
Der Azure Load Balancer verfügt über einen internen Load Balancer, der Verbindungen schließt, wenn sich diese für einen festgelegten Zeitraum im Leerlauf befinden (Leerlaufzeitlimit). SAP-Workprozesse öffnen in Dialoginstanzen Verbindungen mit dem SAP-Enqueue-Prozess, sobald die erste Enqueue-/Dequeue-Anforderung gesendet werden muss. Diese Verbindungen bleiben in der Regel bestehen, bis der Workprozess oder Enqueue-Prozess neu gestartet wird. Wenn sich die Verbindung für einen bestimmten Zeitraum im Leerlauf befindet, schließt der interne Azure Load Balancer jedoch die Verbindungen. Dies stellt kein Problem dar, da der SAP-Arbeitsprozesses die Verbindung mit dem Enqueue-Prozess erneut herstellt, wenn sie nicht mehr vorhanden sein sollte. Diese Aktivitäten werden in den Entwickler-Traces von SAP-Prozessen dokumentiert, sie erstellen jedoch eine große Menge an zusätzlichen Inhalten in diesen Traces. Daher wird empfohlen, die TCP/IP-Einstellungen `KeepAliveTime` und `KeepAliveInterval` auf beiden Clusterknoten zu ändern. Kombinieren Sie diese Änderungen an den TCP/IP-Parametern mit SAP-Profilparametern, die weiter unten in diesem Artikel beschrieben werden.

Fügen Sie diese Windows-Registrierungseinträge auf beiden Windows-Clusterknoten für SAP ASCS/SCS hinzu:

| Pfad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Variablenname |`KeepAliveTime` |
| Variablentyp |REG_DWORD-Wert (dezimal) |
| Wert |120000 |
| Link zur Dokumentation |[https://technet.microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

***Tabelle 3:** Ändern des ersten TCP/IP-Parameters*

| Pfad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Variablenname |`KeepAliveInterval` |
| Variablentyp |REG_DWORD-Wert (dezimal) |
| Wert |120000 |
| Link zur Dokumentation |[https://technet.microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

***Tabelle 4:** Ändern des zweiten TCP/IP-Parameters*

Starten Sie beide Clusterknoten neu, um die Änderungen zu übernehmen.

### <a name="a-name0d67f090792843e087725ccbf8f59aaba-set-up-a-windows-server-failover-clustering-cluster-for-an-sap-ascsscs-instance"></a><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Einrichten eines Windows Server-Failoverclustering-Clusters für eine SAP ASCS/SCS-Instanz
#### <a name="a-name5eecb071c7034cccba6dfe9c6ded9d79a-collect-the-cluster-nodes-in-a-cluster-configuration"></a><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Erfassen der Clusterknoten in einer Clusterkonfiguration
Der erste Schritt ist das Hinzufügen von Failoverclustering auf beiden Clusterknoten. Verwenden Sie den Assistenten zum Hinzufügen von Rollen und Features.

Der zweite Schritt ist das Einrichten des Failoverclusters mithilfe des Failovercluster-Managers.

Wählen Sie im Failovercluster-Manager **Cluster erstellen** aus, und fügen Sie nur den Namen des ersten Clusterknotens A hinzu, z.B. **pr1-ascs-0**. Fügen Sie den zweiten Knoten noch nicht hinzu. Sie fügen den zweiten Knoten in einem späteren Schritt hinzu.

![Abbildung 16: Hinzufügen eines Namens für den Server oder virtuellen Computer des ersten Clusterknotens][sap-ha-guide-figure-3007]

***Abbildung 16:** Hinzufügen eines Namens für den Server oder virtuellen Computer des ersten Clusterknotens*

Als Nächstes werden Sie aufgefordert, den Netzwerknamen (den Namen des virtuellen Hosts) für den Cluster einzugeben.

![Abbildung 17: Definieren des Clusternamens][sap-ha-guide-figure-3008]

***Abbildung 17:** Definieren des Clusternamens*

Führen Sie nach dem Erstellen des Clusters einen Test zur Überprüfung des Clusters durch.

![Abbildung 18: Ausführen der Clusterüberprüfung][sap-ha-guide-figure-3009]

***Abbildung 18:** Ausführen der Clusterüberprüfung*

![Abbildung 19: Es wurde kein Quorumdatenträger gefunden][sap-ha-guide-figure-3010]

***Abbildung 19:** Es wurde kein Quorumdatenträger gefunden*

Sie können an diesem Punkt im Prozess sämtliche Warnungen zu Datenträgern ignorieren. Sie fügen später einen Dateifreigabenzeugen und die freigegebenen SIOS-Datenträger hinzu. In dieser Phase müssen Sie noch kein Quorum berücksichtigen.

![Abbildung 20: Die Hauptclusterressource benötigt eine neue IP-Adresse][sap-ha-guide-figure-3011]

***Abbildung 20:** Die Hauptclusterressource benötigt eine neue IP-Adresse*

Der Cluster kann nicht gestartet werden, da die IP-Adresse des Servers auf einen der virtuellen Computerknoten verweist. Sie müssen die IP-Adresse des Hauptclusterdiensts ändern.

Wir müssen z.B. dem virtuellen Hostnamen des Clusters **pr1-ascs-vir** eine IP-Adresse (im Beispiel **10.0.0.42**) zuweisen. Dies wird auf der Eigenschaftenseite der IP-Ressource des Hauptclusterdiensts durchgeführt, die in Abbildung 21 dargestellt ist.

![Abbildung 21: Ändern der IP-Adresse im Dialogfeld **Eigenschaften**][sap-ha-guide-figure-3012]

***Abbildung 21:** Ändern der IP-Adresse im Dialogfeld **Eigenschaften***

![Abbildung 22: Zuweisen der für den Cluster reservierten IP-Adresse][sap-ha-guide-figure-3013]

***Abbildung 22:** Zuweisen der für den Cluster reservierten IP-Adresse*

Schalten Sie nach dem Ändern der IP-Adresse den virtuellen Hostnamen des Clusters online.

![Abbildung 23: Hauptclusterdienst im laufenden Betrieb mit ordnungsgemäßer IP-Adresse][sap-ha-guide-figure-3014]

***Abbildung 23:** Hauptclusterdienst im laufenden Betrieb mit ordnungsgemäßer IP-Adresse*

Nachdem der Hauptclusterdienst in Betrieb ist, können Sie den zweiten Clusterknoten hinzufügen.

![Abbildung 24: Hinzufügen des zweiten Clusterknotens][sap-ha-guide-figure-3015]

***Abbildung 24:** Hinzufügen des zweiten Clusterknotens*

![Abbildung 25: Hinzufügen des Hostnamens des zweiten Clusterknotens – Beispiel: pr1-ascs-1][sap-ha-guide-figure-3016]

***Abbildung 25:** Hinzufügen des Hostnamens des zweiten Clusterknotens – Beispiel: **pr1-ascs-1***

![Abbildung 26: Kontrollkästchen nicht aktivieren][sap-ha-guide-figure-3017]

***Abbildung 26:** Kontrollkästchen *nicht* aktivieren*

> [!IMPORTANT]
> Stellen Sie sicher, dass das Kontrollkästchen **Der gesamte geeignete Speicher soll dem Cluster hinzugefügt werden** *nicht* aktiviert wird.  
> 
> 

![Abbildung 27: Ignorieren Sie Warnungen zum Datenträgerquorum][sap-ha-guide-figure-3018]

***Abbildung 27:** Ignorieren Sie Warnungen zum Datenträgerquorum*

Sie können Warnungen zu Quorum und Datenträgern ignorieren. Sie konfigurieren das Quorum und die Freigabe der Datenträger später in [Installieren von SIOS DataKeeper Cluster Edition für SAP ASCS/SCS-Clusterfreigabe-Datenträger][sap-ha-guide-8.12.3].

#### <a name="a-namee49a452950c94dcfbde715a0c21d21caa-configure-a-cluster-file-share-witness"></a><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Konfigurieren eines Cluster-Dateifreigabenzeugen
##### <a name="a-name06260b30d6974c4db1c9d22c0bd64855a-create-a-file-share"></a><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Erstellen einer Dateifreigabe
Wählen Sie anstelle eines Quorumdatenträgers einen Dateifreigabenzeugen aus. Diese Option wird von SIOS DataKeeper unterstützt.

In den Beispielen in diesem Artikel befindet sich der Dateifreigabenzeuge auf dem Active Directory-/DNS-Server, der in Azure ausgeführt wird. Der Dateifreigabenzeuge heißt **domcontr 0**. Da Sie eine VPN-Verbindung (Virtual Private Network) mit Azure (Site-to-Site-VPN oder Azure ExpressRoute) konfiguriert hätten, ist Ihr Active Directory-/DNS-Dienst lokal und nicht geeignet für die Ausführung eines Dateifreigabenzeugen.

> [!NOTE]
> Wenn Ihr Active Directory-/DNS-Dienst nur lokal ausgeführt wird, konfigurieren Sie keinen Dateifreigabenzeugen auf dem lokalen Windows-Betriebssystem mit Active Directory/DNS. Die Netzwerklatenz zwischen Clusterknoten in Azure und lokalen Active Directory-/DNS-Diensten wäre zu hoch und könnte Verbindungsprobleme verursachen. Konfigurieren Sie den Dateifreigabenzeugen unbedingt auf einem virtuellen Azure-Computer, der in der Nähe des Clusterknotens betrieben wird.  
> 
> 

Das Quorumlaufwerk benötigt mindestens 1.024 MB freien Speicherplatz. Es werden 2.048 MB freier Speicherplatz empfohlen.

Der erste Schritt ist das Hinzufügen des Clusternamenobjekts.

![Abbildung 28: Zuweisen der Berechtigungen für die Freigabe für das Clusternamenobjekt][sap-ha-guide-figure-3019]

***Abbildung 28:** Zuweisen der Berechtigungen für die Freigabe für das Clusternamenobjekt*

Vergewissern Sie sich, dass die Berechtigungen das Ändern von Daten auf der Freigabe für das Clusternamenobjekt zulassen (im Beispiel **pr1-ascs-vir$**). Wählen Sie **Hinzufügen** aus, um das Clusternamenobjekt der Liste hinzuzufügen. Ändern Sie den Filter, um zusätzlich zu den in Abbildung 29 dargestellten Objekten auch Computerobjekten einzuschließen:

![Abbildung 29: Ändern des Objekttyps, um Computerobjekte einzuschließen][sap-ha-guide-figure-3020]

***Abbildung 29:** Ändern des Objekttyps, um Computerobjekte einzuschließen*

![Abbildung 30: Aktivieren des Kontrollkästchens für Computerobjekte][sap-ha-guide-figure-3021]

***Abbildung 30:** Aktivieren des Kontrollkästchens für Computerobjekte*

Geben Sie anschließend das Clusternamenobjekt ein, wie in Abbildung 29 dargestellt. Da der Datensatz bereits erstellt wurde, können Sie die Berechtigungen ändern (siehe Abbildung 28).

Wählen Sie als Nächstes die Registerkarte **Sicherheit** für die Freigabe aus, und legen Sie anschließend ausführlichere Berechtigungen für das Clusternamenobjekt fest.

![Abbildung 31: Festlegen von Sicherheitsattributen für das Clusternamenobjekt im Dateifreigabequorum][sap-ha-guide-figure-3022]

***Abbildung 31:** Festlegen von Sicherheitsattributen für das Clusternamenobjekt im Dateifreigabequorum*

##### <a name="a-name4c08c38778a046b19d27b497b08cac3da-set-the-file-share-witness-quorum-in-failover-cluster-manager"></a><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Festlegen des Dateifreigabenzeugen-Quorums im Failovercluster-Manager
Ändern Sie mithilfe des Failovercluster-Managers die Clusterkonfiguration in einen Dateifreigabenzeugen.

![Abbildung 32: Starten des Assistenten zum Konfigurieren des Clusterquorums][sap-ha-guide-figure-3023]

***Abbildung 32:** Starten des Assistenten zum Konfigurieren des Clusterquorums*

![Abbildung 33: Zur Auswahl stehende Quorumkonfigurationen][sap-ha-guide-figure-3024]

***Abbildung 33:** Zur Auswahl stehende Quorumkonfigurationen*

Wählen Sie **Quorumzeugen auswählen** aus.

![Abbildung 34: Auswählen des Dateifreigabenzeugen][sap-ha-guide-figure-3025]

***Abbildung 34:** Auswählen des Dateifreigabenzeugen*

Wählen Sie **Dateifreigabenzeugen konfigurieren** aus.

![Abbildung 35: Festlegen des Speicherorts der Dateifreigabe für die Zeugenfreigabe][sap-ha-guide-figure-3026]

***Abbildung 35:** Festlegen des Speicherorts der Dateifreigabe für die Zeugenfreigabe*

Geben Sie den UNC-Pfad zur Dateifreigabe ein (im Beispiel \\domcontr-0\FSW).

Wählen Sie **Weiter** aus, um eine Liste der Änderungen anzuzeigen, die Sie vornehmen können. Wählen Sie die gewünschten Änderungen und dann **Weiter** aus.

![Abbildung 36: Bestätigung der Neukonfiguration des Clusters][sap-ha-guide-figure-3027]

***Abbildung 36:** Bestätigung der Neukonfiguration des Clusters*

In diesem letzten Schritt müssen Sie den Cluster erfolgreich neu konfigurieren, wie in Abbildung 36 dargestellt.  

### <a name="a-name5c8e5482841e45e1a89da05c0907c868a-install-sios-datakeeper-cluster-edition-for-the-sap-ascsscs-cluster-share-disk"></a><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Installieren von SIOS DataKeeper Cluster Edition für den SAP ASCS/SCS-Clusterfreigabe-Datenträger
Sie verfügen nun über eine funktionierende Windows Server-Failoverclusteringkonfiguration in Azure. Um eine SAP ASCS/SCS-Instanz installieren zu können, benötigen Sie jedoch eine freigegebene Datenträgerressource. Sie können die erforderlichen freigegebenen Datenträgerressourcen nicht in Azure erstellen. SIOS DataKeeper Cluster Edition ist eine Drittanbieterlösung, die Sie zum Erstellen von freigegebenen Datenträgerressourcen verwenden können.

#### <a name="a-name1c2788c336484e829e0de058e475e2a3a-add-the-net-framework-35"></a><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Hinzufügen von .NET Framework 3.5
Microsoft .NET Framework 3.5 wird unter Windows Server 2012 R2 nicht automatisch aktiviert oder installiert. Für SIOS DataKeeper muss jedoch auf allen Knoten, auf denen Sie DataKeeper installieren, .NET Framework vorhanden sein. Aus diesem Grund müssen Sie .NET Framework 3.5 auf dem Gastbetriebssystem aller virtuellen Computer im Cluster installieren.

Es gibt zwei Möglichkeiten, .NET Framework 3.5 hinzuzufügen. Eine Möglichkeit bietet der Assistent zum Hinzufügen von Rollen und Features in Windows, der in Abbildung 37 dargestellt ist.

![Abbildung 37: Installieren von .NET Framework 3.5 mithilfe des Assistenten zum Hinzufügen von Rollen und Features][sap-ha-guide-figure-3028]

***Abbildung 37:** Installieren von .NET Framework 3.5 mithilfe des Assistenten zum Hinzufügen von Rollen und Features*

![Abbildung 38: Statusanzeige für die Installation von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features][sap-ha-guide-figure-3029]

***Abbildung 38:** Statusanzeige für die Installation von .NET Framework 3.5 mit dem Assistenten zum Hinzufügen von Rollen und Features*

Die zweite Möglichkeit zum Aktivieren von .NET Framework 3.5 ist das Befehlszeilentool „dism.exe“. Für diese Art von Installation benötigen Sie Zugriff auf das Verzeichnis „SxS“ auf dem Windows-Installationsmedium. Führen Sie diesen Befehl in einem Eingabeaufforderungsfenster mit erhöhten Rechten aus:

```
Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
```

#### <a name="a-namedd41d5a28083415b9878839652812102a-install-sios-datakeeper"></a><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Installieren von SIOS DataKeeper
Installieren Sie SIOS DataKeeper Cluster Edition auf jedem Knoten im Cluster. Mit SIOS DataKeeper können Sie virtuellen freigegebenen Speicher und eine synchronisierte Spiegelung erstellen und dann freigegebenen Clusterspeicher simulieren.

Erstellen Sie vor der Installation der SIOS-Software den Domänenbenutzer **DataKeeperSvc**.

> [!NOTE]
> Fügen Sie den Benutzer **DataKeeperSvc** der Gruppe **Lokale Administratoren** auf beiden Clusterknoten hinzu.
> 
> 

Installieren Sie die SIOS-Software auf beiden Clusterknoten.

![SIOS-Installationsprogramm][sap-ha-guide-figure-3030]

![Abbildung 39: Erster Bildschirm der SIOS DataKeeper-Installation][sap-ha-guide-figure-3031]

***Abbildung 39:** Erster Bildschirm der SIOS DataKeeper-Installation*

![Abbildung 40: DataKeeper informiert über die Deaktivierung eines Diensts][sap-ha-guide-figure-3032]

***Abbildung 40:** DataKeeper informiert über die Deaktivierung eines Diensts*

Wählen Sie in dem in Abbildung 40 dargestellten Dialogfeld **Ja** aus.

![Abbildung 41: Benutzerauswahl für SIOS DataKeeper][sap-ha-guide-figure-3033]

***Abbildung 41:** Benutzerauswahl für SIOS DataKeeper*

Sie sollten auf dem in Abbildung 41 dargestellten Bildschirm **Domänen- oder Serverkonto** auswählen.

![Abbildung 42: Eingeben des Domänenbenutzernamens und des Kennworts für die SIOS DataKeeper-Installation][sap-ha-guide-figure-3034]

***Abbildung 42:** Eingeben des Domänenbenutzernamens und des Kennworts für die SIOS DataKeeper-Installation*

Geben Sie den Benutzernamen für das Domänenkonto und die Kennwörter ein, die Sie für SIOS DataKeeper erstellt haben.

![Abbildung 43: Eingeben Ihrer SIOS DataKeeper-Lizenz][sap-ha-guide-figure-3035]

***Abbildung 43:** Eingeben Ihrer SIOS DataKeeper-Lizenz*

Installieren Sie den Lizenzschlüssel für Ihre SIOS DataKeeper-Instanz, wie in Abbildung 43 gezeigt. Am Ende der Installation werden Sie aufgefordert, den virtuellen Computer neu zu starten.

#### <a name="a-named9c1fc8e87104dffbec21f535db7b006a-set-up-sios-datakeeper"></a><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Einrichten von SIOS DataKeeper
Nach der Installation von SIOS DataKeeper auf beiden Knoten müssen Sie mit der Konfiguration beginnen. Ziel der Konfiguration ist eine synchrone Datenreplikation zwischen den zusätzlichen VHDs, die Sie an jeden virtuellen Computer angefügt haben. Mit diesen Schritten konfigurieren Sie beide Knoten.

![Abbildung 44: SIOS DataKeeper-Tool für Verwaltung und Konfiguration][sap-ha-guide-figure-3036]

***Abbildung 44:** SIOS DataKeeper-Tool für Verwaltung und Konfiguration*

Starten Sie das DataKeeper-Tool für die Verwaltung und Konfiguration, und wählen Sie dann **Verbindung mit Server herstellen** aus. (In Abbildung 44 ist diese Option rot markiert.)

![Abbildung 45: Einfügen des Namens oder der TCP/IP-Adresse des ersten Knotens und in einem zweiten Schritt des zweiten Knotens, mit denen das Tool für die Verwaltung und Konfiguration eine Verbindung herstellen soll][sap-ha-guide-figure-3037]

***Abbildung 45:** Einfügen des Namens oder der TCP/IP-Adresse des ersten Knotens und in einem zweiten Schritt des zweiten Knotens, mit denen das Tool für die Verwaltung und Konfiguration eine Verbindung herstellen soll*

Der nächste Schritt ist das Erstellen des Replikationsauftrags zwischen den beiden Knoten.

![Abbildung 46: Erstellen eines Replikationsauftrags][sap-ha-guide-figure-3038]

***Abbildung 46:** Erstellen eines Replikationsauftrags*

Ein Assistent führt Sie durch die Erstellung eines Replikationsauftrags.

![Abbildung 47: Festlegen des Namens des Replikationsauftrags][sap-ha-guide-figure-3039]

***Abbildung 47:** Festlegen des Namens des Replikationsauftrags*

![Abbildung 48: Definieren der Basisdaten für den Knoten, der der aktuelle Quellknoten sein soll][sap-ha-guide-figure-3040]

***Abbildung 48:** Definieren der Basisdaten für den Knoten, der der aktuelle Quellknoten sein soll*

Im ersten Schritt müssen Sie den Namen, die TCP/IP-Adresse und das Datenträgervolume des Quellknotens definieren. Im zweiten Schritt wird der Zielknoten festgelegt. Wie bereits erläutert, müssen Sie den Namen, die TCP/IP-Adresse und das Datenträgervolume des Zielknotens definieren.

![Abbildung 49: Definieren der Basisdaten für den Knoten, der der aktuelle Zielknoten sein soll][sap-ha-guide-figure-3041]

***Abbildung 49:** Definieren der Basisdaten für den Knoten, der der aktuelle Zielknoten sein soll*

Als Nächstes definieren Sie die Komprimierungsalgorithmen. Für unser Beispiel wird das Komprimieren des Replikationsdatenstroms empfohlen. Insbesondere in Situationen mit erneuter Synchronisierung wird die dafür erforderliche Dauer durch die Komprimierung des Replikationsdatenstroms erheblich verkürzt. Beachten Sie, dass die Komprimierung CPU- und Arbeitsspeicherressourcen eines virtuellen Computers verbraucht. Wenn die Komprimierungsrate zunimmt, steigt auch die Menge der erforderlichen CPU-Ressourcen. Sie können diese Einstellung später anpassen.

Eine weitere Einstellung, die Sie überprüfen müssen, ist, ob die Replikation asynchron oder synchron erfolgt. *Wenn Sie SAP ASCS/SCS-Konfigurationen schützen, müssen Sie die synchrone Replikation verwenden*.  

![Abbildung 50: Festlegen von Replikationsdetails][sap-ha-guide-figure-3042]

***Abbildung 50:** Festlegen von Replikationsdetails*

Im letzten Schritt legen Sie fest, ob das Volume, das vom Replikationsauftrag repliziert wird, gegenüber einer Windows Server-Failoverclustering-Clusterkonfiguration als freigegebener Datenträger dargestellt werden sollte. Für die SAP ASCS/SCS-Konfiguration wählen Sie **Ja** aus, damit der Windows-Cluster das replizierte Volume als freigegebenen Datenträger erkennt, das als Clustervolume verwendet werden kann.

![Abbildung 51: Klicken auf **Ja**, um das replizierte Volume als Clustervolume festzulegen][sap-ha-guide-figure-3043]

***Abbildung 51:** Klicken auf **Ja**, um das replizierte Volume als Clustervolume festzulegen*

Nachdem das Volume erstellt wurde, zeigt das DataKeeper-Tool für die Verwaltung und Konfiguration an, dass der Replikationsauftrag aktiv ist.

![Abbildung 52: Die synchrone Spiegelung von DataKeeper für den freigegebenen SAP ASCS/SCS-Datenträgers ist aktiv][sap-ha-guide-figure-3044]

***Abbildung 52:** Die synchrone Spiegelung von DataKeeper für den freigegebenen SAP ASCS/SCS-Datenträgers ist aktiv*

Der Failovercluster-Manager zeigt nun den Datenträger als DataKeeper-Datenträger an, wie in Abbildung 53 dargestellt.

![Abbildung 53: Failovercluster-Manager zeigt den von DataKeeper replizierten Datenträger an][sap-ha-guide-figure-3045]

***Abbildung 53:** Failovercluster-Manager zeigt den von DataKeeper replizierten Datenträger an*

## <a name="a-namea06f0b498a7a42bf8b0dc12026c5746ba-install-the-sap-netweaver-system"></a><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Installieren des SAP NetWeaver-Systems
Das DBMS-Setup wird hier nicht beschrieben, da die Einrichtung vom verwendeten DBMS-System abhängt. Jedoch wird davon ausgegangen, dass auf hohe Verfügbarkeit bezogene Aspekte des DBMS mit den Funktionen verwaltet werden, die verschiedene DBMS-Hersteller für Azure unterstützen. Beispiele hierfür sind Always On oder Datenbankspiegelung für SQL Server und Oracle Data Guard für Oracle. In dem Szenario in diesem Artikel haben wir keinen zusätzlichen Schutz für das DBMS hinzugefügt.

Es sind keine Besonderheiten zu berücksichtigen, wenn unterschiedliche DBMS-Dienste mit dieser Art von SAP ASCS/SCS-Clusterkonfiguration in Azure interagieren.

> [!NOTE]
> Der Installationsvorgang von SAP NetWeaver ABAP-, Java- und ABAP+Java-Systemen ist nahezu identisch. Der wichtigste Unterschied ist, dass ein SAP ABAP-System eine ASCS-Instanz hat. Das SAP Java-System hat eine SCS-Instanz. Das SAP ABAP+Java-System hat eine ASCS- und eine SCS-Instanz, die in der gleichen Microsoft-Failoverclustergruppe ausgeführt werden. Unterschiede bei der Installation der einzelnen SAP NetWeaver-Installationsstapel werden explizit angegeben. Sie können davon ausgehen, dass alle anderen Teile gleich sind.  
> 
> 

### <a name="a-name31c6bd4f51df40579fdf3fcbc619c170a-install-sap-with-a-highavailability-ascsscs-instance"></a><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Installieren von SAP mit einer hoch verfügbaren ASCS/SCS-Instanz
> [!IMPORTANT]
> Achten Sie darauf, nicht die Auslagerungsdatei auf von DataKeeper gespiegelten Volumes zu platzieren. DataKeeper unterstützt keine gespiegelten Volumes. Belassen Sie die Auslagerungsdatei auf dem temporären Laufwerk D eines virtuellen Azure-Computers (dies ist auch die Standardeinstellung). Wenn sie sich noch nicht dort befindet, verschieben Sie die Windows-Auslagerungsdatei auf Laufwerk D des virtuellen Azure-Computers.
> 
> 

#### <a name="a-namea97ad604909444fea364f89cb39bf097a-create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance"></a><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Erstellen eines virtuellen Hostnamens für die SAP ASCS/SCS-Clusterinstanz
Erstellen Sie zunächst im Windows-DNS-Manager einen DNS-Eintrag für den virtuellen Hostnamen der ASCS/SCS-Instanz. Legen Sie anschließend die IP-Adresse für den virtuellen Hostnamen fest.

> [!NOTE]
> Beachten Sie, dass die IP-Adresse, die Sie dem virtuellen Hostnamen der ASCS/SCS-Instanz zuweisen, mit der IP-Adresse identisch sein muss, die Sie dem Azure Load Balancer zugeordnet haben (<*SID*>-lb-ascs).  
> 
> 

Die IP-Adresse des virtuellen Hostnamens der SAP ASCS/SCS-Instanz (pr1-ascs-sap) ist mit der IP-Adresse des Azure Load Balancers (pr1-lb-ascs) identisch.

In einem Windows Server-Failovercluster in Azure kann nur eine SAP-Failoverclusterrolle ausgeführt werden. Das bedeutet zum Beispiel eine ASCS-Instanz für das ABAP-System und eine SCS-Instanz für das Java-System. Für ABAP+Java wären es eine ASCS-Instanz und eine SCS-Instanz.

> [!NOTE]
> Das Bilden von Clustern mit mehreren SIDs, wie in den SAP-Installationshandbüchern beschrieben (siehe [SAP-Installationshandbücher][sap-installation-guides]), ist derzeit in Azure nicht möglich.
> 
> 

![Abbildung 54: Festlegen des DNS-Eintrags für den virtuellen SAP ASCS/SCS-Clusternamen und der TCP/IP-Adresse][sap-ha-guide-figure-3046]

***Abbildung 54:** Festlegen des DNS-Eintrags für den virtuellen SAP ASCS/SCS-Clusternamen und der TCP/IP-Adresse*

Der Eintrag wird im DNS-Manager unter der Domäne angezeigt, wie in Abbildung 55 dargestellt.

![Abbildung 55: Neuer virtueller Name und TCP/IP-Adresse für die SAP ASCS/SCS-Clusterkonfiguration][sap-ha-guide-figure-3047]

***Abbildung 55:** Neuer virtueller Name und TCP/IP-Adresse für die SAP ASCS/SCS-Clusterkonfiguration*

#### <a name="a-nameeb5af918b42f4803bb50eff41f84b0b0a-install-the-sap-first-cluster-node"></a><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Installieren des ersten SAP-Clusterknotens
Um den ersten SAP-Cluster zu installieren, führen Sie die erste Clusterknotenoption auf Knoten A aus, z.B. auf dem Host **pr1-ascs-0**.

Wenn Sie für den internen Azure Load Balancer die Standardports beibehalten möchten, wählen Sie Folgendes aus:

* Für ein **ABAP-System**: **ASCS**-Instanznummer **00**
* Für ein **Java-System**: **SCS**-Instanznummer **01**
* Für ein **ABAP+JAVA-System**: **ASCS**-Instanznummer **00** und **SCS**-Instanznummer **01**

Wenn Sie andere Instanznummern als 00 für eine ABAP ASCS-Instanz und 01 für eine Java SCS-Instanz verwenden möchten, müssen Sie zunächst die Standardregeln für den Lastenausgleich durch den Azure Load Balancer ändern. Dieser Vorgang wird in [Ändern der Standardregeln für den ASCS/SCS-Lastenausgleich durch den internen Azure Load Balancer][sap-ha-guide-8.9] beschrieben.

Führen Sie anschließend einige Schritte durch, die in der üblichen SAP-Installationsdokumentation nicht beschrieben sind.

> [!NOTE]
> Die SAP-Installationsdokumentation beschreibt, wie der erste ASCS/SCS-Clusterknoten installiert wird.
> 
> 

#### <a name="a-namee4caaab2e90f4f2cbc842cd2e12a9556a-modify-the-sap-profile-of-the-ascsscs-ipowershellnstance"></a><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Ändern des SAP-Profils der ASCS/SCS-Instanz
Sie müssen einen neuen Profilparameter hinzuzufügen. Dieser Profilparameter verhindert das Schließen von Verbindungen zwischen SAP-Workprozessen und dem Enqueue-Server, wenn diese sich zu lange im Leerlauf befinden. Dieses Problem wurde bereits in diesem Artikel unter [Hinzufügen von Registrierungseinträgen auf beiden Clusterknoten für eine SAP ASCS/SCS-Instanz][sap-ha-guide-8.11] erwähnt. In diesem Abschnitt haben wir auch zwei Änderungen an grundlegenden TCP/IP-Verbindungsparametern vorgenommen. In einem zweiten Schritt müssen Sie den Enqueue-Server für das Senden eines **keep_alive**-Signals konfigurieren, damit die Verbindungen nicht den Schwellenwert für den Leerlauf des internen Azure Load Balancers erreichen.

Fügen Sie diesen Profilparameter dem Profil der SAP ASCS/SCS-Instanz hinzu:

```
enque/encni/set_so_keepalive = true
```
Bei unserem Beispiel lautet der Pfad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

Beispiel für das Profil einer SAP SCS-Instanz und den zugehörigen Pfad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

#### <a name="a-name10822f4f32e74871b63a9b86c76ce761a-add-a-probe-port"></a><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Hinzufügen eines Testports
Verwenden Sie die Testfunktion des interner·Load Balancers, um die gesamte Clusterkonfiguration mit dem Load Balancer verwendbar zu machen. Der interne Azure Load Balancer verteilt in der Regel die eingehende Workload gleichmäßig auf die beteiligten virtuellen Computer. Allerdings funktioniert dies bei einigen Clusterkonfigurationen nicht, da nur eine Instanz aktiv ist. Die andere Instanz ist passiv und kann daher keine Workload annehmen. Eine Testfunktion ist hilfreich, wenn der interne Azure Load Balancer die Workload nur einer aktiven Instanz zuweist. Mit der Testfunktion kann der internen Load Balancer erkennen, welche Instanzen aktiv sind, und dann nur dieser Instanz Workload zuweisen.

Überprüfen Sie zunächst die aktuelle Einstellung für **ProbePort** mit dem folgenden PowerShell-Befehl. Führen Sie ihn auf einem virtuellen Computer in der Clusterkonfiguration aus:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Abbildung 56: Testport der Clusterkonfiguration ist standardmäßig 0][sap-ha-guide-figure-3048]

***Abbildung 56:** Testport der Clusterkonfiguration ist standardmäßig 0*

Legen Sie anschließend einen Testport fest. Die Standardnummer für den Testport ist 0. In unserem Beispiel verwenden wir als Testport **62300**.

Die Portnummer ist in Azure Resource Manager-Vorlagen für SAP definiert. Sie können die Portnummer in PowerShell zuweisen.

Rufen Sie zuerst den virtuellen SAP-Hostnamen der Clusterressource **SAP WAC IP** ab.

```PowerShell
$var = Get-ClusterResource | Where-Object {  $_.name -eq "SAP PR1 IP"  }
```

Legen Sie dann den Testport auf **62300** fest.

```PowerShell
$var | Set-ClusterParameter -Multiple @{"Address"="10.0.0.43";"ProbePort"=62300;"Subnetmask"="255.255.255.0";"Network"="Cluster Network 1";"OverrideAddressMatch"=1;"EnableDhcp"=0}  
```

Sie müssen die Clusterrolle **SAP PR1** beenden und neu starten, damit die Änderungen übernommen werden.

Überprüfen Sie, nachdem Sie die Clusterrolle **SAP PR1** online geschaltet haben, ob **ProbePort** auf den neuen Wert festgelegt wurde:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Abbildung 57: Überprüfen des Clusterports nach dem Festlegen des neuen Werts][sap-ha-guide-figure-3049]

***Abbildung 57:** Überprüfen des Clusterports nach dem Festlegen des neuen Werts*

Die Einstellung **ProbePort** wurde auf **62300** festgelegt. Sie können nun auf die Dateifreigabe **\\\ascsha-clsap\sapmnt** von anderen Hosts wie **ascsha-dbas** zugreifen.

### <a name="a-name85d78414b21d409792b634d8bcb724b7a-install-the-database-instance"></a><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Installieren der Datenbankinstanz
Führen Sie für die Installation der Datenbankinstanz die in der SAP-Installationsdokumentation beschriebene Vorgehensweise durch.

### <a name="a-name8a276e16f5074071b829cdc0a4d36748a-install-the-second-cluster-node"></a><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Installieren des zweiten Clusterknotens
Führen Sie zum Installieren des zweiten Clusterknotens die Schritte im SAP-Installationshandbuch aus.

### <a name="a-name094bc89531d4447191cc1513b64e406aa-change-the-start-type-of-the-sap-ers-windows-service-instance"></a><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Ändern des Starttyps der Windows-Dienstinstanz für SAP ERS
Ändern Sie den Starttyp des Windows-Diensts für SAP ERS (Enqueue Replication Server) auf beiden Clusterknoten in **Automatisch (Verzögerter Start)**.

![Abbildung 58: Ändern des Starttyps der SAP ERS-Instanz in „Automatisch (Verzögerter Start)“][sap-ha-guide-figure-3050]

***Abbildung 58:** Ändern des Starttyps der SAP ERS-Instanz in „Automatisch (Verzögerter Start)“*

### <a name="a-name2477e58fc5a74a5d9ae37b91022cafb5a-install-the-sap-primary-application-server"></a><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Installieren des primären SAP-Anwendungsservers
Installieren Sie die PAS-Instanz (primärer Anwendungsserver) <*SID*>-di-0 auf dem virtuellen Computer, den Sie als Host für den PAS vorgesehen haben. Es bestehen keine Abhängigkeiten mit Azure oder DataKeeper.

### <a name="a-name0ba4a6c1cc374bcfa8dc025de4263772a-install-the-sap-additional-application-server"></a><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Installieren des zusätzlichen SAP-Anwendungsservers
Installieren Sie einen zusätzlichen SAP-Anwendungsserver (Additional Application Server, AAS) auf allen virtuellen Computern, die Sie als Hosts von SAP-Anwendungsservern festgelegt haben. Beispielsweise auf <*SID*>-di-1 bis <*SID*>-di-<n>.

## <a name="a-name18aa2b9d92d24c0e8ddd5acaabda99e9a-test-the-sap-ascsscs-instance-failover-and-sios-replication"></a><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Testen des Failovers der SAP ASCS/SCS-Instanz und der SIOS-Replikation
Mithilfe des Failovercluster-Managers und der SIOS DataKeeper-Benutzeroberfläche können Sie ganz einfach das Failover für eine SAP ASCS/SCS-Instanz und die SIOS-Datenträgerreplikation testen und überwachen.

### <a name="a-name65fdef0f9f9441f9b314ea45bbfea445a-sap-ascsscs-instance-is-running-on-cluster-node-a"></a><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> Die SAP ASCS/SCS-Instanz wird auf Clusterknoten A ausgeführt
Die Clustergruppe **SAP WAC** wird auf dem Clusterknoten A ausgeführt, z.B. auf **ascsha-clna**. Weisen Sie den freigegebenen Datenträger S, der Teil der Clustergruppe **SAP WAC** ist und von der ASCS/SCS-Instanz verwendet wird, dem Clusterknoten A zu.

![Abbildung 59: Failovercluster-Manager: Die SAP-Clustergruppe <*SID*> wird auf Clusterknoten A ausgeführt][sap-ha-guide-figure-5000]

***Abbildung 59:** Failovercluster-Manager: Die SAP-Clustergruppe <*SID*> wird auf Clusterknoten A ausgeführt*

Auf der SIOS DataKeeper-Benutzeroberfläche erkennen Sie, dass die Daten auf dem freigegebenen Datenträger synchron vom Quellvolume S auf Clusterknoten A auf das Zielvolume S auf Clusterknoten B repliziert werden – also z.B. von **ascsha-clna [10.0.0.41]** nach **ascsha-clnb [10.0.0.42]**.

![Abbildung 60: In SIOS DataKeeper wird das lokale Volume von Clusterknoten A auf Clusterknoten B repliziert][sap-ha-guide-figure-5001]

***Abbildung 60:** In SIOS DataKeeper wird das lokale Volume von Clusterknoten A auf Clusterknoten B repliziert*

### <a name="a-name5e959fa98fcd49e5a12c37f6ba07b916a-failover-from-node-a-to-node-b"></a><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover von Knoten A auf Knoten B
Sie können eine dieser Optionen verwenden, um ein Failover der SAP-Clustergruppe <*SID*> von Clusterknoten A auf Clusterknoten B auszulösen:

* Mithilfe des Failovercluster-Managers  
* Mithilfe der Failovercluster-PowerShell
  ```powershell
  Move-ClusterGroup -Name "SAP WAC"
  ```
* Durch einen Neustart von Clusterknoten A im Windows-Gastbetriebssystem (dadurch wird ein automatisches Failover der SAP-Clustergruppe <*SID*> von Knoten A auf Knoten B ausgelöst)  
* Durch einen Neustart von Clusterknoten A im Azure-Portal (dadurch wird ein automatisches Failover der SAP-Clustergruppe <*SID*> von Knoten A auf Knoten B ausgelöst)  
* Durch einen Neustart von Clusterknoten A mit Azure PowerShell (dadurch wird ein automatisches Failover der SAP-Clustergruppe <*SID*> von Knoten A auf Knoten B ausgelöst)
  
  ```powershell
  Restart-AzureVM -Name ascsha-clna -ServiceName ascsha-cluster
  ```

Nach dem Failover wird die SAP-Clustergruppe <*SID*> auf dem Clusterknoten B ausgeführt, z.B. auf **ascsha-clnb**.

![Abbildung 61: Failovercluster-Manager: Die SAP-Clustergruppe <*SID*> wird auf Clusterknoten B ausgeführt][sap-ha-guide-figure-5002]

***Abbildung 61:** Failovercluster-Manager: Die SAP-Clustergruppe <*SID*> wird auf Clusterknoten B ausgeführt*

Der freigegebene Datenträger wird jetzt auf Clusterknoten B bereitgestellt. SIOS DataKeeper repliziert Daten vom Quellvolume S auf Clusterknoten B auf das Zielvolume S auf Clusterknoten A – also z.B. von **ascsha-clnb [10.0.0.42]** nach **ascsha-clna [10.0.0.41]**.

![Abbildung 62: SIOS DataKeeper repliziert das lokale Volume von Clusterknoten B auf Clusterknoten A][sap-ha-guide-figure-5003]

***Abbildung 62:** SIOS DataKeeper repliziert das lokale Volume von Clusterknoten B auf Clusterknoten A*

<!---HONumber=Oct16_HO2-->


