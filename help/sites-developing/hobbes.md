---
title: Testa användargränssnittet
description: AEM tillhandahåller ett ramverk för automatisering av tester för AEM användargränssnitt
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: components, testing
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 73a3a6386a857eae1b7392d0fdf943b9b6799b33
workflow-type: tm+mt
source-wordcount: '754'
ht-degree: 2%

---

# Testa användargränssnittet{#testing-your-ui}

>[!NOTE]
>
>Från och med AEM 6.5 är testmiljön hobbes.js UI föråldrat. Adobe planerar inte att göra ytterligare förbättringar av programmet och rekommenderar kunderna att använda Selenium Automation.
>

AEM tillhandahåller ett ramverk för automatisering av tester för ditt AEM-gränssnitt. Med hjälp av ramverket kan du skriva och köra gränssnittstester direkt i en webbläsare. Ramverket innehåller ett JavaScript-API för att skapa tester.

AEM testramverk använder Hobbes.js, ett testbibliotek skrivet i JavaScript. Hobbes.js-ramverket utvecklades för testning av AEM som en del av utvecklingsprocessen. Ramverket är nu tillgängligt för allmän användning för testning av dina AEM-program.

>[!NOTE]
>
>Mer information om API:t finns i [dokumentationen](https://developer.adobe.com/experience-manager/reference-materials/6-5/test-api/index.html) för Hobbes.js.

## Testernas struktur {#structure-of-tests}

När du använder automatiska tester i AEM är följande termer viktiga att förstå:

| Åtgärd | En **åtgärd** är en specifik aktivitet på en webbsida, som att klicka på en länk eller en knapp. |
|---|---|
| Testfall | **Testfall** är en specifik situation som kan bestå av en eller flera **åtgärder**. |
| Test Suite | En **testsvit** är en grupp med relaterade **testfall** som tillsammans testar ett visst användningsfall. |

## Kör tester {#executing-tests}

### Visa testsviter {#viewing-test-suites}

Öppna testkonsolen för att se de registrerade testsviterna. Testpanelen innehåller en lista med testsviter och deras testfall.

Navigera till verktygskonsolen via **Global navigering > Verktyg > Åtgärder > Testning**.

![chlimage_1-63](assets/chlimage_1-63.png)

När du öppnar konsolen visas testsviterna till vänster tillsammans med ett alternativ för att köra alla sekventiellt. Utrymmet till höger som visas med en schackmönstrad bakgrund är en platshållare för hur sidinnehållet visas när testerna körs.

![chlimage_1-64](assets/chlimage_1-64.png)

### Köra en testsvit {#running-a-single-test-suite}

Testsviter kan köras individuellt. När du kör en testsvit ändras sidan allt eftersom testärenden och deras åtgärder körs och resultaten visas när testet har slutförts. Ikoner anger resultatet.

En bockmarkeringsikon anger att testet har slutförts:

![Kryssmarkeringsikon.](do-not-localize/chlimage_1-2.png)

En X-ikon anger att testet misslyckades:

![Det gick inte att testa ikonen som indikeras av ett X inuti en cirkel.](do-not-localize/chlimage_1-3.png)

Så här kör du en testsvit:

1. Klicka på namnet på det testfall som du vill köra på panelen Testa för att visa information om åtgärderna.

   ![chlimage_1-65](assets/chlimage_1-65.png)

1. Klicka på **Kör test**.

   ![En bild av knappen Kör test, som indikeras av en högerriktad pekare inuti en cirkel.](do-not-localize/chlimage_1-4.png)

1. Platshållaren ersätts med sidinnehåll när testet utförs.

   ![chlimage_1-66](assets/chlimage_1-66.png)

1. Granska resultatet av testfallet genom att trycka på eller klicka på beskrivningen för att öppna panelen **Resultat**. Om du trycker eller klickar på namnet på testfallet på panelen **Resultat** visas all information.

   ![chlimage_1-67](assets/chlimage_1-67.png)

### Köra flera tester {#running-multiple-tests}

Testsviter körs sekventiellt i den ordning som de visas i konsolen. Du kan fördjupa dig i ett test för att se de detaljerade resultaten.

![chlimage_1-68](assets/chlimage_1-68.png)

1. Klicka på knappen **Kör alla tester** eller knappen **Kör tester** under namnet på testsviten som du vill köra på testpanelen.

   ![En bild av knappen Kör alla test och knappen Kör test, som indikeras av en högerriktad pekare inuti en cirkel.](do-not-localize/chlimage_1-5.png)

1. Om du vill visa resultatet av varje testfall klickar du på titeln på testfallet. Om du klickar på namnet på testet på panelen **Resultat** visas all information.

   ![chlimage_1-69](assets/chlimage_1-69.png)

## Skapa och använda en enkel testprogramsvit {#creating-and-using-a-simple-test-suite}

Följande procedur hjälper dig att skapa och köra en testsvit med [We.Retail-innehåll](/help/sites-developing/we-retail.md), men du kan enkelt ändra testet så att det använder en annan webbsida.

Mer information om hur du skapar egna testsviter finns i [Hobbes.js API-dokumentationen](https://developer.adobe.com/experience-manager/reference-materials/6-5/test-api/index.html).

1. Öppna CRXDE Lite. ([https://localhost:4502/crx/de](https://localhost:4502/crx/de))
1. Högerklicka på mappen `/etc/clientlibs` och klicka på **Skapa > Skapa mapp**. Skriv `myTests` som namn och klicka på **OK**.
1. Högerklicka på mappen `/etc/clientlibs/myTests` och klicka på **Skapa > Skapa nod**. Använd följande egenskapsvärden och klicka sedan på **OK**:

   * Namn: `myFirstTest`
   * Typ: `cq:ClientLibraryFolder`

1. Lägg till följande egenskaper i noden myFirstTest:

   | Namn | Typ | Värde |
   |---|---|---|
   | `categories` | Sträng[] | `granite.testing.hobbes.tests` |
   | `dependencies` | Sträng[] | `granite.testing.hobbes.testrunner` |

   >[!NOTE]
   >
   >**Endast AEM Forms**
   >
   >
   >Om du vill testa adaptiva formulär lägger du till följande värden i kategorierna och beroenden. Till exempel:
   >
   >
   >**kategorier**: `granite.testing.hobbes.tests, granite.testing.hobbes.af.commons`
   >
   >
   >**beroenden**: `granite.testing.hobbes.testrunner, granite.testing.hobbes.af`

1. Klicka på **Spara alla**.
1. Högerklicka på noden `myFirstTest` och klicka på **Skapa > Skapa fil**. Ge filen namnet `js.txt` och klicka på **OK**.
1. Ange följande text i filen `js.txt`:

   ```
   #base=.
   myTestSuite.js
   ```

1. Klicka på **Spara alla** och stäng sedan filen `js.txt`.
1. Högerklicka på noden `myFirstTest` och klicka på **Skapa > Skapa fil**. Ge filen namnet `myTestSuite.js` och klicka på **OK**.
1. Kopiera följande kod till filen `myTestSuite.js` och spara sedan filen:

   ```
   new hobs.TestSuite("Experience Content Test Suite", {path:"/etc/clientlibs/myTests/myFirstTest/myTestSuite.js"})
      .addTestCase(new hobs.TestCase("Navigate to Experience Content")
         .navigateTo("/content/we-retail/us/en/experience/arctic-surfing-in-lofoten.html")
      )
      .addTestCase(new hobs.TestCase("Hover Over Topnav")
         .mouseover("li.visible-xs")
      )
      .addTestCase(new hobs.TestCase("Click Topnav Link")
         .click("li.active a")
   );
   ```

1. Gå till **testkonsolen** och testa testsviten.
