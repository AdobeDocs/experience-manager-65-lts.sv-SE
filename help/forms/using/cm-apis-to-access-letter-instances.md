---
title: API:er för åtkomst till bokstavsinstanser
description: Upptäck API:er och använd dem för att programmässigt komma åt bokstavsinstanser i AEM Forms-miljön.
content-type: reference
products: SG_EXPERIENCEMANAGER/6.5/FORMS
topic-tags: correspondence-management
feature: Correspondence Management
solution: Experience Manager, Experience Manager Forms
role: Admin, User, Developer
exl-id: f698980d-d6f9-435d-853f-4d2827640aaf
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---

# API:er för åtkomst till bokstavsinstanser {#apis-to-access-letter-instances}

## Ökning {#overview}

Med hjälp av användargränssnittet Skapa korrespondens för korrespondenshantering kan du spara utkast av pågående bokstavsinstanser och det finns skickade brevinstanser.

Med Correspondence Management får du API:er som du kan använda för att skapa listgränssnittet som ska fungera med skickade brev eller utkast. API:erna listar och öppnar instanser av en agent som skickats och utkastbrev, så att agenten kan fortsätta arbeta med utkast eller skickade brev.

## Hämtar bokstavsinstanser {#fetching-letter-instances}

Correspondence Management exponerar API:er för att hämta bokstavsinstanser via tjänsten LetterInstanceService.

| Metod | Beskrivning |
|--- |--- |
| getAllLetterInstances | Hämtar bokstavsinstanser baserat på frågeindataparametern. Om du vill hämta alla bokstavsinstanser skickar du frågeparametern som null. |
| getLetterInstance | Hämtar den angivna bokstavsinstansen baserat på bokstavsinstansens ID. |
| letterInstanceExists | Kontrollerar om det finns en LetterInstance med det angivna namnet. |

>[!NOTE]
>
>LetterInstanceService är en OSGI-tjänst och instansen kan hämtas med @Reference i Java™
>Class eller sling.getService(LetterInstanceService). klass) i JSP.

### Använda getAllLetterInstances {#using-nbsp-getallletterinstances}

Följande API hittar bokstavsinstanserna baserat på frågeobjektet (både Skickat och Utkast). Om frågeobjektet är null returneras alla bokstavsinstanser. Detta API returnerar en lista med [LetterInstanceVO](https://helpx.adobe.com/aem-forms/6-2/javadocs/com/adobe/icc/dbforms/obj/LetterInstanceVO.html) -objekt, som kan användas för att extrahera ytterligare information om bokstavsinstansen.

**Syntax**: `List getAllLetterInstances(Query query) throws ICCException;`

<table>
 <tbody>
  <tr>
   <td><strong>Parameter</strong></td>
   <td><strong>Beskrivning</strong></td>
  </tr>
  <tr>
   <td>fråga</td>
   <td>Frågeparametern används för att hitta/filtrera instansen av Letter. Här har frågan bara stöd för objektets attribut/egenskaper på den översta nivån. Frågan består av satser och "attributeName" som används i Statement-objektet ska vara namnet på egenskapen i Letter-instansobjektet.<br /> </td>
  </tr>
 </tbody>
</table>

#### Exempel 1: Hämta alla bokstavsinstanser av typen INSKICKT {#example-fetch-all-the-letter-instances-of-type-submitted}

Följande kod returnerar listan med skickade bokstavsinstanser. Om du bara vill hämta utkast ändrar du `LetterInstanceType.COMPLETE.name()` till `LetterInstanceType.DRAFT.name().`

```java
@Reference
LetterInstanceService letterInstanceService;
Query query = new Query();

List<LetterInstanceVO> submittedLetterInstances = new ArrayList<LetterInstanceVO>();

Statement statementForInstanceType = new Statement();
statementForInstanceType.setAttributeName("letterInstanceType");
statementForInstanceType.setOperator(Operator.EQUALS);
statementForInstanceType.setAttributeValue(LetterInstanceType.COMPLETE.name());
query.addStatement(statementForInstanceType);
submittedLetterInstances = letterInstanceService.getAllLetterInstances(query);
```

#### Exempel 2: Hämta alla bokstavsinstanser som skickats av en användare och teckeninstanstypen är DRAFT {#example-nbsp-fetch-all-the-letter-instances-submitted-by-a-user-and-letter-instance-type-is-draft}

Följande kod har flera satser i samma fråga för att få resultaten filtrerade baserat på olika villkor, t.ex. bokstavsinstans som skickats (attribut som skickats av en användare) och typen av letterInstanceType är DRAFT.

```java
@Reference
LetterInstanceService letterInstanceService;

String submittedBy = "tglodman";
Query query = new Query();

List<LetterInstanceVO> submittedLetterInstances = new ArrayList<LetterInstanceVO>();

Statement statementForInstanceType = new Statement();
statementForInstanceType.setAttributeName("letterInstanceType");
statementForInstanceType.setOperator(Operator.EQUALS);
statementForInstanceType.setAttributeValue(LetterInstanceType.COMPLETE.name());
query.addStatement(statementForInstanceType);

Statement statementForSubmittedBy = new Statement();
statementForSubmittedBy .setAttributeName("submittedby");
statementForSubmittedBy .setOperator(Operator.EQUALS);
statementForSubmittedBy .setAttributeValue(submittedBy);
query.addStatement(statementForSubmittedBy );
submittedLetterInstances = letterInstanceService.getAllLetterInstances(query);
```

### Använda getLetterInstance {#using-nbsp-getletterinstance}

Hämta den bokstavsinstans som identifieras av det angivna bokstavsinstansens ID. Det returnerar &quot;null om instans-ID inte matchas.

**Syntax:** `public LetterInstanceVO getLetterInstance(String letterInstanceId) throws ICCException;`

```java
@Reference
LetterInstanceService letterInstanceService;
String letterInstanceId = "/content/apps/cm/letterInstances/1001/sampleLetterInstance";
LetterInstanceVO letterInstance = letterInstanceService.getLetterInstance(letterInstanceId );
```

### Verifierar om LetterInstance finns {#verifying-if-letterinstance-exist}

Kontrollera om det finns en bokstavsinstans med det angivna namnet

**Syntax**: `public Boolean letterInstanceExists(String letterInstanceName) throws ICCException;`

| **Parameter** | **Beskrivning** |
|---|---|
| letterInstanceName | Namnet på den bokstavsinstans som du vill kontrollera om den finns. |

```java
@Reference
LetterInstanceService letterInstanceService;
String letterInstanceName = "sampleLetterInstance";
Boolean result = letterInstanceService.letterInstanceExists(letterInstanceName );
```

## Inledande bokstavsinstanser {#opening-letter-instances}

Bokstavsinstans kan vara av typen Skickat eller Utkast. Om du öppnar båda bokstavsinstansstyperna visas olika beteenden:

* Om det finns en instans av Skickat brev öppnas en PDF som representerar förekomsten av brevet. Den skickade Letter-instansen som finns på servern innehåller även dataXML och bearbetad XDP, som kan användas för att utföra och ytterligare anpassad användning av exempelvis PDF/A.
* Om det finns en förekomst av ett utkast till brev, läses gränssnittet för att skapa korrespondens in på nytt till exakt föregående läge som det var när utkastet skapades

### Instans för inledande av utkast  {#opening-draft-letter-instance-nbsp}

CCR-gränssnittet stöder parametern cmLetterInstanceId som kan användas för att läsa in bokstaven igen.

`https://[hostName]:[portNo]/[contextPath]//aem/forms/createcorrespondence.html?random=[randomNo]&cmLetterInstanceId=[letterInstanceId]`

>[!NOTE]
>
>Du behöver inte ange cmLetterId eller cmLetterName/State/Version när du läser in korrespondensen igen, eftersom skickade data redan innehåller all information om korrespondensen som läses in igen. RandomNo används för att undvika problem med webbläsarens cache-lagring. Du kan använda en tidsstämpel som ett slumpmässigt tal.

### Öppnar skickad bokstavsinstans {#opening-submitted-letter-instance}

Skickade PDF kan öppnas direkt med ID:t för bokstavsinstansen:

`https://[hostName]:[portNo]/[contextPath]/[letterInstanceId]`
