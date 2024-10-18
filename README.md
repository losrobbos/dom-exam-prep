# DOM Prep

  <h2>Exam prep</h2>

  <ul>
    <li>Einbindung von JavaScript in HTML</li>
    <li>Import/Export von Modulen</li>
    <li>npm-Projektinitialisierung und Packages</li>
    <li>Browser-Objekte wie window</li>
    <li>Events und Event-Handling</li>
    <li>HTTP-Methoden (z.B. POST)</li>
    <li>Asynchrone Funktionen (z.B. setTimeout, Promises, async/await)</li>
    <li>CORS (Cross-Origin Resource Sharing)</li>
    <li>JavaScript Engines</li>
    <li>DOM und DOM-Methoden</li>
  </ul>

## Einbindung JS

### Einbindung im Body Tag

```
<body>
  <div>Hallo</div>
  <script src="./js/script.js"></script>
</body>
```

### Einbindung in Head

```
<head>
  <script src="./js/script.js"></script>
</head>
```

### Einbindung in Head, mit defer

```
<head>
  <script defer src="./js/script.js"></script>
</head>
```

## ES Modules

Wenn ich in meinem JS Script andere Files importen will, muss ich das Skript
als "Module" laden

```
  <script defer src="./js/script.js" type="module"></script>

```

Im Anschluss können wir in script.js imports von anderen Files durchführen:

```
import theDefaultExport from './calc.js'
```

## Names vs Default Export

### Default Export & Import

Default Export nutzt man häufig, wenn ein Modul genau EINE Function
oder EIN Objekt exportiert.

Export:

```
  // exportiere eine Funktion
  export default () => {
    console.log("Bla")
  }

  // exportiere ein Object
  /* export default {
    name: "Blub"
  }*/

```

Importiere default export in einem JS File

```
// we can give the default any name we like
import theDefaultExport from './calc.js'

```

Exportiere Funktionen (oder beliebige andere JS Elemente) "named":

```
export const sum = () => {...}
export const subtract = () => {...}

```

Importiere named in einem File:

```
// do ONE named import
import { sum } from './calc.js'
// do multiple named imports at once
import { sum, subtract } from './calc.js'
```

### Default + named import in einem Schritt

```
import theDefaultExport, { sum } from './calc.js'
```

Rename named import, wenn ich den Namen z.B. schon benutze

Verhindert Namens-Kollisionen:

```
import theDefaultExport, { sum as mySum } from './calc.js'

const sum = () => {
  ....
}
```

## Einbindung Packages (aus dem Web)

Per CDN:

Lade ein Skript über das Web
CDNS erzeugen häufig ein globales Objekt in Window.
So kann ich das Package benutzen ohne Imports im Code

```
  <!-- CDN import -->
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>

```

Alternative:
Installiere package per npm

```
npm install gsap
```

Wenn wir Packages mit NPM installieren, kann der Browser 
diese standardmäßig nicht importieren.

Beispiel, die Zeile: 
`import gsap from 'gsap'`
würde scheitern, weil das Gsap Modul nicht gefunden werden kann.

Jetzt brauchen wir einen BUNDLER, der den Package Code 
und den eigenen Code ZUSAMMEN MERGED in ein sogenanntes BUNDLE.

Das ist nun der Schritt, wo wir zu einem npm WebProjekt wechseln. 

## npm Projektinitialisierung

Web-Projekte brauchen wir oft, wenn wir JavaScript Code benutzen,
der nicht im Browser lauffähig ist. Wie z.B. JSX von React.

Dieses JavaScript muss beim Laden in den Browser UMGEWANDELT
werden in normales JS, dass der Browser versteht.

Dafür brauchen wir einen sogenannten BUILD Prozess,
der diese Umwandlung macht.

Auch können wir so einfacher Third Party Packages in unserem Code importieren.

Der Build Prozess merged den eigenen Code und den Code der Packages 
zu einem BUNDLE, das der Browser ausführen kann.

### Projekt initialisieren

Projekte werden häufig mit dem Befehl "npm create" angelegt.

So können wir in unserem Code Packages von anderen importieren.

```
import gsap from 'gsap'
```

Ein Standard Build-Tool in WebDev ist aktuell **Vite**.

Ein Web-Projekt mit Vite initialisieren:

```
npm create vite <ProjektName> // ProjektName => FolderName
```

Dadurch wird ein Boilerplate Code generiert, der bereits einen Bundler hat (=Vite).

Man muss sich dadurch nicht um das Building und die Optimierung
von HTML und CSS selbst kümmern.

Installieren der Packages:
`npm install`

Starten des Web Projektes:
`npm run dev`

## Browser-Objekte wie window

Typische globale Objekte im window scope
(das sind einfach Unterobjekte des window Objektes)

```
window.document
window.alert
window.onresize = () => {...}
window.JSON
window.URL

Auf alle Unterobjekte von window können wir direkt zugreifen, ohne .window:
```
document
alert
onresize = () => {...}
etc
```

Prüfen ob bestimmtes Element in window ist

`console.log( "<objektname>" in window )`

## Events und Event-Handling

1. Hole Item dass Event produziert (Click, Input, Change, KeyDown)
```
const btn = document.querySelector(".btnToSomething");
const input = document.querySelector(".input")
console.log(input)
```

2. Adde event listener

Methode 1 => nur EIN Event auf dem Item festlegen 

```
btn.onclick = () => {
   console.log("Geklickt")
}
```

Methode 2 => addEventListener 

Mit addEventListener können wir MEHRERE Functions an ein Event hängen

```
// bei addEventListener kann man "on" bei Events weglassen, weil klar ist
// dass es Events sind, die alle mit on anfangen
btn.addEventListener("click", () => {
  console.log("Geklickt 1");
})
btn.addEventListener("click", () => {
  console.log("Geklickt 2");
});
```

Beide (!) Functions werden nun bei Klick auf Button ausgeführt

INPUT Event

```
input.addEventListener("input", () => {
  console.log(input.value)
})
```

## HTTP-Methoden (z.B. POST)

Es gibt VIER Data-Operations, die wir gegen ein BACKEND machen können.

- Alle Requests gehen meist zur selben BASE URL:
  - Beispiel: /api/todos 
- Daten holen (Arrays und ein Objekt): GET
  - /api/todos
  - /api/todos/12345
- Daten senden: POST
  - /api/todos
- Daten updaten: PATCH / PUT
  - /api/todos/12345
- Daten löschen: DELETE
  - /api/todos/12345

Fetch Requests sind immer HTTP requests.


### Get Request
```
fetch('http://rob-message-api.herokuapp.com/messages')
```

### POST Request
```
fetch('http://rob-message-api.herokuapp.com/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify( msgNew )
  })
```

### PATCH / PUT Request
```
fetch(`http://rob-message-api.herokuapp.com/messages/12345`, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify( msgUpdate )
  })
```

### Delete Request
```
fetch(`http://rob-message-api.herokuapp.com/messages/12345`, {
    method: 'DELETE
})
```

Full Guide:
[https://github.com/losrobbos/api-connect-guide?tab=readme-ov-file#testing-from-a-frontend-react]

## Asynchrone Funktionen (z.B. setTimeout, Promises, async/await)

## CORS (Cross-Origin Resource Sharing)



## JavaScript Engines

Eine JavaScript Engine ist der Teil im Browser,
der die JavaScript einliest, die Syntax prüft und wenn die Syntax passt ausführt. Die Engine gibt auch die JS-Fehler in der console aus.


## DOM und DOM-Methoden

### Query Methoden (Elemente holen)

```
document.querySelector // holt nur EIN Element
document.querySelectorAll // holt ALLE Elements, die den Selector matchen
document.getElementById // holt genau EIN Element mit dieser ID
document.getElementsByClassName // holt ALLE Elements dieser Klasse 
```


## Styling with JS

### Method 1: style prop

`element.style.cssPropertyCamelCase`
Beispiel: div.style.backgroundColor = "red"

### Method 2: classList 

`element.classList.add("<className>")`
`element.classList.remove("<className>")`
`element.classList.toggle("<className>")`

Beispiel: div.classList.toggle("active")

# Method 3: style.cssText prop

Alles CSS des Elements überschreiben

Beispiel: div.style.cssText = "font-size: 16px; background-color: aqua;
