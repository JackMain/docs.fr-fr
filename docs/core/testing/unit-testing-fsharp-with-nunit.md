---
title: "Effectuer des tests unitaires des bibliothèques F# dans .NET Core à l’aide de dotnet test et de NUnit"
description: "Apprenez les concepts des tests unitaires pour F# dans .NET Core de manière interactive en créant un exemple de solution pas à pas à l’aide de dotnet test et de NUnit."
author: rprouse
ms.date: 12/01/2017
ms.topic: article
dev_langs: fsharp
ms.prod: .net-core
ms.openlocfilehash: 27a7bb889fd736294252da39b1839b2197b8df03
ms.sourcegitcommit: 401c4427a3ec0d1263543033b3084039278509dc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2017
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-nunit"></a>Effectuer des tests unitaires des bibliothèques F# dans .NET Core à l’aide de dotnet test et de NUnit

Ce didacticiel vous guide pas à pas dans la création d’un exemple de solution pour apprendre les concepts des tests unitaires. Si vous préférez suivre le didacticiel à l’aide d’une solution prédéfinie, [affichez ou téléchargez l’exemple de code](https://github.com/dotnet/docs/tree/master/samples/core/getting-started/unit-testing-with-fsharp-nunit/) avant de commencer. Pour obtenir des instructions de téléchargement, consultez [Exemples et didacticiels](../../samples-and-tutorials/index.md#viewing-and-downloading-samples).

## <a name="creating-the-source-project"></a>Création du projet source

Ouvrez une fenêtre d’interpréteur de commandes. Créez un répertoire appelé *unit-testing-with-fsharp* qui contiendra la solution.
Dans ce nouveau répertoire, exécutez [`dotnet new sln`](../tools/dotnet-new.md) pour créer une solution. Ceci permet de simplifier la gestion de la bibliothèque de classes et du projet de test unitaire.
Dans le répertoire de la solution, créez un répertoire *MathService*. La structure du répertoire et des fichiers jusqu’ici est indiquée ci-dessous :

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

Accédez au répertoire *MathService* et exécutez [`dotnet new classlib -lang F#`](../tools/dotnet-new.md) pour créer le projet source.  Pour utiliser le développement piloté par les tests (TDD), vous devez créer une implémentation défaillante du service Math :

```fsharp
module MyMath =
    let sumOfSquares xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

Accédez de nouveau au répertoire *unit-testing-with-fsharp*. Exécutez [`dotnet sln add .\MathService\MathService.fsproj`](../tools/dotnet-sln.md) pour ajouter le projet de la bibliothèque de classes à la solution.

## <a name="install-the-nunit-project-template"></a>Installer le modèle de projet NUnit

Les modèles de projet de test NUnit doivent être installés avant de créer un projet de test. Cette opération ne doit être effectuée qu’une fois sur chaque ordinateur de développeur où vous allez créer des projets NUnit. Exécutez [`dotnet new -i NUnit3.DotNetNew.Template`](../tools/dotnet-new.md) pour installer les modèles NUnit.

 ```
 dotnet new -i NUnit3.DotNetNew.Template
 ```

## <a name="creating-the-test-project"></a>Création du projet de test

Ensuite, créez le répertoire *MathService.Tests*. La structure du répertoire est illustrée ci-dessous :

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

Accédez au répertoire *MathService.Tests* et créez un projet à l’aide de [`dotnet new nunit -lang F#`](../tools/dotnet-new.md). Vous obtenez un projet de test qui utilise NUnit comme framework de test. Le modèle généré configure le Test Runner dans *MathServiceTests.fsproj* :

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="NUnit" Version="3.9.0" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.9.0" />
</ItemGroup>
```

Le projet de test a besoin d’autres packages pour créer et exécuter des tests unitaires. `dotnet new` dans l’étape précédente a ajouté NUnit et l’adaptateur de test NUnit. Maintenant, ajoutez la bibliothèque de classes `MathService` en tant qu’une autre dépendance au projet. Utilisez la commande [`dotnet add reference`](../tools/dotnet-add-reference.md) :

```
dotnet add reference ../MathService/MathService.fsproj
```

Vous pouvez consulter le fichier dans son intégralité dans le [dépôt d’exemples](https://github.com/dotnet/docs/blob/master/samples/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj) sur GitHub.

La solution finale se présente comme suit :

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
        Test Source Files
        MathServiceTests.fsproj
```

Exécutez [`dotnet sln add .\MathService.Tests\MathService.Tests.fsproj`](../tools/dotnet-sln.md) dans le répertoire *unit-testing-with-fsharp*.

## <a name="creating-the-first-test"></a>Création du premier test

L’approche TDD impose d’écrire un test défaillant, de le corriger pour qu’il réussisse, puis de répéter le processus. Ouvrez *Tests.fs* et ajoutez le code suivant :

```fsharp
namespace MathService.Tests

open System
open NUnit.Framework
open MathService

[<TestFixture>]
type TestClass () =

    [<Test>]
    member this.TestMethodPassing() =
        Assert.True(true)

    [<Test>]
     member this.FailEveryTime() = Assert.True(false)
```

L’attribut `[<TestFixture>]` désigne une classe qui contient des tests. L’attribut `[<Test>]` désigne une méthode de test qui est exécutée par le Test Runner. À partir du répertoire *unit-testing-with-fsharp*, exécutez [`dotnet test`](../tools/dotnet-test.md) pour générer les tests et la bibliothèque de classes, puis exécutez les tests. Le Test Runner NUnit contient le point d’entrée de programme qui permet d’exécuter vos tests. `dotnet test` démarre le Test Runner à l’aide du projet de test unitaire que vous avez créé.

Ces deux tests illustrent les tests de réussite et d’échec les plus basiques. `My test` réussit et `Fail every time` échoue. À présent, créez un test pour la méthode `sumOfSquares`. La méthode `sumOfSquares` retourne la somme des carrés de toutes les valeurs de nombre entier impair qui font partie de la séquence d’entrée. Au lieu d’essayer d’écrire toutes ces fonctions simultanément, vous pouvez créer de manière itérative des tests qui valident les fonctionnalités. La réussite de chaque test correspond à la création de la fonctionnalité nécessaire pour la méthode.

Le test le plus simple que nous pouvons écrire consiste à appeler `sumOfSquares` avec tous les nombres pairs, où le résultat doit être une séquence vide de nombres entiers.  Voici ce test :

```fsharp
[<Test>]
member this.TestEvenSequence() =
    let expected = Seq.empty<int> |> Seq.toList
    let actual = MyMath.sumOfSquares [2; 4; 6; 8; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

Notez que la séquence `expected` a été convertie en liste. Le framework NUnit s’appuie sur de nombreux types .NET standard. Cette dépendance signifie que votre interface publique et les résultats attendus prennent en charge <xref:System.Collections.ICollection> plutôt que <xref:System.Collections.IEnumerable>.

Lorsque vous exécutez le test, vous constatez que votre test échoue. Vous n’avez pas encore créé l’implémentation. Effectuez ce test en écrivant le code le plus simple dans la classe `Mathservice` qui fonctionne :

```csharp
let sumOfSquares xs =
    Seq.empty<int> |> Seq.toList
```

Dans le répertoire *unit-testing-with-fsharp*, réexécutez `dotnet test`. La commande `dotnet test` exécute une build pour le projet `MathService` puis pour le projet `MathService.Tests`. Après la création des deux projets, il exécute ce test unique. Le test réussit.

## <a name="completing-the-requirements"></a>Finalisation des spécifications

Maintenant que vous avez fait réussir un test, le moment est venu d’écrire plus de code. Le cas simple suivant fonctionne avec une séquence dont le seul nombre impair est `1`. Le nombre 1 est plus facile, car le carré de 1 est 1. Voici ce test suivant :

```fsharp
[<Test>]
member public this.SumOnesAndEvens() =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.sumOfSquares [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

L’exécution de `dotnet test` fait échouer le nouveau test. Vous devez mettre à jour la méthode `sumOfSquares` pour gérer ce nouveau test. Vous devez filtrer tous les nombres pairs hors de la séquence pour que ce test réussisse. Pour ce faire, vous pouvez écrire une petite fonction de filtre et utiliser `Seq.filter` :

```fsharp
let private isOdd x = x % 2 <> 0

let sumOfSquares xs =
    xs
    |> Seq.filter isOdd
    |> Seq.toList
```

Notez l’appel à `Seq.toList`. Ceci crée une liste qui implémente l’interface <xref:System.Collections.ICollection>.

Encore une étape : calculer le carré de chaque nombre impair. Commencez par écrire un nouveau test :

```fsharp
[<Test>]
member public this.TestSquaresOfOdds() =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.sumOfSquares [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

Vous pouvez corriger le test en redirigeant la séquence filtrée via une opération de mappage pour calculer le carré de chaque nombre impair :

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let sumOfSquares xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
    |> Seq.toList
```

Vous avez créé une petite bibliothèque et un ensemble de tests unitaires pour cette bibliothèque. Vous avez structuré la solution afin que l’ajout de nouveaux packages et tests fasse partie du flux de travail normal. Vous avez concentré la plupart de votre temps et de vos efforts sur la résolution des objectifs de l’application.
