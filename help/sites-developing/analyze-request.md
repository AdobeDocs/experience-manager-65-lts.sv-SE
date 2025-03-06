---
title: Begär analysskript
description: Analysskriptet för begäran görs för att underlätta analysen av access.log-filerna som skapar en läsbar rapport för senare bearbetning
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: testing
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 9fe575ad-1e8d-460f-a933-ddc2e927a6e8
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Begär analysskript{#request-analysis-script}

## Ladda ned {#download}

Det här skriptet görs för att underlätta analysen av de `access.log`-filer som skapar en läsbar rapport för senare bearbetning.

[Hämta fil](assets/analyse-access.sh)

## Beskrivning {#description}

Det här skriptet görs för att underlätta analysen av de `access.log`-filer som skapar en läsbar rapport för senare bearbetning.

Det genererar det totala antalet förfrågningar, GET kontra POST, Begär distribution över tid med mera.

Utdata är i Markdown-syntax och det blir därför enklare att konvertera dem till PDF-filer med verktyg som pandoc eller att visa dem i en webbläsare med plugin-program som Markdown Viewer.

Den kan analysera en anpassad sökväg som finns på kommandoraden.

Utgå från kommentaren i filen som talar om hur den ska köras:

Analysera CQ `access.log` genom att extrapolera olika uppgifter och skapa en Markdown-utdata på `stdout`.

## Användning {#usage}

`./analyse-access.sh access.log.2013-&ast;`

du kan ange ytterligare anpassade sökvägar att analysera på kommandoraden

`/analyse-access.sh access.log.2013-&ast; /my/custom/path/1 /my/custom/path/2`

du kan spara utdata med en enkel rörledning

`./analyse-access.sh access.log.2013-&ast; | tee yr2013.md`
