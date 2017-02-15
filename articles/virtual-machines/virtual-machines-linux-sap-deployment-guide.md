---
title: "SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Bereitstellungshandbuch | Microsoft Docs"
description: "SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Bereitstellungshandbuch"
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
translationtype: Human Translation
ms.sourcegitcommit: 0f8bc125855bc5a5b67fde5b0b742c73b5da7610
ms.openlocfilehash: d717acf4c8c758de45c7bd70e165c50584ec10a5


---
# <a name="sap-netweaver-on-azure-virtual-machines-vms--deployment-guide"></a>SAP NetWeaver auf virtuellen Azure-Computern – Bereitstellungshandbuch
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

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Bereitstellungshandbuch) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching für VMs und VHDs) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Struktur einer RDBMS-Bereitstellung) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Hohe Verfügbarkeit und Notfallwiederherstellung mit virtuellen Azure-Computern) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 und höher) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 und frühere Versionen) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Verwenden eines SQL Server-Images aus dem Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (SQL Server für SAP in Azure – Allgemeine Zusammenfassung) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Besonderheiten bei SQL Server-RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Speicherkonfiguration) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Sichern und Wiederherstellen) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Leistungsüberlegungen hinsichtlich Sicherung und Wiederherstellung) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Sonstiges) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Bereitstellungshandbuch) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP-Ressourcen) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Bereitstellungsszenarien für virtuelle Computer für SAP in Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Bereitstellen von Azure PowerShell-Cmdlets) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Herunterladen und Importieren von SAP-relevanten PowerShell-Cmdlets) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Einbinden virtueller Computer in die lokale Domäne – nur Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [deployment-guide-4.4]:virtual-machines-linux-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Herunterladen, Installieren und Aktivieren von Azure VM-Agent) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure-Befehlszeilenschnittstelle) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurieren der erweiterten Azure-Überwachung für SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Bereitschaftsüberprüfung für die erweiterte Azure-Überwachung für SAP) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver auf virtuellen Linux-Computern (VMs) – Planungs- und Implementierungshandbuch) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Ressourcen) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Hohe Verfügbarkeit (HA) und Disaster Recovery (DR) für SAP NetWeaver auf virtuellen Azure-Computern) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (Hohe Verfügbarkeit für SAP-Anwendungsserver) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Verwenden von Autostart für SAP-Instanzen) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Nur Cloud – Bereitstellungen virtueller Computer in Azure ohne Abhängigkeiten vom lokalen Kundennetzwerk) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Standortübergreifend – Bereitstellung einzelner oder mehrerer virtueller SAP-Computer in Azure mit der Anforderung der vollständigen Integration in das lokale Netzwerk) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure-Regionen) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fehlerdomänen) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgradedomänen) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure-Verfügbarkeitsgruppen) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Das Konzept der virtuellen Microsoft Azure-Computer) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Storage Premium) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Verschieben eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bereitstellen eines virtuellen Computers mit einem kundenspezifischen Image) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Vorbereitung des Verschiebens eines virtuellen lokalen Computers mit einem nicht generalisierten Datenträger in Azure) [planning-guide-5.2.2]:virtual-machines-linux-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Vorbereitung der Bereitstellung eines virtuellen Computers mit einem kundenspezifischen Image für SAP) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Vorbereiten virtueller Computer mit SAP für Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Unterschied zwischen einem Azure-Datenträger und einem Azure-Image) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Hochladen einer virtuellen Festplatte vom lokalen Standort in Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopieren von Datenträgern zwischen Azure-Speicherkonten) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM-/VHD-Struktur für SAP-Bereitstellungen) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Festlegen der automatischen Bereitstellung für angefügte Datenträger) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Schulungs-/Demoszenario mit einem einzelnen virtuellen Computer mit SAP NetWeaver) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Konzepte für Nur-Cloud-Bereitstellung von SAP-Instanzen) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure-Überwachungslösung für SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Storage Premium)

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
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../azure-resource-manager/resource-group-overview.md
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
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
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
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure ermöglicht Unternehmen das Abrufen von Server- und Speicherressourcen in kürzester Zeit ohne langwierige Beschaffungszyklen. Virtuelle Azure-Computer ermöglichen Unternehmen das Bereitstellen klassischer, beispielsweise SAP NetWeaver-basierter, Anwendungen in Azure sowie das Erweitern derer Zuverlässigkeit und Verfügbarkeit, ohne Bedarf an weiteren lokal verfügbaren Ressourcen. Microsoft Azure unterstützt auch standortübergreifende Konnektivität, wodurch Unternehmen virtuelle Azure-Computer aktiv in lokale Domänen, private Clouds und ihre SAP-Systemlandschaft integrieren können.

Dieses Whitepaper beschreibt Schritt für Schritt, wie ein virtueller Azure-Computer für die Bereitstellung von SAP NetWeaver-basierten Anwendungen vorbereitet wird. Es wird davon ausgegangen, dass die Informationen im [Planungs- und Implementierungshandbuch][planning-guide] bekannt sind. Andernfalls sollte das genannte Dokument zunächst gelesen werden.

Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

[!INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="introduction"></a>Einführung
SAP NetWeaver – insbesondere die SAP Business Suite – wird weltweit von einer großen Anzahl von Firmen verwendet, um unternehmenskritische Geschäftsprozesse auszuführen. Die Systemintegrität ist dabei eine wichtige Größe, sodass die Fähigkeit für unternehmensweiten Support im Fehlerfall und bei Leistungseinbrüchen zu einer entscheidenden Anforderung wird.
Microsoft Azure bietet eine überragende Plattforminstrumentierung für den Supportbedarf unternehmenskritischer Anwendungen. Dieses Handbuch bietet eine Anleitung dafür, virtuelle Microsoft Azure-Computer für die Bereitstellung von SAP-Software so zu konfigurieren, dass unternehmensweiter Support geleistet werden kann – und zwar unabhängig von der Erstellungsweise des virtuellen Computers und davon, ob der virtuelle Computer aus dem Azure Marketplace stammt oder ein kundenspezifisches Image eingesetzt wird.
Im Folgenden werden alle erforderlichen Einrichtungsschritte im Detail beschrieben.

## <a name="prerequisites-and-resources"></a>Voraussetzungen und Ressourcen
### <a name="prerequisites"></a>Voraussetzungen
Achten Sie vor dem Beginn darauf, dass alle in den folgenden Kapiteln beschriebenen Voraussetzungen erfüllt sind.

#### <a name="local-personal-computer"></a>Lokaler PC
Die Einrichtung eines virtuellen Azure-Computers zur Bereitstellung von SAP-Software umfasst mehrere Schritte. Zum Verwalten virtueller Windows- oder Linux-Computer werden ein PowerShell-Skript und das Microsoft Azure-Portal verwendet. Dazu ist ein lokaler PC mit Windows 7 oder höher erforderlich. Falls nur virtuelle Linux-Computer verwaltet werden sollen und Sie diese Aufgabe auf einem Linux-Computer durchführen möchten, kann auch die Azure-Befehlszeilenschnittstelle (Azure-CLI) verwendet werden.

#### <a name="internet-connection"></a>Internetverbindung
Für das Herunterladen und Ausführen der erforderlichen Tools und Skripts ist ein Internetzugang erforderlich. Zudem benötigt auch der virtuelle Microsoft Azure-Computer, der die Azure-Erweiterung zur verbesserten Überwachung ausführt, Zugang zum Internet. Falls dieser virtuelle Azure-Computer Teil eines virtuellen Azure-Netzwerks oder einer lokalen Domäne ist, achten Sie auf eine korrekte Einrichtung der entsprechenden Proxyeinstellungen. Diese werden im Kapitel [Konfigurieren des Proxys][deployment-guide-configure-proxy] dieses Dokuments beschrieben.

#### <a name="microsoft-azure-subscription"></a>Microsoft Azure-Abonnement
Diese Anleitung sieht vor, dass bereits ein Azure-Konto vorhanden ist und die zugehörigen Anmeldeinformationen bekannt sind.

#### <a name="topology-consideration-and-networking"></a>Topologieaspekte und Netzwerk
Für die SAP-Bereitstellung in Azure muss eine geeignete Topologie und Architektur festgelegt werden. Folgende Aspekte sind für die Architektur von Belang:

* Verwendete Microsoft Azure-Speicherkonten
* Virtuelles Netzwerk zur Bereitstellung des SAP-Systems
* Ressourcengruppe zur Bereitstellung des SAP-Systems
* Azure-Region zur Bereitstellung des SAP-Systems
* SAP-Konfiguration (zwei oder drei Ebenen)
* Größe der virtuellen Computer und Anzahl der zusätzlichen VHDs, die auf den VMs eingebunden werden sollen
* Konfiguration des SAP-Transport- und Korrektursystems

Azure-Speicherkonten oder virtuelle Azure-Netzwerke sollten bereits erstellt und konfiguriert sein. Eine Anleitung zu ihrer Erstellung und Konfiguration finden Sie im [Planungs- und Implementierungshandbuch][planning-guide].

#### <a name="sap-sizing"></a>SAP-Größenanpassung
* Die projektierte SAP-Workload sollte beispielsweise mit dem SAP-Quicksizer bereits bestimmt worden sein. Außerdem sollte die zugehörige SAPS-Zahl bekannt sein. 
* Die für das SAP-System erforderlichen CPU-Ressourcen und der Arbeitsspeicher sollten bekannt sein.
* Die erforderlichen E/A-Vorgänge pro Sekunde sollten bekannt sein.
* Die erforderliche Netzwerkbandbreite bei der endgültigen Kommunikation zwischen verschiedenen virtuellen Computern in Azure ist bekannt.
* Die erforderliche Netzwerkbandbreite zwischen den lokalen Assets und den über Azure bereitgestellten SAP-Systemen ist bekannt.

#### <a name="resource-groups"></a>Ressourcengruppen
Ressourcengruppen sind ein neues Konzept. Sie enthalten jeweils alle Ressourcen mit dem gleichen Lebenszyklus, da sie z.B. zur gleichen Zeit erstellt und gelöscht werden. Weitere Informationen zu Ressourcengruppen finden Sie in [diesem Artikel][resource-group-overview]. 

### <a name="a-name42ee2bdb-1efc-4ec7-ab31-fe4c22769b94asap-resources"></a><a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-Ressourcen
Während der Konfigurationsarbeit werden die folgenden Ressourcen benötigt:

* SAP-Hinweis [1928533]
  * Liste der für die Bereitstellung von SAP-Software unterstützten Größen für virtuelle Azure-Computer 
  * Wichtige Kapazitätsinformationen der einzelnen Größen virtueller Azure-Computer
  * Unterstützte SAP-Software sowie Kombination aus Betriebssystem und Datenbank
* SAP-Hinweis [2015553] mit Voraussetzungen von SAP für das Bereitstellen von SAP-Software in Microsoft Azure.
* SAP-Hinweis [1999351] mit zusätzlichen Informationen für die Problembehandlung der verbesserten Azure-Überwachung für SAP.
* SAP-Hinweis [2178632] mit ausführlichen Angaben zu den verfügbaren Überwachungsmetriken für SAP in Microsoft Azure. 
* SAP-Hinweis [1409604] mit der erforderlichen SAP-Host-Agent-Version für Windows in Microsoft Azure bei der Bereitstellung im neuen Azure Resource Manager.
* SAP-Hinweis [2191498] mit der erforderlichen SAP-Host-Agent-Version für Linux auf Microsoft Azure bei der Bereitstellung im neuen Azure Resource Manager.
* SAP-Hinweis [2243692] mit Informationen über die Lizenzierung für SAP unter Linux in Azure.
* SAP-Hinweis [1984787] enthält allgemeine Informationen zu SUSE LINUX Enterprise Server 12.
* SAP-Hinweis [2002167] enthält allgemeine Informationen zu Red Hat Enterprise Linux 7.x.
* [SCN](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) enthält alle erforderlichen SAP-Hinweise für Linux.
* SAP-spezifische PowerShell-Cmdlets, die Teil von [Azure PowerShell][azure-ps] sind
* SAP-spezifische Azure-CLI-Befehle, die Teil der [Azure-Befehlszeilenschnittstelle][azure-cli] sind
* [Microsoft Azure-Portal][azure-portal]

[comment]: <> (MSSedusch TODO Add ARM patch level for SAP Host Agent in SAP Note 1409604)

Die folgenden Anleitungen behandeln ebenfalls das Thema SAP in Microsoft Azure:

* [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch][planning-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – Bereitstellungshandbuch (dieses Dokument)][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]

## <a name="a-nameb3253ee3-d63b-4d74-a49b-185e76c4088eadeployment-scenarios-of-vms-for-sap-on-microsoft-azure"></a><a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Bereitstellungsszenarien für virtuelle Computer für SAP in Microsoft Azure
In diesem Kapitel lernen Sie die verschiedenen Arten der Bereitstellung kennen sowie die jeweils erforderlichen Einzelschritte.

### <a name="deployment-of-vms-for-sap"></a>Bereitstellung von virtuellen Computern für SAP
Microsoft Azure bietet mehrere Methoden zum Bereitstellen virtueller Computer und der zugehörigen Datenträger. Daher ist es wichtig, die Unterschiede zu kennen, da die Vorbereitung der virtuellen Computer abhängig von der Bereitstellungsart ist. Im Allgemeinen behandeln wir die folgenden Szenarios:

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers aus dem Azure Marketplace
Sie möchten ein Image aus dem Azure Marketplace für die Bereitstellung des virtuellen Computers verwenden, das von Microsoft oder einem Drittanbieter stammt. Nachdem Sie Ihren virtuellen Computer in Microsoft Azure bereitgestellt haben, befolgen Sie zum Installieren der SAP-Software in dem virtuellen Computer dieselben Richtlinien und Tools wie in einer lokalen Umgebung. Für die Installation der SAP-Software auf dem virtuellen Azure-Computer empfehlen SAP und Microsoft das Hochladen und Speichern der SAP-Installationsmedien in Azure-VHDs oder das Erstellen eines virtuellen Azure-Computers, der als „Dateiserver“ fungiert und die erforderlichen SAP-Installationsmedien enthält.

[comment]: <> (MSSedusch TODO why do we need to recommend a file management e.g. File Server or VHD? Is that so different from on-premises?)

Einzelheiten hierzu finden Sie im Kapitel [Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP][deployment-guide-3.2].

#### <a name="a-name3688666f-281f-425b-a312-a77e7db2dfabadeploying-a-vm-with-a-custom-image"></a><a name="3688666f-281f-425b-a312-a77e7db2dfab"></a>Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image
Aufgrund spezifischer Patch-Erfordernisse im Hinblick auf die beabsichtigte Betriebssystem- oder DBMS-Version sind die im Azure Marketplace erhältlichen Images möglicherweise nicht für Ihren Bedarf geeignet. Daher muss möglicherweise anhand eines eigenen, „privaten“ VM-Images mit Betriebssystem und Datenbank ein virtueller Computer erstellt werden, der anschließend mehrmals bereitgestellt werden kann.
Die Schritte zum Erstellen eines privaten Images unterscheiden sich für Windows- und Linux-Images.

- - -
> ![ Windows][Logo_Windows]  Windows
> 
> Um ein Windows-Image vorzubereiten, das zum Bereitstellen mehrerer virtueller Maschinen verwendet werden kann, müssen die Windows-Einstellungen (wie Windows-SID und Hostname) auf dem lokalen virtuellen Computer abstrahiert/generalisiert werden. Hierzu können Sie „sysprep“ verwenden (wie unter <https://technet.microsoft.com/library/cc721940.aspx> beschrieben).
> 
> ![Linux][Logo_Linux]  Linux
> 
> Um ein Linux-Image vorzubereiten, das zum Bereitstellen mehrerer virtueller Maschinen verwendet werden kann, müssen einige Linux-Einstellungen auf dem lokalen virtuellen Computer abstrahiert/generalisiert werden. Dies kann mithilfe von „waagent -deprovision“ durchgeführt werden, wie in [diesem Artikel][virtual-machines-linux-capture-image] und in [diesem Artikel][virtual-machines-linux-agent-user-guide-command-line-options] beschrieben.
> 
> 

- - -
Sie können den Datenbankinhalt einrichten, indem Sie entweder den SAP Software Provision Manager verwenden, um ein neues SAP-System zu installieren, eine Datenbanksicherung von einer VHD wiederherstellen, die an den virtuellen Computer angeschlossen ist, oder direkt eine Datenbanksicherung aus Azure Storage wiederherstellen, falls das DBMS dies unterstützt. (Weitere Informationen finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide].) Wenn Sie bereits ein SAP-System auf dem lokalen virtuellen Computer installiert haben (insbesondere bei Systemen mit zwei Ebenen), können Sie die SAP-Systemeinstellungen nach der Bereitstellung des virtuellen Azure-Computers über das vom SAP Software Provisioning Manager unterstützte Verfahren zum Umbenennen von Systemen anpassen (SAP-Hinweis [1619720]). Andernfalls können Sie die SAP-Software auch später, nach der Bereitstellung des virtuellen Azure-Computers installieren.

Einzelheiten hierzu finden Sie im Kapitel [Szenario 2: Bereitstellen eines virtuellen Computers mit benutzerdefiniertem Image für SAP][deployment-guide-3.3].

#### <a name="moving-a-vm-from-on-premises-to-microsoft-azure-with-a-non-generalized-disk"></a>Verschieben eines virtuellen lokalen Computers nach Microsoft Azure mit einem nicht generalisierten Datenträger
Sie planen, ein bestimmtes lokales SAP-System nach Microsoft Azure zu verschieben. Dies kann durch Hochladen einer VHD mit dem Betriebssystem sowie den SAP- und eventuell DBMS-Binärdateien erfolgen. Zusätzlich müssen VHDs mit den Daten und Protokolldateien des DBMS nach Microsoft Azure hochgeladen werden. Im Gegensatz zu dem Szenario aus dem oben genannten Kapitel [Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image][deployment-guide-3.1.2] werden Hostname, SAP-SID und SAP-Benutzerkonten auf dem virtuellen Azure-Computer entsprechend der lokalen Konfiguration weitergeführt. Daher ist eine Generalisierung des Betriebssystems nicht erforderlich. Dieser Anwendungsfall gilt hauptsächlich für standortübergreifende Szenarios, bei denen ein Teil der SAP-Landschaft lokal und andere Teile in Microsoft Azure ausgeführt werden.

Weitere Einzelheiten finden Sie in [Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP][deployment-guide-3.4].

### <a name="a-namedb477013-9060-4602-9ad4-b0316f8bb281ascenario-1-deploying-a-vm-out-of-the-azure-marketplace-for-sap"></a><a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Szenario 1: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace für SAP
Microsoft Azure bietet die Möglichkeit, eine VM-Instanz aus dem Azure Marketplace bereitzustellen, in dem Sie einige Standard-Betriebssystemimages von Windows Server und verschiedenen Linux-Distributionen finden. Es ist auch möglich, ein Image bereitzustellen, das DBMS-SKUs enthält, beispielsweise SQL Server. Details zur Verwendung dieser Images mit DBMS-SKUs finden Sie im [DBMS-Bereitstellungshandbuch][dbms-guide].

Die SAP-spezifische Abfolge der Schritte zum Bereitstellen eines virtuellen Computers aus dem Azure Marketplace ist folgende:

![Flussdiagramm der VM-Bereitstellung für SAP-Systeme mithilfe eines VM-Images aus Azure Marketplace][deployment-guide-figure-100]

Gemäß dem Flussdiagramm müssen die folgenden Schritte ausgeführt werden:

#### <a name="create-virtual-machine-using-the-azure-portal"></a>Erstellen eines virtuellen Computer über das Azure-Portal
Die einfachste Möglichkeit zum Erstellen eines neuen virtuellen Computers mithilfe eines Images aus dem Azure Marketplace ist über das Azure-Portal. Navigieren Sie zu <https://portal.azure.com/#create>. Geben Sie das Betriebssystem, das Sie bereitstellen möchten, z.B. Windows, SLES oder RHEL in das Suchfeld ein, und wählen Sie die Version aus. Achten Sie darauf, das Bereitstellungsmodell „Azure Resource Manager“ auszuwählen, und klicken Sie auf „Erstellen“.

Der Assistent führt Sie durch die erforderlichen Parameter zum Erstellen des virtuellen Computers mit allen erforderlichen Ressourcen wie Netzwerkschnittstellen oder Speicherkonten. Diese Parameter sind beispielsweise:

1. Grundlagen
   1. Name: Ressourcenname bzw. Name des virtuellen Computers
   2. Benutzername und Kennwort/öffentlicher SSH-Schlüssel: Geben Sie Benutzernamen und Kennwort des Benutzers ein, der während der Bereitstellung erstellt wird. Für einen virtuellen Linux-Computer können Sie auch den öffentlichen SSH-Schlüssel eingeben, der beim Anmelden an dem Computer mittels SSH verwendet werden soll.
   3. Abonnement: Wählen Sie das Abonnement aus, das Sie verwenden möchten, um den neuen virtuellen Computer bereitzustellen.
   4. Ressourcengruppe: Name der Ressourcengruppe. Sie können entweder den Namen einer neuen oder einer bereits vorhanden Ressourcengruppe einfügen.
   5. Speicherort: Wählen Sie den Speicherort aus, in dem der neue virtuelle Computer bereitgestellt werden soll. Wenn Sie den virtuellen Computer mit dem lokalen Netzwerk verbinden möchten, achten Sie darauf, den Speicherort des virtuellen Netzwerks auszuwählen, das Azure mit dem lokalen Netzwerk verbindet. Weitere Informationen finden Sie im [Planungshandbuch][planning-guide] im Kapitel [Microsoft Azure-Netzwerk][planning-guide-microsoft-azure-networking].
2. Größe: Lesen Sie den SAP-Hinweis [1928533] , der eine Liste der unterstützten VM-Typen enthält. Achten Sie auch darauf, den richtigen Typ auszuwählen, wenn Sie Storage Premium verwenden möchten. Nicht alle Typen von virtuellen Computern unterstützten Storage Premium. Weitere Informationen finden Sie im [Planungshandbuch][planning-guide] in den Kapiteln [Speicher: Microsoft Azure Storage und Datenträger][planning-guide-storage-microsoft-azure-storage-and-data-disks] und [Azure Storage Premium][planning-guide-azure-premium-storage].
3. Einstellungen
   1. Speicherkonto: Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. Lesen Sie im [DBMS-Handbuch][dbms-guide] das Kapitel [Microsoft Azure Storage][dbms-guide-2.3], um weitere Informationen zu den verschiedenen Speichertypen zu erhalten. Beachten Sie, dass nicht alle Speichertypen für die Ausführung von SAP-Anwendungen unterstützt werden.
   2. Virtuelles Netzwerk und Subnetz: Wählen Sie das virtuelle Netzwerk aus, das mit dem lokalen Netzwerk verbunden ist, wenn Sie den virtuellen Computer in das Intranet integrieren möchten.
   3. Öffentliche IP-Adresse: Wählen Sie die öffentliche IP-Adresse aus, die Sie verwenden möchten, oder geben Sie die Parameter ein, um eine neue öffentliche IP-Adresse zu erstellen. Mithilfe der öffentlichen IP-Adresse können Sie über das Internet auf den virtuellen Computer zugreifen. Achten Sie auch darauf, eine Netzwerksicherheitsgruppe zu erstellen, um den Zugriff auf den virtuellen Computer zu filtern.
   4. Netzwerksicherheitsgruppe: Weitere Details finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)][virtual-networks-nsg].
   5. Überwachung: Sie können die Diagnoseeinstellung deaktivieren. Sie wird automatisch aktiviert, wenn die Befehle zum Aktivieren der erweiterten Azure-Überwachung ausgeführt werden. Dies wird im Kapitel [Konfigurieren der Überwachung][deployment-guide-configure-monitoring-scenario-1] beschrieben.
   6. Verfügbarkeit: Wählen Sie eine vorhandene Verfügbarkeitsgruppe aus, oder geben Sie die Parameter zum Erstellen einer neuen ein. Weitere Informationen finden Sie im Kapitel [Azure-Verfügbarkeitsgruppen][planning-guide-3.2.3].
4. Zusammenfassung: Überprüfen Sie die Informationen auf der Übersichtsseite, und klicken Sie auf „OK“.

Nachdem der Assistent beendet wird, erfolgt die Bereitstellung des virtuellen Computers in der ausgewählten Ressourcengruppe.

#### <a name="create-virtual-machine-using-a-template"></a>Erstellen virtueller Computer mithilfe einer Vorlage
Sie können eine Bereitstellung auch mithilfe der im GitHub-Repository [azure-quickstart-templates][azure-quickstart-templates-github] veröffentlichten SAP-Vorlagen erstellen. Oder Sie erstellen über das [Azure-Portal][virtual-machines-windows-tutorial], mit [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms] oder mithilfe der [Azure-Befehlszeilenschnittstelle][virtual-machines-linux-tutorial] manuell einen virtuellen Computer.

* [Vorlage für die Konfiguration mit 2 Ebenen (nur ein virtueller Computer)][sap-templates-2-tier-marketplace-image]: Verwenden Sie diese Vorlage, wenn Sie ein System mit zwei Ebenen und nur einem virtuellen Computer erstellen möchten.
* [Vorlage für die Konfiguration mit 3 Ebenen (mehrere virtuelle Computer)][sap-templates-3-tier-marketplace-image]: Verwenden Sie diese Vorlage, wenn Sie ein System mit drei Ebenen und mehreren virtuellen Computern erstellen möchten.

Nach dem Öffnen einer der oben genannten Vorlagen wechselt das Azure-Portal zum Bereich „Parameter bearbeiten“. Geben Sie Folgendes ein:

* **sapSystemId**: SAP-System-ID
* **osType**: Betriebssystem für die Bereitstellung (etwa Windows Server 2012 R2, SLES 12 oder RHEL 7.2)
  * Die Liste enthält nur Versionen, die von SAP auf Microsoft Azure unterstützt werden.
* **sapSystemSize**: Größe des SAP-Systems
  * Die Anzahl der vom neuen System bereitgestellten SAPS. Wenn Sie nicht sicher sind, wie viele SAPS das System benötigt, wenden Sie sich an den SAP-Technologiepartner oder Systemintegrator.
* **systemAvailability**: (nur Vorlage für 3 Ebenen) Verfügbarkeit des Systems 
  * Wählen Sie „HA“ (hohe Verfügbarkeit) aus, um eine Konfiguration für eine Installation mit hoher Verfügbarkeit zu erhalten. Es werden zwei Datenbankserver und zwei Server für ASCS erstellt.
* storageType: (nur Vorlage für 2 Ebenen) Zu verwendender Speichertyp 
  * Für größere Systeme wird dringend die Verwendung von Storage Premium empfohlen. Weitere Informationen zu den anderen Speichertypen finden Sie unter 
    * [Microsoft Azure Storage][dbms-guide-2.3] im [DBMS-Handbuch][dbms-guide].
    * [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer][storage-premium-storage-preview-portal]
    * [Einführung in Microsoft Azure Storage][storage-introduction]
* **adminUsername** und **adminPassword**: Benutzername und Kennwort
  * Es wird ein neuer Benutzer erstellt, der sich am Computer anmelden kann.
* **newOrExistingSubnet**: Bestimmt, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, das mit dem lokalen Netzwerk verbunden ist, wählen Sie hier „vorhanden“ aus.
* **subnetId**: Die ID des Subnetzes, mit dem die virtuellen Computer eine Verbindung herstellen sollen. Wählen Sie das Subnetz des virtuellen VPN- oder Express Route-Netzwerks aus, um den virtuellen Computer mit dem lokalen Netzwerk zu verbinden. Die ID sieht in der Regel wie folgt aus: /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`>

Nachdem Sie alle Parameter eingegeben haben, wählen Sie das Abonnement und die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine vorhandene Ressourcengruppe auswählen oder durch Auswahl von „+ Neu“ im Dropdownmenü eine neue erstellen. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie auch die Region auswählen, in der die Ressourcengruppe und der virtuelle Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen, und akzeptieren Sie sie. Klicken Sie anschließend auf „Erstellen“.

Beachten Sie, dass der Azure-VM-Agent bei Verwendung eines Images aus dem Azure Marketplace standardmäßig bereitgestellt wird.

#### <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen
Abhängig von der lokalen Netzwerkkonfiguration kann es erforderlich sein, den Proxy auf Ihrem virtuellen Computer zu konfigurieren, wenn dieser mit dem lokalen Netzwerk über VPN oder Express Route verbunden ist. Andernfalls kann der virtuelle Computer möglicherweise nicht auf das Internet zugreifen und in diesem Fall die erforderlichen Erweiterungen nicht herunterladen und keine Überwachungsdaten sammeln. Weitere Informationen hierzu finden Sie in diesem Dokument im Kapitel [Konfigurieren von Proxys][deployment-guide-configure-proxy].

#### <a name="join-domain-windows-only"></a>Beitreten zu einer Domäne (nur Windows)
Falls die Bereitstellung in Azure über Azure Standort-zu-Standort oder Express Route mit dem lokalen AD/DNS verbunden ist (im [Planungs- und Implementierungshandbuch][planning-guide] auch als standortübergreifend bezeichnet), wird davon ausgegangen, dass der virtuelle Computer einer lokalen Domäne beitritt. Überlegungen zu diesem Schritt finden Sie in diesem Dokument im Kapitel [Beitreten eines virtuellen Computers zu einer lokalen Domäne (nur Windows)][deployment-guide-4.3].

#### <a name="a-nameec323ac3-1de9-4c3a-b770-4ff701def65baconfigure-monitoring"></a><a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Konfigurieren der Überwachung
Konfigurieren Sie die Azure-Erweiterung zur verbesserten Überwachung für SAP wie in diesem Dokument im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben.

Überprüfen Sie die Voraussetzungen für die SAP-Überwachung im Hinblick auf die erforderlichen Mindestversionen des SAP-Kernels und des SAP-Host-Agents in den Ressourcen, die in diesem Dokument im Kapitel [SAP-Ressourcen][deployment-guide-2.2] aufgeführt sind.

#### <a name="monitoring-check"></a>Überprüfen der Überwachung
Überprüfen Sie, ob die Überwachung wie im Kapitel [Überprüfungen und Problembehandlung für die Einrichtung der Ende-zu-Ende-Überwachung für SAP in Azure][deployment-guide-troubleshooting-chapter] beschrieben funktioniert.

#### <a name="post-deployment-steps"></a>Schritte nach der Bereitstellung
Nach dem Erstellen des virtuellen Computers wird dieser bereitgestellt. Es liegt danach in Ihrer Verantwortung, die erforderlichen Softwarekomponenten auf dem virtuellen Computer zu installieren. Daher erfordert diese Art der VM-Bereitstellung, dass die Software entweder bereits auf einem anderen in Microsoft Azure erreichbaren virtuellen Computer vorhanden ist, oder in Form eines Datenträgers eingebunden wird. Denkbar sind auch standortübergreifende Szenarios, in denen der Zugriff auf lokale Assets (Installationsfreigaben) bereits gesichert ist.

### <a name="a-name54a1fc6d-24fd-4feb-9c57-ac588a55dff2ascenario-2-deploying-a-vm-with-a-custom-image-for-sap"></a><a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Szenario 2: Bereitstellen eines virtuellen Computers mit einem benutzerdefinierten Image für SAP
Wie bereits ausführlich im [Planungs- und Implementierungshandbuch][planning-guide] beschrieben, besteht die Möglichkeit, ein benutzerdefiniertes Image vorzubereiten und zu erstellen, mit dem dann mehrere neue virtuelle Computer erstellt werden. Die Reihenfolge der Schritte im Flussdiagramm ist folgende:

![Flussdiagramm der VM-Bereitstellung für SAP-Systeme mithilfe eines VM-Images aus dem privaten Marketplace][deployment-guide-figure-300]

Gemäß dem Flussdiagramm müssen die folgenden Schritte ausgeführt werden:

#### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Verwenden Sie zum Erstellen einer Bereitstellung über ein privates Betriebssystemimage im Azure-Portal eine der im GitHub-Repository [azure-quickstart-templates github repository][azure-quickstart-templates-github] veröffentlichten SAP-Vorlagen.
Sie können auch mithilfe von [PowerShell][virtual-machines-upload-image-windows-resource-manager] einen virtuellen Computer manuell erstellen. 

* [Vorlage für die Konfiguration mit 2 Ebenen (nur ein virtueller Computer)][sap-templates-2-tier-user-image]: Verwenden Sie diese Vorlage, wenn Sie ein System mit zwei Ebenen, nur einem virtuellen Computer und Ihrem eigenen Betriebssystemimage erstellen möchten.
* [Vorlage für die Konfiguration mit 3 Ebenen (mehrere virtuelle Computer)][sap-templates-3-tier-user-image]: Verwenden Sie diese Vorlage, wenn Sie ein System mit drei Ebenen, mehreren virtuellen Computern und Ihrem eigenen Betriebssystemimage erstellen möchten.

Nach dem Öffnen einer der oben genannten Vorlagen wechselt das Azure-Portal zum Bereich „Parameter bearbeiten“. Geben Sie Folgendes ein:

* **sapSystemId**: SAP-System-ID
* **osType**: Betriebssystem für die Bereitstellung, Windows oder Linux
* **sapSystemSize**: Größe des SAP-Systems
  * Die Anzahl der vom neuen System bereitgestellten SAPS. Wenn Sie nicht sicher sind, wie viele SAPS das System benötigt, wenden Sie sich an den SAP-Technologiepartner oder Systemintegrator.
* **systemAvailability**: (nur Vorlage für 3 Ebenen) Verfügbarkeit des Systems 
  * Wählen Sie „HA“ (hohe Verfügbarkeit) aus, um eine Konfiguration für eine Installation mit hoher Verfügbarkeit zu erhalten. Es werden zwei Datenbankserver und zwei Server für ASCS erstellt.
* **storageType**: (nur Vorlage für 2 Ebenen) Zu verwendender Speichertyp 
  * Für größere Systeme wird dringend die Verwendung von Storage Premium empfohlen. Weitere Informationen zu den anderen Speichertypen finden Sie unter 
    * [Microsoft Azure Storage][dbms-guide-2.3] im [DBMS-Handbuch][dbms-guide].
    * [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer][storage-premium-storage-preview-portal]
    * [Einführung in Microsoft Azure Storage][storage-introduction]
* **adminUsername** und **adminPassword**: Benutzername und Kennwort
  * Es wird ein neuer Benutzer erstellt, der sich am Computer anmelden kann.
* **userImageVhdUri**: URI der VHD mit dem privaten Betriebssystemimage. Beispiel: https://`<accountname`>.blob.core.windows.net/vhds/userimage.vhd.
* **userImageStorageAccount**: Name des Speicherkontos, in dem das private Betriebssystemimage gespeichert ist. Beispiel: `<accountname`> (im obigen Beispiel-URI).
* **newOrExistingSubnet**: Bestimmt, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, das mit dem lokalen Netzwerk verbunden ist, wählen Sie hier „vorhanden“ aus.
* **subnetId**: Die ID des Subnetzes, mit dem die virtuellen Computer eine Verbindung herstellen sollen. Wählen Sie das Subnetz des virtuellen VPN- oder Express Route-Netzwerks aus, um den virtuellen Computer mit dem lokalen Netzwerk zu verbinden. Die ID sieht in der Regel wie folgt aus: /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`>

Nachdem Sie alle Parameter eingegeben haben, wählen Sie das Abonnement und die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine vorhandene Ressourcengruppe auswählen oder durch Auswahl von „+ Neu“ im Dropdownmenü eine neue erstellen. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie auch die Region auswählen, in der die Ressourcengruppe und der virtuelle Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen, und akzeptieren Sie sie. Klicken Sie anschließend auf „Erstellen“.

#### <a name="install-vm-agent-linux-only"></a>Installieren des VM-Agents (nur Linux)
Der Linux-Agent muss bereits im Benutzerimage installiert sein, wenn die oben genannten Vorlagen verwendet werden sollen. Andernfalls schlägt die Bereitstellung fehl. Laden Sie den VM-Agent in das Benutzerimage herunter, und installieren Sie ihn. Eine Beschreibung finden Sie in diesem Dokument im Kapitel [Herunterladen, Installieren und Aktivieren von Azure VM-Agent][deployment-guide-4.4].
Wenn Sie die oben genannten Vorlagen nicht verwenden, können Sie den VM-Agent auch nachträglich installieren.

#### <a name="join-domain-windows-only"></a>Beitreten zu einer Domäne (nur Windows)
Falls die Bereitstellung in Azure über Azure Standort-zu-Standort oder Express Route mit dem lokalen AD/DNS verbunden ist (im [Planungs- und Implementierungshandbuch][planning-guide] auch als standortübergreifend bezeichnet), wird davon ausgegangen, dass der virtuelle Computer einer lokalen Domäne beitritt. Überlegungen zu diesem Schritt finden Sie in diesem Dokument im Kapitel [Beitreten eines virtuellen Computers zu einer lokalen Domäne (nur Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen
Abhängig von der lokalen Netzwerkkonfiguration kann es erforderlich sein, den Proxy auf Ihrem virtuellen Computer zu konfigurieren, wenn dieser mit dem lokalen Netzwerk über VPN oder Express Route verbunden ist. Andernfalls kann der virtuelle Computer möglicherweise nicht auf das Internet zugreifen und in diesem Fall die erforderlichen Erweiterungen nicht herunterladen und keine Überwachungsdaten sammeln. Weitere Informationen hierzu finden Sie in diesem Dokument im Kapitel [Konfigurieren von Proxys][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurieren der Überwachung
Konfigurieren Sie die Azure-Erweiterung zur Überwachung für SAP, wie in diesem Dokument im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben.
Überprüfen Sie die Voraussetzungen für die SAP-Überwachung im Hinblick auf die erforderlichen Mindestversionen des SAP-Kernels und des SAP-Host-Agents in den Ressourcen, die in diesem Dokument im Kapitel [SAP-Ressourcen][deployment-guide-2.2] aufgeführt sind.

#### <a name="monitoring-check"></a>Überprüfen der Überwachung
Überprüfen Sie, ob die Überwachung wie im Kapitel [Überprüfungen und Problembehandlung für die Einrichtung der Ende-zu-Ende-Überwachung für SAP in Azure][deployment-guide-troubleshooting-chapter] beschrieben funktioniert.

### <a name="a-namea9a60133-a763-4de8-8986-ac0fa33aa8c1ascenario-3-moving-a-vm-from-on-premises-using-a-non-generalized-azure-vhd-with-sap"></a><a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Szenario 3: Verschieben eines virtuellen lokalen Computers mit einer nicht generalisierten virtuellen Azure-Festplatte mit SAP
In diesem Szenario wird die direkte Übertragung eines SAP-Systems ohne strukturelle Anpassung von der lokalen Installation nach Azure betrachtet. Insbesondere findet keine Änderung des Windows- oder Linux-Hostnamens oder der SAP SID statt. In diesem Fall wird die VHD nicht als Image während der Bereitstellung verwendet, sondern direkt als Betriebssystemlaufwerk verwendet. Im Hinblick auf die Bereitstellung unterscheidet sich dieser Anwendungsfall von den vorherigen beiden Fällen, da der VM-Agent während der Bereitstellung nicht automatisch installiert werden kann. Der Azure-VM-Agent muss von Microsoft heruntergeladen, installiert und anschließend im virtuellen Computer aktiviert werden. Nachdem diese Aufgabe erfolgreich ausgeführt wurde, kann die Einrichtung der Azure-Erweiterung für die SAP-Hostüberwachung und ihre Konfiguration fortgesetzt werden. Einzelheiten zur Funktion des Azure-VM-Agents finden Sie in folgendem Artikel:

[comment]: <> (MSSedusch TODO Update Windows Link below) 

- - -
> ![Windows][Logo_Windows] Windows
> 
> <http://blogs.msdn.com/b/wats/archive/2014/02/17/bginfo-guest-agent-extension-for-azure-vms.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> [Benutzerhandbuch für Azure Linux-Agent][virtual-machines-linux-agent-user-guide]
> 
> 

- - -
Der Workflow der verschiedenen Schritte sieht wie folgt aus:

![Flussdiagramm der VM-Bereitstellung für SAP-Systeme mithilfe eines VM-Datenträgers][deployment-guide-figure-400]

Wenn der Datenträger bereits hochgeladen und in Azure definiert wurde (Informationen hierzu finden Sie im [Planungs- und Implementierungshandbuch][planning-guide]), führen Sie folgende Schritte aus:

#### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Verwenden Sie zum Erstellen einer Bereitstellung über einen privaten Betriebssystem-Datenträger im Azure-Portal die im GitHub-Repository [azure-quickstart-templates github repository][azure-quickstart-templates-github] veröffentlichte SAP-Vorlage. Sie können auch mithilfe von PowerShell oder der Azure-CLI einen virtuellen Computer manuell erstellen.

* [Vorlage für die Konfiguration mit 2 Ebenen (nur ein virtueller Computer)][sap-templates-2-tier-os-disk]
  *  Verwenden Sie diese Vorlage, wenn Sie ein System mit 2 Ebenen und nur einem virtuellen Computer erstellen möchten.

Nach dem Öffnen der oben genannten Vorlage wechselt das Azure-Portal zum Anzeigebereich „Parameter bearbeiten“. Geben Sie Folgendes ein:

* **sapSystemId**: SAP-System-ID
* **osType**: Betriebssystem für die Bereitstellung, Windows oder Linux
* **sapSystemSize**: Größe des SAP-Systems
  * Die Anzahl der vom neuen System bereitgestellten SAPS. Wenn Sie nicht sicher sind, wie viele SAPS das System benötigt, wenden Sie sich an den SAP-Technologiepartner oder Systemintegrator.
* **storageType**: (nur Vorlage für 2 Ebenen) Zu verwendender Speichertyp 
  * Für größere Systeme wird dringend die Verwendung von Storage Premium empfohlen. Weitere Informationen zu den anderen Speichertypen finden Sie unter 
    * [Microsoft Azure Storage][dbms-guide-2.3] im [DBMS-Handbuch][dbms-guide].
    * [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer][storage-premium-storage-preview-portal]
    * [Einführung in Microsoft Azure Storage][storage-introduction]
* **osDiskVhdUri**: URI des privaten Betriebssystem-Datenträgers. Beispiel: https://`<accountname`>.blob.core.windows.net/vhds/osdisk.vhd.
* **newOrExistingSubnet**: Bestimmt, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, das mit dem lokalen Netzwerk verbunden ist, wählen Sie hier „vorhanden“ aus.
* **subnetId**: Die ID des Subnetzes, mit dem die virtuellen Computer eine Verbindung herstellen sollen. Wählen Sie das Subnetz des virtuellen VPN- oder Express Route-Netzwerks aus, um den virtuellen Computer mit dem lokalen Netzwerk zu verbinden. Die ID sieht in der Regel wie folgt aus: /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`>

Nachdem Sie alle Parameter eingegeben haben, wählen Sie das Abonnement und die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine vorhandene Ressourcengruppe auswählen oder durch Auswahl von „+ Neu“ im Dropdownmenü eine neue erstellen. Wenn Sie eine neue Ressourcengruppe erstellen, müssen Sie auch die Region auswählen, in der die Ressourcengruppe und der virtuelle Computer erstellt werden.

Überprüfen Sie die Vertragsbedingungen, und akzeptieren Sie sie. Klicken Sie anschließend auf „Erstellen“.

#### <a name="install-vm-agent"></a>Installieren des VM-Agents
Der Linux-Agent muss bereits im Betriebssystem-Datenträger installiert sein, wenn die oben genannten Vorlagen verwendet werden sollen. Andernfalls schlägt die Bereitstellung fehl. Laden Sie den VM-Agent auf den virtuellen Computer herunter, und installieren Sie ihn. Eine Beschreibung finden Sie in diesem Dokument im Kapitel [Herunterladen, Installieren und Aktivieren von Azure VM-Agent][deployment-guide-4.4].

Wenn Sie die oben genannten Vorlagen nicht verwenden, können Sie den VM-Agent auch nachträglich installieren.

#### <a name="join-domain-windows-only"></a>Beitreten zu einer Domäne (nur Windows)
Falls die Bereitstellung in Azure über Azure Standort-zu-Standort oder Express Route mit dem lokalen AD/DNS verbunden ist (im [Planungs- und Implementierungshandbuch][planning-guide] auch als standortübergreifend bezeichnet), wird davon ausgegangen, dass der virtuelle Computer einer lokalen Domäne beitritt. Überlegungen zu diesem Schritt finden Sie in diesem Dokument im Kapitel [Beitreten eines virtuellen Computers zu einer lokalen Domäne (nur Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen
Abhängig von der lokalen Netzwerkkonfiguration kann es erforderlich sein, den Proxy auf Ihrem virtuellen Computer zu konfigurieren, wenn dieser mit dem lokalen Netzwerk über VPN oder Express Route verbunden ist. Andernfalls kann der virtuelle Computer möglicherweise nicht auf das Internet zugreifen und in diesem Fall die erforderlichen Erweiterungen nicht herunterladen und keine Überwachungsdaten sammeln. Weitere Informationen hierzu finden Sie in diesem Dokument im Kapitel [Konfigurieren von Proxys][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Konfigurieren der Überwachung
Konfigurieren Sie die Azure-Erweiterung zur verbesserten Überwachung für SAP wie in diesem Dokument im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben.

Überprüfen Sie die Voraussetzungen für die SAP-Überwachung im Hinblick auf die erforderlichen Mindestversionen des SAP-Kernels und des SAP-Host-Agents in den Ressourcen, die in diesem Dokument im Kapitel [SAP-Ressourcen][deployment-guide-2.2] aufgeführt sind.

#### <a name="monitoring-check"></a>Überprüfen der Überwachung
Überprüfen Sie, ob die Überwachung wie im Kapitel [Überprüfungen und Problembehandlung für die Einrichtung der Ende-zu-Ende-Überwachung für SAP in Azure][deployment-guide-troubleshooting-chapter] beschrieben funktioniert.

### <a name="scenario-4-updating-the-monitoring-configuration-for-sap"></a>Szenario 4: Aktualisieren der Überwachungskonfiguration für SAP
Es gibt Fälle, in denen die Überwachungskonfiguration aktualisiert werden sollte:

* Das gemeinsame MS/SAP-Team hat die Überwachungsfunktionen erweitert und entschieden, einige Indikatoren hinzuzufügen und andere zu entfernen. 
* Microsoft führt eine neue Version der zugrunde liegenden Azure-Infrastruktur für die Lieferung der Überwachungsdaten ein, und die Azure-Erweiterung zur verbesserten Überwachung für SAP wird an diese Änderungen angepasst.
* Es werden zusätzliche VHDs in den virtuellen Azure-Computer eingebunden oder von diesem entfernt. In diesem Fall muss die Sammlung der speicherbezogenen Daten aktualisiert werden. Wenn die Konfiguration durch Hinzufügen oder Löschen von Endpunkten oder Zuweisen von IP-Adressen zu einem virtuellen Computer geändert wird, wirkt sich dies nicht auf die Konfiguration die Überwachung aus.
* Die Größe des virtuellen Azure-Computers wird z.B. von A5 auf eine andere VM-Größe geändert.
* Dem virtuellen Azure-Computer werden neue Netzwerkschnittstellen hinzugefügt.

Gehen Sie wie folgt vor, um die Überwachungskonfiguration zu aktualisieren:

* Aktualisieren Sie die Überwachungsinfrastruktur anhand der Schritte in Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] in diesem Dokument. Bei einer erneuten Ausführung des in diesem Kapitel genannten Skripts wird erkannt, dass eine Überwachungskonfiguration bereitgestellt wurde, und die erforderlichen Änderungen an der Überwachungskonfiguration werden vorgenommen. 

- - -
> ![ Windows][Logo_Windows]  Windows
> 
> Zur Aktualisierung des Azure-VM-Agents ist kein Benutzereingriff erforderlich. Der VM-Agent aktualisiert sich selbsttätig und erfordert keinen Neustart des virtuellen Computers.
> 
> ![ Linux][Logo_Linux] Linux
> 
> Führen Sie die Schritte in [diesem Artikel][virtual-machines-linux-update-agent] aus, um den Azure Linux-Agent zu aktualisieren. 
> 
> 

- - -
## <a name="detailed-single-deployment-steps"></a>Ausführliche Schritte für die einzelnen Bereitstellungen
### <a name="a-name604bcec2-8b6e-48d2-a944-61b0f5dee2f7adeploying-azure-powershell-cmdlets"></a><a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Bereitstellen von Azure PowerShell-Cmdlets
* Wechseln Sie zu <https://azure.microsoft.com/downloads/>.
* Im Abschnitt „Befehlszeilentools“ befindet sich der Unterabschnitt „PowerShell“. Folgen Sie dem Link „Windows-Installation“.
* Der Microsoft-Download-Manager wird aufgerufen und zeigt einen Eintrag mit der Endung „.exe“. Wählen Sie die Option „Ausführen“ aus.
* Sie werden in einem Popup gefragt, ob der Microsoft-Webplattform-Installer ausgeführt werden soll. Klicken Sie auf „Ja“.
* Ein Bildschirm wie der folgende wird angezeigt:

![Installationsbildschirm für Azure PowerShell-Cmdlets][deployment-guide-figure-500]
<a name="figure-5"></a>

* Klicken Sie auf „Installieren“, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Überprüfen Sie regelmäßig, ob die PowerShell-Cmdlets aktualisiert wurden. In der Regel erfolgt eine monatliche Aktualisierung. Der einfachste Weg zur Überprüfung ist das Befolgen der oben genannten Installationsschritte bis zum Installationsbildschirm wie in [dieser][deployment-guide-figure-5] Abbildung. In diesem Bildschirm werden das Veröffentlichungsdatum der Cmdlets sowie die aktuelle Versionsnummer angezeigt Sofern in den SAP-Hinweisen [1928533] oder [2015553] nicht anders angegeben, sollten Sie immer mit den jeweils neuesten Versionen der Azure PowerShell-Cmdlets arbeiten.

Die derzeit installierte Version der Azure-Cmdlets auf dem Desktopcomputer/Laptop kann mit dem Befehl „PS“ überprüft werden:

```powershell
Import-Module Azure
(Get-Module Azure).Version
```

Das Ergebnis wird wie in [dieser][deployment-guide-figure-6] Abbildung gezeigt dargestellt.

![Ergebnis der Überprüfung der Azure PS-Cmdlet-Version][deployment-guide-figure-600]
<a name="figure-6"></a>

Wenn die auf dem Desktopcomputer/Laptop installierte Version der Azure-Cmdlets aktuell ist, sieht der erste Bildschirm als Einleitung des Microsoft-Webplattform-Installers etwas anders aus als in [dieser][deployment-guide-figure-5] Abbildung.

Beachten Sie den roten Kreis in der folgenden [Abbildung][deployment-guide-figure-7].

![Installationsbildschirm für Azure PowerShell-Cmdlets, der angibt, dass die neueste Version der Azure PS-Cmdlets installiert ist][deployment-guide-figure-700]
<a name="figure-7"></a>

Falls der Bildschirm wie [oben][deployment-guide-figure-7] dargestellt aussieht, muss die Installation nicht fortgesetzt werden, da bereits die aktuelle Version der Azure-Cmdlets installiert ist. In diesem Fall können Sie die Installation hier beenden.

### <a name="a-name1ded9453-1330-442a-86ea-e0fd8ae8cab3adeploying-azure-cli"></a><a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Bereitstellen von Azure-CLI
* Wechseln Sie zu <https://azure.microsoft.com/downloads/>.
* Im Abschnitt „Befehlszeilentools“ befindet sich der Unterabschnitt „Azure-Befehlszeilenschnittstelle“. Führen Sie den Installationslink für Ihr Betriebssystem aus.

Überprüfen Sie regelmäßig, ob die Azure-CLI aktualisiert wurde. In der Regel erfolgt eine monatliche Aktualisierung. Die einfachste Möglichkeit zur Überprüfung besteht darin, die oben beschriebenen Installationsschritte durchzuführen.

Die derzeit installierte Version der Azure-CLI auf dem Desktopcomputer/Laptop kann mit folgendem Befehl überprüft werden:

```
azure --version
```

Das Ergebnis wird wie in [dieser][deployment-guide-figure-azure-cli-version] Abbildung gezeigt dargestellt.

![Ergebnis der Überprüfung der Azure-CLI-Version][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="a-name31d9ecd6-b136-4c73-b61e-da4a29bbc9ccajoin-vm-into-on-premises-domain-windows-only"></a><a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Einbinden virtueller Computer in die lokale Domäne – nur Windows
In Fällen, in denen virtuelle SAP-Computer in einem standortübergreifenden Szenario bereitgestellt werden, bei dem lokale AD- und DNS-Dienste nach Azure erweitert werden, sollten die virtuellen Computer in eine lokale Domäne eingebunden werden. Die genauen Einzelschritte zur Einbindung eines virtuellen Computers in einer lokalen Domäne sowie die hierzu erforderliche zusätzliche Software sind kundenspezifisch. In der Regel erfordert der Beitritt zu einer lokalen Domäne die Installation zusätzlicher Software wie eines Schadsoftwareschutzes oder von verschiedenen Agents zur Datensicherung oder Überwachung.

Zudem müssen in Fällen, bei denen der Beitritt zu einer Domäne erzwungene Proxyeinstellungen erfordert, die lokalen Windows-Systemkonten (S-1-5-18) des virtuellen Gastcomputers ebenfalls diese Einstellungen aufweisen. Am einfachsten ist es, die Proxyeinstellungen über eine Domänengruppenrichtlinien für Systeme innerhalb der Domäne zu erzwingen.

### <a name="a-namec7cbb0dc-52a4-49db-8e03-83e7edc2927dadownload-install-and-enable-azure-vm-agent"></a><a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Herunterladen, Installieren und Aktivieren von Azure VM-Agent
Die folgenden Schritte sind erforderlich, wenn ein virtueller Computer für SAP über ein Betriebssystemimage bereitgestellt wird, das nicht generalisiert wurde, also z.B. nicht mit sysprep für Windows vorbereitet wurde. Für virtuelle Computer aus dem Azure Marketplace ist das Installieren des Agents nicht erforderlich. Diese Images enthalten den Azure-Agent bereits.

#### <a name="a-nameb2db5c9a-a076-42c6-9835-16945868e866awindows"></a><a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
* Laden Sie den Azure-VM-Agent herunter:
  * Das Installationspaket für den Azure-VM-Agent kann hier heruntergeladen werden: <https://go.microsoft.com/fwlink/?LinkId=394789>.
  * Speichern Sie das MSI-Paket für den VM-Agent lokal auf dem Laptop oder einem Server.
* Installieren Sie den Azure-VM-Agent:
  * Stellen Sie über Terminaldienste (RDP) eine Verbindung mit dem bereitgestellten virtuellen Azure-Computer her.
  * Öffnen Sie ein Windows Explorer-Fenster auf dem virtuellen Computer und ein Zielverzeichnis für die MSI-Datei des VM-Agents.
  * Verschieben Sie die MSI-Datei für die Installation des Azure-VM-Agents per Drag & Drop vom lokalen Laptop/Server in das Zielverzeichnis des VM-Agents auf dem virtuellen Computer.
  * Doppelklicken Sie anschließend auf dem virtuellen Computer auf die MSI-Datei.
  * Achten Sie bei virtuellen Computern nach dem Beitritt zu einer lokalen Domäne darauf, dass die Internetproxyeinstellungen auch für das lokale Windows-Systemkonto (S-1-5-18) des virtuellen Computers gelten, wie im Kapitel [Konfigurieren von Proxys][deployment-guide-configure-proxy] beschrieben. Der VM-Agent wird in diesem Kontext ausgeführt und muss eine Verbindung mit Azure herstellen können.

#### <a name="a-name6889ff12-eaaf-4f3c-97e1-7c9edc7f7542alinux"></a><a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Installieren Sie den VM-Agent für Linux mit dem folgenden Befehl:

* **SLES**

```
sudo zypper install WALinuxAgent
```
* **RHEL**

```
sudo yum install WALinuxAgent
```

### <a name="a-namebaccae00-6f79-4307-ade4-40292ce4e02daconfigure-proxy"></a><a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Konfigurieren von Proxys
Die Schritte zum Konfigurieren des Proxys unterscheiden sich zwischen Windows und Linux.

#### <a name="windows"></a> Windows
Diese Einstellung müssen auch für den Internetzugang des lokalen Systemkontos gültig sein. Wenn die Proxyeinstellungen nicht von einer Gruppenrichtlinie festgelegt werden, können Sie die Einstellungen wie folgt konfigurieren.

1. Öffnen Sie „gpedit.msc“.
2. Wechseln Sie zu „Computerkonfiguration“ -> „Administrative Vorlagen“ -> „Windows-Komponenten“ -> „Internet Explorer“, und aktivieren Sie „Proxyeinstellungen pro Computer vornehmen (anstelle von pro Benutzer)“.
3. Öffnen Sie die Systemsteuerung, und wechseln Sie zu „Netzwerk und Internet“ -> „Internetoptionen“.
4. Öffnen Sie die Registerkarte „Verbindungen“, und klicken Sie auf „LAN-Einstellungen“.
5. Deaktivieren Sie „Einstellungen automatisch erkennen“.
6. Aktivieren Sie „Proxyserver für das LAN verwenden“, und geben Sie Hostnamen und Port des Proxys ein.

#### <a name="linux"></a> Linux
Konfigurieren Sie zunächst den richtigen Proxy in der Konfigurationsdatei des Microsoft Azure Gast-Agents „/etc/waagent.conf“. Folgende Parameter müssen festgelegt werden:

```
HttpProxy.Host=<proxy host e.g. proxy.corp.local>
HttpProxy.Port=<port of the proxy host e.g. 80>
```

Starten Sie nach dem Ändern der Konfiguration den Agent neu.

```
sudo service waagent restart
```

Die Proxyeinstellungen in „/etc/waagent.conf“ gelten auch für die erforderlichen VM-Erweiterungen. Falls Sie die Azure-Repositorys verwenden möchten, achten Sie darauf, dass der Datenverkehr mit diesen Repositorys nicht über das lokale Intranet geführt wird. Falls benutzerdefinierte Routen zum Aktivieren erzwungener Tunnel erstellt wurden, muss eine Route hinzugefügt werden, die den Datenverkehr an die Repositorys direkt in das Internet umleitet, und nicht über die Standort-zu-Standort-Verbindung.

* **SLES** Zudem müssen Routen für die IP-Adressen in „/etc/regionserverclnt.cfg“ hinzugefügt werden. Der nachfolgende Screenshot zeigt ein Beispiel. 
* **RHEL** Zudem müssen Routen für die IP-Adressen der in „etc/yum.repos.d/rhui-load-balancers“ aufgelisteten Hosts hinzugefügt werden. Der nachfolgende Screenshot zeigt ein Beispiel.

Weitere Informationen zu benutzerdefinierten Routen finden Sie in [diesem Artikel][virtual-networks-udr-overview].

![Tunnelerzwingung][deployment-guide-figure-50]

### <a name="a-named98edcd3-f2a1-49f7-b26a-07448ceb60caaconfigure-azure-enhanced-monitoring-extension-for-sap"></a><a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Konfigurieren der erweiterten Azure-Überwachung für SAP
Nachdem der virtuelle Computer wie im Kapitel [Bereitstellungsszenarios für virtuelle Computer für SAP in Microsoft Azure][deployment-guide-3] beschrieben vorbereitet wurde, wird der Azure-VM-Agent auf dem Computer installiert. Der nächste wichtige Schritt ist das Bereitstellen der Azure-Erweiterung zur verbesserten Überwachung für SAP, die im Azure-Repository für Erweiterungen in den globalen Rechenzentren von Microsoft Azure erhältlich ist. Weitere Informationen finden Sie im Planungs- und Implementierungshandbuch][planning-guide-9.1]. 

Zum Installieren und Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP können Sie Azure PowerShell oder Azure-CLI verwenden. Lesen Sie das Kapitel [Azure PowerShell][deployment-guide-4.5.1], falls Sie die Erweiterung mithilfe eines Windows-Computers auf einem virtuellen Windows- oder Linux-Computer installieren möchten. Lesen Sie das Kapitel [Azure-CLI][deployment-guide-4.5.2], falls Sie die Erweiterung mithilfe eines Linux-Desktopcomputers auf einem virtuellen Linux-Computer installieren möchten.

#### <a name="a-name987cf279-d713-4b4c-8143-6b11589bb9d4aazure-powershell-for-linux-and-windows-vms"></a><a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell für virtuelle Linux- und Windows-Computer
Führen Sie folgende Schritte aus, um die Azure-Erweiterung zur verbesserten Überwachung für SAP zu installieren:

* Achten Sie darauf, die aktuelle Version der Microsoft Azure PowerShell-Cmdlets zu verwenden. Lesen Sie hierzu das Kapitel [Bereitstellen von Azure PowerShell-Cmdlets][deployment-guide-4.1] in diesem Dokument.  
* Führen Sie das folgende PowerShell-Cmdlet aus. Eine Liste verfügbarer Umgebungen erhalten Sie durch Ausführen des Cmdlets Get-AzureRmEnvironment. Falls Sie das öffentliche Azure verwenden möchten, ist die zugehörige Umgebung „AzureCloud“. Wählen Sie für Azure in China „AzureChinaCloud“ aus.

```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

Nachdem die Kontodaten und der virtuelle Azure-Computer angegeben wurden, stellt das Skript die erforderlichen Erweiterungen bereit und aktiviert die benötigten Features. Dies kann einige Minuten dauern.
Weitere Informationen zu „Set-AzureRmVMAEMExtension“ finden Sie in [diesem MSDN-Artikel][msdn-set-azurermvmaemextension].

![Ergebnisbildschirm der erfolgreichen Ausführung des SAP-spezifischen Azure-Cmdlets Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Bei einer erfolgreichen Ausführung von Set-AzureRmVMAEMExtension werden alle erforderlichen Schritte zur Konfiguration der Hostüberwachungsfunktion für SAP durchgeführt. 

Die Ausgabe des Skripts sollte in etwa wie folgt aussehen:

* Bestätigung, dass die Überwachungskonfiguration für die Basis-VHD (mit dem Betriebssystem) und alle zusätzlichen in den virtuellen Computer eingebundenen VHDs konfiguriert wurde.
* Durch die nächsten beiden Meldungen wird die Konfiguration der Speichermetriken für ein bestimmtes Speicherkonto bestätigt. 
* Eine Zeile der Ausgabe zeigt den Zustand der laufenden Aktualisierung der Überwachungskonfiguration an.
* In einer weiteren Zeile wird bestätigt, dass die Konfiguration bereitgestellt oder aktualisiert wurde.
* Die letzte Zeile der Ausgabe enthält Informationen zu der Möglichkeit, die Überwachungskonfiguration zu testen.
* Um zu überprüfen, ob alle Schritte für die verbesserte Azure-Überwachung erfolgreich ausgeführt wurden und ob die Azure-Infrastruktur die notwendigen Daten bereitstellt, fahren Sie mit dem Bereitschaftstest für die Azure-Erweiterung zur verbesserten Überwachung für SAP fort, wie in diesem Dokument im Kapitel [Bereitschaftsüberprüfung für die verbesserte Azure-Überwachung für SAP][deployment-guide-5.1] beschrieben. 
* Warten Sie bis dahin jedoch etwa 15–30 Minuten, bis Azure-Diagnose die entsprechenden Daten erfasst hat.

#### <a name="a-name408f3779-f422-4413-82f8-c57a23b4fc2faazure-cli-for-linux-vms"></a><a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure-CLI für virtuelle Linux-Computer
Führen Sie folgende Schritte aus, um die Azure-Erweiterung zur verbesserten Überwachung für SAP mit der Azure-Befehlszeilenschnittstelle zu installieren:

1. Installieren Sie die Azure-Befehlszeilenschnittstelle wie in [diesem][azure-cli] Artikel beschrieben.
2. Melden Sie sich bei Ihrem Azure-Konto an.
   
    ```
    azure login
    ```
3. Wechseln Sie in den Azure Resource Manager-Modus.
   
    ```
    azure config mode arm
    ```
4. Aktivieren Sie die erweiterte Azure-Überwachung.
   
    ```
    azure vm enable-aem <resource-group-name> <vm-name>
    ```  
5. Stellen Sie sicher, dass die erweiterte Azure-Überwachung auf dem virtuellen Azure Linux-Computer aktiv ist. Überprüfen Sie, ob die Datei „/var/lib/AzureEnhancedMonitor/PerfCounters“ vorhanden ist. Wenn sie vorhanden ist, zeigen Sie Informationen, die von AEM gesammelt werden, an mit:
   
    ```
    cat /var/lib/AzureEnhancedMonitor/PerfCounters
    ```
    Anschließend erhalten Sie eine Ausgabe wie folgt:
   
    ```
    2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
    2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
    …
    …
    ```

## <a name="a-name564adb4f-5c95-4041-9616-6635e83a810bachecks-and-troubleshooting-for-end-to-end-monitoring-setup-for-sap-on-azure"></a><a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Überprüfungen und Problembehandlung für die Einrichtung der End-to-End-Überwachung für SAP in Azure
Nachdem Sie den virtuellen Azure-Computer bereitgestellt und die entsprechende Azure-Überwachungsinfrastruktur eingerichtet haben, überprüfen Sie, ob alle Komponenten der verbesserten Azure-Überwachung ordnungsgemäß funktionieren. 

Führen Sie hierzu den Bereitschaftstest für die Azure-Erweiterung zur verbesserten Überwachung für SAP aus, wie im Kapitel [Bereitschaftsüberprüfung für die verbesserte Azure-Überwachung für SAP][deployment-guide-5.1] beschrieben. Wenn das Ergebnis dieser Überprüfung positiv ist und alle relevanten Leistungsindikatoren vorliegen, wurde die Azure-Überwachung erfolgreich eingerichtet. Fahren Sie in diesem Fall mit der Installation des SAP-Host-Agents fort, wie in den SAP-Hinweisen im Kapitel [SAP-Ressourcen][deployment-guide-2.2] dieses Dokuments beschrieben. Wenn im Ergebnis des Bereitschaftstests fehlende Leistungsindikatoren angegeben werden, fahren Sie mit dem Integritätstest für die Azure-Überwachungsinfrastruktur fort, wie im Kapitel [Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur][deployment-guide-5.2] beschrieben. Lesen Sie im Falle eines Problems mit der Azure-Überwachungskonfiguration das Kapitel [Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP][deployment-guide-5.3], um weitere Hilfestellung bei der Problembehandlung zu erhalten.

### <a name="a-namebb61ce92-8c5c-461f-8c53-39f5e5ed91f2areadiness-check-for-azure-enhanced-monitoring-for-sap"></a><a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Bereitschaftsüberprüfung für die erweiterte Azure-Überwachung für SAP
Mit dieser Überprüfung stellen Sie sicher, dass die in der SAP-Anwendung angezeigten Metriken von der zugrunde liegenden Azure-Überwachungsinfrastruktur vollständig bereitgestellt werden. 

#### <a name="execute-the-readiness-check-on-a-windows-vm"></a>Ausführen des Bereitschaftstests auf einem virtuellen Windows-Computer
Zum Ausführen des Bereitschaftstests melden Sie sich auf dem virtuellen Computer an (ein Administratorkonto ist nicht erforderlich) und führen die folgenden Schritte aus:

* Öffnen Sie die Windows-Eingabeaufforderung, und wechseln Sie in den Installationsordner der Azure-Überwachungserweiterung für SAP unter „C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\\`<version`>\drop“.

Der im obigen Pfad zur Überwachungserweiterung enthaltene Versionsteil kann abweichen. Sollten mehrere Ordner der Überwachungserweiterungsversion im Installationsordner vorhanden sein, überprüfen Sie die Konfiguration des Windows-Diensts AzureEnhancedMonitoring, und wechseln Sie zu dem als „Pfad zur ausführbaren Datei“ aufgeführten Ordner.

![Eigenschaften des Diensts, der die Azure-Erweiterung zur verbesserten Überwachung für SAP ausführt][deployment-guide-figure-1000]

* Führen Sie im Befehlsfenster „azperflib.exe“ ohne Parameter aus.

> [!NOTE]
> Das Programm „azperflib.exe“ wird in einer Schleife ausgeführt und aktualisiert die erfassten Leistungsindikatoren alle 60 Sekunden. Schließen Sie das Befehlsfenster, um die Schleife zu beenden.
> 
> 

Wenn die Azure-Erweiterung zur verbesserten Überwachung nicht installiert ist oder der Dienst AzureEnhancedMonitoring nicht ausgeführt wird, wurde die Erweiterung nicht ordnungsgemäß konfiguriert. Befolgen Sie in diesem Fall die Hinweise im Kapitel [Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP][deployment-guide-5.3] mit einer ausführlichen Anleitung zur erneuten Bereitstellung der Erweiterung.

##### <a name="check-the-output-of-azperflibexe"></a>Überprüfen der Ausgabe von „azperflib.exe“
Die Ausgabe von „azperflib.exe“ enthält alle ausgefüllten Azure-Leistungsindikatoren für SAP. Am Ende der Liste der gesammelten Indikatoren finden Sie eine Übersicht und einen Integritätsindikator, der den Zustand der Azure-Überwachung anzeigt. 

![Ausgabe der Integritätsprüfung durch Ausführen von „azperflib.exe“, wenn keine Probleme bestehen][deployment-guide-figure-1100]
<a name="figure-11"></a>

Prüfen Sie das Ergebnis unter „Counters total“ (Indikatoren gesamt). Es enthält die als „leer“ und für die Integritätsprüfung gemeldeten Indikatoren, wie in der [obigen Abbildung][deployment-guide-figure-11] zu sehen.

Sie können die Ergebniswerte wie folgt interpretieren:

| Ergebniswerte von „azperflib.exe“ | Bereitschaftsstatus der Azure-Überwachung |
| --- | --- |
| **Indikatoren gesamt: leer** |Die folgenden beiden Azure Storage-Indikatoren dürfen leer sein:  <ul><li>Storage Read Op Latency Server msec</li><li>Storage Read Op Latency E2E msec</li></ul>Alle anderen Indikatoren müssen Werte enthalten. |
| **Integritätsprüfung** |Nur in Ordnung bei Rückgabestatus „OK“ |

Falls nicht beide Rückgabewerte von „azperflib.exe“ zeigen, dass die aufgefüllten Indikatoren korrekt zurückgegeben wurden, befolgen Sie die Anleitung der Integritätsprüfung für die Infrastrukturkonfiguration der Azure-Überwachung im folgenden Kapitel: [Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur][deployment-guide-5.2].

#### <a name="execute-the-readiness-check-on-a-linux-vm"></a>Ausführen des Bereitschaftstests auf einem virtuellen Linux-Computer
Zum Ausführen des Bereitschaftstests stellen Sie über SSH eine Verbindung mit dem virtuellen Azure-Computer her, und führen Sie die folgenden Schritte aus:

* Überprüfen Sie die Ausgabe der Azure-Erweiterung zur verbesserten Überwachung:
  * more /var/lib/AzureEnhancedMonitor/PerfCounters
    * sollte eine Liste der Leistungsindikatoren erstellen. Die Datei sollte nicht leer sein.
  * cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error
    * Es sollte nur eine einzelne Zeile ohne Fehler zurückgegeben werden. Beispiel: 3;config;Error;;0;0;**none**;0;1456416792;tst-servercs;
  * more /var/lib/AzureEnhancedMonitor/LatestErrorRecord
    * sollte leer sein oder nicht vorhanden
* Wenn die erste Prüfung oben nicht erfolgreich ist, führen Sie folgende zusätzliche Prüfungen durch:
  * Achten Sie darauf, dass waagent installiert und gestartet ist.
    * sudo ls -al /var/lib/waagent/
      * sollte den Inhalt des Verzeichnisses „waagent“ auflisten
    * ps -ax | grep waagent
      * sollte einen Eintrag auflisten, z.B. „python /usr/sbin/waagent -daemon“
  * Achten Sie darauf, dass die Linux-Diagnoseerweiterung installiert ist und gestartet wurde.
    * sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-*'
      * sollte den Inhalt des Ordners der Linux-Diagnoseerweiterung aufführen
    * ps -ax | grep diagnostic
      * sollte einen Eintrag auflisten, z.B. „python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py -daemon“
  * Achten Sie darauf, dass die Azure-Erweiterung zur verbesserten Überwachung installiert ist und gestartet wurde.
    * sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'
      * sollte den Inhalt des Ordners der Azure-Erweiterung zur verbesserten Überwachung auflisten
    * ps -ax | grep AzureEnhanced
      * sollte einen Eintrag auflisten, z.B. „python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon“
* Installieren Sie den SAP-Host-Agent gemäß dem SAP-Hinweis [1031096] , und überprüfen Sie die Ausgabe von saposcol.
  * Führen Sie „/usr/sap/hostctrl/exe/saposcol -d“ aus.
  * Führen Sie „dump ccm“ aus.
  * Überprüfen Sie, ob die Metrik „Virtualization_Configuration\Enhanced Monitoring Access“ TRUE zurückliefert.
* Wenn bereits ein SAP NetWeaver ABAP-Anwendungsserver installiert ist, öffnen Sie die Transaktion ST06, und überprüfen Sie, ob die verbesserte Überwachung aktiviert ist.

Falls bei mindestens einer der oben angegebenen Überprüfungen ein Fehler auftritt, finden Sie im Kapitel [Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP][deployment-guide-5.3] eine ausführliche Anleitung zur erneuten Bereitstellung der Erweiterung.

### <a name="a-namee2d592ff-b4ea-4a53-a91a-e5521edb6cd1ahealth-check-for-azure-monitoring-infrastructure-configuration"></a><a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur
Falls der im Kapitel [Bereitschaftsüberprüfung der verbesserten Azure-Überwachung für SAP][deployment-guide-5.1] beschriebene Test meldet, dass einige Überwachungsdaten nicht korrekt bereitgestellt wurden, führen Sie das Cmdlet „Test-AzureRmVMAEMExtension“ aus. Damit überprüfen Sie, ob die aktuelle Konfiguration der Azure-Überwachungsinfrastruktur und der Überwachungserweiterung für SAP korrekt ist.

Führen Sie die folgenden Schritte aus, um die Überwachungskonfiguration zu testen:

* Achten Sie darauf, dass die neueste Version des Microsoft Azure PowerShell-Cmdlets installiert ist, wie in diesem Dokument im Kapitel [Bereitstellen von Azure PowerShell-Cmdlets][deployment-guide-4.1] beschrieben.
* Führen Sie das folgende PowerShell-Cmdlet aus. Eine Liste verfügbarer Umgebungen erhalten Sie durch Ausführen des Cmdlets Get-AzureRmEnvironment. Falls Sie das öffentliche Azure verwenden möchten, ist die zugehörige Umgebung „AzureCloud“. Wählen Sie für Azure in China „AzureChinaCloud“ aus.

```powershell
$env = Get-AzureRmEnvironment -Name <name of the environment>
Login-AzureRmAccount -Environment $env
Set-AzureRmContext -SubscriptionName <subscription name>
Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

* Nachdem die Kontodaten und der virtuelle Azure-Computer angegeben wurden, testet das Skript die Konfiguration des ausgewählten virtuellen Computers.

![Eingabebildschirm des SAP-spezifischen Azure-Cmdlets Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

Nachdem die Kontodaten und der virtuelle Azure-Computer eingegeben wurden, testet das Skript die Konfiguration des ausgewählten virtuellen Computers.

![Ausgabe eines erfolgreichen Tests der Azure-Überwachungsinfrastruktur für SAP][deployment-guide-figure-1300]

Achten Sie darauf, dass jeder Test mit „OK“ markiert ist. Sollten einige Tests nicht mit „OK“ gekennzeichnet sein, führen Sie das update-Cmdlet aus, wie in diesem Dokument im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben. Führen Sie nach weiteren 15 Minuten erneut die in den Kapiteln [Bereitschaftsüberprüfung für die verbesserte Azure-Überwachung für SAP][deployment-guide-5.1] und [Integritätsprüfung der Konfiguration für die Azure-Überwachungsinfrastruktur][deployment-guide-5.2] beschriebenen Tests durch. Falls die Tests weiterhin für einige oder alle Indikatoren Probleme melden, fahren Sie mit dem Kapitel [Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP][deployment-guide-5.3] fort.

### <a name="a-namefe25a7da-4e4e-4388-8907-8abc2d33cfd8afurther-troubleshooting-of-azure-monitoring-infrastructure-for-sap"></a><a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Weitere Problembehandlung für die Azure-Überwachungsinfrastruktur für SAP
#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![ Windows][Logo_Windows]  Es werden keinerlei Azure-Leistungsindikatoren angezeigt.
Die Auflistung der Leistungsmetriken in Azure erfolgt durch den Windows-Dienst AzureEnhancedMonitoring. Wenn der Dienst nicht ordnungsgemäß installiert wurde, oder wenn er im virtuellen Computer nicht ausgeführt wird, können keine Leistungsmetriken erfasst werden.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Das Installationsverzeichnis der Azure-Erweiterung zur verbesserten Überwachung ist leer.
###### <a name="issue"></a>Problem
Das Installationsverzeichnis „C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`>\drop“ ist leer.

###### <a name="solution"></a>Lösung
Die Erweiterung ist nicht installiert. Überprüfen Sie, ob (wie zuvor beschrieben) ein Proxyproblem vorliegt. Möglicherweise müssen Sie einen Neustart des Computers durchführen und/oder das Konfigurationsskript Set-AzureRmVMAEMExtension erneut ausführen.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Der Dienst für die verbesserte Azure-Überwachung ist nicht vorhanden.
###### <a name="issue"></a>Problem
Der Windows-Dienst AzureEnhancedMonitoring ist nicht vorhanden. Azperflib.exe: Die Ausgabe von „azperflib.exe“ löst den in der [folgenden Abbildung][deployment-guide-figure-14] gezeigten Fehler aus.

![Ausführung von „azperflib.exe“ mit der Angabe, dass die Azure-Erweiterung zur verbesserten Überwachung für SAP nicht ausgeführt wird][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Lösung
Wenn der Dienst wie in der [obigen Abbildung][deployment-guide-figure-14] gezeigt nicht vorhanden ist, wurde die Azure-Überwachungserweiterung für SAP nicht richtig installiert. Stellen Sie die Erweiterung gemäß den für Ihr Szenario geltenden Schritten im Kapitel [Bereitstellungsszenarios für virtuelle Computer für SAP in Microsoft Azure][deployment-guide-3] erneut bereit. 

Prüfen Sie nach der erneuten Bereitstellung der Erweiterung noch einmal, ob die Azure-Leistungsindikatoren nach 1 Stunde im virtuellen Azure-Computer bereitgestellt wurden.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Der Dienst für die verbesserte Azure-Überwachung ist vorhanden, startet jedoch nicht.
###### <a name="issue"></a>Problem
Der Windows-Dienst AzureEnhancedMonitoring ist vorhanden und aktiviert, startet jedoch nicht. Überprüfen Sie das Anwendungsereignisprotokoll für weitere Informationen.

###### <a name="solution"></a>Lösung
Die Konfiguration ist fehlerhaft. Aktivieren Sie die Überwachungserweiterung für den virtuellen Computer erneut, wie in diesem Dokument im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben.

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows]  Einige Azure-Leistungsindikatoren fehlen.
Die Sammlung der Leistungsmetriken in Azure erfolgt durch den Windows-Dienst AzureEnhancedMonitoring, der seine Daten aus mehreren Quellen bezieht. Einige Konfigurationsdaten werden lokal gesammelt, Leistungsmetriken werden aus Azure-Diagnose eingelesen und Storage-Indikatoren werden aus der Protokollierung auf Speicherabonnementebene bezogen.

Sollte die Problembehandlung anhand des SAP-Hinweises [1999351] nicht hilfreich sein, führen Sie erneut das Konfigurationsskript Set-AzureRmVMAEMExtension aus. Möglicherweise ist eine Wartezeit von 1 Stunde erforderlich, da Speicheranalyse- und Diagnoseindikatoren nicht sofort nach der Aktivierung erstellt werden. Falls das Problem weiterhin besteht, öffnen Sie eine SAP-Kundendienstmeldung für die Komponente BC-OP-NT-AZR.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![ Linux][Logo_Linux]  Es werden keinerlei Azure-Leistungsindikatoren angezeigt.
Das Erfassen der Leistungsmetriken in Azure erfolgt durch einen Daemon. Wenn der Daemon nicht ausgeführt wird, können keine Leistungsmetriken erfasst werden.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>Das Installationsverzeichnis der Azure-Erweiterung zur verbesserten Überwachung ist leer.
###### <a name="issue"></a>Problem
Das Verzeichnis „/var/lib/waagent/“ enthält kein Unterverzeichnis für die Azure-Erweiterung zur verbesserten Überwachung.

###### <a name="solution"></a>Lösung
Die Erweiterung ist nicht installiert. Überprüfen Sie, ob (wie zuvor beschrieben) ein Proxyproblem vorliegt. Möglicherweise müssen Sie einen Neustart des Computers durchführen und/oder das Konfigurationsskript Set-AzureRmVMAEMExtension erneut ausführen.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![ Linux][Logo_Linux]  Einige Azure-Leistungsindikatoren fehlen.
Die Sammlung der Leistungsmetriken in Azure erfolgt durch einen Daemon, der seine Daten aus mehreren Quellen bezieht. Einige Konfigurationsdaten werden lokal gesammelt, Leistungsmetriken werden aus Azure-Diagnose eingelesen und Storage-Indikatoren werden aus der Protokollierung auf Speicherabonnementebene bezogen.

Eine vollständige und aktuelle Liste bekannter Probleme finden Sie im SAP-Hinweis [1999351] , der auch zusätzliche Informationen zur Problembehandlung für die verbesserte Azure-Überwachung für SAP enthält.

Falls die Problembehandlung gemäß SAP-Hinweis [1999351] nicht hilfreich war, führen Sie erneut das Konfigurationsskript „Set-AzureRmVMAEMExtension“ aus, wie im Kapitel [Konfigurieren der Azure-Erweiterung zur verbesserten Überwachung für SAP][deployment-guide-4.5] beschrieben. Möglicherweise ist eine Wartezeit von 1 Stunde erforderlich, da Speicheranalyse- und Diagnoseindikatoren nicht sofort nach der Aktivierung erstellt werden. Falls das Problem weiterhin besteht, öffnen Sie eine SAP-Kundendienstmeldung für die Komponente BC-OP-NT-AZR für virtuelle Windows-Computer oder BC-OP-LNX-AZR für virtuelle Linux-Computer.




<!--HONumber=Nov16_HO3-->


