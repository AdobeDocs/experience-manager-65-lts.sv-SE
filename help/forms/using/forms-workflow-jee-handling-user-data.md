---
title: Forms JEE-arbetsflöden | Hantera användardata
description: Lär dig hur du använder AEM Forms JEE-arbetsflöden för att utforma, skapa och hantera affärsprocesser.
topic-tags: grdp
products: SG_EXPERIENCEMANAGER/6.5/FORMS
role: Admin,User
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
source-git-commit: 29391c8e3042a8a04c64165663a228bb4886afb5
workflow-type: tm+mt
source-wordcount: '1379'
ht-degree: 0%

---

# Forms JEE-arbetsflöden | Hantera användardata {#forms-jee-workflows-handling-user-data}

AEM Forms JEE-arbetsflöden innehåller verktyg för att utforma, skapa och hantera affärsprocesser. En arbetsflödesprocess består av en serie steg som körs i en angiven ordning. Varje steg utför en specifik åtgärd, till exempel att tilldela en uppgift till en användare eller skicka ett e-postmeddelande. En process kan samverka med resurser, användarkonton och tjänster och kan aktiveras på något av följande sätt:

* Starta en process från AEM Forms Workspace
* Använda SOAP eller RESTful-tjänsten
* Skicka ett anpassat formulär
* Använda bevakad mapp
* Använda e-post

Mer information om hur du skapar arbetsflöden i AEM Forms JEE finns i [Workbench-hjälpen](https://www.adobe.com/go/learn_aemforms_workbench_65).

## Användardata och datalager {#user-data-and-data-stores}

När en process aktiveras och fortskrider, hämtas data om processdeltagarna, data som anges av deltagarna i det formulär som är kopplat till processen samt bilagor som läggs till i formuläret. Data lagras i AEM Forms JEE-serverdatabasen, och om de är konfigurerade lagras vissa data, som bilagor, i GDS-katalogen (Global Document Storage). GDS-katalogen kan konfigureras på ett delat filsystem eller en databas.

## Få åtkomst till och ta bort användardata {#access-and-delete-user-data}

När en process aktiveras genereras ett unikt processinstans-ID och ett långlivat anrops-ID som associeras med processinstansen. Du kan komma åt och ta bort data för en processinstans baserat på det långvariga anrops-ID:t. Du kan ta fram det långvariga anrops-ID:t för en processinstans med användarnamnet för processinitieraren eller processdeltagarna som har skickat sina uppgifter.

Du kan dock inte identifiera processens instans-ID för en initierare i följande scenarier:

* **Processen som utlöses via en bevakad mapp**: Det går inte att identifiera en processinstans med dess initierare om processen utlöses av en bevakad mapp. I det här fallet kodas användarinformationen i de lagrade data.
* **Processen har initierats från publiceringen av AEM-instansen**: Alla processinstanser som har utlösts från AEM publiceringsinstans samlar inte in information om initieraren. Användardata kan dock hämtas i det format som är associerat med processen, som lagras i arbetsflödesvariabler.
* **Process initierad via e-post**: Avsändarens e-post-ID hämtas som en egenskap i en ogenomskinlig blobbkolumn i databastabellen `tb_job_instance` som inte kan frågas direkt.

### Identifiera processinstans-ID när arbetsflödesinitieraren eller deltagaren är känd {#initiator-participant}

Utför följande steg så att du kan identifiera processinstans-ID för en arbetsflödesinitierare eller deltagare:

1. Kör följande kommando i AEM Forms Server-databasen för att hämta det primära ID:t för arbetsflödesinitieraren eller deltagaren från databastabellen `edcprincipalentity`.

   ```sql
   select id from edcprincipalentity where canonicalname='user_ID'
   ```

   Frågan returnerar det huvud-ID som har angetts för `user_ID`.

1. (**För arbetsflödesinitieraren**) Kör följande kommando för att hämta alla uppgifter som är associerade med initierarens huvud-ID från databastabellen `tb_task`.

   ```sql
   select * from tb_task where start_task = 1 and create_user_id= 'initiator_principal_id'
   ```

   Frågan returnerar uppgifter som initierats av angiven `initiator`_ `principal_id`. Det finns två typer av uppgifter:

   * **Slutförda aktiviteter**: De här aktiviteterna har skickats och visar ett alfanumeriskt värde i fältet `process_instance_id`. Observera alla processinstans-ID:n för skickade uppgifter och fortsätt med stegen.
   * **Aktiviteter har initierats men inte slutförts**: De här aktiviteterna har initierats men har inte skickats ännu. Värdet i fältet `process_instance_id` för de här aktiviteterna är **0** (noll). I det här fallet bör du notera motsvarande aktivitets-ID och se [Arbeta med överblivna aktiviteter](#orphan).

1. (**För arbetsflödesdeltagare**) Kör följande kommando för att hämta processinstans-ID:n som är associerade med processtagarens huvud-ID från databastabellen `tb_assignment`.

   ```sql
   select distinct a.process_instance_id from tb_assignment a join tb_queue q on a.queue_id = q.id where q.workflow_user_id='participant_principal_id'
   ```

   Frågan returnerar instans-ID:n för alla processer som är associerade med deltagaren, inklusive de där deltagaren inte har skickat någon uppgift.

   Observera alla processinstans-ID:n för skickade uppgifter och fortsätt med stegen.

   För överblivna uppgifter eller uppgifter där `process_instance_id` är 0 (noll) bör du notera motsvarande aktivitets-ID och se [Arbeta med överblivna aktiviteter](#orphan).

1. Följ instruktionerna i avsnittet [Rensa användardata från arbetsflödesinstanser baserat på processinstans-ID:n](/help/forms/using/forms-workflow-jee-handling-user-data.md#purge) så att du kan ta bort användardata för identifierade processinstans-ID:n.

### Identifiera processens instans-ID när användardata lagras i primitiva variabler {#primitive}

Ett arbetsflöde kan utformas så att användardata hämtas i en variabel som lagras som en blob i databasen. I sådana fall kan du bara fråga efter användardata om de lagras i någon av följande primitiva variabeltyper:

* **String**: Innehåller användar-ID direkt eller som en delsträng och kan frågas med SQL.
* **Numeric**: Innehåller användar-ID direkt.
* **XML**: Innehåller användar-ID som en delsträng i texten som lagras som textkolumner i databasen och kan frågas som strängar.

Utför följande steg så att du kan avgöra om ett arbetsflöde som lagrar data i primitiva variabler innehåller data för användaren:

1. Kör följande databaskommando:

   ```sql
   select database_table from omd_object_type where name='pt_<app_name>/<workflow_name>'
   ```

   Frågan returnerar ett tabellnamn i formatet `tb_<number>` för det angivna programmet ( `app_name`) och arbetsflödet ( `workflow_name`).

   >[!NOTE]
   >
   >Värdet för egenskapen `name` kan vara komplext om arbetsflödet är kapslat i undermappar i programmet. Se till att du anger den exakta fullständiga sökvägen till arbetsflödet, som du kan hämta från databastabellen `omd_object_type`.

1. Granska tabellschemat `tb_<number>`. Tabellen innehåller variabler som lagrar användardata för det angivna arbetsflödet. Variablerna i tabellen motsvarar variablerna i arbetsflödet.

   Identifiera och notera variabeln som motsvarar arbetsflödesvariabeln som innehåller användar-ID:t. Om den identifierade variabeln är av primitiv typ kan du köra en fråga för att avgöra vilka arbetsflödesinstanser som är kopplade till ett användar-ID.

1. Kör följande databaskommando. I det här kommandot är `user_var` den primitiva variabeltypen som innehåller användar-ID.

   ```sql
   select process_instance_id from <tb_name> where <user_var>=<user_ID>
   ```

   Frågan returnerar alla processinstans-ID:n som är associerade med den angivna `user_ID`.

1. Följ instruktionerna i avsnittet [Rensa användardata från arbetsflödesinstanser baserat på processinstans-ID:n](/help/forms/using/forms-workflow-jee-handling-user-data.md#purge) så att du kan ta bort användardata för identifierade processinstans-ID:n.

### Rensa användardata från arbetsflödesinstanser baserat på processens instans-ID {#purge}

Nu när du har identifierat de processinstans-ID som är kopplade till en användare gör du följande för att ta bort användardata från respektive processinstans.

1. Kör följande kommando så att du kan hämta ett långlivat anrops-ID och status för en processinstans från tabellen `tb_process_instance`.

   ```sql
   select long_lived_invocation_id, status from tb_process_instance where id='process_instance_id'
   ```

   Frågan returnerar det långlivade anrops-ID:t och statusen för den angivna `process_instance_id`.

1. Skapa en instans av den offentliga `ProcessManager`-klienten ( `com.adobe.idp.workflow.client.ProcessManager`) med en `ServiceClientFactory`-instans med rätt anslutningsinställningar.

   Mer information finns i Java™ API-referens för [klassen ProcessManager](https://helpx.adobe.com/experience-manager/6-3/forms/ProgramLC/javadoc/com/adobe/idp/workflow/client/ProcessManager.html).

1. Kontrollera arbetsflödesinstansens status. Om statusen är annan än 2 (COMPLETE) eller 4 (TERMINATED) avslutar du instansen först genom att anropa följande metod:

   `ProcessManager.terminateProcess(<long_lived_invocation_id>)`.

1. Rensa arbetsflödesinstansen genom att anropa följande metod:

   `ProcessManager.purgeProcessInstance(<long_lived_invocation_id>)`

   Metoden `purgeProcessInstance` tar helt bort alla data för det angivna anrops-ID:t från AEM Forms Server-databasen och GDS, om de är konfigurerade.

### Arbeta med överblivna uppgifter {#orphan}

Enstaka uppgifter är de uppgifter vars innehållsprocess har initierats men inte skickats ännu. I det här fallet är `process_instance_id` **0** (noll). Därför kan du inte spåra användardata som lagrats för ägarlösa uppgifter med hjälp av processens instans-ID. Du kan dock spåra den med uppgifts-ID:t för en överbliven uppgift. Du kan identifiera uppgifts-ID:n från tabellen `tb_task` för en användare enligt beskrivningen i [Identifiera processinstans-ID:n när arbetsflödesinitieraren eller deltagaren är känd](/help/forms/using/forms-workflow-jee-handling-user-data.md#initiator-participant).

När du har uppgifts-ID:n gör du följande för att rensa de associerade filerna och data med en överbliven åtgärd från GDS och databasen.

1. Kör följande kommando i AEM Forms Server-databasen så att du kan hämta ID:n för de identifierade uppgifts-ID:n.

   ```sql
   select id from tb_form_data where task_id=<task_id>
   ```

   Frågan returnerar en lista med ID:n. För varje ID ( `fd_id`) som returneras i resultatet skapar du en lista med sträng för sessions-ID enligt följande:

   * _ `wfattach<task_id>`
   * `_wftask<fd_id>`
   * `_wftaskformid<fd_id>`

1. Utför något av följande steg beroende på om GDS pekar på ett filsystem eller en databas:

   1. **GDS i filsystemet**

      I GDS-filsystemet:

      1. Sök efter filer med följande sessions-ID-strängar som tillägg:

      * `_wfattach<task_id>`
      * `_wftask<fd_id>`
      * `_wftaskformid<fd_id>`

      Filerna med dessa tillägg är markörfilerna. De lagras med filnamn i följande format:

      `<file_name_guid>.session<session_id_string>`

      1. Ta bort alla markörfiler och andra filer med det exakta filnamnet som `<file_name_guid>` från filsystemet.

   1. **GDS i databasen**

      Kör följande kommandon för varje sessions-ID:

      ```sql
      delete from tb_dm_chunk where documentid in (select documentid from tb_dm_session_reference where sessionid=<session_id>)
      delete from tb_dm_session_reference where sessionid=<session_id>
      delete from tb_dm_deletion where sessionid=<session_id>
      ```

1. Kör följande kommandon så att du kan ta bort data för uppgifts-ID:n från AEM Forms Server-databasen:

   ```sql
   delete from tb_task_acl where task_id=<task_id>
   delete from tb_task_attachment where task_id=<task_id>
   delete from tb_form_data where task_id=<task_id>
   delete from tb_assignment where task_id=<task_id>
   delete from tb_task where id=<task_id>
   ```
