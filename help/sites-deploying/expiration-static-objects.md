---
title: Förfallotid för statiska objekt
description: Lär dig hur du konfigurerar Adobe Experience Manager så att statiska objekt inte upphör att gälla (under en rimlig tidsperiod).
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: configuring
content-type: reference
feature: Configuring
solution: Experience Manager, Experience Manager Sites
role: Admin
hide: true
hidefromtoc: true
exl-id: a279ebed-a116-4719-989e-1b2f79f6caa4
source-git-commit: f145e5f0d70662aa2cbe6c8c09795ba112e896ea
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---

# Förfallotid för statiska objekt{#expiration-of-static-objects}

Statiska objekt (till exempel ikoner) ändras inte. Därför bör systemet konfigureras så att det inte upphör att gälla (under en rimlig tidsperiod) och på så sätt minskar onödig trafik.

Detta har följande effekt:

* Avlastar begäranden från serverinfrastrukturen.
* Förbättrar sidinläsningens prestanda när webbläsaren cachelagrar objekt i webbläsarens cache.

Förfallotider anges av HTTP-standarden med avseende på &quot;förfallodatum&quot; för filer (se t.ex. kapitel 14.21 i [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt) &quot; Hypertext Transfer Protocol - HTTP 1.1&quot;). I den här standarden används rubriken för att tillåta klienter att cachelagra objekt tills de betraktas som inaktuella. Sådana objekt cachelagras under den angivna tiden utan att någon statuskontroll görs på den ursprungliga servern.

>[!NOTE]
>
>Den här konfigurationen är separat från (och fungerar inte för) Dispatcher.
>
>Syftet med Dispatcher är att cacha data framför Adobe Experience Manager (AEM).

Alla filer, som inte är dynamiska och som inte ändras över tid, kan och bör cachas. Konfigurationen för Apache HTTPD-servern kan se ut som något av följande, beroende på miljön:

>[!CAUTION]
>
>Var försiktig när du definierar den tidsperiod under vilken ett objekt anses vara uppdaterat. Eftersom det finns *ingen kontroll förrän den angivna tidsperioden har gått ut* kan klienten visa gammalt innehåll från cachen.

1. **För en författarinstans:**

   ```xml
   LoadModule expires_module modules/mod_expires.so
   <Location /libs>
     ExpiresByType text/css "access plus 1 month"
     ExpiresByType text/javascript "access plus 1 month"
     ExpiresByType image/png "access plus 1 month"
     ExpiresByType image/gif "access plus 1 month"
   </Location>
   ```

   Detta gör att det mellanliggande cacheminnet (till exempel webbläsarens cacheminne) kan lagra CSS-, JavaScript-, PNG- och GIF-filer i upp till en månad tills de upphör att gälla. Det innebär att de inte behöver begäras från AEM eller webbservern, men de kan finnas kvar i webbläsarens cacheminne.

   Andra delar av webbplatsen bör inte cachas i en författarinstans eftersom de kan ändras när som helst.

1. **För en publiceringsinstans:**

   ```xml
   LoadModule expires_module modules/mod_expires.so
   <Location /content>
     ExpiresByType text/css "access plus 1 day"
     ExpiresByType text/javascript "access plus 1 day"
     ExpiresByType image/png "access plus 1 day"
     ExpiresByType image/gif "access plus 1 day"
   </Location>
   <Location /etc/designs>
     ExpiresByType text/css "access plus 1 day"
     ExpiresByType text/javascript "access plus 1 day"
     ExpiresByType image/png "access plus 1 day"
     ExpiresByType image/gif "access plus 1 day"
   </Location>
   ```

   Detta gör att det mellanliggande cacheminnet (till exempel webbläsarens cacheminne) kan lagra CSS-, JavaScript-, PNG- och GIF-filer för upp till en dag i klientcachen. Det här exemplet visar globala inställningar för allt under `/content` och `/etc/designs`, men du bör göra det mer detaljerat.

   Beroende på hur ofta webbplatsen uppdateras kan du även cachelagra HTML-sidor. En rimlig tidsperiod skulle vara en timme:

   ```xml
   <Location /content>
     ExpiresByType text/html "access plus 1 hour"
   </Location>
   ```

När du har konfigurerat de statiska objekten kan du kontrollera `request.log`, medan du väljer sidor som innehåller sådana objekt, att inga (onödiga) begäranden görs för statiska objekt.
