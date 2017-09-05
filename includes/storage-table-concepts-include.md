## <a name="what-is-table-storage"></a>Was ist Table Storage?
Mit Azure Table Storage können Sie große Mengen strukturierter Daten speichern. Der Dienst ist ein NoSQL-Datenspeicher zur Annahme authentifizierter Anrufe von innerhalb und außerhalb der Azure-Cloud. Azure-Tabellen sind hervorragend zur Speicherung strukturierter nicht relationaler Daten geeignet. Table Storage wird hauptsächlich für folgende Zwecke verwendet:

* Speicherung strukturierter Daten in TB-Größe zur Verarbeitung webbasierter Anwendungen
* Speicherung von Datensätzen, die keine komplexen Verknüpfungen, Fremdschlüssel oder gespeicherte Prozeduren erfordern und für schnellen Zugriff denormalisiert werden können
* Schnelle Datenabfrage mithilfe eines gruppierten Index
* Datenzugriff über das OData-Protokoll und LINQ-Abfragen mit WCF Date Service .NET-Bibliotheken

Sie können Table Storage verwenden, um sehr große Mengen strukturierter nicht relationaler Daten zu speichern und abzufragen und Ihre Tabellen bei steigendem Bedarf skalieren.

## <a name="table-storage-concepts"></a>Table Storage – Konzepte
Table Storage umfasst die folgenden Komponenten:

![Table Storage-Komponentendiagramm][Table1]

* **URL-Format**: Der Code adressiert Tabellen in einem Konto mithilfe dieses Adressformats:   
  http://`<storage account>`.table.core.windows.net/`<table>`  
  
  Über diese Adresse können Azure-Tabellen direkt mit dem OData-Protokoll adressiert werden. Weitere Informationen finden Sie unter [OData.org][OData.org].
* **Speicherkonto:** Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Weitere Informationen zur Kapazität der Speicherkonten finden Sie unter [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Skalierbarkeits- und Leistungsziele für Microsoft Azure-Speicher, in englischer Sprache).
* **Tabelle**: Eine Tabelle ist eine Sammlung von Entitäten. Tabellen erzwingen kein Schema für Entitäten. Das bedeutet, dass eine einzelne Tabelle Entitäten mit verschiedenen Eigenschaftensätzen enthalten kann. Die Anzahl von Tabellen, die ein Speicherkonto enthalten kann, ist nur durch die Kapazität des Speicherkontos begrenzt.
* **Entität**: Eine Entität ist ein Satz von Eigenschaften, der einer Datenbankzeile ähnelt. Eine Entität kann bis zu 1 MB groß sein.
* **Eigenschaften**: Eine Eigenschaft ist ein Name-Wert-Paar. Jede Entität kann bis zu 252 Eigenschaften zur Datenspeicherung enthalten. Jede Entität weist außerdem drei Systemeigenschaften auf, die einen Partitionsschlüssel, einen Zeilenschlüssel und einen Zeitstempel definieren. Entitäten mit demselben Partitionsschlüssel können schneller abgefragt und in atomaren Operationen eingesetzt/aktualisiert werden. Der Zeilenschlüssel einer Entität ist ihr eindeutiger Bezeichner innerhalb einer Partition.

Ausführliche Informationen zum Benennen von Tabellen und zu Eigenschaften finden Sie unter [Understanding the Table Service Data Model](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model)(Grundlegendes zum Tabellenspeicherdienst-Datenmodell).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
