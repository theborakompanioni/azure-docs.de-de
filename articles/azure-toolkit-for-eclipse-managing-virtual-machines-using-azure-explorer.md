---
title: "Verwalten virtueller Computer mit dem Azure-Explorer für Eclipse | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre virtuellen Azure-Computer mit dem Azure-Explorer für Eclipse verwalten."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: f8631c5fe566762862f64b47843e154b68bb8031
ms.lasthandoff: 04/22/2017


---

# <a name="managing-virtual-machines-using-the-azure-explorer-for-eclipse"></a>Verwalten virtueller Computer mit dem Azure-Explorer für Eclipse

Der Azure-Explorer, der zum Azure-Toolkit für Eclipse gehört, bietet Java-Entwicklern eine einfach zu bedienende Lösung zum Verwalten von virtuellen Computern in ihrem Azure-Konto innerhalb der IDE von Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="creating-a-virtual-machine-in-eclipse"></a>Erstellen eines virtuellen Computers in Eclipse

Die folgenden Schritte begleiten Sie durch die Schritte zum Erstellen eines virtuellen Computers mit dem Azure-Explorer.

1. Melden Sie sich beim Azure-Konto gemäß den Anweisungen im Artikel [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse] an.

1. Erweitern Sie in der Ansicht des **Azure-Explorers** den Knoten **Azure**. Klicken Sie mit der rechten Maustaste auf **Virtuelle Computer**, und klicken Sie dann auf **VM erstellen**.
   ![Menü zum Erstellen eines virtuellen Computers][CR01]

1. Wenn der Assistent zum **Erstellen eines neuen virtuellen Computers** angezeigt wird, wählen Sie Ihr Abonnement aus, und klicken Sie dann auf **Weiter**.
   ![Assistent zum Erstellen eines neuen virtuellen Computers][CR02]

1. Geben Sie auf dem nächsten Bildschirm des Assistenten die folgenden Optionen an, und klicken Sie dann auf **Weiter**:  ![Assistent zum Erstellen eines neuen virtuellen Computers][CR03]

   a. **Standort:** gibt den Standort an, an dem Ihr virtueller Computer erstellt wird, z.B. „USA, Westen“

   b. **Herausgeber:** gibt den Herausgeber an, der das zum Erstellen des virtuellen Computers verwendete Image erstellt hat, z.B. „Microsoft“

   c. **Angebot:** gibt an, welches Angebot eines virtuellen Computers für den ausgewählten Herausgeber verwendet werden soll, z.B. „JDK“

   d. **SKU:** gibt die zu verwendende *SKU (Stock-Keeping Unit, Artikelnummer)* aus dem ausgewählten Angebot an; z.B. „JDK_8“

   e. **Versionsnr.:** gibt an, welche Version der ausgewählten SKU verwendet werden soll

1. Geben Sie auf dem nächsten Bildschirm des Assistenten die folgenden Optionen an, und klicken Sie dann auf **Weiter**: ![Assistent zum Erstellen eines neuen virtuellen Computers][CR04]

   a. **Name des virtuellen Computers:** gibt den Namen Ihres virtuellen Computers an. Dieser muss mit einem Buchstaben beginnen und darf nur Buchstaben, Ziffern und Bindestriche enthalten.

   b. **Größe:** gibt die Anzahl der Kerne und den Speicher an, die dem virtuellen Computer zugeordnet werden sollen

   c. **Benutzername:** gibt das Administratorkonto an, das für die Verwaltung Ihres virtuellen Computer erstellt werden soll

   d. **Kennwort** und **Bestätigen**: geben das Kennwort für Ihr Administratorkonto an

1. Legen Sie auf dem letzten Bildschirm des Assistenten die folgenden Optionen fest:

   a. **Ressourcengruppe:** Geben Sie die Ressourcengruppe für den virtuellen Computer an. Sie müssen eine der folgenden Optionen auswählen:
      * **Neu erstellen:** gibt an, dass Sie eine neue Ressourcengruppe erstellen möchten
      * **Vorhandene verwenden:** gibt an, dass Sie in einer Liste von Ressourcengruppen, die Ihrem Azure-Konto zugeordnet sind, eine Auswahl treffen möchten

   b. **Speicherkonto:** gibt das Speicherkonto zum Speichern Ihres virtuellen Computers an. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues Konto erstellen. Wenn Sie **&lt;&lt;Neu erstellen&gt;&gt;** auswählen, wird das folgende Dialogfeld angezeigt:

      ![Dialogfeld „Neues Speicherkonto erstellen“][CR05]

   c. **Virtuelles Netzwerk** und **Subnetz**: geben das virtuelle Netzwerk und das Subnetz an, mit dem Ihr virtueller Computer eine Verbindung herstellt. Sie können ein vorhandenes Netzwerk und Subnetz für den virtuellen Computer auswählen oder ein neues Netzwerk und Subnetz erstellen. Wenn Sie **&lt;&lt;Neu erstellen&gt;&gt;** auswählen, wird das folgende Dialogfeld angezeigt:<br/>

      ![Dialogfeld „Neues virtuelles Netzwerk erstellen“][CR06]

   d. **Öffentliche IP-Adresse:** gibt eine von außen erreichbare IP-Adresse für den virtuellen Computer an. Sie können eine neue IP-Adresse erstellen oder **(Keine)** auswählen, wenn Ihr virtueller Computer nicht über eine öffentliche IP-Adresse verfügen soll.

   e. **Netzwerksicherheitsgruppe:** gibt eine optionale Netzwerkfirewall für den virtuellen Computer an. Sie können eine vorhandene Firewall verwenden oder **(Keine)** auswählen, wenn Ihr virtueller Computer keine Netzwerkfirewall verwendet.

   f. **Verfügbarkeitsgruppe:** gibt eine optionale Verfügbarkeitsgruppe an, der der virtuelle Computer angehören kann. Sie können eine vorhandene Verfügbarkeitsgruppe auswählen, eine neue Verfügbarkeitsgruppe erstellen oder **(Keine)** auswählen, wenn Ihr virtueller Computer keiner Verfügbarkeitsgruppe angehören soll.

1. Nachdem Sie alle oben aufgeführten Optionen eingegeben haben, klicken Sie auf **Fertig stellen**: ![Assistent zum Erstellen eines neuen virtuellen Computers][CR07]

1. Wenn Sie die oben aufgeführten Schritte abgeschlossen haben, wird der neue virtuelle Computer im Fenster des Azure-Explorers angezeigt.
   ![Neuer virtueller Computer][CR08]

## <a name="restarting-a-virtual-machine-in-eclipse"></a>Neustarten eines virtuellen Computers in Eclipse

Um einen virtuellen Computer mithilfe des Azure-Explorers in Eclipse neu zu starten, führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Ansicht des **Azure-Explorers** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Neu starten** aus.
   ![Neustarten eines virtuellen Computers][RE01]

1. Klicken Sie auf **Ja** wenn Sie gefragt werden, ob der virtuelle Computer neu gestartet werden soll.
   ![Neustarten eines virtuellen Computers][RE02]

## <a name="shutting-down-a-virtual-machine-in-eclipse"></a>Herunterfahren eines virtuellen Computers in Eclipse

Um einen ausgeführten virtuellen Computer mithilfe des Azure-Explorers in Eclipse herunterzufahren, führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Ansicht des **Azure-Explorers** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Herunterfahren** aus.
   ![Herunterfahren eines virtuellen Computers][SH01]

1. Klicken Sie auf **Ja** wenn Sie gefragt werden, ob der virtuelle Computer heruntergefahren werden soll.
   ![Herunterfahren eines virtuellen Computers][SH02]

## <a name="deleting-a-virtual-machine-in-eclipse"></a>Löschen eines virtuellen Computers in Eclipse

Um einen virtuellen Computer mithilfe des Azure-Explorers in Eclipse zu löschen, führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Ansicht des **Azure-Explorers** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Löschen** aus.
   ![Löschen eines virtuellen Computers][DE01]

1. Klicken Sie auf **Ja** wenn Sie gefragt werden, ob der virtuelle Computer gelöscht werden soll.
   ![Löschen eines virtuellen Computers][DE02]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Größen und Preisen für virtuelle Azure-Computer finden Sie unter den folgenden Links:

* Größen für virtuelle Azure-Computer
  * [Größen für virtuelle Windows-Computer in Azure]
  * [Größen für virtuelle Linux-Computer in Azure]
* Preise für Azure Virtual Machines
  * [Preise von virtuellen Windows-Computern]
  * [Preise von virtuellen Linux-Computern]

Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse] (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse)
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Azure Sign In Instructions for the Azure Toolkit for IntelliJ] (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ)
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center] und in den [Java-Tools für Visual Studio Team Services].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse)
[Azure Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ)
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

[Größen für virtuelle Windows-Computer in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Größen für virtuelle Linux-Computer in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Preise von virtuellen Windows-Computern]: /pricing/details/virtual-machines/windows/
[Preise von virtuellen Linux-Computern]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png

