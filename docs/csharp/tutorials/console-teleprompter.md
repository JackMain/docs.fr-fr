---
title: Application console
description: "Ce didacticiel vous présente un certain nombre de fonctionnalités dans .NET Core et le langage C#."
keywords: .NET, .NET Core
author: BillWagner
ms.author: wiwagn
ms.date: 03/06/2017
ms.topic: article
ms.prod: .net-core
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 883cd93d-50ce-4144-b7c9-2df28d9c11a0
ms.openlocfilehash: 08dab8e7b210ab5159645563cd381d50145d764b
ms.sourcegitcommit: be7862cac09066bc505586cbf071d0e2c8fb1508
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2017
---
# <a name="console-application"></a>Application console

## <a name="introduction"></a>Introduction
Ce didacticiel vous présente un certain nombre de fonctionnalités de .NET Core et du langage C#. Vous apprendrez à :
*   Principes de base de l’interface de ligne de commande (CLI) de .NET Core
*   La structure d’une application de console C#
*   E/S console
*   Les principes fondamentaux de l’API d’E/S de fichier dans .NET Core
*   Les principes fondamentaux du modèle de programmation asynchrone des tâches dans .NET Core

Vous allez générer une application qui lit un fichier texte et retourne le contenu du fichier texte dans la console. La sortie sur la console se fera à un rythme permettant de la lire à haute voix. Vous pouvez accélérer ou ralentir la vitesse en appuyant sur les touches '<’ ou ’>'.

Il existe un grand nombre de fonctionnalités dans ce didacticiel. Nous allons les construire une par une. 
## <a name="prerequisites"></a>Conditions préalables
Vous devez configurer votre ordinateur pour exécuter .NET Core. Vous trouverez les instructions d’installation sur la page de [.NET Core](https://www.microsoft.com/net/core). Vous pouvez exécuter cette application sur Windows, Linux, Mac OS ou dans un conteneur Docker. Vous devez installer l’éditeur de code de votre choix. 
## <a name="create-the-application"></a>Création de l’application
La première étape consiste à créer une nouvelle application. Ouvrez une invite de commandes et créez un nouveau répertoire pour votre application. Réglez-le comme répertoire actuel. Saisissez la commande `dotnet new console` à l’invite. Elle crée les fichiers de démarrage d’une application « Hello World » de base.

Avant d’apporter des modifications, examinons les étapes nécessaires pour exécuter l’application simple Hello World. Après avoir créé l’application, tapez `dotnet restore` ([voir la Remarque](#dotnet-restore-note)) à l’invite de commandes. Cette commande exécute le processus de restauration de package NuGet. NuGet est un gestionnaire de packages .NET. Cette commande télécharge les dépendances manquantes pour votre projet. Comme il s’agit d’un nouveau projet, aucune des dépendances n’est en place, donc la première exécution téléchargera le framework .NET Core. Après cette étape initiale, vous devez uniquement exécuter `dotnet restore` ([voir la Remarque](#dotnet-restore-note)) lorsque vous ajoutez de nouveaux packages dépendants ou mettre à jour les versions de vos dépendances. Ce processus crée également le fichier de verrouillage du projet (project.lock.json) dans votre répertoire de projet. Ce fichier permet de gérer les dépendances du projet. Il contient l’emplacement local de toutes les dépendances du projet. Vous n’avez pas besoin de créer le fichier de contrôle de code source ; Il est généré lorsque vous exécutez `dotnet restore` ([voir la Remarque](#dotnet-restore-note)). 

Après la restauration des packages, vous exécutez `dotnet build`. Cela exécute le moteur de génération et crée l’exécutable de votre application. Enfin, vous exécutez `dotnet run` pour lancer votre application.  

Le code d’application Hello World simple se trouve intégralement dans le fichier Program.cs. Ouvrez ce fichier avec votre éditeur de texte préféré. Nous allons apporter nos premières modifications.
En haut du fichier, remarquez l’instruction using :

```csharp
using System;
```

Cette instruction indique au compilateur que tous les types de l’espace de noms `System` sont dans la portée. Comme d’autres langages orientés objet que vous avez peut-être utilisés, C# utilise des espaces de noms pour organiser les types. Ce programme hello world ne fait pas exception. Vous pouvez voir que le programme est placé dans l’espace de noms `ConsoleApplication`. Ce qui n’est pas un nom très descriptif, donc modifiez-le en `TeleprompterConsole` :

```csharp
namespace TeleprompterConsole
```

## <a name="reading-and-echoing-the-file"></a>Lecture et affichage du fichier
La première fonctionnalité à ajouter est la capacité à lire un fichier texte et à afficher tout le texte dans la console. Tout d’abord, nous allons ajouter un fichier texte. Copiez le fichier [sampleQuotes.txt](https://raw.githubusercontent.com/dotnet/docs/master/samples/csharp/getting-started/console-teleprompter/sampleQuotes.txt) à partir du dépôt GitHub pour cet [exemple](https://github.com/dotnet/docs/tree/master/samples/csharp/getting-started/console-teleprompter) dans votre répertoire de projet. Il servira de script pour votre application. Si vous voulez des informations sur la façon de télécharger l’exemple d’application de cette rubrique, consultez les instructions de la rubrique [Exemples et didacticiels](../../samples-and-tutorials/index.md#viewing-and-downloading-samples).

Ensuite, ajoutez la méthode suivante dans votre classe Program (juste en dessous de la méthode `Main`) :

```csharp
static IEnumerable<string> ReadFrom(string file)
{
    string line;
    using (var reader = File.OpenText(file))
    {
        while ((line = reader.ReadLine()) != null)
        {
            yield return line;
        }
    }
}
```

Cette méthode utilise les types de deux nouveaux espaces de noms. Pour que cela compile, vous devez ajouter les deux lignes suivantes au début du fichier :

```csharp
using System.Collections.Generic;
using System.IO;
```

L'interface `IEnumerable<T>` est définie dans l’espace de noms `System.Collections.Generic`. La classe <xref:System.IO.File> est définie dans l’espace de noms `System.IO`.

Cette méthode est un type spécial de méthode C# appelé *méthode d’énumérateur*. Les méthodes d’énumérateur retournent des séquences qui sont évaluées de manière tardive. Cela signifie que chaque élément de la séquence est généré lorsque cela est demandé par le code utilisant la séquence. Les méthodes d’énumérateur sont des méthodes qui contiennent une ou plusieurs instructions `yield return`. L’objet retourné par la méthode `ReadFrom` contient le code pour générer chaque élément dans la séquence. Dans cet exemple, cela implique la lecture de la ligne de texte suivante à partir du fichier source et le renvoi de cette chaîne. Chaque fois que le code appelant demande l’élément suivant de la séquence, le code lit la ligne suivante du texte à partir du fichier et la renvoie. Lorsque le fichier a été entièrement lu, la séquence indique qu’il n’y a pas d’autres éléments.

Il existe deux autres éléments de syntaxe de C# qui peuvent être nouveaux pour vous. L’instruction `using` dans cette méthode gère le nettoyage des ressources. La variable initialisée dans l’instruction `using` (`reader`, dans cet exemple) doit implémenter l’interface `IDisposable`. L’interface <xref:System.IDisposable> définit une méthode unique, `Dispose`, qui doit être appelée lorsque les ressources doivent être libérées. Le compilateur génère l’appel lorsque l’exécution atteint l’accolade fermante de l’instruction `using`. Le code généré par le compilateur garantit que la ressource est libérée même si une exception est levée à partir du code dans le bloc défini par l’instruction using.

La variable `reader` est définie à l’aide du mot-clé `var`. `var` définit une *variable locale implicitement typée*. Cela signifie que le type de la variable est déterminé par le type au moment de la compilation de l’objet assigné à la variable. Ici, qui est la valeur de retour à partir de la <xref:System.IO.File.OpenText(System.String)> (méthode), qui est un <xref:System.IO.StreamReader> objet.
 
À présent, nous allons remplir le code pour lire le fichier dans la méthode `Main` : 

```csharp
var lines = ReadFrom("sampleQuotes.txt");
foreach (var line in lines)
{
    Console.WriteLine(line); 
}
```

Exécutez le programme (à l’aide de `dotnet run`, chaque ligne est imprimée sur la console).  

## <a name="adding-delays-and-formatting-output"></a>Ajout de délais et de mise en forme à la sortie
Ce que vous avez s’affiche beaucoup trop rapidement pour le lire à haute voix. Vous devez maintenant ajouter des délais à la sortie. Lorsque vous commencez, vous créez une partie du code principal qui permet le traitement asynchrone. Toutefois, ces premières étapes suivront quelques anti-modèles. Les anti-modèles sont signalés dans les commentaires lorsque vous ajoutez le code, et le code sera actualisé ultérieurement.

Il existe deux étapes dans cette section. Tout d’abord, vous allez mettre à jour la méthode d’itérateur pour retourner des mots uniques au lieu de lignes entières. Vous le faites avec ces modifications. Remplacez l’instruction `yield return line;` par le code suivant :

```csharp
var words = line.Split(' ');
foreach (var word in words)
{
    yield return word + " ";
}
yield return Environment.NewLine;
```

Ensuite, vous devez modifier la façon dont vous consommez les lignes du fichier, et ajoutez un délai après chaque mot. Remplacez l’instruction `Console.WriteLine(line)` dans la méthode `Main` avec le bloc suivant :

```csharp
Console.Write(line);
if (!string.IsNullOrWhiteSpace(line))
{
    var pause = Task.Delay(200);
    // Synchronously waiting on a task is an
    // anti-pattern. This will get fixed in later
    // steps.
    pause.Wait();
}
```

La classe `Task` se trouve dans l’espace de noms `System.Threading.Tasks`, vous devez donc ajouter cette instruction `using` au début du fichier :

```csharp
using System.Threading.Tasks;
```

Exécutez l’exemple et vérifiez le résultat. À présent, chaque mot unique est affiché séparément, suivi d’un délai de 200 ms. Toutefois, la sortie affichée présente certains problèmes, car le fichier texte source possède plusieurs lignes qui ont plus de 80 caractères sans saut de ligne. Ce qui peut être difficile à lire lors du défilement. Ce problème est facile à résoudre. Vous allez simplement effectuer le suivi de la longueur de chaque ligne et générer une nouvelle ligne chaque fois que la longueur de ligne atteint un certain seuil. Déclarez une variable locale après la déclaration de `words` qui contient la longueur de ligne :

```csharp
var lineLength = 0;
```
 
Ensuite, ajoutez le code suivant après l’instruction `yield return word + " ";` (avant l’accolade fermante) :

```csharp
lineLength += word.Length + 1;
if (lineLength > 70)
{
    yield return Environment.NewLine;
    lineLength = 0;
}
```
 
Exécutez l’exemple et vous serez en mesure de lire à haute voix au rythme préconfiguré.

## <a name="async-tasks"></a>Tâches asynchrones
Dans cette étape, vous allez ajouter le code pour écrire la sortie de façon asynchrone dans une tâche, lorsque vous exécutez également une autre tâche pour lire d’entrée de l’utilisateur s’il souhaite accélérer ou ralentir l’affichage du texte. Cela représente plusieurs étapes et à la fin, vous aurez toutes les mises à jour dont vous avez besoin.
La première étape consiste à créer une méthode de retour <xref:System.Threading.Tasks.Task> asynchrone qui représente le code que vous avez créé jusqu'à présent pour lire et afficher le fichier.

Ajoutez cette méthode à votre classe `Program` (elle est extraite du corps de votre méthode `Main`) :

```csharp
private static async Task ShowTeleprompter()
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var line in words)
    {
        Console.Write(line);
        if (!string.IsNullOrWhiteSpace(line))
        {
            await Task.Delay(200);
        }
    }
}
```

Vous remarquerez deux modifications. Tout d’abord, dans le corps de la méthode, au lieu d’appeler <xref:System.Threading.Tasks.Task.Wait> pour attendre de manière synchrone qu’une tâche se termine, cette version utilise le mot-clé `await`. Pour ce faire, vous devez ajouter le modificateur `async` pour la signature de méthode. Cette méthode renvoie une `Task`. Notez qu’il n’y a aucune instruction de retour renvoyant un objet `Task`. Au lieu de cela, cet objet `Task` est créé par le code que le compilateur génère lorsque vous utilisez l’opérateur `await`. Vous pouvez imaginer que cette méthode renvoie une valeur lorsqu’elle atteint un `await`. La `Task` renvoyée indique que la tâche n’a pas été effectuée.
La méthode reprend lorsque la tâche attendue se termine. Lorsqu’elle a terminé son exécution, la `Task` renvoyée indique qu’elle est terminée.
Le code appelant peut surveiller cette `Task` renvoyée pour déterminer si elle est terminée.

Vous pouvez appeler cette nouvelle méthode dans votre méthode `Main` :

```csharp
ShowTeleprompter().Wait();
```

Ici, dans `Main`, le code attend de façon synchrone. Vous devez utiliser l’opérateur `await` au lieu d’attendre de manière synchrone lorsque cela est possible. Mais, dans une méthode `Main` d’application de console, vous ne pouvez pas utiliser l’opérateur `await`. Cela entraînerait la fermeture de l’application avant la fin de toutes les tâches.

Ensuite, vous devez écrire la seconde méthode asynchrone pour lire à partir de la console et chercher les touches '<’ et ’>'. Voici la méthode que vous ajoutez pour cette tâche :

```csharp
private static async Task GetInput()
{
    var delay = 200;
    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
            {
                delay -= 10;
            }
            else if (key.KeyChar == '<')
            {
                delay += 10;
            }
        } while (true);
    };
    await Task.Run(work);
}
```

Cela crée une expression lambda pour représenter un délégué <xref:System.Action> qui lit une touche de la console et modifie une variable locale représentant le délai lorsque l’utilisateur appuie sur les touches '<’ ou ’>'. Cette méthode utilise <xref:System.Console.ReadKey> pour bloquer et attendre que l’utilisateur appuie sur une touche.

Pour terminer cette fonctionnalité, vous devez créer une nouvelle méthode de retour `async Task` qui démarre ces deux tâches (`GetInput` et `ShowTeleprompter`) et gère également les données partagées entre ces deux tâches.
 
Il est temps de créer une classe qui peut gérer les données partagées entre ces deux tâches. Cette classe contient deux propriétés publiques : le délai et un indicateur pour indiquer que le fichier a été entièrement lu :

```csharp
namespace TeleprompterConsole
{
    internal class TelePrompterConfig
    {
        private object lockHandle = new object();
        public int DelayInMilliseconds { get; private set; } = 200;

        public void UpdateDelay(int increment) // negative to speed up
        {
            var newDelay = Min(DelayInMilliseconds + increment, 1000);
            newDelay = Max(newDelay, 20);
            lock (lockHandle)
            {
                DelayInMilliseconds = newDelay;
            }
        }
    }
}
```

Placez cette classe dans un nouveau fichier et ajoutez-la à l’espace de noms `TeleprompterConsole`, comme indiqué ci-dessus. Vous devrez également ajouter une instruction `using static` pour pouvoir référencer les méthodes `Min` et `Max` sans les noms de la classe ou de l’espace de noms qui les contient. Une instruction `using static` importe les méthodes d’une classe. Cela est en opposition avec les instructions `using` utilisées jusqu'à présent, et qui avaient importé toutes les classes à partir d’un espace de noms.

```csharp
using static System.Math;
```

L’autre fonctionnalité de langage qui est une nouveauté est l’instruction `lock`. Cette instruction permet de garantir qu’un seul thread peut se trouver dans ce code à un moment donné. Si un thread se trouve dans la section verrouillée, les autres threads doivent attendre que le premier thread quitte cette section. L’instruction `lock` utilise un objet qui assure la protection de la section de verrou. Cette classe suit un idiome standard pour verrouiller un objet privé dans la classe.

Ensuite, vous devez mettre à jour les méthodes `ShowTeleprompter` et `GetInput` pour utiliser le nouvel objet `config`. Écrivez une dernière `Task` renvoyant une méthode `async` pour démarrer les deux tâches et quitter lorsque la première tâche est terminée :

```csharp
private static async Task RunTeleprompter()
{
    var config = new TelePrompterConfig();
    var displayTask = ShowTeleprompter(config);

    var speedTask = GetInput(config);
    await Task.WhenAny(displayTask, speedTask);
}
```

Une nouvelle méthode ici est la <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])> appeler. Elle crée une `Task` qui se termine dès que les tâches de sa liste d’arguments se terminent.

Ensuite, vous devez mettre à jour les méthodes `ShowTeleprompter` et `GetInput` pour utiliser l’objet `config` pour le délai :

```csharp
private static async Task ShowTeleprompter(TelePrompterConfig config)
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var line in words)
    {
        Console.Write(line);
        if (!string.IsNullOrWhiteSpace(line))
        {
            await Task.Delay(config.DelayInMilliseconds);
        }
    }
    config.SetDone();
}

private static async Task GetInput(TelePrompterConfig config)
{

    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
                config.UpdateDelay(-10);
            else if (key.KeyChar == '<')
                config.UpdateDelay(10);
        } while (!config.Done);
    };
    await Task.Run(work);
}
```

Cette nouvelle version de `ShowTeleprompter` appelle une nouvelle méthode dans la classe `TeleprompterConfig`. À présent, vous devez mettre à jour `Main` pour appeler `RunTeleprompter` au lieu de `ShowTeleprompter` :

```csharp
RunTeleprompter().Wait();
```

Pour terminer, vous devez ajouter la méthode `SetDone` et la propriété `Done` à la classe `TelePrompterConfig` :

```csharp
public bool Done => done;

private bool done;

public void SetDone()
{
    done = true;    
}
```

## <a name="conclusion"></a>Conclusion
Ce didacticiel vous a montré certaines des fonctionnalités du langage C# et les bibliothèques .NET Core liées au travail dans les applications console.
Vous pouvez utiliser ces connaissances pour explorer davantage le langage ainsi que les classes présentées ici. Vous avez vu les principes de base des E/S de fichiers et de console, l’utilisation bloquante et non bloquante du modèle de programmation asynchrone basé sur les tâches, une présentation du langage C# et de la façon dont les programmes C# sont organisés, et l’interface de ligne de commande et les outils de programmation .NET Core.
 
<a name="dotnet-restore-note"></a>
[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
