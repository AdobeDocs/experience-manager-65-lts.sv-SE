---
title: Anteckningar vid redigering av en sida
description: Om du lägger till innehåll på webbplatsens sidor kan det ofta bli fråga om diskussioner innan de faktiskt publiceras. För att underlätta detta kan du lägga till en anteckning i många komponenter som är direkt relaterade till innehållet.
page-status-flag: de-activated
contentOwner: Chris Bohnert
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: page-authoring
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Anteckningar vid redigering av en sida{#annotations-when-editing-a-page}

Om du lägger till innehåll på webbplatsens sidor kan det ofta bli fråga om diskussioner innan de faktiskt publiceras. För att underlätta detta kan du lägga till en anteckning i många komponenter som är direkt relaterade till innehållet (till skillnad från att till exempel utforma det).

En anteckning placerar en färgmarkör/anteckning på sidan. Med anteckningen kan du (eller andra användare) lämna kommentarer och/eller frågor till andra författare/granskare.

>[!NOTE]
>
>Definitionen av en enskild komponenttyp avgör om det är möjligt att lägga till en anteckning (eller inte) i instanser av den komponenten.

>[!NOTE]
>
>Anteckningar som skapas i det klassiska användargränssnittet visas i det pekoptimerade användargränssnittet. Skisser är dock användargränssnittsspecifika och visas endast i det användargränssnitt där de skapades.

>[!CAUTION]
>
>Om du tar bort en resurs (till exempel ett stycke) tas alla anteckningar och skisser som är kopplade till resursen bort, oavsett deras position på sidan som helhet.

>[!NOTE]
>
>Beroende på dina behov kan du även utveckla ett arbetsflöde för att skicka meddelanden när anteckningar läggs till, uppdateras eller tas bort.

## Anteckningar {#annotations}

Beroende på styckedesignen är anteckningen antingen tillgänglig som ett alternativ på snabbmenyn (vanligtvis högerknappen när den placeras över det önskade stycket) eller som en knapp på styckets redigeringsfält.

I båda fallen väljer du **Anteckning**. En färgad anteckning används i stycket. Du är direkt i redigeringsläget och kan lägga till text direkt:

![chlimage_1-137](assets/chlimage_1-137.png)

Du kan flytta anteckningen till en ny plats på sidan. Klicka på området för den övre kantlinjen, håll sedan ned och dra samtidigt anteckningen till den nya positionen. Det kan finnas var som helst på sidan, men det är ofta praktiskt att hålla det kopplat till stycket på något sätt.

Anteckningar (inklusive relaterade skisser) ingår också i kopierings-, klippnings- eller raderingsåtgärder som utförs på det stycke som de är kopplade till. För kopierings- eller klippåtgärder behåller anteckningens (och relaterade skisser) position i förhållande till det ursprungliga stycket.

Storleken på anteckningen kan också ökas eller minskas genom att dra i det nedre högra hörnet.

I spårningssyfte anger sidfotsraden den användare som skapade anteckningen och datumet. Efterföljande författare kan redigera samma anteckning (sidfoten uppdateras) eller skapa en annan anteckning för samma stycke.

Bekräftelse krävs när du väljer att ta bort anteckningen (om du tar bort en anteckning tas även alla skisser som är kopplade till anteckningen bort).

Med de tre ikonerna längst upp till vänster kan du minimera anteckningen (tillsammans med relaterade skisser), ändra färg och lägga till skisser.

>[!NOTE]
>
>Anteckningar visas bara i redigeringsläget i författarmiljön.
>
>De visas inte i en publiceringsmiljö eller i förhandsgransknings- eller designläge som är tillgängliga i en författarmiljö.

>[!NOTE]
>
>Anteckningar kan inte läggas till på en sida som har låsts av en annan användare.

## Anteckningskisser {#annotation-sketches}

>[!NOTE]
>
>Skisser är inte tillgängliga i Internet Explorer så:
>
>* ikonen inte visas.
>* befintliga skisser som skapats i en annan webbläsare visas inte.
>

Skisser är en funktion av anteckningar som gör att du kan skapa enkla linjeregrafik var som helst i webbläsarfönstret (synlig del):

![chlimage_1-138](assets/chlimage_1-138.png)

* Markören ändras till en korstråd när du är i skissläge. Du kan rita flera distinkta linjer.
* Skisslinjen återspeglar anteckningsfärgen och kan vara antingen:

   * frihand

     standardläget. Avsluta genom att släppa musknappen.

   * rak:

     håll ned `ALT` och klicka på start- och slutpunkterna. Avsluta med ett dubbelklick.

* När du har avslutat skissläget kan du klicka på en skiss för att markera den.
* Flytta en skiss genom att markera skissen och sedan dra den till önskad plats.
* En skiss täcker över innehållet. Det innebär att du inte kan klicka på det underliggande stycket i skissens fyra hörn. Om du till exempel måste redigera eller komma åt en länk. Om det här blir ett problem (du till exempel har en skiss som täcker ett stort område på sidan) minimerar du sedan lämplig anteckning, eftersom detta även minimerar alla relaterade skisser och ger dig tillgång till det underliggande området.
* Om du vill ta bort en enskild skiss markerar du önskad skiss och trycker sedan på tangenten **Delete** (**fn**-**backspace** på en Mac).

* Om du flyttar, eller kopierar, ett stycke flyttas eller kopieras även alla relaterade anteckningar och skisser, och deras placering i förhållande till stycket ändras inte.
* Om du tar bort en anteckning tas även alla skisser som är kopplade till den anteckningen bort.
