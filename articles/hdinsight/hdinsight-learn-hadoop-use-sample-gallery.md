<properties
   pageTitle="Kennenlernen von Hadoop in HDInsight mithilfe der Beispielgalerie | Microsoft Azure"
   description="Lernen Sie Hadoop schnell durch das Ausführen von Beispielanwendungen aus der HDInsight-Galerie für erste Schritte kennen. Verwenden Sie Beispieldaten, oder stellen Sie eigene Beispieldaten bereit."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.workload="big-data"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="07/25/2016"
   ms.author="jgao"/>

# Kennenlernen von Hadoop mithilfe der Azure HDInsight-Galerie für erste Schritte

Der Erste-Schritte-Katalog ist nur für Windows-basierte HDInsight-Cluster verfügbar. Der Katalog bietet eine schnelle und einfache Möglichkeit, Hadoop durch das Ausführen von Beispielanwendungen in HDInsight kennenzulernen. Einige Beispiele verfügen über Beispieldaten. Für die restlichen Beispiele können Sie eigene Daten bereitstellen. Derzeit sind die folgenden sechs Beispiele verfügbar (weitere folgen):

- Lösungen mit Ihren Azure-Daten
	- Microsoft Azure Website-Protokollanalyse
	- Microsoft Azure-Speicheranalyse
- Lösungen mit Beispieldaten
	- Analysieren von Sensordaten
	- Twitter-Trendanalyse
	- Analysieren von Website-Protokollen
	- Mahout-Filmempfehlung

![Lösungen für HDInsight Hadoop, Storm und HBase-Galerie für erste Schritte enthalten Beispieldaten.][hdinsight.sample.gallery]

Das folgende Video zeigt, wie das Beispiel zur Twitter-Trendanalyse ausgeführt wird:

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.youtube.com/embed/7ePbHot1SN4></a></center>

Zugriff auf das Dashboard ist über „http://<Name\_Ihres-HDInsight-Clusters>.azurehdinsight.net/“ oder das Azure-Portal möglich.

**So führen Sie ein Beispiel aus der Galerie für erste Schritte aus**

1. Melden Sie sich beim [Azure-Portal][azure.portal] an.
2. Klicken Sie im linken Menü auf **Durchsuchen**, klicken Sie auf **HDInsight-Cluster**, und klicken Sie dann auf Ihren Clusternamen.
3. Klicken Sie im Hauptmenü auf **Dashboard**.
4. Geben Sie den Benutzernamen und das Kennwort für den HTTP-Benutzer (auch als Clusterbenutzer bezeichnet) ein.
6. Klicken Sie oben auf **Erste Schritte mit der Galerie**.
7. Klicken Sie auf eines der Beispiele. Jedes Beispiel bietet ausführliche Anweisungen zur Ausführung. Die folgende Abbildung zeigt das Beispiel zur Twitter-Trendanalyse:

	![HDInsight Twitter-Trendanalysebeispiel][hdinsight.twitter.sample]

## Nächste Schritte
Andere Möglichkeiten zum Kennenlernen von HDInsight:

- [HDInsight-Lernkonzept][hdinsight.learn.map]
- [HDInsight-Infografik][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]: https://portal.azure.com

<!---HONumber=AcomDC_0727_2016-->