---
title: Exportera Experience Fragments till Adobe Target
description: Lär dig hur du exporterar Adobe Experience Manager (AEM) Experience Fragments till Adobe Target.
contentOwner: carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: integration
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Integration
role: Admin
exl-id: 39473f0a-e4ee-4372-a0ea-ccf5d32501b9
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1438'
ht-degree: 0%

---

# Exportera Experience Fragments till Adobe Target{#exporting-experience-fragments-to-adobe-target}

Du kan exportera [Experience Fragments](/help/sites-authoring/experience-fragments.md), som har skapats i Adobe Experience Manager (AEM), till Adobe Target (Target). De kan sedan användas som erbjudanden i Target-aktiviteter för att testa och personalisera upplevelser i stor skala.

Det finns tre formatalternativ för att exportera ett Experience Fragment till Adobe Target:

* HTML (standard): Stöd för leverans av webb- och hybridinnehåll
* JSON: Stöd för leverans av headless-innehåll
* HTML &amp; JSON

AEM Experience Fragments kan exporteras till standardarbetsytan i Adobe Target eller till användardefinierade arbetsytor för Adobe Target. Detta görs med Adobe Developer Console, som AEM måste [integreras med Adobe Target med IMS](/help/sites-administering/setting-up-ims-integrations-for-aem.md) för.

>[!NOTE]
>
>[IMS-integreringar har nu konfigurerats med S2S OAuth](/help/sites-administering/setting-up-ims-integrations-for-aem.md).
>
>Tidigare konfigurationer gjordes med [JWT-autentiseringsuppgifter som nu är borttagna i Adobe Developer Console](/help/sites-administering/jwt-credentials-deprecation-in-adobe-developer-console.md).

>[!NOTE]
>
>Adobe Target arbetsytor finns inte i själva Adobe Target. De definieras och hanteras i Adobe IMS (Identity Management System) och väljs sedan ut för användning i olika lösningar med hjälp av integreringar från Adobe Developer Console.

>[!NOTE]
>
>Adobe Target arbetsytor kan användas för att tillåta medlemmar i en organisation (grupp) att endast skapa och hantera erbjudanden och aktiviteter för den här organisationen, utan att ge åtkomst till andra användare. Till exempel landsspecifika organisationer inom ett globalt område.

>[!NOTE]
>
>Mer information finns även i:
>
>* [Adobe Target-utveckling](https://developers.adobetarget.com/)
>* [Kärnkomponenter - Upplevelsefragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html)
>

## Förutsättningar {#prerequisites}

Du måste utföra olika åtgärder:

1. Du måste [integrera AEM med Adobe Target med IMS](/help/sites-administering/setting-up-ims-integrations-for-aem.md).

   >[!NOTE]
   >
   >[IMS-integreringar har nu konfigurerats med S2S OAut](/help/sites-administering/setting-up-ims-integrations-for-aem.md).
   >
   >Tidigare konfigurationer gjordes med [JWT-autentiseringsuppgifter som nu är borttagna i Adobe Developer Console](/help/sites-administering/jwt-credentials-deprecation-in-adobe-developer-console.md).

1. Experience Fragments exporteras från AEM-författarinstansen, så du måste [konfigurera AEM Link Externalizer](/help/sites-administering/target-requirements.md#configuring-the-aem-link-externalizer) för författarinstansen för att se till att alla referenser i Experience Fragment är externaliserade för webbleverans.

   >[!NOTE]
   >
   >[Experience Fragment Link Rewriter-providern](/help/sites-developing/experience-fragments.md#the-experience-fragment-link-rewriter-provider-html) är tillgänglig för läntreskrivning som inte omfattas av standardinställningen. Med detta kan ni utveckla anpassade regler för er instans.

## Lägg till molnkonfigurationen {#add-the-cloud-configuration}

Innan du exporterar ett fragment måste du lägga till **molnkonfigurationen** för **Adobe Target** i fragmentet eller mappen. Detta gör även att du kan:

* ange de formatalternativ som ska användas för exporten
* välj en målarbetsyta som mål
* välj en Externalizer-domän för att skriva om referenser i Experience Fragment (valfritt)

De obligatoriska alternativen kan väljas i **Sidegenskaper** för den obligatoriska mappen och/eller fragmentet. Specifikationen ärvs vid behov.

1. Navigera till konsolen **Experience Fragments**.

1. Öppna **Sidegenskaper** för rätt mapp eller fragment.

   >[!NOTE]
   >
   >Om du lägger till molnkonfigurationen i den överordnade Experience Fragment-mappen ärvs konfigurationen av alla underordnade.
   >
   >
   >Om du lägger till molnkonfigurationen i själva Experience Fragment ärvs konfigurationen av alla variationer.

1. Välj fliken **Molntjänster**.

1. Under **Cloud Service Configuration** väljer du **Adobe Target** i listrutan.

   >[!NOTE]
   >
   >JSON-formatet i ett Experience Fragment-erbjudande kan anpassas. För att göra detta definierar du en Customer Experience Fragment-komponent och kommenterar sedan hur dess egenskaper ska exporteras i komponentens Sling Model.
   >
   >Se kärnkomponenten:
   >
   >[Kärnkomponenter - Upplevelsefragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html)

   Under **Adobe Target** väljer du:

   * lämplig konfiguration
   * det obligatoriska formatalternativet
   * en Adobe Target-arbetsyta
   * vid behov - Externalizer-domänen

   >[!CAUTION]
   >
   >Externalizer-domänen är valfri.
   >
   >En AEM-externerare konfigureras när du vill att det exporterade innehållet ska peka på en specifik *publiceringsdomän*. Mer information finns i [Konfigurera AEM Link Externalizer](/help/sites-administering/target-requirements.md#configuring-the-aem-link-externalizer).
   >
   >Observera också att Externalizer-domäner bara är relevanta för innehållet i Experience Fragment som skickas till Target, och inte för metadata som Visa erbjudandeinnehåll.

   För en mapp:

   ![Mapp - molntjänster](assets/xf-target-integration-01.png "Mapp - molntjänster")

1. **Spara och stäng**.

## Exportera ett Experience Fragment till Adobe Target {#exporting-an-experience-fragment-to-adobe-target}

>[!CAUTION]
>
>För medieresurser, till exempel bilder, exporteras bara en referens till Target. Resursen lagras i AEM Assets och levereras från AEM publiceringsinstans.
>
>Därför måste Experience Fragment, med alla relaterade resurser, publiceras innan du exporterar till Target.

Så här exporterar du ett Experience Fragment från AEM till Target (efter att du har angett molnkonfigurationen):

1. Navigera till Experience Fragment-konsolen.
1. Välj den Experience Fragment som du vill exportera till mål.

   >[!NOTE]
   >
   >Det måste vara en webbvariant för Experience Fragment.

1. Klicka på **Exportera till Adobe Target**.

   >[!NOTE]
   >
   >Om Experience Fragment redan har exporterats väljer du **Uppdatera i Adobe Target**.

1. Klicka på **Exportera utan publicering** eller **Publicera** efter behov.

   >[!NOTE]
   >
   >Om du väljer **Publicera** publiceras Experience Fragment direkt och skickas till Target.

1. Klicka på **OK** i bekräftelsedialogrutan.

   Din Experience Fragment bör nu vara i Target.

   >[!NOTE]
   >
   >[Olika detaljer](/help/sites-authoring/experience-fragments.md#details-of-your-experience-fragment) i exporten visas i **listvyn** i konsolen och i **Egenskaper**.

   >[!NOTE]
   >
   >När du visar ett Experience Fragment i Adobe Target är det *senast ändrade*-datum som visas det datum då fragmentet senast ändrades i AEM, inte det datum då fragmentet senast exporterades till Adobe Target.

>[!NOTE]
>
>Du kan också exportera från sidredigeraren med jämförbara kommandon på menyn [Sidinformation](/help/sites-authoring/author-environment-tools.md#page-information) .

## Använda dina upplevelsefragment i Adobe Target {#using-your-experience-fragments-in-adobe-target}

När du har utfört ovanstående uppgifter visas Experience Fragment på offertsidan i Adobe Target. Titta på den [specifika Target-dokumentationen](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html) för att lära dig mer om vad du kan uppnå där.

>[!NOTE]
>
>När du visar ett Experience Fragment i Adobe Target är det *senast ändrade*-datum som visas det datum då fragmentet senast ändrades i AEM, inte det datum då fragmentet senast exporterades till Adobe Target.

## Ta bort ett Experience Fragment som redan har exporterats till Adobe Target {#deleting-an-experience-fragment-already-exported-to-adobe-target}

Om du tar bort ett Experience Fragment som redan har exporterats till Target kan det orsaka problem om fragmentet redan används i ett erbjudande i Adobe Target. Om du tar bort fragmentet blir erbjudandet oanvändbart eftersom fragmentinnehållet levereras av AEM.

För att undvika sådana situationer:

* Om Experience Fragment inte används för närvarande i en aktivitet kan AEM ta bort fragmentet utan ett varningsmeddelande.
* Om Experience Fragment används av en aktivitet i Adobe Target får AEM-användaren ett felmeddelande om eventuella konsekvenser som en borttagning av fragmentet kan ha för aktiviteten.

  Felmeddelandet i AEM förhindrar inte användaren från att (tvinga) ta bort Experience Fragment. Om Experience Fragment tas bort:

   * Target-erbjudandet med AEM Experience Fragment kan visa oönskat beteende

      * Erbjudandet kommer troligtvis fortfarande att återges eftersom Experience Fragment HTML skickades till Target
      * Eventuella referenser i Experience Fragment kanske inte fungerar korrekt om refererade resurser även har tagits bort i AEM.

   * Ytterligare ändringar av Experience Fragment är omöjliga eftersom Experience Fragment inte längre finns i AEM.


## Ta bort ClientLibs från Experience Fragments som exporterats till Target {#removing-clientlibs-from-fragments-exported-target}

Experience Fragments innehåller fullständiga html-taggar och alla nödvändiga klientbibliotek (CSS/JS) för att återge fragmentet exakt som det skapades av Experience Fragment Content Author. Det här är underdesign.

När du använder ett Experience Fragment-erbjudande med Adobe Target på en sida som levereras av AEM innehåller målsidan redan alla nödvändiga klientbibliotek. Dessutom behövs inte heller den överflödiga HTML-koden i Experience Fragment-erbjudandet (se [Considerations](#considerations)).

Här följer ett pseudoexempel på html i ett Experience Fragment-erbjudande:

```html
<!DOCTYPE>
<html>
   <head>
      <title>…</title>
      <!-- all the client libraries (css/js) -->
      …
   </head>
   <body>
        <!--/* Actual XF Offer content would appear here... */-->
   </body>
</html>
```

När AEM exporterar ett Experience Fragment till Adobe Target sker detta på en hög nivå med hjälp av flera ytterligare Sling Selectors. URL:en för det exporterade Experience Fragment kan till exempel se ut så här (meddelande `nocloudconfigs.atoffer`):

* http://www.your-aem-instance.com/content/experience-fragments/my-offers/my-xf-offer.nocloudconfigs.atoffer.html

`nocloudconfigs`-väljaren definieras med hjälp av HTML och kan överlappas genom att den kopieras från:

* /libs/cq/experience-fragments/components/xfpage/nocloudconfigs.html

`atoffer`-väljaren används för efterbearbetning med [Sling Rewriter](/help/sites-developing/experience-fragments.md#the-experience-fragment-link-rewriter-provider-html). Båda kan användas för att ta bort klientbiblioteken.

### Exempel {#example}

I detta syfte ska vi illustrera hur du gör detta med `nocloudconfigs`.

>[!NOTE]
>
>Mer information finns i [Redigerbara mallar](/help/sites-developing/templates.md#editable-templates).

#### Övertäckningar {#overlays}

I det här exemplet tar de [övertäckningar](/help/sites-developing/overlays.md) som ingår bort klientbiblioteken *och* den överflödiga HTML-koden. Du förutsätts redan ha skapat malltypen Experience Fragment. Nödvändiga filer som behöver kopieras från `/libs/cq/experience-fragments/components/xfpage/` är:

* `nocloudconfigs.html`
* `head.nocloudconfigs.html`
* `body.nocloudconfigs.html`

#### Malltypsövertäckningar {#template-type-overlays}

I det här exemplet ska vi ha följande struktur:

![Malltypsövertäckningar](assets/xf-target-integration-02.png "Malltypsövertäckningar")

Innehållet i dessa filer är följande:

* `body.nocloudconfigs.html`

  ![body.nocloudconfigs.html](assets/xf-target-integration-03.png "body.nocloudconfigs.html")

* `head.nocloudconfigs.html`

  ![head.nocloudconfigs.html](assets/xf-target-integration-04.png "head.nocloudconfigs.html")

* `nocloudconfigs.html`

  ![nocloudconfigs.html](assets/xf-target-integration-05.png "nocloudconfigs.html")

>[!NOTE]
>
>Om du vill använda `data-sly-unwrap` för att ta bort body-taggen behöver du `nocloudconfigs.html`.

### Överväganden {#considerations}

Om du behöver stöd för både AEM-webbplatser och icke-AEM-webbplatser med Experience Fragment-erbjudanden i Adobe Target måste du skapa två Experience Fragments (två olika malltyper):

* En med övertäckningen som tar bort clientlibs/extra html

* En som inte har övertäckningen och därför innehåller de nödvändiga klientlibs
