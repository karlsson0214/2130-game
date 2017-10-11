Spelet i korthet.

Hjälten ska styras med piltangenterna. Hjälten flyttar 32 pixlar. Grafiska element är kvadrater med sidan 32 pixlar.

Hjälten kan inte gå ur spelpanen och inte gå in i väggar.

Hjälten kan samla guld och får då poäng.

Om hjälten stöter på ett monster så är spelet slut.

Stegvisa instruktioner följer.

JavaScript biblioteket jquery.com används. $ hör till jQuery.

Gjorda förberedelser
====================
Det finns några filer med innehåll.

index.html
-----------
HTML-kod för spelplanen.

Detta behöver du göra:

**Byt facit.js till main.js i script-taggen**

style.css
----------
Stilning av spelplanen och dess element.

main.js
--------
I denna fil skriver du din kod.

mappen crawl-tiles-oct-5-2010
-----------------------------
Innehåller bilder med storleken 32 x 32 pixlar.

Bilderna har hämtats från:

http://opengameart.org/content/dungeon-crawl-32x32-tiles

Allmänt
=========
Kom ihåg att testa att koden fungerar ofta, väldigt ofta.

Har du ändrat i koden måste du testa att det fungerar.
Fundera ut vilken form av tester som du behöver göra, och gör dessa.

Styra med tangentbordet
=======================
Skriv följande if-sats

    om key är 37
        flytta hjälten 32 pixlar till vänster med koden nedan
        hero.css("left", hero.position().left - 32);
        sätter css-egenskapen left till hjältens nuvarande position - 32

    om key är 39
        flytta hjälten 32 pixlar till höger

    om key är 38
        flytta hjälten 32 pixlar uppåt med koden nedan
        hero.css("top", hero.position().top - 32);

    om key är 40
        flytta hjälten 32 pixlar nedåt


Testa

Problem
---------
Det finns ett problem. Hjälten kan fly ut från spelplanen. Detta löses i nästa steg.

Ej utanför spelplanen mer
=========================
På svenska blandat med JavaScript blir lösningen.

    om key är 37
        flytta hjälten 32 pixlar till vänster med koden nedan
        hero.css("left", hero.position().left - 32);
        om hjälten är för långt till vänster
            flytta hjälten till vänster kant

På samma sätt för de andra fallen höger, upp och ner.

Extra konstanter
================
Det är dags att göra koden vackrare, samt lättare att läsa och underhålla.

Deklarera fyra konstanter ovanför if-satserna. Döp konstanterna till ARROW_LEFT, ARROW_RIGHT, ARROW_UP, och ARROW_DOWN. Låt de få värdena 37, 39 38 och 40.

    const MIN_KONSTANT = 8; //exempel på konstant

Byt ut siffervärdena i if-satserna mot ARROW_LEFT, och så vidare.

Extra funktioner
================
Det är dags att strukturera koden. Koden kommer att bli lättare att läsa, samt lättare att underhålla. Koden kommer att kunna återanvändas.

Skriv en funktion `moveRight()`.

Skriv koden under kommentaren:

    //interna styrfunktioner
    const moveRight = function () {
        //kod för att flytta 32 pixlar åt höger
        //flytta koden från if-satsen
    };

Anropa funktionen i if-satsen. Anropa så här:

    moveRight();

Skriv även kod för funktionerna

    moveLeft()
    moveUp()
    moveDown()


Lägg till guld
==============

Lägg till kod under:

    //interna hjälpfunktioner

Deklarera en funktion `addGold`.

I metodkroppen lägger du till koden för att lägga till en guldhög:

    //lägg till ett span-element i elementet med id frame
    $("#frame").append("<span class='gold'></span>");
    //variabeln left får ett slumptal 0, 32, 64, ..., 480
    //Använd Math.random() och Math.ceil() kolla dokumentationen

    //variabeln top får också ett slumptal i samma intervall

    //sätt css-värdet left till det slumpade värdet
    //du har gjort det vid styrning med piltangenter

    //sätt css-värdet top på samma sätt

Anropa metoden efter metoddeklarationen.

Mer guld kommer vi att lägga till senare. Vi ska bara se att det fungerar först.

Detektera kollisioner
=====================
Det är dags att göra så att hjälten kan plocka upp guldet.

Funktionen hasSamePosition
--------------------------
Under

    //externa hjälpfunktioner

deklarerar du metoden `hasSamePosition`. En del av koden följer nedan.

    //externa hjälpfunktioner
    /**
     * @param object1 är ett jQuery-objekt
     * @param object2 är också ett jQuery-objekt
     * @return true om de båda objekten har samma position,
     * annars false
     */
    const hasSamePosition = function (object1, object2) {
        "use strict";
        //om objekt 1 och 2 har samma css-left-värde och
        //samma css-top-värde
            return true;
        //annars
            return false;
    };

Kod för kollisionsdetektering
----------------------------

Anropa funktionen `hasSamePosition` på följande vis:

    hasSamePosition($("#hero"),$(".gold"))

Efter if-satsen för förflyttning av hjälten lägger du till följande kod. Översätt själv till JavaScript.

    om hero och gold har samma position
        $(".gold").remove();
        //kod för att öka poängen, men det är näst utmaning


Poäng
=====
Under

    //variabler i spelet

deklarera du en variabel `score`. Sätt den till 0.

Under

    //interna hjälpfunktioner

deklarerar du en funktion `increaseScore`. Funktionen ska ta en parameter extraPoints. Följande ska hända i funktionen.

    Öka score med ett.
    Sätt texten på elementet med id score till score.

Tips:

    $("#mitt-id").text("min text")
    //sätter texten till "min text" på elementet med id mitt-id

Kom ihåg att anropa metoden på lämpligt ställe, se föregående uppgift om du behöver hjälp.

Mer guld
=========

Lägg en for-slinga runt koden och byt

    $(".gold")

mot

    $(".gold:last-child")

Tips:

    let i;
    for (i = 0; i < 10; i = i + 1) {
        //kod som ska upprepas här
    }

Skriv om kollisionsdetekteringen
--------------------------------
Den första versionen av kollisionsdetektering förutsätter att det bara finns ett guld-element på spelplanen. Nu ska vi göra det mer generellt.

Koden ges i sin helhet nedan. Var och en av guld-elementen på spelplanen gås igenom.

    $(".gold").each(function (index, element) {
        let doIntersect = hasSamePosition($(element), $("#hero"));
        if (doIntersect) {
            element.remove();
            increaseScore(1);
        }
    });

Vägg
====
Skriv en metod `addWalls()`. Den är lik metoden `addGold()`.

Börja med att bara lägga till ett vägg-element och testa att det fungerar.

CSS-klassen `wall` används.

Anropa metoden efter definitionen.

Lägg till fler vägg-element efter eget tycke och smak.
Använd for-slingor för att lägga till längre väggsektioner.

Kollision med vägg
------------------
Hjälten ska inte kunna gå in i en vägg.

Koden för detta läggs i if-satsen som sköter förflyttningen.

    Om hjälten flyttas in i en vägg flyttar den tillbaka igen.

Mer tydligt.

    om key är vänsterpil
        flytta 32 pixlar till vänster
        om utanför spelpanen
            flytta in igen
        //nytt
        för varje vägg-element
            om hjälten och elementet har samma position
                flytta 32 pixlar till höger

Andra lösningar är möjliga.

Monster
=======
Skriv en metod `addMonsters()`, som lägger till monster.

Monstrens CCS-klass heter danger.

Om hjälten går in i ett monster så avslutas spelet.

Kollision med monster
----------------------

Kolla efter kollision med monster som för guld.

    För varje monster
        Om hjälten och monstret har samma position
            Flytta texten Game over till mitten av spelpanen.
            (Texten är gömd utan för bild till vänster.)
            Elementet med texten har id: game-over

Mitten fås på följande vis

    #frame:s bredd / 2 - #game-over:s bredd / 2
    Använd Math.floor() för att kapa decimalerna.

Förhindra att hjälten kan flytta
--------------------------------

Lägg till en variabel `isPlaying` under

    //variabler i spelet

Låt variabeln få värdet `true`.

Sätt den till false om hjälten går in i ett monster.

Lägg all kod som finns i

    document.onkeydown = function (event) {

i en if-sats där innehållet görs om

    isPlaying

har värdet `true`.








