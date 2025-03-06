---
title: Stöd för nya språk för lokalisering av adaptiva formulär
description: Med AEM Forms kan du lägga till nya språk för lokalisering av anpassningsbara formulär. De språkområden som stöds är som standard engelska, franska, tyska och japanska.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: Configuration
docset: aem65
feature: Adaptive Forms,Foundation Components
role: Admin,User
solution: Experience Manager, Experience Manager Forms
exl-id: 9c516c90-1b1d-406a-b42d-909aae8bb634
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---

# Stöd för nya språk för lokalisering av adaptiva formulär{#supporting-new-locales-for-adaptive-forms-localization}

| Version | Artikellänk |
| -------- | ---------------------------- |
| AEM as a Cloud Service | [Klicka här](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-foundation-components/supporting-new-language-localization.html) |
| AEM 6.5 | Den här artikeln |

## Om språkordlistor {#about-locale-dictionaries}

Lokaliseringen av anpassningsbara formulär bygger på två typer av språkordlistor:

**Formulärspecifik ordlista** innehåller strängar som används i anpassningsbara formulär. Till exempel etiketter, fältnamn, felmeddelanden, hjälpbeskrivningar och så vidare. Den hanteras som en uppsättning XLIFF-filer för varje språkinställning och du kan komma åt den på `https://<host>:<port>/libs/cq/i18n/translator.html`.

**Globala ordlistor** Det finns två globala ordlistor, som hanteras som JSON-objekt, i AEM klientbibliotek. De här ordlistorna innehåller standardfelmeddelanden, namn på månader, valutasymboler, datum- och tidsmönster osv. Dessa ordlistor finns i CRXDe Lite på /libs/fd/xfaforms/clientlibs/I18N. Dessa platser innehåller separata mappar för varje språkområde. Eftersom globala ordlistor vanligtvis inte uppdateras så ofta, kan webbläsare cachelagra olika JavaScript-filer för varje språkområde och minska bandbreddsanvändningen när de använder olika adaptiva formulär på samma server.

### Hur lokalisering av anpassningsbara formulär fungerar {#how-localization-of-adaptive-form-works}

Det finns två metoder för att identifiera det anpassade formulärets språkområde. När ett anpassat formulär återges identifieras det begärda språket av :

* tittar på väljaren `[local]` i den anpassningsbara formulärets URL. URL-formatet är `http://host:port/content/forms/af/[afName].[locale].html?wcmmode=disabled`. Om du använder väljaren `[local]` kan du cachelagra ett anpassat formulär.

* Titta på följande parametrar i den angivna ordningen:

   * Begäranparameter `afAcceptLang`
Om du vill åsidosätta webbläsarens språkområde för användare kan du skicka parametern `afAcceptLang` request för att tvinga fram språkområdet. Följande URL tvingade till exempel att återge formuläret på japanska:
     `https://'[server]:[port]'/<contextPath>/<formFolder>/<formName>.html?wcmmode=disabled&afAcceptLang=ja`

   * Webbläsarens språkområdesuppsättning för användaren, som anges i begäran med rubriken `Accept-Language`.

   * Språkinställningen för den användare som anges i AEM.

   * Webbläsarens språkområde är aktiverat som standard. Om du vill ändra språkinställningen för webbläsaren
      * Öppna konfigurationshanteraren. URL:en är `http://[server]:[port]/system/console/configMgr`
      * Leta reda på och öppna **[!UICONTROL Adaptive Form and Interactive Communication Web Channel]**-konfigurationen.
      * Ändra status för alternativet **[!UICONTROL Use Browser Locale]** och **[!UICONTROL Save]** konfigurationen.

När språkområdet har identifierats väljs den formulärspecifika ordlistan i de adaptiva formulären. Om det inte går att hitta den formulärspecifika ordlistan för den begärda språkversionen används ordlistan för det språk som det anpassade formuläret skapades på.

Om det inte finns någon språkinformation skickas ett anpassat formulär på formulärets originalspråk. Det ursprungliga språket är det språk som används vid utvecklingen av det anpassade formuläret.

Om det inte finns något klientbibliotek för det begärda språket söker programmet efter språkkoden i klientbiblioteket. Om det begärda språket till exempel är `en_ZA` (sydafrikansk engelska) och klientbiblioteket för `en_ZA` inte finns, kommer det adaptiva formuläret att använda klientbiblioteket för språket `en` (engelska), om det finns. Om det inte finns någon av dem används lexikonet för språket `en` i det adaptiva formuläret.

## Lägg till lokaliseringsstöd för språk som inte stöds {#add-localization-support-for-non-supported-locales}

AEM Forms har för närvarande stöd för lokalisering av anpassningsbara formulärinnehåll på engelska (en), spanska (es), franska (fr), italienska (it), tyska (de), japanska (ja), portugisiska-brasilianska (pt-BR), kinesiska (zh-CN), kinesiska-taiwanesiska (zh-TW) och koreanska (ko-KR).

Så här lägger du till stöd för en ny språkinställning vid körning av adaptiva formulär:

1. [Lägg till en språkinställning i tjänsten GuideLocalizationService](../../forms/using/supporting-new-language-localization.md#p-add-a-locale-to-the-guide-localization-service-br-p)

1. [Lägg till XFA-klientbibliotek för en språkinställning](../../forms/using/supporting-new-language-localization.md#p-add-xfa-client-library-for-a-locale-br-p)

1. [Lägg till anpassat formulärklientbibliotek för en språkinställning](../../forms/using/supporting-new-language-localization.md#p-add-adaptive-form-client-library-for-a-locale-br-p)
1. [Lägg till språkstöd för ordlistan](../../forms/using/supporting-new-language-localization.md#p-add-locale-support-for-the-dictionary-br-p)
1. [Starta om servern](../../forms/using/supporting-new-language-localization.md#p-restart-the-server-p)

### Lägga till en språkinställning i tjänsten för guidelokalisering {#add-a-locale-to-the-guide-localization-service-br}

1. Gå till `https://'[server]:[port]'/system/console/configMgr`.
1. Klicka för att redigera **Guiden Localization Service** -komponenten.
1. Lägg till det språkområde som du vill lägga till i listan över språkområden som stöds.

![GuideLocalizationService](assets/configservice.png)

### Lägg till XFA-klientbibliotek för en språkinställning {#add-xfa-client-library-for-a-locale-br}

Skapa en nod av typen `cq:ClientLibraryFolder` under `etc/<folderHierarchy>`, med kategorin `xfaforms.I18N.<locale>`, och lägg till följande filer i klientbiblioteket:

* **I18N.js** defining `xfalib.locale.Strings` for the `<locale>` as defined in `/etc/clientlibs/fd/xfaforms/I18N/ja/I18N`.

* **js.txt** som innehåller följande:

```text
/libs/fd/xfaforms/clientlibs/I18N/Namespace.js
I18N.js
/etc/clientlibs/fd/xfaforms/I18N/LogMessages.js
```

### Lägg till anpassat formulärklientbibliotek för en språkinställning {#add-adaptive-form-client-library-for-a-locale-br}

Skapa en nod av typen `cq:ClientLibraryFolder` under `etc/<folderHierarchy>`, med kategorin `guides.I18N.<locale>` och beroenden som `xfaforms.3rdparty`, `xfaforms.I18N.<locale>` och `guide.common`. &quot;

Lägg till följande filer i klientbiblioteket:

* **i18n.js** defining `guidelib.i18n`, med mönstren &quot;calendarSymbols&quot;, `datePatterns`, `timePatterns`, `dateTimeSymbols`, `numberPatterns`, `numberSymbols`, `currencySymbols`, `typefaces` för `<locale>` enligt XFA-specifikationerna som beskrivs i [Språkinställningsspecifikationen](https://helpx.adobe.com/content/dam/Adobe/specs/xfa_spec_3_3.pdf). Du kan också se hur den har definierats för andra språkområden som stöds i `/etc/clientlibs/fd/af/I18N/fr/javascript/i18n.js`.
* **LogMessages.js** definierar `guidelib.i18n.strings` och `guidelib.i18n.LogMessages` för `<locale>` enligt definitionen i `/etc/clientlibs/fd/af/I18N/fr/javascript/LogMessages.js`.
* **js.txt** som innehåller följande:

```text
i18n.js
LogMessages.js
```

### Lägg till språkstöd för ordlistan {#add-locale-support-for-the-dictionary-br}

Utför bara det här steget om `<locale>` som du lägger till inte finns bland `en`, `de`, `es`, `fr`, `it`, `pt-br`, `zh-cn`, `zh-tw`, `ja`, `ko-kr`.

1. Skapa en `nt:unstructured`-nod `languages` under `etc`, om den inte redan finns.

1. Lägg till en strängegenskap `languages` med flera värden i noden, om den inte redan finns.
1. Lägg till `<locale>` standardvärden för språkområde `de`, `es`, `fr`, `it`, `pt-br`, `zh-cn`, `zh-tw`, `ja`, `ko-kr`, om de inte redan finns.

1. Lägg till `<locale>` i värdena för egenskapen `languages` för `/etc/languages`.

`<locale>` visas `https://'[server]:[port]'/libs/cq/i18n/translator.html`.

### Starta om servern {#restart-the-server}

Starta om AEM-servern så att den nya språkinställningen börjar gälla.

>[!NOTE]
>
> Du bör använda kommandot Ctrl + C för att starta om SDK. Om du startar om AEM SDK med alternativa metoder, till exempel att stoppa Java-processer, kan det leda till inkonsekvenser i AEM utvecklingsmiljö.

## Exempelbibliotek för att lägga till stöd för spanska {#sample-libraries-for-adding-support-for-spanish}

Exempel på klientbibliotek för att lägga till stöd för spanska

[Hämta fil](assets/sample.zip)
