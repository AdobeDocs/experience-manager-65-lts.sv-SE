---
title: Innehållsfragment Konfigurera komponenter för återgivning
description: Innehållsfragment Konfigurera komponenter för återgivning
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: extending-aem
content-type: reference
docset: aem65
solution: Experience Manager, Experience Manager Sites
feature: Content Fragments
role: Developer
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 1%

---

# Innehållsfragment Konfigurera komponenter för återgivning{#content-fragments-configuring-components-for-rendering}

Det finns flera [avancerade tjänster](/help/sites-developing/content-fragments-config-components-rendering.md#definition-of-advanced-services-that-need-configuration) som är relaterade till återgivningen av innehållsfragment. För att kunna använda dessa tjänster måste resurstyperna för sådana komponenter göra sig kända för innehållsfragmentets ramverk.

Detta görs genom att konfigurera [OSGi-tjänsten - komponentkonfigurationen för innehållsfragment](#osgi-service-content-fragment-component-configuration).

>[!CAUTION]
>
>Om du inte behöver de [avancerade tjänster](/help/sites-developing/content-fragments-config-components-rendering.md#definition-of-advanced-services-that-need-configuration) som beskrivs nedan kan du ignorera den här konfigurationen.

>[!CAUTION]
>
>När du utökar eller använder en eller flera komponenter som är klara att användas bör du inte ändra konfigurationen.

>[!CAUTION]
>
>Du kan skriva en helt ny komponent som endast använder API:t för innehållsfragment, utan några avancerade tjänster. I så fall måste du dock utveckla komponenten så att den hanterar lämplig bearbetning.
>
>Därför rekommenderas att kärnkomponenterna används.

## Definition av avancerade tjänster som behöver konfigureras {#definition-of-advanced-services-that-need-configuration}

De tjänster som kräver registrering av en komponent är:

* Kontrollera beroenden korrekt under publiceringen (d.v.s. se till att fragment och modeller kan publiceras automatiskt med en sida om de har ändrats sedan den senaste publiceringen).
* Stöd för innehållsfragment vid fulltextsökning.
* Hantering/hantering av *mellanliggande innehåll.*
* Hantering/hantering av *blandade medieresurser.*
* Dispatcher rensar för refererade fragment (om en sida som innehåller ett fragment publiceras om).
* Använda styckebaserad återgivning.

Om du behöver en eller flera av de här funktionerna är det (oftast) enklare att använda de färdiga funktionerna i stället för att utveckla dem från början.

## OSGi-tjänst - Konfiguration av komponent för innehållsfragment {#osgi-service-content-fragment-component-configuration}

Konfigurationen måste bindas till OSGi-tjänsten **Komponentkonfiguration för innehållsfragment**:

`com.adobe.cq.dam.cfm.impl.component.ComponentConfigImpl`

>[!NOTE]
>
>Mer information finns i [Konfigurera OSGi](/help/sites-deploying/configuring-osgi.md).

Till exempel:

![cfm-01](assets/cfm-01.png)

OSGi-konfigurationen är:

<table>
 <tbody>
  <tr>
   <td>Etikett</td>
   <td>OSGi-konfiguration<br /> </td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td><strong>Resurstyp</strong></td>
   <td><code>dam.cfm.component.resourceType</code></td>
   <td>Resurstypen som ska registreras, till exempel <br /> <p><span class="cmp-examples-demo__property-value"><code>core/wcm/components/contentfragment/v1/contentfragment</code></code></p> </td>
  </tr>
  <tr>
   <td><strong>Referensegenskap</strong></td>
   <td><code>dam.cfm.component.fileReferenceProp</code></td>
   <td>Namnet på egenskapen som innehåller referensen till fragmentet, till exempel <code>fragmentPath</code> eller <code>fileReference</code></td>
  </tr>
  <tr>
   <td><strong>Elementegenskap(er)</strong></td>
   <td><code>dam.cfm.component.elementsProp</code></td>
   <td>Namnet på den egenskap som innehåller namnen på elementen som ska återges, till exempel<code>elementName</code></td>
  </tr>
  <tr>
   <td><strong>Variationsegenskap</strong><br /> </td>
   <td><code>dam.cfm.component.variationProp</code></td>
   <td>Namnet på den egenskap som innehåller namnet på variationen som ska återges, till exempel<code>variationName</code></td>
  </tr>
 </tbody>
</table>

För vissa funktioner (till exempel för att endast återge ett styckeintervall) måste du följa vissa konventioner:

<table>
 <tbody>
  <tr>
   <td>Egenskapsnamn</td>
   <td>Beskrivning</td>
  </tr>
  <tr>
   <td><code>paragraphRange</code></td>
   <td><p>En strängegenskap som definierar det intervall med stycken som ska skrivas ut i <em>renderingsläget för ett element</em>.</p> <p>Format:</p>
    <ul>
     <li><code>1</code> eller <code>1-3</code> eller <code>1-3;6;7-8</code> eller <code>*-3;5-*</code></li>
     <li>endast utvärderat om <code>paragraphScope</code> är inställt på <code>range</code></li>
    </ul> </td>
  </tr>
  <tr>
   <td><code>paragraphScope</code></td>
   <td><p>En strängegenskap som definierar hur stycken ska skrivas ut i <em>renderingsläget för ett element</em>.</p> <p>Värden:</p>
    <ul>
     <li><code>all</code> : för att återge alla stycken</li>
     <li><code>range</code> : för att återge styckeintervallet från <code>paragraphRange</code></li>
    </ul> </td>
  </tr>
  <tr>
   <td><code>paragraphHeadings</code></td>
   <td>En boolesk egenskap som definierar om rubriker (till exempel <code>h1</code>, <code>h2</code>, <code>h3</code>) räknas som stycken (<code>true</code>) eller inte (<code>false</code>)</td>
  </tr>
 </tbody>
</table>

>[!CAUTION]
>
>Detta kan förändras i senare 6,5 milstolpar.

## Exempel {#example}

Se följande (i en körklar AEM-instans) som exempel:

```
/apps/core/wcm/config/com.adobe.cq.dam.cfm.impl.component.ComponentConfigImpl-core-comp-v1.config
```

Detta innehåller:

```
dam.cfm.component.resourceType="core/wcm/components/contentfragment/v1/contentfragment"
dam.cfm.component.fileReferenceProp="fragmentPath"
dam.cfm.component.elementsProp="elementName"
dam.cfm.component.variationProp="variationName"
```
