---
title: Hur skapar eller anpassar man adaptiva formulärteman?
description: Lär dig att skapa eller anpassa teman för adaptiva Forms Core-komponenter med BEM-specifikationer
keywords: skapa anpassningsbara formulär, skapa ett nytt tema, anpassa tema, ladda upp nytt tema, använda tema i formulär, ta bort ett tema, skapa ett tema i AEM 6.5-formulär
contentOwner: Khushwant Singh
topic-tags: Adaptive Forms
docset: aem65
role: Admin, Developer
feature: Adaptive Forms,Core Components
solution: Experience Manager, Experience Manager Forms
exl-id: 59b54622-55c4-4526-b584-c08bbd1d08bb
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1933'
ht-degree: 0%

---

# Skapa eller anpassa ett anpassat formulärtema {#introduction-to-theme}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components.html?lang=sv-SE) |
| AEM 6.5 | Denna artikel |


<!--**Applies to:** ✅ Adaptive Form Core Components ❎ [Adaptive Form Foundation Components](/help/forms/using/create-adaptive-form.md).-->

I AEM Forms 6.5 är ett tema ett AEM-klientbibliotek som du använder för att definiera format (utseende och känsla) för ett adaptivt formulär. Ett tema innehåller formatinformation för komponenterna och panelerna. Format innehåller egenskaper som bakgrundsfärger, lägesfärger, genomskinlighet, justering och storlek. När du använder ett tema återspeglas det angivna formatet i motsvarande komponenter. Ett tema hanteras separat utan referens till ett adaptivt formulär och kan återanvändas i flera adaptiva Forms.

## Tillgängliga teman {#available-theme}

AEM 6.5-miljön innehåller följande teman för Core Components based Adaptive Forms:

* [Arbetsytans tema](https://github.com/adobe/aem-forms-theme-canvas)
* [WKND-tema](https://github.com/adobe/aem-forms-theme-wknd)
* [EASEL-tema](https://github.com/adobe/aem-forms-theme-easel)
* [FSI-tema](https://github.com/adobe/aem-forms-theme-fsi)
* [Hälsovårdstema](https://github.com/adobe/aem-forms-theme-healthcare)
* [Offentligt tema](https://github.com/adobe/aem-forms-theme-public)
* [Tillverkningstema](https://github.com/adobe/aem-forms-theme-manufacturing)

## Om temats struktur {#understanding-structure-of-theme}

Ett tema är ett paket som omfattar CSS-filen, JavaScript-filer och resurser (som ikoner) som definierar formatet för din adaptiva Forms. Temat Adaptiv form följer en särskild organisation som består av följande komponenter:

* `src/theme.scss`: Den här mappen innehåller CSS-filen som har stor effekt på hela temat. Det fungerar som en central plats för att definiera och hantera temats format och beteende. Genom att redigera den här filen kan du göra ändringar som tillämpas överallt i temat, vilket påverkar både utseendet och funktionaliteten på dina adaptiva Forms- och AEM Sites-sidor.

* `src/site`: Den här mappen innehåller CSS-filer som används på en hel AEM-webbplats. De här filerna består av kod och format som påverkar den övergripande funktionaliteten och layouten på din AEM-webbplats. Alla ändringar som görs här återspeglas på alla sidor på webbplatsen.

* `src/components`: CSS-filerna i den här mappen är utformade för enskilda AEM kärnkomponenter. Varje dedikerad mapp för en komponent innehåller en `.scss`-fil som formaterar den specifika komponenten i ett adaptivt formulär. Filen `/src/components/button/_button.scss` innehåller till exempel formatinformation för den adaptiva Forms Button-komponenten.

  ![Arbetsytans temastruktur](/help/forms/using/assets/component-based-theme-folder-structure.png)

* `src/resources`: Den här mappen innehåller statiska filer som ikoner, logotyper och teckensnitt. Resurserna används för att förbättra temats visuella element och övergripande design.

## Skapa ett tema

AEM Forms 6.5 innehåller följande teman för Core Components based Adaptive Forms.

* [Arbetsytans tema](https://github.com/adobe/aem-forms-theme-canvas)
* [WKND-tema](https://github.com/adobe/aem-forms-theme-wknd)
* [EASEL-tema](https://github.com/adobe/aem-forms-theme-easel)
* [Offentligt tema](https://github.com/adobe/aem-forms-theme-public)
* [Tillverkningstema](https://github.com/adobe/aem-forms-theme-manufacturing)

Du kan [anpassa något av dessa teman och skapa ett ](#customize-a-theme-core-components)-tema.

## Anpassa ett tema {#customize-a-theme-core-components-based-adaptive-forms}

Att anpassa ett tema avser processen att ändra och anpassa utseendet på ett tema. När du anpassar ett tema ändrar du dess designelement, layout, färger, typografi och ibland den underliggande koden. På så sätt kan du skapa ett unikt och skräddarsytt utseende för webbplatsen eller tillämpningen, samtidigt som du bibehåller den grundläggande strukturen och funktionaliteten som temat ger.

>[!NOTE]
>
> * Använd Pakethanteraren för att distribuera ett tema på alla författare- och publiceringsinstanser.
> * Ett temaklientbibliotek importeras eller exporteras via Package Manager precis som andra paket.

### Krav för att anpassa ett tema {#prerequisites}

* [Aktivera adaptiva Forms Core-komponenter](/help/forms/using/enable-adaptive-forms-core-components.md) för din miljö.

* Installera den senaste versionen av [Apache Maven.](https://maven.apache.org/download.cgi) Apache Maven är ett automatiserat byggverktyg som ofta används för Java™-projekt. Genom att installera den senaste versionen får du de beroenden du behöver för att anpassa temat.

* Lär dig skapa ett [klientbibliotek i Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-65-lts/developing/introduction/clientlibs.html). AEM tillhandahåller klientbibliotek, som gör att du kan lagra din klientkod i databasen, ordna den i kategorier och definiera när och hur varje kodkategori ska skickas till klienten.

* Installera en vanlig textredigerare. Exempel: Microsoft® Visual Studio Code. Med en vanlig textredigerare som Microsoft® Visual Studio Code får du en användarvänlig miljö där du kan redigera och ändra temafiler.

* Kontrollera att AEM Forms-miljön är igång.

### Att tänka på när du anpassar ett tema {#consideration}

* Se till att du använder [Arketype-projektet som används för att aktivera adaptiva Forms Core-komponenter](/help/forms/using/enable-adaptive-forms-core-components.md) i din miljö för att anpassa dina teman.

* När du publicerar ett adaptivt formulär publiceras inte klientbiblioteken automatiskt vid publiceringsinstansen. Kontrollera att du manuellt publicerar klientbiblioteket som det hänvisas till i ett adaptivt formulär till dina publiceringsmiljöer.

* Adobe rekommenderar att du inte ändrar klassnamn för klientbibliotek.

### Anpassa ett tema {#customize-a-theme-core-components}

Att skapa eller anpassa ett tema är en process i flera steg. Utför stegen i listan för att skapa/anpassa temat:

1. [Klona ett tema](#clone-git-repo-of-theme)
1. [Anpassa temats utseende](#customize-the-theme)
1. [Temat är klart för lokal distribution](#generate-the-clientlib)
1. [Distribuera temat i en lokal miljö](#deploy-the-theme-on-a-local-environment)
1. [Distribuera temat i produktionsmiljön](#5-deploy-a-theme-on-your-production-environment)

<!--
 ![Theme Customization workflow](/help/forms/using/assets/custom-theme-steps.png)
-->

Exemplen i dokumentet är baserade på temat **Canvas**, men du kan klona ett tema och anpassa det med samma instruktioner. Dessa instruktioner kan användas för alla teman och du kan ändra teman efter dina specifika behov.

#### 1. Klona Git-databasen för temat {#clone-git-repo-of-theme}

Om du vill klona ett tema för Core Components based Adaptive Forms väljer du ett av följande teman:

* [Arbetsytans tema](https://github.com/adobe/aem-forms-theme-canvas)
* [WKND-tema](https://github.com/adobe/aem-forms-theme-wknd)
* [EASEL-tema](https://github.com/adobe/aem-forms-theme-easel)

Utför följande instruktioner för att klona ett tema:

1. Öppna kommandotolken eller terminalfönstret i den lokala utvecklingsmiljön.

1. Kör kommandot `git clone` om du vill klona ett tema.

   ```
      git clone [Path of Git Repository of the theme]
   ```

   Ersätt [Sökvägen till Git-databasen för temat] med den faktiska URL:en för motsvarande Git-databas för temat

   Om du till exempel vill klona arbetsytans tema kör du följande kommando:

   ```
      git clone https://github.com/adobe/aem-forms-theme-canvas
   ```

1. Välj **Lita på författarna till alla filer i den överordnade mappen** och klicka på **Ja, jag litar på författarna**.

När kommandot har körts har du en lokal kopia av temat på datorn i mappen `aem-forms-theme-canvas`.

#### 2. Anpassa temat {#customize-the-theme}

Du kan anpassa enskilda komponenter eller göra ändringar på temanivå med hjälp av temats globala variabler. När du ändrar globala variabler får du en överlappande effekt på alla enskilda komponenter. Du kan till exempel använda globala variabler för att ändra kantfärgen på alla komponenter i ett adaptivt formulär eller tillämpa en levande fyllningsfärg på knapparna Call to Action (CTA). Du kan:

* [Ange format för temanivåer](#theme-customization-global-level)

* [Ange format för komponentnivå](#component-based-customization)

##### Ange format för temanivåer {#theme-customization-global-level}

Filen `variable.scss` innehåller temats globala variabler. Genom att uppdatera dessa variabler kan du göra formatrelaterade ändringar på temanivå. Så här använder du format på temanivå:

1. Öppna filen `<your-theme-sources>/src/site/_variables.scss` för redigering.
1. Ändra värdet för alla egenskaper. Standardfelfärgen är t.ex. röd. Om du vill ändra felfärgen från rött till blått ändrar du färghexkoden för variabeln `$error`. Exempel: `$error: #196ee5`.

   ![Exempel: Felfärgen är blå](/help/forms/using/assets/theme-level-changes.png)

1. Spara och stäng filen.


På samma sätt kan du använda filen `variable.scss` för att ange teckensnittsfamilj och -typ, tema- och teckensnittsfärger, teckensnittsstorlek, temaavstånd, felikon, temagränsformat och mer variabel som påverkar flera adaptiva formulärkomponenter.

##### Ange format för komponentnivå {#component-based-customization}

Du kan också anpassa teckensnitt, färg, storlek och andra CSS-egenskaper för specifika komponenter i den adaptiva formulärkärnan, som knappar, kryssrutor, behållare, sidfötter och mycket annat. Genom att redigera den CSS-fil som är kopplad till den specifika komponenten kan du anpassa dess format efter din organisations varumärke. Så här anpassar du en komponents stil:

1. Öppna filen `<your-theme-sources>/src/components/<component>/<component.scss>` för redigering. Om du till exempel vill ändra teckenfärgen för knappkomponenten öppnar du filen `<your-theme-sources>/src/components/button/button.scss` .
1. Ändra värdet enligt dina önskemål. Om du till exempel vill ändra färgen på knappkomponenten vid mushovring till Grön, ändrar du värdet för egenskapen `color: $white` i klassen `cmp-adaptiveform-button__widget:hover` till hexkoden 12b453 eller någon annan grön nyans. Den färdiga koden ser ut så här:

   ```
    .cmp-adaptiveform-button__widget:hover {
    background: $dark-gray;
    color: #12b453;
    }
   ```


1. Spara och stäng filen.

<!--

   ![Example: Hover color set to green](/help/forms/using/assets/button-customization.png)

-->

>[!NOTE]
>
> När ett format definieras både på tema- och komponentnivå prioriteras det format som definieras på komponentnivå.

#### 3. Temat är klart för distribution {#generate-the-clientlib}

Om du vill distribuera ett tema till en AEM-instans måste det konverteras till ett klientbibliotek. Så här konverterar du temat till ett klientbibliotek:

1. Öppna kommandotolken eller terminalfönstret.
1. Navigera till mappen `<your-theme-sources>`. Exempel: `C:\aem-forms-theme-canvas`
1. Kör följande kommando:

   ```
      npm run create-clientlib --category=adaptiveform.theme.[yourtheme]
   ```

   Ersätt `[yourtheme]` med namnet på det anpassade temat. Om namnet på det anpassade temat är `customcanvastheme` kör du följande kommando

   ```
       npm run create-clientlib --category=adaptiveform.theme.customcanvastheme
   ```

   När kommandot har körts skapas en klientbiblioteksmapp på `themerepo\theme-clientlibs\[yourtheme]`.

   ![Generering av klientbibliotek](/help/forms/using/assets/clientlib_created.png)


   ![Klientbibliotekets plats](/help/forms/using/assets/adaptiveform.theme.easel.png)

#### 4. Använd temat i en lokal miljö {#deploy-the-theme-on-a-local-environment}

Så här distribuerar du temat till din lokala utvecklings- eller testmiljö:

1. Kopiera klientbiblioteket, som skapades i föregående avsnitt, till ditt Archetype-projekt på följande sökväg:

   `/ui.apps/src/main/content/jcr_root/apps/[AEM Archetype Project Folder]/clientlibs/<yourtheme>`

1. Öppna kommandotolken eller terminalen.
1. Gå till rotkatalogen för ditt AEM Archetype-projekt, det projekt som används för att aktivera kärnkomponenter för adaptiva formulär.
1. Kör följande kommando för att distribuera det anpassade temat i din miljö:

   `mvn clean install`

   ![Klientbiblioteksbygge](/help/forms/using/assets/mvndeploy.png)

<!--

#### 5. Test the theme with a local Adaptive Form {#test-the-theme-with-a-local-adaptive-form}

To apply and test the customized theme with an Adaptive Form:

**Apply theme while creating an Adaptive Form**

1. Log in to your AEM Forms author instance. 

1. Select **Adobe Experience Manager** > **Forms** > **Forms & Documents**.

1. Click **Create** > **Adaptive Forms**. The wizard for creating Adaptive Form opens.

1. Select the core component template in the **Source** tab.
1. Select the theme in the **Style** tab.
1. Click **Create**.

An Adaptive Form with the selected theme is created. 

**Apply theme to an existing Adaptive Form**

1. Log in to your AEM Forms author instance. 

1. Select **Adobe Experience Manager** > **Forms** > **Forms & Documents**.

1. Select an Adaptive Form and click Properties. 

1. For the **Theme Client Library** option, select the theme. 

1. Click **Save & Close**.

The selected theme is applied to the Adaptive Form. 
-->

#### 5. Använd ett tema i produktionsmiljön {#deploy-theme}

När du har testat temat på den lokala utvecklingsmiljön kan du fortsätta att distribuera temat till produktionsmiljöerna, inklusive både författaren och publiceringsinstanser. Så här distribuerar du temat i dina produktionsmiljöer:

1. Logga in i din AEM-miljö.
1. Öppna Pakethanteraren. Standardwebbadressen är `https://localhost:4502/crx/packmgr/index.jsp`.
1. Klicka på **Överför paket** och klicka på **Bläddra**.
1. Navigera till och markera `[AEM Archetype Project Folder]\all\target[appid].all-[version].zip`. Klicka på **Öppna**.
1. Klicka på Installera. Upprepa steget i alla produktionsmiljöer.


När paketet har installerats är temat tillgängligt för val.

![Temaklientbibliotek](/help/forms/using/assets/themeclientlibrary.png)

>[!NOTE]
>
>
> Om du får problem med att komma åt inloggningsdialogrutan på en publiceringsinstans för att installera paketet via pakethanteraren kan du försöka logga in via följande URL: `http://[Publish Server URL]:[PORT]/system/console`. Detta ger dig åtkomst till att logga in på Publish-instansen, så att du kan fortsätta med installationsprocessen.

## Använda ett tema i ett anpassat formulär {#using-theme-in-adaptive-form}

Steg för att tillämpa ett tema på ett adaptivt formulär är:

1. Logga in på din lokala AEM-författarinstans.
1. Ange dina inloggningsuppgifter på Experience Manager inloggningssida. Välj **Adobe Experience Manager** > **Forms** > **Forms &amp; Documents**.
1. Klicka på **Skapa** > **Adaptiv Forms**.
1. Välj en anpassad Forms Core Components-mall och klicka på **Nästa**. **Lägg till egenskaper** visas
1. Ange **namnet** för ditt adaptiva formulär.


   >[!NOTE]
   >
   > * Som standard är temat `adaptiveform.theme.canvas3` valt.
   > * Du kan välja ett annat tema i listrutan **Temaklientbibliotek**.

1. Klicka på **Skapa**.

Adaptiva formulärteman används som en del av en adaptiv formulärmall för att definiera format när du skapar ett adaptivt formulär.

## Ta bort ett tema {#delete-a-theme}

Så här tar du bort oanvända eller oönskade teman:

1. Logga in på din Author-instans.
1. Öppna `http://[Publish Server URL]:[PORT]/crx/de/index.jsp`
1. Navigera till `apps/[AEM Archetype Project Folder]/clientlibs/[yourtheme]`.
1. Ta bort temamappen och spara ändringarna.


## Frågor och svar {#faq}

**F:** Vilken anpassning prioriterar när du gör anpassningar i en temamapp på både global nivå och komponentnivå?

**Ans:** När en stil definieras på både tema- och komponentnivå har den stil som definieras på komponentnivå företräde.

**F:** Vilka åtgärder ska vidtas om det anpassade temat inte syns i **[!UICONTROL Theme Client Library]**?

**Ans:** Om ditt anpassade tema inte visas i listrutan **[!UICONTROL Theme Client Library]** gör du så här:

1. Navigera till den plats där du har lagt till ditt anpassade temaklientbibliotek. Den rekommenderade sökvägen är `/ui.apps/src/main/content/jcr_root/apps[AEM Archetype Project Folder]/clientlibs/<yourtheme>`.

1. Öppna filen `.content.xml` och inkludera följande metadata:

   ```
       formstheme:true
       allowproxy:true
   ```

1. Spara filen och distribuera temat igen.

## Se även

* [Skapa en grundkomponentbaserad adaptiv form](create-an-adaptive-form-core-components.md)
* [Använd regelredigeraren för att lägga till dynamiskt beteende i formulär](rule-editor.md)
* [Skapa eller anpassa teman för grundkomponentbaserade adaptiva Forms](create-or-customize-themes-for-adaptive-forms-core-components.md)
* [Skapa en mall för Core Components-baserade Adaptive Forms](template-editor.md)
* [Skapa eller lägga till ett anpassat formulär på en AEM Sites-sida eller ett Experience Fragment](create-or-add-an-adaptive-form-to-aem-sites-page.md)
* [Exempelmallar för teman och formulärdatamodeller](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/sample-themes-templates-form-data-models-core-components.html?lang=sv-SE)
