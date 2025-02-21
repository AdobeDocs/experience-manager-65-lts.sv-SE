---
title: Extern URL
description: Externalizer är en tjänst av typen OSGI som gör att du kan omvandla en resurssökväg till en extern och absolut URL-adress med hjälp av programmet
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---

# Extern URL{#externalizing-urls}

I Adobe Experience Manager (AEM) är **Externalizer** en OSGI-tjänst som gör att du programmässigt kan omvandla en resurssökväg (till exempel `/path/to/my/page`) till en extern och absolut URL (till exempel `https://www.mycompany.com/path/to/my/page`) genom att prefixera sökvägen med en förkonfigurerad DNS.

Eftersom en instans inte känner till sin externt synliga URL-adress om den körs bakom ett webblager, och eftersom en länk ibland måste skapas utanför det begärda omfånget, utgör den här tjänsten en central plats för att konfigurera de externa URL-adresserna och skapa dem.

På den här sidan beskrivs hur du konfigurerar tjänsten **Externalizer** och hur du använder den. Mer information finns i [Javadocs](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/Externalizer.html).

## Konfigurera tjänsten Externalizer {#configuring-the-externalizer-service}

Med tjänsten **Externalizer** kan du centralt definiera flera domäner som kan användas för att programmässigt prefix för resurssökvägar. Varje domän identifieras med ett unikt namn som används för att programmässigt referera till domänen.

Så här definierar du en domänmappning för tjänsten **Externalizer**:

1. Navigera till konfigurationshanteraren via **Verktyg** och sedan **Webbkonsol** eller ange:

   `https://<host>:<port>/system/console/configMgr`

1. Klicka på **Day CQ Link Externalizer** för att öppna konfigurationsdialogrutan.

   >[!NOTE]
   >
   >Den direkta länken till konfigurationen är `https://<host>:<port>/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl`

   ![aem-externalizer-01](assets/aem-externalizer-01.png)

1. Definiera en **Domäner**-mappning: en mappning består av ett unikt namn som kan användas i koden för att referera till domänen, ett blanksteg och domänen:

   `<unique-name> [scheme://]server[:port][/contextpath]`

   Var:

   * **scheme** är http eller https, men kan också vara ftp och så vidare.

      * använd https för att framtvinga https-länkar, om det behövs
      * den används om klientkoden inte åsidosätter schemat när den frågar efter en URL-adress.

   * **server** är värdnamnet (kan vara ett domännamn eller en IP-adress).
   * **port** (valfritt) är portnumret.
   * **kontextsökväg** (valfritt) anges bara om AEM har installerats som en webbapp under en annan kontextsökväg.

   Till exempel: `production https://my.production.instance`

   Följande mappningsnamn är fördefinierade och måste anges eftersom AEM förlitar sig på dem:

   * `local` - den lokala instansen
   * `author` - redigeringssystemets DNS
   * `publish` - den offentliga webbplatsens DNS

   >[!NOTE]
   >
   >Med en anpassad konfiguration kan du lägga till en kategori, till exempel `production`, `staging` eller till och med externa icke-AEM-system som `my-internal-webservice`. Det är praktiskt att undvika hårdkodning av sådana URL:er på olika platser i ett projekts kodbas.

1. Klicka på **Spara** för att spara ändringarna.

>[!NOTE]
>
>Adobe rekommenderar att du [lägger till konfigurationen i databasen](/help/sites-deploying/configuring.md#addinganewconfigurationtotherepository).

### Använda tjänsten Externalizer {#using-the-externalizer-service}

I det här avsnittet visas några exempel på hur tjänsten **Externalizer** kan användas:

1. **Så här hämtar du tjänsten Externalizer i en JSP:**

   ```java
   Externalizer externalizer = resourceResolver.adaptTo(Externalizer.class);
   ```

1. **Så här gör du en extern sökväg med domänen &#39;publish&#39;:**

   ```java
   String myExternalizedUrl = externalizer.publishLink(resolver, "/my/page") + ".html";
   ```

   Anta domänmappningen:

   * `publish https://www.website.com`

   `myExternalizedUrl` slutar med värdet:

   * `https://www.website.com/contextpath/my/page.html`

1. **Så här gör du en extern sökväg med domänen &#39;författare&#39;:**

   ```java
   String myExternalizedUrl = externalizer.authorLink(resolver, "/my/page") + ".html";
   ```

   Anta domänmappningen:

   * `author https://author.website.com`

   `myExternalizedUrl` slutar med värdet:

   * `https://author.website.com/contextpath/my/page.html`

1. **Så här gör du en extern sökväg med domänen local:**

   ```java
   String myExternalizedUrl = externalizer.externalLink(resolver, Externalizer.LOCAL, "/my/page") + ".html";
   ```

   Anta domänmappningen:

   * `local https://publish-3.internal`

   `myExternalizedUrl` slutar med värdet:

   * `https://publish-3.internal/contextpath/my/page.html`

1. Fler exempel finns i [Javadocs](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/Externalizer.html).
