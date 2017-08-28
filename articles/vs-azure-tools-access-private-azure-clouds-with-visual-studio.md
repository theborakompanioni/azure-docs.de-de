---
title: Zugreifen auf private Azure-Clouds mit Visual Studio | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe von Visual Studio auf private Cloudressourcen zugreifen.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: b29a3299433e57709ea31de3c7849230ea09c09a
ms.contentlocale: de-de
ms.lasthandoff: 03/21/2017

---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Zugreifen auf private Azure-Clouds mit Visual Studio
Visual Studio unterstützt standardmäßig REST-Endpunkte der öffentlichen Azure-Cloud. In diesem Thema erfahren Sie, wie Sie über das Zertifikat der privaten Cloud auf die private Cloud in Visual Studio zugreifen – und mit dieser interagieren.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>So greifen Sie auf eine private Azure-Cloud in Visual Studio zu
1. Laden Sie im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) für die private Cloud die Datei mit den Veröffentlichungseinstellungen herunter, oder wenden Sie sich an Ihren Administrator, um eine Datei mit Veröffentlichungseinstellungen zu erhalten. Für die öffentliche Version von Azure ist dies der Link zum Herunterladen: [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Die heruntergeladene Datei muss die Erweiterung `.publishsettings` haben.)

1. Öffnen Sie Visual Studio.

1. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie im Kontextmenü die Option **Abonnements verwalten und filtern** aus.
   
    ![Befehl „Abonnements verwalten“](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Wählen Sie im Dialogfeld **Microsoft Azure-Abonnements verwalten** die Registerkarte **Zertifikate** und dann **Importieren** aus.
   
    ![Importieren von Azure-Zertifikaten](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Wählen Sie im Dialogfeld **Microsoft Azure-Abonnements importieren** die Option **Durchsuchen** aus.

    ![Schaltfläche „Durchsuchen“ im Dialogfeld „Microsoft Azure-Abonnements importieren“](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. Navigieren Sie im Dialogfeld **Öffnen** zu dem Verzeichnis, in dem Sie die Datei mit den Veröffentlichungseinstellungen gespeichert haben, markieren Sie die Datei, und wählen Sie dann **Öffnen** aus.

    ![Auswählen der Datei mit den Veröffentlichungseinstellungen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Kehren Sie zum Dialogfeld **Microsoft Azure-Abonnements importieren** zurück, und wählen Sie dort die Option **Importieren** aus.

    ![Importieren der Datei mit den Veröffentlichungseinstellungen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Die Zertifikate aus der Datei mit den Veröffentlichungseinstellungen werden in Visual Studio importiert, und Sie können nun mit den Ressourcen der privaten Cloud interagieren.
   
## <a name="next-steps"></a>Nächste Schritte
- [Veröffentlichen eines Clouddiensts mit Azure Tools](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Gewusst wie: Herunterladen und Importieren von Veröffentlichungseinstellungen und Abonnementinformationen](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)

