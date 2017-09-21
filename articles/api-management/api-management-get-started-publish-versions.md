---
title: "Veröffentlichen von Versionen Ihrer API mithilfe von Azure API Management | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie in API Management mehrere Versionen veröffentlichen."
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
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 7c355e2feb5ebe5971d8391b326422a1abec1497
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="publish-multiple-versions-of-your-api-in-a-predictable-way"></a>Planbares Veröffentlichen mehrerer Versionen Ihrer API
In diesem Tutorial erfahren Sie, wie Sie Versionen Ihrer API einrichten und auswählen, wie sie von API-Entwicklern aufgerufen werden.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial müssen Sie einen API Management-Dienst erstellen und über eine API verfügen, die Sie verändern können (anstelle von „Conference API“).

## <a name="about-versions"></a>Informationen zu Versionen
Manchmal ist es unpraktisch, wenn alle Aufrufer genau die gleiche Version Ihrer API verwenden müssen. Manchmal möchten Sie neue oder andere API-Features für einige Benutzer veröffentlichen, während andere weiterhin die API verwenden möchten, die derzeit für sie geeignet ist. Wenn Aufrufer auf eine höhere Version upgraden möchten, möchten Sie dafür einen leicht verständlichen Ansatz verwenden.  Dies ist mit **Versionen** in Azure API Management möglich.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
In dieser exemplarischen Vorgehensweise fügen wir eine neue Version für eine vorhandene API hinzu. Dabei wählen wir ein Schema für die Versionsverwaltung und einen Versionsbezeichner aus.

## <a name="add-a-new-version"></a>Hinzufügen einer neuen Version
![API-Kontextmenü – Version hinzufügen](media/api-management-getstarted-publish-versions/AddVersionMenu.png)
1. Navigieren Sie im Azure-Portal innerhalb Ihres API Management-Diensts zur Seite **APIs**.
2. Wählen Sie in der API-Liste die Option **Conference API** aus, und rufen Sie anschließend über **...** das Kontextmenü auf.
3. Klicken Sie auf **+ Version hinzufügen**.

    > [!TIP]
    > Versionen können auch beim Erstellen einer neuen API aktiviert werden. Wählen Sie hierzu auf dem Bildschirm **API hinzufügen** die Option **Versionsverwaltung für diese API?** aus.

## <a name="choose-a-versioning-scheme"></a>Auswählen eines Schemas für die Versionsverwaltung
Mit Azure API Management können Sie auswählen, wie Aufrufer angeben können, welche Version Ihrer API sie verwenden möchten. Hierzu wählen Sie ein **Schema für die Versionsverwaltung** aus. Mögliche Optionen für dieses Schema sind **Pfad, Header oder Abfragezeichenfolge**. In unserem Beispiel verwenden wir „Pfad“.
![Bildschirm zum Hinzufügen von Versionen](media/api-management-getstarted-publish-versions/AddVersion.PNG)
1. Lassen Sie **Pfad** als **Schema für Versionsverwaltung** ausgewählt.
2. Fügen Sie **v1** als **Versionsbezeichner** hinzu.

    > [!TIP]
    > Wenn Sie **Header** oder **Abfragezeichenfolge** als Schema für die Versionsverwaltung auswählen, müssen Sie einem zusätzlichen Wert (den Namen des Headers oder Abfragezeichenfolgen-Parameters) angeben.

3. Geben Sie bei Bedarf eine Beschreibung an.
4. Klicken Sie auf **Erstellen**, um Ihre neue Version einzurichten.
5. In der API-Liste werden unter **Big Conference API** nun zwei unterschiedliche APIs angezeigt: **Original** und **v1**.
![Unter einer API aufgeführte Versionen im Azure-Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Wenn Sie eine Version für eine API ohne Versionsverwaltung hinzufügen, wird immer ein **Original** hinzugefügt, das auf die Standard-URL reagiert. Dadurch wird sichergestellt, dass bereits vorhandene Aufrufer durch das Hinzufügen einer Version nicht beeinträchtigt werden. Wenn Sie Versionen gleich bei der Erstellung einer neuen API aktivieren, wird kein Original erstellt.

6. Nun können Sie **v1** als eine API bearbeiten und konfigurieren, die vollständig vom **Original** getrennt ist. Änderungen an einer Version haben keine Auswirkungen auf die andere.

## <a name="add-the-version-to-a-product"></a>Hinzufügen der Version zu einem Produkt
Damit Ihre neue Version für Aufrufer sichtbar ist, muss sie einem **Produkt** hinzugefügt werden. (Produkte werden nicht von übergeordneten Versionen geerbt.)

1. Klicken Sie auf der Dienstverwaltungsseite auf **Produkte**.
2. Klicken Sie auf **Unbegrenzt**.
3. Klicken Sie auf **APIs**.
4. Wählen Sie **Hinzufügen**.
5. Klicken Sie auf **Conference API, Version v1**.
6. Kehren Sie zur Dienstverwaltungsseite zurück, und klicken Sie auf **APIs**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Anzeigen der Version im Entwicklerportal
1. Klicken Sie im oberen Menü auf **Entwicklerportal**.
2. Klicken Sie auf **APIs**. Wie Sie sehen, werden für **Conference API** die Versionen **Original** und **v1** angezeigt.
3. Klicken Sie auf **v1**.
4. Beachten Sie die **Anforderungs-URL** des ersten Vorgangs in der Liste. Dort sehen Sie, dass der API-URL-Pfad **v1** enthält.
![Angezeigte Version im Entwicklerportal](media/api-management-getstarted-publish-versions/VersionDevPortal.PNG)

