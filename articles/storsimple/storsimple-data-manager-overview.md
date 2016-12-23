---
title: "Übersicht über Microsoft Azure StorSimple Data Manager | Microsoft Docs"
description: "Bietet eine Übersicht über den StorSimple Data Manager-Dienst (private Vorschau)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
translationtype: Human Translation
ms.sourcegitcommit: 32fcca540c1db317c5f11e94b2ee3359cadcc806
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73

---

# <a name="storsimple-data-manager-overview-private-preview"></a>Übersicht über StorSimple Data Manager (private Vorschau)

## <a name="overview"></a>Übersicht

Microsoft Azure StorSimple ist eine Hybridcloud-Speicherlösung, die sich mit der Komplexität von unstrukturierten Daten befasst, die bei Datenfreigabe häufig auftreten. StorSimple verwendet den Cloudspeicher als eine Erweiterung der lokalen Lösung, und verteilt die Daten automatisch auf die Ebenen des lokalen Speichers und des Cloudspeichers. Durch den integrierten Datenschutz mit lokalen Momentaufnahmen und Cloudmomentaufnahmen wird keine ausgedehnte Speicherinfrastruktur mehr benötigt. Archivierung und Notfallwiederherstellung funktioniert ebenfalls nahtlos, nachdem die Cloud als Offsitestandort fungiert.

Der Datentransformationsdienst, der in diesem Dokument vorgestellt wird, erlaubt es Ihnen, nahtlos auf Daten von StorSimple in der Cloud zuzugreifen. Dieser Dienst stellt APIs zur Verfügung, um Daten aus StorSimple zu extrahieren und anderen Azure-Diensten in Formaten darzustellen, die einfach verarbeitet werden können. Die in dieser Vorschau unterstützten Formate sind Azure-Blobs und Azure Media Services-Objekte. Diese Transformation ermöglicht es Ihnen, Dienste wie Azure Media Services, Azure HDInsight, Azure Machine Learning und Azure Search einfach zu verknüpfen, um Daten auf einem lokalen Gerät der StorSimple 8000-Serie zu verwalten.

Das folgende allgemeine Blockdiagramm demonstriert diesen Vorgang.

![Übersichtsdiagramm](./media//storsimple-data-manager-overview/high-level-diagram.png)

In diesem Dokument wird erläutert, wie Sie sich für eine private Vorschau dieses Dienstes registrieren können. Außerdem wird erklärt, wie Sie diesen Dienst nutzen können, um Anwendungen zu schreiben, die StorSimple-Daten und andere Azure-Dienste in der Cloud verwenden.

## <a name="sign-up-for-data-manager-preview"></a>Registrieren für die Data Manager-Vorschau
Überprüfen Sie, ob die folgenden Voraussetzungen erfüllt sind, bevor Sie sich für den Data Manager-Dienst registrieren.

### <a name="prerequisites"></a>Voraussetzungen

In dieser Übung wird davon ausgegangen, dass Sie über Folgendes verfügen:
* ein aktives Azure-Abonnement
* Zugriff auf ein registriertes Gerät der StorSimple 8000-Serie
* alle Schlüssel, die dem Gerät der StorSimple 8000-Serie zugeordnet sind.

### <a name="sign-up"></a>Registrieren

StorSimple Data Manager befindet sich in der privaten Vorschau. Führen Sie die folgenden Schritte aus, um sich für eine private Vorschau dieses Diensts zu registrieren:

1.  Melden Sie sich mit der StorSimple Data Manager-Erweiterung im Azure-Portal an: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Verwenden Sie Ihre Azure AD-Anmeldeinformationen, um sich anzumelden.

2.  Klicken Sie auf die das Symbol **+**, um einen Dienst zu erstellen. Klicken Sie auf **Speicher**, und klicken Sie dann auf dem Blatt, das geöffnet wird, auf **Alle anzeigen**.

    ![Suchen des StorSimple Data Manager-Symbols](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Das StorSimple Data Manager-Symbol wird angezeigt.

    ![Auswahl des StorSimple Data Manager-Symbols](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Klicken Sie auf das StorSimple Data Manager-Symbol, und klicken Sie dann auf **Erstellen**. Wählen Sie das Abonnement aus, das Sie für die private Vorschau aktivieren wollen, und klicken Sie dann auf **Sign me up!** (Registrieren!).

    ![Registrieren](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Dadurch wird eine Anfrage für Ihre Registrierung gesendet. Sie werden sobald wie möglich registriert. Nachdem Ihr Abonnement aktiviert ist, können Sie einen StorSimple Data Manager-Dienst erstellen.

6. Klicken Sie auf das Sternsymbol, um den Dienst zu Ihren Favoriten hinzuzufügen, damit Sie einfach auf StorSimple Data Manager zugreifen können.

    ![Zugriff auf StorSimple Data Manager](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Nächste Schritte

[Use StorSimple Data Manager UI to transform your data (Verwenden der StorSimple Data Manager-UI zum Transformieren von Daten) ](storsimple-data-manager-ui.md).


<!--HONumber=Nov16_HO4-->


