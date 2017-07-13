---
title: "Param&#232;tres XSLT | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: fe60aaa0-ae43-4b1c-9be1-426af66ba757
caps.latest.revision: 2
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 2
---
# Param&#232;tres XSLT
Des paramètres XSLT sont ajoutés à l'objet <xref:System.Xml.Xsl.XsltArgumentList> en utilisant la méthode <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A>.  Un nom qualifié et un URI d'espace de noms sont associés à l'objet de paramètre à ce stade.  
  
### Pour utiliser un paramètre XSLT  
  
1.  Créez un objet <xref:System.Xml.Xsl.XsltArgumentList> et ajoutez le paramètre à l'aide de la méthode <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A>.  
  
2.  Appelez le paramètre à partir de la feuille de style.  
  
3.  Transmettez l'objet <xref:System.Xml.Xsl.XsltArgumentList> à la méthode <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A>.  
  
## Types de paramètres  
 L'objet de paramètre doit correspondre à un type W3C.  Le tableau suivant illustre les types W3C correspondants, les classes Microsoft .NET Framework équivalentes \(type\) et précise si le type W3C est un type XPath ou XSLT.  
  
|Type W3C|Équivalent de classe .NET \(type\)|Type XPath ou XSLT|  
|--------------|----------------------------------------|------------------------|  
|`String`|<xref:System.String?displayProperty=fullName>|XPath|  
|`Boolean`|<xref:System.Boolean?displayProperty=fullName>|XPath|  
|`Number`|<xref:System.Double?displayProperty=fullName>|XPath|  
|`Result Tree Fragment`|<xref:System.Xml.XPath.XPathNavigator?displayProperty=fullName>|XSLT|  
|`Node*`|<xref:System.Xml.XPath.XPathNavigator?displayProperty=fullName>|XPath|  
|`Node Set`|<xref:System.Xml.XPath.XPathNodeIterator><br /><br /> **XPathNavigator\[\]**|XPath|  
  
 \*Cela équivaut à une collection de nœuds contenant un seul nœud.  
  
 Si l'objet de paramètre n'est pas l'une des classes ci\-dessus, il est converti selon les règles suivantes.  Les types CLR numériques sont convertis en objet <xref:System.Double>.  Le type <xref:System.DateTime> est converti en <xref:System.String>.  Les types <xref:System.Xml.XPath.IXPathNavigable> sont convertis en <xref:System.Xml.XPath.XPathNavigator>.  **XPathNavigator\[\]** est converti en objet <xref:System.Xml.XPath.XPathNodeIterator>.  
  
 Tous les autres types provoquent une erreur.  
  
## Exemple  
 L'exemple suivant utilise la méthode <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> pour créer un paramètre destiné à contenir une date de remise calculée.  La date de la remise correspond à 20 jours à partir de la date de la commande.  
  
 [!code-csharp[XSLT_Param#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_Param/CS/xsltparam.cs#1)]
 [!code-vb[XSLT_Param#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_Param/VB/xsltparam.vb#1)]  
  
### Entrée  
  
##### order.xml  
 [!code-xml[XSLT_Param#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Param/XML/order.xml#2)]  
  
##### discount.xsl  
 [!code-xml[XSLT_Param#3](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Param/XML/discount.xsl#3)]  
  
### Sortie  
  
```  
<?xml version="1.0" encoding="utf-8"?>  
<order>  
  <total>36.9</total>  
     15% discount if paid by: 2/4/2004 12:00:00 AM  
</order>  
```  
  
## Voir aussi  
 [Transformations XSLT](../../../../docs/standard/data/xml/xslt-transformations.md)