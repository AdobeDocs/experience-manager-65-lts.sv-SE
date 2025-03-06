---
product: adobe experience manager
description: Adobe Experience Manager 6.5 LTS-dokumentation.
git-repo: https://github.com/AdobeDocs/experience-manager-65-lts.sv-SE
index: y
type: Documentation
solution: Experience Manager
feature-set: Experience Manager Assets,Experience Manager Sites,Experience Manager, Experience Manager Forms, Experience Manager Cloud Manager
cloud: Experience Cloud
recommendations: noDisplay
source-git-commit: 766b9eaad3ebe43db3659e8ba4afb8b4496a1f3a
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 0%

---


# Metadata för intern användning

Metadata i GitHub-redigeringssystemet är hierarkiska och definieras enligt följande högre prioritetsnivåer.

1. metadata.md
1. TillC
1. Artikel

Metadata som definieras i filen metadata.md gäller för hela repon, men kan åsidosättas på ToC- och artikelnivå. Alla åsidosättningar av metadata bör göras på lägsta möjliga nivå.

Metadata i experience-manager-cloud-service.en repo är det minsta nödvändiga.

metadata.md

* `product`
* `git-repo`
* `index`
* `solution-title`
* `solution-hub-url`
* `getting-started-title`
* `getting-started-url`
* `tutorials-title`
* `tutorials-url`

ToCS

* `sub-product`
* `user-guide-title`

Artikel

* `title`
* `description`
* `contentOwner` (endast för kärnresursinnehåll under `/help/assets`)
