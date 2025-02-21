---
title: Internationaliserar gränssnittssträngar
description: Java&handel; och JavaScript API:er gör att du kan internationalisera strängar
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: components
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1091'
ht-degree: 0%

---

# Internationaliserar gränssnittssträngar {#internationalizing-ui-strings}

Med API:erna för Java™ och JavaScript kan du internationalisera strängar med följande typer av resurser:

* Java™-källfiler.
* JSP-skript.
* JavaScript i bibliotek på klientsidan eller i sidkällan.
* Egenskapsvärden för JCR-nod som används i dialogrutor och komponentkonfigurationsegenskaper.

En översikt över internationaliseringen och lokaliseringsprocessen finns i [Internationalisering av komponenter](/help/sites-developing/i18n.md).

## Internationalisering av strängar i Java™ och JSP Code {#internationalizing-strings-in-java-and-jsp-code}

Med Java™-paketet `com.day.cq.i18n` kan du visa lokaliserade strängar i ditt användargränssnitt. Klassen `I18n` innehåller metoden `get` som hämtar lokaliserade strängar från Adobe Experience Manager-ordlistan (AEM). Den enda obligatoriska parametern för metoden `get` är stränglitteralen på engelska. Engelska är standardspråk för användargränssnittet. I följande exempel lokaliseras ordet `Search`:

`i18n.get("Search");`

Att identifiera strängen på det engelska språket skiljer sig från vanliga internationaliseringsramverk där ett ID identifierar en sträng och används för att referera till strängen vid körning. Att använda den engelska stränglitteralen ger följande fördelar:

* Kod är lätt att förstå.
* Strängen i standardspråket är alltid tillgänglig.

### Bestämma användarens språk {#determining-the-user-s-language}

Det finns två sätt att avgöra vilket språk användaren föredrar:

* För autentiserade användare anger du språket i inställningarna för användarkontot.
* Språkinställningen för den begärda sidan.

Användarkontots language-egenskap är den metod som rekommenderas eftersom den är mer tillförlitlig. Användaren måste dock vara inloggad för att kunna använda den här metoden.

#### Skapa I18n Java™-objektet {#creating-the-i-n-java-object}

I18n-klassen innehåller två konstruktorer. Hur du avgör vilket språk användaren föredrar avgör vilken konstruktor som ska användas.

Om du vill presentera strängen på det språk som anges i användarkontot använder du följande konstruktor (efter import av `com.day.cq.i18n.I18n)`):

```java
I18n i18n = new I18n(slingRequest);
```

Konstruktorn använder `SlingHTTPRequest` för att hämta användarens språkinställning.

Om du vill använda sidans språkområde för att fastställa språket hämtar du först ResourceBundle för den begärda sidans språk:

```java
Locale pageLang = currentPage.getLanguage(false);
ResourceBundle resourceBundle = slingRequest.getResourceBundle(pageLang);
I18n i18n = new I18n(resourceBundle);
```

#### Internationalisering av en sträng {#internationalizing-a-string}

Använd metoden `get` för objektet `I18n` för att internationalisera en sträng. Den enda obligatoriska parametern för metoden `get` är strängen för internationalisering. Strängen motsvarar en sträng i en Translator-ordlista. get-metoden söker upp strängen i ordlistan och returnerar översättningen för det aktuella språket.

Det första argumentet för metoden `get` måste uppfylla följande regler:

* Värdet måste vara en stränglitteral. En variabel av typen `String` kan inte accepteras.
* Stränglitteralen måste uttryckas på en rad.
* Strängen är skiftlägeskänslig.

```xml
i18n.get("Enter a search keyword");
```

#### Använda översättningstips {#using-translation-hints}

Ange [översättningstips](/help/sites-developing/i18n-translator.md#adding-changing-and-removing-strings) för den internationaliserade strängen för att skilja mellan dubblettsträngar i ordlistan. Använd den andra valfria parametern för metoden `get` för att tillhandahålla översättningstipset. Översättningstipset måste exakt matcha kommentaregenskapen för objektet i ordlistan.

Ordlistan innehåller till exempel strängen `Request` två gånger: en gång som ett verb och en gång som ett substantiv. I följande kod inkluderas översättningstipset som ett argument i metoden `get`:

```java
i18n.get("Request","A noun, as in a request for a web page");
```

#### Inkludera variabler i lokaliserade meningar {#including-variables-in-localized-sentences}

Inkludera variabler i den lokaliserade strängen för att skapa sammanhangsbaserad betydelse i en mening. När du har loggat in i ett webbprogram visas till exempel meddelandet &quot;Welcome back Administrator&quot; på startsidan. Du har två meddelanden i din inkorg.&quot; Sidkontexten bestämmer användarnamnet och antalet meddelanden.

[I ordlistan](/help/sites-developing/i18n-translator.md#adding-changing-and-removing-strings) representeras variablerna i strängar som index inom parentes. Ange värdena för variablerna som argument för metoden `get`. Argumenten placeras efter översättningstipset och indexen motsvarar argumentens ordning:

```xml
i18n.get("Welcome back {0}. You have {1} messages.", "user name, number of messages", user.getDisplayName(), numItems);
```

Den internationaliserade strängen och översättningstipset måste exakt matcha strängen och kommentaren i ordlistan. Du kan utelämna lokaliseringstipset genom att ange ett `null`-värde som det andra argumentet.

#### Använda metoden Statisk hämtning {#using-the-static-get-method}

Klassen `I18N` definierar en statisk `get`-metod som är användbar när du måste lokalisera några strängar. Förutom parametrarna för ett objekts `get`-metod kräver den statiska metoden det `SlingHttpRequest` -objekt eller den `ResourceBundle` som du använder, enligt hur du avgör vilket språk som användaren föredrar:

* Använd användarens språkinställning: Ange SlingHttpRequest som den första parametern.

  `I18n.get(slingHttpRequest, "Welcome back {}. You have {} messages.", "user name, number of messages", user.getDisplayName(), numItems);`
* Använd sidspråket: Ange ResourceBundle som den första parametern.

  `I18n.get(resourceBundle,"Welcome back {}. You have {} messages.", "user name, number of messages", user.getDisplayName(), numItems);`

### Internationalisering av strängar i JavaScript Code {#internationalizing-strings-in-javascript-code}

Med JavaScript API kan du lokalisera strängar på klienten. Precis som med [Java™- och JSP](#internationalizing-strings-in-java-and-jsp-code)-kod kan du med JavaScript API identifiera strängar för lokalisering, tillhandahålla lokaliseringstips och ta med variabler i de lokaliserade strängarna.

Klientbiblioteksmappen `granite.utils` [](/help/sites-developing/clientlibs.md) innehåller JavaScript API. Om du vill använda API:t inkluderar du den här klientbiblioteksmappen på sidan. Lokaliseringsfunktioner använder namnutrymmet `Granite.I18n`.

Innan du presenterar lokaliserade strängar anger du språkområdet med funktionen `Granite.I18n.setLocale`. Funktionen kräver språkkoden för språkområdet som ett argument:

```
Granite.I18n.setLocale("fr");
```

Om du vill presentera en lokaliserad sträng använder du funktionen `Granite.I18n.get`:

```
Granite.I18n.get("string to localize");
```

I följande exempel internationaliseras strängen &quot;Welcome back&quot;:

```
Granite.I18n.setLocale("fr");
Granite.I18n.get("string to localize", [variables], "localization hint");
```

Funktionsparametrarna skiljer sig från Java™ I18n.get-metoden:

* Den första parametern är stränglitteralen som ska lokaliseras.
* Den andra parametern är en array med värden som ska matas in i stränglitteralen.
* Den tredje parametern är lokaliseringstipset.

I följande exempel används JavaScript för att lokalisera administratören för välkomstsidan. Du har två meddelanden i din inkorg.&quot; mening:

```
Granite.I18n.setLocale("fr");
Granite.I18n.get("Welcome back {0}. You have {1} new messages in your inbox.", [username, numMsg], "user name, number of messages");
```

### Internationalisering av strängar från JCR-noder {#internationalizing-strings-from-jcr-nodes}

Gränssnittssträngar baseras ofta på egenskaper för JCR-noder. Egenskapen `jcr:title` för en sida används till exempel vanligtvis som innehåll för elementet `h1` i sidkoden. Klassen `I18n` innehåller metoden `getVar` för att lokalisera strängarna.

Följande exempel på JSP-skript hämtar egenskapen `jcr:title` från databasen och visar den lokaliserade strängen på sidan:

```java
<% title = properties.get("jcr:title", String.class);%>
<h1><%=i18n.getVar(title) %></h1>
```

#### Ange översättningstips för JCR-noder {#specifying-translation-hints-for-jcr-nodes}

Ungefär som [översättningstips i Java™ API](#using-translation-hints) kan du ange översättningstips för att skilja på dubblettsträngar i ordlistan. Ange översättningstipset som en egenskap för noden som innehåller den internationaliserade egenskapen. Namnet på tipsegenskapen består av namnet på den internationella egenskapen med suffixet `_commentI18n`:

`${prop}_commentI18n`

En `cq:page`-nod innehåller till exempel egenskapen jcr:title som lokaliseras. Tipset anges som värdet för egenskapen jcr:title_commentI18n.

### Testa täckningen för internationalisering {#testing-internationalization-coverage}

Testa om du har internationaliserat alla strängar i användargränssnittet. Om du vill se vilka strängar som täcks anger du användarspråket till zz_ZZ och öppnar användargränssnittet i webbläsaren. De internationaliserade strängarna visas med en stub-översättning i följande format:

`USR_*Default-String*_尠`

Följande bild visar stub-översättning för AEM hemsida:

![chlimage_1](assets/chlimage_1a.jpeg)

Konfigurera språkegenskapen för inställningsnoden för användarkontot för att ställa in språket för användaren.

Noden preferences för en användare har en sökväg som den här:

`/home/users/<letter>/<hash>/preferences`

![chlimage_1-1](assets/chlimage_1-1a.jpeg)
