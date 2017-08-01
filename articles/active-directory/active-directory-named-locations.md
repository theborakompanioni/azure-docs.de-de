---
title: Benannte Orte in Azure Active Directory | Microsoft-Dokumentation
description: "Durch Konfigurieren benannter Orte können Sie vermeiden, dass IP-Adressen, die Ihrer Organisation gehören, falsch positive Ergebnisse für den Risikoereignistyp „Unmöglicher Ortswechsel zu atypischen Orten“ liefern."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: e11f19d518b22a7be4f8daf93304821b42e94a9b
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017

---
# <a name="named-locations-in-azure-active-directory"></a>Benannte Orte in Azure Active Directory

Mit dem Feature „Benannte Orte“ von Azure Active Directory können Sie in Ihren Organisationen vertrauenswürdige IP-Adressbereiche bezeichnen. Sie können in Ihrer Umgebung benannte Orte im Kontext der Erkennung von [Risikoereignissen](active-directory-reporting-risk-events.md) verwenden. Das Feature verringert die Anzahl der gemeldeten falschen Positive für den Risikoereignistyp *Unmöglicher Ortswechsel zu atypischen Orten*. 

## <a name="configuration"></a>Konfiguration

So konfigurieren Sie einen benannten Ort

1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im linken Bereich auf **Azure Active Directory**.

    ![Klicken Sie im linken Bereich auf den Link „Azure Active Directory“.](./media/active-directory-named-locations/01.png)

3. Klicken Sie auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

    ![Der Befehl „Bedingter Zugriff“](./media/active-directory-named-locations/05.png)


4. Klicken Sie auf dem Blatt **Azure Active Directory** im Abschnitt **Verwalten** auf **Benannte Orte**.

    ![Der Befehl „Benannte Orte“](./media/active-directory-named-locations/06.png)


5. Klicken Sie auf dem Blatt **Benannte Orte** auf **Neuer Standort**.

    ![Der Befehl „Neuer Standort“](./media/active-directory-named-locations/07.png)


6. Gehen Sie auf dem Blatt **Neu** wie folgt vor:

    ![Das Blatt „Neu“](./media/active-directory-named-locations/08.png)

    a. Geben Sie im Feld **Name** einen Namen für den benannten Ort ein.

    b. Geben Sie im Feld **IP-Bereiche** einen IP-Adressbereich ein. Der IP-Adressbereich muss das Format *Classless Inter-Domain Routing* (CIDR) haben.  

    c. Klicken Sie auf **Erstellen**.



## <a name="what-you-should-know"></a>Wichtige Informationen

**Massenaktualisierungen:** Beim Erstellen oder Aktualisieren benannter Orte können Sie für eine Massenaktualisierung eine CSV-Datei mit den IP-Adressbereichen hochladen. Durch das Hochladen werden die IP-Adressbereiche in der Datei der Liste hinzugefügt, anstatt diese zu überschreiben.

![Die Links zum Hochladen und Herunterladen](./media/active-directory-named-locations/09.png)


**Einschränkungen:** Sie können maximal 60 benannte Orte definieren, wobei jedem ein IP-Adressbereich zugewiesen wird. Wenn Sie nur einen benannten Ort konfiguriert haben, können Sie dafür bis zu 500 IP-Adressbereiche definieren.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Risikoereignissen finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md).


