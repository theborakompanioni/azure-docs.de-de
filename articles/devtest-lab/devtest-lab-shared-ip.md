---
title: Grundlegendes zu freigegebenen IP-Adressen in Azure DevTest Labs | Microsoft-Dokumentation
description: "Erfahren Sie, wie Azure DevTest Labs freigegebene IP-Adressen verwendet, um die Anzahl von öffentlichen IP-Adressen zu minimieren, die für den Zugriff auf die virtuellen Computer Ihres Labs erforderlich sind."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: b4ae6b16880e4a5e53c8f94dc814e25254c20d73
ms.lasthandoff: 03/10/2017

---

# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Grundlegendes zu freigegebenen IP-Adressen in Azure DevTest Labs

Azure DevTest Labs verwendet freigegebene IP-Adressen, um die Anzahl von öffentlichen IP-Adressen zu minimieren, die für den Zugriff auf die einzelnen virtuellen Computer Ihres Labs erforderlich sind.  Dieser Artikel beschreibt die Funktionsweise von freigegebenen IP-Adressen und die zugehörigen Konfigurationsoptionen.

## <a name="shared-ip-setting"></a>Einstellung für freigegebene IP-Adressen

Wenn Sie ein Lab erstellen, befindet dieses sich in einem Subnetz eines virtuellen Netzwerks.  Beim Erstellen dieses Subnetzes ist die Einstellung **Freigegeben öffentliche IP-Adresse aktivieren** standardmäßig auf *Ja* festgelegt.  Diese Konfiguration erstellt eine einzige öffentliche IP-Adresse für das gesamte Subnetz.  Sie können diese Einstellung nach dem Erstellen eines Labs deaktivieren, indem Sie [die Schritte in diesem Artikel ausführen](devtest-lab-configure-vnet.md).

![Neues Labsubnetz](media/devtest-lab-shared-ip/lab-subnet.png)

Alle in diesem Lab erstellten virtuellen Computer verwenden standardmäßig eine freigegebene IP-Adresse.  Beim Erstellen des virtuellen Computers können Sie diese Einstellung auf dem Blatt **Erweiterte Einstellungen** unter **IP-Adresskonfiguration** anzeigen.

![Neuer virtueller Computer](media/devtest-lab-shared-ip/new-vm.png)

Wenn ein virtueller Computer mit einer aktivierten freigegebenen IP-Adresse zum Subnetz hinzugefügt wird, wird der öffentlichen IP-Adresse ein TCP-Port zugewiesen, der Datenverkehr an den RDP-Port des virtuellen Computers weiterleitet.  

## <a name="using-the-shared-ip"></a>Verwenden der freigegebenen IP-Adresse

Zum Herstellen einer Verbindung mit Remotedesktop auf dem virtuellen Computer eines RDP-Clients verwenden Sie die IP-Adresse oder den vollqualifizierten Domänennamen, gefolgt von einem Doppelpunkt und dem Port.  In der folgenden Abbildung lautet die RDP-Adresse für die Verbindung mit dem virtuellen Computer `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.  Alternativ dazu wählen Sie im Azure-Portal die Schaltfläche **Verbinden**, um eine vorkonfigurierte RDP-Datei herunterzuladen.

![Beispiel für einen virtuellen Computer](media/devtest-lab-shared-ip/vm-info.png)

## <a name="next-steps"></a>Nächste Schritte

* [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Konfigurieren eines virtuellen Netzwerks in Azure DevTest Labs](devtest-lab-configure-vnet.md)






