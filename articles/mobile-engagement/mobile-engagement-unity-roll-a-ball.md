<properties
	pageTitle="Unity-Tutorial für ein Roll-a-Ball-Spiel"
	description="Schritte zum Erstellen des klassischen Unity Roll-a-Ball-Spiels, das Voraussetzung für alle Mobile Engagement Unity-Tutorials ist."
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager=""
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="08/19/2016"
	ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Erstellen eines Unity Roll-a-Ball-Spiels

Dieses Tutorial zeigt die wichtigsten Schritte für ein leicht abgewandeltes [Tutorial für ein Unity Roll-a-Ball-Spiel](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Dieses Beispielspiel besteht aus einem sphärischen „Player-Objekt“, das vom Benutzer der App gesteuert wird. Ziel des Spiels ist es, sammelbare Objekte zu „sammeln“, indem das Player-Objekt gegen diese Objekte stößt. Für dieses Tutorial werden Grundkenntnisse im Umgang mit der Unity-Editor-Umgebung vorausgesetzt. Sehen Sie sich bei Problemen das vollständige Tutorial an.

### Einrichten des Spiels
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141).

1. Öffnen Sie den **Unity-Editor**, und klicken Sie auf **New**.
	
	![][51]
	
2. Geben Sie unter **Project name** und **Location** einen Projektnamen und Speicherort an, wählen Sie **3D** aus, und klicken Sie auf **Create project**.
	
	![][52]

3. Speichern Sie die gerade erstellte Standardszene unter dem Namen **MiniGame** im Ordner **Assets** innerhalb eines neuen Ordners **\_Scenes** als Teil des neuen Projekts:
 	
	![][53]

4. Erstellen Sie ein 3D-Objekt über **3D Object > Plane** als Spielfeld, und geben Sie diesem Ebenenobjekt den Namen **Ground**.

	![][1]

5. Setzen Sie die Transformationskomponente für dieses **Ground**-Objekt zurück, sodass sie wieder am Ursprung ist.

	![][3]

6. Deaktivieren Sie im Menü **Gizmos** die Option **Show Grid** für das **Ground**-Objekt.

	![][4]

7. Setzen Sie die **Scale**-Komponente für das **Ground**-Objekt auf die folgenden Werte: X = 2, Y = 1, Z = 2.

	![][5]

8. Fügen Sie dem Projekt ein neues 3D-Objekt über **3D Object > Sphere** hinzu, und geben Sie diesem Kugelobjekt den Name**Player**.

	![][6]

9. Wählen Sie das **Player**-Objekt aus, und klicken Sie auf **Reset Transform** ähnlich wie beim Plane-Objekt.

10. Setzen Sie die Komponente **Transform > Position > Y Coordinate** für Player Y auf 0,5.

	![][7]

11. Erstellen Sie im Projekt einen neuen Ordner namens **Materials**, in dem wir das Material erstellen, mit dem die Farbe für den Player bestimmt wird.

12. Erstellen Sie in diesem Ordner ein neues **Material** namens **Background**.

	![][8]

13. Aktualisieren Sie die Farbe des Materials, indem Sie dessen **Albedo**-Eigenschaft aktualisieren. Sie können die RGB-Werte [0,32,64] auswählen.

	![][9]

14. Ziehen Sie dieses Material in die Szenenansicht, um die Farbe auf das **Ground**-Objekt anzuwenden.

	![][10]

17. Setzen Sie für mehr Klarheit abschließend **Transform > Rotation > Y** des Directional Light-Objekts auf 60.

	![][12]

### Bewegen des Players
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141).

1. Fügen Sie dem **Player**-Objekt eine **RigidBody**-Komponente hinzu.

	![][13]

2. Erstellen Sie im Projekt einen neuen Ordner namens **Scripts**.

3. Klicken Sie auf **Add Component > New Script > C# Script**. Geben Sie den Namen **PlayerContoller** ein, und klicken Sie auf **Create and Add**. Damit wird ein Skript erstellt und dem Player-Objekt hinzugefügt.

	![][14]

5. Verschieben Sie dieses Skript im Projekt in den Ordner **Scripts**.

6. Öffnen Sie das Skript zur Bearbeitung in Ihrem bevorzugten Skripteditor. Aktualisieren Sie den Skriptcode durch den folgenden Code, und speichern Sie ihn.

		using UnityEngine;
		using System.Collections;
		
		public class PlayerController : MonoBehaviour 
		{
			public float speed;
			private Rigidbody rb;
			void Start ()
			{
			    rb = GetComponent<Rigidbody>();
			}
			void FixedUpdate ()
			{
			    float moveHorizontal = Input.GetAxis ("Horizontal");
			    float moveVertical = Input.GetAxis ("Vertical");
				Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
				rb.AddForce (movement * speed);
			}
		}
	
8. Beachten Sie, dass das obige Skript eine **Speed**-Eigenschaft verwendet. Setzen Sie im Unity-Editor die Speed-Eigenschaft für die Geschwindigkeit auf 10.

	![][15]

9. Klicken Sie im Unity-Editor auf **Play**. Jetzt sollten Sie die Kugel mit der Tastatur steuern können. Die Kugel sollte sich drehen und bewegen.

### Bewegen der Kamera
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141). Mit diesen Schritten wird die **Main Camera** an das **Player**-Objekt gebunden.

1. Setzen Sie **Transform.Position** auf X = 0, Y = 10,5, Z = -10.
2. Setzen Sie **Transform.Rotation** auf X = 45, Y = 0, Z = 0.

	![][16]

2. Fügen Sie ein neues Skript namens **CameraController** der **MainCamera** hinzu, und verschieben Sie es in den Ordner „Scripts“.

	![][17]

3. Öffnen Sie das Skript zur Bearbeitung, und fügen Sie den folgenden Code ein:

		using UnityEngine;
		using System.Collections;
		
		public class CameraController : MonoBehaviour {
		
		    public GameObject player;
		
		    private Vector3 offset;
		
		    void Start ()
		    {
		        offset = transform.position - player.transform.position;
		    }
		    
		    void LateUpdate ()
		    {
		        transform.position = player.transform.position + offset;
		    }
		}
	
5. Ziehen Sie in der Unity-Umgebung die Player-Variable in den Player-Bereich des Main Camera-Objekts, damit die beiden miteinander verknüpft sind.

	![][18]

6. Wenn Sie jetzt im Unity-Editor auf „Play“ klicken und das Player Ball-Objekt drehen, folgt die Kamera der Bewegung der Kugel.

### Einrichten des Spielbereichs
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Wir erstellen die Wände rund um die Grundfläche („Ground“), damit das Player Ball-Objekt nicht aus dem Spielbereich rollt.

1. Klicken Sie auf **Create > Create Empty > Game Object**, und geben Sie ihm den Namen **Walls**.

	![][19]

2. Erstellen Sie unter diesem Walls-Objekt ein neues 3D-Objekt über **3D Object > Cube** mit dem Namen „West wall“.

	![][20]

3. Aktualisieren Sie die Werte für **Transform > Position** und **Transform > Scale** für dieses West Wall-Objekt.

	![][21]

4. Duplizieren Sie die „West Wall“, um eine **East wall** mit der aktualisierten Transformationsposition und -skalierung zu erstellen.

	![][22]

5. Duplizieren Sie die „East Wall“, um eine **North wall** mit der aktualisierten Transformationsposition und -skalierung zu erstellen.

	![][23]

6. Duplizieren Sie die „North Wall“, um eine **South wall** mit der aktualisierten Transformationsposition und -skalierung zu erstellen.

	![][24]

### Erstellen von sammelbaren Objekten
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Erstellen Sie nun einige gut aussehenden Objekte, die einen Satz an sammelbaren Objekten bilden, die das Player Ball-Objekt „sammeln“ muss, indem es mit ihnen zusammenstößt.

1. Erstellen Sie ein neues 3D-Objekt über **3D Object > Cube**, und geben Sie ihm den Namen „Pickup“.

2. Passen Sie die Werte für **Transform > Rotation** und **Transform > Scale** des Pickup-Objekts an.

	![][25]

3. Erstellen Sie ein **neues C#-Skript** namens **Rotator**, und hängen Sie es an das Pickup-Objekt an. Stellen Sie sicher, dass Sie das Skript im Ordner „Scripts“ speichern.

	![][26]

4. Öffnen Sie dieses Skript zur Bearbeitung, und aktualisieren Sie es folgendermaßen:

		using UnityEngine;
		using System.Collections;
		
		public class Rotator : MonoBehaviour {
		
		    void Update () 
		    {
		        transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
		    }
		}

5. Öffnen Sie jetzt den Spielmodus im Unity-Editor. Das Pickup-Objekt sollte sich jetzt um seine Achse drehen.

6. Erstellen Sie einen neuen Ordner namens **Prefabs**.

	![][27]

7. Ziehen Sie das **Pickup**-Objekt in den Ordner „Prefabs“.

	![][28]

8. Erstellen Sie ein neues **leeres Spielobjekt** namens **Pickups**. Setzen Sie die Position auf den Ursprung zurück, und ziehen Sie dann das Pickup-Objekt unter dieses Spielobjekt.

	![][29]

9. Duplizieren Sie das **Pickup**-Objekt, und verteilen Sie es auf dem **Ground**-Objekt rund um das **Player**-Objekt, indem Sie die **X- und Z-Werte für „Transform.Position“** entsprechend anpassen.

	![][30]

10. Erstellen Sie ein **neues Material** namens **Pickup**, und legen Sie als Farbe „Rot“ fest, indem Sie die **Albedo-Eigenschaft** aktualisieren, wie zuvor schon beim Aktualisieren des Ground-Objekts.

	![][31]

11. Wenden Sie das Material auf alle vier Pickup-Objekte an.

	![][32]

### Sammeln der Pickup-Objekte
Die folgenden Schritte stammen aus dem [Unity-Tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Aktualisieren Sie den Player, damit er die Pickup-Objekte „sammeln“ kann, indem er mit ihnen zusammenstößt.

1. Öffnen Sie das an das Player-Objekt angefügte **PlayerController**-Skript zur Bearbeitung, und aktualisieren Sie es folgendermaßen:

		using UnityEngine;
		using System.Collections;
		
		public class PlayerController : MonoBehaviour {
		
		    public float speed;
		
		    private Rigidbody rb;
		
		    void Start ()
		    {
		        rb = GetComponent<Rigidbody>();
		    }
		
		    void FixedUpdate ()
		    {
		        float moveHorizontal = Input.GetAxis ("Horizontal");
		        float moveVertical = Input.GetAxis ("Vertical");
		
		        Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
		
		        rb.AddForce (movement * speed);
		    }
		
		    void OnTriggerEnter(Collider other) 
		    {
		        if (other.gameObject.CompareTag ("Pick Up"))
		        {
		            other.gameObject.SetActive (false);
		        }
		    }
		}

2. Erstellen Sie ein neues **Tag** namens **Pick Up** (es muss dem Skript entsprechen).

	![][33]
	
	![][34]

3. Wenden Sie dieses **Tag** auf das Prefab Pickup-Objekt an.

	![][35]

4. Aktivieren Sie das Kontrollkästchen **IsTrigger** für das Prefab-Objekt.

	![][36]

5. Fügen Sie dem Pickup Prefab-Objekt eine Rigidbody-Komponente hinzu. Zur Optimierung der Leistung aktualisieren Sie den statischen Collider, den wir für einen dynamischen Collider verwendet haben.

	![][37]
  
6. Aktivieren Sie schließlich die **IsKinematic**-Eigenschaft für das Prefab-Objekt.

	![][38]

7. Drücken Sie im Unity-Editor auf **Play**, um das **Roll-a-Ball**-Spiel zu spielen, indem Sie das Player-Objekt mit der Tastatur in die gewünschte Richtung bewegen.

### Aktualisieren des Spiels für mobile Geräte
Mit den obigen Abschnitten ist das Tutorial von Unity abgeschlossen. Jetzt nehmen wir ein paar Änderungen vor, um das Spiel auch auf mobilen Geräten spielen zu können. Beachten Sie, dass wir zu Testzwecken bislang das Spiel mit Tastatureingaben gesteuert haben. Das Spiel wird nun so geändert, dass der Player durch Bewegen des Telefons bewegt wird, sprich es wird ein Beschleunigungsmesser als Eingabe verwendet.

Öffnen Sie das **PlayerContoller**-Skript zur Bearbeitung, und aktualisieren Sie die **FixedUpdate**-Methode, um das Player-Objekt mithilfe der Bewegung des Beschleunigungsmessers zu bewegen.

	    void FixedUpdate()
	    {
	        //float moveHorizontal = Input.GetAxis("Horizontal");
	        //float moveVertical = Input.GetAxis("Vertical");
	        //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
	        rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
	    }

In diesem Tutorial wird die Erstellung eines einfachen Spiels mit Unity vorgestellt. Dies können Sie auf einem Gerät Ihrer Wahl bereitstellen, um das Spiel zu spielen.

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

	
	
	
	
	
	
	
	
	
	
	
	

<!---HONumber=AcomDC_0824_2016-->