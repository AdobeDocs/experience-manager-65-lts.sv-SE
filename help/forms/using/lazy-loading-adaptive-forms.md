---
title: Förbättra prestanda för stora formulär med lat inläsningsverktyg
description: Lazy loading förbättrar prestanda avsevärt för stora och komplexa adaptiva formulär genom att skjuta upp initieringen och inläsningen av formulärfragment tills de syns.
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: develop
docset: aem65
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: User, Developer
exl-id: 768f2f8a-6ac7-4a20-8500-bcfc960861ce
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 0%

---

# Förbättra prestanda för stora formulär med lat inläsningsverktyg{#improve-performance-of-large-forms-with-lazy-loading}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/create-an-adaptive-form-on-forms-cs/lazy-loading-adaptive-forms.html) |
| AEM 6.5 | Den här artikeln |

## Introduktion till lazy loading {#introduction-to-lazy-loading}

När formuläret blir stort och komplext med hundratals och tusentals fält får slutanvändarna lång svarstid när de återger formulär vid körning. För att minimera svarstiden kan adaptiva formulär dela upp formulär i logiska fragment och konfigureras för att skjuta upp initiering eller inläsning av fragment tills fragmentet behöver vara synligt. Det kallas för lat inläsningsarbete. Dessutom tas de fragment som konfigurerats för lazy loading bort när användaren navigerar till andra avsnitt i formuläret och fragmenten inte längre visas.

Låt oss först förstå kraven och de förberedande stegen innan du konfigurerar lazy loading.

## Förbereder för att konfigurera lazy loading {#preparing-to-configure-lazy-loading}

Innan du konfigurerar lazy loading av fragment i ditt adaptiva formulär är det viktigt att du definierar strategier för att skapa fragment, identifiera värden som används i skript eller som refereras i andra fragment samt definierar regler för att styra visningen av fält i lagerinlästa fragment.

* **Identifiera och skapa fragment**
Du kan bara konfigurera adaptiva formulärfragment för lazy loading. Ett fragment är ett fristående segment som ligger utanför ett anpassningsbart formulär och kan återanvändas i olika formulär. Så det första steget mot att implementera lat inläsningsarbete är att identifiera logiska avsnitt i ett formulär och konvertera dem till fragment. Du kan skapa ett fragment från grunden eller spara en befintlig formulärpanel som fragment.

  Mer information om hur du skapar fragment finns i [Adaptiva formulärfragment](../../forms/using/adaptive-form-fragments.md).

* **Identifiera och markera globala värden**
Forms-baserade transaktioner innefattar dynamiska element för att hämta in relevanta data från användarna och bearbeta dem för att förenkla ifyllandet av formulär. Formuläret har till exempel fält A i fragment X vars värde bestämmer giltigheten för fält B i ett annat fragment. Om fragment X i det här fallet har markerats för lazy loading måste värdet i fält A vara tillgängligt för att validera fält B även när fragment X inte har lästs in. För att uppnå detta kan du markera fält A som globalt, vilket garanterar att dess värde är tillgängligt för validering av fält B när fragment X inte har lästs in.

  Mer information om hur du gör ett fältvärde globalt finns i [Konfigurera lazy loading](../../forms/using/lazy-loading-adaptive-forms.md#p-configuring-lazy-loading-p).

* **Skriv regler för att kontrollera synlighet för fält**
Forms innehåller fält och avsnitt som inte är tillämpliga för alla användare och under alla förhållanden. Forms författare och utvecklare använder synlighets- eller visningsregler för att styra synligheten baserat på användarindata. Fältet Kontorsadress visas t.ex. inte för användare som väljer Arbetslösa i fältet Anställningsstatus i ett formulär. Mer information om hur du skriver regler finns i [Använda regelredigeraren](../../forms/using/rule-editor.md).

  Du kan använda synlighetsregler i de lagerinlästa fragmenten så att villkorsfält bara visas när de är obligatoriska. Markera dessutom det villkorliga fältet globalt så att det refererar till det i synlighetsuttrycket för det lagerinlästa fragmentet.

## Konfigurerar lazy loading {#configuring-lazy-loading}

Utför följande steg för att aktivera lazy loading på ett adaptivt formulärfragment:

1. Öppna det adaptiva formuläret i redigeringsläget som innehåller det fragment som du vill aktivera för lazy loading.
1. Markera det adaptiva formulärfragmentet och välj ![cmpr](assets/cmppr.png).
1. Aktivera **[!UICONTROL Load fragment lazily]** i sidofältet och välj **Klar**.

   ![Aktivera lazy loading för det adaptiva formulärfragmentet](assets/lazy-loading-fragment.png)

   Fragmentet är nu aktiverat för lazy loading.

Du kan markera objektvärden i det lagerinlästa fragmentet som globala så att de är tillgängliga för användning i skript när det innehållande fragmentet inte läses in. Gör följande:

1. Öppna det adaptiva formulärfragmentet i redigeringsläge.
1. Markera fältet vars värde du vill markera som globalt och välj sedan ![cmpr](assets/cmppr.png).
1. Aktivera **Använd värde vid lazy loading** i sidofältet.

   ![Lazy loading field in sidebar](assets/enable-lazy-loading.png)

   Värdet är nu markerat som globalt och kommer att vara tillgängligt för användning i skript även när det innehållande fragmentet har tagits bort.

## Överväganden och bästa praxis för konfiguration av lat inläsningsarbete {#considerations-and-best-practices-for-configuring-lazy-loading}

Vissa begränsningar, rekommendationer och viktiga punkter som du bör tänka på när du arbetar med lazy loading är följande:

* Använd XSD-schemabaserade adaptiva formulär över XFA-baserade adaptiva formulär för att konfigurera lat inläsande på stora formulär. Prestandavinster på grund av lazy loading-implementering i XFA-baserade adaptiva formulär är relativt mindre än förstärkning i XSD-baserade adaptiva formulär.
* Konfigurera inte lat inläsning på fragment i ett adaptivt formulär som använder layouten **[!UICONTROL Responsive -everything on one page without navigation]** för rotpanelen. Som ett resultat av layoutkonfigurationen Responsiv läses alla fragment in samtidigt i en adaptiv form. Det kan också leda till försämrade prestanda.
* Vi rekommenderar att du inte konfigurerar lazy loading för det första avsnittet i en adaptiv form.
* Vi rekommenderar att du inte konfigurerar lazy loading på fragment på den första panelen som återges när det adaptiva formuläret läses in.
* Lazy loading stöds upp till två nivåer i fragmenthierarkin.
* Se till att fält som markerats som globala är unika i ett adaptivt formulär.
* Överväg att skriva synlighetsregler för fragment som ska visas eller döljas baserat på ett villkor. Du kan till exempel visa eller dölja fragmentet med information om make/maka baserat på den civilstånd som anges av en användare.
* Komponenter för bifogade filer och villkor stöds inte i lagerinlästa fragment.

### Bästa skriptpraxis för konfigurering av lazy loading {#scripting-best-practices-for-configuring-lazy-loading}

Viktiga punkter att tänka på när du utvecklar skript för lazy loading-paneler är följande:

* Se till att initiera och beräkna skript som används på fälten i ett lazy loaded fragment är idempotenta till sin natur. Idempotenta skript är sådana som har samma effekt även efter flera exekveringar.
* Använd den globalt tillgängliga egenskapen för fält för att göra fältvärden i en lat inläsningspanel tillgängliga för alla andra paneler i ett formulär.
* Vidarebefordra inte referensvärdet för ett fält i en lat panel oavsett om fältet markeras globalt över fragment eller inte.
* Använd panelåterställningsfunktionen för att återställa allt som visas på panelen med följande klickuttryck.\
  guideBridge.resolveNode(guideBridge.getFocus({&quot;focusOption&quot;: &quot;navigablePanel&quot;})).resetData()
