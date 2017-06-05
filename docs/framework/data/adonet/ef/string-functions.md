---
title: "Fonctions de cha&#238;ne | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: 338f0c26-8aee-43eb-bd1a-ec0849a376b9
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# Fonctions de cha&#238;ne
Le fournisseur de données .NET Framework pour SQL Server (SqlClient) propose des fonctions `String` qui effectuent des opérations sur une valeur d'entrée `String` et retournent une valeur de résultat `String` ou numérique. Ces fonctions se trouvent dans l'espace de noms SqlServer, lequel est disponible lorsque vous utilisez SqlClient. La propriété d'espace de noms d'un fournisseur permet à Entity Framework de découvrir le préfixe attribué par ce fournisseur à des constructions spécifiques, telles que des types et des fonctions.  
  
 Le tableau suivant présente les fonctions `String` SqlClient.  
  
|Fonction|Description|  
|--------------|-----------------|  
|`ASCII(` `expression` `)`|Retourne la valeur du code ASCII du caractère placé le plus à gauche d'une expression de chaîne.<br /><br /> **Arguments**<br /><br /> `expression` : toute expression valide d'un type `String` ASCII.<br /><br /> **Valeur de retour**<br /><br /> Élément `Int32`.<br /><br /> **Exemple**<br /><br /> `SqlServer.ASCII('A')`|  
|`CHAR(` `expression` `)`|Convertit un code `Int32` en chaîne (String) ASCII.<br /><br /> **Arguments**<br /><br /> `expression` : valeur `Int32`.<br /><br /> **Valeur de retour**<br /><br /> `String` ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.char(97)`|  
|`CHARINDEX(` `expression1, expression2` [, `start_location`]`)`|Retourne la position de départ, dans une chaîne de caractères, de l'expression spécifiée.<br /><br /> **Arguments**<br /><br /> `expression1` : expression qui contient la séquence de caractères à rechercher. L'expression peut être de type chaîne (String, ASCII ou Unicode) ou de type binaire (Binary).<br /><br /> `expression2` : expression, généralement une colonne, dans laquelle rechercher la séquence spécifiée. L'expression peut être de type chaîne (String, ASCII ou Unicode) ou de type binaire (Binary).<br /><br /> `start_location`: (Facultatif) valeur Int64 (non retournée dans SQL Server 2000) ou Int32 qui représente la position de caractère à partir de laquelle rechercher expression1 dans expression2. Si start_location n'est pas spécifié ou s'il s'agit d'un nombre négatif ou égal à zéro, la recherche commence au début d'expression2.<br /><br /> **Valeur de retour**<br /><br /> Élément `Int32`.<br /><br /> **Exemple**<br /><br /> `SqlServer.CHARINDEX('h', 'habcdefgh', 2)`|  
|`DIFFERENCE(` `expression, expression` `)`|Compare les valeurs `SOUNDEX` de deux chaînes et évalue leur ressemblance.<br /><br /> **Arguments**<br /><br /> Type `String` ASCII ou Unicode. `expression` peut être une constante, une variable ou une colonne.<br /><br /> **Valeur de retour**<br /><br /> Retourne un `Int32` qui représente la différence entre les valeurs SOUNDEX de deux expressions de caractères. La plage est comprise entre 0 et 4. 0 indique une similarité nulle ou faible, et 4 indique une forte similarité ou des valeurs identiques.<br /><br /> **Exemple**<br /><br /> `// The following example returns a DIFFERENCE value of 4,`<br /><br /> `//the least possible difference or the best match.`<br /><br /> `SqlServer.DIFFERENCE('Green','Greene');`|  
|`LEFT(` `expression, count` `)`|Retourne la partie de gauche d'une chaîne de caractères avec le nombre spécifié de caractères.<br /><br /> **Arguments**<br /><br /> `expression` : type String Unicode ou ASCII. Utilisez la fonction CAST pour convertir explicitement character_expression.<br /><br /> `count` : type `Int64` (non retourné dans SQL Server 2000) ou `Int32` qui spécifie le nombre de caractères de character_expression à retourner.<br /><br /> **Valeur de retour**<br /><br /> Valeur `String` Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.LEFT('SQL Server', 4)`|  
|`LEN(` `expression` `)`|Retourne le nombre de caractères de l'expression de type String spécifiée, à l'exception des espaces de fin.<br /><br /> **Arguments**<br /><br /> `expression` : expression d'un type `String` (Unicode ou ASCII) ou d'un type `Binary`.<br /><br /> **Valeur de retour**<br /><br /> Élément `Int32`.<br /><br /> **Exemple**<br /><br /> `SqlServer.LEN('abcd')`|  
|`LOWER(` `expression` `)`|Retourne une expression de type `String` après avoir converti les caractères majuscules en caractères minuscules.<br /><br /> **Arguments**<br /><br /> `expression`: toute expression valide du type `String`.<br /><br /> **Valeur de retour**<br /><br /> `String`<br /><br /> **Exemple**<br /><br /> `SqlServer.LOWER('AbB')`|  
|`LTRIM(` `expression` `)`|Retourne une expression `String` après avoir supprimé les espaces de début.<br /><br /> **Arguments**<br /><br /> `expression` : toute expression valide de type `String`.<br /><br /> **Valeur de retour**<br /><br /> `String`<br /><br /> **Exemple**<br /><br /> `SqlServer.LTRIM('   d')`|  
|`NCHAR(` `expression` `)`|Retourne une chaîne (`String`) Unicode avec le code d'entier spécifié, tel que défini dans la norme Unicode.<br /><br /> **Arguments**<br /><br /> `expression` : valeur `Int32`.<br /><br /> **Valeur de retour**<br /><br /> `String` Unicode.<br /><br /> **Exemple**<br /><br /> `SqlServer.NCHAR(65)`|  
|`PATINDEX(` `'%pattern%'`, `expression``)`|Retourne la position de départ de la première occurrence d'un modèle dans une expression `String` spécifiée.<br /><br /> **Arguments**<br /><br /> `'%pattern%'` : type `String` ASCII ou Unicode. Vous pouvez utiliser des caractères génériques ; toutefois, le caractère % doit précéder et suivre le modèle (sauf lors de recherches des premiers ou derniers caractères).<br /><br /> `expression` : `String` ASCII ou Unicode où rechercher le modèle spécifié.<br /><br /> **Valeur de retour**<br /><br /> Élément `Int32`.<br /><br /> **Exemple**<br /><br /> `SqlServer.PATINDEX('abc', 'ab')`|  
|`QUOTENAME(` `'char_string'` [, '`quote_char'`]`)`|Retourne une chaîne (`String`) Unicode avec les délimiteurs ajoutés afin que la chaîne d'entrée soit un identificateur délimité SQL Server 2005 valide.<br /><br /> **Arguments**<br /><br /> `char_string` : chaîne `String` Unicode.<br /><br /> `quote_char` : chaîne d'un seul caractère à utiliser en tant que délimiteur. Il peut s'agir d'une apostrophe ( ' ), d'un crochet gauche ou droit ( [ ] ) ou d'un guillemet double ( " ). Si `quote_char` n'est pas spécifié, les crochets sont utilisés.<br /><br /> **Valeur de retour**<br /><br /> `String` Unicode.<br /><br /> **Exemple**<br /><br /> `SqlServer.QUOTENAME('abc[]def')`|  
|`REPLACE(` `expression1`, `expression2, expression3``)`|Répète une expression de caractères le nombre de fois indiqué.<br /><br /> **Arguments**<br /><br /> `expression1` : expression de chaîne à rechercher. string_expression1 peut être d'un type String Unicode ou ASCII.<br /><br /> `expression2`: La sous-chaîne à rechercher. string_expression2 peut être d'un type String Unicode ou ASCII.<br /><br /> `expression3` : la chaîne de remplacement. string_expression3 peut être d'un type String Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.REPLACE('aabbcc', 'bc', 'zz')`|  
|`REPLICATE(``char_expression`, int_`expression``)`|Répète une expression de caractères le nombre de fois indiqué.<br /><br /> **Arguments**<br /><br /> `char_expression` : type `String` Unicode ou ASCII.<br /><br /> `int_expression` : valeur `Int64` (non prise en charge dans SQL Server 2000) ou `Int32`.<br /><br /> **Valeur de retour**<br /><br /> Type `String` Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.REPLICATE('aa',2)`|  
|`REVERSE(` `expression` `)`|Retourne des données de type String Unicode ou ASCII dans une chaîne reprenant dans l'ordre inverse les caractères de la chaîne d'entrée.<br /><br /> **Arguments**<br /><br /> `expression` : type `String` Unicode ou ASCII.<br /><br /> **Valeur de retour**<br /><br /> Type `String` Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.REVERSE('abcd')`|  
|`RIGHT(` `char_expression`, `count``)`|Retourne la partie de droite d'une chaîne de caractères avec le nombre spécifié de caractères.<br /><br /> **Arguments**<br /><br /> `char_expression`: Un type String Unicode ou ASCII. Utilisez la fonction CAST pour convertir explicitement character_expression.<br /><br /> `count` : type `Int64` (non retourné dans SQL Server 2000) ou `Int32` qui spécifie le nombre de caractères de character_expression à retourner.<br /><br /> **Valeur de retour**<br /><br /> Type `String` ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.RIGHT('SQL Server', 6)`|  
|`RTRIM(` `expression` `)`|Retourne une valeur String Unicode ou ASCII après la suppression des espaces de fin.<br /><br /> **Arguments**<br /><br /> `expression` : type `String` Unicode ou ASCII.<br /><br /> **Valeur de retour**<br /><br /> Type `String` Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.RTRIM('   d   e      ')`|  
|`SOUNDEX(` `expression` `)`|Retourne un code de quatre-caractères (SOUNDEX) pour évaluer la similarité entre deux chaînes. **Arguments**<br /><br /> `expression` : type String Unicode ou ASCII.<br /><br /> **Valeur de retour**<br /><br /> `String` ASCII. Un code de quatre caractères (SOUNDEX) est une chaîne qui évalue la similitude entre deux chaînes.<br /><br /> **Exemple**<br /><br /> `Select SqlServer.SOUNDEX('Smith'), SqlServer.SOUNDEX('Smythe') FROM {1}`<br /><br /> **Retourne**<br /><br /> `----- -----  S530  S530`|  
|`SPACE(` `int_expression` `)`|Retourne une chaîne `String` ASCII composée d'espaces consécutifs.<br /><br /> **Arguments**<br /><br /> `int_expression` : valeur `Int64` (non retournée dans SQL Server 2000) ou `Int32` qui indique le nombre d'espaces.<br /><br /> **Valeur de retour**<br /><br /> `String` ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.SPACE(2)`|  
|`STR(` `float_expression` [, `length` [, `decimal`]]`)`|Retourne une valeur de type `String` ASCII convertie à partir de données numériques.<br /><br /> **Arguments**<br /><br /> `float _expression` : expression de type de données (`Double`) numérique approché avec virgule décimale.<br /><br /> `length` : (facultatif) valeur `Int32` qui représente la longueur totale. Inclut le séparateur décimal, le signe, les chiffres et les espaces. La valeur par défaut est 10.<br /><br /> `decimal`: (facultatif) un `Int32` qui représente le nombre de décimales à droite de la virgule décimale. decimal doit être inférieur ou égal à 16. Si decimal est supérieur à 16, le résultat est tronqué à la seizième décimale à droite du séparateur décimal.<br /><br /> **Valeur de retour**<br /><br /> `String` ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.STR(212.0)`|  
|`STUFF(` `str_expression`, `start, length`, `str_expression_to_insert``)`|Supprime une longueur spécifique de caractères et insère un autre jeu de caractères à un point de départ spécifié dans une expression de chaîne.<br /><br /> **Arguments**<br /><br /> `str_expression` : chaîne `String` Unicode ou ASCII.<br /><br /> `start:`Un `Int64` (non retourné dans SQL Server 2000) ou `Int32` valeur qui spécifie la position de départ de la suppression et l’insertion.<br /><br /> `length`: valeur `Int64` (non retournée dans SQL Server 2000) ou`Int32` qui spécifie le nombre de caractères à supprimer.<br /><br /> `str_expression_to_insert` : chaîne `String` Unicode ou ASCII.<br /><br /> **Valeur de retour**<br /><br /> Valeur `String` Unicode ou ASCII.<br /><br /> **Exemple**<br /><br /> `SqlServer.STUFF('abcd', 2, 2, 'zz')`|  
|`SUBSTRING(` `str_expression`, `start, length``)`|Retourne une partie d'une expression `String`.<br /><br /> **Arguments**<br /><br /> `str_expression` : expression d'un type `String` (Unicode ou ASCII) ou d'un type `Binary`.<br /><br /> `start` : valeur `Int64` (non retournée dans SQL Server 2000) ou `Int32` qui indique où commence la sous-chaîne. 1 fait référence au premier caractère de la chaîne.<br /><br /> `length` : valeur `Int64` (non retournée dans SQL Server 2000) ou `Int32` qui spécifie le nombre de caractères de l'expression à retourner.<br /><br /> **Valeur de retour**<br /><br /> Type `String` (ASCII ou Unicode) ou type `Binary`.<br /><br /> **Exemple**<br /><br /> `SqlServer.SUBSTRING('abcd', 2, 2)`|  
|`UNICODE(` `expression` `)`|Retourne la valeur entière, telle que définie par la norme Unicode, pour le premier caractère de l'expression d'entrée.<br /><br /> **Arguments**<br /><br /> `expression` : chaîne `String` Unicode.<br /><br /> **Valeur de retour**<br /><br /> Élément `Int32`.<br /><br /> **Exemple**<br /><br /> `SqlServer.UNICODE('a')`|  
|`UPPER(` `expression` `)`|Retourne une expression `String` après conversion en majuscules des données en caractères minuscules.<br /><br /> **Arguments**<br /><br /> `expression` : expression d'un type String ASCII ou Unicode.<br /><br /> **Valeur de retour**<br /><br /> Type `String` ASCII ou Unicode.<br /><br /> **Exemple**<br /><br /> `SqlServer.UPPER('AbB')`|  
  
 Pour plus d'informations sur les fonctions `String` prises en charge par SqlClient, voir la documentation correspondant à la version de SQL Server que vous avez spécifiée dans le manifeste du fournisseur SqlClient :  
  
|SQL Server 2000|SQL Server 2005|SQL Server 2008|  
|---------------------|---------------------|---------------------|  
|[Fonctions de chaîne (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=115915)|[Fonctions de chaîne (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=115916)|[Fonctions de chaîne (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=115914)|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions SqlClient pour Entity Framework](../../../../../docs/framework/data/adonet/ef/sqlclient-for-ef-functions.md)   
 [Problèmes connus dans SqlClient pour Entity Framework](../../../../../docs/framework/data/adonet/ef/known-issues-in-sqlclient-for-entity-framework.md)