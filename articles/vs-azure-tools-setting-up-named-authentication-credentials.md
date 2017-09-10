---
title: Einrichten benannter Authentifizierungsanmeldeinformationen | Microsoft Docs
description: "Erfahren Sie, wie Sie die Anmeldeinformationen angeben, mit denen Visual Studio Anforderungen für Azure authentifizieren kann, um eine Anwendung von Visual Studio aus in Azure zu veröffentlichen oder einen vorhandenen Clouddienst zu überwachen. "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: c486676a70e195ec85ad40540ea4b7caaa86bc48
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="setting-up-named-authentication-credentials"></a>Einrichten benannter Authentifizierungsanmeldeinformationen
Geben Sie die Anmeldeinformationen an, mit denen Visual Studio Anforderungen für Azure authentifizieren kann, um eine Anwendung von Visual Studio aus in Azure zu veröffentlichen oder einen vorhandenen Clouddienst zu überwachen. Es gibt verschiedene Stellen in Visual Studio, an denen Sie sich anmelden können, um diese Anmeldeinformationen bereitzustellen. Beispielsweise können Sie im Server-Explorer das Kontextmenü des Knotens **Azure** öffnen und **Verbindung mit Microsoft Azure-Abonnement herstellen** auswählen. Wenn Sie sich anmelden, stehen die Abonnementinformationen, die dem Azure-Konto zugeordnet sind, in Visual Studio zur Verfügung. Sie müssen keine weiteren Schritte ausführen.

Die Azure-Tools unterstützen auch eine ältere Möglichkeit zum Bereitstellen von Anmeldeinformationen mithilfe der Abonnementdatei (.publishsettings). In diesem Thema wird diese Methode beschrieben, die in Azure SDK 2.2 weiterhin unterstützt wird.

Die folgenden Elemente sind für die Authentifizierung bei Azure erforderlich:

* Ihre Abonnement-ID
* Ein gültiges X.509 v3-Zertifikat

> [!NOTE]
> Der Schlüssel für das X.509 v3-Zertifikat muss mindestens 2048 Bit lang sein. Von Azure werden alle Zertifikate abgelehnt, die diese Anforderung nicht erfüllen oder ungültig sind.
>
>

Visual Studio verwendet die Abonnement-ID zusammen mit den Zertifikatdaten als Anmeldeinformationen. Die entsprechenden Anmeldeinformationen werden in der Abonnementdatei (.publishsettings) verwiesen wird, enthält einen öffentlichen Schlüssel für das Zertifikat. Die Abonnementdatei kann Anmeldeinformationen für mehrere Abonnements enthalten.

Im Dialogfeld **Neues Abonnement/Abonnement bearbeiten** können Sie die Abonnementinformationen bearbeiten, wie weiter unten in diesem Thema erläutert.

Wenn Sie ein Zertifikat selbst erstellen möchten, finden Sie die entsprechenden Anweisungen unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx). Laden Sie anschließend das Zertifikat manuell in das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) hoch.

> [!NOTE]
> Diese Anmeldeinformationen, die Visual Studio zum Verwalten Ihrer Clouddienste benötigt, müssen nicht den Anmeldeinformationen entsprechen, die zum Authentifizieren einer Anforderung für die Azure-Speicherdienste erforderlich sind.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Importieren, Einrichten oder Bearbeiten von Anmeldeinformationen für die Authentifizierung in Visual Studio
Sie können die Anmeldeinformationen für die Authentifizierung auch im Dialogfeld **Neues Abonnement** importieren, einrichten oder ändern. Dieses Dialogfeld wird angezeigt, wenn Sie eine der folgenden Aktionen ausführen:

* Öffnen Sie im **Server-Explorer** das Kontextmenü des Knotens **Azure**, und wählen Sie **Abonnements verwalten und filtern...** aus. Wählen Sie die Registerkarte **Zertifikate**, und führen Sie eine der folgenden Aktionen aus:

    * Wählen Sie **Importieren**, um das Dialogfeld „Microsoft Azure-Abonnements importieren“ zu öffnen, in dem Sie die Abonnementsdatei für das derzeit geladene Abonnement herunterladen können, navigieren Sie zu ihrem Downloadspeicherort, und importieren Sie sie anschließend zur Verwendung für die Authentifizierung.
    * Wählen Sie **Neu**, um das Dialogfeld „Neues Abonnement“ zu öffnen, in dem Sie ein neues Abonnement zur Verwendung für die Authentifizierung einrichten können.
    * Wählen Sie (nach der Auswahl des aktiven Abonnements) **Bearbeiten**, um das Dialogfeld „Abonnement bearbeiten“ zu öffnen, in dem Sie ein vorhandenes Abonnement zur Verwendung für die Authentifizierung bearbeiten können. 

Bei der folgenden Vorgehensweise wird davon ausgegangen, dass das Dialogfeld **Neues Abonnement** geöffnet ist.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>So richten Sie Anmeldeinformationen für die Authentifizierung in Visual Studio ein
1. Wählen Sie in der Liste **Wählen Sie ein vorhandenes Zertifikat für die Authentifizierung aus** ein vorhandenes Zertifikat aus.
2. Klicken Sie auf den Link **Vollständigen Pfad kopieren**. Der Pfad des Zertifikats (CER-Datei) wird in die Zwischenablage kopiert.

   > [!IMPORTANT]
   > Laden Sie dieses Zertifikat in das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) hoch, um die Azure-Anwendung aus Visual Studio zu veröffentlichen.
   >
   >
3. So laden Sie das Zertifikat in das Azure-Portal hoch:

   1. Öffnen Sie das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   2. Wenn Sie aufgefordert werden, melden Sie sich beim Portal an, und navigieren Sie zu **Einstellungen**, **Verwaltungszertifikate**.
   3. Wählen Sie im Bereich „Verwaltungszertifikate“ **Hochladen**.
   4. Wählen Sie Ihr Azure-Abonnement, fügen Sie den vollständigen Pfad der CER-Datei ein, die Sie soeben erstellt haben, und wählen Sie **Hochladen**.

