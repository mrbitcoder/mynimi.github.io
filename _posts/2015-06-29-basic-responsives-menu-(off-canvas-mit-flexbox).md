---
date: 2015-06-29 22:02:53 +0200
title: Basic Responsives Menu (off-canvas mit flexbox) ♦
id: BRM(mf
lang: de
subtitle: so ziemlich das einfachste 1-Level-Menu ever
tags: [tutorial, navigation,  sass, css, html, flexbox, ♦]
description: Wie man ein einfaches responsives 1-Level-Menu mit Flexbox macht
---
Menus. Als ich zum ersten Mal ein Menu geschrieben habe, war das in einer Liste drin. Ich bin noch immer für einen Listenaufbau, wenn es um ein Dropdown geht. Doch seit Flexbox endlich genug weit verbreitet ist, habe ich die Liste längst aufgegeben und bin nun bei minimalem Markup und minimaler CSS.
Diese Navigation ist geschrieben mit Flexbox und wird zu einem Off-Canvas-Menu, sobald der Breakpoint überschritten wird.
<br>
<!-- more -->
<p data-height="266" data-theme-id="7132" data-slug-hash="dPNyMz" data-default-tab="result" data-user="mynimi" class='codepen'>See the Pen <a href='http://codepen.io/mynimi/pen/dPNyMz/'>responsive 1-Level Menu</a> by Myri (<a href='http://codepen.io/mynimi'>@mynimi</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Ich verwende in diesem Tutorial jeweils [Font-Awesome](http://fontawesome.io/) und [jQuery](https://jquery.com/), weil ich beide davon stets in meinen Projekten eingebunden habe. Allerdings sollte dieses Menu auch ohne diese beiden Dependenzen einfach umzusetzen sein. Falls das gewünscht ist, bitte Bescheid geben.

Font Awesome verwende ich für das Menu-Icon und jQuery um das Off-Canvas-Menu per Klick zu aktivieren.

### Markup
Ich verwende meine Menus meist irgendwo im Header. Aus diesem Grund, habe ich beschlossen, das ganze in einem Header zu machen. Da ich den Menu-Toggle-Button ausserhalb der Navigation platziere und der nicht einfach so in der Luft hängen soll. Die Links platziere ich in einem `nav`-Element. Toggle-Links gibt es zwei. Wenn diese geklickt werden, wird das Menu entweder sichtbar, oder unsichtbar. Das ist jeweils bloss für die responsive Ansicht, aber sie sind wichtig. Den inneren toggle-Link versehe ich mit einem _X_, den äusseren mit dem Hamburger Icon und einem Text der "Menu" sagt. Ganz simpel.

Und damit es noch ein bisschen schneller geht, schreib ich das alles in jade

```slim
header
    a.toggle-link(href='#menu')
        | <i class="fa fa-navicon"></i> Menu
    nav.main-menu#menu
        a.toggle-link(href='#menu')
            i.fa.fa-close
        each val in [1, 2, 3, 4, 5]
            a(href='index'+val+'.html')= 'Item ' + val
```

in HTML schaut das dann so aus:

```html
<header>
    <a href="#menu" class="toggle-link">
        <i class="fa fa-navicon"></i> Menu
    </a>
    <nav id="menu" class="main-menu">
        <a href="#menu" class="toggle-link">
            <i class="fa fa-close"></i>
        </a>
        <a href="index1.html">Item 1</a>
        <a href="index2.html">Item 2</a>
        <a href="index3.html">Item 3</a>
        <a href="index4.html">Item 4</a>
        <a href="index5.html">Item 5</a>
  </nav>
</header>
```

### Styling
Styling wird mit Flexbox erledigt. Kommentare erläutern etwas mehr. Ich mach das in Sass, weil ich einfach nie wieder in meinem Leben CSS ohne Superkräfte verwenden werde.

```scss
// punkt an dem das Menu off-canvas wird
$menu_collapse: 500px;


// menu style
.main-menu {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    
    // mit flex-direction wird die Darstellung der Links angepasst
    
    // horizontal in nicht-responsiver Version
    @media screen and (min-width: $menu_collapse) {
        flex-direction: row;
        justify-content: space-between;
        max-width: 800px;
        
        .toggle-link {
            display: none;
        }
    }
    
    // off-canvas look
    @media screen and (max-width: $menu_collapse) {
        // links untereinander
        flex-direction: column;
        justify-content: flex-start;
        
        // off-canvas style, geschlossen
        background: darken(white, 10%);
        z-index: 100;
        width: 320px;
        min-height: 100%;
        position: fixed;
        top: 0;
        left: 0;
        transition: .3s all linear;
        box-sizing: border-box;
        padding: 10px;
        transform: translateX(-320px);
        
        // off-canvas stlye, geöffnet
        &.active {
            transform: translateX(0px);
        }
        
        // positioniere "x" oben rechts
        .toggle-link {
            align-self: flex-end;
        }
    }
}

// zeige toggle links in off-canvas modus
header {
    .toggle-link {
        @media screen and (min-width: $menu_collapse) {
            display: none;
        }
    }
}
```

und CSS schaut dann nach Sass und Autoprefixer so aus:

``` css
.main-menu {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-flex-wrap: wrap;
      -ms-flex-wrap: wrap;
          flex-wrap: wrap;
  -webkit-box-align: center;
  -webkit-align-items: center;
      -ms-flex-align: center;
          align-items: center;
}
@media screen and (min-width: 500px) {
  .main-menu {
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
    -webkit-flex-direction: row;
        -ms-flex-direction: row;
            flex-direction: row;
    -webkit-box-pack: justify;
    -webkit-justify-content: space-between;
        -ms-flex-pack: justify;
            justify-content: space-between;
    max-width: 800px;
  }
  .main-menu .toggle-link {
    display: none;
  }
}
@media screen and (max-width: 500px) {
  .main-menu {
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -webkit-flex-direction: column;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-pack: start;
    -webkit-justify-content: flex-start;
        -ms-flex-pack: start;
            justify-content: flex-start;
    background: #e6e6e6;
    z-index: 100;
    width: 320px;
    min-height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    -webkit-transition: .3s all linear;
            transition: .3s all linear;
    box-sizing: border-box;
    padding: 10px;
    -webkit-transform: translateX(-320px);
        -ms-transform: translateX(-320px);
            transform: translateX(-320px);
  }
  .main-menu.active {
    -webkit-transform: translateX(0px);
        -ms-transform: translateX(0px);
            transform: translateX(0px);
  }
  .main-menu .toggle-link {
    -webkit-align-self: flex-end;
        -ms-flex-item-align: end;
            align-self: flex-end;
  }
}

@media screen and (min-width: 500px) {
  header .toggle-link {
    display: none;
  }
}
```


### und ein Hauch jQuery
Damit das Menu auch responsive funktioniert, müssen wir nun nur noch einen klick-Event für den toggle-link schreiben, bei dem wir die Klasse `active` zuordnen.

```js
$(document).ready(function () {
    $(".toggle-link").click(function () {
        $("#menu").toggleClass("active");
    });
});
```

Und das ist es schon. Super einfach, super simpel, sehr wandelbar.