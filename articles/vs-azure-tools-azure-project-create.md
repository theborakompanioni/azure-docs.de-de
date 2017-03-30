---
title: Erstellen eines Azure-Clouddienstprojekts mit Visual Studio | Microsoft Docs
description: Erfahren Sie, wie Sie ein Azure-Clouddienstprojekt mit Visual Studio erstellen.
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: b1870b5db6e194f61d2f8b526e0c81b4d46699e4
ms.lasthandoff: 03/22/2017


---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Erstellen eines Azure-Clouddienstprojekts mit Visual Studio
Die Azure Tools für Visual Studio umfassen eine Projektvorlage, mit der Sie einen Azure-Clouddienst erstellen können. Nach der Erstellung des Projekts können Sie den Clouddienst mit Visual Studio in Azure konfigurieren, debuggen und bereitstellen.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Schritte zum Erstellen eines Azure-Clouddienstprojekts in Visual Studio
In diesem Abschnitt wird das Erstellen eines Azure-Clouddienstprojekts in Visual Studio mit einem oder mehreren Webrollen erläutert.  

1. Starten Sie Visual Studio als Administrator.

1. Wählen Sie im Hauptmenü die Optionen **Datei** > **Neu** > **Projekt** aus.

1. Wählen Sie **Cloud** in den Projektvorlagenknoten „Visual C#“ oder „Visual Basic“ und dann **Azure-Clouddienst** in der Liste der Vorlagen aus.

    ![Neuer Azure-Clouddienst](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Geben Sie an, mit welcher Version von .NET Framework Sie das Projekt entwickeln möchten.

1. Geben Sie einen Namen und Speicherort für das Projekt und einen Namen für die Projektmappe ein. 

1. Klicken Sie auf **OK**.

1. Wählen Sie im Dialogfeld **Neuer Clouddienst** die Rollen aus, die Sie hinzufügen möchten, und klicken Sie auf die Schaltfläche mit dem Pfeil nach rechts, um sie der Projektmappe hinzuzufügen.

    ![Auswählen neuer Azure-Clouddienstrollen](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Um eine Rolle umzubenennen, die Sie hinzugefügt haben, zeigen Sie im Dialogfeld **Neuer Clouddienst** mit dem Mauszeiger auf die Rolle, und wählen Sie im Kontextmenü die Option **Umbenennen** aus. Sie können eine Rolle auch in der Projektmappe umbenennen (im **Projektmappen-Explorer**), nachdem sie hinzugefügt wurde.

    ![Umbenennen einer Azure Clouddienstrolle](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

Das Visual Studio-Azure-Projekt weist Zuordnungen zu den Rollenprojekten in der Projektmappe auf. Das Projekt enthält zudem die *Dienstdefinitionsdatei* und die *Dienstkonfigurationsdatei*:

- **Dienstdefinitionsdatei:** Definiert die Laufzeiteinstellungen für Ihre Anwendung, u.a. erforderliche Rollen, Endpunkte und Größe des virtuellen Computers. 
- **Dienstkonfigurationsdatei:** Konfiguriert, wie viele Instanzen einer Rolle ausgeführt werden, und die Werte der für eine Rolle definierten Einstellungen. 

Weitere Informationen zu diesen Dateien finden Sie unter [Konfigurieren der Rollen für einen Azure-Clouddienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Rollen in Azure-Clouddienstprojekten mit Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)

