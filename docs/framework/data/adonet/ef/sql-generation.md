---
title: "Génération SQL"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 0e16aa02-d458-4418-a765-58b42aad9315
caps.latest.revision: "2"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: 6b2d4ba925af61f89b47c494f5fb17f5f9f0d995
ms.sourcegitcommit: ed26cfef4e18f6d93ab822d8c29f902cff3519d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="sql-generation"></a>Génération SQL
Lorsque vous écrivez un fournisseur pour [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)], vous devez traduire des arborescences de commandes [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] dans un langage SQL qu'une base de données spécifique puisse comprendre, tel que Transact-SQL pour SQL Server ou PL/SQL pour Oracle. Dans cette section, vous allez apprendre à développer un composant de génération SQL (pour les requêtes SELECT) pour un fournisseur [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]. Pour plus d’informations sur les insérer, mettre à jour et supprimer des requêtes, consultez [génération SQL de Modification](../../../../../docs/framework/data/adonet/ef/modification-sql-generation.md).  
  
 Pour comprendre cette section, vous devez être familiarisé avec [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] et le modèle de fournisseur ADO.NET. Vous devez également comprendre les arborescences de commandes et les <xref:System.Data.Common.CommandTrees.DbExpression>.  
  
## <a name="the-role-of-the-sql-generation-module"></a>Rôle du module de génération SQL  
 Le module de génération SQL d'un fournisseur [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] traduit une arborescence de commandes d'une requête donnée en instruction SQL SELECT unique qui cible une base de données conforme SQL:1999. Le SQL généré doit avoir aussi peu de requêtes imbriquées que possible. Le module de génération SQL ne doit pas simplifier l'arborescence de commandes de requête de sortie. [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] le fera, par exemple en éliminant des jointures et en réduisant des nœuds de filtre consécutifs.  
  
 La classe <xref:System.Data.Common.DbProviderServices> est le point de départ permettant d'accéder à la couche de génération SQL pour convertir des arborescences de commandes en <xref:System.Data.Common.DbCommand>.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Forme des arborescences de commandes](../../../../../docs/framework/data/adonet/ef/the-shape-of-the-command-trees.md)  
  
 [Génération SQL à partir d’arborescences de commandes - Bonnes pratiques](../../../../../docs/framework/data/adonet/ef/generating-sql-from-command-trees-best-practices.md)  
  
 [Génération SQL dans l’exemple de fournisseur](../../../../../docs/framework/data/adonet/ef/sql-generation-in-the-sample-provider.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Écriture d’un fournisseur de données Entity Framework](../../../../../docs/framework/data/adonet/ef/writing-an-ef-data-provider.md)
