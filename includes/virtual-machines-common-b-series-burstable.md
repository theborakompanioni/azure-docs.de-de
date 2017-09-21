
> [!NOTE] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> Diese Vorschauversion ist auf folgende Regionen beschränkt:
> - USA, Westen 2
> - USA, Osten
> - Europa, Westen
> - Asien-Pazifik, Südosten


Bei virtuellen Computern der B-Serie können Sie wählen, welche VM-Größe Ihnen die nötige Grundleistung für Ihre Workload bietet, und Sie haben die Möglichkeit, die CPU-Leistung um bis zu 100 Prozent einer vCPU vom Typ Intel® Broadwell E5-2673 v4 2,3 GHz oder Intel® Haswell 2,4 GHz E5-2673 v3 zu steigern.

Virtuelle Computer der B-Serie sind ideal für Workloads, die nicht kontinuierlich die volle Leistung der CPU benötigen. Hierzu zählen beispielsweise Webserver, kleine Datenbanken sowie Entwicklungs- und Testumgebungen. Diese Workloads haben in der Regel kurzfristige Leistungsanforderungen. Mit der B-Serie können Sie eine VM-Größe mit einer Grundleistung erwerben, wobei die VM-Instanz Guthaben bildet, wenn sie weniger als die Grundleistung beansprucht. Wenn für den virtuellen Computer Guthaben gebildet wurde, kann Leistung genutzt werden, die über die Grundleistung hinausgeht. Dabei können bis zu 100 Prozent der vCPU verwendet werden, wenn Ihre Anwendung mehr CPU-Leistung benötigt.

Die B-Serie ist in sechs VM-Größen erhältlich:

| Größe          | vCPUs | Arbeitsspeicher: GiB | Lokales SSD: GiB | CPU-Grundleistung des virtuellen Computers | Maximale CPU-Leistung des virtuellen Computers | Guthabenbildung/Stunde | Maximalguthaben |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | 10 %                            | 100 %                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | 20 %                            | 100 %                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40 %                            | 200 %                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60 %                            | 200 %                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90 %                            | 400 %                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135 %                           | 800 %                      | 81                    | 1944               |




## <a name="q--a-about-this-preview"></a>Fragen und Antworten zu dieser Vorschauversion

### <a name="q-how-can-i-participate-in-this-preview"></a>F: Wie kann ich an dieser Vorschauversion teilnehmen?
**A:** Fordern Sie ein Kontingent für die B-Serie in einer der unterstützten Regionen an.  Nach der Freigabe Ihres Kontingents können Sie Ihre Bereitstellung wie gewohnt über das Portal oder über die APIs durchführen. Weitere Informationen finden Sie unter [Anforderungen zur Erhöhung von Azure Resource Manager-Kernkontingenten](../articles/azure-supportability/resource-manager-core-quotas-request.md).

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>F: Wie erhalte ich einen virtuellen Computer mit einer Grundleistung von 135 Prozent?
**A:** Die 135 Prozent verteilen sich auf die acht vCPUs, die der VM-Größe zugrunde liegen. Wenn Ihre Anwendung also beispielsweise vier der acht Kerne für die Batchverarbeitung nutzt und jede dieser vier vCPUs zu 30 Prozent ausgelastet ist, beläuft sich die CPU-Leistung des virtuellen Computers auf insgesamt 120 Prozent.  Das bedeutet, dass der virtuelle Computer ein Zeitguthaben auf der Grundlage des 15-prozentigen Deltas (im Vergleich zu Ihrer Grundleistung) bildet.  Es bedeutet aber auch Folgendes: Wenn Sie über Guthaben verfügen, kann der gleiche virtuelle Computer 100 Prozent aller acht vCPUs nutzen, wodurch sich eine maximale CPU-Leistung von 800 Prozent ergibt.

### <a name="q-is-there-a-discount-on-price-during-the-preview"></a>F: Ist der Preis während der Vorschauphase rabattiert?
**A:** Ja. Die Preise für die Vorschauversion finden Sie auf unserer [Preisseite](http://aka.ms/vmsizes).

### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>F: Wie kann ich mein Guthaben und den Verbrauch überwachen?
**A:** In den kommenden Wochen werden zwei neue Metriken eingeführt. Die Metrik **Credit** gibt Aufschluss darüber, wie viel Guthaben Ihr virtueller Computer gebildet hat, und die Metrik **ConsumedCredit** zeigt, wie viel CPU-Guthaben Ihr virtueller Computer aus dem Vorrat verbraucht hat.    Diese Metriken können Sie im Metrikbereich des Portals oder programmgesteuert über die Azure Monitor-APIs anzeigen.

Weitere Informationen zum Zugreifen auf die Metrikdaten für Azure finden Sie in der [Übersicht über Metriken in Microsoft Azure](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>F: Wie wird Guthaben gebildet?
**A:** Bildungs- und Verbrauchsrate des virtuellen Computers sind so gestaltet, dass es bei einem virtuellen Computer, der exakt auf dem Grundleistungsniveau betrieben wird, weder zu einer Nettobildung noch zu einem Verbrauch von Burst-Guthaben kommt.  Eine Nettoerhöhung des Guthabens erfolgt, wenn der virtuelle Computer unter seiner Grundleistung betrieben wird. Bei einer CPU-Nutzung, die über das Grundleistungsniveau hinausgeht, kommt es hingegen zu einer Nettoverringerung des Guthabens.

**Beispiel:** Angenommen, ich stelle für meine kleine Zeit- und Anwesenheitsdatenbank einen virtuellen Computer der Größe „B1ms“ bereit. Bei dieser Größe kann meine Anwendung bis zu 20 Prozent einer vCPU als Grundleistung nutzen. Das entspricht 0,2 Guthaben pro Minute, die ich entweder nutzen oder sammeln kann. 

Meine Anwendung ist jeweils am Anfang und Ende des Arbeitstags meiner Mitarbeiter aktiv (also zwischen 7:00 und 9:00 Uhr sowie zwischen 16:00 und 18:00 Uhr). Während der restlichen 20 Stunden des Tages befindet sich die Anwendung in der Regel im Leerlauf und nutzt lediglich zehn Prozent der vCPU. In der Nebenzeit erhalte ich pro Minute 0,2 Guthaben, verbrauche aber nur 0,1 Guthaben pro Minute. Der virtuelle Computer bildet also pro Stunde ein Guthaben von sechs Einheiten (0,1 x 60).  Für die 20 Stunden, die nicht als Spitzenzeit gelten, entsteht somit ein Guthaben von 120 Einheiten.  

Während der Spitzenzeit beansprucht meine Anwendung im Schnitt 60 Prozent der vCPU. Ich erhalte zwar weiterhin 0,2 Guthaben pro Minute, verbrauche nun aber 0,6 Guthaben pro Minute. Die Minute kostet also netto 0,4 Guthaben. In der Stunde sind das 24 Guthaben (0,4 x 60). Die Spitzenauslastung dauert pro Tag vier Stunden und kostet somit 96 Guthaben (4 x 24).

Wenn ich nun die 120 Guthaben, die ich während der Nebenzeit gebildet habe, mit den 96 Guthaben verrechne, die für meine Spitzenzeit anfallen, bleiben täglich 24 Guthaben übrig, die ich für andere Aktivitäten mit höheren Anforderungen nutzen kann.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>F: Unterstützt die B-Serie Storage Premium-Datenträger?
**A:** Ja. Storage Premium-Datenträger werden von allen Größen der B-Serie unterstützt.   
    

### <a name="q-which-regions-can-i-access-the-preview-from"></a>F: Über welche Regionen kann ich auf die Vorschauversion zugreifen?
**A:** Die Vorschauversion der B-Serie ist in folgenden Regionen verfügbar:
- USA, Westen 2
- USA, Osten
- Europa, Westen
- Asien-Pazifik, Südosten

Nach Abschluss der Vorschauphase wird die B-Serie für die restlichen Regionen freigegeben.
    

    
