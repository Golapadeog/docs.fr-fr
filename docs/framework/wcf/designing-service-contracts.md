---
title: "Conception de contrats de service | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "contrats de service (WCF)"
ms.assetid: 8e89cbb9-ac84-4f0d-85ef-0eb6be0022fd
caps.latest.revision: 34
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 33
---
# Conception de contrats de service
Cette rubrique explique ce que sont les contrats de service, comment ils sont définis, quelles opérations sont disponibles \(et les implications des échanges de messages sous\-jacents\), quels types de données sont utilisés et d'autres aspects qui vous aident à concevoir des opérations qui répondent aux spécifications de votre scénario.  
  
## Création d'un contrat de service  
 Les services exposent un certain nombre d'opérations.Dans les applications [!INCLUDE[indigo1](../../../includes/indigo1-md.md)], définissez les opérations en créant une méthode et en la marquant avec l'attribut <xref:System.ServiceModel.OperationContractAttribute>.Ensuite, pour créer un contrat de service, groupez vos opérations en les déclarant dans une interface marquée avec l'attribut <xref:System.ServiceModel.ServiceContractAttribute> ou en les définissant dans une classe marquée avec le même attribut.\(Pour obtenir un exemple de base, consultez [Comment : définir un contrat de service](../../../docs/framework/wcf/how-to-define-a-wcf-service-contract.md).\)  
  
 Toute méthode qui n'a pas d'attribut <xref:System.ServiceModel.OperationContractAttribute> n'est pas une opération de service et n'est pas exposée pour une utilisation par des clients de services [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].  
  
 Cette rubrique décrit les points de décision suivants lors de la conception d'un contrat de service :  
  
-   S'il faut utiliser des classes ou des interfaces.  
  
-   Comment spécifier les types de données que vous souhaitez échanger.  
  
-   Les types de modèles d'échange que vous pouvez utiliser.  
  
-   Si vous pouvez inclure des conditions de sécurité explicites dans le contrat.  
  
-   Les restrictions d'entrée et de sortie d'opération.  
  
## Classes ou interfaces  
 Les classes et les interfaces représentent un groupement de fonctionnalités et, par conséquent, toutes deux peuvent être utilisées pour définir un contrat de service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].Toutefois, il est recommandé d'utiliser des interfaces car elles modèlent directement des contrats de service.Sans implémentation, les interfaces ne font que définir un groupement de méthodes avec certaines signatures.Implémentez une interface de contrat de service et vous avez implémenté un service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].  
  
 Tous les avantages liées aux interfaces managées s'appliquent aux interfaces de contrat de service :  
  
-   Les interfaces de contrat de service peuvent étendre une quantité quelconque d'autres interfaces de contrat de service.  
  
-   Une classe unique peut implémenter une quantité quelconque de contrats de service en implémentant ces interfaces de contrat de service.  
  
-   Vous pouvez modifier l'implémentation d'un contrat de service en modifiant l'implémentation d'interface, tandis que le contrat de service reste le même.  
  
-   Vous pouvez affecter une version à votre service en implémentant l'ancienne interface et la nouvelle.Les clients anciens se connectent à la version d'origine, tandis que les nouveaux clients peuvent se connecter à la version plus récente.  
  
> [!NOTE]
>  Lorsque vous héritez d'autres interfaces de contrat de service, vous ne pouvez pas substituer de propriétés d'opération, telles que le nom ou l'espace de noms.Si vous essayez, vous créez une nouvelle opération dans le contrat de service actuel.  
  
 [!INCLUDE[crexample](../../../includes/crexample-md.md)] l'utilisation d'une interface pour créer un contrat de service, consultez [Comment : créer un service avec une interface de contrat](../../../docs/framework/wcf/feature-details/how-to-create-a-service-with-a-contract-interface.md).  
  
 Toutefois, vous pouvez utiliser une classe pour définir un contrat de service et implémenter ce contrat en même temps.Les avantages liés à la création de vos services en appliquant <xref:System.ServiceModel.ServiceContractAttribute> et <xref:System.ServiceModel.OperationContractAttribute> directement à la classe et aux méthodes sur la classe, respectivement, sont la vitesse et la simplicité.Les inconvénients sont que les classes managées ne prennent pas en charge l'héritage multiple ; en conséquence, elles peuvent implémenter un seul contrat de service à la fois.De plus, toute modification de la classe ou des signatures de méthode modifie le contrat public pour ce service, ce qui peut empêcher les clients non modifiés d'utiliser votre service.[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Implémentation de contrats de service](../../../docs/framework/wcf/implementing-service-contracts.md).  
  
 Pour obtenir un exemple de l'utilisation d'une classe pour créer et implémenter un contrat de service simultanément, consultez [Comment : créer un service avec une classe Contract](../../../docs/framework/wcf/feature-details/how-to-create-a-wcf-contract-with-a-class.md).  
  
 À ce stade, vous devez comprendre la différence entre le fait de définir votre contrat de service en utilisant une interface et en utilisant une classe.L'étape suivante consiste à déterminer les données qui peuvent être passées entre un service et ses clients.  
  
## Paramètres et valeurs de retour  
 Chaque opération a une valeur de retour et un paramètre, même s'il s'agit de `void`.Toutefois, contrairement à une méthode locale, dans laquelle vous pouvez passer des références à des objets d'un objet à un autre, les opérations de service ne passent pas de références à des objets.Au lieu de cela, elles passent des copies des objets.  
  
 Cette différence est significative car chaque type utilisé dans un paramètre ou une valeur de retour doit être sérialisable ; autrement dit, il doit être possible de convertir un objet de ce type en un flux d'octets et d'un flux d'octets en un objet.  
  
 Les types primitifs sont sérialisables par défaut, comme le sont de nombreux types dans le .NET Framework.  
  
> [!NOTE]
>  La valeur des noms de paramètres dans la signature de l'opération fait partie du contrat et respecte la casse.Si vous souhaitez utiliser le même nom de paramètre localement mais modifier le nom dans les métadonnées publiées, consultez <xref:System.ServiceModel.MessageParameterAttribute?displayProperty=fullName>.  
  
#### Contrats de données  
 Les applications orientées service telles que les applications [!INCLUDE[indigo1](../../../includes/indigo1-md.md)] sont conçues pour interagir avec la quantité la plus large possible d'applications clientes sur les plateformes Microsoft et non\-Microsoft.Pour l'interopérabilité la plus large possible, il est recommandé de marquer vos types avec les attributs <xref:System.Runtime.Serialization.DataContractAttribute> et <xref:System.Runtime.Serialization.DataMemberAttribute> afin de créer un contrat de données, qui est la partie du contrat de service qui décrit les données échangées par vos opérations de service.  
  
 Les contrats de données sont des contrats de style abonnement : aucun membre de données ou de type n'est sérialisé, à moins que vous n'appliquiez l'attribut de contrat de données explicitement.Les contrats de données ne sont pas liés à la portée d'accès du code managé : les membres de données privés peuvent être sérialisés et envoyés ailleurs afin d'être accessibles publiquement.\(Pour obtenir un exemple de contrat de données, consultez [Comment : créer un contrat de données de base destiné à une classe ou une structure](../../../docs/framework/wcf/feature-details/how-to-create-a-basic-data-contract-for-a-class-or-structure.md).\)[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] gère la définition des messages SOAP sous\-jacents qui activent les fonctionnalités de l'opération ainsi que la sérialisation de vos types de données dans et hors du corps des messages.Tant que vos types de données sont sérialisables, vous n'avez pas à vous soucier de l'infrastructure d'échange de messages sous\-jacente lors de la conception de vos opérations.  
  
 Bien que l'application [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] typique utilise les attributs <xref:System.Runtime.Serialization.DataContractAttribute> et <xref:System.Runtime.Serialization.DataMemberAttribute> pour créer des contrats de données pour les opérations, vous pouvez utiliser d'autres mécanismes de sérialisation.Les mécanismes <xref:System.Runtime.Serialization.ISerializable>, <xref:System.SerializableAttribute> et <xref:System.Xml.Serialization.IXmlSerializable> standard gèrent tous la sérialisation de vos types de données dans les messages SOAP sous\-jacents qui les transportent d'une application à une autre.Vous pouvez employer davantage de stratégies de sérialisation si vos types de données requièrent une prise en charge spéciale.[!INCLUDE[crabout](../../../includes/crabout-md.md)] les choix pour la sérialisation des types de données dans les applications [!INCLUDE[indigo2](../../../includes/indigo2-md.md)], consultez [Spécification du transfert de données dans des contrats de service](../../../docs/framework/wcf/feature-details/specifying-data-transfer-in-service-contracts.md).  
  
#### Mappage des paramètres et des valeurs de retour aux échanges de messages  
 Les opérations de service sont prises en charge par un échange sous\-jacent de messages SOAP qui transfèrent les données d'application, en plus des données requises par l'application pour prendre en charge certaines fonctionnalités standard liées à la sécurité, aux transaction et aux sessions.La signature d'une opération de service dicte un certain *modèle d'échange de messages \(MEP\)* sous\-jacent qui peut prendre en charge le transfert de données et les fonctionnalités requises par une opération.Vous pouvez spécifier trois modèles dans le modèle de programmation [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] : demande\/réponse, unidirectionnel et duplex.  
  
##### Demande\/réponse  
 Un modèle de demande\/réponse est un modèle dans lequel un expéditeur de demande \(une application cliente\) reçoit une réponse avec laquelle la demande est mise en corrélation.Il s'agit du modèle d'échange de messages par défaut, car il prend en charge une opération dans laquelle un ou plusieurs paramètres sont passés à l'opération et une valeur de retour est repassée à l'appelant.Par exemple, l'extrait de code C\# suivant illustre une opération de service simple qui prend une chaîne et retourne une chaîne.  
  
```csharp  
[OperationContractAttribute]  
string Hello(string greeting);  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
```vb  
<OperationContractAttribute()>  
Function Hello (ByVal greeting As String) As String  
```  
  
 Cette signature d'opération dicte la forme de l'échange de messages sous\-jacent.Si aucune corrélation n'existe, [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] ne peut pas déterminer à quelle opération la valeur de retour est destinée.  
  
 Notez qu'à moins que vous ne spécifiiez un modèle de messages sous\-jacent différent, même les opérations de service qui retournent `void` \(`Nothing` en [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\) sont des échanges de messages demande\/réponse.Le résultat de votre opération est qu'à moins qu'un client n'appelle l'opération de façon asynchrone, le client cesse le traitement jusqu'à ce que le message de retour soit reçu, bien que ce message soit vide dans le cas normal.L'exemple de code C\# suivant illustre une opération qui n'est retournée que lorsque le client a reçu un message vide dans la réponse.  
  
```csharp  
[OperationContractAttribute]  
void Hello(string greeting);  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
```vb  
<OperationContractAttribute()>  
Sub Hello (ByVal greeting As String)  
```  
  
 L'exemple précédent peut ralentir les performances et la réactivité du client si l'exécution de l'opération prend beaucoup de temps, mais les opérations demande\/réponse présentent des avantages même lorsqu'elles retournent `void`.Le plus évident est que des erreurs SOAP peuvent être retournées dans le message de réponse, ce qui indique qu'une certaine condition d'erreur liée au service s'est produite, lors de la communication ou lors du traitement.Les erreurs SOAP spécifiées dans un contrat de service sont passées à l'application cliente en tant qu'objets <xref:System.ServiceModel.FaultException%601>, où le paramètre de type est le type spécifié dans le contrat de service.Cela simplifie la notification des clients à propos des conditions d'erreur dans les services [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].[!INCLUDE[crabout](../../../includes/crabout-md.md)] les exceptions, les erreurs SOAP et la gestion des erreurs, consultez [Spécification et gestion des erreurs dans les contrats et les services](../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md).Pour obtenir un exemple d'un service et d'un client demande\/réponse, consultez [Comment : créer un contrat demande\-réponse](../../../docs/framework/wcf/feature-details/how-to-create-a-request-reply-contract.md).[!INCLUDE[crabout](../../../includes/crabout-md.md)] les problèmes avec le modèle de demande\-réponse, consultez [Services demande\-réponse](../../../docs/framework/wcf/feature-details/request-reply-services.md).  
  
##### Unidirectionnel  
 Si le client d'une application de service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] ne doit pas attendre que l'opération ait été traitée et ne traite pas les erreurs SOAP, l'opération peut spécifier un modèle de message unidirectionnel.Une opération unidirectionnelle est une opération dans laquelle un client appelle une opération et continue le traitement après que [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] a écrit le message sur le réseau.En général, cela signifie qu'à moins que le volume des données envoyées dans le message sortant ne soit extrêmement important, le client continue de s'exécuter presque immédiatement \(à moins qu'une erreur ne soit survenue pendant l'envoi des données\).Ce type de modèle d'échange de messages prend en charge un comportement semblable aux événements d'un client à une application de service.  
  
 Un échange de messages dans lequel un message est envoyé et aucun message n'est reçu ne peut pas prendre en charge une opération de service qui spécifie une valeur de retour autre que `void` ; dans ce cas, une exception <xref:System.InvalidOperationException> est levée.  
  
 L'absence de message de retour signifie également qu'il ne peut y avoir aucune erreur SOAP retournée pour signaler une erreur lors du traitement ou de la communication.\(La communication d'informations sur les erreurs lorsque les opérations sont unidirectionnelles requiert un modèle d'échange de messages duplex.\)  
  
 Pour spécifier un échange de messages unidirectionnel pour une opération qui retourne `void`, affectez à la propriété <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> la valeur `true`, comme dans l'exemple de code C\# suivant.  
  
```csharp  
[OperationContractAttribute(IsOneWay=true)]  
void Hello(string greeting);  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
```vb  
<OperationContractAttribute(IsOneWay := True)>  
Sub Hello (ByVal greeting As String)  
```  
  
 Cette méthode est identique à l'exemple de demande\/réponse précédent, mais le fait d'affecter à la propriété <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> la valeur `true` signifie que bien que la méthode soit identique, l'opération de service n'envoie pas de message de retour et les clients retournent immédiatement une fois le message sortant transmis à la couche du canal.Pour obtenir un exemple, consultez [Comment : créer un contrat unidirectionnel](../../../docs/framework/wcf/feature-details/how-to-create-a-one-way-contract.md).[!INCLUDE[crabout](../../../includes/crabout-md.md)] le modèle unidirectionnel, consultez [Services monodirectionnels](../../../docs/framework/wcf/feature-details/one-way-services.md).  
  
##### Duplex  
 Un modèle duplex se caractérise par la capacité qu'ont le service et le client à s'envoyer des messages l'un à l'autre, que le mode de messagerie utilisé soit unidirectionnel ou demande\/réponse.Cette forme de communication bidirectionnelle est utile pour les services qui doivent communiquer directement avec le client ou pour fournir une expérience asynchrone à l'un des côtés d'un échange de messages, y compris un comportement semblables aux événements.  
  
 Le modèle duplex est légèrement plus complexe que le modèle demande\/réponse ou unidirectionnel à cause du mécanisme supplémentaire nécessaire pour la communication avec le client.  
  
 Pour concevoir un contrat duplex, vous devez également concevoir un contrat de rappel et assigner le type de ce contrat de rappel à la propriété <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A> de l'attribut <xref:System.ServiceModel.ServiceContractAttribute> qui marque votre contrat de service.  
  
 Pour implémenter un modèle duplex, vous devez créer une deuxième interface qui contient les déclarations de méthode appelées sur le client.  
  
 [!INCLUDE[crexample](../../../includes/crexample-md.md)] la création d'un service, et un client qui accède à ce service, consultez [Comment : créer un contrat duplex](../../../docs/framework/wcf/feature-details/how-to-create-a-duplex-contract.md) et [Comment : accéder aux services ayant un contrat duplex](../../../docs/framework/wcf/feature-details/how-to-access-services-with-a-duplex-contract.md).Pour obtenir un exemple fonctionnel, consultez [Duplex](../../../docs/framework/wcf/samples/duplex.md).[!INCLUDE[crabout](../../../includes/crabout-md.md)] les problèmes liés à l'utilisation des contrats duplex, consultez [Services duplex](../../../docs/framework/wcf/feature-details/duplex-services.md).  
  
> [!CAUTION]
>  Lorsqu'un service reçoit un message duplex, il consultez l'élément `ReplyTo` dans ce message entrant afin de déterminer où envoyer la réponse.Si le canal utilisé pour recevoir le message n'est pas sécurisé, un client non fiable peut envoyer un message malveillant avec l'élément `ReplyTo` d'un ordinateur cible, ce qui peut provoquer un déni de service de cet ordinateur cible.  
  
##### Paramètres Out et Ref  
 Dans la plupart des cas, vous pouvez utiliser des paramètres `in` \(`ByVal` dans [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\) et des paramètres `out` et `ref` \(`ByRef` dans [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]\).Les paramètres `out` et `ref` indiquant tous deux que les données sont retournées à partir d'une opération, une signature d'opération telle que la suivante spécifie qu'une opération demande\/réponse est requise bien que la signature d'opération retourne `void`.  
  
```csharp  
[ServiceContractAttribute]  
public interface IMyContract  
{  
  [OperationContractAttribute]  
  public void PopulateData(ref CustomDataType data);  
}  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
 \[Visual Basic\]  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface IMyContract  
  <OperationContractAttribute()> _  
  Public Sub PopulateData(ByRef data As CustomDataType)  
End Interface  
```  
  
 Les seules exceptions sont les cas dans lesquels votre signature a une structure particulière.Par exemple, vous pouvez utiliser la liaison <xref:System.ServiceModel.NetMsmqBinding> pour communiquer avec les clients uniquement si la méthode utilisée pour déclarer une opération retourne `void` ; il ne peut y avoir aucune valeur de sortie, qu'il s'agisse d'une valeur de retour ou d'un paramètre `ref` ou `out`.  
  
 De plus, l'utilisation de paramètres `out` ou `ref` requiert que l'opération contienne un message de réponse sous\-jacent afin de retourner l'objet modifié.Si votre opération est unidirectionnelle, une exception <xref:System.InvalidOperationException> est levée au moment de l'exécution.  
  
### Spécifier le niveau de protection des messages sur le contrat  
 Lorsque vous concevez votre contrat, vous devez également décider du niveau de protection des messages des services qui implémentent votre contrat.Cela est nécessaire uniquement si la sécurité de message est appliquée à la liaison dans le point de terminaison du contrat.Si la sécurité est désactivée pour la liaison \(autrement dit, si la liaison fournie par le système affecte au <xref:System.ServiceModel.SecurityMode?displayProperty=fullName> la valeur <xref:System.ServiceModel.SecurityMode?displayProperty=fullName>\), vous n'avez pas à choisir le niveau de protection des messages pour le contrat.Dans la plupart des cas, les liaisons fournies par le système avec la sécurité au niveau du message appliquée fournissent un niveau de protection suffisant et il n'est pas nécessaire de considérer le niveau de protection sur la base de chaque opération ou de chaque message.  
  
 Le niveau de protection est une valeur qui spécifie si les messages \(ou parties de messages\) qui prennent en charge un service sont signés, signés et chiffrés, ou envoyés sans signature ou chiffrement.Le niveau de protection peut être défini à différentes portées : au niveau du service, pour une opération particulière, pour un message dans cette opération, ou pour une partie de message.Les valeurs définies à une portée deviennent la valeur par défaut pour les plus petites portées, sauf substitution explicite.Si une configuration de liaison est incapable de fournir le niveau de protection minimum requis pour le contrat, une exception est levée.Et lorsque aucune valeur de niveau de protection n'est définie explicitement sur le contrat, la configuration de liaison contrôle le niveau de protection pour tous les messages si la liaison a la sécurité de message.Il s'agit du comportement par défaut.  
  
> [!IMPORTANT]
>  L'affectation explicite à plusieurs portées d'un contrat un niveau de protection inférieur au niveau complet de <xref:System.Net.Security.ProtectionLevel?displayProperty=fullName> est en général une décision qui implique un compromis entre le degré de sécurité et les performances.Dans ces cas\-là, vos décisions doivent dépendre de vos opérations et de la valeur des données qu'elles échangent.[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Sécurisation de services](../../../docs/framework/wcf/securing-services.md).  
  
 Par exemple, l'exemple de code suivant ne définit pas la propriété <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> ou <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A> sur le contrat.  
  
```csharp  
[ServiceContract]  
public interface ISampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute]  
  public int GetInt();    
}  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
 \[Visual Basic\]  
  
```vb  
<ServiceContractAttribute()> _  
Public Interface ISampleService  
  
  <OperationContractAttribute()> _  
  Public Function GetString()As String  
  
  <OperationContractAttribute()> _  
  Public Function GetData() As Integer  
  
End Interface  
  
```  
  
 Lors de l'interaction avec une implémentation `ISampleService` dans un point de terminaison avec un <xref:System.ServiceModel.WSHttpBinding> par défaut \(le <xref:System.ServiceModel.SecurityMode?displayProperty=fullName>par défaut, qui est <xref:System.ServiceModel.SecurityMode>\), tous les messages sont chiffrés et signés puisqu'il s'agit du niveau de protection par défaut.Toutefois, lorsqu'un service `ISampleService` est utilisé avec un <xref:System.ServiceModel.BasicHttpBinding> par défaut \(le <xref:System.ServiceModel.SecurityMode>par défaut, qui est <xref:System.ServiceModel.SecurityMode>\), tous les messages sont envoyés comme du texte puisqu'il n'y a aucune sécurité pour cette liaison, et le niveau de protection est donc ignoré \(autrement dit, les messages ne sont ni chiffrés ni signés\).Si le <xref:System.ServiceModel.SecurityMode> était changé en <xref:System.ServiceModel.SecurityMode>, ces messages seraient chiffrés et signés \(car ce serait maintenant le niveau de protection par défaut de la liaison\).  
  
 Si vous souhaitez spécifier ou ajuster explicitement les exigences en matière de protection pour votre contrat, affectez à la propriété <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> \(ou à l'une des propriétés `ProtectionLevel` à une plus petite portée\) le niveau requis par votre contrat de service.Dans ce cas, l'utilisation d'un paramètre explicite requiert que la liaison prenne en charge ce paramètre au minimum pour la portée utilisée.Par exemple, l'exemple de code suivant spécifie explicitement une valeur <xref:System.ServiceModel.OperationContractAttribute.ProtectionLevel%2A>, pour l'opération `GetGuid`.  
  
```csharp  
[ServiceContract]  
public interface IExplicitProtectionLevelSampleService  
{  
  [OperationContractAttribute]  
  public string GetString();  
  
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.None)]  
  public int GetInt();    
  [OperationContractAttribute(ProtectionLevel=ProtectionLevel.EncryptAndSign)]  
  public int GetGuid();    
}  
```  
  
 L'exemple suivant illustre le code Visual Basic équivalent.  
  
```vb  
<ServiceContract()> _   
Public Interface IExplicitProtectionLevelSampleService   
    <OperationContract()> _   
    Public Function GetString() As String   
    End Function   
  
    <OperationContract(ProtectionLevel := ProtectionLevel.None)> _   
    Public Function GetInt() As Integer   
    End Function   
  
    <OperationContractAttribute(ProtectionLevel := ProtectionLevel.EncryptAndSign)> _   
    Public Function GetGuid() As Integer   
    End Function   
  
End Interface  
  
```  
  
 Un service qui implémente ce contrat `IExplicitProtectionLevelSampleService` et qui a un point de terminaison qui utilise le <xref:System.ServiceModel.WSHttpBinding> par défaut \(le <xref:System.ServiceModel.SecurityMode?displayProperty=fullName>par défaut, qui est <xref:System.ServiceModel.SecurityMode>\) a le comportement suivant :  
  
-   Les messages d'opération `GetString` sont chiffrés et signés.  
  
-   Les messages d'opération `GetInt` sont envoyés comme texte non chiffré et non signé \(autrement dit, en texte clair\).  
  
-   L'opération `GetGuid`<xref:System.Guid?displayProperty=fullName> est retournée dans un message qui est chiffré et signé.  
  
 [!INCLUDE[crabout](../../../includes/crabout-md.md)] les niveaux de protection et comment les utiliser, consultez [Fonctionnement des niveaux de protection](../../../docs/framework/wcf/understanding-protection-level.md).[!INCLUDE[crabout](../../../includes/crabout-md.md)] la sécurité, consultez [Sécurisation de services](../../../docs/framework/wcf/securing-services.md).  
  
##### Autres spécifications de signature d'opération  
 Certaines fonctionnalités d'application requièrent un type particulier de signature d'opération.Par exemple, la liaison <xref:System.ServiceModel.NetMsmqBinding> prend en charge les services et clients fiables, dans lesquels une application peut redémarrer au milieu de la communication et reprendre là où elle s'est interrompue sans entraîner la perte de messages.\([!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Files d'attente dans WCF](../../../docs/framework/wcf/feature-details/queues-in-wcf.md).\) Toutefois, les opérations fiables doivent prendre un seul paramètre `in` et n'avoir aucune valeur de retour.  
  
 Un autre exemple est l'utilisation de types <xref:System.IO.Stream> dans les opérations.Étant donné que le paramètre <xref:System.IO.Stream> inclut le corps du message entier, si une entrée ou une sortie \(autrement dit, un paramètre `ref`, un paramètre `out` ou une valeur de retour\) est de type <xref:System.IO.Stream>, il doit s'agir de la seule entrée ou sortie spécifiée dans votre opération.De plus, le paramètre ou type de retour doit être <xref:System.IO.Stream>, <xref:System.ServiceModel.Channels.Message?displayProperty=fullName> ou <xref:System.Xml.Serialization.IXmlSerializable?displayProperty=fullName>.[!INCLUDE[crabout](../../../includes/crabout-md.md)] les flux, consultez [Données volumineuses et diffusion en continu](../../../docs/framework/wcf/feature-details/large-data-and-streaming.md).  
  
##### Noms, espaces de noms et obscurcissement  
 Les noms et les espaces de noms des types .NET dans la définition de contrats et les opérations sont significatifs lorsque les contrats sont convertis en WSDL et lorsque les messages de contrat sont créés et envoyés.Par conséquent, il est vivement recommandé que les noms et les espaces de noms du contrat de service soient définis explicitement à l'aide des propriétés `Name` et `Namespace` de tous les attributs de contrat de prise en charge tels que <xref:System.ServiceModel.ServiceContractAttribute>, <xref:System.ServiceModel.OperationContractAttribute>, <xref:System.Runtime.Serialization.DataContractAttribute>,  <xref:System.Runtime.Serialization.DataMemberAttribute> et d'autres attributs de contrat.  
  
 Il en résulte notamment que si les noms et les espaces de noms ne sont pas définis explicitement, l'utilisation de l'obscurcissement IL sur l'assembly altère les noms et les espaces de noms des types de contrat, WSDL est modifié et les échanges sur le câble échouent généralement.Si vous ne définissez pas explicitement les noms et les espaces de noms des contrats mais prévoyez d'utiliser l'obscurcissement, utilisez les attributs <xref:System.Reflection.ObfuscationAttribute> et <xref:System.Reflection.ObfuscateAssemblyAttribute> pour empêcher la modification des noms et des espaces de noms des types de contrat.  
  
## Voir aussi  
 [Comment : créer un contrat demande\-réponse](../../../docs/framework/wcf/feature-details/how-to-create-a-request-reply-contract.md)   
 [Comment : créer un contrat unidirectionnel](../../../docs/framework/wcf/feature-details/how-to-create-a-one-way-contract.md)   
 [Comment : créer un contrat duplex](../../../docs/framework/wcf/feature-details/how-to-create-a-duplex-contract.md)   
 [Spécification du transfert de données dans des contrats de service](../../../docs/framework/wcf/feature-details/specifying-data-transfer-in-service-contracts.md)   
 [Spécification et gestion des erreurs dans les contrats et les services](../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md)   
 [Utilisation de sessions](../../../docs/framework/wcf/using-sessions.md)   
 [Opérations synchrones et asynchrones](../../../docs/framework/wcf/synchronous-and-asynchronous-operations.md)   
 [Services fiables](../../../docs/framework/wcf/reliable-services.md)   
 [Services et transactions](../../../docs/framework/wcf/services-and-transactions.md)