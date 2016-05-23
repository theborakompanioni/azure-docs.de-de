<properties
   pageTitle="Verwenden von SAP auf virtuellen Windows-Computern | Microsoft Azure"
   description="Hier erfahren Sie mehr über die Verwendung von SAP auf virtuellen Windows-Computern in Microsoft Azure."
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="juergent"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="05/04/2016"
   ms.author="sedusch"/>

# Verwenden von SAP auf virtuellen Windows-Computern in Azure

Cloud Computing ist ein häufig verwendeter Begriff, der in der IT-Branche mehr und mehr an Bedeutung gewinnt – in kleinen Unternehmen ebenso wie in großen und multinationalen Konzernen. Microsoft Azure ist die Plattform für Clouddienste von Microsoft, die ein umfangreiches Spektrum an neuen Möglichkeiten bietet. Nun können Kunden Anwendungen schnell als Clouddienste bereitstellen bzw. die Bereitstellung aufheben und sind nicht mehr durch technische oder budgetbezogene Aspekte eingeschränkt. Anstatt Zeit und Geld in die Hardwareinfrastruktur zu investieren, können sich Unternehmen auf die Anwendung, Geschäftsprozesse und ihre Vorteile für Kunden und Benutzer konzentrieren.

Mit den virtuellen Computern in Microsoft Azure bietet Microsoft eine umfassende IaaS-Plattform (Infrastructure-as-a-Service). Auf SAP NetWeaver basierende Anwendungen werden auf Azure Virtual Machines (IaaS) unterstützt. Die folgenden Whitepaper beschreiben, wie auf SAP NetWeaver basierende Anwendungen auf virtuellen Windows-Computern in Azure geplant und implementiert werden. Sie können auf SAP NetWeaver basierende Anwendungen auch auf [virtuellen Linux-Computern](virtual-machines-linux-classic-sap-get-started.md) implementieren.

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## SAP NetWeaver in Azure – HA

Titel: SAP NetWeaver in Azure – Clustering von SAP ASCS/SCS-Instanzen mithilfe von Windows Server-Failoverclustern in Azure mit SIOS DataKeeper

Zusammenfassung: In diesem Dokument wird beschrieben, wie SIOS DataKeeper verwendet wird, um eine hoch verfügbare SAP ASCS/SCS-Konfiguration in Azure einzurichten. SAP schützt die Single Point of Failure-Komponenten wie SAP ASCS/SCS oder Enqueue Replication Services mit Windows Server-Failoverclusterkonfigurationen, die freigegebene Datenträger erfordern. Diese SAP-Komponenten sind für die Funktionalität eines SAP-Systems unverzichtbar. Aus diesem Grund müssen Funktionen für hohe Verfügbarkeit eingerichtet sein, um sicherzustellen, dass diese Komponenten einen Ausfall eines Servers oder eines virtuellen Computers tolerieren – z. B. mit Windows-Clusterkonfigurationen für Bare-Metal- und Hyper-V-Umgebungen. Seit August 2015 kann Azure selbst keine freigegebenen Datenträger bereitstellen, die für die Windows-basierten hoch verfügbaren Konfigurationen für diese kritischen SAP-Komponenten erforderlich wären. Jedoch können mit dem Produkt DataKeeper von SIOS auf der Azure IaaS-Plattform Windows Server-Failoverclusterkonfigurationen erstellt werden, wie sie für SAP ASCS/SCS erforderlich sind. Dieses Whitepaper beschreibt die einzelnen Schritte zur Installation einer Windows Server-Failoverclusterkonfiguration mit freigegebenen Datenträgern, die von SIOS Datakeeper in Azure bereitgestellt werden. Im Whitepaper werden Konfigurationsdetails in Azure, Windows und SAP erläutert, die für eine optimale Funktionsweise der hoch verfügbaren Konfiguration sorgen. Das Dokument ergänzt die SAP-Installationsdokumentation und die SAP-Hinweise, die die primäre Ressource für Installationen und Bereitstellungen von SAP-Software auf verschiedenen Plattformen darstellen.

Aktualisierung: August 2015

[Dieses Handbuch jetzt herunterladen](http://go.microsoft.com/fwlink/?LinkId=613056)

<!---HONumber=AcomDC_0511_2016-->