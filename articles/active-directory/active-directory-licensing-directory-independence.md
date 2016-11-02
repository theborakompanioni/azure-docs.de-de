<properties
   pageTitle="Hinzufügen und Verwalten von mehreren Azure Active Directory-Verzeichnissen | Microsoft Azure"
   description="Anleitungen und bewährte Methoden für das Hinzufügen und Verwalten von Azure Active Directory-Verzeichnissen, wobei Verzeichnisse als vollständig unabhängige Ressource betrachtet werden"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>


# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Hinzufügen und Verwalten von mehreren Azure Active Directory-Verzeichnissen

In Azure Active Directory (Azure AD) ist jedes Verzeichnis eine vollständig unabhängige Ressource: gleichgestellt, voll funktionsfähig und logisch unabhängig von anderen Verzeichnissen, die Sie verwalten. Es gibt keine unter- und übergeordneten Beziehungen zwischen den Verzeichnissen. Diese Unabhängigkeit zwischen den Verzeichnissen beinhaltet Ressourcen-, Verwaltungs- und Synchronisierungsunabhängigkeit.

##<a name="resource-independence"></a>Ressourcenunabhängigkeit

Wenn Sie eine Ressource in einem Verzeichnis erstellen oder löschen, hat dies keine Auswirkungen auf Ressourcen in einem anderen Verzeichnis. Eine teilweise geltende Ausnahme bilden externe Benutzer, wie unten beschrieben. Wenn Sie eine benutzerdefinierte Domäne "contoso.com" in einem Verzeichnis verwenden, kann sie in keinem anderen Verzeichnis verwendet werden.

##<a name="administrative-independence"></a>Verwaltungsunabhängigkeit

Wenn ein Benutzer ohne Administratorrechte aus dem Verzeichnis „Contoso“ ein Testverzeichnis „Test“ erstellt, geschieht Folgendes:
- Standardmäßig wird der Benutzer, der ein Verzeichnis erstellt, als externer Benutzer in diesem neuen Verzeichnis hinzugefügt, und ihm wird ihm die globale Administratorrolle in diesem Verzeichnis zugewiesen.
- Die Administratoren für das Verzeichnis „Contoso“ haben keine direkten Administratorberechtigungen für das Verzeichnis „Test“, sofern ihnen nicht ein Administrator diese Verzeichnisses die Berechtigungen explizit erteilt. Die Administratoren von „Contoso“ können den Zugriff auf das Verzeichnis „Test“ steuern, wenn sie das Benutzerkonto steuern, mit dem dieses Verzeichnis erstellt wurde.
- Wenn Sie eine Administratorrolle für einen Benutzer in einem Verzeichnis ändern (hinzufügen oder entfernen), hat die Änderung keine Auswirkungen auf eine Administratorrolle, die der Benutzer möglicherweise in einem anderen Verzeichnis hat.

##<a name="synchronization-independence"></a>Synchronisierungsunabhängigkeit

Sie können jedes Azure AD-Verzeichnis unabhängig voneinander konfigurieren, um Daten mit einer Instanz einer der folgenden Komponenten zu synchronisieren:
  - Dem Tool für die Verzeichnissynchronisierung (DirSync), um Daten mit einer AD-Gesamtstruktur zu synchronisieren.
  - Dem Azure Active Directory Connector für Forefront Identity Manager, um Daten mit mindestens einer lokalen Gesamtstruktur und/oder anderen Datenquellen als Azure AD zu synchronisieren.

##<a name="add-an-azure-ad-directory"></a>Hinzufügen eines Azure AD-Verzeichnisses

Um ein Azure AD-Verzeichnis im klassischen Azure-Portal hinzuzufügen, wählen Sie die Active Directory-Erweiterung auf der linken Seite aus, und tippen Sie auf **Hinzufügen**.

> [AZURE.NOTE]   Im Gegensatz zu anderen Azure-Ressourcen sind Ihre Verzeichnisse keine untergeordneten Ressourcen eines Azure-Abonnements. Wenn Sie Ihr Azure-Abonnement kündigen oder ablaufen lassen, können Sie weiterhin mithilfe von Azure PowerShell, der Azure Graph-API oder anderen Schnittstellen wie Office 365 Admin Center auf Ihre Verzeichnisdaten zugreifen. Sie können dem Verzeichnis auch ein anderes Abonnement zuordnen.

Eine grobe Übersicht über Lizenzierungsprobleme und bewährte Methoden im Zusammenhang mit Azure AD finden Sie unter [Was ist Azure Active Directory-Lizenzierung?](active-directory-licensing-what-is.md).



<!--HONumber=Oct16_HO2-->


