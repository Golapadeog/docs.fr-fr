---
title: "Comment&#160;: d&#233;chiffrer des &#233;l&#233;ments XML avec les certificats X.509 | Microsoft Docs"
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
helpviewer_keywords: 
  - "certificats, certificats X.509"
  - "déchiffrement"
  - "System.Security.Cryptography.EncryptedXml (classe)"
  - "System.Security.Cryptography.X509Certificate2 (classe)"
  - "certificats X.509"
  - "chiffrement XML"
ms.assetid: bd015722-d88d-408d-8ca8-e4e475c441ed
caps.latest.revision: 7
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 7
---
# Comment&#160;: d&#233;chiffrer des &#233;l&#233;ments XML avec les certificats X.509
Vous pouvez utiliser les classes de l'espace de noms <xref:System.Security.Cryptography.Xml> pour chiffrer et déchiffrer un élément d'un document XML.  Le chiffrement XML est une méthode normalisée qui permet d'échanger et de stocker des données XML chiffrées sans que celles\-ci ne puissent être lues facilement.  Pour plus d'informations sur la norme de chiffrement XML, consultez la spécification W3C \(World Wide Web Consortium\) relative au chiffrement XML, située à l'adresse http:\/\/www.w3.org\/TR\/xmldsig\-core\/.  
  
 Cet exemple déchiffre un élément XML qui a été chiffré à l'aide des méthodes décrites dans : [Comment : chiffrer des éléments XML avec les certificats X.509](../../../docs/standard/security/how-to-encrypt-xml-elements-with-x-509-certificates.md).  Il recherche un élément \<`EncryptedData`\>, le déchiffre, puis le remplace par l'élément XML en texte brut d'origine.  
  
 L'exemple de code de cette procédure déchiffre un élément XML à l'aide d'un certificat X.509 depuis le magasin de certificats local du compte d'utilisateur actuel.  L'exemple utilise la méthode <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> pour récupérer automatiquement le certificat X.509 et déchiffrer une clé de session stockée dans l'élément \<`EncryptedKey`\> de l'élément \<`EncryptedData`\>.  La méthode <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> utilise ensuite automatiquement la clé de session pour déchiffrer l'élément XML.  
  
 Cet exemple convient quand plusieurs applications doivent partager des données chiffrées ou quand une application doit enregistrer des données chiffrées entre chaque exécution.  
  
### Pour déchiffrer un élément XML avec un certificat X.509  
  
1.  Créez un objet <xref:System.Xml.XmlDocument> en chargeant un fichier XML à partir du disque.  L'objet <xref:System.Xml.XmlDocument> contient l'élément XML à déchiffrer.  
  
     [!code-csharp[HowToDecryptXMLElementX509#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#2)]
     [!code-vb[HowToDecryptXMLElementX509#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#2)]  
  
2.  Créez un objet <xref:System.Security.Cryptography.Xml.EncryptedXml> en passant l'objet <xref:System.Xml.XmlDocument> au constructeur.  
  
     [!code-csharp[HowToDecryptXMLElementX509#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#3)]
     [!code-vb[HowToDecryptXMLElementX509#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#3)]  
  
3.  Déchiffrez le document XML à l'aide de la méthode <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A>.  
  
     [!code-csharp[HowToDecryptXMLElementX509#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#4)]
     [!code-vb[HowToDecryptXMLElementX509#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#4)]  
  
4.  Enregistrez l'objet <xref:System.Xml.XmlDocument>.  
  
     [!code-csharp[HowToDecryptXMLElementX509#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#5)]
     [!code-vb[HowToDecryptXMLElementX509#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#5)]  
  
## Exemple  
 Cet exemple suppose qu'un fichier nommé `"test.xml"` se trouve dans le même répertoire que le programme compilé.  Il suppose également que `"test.xml"` contient un élément `"creditcard"`.  Vous pouvez placer le code XML suivant dans un fichier appelé `test.xml` et l'utiliser avec cet exemple.  
  
```  
<root>  
    <creditcard>  
        <number>19834209</number>  
        <expiry>02/02/2002</expiry>  
    </creditcard>  
</root>  
  
```  
  
 [!code-csharp[HowToDecryptXMLElementX509#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#1)]
 [!code-vb[HowToDecryptXMLElementX509#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#1)]  
  
## Compilation du code  
  
-   Pour compiler cet exemple, vous devez inclure une référence à `System.Security.dll`.  
  
-   Incluez les espaces de noms suivants : <xref:System.Xml>, <xref:System.Security.Cryptography> et <xref:System.Security.Cryptography.Xml>.  
  
## Sécurité .NET Framework  
 Le certificat X.509 utilisé dans cet exemple sert à des fins de test uniquement.  Les applications doivent utiliser un certificat X.509 généré par une autorité de certification approuvée ou utiliser un certificat généré par le serveur de certificats Microsoft Windows.  
  
## Voir aussi  
 <xref:System.Security.Cryptography.Xml>   
 [Comment : chiffrer des éléments XML avec les certificats X.509](../../../docs/standard/security/how-to-encrypt-xml-elements-with-x-509-certificates.md)