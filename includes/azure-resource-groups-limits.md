| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Ressourcen pro [Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (pro Ressourcentyp) |800 |Variiert je nach Ressourcentyp |
| Bereitstellungen pro Ressourcengruppe |800 |800 |
| Ressourcen pro Bereitstellung |800 |800 |
| Managementsperren (pro eindeutigem Bereich) |20 |20 |
| Anzahl Tags (pro Ressource oder Ressourcengruppe) |15 |15 |
| Tagschlüssellänge |512 |512 |
| Tagwertlänge |256 |256 |


#### <a name="template-limits"></a>Vorlagengrenzwerte

| Wert | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Parameter |256 |256 |
| Variables |256 |256 |
| Ressourcen (einschließlich copy-Anzahl) |800 |800 |
| Ausgaben |64 |64 |
| Vorlagenausdruck |24.576 Zeichen |24.576 Zeichen |
| Ressourcen in exportierten Vorlagen |200 |200 | 
| Vorlagengröße |1 MB |1 MB |
| Parameterdateigröße |64 KB |64 KB |

Sie können einige Vorlagengrenzwerte überschreiten, indem Sie eine geschachtelte Vorlage verwenden. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](../articles/azure-resource-manager/resource-group-linked-templates.md). Um die Anzahl von Parametern, Variablen oder Ausgaben zu reduzieren, können Sie mehrere Werte in einem Objekt kombinieren. Weitere Informationen finden Sie unter [Objekte als Parameter](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).