---
title: Begränsningar för dynamiska media
description: Lär dig mer om de effektivaste strategierna och de tvingande gränserna när du skapar en bilduppsättning eller en snurruppsättning, eller överför en PDF. Läs också om webbläsarkombinationer och operativsystemkombinationer som inte stöds för Dynamic Media.
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/Dynamic-Media-Classic
geptopics: SG_SCENESEVENONDEMAND_PK/categories/ecatalogs
feature: Dynamic Media Classic,Asset Management,Image Sets,Spin Sets,eCatalog
role: User
solution: Experience Manager, Experience Manager Assets
exl-id: 5aa07827-8483-4a99-9197-a17485226a13
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 1%

---

# Dynamiska mediebegränsningar

I följande avsnitt beskrivs begränsningar i Dynamic Media.

Det här avsnittet innehåller följande avsnitt:

* [Bästa praxis och tvingande begränsningar av resurstyper från Dynamic Media](#best-practice-enforced-limits)
* [Webbläsare- och operativsystemkombinationer som inte stöds för Dynamic Media](#unsupported-browser-os)

## Bästa praxis och tvingande begränsningar av resurstyper från Dynamic Media {#best-practice-enforced-limits}

När du skapar en snurra uppsättning eller en bilduppsättning, eller överför PDF-filer för sidextrahering, rekommenderar Adobe följande metodtips och tillämpar följande begränsningar:

| Resurs - begränsningstyp | Bästa praxis | Begränsning har införts |
| --- | --- | --- |
| **Bild** - antal smarta beskärningar per bild | 5 | 100 |
| **Alla uppsättningar** - antal dubblettresurser per uppsättning | Inga dubbletter | 20‡ |
| **Alla uppsättningar** - Maximalt antal resurser per uppsättning | 5-10 bilder per uppsättning | 1000 |
| **Snurra uppsättning** - Maximalt antal rader/kolumner per 2D-uppsättning | 12-18 bilder per uppsättning | 1000 |
| **PDF** - Maximalt antal sidor för en PDF som ska användas för extrahering |  | 100 (för alla PDF-filer) |

‡ Bästa praxis är att inte ha duplicerade resurser i en uppsättning. Gränsen är 20 kopior för en enskild resurs. Om du lägger till ytterligare en dubblett för den resursen, inom den uppsättningen, returnerar begäran ett fel eller ignorerar dubbletten.
<!-- See also [Dynamic Media limitations](/help/assets/limitations.md). -->

## Webbläsare- och operativsystemkombinationer som inte stöds för Dynamic Media {#unsupported-browser-os}

Dynamic Media stöder inte följande kombinationer av webbläsare och operativsystem.

* Internet Explorer 11 + Windows 7
* Internet Explorer 11 + Windows 8.1
* Internet Explorer 11 + Windows Phone 8.1
* Internet Explorer 11 + Windows Phone 8.1 - uppdatering
* Safari 6 + iOS 6.0.1
* Safari 7 + iOS 7.1
* Safari 7 + OS X 10.9 Mavericks
* Safari 8 + iOS 8.4
* Safari 8 + OS X 10.10 Yosemite

## Stöd för Secure Socket Layer 2.0 och 3.0 samt Transport Layer Security 1.0 och 1.1 har upphört {#tls}

<!-- CQDOC-19433 (original ticket)
and CQDOC-19792 (removed as per this ticket December 5, 2022) -->

Från och med 30 april 2024 upphör stödet för Adobe Dynamic Media för följande:

* SSL (Secure Socket Layer) 2.0
* SSL 3.0
* TLS (Transport Layer Security) 1.0 och 1.1
* Följande svaga lärare i TLS 1.2:
   * `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
   * `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
   * `TLS_RSA_WITH_AES_256_GCM_SHA384`
   * `TLS_RSA_WITH_AES_256_CBC_SHA256`
   * `TLS_RSA_WITH_AES_256_CBC_SHA`
   * `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
   * `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
   * `TLS_RSA_WITH_AES_128_GCM_SHA256`
   * `TLS_RSA_WITH_AES_128_CBC_SHA256`
   * `TLS_RSA_WITH_AES_128_CBC_SHA`
   * `TLS_RSA_WITH_CAMELLIA_256_CBC_SHA`
   * `TLS_RSA_WITH_CAMELLIA_128_CBC_SHA`
   * `TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA`
   * `TLS_RSA_WITH_SDES_EDE_CBC_SHA`
