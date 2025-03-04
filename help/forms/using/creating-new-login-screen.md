---
title: Skapa en inloggningsskärm
description: Så här ändrar du inloggningssidan för LiveCycle-moduler, till exempel för arbetsytan i AEM Forms eller Forms Manager.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: forms-workspace
docset: aem65
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
source-git-commit: 168cb023768ff3139937ab7f437ab7d00185bca0
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 1%

---

# Skapa en inloggningsskärm{#creating-a-new-login-screen}

Du kan ändra inloggningsskärmen för alla AEM Forms-moduler som använder inloggningsskärmen för AEM Forms. Ändringarna påverkar till exempel inloggningsskärmen för både Forms Manager och AEM Forms.

## Förutsättning {#prerequisite}

1. Logga in på `/lc/crx/de` med administratörsbehörighet.
1. Gör följande:

   1. Replikera den hierarkiska strukturen: av `/libs/livecycle/core/content` vid `/apps/livecycle/core/content`.

      Behåll samma egenskaper (nod/mapp) och åtkomstkontroll.

   1. Kopiera innehållsmappen:

      från: `/libs/livecycle/core`

      till: `/apps/livecycle/core`.

   1. Ta bort innehållet i mappen `/apps/livecycle/core`.

1. Utför följande åtgärder:

   1. Replikera den hierarkiska strukturen: av `/libs/livecycle/core/components/login` vid `/apps/livecycle/core/components/login`. Behåll samma egenskaper (nod/mapp) och åtkomstkontroll.

   1. Kopiera komponentmappen: från `/libs/livecycle/core` till `/apps/livecycle/core`.

   1. Ta bort innehållet i mappen: `/apps/livecycle/core/components/login`.

### Lägga till en ny språkinställning {#adding-a-new-locale}

1. Kopiera mappen `i18n`:

   * från `/libs/livecycle/core/components/login`
   * till `/apps/livecycle/core/components/login`

1. Ta bort alla mappar i `i18n` utom en, till exempel `en`.

1. Utför följande åtgärder i mappen `en`:

   1. Byt namn på mappen till det språknamn som du vill använda. Exempel: `ar`.

   1. Ändra värdet för egenskapen `jcr:language` till `ar`(för mappen `ar`).

   >[!NOTE]
   >
   >Om språkinställningen är en kombination av språkkod, till exempel `ar-DZ`, ändrar du mappnamnet och egenskapsvärdet till `ar-DZ`.

1. Kopiera `login.jsp`:

   * från `/libs/livecycle/core/components/login`
   * till `/apps/livecycle/core/components/login`

1. Ändra följande kodfragment för `/apps/livecycle/core/components/login/login.jsp`:

***Språkkoden är***

```jsp
String browserLocale = "en";

    for(int i=0; i<locales.length; i++)
    {
        String prioperty = locales[i];
        if(prioperty.trim().startsWith("en")) {
            browserLocale = "en";
            break;
        }
        if(prioperty.trim().startsWith("de")){
            browserLocale = "de";
            break;
        }
        if(prioperty.trim().startsWith("ja")){
            browserLocale = "ja";
            break;
        }
        if(prioperty.trim().startsWith("fr")){
            browserLocale = "fr";
            break;
        }
    }
```

Till

```jsp
String browserLocale = "en";
    for(int i=0; i<locales.length; i++)
    {
        String prioperty = locales[i];
        if(prioperty.trim().startsWith("ar")) {
            browserLocale = "ar";
            break;
        }
        if(prioperty.trim().startsWith("en")) {
            browserLocale = "en";
            break;
        }
        if(prioperty.trim().startsWith("de")){
            browserLocale = "de";
            break;
        }
        if(prioperty.trim().startsWith("ja")){
            browserLocale = "ja";
            break;
        }
        if(prioperty.trim().startsWith("fr")){
            browserLocale = "fr";
            break;
        }
    }
```

```jsp
String browserLocale = "en";

    for(int i=0; i<locales.length; i++)
    {
        String prioperty = locales[i];
        if(prioperty.trim().startsWith("en")) {
            browserLocale = "en";
            break;
        }
        if(prioperty.trim().startsWith("de")){
            browserLocale = "de";
            break;
        }
        if(prioperty.trim().startsWith("ja")){
            browserLocale = "ja";
            break;
        }
        if(prioperty.trim().startsWith("fr")){
            browserLocale = "fr";
            break;
        }
    }
```

Till

```jsp
String browserLocale = "en";
    for(int i=0; i<locales.length; i++)
    {
        String prioperty = locales[i];
        if(prioperty.trim().equalsIgnoreCase("ar-DZ")) {
            browserLocale = "ar-DZ";
            break;
        }
        if(prioperty.trim().startsWith("en")) {
            browserLocale = "en";
            break;
        }
        if(prioperty.trim().startsWith("de")){
            browserLocale = "de";
            break;
        }
        if(prioperty.trim().startsWith("ja")){
            browserLocale = "ja";
            break;
        }
        if(prioperty.trim().startsWith("fr")){
            browserLocale = "fr";
            break;
        }
    }
```

***Om du vill ändra standardspråk***

```jsp
   String browserLocale = "en";
   for(int i=0; i<locales.length; i++)

   To

   String browserLocale = "ar";
   for(int i=0; i<locales.length; i++)
```

### Lägga till ny text eller ändra befintlig text {#adding-new-text-or-modifying-existing-text}

1. Kopiera mappen `i18n`:

   * från `/libs/livecycle/core/components/login`
   * till `/apps/livecycle/core/components/login`

1. Ändra nu värdet för egenskapen `sling:message` för noden (under den önskade språkkodsmappen) som du vill ändra texten för. Översättningen görs via nyckeln som anges i värdet för egenskapen `sling:key` för noden.

1. Utför följande åtgärder om du vill lägga till ett nytt nyckelvärdepar. Markera ett exempel i skärmbilden som följer.

   1. Skapa en nod av typen `sling:MessageEntry`, eller kopiera en befintlig nod och byt namn på den, under alla språkmappar.
   1. Kopiera `login.jsp`:

      * från `/libs/livecycle/core/components/login`

      * till `/apps/livecycle/core/components/login`

   1. Ändra `/apps/livecycle/core/components/login/login.jsp` om du vill inkludera den nya tillagda texten.

   ![Lägg till nytt nyckelvärdepar](assets/capture_new.png)

   ```jsp
   div class="loginContent">
   
                       <span class="loginFlow"></code>
                       <span class="loginVersion"><%= i18n.get("Version: 11.0.0") %></code>
                       <span class="loginTitle"><%= i18n.get("Login") %></code>
                       <% if (loginFailed) {%>
   ```

   Till

   ```jsp
   div class="loginContent">
   
                       <span class="loginFlow"></code>
                       <span class="loginVersion"><%= i18n.get("My Welcome Message") %></code>
                       <span class="loginVersion"><%= i18n.get("Version: 11.0.0") %></code>
                       <span class="loginTitle"><%= i18n.get("Login") %></code>
                       <% if (loginFailed) {%>
   ```

### Lägga till nytt format eller ändra befintligt format {#adding-new-style-or-modifying-existing-style}

1. Kopiera noden `login`:

   * från `/libs/livecycle/core/content`
   * till `/apps/livecycle/core/content`

1. Ta bort filer `login.js` och `jquery-1.8.0.min.js` från noden `/apps/livecycle/core/content/login.`
1. Ändra formaten i CSS-filen.
1. Så här lägger du till nya format:

   1. Lägg till nya format i `/apps/livecycle/core/content/login/login.css`
   1. Kopiera `login.jsp`

      * från `/libs/livecycle/core/components/login`

      * till `/apps/livecycle/core/components/login`

   1. Ändra `/apps/livecycle/core/components/login/login.jsp` om du vill inkludera de nya formaten.


Till exempel:

* Lägg till följande i `/apps/livecycle/core/content/login/login.css`.

```
css.newLoginContentArea {
    width: 700px;
    padding: 100px 0px 0px 100px;
   }
```

* Ändra följande i `/apps/livecycle/core/components/login.jsp`.


  ```jsp
  <div class="loginContentArea">
  ```

  Till

  ```jsp
  <div class="newLoginContentArea">
  ```

>[!NOTE]
>
>Om de befintliga bilderna i `/apps/livecycle/core/content/login` (kopierade från `/libs/livecycle/core/content/login`) tas bort tar du bort motsvarande referenser i CSS.

### Lägg till nya bilder {#add-new-images}

1. Följ stegen i Lägga till nytt format eller ändra befintligt format (dokumenteras ovan).
1. Lägg till nya bilder i `/apps/livecycle/core/content/login`. Så här lägger du till bild:

   1. Installera WebDAV-klienten.
   1. Navigera till mappen `/apps/livecycle/core/content/login` med webDAV-klienten. Mer information finns i [WebDAV-åtkomst](https://experienceleague.adobe.com/docs/experience-manager-65-lts/administering/contentmanagement/webdav-access.html?lang=en).

   1. Lägg till nya bilder.

1. Lägg till nya format i `/apps/livecycle/core/content/login/login.css,` som motsvarar nya bilder som lagts till i `/apps/livecycle/core/content/login`.
1. Använd de nya formaten i `login.jsp` vid `/apps/livecycle/core/components`.

Exempel:


```css
.newLoginContainerBkg {

 background-image: url(my_Bg.gif);
 background-repeat: no-repeat;
 background-position: left top;
 width: 727px;
}
```


    * Ändra följande i /apps/livecycle/core/components/login.jsp.

```jsp
<div class="loginContainerBkg">
```

Till

```jsp
<div class="newLginContainerBkg">
```
