---
title: Datamodellering - David Nuescheler's Model
description: David Nueschelers rekommendationer för innehållsmodellering
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: introduction
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: ed55397c-6777-4d00-a6e2-8b1b025e533f
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1775'
ht-degree: 0%

---

# Datamodellering - David Nuescheler&#39;s Model{#data-modeling-david-nuescheler-s-model}

## Source {#source}

Följande detaljer är idéer och kommentarer från David Nuescheler.

David var en av grundarna och CTO på Day Software AG, en ledande leverantör av programvara för global innehållshantering och innehållsinfrastruktur, som förvärvades av Adobe 2010. David är nu medlem i och Vice President för Enterprise Technology på Adobe och leder också utvecklingen av JSR-170, Java™ Content Repository (JCR), teknikstandarden för content management.

Ytterligare uppdateringar kan också visas på [https://cwiki.apache.org/confluence/display/jackrabbit/DavidsModel](https://cwiki.apache.org/confluence/display/jackrabbit/DavidsModel).

## Introduktion från David {#introduction-from-david}

I olika diskussioner fann jag att utvecklare är lite illa ute med de funktioner som JCR presenterade vid innehållsmodellering. Det finns ännu ingen guide och liten erfarenhet av att modellera innehåll i en databas och varför en innehållsmodell är bättre än den andra.

I den relationella världen har programvarubranschen erfarenhet av att modellera data, men den är fortfarande i ett tidigt skede för innehållsarkivet.

Jag vill börja fylla denna tomrumpa genom att uttrycka mina åsikter om hur innehåll bör modelleras. Jag hoppas att detta en dag kan förvandlas till något mer meningsfullt för utvecklarens community, vilket inte bara är &quot;min åsikt&quot; utan något som är mer allmänt tillämpligt. Så tänk på det här som min snabbaste början på det.

>[!NOTE]
>
>Ansvarsfriskrivning: Dessa riktlinjer uttrycker mina personliga, ibland kontroversiella åsikter. Jag ser fram emot att diskutera dessa riktlinjer och förfina dem.

## Sju enkla regler {#seven-simple-rules}

### Regel 1: Data först, struktur senare. Kanske. {#rule-data-first-structure-later-maybe}

#### Förklaring {#explanation-1}

Jag rekommenderar att man inte behöver bekymra sig om en deklarerad datastruktur i ett ERD-avseende. Inledningsvis.

Lär dig att älska inte:ostrukturerade (&amp; vänner) under utvecklingen.

Mitt slutresultat: Strukturen är dyr och det är ofta helt onödigt att uttryckligen deklarera strukturen till det underliggande lagringsutrymmet.

Det finns ett implicit kontrakt om strukturen som ditt program använder. Jag lagrar ändringsdatumet för ett blogginlägg i en lastModified-egenskap. Min app kan automatiskt läsa ändringsdatumet från samma egenskap igen, det finns ingen anledning att deklarera det uttryckligen.

Ytterligare databegränsningar som obligatoriska eller typ- och värdebegränsningar bör endast tillämpas där det är nödvändigt av dataintegritetsskäl.

#### Exempel {#example-1}

Ovanstående exempel på hur du använder en `lastModified` Date-egenskap på till exempel en bloggpost-nod innebär inte att det behövs en särskild nodtyp. Jag skulle definitivt använda `nt:unstructured` för mina blogginläggsnoder åtminstone från början. Allt jag gör i mitt bloggprogram är att visa datumet lastModified i alla fall (eventuellt &quot;order by&quot;). Jag bryr mig knappt om det är ett datum alls. Eftersom jag i alla fall har förtroende för att mitt bloggskrivande-program ska placera ett &quot;datum&quot; behöver jag inte deklarera ett `lastModified`-datum i form av en nodtyp.

### Regel 2: Kör innehållshierarkin, låt det inte ske. {#rule-drive-the-content-hierarchy-don-t-let-it-happen}

#### Förklaring {#explanation-2}

Innehållshierarkin är en värdefull tillgång. Låt det inte hända, designa det inte. Om du inte har ett &quot;bra&quot;, läsbart namn för en nod är det antagligen något du bör tänka om. Godtyckliga siffror är knappast ett&quot;bra namn&quot;.

Även om det kan vara enkelt att snabbt lägga in en befintlig relationsmodell i en hierarkisk modell, bör man tänka lite i den processen.

Jag tycker att om man tänker på åtkomstkontroll och inneslutning som bra drivrutiner för innehållshierarkin. Tänk på det som om det vore ditt filsystem. Du kan till och med använda filer och mappar för att modellera dem på den lokala hårddisken.

Personligen föredrar jag hierarkiska konventioner framför nodtypningssystemet först och introducerar typningen senare.

>[!CAUTION]
>
>Det sätt på vilket en innehållsdatabas är strukturerad kan även påverka prestanda. För bästa prestanda bör antalet underordnade noder som är kopplade till enskilda noder i en innehållsdatabas inte överstiga 1 000.
>
>Se [Hur mycket data kan CRX hantera?](https://helpx.adobe.com/experience-manager/kb/CrxLimitation.html)

#### Exempel {#example-2}

Jag skulle modellera ett enkelt bloggsystem enligt följande. Till att börja med bryr jag mig inte ens om vilka nodtyper jag använder just nu.

```xml
/content/myblog
/content/myblog/posts
/content/myblog/posts/what_i_learned_today
/content/myblog/posts/iphone_shipping

/content/myblog/comments/iphone_shipping/i_like_it_too
/content/myblog/comments/iphone_shipping/i_like_it_too/i_hate_it
```

Jag tror att en av de saker som blir uppenbart är att innehållets struktur förstås utifrån exemplet utan några ytterligare förklaringar.

Det som till att börja med kan vara oväntat är varför jag inte skulle lagra&quot;kommentarerna&quot; med&quot;posten&quot;, som beror på åtkomstkontroll som jag skulle vilja ha tillämpad på ett rimligt hierarkiskt sätt.

Med innehållsmodellen ovan kan jag enkelt låta den anonyma användaren&quot;skapa&quot; kommentarer, men behålla den anonyma användaren skrivskyddad för resten av arbetsytan.

### Regel 3: Arbetsytorna är för clone(), merge() och update(). {#rule-workspaces-are-for-clone-merge-and-update}

#### Förklaring {#explanation-3}

Om du inte använder metoderna `clone()`, `merge()` eller `update()` i ditt program är en enda arbetsyta antagligen den rätta vägen.

&quot;Motsvarande noder&quot; är ett koncept som definieras i JCR-specifikationen. Det handlar i princip om noder som representerar samma innehåll, i olika så kallade arbetsytor.

JCR introducerar det abstrakta begreppet Arbetsytor, vilket gör att många utvecklare inte vet vad de ska göra med dem. Jag vill föreslå att du använder arbetsytorna för att testa följande.

Om du har en avsevärd överlappning av&quot;motsvarande&quot; noder (i huvudsak noder med samma UUID) i flera arbetsytor kan du använda arbetsytorna på ett bra sätt.

Om det inte finns någon överlappning av noder med samma UUID använder du antagligen arbetsytor.

Använd inte arbetsytor för åtkomstkontroll. Synlighet för innehåll för en viss användargrupp är inget bra argument för att dela upp saker i olika arbetsytor. JCR har &quot;Åtkomstkontroll&quot; i innehållsdatabasen för att tillhandahålla den.

Arbetsytor är gränserna för referenser och frågor.

#### Exempel {#example-3}

Använd arbetsytor för exempelvis:

* v1.2 av projektet jämfört med v1.3 av projektet
* ett&quot;utvecklingstillstånd&quot;,&quot;QA&quot; och ett&quot;publicerat&quot; innehållstillstånd

Använd inte arbetsytor för exempelvis:

* användarens hemkataloger
* distinkt innehåll för olika målgrupper som public, private, local, ...
* e-postinkorgar för olika användare

### Regel 4: Se upp för samma namn på jämställda. {#rule-beware-of-same-name-siblings}

#### Förklaring {#explanation-4}

SNS (Same Name Siblings) har införts i specifikationen för att möjliggöra kompatibilitet med datastrukturer som är utformade för och uttryckta via XML och därför är värdefulla för JCR. SNS har dock en generell kostnad och komplexitet för databasen.

Alla sökvägar till innehållsdatabasen som innehåller en SNS i ett av dess sökvägssegment blir mycket mindre stabila. Om en SNS tas bort eller ordnas om påverkar det sökvägarna för alla andra SNS och deras barn.

För import av XML eller interaktion med befintlig XML kan SNS vara nödvändigt och användbart, men jag har aldrig använt SNS (och aldrig tänkt) i mina&quot;green field&quot;-datamodeller.

#### Exempel {#example-4}

Använd

```xml
/content/myblog/posts/what_i_learned_today
/content/myblog/posts/iphone_shipping
```

I stället för

```xml
/content/blog[1]/post[1]
/content/blog[1]/post[2]
```

### Regel 5: Hänvisningar anses vara skadliga. {#rule-references-considered-harmful}

#### Förklaring {#explanation-5}

Referenser innebär referensintegritet. Det är viktigt att förstå att referenser inte bara lägger till en extra kostnad för databasen som hanterar referensintegriteten, utan även är kostsamma ur ett flexibelt perspektiv.

Personligen använder jag bara referenser när jag egentligen inte kan hantera en farlig referens och i annat fall använda en sökväg, ett namn eller en sträng-UID för att referera till en annan nod.

#### Exempel {#example-5}

Låt oss anta att jag tillåter &quot;referenser&quot; från ett dokument (a) till ett annat dokument (b). Om jag modellerar den här relationen med hjälp av referensegenskaper innebär det att de två dokumenten är länkade på en databasnivå. Jag kan inte exportera/importera dokument (a) separat, eftersom referensegenskapens mål kanske inte finns. Även andra åtgärder som sammanfogning, uppdatering, återställning och kloning påverkas.

Därför skulle jag antingen modellera dessa referenser som &quot;svag referens&quot; (i JCR v1.0 så här böjer det sig i princip ned till strängegenskaper som innehåller målnodens uuid) eller bara använda en sökväg. Ibland är banan mer meningsfull att börja med.

Jag tror att det finns fall där ett system verkligen inte fungerar om en referens är farlig, men jag kan inte komma på ett bra &quot;verkligt&quot; men enkelt exempel från min direkta erfarenhet.

### Regel 6: Filer är filer. {#rule-files-are-files}

#### Förklaring {#explanation-6}

Om en innehållsmodell visar något som till och med luktar på fjärrbasis som en fil eller en mapp försöker jag använda (eller utöka från) `nt:file`, `nt:folder` och `nt:resource`.

Enligt min erfarenhet tillåter många generiska program interaktion med nt:folder och nt:files implicit och vet hur de ska hantera och visa dessa händelser om de har anrikats med ytterligare metainformation. En direkt interaktion med filserverimplementeringar som CIFS eller WebDAV som sitter ovanpå JCR blir till exempel implicit.

Som tumregel kan jag använda följande: Om du måste lagra filnamnet och mime-typen så är `nt:file`/ `nt:resource` en bra matchning. Om du kan ha flera &quot;filer&quot; kan det vara bra att lagra mappen int:folder.

Om du måste lägga till metainformation för resursen, kan vi säga &quot;författare&quot; eller &quot;beskrivning&quot;, utöka `nt:resource` inte `nt:file`. Jag utökar sällan ingen:file och utökar `nt:resource` ofta.

#### Exempel {#example-6}

Låt oss anta att någon vill ladda upp en bild till ett blogginlägg på:

```xml
/content/myblog/posts/iphone_shipping
```

Och den initiala utmattningsreaktionen kan vara att lägga till en binär egenskap som innehåller bilden.

Även om det finns bra användningsexempel för att bara använda en binär egenskap (låt oss säga att namnet är irrelevant och MIME-typen är implicit) rekommenderar jag i det här fallet följande struktur för bloggexemplet.

```xml
/content/myblog/posts/iphone_shipping/attachments [nt:folder]
/content/myblog/posts/iphone_shipping/attachments/front.jpg [nt:file]
/content/myblog/posts/iphone_shipping/attachments/front.jpg/jcr:content [nt:resource]
```

### Regel 7: ID:n är onda. {#rule-ids-are-evil}

#### Förklaring {#explanation-7}

I relationsdatabaser är id:n ett nödvändigt sätt att uttrycka relationer, så folk tenderar att använda dem även i innehållsmodeller. Mestadels av fel skäl.

Om innehållsmodellen är full av egenskaper som slutar på&quot;ID&quot; använder du förmodligen inte hierarkin korrekt.

Det är sant att vissa noder behöver en stabil identifiering under hela sin livscykel, men färre än du tror. Men `mix:referenceable` har en sådan inbyggd funktion i databasen, så det finns inget behov av att hitta ett extra sätt att identifiera en nod på ett stabilt sätt.

Tänk också på att objekt kan identifieras med hjälp av sökväg. Och så mycket som &quot;symlinks&quot; är mycket mer vettigt för de flesta användare än för hårda länkar i ett UNIX®-filsystem, är en sökväg bra om de flesta program ska referera till en målnod.

Viktigare är att det är **mix**:referenable, vilket betyder att det kan tillämpas på en nod vid den tidpunkt då du faktiskt måste referera till den.

Bara för att du vill kunna referera till en nod av typen Dokument innebär det inte att nodtypen Dokument måste sträcka sig från `mix:referenceable` på ett statiskt sätt. Det beror på att det kan läggas till dynamiskt i alla instanser av &quot;Dokument&quot;.

#### Exempel {#example-7}

Använd:

```xml
/content/myblog/posts/iphone_shipping/attachments/front.jpg
```

I stället för:

```xml
[Blog]
-- blogId
-- author
[Post]
-- postId
-- blogId
-- title
-- text
-- date
[Attachment]
-- attachmentId
-- postId
-- filename
+ resource (nt:resource)
```
