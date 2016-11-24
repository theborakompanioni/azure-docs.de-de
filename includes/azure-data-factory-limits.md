Data Factory ist ein mehrinstanzenfähiger Dienst mit den folgenden Standardlimits, um sicherzustellen, dass Kundenabonnements vor anderen Arbeitslasten geschützt sind. Viele der Limits können problemlos für Ihr Abonnement bis zum maximalen Limit angehoben werden, indem Sie sich an den Support wenden. 

| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Data Factorys in einem Azure-Abonnement |50 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pipelines innerhalb einer Data Factory |2500 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Datasets innerhalb einer Data Factory |5.000 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Gleichzeitige Slices pro Dataset |10 |10 |
| Bytes pro Objekt für Pipelineobjekte <sup>1</sup> |200 KB |2000 KB |
| Bytes pro Objekt für Datasetobjekte und Objekte für verknüpfte Dienste <sup>1</sup> |100 KB |2000 KB |
| Bedarfsgesteuerte HDInsight-Clusterkerne innerhalb eines Abonnements <sup>2</sup> |48 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Einheit für Clouddatenverschiebungen <sup>3</sup> |8 |[Support kontaktieren](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Anzahl der Wiederholungsversuche für Ausführungen von Pipelineaktivitäten |1000 |MaxInt (32 Bit) |

<sup>1</sup> Pipeline-, Dataset- und verknüpfte Dienstobjekte stellen eine logische Gruppierung Ihrer Workload dar. Limits für diese Objekte beziehen sich nicht auf die Datenmenge, die Sie mit dem Azure Data Factory-Dienst verschieben und verarbeiten können. Data Factory kann so skaliert werden, dass Petabytes an Daten verarbeitet werden können.

<sup>2</sup> Bedarfsgesteuerte HDInsight-Kerne werden über das Abonnement zugewiesen, das die Data Factory enthält. Daher ist das obige Limit das von der Data Factory erzwungene Kernlimit für bedarfsgesteuerte HDInsight-Kerne und unterscheidet sich vom Kernlimit, das für Ihr Azure-Abonnement gilt.

<sup>3</sup> Die Einheit für Clouddatenverschiebungen wird in einem Cloud-zu-Cloud-Kopiervorgang verwendet. Sie ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Data Factory. Durch die Nutzung von weitere DMUs für einige Szenarios können Sie einen höheren Durchsatz erreichen. Weitere Informationen finden Sie unter [Einheiten für Clouddatenverschiebungen](../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units).

| **Ressource** | **Unteres Standardlimit** | **Minimales Limit** |
| --- | --- | --- |
| Planungsintervall |15 Minuten |15 Minuten |
| Intervall zwischen Wiederholungsversuchen |1 Sekunde |1 Sekunde |
| Timeoutwert für Wiederholungsversuche |1 Sekunde |1 Sekunde |

### <a name="web-service-call-limits"></a>Limits für Webdienstaufrufe
Azure Resource Manager weist Grenzwerte für API-Aufrufe auf. Sie können API-Aufrufe mit einer Rate innerhalb der [API-Limits von Azure Resource Manager](../articles/azure-subscription-service-limits.md#resource-group-limits)ausführen. 



<!--HONumber=Nov16_HO3-->


