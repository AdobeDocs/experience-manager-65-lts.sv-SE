---
title: Konfigurerar cookie-användning
description: AEM tillhandahåller en tjänst som gör att du kan konfigurera och styra hur cookies används med dina webbsidor.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 219555d8-26e1-4047-b885-ec34084154c1
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Konfigurerar cookie-användning{#configuring-cookie-usage}

AEM tillhandahåller en tjänst som gör att du kan konfigurera och styra hur cookies används på dina webbsidor:

* En konfigurerbar tjänst på serversidan upprätthåller en lista över cookies som kan användas.
* Med ett JavaScript-API kan din JavaScript-kod verifiera att en cookie kan användas.

Använd den här funktionen för att kontrollera att sidorna uppfyller användarnas samtycke när det gäller användningen av cookies.

## Konfigurera tillåtna cookies {#configuring-allowed-cookies}

Konfigurera Adobe Granite-avanmälningstjänsten för att ange hur cookies ska användas på dina webbsidor. I följande tabell beskrivs de egenskaper som du kan konfigurera.

Om du vill konfigurera tjänsten kan du använda [webbkonsolen](/help/sites-deploying/configuring-osgi.md#osgi-configuration-with-the-web-console) eller [lägga till en OSGi-konfiguration i databasen](/help/sites-deploying/configuring-osgi.md#adding-a-new-configuration-to-the-repository). I följande tabell beskrivs de egenskaper som du behöver för båda metoderna. Tjänstens-PID är `com.adobe.granite.optout` för en OSGi-konfiguration.

| Egenskapsnamn (webbkonsol) | OSGi-egenskapsnamn | Beskrivning |
|---|---|---|
| Cookies för avanmälan | optout.cookies | Namnen på cookies som anger, när de finns på användarens enhet, att användaren inte har samtyckt till att använda cookies. |
| Avanmäl HTTP-huvuden | optout.headers | Namnen på HTTP-rubriker som anger, i förekommande fall, att användaren inte har samtyckt till att använda cookies. |
| Cookies i vitlista | optout.whitelist.cookies | En lista över cookies som är viktiga för att webbplatsen ska fungera och som kan användas utan användarens samtycke. |

## Verifierar cookie-användning {#validating-cookie-usage}

Använd JavaScript på klientsidan för att ringa Adobe Granite-avanmälningstjänsten för att verifiera att du kan använda en cookie. Använd Granite.OptOutUtil JavaScript-objektet för att utföra någon av följande åtgärder:

* Hämta en lista med cookie-namn som anger att användaren inte godkänner att cookies används i spårningssyfte.
* Hämta en lista med cookies som kan användas.
* Avgör om webbläsaren innehåller en cookie som anger att användaren inte godkänner användningen av cookies för spårning.
* Avgör om en viss cookie kan användas.

Klientbiblioteksmappen [granite.utils ](/help/sites-developing/clientlibs.md#referencing-client-side-libraries) innehåller objektet Granite.OptOutUtil. Lägg till följande kod i sidhuvud-JSP för att inkludera en länk till JavaScript-biblioteket:

`<ui:includeClientLib categories="granite.utils" />`

Följande JavaScript-funktion avgör till exempel om cookien COOKIE_NAME får användas innan den skrivs till den:

```
function writeCookie(value){
   if (!Granite.OptOutUtil.maySetCookie("COOKIE_NAME"))
      return;
   if (value) {
      value = encodeURIComponent(value);
      document.cookie = "COOKIE_NAME=" + value;
   }
}
```

## JavaScript-objektet Granite.OptOutUtil {#the-granite-optoututil-javascript-object}

Granite.OptOutUtil gör att du kan avgöra om cookie-användning är tillåten.

### funktionen getCookieNames() {#getcookienames-function}

Namnen på de cookies som, i förekommande fall, anger att användaren inte har gett sitt samtycke till användningen av cookies.

**Parametrar**

Ingen.

**Returnerar**

En array med cookie-namn.

#### funktionen getWhitelistCookieNames() {#getwhitelistcookienames-function}

Namnen på cookies som kan användas oavsett användarens samtycke.

**Parametrar**

Ingen.

**Returnerar**

En array med cookie-namn.

#### funktionen isOptedOut() {#isoptedout-function}

Avgör om användarens webbläsare innehåller cookies som anger att samtycke inte har getts för att använda cookies.

**Parametrar**

Ingen.

**Returnerar**

Ett booleskt värde på `true` om en cookie påträffas som inte anger något samtycke, och värdet `false` om inga cookies indikerar icke-samtycke.

### Funktionen maySetCookie(cookieName) {#maysetcookie-cookiename-function}

Avgör om en viss cookie kan användas i användarens webbläsare. Den här funktionen motsvarar att använda funktionen `isOptedOut` för att avgöra om den angivna cookien finns med i listan som funktionen `getWhitelistCookieNames` returnerar.

**Parametrar**

* cookieName: String. Namnet på kakan.

**Returnerar**

Ett booleskt värde på `true` om `cookieName` kan användas, eller värdet `false` om `cookieName` inte kan användas.
