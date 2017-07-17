---
title: Anmeldungen nach mehreren Fehlern
description: Ein Bericht, der die Benutzer ausweist, die sich nach mehreren aufeinander folgenden misslungenen Anmeldeversuchen erfolgreich angemeldet haben.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: e4ec1a39-9c20-418f-8a75-6497d0117176
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e55e0145adbdb1f41a8b8753d5555f20e96bf161
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016

---
# Anmeldungen nach mehreren Fehlern
<a id="sign-ins-after-multiple-failures" class="xliff"></a>
Dieser Bericht weist die Benutzer aus, die sich nach mehreren aufeinander folgenden misslungenen Anmeldeversuchen erfolgreich angemeldet haben. Mögliche Ursachen:

* Der Benutzer hat sein Kennwort vergessen</li><li>Der Benutzer ist Opfer eines erfolgreichen Brute-Force-Angriffs, bei dem das Kennwort erraten wurde.

Die Ergebnisse aus diesem Bericht zeigen Ihnen die Anzahl der aufeinander folgenden fehlgeschlagenen Anmeldeversuche vor der erfolgreichen Anmeldung und einen Zeitstempel zur ersten erfolgreichen Anmeldung an.

**Berichtseinstellungen**: Sie können die minimale Anzahl der aufeinanderfolgenden fehlerhaften Anmeldeversuche, die auftreten müssen, bevor es im Bericht angezeigt werden kann, konfigurieren. Beim Vornehmen von Änderungen an dieser Einstellung ist zu beachten, dass diese Änderungen nicht für vorhandene misslungene Anmeldungen gelten, die derzeit im vorhandenen Bericht enthalten sind. Sie gelten jedoch für alle künftigen Anmeldungen. Änderungen an diesem Bericht können nur von lizenzierten Administratoren vorgenommen werden.

![Anmeldungen nach mehreren Fehlern](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)


