---
title: Mappning av anpassade användargrupper i AEM 6.5
description: Läs om hur anpassad mappning av användargrupper fungerar i Adobe Experience Manager.
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.5/SITES
topic-tags: Security
content-type: reference
docset: aem65
feature: Administering
solution: Experience Manager, Experience Manager Sites
role: Admin
source-git-commit: 9c1c92e094f4d49881876aad4554c783825e5c3d
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 0%

---

# Mappning av anpassade användargrupper i AEM 6.5 {#custom-user-group-mapping-in-aem}

## Jämförelse av JCR-innehåll relaterat till CUG (anpassad användargrupp) {#comparison-of-jcr-content-related-to-cug}

<table>
 <tbody>
  <tr>
   <td><strong>Äldre AEM-versioner</strong></td>
   <td><strong>AEM 6.5</strong></td>
   <td><strong>Kommentar</strong></td>
  </tr>
  <tr>
   <td><p>Egenskap: cq:cugEnabled</p> <p>Deklarerar nodtyp: N/A, rest-egenskap</p> </td>
   <td><p>Behörighet:</p> <p>Nod: rep:cugPolicy of node type rep:CugPolicy</p> <p>Deklarera nodtyp: rep:CugMixin</p> <p> </p> <p> </p> <p> </p> Autentisering:</p> <p>Blandningstyp: granit:AuthenticationRequired</p> </td>
   <td><p>För att begränsa läsåtkomst tillämpas en dedikerad CUG-princip på målnoden.</p> <p>Obs! Profiler kan bara tillämpas på de sökvägar som stöds.</p> <p>Noder med namnet rep:cugPolicy och typen rep:CugPolicy är skyddade och kan inte skrivas med vanliga JCR API-anrop. Använd i stället JCR-åtkomststyrningshantering.</p> <p>Mer information finns på <a href="https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html">den här sidan</a>.</p> <p>För att framtvinga autentiseringskrav på en nod räcker det att lägga till blandningstypen granite:AuthenticationRequired.</p> <p>OBS! Endast under de sökvägar som stöds.</p> </td>
  </tr>
  <tr>
   <td><p>Egenskap: cq:cugPrincipals</p> <p>Deklarerar nodtyp: NA, resterande egenskap</p> </td>
   <td><p>Egenskap: rep:mainNames</p> <p>Deklarera nodtyp: rep:CugPolicy</p> </td>
   <td><p>Egenskapen som innehåller namnen på de objekt som har tillåtelse att läsa innehållet under den begränsade CUG-filen är skyddad och kan inte skrivas med vanliga JCR API-anrop. Använd i stället JCR-åtkomstkontrollhantering.</p> <p>Mer information om implementeringen finns på <a href="https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/authorization/PrincipalSetPolicy.html">den här sidan</a>.</p> </td>
  </tr>
  <tr>
   <td><p>Egenskap: cq:cugLoginPage</p> <p>Deklarerar nodtyp: NA, resterande egenskap</p> </td>
   <td><p>Egenskap: granite:loginPath (valfritt)</p> <p>Deklarerar nodtyp: granite:AuthenticationRequired</p> </td>
   <td><p>En JCR-nod som har blandningstypen granite:AuthenticationRequired definierad kan eventuellt definiera en alternativ inloggningssökväg.</p> <p>OBS! Endast under de sökvägar som stöds.</p> </td>
  </tr>
  <tr>
   <td><p>Egenskap: cq:cugRealm</p> <p>Deklarerar nodtyp: NA, resterande egenskap</p> </td>
   <td>NA</td>
   <td>Stöds inte längre med den nya implementeringen.</td>
  </tr>
 </tbody>
</table>

## Jämförelse av OSGi-tjänster {#comparison-of-osgi-services}

**Äldre AEM-versioner**

Etikett: Stöd för Adobe Granite Closed User Group (CUG)

Namn: com.day.cq.auth.impl.CugSupportImpl

**AEM 6.5**

* Etikett: Konfiguration av Apache Jackrabbit Oak CUG

  Namn: org.apache.jackrabbit.oak.spi.security.permission.cug.impl.CugConfiguration

  ConfigurationPolicy = REQUIRED

* Etikett: Apache Jackrabbit Oak CUG Exclude List

  Namn: org.apache.jackrabbit.oak.spi.security.permission.cug.impl.CugExcludeImpl

  ConfigurationPolicy = REQUIRED

* Namn: com.adobe.granite.auth.requirements.impl.RequirementService
* Etikett: Adobe Granite Authentication Requirement och Login Path Handler

  Namn: com.adobe.granite.auth.requirements.impl.DefaultRequirementHandler

  ConfigurationPolicy = REQUIRED

**Kommentarer**

* Konfiguration av CUG-auktoriseringen och aktivera/inaktivera utvärderingen.
Tjänst för att konfigurera exkluderingslista för huvudobjekt som inte ska påverkas av CUG-auktoriseringen.

  >[!NOTE]
  > 
  >Om `CugExcludeImpl` inte har konfigurerats återgår `CugConfiguration` till standardvärdet.

  Det går att koppla en anpassad CugExclude-implementering om det finns särskilda behov.

* OSGi-komponenten som implementerar LoginPathProvider som visar en matchande inloggningssökväg för LoginSelectorHandler. Den har en obligatorisk referens till en RequirementHandler som används för att registrera observatören som lyssnar på ändrade autentiseringskrav som lagras i innehållet med hjälp av blandningstypen granite:AuthenticationRequired.
* OSGi-komponenten som implementerar RequirementHandler som meddelar SlingAuthenticator om ändringar i auktoriseringskrav.

  Eftersom konfigurationsprincipen för den här komponenten är NÖDVÄNDIG aktiveras den bara om en uppsättning sökvägar som stöds har angetts.

  Om tjänsten aktiveras startas RequirementService.

<!-- nested tables not supported - text above is the table>
<table>
 <tbody>
  <tr>
   <td><strong>Older AEM Versions</strong></td>
   <td><strong>AEM 6.5</strong></td>
   <td><strong>Comments</strong></td>
  </tr>
  <tr>
   <td><p>Label: Adobe Granite Closed User Group (CUG) Support</p> <p>Name: com.day.cq.auth.impl.CugSupportImpl</p> </td>
   <td><p>Label: Apache Jackrabbit Oak CUG Configuration</p> <p>Name: org.apache.jackrabbit.oak.spi.security.authorization.cug.impl.CugConfiguration</p> <p>ConfigurationPolicy = REQUIRED</p> </td>
    <td><p>Label: Apache Jackrabbit Oak CUG Exclude List</p> <p>Name: org.apache.jackrabbit.oak.spi.security.authorization.cug.impl.CugExcludeImpl</p> <p>ConfigurationPolicy = REQUIRED</p> <p> </p> <p> </p> <p> </p> <p> </p> </td>
      </tr>
      <tr>
       <td>Name: com.adobe.granite.auth.requirement.impl.RequirementService</td>
      </tr>
      <tr>
       <td><p>Label: Adobe Granite Authentication Requirement and Login Path Handler</p> <p>Name: com.adobe.granite.auth.requirement.impl.DefaultRequirementHandler</p> <p>ConfigurationPolicy = REQUIRED</p> </td>
      </tr>
     </tbody>
    </table> </td>
   <td>
     <tbody>
      <tr>
       <td>Configuration of the CUG authorization and enable/disable the evaluation.</td>
      </tr>
      <tr>
       <td><p>Service to configure exclusion list of principals which should not be affected by the CUG authorization.</p> <p>NOTE: If the CugExcludeImpl is not configured, the CugConfiguration will fall back to the default.</p> <p>It is possible to plug a custom CugExclude implementation if there are special needs.</p> </td>
      </tr>
      <tr>
       <td>OSGi component implementing LoginPathProvider that exposes a matching login path to the LoginSelectorHandler. It has a mandatory reference to a RequirementHandler which is used to register the observer that listens to changed auth requirements stored in the content by the means of the granite:AuthenticationRequired mixin type. </td>
      </tr>
      <tr>
       <td><p>OSGi component implementing RequirementHandler that notifies the SlingAuthenticator about changes to authrequirements.</p> <p>As configuration policy for this component is REQUIRE it will only be activated if a set of supported paths is specified.</p> <p>Enabling the service will launch the RequirementService.</p> </td>
      </tr>
     </tbody>
     </td>
  </tr>
  <tr>
   <td> </td>
   <td> </td>
   <td> </td>
  </tr>
  <tr>
   <td> </td>
   <td> </td>
   <td> </td>
  </tr>
  <tr>
   <td> </td>
   <td> </td>
   <td> </td>
  </tr>
 </tbody>
</table>
-->
