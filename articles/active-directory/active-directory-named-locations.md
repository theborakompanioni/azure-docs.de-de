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
ms.date: 05/10/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 9637c56cf88ccf4c54fda789d4e7e3ca3e0a4fed
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="named-locations-in-azure-active-directory"></a>Benannte Orte in Azure Active Directory

Benannte Speicherplätze ist ein Feature von Azure Active Directory, das Ihnen ermöglicht, in Ihrer Organisation vertrauenswürdige IP-Adressbereiche zu bezeichnen. In Ihrer Umgebung können Sie Benannte Orte im Kontext der Erkennung von [Risikoereignissen](active-directory-reporting-risk-events.md) verwenden, um die Anzahl der gemeldeten falsch positiven Ergebnisse für den Risikoereignistyp *Unmöglicher Ortswechsel zu atypischen Orten* zu reduzieren. 




## <a name="configuration"></a>Konfiguration

**So konfigurieren Sie einen benannten Ort**

1. Melden Sie sich als globaler Administrator beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im linken Bereich auf **Azure Active Directory**.

    ![Benannte Orte](./media/active-directory-named-locations/01.png)

3. Klicken Sie auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

    ![Benannte Orte](./media/active-directory-named-locations/05.png)


4. Klicken Sie auf dem Blatt **Azure Active Directory** im Abschnitt **Verwalten** auf **Benannte Orte**.

    ![Benannte Orte](./media/active-directory-named-locations/06.png)


5. Klicken Sie auf dem Blatt **Benannte Orte** im Menü am oberen Rand auf **Neue Orte**.

    ![Benannte Orte](./media/active-directory-named-locations/07.png)


6. Führen Sie auf dem Blatt **Neu** die folgenden Schritte aus:

    ![Benannte Orte](./media/active-directory-named-locations/08.png)

    a. Geben Sie in das Textfeld **Name** einen Namen für den benannten Ort ein.

    b. Geben Sie im Textfeld **IP-Bereich** einen IP-Bereich ein. Der IP-Adressbereich muss das Format *Classless Inter-Domain Routing* (CIDR) haben.  

    c. Klicken Sie auf **Erstellen**.



## <a name="what-you-should-know"></a>Wichtige Informationen

**Massenaktualisierungen**: Beim Erstellen oder Aktualisieren benannter Orte können Sie für eine Massenaktualisierung eine CSV-Datei mit den IP-Adressbereichen hochladen. Durch das Hochladen werden die IP-Adressbereiche in der Datei der Liste hinzugefügt, anstatt diese zu überschreiben.

![Benannte Orte](./media/active-directory-named-locations/09.png)


**Einschränkungen**: Sie können maximal 60 benannte Orte definieren, wobei jedem ein IP-Adressbereich zugewiesen wird. Wenn Sie nur einen benannten Ort konfiguriert haben, können Sie dafür bis zu 500 IP-Adressbereiche definieren.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- Informationen zu Risikoereignissen finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md).


