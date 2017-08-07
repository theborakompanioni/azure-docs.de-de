
Die NC- und NV-Größen werden auch als GPU-fähige Instanzen bezeichnet. Dabei handelt es sich um spezielle virtuelle Computer, die für verschiedene Szenarien und Anwendungsfälle optimierte GPU-Karten von NVIDIA enthalten. Die NV-Größen sind optimiert und konzipiert für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX. Die NC-Größen sind eher für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert, beispielsweise CUDA- und OpenCL-basierte Anwendungen und Simulationen. 


Die NV-Instanzen nutzen die Tesla M60 GPU-Karte von NVIDIA sowie NVIDIA GRID und ermöglichen die Verwendung beschleunigter Desktopanwendungen und virtueller Desktops, mit denen Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows auf den NV-Instanzen visualisieren, um überlegene Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen, z.B. Codierung und Rendering. Tesla M60 bietet 4096 CUDA-Kerne als duale GPU mit bis zu 36 Datenströmen mit 1080p H.264. 

Die NC-Instanzen werden durch die Grafikkarte Tesla K80 von NVIDIA unterstützt. Benutzer können Daten jetzt wesentlich schneller analysieren, indem sie CUDA für Anwendungen zur Energieuntersuchung, Absturzsimulationen, Rendering mit Raytracing, Deep Learning und mehr verwenden. Tesla K80 bietet 4992 CUDA-Kerne als duale GPU mit einer Leistung von bis zu 2,91 Teraflops mit doppelter Genauigkeit und bis zu 8,93 Teraflops mit einfacher Genauigkeit.

## <a name="nv-instances"></a>NV-Instanzen

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | GPU | Maximale Anzahl von Datenträgern |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 |
| Standard_NV12 |12 |112 |680 | 2 | 16 |
| Standard_NV24 |24 |224 |1.440 | 4 | 32 |

1 GPU = halbe M60-Karte

## <a name="nc-instances"></a>NC-Instanzen

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | GPU | Maximale Anzahl von Datenträgern |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 8 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 |
| Standard_NC24 |24 |224 | 1.440 | 4 | 32 |
| Standard_NC24r* |24 |224 | 1.440 | 4 | 32 |

1 GPU = halbe K80-Karte

* RDMA-fähig


