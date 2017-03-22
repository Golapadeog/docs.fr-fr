---
title: "Types de données composites (Visual Basic) | Documents Microsoft"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- classes [Visual Basic], composite data types
- composite types
- composite data types
- data types [Visual Basic], composite
- arrays [Visual Basic], composite data types
- structures, composite data types
- classes [Visual Basic], composite types
- types [Visual Basic], composite
ms.assetid: 62970f2e-52c0-4369-8963-613820f1f434
caps.latest.revision: 19
author: stevehoag
ms.author: shoag
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: d81b2c08155cb16754e780fdfb341b596322302d
ms.lasthandoff: 03/13/2017

---
# <a name="composite-data-types-visual-basic"></a>Types de données composites (Visual Basic)
Outre les types de données élémentaires [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] fournit, vous pouvez assembler des éléments de différents types pour créer *types de données composites* tels que des structures, des tableaux et des classes. Vous pouvez créer des types de données composites à partir des types élémentaires et d’autres types composites. Par exemple, vous pouvez définir un tableau d’éléments de structure ou une structure avec des membres de tableau.  
  
## <a name="data-types"></a>Types de données  
 Un type composite diffère du type de données d’un de ses composants. Par exemple, un tableau de `Integer` éléments n’est pas de la `Integer` type de données.  
  
 Un type de données de tableau est représenté normalement à l’aide du type d’élément, parenthèses et des virgules si nécessaire. Par exemple, un tableau unidimensionnel de `String` éléments est représenté en tant que `String()`, un tableau à deux dimensions de `Boolean` éléments est représenté en tant que `Boolean(,)`.  
  
## <a name="structure-types"></a>Types de structure  
 Il n’existe aucun type de données unique comprenant toutes les structures. Au lieu de cela, chaque définition d’une structure représente un type de données unique, même si deux structures définissent des éléments identiques dans le même ordre. Toutefois, si vous créez deux ou plusieurs instances de la même structure, [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] les considère comme du même type de données.  
  
## <a name="array-types"></a>Types tableau  
 Il n’existe aucun type de données englobant tous les tableaux. Le type de données d’une instance particulière d’un tableau est déterminé par le texte suivant :  
  
-   Le fait d’être un tableau  
  
-   Le rang (nombre de dimensions) du tableau  
  
-   Le type d’élément du tableau  
  
 En particulier, la longueur d’une dimension donnée n’est pas partie du type de données de l’instance. L'exemple suivant illustre ce comportement.  
  
```  
Dim arrayA( ) As Byte = New Byte(12) {}  
Dim arrayB( ) As Byte = New Byte(100) {}  
Dim arrayC( ) As Short = New Short(100) {}  
Dim arrayD( , ) As Short  
Dim arrayE( , ) As Short = New Short(4, 10) {}  
```  
  
 Dans l’exemple précédent, les variables tableau `arrayA` et `arrayB` sont considérés comme étant du même type de données : `Byte()` , même si elles sont initialisées avec des longueurs différentes. Variables `arrayB` et `arrayC` ne sont pas du même type car leurs types d’éléments sont différents. Variables `arrayC` et `arrayD` ne sont pas du même type car leurs rangs sont différents. Variables `arrayD` et `arrayE` ont le même type — `Short(,)` , car leurs rangs et types d’élément sont identiques, même si `arrayD` n’est pas encore initialisé.  
  
 Pour plus d’informations sur les tableaux, consultez [tableaux](../../../../visual-basic/programming-guide/language-features/arrays/index.md).  
  
## <a name="class-types"></a>Types de classes  
 Il n’existe aucun type de données unique comprenant toutes les classes. Bien qu’une classe peut hériter d’une autre classe, chacune est un type de données distinct. Plusieurs instances de la même classe sont du même type de données. Si vous assignez une variable d’instance de classe à une autre, non seulement font qu’ils ont les mêmes type de données, ils pointent vers la même instance de classe en mémoire.  
  
 Pour plus d’informations sur les classes, consultez [objets et Classes](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données](../../../../visual-basic/programming-guide/language-features/data-types/index.md)   
 [Types de données élémentaires](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)   
 [Types génériques dans Visual Basic](../../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)   
 [Types valeur et Types référence](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)   
 [Conversions de type dans Visual Basic](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)   
 [Structures](../../../../visual-basic/programming-guide/language-features/data-types/structures.md)   
 [Dépannage des Types de données](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)   
 [Guide pratique : stocker plusieurs valeurs dans une variable](../../../../visual-basic/programming-guide/language-features/data-types/how-to-hold-more-than-one-value-in-a-variable.md)