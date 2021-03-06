---
title: "Procédure : rechercher des instances non persistantes"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 294019b1-c1a7-4b81-a14f-b47c106cd723
caps.latest.revision: "5"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: bde3ab1049edf6cb52a221225321f1e505a2491b
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="how-to-query-for-non-persisted-instances"></a>Procédure : rechercher des instances non persistantes
Lorsqu'une nouvelle instance de service est créée et que le comportement de magasin d'instances de workflow SQL est défini pour le service, l'hôte de service crée une entrée initiale pour cette instance de service dans le magasin d'instances. Par la suite, lorsque l'instance de service persiste pour la première fois, le comportement de magasin d'instances de workflow SQL stocke l'état de l'instance actuelle avec les données supplémentaires qui sont requises pour l'activation, la récupération et le contrôle.  
  
 Si une instance n'est pas persistante après la création de l'entrée initiale pour l'instance, l'instance de service est considérée comme étant dans l'état non persistant. Toutes les instances de service persistantes peuvent être interrogées et contrôlées. Les instances de service non persistantes ne peuvent être ni interrogées, ni contrôlées. Si une instance non persistante est interrompue en raison d'une exception non prise en charge, elle peut être interrogée, mais pas contrôlée.  
  
 Les instances de service durables qui ne sont pas encore persistantes restent dans un état non persistant dans les scénarios suivants :  
  
-   L'hôte de service se bloque avant que l'instance soit persistante pour la première fois. L'entrée initiale pour l'instance reste dans le magasin d'instances. L'instance n'est pas récupérable. Si un message corrélé arrive, l'instance devient à nouveau active.  
  
-   L'instance rencontre une exception non prise en charge avant qu'elle soit persistante pour la première fois. Les scénarios suivants se produisent :  
  
    -   Si la valeur de la **UnhandledExceptionAction** est définie sur **abandonner**, les informations de déploiement de service sont écrites dans le magasin d’instances et l’instance est déchargée de la mémoire. L'instance reste dans l'état non persistant dans la base de données de persistance.  
  
    -   Si la valeur de la **UnhandledExceptionAction** est définie sur **AbandonAndSuspsend**, les informations de déploiement de service sont écrites pour la base de données de persistance et l’état de l’instance a la valeur  **Suspendu**. L'instance ne peut pas reprendre, être annulée ou être terminée. L'hôte de service ne peut pas charger l'instance car l'instance n'est pas encore persistante et, par conséquent, l'entrée de base de données pour l'instance n'est pas terminée.  
  
    -   Si la valeur de la **UnhandledExceptionAction** est définie sur **Annuler** ou **Terminate**, les informations de déploiement de service sont écrites dans le magasin d’instances et la état de l’instance a la valeur **terminé**.  
  
 Les sections suivantes fournissent des exemples de requêtes pour rechercher des instances non persistantes dans la base de données de persistance SQL et supprimer ces instances de la base de données.  
  
## <a name="to-find-all-instances-not-persisted-yet"></a>Pour rechercher toutes les instances pas encore persistantes  
 La requête SQL suivante retourne l'ID et l'heure de création de toutes les instances qui ne sont pas encore persistantes dans la base de données de persistance.  
  
```  
select InstanceId, CreationTime from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0;  
```  
  
## <a name="to-find-all-instances-not-persisted-yet-and-also-not-loaded"></a>Pour rechercher toutes les instances pas encore persistantes et également non chargées  
 La requête SQL suivante retourne l'ID et l'heure de création de toutes les instances qui ne sont pas persistantes et qui ne sont pas non plus chargées.  
  
```  
select InstanceId, CreationTime from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0 and CurrentMachine is NULL;  
```  
  
## <a name="to-find-all-suspended-instances-not-persisted-yet"></a>Pour rechercher toutes les instances interrompues pas encore persistantes  
 La requête SQL suivante retourne l'ID, l'heure de création, la raison de l'interruption et le nom de l'exception d'interruption de toutes les instances qui ne sont pas persistantes et qui sont également dans un état interrompu.  
  
```  
select InstanceId, CreationTime, SuspensionReason, SuspensionExceptionName from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0 and IsSuspended = 1;  
```  
  
## <a name="to-delete-non-persisted-instances-from-the-persistence-database"></a>Pour supprimer des instances non persistantes de la base de données de persistance  
 Vous devez vérifier régulièrement les instances non persistantes dans le magasin d'instances, et les supprimer si vous êtes sûr que l'instance ne recevra pas de message corrélé. Par exemple, si l'instance se trouve dans la base de données depuis plusieurs mois et que vous savez que le workflow a généralement une durée de vie de quelques jours, il est possible de supposer sans risque que c'est une instance non initialisée qui a bloqué.  
  
 En général, il est possible de supprimer sans risque des instances non persistantes qui ne sont pas interrompues ou pas chargées. Vous ne devez pas supprimer **tous les** les instances non persistantes car cet ensemble d’instances inclut des instances qui viennent d’être créées mais qui ne sont pas encore persistantes. Vous devez supprimer uniquement les instances non persistantes qui restent car l'hôte du service de workflow sur lequel l'instance était chargée a provoqué une exception ou l'instance elle-même a provoqué une exception.  
  
> [!WARNING]
>  La suppression d'instances non persistantes du magasin d'instances diminue la taille du magasin et peut améliorer les performances des opérations de stockage.
