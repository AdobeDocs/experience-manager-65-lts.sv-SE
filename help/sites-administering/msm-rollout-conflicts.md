---
title: MSM-utrullningskonflikter
description: Lär dig hur du hanterar driftsättningskonflikter i Multi Site Manager.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: site-features
content-type: reference
feature: Multi Site Manager
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: 3c207bfd-5d40-4355-8710-a620f0d66399
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '905'
ht-degree: 0%

---

# MSM-utrullningskonflikter{#msm-rollout-conflicts}

Konflikter kan uppstå om nya sidor med samma sidnamn skapas både i den blå grenen och i en beroende livekopiegren.

Sådana konflikter måste hanteras och lösas vid utrullning.

## Konflikthantering {#conflict-handling}

När det finns sidor som är i konflikt (i grenarna utkast och live copy) kan du definiera hur (eller till och med om) de ska hanteras.

För att säkerställa att utrullningen inte blockeras kan möjliga definitioner omfatta:

* vilken sida (utkast eller live-kopia) som har prioritet under utrullningen,
* vilka sidor som har bytt namn (och hur),
* hur detta påverkar publicerat innehåll.

  Standardbeteendet för Adobe Experience Manager (AEM) (körklar) är att publicerat innehåll inte påverkas. Om en sida som skapades manuellt i en livekopiegren har publicerats, kommer innehållet fortfarande att publiceras efter konflikthanteringen och utrullningen.

Förutom standardfunktionerna kan anpassade konflikthanterare läggas till för att implementera olika regler. Detta kan även möjliggöra publiceringsåtgärder som en enskild process.

### Exempelscenario {#example-scenario}

I följande avsnitt måste du använda exemplet på en ny sida, `b`, som har skapats både i grenen för utkast och live-kopia (som har skapats manuellt), för att illustrera olika metoder för konfliktlösning:

* utkast: `/b`

  En mallsida, med en underordnad sida, bp-level-1.

* live-kopia: `/b`

  En sida som skapats manuellt i livekopieringsgrenen, med en underordnad sida, `lc-level-1`.

   * Aktiverad vid publicering som `/b`, tillsammans med den underordnade sidan.

**Före utrullning**

<table>
 <tbody>
  <tr>
   <td><strong>skiss före utrullning</strong></td>
   <td><strong>live copy före utrullning</strong></td>
   <td><strong>publicera före lansering</strong></td>
  </tr>
  <tr>
   <td><code>b</code><br /> <br /> (skapat i en skissgren, klar för utrullning)<br /> </td>
   <td><code>b</code><br /> <br /> (skapat manuellt i en förgrening för live-kopia)<br /> </td>
   <td><code>b</code><br /> <br /> (innehåller innehållet på sidan b som skapades manuellt i den aktiva kopiegrenen)</td>
  </tr>
  <tr>
   <td><code> /bp-level-1</code></td>
   <td><code> /lc-level-1</code><br /> <br /> (skapat manuellt i en förgrening för live-kopia)<br /> </td>
   <td><code> /lc-level-1</code><br /> <br /> (innehåller innehållet på sidan <br /> underordnad nivå-1 som skapades manuellt i den aktiva kopiegrenen)</td>
  </tr>
 </tbody>
</table>

## Utrullningshanteraren och konflikthantering {#rollout-manager-and-conflict-handling}

Med utrullningshanteraren kan du aktivera eller inaktivera konflikthantering.

Detta görs med [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md) av **Day CQ WCM Rollout Manager**:

* **Hantera konflikter med manuellt skapade sidor**:

  ( `rolloutmgr.conflicthandling.enabled`)

  Ange som true om rullningshanteraren ska hantera konflikter från en sida som skapats i live-kopian med ett namn som finns i ritningen.

AEM har [fördefinierat beteende när konflikthantering har inaktiverats](#behavior-when-conflict-handling-deactivated).

## Konflikthanterare {#conflict-handlers}

AEM använder konflikthanterare för att lösa eventuella sidkonflikter som uppstår när man distribuerar innehåll från en ritning till en live-kopia. Att byta namn på sidor är en (vanlig) metod för att lösa sådana konflikter. Mer än en konflikthanterare kan vara användbar för att tillåta ett urval av olika beteenden.

AEM tillhandahåller:

* [standardkonflikthanteraren](#default-conflict-handler):

   * `ResourceNameRolloutConflictHandler`

* Möjligheten att implementera en [anpassad hanterare](#customized-handlers).
* Den rangordningsmekanism som gör att du kan ange prioriteten för varje enskild hanterare. Tjänsten med högst rankning används.

### Standardhanterare för konflikter {#default-conflict-handler}

Standardkonflikthanteraren:

* Anropas till `ResourceNameRolloutConflictHandler`

* Med den här hanteraren får plantryckssidan företräde.
* Tjänstrankningen för den här hanteraren är låg (d.v.s. under standardvärdet för egenskapen `service.ranking`) eftersom antagandet är att anpassade hanterare behöver en högre rankning. Rankningen är dock inte den absolut minsta nivån för att garantera flexibilitet vid behov.

Den här konflikthanteraren ger prioritet åt ritningen. Den aktiva kopieringssidan `/b` har flyttats (inom den aktiva kopiegrenen) till `/b_msm_moved`.

* live-kopia: `/b`

  Har flyttats (inom den aktiva kopian) till `/b_msm_moved`. Detta fungerar som en säkerhetskopia och säkerställer att inget innehåll går förlorat.

   * `lc-level-1` flyttas inte.

* utkast: `/b`

  Har introducerats på live-kopieringssidan `/b`.

   * `bp-level-1` har rullats ut till live-kopian.

**Efter utrullning**

<table>
 <tbody>
  <tr>
   <td><strong>skiss efter utrullning</strong></td>
   <td><strong>live copy efter utrullning</strong><br /> </td>
   <td></td>
   <td><strong>live copy efter utrullning</strong><br /> <br /> <br /> </td>
   <td><strong>publicera efter utrullning</strong><br /> <br /> </td>
  </tr>
  <tr>
   <td><code>b</code></td>
   <td><code>b</code><br /> <br /> (har innehållet på den plana sidan b som lanserades)<br /> </td>
   <td></td>
   <td><code>b_msm_moved</code><br /> <br /> (har innehållet på sidan b som skapades manuellt i den aktiva kopiegrenen)</td>
   <td><code>b</code><br /> <br /> (ingen ändring; innehåller innehållet på originalsidan b som skapades manuellt i den aktiva kopiegrenen och nu kallas b_msm_move)<br /> </td>
  </tr>
  <tr>
   <td><code> /bp-level-1</code></td>
   <td><code class="code"> /bp-level-1</code></td>
   <td><code> /lc-level-1</code><br /> <br /> (ingen ändring)</td>
   <td><code> </code></td>
   <td><code> /lc-level-1</code><br /> <br /> (ingen ändring)</td>
  </tr>
 </tbody>
</table>

### Anpassade hanterare {#customized-handlers}

Med anpassade konflikthanterare kan du implementera egna regler. Med servicerangordningsmekanismen kan du även definiera hur de interagerar med andra hanterare.

Anpassade konflikthanterare kan ha följande:

* Namngivna enligt dina önskemål.
* Utvecklas/konfigureras enligt dina krav. Du kan t.ex. utveckla en hanterare så att den aktiva kopieringssidan ges företräde.
* Utformad för att konfigureras med [OSGi-konfigurationen](/help/sites-deploying/configuring-osgi.md), särskilt:

   * **Servicerankning**:

     Definierar ordningen som är relaterad till andra konflikthanterare ( `service.ranking`).

     Standardvärdet är 0.

### Beteende vid inaktiverad konflikthantering {#behavior-when-conflict-handling-deactivated}

Om du [inaktiverar konflikthantering](#rollout-manager-and-conflict-handling) manuellt utför AEM ingen åtgärd på sidor som är i konflikt (sidor som inte är i konflikt rullas ut som förväntat).

>[!CAUTION]
>
>AEM ger inga indikationer på att konflikter ignoreras eftersom detta beteende måste konfigureras explicit, så det antas att det är det obligatoriska beteendet.

I det här fallet har live-kopian företräde. Den blå sidan `/b` kopieras inte och den aktiva kopieringssidan `/b` lämnas orörd.

* utkast: `/b`

  Inte kopierat alls, men ignoreras.

* live-kopia: `/b`

  Samma.

<table>
 <caption>
   Efter utrullning
 </caption>
 <tbody>
  <tr>
   <td><strong>skiss efter utrullning</strong></td>
   <td><strong>live copy efter utrullning</strong><br /> <br /> <br /> </td>
   <td><strong>publicera efter utrullning</strong><br /> <br /> </td>
  </tr>
  <tr>
   <td><code>b</code></td>
   <td><code>b</code><br /> <br /> (ingen ändring; har innehållet på sidan b som skapades manuellt i den aktiva kopiegrenen)</td>
   <td><code>b</code><br /> <br /> (ingen ändring; innehåller innehållet på sidan b som skapades manuellt i den aktiva kopiegrenen)<br /> </td>
  </tr>
  <tr>
   <td><code> /bp-level-1</code><br /> </td>
   <td><code> /lc-level-1</code><br /> <br /> (ingen ändring)</td>
   <td><code> /lc-level-1</code><br /> <br /> (ingen ändring)</td>
  </tr>
 </tbody>
</table>

### Servicerangordning {#service-rankings}

Tjänstrankningen [OSGi](https://www.osgi.org/) kan användas för att definiera prioriteten för enskilda konflikthanterare.
