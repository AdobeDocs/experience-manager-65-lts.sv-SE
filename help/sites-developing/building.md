---
title: Bygga taggar i ett AEM-program
description: Arbeta med taggar eller utöka taggar i ett anpassat AEM-program
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: platform
content-type: reference
feature: Developing,Tagging
solution: Experience Manager, Experience Manager Sites
role: Developer
exl-id: c835a110-89cf-4857-9ee0-c0ad781a66ae
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '868'
ht-degree: 0%

---

# Bygga taggar i ett AEM-program{#building-tagging-into-an-aem-application}

För programmatiskt arbete med taggar eller tillägg av taggar i ett anpassat AEM-program beskriver den här sidan användningen av

* [API för taggning](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/tagging/package-summary.html)

Det interagerar med

* [Ramverk för taggning](/help/sites-developing/framework.md)

Mer information om taggning finns i:

* [Administrera taggar](/help/sites-administering/tags.md) om du vill ha information om hur du skapar och hanterar taggar och till vilka innehållstaggar har tillämpats.
* [Använder taggar](/help/sites-authoring/tags.md) för information om att tagga innehåll.

## Översikt över taggnings-API {#overview-of-the-tagging-api}

Implementeringen av [taggningsramverket](/help/sites-developing/framework.md) i AEM tillåter hantering av taggar och tagginnehåll med JCR-API:t. TagManager ser till att taggar som anges som värden i strängmatrisegenskapen `cq:tags` inte dupliceras, det tar bort tagg-ID:n som pekar på taggar som inte finns och uppdaterar tagg-ID:n för flyttade eller sammanfogade taggar. TagManager använder en JCR-observationslyssnare som återställer felaktiga ändringar. Huvudklasserna finns i paketet [com.day.cq.ta](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html?com/day/cq/tagging/package-summary.html):

* JcrTagManagerFactory - returnerar en JCR-baserad implementering av `TagManager`. Det är referensimplementeringen av taggnings-API:t.
* `TagManager` - gör att du kan matcha och skapa taggar efter sökvägar och namn.
* `Tag` - definierar taggobjektet.

### Hämta en JCR-baserad TagManager {#getting-a-jcr-based-tagmanager}

Om du vill hämta en TagManager-instans måste du ha en JCR `Session` och anropa `getTagManager(Session)`:

```java
@Reference
JcrTagManagerFactory jcrTagManagerFactory;

TagManager tagManager = jcrTagManagerFactory.getTagManager(session);
```

I det typiska Sling-sammanhanget kan du även anpassa dig till en `TagManager` från `ResourceResolver`:

```java
TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
```

### Hämta ett taggobjekt {#retrieving-a-tag-object}

En `Tag` kan hämtas via `TagManager` genom att antingen matcha en befintlig tagg eller skapa en:

```java
Tag tag = tagManager.resolve("my/tag"); // for existing tags

Tag tag = tagManager.createTag("my/tag"); // for new tags
```

För den JCR-baserade implementeringen, som mappar `Tags` till JCR `Nodes`, kan du direkt använda Sling `adaptTo`-mekanismen om du har resursen (till exempel `/content/cq:tags/default/my/tag`):

```java
Tag tag = resource.adaptTo(Tag.class);
```

Även om en tagg endast kan konverteras *från *en resurs (inte en nod), kan en tagg konverteras *till *både en nod och en resurs:

```java
Node node = tag.adaptTo(Node.class);
Resource node = tag.adaptTo(Resource.class);
```

>[!NOTE]
>
>Det går inte att direkt anpassa från `Node` till `Tag` eftersom `Node` inte implementerar metoden Sling `Adaptable.adaptTo(Class)`.

### Hämta och ställa in taggar {#getting-and-setting-tags}

```java
// Getting the tags of a Resource:
Tag[] tags = tagManager.getTags(resource);

// Setting tags to a Resource:
tagManager.setTags(resource, tags);
```

### Söker efter taggar {#searching-for-tags}

```java
// Searching for the Resource objects that are tagged with the tag object:
Iterator<Resource> it = tag.find();

// Retrieving the usage count of the tag object:
long count = tag.getCount();

// Searching for the Resource objects that are tagged with the tagID String:
 RangeIterator<Resource> it = tagManager.find(tagID);
```

>[!NOTE]
>
>Giltiga `RangeIterator` som ska användas är:
>
>`com.day.cq.commons.RangeIterator`

### Ta bort taggar {#deleting-tags}

```java
tagManager.deleteTag(tag);
```

### Replikerar taggar {#replicating-tags}

Det går att använda replikeringstjänsten ( `Replicator`) med taggar eftersom taggar är av typen `nt:hierarchyNode`:

```java
replicator.replicate(session, replicationActionType, tagPath);
```

## Tagga på klientsidan {#tagging-on-the-client-side}

Formulärwidgeten `CQ.tagging.TagInputField` används för att ange taggar. Den har en snabbmeny där du kan välja bland befintliga taggar, som innehåller automatisk komplettering och många andra funktioner. Dess xtype är `tags`.

## Taggskräpinsamlaren {#the-tag-garbage-collector}

Taggskräpinsamlaren är en bakgrundstjänst som rensar de dolda och oanvända taggarna. Dolda och oanvända taggar är taggar under `/content/cq:tags` som har en `cq:movedTo`-egenskap och inte används på en innehållsnod - de har ett antal som är noll. Genom att använda den här lat borttagningsprocessen behöver inte innehållsnoden (det vill säga egenskapen `cq:tags`) uppdateras som en del av flyttnings- eller sammanfogningsåtgärden. Referenserna i egenskapen `cq:tags` uppdateras automatiskt när egenskapen `cq:tags` uppdateras, till exempel via dialogrutan för sidegenskaper.

Taggskräpinsamlaren körs som standard en gång om dagen. Du kan konfigurera den på:

```xml
http://localhost:4502/system/console/configMgr/com.day.cq.tagging.impl.TagGarbageCollector
```

## Märkordssökning och tagglista {#tag-search-and-tag-listing}

Sökningen efter taggar och tagglistan fungerar enligt följande:

* Sökningen efter TagID söker efter de taggar som har egenskapen `cq:movedTo` inställd på TagID och följer genom `cq:movedTo` TagID:n.

* Om du söker efter taggen Title genomsöks bara de taggar som inte har någon `cq:movedTo`-egenskap.

## Taggar på olika språk {#tags-in-different-languages}

Som beskrivs i dokumentationen för att administrera taggar kan taggen `title` definieras på olika språk i avsnittet [Hantera taggar på olika språk](/help/sites-administering/tags.md#managing-tags-in-different-languages). Sedan läggs en språkkänslig egenskap till i taggnoden. Den här egenskapen har formatet `jcr:title.<locale>`, till exempel `jcr:title.fr` för den franska översättningen. `<locale>` måste vara en ISO-språksträng med gemener och använda&quot;_&quot; i stället för&quot;-&quot;, till exempel: `de_ch`.

När taggen **Djur** läggs till på sidan **Produkter** läggs värdet `stockphotography:animals` till i egenskapen `cq:tags` för noden /content/geometrixx/en/products/jcr:content. Översättningen refereras från taggnoden.

API:t på serversidan har lokaliserade `title`-relaterade metoder:

* [com.day.cq.tagging.Tag](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html?com/day/cq/tagging/Tag.html)

   * getLocalizedTitle(språkområde)
   * getLocalizedTitlePaths()
   * getLocalizedTitles()
   * getTitle(Locale locale)
   * getTitlePath(språkområde)

* [com.day.cq.tagging.TagManager](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html?com/day/cq/tagging/TagManager.html)

   * canCreateTagByTitle(String tagTitlePath, Locale locale)
   * createTagByTitle(String tagTitlePath, Locale locale)
   * resolveByTitle(String tagTitlePath, locale)

I AEM kan språket hämtas antingen från sidspråket eller från användarspråket:

* för att hämta sidspråket i en JSP:

   * `currentPage.getLanguage(false)`

* för att hämta användarspråket i en JSP:

   * `slingRequest.getLocale()`

`currentPage` och `slingRequest` är tillgängliga i en JSP via taggen [&lt;cq:definedObjects>](/help/sites-developing/taglib.md).

För taggning beror lokaliseringen på sammanhanget eftersom taggen `titles` kan visas på sidspråket, på användarspråket eller på något annat språk.

### Lägga till ett nytt språk i dialogrutan Redigera tagg {#adding-a-new-language-to-the-edit-tag-dialog}

Följande procedur beskriver hur du lägger till ett språk (finska) i dialogrutan **Taggredigering**:

1. Redigera flervärdesegenskapen `languages` för noden `/content/cq:tags` i **CRXDE**.

1. Lägg till `fi_fi` - som representerar den finska språkinställningen - och spara ändringarna.

Det nya språket (finska) är nu tillgängligt i taggdialogrutan för sidegenskaperna och i dialogrutan **Redigera tagg** när du redigerar en tagg i konsolen **Taggning** .

>[!NOTE]
>
>Det nya språket måste vara ett av de av AEM erkända språken. Det måste alltså vara tillgängligt som en nod under `/libs/wcm/core/resources/languages`.

>[!CAUTION]
>
>Om du installerar taggning av relaterat innehåll direkt i ett officiellt uppdateringspaket (inklusive Service Packs, Security Service Packs, Extended Feature Packs, Cumulative Feature Packs, Patches och liknande) återställs egenskapen languages för noden `/content/cq:tags` till standardvärdet. Det är därför nödvändigt att lägga till den från egenskaperna före installationen.
