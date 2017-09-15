---
title: "MongoDB-, Angular- und Note-Tutorial für Azure – Teil 3 | Microsoft-Dokumentation"
description: "Teil 3 der Tutorialreihe zur Erstellung einer MongoDB-App mit Angular und Note in Azure Cosmos DB unter Verwendung der gleichen APIs wie für MongoDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: 1fb8d7b9f1014f37f0f3afa20605fce10c45a967
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-3-build-the-ui-with-angular"></a>Erstellen einer MongoDB-App mit Angular und Azure Cosmos DB – Teil 3: Erstellen der Benutzeroberfläche mit Angular

In diesem mehrteiligen Tutorial erfahren Sie, wie Sie eine neue, in Node.js geschriebene [MongoDB-API](mongodb-introduction.md)-App mit Express und Angular erstellen und mit Ihrer Azure Cosmos DB-Datenbank verbinden.

Teil 3 des Tutorials baut auf [Teil 2](tutorial-develop-mongodb-nodejs-part2.md) auf und umfasst folgende Aufgaben:

> [!div class="checklist"]
> * Erstellen der Angular-Benutzeroberfläche
> * Festlegen des Erscheinungsbilds mithilfe von CSS
> * Lokales Testen der App

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video

> [!VIDEO https://www.youtube.com/embed/MnxHuqcJVoM]

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Teil des Tutorials wird vorausgesetzt, dass Sie die Schritte aus [Teil 2](tutorial-develop-mongodb-nodejs-part2.md) des Tutorials ausgeführt haben.

> [!TIP]
> In diesem Tutorial wird die Anwendungserstellung Schritt für Schritt erläutert. Für den Fall, dass Sie das fertige Projekt herunterladen möchten, steht die fertige Anwendung auf GitHub im [Repository „angular-cosmosdb“](https://github.com/Azure-Samples/angular-cosmosdb) zur Verfügung.

## <a name="build-the-ui"></a>Erstellen der Benutzeroberfläche

1. Klicken Sie in Visual Studio Code auf die Schaltfläche „Beenden“ ![Schaltfläche „Beenden“ in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/stop-button.png) , um die Node-App zu beenden.

2. Geben Sie an der Eingabeaufforderung (Windows) oder im Terminalfenster (Mac) den folgenden Befehl ein, um eine heroes-Komponente zu generieren. In diesem Code steht „g“ für „Generieren“ und „c“ für „Komponente“. „heroes“ ist der Name der Komponente, und es wird eine Flatfile-Struktur (--flat) verwendet, damit kein Unterordner erstellt wird.

    ```
    ng g c heroes --flat 
    ```

    Im Terminalfenster wird eine Bestätigung für die neuen Komponenten angezeigt.

    ```bash
    installing component
      create src\client\app\heroes.component.ts
      update src\client\app\app.module.ts 
    ```

    Werfen wir einen Blick auf die Dateien, die erstellt und aktualisiert wurden. 

3. Navigieren Sie in Visual Studio Code im Bereich **Explorer** zum neuen Ordner **src\client\app**, und öffnen Sie die neue, in Schritt 2 erstellte Datei **heroes.component.ts**. Diese TypeScript-Komponentendatei wurde mit dem vorherigen Befehl erstellt.

    > [!TIP]
    > Sollte der Anwendungsordner in Visual Studio Code nicht angezeigt werden, öffnen Sie durch Drücken von CMD+UMSCHALT+P (Mac) oder STRG+UMSCHALT+P (Windows) die Befehlspalette, und geben Sie dann *Reload Window* ein, um die Systemänderung zu übernehmen.

    ![Öffnen der Datei „heroes.component.ts“](./media/tutorial-develop-mongodb-nodejs-part3/open-folder.png)

4. Öffnen Sie im gleichen Ordner die Datei **app.module.ts**, und beachten Sie, dass sie die `HeroesComponent` den Deklarationen in Zeile 5 hinzugefügt und sie in Zeile 10 auch importiert hat.

    ![Öffnen der Datei „app-module.ts“](./media/tutorial-develop-mongodb-nodejs-part3/app-module-file.png)

    Nachdem Sie nun über Ihre heroes-Komponente verfügen, erstellen Sie eine neue Datei für den HTML-Code der heroes-Komponente. Da wir eine minimalistische App erstellt haben, war geplant, den HTML-Code in der gleichen Datei zu platzieren wie die TypeScript-Datei. Wir möchten den HTML-Code jedoch gesondert behandeln und eine separate Datei erstellen.

5. Klicken Sie im Bereich **Explorer** mit der rechten Maustaste auf den Ordner **app**, klicken Sie auf **Neue Datei**, und nennen Sie die neue Datei *heroes.component.html*.

6. Löschen Sie in der Datei **heroes.component.ts** die Zeilen 5 bis 9. 

    ```ts
    template: `
        <p>
          heroes Works!
        </p>
      `,
      ```
      Ersetzen Sie sie durch Folgendes:
  
    ```ts
    templateUrl: './heroes.component.html',
    ```

    Dadurch wird auf die neue HTML-Datei verwiesen.
 
    > [!TIP]
    > Zur Beschleunigung der Entwicklung können Sie auf die Angular Essentials-Erweiterungen und -Codeausschnitte für Visual Studio Code von John Papa zurückgreifen. 
    > 1. Klicken Sie auf die Schaltfläche **Erweiterungen** ![Visual Studio Code: Schaltfläche „Erweiterungen“](./media/tutorial-develop-mongodb-nodejs-part3/extensions-button.png).
    > 2. Geben Sie *angular essentials* in das Suchfeld ein.
    > 3. Klicken Sie auf **Installieren**. 
    > 4. Klicken Sie auf die Schaltfläche **Erneut laden**, um die neuen Erweiterungen zu verwenden.
    > Oder laden Sie sie unter [http://jpapa.me/angularessentials](http://jpapa.me/angularessentials) herunter. 
    > ![Erweiterung „Angular Essentials“](./media/tutorial-develop-mongodb-nodejs-part3/angular-essentials-extension.png)

7. Navigieren Sie wieder zur Datei **heroes.component.html**, und fügen Sie den folgenden Code ein. `<div>` ist der Container für die gesamte Seite. Der Container enthält eine Liste mit Helden, die wir erstellen müssen, damit Sie auf der Benutzeroberfläche auf einen Helden klicken können, um ihn auszuwählen und zu bearbeiten oder zu löschen. Danach folgt im HTML-Code etwas Formatierung, um deutlich zu machen, welcher Held ausgewählt wurde. Darüber hinaus gibt es einen Bearbeitungsbereich, in dem Sie einen neuen Helden hinzufügen oder einen bereits vorhandenen Helden bearbeiten können. 

    ```html
    <div>
      <ul class="heroes">
        <li *ngFor="let hero of heroes" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
          <button class="delete-button" (click)="deleteHero(hero)">Delete</button>
          <div class="hero-element">
            <div class="badge">{{hero.id}}</div>
            <div class="name">{{hero.name}}</div>
            <div class="saying">{{hero.saying}}</div>
          </div>
        </li>
      </ul>
      <div class="editarea">
        <button (click)="enableAddMode()">Add New Hero</button>
        <div *ngIf="selectedHero">
          <div class="editfields">
            <div>
              <label>id: </label>
              <input [(ngModel)]="selectedHero.id" placeholder="id" *ngIf="addingHero" />
              <label *ngIf="!addingHero" class="value">{{selectedHero.id}}</label>
            </div>
            <div>
              <label>name: </label>
              <input [(ngModel)]="selectedHero.name" placeholder="name" />
            </div>
            <div>
              <label>saying: </label>
              <input [(ngModel)]="selectedHero.saying" placeholder="saying" />
            </div>
          </div>
          <button (click)="cancel()">Cancel</button>
          <button (click)="save()">Save</button>
        </div>
      </div>
    </div>
    ```

8. Nachdem wir nun über den HTML-Code verfügen, müssen wir ihn der Datei **heroes.component.ts** hinzufügen, damit wir mit der Vorlage interagieren können. Der neue Code, der in **heroes.component.ts** eingefügt wurde (siehe weiter unten), fügt die Vorlage unserer Komponentendatei hinzu. Ein Konstruktor wurde hinzugefügt, der einige Helden abruft und die hero-Dienstkomponente initialisiert, um alle Daten abzurufen. Der Code fügt auch alle erforderlichen Methoden für die Behandlung von Benutzeroberflächenereignissen hinzu. Sie können den in **heroes.component.ts** vorhandenen Code durch folgenden Code ersetzen: 

    ```ts
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-heroes',
      templateUrl: './heroes.component.html'
    })
    export class HeroesComponent implements OnInit {
      addingHero = false;
      heroes: any = [];
      selectedHero: Hero;
    
      constructor(private heroService: HeroService) {}
    
      ngOnInit() {
       this.getHeroes();
      }

      cancel() {
        this.addingHero = false;
        this.selectedHero = null;
      }

      deleteHero(hero: Hero) {
        this.heroService.deleteHero(hero).subscribe(res => {
          this.heroes = this.heroes.filter(h => h !== hero);
          if (this.selectedHero === hero) {
            this.selectedHero = null;
          }
        });
      }

      getHeroes() {
        return this.heroService.getHeroes().subscribe(heroes => {
          this.heroes = heroes;
        });
      }

      enableAddMode() {
        this.addingHero = true;
        this.selectedHero = new Hero();
      }

      onSelect(hero: Hero) {
        this.addingHero = false;
        this.selectedHero = hero;
      }

      save() {
        if (this.addingHero) {
          this.heroService.addHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
            this.heroes.push(hero);
          });
        } else {
          this.heroService.updateHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
          });
        }
      }
    }
    ```

9. Öffnen Sie im **Explorer** die Datei **app/app.module.ts**, und aktualisieren Sie die Zeilen 13 (zusätzliches Komma) und 14, um einen Import für ein `FormsModule` hinzuzufügen. Der Importabschnitt sollte nun wie folgt aussehen:

    ```
    imports: [
      BrowserModule,
      FormsModule
    ],
    ```

10. Fügen Sie in Zeile 3 einen Import für das neue FormsModule-Modul hinzu. 

    ```
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    ```

## <a name="use-css-to-set-the-look-and-feel"></a>Festlegen des Erscheinungsbilds mithilfe von CSS

1. Öffnen Sie im Explorer-Bereich die Datei **src/client/styles.scss**.

2. Ersetzen Sie den Inhalt der Datei, indem Sie den folgenden Code kopieren und in **styles.scss** einfügen.

    ```css
    /* You can add global styles to this file, and also import other style files */

    * {
      font-family: Arial;
    }
    h2 {
      color: #444;
      font-weight: lighter;
    }
    body {
      margin: 2em;
    }
    
    body,
    input[text],
    button {
      color: #888;
      // font-family: Cambria, Georgia;
    }
    button {
      font-size: 14px;
      font-family: Arial;
      background-color: #eee;
      border: none;
      padding: 5px 10px;
      border-radius: 4px;
      cursor: pointer;
      cursor: hand;
      &:hover {
        background-color: #cfd8dc;
      }
      &.delete-button {
        float: right;
        background-color: gray !important;
        background-color: rgb(216, 59, 1) !important;
        color: white;
        padding: 4px;
        position: relative;
        font-size: 12px;
      }
    }
    div {
      margin: .1em;
    }

    .selected {
      background-color: #cfd8dc !important;
      background-color: rgb(0, 120, 215) !important;
      color: white;
    }

    .heroes {
      float: left;
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      li {
        cursor: pointer;
        position: relative;
        left: 0;
        background-color: #eee;
        margin: .5em;
        padding: .5em;
        height: 3.0em;
        border-radius: 4px;
        width: 17em;
        &:hover {
          color: #607d8b;
          color: rgb(0, 120, 215);
          background-color: #ddd;
          left: .1em;
        }
        &.selected:hover {
          /*background-color: #BBD8DC !important;*/
          color: white;
        }
      }
      .text {
        position: relative;
        top: -3px;
      }
      .saying {
        margin: 5px 0;
      }
      .name {
        font-weight: bold;
      }
      .badge {
        /* display: inline-block; */
        float: left;
        font-size: small;
        color: white;
        padding: 0.7em 0.7em 0 0.5em;
        background-color: #607d8b;
        background-color: rgb(0, 120, 215);
        background-color:rgb(134, 183, 221);
        line-height: 1em;
        position: relative;
        left: -1px;
        top: -4px;
        height: 3.0em;
        margin-right: .8em;
        border-radius: 4px 0 0 4px;
        width: 1.2em;
      }
    }

    .header-bar {
      background-color: rgb(0, 120, 215);
      height: 4px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    label {
      display: inline-block;
      width: 4em;
      margin: .5em 0;
      color: #888;
      &.value {
        margin-left: 10px;
        font-size: 14px;
      }
    }

    input {
      height: 2em;
      font-size: 1em;
      padding-left: .4em;
      &::placeholder {
          color: lightgray;
          font-weight: normal;
          font-size: 12px;
          letter-spacing: 3px;
      }
    }

    .editarea {
      float: left;
      input {
        margin: 4px;
        height: 20px;
        color: rgb(0, 120, 215);
      }
      button {
        margin: 8px;
      }
      .editfields {
        margin-left: 12px;
      }
    }
    ``` 
3. Speichern Sie die Datei. 

## <a name="display-the-component"></a>Anzeigen der Komponente

Wir verfügen nun also über die Komponente. Aber wie bekommen wir sie auf den Bildschirm? Dazu ändern wir die Standardkomponenten in **app.component.ts**.

1. Öffnen Sie im Explorer-Bereich die Datei **client/app/app.component.ts**.

2. Ändern Sie in den Zeilen 6 bis 8 den Titel in „Heroes“, und fügen Sie dann den Namen der Komponente ein, die wir in **heroes.components.ts** erstellt haben (app-heroes), um auf diese neue Komponente zu verweisen. Der Vorlagenabschnitt sollte nun wie folgt aussehen: 

    ```ts
    template: `
      <h1>Heroes</h1>
      <div class="header-bar"></div>
      <app-heroes></app-heroes>
    `,
    ```

3. Da in **heroes.components.ts** noch auf weitere Komponenten (beispielsweise die hero-Komponente) verwiesen wird, müssen wir auch diese erstellen. Erstellen Sie an der Eingabeaufforderung der Angular-Befehlszeilenschnittstelle mithilfe des folgenden Befehls ein hero-Modell und eine Datei namens **hero.ts** mit „g“ für „Generieren“, „cl“ für „Klasse“ und „hero“ als Klassenname.

    ```bash
    ng g cl hero
    ```

    Im Terminalfenster wird eine Bestätigung für die neue Klasse angezeigt.

    ```bash
    installing class
    create src\client\app\hero.ts
    ```

4. Öffnen Sie im Explorer-Bereich die Datei **src\client\app\hero.ts**.

5. Ersetzen Sie in **hero.ts** den Inhalt der Datei durch den folgenden Code, um eine hero-Klasse mit einer ID, einem Namen und einem Spruch hinzuzufügen. 

    ```ts
      export class Hero {
      id: number;
      name: string;
      saying: string;
    }
    ```

6. Kehren Sie zu **heroes.components.ts** zurück, und beachten Sie, dass in der Zeile mit `selectedHero: Hero;` (Zeile 10) der Text `Hero` rot unterringelt ist. 

7. Klicken Sie auf den Begriff `Hero`. Daraufhin zeigt Visual Studio links neben dem Codeblock ein Glühbirnensymbol an. 

    ![Glühbirne in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/light-bulb.png)

8. Klicken Sie auf die Glühbirne und anschließend auf **"Hero" aus "client/app/hero" importieren.** oder auf **"Hero" aus "./hero" importieren.** (Die Nachricht ist abhängig von Ihrem Setup.)

    In Zeile 2 erscheint eine neue Codezeile. Falls Zeile 2 auf „client/app/hero“ verweist, passen Sie sie so an, dass sie auf die hero-Datei aus dem lokalen Ordner (./hero) verweist. Zeile 2 sollte wie folgt aussehen:

   ```
   import { Hero } from "./hero";
   ``` 

    Damit hätten wir uns um das Modell gekümmert. Wir müssen aber auch noch den Dienst erstellen.

## <a name="create-the-service"></a>Erstellen des Diensts

1. Geben Sie an der Eingabeaufforderung der Angular-Befehlszeilenschnittstelle den folgenden Befehl ein, um in **app.module.ts** einen hero-Dienst zu erstellen. „g“ steht hierbei für „Generieren“, „s“ steht für „Dienst“, „hero“ ist der Name des Diensts, und „-m“ dient zum Platzieren in „app.module“.

    ```bash
    ng g s hero -m app.module
    ```

    Der Ausgabe können Sie entnehmen, dass **hero.service.ts** erstellt und **app.module.ts** aktualisiert wurde.
  
    ```bash
    installing service
      create src\client\app\hero.service.ts
      update src\client\app\app.module.ts
    ```
    
    In „app.module.ts“ wurden folgende Codezeilen (6 und 17) hinzugefügt:
    
    ```typescript
    import { HeroService } from './hero.service';
    ...
        providers: [HeroService],
    ```

2. Öffnen Sie in Visual Studio Code die Datei **hero.service.ts**, und ersetzen Sie den Inhalt der Datei durch den folgenden Code:

    ```ts
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    
    import { Hero } from './hero';
    
    const api = '/api';

    @Injectable()
    export class HeroService {
      constructor(private http: HttpClient) {}

      getHeroes() {
        return this.http.get<Array<Hero>>(`${api}/heroes`)
      }

      deleteHero(hero: Hero) {
        return this.http.delete(`${api}/hero/${hero.id}`);
      }

      addHero(hero: Hero) {
        return this.http.post<Hero>(`${api}/hero/`, hero);
      }

      updateHero(hero: Hero) {
        return this.http.put<Hero>(`${api}/hero/${hero.id}`, hero);
      }
    }
    ```

    Dieser Code verwendet die neueste Version des HTTP-Clients von Angular. Hierbei handelt es sich um ein Modul, das bereitgestellt werden muss. Und genau das machen wir im nächsten Schritt.

3. Öffnen Sie in Visual Studio Code die Datei **app.module.ts**, und importieren Sie das HTTP-Clientmodul, indem Sie den Importabschnitt so aktualisieren, dass er „HttpClientModule“ enthält.

    ```ts
    imports: [
      BrowserModule,
      FormsModule,
      HttpClientModule
    ],
    ```

4. Fügen Sie der Liste mit den Importen in **app.module.ts** die Anweisung zum Importieren von „HttpClientModule“ hinzu.

    ```ts
    import { HttpClientModule } from '@angular/common/http';
    ```

5. Navigieren Sie in Visual Studio Code wieder zu **heroes.components.ts** Beachten Sie, dass in der Zeile mit `constructor(private heroService: HeroService) {}` (Zeile 13) der Text `HeroService` rot unterringelt ist. Klicken Sie auf `HeroService`. Daraufhin wird links neben dem Codeblock eine Glühbirne angezeigt. Klicken Sie auf die Glühbirne und anschließend auf **"HeroService" aus "./hero.service" importieren.** oder auf **"HeroService" aus "client/app/hero.service" importieren.**

    Durch Klicken auf die Glühbirne wird in Zeile 2 eine neue Codezeile eingefügt. Falls Zeile 2 auf den Ordner „client/app/hero.service“ verweist, passen Sie sie so an, dass sie auf die hero-Datei aus dem lokalen Ordner (./hero.service) verweist. Zeile 2 sollte wie folgt aussehen:
    
    ```javascript
    import { HeroService } from "./hero.service"
    ```

6. Speichern Sie alle Dateien in Visual Studio Code.

## <a name="build-the-app"></a>Erstellen der App

1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um die Angular-Anwendung zu erstellen: 

    ```bash
    ng b
    ``` 

    Sollte ein Problem auftreten, werden im Terminalfenster Informationen zu den zu korrigierenden Dateien angezeigt. Nach Abschluss des Buildvorgangs befinden sich die neuen Dateien im Ordner **dist**. Falls gewünscht, können Sie sich die neuen Dateien im Ordner **dist** ansehen.

    Nun wollen wir jedoch die App ausführen.

2. Klicken Sie im linken Bereich von Visual Studio Code auf die Schaltfläche **Debuggen** ![Symbol „Debuggen“ in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png) und anschließend auf die Schaltfläche **Debugging starten** ![Symbol „Debugging starten“ in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/start-debugging-button.png).

3. Navigieren Sie in einem Internetbrowser zu **localhost:3000**, um zur lokal ausgeführten App zu gelangen.

     ![Lokal ausgeführte hero-Anwendung](./media/tutorial-develop-mongodb-nodejs-part3/azure-cosmos-db-mongodb-mean-app.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Erstellen der Angular-Benutzeroberfläche
> * Lokales Testen der App

Im nächsten Teil des Tutorials wird ein Azure Cosmos DB-Konto erstellt.

> [!div class="nextstepaction"]
> [Erstellen eines Azure Cosmos DB-Kontos mithilfe der Azure-Befehlszeilenschnittstelle](tutorial-develop-mongodb-nodejs-part4.md)

