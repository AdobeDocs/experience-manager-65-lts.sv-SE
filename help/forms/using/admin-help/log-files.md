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
exl-id: ff4dce07-725e-4750-9e95-4261b50580bd
source-git-commit: c3e9029236734e22f5d266ac26b923eafbe0a459
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 0%

---

# Loggfiler {#log-files}

Händelser som körnings- eller startfel registreras i programserverns loggfiler. Om du har problem med att distribuera till programservern kan du använda loggfilerna för att hitta problemet. Du kan öppna loggfilerna med valfri textredigerare.

(JBoss) Följande loggfiler finns i katalogen `[appserver root]/server/'server'/log`:

* boot.log
* server.log.*[åååå-mm-dd]*
* server.log

(WebLogic) Domänloggfilerna finns i katalogen `[appserverdomain]` och serverloggfilerna finns i katalogen `[appserverdomain]/servers/[appserver name]/logs`:

* `access.log`
* `[appserver name].log`
* `[appserver name].out.[incremental number]`

(WebSphere) Följande loggfiler finns i katalogen `[appserver root]/profiles/default/logs/[appserver name]`:

* SystemErr.log
* SystemOut.log
* StartServer.log
