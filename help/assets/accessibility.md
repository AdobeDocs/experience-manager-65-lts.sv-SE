---
title: Tillgängliga funktioner och gränssnitt för  [!DNL Experience Manager Assets]
description: Se hur tillgänglighetsfunktionerna i  [!DNL Adobe Experience Manager] 6.5 LTS [!DNL Assets] kan hjälpa användare med funktionshinder.
feature: Asset Management
role: User, Architect, Leader
solution: Experience Manager, Experience Manager Assets
exl-id: f9540bfb-1c4f-41f0-9caa-ef9265225648
source-git-commit: 61a1bb55f4b8da8c421f51ee203ca8e433a73a64
workflow-type: tm+mt
source-wordcount: '1880'
ht-degree: 0%

---

<!--
Possible topics to cover in this article are below.

* Compile a list of enhancements done in the last ~1 year.
* Showcase a few prominent use cases (search?) in a screencast.
* Top-level actions supported, such as clickable UI elements, keyboard shortcuts, popup dialogs, and so on.
* List all UIs that are keyboard navigable.
* Unified list of the product tasks supported, such as, search assets, download assets, add or editing metadata, use DM Viewers, and so on.
* Do we need to add support matrix of user tasks with browser and screen reader combinations. Everything may not work in all browsers and/or using all screen readers.
* Any exceptions that users should be aware of. It may help to call out (it may be done in ACR) what tasks are NOT supported.
* CTAs – what's next and more info from AEM team:
  * Link to ACRs on a.com.
  * Generic a11y info by Adobe to begin with.
  * Link to a11y-specific online methods to report issues, seek support, or request enhancements, if any. Asked the a11y team on Slack.
-->

# Hjälpmedelsfunktioner i [!DNL Adobe Experience Manager Assets] {#accessibility-in-aem-assets}

Med [!DNL Adobe Experience Manager] kan innehållsskapare och utgivare leverera fantastiska upplevelser på webben. Adobe strävar efter att inkludera skapare med funktionshinder genom att förbättra tillgängligheten för [!DNL Experience Manager]. Programmen har ständigt förbättrats för att uppfylla behoven hos alla typer av användare och följer världsstandarden som omfattar personer med nedsatt syn, hörsel, mobilitet eller andra funktionshinder.

[!DNL Experience Manager] publicerar överensstämmelseinformation som beskriver de standarder som den följer, beskriver tillgänglighetsfunktionerna i produkten och beskriver efterlevnadsnivån. Rapporterna om överensstämmelse för tillgänglighet hjälper [!DNL Experience Manager]-användare att förstå graden av efterlevnad av olika standarder. Tack vare förbättringarna i [!DNL Assets] kan alla användare enkelt använda gränssnitten via tangentbord, skärmläsare, förstoringsglaset och annan hjälpmedelsteknik.

[!DNL Experience Manager] har olika supportnivåer för följande standarder:

* [Riktlinjer för tillgängligt webbinnehåll (WCAG) 2.1](https://www.w3.org/TR/WCAG/).
* [Reviderat avsnitt 508 i Rehabilitation Act](https://www.access-board.gov/guidelines-and-standards/communications-and-it/about-the-ict-refresh/final-rule/text-of-the-standards-and-guidelines).
* [Accessibility Initiative - Accessible Rich Internet Applications (WAI-ARIA) av W3C](https://www.w3.org/WAI/standards-guidelines/aria/).
* [EN 301 549](https://en.wikipedia.org/wiki/EN_301_549).

Om du vill läsa en rapport med information om efterlevnadsnivån läser du [Rapport om tillgänglighetskonformning](https://www.adobe.com/accessibility/compliance.html) (ACR).

Mer information om hur [!DNL Dynamic Media] är tillgänglig finns i [hjälpmedel i [!DNL Dynamic Media]](/help/assets/accessibility-dm.md).

## Hjälpmedel {#at-support}

Användare med funktionshinder förlitar sig ofta på maskinvara och programvara för att få tillgång till webbinnehåll och använda programvaror. Dessa verktyg kallas hjälpmedelstekniker. [!DNL Experience Manager Assets] kan arbeta med följande typer av hjälpmedelstekniker (AT) när programvarans kärnfunktioner används:

* Skärmläsare och skärmförstorare.
* Taligenkänningsprogram.
* Tangentbordsanvändning - navigering och genvägar.
* Hjälpmaskinvara, inklusive switchkontroller, uppdateringsbara blindskriftsskärmar och andra indataenheter.
* Förstoringsverktygen i användargränssnittet.

## [!DNL Experience Manager Assets] använder fall som är tillgängliga {#accessible-assets-use-cases}

I [!DNL Experience Manager] uppfyller hjälpmedelsfunktionerna två huvudkrav för [!DNL Experience Manager]-användare och deras kunder.

* För designers och kreatörer finns det funktioner för att skapa och publicera tillgängligt innehåll som i sin tur används av kunder och besökare på webbplatser. Personer med funktionshinder använder innehållet med hjälp av hjälpmedelstekniker. Mer information finns i [Riktlinjer för webbhjälpmedel](/help/managing/web-accessibility.md).
* [!DNL Experience Manager] ger även användare och administratörer med funktionshinder åtkomst till användargränssnittet och kontrollerna för att skapa och hantera innehåll. Personer med funktionshinder kan använda hjälpmedelstekniker för att navigera, använda och hantera [!DNL Assets]-funktionen.

Kärnfunktionerna i [!DNL Assets] är mer tillgängliga än tidigare och uppdateras regelbundet för att förbättra efterlevnaden av globala standarder. CRUD-åtgärderna i [!DNL Assets] har en viss grad av inbyggda hjälpmedel. DAM-arbetsflöden som att lägga till, hantera, söka efter och distribuera resurser är tillgängliga med hjälp av kortkommandon, skärmläsartext, färgkontrast och så vidare.

## Stöd för användning av tangentbord {#keyboard-use}

Många element i användargränssnittet som är klickbara eller kan användas med en pekare kan också aktiveras med tangentbordet. Med ett tangentbord kan användarna fokusera på gränssnittselement och vidta lämpliga åtgärder. Användare kan använda kortkommandon direkt för att utlösa ett kommando eller en åtgärd utan att behöva fokusera på gränssnittselement och utlösa det med tangentbordet. Användare kan till exempel öppna tidslinjen för en resurs på vänstra sidan av användargränssnittet genom att bläddra till användargränssnittskontrollen med ett tangentbord och välja `Return` och sedan välja kortkommandot `Alt + 2`.

<!-- TBD items:

* The option to toggle between list view and card view exposes relevant info to the screen readers. What about column view option? This info can go into 'basic handling' info aka article to 'understand and use the workspace'.
* How to open and browse through the profile popup dialog in [!DNL Experience Manager] UI using a keyboard? The navigation does not match the order of visual display of options on the UI. This info can go into 'basic handling' info aka article to 'understand and use the workspace'. What about setting preferences and impersonating a user?
* Using the [!DNL Experience Manager] tag browser and operating the options like delete tag? This info can go into 'basic handling' info aka article to 'understand and use the workspace'.
* Read-only form fields can be focused with the keyboard. Can users tab to these fields to understand the contents and are they able to copy text from the fields?
-->

### Kortkommandon i [!DNL Assets] {#keyboard-shortcuts}

Följande åtgärder i [!DNL Assets] fungerar med de kortkommandon som visas. De flesta kortkommandon som gäller för [!DNL Experience Manager]-konsoler gäller även för [!DNL Assets]. Se [kortkommandon för konsoler](/help/sites-authoring/keyboard-shortcuts.md#keyboard-shortcuts). Se hur du [aktiverar eller inaktiverar kortkommandon](/help/sites-authoring/keyboard-shortcuts.md#deactivating-keyboard-shortcuts).

| Användargränssnitt eller scenario | Kortkommando | Åtgärd |
|---|---|---|
| Kolumnvy i användargränssnittet [!DNL Assets] | Uppåtpil och nedåtpil | Navigera till filer och mappar i samma hierarki. |
| Kolumnvy i användargränssnittet [!DNL Assets] | Vänster- och högerpilstangenter | Navigera till filer och mappar ovanför eller nedanför den aktuella mappen. |
| Bläddrar bland mappar i [!DNL Assets] | `/` | Anropa sökning genom att öppna Omnissearch-rutan. |
| [!DNL Assets]-konsol | &grave; | Växla sidoskenor |
| [!DNL Assets]-konsol | `Alt + 1` | Öppna innehållsträdet. |
| [!DNL Assets]-konsol | `Alt + 2` | Öppna den vänstra listen [!UICONTROL Navigation]. |
| [!DNL Assets]-konsol | `Alt + 3` | Visa [!UICONTROL Timeline] för en vald resurs. |
| [!DNL Assets]-konsol | `Alt + 4` | Öppna Live Copy-referenser för den valda resursen. |
| [!DNL Assets]-konsol | `Alt + 5` | Starta sökning och sökning i den markerade mappen. |
| Resursen eller mappen är markerad | Backsteg | Ta bort den markerade resursen eller mappen. |
| Resursen eller mappen är markerad | `p` | Öppna sidan Egenskaper för den valda resursen. |
| Resursen eller mappen är markerad | `e` | Redigera den valda resursen. |
| Resursen eller mappen är markerad | `m` | Flytta den markerade resursen. |
| Resursen eller mappen är markerad | `Ctrl + c` | Kopiera den markerade resursen. |
| Resursen eller mappen är markerad | `Esc` | Avbryt markeringen. |
| Dialogrutan öppnas och är i fokus | `Esc` | Stäng dialogrutan. |
| I en mapp i DAM | `Ctrl + v` | Klistra in kopierad resurs. |
| [!DNL Assets]-konsol | `Ctrl + A` | Markera alla resurser. |
| Egenskapssidor för resurs | `Ctrl + S` | Spara ändringar. |
| [!DNL Assets]-konsol | `?` | Visa en lista med kortkommandon. |

## Logga in och navigera i användargränssnittet för [!DNL Assets] {#login}

Användare kan använda tangentbordet för att navigera till och fylla i inloggningsfältet för att logga in. Felmeddelandena på grund av felaktiga kombinationer av användarnamn och lösenord på inloggningssidan meddelas av skärmläsare varje gång felet inträffar.

Efter inloggning kan DAM-användare navigera i användargränssnittet för [!DNL Assets] med hjälp av tangentbordet. Elementen i användargränssnittet, som vänsterkant, menyer, användarprofil, sökfält, filer och mappar samt inställningar för administration och konfiguration, kan navigeras med tangentbordet. Tangentbordsnavigeringsordningen är från vänster till höger och uppifrån och ned. När du navigerar med ett tangentbord markeras ett alternativ som kan användas när det är i fokus med bättre färgkontrast och berättas av en skärmläsare. Om det är lämpligt kommer läget - t.ex. utvidgat, komprimerat och blandat läge - för de fokuserade alternativen på menyn att meddelas av en skärmläsare. Skärmläsaren meddelar även syftet med det användbara alternativet, i stället för utseendet eller gränssnittets placering.

Om en användare utökar alternativet för hjälp eller användarprofil på menyn, visas rätt alternativ eller status i skärmläsaren. Om en användare utökar alternativet för användarprofil kan de tillgängliga alternativen väljas med hjälp av ett tangentbord. En administratör kan till exempel personifiera en annan användare. Om en användare söker efter en sträng från alternativet [!UICONTROL Help], meddelar en berättare om &quot;Hjälp för sökning&quot; för att ange att en sökning pågår.

<!-- TBD: Removing for now. Add a more informative video later. Host it on tv.adobe

![Keyboard navigation of top options in [!DNL Experience Manager] user interface](assets/keyboard-navigation-in-aem.gif)

*Figure: Navigating through the options at the top of [!DNL Experience Manager] user interface using `Tab` key.*
-->

## Bläddra bland resurser och visa relaterad information {#browse}

I användargränssnittet i [!DNL Assets] kan användare använda tangentbordet för att bläddra igenom listan över befintliga digitala resurser i DAM-databasen, förhandsgranska eller ladda ned en resurs, se genererade återgivningar, växla vyer, se genererade återgivningar, se tidslinje- och versionshistorik, se kommentarer och referenser samt visa och hantera metadata.

<!-- TBD: Not sure about the following list items mean:

In [!DNL Experience Manager] header section, when navigating in browse mode, screen reader now announces,
  
  * Suggestions to search in Omnisearch.
  * The state as expanded or collapsed for Solutions, Help, Inbox and User options.
  * The Searching Help status message that is displayed when user enters a search string in Search for Help field under Help option
  * The error message if incorrect value is entered in Impersonate as field under User option and focus correctly moves to the text field (NPR-33804).

Review CQ-4282133 before adding - Close option in a coral-dialog was not accessible through keyboard, due to which user cannot trigger close option through keyboard press in version preview dialog. After fix, user can close dialog through close option using keyboard.

* CQ-4273122 - Assets of video/audio type will have aria-label in format "Multimedia player: <Title>" so users relying on screen-reader will get to know that they are video/audio assets.
-->

Följande funktioner förbättrar tillgängligheten när du bläddrar i resurskatalogen:

* Skärmläsaren meddelar textalternativ som avbildar ikonernas syfte eller funktion i stället för deras namn.
* Användare kan komma åt och fokusera alternativen för det interaktiva användargränssnittet i referenslistan med resurser med hjälp av tangentbordstangenter.
* Elementen på varje rad i listvyn presenteras som elementen på samma rad av skärmläsare.
* När du navigerar med `Tab`-tangenten kan fokus flyttas till stängningsalternativet i förhandsversionen.
* När du använder tangentbordet för att bläddra har de markerade alternativen för det användbara gränssnittet mer framträdande visuellt fokus med förbättrad kontrast. Det gör det fokuserade området mer identifierbart för användaren.
* Om du använder `Esc`-tangenten för att ta bort snabbåtgärdsikonerna från miniatyrbildsvyn tas inte tangentbordsfokus bort från det senast fokuserade objektet.
* När en resurs är markerad och du väljer kortkommandot `Alt + 4` öppnas listan [!UICONTROL References] i den vänstra listen. Med nyckeln `Tab` kan användare navigera genom referensposter som inte är noll. Att bläddra igenom enbart referensposter som inte är noll sparar både arbete och tangenttryckningar.
* Kommentarer om en resurs är tillgängliga på resursens tidslinje. Det är tillgängligt om du har åtkomst till den vänstra listen via ett tangentbord eller ett kortkommando.
* [!UICONTROL View Settings] i [!DNL Experience Manager] är tillgängliga via ett tangentbord. Användare kan navigera bland de tillgängliga kortstorlekarna med piltangenterna och välja och bläddra igenom för att ange andra element i den befintliga vyn Visa inställningar.

<!-- TBD: Gradually, as more enhancements are done in these categories, add more content.

## Add and upload digital assets {#upload}

## Configure and administer [!DNL Assets] {#config-admin}

* List the a11y fixes in workflows to configure and administer [!DNL Experience Manager Assets]?
* Some enhancements in Processing profiles creation or application to a folder?
* Some enhancements to metadata properties UI?
-->

## Hantera digitala resurser {#manage-assets}

Många resurshanteringsåtgärder, som CRUD-åtgärder, hämtning av resurser och tillägg av metadata, är tillgängliga i olika grader. Med [!DNL Assets] kan du utföra åtgärderna med hjälp av olika hjälpmedelstekniker, till exempel en skärmläsare och ett tangentbord.

Se en videodemonstration av hur du använder ett tangentbord för att [bläddra i databasen och hämta en resurs](https://youtu.be/K3dgqMRQJys).

För metadataåtgärder som vanligtvis utförs av roller som marknadsförare och administratörer förbättrar följande funktioner tillgängligheten:

* [!UICONTROL Save & Close]-alternativet på resursen [!UICONTROL Properties] kan nu nås via tangentbordet.
* Skärmläsare meddelar alternativen för att ta bort de markerade taggarna på fliken [!UICONTROL Basic] för resursen [!UICONTROL Properties].
* Användare kan använda popup-dialogrutan Datumväljare med ett tangentbord. Användargränssnittselementet Datepicker används för att ställa in tider och tider och välja datum.
* Dragningsfunktionen med tangentbordet fungerar korrekt i [!UICONTROL Metadata Schema Editor] i skärmläsarläge.
* En användare kan flytta fokus med tangentbordet till fältet Lägg till användare eller grupp under [!UICONTROL Closed User Group] på fliken [!UICONTROL Permissions] i mappen [!UICONTROL Properties].

## Söka efter digitala resurser {#search-assets}

En snabb och smidig sökupplevelse av resurser ökar innehållets hastighet. Användningsexempel för innehållshastighet är en del av kärnfunktionen [!DNL Assets]. Om du vill starta en sökning från omsökningsfältet kan användarna använda kortkommandot `/` eller använda `Tab` tillsammans med skärmläsare för att snabbt hitta sökalternativet. Skärmläsaren lägger till namnet på alternativet som &quot;Sökknapp&quot; när sökalternativet ![sökalternativ](assets/do-not-localize/search_icon.png) är i fokus. Användare kan välja `Return` för att öppna sökrutan. Skärmläsaren lägger inte bara till en berättarröst för det nyckelord som skrivs i sökrutan, utan lägger även till en berättarröst för förslagen från [!DNL Experience Manager Assets]. Användare kan använda en kombination av piltangenter, `Return` och `Tab` för att komma åt de olika alternativen för att utlösa en sökning.

Sökfunktionen är tillgänglig med följande funktioner:

* Sidtitel, som är tillgänglig för en skärmläsare, hjälper dig att identifiera sidan som resursens söksida.
* Användare söker efter resurser inifrån sökfältet. Användarna kan öppna den antingen med hjälp av tangentbordsnavigering eller med kortkommandot `/`.
* Användarna kan börja skriva söknyckelordet och sedan välja de automatiska förslagen med piltangenterna. Markerat förslag kan väljas med tangenten `Return` och resurser söks efter det valda förslaget.
* Skärmläsare kan identifiera och meddela kryssrutorna för blandade tillstånd (där kryssrutorna på första nivån inte markeras och genomstrykas om du inte markerar alla kapslade prefix) i panelen Filter när sökresultaten filtreras.
* Användarfokus flyttas till sökalternativen när Omnissökrutan har stängts.

Vid filtrering av sökresultat:

* Sökresultatsidan har en informativ titel som ger en bättre förståelse för skärmläsaranvändare.
* En skärmläsare meddelar alternativen i sökfiltret som utökningsbara dragspelspaneler.
* Prognoser med alternativ för blandat läge meddelas av skärmläsare.

## Dela resurser {#share-assets}

<!-- TBD: Anything about accessibility in DA, BP? AAL team confirmed that there's no content for AAL a11y on helpx.
-->

Följande funktioner förbättrar tillgängligheten när du delar resurser:

* En användare kan flytta fokus med tangentbordet i fältet Sök och lägg till e-postadress i dialogrutan för länkdelning.

* Skärmläsarna visas i dialogrutan för länkdelning när du navigerar i bläddringsläge.

   * Lägg inte in en berättarröst i tabellinformationen när dialogrutan öppnas.
   * Kan navigera till alla förslag i listan.
   * Lägg till en berättarröst för de förslag som visas för fälten Lägg till e-postadress och Sök.

## Tillgänglig dokumentation {#accessible-docs}

[!DNL Experience Manager] innehåller hjälpmedelsanpassad dokumentation som kan användas av personer med funktionshinder. Följande hjälper till att göra innehållet tillgängligt för tillfället, medan Adobe fortsätter att förbättra mallen och innehållet:

* Skärmläsare kan läsa texten.
* Bilder och illustrationer har alternativ text tillgänglig.
* Tangentbordsnavigering är möjlig.
* Kontrastförhållanden hjälper till att lyfta fram vissa delar av dokumentationswebbplatsen.

## Ge feedback {#a11y-feedback}

Om du vill ge feedback, ställa frågor och begära produktförbättringar som rör tillgänglighet använder du följande metoder:

* Fyll i formuläret på [www.adobe.com/accessibility/feedback.html](https://www.adobe.com/accessibility/feedback.html).
* Mejla oss på access@adobe.com.

>[!MORELIKETHIS]
>
>* [Hjälpmedelsfunktioner i [!DNL Dynamic Media]](/help/assets/accessibility-dm.md).
>* [Versionsinformation om förbättringar som har gjorts i varje Service Pack-version](/help/release-notes/release-notes.md).
>* [[!DNL Adobe Experience Manager] hjälpmedelsvägledning](/help/managing/web-accessibility.md).
>* [Överensanderapporter (ACR) och VPAT-listor för Adobe-lösningar](https://www.adobe.com/accessibility/compliance.html).
