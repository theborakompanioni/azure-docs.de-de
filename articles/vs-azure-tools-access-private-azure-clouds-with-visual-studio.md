---
title: Zugreifen auf private Azure-Clouds mit Visual Studio | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe von Visual Studio auf private Cloudressourcen zugreifen.
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26fea09810e4a5ed1dc19123a5354905ec3e37ea


---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Zugreifen auf private Azure-Clouds mit Visual Studio
## <a name="overview"></a>Übersicht
Visual Studio unterstützt standardmäßig REST-Endpunkte der öffentlichen Azure-Cloud. Dies kann jedoch ein Problem sein, wenn Sie Visual Studio mit einer privaten Azure-Cloud verwenden. Sie können Zertifikate verwenden, um Visual Studio für den Zugriff auf REST-Endpunkte der privaten Azure-Cloud zu konfigurieren. Sie erhalten diese Zertifikate über Ihre Datei mit den Azure-Veröffentlichungseinstellungen.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>So greifen Sie auf eine private Azure-Cloud in Visual Studio zu
1. Laden Sie im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) für die private Cloud die Datei mit den Veröffentlichungseinstellungen herunter, oder wenden Sie sich an Ihren Administrator, um eine Datei mit Veröffentlichungseinstellungen zu erhalten. Für die öffentliche Version von Azure ist dies der Link zum Herunterladen: [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Die herunterzuladende Datei muss die Erweiterung ".publishsettings" aufweisen.)
2. Wählen Sie in Visual Studio im **Server-Explorer** den Knoten **Azure** und dann im Kontextmenü den Befehl **Abonnements verwalten** aus.
   
    ![Befehl „Abonnements verwalten“](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)
3. Klicken Sie im Dialogfeld **Microsoft Azure-Abonnements verwalten** auf die Registerkarte **Zertifikate** und dann auf die Schaltfläche **Importieren**.
   
    ![Importieren von Azure-Zertifikaten](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)
4. Navigieren Sie im Dialogfeld **Microsoft Azure-Abonnements importieren** zum Ordner, in dem Sie die Datei mit den Veröffentlichungseinstellungen gespeichert haben, und klicken Sie dann auf die Schaltfläche **Importieren**. Daraufhin werden die Zertifikate in der Datei mit den Veröffentlichungseinstellungen in Visual Studio importiert. Sie sollten jetzt in der Lage sein, mit privaten Cloudressourcen zu interagieren.
   
    ![Importieren von Veröffentlichungseinstellungen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Nächste Schritte
[Veröffentlichen eines Clouddiensts mit Azure Tools](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Gewusst wie: Herunterladen und Importieren von Veröffentlichungseinstellungen und Abonnementinformationen](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)




<!--HONumber=Nov16_HO3-->


