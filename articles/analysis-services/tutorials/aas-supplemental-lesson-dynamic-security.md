---
title: "Ergänzende Lektion zum Azure Analysis Services-Tutorial – Dynamische Sicherheit | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie die dynamische Sicherheit mit Zeilenfiltern im Tutorial zu Azure Analysis Services verwenden können."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: cd74b0cb0d58036cc7b1198a58649ba38e386322
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="supplemental-lesson---dynamic-security"></a>Ergänzende Lektion – Dynamische Sicherheit

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser ergänzenden Lektion erstellen Sie eine zusätzliche Rolle, die dynamische Sicherheit implementiert. Die dynamische Sicherheit bietet Sicherheit auf Ebene der Zeilen auf Grundlage des Benutzernamens oder der Anmelde-ID des aktuell angemeldeten Benutzers. 
  
Fügen Sie Ihrem Modell eine Tabelle hinzu, die die Benutzernamen der Benutzer enthält, die eine Verbindung mit dem Modell herstellen, und die Modellobjekte und -daten durchsuchen, um die dynamische Sicherheit zu implementieren. Das Modell, dass Sie in diesem Tutorial erstellen, steht im Zusammenhang mit Adventure Works. Sie müssen allerdings eine Tabelle mit Benutzern aus Ihrer eigenen Domäne hinzufügen, um diese Lektion abschließen zu können. Die Kennwörter für die hinzugefügten Benutzernamen benötigen Sie nicht. Verwenden Sie die Einfügen-Funktion, um die Tabelle „EmployeeSecurity“ mit einer kleinen Benutzerauswahl aus Ihrer eigenen Domäne zu erstellen. Fügen Sie die Mitarbeiterdaten aus einer Excel-Tabelle ein. In einem realistischen Szenario wäre die Tabelle mit den Benutzernamen normalerweise eine Tabelle aus einer tatsächlichen Datenquelle, also z.B. aus einer vorhandenen DimEmployee-Tabelle.  
  
Zur Implementierung von dynamischer Sicherheit verwenden Sie zwei DAX-Funktionen: [die Funktion USERNAME (DAX)](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) und [die Funktion LOOKUPVALUE (DAX)](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Diese in einem Zeilenfilter angewandten Funktionen werden in einer neuen Rolle definiert. Mit der LOOKUPVALUE-Funktion gibt die Formel einen Wert aus der Tabelle „EmployeeSecurity“ an. Die Formel übergibt diesen Wert dann an die USERNAME-Funktion, die angibt, dass der Benutzername des angemeldeten Benutzers zu dieser Rolle gehört. Anschließend kann der Benutzer nur noch Daten durchsuchen, die vom Zeilenfilter der Rolle angegeben werden. In diesem Szenario geben Sie an, dass die Vertriebsmitarbeiter nur die Daten zu Internetverkäufen Ihres eigenen Gebiets durchsuchen können.  
  
Die Aufgaben, die sich auf dieses spezielle Szenario des tabellarischen Adventure Works-Modells beziehen, aber nicht unbedingt auf realistische Szenarios anwendbar sind, sind als solche gekennzeichnet. Jede Aufgabe enthält Zusatzinformationen zum Zweck der Aufgabe.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **30 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie diese ergänzende Lektion beginnen, sollten Sie alle vorherigen Lektionen abgeschlossen haben.  
  
## <a name="add-the-dimsalesterritory-table-to-the-aw-internet-sales-tabular-model-project"></a>Fügen Sie die Tabelle „DimSalesTerritory“ in das tabellarische Modellprojekt „AW Internet Sales“ ein.  
Sie müssen zwei weitere Tabellen in Ihr Modell einfügen, um die dynamische Sicherheit für dieses Adventure Works-Szenario zu implementieren. Fügen Sie zunächst „DimSalesTerritory“ (als Vertriebsgebiet) aus der gleichen AdventureWorksDW-Datenbank ein. Später wenden Sie einen Zeilenfilter auf die Tabelle „SalesTerritory“ an, der die Daten definiert, die der angemeldete Benutzer durchsuchen kann.  
  
#### <a name="to-add-the-dimsalesterritory-table"></a>So fügen Sie die Tabelle „DimSalesTerritory“ hinzu  
  
1.  Erweitern Sie im tabellarischen Modell-Explorer **Datenquellen**, und klicken Sie anschließend auf Ihre Verbindung und dann auf **Neue Tabelle importieren**.  

    Wenn das Dialogfeld „Identitätswechsel-Anmeldeinformationen“ angezeigt wird, geben Sie die Anmeldeinformationen zum Identitätswechsel aus Lektion 2: Hinzufügen von Daten ein.
  
2.  Wählen Sie im Navigator die Tabelle **DimSalesTerritory** aus, und klicken Sie anschließend auf **OK**.    
  
3.  Klicken Sie im Abfrage-Editor auf die Abfrage **DimSalesTerritory**, und entfernen Sie dann die Spalte **SalesTerritoryAlternateKey**.  
  
7.  Klicken Sie auf **Importieren**.  
  
    Die neue Tabelle wird im Arbeitsbereich des Modells hinzugefügt. Dann werden Objekte und Modelle aus der Quelltabelle „DimSalesTerritory“ in Ihr tabellarisches Modell „AW Internet Sales“ importiert.  
  
9. Nachdem die Tabelle erfolgreich importiert wurde, klicken Sie auf **Schließen**.  

## <a name="add-a-table-with-user-name-data"></a>Hinzufügen einer Tabelle mit Benutzernamendaten  
Die Tabelle „DimEmployee“ in der Beispieldatenbank „AdventureWorksDW“ enthält Benutzer in der Domäne „AdventureWorks“. Diese Benutzernamen sind in Ihrer Umgebung nicht vorhanden. Sie müssen eine Tabelle in Ihrem Modell erstellen, die einen kleinen Teil (mindestens drei) der tatsächlichen Benutzer in Ihrer Organisation enthält. Dann können Sie diese Benutzer als Member der neuen Rolle eintragen. Sie benötigen nicht die Kennwörter der Beispielbenutzernamen, aber Sie benötigen tatsächliche Windows-Benutzernamen aus Ihrer Domäne.  
  
#### <a name="to-add-an-employeesecurity-table"></a>So fügen Sie eine Tabelle „EmployeeSecurity“ hinzu  
  
1.  Öffnen Sie Microsoft Excel, und erstellen Sie ein Arbeitsblatt.  
  
2.  Kopieren Sie die folgende Tabelle mitsamt der Überschriftenzeile, und fügen Sie diese auf dem Arbeitsblatt ein.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Ersetzen Sie den Vornamen, Nachnamen und den Domänen-/Benutzernamen durch die Namen und Anmelde-IDs von drei Benutzern in Ihrer Organisation. Geben Sie den gleichen Benutzer in den ersten beiden Zeilen bei „EmployeeId 1“ ein, um zu zeigen, dass dieser Benutzer mehr als einem Vertriebsgebiet angehört. Belassen Sie die Felder „EmployeeId“ und „SalesTerritoryId“ wie sie sind.  
  
4.  Speichern Sie das Arbeitsblatt unter **SampleEmployee**.  
  
5.  Wählen Sie auf dem Arbeitsblatt alle Zellen mit Angestelltendaten aus, einschließlich des Headers, und klicken Sie dann mit der rechten Maustaste auf die ausgewählten Daten und anschließend auf **Kopieren**.  
  
6.  Klicken Sie in SSDT auf das Menü **Bearbeiten** und anschließend auf **Einfügen**.  
  
    Wenn „Einfügen“ ausgegraut ist, klicken Sie auf eine beliebige Spalte in einer beliebigen Tabelle im Modell-Designerfenster, und versuchen Sie es erneut.  
  
7.  Geben Sie im Dialogfeld **Vorschau der einzufügenden Dateien** unter **Tabellenname** **EmployeeSecurity** ein.  
  
8.  Überprüfen Sie unter **Einzufügende Daten**, dass die Daten auch alle Benutzerdaten und Header des Arbeitsblatts „SampleEmployee“ einschließen.  
  
9. Überprüfen Sie, ob **Erste Zeile als Spaltenüberschriften verwenden** aktiviert ist, und klicken Sie dann auf **OK**.  
  
    Es wird eine neue Tabelle mit dem Namen „EmployeeSecurity“ mit aus dem Arbeitsblatt „SampleEmployee“ kopierten Angestelltendaten erstellt.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>Erstellen von Beziehungen zwischen den Tabellen „FactInternetSales“, „DimGeography“ und „DimSalesTerritory“  
Die Tabellen „FactInternetSales“, „DimGeography“ und „DimSalesTerritory“ haben eine Spalte gemeinsam, und zwar „SalesTerritoryId“. Die Spalte „SalesTerritoryId“ in der Tabelle „DimSalesTerritory“ enthält Werte mit einer anderen ID für jedes Vertriebsgebiet.  
  
#### <a name="to-create-relationships-between-the-factinternetsales-dimgeography-and-the-dimsalesterritory-table"></a>So erstellen Sie Beziehungen zwischen den Tabellen „FactInternetSales“, „DimGeography“ und „DimSalesTerritory“  
  
1.  Klicken Sie in der Diagrammansicht in der Tabelle **DimGeography** auf die Spalte **SalesTerritoryId**, halten Sie diese, und ziehen Sie den Mauszeiger zur Spalte **SalesTerritoryId** in der Tabelle **DimSalesTerritory**. Lassen Sie nun die Maustaste wieder los.  
  
2.  Klicken Sie in der Tabelle **FactInternSales** auf die Spalte **SalesTerritoryId**, halten Sie diese, und ziehen Sie den Mauszeiger zur Spalte **SalesTerritoryId** in der Tabelle **DimSalesTerritory**. Lassen Sie nun die Maustaste wieder los.  
  
    Beachten Sie, dass die Active-Eigenschaft für diese Beziehung den Wert „False“ aufweist. Dies bedeutet, dass sie nicht aktiv ist. Die Tabelle „FactInternetSales“ verfügt bereits über eine andere aktive Beziehung.  
  
## <a name="hide-the-employeesecurity-table-from-client-applications"></a>Ausblenden der Tabelle „EmployeeSecurity“ aus Client-Anwendungen  
In dieser Aufgabe blenden Sie die Tabelle „EmployeeSecurity“ aus, sodass sie nicht in der Feldliste einer Client-Anwendung angezeigt wird. Denken Sie daran, dass eine Tabelle durch die Ausblendung nicht geschützt wird. Benutzer können trotzdem Daten der Tabelle „EmployeeSecurity“ abfragen, sie müssen nur wissen wie. In einer späteren Aufgabe fügen Sie einen Filter hinzu, der Benutzer davon abhält, alle Daten der Tabelle „EmployeeSecurity“ abzufragen. Damit können Sie die Tabelle schützen.  
  
#### <a name="to-hide-the-employeesecurity-table-from-client-applications"></a>So blenden Sie die Tabelle „EmployeeSecurity“ aus Client-Anwendungen aus  
  
-   Klicken Sie im Modell-Designer in der Diagrammansicht mit der rechten Maustaste auf die Überschrift der Tabelle **Employee**, und klicken Sie dann auf **Aus Clienttools ausblenden**.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Erstellen Sie die Benutzerrolle „Sales Employees by Territory“ (Vertriebsmitarbeiter nach Gebiet)  
In dieser Aufgabe erstellen Sie eine Benutzerrolle. Diese Rolle beinhaltet einen neuen Zeilenfilter, der definiert, welche Zeilen der Tabelle „DimSalesTerritory“ die Benutzer sehen können. Der Filter wird dann nach der „1:n-Beziehung“ auf alle anderen Tabellen, die mit „DimSalesTerritory“ in Verbindung stehen, angewendet. Außerdem wenden Sie einen Filter an, der die gesamte Tabelle „EmployeeSecurity“ davor schützt, von jedem Benutzer dieser Rolle abgerufen zu werden.  
  
> [!NOTE]  
> Die Rolle „Sales Employees by Territory“, die Sie in dieser Lektion erstellen, bewirkt, dass Member nur Verkaufsdaten Ihres eigenen Gebiets durchsuchen oder abfragen können. Wenn Sie einen Benutzer als Member der Rolle „Sales Employees by Territory“ hinzufügen, der auch Member einer in [Lektion 11: Erstellen von Rollen](../tutorials/aas-lesson-11-create-roles.md) erstellten Rolle ist, erhalten Sie eine Kombination der Berechtigungen. Wenn ein Benutzer ein Member mehrerer Rollen ist, sind die Berechtigungen und Zeilenfilter jeder Rolle kumulativ. Das heißt, dass der Benutzer durch die Kombination von Rollen höhere Berechtigungen hat.  
  
#### <a name="to-create-a-sales-employees-by-territory-user-role"></a>So erstellen Sie die Benutzerrolle „Sales Employees by Territory“  
  
1.  Klicken Sie in SSDT auf das Menü **Modell**, und klicken Sie dann auf **Rollen**.  
  
2.  Klicken Sie im **Rollen-Manager** auf **Neu**.  
  
    Der Liste wird eine neue Rolle mit der Berechtigung „None“ hinzugefügt.  
  
3.  Klicken Sie auf die neue Rolle und anschließend auf die Spalte **Name**, und benennen Sie die Rolle in **Sales Employees by Territory** um.  
  
4.  Klicken sie in der Spalte **Berechtigungen** auf die Dropdownliste, und wählen Sie anschließend die Berechtigung **Lesen** aus.  
  
5.  Optional: Klicken Sie auf die Registerkarte **Member** und anschließend auf **Hinzufügen**.  
  
6.  Geben Sie im Dialogfeld **Benutzer oder Gruppe auswählen** unter **Enter the object named to select** (Auszuwählendes benanntes Objekt eingeben) den ersten Beispielbenutzernamen ein, den Sie beim Erstellen der Tabelle „EmployeeSecurity“ verwendet haben. Klicken Sie auf **Namen prüfen**, um zu prüfen, ob der Name zulässig ist, und klicken Sie anschließend auf **OK**.  
  
    Wiederholen Sie diesen Schritt, und fügen Sie den anderen Beispielbenutzernamen hinzu, den Sie beim Erstellen der Tabelle „EmployeeSecurity“ verwendet haben.  
  
7.  Klicken Sie auf die Registerkarte **Zeilenfilter**.  
  
8.  Geben Sie für die Tabelle **EmployeeSecurity** in der Spalte **DAX-Filter** folgende Formel ein:  
  
    ```
      =FALSE()  
    ```
  
    Diese Formel gibt an, dass alle Spalten zur booleschen Bedingung „false“ aufgelöst werden. Es können keine Spalten der Tabelle „EmployeeSecurity“ von einem Mitglied der Benutzerrolle „Sales Employees by Territory“ abgefragt werden.  
  
9. Geben Sie für die Tabelle **DimSalesTerritory** folgende Formel ein:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    In dieser Formel gibt die Funktion LOOKUPVALUE alle Werte der Spalte „DimEmployeeSecurity[SalesTerritoryId]“ zurück, bei denen „EmployeeSecurity[LoginId]“ mit dem aktuell angemeldeten Windows-Benutzernamen übereinstimmt, und wo „EmployeeSecurity[SalesTerritoryId]“ mit „DimSalesTerritory[SalesTerritoryId]“ übereinstimmt.  
  
    Die von LOOKUPVALUE zurückgegebenen Territory-IDs werden dann verwendet, um die in der Tabelle „DimSalesTerritory“ gezeigten Zeilen einzuschränken. Es werden nur Zeilen angezeigt, deren „SalesTerritoryID“ unter den von LOOKUPVALUE zurückgegebenen IDs ist.  
  
10. Klicken Sie im Rollen-Manager auf **OK**.  
  
## <a name="test-the-sales-employees-by-territory-user-role"></a>Testen der Benutzerrolle „Sales Employees by Territory“  
In dieser Aufgabe verwenden Sie die Funktion „Analysieren in Excel“ von SSDT, um die Wirksamkeit der Benutzerrolle „Sales Employees by Territory“ zu testen. Geben Sie einen der Benutzernamen an, die in die Tabelle „EmployeeSecurity“ und als Member der Rolle eingefügt wurden. Dieser Benutzername wird dann als effektiver Benutzername in der Verbindung zwischen Excel und dem Modell verwendet.  
  
#### <a name="to-test-the-sales-employees-by-territory-user-role"></a>So testen Sie die Benutzerrolle „Sales Employees by Territory“  
  
1.  Klicken Sie in SSDT auf das Menü **Modell**, und klicken Sie dann auf **In Excel analysieren**.  
  
2.  Wählen Sie im Dialogfeld **In Excel analysieren** unter **Geben Sie den Benutzernamen oder die Rolle für die Verbindung mit dem Modell** an **Andere Windows-Benutzer** aus, und klicken Sie auf **Durchsuchen**.  
  
3.  Geben Sie im Dialogfeld **Benutzer oder Gruppe auswählen** unter **Namen des auszuwählenden Objekts eingeben** einen Benutzernamen ein, den Sie in die Tabelle „EmployeeSecurity“ einbezogen haben, und klicken Sie auf **Namen überprüfen**.  
  
4.  Klicken Sie auf **OK**, um das Dialogfeld **Benutzer oder Gruppe auswählen** zu schließen, und klicken Sie anschließend auf **OK**, um das Dialogfeld **In Excel analysieren** zu schließen.  
  
    Excel wird mit einer neuen Arbeitsmappe geöffnet. Eine PivotTable wird automatisch erstellt. Die PivotTable-Feldliste enthält die meisten Datenfelder, die im neuen Modell verfügbar sind.  
  
    Beachten Sie, dass die Tabelle „EmployeeSecurity“ in der Liste „PivotTable-Felder“ nicht angezeigt wird. Sie haben diese Tabelle in einer vorherigen Aufgabe in den Clienttools ausgeblendet.  
  
5.  Wählen Sie in der Liste **Felder** unter **∑ Internet Sales** (Measures) das Measure **InternetTotalSales** aus. Das Measure wird im Feld **Values** eingegeben.  
  
6.  Wählen Sie die Spalte **SalesTerritoryId** aus der Tabelle **DimSalesTerritory**. Die Spalte wird in den Feldern **Zeilenbeschriftung** eingegeben.  
  
    Beachten Sie, dass Zahlen zum Internetverkauf nur für die Region angezeigt werden, der der effektive Benutzername, den Sie verwendet haben, angehört. Wenn Sie eine andere Spalte in der Tabelle „DimGeography“ als Zeilenbezeichnungsfeld auswählen, z.B. „City“, werden nur Städte des Verkaufsgebiets angezeigt, dem der effektive Benutzer angehört.  
  
    Dieser Benutzer kann keine Internetverkaufsdaten eines anderen Gebiets durchsuchen oder abfragen, dem er nicht angehört. Diese Einschränkung wird vom Zeilenfilter verursacht, der für die Tabelle „DimSalesTerritory“ in der Benutzerrolle „Sales Employees by Territory“ definiert wurde. Dieser Filter schützt Daten für alle Daten, die mit anderen Verkaufsgebieten in Verbindung stehen.  
  
## <a name="see-also"></a>Weitere Informationen  
[USERNAME Function (DAX) (Die Funktion USERNAME (DAX))](https://msdn.microsoft.com/library/hh230954.aspx)  
[LOOKUPVALUE Function (DAX) (Die Funktion LOOKUPVALUE (DAX))](https://msdn.microsoft.com/library/gg492170.aspx)  
[CUSTOMDATA Function (DAX) (Die Funktion CUSTOMDATA (DAX))](https://msdn.microsoft.com/library/hh213140.aspx)  
