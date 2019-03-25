---
title: パターン マッチング機能を使用してデータ型を拡張する
description: この高度なチュートリアルでは、パターン マッチング手法を使用して、別々に作成されたデータとアルゴリズムを使用して機能を作成する方法を示します。
ms.date: 03/13/2019
ms.custom: mvc
ms.openlocfilehash: 0d7c853709d0986710bf4d1a72daeb1f7cda3109
ms.sourcegitcommit: 16aefeb2d265e69c0d80967580365fabf0c5d39a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2019
ms.locfileid: "58125812"
---
# <a name="tutorial-using-pattern-matching-features-to-extend-data-types"></a><span data-ttu-id="2bd86-103">チュートリアル: パターン マッチング機能を使用してデータ型を拡張する</span><span class="sxs-lookup"><span data-stu-id="2bd86-103">Tutorial: Using pattern matching features to extend data types</span></span>

<span data-ttu-id="2bd86-104">C# 7 で、基本的なパターン マッチング機能が導入されました。</span><span class="sxs-lookup"><span data-stu-id="2bd86-104">C# 7 introduced basic pattern matching features.</span></span> <span data-ttu-id="2bd86-105">C# 8 では、これらの機能が新しい式とパターンで拡張されています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-105">Those features are extended in C# 8 with new expressions and patterns.</span></span> <span data-ttu-id="2bd86-106">他のライブラリ内に存在する可能性がある型を拡張したかのように動作する機能を記述できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-106">You can write functionality that behaves as though you extended types that may be in other libraries.</span></span> <span data-ttu-id="2bd86-107">パターンの別の用途は、アプリケーションで必要な、拡張される型の基本機能ではない機能を作成することです。</span><span class="sxs-lookup"><span data-stu-id="2bd86-107">Another use for patterns is to create functionality your application requires that isn't a fundamental feature of the type being extended.</span></span>

<span data-ttu-id="2bd86-108">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-108">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2bd86-109">パターン マッチングを使用する必要がある状況を認識する方法。</span><span class="sxs-lookup"><span data-stu-id="2bd86-109">How to recognize situations where pattern matching should be used.</span></span>
> * <span data-ttu-id="2bd86-110">パターン マッチング式を使用して、型とプロパティの値に基づく動作を実装する方法。</span><span class="sxs-lookup"><span data-stu-id="2bd86-110">How to use pattern matching expressions to implement behavior based on types and property values.</span></span>
> * <span data-ttu-id="2bd86-111">パターン マッチングと他の手法を組み合わせて、完全なアルゴリズムを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="2bd86-111">How to combine pattern matching with other techniques to create complete algorithms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bd86-112">前提条件</span><span class="sxs-lookup"><span data-stu-id="2bd86-112">Prerequisites</span></span>

<span data-ttu-id="2bd86-113">お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 プレビュー コンパイラも実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="2bd86-113">You'll need to set up your machine to run .NET Core, including the C# 8.0 preview compiler.</span></span> <span data-ttu-id="2bd86-114">C# 8 プレビュー コンパイラは、最新の [Visual Studio 2019 プレビュー](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview) または最新の [.NET Core 3.0 プレビュー](https://dotnet.microsoft.com/download/dotnet-core/3.0)で使用できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-114">The C# 8 preview compiler is available with the latest [Visual Studio 2019 preview](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview), or the latest [.NET Core 3.0 preview](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

<span data-ttu-id="2bd86-115">このチュートリアルでは、.NET と、C# と Visual Studio または .NET Core CLI のいずれかに精通していることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-115">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="scenarios-for-pattern-matching"></a><span data-ttu-id="2bd86-116">パターン マッチングのシナリオ</span><span class="sxs-lookup"><span data-stu-id="2bd86-116">Scenarios for pattern matching</span></span>

<span data-ttu-id="2bd86-117">多くの場合、最新の開発には、1 つのまとまりのあるアプリケーションの中で、複数のソースから受信するデータを統合し、それらのデータに基づいて情報と分析情報を提示することが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-117">Modern development often includes integrating data from multiple sources and presenting information and insights from that data in a single cohesive application.</span></span> <span data-ttu-id="2bd86-118">開発者やチームは、受信データを表す型のすべてをコントロールしたり、アクセスしたりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-118">You and your team won't have control or access for all the types that represent the incoming data.</span></span>

<span data-ttu-id="2bd86-119">従来のオブジェクト指向設計では、複数のデータ ソースから受信する各データ型を表す型をアプリケーション内で作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-119">The classic object-oriented design would call for creating types in your application that represent each data type from those multiple data sources.</span></span> <span data-ttu-id="2bd86-120">その後、アプリケーションで、それらの新しい型を操作し、継承階層を構築し、仮想メソッドを作成し、抽象化を実装します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-120">Then, your application would work with those new types, build inheritance hierarchies, create virtual methods, and implement abstractions.</span></span> <span data-ttu-id="2bd86-121">これらの手法は有効であり、ときには最善のツールです。</span><span class="sxs-lookup"><span data-stu-id="2bd86-121">Those techniques work, and sometimes they are the best tools.</span></span> <span data-ttu-id="2bd86-122">少ないコードを記述できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-122">Other times you can write less code.</span></span> <span data-ttu-id="2bd86-123">データと、そのデータを操作する操作を分離する手法を使用して、もっと読みやすいコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-123">You can write more clear code using techniques that separate the data from the operations that manipulate that data.</span></span>

<span data-ttu-id="2bd86-124">このチュートリアルでは、1 つのシナリオでさまざまな外部ソースから受信データを受け取るアプリケーションを作成して探索します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-124">In this tutorial, you'll create and explore an application that takes incoming data from several external sources for a single scenario.</span></span> <span data-ttu-id="2bd86-125">**パターン マッチング**を使用して、元のシステムには含まれていない効率的な方法で、データを使用して処理するしくみを見ていきます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-125">You'll see how **pattern matching** provides an efficient way to consume and process that data in ways that weren't part of the original system.</span></span>

<span data-ttu-id="2bd86-126">通行料金とピーク時間帯料金を使用して交通量を管理する大都市圏について検討します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-126">Consider a major metro area that is using tolls and peak time pricing to manage traffic.</span></span> <span data-ttu-id="2bd86-127">車種に基づいて通行料金を計算するアプリケーションを記述します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-127">You write an application that calculates tolls for a vehicle based on its type.</span></span> <span data-ttu-id="2bd86-128">機能強化として、車両の乗員数に基づく料金を組み込みます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-128">Later enhancements incorporate pricing based on the number of occupants in the vehicle.</span></span> <span data-ttu-id="2bd86-129">さらなる機能強化として、時間帯と曜日に基づく料金を追加します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-129">Further enhancements add pricing based on the time and the day of the week.</span></span>

<span data-ttu-id="2bd86-130">この簡単な説明から、このシステムをモデル化するオブジェクト階層をすぐに思い描くことができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-130">From that brief description, you may have quickly sketched out an object hierarchy to model this system.</span></span> <span data-ttu-id="2bd86-131">ただし、データは、他の車両登録管理システムなどの複数のソースから送信されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-131">However, your data is coming from multiple sources like other vehicle registration management systems.</span></span> <span data-ttu-id="2bd86-132">これらのシステムでは異なるクラスを使用してデータがモデル化されているため、使用できる単一のオブジェクト モデルはありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-132">These systems provide different classes to model that data and you don't have a single object model you can use.</span></span> <span data-ttu-id="2bd86-133">このチュートリアルでは、次のコードに示すように、簡略化されたクラスを使用して外部システムからの車両データをモデル化します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-133">In this tutorial, you'll use these simplified classes to model for the vehicle data from these external systems, as shown in the following code:</span></span>

[!code-csharp[ExternalSystems](~/samples/csharp/tutorials/patterns/start/toll-calculator/ExternalSystems.cs)]

<span data-ttu-id="2bd86-134">GitHub の [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start) リポジトリから、スタート コードをダウンロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-134">You can download the starter code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start) GitHub repository.</span></span> <span data-ttu-id="2bd86-135">さまざまなシステムからの車両クラスがあり、異なる名前空間に存在していることがわかります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-135">You can see that the vehicle classes are from different systems, and are in different namespaces.</span></span> <span data-ttu-id="2bd86-136">`System.Object` を除いて、利用できる共通基底クラスはありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-136">No common base class, other than `System.Object` can be leveraged.</span></span>

## <a name="pattern-matching-designs"></a><span data-ttu-id="2bd86-137">パターン マッチング設計</span><span class="sxs-lookup"><span data-stu-id="2bd86-137">Pattern matching designs</span></span>

<span data-ttu-id="2bd86-138">このチュートリアルで使用するシナリオでは、パターン マッチングを使用して解決するのに適した種類の問題に焦点を当てています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-138">The scenario used in this tutorial highlights the kinds of problems that are well suited to use pattern matching to solve:</span></span> 

- <span data-ttu-id="2bd86-139">操作する必要があるオブジェクトは、目標と一致するオブジェクト階層内にはありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-139">The objects you need to work with aren't in an object hierarchy that matches your goals.</span></span> <span data-ttu-id="2bd86-140">別個のシステムの一部であるクラスを操作する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-140">You may be working with classes that are part of unrelated systems.</span></span>
- <span data-ttu-id="2bd86-141">追加する機能は、これらのクラスのコア抽象化の一部ではありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-141">The functionality you're adding isn't part of the core abstraction for these classes.</span></span> <span data-ttu-id="2bd86-142">車両が支払う通行料金は、車両の種類によって "*変化*" しますが、通行料金は車両の中心的な関数ではありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-142">The toll paid by a vehicle *changes* for different types of vehicles, but the toll isn't a core function of the vehicle.</span></span>

<span data-ttu-id="2bd86-143">データの "*形状*" とデータに対する "*操作*" が一緒に記述されていない場合は、C# のパターン マッチング機能によって、作業が容易になります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-143">When the *shape* of the data and the *operations* on that data are not described together, the pattern matching features in C# make it easier to work with.</span></span> 

## <a name="implement-the-basic-toll-calculations"></a><span data-ttu-id="2bd86-144">基本的な通行料金計算を実装する</span><span class="sxs-lookup"><span data-stu-id="2bd86-144">Implement the basic toll calculations</span></span>

<span data-ttu-id="2bd86-145">最も基本的な通行料金計算は、車種にのみ依存します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-145">The most basic toll calculation relies only on the vehicle type:</span></span>

- <span data-ttu-id="2bd86-146">`Car` は $2.00。</span><span class="sxs-lookup"><span data-stu-id="2bd86-146">A `Car` is $2.00.</span></span>
- <span data-ttu-id="2bd86-147">`Taxi` は $3.50。</span><span class="sxs-lookup"><span data-stu-id="2bd86-147">A `Taxi` is $3.50.</span></span>
- <span data-ttu-id="2bd86-148">`Bus` は $5.00。</span><span class="sxs-lookup"><span data-stu-id="2bd86-148">A `Bus` is $5.00.</span></span>
- <span data-ttu-id="2bd86-149">`DeliveryTruck` は $10.00。</span><span class="sxs-lookup"><span data-stu-id="2bd86-149">A `DeliveryTruck` is $10.00</span></span>

<span data-ttu-id="2bd86-150">新しい `TollCalculator` クラスを作成し、車種に対するパターン マッチングを実装して、通行料金の金額を取得します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-150">Create a new `TollCalculator` class, and implement pattern matching on the vehicle type to get the toll amount.</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    public class TollCalculator
    {
        public decimal CalculateToll(object vehicle) =>
            vehicle switch
        {
            Car c           => 2.00m,
            Taxi t          => 3.50m,
            Bus b           => 5.00m,
            DeliveryTruck t => 10.00m,
            { }             => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
            null            => throw new ArgumentNullException(nameof(vehicle))
        };
    }
}
```

<span data-ttu-id="2bd86-151">上記のコードでは、**型パターン**をテストする **switch 式** ([`switch`](../language-reference/keywords/switch.md) ステートメントとは異なります) が使用されています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-151">The preceding code uses a **switch expression** (not the same as a [`switch`](../language-reference/keywords/switch.md) statement) that tests the **type pattern**.</span></span> <span data-ttu-id="2bd86-152">上記のコードでは、**switch 式**は変数 `vehicle` で始まり、`switch` キーワードが続きます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-152">A **switch expression** begins with the variable, `vehicle` in the preceding code, followed by the `switch` keyword.</span></span> <span data-ttu-id="2bd86-153">次に、すべての **switch アーム**が中かっこ内に指定されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-153">Next comes all the **switch arms** inside curly braces.</span></span> <span data-ttu-id="2bd86-154">`switch` 式は、`switch` ステートメントを囲む構文に対して、その他の絞り込みを行います。</span><span class="sxs-lookup"><span data-stu-id="2bd86-154">The `switch` expression makes other refinements to the syntax that surrounds the `switch` statement.</span></span> <span data-ttu-id="2bd86-155">`case` キーワードは省略され、各アームの結果が式になります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-155">The `case` keyword is omitted, and the result of each arm is an expression.</span></span> <span data-ttu-id="2bd86-156">最後の 2 つのアームは、新しい言語機能を示しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-156">The last two arms show a new language feature.</span></span> <span data-ttu-id="2bd86-157">`{ }` case は、前のアームと一致しなかった null 以外のオブジェクトと一致します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-157">The `{ }` case matches any non-null object that didn't match an earlier arm.</span></span> <span data-ttu-id="2bd86-158">このアームは、このメソッドに渡された正しくない型をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2bd86-158">This arm catches any incorrect types passed to this method.</span></span> <span data-ttu-id="2bd86-159">最後に、`null` パターンは、このメソッドに `null` が渡されたときに、それをキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2bd86-159">Finally, the `null` pattern catches when `null` is passed to this method.</span></span> <span data-ttu-id="2bd86-160">他の型パターンが null 以外の正しい車種のオブジェクトのみと一致するため、`null` パターンを最後にすることができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-160">The `null` pattern can be last because the other type patterns match only a non-null object of the correct type.</span></span>

<span data-ttu-id="2bd86-161">このコードを `Program.cs` の次のコードを使用してテストできます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-161">You can test this code using the following code in `Program.cs`:</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var tollCalc = new TollCalculator();

            var car = new Car();
            var taxi = new Taxi();
            var bus = new Bus();
            var truck = new DeliveryTruck();

            Console.WriteLine($"The toll for a car is {tollCalc.CalculateToll(car)}");
            Console.WriteLine($"The toll for a taxi is {tollCalc.CalculateToll(taxi)}");
            Console.WriteLine($"The toll for a bus is {tollCalc.CalculateToll(bus)}");
            Console.WriteLine($"The toll for a truck is {tollCalc.CalculateToll(truck)}");

            try
            {
                tollCalc.CalculateToll("this will fail");
            }
            catch (ArgumentException e)
            {
                Console.WriteLine("Caught an argument exception when using the wrong type", DayOfWeek.Friday);
            }
            try
            {
                tollCalc.CalculateToll(null);
            }
            catch (ArgumentNullException e)
            {
                Console.WriteLine("Caught an argument exception when using null");
            }
        }
    }
}
```

<span data-ttu-id="2bd86-162">このコードはスタート プロジェクトに含まれていますが、コメント アウトされています。コメントを削除することで、自分で記述したコードをテストできます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-162">That code is included in the starter project, but is commented out. Remove the comments, and you can test what you've written.</span></span>

<span data-ttu-id="2bd86-163">コードとデータが分離されるアルゴリズムの作成にパターンがどのように役立つかを示す最初の例を見てきました。</span><span class="sxs-lookup"><span data-stu-id="2bd86-163">You're starting to see how patterns can help you create algorithms where the code and the data are separate.</span></span> <span data-ttu-id="2bd86-164">`switch` 式によって型がテストされ、結果に基づいて別の値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-164">The `switch` expression tests the type and produces different values based on the results.</span></span> <span data-ttu-id="2bd86-165">これはほんの序の口に過ぎません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-165">That's only the beginning.</span></span>

## <a name="add-occupancy-pricing"></a><span data-ttu-id="2bd86-166">乗員料金を追加する</span><span class="sxs-lookup"><span data-stu-id="2bd86-166">Add occupancy pricing</span></span>

<span data-ttu-id="2bd86-167">料金徴収機関は、車両が乗車定員を乗せて走行することを推奨したいと思っています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-167">The toll authority wants to encourage vehicles to travel at maximum capacity.</span></span> <span data-ttu-id="2bd86-168">車両の乗員数が少ない場合は多く請求し、乗車定員の場合は料金を下げることによって、定員いっぱいでの走行を推奨することを決定しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-168">They've decided to charge more when vehicles have fewer passengers, and encourage full vehicles by offering lower pricing:</span></span>

- <span data-ttu-id="2bd86-169">乗客がいない自動車とタクシーは、$0.50 余分に支払う。</span><span class="sxs-lookup"><span data-stu-id="2bd86-169">Cars and taxis with no passengers pay an extra $0.50.</span></span>
- <span data-ttu-id="2bd86-170">2 名の乗客がいる自動車とタクシーは、$0.50 割引される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-170">Cars and taxis with two passengers get a 0.50 discount.</span></span>
- <span data-ttu-id="2bd86-171">3 名以上の乗客がいる自動車とタクシーは、$1.00 割引される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-171">Cars and taxis with three or more passengers get a $1.00 discount.</span></span>
- <span data-ttu-id="2bd86-172">乗車率が 50% 未満のバスは、$2.00 余分に支払う。</span><span class="sxs-lookup"><span data-stu-id="2bd86-172">Buses that are less than 50% full pay an extra $2.00.</span></span>
- <span data-ttu-id="2bd86-173">乗車率が 90% を超えるバスは、$1.00 割引される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-173">Buses that are more than 90% full get a $1.00 discount.</span></span>

<span data-ttu-id="2bd86-174">このルールは、同じ switch 式で**プロパティ パターン**を使用して実装できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-174">These rules can be implemented using the **property pattern** in the same switch expression.</span></span> <span data-ttu-id="2bd86-175">プロパティ パターンでは、型が決定した後、オブジェクトのプロパティが調べられます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-175">The property pattern examines properties of the object once the type has been determined.</span></span>  <span data-ttu-id="2bd86-176">`Car` に対する 1 つの case が、4 つの異なる case に拡張されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-176">The single case for a `Car` expands to four different cases:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1 }       => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c when c.Passengers > 2 => 2.00m - 1.0m,

    // ...
};
```

<span data-ttu-id="2bd86-177">最初の 3 つの case では、型を `Car` としてテストした後、`Passengers` プロパティの値がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-177">The first three cases test the type as a `Car`, then check the value of the `Passengers` property.</span></span> <span data-ttu-id="2bd86-178">両方が一致した場合は、その式が評価され、結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-178">If both match, that expression is evaluated and returned.</span></span> <span data-ttu-id="2bd86-179">最後の句は、switch アームの `when` 句を示しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-179">The final clause shows the `when` clause of a switch arm.</span></span> <span data-ttu-id="2bd86-180">`when` 句を使用して、プロパティの等価以外の条件をテストできます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-180">You use the `when` clause to test conditions other than equality on a property.</span></span> <span data-ttu-id="2bd86-181">上記の例では、`when` 句によって、自動車の乗客が 2 名を超えているかどうかをテストしています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-181">In the preceding example, the `when` clause tests to see that there are more than 2 passengers in the car.</span></span> <span data-ttu-id="2bd86-182">厳密に言えば、それはこの例では必要ありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-182">Strictly speaking, it's not necessary in this example.</span></span>

<span data-ttu-id="2bd86-183">タクシーに対する case も、同様の方法で拡張します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-183">You would also expand the cases for taxis in a similar manner:</span></span>

```csharp
vehicle switch
{
    // ...

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    // ...
};
```

<span data-ttu-id="2bd86-184">上記の例では、最後の case の `when` 句は省略されています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-184">In the preceding example, the `when` clause was omitted on the final case.</span></span>

<span data-ttu-id="2bd86-185">次に、次の例に示すように、バスに対する case を拡張して、乗車率ルールを実装します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-185">Next, implement the occupancy rules by expanding the cases for buses, as shown in the following example:</span></span>

```csharp
vehicle switch
{
    // ...

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
    Bus b => 5.00m,
    
    // ...
};
```

<span data-ttu-id="2bd86-186">料金徴収機関は、配送トラックの乗客数には関心がありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-186">The toll authority isn't concerned with the number of passengers in the delivery trucks.</span></span> <span data-ttu-id="2bd86-187">代わりに、トラックの重量クラスに基づいて請求します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-187">Instead, they charge more based on the weight class of the trucks.</span></span> <span data-ttu-id="2bd86-188">5,000 lbs を超えるトラックは、$5.00 余分に請求される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-188">Trucks over 5000 lbs are charged an extra $5.00.</span></span> <span data-ttu-id="2bd86-189">3,000 lbs を下回る軽トラックは、$2.00 割引される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-189">Light trucks, under 3000 lbs are given a $2.00 discount.</span></span>  <span data-ttu-id="2bd86-190">これらのルールは、次のコードで実装されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-190">That rule is implemented with the following code:</span></span>

```csharp
vehicle switch
{
    // ...

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="2bd86-191">完了すると、以下によく似たメソッドが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-191">When you've finished, you'll have a method that looks much like the following:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1}        => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c when c.Passengers > 2 => 2.00m - 1.0m,
   
    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,
    
    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
    Bus b => 5.00m,
    
    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="2bd86-192">これらの switch アームの多くは、**再帰パターン**の例です。</span><span class="sxs-lookup"><span data-stu-id="2bd86-192">Many of these switch arms are examples of **recursive patterns**.</span></span> <span data-ttu-id="2bd86-193">たとえば、`Car { Passengers: 1}` は、プロパティ パターンの内部の定数パターンを示しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-193">For example, `Car { Passengers: 1}` shows a constant pattern inside a property pattern.</span></span>

<span data-ttu-id="2bd86-194">switch を入れ子にして使用することで、このコードの反復を少なくすることができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-194">You can make this code less repetitive by using nested switches.</span></span> <span data-ttu-id="2bd86-195">`Car` と `Taxi` は、どちらにも上記の例で示した 4 つの異なるアームがあります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-195">The `Car` and `Taxi` both have four different arms in the preceding examples.</span></span> <span data-ttu-id="2bd86-196">両方に対して、プロパティ パターンに供給される型パターンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-196">In both cases, you can create a type pattern that feeds into a property pattern.</span></span> <span data-ttu-id="2bd86-197">この手法を次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-197">This technique is shown in the following code:</span></span>

```csharp
public decimal CalculateToll(object vehicle) =>
    vehicle switch
    {
        Car c => c.Passengers switch
        {
            0 => 2.00m + 0.5m,
            1 => 2.0m,
            2 => 2.0m - 0.5m,
            _ => 2.00m - 1.0m
        },
    
        Taxi t => t.Fares switch
        {
            0 => 3.50m + 1.00m,
            1 => 3.50m,
            2 => 3.50m - 0.50m,
            _ => 3.50m - 1.00m
        },
    
        Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
        Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
        Bus b => 5.00m,
    
        DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
        DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
        DeliveryTruck t => 10.00m,

        { }  => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
        null => throw new ArgumentNullException(nameof(vehicle))
    };
```

<span data-ttu-id="2bd86-198">上記の例では、再帰式の使用は、プロパティの値をテストする子アームを含む`Car` と `Taxi` のアームを繰り返さないことを意味しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-198">In the preceding sample, using a recursive expression means you don't repeat the `Car` and `Taxi` arms contain child arms that test the property value.</span></span> <span data-ttu-id="2bd86-199">この手法は、プロパティの個別の値ではなくその範囲をテストする `Bus` と `DeliveryTruck` のアームでは使用されません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-199">This technique isn't used for the `Bus` and `DeliveryTruck` arms because those arms are testing ranges for the property, not discrete values.</span></span>

## <a name="add-peak-pricing"></a><span data-ttu-id="2bd86-200">ピーク料金を追加する</span><span class="sxs-lookup"><span data-stu-id="2bd86-200">Add peak pricing</span></span>

<span data-ttu-id="2bd86-201">最後の機能として、料金徴収機関は、時間に依存するピーク料金を追加することを望んでいます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-201">For the final feature, the toll authority wants to add time sensitive peak pricing.</span></span> <span data-ttu-id="2bd86-202">朝と夕方のラッシュ アワーの間は、通行料金を倍にします。</span><span class="sxs-lookup"><span data-stu-id="2bd86-202">During the morning and evening rush hours, the tolls are doubled.</span></span> <span data-ttu-id="2bd86-203">このルールは、片方向の通行のみに影響し、朝のラッシュ時は市内に入る (インバウンド) 通行が、夕方のラッシュ時は市外に出てゆく (アウトバウンド) 通行が対象になります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-203">That rule only affects traffic in one direction: inbound to the city in the morning, and outbound in the evening rush hour.</span></span> <span data-ttu-id="2bd86-204">平日のその他の時間帯では、通行料金は 50% 増額される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-204">During other times during the workday, tolls increase by 50%.</span></span> <span data-ttu-id="2bd86-205">深夜と早朝の通行料金は、25% 減額される。</span><span class="sxs-lookup"><span data-stu-id="2bd86-205">Late night and early morning, tolls are reduced by 25%.</span></span> <span data-ttu-id="2bd86-206">週末は、時間に関係なく、通常料金になる。</span><span class="sxs-lookup"><span data-stu-id="2bd86-206">During the weekend, it's the normal rate, regardless of the time.</span></span>

<span data-ttu-id="2bd86-207">この機能のためにパターン マッチングを使用しますが、それは他の手法と統合します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-207">You'll use pattern matching for this feature, but you'll integrate it with other techniques.</span></span> <span data-ttu-id="2bd86-208">方向、曜日、および時間帯のすべてを組み合わせた単一のパターン マッチ式を作成できますが、</span><span class="sxs-lookup"><span data-stu-id="2bd86-208">You could build a single pattern match expression that would account all the combinations of direction, day of the week, and time.</span></span> <span data-ttu-id="2bd86-209">結果は、複雑な式になるでしょう。</span><span class="sxs-lookup"><span data-stu-id="2bd86-209">The result would be a complicated expression.</span></span> <span data-ttu-id="2bd86-210">読みにくく、理解しにくくなるでしょう。</span><span class="sxs-lookup"><span data-stu-id="2bd86-210">It would be hard to read and difficult to understand.</span></span> <span data-ttu-id="2bd86-211">それは、正確さを保証することを難しくします。</span><span class="sxs-lookup"><span data-stu-id="2bd86-211">That makes it hard to ensure correctness.</span></span> <span data-ttu-id="2bd86-212">代わりに、メソッドを組み合わせて、すべての状態を簡潔に記述する値のタプルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-212">Instead, combine those methods to build a tuple of values that concisely describes all those states.</span></span> <span data-ttu-id="2bd86-213">その後、パターン マッチングを使用して、通行料金の乗数を計算できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-213">Then use pattern matching to calculate a multiplier for the toll.</span></span> <span data-ttu-id="2bd86-214">タプルには、3 つの個別の条件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-214">The tuple contains three discrete conditions:</span></span>

- <span data-ttu-id="2bd86-215">その日が平日または週末のどちらであるか。</span><span class="sxs-lookup"><span data-stu-id="2bd86-215">The day is either a weekday or a weekend.</span></span>
- <span data-ttu-id="2bd86-216">通行料金が収集される時間帯。</span><span class="sxs-lookup"><span data-stu-id="2bd86-216">The band of time when the toll is collected.</span></span>
- <span data-ttu-id="2bd86-217">方向が市内に入るほうか市外に出るほうか。</span><span class="sxs-lookup"><span data-stu-id="2bd86-217">The direction is into the city or out of the city</span></span>

<span data-ttu-id="2bd86-218">次の表に、入力値の組み合わせとピーク時の乗数を示します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-218">The following table shows the combinations of input values and the peak pricing multiplier:</span></span>

| <span data-ttu-id="2bd86-219">曜日</span><span class="sxs-lookup"><span data-stu-id="2bd86-219">Day</span></span>        | <span data-ttu-id="2bd86-220">時間帯</span><span class="sxs-lookup"><span data-stu-id="2bd86-220">Time</span></span>         | <span data-ttu-id="2bd86-221">方向</span><span class="sxs-lookup"><span data-stu-id="2bd86-221">Direction</span></span> | <span data-ttu-id="2bd86-222">割り増し</span><span class="sxs-lookup"><span data-stu-id="2bd86-222">Premium</span></span> |
| ---------- | ------------ | --------- |--------:|
| <span data-ttu-id="2bd86-223">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-223">Weekday</span></span>    | <span data-ttu-id="2bd86-224">朝のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-224">morning rush</span></span> | <span data-ttu-id="2bd86-225">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-225">inbound</span></span>   | <span data-ttu-id="2bd86-226">x 2.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-226">x 2.00</span></span>  |
| <span data-ttu-id="2bd86-227">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-227">Weekday</span></span>    | <span data-ttu-id="2bd86-228">朝のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-228">morning rush</span></span> | <span data-ttu-id="2bd86-229">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-229">outbound</span></span>  | <span data-ttu-id="2bd86-230">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-230">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-231">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-231">Weekday</span></span>    | <span data-ttu-id="2bd86-232">日中</span><span class="sxs-lookup"><span data-stu-id="2bd86-232">daytime</span></span>      | <span data-ttu-id="2bd86-233">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-233">inbound</span></span>   | <span data-ttu-id="2bd86-234">x 1.50</span><span class="sxs-lookup"><span data-stu-id="2bd86-234">x 1.50</span></span>  |
| <span data-ttu-id="2bd86-235">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-235">Weekday</span></span>    | <span data-ttu-id="2bd86-236">日中</span><span class="sxs-lookup"><span data-stu-id="2bd86-236">daytime</span></span>      | <span data-ttu-id="2bd86-237">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-237">outbound</span></span>  | <span data-ttu-id="2bd86-238">x 1.50</span><span class="sxs-lookup"><span data-stu-id="2bd86-238">x 1.50</span></span>  |
| <span data-ttu-id="2bd86-239">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-239">Weekday</span></span>    | <span data-ttu-id="2bd86-240">夕方のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-240">evening rush</span></span> | <span data-ttu-id="2bd86-241">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-241">inbound</span></span>   | <span data-ttu-id="2bd86-242">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-242">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-243">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-243">Weekday</span></span>    | <span data-ttu-id="2bd86-244">夕方のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-244">evening rush</span></span> | <span data-ttu-id="2bd86-245">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-245">outbound</span></span>  | <span data-ttu-id="2bd86-246">x 2.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-246">x 2.00</span></span>  |
| <span data-ttu-id="2bd86-247">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-247">Weekday</span></span>    | <span data-ttu-id="2bd86-248">夜間</span><span class="sxs-lookup"><span data-stu-id="2bd86-248">overnight</span></span>    | <span data-ttu-id="2bd86-249">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-249">inbound</span></span>   | <span data-ttu-id="2bd86-250">x 0.75</span><span class="sxs-lookup"><span data-stu-id="2bd86-250">x 0.75</span></span>  |
| <span data-ttu-id="2bd86-251">平日</span><span class="sxs-lookup"><span data-stu-id="2bd86-251">Weekday</span></span>    | <span data-ttu-id="2bd86-252">夜間</span><span class="sxs-lookup"><span data-stu-id="2bd86-252">overnight</span></span>    | <span data-ttu-id="2bd86-253">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-253">outbound</span></span>  | <span data-ttu-id="2bd86-254">x 0.75</span><span class="sxs-lookup"><span data-stu-id="2bd86-254">x 0.75</span></span>  |
| <span data-ttu-id="2bd86-255">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-255">Weekend</span></span>    | <span data-ttu-id="2bd86-256">朝のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-256">morning rush</span></span> | <span data-ttu-id="2bd86-257">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-257">inbound</span></span>   | <span data-ttu-id="2bd86-258">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-258">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-259">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-259">Weekend</span></span>    | <span data-ttu-id="2bd86-260">朝のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-260">morning rush</span></span> | <span data-ttu-id="2bd86-261">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-261">outbound</span></span>  | <span data-ttu-id="2bd86-262">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-262">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-263">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-263">Weekend</span></span>    | <span data-ttu-id="2bd86-264">日中</span><span class="sxs-lookup"><span data-stu-id="2bd86-264">daytime</span></span>      | <span data-ttu-id="2bd86-265">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-265">inbound</span></span>   | <span data-ttu-id="2bd86-266">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-266">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-267">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-267">Weekend</span></span>    | <span data-ttu-id="2bd86-268">日中</span><span class="sxs-lookup"><span data-stu-id="2bd86-268">daytime</span></span>      | <span data-ttu-id="2bd86-269">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-269">outbound</span></span>  | <span data-ttu-id="2bd86-270">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-270">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-271">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-271">Weekend</span></span>    | <span data-ttu-id="2bd86-272">夕方のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-272">evening rush</span></span> | <span data-ttu-id="2bd86-273">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-273">inbound</span></span>   | <span data-ttu-id="2bd86-274">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-274">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-275">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-275">Weekend</span></span>    | <span data-ttu-id="2bd86-276">夕方のラッシュ時</span><span class="sxs-lookup"><span data-stu-id="2bd86-276">evening rush</span></span> | <span data-ttu-id="2bd86-277">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-277">outbound</span></span>  | <span data-ttu-id="2bd86-278">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-278">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-279">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-279">Weekend</span></span>    | <span data-ttu-id="2bd86-280">夜間</span><span class="sxs-lookup"><span data-stu-id="2bd86-280">overnight</span></span>    | <span data-ttu-id="2bd86-281">インバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-281">inbound</span></span>   | <span data-ttu-id="2bd86-282">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-282">x 1.00</span></span>  |
| <span data-ttu-id="2bd86-283">週末</span><span class="sxs-lookup"><span data-stu-id="2bd86-283">Weekend</span></span>    | <span data-ttu-id="2bd86-284">夜間</span><span class="sxs-lookup"><span data-stu-id="2bd86-284">overnight</span></span>    | <span data-ttu-id="2bd86-285">アウトバウンド</span><span class="sxs-lookup"><span data-stu-id="2bd86-285">outbound</span></span>  | <span data-ttu-id="2bd86-286">x 1.00</span><span class="sxs-lookup"><span data-stu-id="2bd86-286">x 1.00</span></span>  |

<span data-ttu-id="2bd86-287">3 つの変数の 16 組の異なる組み合わせがあります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-287">There are 16 different combinations of the three variables.</span></span> <span data-ttu-id="2bd86-288">いくつかの条件を組み合わせることで、最終的な switch 式を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-288">By combining some of the conditions, you'll simplify the final switch expression.</span></span>

<span data-ttu-id="2bd86-289">ツールを収集するシステムでは、通行料金が徴収される時刻に対して <xref:System.DateTime> 構造体を使用しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-289">The system that collects the tools uses a <xref:System.DateTime> structure for the time when the toll was collection.</span></span> <span data-ttu-id="2bd86-290">上記の表から、変数を作成するメンバー メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-290">Build member methods that create the variables from the preceding table.</span></span>  <span data-ttu-id="2bd86-291">次の関数では、パターン マッチングの switch 式を使用して、<xref:System.DateTime> が週末または平日のどちらを表しているかを示しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-291">The following function uses as pattern matching switch expression to express whether a <xref:System.DateTime> represents a weekend or a weekday:</span></span>

```csharp
private static bool IsWeekDay(DateTime timeOfToll) =>
    timeOfToll.DayOfWeek switch
    {
        DayOfWeek.Monday    => true,
        DayOfWeek.Tuesday   => true,
        DayOfWeek.Wednesday => true,
        DayOfWeek.Thursday  => true,
        DayOfWeek.Friday    => true,
        DayOfWeek.Saturday  => false,
        DayOfWeek.Sunday    => false
    };
```

<span data-ttu-id="2bd86-292">このメソッドは機能しますが、繰り返しが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-292">That method works, but it's repetitious.</span></span> <span data-ttu-id="2bd86-293">次のコードに示すように簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-293">You can simplify it, as shown in the following code:</span></span>

[!code-csharp[IsWeekDay](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#IsWeekDay)]

<span data-ttu-id="2bd86-294">次に、時間をブロックに分類する同様の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-294">Next, add a similar function to categorize the time into the blocks:</span></span>

[!code-csharp[GetTimeBand](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#GetTimeBand)]

<span data-ttu-id="2bd86-295">上記のメソッドでは、パターン マッチングは使用されていません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-295">The previous method doesn't use pattern matching.</span></span> <span data-ttu-id="2bd86-296">使い慣れた `if` ステートメントの連鎖を使用するほうがわかりやすくなります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-296">It's clearer using a familiar cascade of `if` statements.</span></span> <span data-ttu-id="2bd86-297">プライベート `enum` を追加して、時間の各範囲を個別の値に変換します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-297">You do add a private `enum` to convert each range of time to a discrete value.</span></span>

<span data-ttu-id="2bd86-298">これらのメソッドを作成したら、別の `switch` 式と**タプル パターン**を使用して、割増料金を計算できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-298">After you create those methods, you can use another `switch` expression with the **tuple pattern** to calculate the pricing premium.</span></span> <span data-ttu-id="2bd86-299">全部で 16 のアームがある `switch` 式を作成できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-299">You could build a `switch` expression with all 16 arms:</span></span>

[!code-csharp[FullTuplePattern](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#TuplePatternOne)]

<span data-ttu-id="2bd86-300">上記のコードは機能しますが、簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-300">The above code works, but it can be simplified.</span></span> <span data-ttu-id="2bd86-301">週末の 8 つの組み合わせは、同じ通行料金になります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-301">All eight combinations for the weekend have the same toll.</span></span> <span data-ttu-id="2bd86-302">8 つのすべてを、次の 1 つの行に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-302">You can replace all eight with the following one line:</span></span>

```csharp
(false, _, _) => 1.0m,
```

<span data-ttu-id="2bd86-303">平日の日中と夜間のインバウンドとアウトバウンドの通行では、同じ乗数が使用されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-303">Both inbound and outbound traffic have the same multiplier during the weekday daytime and overnight hours.</span></span> <span data-ttu-id="2bd86-304">これら 4 つの switch アームは、次の 2 行に置換できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-304">Those four switch arms can be replaced with the follow two lines:</span></span>

```csharp
(true, TimeBand.Overnight, _) => 0.75m,
(true, TimeBand.Daytime, _)   => 1.5m,
```

<span data-ttu-id="2bd86-305">これら 2 つの変更を行った後のコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2bd86-305">The code should look like the following code after those two changes:</span></span>

```csharp
public decimal PeakTimePremium(DateTime timeOfToll, bool inbound) =>
    (IsWeekDay(timeOfToll), GetTimeBand(timeOfToll), inbound) switch
    {
        (true, TimeBand.MorningRush, true)  => 2.00m,
        (true, TimeBand.MorningRush, false) => 1.00m,
        (true, TimeBand.Daytime,     _)     => 1.50m,
        (true, TimeBand.EveningRush, true)  => 1.00m,
        (true, TimeBand.EveningRush, false) => 2.00m,
        (true, TimeBand.Overnight,   _)     => 0.75m,
        (false, _,                   _)     => 1.00m,
    };
```

<span data-ttu-id="2bd86-306">最後に、通常料金を支払う 2 つのラッシュの時間帯は削除できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-306">Finally, you can remove the two rush hour times that pay the regular price.</span></span> <span data-ttu-id="2bd86-307">これらのアームを削除した後、最後の switch アーム内の `false` を破棄 (`_`)  に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-307">Once you remove those arms, you can replace the `false` with a discard (`_`) in the final switch arm.</span></span> <span data-ttu-id="2bd86-308">次のメソッドが完成します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-308">You'll have the following finished method:</span></span>

[!code-csharp[SimplifiedTuplePattern](../../../samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#FinalTuplePattern)]

<span data-ttu-id="2bd86-309">この例では、パターン マッチングの利点の 1 つに注目しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-309">This example highlights one of the advantages of pattern matching.</span></span> <span data-ttu-id="2bd86-310">パターンの分岐は、順序正しく評価されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-310">The pattern branches are evaluated in order.</span></span> <span data-ttu-id="2bd86-311">前のほうの分岐で後ろにあるいずれかの case が処理されるようにパターンを並べ替えると、コンパイラによって警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-311">If you rearrange them so that an earlier branch handles one of your later cases, the compiler warns you.</span></span> <span data-ttu-id="2bd86-312">これらの言語ルールによって、コードが変化しないという自信を持って、前述した簡略化を簡単に実行できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-312">Those language rules made it easier to do the preceding simplifications with confidence that the code didn't change.</span></span>

<span data-ttu-id="2bd86-313">パターン マッチングは、オブジェクト指向手法を使用した場合に作成するものとは異なるソリューションを実装するための自然な構文を提供します。</span><span class="sxs-lookup"><span data-stu-id="2bd86-313">Pattern matching provides a natural syntax to implement different solutions than you'd create if you used object-oriented techniques.</span></span> <span data-ttu-id="2bd86-314">クラウドによって、データと機能は分離されています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-314">The cloud is causing data and functionality to live apart.</span></span> <span data-ttu-id="2bd86-315">データの "*形状*" とデータに対する "*操作*" は、必ずしも一緒に記述されるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="2bd86-315">The *shape* of the data and the *operations* on it aren't necessarily described together.</span></span> <span data-ttu-id="2bd86-316">このチュートリアルでは、既存のデータを、元の関数とは完全に異なる方法で使用しています。</span><span class="sxs-lookup"><span data-stu-id="2bd86-316">In this tutorial, you consumed existing data in entirely different ways from its original function.</span></span> <span data-ttu-id="2bd86-317">パターン マッチングでは、型を拡張できない場合でも、それらに対する機能を記述できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-317">Pattern matching gave you the ability to write functionality that over those types, even though you couldn't extend them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bd86-318">次の手順</span><span class="sxs-lookup"><span data-stu-id="2bd86-318">Next steps</span></span>

<span data-ttu-id="2bd86-319">GitHub リポジトリの [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished) から、完成したコードをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-319">You can download the finished code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished) GitHub repository.</span></span> <span data-ttu-id="2bd86-320">自分のパターンを調査し、通常のコーディング アクティビティにこの手法を追加してください。</span><span class="sxs-lookup"><span data-stu-id="2bd86-320">Explore patterns on your own and add this technique into your regular coding activities.</span></span> <span data-ttu-id="2bd86-321">これらの手法を習得することで、別の方法で問題にアプローチし、新しい機能を作成できます。</span><span class="sxs-lookup"><span data-stu-id="2bd86-321">Learning these techniques gives you another way to approach problems and create new functionality.</span></span>