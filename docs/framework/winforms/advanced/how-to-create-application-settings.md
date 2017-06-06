---
title: "Comment&#160;: cr&#233;er des param&#232;tres d&#39;application | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "paramètres d'application, créer"
  - "paramètres d'application, Windows Forms"
ms.assetid: 1e7aa347-af75-41e5-89ca-f53cab704f72
caps.latest.revision: 13
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 13
---
# Comment&#160;: cr&#233;er des param&#232;tres d&#39;application
À l'aide de code managé, vous pouvez créer des paramètres d'application et les lier à des propriétés ou des contrôles sur votre formulaire, pour que ces paramètres soient chargés et enregistrés automatiquement au moment de l'exécution.  
  
 Dans la procédure suivante, vous allez créer manuellement une classe wrapper qui dérive de <xref:System.Configuration.ApplicationSettingsBase>.  Vous ajouterez à cette classe une propriété accessible publiquement pour chaque paramètre d'application que vous souhaitez exposer.  
  
 Vous pouvez également appliquer cette procédure avec un minimum de code dans le concepteur Visual Studio.  Consultez également [Comment : créer des paramètres d'application à l'aide du concepteur](http://msdn.microsoft.com/library/wabtadw6%20\(v=vs.110\)).  
  
### Pour créer des paramètres d'application par programmation  
  
1.  Ajoutez une nouvelle classe à votre projet et renommez\-la.  Pour cette procédure, nous appellerons cette classe  `MyUserSettings`.  Modifiez la définition de la classe pour qu'elle dérive de <xref:System.Configuration.ApplicationSettingsBase>.  
  
2.  Définissez une propriété sur cette classe wrapper pour chaque paramètre d'application nécessaire et appliquez cette propriété avec le <xref:System.Configuration.ApplicationScopedSettingAttribute> ou le <xref:System.Configuration.UserScopedSettingAttribute>, selon la portée du paramètre.  Pour plus d'informations sur la portée des paramètres, consultez [Vue d'ensemble des paramètres d'application](../../../../docs/framework/winforms/advanced/application-settings-overview.md).  Votre code doit maintenant ressembler à ce qui suit :  
  
     [!code-csharp[ApplicationSettings.Create#1](../../../../samples/snippets/csharp/VS_Snippets_Winforms/ApplicationSettings.Create/CS/MyAppSettings.cs#1)]
     [!code-vb[ApplicationSettings.Create#1](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/ApplicationSettings.Create/VB/MyAppSettings.vb#1)]  
  
3.  Créez une instance de cette classe wrapper dans votre application.  Il s'agira généralement d'un membre privé du formulaire principal.  Maintenant que vous avez défini votre classe, vous devez la lier à une propriété. Ici, il s'agit de la propriété <xref:System.Windows.Forms.Form.BackColor%2A> de votre formulaire.  Vous pouvez effectuer cette opération dans le gestionnaire d'événements  `Load`  de votre formulaire.  
  
     [!code-csharp[ApplicationSettings.Create#2](../../../../samples/snippets/csharp/VS_Snippets_Winforms/ApplicationSettings.Create/CS/Form1.cs#2)]
     [!code-vb[ApplicationSettings.Create#2](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/ApplicationSettings.Create/VB/Form1.vb#2)]  
  
4.  Si vous proposez une manière de modifier les paramètres au moment de l'exécution, vous devrez enregistrer les paramètres actuels de l'utilisateur sur disque lors de la fermeture de votre formulaire, sinon ces modifications seront perdues.  
  
     [!code-csharp[ApplicationSettings.Create#3](../../../../samples/snippets/csharp/VS_Snippets_Winforms/ApplicationSettings.Create/CS/Form1.cs#3)]
     [!code-vb[ApplicationSettings.Create#3](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/ApplicationSettings.Create/VB/Form1.vb#3)]  
  
     Vous avez créé un paramètre d'application et vous l'avez lié à la propriété spécifiée.  
  
## Sécurité .NET Framework  
 Le fournisseur de paramètres par défaut, <xref:System.Configuration.LocalFileSettingsProvider>, conserve les informations dans des fichiers de configuration en texte brut.  Cela limite la sécurité à la sécurité d'accès de fichier fournie par le système d'exploitation pour l'utilisateur actuel.  Pour cette raison, vous devez faire attention aux informations stockées dans les fichiers de configuration.  Par exemple, l'une des utilisations courantes des paramètres d'application consiste à stocker des chaînes de connexion qui pointent vers le magasin de données de l'application.  Toutefois, pour des raisons de sécurité, ces chaînes ne doivent pas inclure de mots de passe.  Pour plus d'informations sur les chaînes de connexion, consultez <xref:System.Configuration.SpecialSetting>.  
  
## Voir aussi  
 <xref:System.Configuration.SpecialSettingAttribute>   
 <xref:System.Configuration.LocalFileSettingsProvider>   
 [Vue d'ensemble des paramètres d'application](../../../../docs/framework/winforms/advanced/application-settings-overview.md)   
 [Comment : valider des paramètres d'application](../../../../docs/framework/winforms/advanced/how-to-validate-application-settings.md)