---
title: Verwalten eines Azure Automation-Kontos | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie die Konfiguration Ihres Automation-Kontos verwalten, z.B. in Bezug auf die Erneuerung, Löschung und Fehlkonfiguration von Zertifikaten."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 41efdbcacede74bac038342688362ff480cadc7e
ms.lasthandoff: 04/15/2017

---

# <a name="manage-azure-automation-account"></a>Verwalten eines Azure Automation-Kontos
Da Ihr Automation-Konto irgendwann abläuft, müssen Sie das Zertifikat erneuern. Wenn Sie der Meinung sind, dass das ausführende Konto kompromittiert wurde, können Sie es löschen und neu erstellen. In diesem Abschnitt wird beschrieben, wie Sie dabei vorgehen.

## <a name="self-signed-certificate-renewal"></a>Erneuerung eines selbstsignierten Zertifikats
Das selbstsignierte Zertifikat, das Sie für das ausführende Konto erstellt haben, läuft ein Jahr nach dem Erstellungsdatum ab. Sie können es vor dem Ablaufdatum jederzeit erneuern. Beim Erneuern wird das aktuelle gültige Zertifikat beibehalten, um sicherzustellen, dass sich keine negativen Auswirkungen auf Runbooks ergeben, die sich in der Warteschlange befinden oder aktiv ausgeführt werden und die mit dem ausführenden Konto authentifiziert werden. Das Zertifikat bleibt bis zum Ablaufdatum gültig.

> [!NOTE]
> Wenn Sie Ihr ausführendes Automation-Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und diese Option verwenden, wird das Unternehmenszertifikat durch ein selbstsigniertes Zertifikat ersetzt.

Führen Sie folgende Schritte aus, um das Zertifikat zu erneuern:

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie auf dem Blatt **Automation-Konto** im Bereich **Kontoeigenschaften** unter **Kontoeinstellungen** die Option **Ausführende Konten**.

    ![Eigenschaftenbereich des Automation-Kontos](media/automation-manage-account/automation-account-properties-pane.png)
3. Wählen Sie auf dem Blatt mit den Eigenschaft für **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten.

4. Klicken Sie auf dem Blatt **Eigenschaften** für das ausgewählte Konto auf **Zertifikat erneuern**.

    ![Erneuern des Zertifikats für das ausführende Konto](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos
In diesem Abschnitt wird beschrieben, wie Sie ein ausführendes Konto oder klassisches ausführendes Konto löschen und neu erstellen. Bei dieser Aktion wird das Automation-Konto beibehalten. Nach dem Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos können Sie es im Azure-Portal neu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie auf dem Blatt **Automation-Konto** im Bereich mit den Kontoeigenschaften die Option **Ausführende Konten**.

3. Wählen Sie auf dem Blatt mit den Eigenschaft für **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten. Klicken Sie anschließend auf dem Blatt **Eigenschaften** für das ausgewählte Konto auf **Löschen**.

 ![Löschen des ausführenden Azure-Kontos](media/automation-manage-account/automation-account-delete-runas.png)

4. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

5. Nach dem Löschen des Kontos können Sie das Konto neu erstellen, indem Sie auf dem Eigenschaftenblatt **Ausführende Konten** die Erstellungsoption für **Ausführendes Azure-Konto** auswählen.

 ![Neuerstellen des ausführenden Automation-Kontos](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Fehlkonfiguration
Einige Konfigurationselemente, die erforderlich sind, damit das ausführende Konto bzw. das klassische ausführende Konto richtig funktioniert, wurden ggf. gelöscht oder bei der anfänglichen Einrichtung nicht richtig erstellt. Beispiele für diese Elemente sind:

* Zertifikatasset
* Verbindungsasset
* Entfernung des ausführenden Kontos aus der Rolle „Mitwirkender“
* Dienstprinzipal oder Anwendung in Azure AD

In den obigen und anderen Fällen einer Fehlkonfiguration erkennt das Automation-Konto die Änderungen und zeigt auf dem Eigenschaftenblatt **Ausführende Konten** des Kontos den Status *Unvollständig* an.

![Konfigurationsstatus „Unvollständig“ für ausführendes Konto](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Wenn Sie das ausführende Konto auswählen, wird im Bereich **Eigenschaften** die folgende Fehlermeldung angezeigt:

![Warnmeldung mit Hinweis auf eine unvollständige Konfiguration des ausführenden Kontos](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

Sie können diese Probleme mit ausführenden Konten schnell beheben, indem Sie das Konto löschen und neu erstellen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md).
* Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure Automation finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
