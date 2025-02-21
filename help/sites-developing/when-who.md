---
title: Testning - när och med vem?
description: Olika roller kan vara involverade i testningen och i olika faser av projektutvecklingen.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: testing
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Testning - när och med vem?{#testing-when-and-with-whom}

Olika roller kan vara involverade i testningen och i olika faser av projektutvecklingen.

<table>
 <tbody>
  <tr>
   <td>Testteam</td>
   <td>Ansvarig för... </td>
   <td>När...</td>
  </tr>
  <tr>
   <td>Utvecklingsgruppen</td>
   <td>Utvecklingsteamet ansvarar för dina enhetstester och vissa integreringstester.</td>
   <td>Dessa tester är först i kedjan, men de upprepas/byggs ut under utvecklingen.</td>
  </tr>
  <tr>
   <td>Kvalitetsteam i Assurance</td>
   <td><p>Du behöver ett kvalitetsteam från Assurance (oavsett storlek) för funktions- och prestandatester.</p> <p>Det här är neutrala, dedikerade testare - en gyllene regel för programvara säger alltid att en utvecklare aldrig ska testa sitt eget arbete.</p> <p>Medlemmarna i det här teamet kan hämtas från Day-projektteamet, partnern och/eller ditt kundteam.</p> </td>
   <td><p>Den första funktionsreleasen ska göras tillgänglig för testarna (när det är möjligt). Även om en tidig interimsversion kan generera många fel kan den ge tidig feedback i viktiga frågor.</p> </td>
  </tr>
  <tr>
   <td>Kundtestteam</td>
   <td><p>Beroende på vilken projektmodell som valts kan det finnas planer på att medlemmar i kundteamet ska delta i testningen, särskilt författare från kundwebbplatsen.</p> <p>Detta är fördelaktigt eftersom det</p>
    <ul>
     <li><p>Kunden får erfarenhet av det projekt som utvecklas.</p> </li>
     <li><p>Ger tidig feedback från kunden.</p> </li>
     <li><p>Användarna uttrycker ofta sina krav i termer av tidigare erfarenhet. Om kunderna deltar i testningen så tidigt som möjligt ökar deras upplevelse av det nya projektet i form av en <i>praktisk</i>-upplevelse.</p> </li>
    </ul> </td>
   <td><p>Återigen är tidiga engagemang bra, men alla releaser som kunderna använder bör vara stabila, med rimlig funktionalitet.</p> <p>Första intrycket är alltid viktigt.</p> </td>
  </tr>
 </tbody>
</table>
