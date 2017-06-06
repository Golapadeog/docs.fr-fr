---
title: "Recherche de d&#233;couverte et FindCriteria | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 99016fa4-1778-495b-b4cc-0e22fbec42c6
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# Recherche de d&#233;couverte et FindCriteria
Une opération de recherche de découverte est lancée par un client pour découvrir un ou plusieurs services ; il s'agit de l'une des principales actions de la découverte.Effectuer une recherche envoie un message Probe WS\-Discovery sur le réseau.Les services qui correspondent aux critères spécifiés répondent avec des messages WS\-Discovery ProbeMatch.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] les messages de découverte, consultez [Spécification WS\-Discovery](http://go.microsoft.com/fwlink/?LinkID=122347).  
  
## DiscoveryClient  
 La classe <xref:System.ServiceModel.Discovery.DiscoveryClient> fournit le mécanisme pour effectuer des opérations de recherche et permet d'effectuer plus facilement des opérations clientes de découverte.Elle contient une méthode <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A>, qui effectue une recherche synchrone \(bloquante\), et une méthode <xref:System.ServiceModel.Discovery.DiscoveryClient.FindAsync%2A>, qui initialise une recherche asynchrone non bloquante.Ces deux méthodes prennent un paramètre <xref:System.ServiceModel.Discovery.FindCriteria> et fournissent les résultats à l'utilisateur via un objet <xref:System.ServiceModel.Discovery.FindResponse>.  
  
## FindCriteria  
 <xref:System.ServiceModel.Discovery.FindCriteria> possède plusieurs propriétés, qui peuvent être regroupées en critères de recherche \(spécifiant les services recherchés\) et critères d'arrêt de la recherche \(durée de la recherche\).Un <xref:System.ServiceModel.Discovery.FindCriteria> peut contenir plusieurs critères de recherche.Par défaut, le service doit correspondre à tous les composants, autrement, il ne se considère pas comme un service correspondant.Si vous souhaitez rechercher des services qui correspondent uniquement à certains critères, vous pouvez implémenter une logique de recherche personnalisée sur le service ou utiliser plusieurs requêtes.  
  
 Les critères de recherche incluent :  
  
-   <xref:System.ServiceModel.Discovery.ContractTypeNames%2A> : facultatif.Nom de contrat du service recherché et critères habituellement utilisés lors de la recherche d'un service.Si plusieurs noms de contrat sont spécifiés, seuls les points de terminaison de service correspondant à TOUS les contrats répondent.Notez que dans [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] un point de terminaison ne peut prendre en charge qu'un seul contrat.  
  
-   <xref:System.ServiceModel.Discovery.Scopes%2A> : facultatif.Les étendues sont des URI absolus utilisés pour définir les catégories de points de terminaison de service individuels.Vous pouvez les utiliser dans des scénarios où plusieurs points de terminaison exposent le même contrat, lorsque vous souhaitez rechercher un sous\-ensemble des points de terminaison.Si plusieurs étendues sont spécifiées, seuls les points de terminaison de service correspondant à TOUTES les étendues répondent.  
  
-   <xref:System.ServiceModel.Discovery.ScopeMatchBy%2A> : spécifie l'algorithme de correspondance à utiliser, lors de la comparaison des étendues dans le message Probe avec celles du point de terminaison.Il existe cinq règles de correspondance d'étendues prises en charge :  
  
    -   <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByExact%2A> fait une comparaison de chaînes de base sensible à la casse.  
  
    -   <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix%2A> correspond par segments séparés par « \/ ».Une recherche de http:\/\/contoso\/building1 fait correspondre un service à l'étendue http:\/\/contoso\/building\/floor1.Notez qu'il ne correspond pas à http:\/\/contoso\/building100, parce que les deux derniers segments ne correspondent pas.  
  
    -   <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByLdap%2A> établit une correspondance des étendues par segments à l'aide d'une URL LDAP.  
  
    -   <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByUuid%2A> établit une correspondance exacte des étendues, à l'aide d'une chaîne UUID.  
  
    -   <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByNone%2A> correspond uniquement aux services qui ne spécifient pas d'étendue.  
  
     Si une règle de correspondance d'étendue n'est pas spécifiée, <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix%2A> est utilisée.  
  
 Les critères d'arrêt sont les suivants :  
  
1.  <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> : temps d'attente maximal pour que les services répondent sur le réseau.La durée par défaut est de 20 secondes.  
  
2.  <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> : nombre maximal de réponses à attendre.Si les réponses <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> sont reçues avant que la propriété <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> expire, l'opération de recherche prend fin.  
  
## FindResponse  
 <xref:System.ServiceModel.Discovery.FindResponse> a une propriété de collection <xref:System.ServiceModel.Discovery.FindResponse.Endpoints%2A> qui contient toutes les réponses envoyées en mettant en correspondance les services sur le réseau.Si aucun service n'a répondu, la collection est vide.Si un ou plusieurs services ont répondu, chaque réponse est stockée dans un objet <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata>, qui contient l'adresse, le contrat et quelques informations supplémentaires sur le service.  
  
 L'exemple suivant indique comment effectuer une opération de recherche dans le code.  
  
```  
// Create DiscoveryClient  
DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
// Create FindCriteria  
FindCriteria findCriteria = new FindCriteria(typeof(IPrinterService));  
findCriteria.Scopes.Add(new Uri("http://www.contoso.com/building1/floor1"));  
findCriteria.Duration = TimeSpan.FromSeconds(10);   
  
// Find ICalculatorService endpoints              
FindResponse findResponse = discoveryClient.Find(findCriteria);  
  
Console.WriteLine("Found {0} ICalculatorService endpoint(s).", findResponse.Endpoints.Count)  
  
```  
  
## Voir aussi  
 [Vue d'ensemble de la découverte WCF](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)   
 [Utilisation du canal client de découverte](../../../../docs/framework/wcf/feature-details/using-the-discovery-client-channel.md)   
 [Discovery with Scopes](../../../../docs/framework/wcf/samples/discovery-with-scopes-sample.md)   
 [Asynchronous Find](../../../../docs/framework/wcf/samples/asynchronous-find-sample.md)   
 [Basic](../../../../docs/framework/wcf/samples/basic-sample.md)