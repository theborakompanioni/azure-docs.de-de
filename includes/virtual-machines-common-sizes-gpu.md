Die NC- und NV-Größen werden auch als GPU-fähige Instanzen bezeichnet. Dabei handelt es sich um spezielle virtuelle Computer, die für verschiedene Szenarien und Anwendungsfälle optimierte GPU-Karten von NVIDIA enthalten. Die NV-Größen sind optimiert und konzipiert für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX. Die NC-Größen sind eher für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert, beispielsweise CUDA- und OpenCL-basierte Anwendungen und Simulationen. 


Die NV-Instanzen werden durch die Tesla M60 GPU-Karte von NVIDIA und NVIDIA GRID unterstützt und ermöglichen für Desktops beschleunigte Anwendungen und virtuelle Desktops, sodass die Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows auf den NV-Instanzen visualisieren, um überlegene Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen, z.B. Codierung und Rendering. Tesla M60 bietet 4096 CUDA-Kerne als duale GPU mit bis zu 36 Datenströmen mit 1080p H.264. 

Die NC-Instanzen werden durch die Grafikkarte Tesla K80 von NVIDIA unterstützt. Benutzer können Daten jetzt wesentlich schneller analysieren, indem sie CUDA für Anwendungen zur Energieuntersuchung, Absturzsimulationen, Rendering mit Raytracing, Deep Learning und mehr verwenden. Tesla K80 bietet 4992 CUDA-Kerne als duale GPU mit einer Leistung von bis zu 2,91 Teraflops mit doppelter Genauigkeit und bis zu 8,93 Teraflops mit einfacher Genauigkeit.

## <a name="nv-instances"></a>NV-Instanzen

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1.440 | 4 |

1 GPU = halbe M60-Karte

## <a name="nc-instances"></a>NC-Instanzen

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1.440 | 4 |
| Standard_NC24r* |24 |224 | 1.440 | 4 |

1 GPU = halbe K80-Karte

* RDMA-fähig


