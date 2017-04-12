---
title: Bereitstellen von SAP S/4HANA oder BW/4HANA auf einem virtuellen Azure-Computer | Microsoft-Dokumentation
description: Bereitstellen von SAP S/4HANA oder BW/4HANA auf einem virtuellen Azure-Computer
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 2a645e6ab50b7b764a56bb7a79ad5cf295881214
ms.lasthandoff: 04/03/2017


---
# <a name="deploy-sap-s4hana-or-bw4hana-on-microsoft-azure"></a>Bereitstellen von SAP S/4HANA oder BW/4HANA in Microsoft Azure
In diesem Artikel wird beschrieben, wie Sie S/4HANA in Microsoft Azure über die SAP Cloud Appliance Library (SAP CAL) 3.0 bereitstellen. Das Bereitstellen von anderen SAP HANA-basierten Lösungen wie BW/4HANA funktioniert in Bezug auf den Prozess genauso. Sie müssen lediglich eine andere Lösung auswählen.

> [!NOTE]
Weitere Informationen zur SAP Cloud Appliance Library finden Sie auf der [Startseite der Website](https://cal.sap.com/). Es gibt auch einen Blog von SAP zur [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

## <a name="step-by-step-process-to-deploy-the-solution"></a>Vorgehensweise zum Bereitstellen der Lösung

Die folgenden Screenshots zeigen, wie S/4HANA in Azure bereitgestellt wird. Die Vorgehensweise funktioniert für andere Lösungen wie BW/4HANA genauso.

Der erste Screenshot zeigt alle SAP CAL HANA-basierten Lösungen, die in Azure verfügbar sind. Beachten Sie die **SAP S/4HANA on-premises edition** (lokale SAP S/4HANA-Edition) unten im Screenshot.

![Screenshot des SAP Cloud Appliance Library Solutions-Fensters](./media/cal-s4h/s4h-pic-1b.jpg)

Erstellen Sie zunächst ein neues SAP CAL-Konto. Unter **Konten** finden Sie zwei Auswahlmöglichkeiten für Azure: Microsoft Azure und eine durch 21Vianet betriebene Azure-Option. Wählen Sie für dieses Beispiel **Microsoft Azure**.

![Screenshot des SAP Cloud Appliance Library-Kontenfensters](./media/cal-s4h/s4h-pic-2.jpg)

Geben Sie dann die Azure-Abonnement-ID an, die Sie im Azure-Portal finden können. Laden Sie danach ein Azure-Verwaltungszertifikat herunter.

![Screenshot des SAP Cloud Appliance Library-Kontenfensters](./media/cal-s4h/s4h-pic3b.jpg)

> [!NOTE]
Um Ihre Azure-Abonnement-ID zu suchen, sollten Sie das klassische Azure-Portal und nicht das aktuellere verwenden. Der Grund dafür ist, dass SAP CAL für das neue Modell noch nicht angepasst wurde, sodass weiterhin das klassische Portal erforderlich ist, um mit Verwaltungszertifikaten zu arbeiten.

Im folgenden Screenshot ist das klassische Portal dargestellt. Wählen Sie unter **SETTINGS** (EINSTELLUNGEN) die Registerkarte **SUBSCRIPTIONS** (ABONNEMENTS) aus, um die Abonnement-ID zu erhalten, die im SAP CAL Accounts-Fenster eingegeben werden muss.

![Screenshot des klassischen Azure-Portals](./media/cal-s4h/s4h-pic4b.jpg)

Wechseln Sie von **EINSTELLUNGEN** zur Registerkarte **VERWALTUNGSZERTIFIKATE**. Laden Sie ein Verwaltungszertifikat hoch, um SAP CAL die Berechtigungen zum Erstellen von virtuellen Computern in einem Kundenabonnement zu erteilen. (Sie laden das Verwaltungszertifikat hoch, das zuvor von SAP CAL heruntergeladen wurde.)

![Screenshot des klassischen Azure-Portals, Registerkarte „Management Certificates“ (Verwaltungszertifikate)](./media/cal-s4h/s4h-pic5.jpg)

Ein Dialogfeld zum Auswählen der heruntergeladenen Zertifikatdatei wird angezeigt.

![Screenshot des Dialogfelds „Upload a management certificate“ (Verwaltungszertifikat hochladen)](./media/cal-s4h/s4h-pic8.jpg)

Nachdem das Zertifikat hochgeladen wurde, kann die Verbindung zwischen SAP CAL und dem Azure-Abonnement in SAP CAL getestet werden. Eine Meldung mit dem Hinweis, dass die Verbindung gültig ist, wird angezeigt.

![Screenshot des SAP Cloud Appliance Library Accounts-Fensters](./media/cal-s4h/s4h-pic9.jpg)

Wählen Sie als Nächstes eine Lösung aus, die bereitgestellt werden soll, und erstellen Sie eine Instanz.
Geben Sie einen Instanznamen ein, wählen Sie eine Azure-Region aus, und definieren Sie das Masterkennwort für die Lösung.

![Screenshot des SAP Cloud Appliance Library Solutions-Fensters](./media/cal-s4h/s4h-pic10.jpg)

Nach einiger Zeit wird je nach Größe und Komplexität (Schätzung wird durch SAP CAL angegeben) die Lösung als „active“ (aktiv) und betriebsbereit angezeigt.

![Screenshot des SAP Cloud Appliance Library Instances-Fensters](./media/cal-s4h/s4h-pic11.jpg)

Sie können einige Details der Lösung ansehen, z.B. welche Art von VMs bereitgestellt wurde. In diesem Fall wurden drei Azure-VMs unterschiedlicher Größe und für verschiedene Zwecke erstellt.

![Screenshot des SAP Cloud Appliance Library Instances-Fensters](./media/cal-s4h/s4h-pic12.jpg)

Im klassischen Azure-Portal werden diese virtuellen Computer jeweils mit dem gleichen Instanznamen angezeigt, der in SAP CAL vergeben wurde.

![Screenshot mit drei virtuellen Computern im klassischen Azure-Portal](./media/cal-s4h/s4h-pic13.jpg)

Nun ist es möglich, für die Lösung über die Schaltfläche „connect“ (Verbinden) im SAP CAL-Portal eine Verbindung herzustellen. Das Dialogfeld enthält einen Link zu einem Benutzerhandbuch, in dem alle Standardanmeldeinformationen für die Arbeit mit der Lösung beschrieben sind.

![Screenshot des Dialogfelds „Connect to the Instance“ (Mit der Instanz verbinden)](./media/cal-s4h/s4h-pic14b.jpg)

Eine weitere Option ist die Anmeldung an der Windows-Client-VM und beispielsweise das Starten der vorkonfigurierten SAP-GUI.

![Screenshot der vorkonfigurierten SAP-GUI](./media/cal-s4h/s4h-pic15.jpg)

