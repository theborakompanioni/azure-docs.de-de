---
title: "Erläuterungen zur Azure Active Directory-Vorschau | Microsoft Docs"
description: "Ein Thema, in dem die Unterschiede zwischen Azure Active Directory im klassischen Portal und der Azure Active Directory-Vorschau im Azure-Portal erläutert werden."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e717c11c-e7c9-4565-8e47-1950905e5b3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 664abe5887b3d96eb48e3b47df9a32648ee623dd


---
# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Vorschau der Azure Active Directory-Verwaltungsoberfläche im Azure-Portal
Die Azure Active Directory-Verwaltungsoberfläche (Azure AD) im Azure-Portal befindet sich in der Vorschau. Sie können dieses Feature ausprobieren, indem Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator Ihres Verzeichnisses anmelden. Wählen Sie anschließend in der Liste der Dienste „Azure Active Directory“ aus, oder wählen Sie **Weitere Dienste** aus, um eine Liste aller Dienste anzuzeigen. Sie benötigen kein Azure-Abonnement, um die Azure AD-Verwaltungsoberfläche im Azure-Portal zu verwenden.

## <a name="learn-about-what-you-can-do-in-the-preview-experience"></a>Lernen Sie Ihre Möglichkeiten in der Vorschauoberfläche kennen
Die Vorschauoberfläche ermöglicht Ihnen das Verwalten vieler Verzeichnisressourcen im Azure-Portal, darunter beispielsweise Benutzer, Gruppen, Anwendungen und Verzeichniseinstellungen. Wir arbeiten an der Verbesserung dieser Oberfläche, um alle Funktionen der Azure AD-Verwaltungsoberfläche im [klassischen Azure-Portal](https://manage.windowsazure.com)bereitzustellen. Bis dies erreicht ist, müssen einige der Aufgaben im Rahmen der Verzeichnisverwaltung weiterhin im klassischen Portal ausgeführt werden.

## <a name="manage-the-same-azure-ad-tenants"></a>Verwaltung derselben Azure AD-Mandanten
Die Vorschauoberfläche führt Lese- und Schreibvorgänge für denselben Azure Active Directory-Mandanten durch wie das klassische Portal und das Office 365 Admin Center. Änderungen, die in einem dieser Portale durchgeführt werden, werden in allen anderen Portalen widergespiegelt.

## <a name="use-the-same-authorization-logic"></a>Verwendung derselben Autorisierungslogik
Die Vorschauoberfläche verwendet dieselbe Autorisierungslogik wie die vorhandenen Active Directory-Clients. Benutzer sind basierend auf ihrer Verzeichnisrolle – z.B. globaler Administrator, Benutzeradministrator und Kennwortadministrator – dazu autorisiert, Änderungen an Verzeichnisressourcen vorzunehmen. Eine Rolle für Azure-Ressourcen oder ein Azure-Abonnement autorisiert einen Benutzer nicht, Verzeichnisressourcen zu verwalten. Weitere Informationen zu Azure AD-Verwaltungsrollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md). 

Die Vorschauoberfläche ist für globale Administratoren optimiert. Wenn Sie die Vorschauoberfläche verwenden, während Sie als ein Benutzer angemeldet sind, der nicht als globaler Administrator konfiguriert ist, ist das Benutzererlebnis möglicherweise beeinträchtigt. Zum Beispiel können Sie möglicherweise auf eine Schaltfläche zum Starten einer Aufgabe klicken, die Sie nicht im Verzeichnis ausführen können. Dieses Verhalten wird bald verbessert.

## <a name="tell-us-what-you-think"></a>Sagen Sie uns Ihre Meinung
Sie können uns im Verwaltungsportalbereich des [Azure AD-Feedbackforums](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc)Feedback zur Vorschauoberfläche geben.




<!--HONumber=Nov16_HO3-->


