---
title: ".NET Core-Tutorial 1 für virtuelle Azure-Computer unter Linux | Microsoft-Dokumentation"
description: ".NET Core-Tutorial für virtuelle Azure-Computer"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b3652e86-0c44-4ac9-8cd1-27abdeaea4d4
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2016
ms.author: nepeters
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: d9d65d8fb5fd7b1ff226e96fe33f8b0757d7a12e
ms.lasthandoff: 03/03/2017


---
# <a name="automating-application-deployments-to-linux-virtual-machines"></a>Automatisieren von Anwendungsbereitstellungen auf virtuellen Linux-Computern 

Diese vierteilige Reihe enthält Details zur Bereitstellung und Konfiguration von Azure-Ressourcen und -Anwendungen mit Azure Resource Manager-Vorlagen. In dieser Reihe wird eine Beispielvorlage bereitgestellt und die Bereitstellungsvorlage untersucht. Ziel dieser Reihe ist die Erläuterung der Beziehung zwischen Azure-Ressourcen und die Durchführung von praktischen Übungen zur Bereitstellung von vollständig integrierten Azure Resource Manager-Vorlagen. In diesem Dokument wird vorausgesetzt, dass Sie über Azure Resource Manager-Grundkenntnisse verfügen. Machen Sie sich vor Beginn dieses Tutorials also mit den grundlegenden Konzepten von Azure Resource Manager vertraut. 

## <a name="music-store-application"></a>Music Store-Anwendung
Das in dieser Reihe verwendete Beispiel ist eine .Net Core-Anwendung, mit der eine Music Store-Einkaufsoberfläche simuliert wird. Diese Anwendung kann auf einem virtuellen Linux- oder Windows-System bereitgestellt werden. Für beide Fälle wurden Beispielbereitstellungen erstellt. Die Anwendung umfasst eine Webanwendung und eine SQL-Datenbank. Stellen Sie vor dem Lesen der Artikel dieser Reihe die Anwendung bereit, indem Sie die Schaltfläche für die Bereitstellung auf dieser Seite verwenden. Nach der vollständigen Bereitstellung sieht die Anwendung bzw. die Azure-Architektur wie im folgenden Diagramm aus. 

Die Music Store-Resource Manager-Vorlage finden Sie unter [Music Store Linux Template (Music Store-Linux-Vorlage)](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Music Store-Anwendung](./media/virtual-machines-linux-dotnet-core/music-store.png)

Jede dieser Komponenten, einschließlich des JSON-Codes der zugehörigen Vorlage, wird in den folgenden vier Artikeln beschrieben.

* [**Anwendungsarchitektur**](virtual-machines-linux-dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Anwendungskomponenten wie Websites und Datenbanken müssen auf Azure-Computerressourcen gehostet werden, z.B. virtuellen Computern und Azure SQL-Datenbanken. In diesem Dokument werden die Zuordnung der Computeanforderungen zu den Azure-Ressourcen und die Bereitstellung dieser Ressourcen über eine Azure Resource Manager-Vorlage Schritt für Schritt beschrieben. 
* [**Zugriff und Sicherheit**](virtual-machines-linux-dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Beim Hosten von Anwendungen in Azure muss berücksichtigt werden, wie auf die Anwendung zugegriffen wird und wie unterschiedliche Anwendungskomponenten aufeinander zugreifen. In diesem Dokument erhalten Sie ausführliche Informationen zum Bereitstellen und Schützen des Internetzugriffs auf eine Anwendung und des Zugriffs zwischen Anwendungskomponenten.
* [**Verfügbarkeit und Skalierung**](virtual-machines-linux-dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Bei der Verfügbarkeit und Skalierung geht es um die Fähigkeit der Anwendungen, auch bei Ausfällen der Infrastruktur weiter ausgeführt zu werden, sowie um die Möglichkeit, Computeressourcen je nach Anwendungsbedarf zu skalieren. Dieses Dokument enthält ausführliche Details zu den Komponenten, die zum Bereitstellen einer hoch verfügbaren Anwendung mit Lastenausgleich benötigt werden.
* [**Anwendungsbereitstellung**](virtual-machines-linux-dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Beim Bereitstellen von Anwendungen auf virtuellen Azure-Computern muss das Verfahren, mit dem die Binärdateien der Anwendung auf dem virtuellen Computer installiert werden, berücksichtigt werden. Dieses Dokument enthält ausführliche Informationen zur Automatisierung der Anwendungsinstallation mit benutzerdefinierten Skripterweiterungen für virtuelle Azure-Computer.

Das Ziel bei der Entwicklung von Azure Resource Manager-Vorlagen ist die Automatisierung der Bereitstellung einer Azure-Infrastruktur und die Installation und Konfiguration von Anwendungen, die in dieser Azure-Infrastruktur gehostet werden. Wenn Sie diese Artikel durcharbeiten, lernen Sie diese Vorgehensweise kennen.

## <a name="deploy-the-music-store-application"></a>Bereitstellen der Music Store-Anwendung
Sie können die Music Store-Anwendung mit dieser Schaltfläche bereitstellen.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Für die Azure Resource Manager-Vorlage sind die folgenden Parameterwerte erforderlich:

| Parametername | Beschreibung |
| --- | --- |
| SSHKEYDATA |SSH-Schlüsseldaten, die zum Schützen des Zugriffs auf den virtuellen Computer verwendet werden. Weitere Informationen zum Erstellen eines SSH-Schlüsselpaars finden Sie unter [Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). |
| ADMINUSERNAME |Der Administratorbenutzername, der auf dem virtuellen Computer und für die Azure SQL-Datenbank verwendet wird. |
| SQLADMINPASSWORD |Kennwort, das für die Azure SQL-Datenbank verwendet wird. |
| NUMBEROFINSTANCES |Die Anzahl zu erstellender virtueller Computer. Auf allen diesen virtuellen Computern wird die Music Store-Webanwendung gehostet, und für den gesamten Datenverkehr wird darüber der Lastenausgleich durchgeführt. |
| PUBLICIPADDRESSDNSNAME |Global eindeutiger DNS-Name, der der öffentlichen IP-Adresse zugeordnet ist. |

Navigieren Sie nach Abschluss der Vorlagenbereitstellung mit einem Internetbrowser zur öffentlichen IP-Adresse. Die .Net Core-Music Store-Website wird angezeigt.

## <a name="next-steps"></a>Nächste Schritte
<hr>

[Schritt 1: Anwendungsarchitektur mit Azure Resource Manager-Vorlagen](virtual-machines-linux-dotnet-core-2-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Schritt 2: Zugriff und Sicherheit in Azure Resource Manager-Vorlagen](virtual-machines-linux-dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Schritt 3: Verfügbarkeit und Skalierung in Azure Resource Manager-Vorlagen](virtual-machines-linux-dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Schritt 4: Anwendungsbereitstellung mit Azure Resource Manager-Vorlagen](virtual-machines-linux-dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


