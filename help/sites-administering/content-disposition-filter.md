---
title: Innehållsdispositionsfilter
description: Lär dig hur du använder filtret för innehållsdisposition för att förhindra XSS-attacker.
contentOwner: trushton
products: SG_EXPERIENCEMANAGER/6.5/SITES
content-type: reference
topic-tags: Security
solution: Experience Manager, Experience Manager Sites
feature: Security
role: Admin
exl-id: 997cb6f3-1ef8-409c-acea-157d5b27a6b2
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Innehållsdispositionsfilter {#content-disposition-filter}

Dispositionsfiltret är en säkerhetsfunktion mot XSS-attacker på SVG-filer.

När filtret har installerats blockeras åtkomsten till alla resurser. Du kan till exempel inte visa en PDF online. I det här avsnittet beskrivs hur du konfigurerar filtret efter dina behov.

## Konfigurera filtret för innehållsdisposition {#configure-content-disposition-filter}

Du kan visa [Dispositionsfiltret för Apache Sling-innehåll i GitHub](https://github.com/apache/sling-org-apache-sling-security/blob/master/src/main/java/org/apache/sling/security/impl/ContentDispositionFilterConfiguration.java).

Alternativen för Innehållsdispositionsfilter innehåller följande funktioner:

* **Sökvägar för innehållsdisposition:** En lista med sökvägar där filtret används följt av en lista med MIME-typer som ska uteslutas på den sökvägen. Sökvägen måste vara en absolut sökväg och kan innehålla ett jokertecken (`*`) i slutet, för att matcha alla resurssökvägar med det angivna sökvägsprefixet. `/content/*:image/jpeg,image/svg+xml` använder till exempel filtret på alla noder i `/content?` förutom JPG- och SVG-bilder.

* **Uteslutna resurssökvägar:** En lista över exkluderade resurser. Varje resurssökväg måste anges som en absolut och fullständig sökväg. Prefixmatchning/jokertecken stöds inte.

* **Aktivera för alla resurssökvägar:** Den här flaggan kontrollerar om det här filtret ska aktiveras för alla sökvägar, utom för de uteslutna sökvägarna som definieras av Undantagna resurssökvägar. Om du anger flaggan som true ignoreras innehållets dispositionssökvägar. Oberoende av konfigurationen täcks bara resurssökvägar som innehåller egenskapen `jcr:data` eller `jcr:content/jcr:data`.
