---
title: クォンタム Machine Learning ライブラリを使用した基本的な分類
description: 'Microsoft QDK の Quantum Machine Learning ライブラリを使用して、Q # で記述されたクォンタムシーケンシャル分類器を実行する方法について説明します。'
author: geduardo
ms.author: v-edsanc@microsoft.com
ms.date: 02/16/2020
ms.topic: article
uid: microsoft.quantum.libraries.machine-learning.basics
ms.openlocfilehash: f42e3e4492f934d7a8f03d4fec6fa0de765401d7
ms.sourcegitcommit: 6ccea4a2006a47569c4e2c2cb37001e132f17476
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "77909927"
---
# <a name="basic-classification-classify-data-with-the-qdk"></a><span data-ttu-id="a47be-103">基本分類: QDK でデータを分類する</span><span class="sxs-lookup"><span data-stu-id="a47be-103">Basic classification: Classify data with the QDK</span></span>

<span data-ttu-id="a47be-104">このクイックスタートでは、QDK の Quantum Machine Learning ライブラリを使用して、Q # で記述されたクォンタムシーケンシャル分類器を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a47be-104">In this Quickstart, you will learn how to execute a quantum sequential classifier written in Q# using the Quantum Machine Learning library of the QDK.</span></span> 

<span data-ttu-id="a47be-105">このガイドでは、Q # で定義された分類子構造を使用して、半月のデータセットを使用します。</span><span class="sxs-lookup"><span data-stu-id="a47be-105">In this guide we will use the half-moon dataset, using a classifier structure defined in Q#.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a47be-106">前提条件</span><span class="sxs-lookup"><span data-stu-id="a47be-106">Prerequisites</span></span>

- <span data-ttu-id="a47be-107">Microsoft [Quantum 開発キット](xref:microsoft.quantum.install)。</span><span class="sxs-lookup"><span data-stu-id="a47be-107">The Microsoft [Quantum Development Kit](xref:microsoft.quantum.install).</span></span>
- [<span data-ttu-id="a47be-108">Q# プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="a47be-108">Create a Q# Project</span></span>](xref:microsoft.quantum.howto.createproject)

## <a name="host-program"></a><span data-ttu-id="a47be-109">ホストプログラム</span><span class="sxs-lookup"><span data-stu-id="a47be-109">Host program</span></span>

<span data-ttu-id="a47be-110">ホストプログラムは、次の3つの部分で構成されます。</span><span class="sxs-lookup"><span data-stu-id="a47be-110">Your host program consists of three parts:</span></span>

- <span data-ttu-id="a47be-111">データセットを読み込み、モデルの開始パラメーターのセットを選択します。</span><span class="sxs-lookup"><span data-stu-id="a47be-111">Load the dataset and choose a set of starting parameters for your model.</span></span>
- <span data-ttu-id="a47be-112">トレーニングを実行して、モデルのパラメーターとバイアスを決定します。</span><span class="sxs-lookup"><span data-stu-id="a47be-112">Execute training to determine the parameters and bias of the model.</span></span>
- <span data-ttu-id="a47be-113">モデルを検証してその精度を確認する</span><span class="sxs-lookup"><span data-stu-id="a47be-113">Validate the model to determine its accuracy</span></span>

    ### <a name="python-with-visual-studio-code-or-the-command-line"></a>[<span data-ttu-id="a47be-114">Visual Studio Code またはコマンド ラインを使用した Python</span><span class="sxs-lookup"><span data-stu-id="a47be-114">Python with Visual Studio Code or the Command Line</span></span>](#tab/tabid-python)

    <span data-ttu-id="a47be-115">Python の Q # 分類子であることを実行するには、次のコードを `host.py`として保存します。</span><span class="sxs-lookup"><span data-stu-id="a47be-115">To run your the Q# classifier from Python, save the following code as `host.py`.</span></span> <span data-ttu-id="a47be-116">このチュートリアルの後半で説明する Q # ファイル `Training.qs` も必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a47be-116">Remember that you also need the Q# file `Training.qs` that is explained later in this tutorial.</span></span>

    :::code language="python" source="~/quantum/samples/machine-learning/half-moons/host.py" range="3-42":::

    <span data-ttu-id="a47be-117">次に、コマンド ラインから Python ホスト プログラムを実行できます。</span><span class="sxs-lookup"><span data-stu-id="a47be-117">You can then run your Python host program from the command line:</span></span>

    ```bash
    $ python host.py
    Preparing Q# environment...
    [...]
    Observed X.XX% misclassifications.
    ```

    ### <a name="c-with-visual-studio-code-or-the-command-line"></a>[<span data-ttu-id="a47be-118">Visual Studio Code またはコマンド ラインを使用した C#</span><span class="sxs-lookup"><span data-stu-id="a47be-118">C# with Visual Studio Code or the Command Line</span></span>](#tab/tabid-csharp)

    <span data-ttu-id="a47be-119">Q # 分類子C#を実行するには、次のコードを `Host.cs`として保存します。</span><span class="sxs-lookup"><span data-stu-id="a47be-119">To run your the Q# classifier from C#, save the following code as `Host.cs`.</span></span> <span data-ttu-id="a47be-120">このチュートリアルの後半で説明する Q # ファイル `Training.qs` も必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a47be-120">Remember that you also need the Q# file `Training.qs` that is explained later in this tutorial.</span></span>

    :::code language="csharp" source="~/quantum/samples/machine-learning/half-moons/Host.cs" range="4-86":::

    <span data-ttu-id="a47be-121">次に、コマンド ラインから C# ホスト プログラムを実行できます。</span><span class="sxs-lookup"><span data-stu-id="a47be-121">You can then run your C# host program from the command line:</span></span>

    ```bash
    $ dotnet run
    [...]
    Observed X.XX% misclassifications.
    ```

    ### <a name="c-with-visual-studio-2019"></a>[<span data-ttu-id="a47be-122">Visual Studio 2019 を使用した C#</span><span class="sxs-lookup"><span data-stu-id="a47be-122">C# with Visual Studio 2019</span></span>](#tab/tabid-vs2019)

    <span data-ttu-id="a47be-123">Visual Studio からC#新しい Q # プログラムを実行するには、次C#のコードを含むように `Host.cs` を変更します。</span><span class="sxs-lookup"><span data-stu-id="a47be-123">To run your new Q# program from C# in Visual Studio, modify `Host.cs` to include the following C# code.</span></span> <span data-ttu-id="a47be-124">このチュートリアルの後半で説明する Q # ファイル `Training.qs` も必要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a47be-124">Remember that you also need the Q# file `Training.qs` that is explained later in this tutorial.</span></span>

    :::code language="csharp" source="~/quantum/samples/machine-learning/half-moons/Host.cs" range="4-86":::

    <span data-ttu-id="a47be-125">F5 キーを押すと、プログラムは実行を開始し、新しいポップアップ ウィンドウに次の結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a47be-125">Then press F5, the program will start execution and a new windows will pop-up with the following results:</span></span> 

    ```bash
    $ dotnet run
    [...]
    Observed X.XX% misclassifications.
    ```
    ***

## <a name="q-classifier-code"></a><span data-ttu-id="a47be-126">Q\# 分類子コード</span><span class="sxs-lookup"><span data-stu-id="a47be-126">Q\# classifier code</span></span>

<span data-ttu-id="a47be-127">次に、ホストプログラムによって呼び出された操作が Q # でどのように定義されているかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a47be-127">Now let's see how the operations invoked by the host program are defined in Q#.</span></span>
<span data-ttu-id="a47be-128">`Training.qs`という名前のファイルに次のコードを保存します。</span><span class="sxs-lookup"><span data-stu-id="a47be-128">We save the following code in a file named `Training.qs`.</span></span>

:::code language="qsharp" source="~/quantum/samples/machine-learning/half-moons/Training.qs" range="4-116":::

<span data-ttu-id="a47be-129">上記のコードで定義されている最も重要な関数と操作は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a47be-129">The most important functions and operations defined in the code above are:</span></span>

- <span data-ttu-id="a47be-130">`ClassifierStructure() : ControlledRotation[]`: この関数では、考慮する制御ゲートのレイヤーを追加することによって、サーキットモデルの構造を設定します。</span><span class="sxs-lookup"><span data-stu-id="a47be-130">`ClassifierStructure() : ControlledRotation[]` : in this function we set the structure of our circuit model by adding the layers of the controlled gates we consider.</span></span> <span data-ttu-id="a47be-131">この手順は、シーケンシャルなディープラーニングモデルでのニューロンのレイヤーの宣言に似ています。</span><span class="sxs-lookup"><span data-stu-id="a47be-131">This step is analogous to the declaration of layers of neurons in a sequential deep learning model.</span></span>
- <span data-ttu-id="a47be-132">`TrainHalfMoonModel() : TrainWineModel() : (Double[], Double)`: この操作はコードの中核となる部分で、トレーニングを定義します。</span><span class="sxs-lookup"><span data-stu-id="a47be-132">`TrainHalfMoonModel() : TrainWineModel() : (Double[], Double)` : this operation is the core part of the code and defines the training.</span></span> <span data-ttu-id="a47be-133">ここでは、ライブラリに含まれるデータセットからサンプルを読み込みます。ここでは、トレーニングのハイパーパラメーターと初期パラメーターを設定し、ライブラリに含まれる操作 `TrainSequentialClassifier` 呼び出してトレーニングを開始します。</span><span class="sxs-lookup"><span data-stu-id="a47be-133">Here we load the samples from the dataset included in the library, we set the hyper parameters and the initial parameters for the training and we start the training by calling the operation `TrainSequentialClassifier` included in the library.</span></span> <span data-ttu-id="a47be-134">パラメーターと、分類子を決定するバイアスを出力します。</span><span class="sxs-lookup"><span data-stu-id="a47be-134">It outputs the parameters and the bias that determine the classifier.</span></span>
- <span data-ttu-id="a47be-135">`ValidateHalfMoonModel(parameters : Double[], bias : Double) : Int`: この操作では、モデルを評価する検証プロセスを定義します。</span><span class="sxs-lookup"><span data-stu-id="a47be-135">`ValidateHalfMoonModel(parameters : Double[], bias : Double) : Int` : this operation defines the validation process to evaluate the model.</span></span> <span data-ttu-id="a47be-136">ここでは、検証用のサンプル、サンプルごとの測定数、および許容範囲を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="a47be-136">Here we load the samples for validation, the number of measurements per sample and the tolerance.</span></span> <span data-ttu-id="a47be-137">検証用に選択されたサンプルのバッチに誤分類の数が出力されます。</span><span class="sxs-lookup"><span data-stu-id="a47be-137">It outputs the number of misclassifications on the chosen batch of samples for validation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a47be-138">次のステップ:</span><span class="sxs-lookup"><span data-stu-id="a47be-138">Next steps</span></span>

<span data-ttu-id="a47be-139">まず、コードを使用して再生し、いくつかのパラメーターを変更してトレーニングにどのように影響するかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="a47be-139">First, you can play with the code and try to change some parameters to see how it affects the training.</span></span> <span data-ttu-id="a47be-140">次のチュートリアル「[独自の分類子の設計](xref:microsoft.quantum.libraries.machine-learning.design)」では、分類子の構造を定義する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a47be-140">Then, in the next tutorial, [Design your own classifier](xref:microsoft.quantum.libraries.machine-learning.design),  you will learn how to define the structure of the classifier.</span></span>