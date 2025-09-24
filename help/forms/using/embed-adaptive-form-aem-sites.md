---
title: Bädda in ett anpassningsbart formulär eller interaktiv kommunikation på AEM webbplats
description: Du kan bädda in anpassningsbara formulär på AEM webbplatssidor. Användarna kan fylla i och skicka formulär utan att lämna webbplatsens sidor.
contentOwner: vishgupt
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: author, interactive-communications
feature: Adaptive Forms,Foundation Components
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: 9d7f3c62-1b49-437c-9f61-1f0fc0156bdf
source-git-commit: 30ec8835be1af46e497457f639d90c1ee8b9dd6e
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# Bädda in ett anpassningsbart formulär eller interaktiv kommunikation på AEM webbplats {#embed-an-adaptive-form-or-interactive-communication-in-aem-sites-page}

<span class="preview"> Adobe rekommenderar att du använder den moderna och utbyggbara datainhämtningen [Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/adaptive-forms/introduction.html?lang=sv-SE) för [att skapa en ny adaptiv Forms](/help/forms/using/create-an-adaptive-form-core-components.md) eller [att lägga till Adaptiv Forms på AEM Sites-sidor](/help/forms/using/create-or-add-an-adaptive-form-to-aem-sites-page.md). De här komponenterna utgör ett betydande framsteg när det gäller att skapa adaptiva Forms-filer, vilket ger imponerande användarupplevelser. I den här artikeln beskrivs det äldre sättet att skapa Adaptiv Forms med baskomponenter. </span>

## Gäller för {#applies-to}

Den här dokumentationen gäller **AEM 6.5 LTS Forms**.

Mer information om AEM as a Cloud Service finns i [AEM Forms på Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/services/embed-adaptive-form-aem-sites.html?lang=sv-SE).


## Ökning {#overview}

Med AEM Forms kan formulärutvecklare smidigt bädda in adaptiva formulär och interaktiv kommunikation på en AEM Sites-sida eller en webbsida som ligger utanför AEM. Det inbäddade adaptiva formuläret och den interaktiva kommunikationen fungerar fullt ut och användarna kan fylla i och skicka formuläret utan att behöva lämna sidan. Det hjälper användaren att hålla sig i rätt sammanhang för andra element på webbsidan och interagera samtidigt med formuläret eller interaktiv kommunikation.

Mer information om hur du bäddar in adaptiva formulär på en extern webbsida finns i [Bädda in adaptiva formulär på en extern webbsida](/help/forms/using/embed-adaptive-form-external-web-page.md).

På AEM Sites-sidan kan du lägga till ett anpassningsbart formulär eller interaktiv kommunikation med:

* **[AEM Forms Container-komponent](/help/forms/using/embed-adaptive-form-aem-sites.md#af-component)**
AEM Forms innehåller en komponent som du kan lägga till på webbplatsens sidor. Med AEM Forms Container-komponenten kan du bädda in ett adaptivt formulär och interaktiv kommunikation.

* **[Resursläsaren](/help/forms/using/embed-adaptive-form-aem-sites.md#asset-browser)**
Alla formulär och interaktiv kommunikation som du skapar finns tillgängliga via Assets. Du kan dra och släppa formuläret som en resurs på sidan.

## Förutsättningar {#prerequisites}

Om du vill bädda in ett adaptivt formulär eller interaktiv kommunikation på en AEM-webbplatssida som använder en redigerbar mall, kontrollerar du att AEM Form-komponenten är konfigurerad som en tillåten komponent i den associerade mallen. Mer information finns i avsnittet **Princip och egenskaper (layoutbehållare)** i [Skapa sidmallar](/help/sites-authoring/templates.md).

Om det finns en platssida som använder en statisk mall måste du konfigurera den i styckesystemet på webbplatssidan. Mer information finns i [Konfigurera komponenter i designläge](/help/sites-authoring/default-components-designmode.md).

## Bädda in ett anpassningsbart formulär eller interaktiv kommunikation {#af-component}

Så här bäddar du in ett adaptivt formulär eller interaktiv kommunikation med komponenten AEM Forms Container:

1. Öppna sidan AEM webbplatser i redigeringsläge där du vill bädda in ett adaptivt formulär eller interaktiv kommunikation.
1. Dra AEM Forms Container-komponenten på sidan från panelen Komponentwebbläsaren.

   Du kan också söka efter ett anpassningsbart formulär eller en interaktiv kommunikation i webbläsaren Assets och dra och släppa det på sidan Webbplatser. Formuläret bäddas in i en AEM Forms-behållare.

   >[!NOTE]
   >
   >Flera AEM Forms Container-komponenter på en sida stöds inte.

1. Markera den inbäddade AEM Forms Container-komponenten på webbplatssidan och välj sedan ![settings_icon](assets/settings_icon.png) i åtgärdsfältet. Dialogrutan **[!UICONTROL Edit AEM Forms Container]** öppnas.
1. Ange följande i dialogrutan Redigera AEM Forms-behållare.

   * **Resurstyp:** Välj den typ av resurs som ska bäddas in. Alternativen är anpassningsbar form och interaktiv kommunikation
   * **Resurssökväg**: Bläddra och markera det adaptiva formuläret eller den interaktiva kommunikationen som ska bäddas in. Den fylls i automatiskt om du utelämnade den från Assets webbläsare.
   * (Endast anpassat formulär) **Efterbeställning**: Välj den åtgärd som ska utlösas när formulär skickas. Du kan visa ett tackmeddelande eller en tacksida.

      * **Tack**! Skriv ett meddelande med textredigeraren som ska visas när formulär skickas. Det här alternativet är endast tillgängligt när du väljer att visa ett tackmeddelande.
      * **Tack för sidan**: Bläddra och markera sidan som ska visas när formulär skickas. Det här alternativet är bara tillgängligt när du väljer att visa en tacksida.
      * **Uppdatera sidan vid överföring**: Aktivera så att du kan uppdatera sidan som innehåller det inbäddade adaptiva formuläret så att du kan se tack-sidan. I annat fall ersätter tack-sidan det anpassade formuläret i AEM Forms-behållaren, utan att sidan uppdateras. Det här alternativet är bara tillgängligt när du väljer att visa en tacksida.

   * **Tema**: Välj ett tema som definierar format för komponenter i ditt adaptiva formulär eller interaktiva kommunikation. Formateringen innehåller utseendeegenskaper som teckensnittsstil, bakgrundsfärg, dimensioner och justering.
   * **Höjd**: Ange behållarens höjd. Lämna det tomt om du vill ändra storlek på behållaren automatiskt.
   * **CSS-klientbibliotek**: Ange sökvägen till ett CSS-klientbibliotek.

1. Spara inställningarna. Det anpassningsbara formuläret eller den interaktiva kommunikationen är nu inbäddad på sidan.

## Publicera inbäddad adaptiv form och interaktiv kommunikation {#publishing-embedded-adaptive-form-and-interactive-communication}

Vi ska titta på följande scenarier för publicering av inbäddade resurser (adaptiv form eller interaktiv kommunikation) på AEM webbplats:

* Om du publicerar AEM webbplats för första gången och den innehåller ett inbäddat adaptivt formulär eller interaktiv kommunikation publicerar du webbplatssidan och den inbäddade resursen.
* Om du bara ändrade det inbäddade adaptiva formuläret eller den interaktiva kommunikationen på en publicerad webbplatssida publicerar du den ursprungliga resursen och ändringarna återspeglas på den publicerade webbplatssidan. Den publicerade webbplatssidan innehåller en referens till resursen och behöver inte publicera om sidan.
* Om du ändrade webbplatssidan och det inbäddade adaptiva formuläret eller den interaktiva kommunikationen publicerar du webbplatssidan och den inbäddade resursen igen.

## Modifiera inbäddad adaptiv form och interaktiv kommunikation {#modifying-embedded-adaptive-form-and-interactive-communication}

På AEM webbplats finns en referens till det adaptiva formuläret och den interaktiva kommunikationen i AEM Forms Container. Därför behålls alla konfigurationer och egenskaper, som tema, format och överföringsåtgärd, som konfigurerats i den ursprungliga adaptiva formen och interaktiv kommunikation i den inbäddade adaptiva formen och i den interaktiva kommunikationen.

Gör något av följande om du vill ändra någon konfiguration eller egenskap för det inbäddade adaptiva formuläret och den interaktiva kommunikationen.

* Öppna originalformuläret i anpassningsbara formulär eller i interaktiv kommunikation i respektive redigerare och ändra dem.
* Markera det adaptiva formuläret eller den interaktiva kommunikationen från webbplatssidan i redigeringsläge och välj sedan **[!UICONTROL Edit in a new window]**. Det ursprungliga formuläret öppnas i redigeringsläge som du kan ändra.

>[!NOTE]
>
>De ändringar som gjorts i det ursprungliga adaptiva formuläret eller den interaktiva kommunikationen återspeglas automatiskt i det inbäddade formuläret. Publicera om det adaptiva formuläret, den interaktiva kommunikationen eller webbplatssidan så att ändringarna på den publicerade sidan återspeglas.

## Överväganden och bästa praxis {#considerations-and-best-practices}

Tänk på följande när du bäddar in anpassningsbara formulär på AEM webbplatser:

* Sidhuvud och sidfot i det ursprungliga formuläret inkluderas inte i det inbäddade formuläret.
* Användarutkast och inskickade inbäddade formulär stöds och visas på flikarna Utkast och Skickat Forms på formulärportalen.
* Den skicka-åtgärd som är konfigurerad i det ursprungliga formuläret behålls i det inbäddade formuläret.
* Upplevelsemål och A/B-tester som konfigurerats på det ursprungliga formuläret fungerar inte i det inbäddade formuläret. Du kan dock använda målinriktning för upplevelser på webbplatssidan för att presentera olika formulär baserat på användarprofiler.
* Om du har konfigurerat Adobe Analytics för det ursprungliga formuläret hämtas analysdata från det inbäddade formuläret i Adobe Analytics. Den är dock inte tillgänglig i rapporten för formuläranalys.
