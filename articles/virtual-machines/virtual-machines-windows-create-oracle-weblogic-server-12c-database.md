<properties
	pageTitle="Virtueller Computer mit Oracle WebLogic Server und Datenbank | Microsoft Azure"
	description="Erstellen Sie ein Azure-Image mit Oracle WebLogic Server 12c und Oracle Database 12c unter Windows Server 2012 mit dem Ressourcen-Manager-Bereitstellungsmodell."
	services="virtual-machines-windows"
	authors="rickstercdn"
	manager="timlt"
	documentationCenter=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows"
	ms.workload="infrastructure-services"
	ms.date="05/17/2016"
	ms.author="rickstercdn" />

#Erstellen eines virtuellen Oracle WebLogic Server 12c- und Oracle Database 12c-Computers in Azure

[AZURE.INCLUDE [virtual-machines-common-oracle-support](../../includes/virtual-machines-common-oracle-support.md)]

Dieser Artikel zeigt, wie Sie eine Oracle WebLogic Server 12c-Instanz und eine Oracle Database 12c-Datenbank auf einem Image erstellen, das Sie zuvor erstellt haben und auf dem Oracle-Software unter Windows Server 2012 in Azure installiert ist.

##So erstellen Sie die auf dem virtuellen Computer gehostete Datenbank

Folgen Sie den Anweisungen in [Erstellen eines virtuellen Oracle Database 12c-Computers in Azure](virtual-machines-windows-classic-create-oracle-database.md) ab dem Abschnitt **So erstellen Sie die Datenbank mithilfe des virtuellen Oracle Database 12c-Computers in Azure**.

##So konfigurieren Sie den auf dem virtuellen Computer gehosteten Oracle WebLogic Server 12c
Folgen Sie den Anweisungen in [Erstellen eines virtuellen Oracle WebLogic Server 12c-Computers in Azure](virtual-machines-windows-create-oracle-weblogic-server-12c.md) ab dem Abschnitt **So konfigurieren Sie den virtuellen Oracle WebLogic Server 12c-Computer in Azure**.

##Zusätzliche Ressourcen
[Verschiedene Überlegungen zu Images für virtuelle Oracle-Computer](virtual-machines-windows-classic-oracle-considerations.md)

[Liste der Images virtueller Oracle-Computer](virtual-machines-linux-classic-oracle-images.md)

[Herstellen einer Verbindung mit der Oracle-Datenbank in einer Java-Anwendung](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

[Verwendung von Oracle WebLogic Server 12c mit Linux in Microsoft Azure](http://www.oracle.com/technetwork/middleware/weblogic/learnmore/oracle-weblogic-on-azure-wp-2020930.pdf)

[Oracle Database 2 Day DBA 12c Release 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)

<!---HONumber=AcomDC_0601_2016-->