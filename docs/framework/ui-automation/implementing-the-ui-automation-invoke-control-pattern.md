---
title: "Implémentation du modèle de contrôle Invoke d'UI Automation"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-bcl
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- UI Automation, Invoke control pattern
- control patterns, Invoke
- Invoke control pattern
ms.assetid: e5b1e239-49f8-468e-bfec-1fba02ec9ac4
caps.latest.revision: "31"
author: Xansky
ms.author: mhopkins
manager: markl
ms.workload: dotnet
ms.openlocfilehash: 1d40bc94887df604577c025181ae7f5f2776cdc1
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="implementing-the-ui-automation-invoke-control-pattern"></a>Implémentation du modèle de contrôle Invoke d’UI Automation
> [!NOTE]
>  Cette documentation s'adresse aux développeurs .NET Framework qui souhaitent utiliser les classes [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] managées définies dans l'espace de noms <xref:System.Windows.Automation>. Pour obtenir les dernières informations sur [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consultez [API Windows Automation : UI Automation](http://go.microsoft.com/fwlink/?LinkID=156746).  
  
 Cette rubrique présente les conventions et recommandations à respecter pour implémenter <xref:System.Windows.Automation.Provider.IInvokeProvider>, notamment des informations sur les événements et les propriétés. Des liens vers des références supplémentaires sont répertoriés à la fin de la rubrique.  
  
 Le modèle de contrôle <xref:System.Windows.Automation.InvokePattern> est utilisé pour prendre en charge les contrôles qui ne conservent pas l’état lorsqu’ils sont activés, mais qui initialisent ou exécutent plutôt une action unique et non équivoque. Les contrôles qui conservent l’état, tels que les cases à cocher et les cases d’option, doivent implémenter respectivement <xref:System.Windows.Automation.Provider.IToggleProvider> et <xref:System.Windows.Automation.Provider.ISelectionItemProvider> à la place. Pour obtenir des exemples de contrôles qui implémentent le modèle de contrôle Invoke, consultez [Control Pattern Mapping for UI Automation Clients](../../../docs/framework/ui-automation/control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>   
## <a name="implementation-guidelines-and-conventions"></a>Conventions et recommandations en matière d'implémentation  
 Quand vous implémentez le modèle de contrôle Invoke, notez les conventions et recommandations suivantes :  
  
-   Les contrôles implémentent <xref:System.Windows.Automation.Provider.IInvokeProvider> si le même comportement n’est pas exposé par le biais d’un autre fournisseur de modèle de contrôle. Par exemple, si la méthode <xref:System.Windows.Automation.InvokePattern.Invoke%2A> exécute, sur un contrôle, la même action que la méthode <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> ou <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> , le contrôle ne doit pas implémenter <xref:System.Windows.Automation.Provider.IInvokeProvider>.  
  
-   Pour appeler un contrôle, il convient généralement de cliquer, de double-cliquer ou d’appuyer sur Entrée, sur un raccourci clavier prédéfini ou sur une autre combinaison de touches.  
  
-   <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent> est déclenché sur un contrôle qui a été activé (en réponse à un contrôle effectuant l’action qui lui est associée). Si possible, l’événement doit être déclenché une fois que le contrôle a terminé l’action et retourné une valeur sans se bloquer. L’événement Invoked doit être déclenché avant de prendre en charge la requête Invoke dans les scénarios suivants :  
  
    -   Il n’est pas possible ou pratique d’attendre que l’action soit terminée.  
  
    -   L’action requiert une intervention de l’utilisateur.  
  
    -   L’action prend du temps et bloque le client pendant un long moment.  
  
-   Si l’appel du contrôle entraîne des effets secondaires significatifs, ces effets secondaires doivent être exposés via la propriété <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HelpText%2A> . Par exemple, bien que la méthode <xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> ne soit pas associée à la sélection, <xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> peut entraîner la sélection d’un autre contrôle.  
  
-   Les effets du pointage (ou du pointage avec la souris) ne constituent généralement pas un événement Invoked. Toutefois, les contrôles qui exécutent une action (et non pas un effet visuel) basée sur l’état de pointage doivent prendre en charge le modèle de contrôle <xref:System.Windows.Automation.InvokePattern> .  
  
> [!NOTE]
>  Cette implémentation est considérée comme un problème d’accessibilité si le contrôle ne peut être appelé que suite à un effet secondaire lié à la souris.  
  
-   L’appel d’un contrôle est différent de la sélection d’un élément. Toutefois, selon le contrôle, l’appel peut avoir comme effet secondaire la sélection de l’élément. Par exemple, l’appel d’un élément de la liste de documents [!INCLUDE[TLA#tla_word](../../../includes/tlasharptla-word-md.md)] dans le dossier Mes documents sélectionne l’élément et ouvre le document.  
  
-   Un élément peut disparaître de l’arborescence [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] dès qu’il est appelé. La requête d’informations de l’élément fourni par le rappel d’événement peut échouer. La pré-récupération des informations mises en cache est la solution recommandée.  
  
-   Les contrôles peuvent implémenter plusieurs modèles de contrôle. Par exemple, le contrôle Fill Color dans la barre d’outils [!INCLUDE[TLA#tla_xl](../../../includes/tlasharptla-xl-md.md)] implémente les deux modèles de contrôle <xref:System.Windows.Automation.InvokePattern> et <xref:System.Windows.Automation.ExpandCollapsePattern> . <xref:System.Windows.Automation.ExpandCollapsePattern> expose le menu et le <xref:System.Windows.Automation.InvokePattern> remplit la sélection active de la couleur choisie.  
  
<a name="Required_Members_for_the_IValueProvider_Interface"></a>   
## <a name="required-members-for-iinvokeprovider"></a>Membres requis pour IInvokeProvider  
 Les propriétés et méthodes suivantes sont nécessaires à l'implémentation d' <xref:System.Windows.Automation.Provider.IInvokeProvider>.  
  
|Membres requis|Type de membre|Notes|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A>|méthode|<xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> est un appel asynchrone et doit retourner immédiatement une valeur sans se bloquer.<br /><br /> Ce comportement est particulièrement critique pour les contrôles qui, directement ou indirectement, lancent une boîte de dialogue modale lorsqu’ils sont appelés. Tout client UI Automation à l’origine de l’événement reste bloqué jusqu’à la fermeture de la boîte de dialogue modale.|  
  
<a name="Exceptions"></a>   
## <a name="exceptions"></a>Exceptions  
 Les fournisseurs doivent lever les exceptions suivantes.  
  
|Type d'exception|Condition|  
|--------------------|---------------|  
|<xref:System.Windows.Automation.ElementNotEnabledException>|Si le contrôle n’est pas activé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des modèles de contrôle UI Automation](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md)  
 [Prendre en charge des modèles de contrôle dans un fournisseur UI Automation](../../../docs/framework/ui-automation/support-control-patterns-in-a-ui-automation-provider.md)  
 [Modèles de contrôle UI Automation pour les clients](../../../docs/framework/ui-automation/ui-automation-control-patterns-for-clients.md)  
 [Appeler un contrôle à l’aide d’UI Automation](../../../docs/framework/ui-automation/invoke-a-control-using-ui-automation.md)  
 [Présentation de l’arborescence UI Automation](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)  
 [Utiliser la mise en cache dans UI Automation](../../../docs/framework/ui-automation/use-caching-in-ui-automation.md)
