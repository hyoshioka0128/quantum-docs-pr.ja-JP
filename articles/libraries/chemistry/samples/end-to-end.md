---
title: NWChem を使用したエンドツーエンド |Microsoft Docs
description: NWChem Docs を使用したエンドツーエンドの
author: cgranade
ms.author: chgranad@microsoft.com
ms.date: 10/23/2018
uid: microsoft.quantum.chemistry.examples.endtoend
ms.openlocfilehash: 8f727ea4599e06b41ced561c624c4e773b9887d9
ms.sourcegitcommit: 8becfb03eb60ba205c670a634ff4daa8071bcd06
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73185819"
---
# <a name="end-to-end-with-nwchem"></a><span data-ttu-id="5526b-103">NWChem を使用したエンドツーエンドの</span><span class="sxs-lookup"><span data-stu-id="5526b-103">End-to-end with NWChem</span></span> #

<span data-ttu-id="5526b-104">このページでは、Nw化学シミュレーションのゲート数を取得する例を、 [Nwchem](http://www.nwchem-sw.org/index.php/Main_Page)入力デッキから順に見ていきます。</span><span class="sxs-lookup"><span data-stu-id="5526b-104">In this page, we will walk through an example of getting gate counts for quantum chemistry simulation, starting from an [NWChem](http://www.nwchem-sw.org/index.php/Main_Page) input deck.</span></span>
<span data-ttu-id="5526b-105">この例に進む前に、Docker がインストールされていることを確認してください。[インストールと検証のガイド](xref:microsoft.quantum.chemistry.concepts.installation)に従ってください。</span><span class="sxs-lookup"><span data-stu-id="5526b-105">Before proceeding with this example, make sure that you've installed Docker, following the [installation and validation guide](xref:microsoft.quantum.chemistry.concepts.installation).</span></span>

<span data-ttu-id="5526b-106">詳細情報</span><span class="sxs-lookup"><span data-stu-id="5526b-106">For more information:</span></span>
- [<span data-ttu-id="5526b-107">NWChem 入力デッキの構造</span><span class="sxs-lookup"><span data-stu-id="5526b-107">Structure of NWChem input decks</span></span>](https://github.com/nwchemgit/nwchem/wiki/Getting-Started#input-file-structure)
    - [<span data-ttu-id="5526b-108">Quantum 開発キットで使用する入力デックコマンド</span><span class="sxs-lookup"><span data-stu-id="5526b-108">Input deck commands for use with the Quantum Development Kit</span></span>](https://github.com/nwchemgit/nwchem/tree/master/contrib/quasar)
- [<span data-ttu-id="5526b-109">化学ライブラリと依存関係のインストール</span><span class="sxs-lookup"><span data-stu-id="5526b-109">Installing the chemistry library and dependencies</span></span>](xref:microsoft.quantum.chemistry.concepts.installation)
- [<span data-ttu-id="5526b-110">リソースカウント</span><span class="sxs-lookup"><span data-stu-id="5526b-110">Resource counting</span></span>](xref:microsoft.quantum.chemistry.examples.resourcecounts)

> [!NOTE]
> <span data-ttu-id="5526b-111">この例では、Windows PowerShell Core を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5526b-111">This example requires Windows PowerShell Core to run.</span></span>
> <span data-ttu-id="5526b-112">Windows、macOS、または Linux 用の PowerShell Core を https://github.com/PowerShell/PowerShell でダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="5526b-112">Download PowerShell Core for Windows, macOS, or Linux at https://github.com/PowerShell/PowerShell.</span></span>

## <a name="importing-required-powershell-modules"></a><span data-ttu-id="5526b-113">必要な PowerShell モジュールのインポート</span><span class="sxs-lookup"><span data-stu-id="5526b-113">Importing Required PowerShell Modules</span></span> ##

<span data-ttu-id="5526b-114">まだ実行していない場合は、 [Microsoft/Quantum リポジトリ](https://github.com/Microsoft/Quantum)を複製します。これには、Quantum 開発キットを操作するためのサンプルとユーティリティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5526b-114">If you haven't already done so, clone the [Microsoft/Quantum repository](https://github.com/Microsoft/Quantum), which contains samples and utilities for working with the Quantum Development Kit:</span></span>

```powershell
git clone https://github.com/Microsoft/Quantum
```

<span data-ttu-id="5526b-115">`Microsoft/Quantum`を複製したら、`utilities/` フォルダーに `cd` を実行し、Docker と NWChem を操作するための PowerShell モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="5526b-115">Once you've cloned `Microsoft/Quantum`, perform `cd` into the `utilities/` folder and import the PowerShell module for working with Docker and NWChem:</span></span>

```powershell
cd utilities
Import-Module InvokeNWChem.psm1
```

> [!NOTE]
> <span data-ttu-id="5526b-116">既定では、Windows によって、すべてのスクリプトまたはモジュールの実行がセキュリティ対策として禁止されます。</span><span class="sxs-lookup"><span data-stu-id="5526b-116">By default, Windows prevents the execution of any scripts or modules as a security measure.</span></span>
> <span data-ttu-id="5526b-117">`Invoke-NWChem.psm1` などのモジュールを Windows で実行できるようにするには、実行ポリシーの変更が必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="5526b-117">To allow modules such as `Invoke-NWChem.psm1` to run on Windows, you may need to change the execution policy.</span></span>
> <span data-ttu-id="5526b-118">これを行うには、`Set-ExecutionPolicy` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5526b-118">To do so, run the `Set-ExecutionPolicy` command:</span></span>
> ```powershell
> Set-ExecutionPolicy RemoteSigned -Scope Process
> ```
> <span data-ttu-id="5526b-119">PowerShell を終了すると、実行ポリシーが元に戻されます。</span><span class="sxs-lookup"><span data-stu-id="5526b-119">The execution policy will then be reverted when you exit PowerShell.</span></span>
> <span data-ttu-id="5526b-120">実行ポリシーを保存する場合は、`-Scope`に別の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="5526b-120">If you would like to save the execution policy, use a different value for `-Scope`:</span></span>
> ```powershell
> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

<span data-ttu-id="5526b-121">これで、`Convert-NWChemToBroombridge` コマンドを使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5526b-121">You should now have the `Convert-NWChemToBroombridge` command available:</span></span>

```powershell
Get-Command -Module InvokeNWChem
```

<span data-ttu-id="5526b-122">次に、 **GetGateCount**サンプルに用意されている `Get-GateCount` コマンドをインポートします。</span><span class="sxs-lookup"><span data-stu-id="5526b-122">Next, we'll import the `Get-GateCount` command provided with the **GetGateCount** sample.</span></span>
<span data-ttu-id="5526b-123">詳細については、「」の[サンプルに記載されている手順](https://github.com/Microsoft/Quantum/tree/master/Chemistry/GetGateCount)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5526b-123">For full details, see the [instructions provided with the sample](https://github.com/Microsoft/Quantum/tree/master/Chemistry/GetGateCount).</span></span>
<span data-ttu-id="5526b-124">次に、オペレーティングシステムに応じて、次のように実行し、`win10-x64`、`osx-x64`、または `linux-x64`を使用して `<runtime>` を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5526b-124">Next, run the following, substituting `<runtime>` with either `win10-x64`, `osx-x64`, or `linux-x64`, depending on your operating system:</span></span>

```powershell
cd ../Chemistry/GetGateCount
dotnet publish --self-contained -r <runtime>
Import-Module ./bin/Debug/netcoreapp2.1/<runtime>/publish/get-gatecount.dll
```

<span data-ttu-id="5526b-125">これで、`Get-GateCount` コマンドを使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5526b-125">You should now have the `Get-GateCount` command available:</span></span>

```powershell
Get-Command Get-GateCount
```

## <a name="input-decks"></a><span data-ttu-id="5526b-126">入力デッキ</span><span class="sxs-lookup"><span data-stu-id="5526b-126">Input Decks</span></span> ##

<span data-ttu-id="5526b-127">NWChem パッケージは、メモリ割り当て設定などの他のパラメーターと共に、解決するクォンタムの化学問題を指定する_入力デッキ_と呼ばれるテキストファイルを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="5526b-127">The NWChem package takes a text file called an _input deck_ which specifies a quantum chemistry problem to be solved, along with other parameters such as memory allocation settings.</span></span>
<span data-ttu-id="5526b-128">この例では、NWChem に付属する事前に作成された入力デッキの1つを使用します。</span><span class="sxs-lookup"><span data-stu-id="5526b-128">For this example, we'll use one of the pre-made input decks that comes with NWChem.</span></span>
<span data-ttu-id="5526b-129">まず、 [nwchemgit/nwchem リポジトリ](https://github.com/nwchemgit/nwchem)を複製します。</span><span class="sxs-lookup"><span data-stu-id="5526b-129">First, clone the [nwchemgit/nwchem repository](https://github.com/nwchemgit/nwchem):</span></span>

> [!NOTE]
> <span data-ttu-id="5526b-130">これは非常に大きなリポジトリであるため、浅い複製を実行して、`--depth 1` 引数を使用して帯域幅とディスク領域を節約することができます。</span><span class="sxs-lookup"><span data-stu-id="5526b-130">Since this is a very large repository, we can do a shallow clone to save some bandwidth and disk space, using the `--depth 1` argument.</span></span>
> <span data-ttu-id="5526b-131">ただし、これは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="5526b-131">This is optional, however.</span></span>
> <span data-ttu-id="5526b-132">複製は、`--depth 1`なしで正常に機能します。</span><span class="sxs-lookup"><span data-stu-id="5526b-132">Cloning will work just fine without `--depth 1`.</span></span>

```powershell
git clone https://github.com/nwchemgit/nwchem --depth 1
```

<span data-ttu-id="5526b-133">`nwchemgit/nwchem` リポジトリには、 [`QA/chem_library_tests` フォルダー](https://github.com/nwchemgit/nwchem/tree/master/QA/chem_library_tests)の下に一覧表示されている Quantum 開発キットで使用するためのさまざまな入力デッキが付属しています。</span><span class="sxs-lookup"><span data-stu-id="5526b-133">The `nwchemgit/nwchem` repository comes with a variety of input decks intended for use with the Quantum Development Kit, listed under the [`QA/chem_library_tests` folder](https://github.com/nwchemgit/nwchem/tree/master/QA/chem_library_tests).</span></span>
<span data-ttu-id="5526b-134">この例では、`H4` 入力デッキを使用します。</span><span class="sxs-lookup"><span data-stu-id="5526b-134">For this example, we'll use the `H4` input deck:</span></span>

```powershell
cd nwchem/QA/chem_library_tests/H4
Get-Content h4_sto6g_0.000.nw
```

<span data-ttu-id="5526b-135">分子の問題は、4つの hydrogen アトムのシステムであり、1つの角度に依存する特定のジオメトリに配置されています。これは、デッキの名前 `h4_sto6g_alpha.nw` に示されているように、パラメーター `alpha` ます。</span><span class="sxs-lookup"><span data-stu-id="5526b-135">The molecule in question is a system of 4 hydrogen atoms that are arranged in a certain geometry that depends on one angle, the parameter `alpha` as indicated in the name `h4_sto6g_alpha.nw` of the deck.</span></span> <span data-ttu-id="5526b-136">H4 は、1970年代以来の計算化学の既知の[分子ベンチマーク](https://onlinelibrary.wiley.com/doi/abs/10.1002/qua.560180511)です。</span><span class="sxs-lookup"><span data-stu-id="5526b-136">H4 is a known [molecular benchmark](https://onlinelibrary.wiley.com/doi/abs/10.1002/qua.560180511) for computational chemistry since the 1970s.</span></span> <span data-ttu-id="5526b-137">パラメーター `sto6g` は、デッキが Slater の型回転に対する表現を実装していることを示しています。具体的には、6つのガウスベースの関数[を使用した、](https://en.wikipedia.org/wiki/STO-nG_basis_sets)その表現に関する表現です。</span><span class="sxs-lookup"><span data-stu-id="5526b-137">The parameter `sto6g` is indicative that the deck implements a representation with respect to a Slater-type orbital, specifically, a representation with respect to an [STO-nG basis set](https://en.wikipedia.org/wiki/STO-nG_basis_sets) with 6 Gaussian basis functions.</span></span> <span data-ttu-id="5526b-138">さらに、この入力デッキには、省略形 Engine (TCE) に対するいくつかの手順が含まれています。このエンジンは、NWChem を使用して、Quantum 開発キットとの相互運用に必要な情報を生成します。</span><span class="sxs-lookup"><span data-stu-id="5526b-138">This input deck furthermore contains several instructions to the NWChem Tensor Contraction Engine (TCE) that direct NWChem to produce the information needed for interoperating with the Quantum Development Kit:</span></span>

```
...
set tce:print_integrals T
set tce:qorb 18
set tce:qela  9
set tce:qelb  9
```

## <a name="producing-and-consuming-broombridge-output-from-nwchem"></a><span data-ttu-id="5526b-139">NWChem からの Broombridge 出力の生成と使用</span><span class="sxs-lookup"><span data-stu-id="5526b-139">Producing and Consuming Broombridge Output from NWChem</span></span> ##

<span data-ttu-id="5526b-140">Broombridge ドキュメントを生成して使用するために必要なすべてのものが揃っています。</span><span class="sxs-lookup"><span data-stu-id="5526b-140">We now have everything we need to produce and consume Broombridge documents.</span></span>
<span data-ttu-id="5526b-141">NWChem を実行し、`h4_sto6g_0.000.nw` 入力デッキの Broombridge ドキュメントを生成するには、`Convert-NWChemToBroombridge`を実行します。</span><span class="sxs-lookup"><span data-stu-id="5526b-141">To run NWChem and produce a Broombridge document for the `h4_sto6g_0.000.nw` input deck, run `Convert-NWChemToBroombridge`:</span></span>

> [!NOTE]
> <span data-ttu-id="5526b-142">このコマンドを初めて実行すると、Docker によって `nwchemorg/nwchem-qc` イメージがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="5526b-142">The first time you run this command, Docker will download the `nwchemorg/nwchem-qc` image for you.</span></span>
> <span data-ttu-id="5526b-143">接続速度によっては、これには少し時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="5526b-143">This may take a little while, depending on your connection speed, possibly providing an opportunity to get a cup of coffee.</span></span>

```powershell
Convert-NWChemToBroombridge h4_sto6g_0.000.nw 
```

<span data-ttu-id="5526b-144">これにより、`Get-GateCount`で使用できる `h4_sto6g_0.000.yaml` という名前の Broombridge ドキュメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="5526b-144">This will produce a Broombridge document called `h4_sto6g_0.000.yaml` that we can use with `Get-GateCount`:</span></span>

```powershell
Get-GateCount -Format YAML h4_sto6g_0.000.yaml
```

<span data-ttu-id="5526b-145">さまざまなクォンタムのシミュレーション方法について、T count、ローテーション数、CNOT count などのリソースの推定を含むコンソール出力が表示されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5526b-145">You should now see console output which contains resource estimation such as T-count, rotations count, CNOT count, etc. for various quantum simulation methods:</span></span>

```powershell 
IntegralDataPath    : C:\Users\martinro\REPOS\nwchem\qa\chem_library_tests\h4\h4_sto6g_0.000.yaml
HamiltonianName     : hamiltonian_0
SpinOrbitals        : 8
Method              : Trotter
TCount              : 0
RotationsCount      : 92
CNOTCount           : 520
ElapsedMilliseconds : 327

IntegralDataPath    : C:\Users\martinro\REPOS\nwchem\qa\chem_library_tests\h4\h4_sto6g_0.000.yaml
HamiltonianName     : hamiltonian_0
SpinOrbitals        : 8
Method              : Qubitization
TCount              : 438
RotationsCount      : 516
CNOTCount           : 2150
ElapsedMilliseconds : 528

IntegralDataPath    : C:\Users\martinro\REPOS\nwchem\qa\chem_library_tests\h4\h4_sto6g_0.000.yaml
HamiltonianName     : hamiltonian_0
SpinOrbitals        : 8
Method              : Optimized Qubitization
TCount              : 3540
RotationsCount      : 18
CNOTCount           : 7932
ElapsedMilliseconds : 721
```

<span data-ttu-id="5526b-146">ここでは、さまざまなことを行います。</span><span class="sxs-lookup"><span data-stu-id="5526b-146">There are many things to go do from here:</span></span> 
- <span data-ttu-id="5526b-147">さまざまな定義済みの入力デッキを試してみてください。たとえば、`h4_sto6g_alpha.nw`でパラメーター `alpha` を変えることで、</span><span class="sxs-lookup"><span data-stu-id="5526b-147">Try out different predefined input decks, e.g., by varying the parameter `alpha` in `h4_sto6g_alpha.nw`,</span></span> 
- <span data-ttu-id="5526b-148">(例: さまざまな選択のために `STO-nG` モデルを調査するなど)、NWChem デッキを直接編集して、デッキを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5526b-148">Try modifying the decks by editing the NWChem decks directly, e.g., exploring `STO-nG` models for various choices of n,</span></span> 
- <span data-ttu-id="5526b-149">`nwchem/qa/chem_library_tests`で利用可能なその他の定義済みの NWChem 入力デッキを試してみてください。</span><span class="sxs-lookup"><span data-stu-id="5526b-149">Try other predefined NWChem input decks that are available at `nwchem/qa/chem_library_tests`,</span></span>
- <span data-ttu-id="5526b-150">NWChem から生成された定義済みの Broombridge YAML ベンチマークのスイートを試してみてください。 [Microsoft/Quantum リポジトリ](https://github.com/Microsoft/Quantum/tree/master/Chemistry/IntegralData/YAML)の一部として入手できます。</span><span class="sxs-lookup"><span data-stu-id="5526b-150">Try out a suite of predefined Broombridge YAML benchmarks that were generated from NWChem and are available as part of the [Microsoft/Quantum repository](https://github.com/Microsoft/Quantum/tree/master/Chemistry/IntegralData/YAML).</span></span> <span data-ttu-id="5526b-151">これらのベンチマークは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5526b-151">These benchmarks include:</span></span> 
    - <span data-ttu-id="5526b-152">分子 hydrogen (H2)、Beryllium (Be)、リチウム水素 (LiH) などの小さな分子</span><span class="sxs-lookup"><span data-stu-id="5526b-152">small molecules such as molecular hydrogen (H2), Beryllium (Be), Lithium hydride (LiH),</span></span>
    - <span data-ttu-id="5526b-153">ozone (O3)、carotene、cytosine などのより大きな分子。</span><span class="sxs-lookup"><span data-stu-id="5526b-153">larger molecules such as ozone (O3), beta-carotene, cytosine, and many more.</span></span> 
- <span data-ttu-id="5526b-154">Microsoft Quantum Development Kit のインターフェイスを特徴とするグラフィカルフロントエンドの[Emsl 矢印](https://arrows.emsl.pnnl.gov/api/qsharp_chem)を試してみてください。</span><span class="sxs-lookup"><span data-stu-id="5526b-154">Try out the graphical front-end [EMSL Arrows](https://arrows.emsl.pnnl.gov/api/qsharp_chem) that features an interface to the Microsoft Quantum Development Kit.</span></span> 


## <a name="producing-broombridge-output-from-emsl-arrows"></a><span data-ttu-id="5526b-155">EMSL 方向から Broombridge 出力を生成しています</span><span class="sxs-lookup"><span data-stu-id="5526b-155">Producing Broombridge Output from EMSL Arrows</span></span> ##

<span data-ttu-id="5526b-156">Web ベースのフロントエンドの EMSL 矢印の使用を開始するには、[ここで](https://arrows.emsl.pnnl.gov/api/qsharp_chem)ブラウザーに移動します。</span><span class="sxs-lookup"><span data-stu-id="5526b-156">To get started with web-based front end EMSL Arrows, navigate a browser [here](https://arrows.emsl.pnnl.gov/api/qsharp_chem).</span></span> 

> [!NOTE]
> <span data-ttu-id="5526b-157">Web ブラウザーで EMSL 矢印を実行するには、JavaScript を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5526b-157">Running EMSL Arrows in a web browser requires JavaScript to be enabled.</span></span> <span data-ttu-id="5526b-158">ブラウザーで JavaScript を有効にする方法については、こちらの[手順](https://www.enable-javascript.com/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5526b-158">Please refer to these [instructions](https://www.enable-javascript.com/) on how to enable JavaScript in your browser.</span></span> 

<span data-ttu-id="5526b-159">最初に、[クエリ] ``Enter an esmiles, esmiles reaction, or other Arrows input, then push the "Run Arrows" button.`` ボックスに分子を入力します。</span><span class="sxs-lookup"><span data-stu-id="5526b-159">First, enter a molecule in the query box that says ``Enter an esmiles, esmiles reaction, or other Arrows input, then push the "Run Arrows" button.``</span></span> 

<span data-ttu-id="5526b-160">多くの分子は、"1, 3, Trimethylxanthine" ではなく "caffeine" のように、普通名で入力できます。</span><span class="sxs-lookup"><span data-stu-id="5526b-160">You can enter many molecules by their colloquial name, such as "caffeine" instead of "1,3,7-Trimethylxanthine".</span></span> 

<span data-ttu-id="5526b-161">次に、[``theory{qsharp_chem}``] と表示されているボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5526b-161">Next, click the button that says ``theory{qsharp_chem}``.</span></span> <span data-ttu-id="5526b-162">これにより、Broombridge YAML 形式で出力をエクスポートするよう実行に指示する命令が、クエリボックスに追加されます。</span><span class="sxs-lookup"><span data-stu-id="5526b-162">This will populate the query box further with an instruction that will tell the run to export output in the Broombridge YAML format.</span></span> 

<span data-ttu-id="5526b-163">次に、``Run Arrows``に時計を入れます。</span><span class="sxs-lookup"><span data-stu-id="5526b-163">Now, clock on ``Run Arrows``.</span></span> <span data-ttu-id="5526b-164">入力のサイズによっては、この処理に時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="5526b-164">Depending on the size of the input, this might take a while.</span></span> <span data-ttu-id="5526b-165">また、特定のモデルが既に計算されている場合は、データベース内での検索にのみ使用されるため、非常に高速に実行できます。</span><span class="sxs-lookup"><span data-stu-id="5526b-165">Or, in case the particular model has already been computed before, it can be done extremely fast as it will only amount to a lookup in a database.</span></span> <span data-ttu-id="5526b-166">どちらの場合も、入力によって指定されたデッキに対する NWChem の特定の実行に関する大量の情報を含む新しいページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5526b-166">In either case, you will be taken to a new page that contains a plethora of information about the particular run of NWChem against the deck specified by your input.</span></span> 

<span data-ttu-id="5526b-167">次のヘッダーから始まるセクションから、Broombridge YAML ファイルをダウンロードして保存することができます。</span><span class="sxs-lookup"><span data-stu-id="5526b-167">You can download and save the Broombridge YAML file from the section that starts with the following header:</span></span> 
```powershell
+==================================================+
||              Molecular Calculation             ||
+==================================================+

Id     = 48443

NWOutput = Link to NWChem Output (download)

Datafiles:
qsharp_chem.yaml-2018-10-23-14:37:42 (download)
...
```

<span data-ttu-id="5526b-168">[``download``] をクリックすると、``qsharp_chem48443.yaml`` のような一意のファイル名でローカルコピーが保存されます (実行ごとに、特定の名前が異なります)。</span><span class="sxs-lookup"><span data-stu-id="5526b-168">Click on ``download``, which saves a local copy with a unique file name such as ``qsharp_chem48443.yaml`` (the particular name will be different for each run).</span></span> <span data-ttu-id="5526b-169">その後、次のようにしてこのファイルをさらに処理できます。</span><span class="sxs-lookup"><span data-stu-id="5526b-169">You can then further process this file as above, e.g., with</span></span> 
```powershell
Get-GateCount -Format YAML qsharp_chem48443.yaml
```
<span data-ttu-id="5526b-170">リソースの数を取得します。</span><span class="sxs-lookup"><span data-stu-id="5526b-170">to get resource counts.</span></span> 

<span data-ttu-id="5526b-171">EMSL 矢印のスタートページの [``Arrows Entry - 3D Builder``] タブからアクセスできる3D 分子 builder を利用できます。</span><span class="sxs-lookup"><span data-stu-id="5526b-171">You might enjoy the 3D molecule builder that can be accessed from the ``Arrows Entry - 3D Builder`` tab on the EMSL Arrows start page.</span></span> <span data-ttu-id="5526b-172">表示されている分子の[JSMol](http://wiki.jmol.org/index.php/JSmol) 3d 画像をクリックすると、それを編集できるようになります。</span><span class="sxs-lookup"><span data-stu-id="5526b-172">Clicking the [JSMol](http://wiki.jmol.org/index.php/JSmol) 3D picture of the shown molecule will let you allow to edit it.</span></span> <span data-ttu-id="5526b-173">原子を移動し、原子をドラッグして、分子間の距離が変化したり、原子を追加/削除したりすることができます。これらの選択のそれぞれに対して、[クエリ] ボックスに ``theory{qsharp_chem}`` を追加した後、Broombridge YAML スキーマのインスタンスを生成し、Quantum 化学ライブラリを使用してさらに詳しく調べることができます。</span><span class="sxs-lookup"><span data-stu-id="5526b-173">You can move atoms around, drag atoms apart so that their inter-molecular distances change, add/remove atoms, etc. For each of these choices, once you added ``theory{qsharp_chem}`` in the query box, you can then generate an instance of the Broombridge YAML schema and further explore it using the Quantum Chemistry Library.</span></span> 