---
title: Loggfiler
description: Händelser som körnings- eller startfel registreras i programserverns loggfiler, som kan öppnas med valfri textredigerare.
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_aem_forms
products: SG_EXPERIENCEMANAGER/6.5/FORMS
solution: Experience Manager, Experience Manager Forms
feature: Adaptive Forms
role: User, Developer
hide: true
hidefromtoc: true
exl-id: ff4dce07-725e-4750-9e95-4261b50580bd
source-git-commit: 103250f3442cf7c2793c51a95b1bf4fbaff71463
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---

# Loggfiler {#log-files}

Händelser som körnings- eller startfel registreras i programserverns loggfiler. Om du har problem med att distribuera till programservern kan du använda loggfilerna för att hitta problemet. Du kan öppna loggfilerna med valfri textredigerare.

(JBoss) Följande loggfiler finns i katalogen `[appserver root]/server/'server'/log`:

* boot.log
* server.log.*[yyy-mm-dd]*
* server.log

(WebLogic) Domänloggfilerna finns i katalogen `[appserverdomain]` och serverloggfilerna finns i katalogen `[appserverdomain]/servers/[appserver name]/logs`:

* `access.log`
* `[appserver name].log`
* `[appserver name].out.[incremental number]`

(WebSphere) Följande loggfiler finns i katalogen `[appserver root]/profiles/default/logs/[appserver name]`:

* SystemError.log
* SystemOut.log
* StartServer.log
