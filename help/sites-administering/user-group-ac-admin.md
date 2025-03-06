---
title: Behörighetsadministration för användare, grupp och åtkomst
description: Läs mer om administration av användare, grupper och behörigheter i Adobe Experience Manager.
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
docset: aem65
feature: Security
solution: Experience Manager, Experience Manager Sites
role: Admin
exl-id: a1f4823f-4861-4e99-88cd-4a686abe3f64
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '3073'
ht-degree: 0%

---

# Behörighetsadministration för användare, grupper och åtkomst{#user-group-and-access-rights-administration}

Att aktivera åtkomst till en CRX-databas omfattar flera ämnen:

* [Åtkomstbehörighet](#how-access-rights-are-evaluated) - begrepp för hur de definieras och utvärderas
* [Användaradministration](#user-administration) - hanterar de enskilda konton som används för åtkomst
* [Gruppadministration](#group-administration) - förenkla användarhantering genom att skapa grupper
* [Hantering av åtkomstbehörighet](#access-right-management) - definierar principer som styr hur dessa användare och grupper kan komma åt resurser

De grundläggande elementen är:

**Användarkonton** - CRX autentiserar åtkomsten genom att identifiera och verifiera en användare (av en person eller ett annat program) enligt informationen i användarkontot.

I CRX är alla användarkonton en nod på arbetsytan. Ett CRX-användarkonto har följande egenskaper:

* Den representerar en användare av CRX.
* Den innehåller ett användarnamn och ett lösenord.
* Gäller för den arbetsytan.
* Den kan inte ha underanvändare. För hierarkiska åtkomsträttigheter bör du använda grupper.

* Du kan ange åtkomsträttigheter för användarkontot.

  För att förenkla hanteringen rekommenderar Adobe att du (i de flesta fall) tilldelar åtkomsträttigheter till gruppkonton. Att tilldela åtkomsträttigheter för varje enskild användare blir snabbt svårt att hantera (undantagen är vissa systemanvändare när det bara finns en eller två instanser).

**Gruppkonton** - Gruppkonton är samlingar med användare och/eller andra grupper. De används för att förenkla hanteringen eftersom en ändring av de åtkomsträttigheter som tilldelats en grupp automatiskt tillämpas på alla användare i gruppen. En användare behöver inte tillhöra någon grupp, men tillhör ofta flera.

I CRX har en grupp följande egenskaper:

* Den representerar en grupp användare med gemensamma åtkomsträttigheter. Till exempel författare eller utvecklare.
* Gäller för den arbetsytan.
* Det kan ha medlemmar. Dessa kan vara enskilda användare eller andra grupper.
* Hierarkisk gruppering kan uppnås med medlemsrelationer. Du kan inte placera en grupp direkt under en annan grupp i databasen.
* Du kan definiera åtkomsträttigheter för alla gruppmedlemmar.

**Åtkomstbehörighet** - CRX använder åtkomsträttigheter för att styra åtkomsten till vissa delar av databasen.

Detta görs genom att tilldela behörigheter för att antingen tillåta eller neka åtkomst till en resurs (nod eller sökväg) i databasen. Eftersom olika behörigheter kan tilldelas, måste de utvärderas för att avgöra vilken kombination som är tillämplig för den aktuella begäran.

Med CRX kan du konfigurera åtkomsträttigheter för både användare och gruppkonton. Samma grundläggande utvärderingsprinciper tillämpas sedan på båda.

## Hur åtkomsträttigheter utvärderas {#how-access-rights-are-evaluated}

>[!NOTE]
>
>CRX implementerar [åtkomstkontroll enligt definitionen i JSR-283](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/2.0/16_Access_Control_Management.html).
>
>En standardinstallation av en CRX-databas är konfigurerad att använda resursbaserade åtkomstkontrollistor. Detta är en möjlig implementering av åtkomstkontrollen JSR-283 och en av implementeringarna i Jackrabbit.

### Ämnen och huvudkonton {#subjects-and-principals}

CRX använder två viktiga koncept vid utvärdering av åtkomsträttigheter:

* Ett **huvudnamn** är en enhet som har åtkomsträttigheter. Huvudposter är:

   * Ett användarkonto
   * Ett gruppkonto

     Om ett användarkonto tillhör en eller flera grupper är det även associerat med vart och ett av dessa gruppobjekt.

* Ett **ämne** används för att representera källan för en begäran.

  Den används för att konsolidera de åtkomsträttigheter som är tillämpliga för den begäran. Dessa hämtas från:

   * Användarens huvudnamn

     De rättigheter som du tilldelar direkt till användarkontot.

   * Alla gruppobjekt som är associerade med den användaren

     Alla rättigheter tilldelas till någon av grupperna som användaren tillhör.

  Resultatet används sedan för att tillåta eller neka åtkomst till den begärda resursen.

#### Kompilera listan över åtkomsträttigheter för ett ämne {#compiling-the-list-of-access-rights-for-a-subject}

I CRX är ämnet beroende av:

* användarens huvudnamn
* alla gruppobjekt som är associerade med den användaren

Den förteckning över åtkomsträttigheter som är tillämpliga för föremålet är uppbyggd på följande:

* de rättigheter som du tilldelar direkt till användarkontot
* plus alla rättigheter som tilldelats någon av grupperna som användaren tillhör

![chlimage_1-56](assets/chlimage_1-56.png)

>[!NOTE]
>
>* CRX tar ingen hänsyn till användarhierarkin när listan kompileras.
>* CRX använder bara en grupphierarki när du tar med en grupp som medlem i en annan grupp. Det finns inget automatiskt arv av gruppbehörigheter.
>* Den ordning som du anger grupperna i påverkar inte åtkomsträttigheterna.
>

### Löser begäran och åtkomsträttigheter {#resolving-request-and-access-rights}

När CRX hanterar begäran jämför den åtkomstbegäran från ämnet med åtkomstkontrollistan på databasnoden:

Så om Linda begär att få uppdatera noden `/features` i följande databasstruktur:

![chlimage_1-57](assets/chlimage_1-57.png)

### Prioritetsordning {#order-of-precedence}

Åtkomsträttigheter i CRX utvärderas enligt följande:

* Användarprinciper har alltid företräde framför grupprincipobjekt oavsett:

   * i åtkomstkontrollistan
   * sin position i nodhierarkin

* För ett givet huvudobjekt finns (som mest) ett neka och 1 tillåt post på en viss nod. Implementeringen rensar alltid bort redundanta poster och ser till att samma privilegium inte finns med i både Tillåt- och Neka-posterna.

>[!NOTE]
>
>Denna utvärderingsprocess är lämplig för den resursbaserade åtkomstkontrollen i en CRX-standardinstallation.

Två exempel visas där användaren `aUser` är medlem i gruppen `aGroup`:

```xml
   + parentNode
     + acl
       + ace: aUser - deny - write
     + childNode
       + acl
         + ace: aGroup - allow - write
       + grandChildNode
```

I ovanstående fall:

* `aUser` har inte behörighet att skriva på `grandChildNode`.

```xml
   + parentNode
     + acl
       + ace: aUser - deny - write
     + childNode
       + acl
         + ace: aGroup - allow - write
         + ace: aUser - deny - write
       + grandChildNode
```

I detta fall:

* `aUser` har inte behörighet att skriva på `grandChildNode`.
* Den andra ACE för `aUser` är redundant.

Åtkomsträttigheter från flera gruppobjekt utvärderas baserat på deras ordning, både i hierarkin och i en enda åtkomstkontrollista.

### Bästa praxis {#best-practices}

I följande tabell visas några rekommendationer och metodtips:

<table>
 <tbody>
  <tr>
   <td>Rekommendation...</td>
   <td>Orsak...</td>
  </tr>
  <tr>
   <td><i>Använd grupper</i></td>
   <td><p>Undvik att tilldela behörigheter per användare. Det finns flera orsaker till detta:</p>
    <ul>
     <li>Du har många fler användare än grupper, så grupper förenklar strukturen.</li>
     <li>Grupper ger en översikt över alla konton.</li>
     <li>Arv är enklare med grupper.</li>
     <li>Användarna kommer och går. Grupper är långsiktiga.</li>
    </ul> </td>
  </tr>
  <tr>
   <td><i>Var positiv</i></td>
   <td><p>Använd alltid Allow-satser för att ange åtkomsträttigheter för grupprincipobjektet (när det är möjligt). Undvik att använda programsatsen Neka.</p> <p>Gruppobjekt utvärderas i ordning, både i hierarkin och i ordning i en enda åtkomstkontrollista.</p> </td>
  </tr>
  <tr>
   <td><i>Behåll det enkelt</i></td>
   <td><p>Att investera lite tid och fundera när du konfigurerar en ny installation är väl återbetalt.</p> <p>Genom att använda en tydlig struktur förenklas det pågående underhållet och administrationen, vilket säkerställer att både dina nuvarande kollegor och/eller framtida efterföljare enkelt kan förstå vad som implementeras.</p> </td>
  </tr>
  <tr>
   <td><i>Testa</i></td>
   <td>Använd en testinstallation för att öva och se till att du förstår relationen mellan olika användare och grupper.</td>
  </tr>
  <tr>
   <td><i>Standardanvändare/grupper</i></td>
   <td>Uppdatera alltid standardanvändare och standardgrupper omedelbart efter installationen för att förebygga säkerhetsproblem.</td>
  </tr>
 </tbody>
</table>

## Användaradministration {#user-administration}

En standarddialogruta används för **användaradministration**.

Du måste vara inloggad på rätt arbetsyta och sedan kan du öppna dialogrutan från båda:

* länken **Användaradministration** på CRX huvudkonsol
* menyn **Säkerhet** i CRX Explorer

![chlimage_1-58](assets/chlimage_1-58.png)

**Egenskaper**

* **Användar-ID**

  Kortnamn för kontot används vid åtkomst till CRX.

* **Principal Name**

  Ett fullständigt textnamn för kontot.

* **Lösenord**

  Behövs vid åtkomst till CRX med det här kontot.

* **ntlmhash**

  Tilldelad automatiskt för varje nytt konto och uppdateras när lösenordet ändras.

* Du kan lägga till nya egenskaper genom att definiera namn, typ och värde. Klicka på Spara (grön bocksymbol) för varje ny egenskap.

**Gruppmedlemskap**

Detta visar alla grupper som kontot tillhör. Den ärvda kolumnen anger medlemskap som har ärvts som ett resultat av medlemskap i en annan grupp.

Om du klickar på ett GroupID (om tillgängligt) öppnas [gruppadministration](#group-administration) för den gruppen.

**Personifierare**

Med personifieringsfunktionen kan en användare arbeta för en annan användares räkning.

Det innebär att ett användarkonto kan ange andra konton (användare eller grupp) som kan användas med deras konto. Med andra ord, om användare-B tillåts personifiera användare-A kan användare-B agera med hjälp av den fullständiga kontoinformationen för användare-A (inklusive ID, namn och åtkomsträttigheter).

Detta gör att persondatorkonton kan slutföra uppgifter som om de använde det konto de personifierar, till exempel under en frånvaro, eller dela en överbelastad belastning på kort sikt.

Om ett konto personifierar ett annat är det svårt att se. Loggfilerna innehåller ingen information om att personifiering har skett för händelserna. Så om användare-B personifierar användare-A kan alla händelser se ut som om de har utförts av användare-A personligen.

### Skapa ett användarkonto {#creating-a-user-account}

1. Öppna dialogrutan **Användaradministration**.
1. Klicka på **Skapa användare**.
1. Sedan kan du ange Egenskaper:

   * **Användar-ID** används som kontonamn.
   * **Lösenord** krävs vid inloggning.
   * **Principal Name** om du vill ange ett fullständigt textnamn.
   * **Mellanliggande sökväg** som kan användas för att skapa en trädstruktur.

1. Klicka på Spara (grön bocksymbol).
1. Dialogrutan är utökad så att du kan göra följande:

   1. Konfigurera **egenskaper**.
   1. Se **Gruppmedlemskap**.
   1. Definiera **personifierare**.

>[!NOTE]
>
>Prestandaförluster kan ibland ses när nya användare registreras i installationer som har ett stort antal av följande:
>
>* användare
>* grupper med många medlemmar
>

### Uppdatera ett användarkonto {#updating-a-user-account}

1. Öppna listvyn med alla konton i dialogrutan **Användaradministration**.
1. Navigera genom trädstrukturen.
1. Klicka på önskat konto så att du kan öppna det för redigering.
1. Gör en ändring och klicka sedan på Spara (grön bocksymbol) för den posten.
1. Klicka på **Stäng** för att slutföra, eller **Lista...** för att återgå till listan med alla användarkonton.

### Ta bort ett användarkonto {#removing-a-user-account}

1. Öppna listvyn med alla konton i dialogrutan **Användaradministration**.
1. Navigera genom trädstrukturen.
1. Markera det önskade kontot och klicka på **Ta bort användare**. Kontot tas bort omedelbart.

>[!NOTE]
>
>Detta tar bort noden för det här huvudkontot från databasen.
>
>Åtkomsthögerposter tas inte bort. Detta garanterar den historiska integriteten.

### Definiera egenskaper {#defining-properties}

Du kan definiera **egenskaper** för nya eller befintliga konton:

1. Öppna dialogrutan **Användaradministration** för det aktuella kontot.
1. Definiera ett **egenskapsnamn**.
1. Välj **Typ** i listrutan.
1. Definiera **värdet**.
1. Klicka på Spara (grön klicksymbol) för den nya egenskapen.

Befintliga egenskaper kan tas bort med papperskorgen.

Förutom lösenordet går det inte att redigera egenskaper, de måste tas bort och återskapas.

#### Ändra lösenordet {#changing-the-password}

**Lösenordet** är en speciell egenskap som du kan ändra genom att klicka på länken **Ändra lösenord**.

Du kan också ändra lösenordet till ditt eget användarkonto på menyn **Säkerhet** i CRX Utforskaren.

### Definiera en personifierare {#defining-an-impersonator}

Du kan definiera personifierare för antingen nya eller befintliga konton:

1. Öppna dialogrutan **Användaradministration** för det aktuella kontot.
1. Ange vilket konto som ska få personifiera det kontot.

   Du kan använda Bläddra.. för att välja ett befintligt konto.

1. Klicka på Spara (grön bocksymbol) för den nya egenskapen.

## Gruppadministration {#group-administration}

En standarddialogruta används för **gruppadministration**.

Du måste vara inloggad på rätt arbetsyta och sedan kan du öppna dialogrutan från båda:

* länken **Gruppadministration** på huvudkonsolen i CRX
* menyn **Säkerhet** i CRX Explorer

![chlimage_1-8](assets/chlimage_1-8.jpeg)

**Egenskaper**

* **Grupp-ID**

  Kortnamn för gruppkontot.

* **Principal Name**

  Ett fullständigt textnamn för gruppkontot.

* Du kan lägga till nya egenskaper genom att definiera namn, typ och värde. Klicka på Spara (grön bocksymbol) för varje ny egenskap.

* **Medlemmar**

  Du kan lägga till användare eller andra grupper som medlemmar i den här gruppen.

**Gruppmedlemskap**

Detta visar alla grupper som det aktuella gruppkontot tillhör. Den ärvda kolumnen anger medlemskap som har ärvts som ett resultat av medlemskap i en annan grupp.

När du klickar på ett GroupID öppnas dialogrutan för den gruppen.

**Medlemmar**

Visar alla konton (användare och/eller grupper) som är medlemmar i den aktuella gruppen.

Kolumnen **Ärvd** anger medlemskap som har ärvts som ett resultat av medlemskap i en annan grupp.

>[!NOTE]
>
>När rollen Ägare, Redigerare eller Visningsprogram tilldelas till en användare i en resursmapp skapas en ny grupp. Gruppnamnet har formatet `mac-default-<foldername>` för varje mapp som rollerna är definierade för.

### Skapa ett gruppkonto {#creating-a-group-account}

1. Öppna dialogrutan **Gruppadministration**.
1. Klicka på **Skapa grupp**.
1. Sedan kan du ange Egenskaper:

   * **Principal Name** om du vill ange ett fullständigt textnamn.
   * **Mellanliggande sökväg** som kan användas för att skapa en trädstruktur.

1. Klicka på Spara (grön bocksymbol).
1. Dialogrutan är utökad så att du kan:

   1. Konfigurera **egenskaper**.
   1. Se **Gruppmedlemskap**.
   1. Hantera **medlemmar**.

### Uppdatera ett gruppkonto {#updating-a-group-account}

1. Öppna listvyn med alla konton i dialogrutan **Gruppadministration**.
1. Navigera genom trädstrukturen.
1. Klicka på önskat konto så att du kan öppna det för redigering.
1. Gör en ändring och klicka sedan på Spara (grön bocksymbol) för den posten.
1. Klicka på **Stäng** för att slutföra, eller **Lista..** för att återgå till listan med alla gruppkonton.

### Ta bort ett gruppkonto {#removing-a-group-account}

1. Öppna listvyn med alla konton i dialogrutan **Gruppadministration**.
1. Navigera genom trädstrukturen.
1. Markera det önskade kontot och klicka på **Ta bort grupp**. Kontot tas bort omedelbart.

>[!NOTE]
>
>Detta tar bort noden för det här huvudkontot från databasen.
>
>Åtkomsthögerposter tas inte bort. Detta garanterar den historiska integriteten.

### Definiera egenskaper {#defining-properties-1}

Du kan definiera egenskaper för nya eller befintliga konton:

1. Öppna dialogrutan **Gruppadministration** för det aktuella kontot.
1. Definiera ett **egenskapsnamn**.
1. Välj **Typ** i listrutan.
1. Definiera **värdet**.
1. Klicka på Spara (grön bocksymbol) för den nya egenskapen.

Befintliga egenskaper kan tas bort med papperskorgen.

### Medlemmar {#members}

Du kan lägga till medlemmar i den aktuella gruppen:

1. Öppna dialogrutan **Gruppadministration** för det aktuella kontot.
1. Antingen:

   * Ange namnet på den obligatoriska medlemmen (användar- eller gruppkonto).
   * Eller använd **Bläddra..** för att söka efter och välja det huvudkonto (användar- eller gruppkonto) som du vill lägga till.

1. Klicka på Spara (grön bocksymbol) för den nya egenskapen.

Eller ta bort en befintlig medlem med papperskorgen.

## Behörighetshantering {#access-right-management}

På fliken **Åtkomstkontroll** i CRXDE Lite kan du definiera åtkomstkontrollprinciper och tilldela de relaterade behörigheterna.

För **Aktuell sökväg** kan du till exempel välja den resurs som krävs i den vänstra rutan på fliken Åtkomstkontroll i den nedre högra rutan:

![crx_access_control_tab](assets/crx_accesscontrol_tab.png)

Policyerna kategoriseras enligt:

* **Tillämpliga åtkomstkontrollprinciper**

  Dessa profiler kan tillämpas.

  Det här är profiler som är tillgängliga för att skapa en lokal profil. När du väljer och lägger till en tillämplig profil blir den en lokal profil.

* **Principer för lokal åtkomstkontroll**

  Detta är åtkomstkontrollprinciper som du har tillämpat. Du kan sedan uppdatera, beställa eller ta bort dem.

  En lokal princip åsidosätter alla principer som ärvs från den överordnade principen.

* **Effektiva åtkomstkontrollprinciper**

  Detta är de åtkomstkontrollprinciper som nu gäller för alla åtkomstbegäranden. De visar de aggregerade policyer som härletts från både lokala policyer och eventuella ärvda från det överordnade.

### Välj profil {#policy-selection}

Du kan välja profiler för:

* **Aktuell sökväg**

  Som i exemplet ovan väljer du en resurs i databasen. Profiler för den här &quot;aktuella sökvägen&quot; visas.

* **Databas**

  Väljer åtkomstkontroll på databasnivå. Om du till exempel anger privilegiet `jcr:namespaceManagement`, som bara är relevant för databasen, är det inte en nod.

* **Principal**

  Ett huvudkonto som är registrerat i databasen.

  Du kan antingen skriva namnet på **huvudkontot** eller klicka på ikonen till höger om fältet för att öppna dialogrutan **Välj huvudkonto** .

  På så sätt kan du **söka** efter en **användare** eller **grupp**. Välj önskat huvudkonto i resultatlistan och klicka sedan på **OK** för att överföra värdet tillbaka till föregående dialogruta.

![crx_access_control_selectpal](assets/crx_accesscontrol_selectprincipal.png)

>[!NOTE]
>
>För att förenkla hanteringen rekommenderar Adobe att du tilldelar åtkomsträttigheter till gruppkonton, inte enskilda användarkonton.
>
>Det är enklare att hantera ett fåtal grupper än många användarkonton.

### Behörighet {#privileges}

Följande behörigheter är tillgängliga när du lägger till en åtkomstkontrollpost (mer information finns i [API:t för säkerhet](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/security/Privilege.html)):

<table>
 <tbody>
  <tr>
   <th><strong>Namn på privilegium</strong></th>
   <th><strong>Vilken styr privilegiet att...</strong></th>
  </tr>
  <tr>
   <td><code>jcr:read</code></td>
   <td>Hämta en nod och läs dess egenskaper och värden.</td>
  </tr>
  <tr>
   <td><code>rep:write</code></td>
   <td>Detta är ett Jackrabbit-specifikt aggregeringsprivilegium för jcr:write och jcr:nodeTypeManagement.<br /> </td>
  </tr>
  <tr>
   <td><code>jcr:all</code></td>
   <td>Det här är ett aggregerat privilegium som innehåller alla andra fördefinierade privilegier.</td>
  </tr>
  <tr>
   <td><strong>Avancerat</strong></td>
   <td> </td>
  </tr>
  <tr>
   <td><code>crx:replicate</code></td>
   <td>Utför replikering av en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:addChildNodes</code></td>
   <td>Skapa underordnade noder för en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:lifecycleManagement</code></td>
   <td>Utför livscykelåtgärder på en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:lockManagement</code></td>
   <td>Lås och lås upp en nod. Uppdatera ett lås.</td>
  </tr>
  <tr>
   <td><code>jcr:modifyAccessControl</code></td>
   <td>Ändra en nods åtkomstkontrollprinciper.</td>
  </tr>
  <tr>
   <td><code>jcr:modifyProperties</code></td>
   <td>Skapa, ändra och ta bort egenskaperna för en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:namespaceManagement</code></td>
   <td>Registrera, avregistrera och ändra namnutrymmesdefinitioner.</td>
  </tr>
  <tr>
   <td><code>jcr:nodeTypeDefinitionManagement</code></td>
   <td>Importera nodtypsdefinitioner till databasen.</td>
  </tr>
  <tr>
   <td><code>jcr:nodeTypeManagement</code></td>
   <td>Lägg till och ta bort blandade nodtyper och ändra den primära nodtypen för en nod. Detta inkluderar även alla anrop till importmetoderna Node.addNode och XML där den nya nodens mixin eller primära typ uttryckligen anges.</td>
  </tr>
  <tr>
   <td><code>jcr:readAccessControl</code></td>
   <td>Läs en nods åtkomstkontrollprincip.</td>
  </tr>
  <tr>
   <td><code>jcr:removeChildNodes</code></td>
   <td>Ta bort underordnade noder för en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:removeNode</code></td>
   <td>Ta bort en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:retentionManagement</code></td>
   <td>Utför kvarhållningsåtgärder på en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:versionManagement</code></td>
   <td>Utför versionsåtgärder på en nod.</td>
  </tr>
  <tr>
   <td><code>jcr:workspaceManagement</code></td>
   <td>Skapa och ta bort arbetsytor med JCR-API:t.</td>
  </tr>
  <tr>
   <td><code>jcr:write</code></td>
   <td>Detta är ett aggregeringsprivilegium som innehåller:<br /> - jcr:modifyProperties<br /> - jcr:addChildNodes<br /> - jcr:removeNode<br /> - jcr:removeChildNodes</td>
  </tr>
  <tr>
   <td><code>rep:privilegeManagement</code></td>
   <td>Registrera ett nytt privilegium.</td>
  </tr>
 </tbody>
</table>

### Registrerar nya behörigheter {#registering-new-privileges}

Du kan även registrera nya behörigheter:

1. Välj **Verktyg** i verktygsfältet och sedan **Behörigheter** för att visa de behörigheter som för närvarande är registrerade.

   ![ac_privileges](assets/ac_privileges.png)

1. Använd ikonen **Registreringsbehörighet** (**+**) så att du kan definiera ett privilegium:

   ![ac_privilegieregister](assets/ac_privilegeregister.png)

1. Klicka på **OK** för att spara. Privilegiet är nu tillgängligt för val.

### Lägga till en åtkomstkontrollpost {#adding-an-access-control-entry}

1. Markera resursen och öppna fliken **Åtkomstkontroll**.

1. Om du vill lägga till nya **lokala åtkomstkontrollprinciper** klickar du på ikonen **+** till höger om listan **Tillämplig åtkomstkontrollprincip** :

   ![crx_access_control_applicable](assets/crx_accesscontrol_applicable.png)

1. En ny post visas under **Lokala åtkomstkontrollprinciper:**

   ![crx_access_control_newlocal](assets/crx_accesscontrol_newlocal.png)

1. Klicka på ikonen **+** så att du kan lägga till en post:

   ![crx_access_control_addentry](assets/crx_accesscontrol_addentry.png)

   >[!NOTE]
   >
   >För närvarande krävs en tillfällig lösning för att ange en tom sträng.
   >
   >För detta måste du använda `""`.

1. Definiera din åtkomstkontrollprincip och klicka på **OK** för att spara. Din nya policy är:

   * listas under **Lokal åtkomstkontrollprincip**
   * ändringarna återspeglas i **Effektiva åtkomstkontrollprinciper**.

CRX validerar ditt val. För ett givet huvudobjekt finns det (som mest) en neka och en Tillåt-post på en viss nod. Implementeringen rensar alltid bort redundanta poster och ser till att samma privilegium inte finns med i både Tillåt- och Neka-posterna.

### Principer för lokal åtkomstkontroll vid beställning {#ordering-local-access-control-policies}

Ordningen i listan anger i vilken ordning profilerna tillämpas.

1. Markera den önskade posten i tabellen **Lokala åtkomstkontrollprinciper** och dra den till den nya positionen i tabellen.

   ![crx_access_control_reorder](assets/crx_accesscontrol_reorder.png)

1. Ändringarna visas i båda tabellerna för **Lokal** och **Effektiva åtkomstkontrollprinciper**.

### Ta bort en åtkomstkontrollprincip {#removing-an-access-control-policy}

1. Klicka på den röda ikonen (-) till höger om posten i tabellen **Lokala åtkomstkontrollprinciper**.
1. Posten tas bort från båda tabellerna för **Lokal** och **Effektiva åtkomstkontrollprinciper**.

### Testa en åtkomstkontrollprincip {#testing-an-access-control-policy}

1. I CRXDE Lite-verktygsfältet väljer du **Verktyg** och sedan **Testa åtkomstkontroll...**.
1. En ny dialogruta öppnas i den övre högra rutan. Välj den **sökväg** och/eller **huvudnamn** som du vill testa.
1. Klicka på **Testa** om du vill se resultatet av markeringen:

   ![crx_access_control_test](assets/crx_accesscontrol_test.png)
