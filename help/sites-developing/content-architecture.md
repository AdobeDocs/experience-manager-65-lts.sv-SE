---
title: Innehållsarkitektur
description: Tips för att skapa ditt innehåll (tips - allt är innehåll)
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: best-practices
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: eb47f730-ac26-47a0-9bd7-3b7e94c79ecd
source-git-commit: cc96a14ebaf9f895a798b5f4904f5b4769b990bb
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# Innehållsarkitektur{#content-architecture}

## Följ David modell {#follow-david-s-model}

David Nuescheler skrev Davids modell för flera år sedan, men hans idéer håller fortfarande i sanning idag. De viktigaste grundsatserna i Davids modell är följande:

* Data kommer först, strukturen senare. Kanske.
* Driv upp innehållshierarkin och låt den inte ske.
* Arbetsytor är för `clone()`, `merge()` och `update()`.
* Se upp för samma namn som syskon.
* Referenser kan anses vara skadliga.
* Filer är filer.
* ID:n är onda.

David&#39;s Model finns på Jackrabbit wiki på [https://wiki.apache.org/jackrabbit/DavidsModel](https://wiki.apache.org/jackrabbit/DavidsModel).

### Allt är innehåll {#everything-is-content}

Allt ska lagras i databasen i stället för att förlita sig på separata datakällor från tredje part, som till exempel databaser. Ett sådant tillvägagångssätt gäller för redigerat innehåll, binära data som bilder, kod och konfigurationer. Det gör att vi kan använda en uppsättning API:er för att hantera allt innehåll och för att hantera kampanjen av det här innehållet genom replikering. Du får också en enda källa för säkerhetskopiering, loggning och så vidare.

### Använd designprincipen&quot;innehållsmodell först&quot; {#use-the-content-model-first-design-principle}

När du skapar en ny funktion börjar du alltid med att designa JCR-innehållsstrukturen först och tittar sedan på hur du läser och skriver ditt innehåll med standardservletarna för Sling. På så sätt kan du säkerställa att implementeringen fungerar bra med åtkomstkontrollsmekanismer som är klara att användas och du slipper generera onödiga CRUD-servrar.

### Var RESTful {#be-restful}

Definiera serverlets baserat på resourceTypes i stället för sökvägar. Med den här metoden kan du använda JCR-åtkomstkontroller, följa REST-principer och använda den resurs- och resurslösare som vi får i begäran. På så sätt kan du ändra de skript som återger URL:er på serversidan utan att ändra några URL:er på klientsidan. Den döljer även implementeringsinformation på serversidan från klienten för ökad säkerhet.

### Undvik att definiera nya nodtyper {#avoid-defining-new-node-types}

Nodtyper fungerar på en låg nivå i infrastrukturskiktet. De flesta kraven uppfylls genom att använda en `sling:resourceType` som tilldelats en `nt:unstructured`-, `oak:Unstructured`-, `sling:Folder`- eller `cq:Page`-nodtyp. Nodtyper motsvarar schemat i databasen och det kan vara dyrt att ändra nodtyperna längs vägen.

### Anta namnkonventioner i den gemensamma CR-rapporten {#adhere-to-naming-conventions-in-the-jcr}

Om du följer namnkonventioner blir kodbasen mer konsekvent, vilket minskar förekomsten av defekter och ökar hastigheten för utvecklare som arbetar i systemet. Följande konventioner används av Adobe i utvecklingen av AEM:

* Nodnamn

   * Bara små bokstäver.
   * Ordseparation med bindestreck.

* Egenskapsnamn

   * Kamerafodral, med början med en gemen bokstav.

* Komponenter (JSP/HTML)

   * Bara små bokstäver.
   * Ordseparation med bindestreck.
