---
title: Använd smart bildbehandling med enhetspixelproportioner på klientsidan
description: Lär dig hur du använder pixelproportioner för enheter på klientsidan med Smart Imaging i Adobe Experience Manager as a Cloud Service med Dynamic Media.
role: Admin,User
solution: Experience Manager, Experience Manager Assets
feature: Smart Imaging
exl-id: 3b4f3624-d76d-4835-834b-e8610c2c40bd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Om Smart Imaging med enhetspixelproportioner på klientsidan (DPR) {#client-side-dpr}

Den aktuella lösningen för Smart Imaging använder användaragentsträngar för att avgöra vilken typ av enhet (dator, surfplatta, mobil och så vidare) som används.

Funktioner för enhetsidentifiering - DPR baserad på användaragentsträngar - är ofta felaktiga, särskilt för Apple-enheter. Dessutom måste den valideras varje gång en ny enhet startas.

DPR på klientsidan ger helt korrekta värden och fungerar för alla enheter, oavsett om det är Apple eller någon annan ny enhet som startades.

## Använd DPR-koden på klientsidan

**Återgivna appar på serversidan**

1. Läs in servicearbetarens init (`srvinit.js`) genom att inkludera följande skript i sidhuvudsavsnittet på din HTML-sida:

   ```javascript
   <script type="text/javascript" src="srvinit.js"></script>
   ```

   Adobe rekommenderar att du läser in det här skriptet _före_ andra skript så att servicearbetaren påbörjar initieringen direkt.

1. Inkludera följande DPR-bildtagg överst i brödavsnittet på din HTML-sida:

   ```html
   <img src="aem_dm_dpr_1x.jpg" style="width:1px;height:1px;display:none"
       srcset="aem_dm_dpr_1x.jpg 1x, aem_dm_dpr_1.1x.jpg 1.1x, aem_dm_dpr_1.2x.jpg 1.2x, aem_dm_dpr_1.3x.jpg 1.3x, aem_dm_dpr_1.4x.jpg 1.4x, aem_dm_dpr_1.5x.jpg 1.5x, aem_dm_dpr_1.6x.jpg 1.6x,          aem_dm_dpr_1.7x.jpg 1.7x, aem_dm_dpr_1.8x.jpg 1.8x, aem_dm_dpr_1.9x.jpg 1.9x,
       aem_dm_dpr_2x.jpg 2x, aem_dm_dpr_2.1x.jpg 2.1x, aem_dm_dpr_2.2x.jpg 2.2x, aem_dm_dpr_2.3x.jpg 2.3x, aem_dm_dpr_2.4x.jpg 2.4x, aem_dm_dpr_2.5x.jpg 2.5x, aem_dm_dpr_2.6x.jpg 2.6x, aem_dm_dpr_2.7x.jpg 2.7x, aem_dm_dpr_2.8x.jpg 2.8x, aem_dm_dpr_2.9x.jpg 2.9x,
       aem_dm_dpr_3x.jpg 3x, aem_dm_dpr_3.1x.jpg 3.1x, aem_dm_dpr_3.2x.jpg 3.2x, aem_dm_dpr_3.3x.jpg 3.3x, aem_dm_dpr_3.4x.jpg 3.4x, aem_dm_dpr_3.5x.jpg 3.5x, aem_dm_dpr_3.6x.jpg 3.6x, aem_dm_dpr_3.7x.jpg 3.7x, aem_dm_dpr_3.8x.jpg 3.8x, aem_dm_dpr_3.9x.jpg 3.9x,
       aem_dm_dpr_4x.jpg 4x, aem_dm_dpr_4.1x.jpg 4.1x, aem_dm_dpr_4.2x.jpg 4.2x, aem_dm_dpr_4.3x.jpg 4.3x, aem_dm_dpr_4.4x.jpg 4.4x, aem_dm_dpr_4.5x.jpg 4.5x, aem_dm_dpr_4.6x.jpg 4.6x, aem_dm_dpr_4.7x.jpg 4.7x, aem_dm_dpr_4.8x.jpg 4.8x, aem_dm_dpr_4.9x.jpg 4.9x,
       aem_dm_dpr_5x.jpg 5x">
   ```

   Du måste inkludera den här DPR-bildtaggen _före_ för alla statiska bilder på din HTML-sida.

**Återgivna appar på klientsidan**

1. Inkludera följande DPR-skript i sidhuvudet på din HTML-sida:

   ```javascript
   <script type="text/javascript" src="srvinit.js"></script>
   <script type="text/javascript" src="dprImageInjection.js"></script>
   ```

   Du kan kombinera båda DPR-skripten till ett för att undvika flera nätverksbegäranden.

   Adobe rekommenderar att du läser in dessa _före_ eventuella andra skript på HTML-sidan.
Adobe rekommenderar också att du använder Bootstrap under HTML-taggen diff i stället för som ett body-element. Orsaken är att `dprImageInjection.js` dynamiskt infogar bildtaggen högst upp i brödavsnittet på HTML-sidan.

## Nedladdning av JavaScript-filer {#client-side-dpr-script}

Följande JavaScript-filer i nedladdningen är endast tillgängliga som exempel. Om du tänker använda de här filerna på HTML-sidor måste du redigera varje fils kod så att den passar dina egna behov.

* `dprImageInjection.js`
* `srvinit.js`
* `srvwrk.js`

[Nedladdning av JavaScript-filer](/help/assets/assets-dm/aem-dynamicmedia-smartimaging-dpr.zip)

>[!MORELIKETHIS]
>
>* [Smart bildbehandling](/help/assets/imaging-faq.md)
