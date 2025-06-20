---
title: Redigera sidegenskaper
description: Definiera de egenskaper som krävs för en sida i Adobe Experience Manager.
solution: Experience Manager, Experience Manager Sites
feature: Authoring
role: User,Admin,Architect,Developer
exl-id: 5148afb9-f447-4475-a15c-1fa345325711
mini-toc-levels: 2
source-git-commit: 4817296c00e094a65744a896d798e429d3ab6c7d
workflow-type: tm+mt
source-wordcount: '2477'
ht-degree: 1%

---


# Redigera sidegenskaper{#editing-page-properties}

Du kan definiera de egenskaper som krävs för en sida. Dessa kan variera beroende på sidans beskaffenhet. Vissa sidor kan t.ex. vara kopplade till en live-kopia medan andra inte är det och live-kopieringsinformationen blir tillgänglig om det är lämpligt.

## Sidegenskaper {#page-properties}

Egenskaperna fördelas på flera flikar.

### Grundläggande {#basic}

#### Titel och taggar {#tile}

* **Rubrik** - Sidans rubrik visas på olika platser
   * Till exempel fliklistan **Webbplatser** och vyerna **Webbplatser** kort/lista.
   * Detta är ett obligatoriskt fält.
* **Taggar** - Här kan du lägga till eller ta bort taggar från sidan genom att uppdatera listan i markeringsrutan.
   * När du har valt en tagg visas den under markeringsrutan. Du kan ta bort en tagg från den här listan med hjälp av x.
   * Du kan ange en ny tagg genom att skriva namnet i en tom markeringsram.
      * Den nya taggen skapas när du trycker på Enter.
      * Den nya taggen visas med en liten stjärna till höger som anger att det är en ny tagg.
   * I listrutan kan du välja bland befintliga taggar.
   * Ett x-tecken visas när du för musen över en taggpost i markeringsrutan, som kan användas för att ta bort taggen för den här sidan.
   * Mer information om taggar finns i [Använda taggar.](/help/sites-authoring/tags.md)
* **Dölj i navigering** - Anger om sidan visas eller döljs i sidnavigeringen för den slutliga platsen

#### Varumärke {#branding}

Använd en enhetlig varumärkesidentitet på alla sidor genom att lägga till en instruktionsmarginal till varje sidrubrik. Den här funktionen kräver att du använder Page Component från version 2.14.0 eller senare av [Core Components.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)

* **Åsidosätt** - Markera för att definiera instruktionsmarginalen för varumärket på den här sidan.
   * Värdet ärvs av alla underordnade sidor såvida inte deras **Åsidosätt**-värden också har angetts.
* **Åsidosätt värde** - texten i instruktionsmarginalen som ska läggas till i sidrubriken
   * Värdet läggs till i sidtiteln efter ett lodstreck som `Cycling Tuscany | Always ready for the WKND`

#### Fler rubriker och beskrivning {#more}

* **Sidtitel** - En titel som ska användas på sidan
   * Används vanligtvis av titelkomponenter
   * Om den är tom används **Title**.
* **Navigeringstitel** - Du kan ange en separat rubrik som ska användas i navigeringen (om du till exempel vill ha något mer koncist).
   * Om den är tom används **Title**.
* **Underrubrik** - Underrubrik för sidan
* **Beskrivning** - Din beskrivning av sidan, dess syfte eller annan information som du vill lägga till

#### På/AvTid {#on-time}

På-/avaktiveringstiden för en sida är ett praktiskt sätt att tillfälligt dölja innehåll som redan är publicerat. Innehållet finns kvar i publiceringsinstansen när den är inaktiverad. Innehållet avpubliceras inte när du stänger av en sida.

* **I tid** - Det datum och den tidpunkt då den publicerade sidan visas (återges) i publiceringsmiljön. Sidan måste publiceras, antingen manuellt eller med förkonfigurerad automatisk replikering.

   * Om den redan är [publicerad](/help/sites-authoring/publishing-pages.md) är den här sidan tillgänglig på publiceringsinstansen, men den behåller vilande (dold) tills återgivningen sker vid den angivna tidpunkten.
   * Om den inte publiceras och [konfigureras för automatisk replikering ](/help/sites-deploying/replication.md) publiceras sidan automatiskt och återges sedan vid den angivna tidpunkten.
   * Om sidan inte är publicerad och inte konfigurerad för automatisk replikering publiceras den inte automatiskt. Därför visas 404 när ett försök görs att komma åt sidan.

* **Fråntid** - Ungefär som och ofta används i kombination med **På tid**, definierar detta den tidpunkt då den publicerade sidan döljs i publiceringsmiljön.

Lämna dessa fält (**Vid tid** och **Fråntid**) tomma för sidor som du vill publicera och ha tillgängliga direkt och tillgängliga i publiceringsmiljön tills de inaktiveras (standardscenariot).

>[!NOTE]
>Om antingen **På-tid** eller **Av-tid** redan har inträffat och automatisk replikering har konfigurerats, utlöses den relevanta åtgärden omedelbart.

>[!TIP]
>
>På-/avaktiveringstider hanterar innehåll som redan har publicerats (antingen manuellt eller via automatisk replikering) strikt. Därför aktiveras inte publiceringsarbetsflöden, t.ex. sådana för godkännande av innehåll, av på-/av-tider och på-/av-tider påverkar inte sidans publiceringsstatus. Av den anledningen lämpar sig bäst för att tillfälligt visa/dölja innehåll som redan har godkänts och publicerats.
>
>Om du vill publicera nytt innehåll med alla associerade arbetsflöden eller helt ta bort (avpublicera innehåll) från webbplatsen bör du överväga att [hantera publikationen.](/help/sites-authoring/publishing-pages.md#manage-publication)

#### Vanity URL {#vanity-url}

Ange en innehålls-URL för den här sidan, vilket kan ge dig en kortare och/eller mer uttrycksfull URL.

Om Vanity-URL:en till exempel är inställd på `welcome` till den sida som identifieras av sökvägen `/v1.0/startpage` för webbplatsen `http://example.com,` blir `http://example.com/welcome` vanity-URL:en för `http://example.com/content/v1.0/startpage`

>[!CAUTION]
>
>Vanity URL:er:
>
>* Måste vara unikt.
>* Använd inte regex-mönster.
>* Ska inte anges till en befintlig sida.

Konfigurera Dispatcher för att aktivera åtkomst till mål-URL:er. Mer information finns i [Aktivera åtkomst till Vanity-URL:er](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#enabling-access-to-vanity-urls-vanity-urls).

* **Lägg till** - Tryck eller klicka för att lägga till en fågel-URL.
* **Ta bort** - Tryck eller klicka för att ta bort en fågel-URL.
  **Omdirigerings-URL** - Anger om du vill att sidan ska använda innehålls-URL:en eller omdirigera till sidans faktiska URL

### Avancerat {#advanced}

#### Inställningar {#settings}

* **Språk** - Sidspråket
* **Språkrot** - Måste kontrolleras om sidan är roten för en språkkopia
* **Omdirigering** - Anger sidan som den här sidan automatiskt ska omdirigeras till
* **Design** - Anger den [design](/help/sites-developing/designer.md) som ska användas för den här sidan.
* **Alias** - Anger ett alias som ska användas för den här sidan
   * Om du till exempel definierar aliaset `private` för sidan `/content/wknd/us/en/magazine/members-only` kan den här sidan också nås via `/content/wknd/us/en/magazine/private`
   * Om du skapar ett alias anges egenskapen `sling:alias` på sidnoden, vilket bara påverkar resursen, inte databassökvägen.
   * Sidor som används av alias i redigeraren kan inte publiceras. [Publiceringsalternativ](/help/sites-authoring/publishing-pages.md) i redigeraren är bara tillgängliga för sidor som du kommer åt via de faktiska sökvägarna.
   * Mer information finns i [Lokaliserade sidnamn under SEO och Bästa praxis för URL-hantering](/help/managing/seo-and-url-management.md#localized-page-names).

#### Konfiguration {#configuration}

* **Ärvs från &lt;*path*>** - Aktivera/inaktivera arv av **molnkonfigurationen** för sidan
* **Molnkonfiguration** - Sökvägen till konfigurationen

#### Mallinställningar {#templates}

* **Tillåtna mallar** - [Definierar listan med mallar som är tillgängliga](/help/sites-authoring/templates.md#allowingatemplate) i den här undergrenen

#### Autentiseringskrav {#authentication}

* **Aktivera** - Aktivera (eller inaktivera) användningen av autentisering så att du kan komma åt sidan
* **Inloggningssida** - Den sida som ska användas för inloggning

>[!NOTE]
>
>Stängda användargrupper för sidan definieras på fliken **[Behörigheter](/help/sites-authoring/editing-page-properties.md#permissions)**.

>[!CAUTION]
>
>På fliken **[Behörigheter](#permissions)** kan du redigera CUG-konfigurationer baserat på förekomsten av `granite:AuthenticationRequired`-mixinen. Om sidbehörigheter konfigureras med inaktuella CUG-konfigurationer, baserat på förekomsten av egenskapen `cq:cugEnabled`, visas ett varningsmeddelande under **Autentiseringskrav** och alternativet kan inte redigeras, och inte heller kan [Behörigheter](/help/sites-authoring/editing-page-properties.md#permissions) redigeras.
>
>
>I så fall måste CUG-behörigheterna redigeras i det [klassiska användargränssnittet](/help/sites-classic-ui-authoring/classic-page-author-edit-page-properties.md).

#### Exportera {#export}

* **Konfiguration** - Anger en exportkonfiguration

#### SEO {#seo}

* **Kanonisk URL** - Används för att skriva över sidans kanoniska URL
   * Om den lämnas tom är sidans URL dess kanoniska URL.
* **Robots-taggar** - Använd listrutan för att välja robots-taggar för att styra beteendet för sökmotorcrawler
   * Vissa alternativ står i konflikt med varandra, och i så fall har det mer tillåtna alternativet företräde.
* **Generera platskarta** - När du väljer det här alternativet genereras en `sitemap.xml` för den här sidan och dess underordnade sidor.

### Bilder {#images}

#### Aktuell bild {#featured-image}

I det här avsnittet kan du välja och konfigurera den bild som ska visas. Detta används i komponenter som refererar till sidan, t.ex. teasers, page lists, etc.

* **Bild** - Du kan **välja** en resurs eller bläddra efter en fil som ska överföras och sedan **redigera** eller **rensa** den markerade bilden.
* **Alternativ text** - Text som används för att representera innebörden och/eller funktionen i bilden, som ofta används av skärmläsare
* **Ärv - värde som tagits från DAM-resursen** - När det här alternativet är markerat fylls den alternativa texten i med värdet för `dc:description`-metadata i DAM.

#### Miniatyrbild {#thumbnail}

STthis-avsnittet används för att välja och konfigurera sidans miniatyrbild. Detta används i komponenter som refererar till sidan, t.ex. teasers, page lists, etc.

* **Generera förhandsvisning** - Skapar en förhandsvisning av sidan som du vill använda som miniatyrbild
* **Överför bild** - Överför en bild som du vill använda som miniatyrbild
* **Välj bild** - Väljer en befintlig resurs som du vill använda som miniatyrbild
* **Återställ** - Det här alternativet blir tillgängligt när du har ändrat miniatyrbilden. Om du inte vill behålla ändringen kan du återställa den innan du sparar.

### Molntjänster {#cloud-services}

* **Cloud Service-konfigurationer** - Definierar vilken konfiguration som används för sidans molntjänster
* **Ärvd från** - För Live-kopior och språkkopior ärvs molnkonfigurationer som standard från utkast.
   * Avmarkera för att åsidosätta arv

### Personalization {#personalization}

#### ContextHub-konfigurationer {#contexthub}

* **Ärvd från** - ContextHub-konfigurationer ärvs som standard från den överordnade sidan.
   * Avmarkera om du vill åsidosätta arv.
* **ContextHub-sökväg** - Väljer [KontextHub-konfigurationen](/help/sites-developing/ch-configuring.md)
* **Segmentsökväg** - Markerar [segmentsökvägen](/help/sites-administering/segmentation.md).

#### Målkonfiguration {#targeting}

Välj ett [varumärke om du vill ange ett omfång för målanpassning.](/help/sites-authoring/target-adobe-campaign.md)

>[!NOTE]
>Det här alternativet kräver att användarkontot finns i gruppen `Target Adminstrators`.

### Behörigheter {#permissions}

Använd fliken **Behörigheter** för att definiera vilka användare, grupper eller [stängda användargrupper (CUG)](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/closed-user-groups.html) som kan komma åt och/eller ändra sidan.

* [Lägg till behörigheter](/help/sites-administering/user-group-ac-admin.md)
* [Redigera stängd användargrupp](/help/sites-administering/cug.md#applying-your-closed-user-group-to-content-pages)
* Visa [gällande behörigheter](/help/sites-administering/user-group-ac-admin.md)

>[!CAUTION]
>
>På fliken **Behörigheter** kan du redigera CUG-konfigurationer baserat på förekomsten av `granite:AuthenticationRequired`-mixinen. Om sidbehörigheter konfigureras med inaktuella CUG-konfigurationer, baserat på förekomsten av egenskapen `cq:cugEnabled`, visas ett varningsmeddelande och CUG-behörigheterna kan inte redigeras. Autentiseringskravet på fliken [ Avancerat](/help/sites-authoring/editing-page-properties.md#advanced) kan inte heller redigeras.
>
>
>I så fall måste CUG-behörigheterna redigeras i det [klassiska användargränssnittet](/help/sites-classic-ui-authoring/classic-page-author-edit-page-properties.md).

>[!NOTE]
>
>På fliken Behörigheter går det inte att skapa tomma CUG-grupper, vilket kan vara ett enkelt sätt att neka alla användare åtkomst. För att göra detta måste CRX Explorer användas. Mer information finns i dokumentet [Behörighetsadministration för användare, grupp och åtkomst](/help/sites-administering/user-group-ac-admin.md).

### Blueprint {#blueprint}

Den här fliken visas bara för sidor som fungerar som utkast. Utkast fungerar som bas för Live-kopior och ingår i [Hantering av flera webbplatser.](/help/sites-administering/msm.md)

* **Utrullning** - Startar en utrullning av utkast till Live-kopior
* **Live Copy-översikt** - Öppnar ett fönster för att bläddra i sidstrukturen i Live Copy
* **Aktuella live-kopior** - En lista med sidor som baseras på (det vill säga på live-kopior av) den valda designsidan
* **Utrullningskonfiguration** - Definierar sidans utrullningskonfiguration

### Live Copy {#live-copy}

Den här fliken visas bara för sidor som har konfigurerats som live-kopior. Precis som med [utkast är ](#blueprint) Live-kopior en del av [Multi Site Management (Hantering av flera webbplatser).](/help/sites-administering/msm.md)

* **Synkronisera** - Synkroniserar Live-kopia med utkast, med lokala ändringar
* **Återställ** - Återställer Live Copy till läget för utkast och tar bort lokala ändringar
* **Gör uppehåll** - Pausar Live Copy från ytterligare rolloutändringar
* **Koppla loss** - Frigör Live-kopia från utkast

#### Source {#source}

* Visar sökvägen till ritningen för den här Live-kopian

#### Status {#status}

* Visar sidans aktuella Live Copy-status

#### Konfiguration {#live-copy-config}

* **Live Copy-arv** - Om det här alternativet är markerat gäller Live Copy-konfigurationen alla underordnade.
* **Ärv utrullningskonfigurationer från överordnad** - Om det här alternativet är markerat ärvs rollout-konfigurationen från den överordnade sidan för sidan.
* **Välj utrullningskonfiguration** - Definierar under vilka omständigheter ändringar sprids från utskriften och bara är tillgängliga när **Ärv utrullningskonfigurationer från överordnad** inte har valts
* **Lista över uteslutna sökvägar**

## Redigera sidegenskaper {#editing-page-properties-1}

Du kan definiera sidegenskaper:

* Från konsolen **Platser**:

   * [Skapa en sida](/help/sites-authoring/managing-pages.md#creating-a-new-page) (en deluppsättning av egenskaperna)

   * Klicka eller peka på **Egenskaper**

      * För en enstaka sida
      * För flera sidor (endast en deluppsättning av egenskaperna är tillgängliga för redigering av en masse)

* Från sidredigeraren:

   * Med **Sidinformation** (och sedan **Öppna egenskaper**)

### Från webbplatskonsolen - en sida {#from-the-sites-console-single-page}

Klicka eller tryck på **Egenskaper** för att definiera sidegenskaperna:

1. Använd konsolen **Platser** för att navigera till platsen för sidan som du vill visa och redigera egenskaper för.

1. Välj alternativet **Egenskaper** för den begärda sidan med något av följande alternativ:

   * [Snabbåtgärder](/help/sites-authoring/basic-handling.md#quick-actions)
   * [Markeringsläge](/help/sites-authoring/basic-handling.md#selectionmode)

   Sidegenskaperna visas med rätt flikar.

1. Visa eller redigera egenskaperna efter behov.

1. Använd sedan **Spara** för att spara uppdateringar följt av **Stäng** så att du kan gå tillbaka till konsolen.

### När en sida redigeras {#when-editing-a-page}

När du redigerar en sida kan du använda **Sidinformation** för att definiera sidegenskaperna:

1. Öppna sidan som du vill redigera egenskaper för.

1. Välj ikonen **Sidinformation** för att öppna markeringsmenyn:

   ![screen_shot_2018-03-22at095740](assets/screen_shot_2018-03-22at095740.png)

1. Välj **Öppna egenskaper** så öppnas en dialogruta där du kan redigera egenskaperna, sorterade efter lämplig flik. Följande knappar finns också till höger om verktygsfältet:

   * **Avbryt**
   * **Spara och stäng**

1. Använd knappen **Spara och stäng** för att spara ändringarna.

### Från webbplatskonsolen - flera sidor {#from-the-sites-console-multiple-pages}

I konsolen **Platser** kan du markera flera sidor och sedan använda **Visa egenskaper** för att visa och/eller redigera sidegenskaperna. Detta kallas massredigering av sidegenskaper.

>[!NOTE]
>
>Det finns även massredigering av egenskaper för Assets. Den liknar, men skiljer sig på några punkter. Mer information finns i [Redigera egenskaper för flera Assets](/help/assets/metadata.md).
>
>Det finns också [gruppredigeraren](/help/sites-administering/bulk-editor.md). Med den här redigeraren kan du söka efter innehåll från flera sidor med hjälp av GQL (Google Query Language) och sedan redigera innehållet direkt med hjälp av gruppredigeraren innan du sparar ändringarna på originalsidorna.

Du kan välja flera sidor för massredigering på olika sätt, bland annat:

* När du bläddrar i konsolen **Platser**
* När du har använt **Sök** för att hitta en uppsättning sidor

![epp-01](assets/epp-01.png)

När du har markerat sidorna och sedan klickat eller tryckt på alternativet **Egenskaper** visas bulkegenskaperna:

![epp-02](assets/epp-02.png)

Du kan bara redigera sidor som:

* Dela samma resurstyp
* Är inte en del av en livecopy

   * Om någon av sidorna finns i en live-kopia visas ett meddelande när egenskaperna öppnas.

När du har valt Massredigering kan du göra följande:

* **Visa**

  När du visar Sidegenskaper för flera sidor kan du se följande:

   * En lista över de sidor som påverkas

      * Du kan markera/avmarkera vid behov

   * Tabbar

      * På samma sätt som när du visar egenskaper för en enskild sida ordnas egenskaperna under flikar.

   * En deluppsättning med egenskaper

      * Egenskaper som är tillgängliga på alla markerade sidor och som uttryckligen har definierats som tillgängliga för massredigering visas.
      * Om du minskar sidmarkeringen till en sida visas alla egenskaper.

   * Vanliga egenskaper med ett gemensamt värde

      * Endast egenskaper med ett gemensamt värde visas i visningsläget.
      * När fältet har flera värden (till exempel Taggar) visas värden bara när *all* är gemensamma. Om bara vissa är vanliga visas de bara vid redigering.

  När det inte finns några egenskaper med ett gemensamt värde visas ett meddelande.

* **Redigera**

  När du redigerar Sidegenskaper för flera sidor:

   * Du kan uppdatera värdena i de tillgängliga fälten.

      * De nya värdena tillämpas på alla markerade sidor när du väljer **Klar**.
      * När fältet har flera värden (till exempel Taggar) kan du antingen lägga till ett nytt värde eller ta bort ett gemensamt värde.

   * Fält som är vanliga, men har olika värden på de olika sidorna, anges med ett särskilt värde som texten `<Mixed Entries>`.

>[!NOTE]
>
>Sidkomponenten kan konfigureras för att ange de fält som är tillgängliga för massredigering. Se [Konfigurera sidan för massredigering av sidegenskaper](/help/sites-developing/bulk-editing.md).
