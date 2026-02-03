---
title: Använda delningsadaptrar
description: Sling erbjuder ett adaptermönster för att enkelt översätta objekt som implementerar gränssnittet Adaptable.
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
solution: Experience Manager, Experience Manager Sites
feature: Developing
role: Developer
exl-id: 7eae83bd-7982-4051-821f-b43f65c5af2b
source-git-commit: cf22b13e0f7c8e66b598f85aab81b022480e60bc
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 0%

---

# Använda Sling-kort{#using-sling-adapters}

[Sling](https://sling.apache.org) erbjuder ett [adaptermönster](https://sling.apache.org/documentation/the-sling-engine/adapters.html) för att enkelt översätta objekt som implementerar gränssnittet [Adaptable](https://sling.apache.org/apidocs/sling5/org/apache/sling/api/adapter/Adaptable.html#adaptTo%28java.lang.Class%29). Det här gränssnittet tillhandahåller en generisk [customiTo()](https://sling.apache.org/apidocs/sling5/org/apache/sling/api/adapter/Adaptable.html#adaptTo%28java.lang.Class%29)-metod som översätter objektet till den klasstyp som skickas som argument.

Om du till exempel vill översätta ett Resource-objekt till motsvarande Node-objekt kan du bara göra följande:

```java
Node node = resource.adaptTo(Node.class);
```

## Användningsexempel {#use-cases}

Det finns följande användningsområden:

* Få implementeringsspecifika objekt.

  En JCR-baserad implementering av det generiska [`Resource`](https://sling.apache.org/apidocs/sling5/org/apache/sling/api/resource/Resource.html)-gränssnittet ger till exempel åtkomst till den underliggande JCR [`Node`](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html).

* Skapa genvägar för objekt som kräver att interna kontextobjekt skickas.

  Den JCR-baserade [`ResourceResolver`](https://sling.apache.org/apidocs/sling5/org/apache/sling/api/resource/ResourceResolver.html) innehåller till exempel en referens till begärans [`JCR Session`](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Session.html) som i sin tur behövs för många objekt som kan fungera baserat på den begärandesessionen, till exempel [`PageManager`](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/PageManager.html) eller [`UserManager`](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/security/UserManager.html).

* Genväg till tjänster.

  Ett sällsynt fall - `sling.getService()` är också enkelt.

### Null, returvärde {#null-return-value}

`adaptTo()` returnerar null.

Skälen varierar, bland annat:

* Implementeringen stöder inte måltypen.
* En adapterfabrik som hanterar det här ärendet är inte aktiv (till exempel på grund av att det saknas tjänstreferenser)
* Ett internt villkor misslyckades.
* Tjänsten är inte tillgänglig.

Det är viktigt att du hanterar skiftläget null på ett smidigt sätt. Vid jsp-återgivning kan det vara acceptabelt att jsp misslyckas om det resulterar i en tom del av innehållet.

### Cachning {#caching}

För att förbättra prestanda kan implementeringar cachelagra objektet som returneras från ett `obj.adaptTo()`-anrop. Om `obj` är samma är det returnerade objektet detsamma.

Den här cachningen utförs för alla `AdapterFactory`-baserade fall.

Det finns dock ingen allmän regel - objektet kan vara antingen en ny eller en befintlig instans. Det innebär att du inte kan förlita dig på något av beteendena. Därför är det viktigt, särskilt i `AdapterFactory`, att objekt kan återanvändas i det här scenariot.

### Så här fungerar det {#how-it-works}

Det finns olika sätt att implementera `Adaptable.adaptTo()`:

* Efter själva objektet, implementera själva metoden och mappa till vissa objekt.
* Med en [`AdapterFactory`](https://sling.apache.org/apidocs/sling5/org/apache/sling/api/adapter/AdapterFactory.html), som kan mappa godtyckliga objekt.

  Objekten måste fortfarande implementera gränssnittet `Adaptable` och måste utöka [`SlingAdaptable`](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/adapter/SlingAdaptable.html) (vilket skickar anropet `adaptTo` till en central adapterhanterare).

  Kopplar ihop med `adaptTo`-mekanismen för befintliga klasser, till exempel `Resource`.

* En kombination av båda.

I det första fallet kan Java™-dokumenten ange vad `adaptTo-targets` är möjligt. Men för vissa underklasser, som JCR-baserade resurser, är det ofta inte möjligt. I det senare fallet är implementeringar av `AdapterFactory` vanligtvis en del av de privata klasserna i ett paket och exponeras därför inte i ett klient-API eller listas i Java™-dokument. Teoretiskt sett skulle det vara möjligt att komma åt alla `AdapterFactory`-implementeringar från [ OSGi](/help/sites-deploying/configuring-osgi.md)-tjänstmiljön och titta på deras&quot;adaptable&quot; (sources and target)-konfigurationer, men inte att mappa dem till varandra. I slutändan beror det på den interna logiken, som måste dokumenteras. Därför referensen.

## Referens {#reference}

### Sling {#sling}

[**Resursen**](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html) anpassas till:

<table>
 <tbody>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html">Nod</a></td>
   <td>Om det är en JCR-nodbaserad resurs eller en JCR-egenskap som refererar till en nod</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Property.html">Egenskap</a></td>
   <td>Om det är en JCR-egenskapsbaserad resurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Item.html">Objekt</a></td>
   <td>Om det är en JCR-baserad resurs (nod eller egenskap)</td>
  </tr>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/util/Map.html">Karta</a></td>
   <td>Den returnerar en karta över egenskaperna, om det är en JCR-nodbaserad resurs (eller annan resurs som stöder värdekartor).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ValueMap.html">ValueMap</a></td>
   <td>Den returnerar en användbar karta över egenskaperna, om det är en JCR-nodbaserad resurs eller annan resurs som stöder värdekartor. Den kan också uppnås (enklare) med <br /> <code><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ResourceUtil.html">ResourceUtil.getValueMap(Resource)</a></code> (hanterar null-skiftläge osv.).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/inherit/InheritanceValueMap.html">ArvValueMap</a></td>
   <td>Tillägget <a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ValueMap.html">ValueMap</a> som gör att hierarkin med resurser kan beaktas vid sökning efter egenskaper.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ModifiableValueMap.html">ModiitableValueMap</a></td>
   <td>Ett tillägg till <a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ValueMap.html">ValueMap</a> som gör att du kan ändra egenskaper på den noden.</td>
  </tr>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/io/InputStream.html">InputStream</a></td>
   <td>Det returnerar det binära innehållet för en filresurs när den är JCR-nodbaserad (<code>nt:file</code> eller <code>nt:resource</code>), en paketresurs eller en filsystemresurs. Den returnerar data för en binär JCR-egenskapsresurs.</td>
  </tr>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/net/URL.html">URL</a></td>
   <td>Den returnerar en URL för resursen. Den returnerar databas-URL:en för en JCR-nodbaserad resurs, JAR-paketets URL för en paketresurs eller fil-URL:en för en filsystemresurs.</td>
  </tr>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/io/File.html">Fil</a></td>
   <td>Om det är en filsystemresurs</td>
  </tr>
  <tr>
   <td><a href="https://sling.apache.org/apidocs/sling5/org/apache/sling/api/scripting/SlingScript.html">SlingScript</a></td>
   <td>Om den här resursen är ett skript (till exempel jsp-fil) som en skriptmotor är registrerad med sling.</td>
  </tr>
  <tr>
   <td><a href="https://www.oracle.com/java/technologies/servlet-technology.html">Servlet</a></td>
   <td>Om den här resursen är ett skript (t.ex. jsp-fil) som en skriptmotor har registrerats med sling eller om det är en serverresurs.</td>
  </tr>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/String.html">String</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Boolean.html">Boolean</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Long.html">Long</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Double.html">Double</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/util/Calendar.html">Calendar</a><br /> <a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Value.html">Value</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/String.html">String[]</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Boolean.html">Boolean[]</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Long.html">Long[]</a><br /> <a href="https://docs.oracle.com/javase/1.5.0/docs/api/java/util/Calendar.html">Calendar[]</a><br /> <a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Value.html">Värde[]</a></td>
   <td>Returnerar värdena om det är en JCR-egenskapsbaserad resurs (och värdet passar).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/LabeledResource.html">EtiketteradResurs</a></td>
   <td>Om det är en JCR-nodbaserad resurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/Page.html">Sida</a></td>
   <td>Om det är en JCR-nodbaserad resurs och noden är en <code>cq:Page</code> (eller <code>cq:PseudoPage</code>).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/components/Component.html">Komponent</a></td>
   <td>Om det är en <code>cq:Component</code>-nodresurs</td>
  </tr>  
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/designer/Design.html">Design</a></td>
   <td>Om det är en designnod (<code>cq:Page</code>)</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/Template.html">Mall</a></td>
   <td>Om det är en <code>cq:Template</code>-nodresurs</td>
  </tr>  
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/msm/api/Blueprint.html">Blueprint</a></td>
   <td>Om det är en <code>cq:Template</code>-nodresurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/dam/api/Asset.html">Tillgång</a></td>
   <td>Om det är en <code>dam:Asset</code>-nodresurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/dam/api/Rendition.html">Återgivning</a></td>
   <td>Om det är en <code>dam:Asset</code>-återgivning (<code>nt:file</code> under återgivningsmappen för en <code>dam:Asset</code>)</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/tagging/Tag.html">Tagg</a></td>
   <td>Om det är en <code>cq:Tag</code>-nodresurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/security/UserManager.html">UserManager</a></td>
   <td>Baserat på JCR-sessionen om det är en JCR-baserad resurs och användaren har behörighet att komma åt UserManager.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/api/security/user/Authorizable.html">Auktoriserbar</a></td>
   <td>Authorizable är det gemensamma grundgränssnittet för Användare och Grupp.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/api/security/user/User.html">Användare</a></td>
   <td>Användaren är en speciell auktoriseringsfunktion som kan autentiseras och personifieras.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/search/SimpleSearch.html">SimpleSearch</a></td>
   <td>Söker under resursen (eller använder setSearchIn()) om det är en JCR-baserad resurs.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/workflow/status/WorkflowStatus.html">WorkflowStatus</a></td>
   <td>Arbetsflödesstatus för den angivna sidans/arbetsflödets nyttolastnod.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/replication/ReplicationStatus.html">ReplicationStatus</a></td>
   <td>Replikeringsstatus för den angivna resursen eller dess <code>jcr:content</code>-undernod (markerad först).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/connector/ConnectorResource.html">KopplingResurs</a></td>
   <td>Den returnerar en anpassad anslutningsresurs för vissa typer, om det är en JCR-nodbaserad resurs.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/contentsync/config/package-summary.html">Konfig</a></td>
   <td>Om det är en <code>cq:ContentSyncConfig</code>-nodresurs</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/contentsync/config/package-summary.html">ConfigEntry</a></td>
   <td>Om den ligger under en <code>cq:ContentSyncConfig</code>-nodresurs</td>
  </tr>
 </tbody>
</table>

[**ResourceResolver**](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/ResourceResolver.html) anpassas till:

<table>
 <tbody>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Session.html">Session</a></td>
   <td>JCR-sessionen för begäran, om det är en JCR-baserad resurslösare (standard).</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/PageManager.html">PageManager</a></td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/components/ComponentManager.html">ComponentManager</a></td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/designer/Designer.html">Designer</a></td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/dam/api/AssetManager.html">AssetManager</a></td>
   <td>Baserat på JCR-sessionen, om det är en JCR-baserad resurslösare.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/tagging/TagManager.html">TagManager</a></td>
   <td>Baserat på JCR-sessionen, om det är en JCR-baserad resurslösare.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/api/security/user/UserManager.html">UserManager</a></td>
   <td>UserManager ger åtkomst till och möjlighet att underhålla auktoriseringsbara objekt som är användare och grupper. UserManager är bundet till en viss session.
   </td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/api/security/user/Authorizable.html">Kan auktoriseras</a> </td>
   <td>Den aktuella användaren.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/jackrabbit/api/security/user/User.html">Användare</a><br /> </td>
   <td>Den aktuella användaren.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/search/QueryBuilder.html">QueryBuilder</a></td>
   <td> </td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/Externalizer.html">Externalizer</a></td>
   <td>För extern generering av absoluta URL:er, även utan begäranobjektet.<br /> </td>
  </tr>
 </tbody>
</table>

[**SlingHttpServletRequest**](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/SlingHttpServletRequest.html) anpassas till:

Inga mål ännu, men implementerar Adaptable och kan användas som källa i en anpassad AdapterFactory.

[**SlingHttpServletResponse**](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/SlingHttpServletResponse.html) anpassas till:

<table>
 <tbody>
  <tr>
   <td><a href="https://docs.oracle.com/javase/1.5.0/docs/api/org/xml/sax/ContentHandler.html">ContentHandler</a><br /> (XML)</td>
   <td>Om det är ett svarsalternativ</td>
  </tr>
 </tbody>
</table>

#### WCM {#wcm}

**[Sidan](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/Page.html)** anpassas till:

<table>
 <tbody>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html">Resurs</a><br /> </td>
   <td>Sidans resurs.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/LabeledResource.html">EtiketteradResurs</a></td>
   <td>Den namngivna resursen är den aktuella resursen. Det vill säga, samma objekt som du tittar på.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html">Nod</a></td>
   <td>Sidans nod.</td>
  </tr>
  <tr>
   <td>...</td>
   <td>Allt som sidans resurs kan anpassas till.</td>
  </tr>
 </tbody>
</table>

**Komponenten [](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/components/Component.html)** anpassas till:

| [Resurs](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html) | Komponentens resurs. |
| --- | --- |
| [LabeledResource](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/LabeledResource.html) | Den namngivna resursen är den aktuella resursen. Det vill säga, samma objekt som du tittar på. |
| [Nod](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html) | Komponentens nod. |
| ... | Allt som komponentens resurs kan anpassas till. |

**[Mallen](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/wcm/api/Template.html)** anpassas till:

<table>
 <tbody>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html">Resurs</a><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html"><br /> </a></td>
   <td>Mallens resurs.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/commons/LabeledResource.html">EtiketteradResurs</a></td>
   <td>Den namngivna resursen är den aktuella resursen. Det vill säga, samma objekt som du tittar på.</td>
  </tr>
  <tr>
   <td><a href="https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html">Nod</a></td>
   <td>Nod för mallen.</td>
  </tr>
  <tr>
   <td>...</td>
   <td>Allt som mallens resurs kan anpassas till.</td>
  </tr>
 </tbody>
</table>

#### Dokumentskydd {#security}

**Behörighetsbar**, **Användare och **Grupp** kan anpassas till:

| [Nod](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html) | Returnerar hemnoden för användaren/gruppen. |
| --- | --- |
| [ReplicationStatus](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/com/day/cq/replication/ReplicationStatus.html) | Den returnerar replikeringsstatusen för användarens/gruppens hemnod. |

#### DAM {#dam}

**Resursen** anpassas till:

| [Resurs](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html) | Resurs för tillgången. |
| --- | --- |
| [Nod](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html) | Nod för resursen. |
| ... | Allt som resursen kan anpassas till. |

#### Tagg {#tagging}

**Tagg** anpassas till:

| [Resurs](https://developer.adobe.com/experience-manager/reference-materials/6-5-lts/javadoc/org/apache/sling/api/resource/Resource.html) | Taggens resurs. |
| --- | --- |
| [Nod](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html) | Taggens nod. |
| ... | Allt som taggens resurs kan anpassas till. |

#### Övriga {#other}

Dessutom innehåller Sling/JCR/OCM även ` [AdapterFactory](https://sling.apache.org/site/adapters.html#Adapters-AdapterFactory)` för anpassade OCM-objekt ([Object Content Mapping](https://jackrabbit.apache.org/jcr/object-content-mapping.html)).
