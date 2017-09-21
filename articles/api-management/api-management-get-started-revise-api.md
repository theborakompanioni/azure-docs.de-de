---
title: Aktualisieren Ihrer API in Azure API Management mithilfe von Revisionen | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie mithilfe von Revisionen geringfügige Änderungen in API Management vornehmen."
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.translationtype: HT
ms.sourcegitcommit: 212b163c49fdd133b0ed2d99b33035fcc391aec6
ms.openlocfilehash: 0d67166a16ae4d640080ad83e7625e594b0dc246
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---

# <a name="make-non-breaking-changes-safely-using-revisions"></a>Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen
In diesem Tutorial erfahren Sie, wie Sie Ihre API gefahrlos ändern und Ihre Entwickler über die Änderung informieren.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial müssen Sie einen API Management-Dienst erstellen und über eine API verfügen, die Sie verändern können (anstelle von „Conference API“).

## <a name="about-revisions"></a>Informationen zu Revisionen
Wenn Ihre API einsatzbereit ist und von Entwicklern verwendet wird, müssen Sie in der Regel bei Änderungen an der API vorsichtig sein, um Aufrufer Ihrer API nicht zu beeinträchtigen. Außerdem empfiehlt es sich, Entwickler über die vorgenommenen Änderungen zu informieren. Hierzu können Sie **Revisionen** in Azure API Management verwenden.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
In dieser exemplarischen Vorgehensweise fügen wir eine neue Revision hinzu, fügen ihr einen Vorgang hinzu, machen die Revision aktuell und erstellen einen Eintrag im Änderungsprotokoll.

## <a name="add-a-new-revision"></a>Hinzufügen einer neuen Revision
1. Navigieren Sie im Azure-Portal innerhalb Ihres API Management-Diensts zur Seite **APIs**.
2. Wählen Sie in der API-Liste die Option **Conference API** aus, und klicken Sie anschließend im Menü im oberen Bereich der Seite auf die Registerkarte **Revisionen**.
3. Klicken Sie auf **+ Revision hinzufügen**.

    > [!TIP]
    > Alternativ können Sie im Kontextmenü (**...**) für die API auf **Revision hinzufügen** klicken.
![Menü „Revisionen“ im oberen Bereich des Bildschirms](media/api-management-getstarted-revise-api/TopMenu.PNG)

4. Geben Sie eine Beschreibung für Ihre neue Revision an, damit Sie wissen, wofür sie verwendet wird.
5. Wählen Sie **Erstellen**
6. Die neue Revision wird erstellt.

    > [!NOTE]
    > Ihre ursprüngliche API bleibt in **Revision 1**. Dies ist die Revision, die weiterhin von den Benutzern aufgerufen wird, bis Sie beschließen, eine andere Revision zur aktuellen Revision zu machen.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vornehmen geringfügiger Änderungen an Ihrer Revision
1. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
2. Beachten Sie, dass die **Revisionsauswahl** (direkt über der Registerkarte „Entwurf“) die aktuelle Revision als **Revision 2** anzeigt.

    > [!TIP]
    > Mit der Revisionsauswahl können Sie zwischen Revisionen wechseln, die Sie bearbeiten möchten.

3. Klicken Sie auf **+ Vorgang hinzufügen**.
4. Legen Sie den neuen Vorgang auf **POST** und Name und Anzeigename des Vorgangs auf **Feedback** fest.
5. Speichern** **Sie Ihren neuen Vorgang.
6. Sie haben nun eine Änderung an **Revision 2** vorgenommen. Wechseln Sie mithilfe der **Revisionsauswahl** im oberen Seitenbereich zu **Revision 1** zurück.
7. Wie Sie sehen, ist der neue Vorgang in **Revision 1** nicht vorhanden. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
1. Klicken Sie im oberen Bereich der Seite auf die Registerkarte **Revisionen**.
![Das Revisionsmenü auf dem Revisionsbildschirm.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
2. Öffnen Sie das Kontextmenü (**...**) für **Revision 2**.
3. Klicken Sie auf **Make Current** (Als aktuell festlegen).
![Festlegen der Revision als aktuelle Revision und Erstellen eines Eintrags im Änderungsprotokoll](media/api-management-getstarted-revise-api/MakeCurrent.PNG)
4. Klicken Sie auf **Post to Public Change Log for this API** (In öffentlichem Änderungsprotokoll für diese API veröffentlichen).
5. Geben Sie eine Änderungsbeschreibung für Entwickler an (beispielsweise **Neuen Feedbackvorgang hinzugefügt.**).
6. **Revision 2** ist jetzt aktuell.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal
1. Klicken Sie im oberen Menü auf **Entwicklerportal**.
2. Klicken Sie auf **APIs** und anschließend auf **Conference API**.
3. Wie Sie sehen, ist Ihr neuer **Feedback**-Vorgang nun verfügbar.
4. Klicken Sie unter dem API-Namen auf **API Change History** (API-Änderungsverlauf).
5. Der Änderungsprotokolleintrag ist in der Liste enthalten.
![Änderungsprotokoll im Entwicklerportal](media/api-management-getstarted-revise-api/ChangeLogDevPortal.PNG)

## <a name="next-steps"></a>Nächste Schritte
[Veröffentlichen von API-Versionen mit Azure API Management](#api-management-getstarted-publish-versions.md)
