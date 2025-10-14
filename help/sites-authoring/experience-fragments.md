---
title: Upplevelsefragment
description: Upplev fragment vid framtagning av Adobe Experience Manager Sites.
solution: Experience Manager, Experience Manager Sites
feature: Authoring,Experience Fragments
role: User
exl-id: 97736093-021a-4487-8818-c0e3f1e2b4e5
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 3%

---

# Upplevelsefragment{#experience-fragments}

I Adobe Experience Manager (AEM) är ett Experience Fragment en grupp med en eller flera komponenter, inklusive innehåll och layout, som kan refereras till på sidor. De kan innehålla alla komponenter.

An Experience Fragment:

* Är en del av en upplevelse (sida).
* Kan användas på flera sidor (som baseras på redigerbara mallar).
* Är baserad på en mall (endast redigerbar) för att definiera struktur och komponenter.
* Den här mallen används för att skapa *rotsidan* i Experience Fragment.
* Består av en eller flera komponenter med layout i ett styckesystem.
* Kan innehålla andra upplevelsefragment.
* Kan kombineras med andra komponenter (inklusive andra Experience Fragments) för att skapa en komplett sida (upplevelse).
* En eller flera varianter kan skapas baserat på rotsidan.
* Dessa variationer kan dela innehåll och/eller komponenter.
* Kan delas upp i byggstenar som kan användas i flera varianter av fragmentet.

Du kan använda Experience Fragments:

* Om en författare vill återanvända delar av en sida (ett fragment av en upplevelse) måste de kopiera och klistra in det fragmentet. Att skapa och underhålla dessa klipp-och-klistra-upplevelser är tidskrävande och leder ofta till användarfel. Upplevelsefragment eliminerar behovet av att kopiera/klistra in.
* För att stödja CMS headless-fallstudier. Författare vill bara använda AEM för att skapa, men inte för att leverera till kunden. Ett system/kontaktyta från tredje part skulle förbruka upplevelsen och sedan leverera till slutanvändaren.
* Med [Multi Site Management (MSM)](/help/sites-administering/msm.md); som en Experience Fragment är en del av en sida. Detta gäller både de enskilda fragmenten och de mappar de finns i.

>[!NOTE]
>
>Skrivåtkomst för upplevelsefragment kräver att användarkontot är registrerat i gruppen:
>
>    `experience-fragments-editors`
>
>Kontakta systemadministratören om du har problem.

## När ska ni använda upplevelsefragment? {#when-should-you-use-experience-fragments}

Experience Fragments ska användas:

* När ni vill återanvända upplevelser.

   * Upplevelser som återanvänds med samma eller liknande innehåll

* När du använder AEM som en plattform för innehållsleverans för tredje part.

   * Alla lösningar som vill använda AEM som plattform för innehållsleverans
   * Bädda in innehåll i kontaktpunkter från tredje part

* Om du har en upplevelse med olika variationer eller renderingar.

   * Kanal- eller kontextspecifika varianter
   * Upplevelser som är bra att gruppera (till exempel en kampanj med olika upplevelser i olika kanaler)

* När du använder Omnichannel Commerce.

   * Dela e-handelsrelaterat innehåll på [sociala medier](/help/sites-developing/experience-fragments.md#social-variations)-kanaler i stor skala
   * Göra kontaktytor transaktionella

## Organisera dina upplevelsefragment {#organizing-your-experience-fragments}

Det rekommenderas att
* använda mappar för att ordna dina upplevelsefragment,

* [konfigurera tillåtna mallar för dessa mappar](#configure-allowed-templates-folder).

Genom att skapa mappar kan du:

* skapa en meningsfull struktur för era Experience Fragments, till exempel enligt klassificering

  >[!NOTE]
  >
  >Det är inte nödvändigt att anpassa strukturen för dina Experience Fragments till sidstrukturen på din plats.

* [allokera tillåtna mallar på mappnivå](#configure-allowed-templates-folder)

  >[!NOTE]
  >
  >Du kan använda [mallredigeraren](/help/sites-authoring/templates.md) för att skapa en egen mall.

WKND-projektet strukturerar vissa Experience Fragments enligt `Contributors`. Den struktur som används visar också hur andra funktioner, som Multi Site Management (inklusive språkkopior), kan användas.

Se:

`http://localhost:4502/aem/experience-fragments.html/content/experience-fragments/wknd/language-masters/en/contributors/kumar-selveraj/master`

![Mappar för Experience Fragments](/help/sites-authoring/assets/xf-folders.png)

## Skapa och konfigurera en mapp för dina Experience Fragments {#creating-and-configuring-a-folder-for-your-experience-fragments}

Om du vill skapa och konfigurera en mapp för dina Experience Fragments bör du:

1. [Skapa en mapp](/help/sites-authoring/managing-pages.md#creating-a-new-folder).

1. [Konfigurera tillåtna Experience Fragment-mallar för den mappen](#configure-allowed-templates-folder).

>[!NOTE]
>
>Det går också att konfigurera [tillåtna mallar för din instans](#configure-allowed-templates-instance), men den här metoden rekommenderas **inte** eftersom värdena kan skrivas över vid uppgradering.

### Konfigurera tillåtna mallar för mappen {#configure-allowed-templates-folder}

>[!NOTE]
>
>Det här är den rekommenderade metoden för att ange **tillåtna mallar** eftersom värdena inte skrivs över vid uppgradering.

1. Navigera till den obligatoriska mappen **Experience Fragments**.

1. Markera mappen och sedan **Egenskaper**.

1. Ange det reguljära uttrycket för hämtning av de nödvändiga mallarna i fältet **Tillåtna mallar**.

   Till exempel:
   `/conf/(.*)/settings/wcm/templates/experience-fragment(.*)?`

   Se:
   `http://localhost:4502/mnt/overlay/cq/experience-fragments/content/experience-fragments/folderproperties.html/content/experience-fragments/wknd`

   ![Upplevelsefragmentegenskaper - tillåtna mallar](/help/sites-authoring/assets/xf-folders-templates.png)

   >[!NOTE]
   >
   >Mer information finns i [Mallar för Experience Fragments](/help/sites-developing/experience-fragments.md#templates-for-experience-fragments).

1. Välj **Spara och stäng**.

### Konfigurera tillåtna mallar för din instans {#configure-allowed-templates-instance}

>[!CAUTION]
>
>Du bör inte ändra **Tillåtna mallar** med den här metoden eftersom de angivna mallarna kan skrivas över vid uppgradering.
>
>Använd den här dialogrutan endast i informationssyfte.

1. Navigera till den nödvändiga konsolen **Experience Fragments**.

1. Välj **Konfigurationsalternativ**:

   ![Konfigurationsknappen](assets/ef-02.png)

1. Ange de mallar som krävs i dialogrutan **Konfigurera Experience Fragments**:

   ![Konfigurera Experience Fragments](assets/ef-01.png)

   >[!NOTE]
   >
   >Mer information finns i [Mallar för Experience Fragments](/help/sites-developing/experience-fragments.md#templates-for-experience-fragments).

1. Välj **Spara**.

## Skapa ett upplevelsefragment {#creating-an-experience-fragment}

Så här skapar du ett Experience Fragment:

1. Välj Experience Fragments i Global Navigation.

   ![xf-01](assets/xf-01.png)

1. Navigera till önskad mapp och välj **Skapa**.

   ![xf-02](assets/xf-02.png)

1. Välj **Experience Fragment** för att öppna guiden **Skapa Experience Fragment**.

   Välj önskad **mall** och sedan **Nästa**:

   ![xf-03](assets/xf-03.png)

1. Ange **egenskaperna** för **upplevelsefragmentet**.

   En **titel** är obligatorisk. Om **Namn** lämnas tomt kommer det att härledas från **Titel**.

   ![xf-04](assets/xf-04.png)

   >[!NOTE]
   >
   >Taggar från Experience Fragment-mallen kommer inte att sammanfogas med taggar på den här Experience Fragment-rotsidan.
   >
   >De här är helt separata.

1. Klicka på **Skapa**.

   Ett meddelande visas. Välj:

   * **Klar** för att återgå till konsolen

   * **Öppna** om du vill öppna fragmentredigeraren

## Redigera din upplevelsefragment {#editing-your-experience-fragment}

Experience Fragment Editor har funktioner som liknar den vanliga sidredigeraren.

>[!NOTE]
>
>Mer information om hur du använder sidredigeraren finns i [Redigera sidinnehåll](/help/sites-authoring/editing-content.md).

Följande exempelprocedur visar hur du skapar ett teaser för en produkt:

1. Dra och släpp **Teaser** från [Components Browser](/help/sites-authoring/author-environment-tools.md#components-browser).

   ![xf-05](assets/xf-05.png)

1. Välj **[Konfigurera](/help/sites-authoring/editing-content.md#edit-configure-copy-cut-delete-paste)** i komponentens verktygsfält.
1. Lägg till **resursen** och definiera **egenskaperna** efter behov.
1. Bekräfta definitionerna med **Klar** (bockikon).
1. Lägg till fler komponenter efter behov.

## Skapa en upplevelsefragmentvariant {#creating-an-experience-fragment-variation}

Ni kan skapa olika upplevelsefragment beroende på era behov:

1. Öppna ditt fragment för [redigering](/help/sites-authoring/experience-fragments.md#editing-your-experience-fragment).
1. Öppna fliken **Variationer**.

   ![xf-authoring-06](assets/xf-authoring-06.png)

1. Med **Skapa** kan du skapa:

   * **Variant**
   * **Variation som [live-copy](/help/sites-administering/msm.md#live-copies)**.

     >[!NOTE]
     >
     >Om du skapar en ursprunglig variant som Live-kopia ärvs titeln med Live Copy Source som huvudvariant.

1. Definiera de nödvändiga egenskaperna:

   * **Mall**
   * **Titel**
   * **Namn**; om det lämnas tomt kommer det att härledas från titeln
   * **Beskrivning**
   * **Variationstaggar**

   ![xf-06](assets/xf-06.png)

1. Bekräfta med **Klar** (bock-ikon), så visas den nya varianten på panelen:

   ![xf-07](assets/xf-07.png)

## Använda ditt Experience Fragment {#using-your-experience-fragment}

Nu kan du använda din Experience Fragment när du redigerar dina sidor:

1. Öppna en sida för redigering.

   >[!NOTE]
   >
   >Sidan måste baseras på en redigerbar mall.

   Till exempel: [https://localhost:4502/editor.html/content/we-retail/language-masters/en/products/men.html](https://localhost:4502/editor.html/content/we-retail/language-masters/en/products/men.html)

1. Skapa en instans av Experience Fragment-komponenten genom att dra komponenten från komponentwebbläsaren till sidstyckesystemet:

   ![xf-08](assets/xf-08.png)

1. Lägg till den faktiska Experience Fragment-instansen i komponentinstansen, antingen:

   * Dra det önskade fragmentet från Assets Browser och släpp det på komponenten
   * Välj **Konfigurera** i komponentverktygsfältet och ange vilket fragment som ska användas, bekräfta med **Klar** (tick)

   ![xf-09](assets/xf-09.png)

   >[!NOTE]
   >
   >Redigera i komponentverktygsfältet fungerar som ett kortkommando för att öppna fragmentet i fragmentredigeraren.

## Byggblock {#building-blocks}

Du kan välja en eller flera komponenter för att skapa en byggsten för återvinning i fragmentet:

### Skapa ett byggblock {#creating-a-building-block}

Så här skapar du ett byggblock:

1. Välj de komponenter som du vill återanvända i Experience Fragment-redigeraren:

   ![xf-10](assets/xf-10.png)

1. Välj **Konvertera till byggblock** i komponentverktygsfältet:

   ![xf-authoring-13-icon](assets/xf-authoring-13-icon.png)

1. Ange namnet på **byggblocket** och bekräfta med **Konvertera**:

   ![xf-11](assets/xf-11.png)

1. **Byggblocket** visas på fliken och kan markeras i styckesystemet:

   ![xf-12](assets/xf-12.png)

#### Hantera ett byggblock {#managing-a-building-block}

Byggblocket visas på fliken **Byggblock**. Följande åtgärder är tillgängliga för varje block:

* Gå till mallsida: öppna varianten av rotsidan på en ny flik
* Byt namn
* Ta bort

![xf-13](assets/xf-13.png)

#### Använda ett byggblock {#using-a-building-block}

Du kan dra byggblocket till styckesystemet för vilket fragment som helst, precis som med andra komponenter.

## Information om ert Experience Fragment {#details-of-your-experience-fragment}

Information om ditt fragment kan ses:

1. Information visas i alla vyer av konsolen **Experience Fragments** med **listvyn** inklusive information om en [export till mål](/help/sites-administering/experience-fragments-target.md):

   ![ef-03](assets/ef-03.png)

1. När du öppnar **Egenskaper** i Experience Fragment:

   ![ef-04](assets/ef-04.png)

   Egenskaperna är tillgängliga på olika flikar:

   >[!CAUTION]
   >
   >De här flikarna visas när du öppnar **Egenskaper** från Experience Fragments-konsolen.
   >
   >
   >Om du **öppnar egenskaperna** när du redigerar ett upplevelsefragment visas rätt [Sidegenskaper](/help/sites-authoring/editing-page-properties.md).

   ![ef-05](assets/ef-05.png)

   * **Grundläggande**

      * **Titel** - obligatoriskt

      * **Beskrivning**
      * **Taggar**
      * **Totalt antal varianter** - endast information

      * **Antal webbvarianter** - endast information
      * **Antal icke-webbvarianter** - endast inf **organisation**

      * **Antal sidor som använder det här fragmentet** - endast information

   * **Molntjänster**

      * **Molnkonfiguration**
      * **Cloud Service-konfigurationer**
      * **Facebook-sid-ID**
      * **Pinterest board**

   * **Referenser**

      * En lista med referenser.

   * **Status för sociala medier**

      * Information om variationer i sociala medier.

## The Plain HTML Rendition {#the-plain-html-rendition}

Med väljaren `.plain.` i URL:en kan du komma åt den vanliga HTML-återgivningen via webbläsaren.

>[!NOTE]
>
>Även om detta är direkt tillgängligt från webbläsaren är det främsta syftet med [att tillåta andra program (till exempel webbprogram från tredje part, anpassade mobilimplementeringar) att komma åt innehållet i Experience Fragment direkt, med enbart URL:en &#x200B;](/help/sites-developing/experience-fragments.md#the-plain-html-rendition).

## Exportera Experience Fragments {#exporting-experience-fragments}

Som standard levereras Experience Fragments i HTML-format. Detta kan användas av både AEM och tredjepartskanaler.

JSON kan även användas för export till Adobe Target. Mer information finns i [Målintegrering med Experience Fragments](/help/sites-administering/experience-fragments-target.md).
