---
title: Upplevelsefragment inom Adobe Experience Manager Sites-utveckling
description: Lär dig hur du anpassar Experience Fragments för Adobe Experience Manager.
contentOwner: AEM Docs
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: bc621086-8128-4836-a580-dca99f61c439
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1786'
ht-degree: 0%

---

# Upplevelsefragment {#experience-fragments}

## Grunderna {#the-basics}

Ett [Experience Fragment](/help/sites-authoring/experience-fragments.md) är en grupp med en eller flera komponenter, inklusive innehåll och layout, som kan refereras till på sidor.

En Experience Fragment Master och/eller Variant använder:

* `sling:resourceType` : `/libs/cq/experience-fragments/components/xfpage`

Eftersom det inte finns någon `/libs/cq/experience-fragments/components/xfpage/xfpage.html` återgår den till

* `sling:resourceSuperType` : `wcm/foundation/components/page`

## The Plain HTML Rendition {#the-plain-html-rendition}

Du kan använda väljaren `.plain.` i URL:en för att få åtkomst till den vanliga HTML-återgivningen.

Detta är tillgängligt från webbläsaren, men dess primära syfte är att tillåta andra program (till exempel webbprogram från tredje part, anpassade mobilimplementeringar) att komma åt innehållet i Experience Fragment direkt, med enbart URL:en.

Den rena HTML-renderingen lägger till protokoll, värd och kontextsökväg till sökvägar som är:

* av typen: `src`, `href` eller `action`

* eller avslutas med: `-src`, eller `-href`

Till exempel:

`.../brooklyn-coat/master.plain.html`

>[!NOTE]
>
>Länkar refererar alltid till publiceringsinstansen. De används av tredje part, så länken anropas alltid från publiceringsinstansen, inte författarinstansen.
>
>Mer information finns i [Externalisera URL:er](/help/sites-developing/externalizer.md).

![xf-14](assets/xf-14.png)

Väljaren för ren återgivning använder en transformator i stället för ytterligare skript. [Sling Rewriter](https://sling.apache.org/documentation/bundles/output-rewriting-pipelines-org-apache-sling-rewriter.html) används som transformator. Detta är konfigurerat på

* `/libs/experience-fragments/config/rewriter/experiencefragments`

### Konfigurera generering av HTML-återgivning {#configuring-html-rendition-generation}

HTML-renderingen genereras med Sling Rewriter-pipelines. Pipelinen definieras på `/libs/experience-fragments/config/rewriter/experiencefragments`. HTML Transformer stöder följande alternativ:

* `allowedCssClasses`
   * Ett RegEx-uttryck som matchar CSS-klasserna som ska lämnas i den slutliga återgivningen.
   * Detta är användbart om kunden vill ta bort vissa specifika CSS-klasser
* `allowedTags`
   * En lista över HTML-taggar som ska tillåtas i den slutliga återgivningen.
   * Som standard är följande taggar tillåtna (ingen konfiguration behövs): html, head, title, body, img, p, span, ul, li, a, b, i, em, strong, h1, h2, h3, h4, h5, h6, br, noscript, div, link och script

Vi rekommenderar att du konfigurerar omskrivaren med en övertäckning. Se [Övertäckningar](/help/sites-developing/overlays.md)

## Sociala variationer {#social-variations}

Sociala varianter kan publiceras på sociala medier (text och bild). I Adobe Experience Manager (AEM) kan dessa sociala varianter innehålla komponenter, till exempel textkomponenter och bildkomponenter.

Bilden och texten för det sociala inlägget kan hämtas från alla bildresurstyper eller textresurstyper på alla djupnivåer (antingen i byggblocket eller layoutbehållaren).

Sociala variationer möjliggör också byggstenar och tar hänsyn till dem vid sociala åtgärder (i publiceringsmiljön).

För att kunna publicera rätt text och bild i sociala medier måste vissa konventioner respekteras om du utvecklar egna anpassade komponenter.

För detta måste följande egenskaper användas:

* Extrahera bilden

   * `fileReference`
   * `fileName`

* För att extrahera texten

   * `text`

Komponenter som inte använder den här konventionen beaktas inte.

## Mallar för Experience Fragments {#templates-for-experience-fragments}

>[!CAUTION]
>
>***Endast*** [redigerbara mallar](/help/sites-developing/page-templates-editable.md) stöds för Experience Fragments.
>
>Upplevelsefragment kan bara användas på sidor som är baserade på redigerbara mallar.

När du utvecklar en ny mall för Experience Fragments kan du följa standardmetoderna för en [redigerbar mall](/help/sites-developing/page-templates-editable.md).

Om du vill skapa en upplevelsefragmentmall som identifieras av guiden **Skapa Experience Fragment** måste du följa någon av dessa regeluppsättningar:

1. Båda:

   1. Mallens resurstyp (den inledande noden) måste ärva från:

      `cq/experience-fragments/components/xfpage`

   1. Och mallens namn måste börja med:

      `experience-fragments`
Detta gör att användare kan skapa upplevelsefragment i /content/experience-fragments eftersom egenskapen `cq:allowedTemplates` i den här mappen innehåller alla mallar som har namn som börjar med `experience-fragment` . Kunder kan uppdatera den här egenskapen så att den omfattar sina egna namngivningsscheman eller mallplatser.

1. [Tillåtna mallar](/help/sites-authoring/experience-fragments.md#configure-allowed-templates-folder) kan konfigureras i Experience Fragments-konsolen.
<!--
1. Add the template details manually in `cq:allowedTemplates` on the `/content/experience-fragment` node.
-->

<!-- >[!NOTE]
>
>[Allowed templates](/help/sites-authoring/experience-fragments.md#configuring-allowed-templates) can be configured in the Experience Fragments console.
-->

## Komponenter för Experience Fragments {#components-for-experience-fragments}

[Utveckla komponenter](/help/sites-developing/components.md) för användning med/i Experience Fragments följer standardrutiner.

Den enda ytterligare konfigurationen är att se till att komponenterna är [tillåtna i mallen, vilket uppnås med innehållsprincipen &#x200B;](/help/sites-developing/page-templates-editable.md#content-policies).

## Experience Fragment Link Rewriter Provider - HTML {#the-experience-fragment-link-rewriter-provider-html}

I AEM kan du skapa Experience Fragments. An Experience Fragment:

* består av en grupp komponenter tillsammans med en layout,
* kan finnas oberoende av en AEM-sida.

Ett av användningsområdena för sådana grupper är att bädda in innehåll i kontaktpunkter från tredje part, som Adobe Target.

### Omskrivning av standardlänk {#default-link-rewriting}

Med funktionen [Exportera till mål](/help/sites-administering/experience-fragments-target.md) kan du:

* skapa en upplevelsefragment,
* lägga till komponenter i den,
* och sedan exportera det som ett Adobe Target-erbjudande, antingen i HTML-format eller JSON-format.

Den här funktionen kan vara [aktiverad på en författarinstans av AEM](/help/sites-administering/experience-fragments-target.md#Prerequisites). Det kräver en giltig Adobe Target-konfiguration och konfigurationer för länkutökningen.

Länkutjämnaren används för att fastställa rätt URL:er som behövs när du skapar HTML-versionen av målerbjudandet, som sedan skickas till Adobe Target. Detta är nödvändigt eftersom Adobe Target kräver att alla länkar i Target HTML-erbjudandet ska vara tillgängliga för alla. Det innebär att alla resurser länkreferensen och själva Experience Fragment måste publiceras innan de kan användas.

Som standard skickas en begäran till en anpassad Sling-väljare i AEM när du skapar ett HTML-erbjudande för mål. Den här väljaren kallas `.nocloudconfigs.html`. Som namnet antyder skapas en ren HTML-återgivning av ett Experience Fragment, men inte molnkonfigurationer (vilket skulle vara överflödig information).

När du har skapat HTML-sidan ändrar Sling Rewriter-flödet utdata:

1. Elementen `html`, `head` och `body` ersätts med elementen `div`. Elementen `meta`, `noscript` och `title` tas bort (de är underordnade element till det ursprungliga `head` -elementet och beaktas inte när det ersätts av `div` -elementet).

   Detta görs för att se till att HTML Target-erbjudandet kan inkluderas i målaktiviteter.

1. AEM ändrar alla interna länkar i HTML så att de pekar på en publicerad resurs.

   För att fastställa vilka länkar som ska ändras följer AEM det här mönstret för attributen för HTML-element:

   1. `src` attribut
   1. `href` attribut
   1. `*-src`-attribut (som data-src, custom-src o.s.v.)
   1. `*-href`-attribut (som `data-href`, `custom-href`, `img-href` och så vidare)

   >[!NOTE]
   >
   >Vanligtvis är de interna länkarna i HTML relativa länkar, men det kan finnas fall när anpassade komponenter tillhandahåller fullständiga URL:er i HTML. Som standard ignorerar AEM dessa fullständiga URL:er och inga ändringar görs.

   Länkarna i de här attributen körs via AEM Link Externalizer `publishLink()` för att återskapa URL:en som om den fanns på en publicerad instans, och som sådan, offentligt tillgänglig.

När du använder en körklar implementering bör den process som beskrivs ovan vara tillräcklig för att generera målerbjudandet från Experience Fragment och sedan exportera det till Adobe Target. Det finns dock vissa användningsområden som inte ingår i den här processen. Exempel:

* Samlingsmappning är bara tillgängligt på publiceringsinstansen
* Dispatcher omdirigerar

I sådana fall tillhandahåller AEM Länkskrivarens providergränssnitt.

### Länk Rewriter-providergränssnitt {#link-rewriter-provider-interface}

För mer komplicerade fall, som inte täcks av [standard](#default-link-rewriting), erbjuder AEM providergränssnittet Länkskrivare. Det här är ett `ConsumerType`-gränssnitt som du kan implementera i dina paket som en tjänst. Den åsidosätter de ändringar AEM utför på interna länkar i ett HTML-erbjudande som återges från en Experience Fragment. Med det här gränssnittet kan du anpassa processen för att skriva om interna HTML-länkar efter företagets behov.

Exempel på användningsområden för implementering av det här gränssnittet som en tjänst är:

* Samlingsmappningar är aktiverade för publiceringsinstanserna, men inte för författarinstansen
* En dispatcher eller liknande teknik används för att omdirigera URL:er internt
* Det finns `sling:alias mechanisms` för resurser

>[!NOTE]
>
>Det här gränssnittet bearbetar bara HTML interna länkar från det genererade Target-erbjudandet.

Länkskrivarens providergränssnitt ( `ExperienceFragmentLinkRewriterProvider`) är följande:

```java
public interface ExperienceFragmentLinkRewriterProvider {

    String rewriteLink(String link, String tag, String attribute);

    boolean shouldRewrite(ExperienceFragmentVariation experienceFragment);

    int getPriority();

}
```

### Så här använder du providergränssnittet Länkskrivare {#how-to-use-the-link-rewriter-provider-interface}

Om du vill använda gränssnittet måste du först skapa ett paket som innehåller en ny tjänstkomponent som implementerar providergränssnittet för Länkskrivare.

Den här tjänsten används för att ansluta till Experience Fragment Export till Target-omskrivning för att få tillgång till de olika länkarna.

Exempel: `ComponentService`:

```java
import com.adobe.cq.xf.ExperienceFragmentLinkRewriterProvider;
import com.adobe.cq.xf.ExperienceFragmentVariation;
import org.osgi.service.component.annotations.Service;
import org.osgi.service.component.annotations.Component;

@Component
@Service
public class GeneralLinkRewriter implements ExperienceFragmentLinkRewriterProvider {

    @Override
    public String rewriteLink(String link, String tag, String attribute) {
        return null;
    }

    @Override
    public boolean shouldRewrite(ExperienceFragmentVariation experienceFragment) {
        return false;
    }

    @Override
    public int getPriority() {
        return 0;
    }

}
```

För att tjänsten ska fungera finns det nu tre metoder som måste implementeras i tjänsten:

* ` [shouldRewrite](#shouldrewrite)`
* ` [rewriteLink](#rewritelink)`

   * `rewriteLinkExample2`

* ` [getPriority](#priorities-getpriority)`

#### shouldRewrite {#shouldrewrite}

Du måste ange för systemet om länkarna behöver skrivas om när du anropar Exportera till mål för en viss Experience Fragment-variation. Detta gör du genom att implementera metoden:

`shouldRewrite(ExperienceFragmentVariation experienceFragment);`

Till exempel:

```java
@Override
public boolean shouldRewrite(ExperienceFragmentVariation experienceFragment) {
    return experienceFragment.getPath().equals("/content/experience-fragment/master");
}
```

Den här metoden tar som parameter emot den Experience Fragment-variation som systemet Exportera till mål för närvarande skriver om.

I exemplet ovan vill vi skriva om:

* länkar finns i `src`

* Endast `href`-attribut

* för ett specifikt Experience Fragment:
  `/content/experience-fragment/master`

Alla andra Experience Fragments som skickas via systemet Export to Target ignoreras och påverkas inte av ändringar som implementeras i den här tjänsten.

#### rewriteLink {#rewritelink}

För den Experience Fragment-variation som påverkas av omskrivningsprocessen fortsätter den sedan att låta tjänsten hantera omskrivningen av länken. Varje gång en länk påträffas i den interna HTML anropas följande metod:

`rewriteLink(String link, String tag, String attribute)`

Som indata tar metoden emot parametrarna:

* `link`
`String`-representationen av länken som bearbetas. Detta är vanligtvis en relativ URL som pekar på resursen på författarinstansen.

* `tag`
Namnet på det HTML-element som bearbetas.

* `attribute`
Det exakta attributnamnet.

Om till exempel systemet Exportera till mål bearbetar det här elementet kan du definiera `CSSInclude` som:

```java
<link rel="stylesheet" href="/etc.clientlibs/foundation/clientlibs/main.css" type="text/css">
```

Anropet till metoden `rewriteLink()` görs med följande parametrar:

```java
rewriteLink(link="/etc.clientlibs/foundation/clientlibs/main.css", tag="link", attribute="href" )
```

När du skapar tjänsten kan du fatta beslut baserat på angivna indata och sedan skriva om länken i enlighet med detta.

Vi vill till exempel ta bort delen `/etc.clientlibs` i URL:en och lägga till den externa domänen. För att förenkla saker och ting anser vi att vi har tillgång till en resurslösare för din tjänst, som i `rewriteLinkExample2`:

>[!NOTE]
>
>Mer information om hur du hämtar en resurslösare via en tjänstanvändare finns i [Tjänstanvändare i AEM](/help/sites-administering/security-service-users.md).

```java
private ResourceResolver resolver;

private Externalizer externalizer;

@Override
public String rewriteLink(String link, String tag, String attribute) {

    // get the externalizer service
    externalizer = resolver.adaptTo(Externalizer.class);
    if(externalizer == null) {
        // if there was an error, then we do not modify the link
        return null;
    }

    // remove leading /etc.clientlibs from resource link before externalizing
    link = link.replaceAll("/etc.clientlibs", "");

    // considering that we configured our publish domain, we directly apply the publishLink() method
    link = externalizer.publishLink(resolver, link);

    return link;
}
```

>[!NOTE]
>
>Om metoden ovan returnerar `null` lämnar systemet Exportera till mål länken som den är, som en relativ länk till en resurs.

#### Prioriteringar - getPriority {#priorities-getpriority}

Det är inte ovanligt att flera tjänster behövs för olika typer av upplevelsefragment, eller till och med för att ha en allmän tjänst som hanterar externalisering och mappning för alla Experience Fragments. I dessa fall kan konflikter uppstå om vilken tjänst som ska användas, så AEM ger möjlighet att definiera **prioritet** för olika tjänster. Prioriteringarna anges med hjälp av metoden:

* `getPriority()`

Den här metoden tillåter användning av flera tjänster där metoden `shouldRewrite()` returnerar true för samma Experience Fragment. Tjänsten som returnerar det högsta antalet från sin `getPriority()`-metod är den tjänst som hanterar Experience Fragment-variationen.

Du kan till exempel ha en `GenericLinkRewriterProvider` som hanterar den grundläggande mappningen för alla Experience Fragments och när metoden `shouldRewrite()` returnerar `true` för alla Experience Fragment Variations. För flera specifika Experience Fragments kanske du vill ha specialhantering, så i det här fallet kan du ange en `SpecificLinkRewriterProvider` som metoden `shouldRewrite()` bara returnerar true för vissa Experience Fragment-variationer. Om du vill vara säker på att `SpecificLinkRewriterProvider` har valts för att hantera dessa Experience Fragment-variationer måste det returnera ett högre tal i sin `getPriority()`-metod än `GenericLinkRewriterProvider.`
