---
title: Q# および QDK とは
description: Microsoft によって作成された、量子コンピューター用のアプリケーションを開発するためのプログラミング言語であり、Microsoft の Quantum 開発キットの主要なコンポーネントである Q# について説明します
author: natke
ms.author: nakersha
ms.date: 10/22/2019
ms.topic: article
uid: microsoft.quantum.overview.qsharp
ms.openlocfilehash: a4bf21887e34ac85f75e5e0b9a033138464fd09d
ms.sourcegitcommit: 7d350db4b5e766cd243633aee7d0a839b6274bd6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77907003"
---
# <a name="what-are-q-and-the-qdk"></a>Q# および QDK とは

Q # は、量子コンピューティング専用に設計された機能を持つプログラミング言語です。
Microsoft の Quantum 開発キット (QDK) の主要なコンポーネントとして、ゲート シーケンスの最適化や量子コンピューターの物理的な実装などの技術的な詳細を気にすることなく、アルゴリズムにフォーカスできるフレームワークを量子プログラマーに提供します。

QDK は、開発者が量子プログラムの作成を開始するのに必要なあらゆる機能を提供する幅広いツールで構成されています。
QDK には、Q# 言語と共に次のものが含まれます。
* *Q # ライブラリ*。これにより、開発者は実際の量子アプリケーションをすぐに作成できます。
* Q# プログラムを実行できるさまざまな "*ターゲット マシン*"。 これには、大規模な量子プログラム用のリソースのエスティメーターとシミュレーター、およびノイズフリーの量子コンピューターとして動作する完全状態の量子シミュレーターが含まれます。 後者は、アイデアの考察、プログラムのデバッグ、および量子物理学の学習に非常に役立ちますが、比較的少ない量子ビットを持つプログラムに対してのみ効率的です。 今後、ターゲット マシンとして量子コンピューティング ハードウェアが利用可能になることを期待しています。
* Q# での作業を可能な限りシームレスにする "*ツール*" (Visual Studio と VS Code の拡張機能、Python や Jupyter Notebooks で使用するパッケージなど)。
* Python や C# などの従来のホスト言語と Q # を連携させるための" *API ドキュメント*"

通常、Microsoft の Quantum 開発キットを使用して開発されたアプリケーションは、次の 2 つの部分で構成されます。
1. 1 つ以上の量子アルゴリズム。Q# 量子プログラミング言語を使用して実装され、従来のホスト プログラムによって呼び出されます。 これは、次のもので構成されます。 
    - Q # 演算: 量子演算を含むサブルーチン 
    - Q # 関数: 量子アルゴリズム内で使用される従来のサブルーチン。
2. 従来のプログラム。Python や C# などの従来のプログラミング言語で実装されます。メイン エントリ ポイントとして機能し、量子アルゴリズムを実行する必要があるときに Q# 演算を呼び出します。

## <a name="write-quantum-programs-not-quantum-circuits"></a>量子回路ではなく、量子プログラムを作成する

初期の量子コンピューティングでは、アルゴリズムは、古典コンピューティングの回路図に似た図に視覚化されていました。
回路モデルは長年の量子コンピューティングの研究において役に立っていますが、Microsoft では開発者が Q# を使用することで、量子回路を超えた量子アルゴリズムとアプリケーションを開発できると考えています。
Q# 言語は、長年にわたる従来のソフトウェア開発を通じて学んだことを活用するために構築されており、量子コンピューティング向けの高度な言語機能が用意されています。

## <a name="how-does-q-work"></a>Q# のしくみ

Q# プログラミング言語には、アルゴリズムを開発するための直観的な型、操作、論理式のセットが用意されているため、量子コンピューターの内部ロジックについて心配する必要がありません。

Q# の基本的な構成要素の 1 つは `Qubit` 型で、実際の量子ビットと同じように、コピーすることも直接アクセスすることもできません。
代わりに、これを測定し、測定の結果を `Result` 変数 (`Zero` と `One` の 2 つの値を取ることができる Q# 型) に格納することができます。
このようなコンストラクトにより、アルゴリズムで量子の物理法則が常に尊重され、量子コンピューターやシミュレーターで正しく実行できます。

また、Q# には条件やループなど従来から使用されている細かいロジック機能もいくつか含まれており、すべての量子法則が尊重されるようになっています。
たとえば、決定論的な従来のサブルーチンのみを含む関数内で量子演算が呼び出されないようにするためにループが実行される方法を制約するなどです。

Q# プログラムは、よく C# や Python で記述されたホスト プログラムと組み合わせて使用されます。これにより、従来から使用されているコードと量子コードを容易に編成できます。
QDK は、C# や Python などの言語をサポートするだけでなく、IQ# Jupyter カーネルを使用することで Jupyter Notebook のサポートを提供します。

## <a name="what-can-i-use-q-for"></a>Q# の用途

### <a name="use-q-to-learn-quantum-computing"></a>Q# を使用して量子コンピューティングを学習する

従来、量子コンピューティングを学習するには、量子ゲートと量子測定の順序付けられたシーケンスの形式でアルゴリズムを理解するために、回路モデルを学習する必要がありました。 Q# では、別のパスを使用できます。量子プログラムを記述することで量子コンピューティングを学べます。

### <a name="use-q-to-design-quantum-algorithms"></a>Q# を使用して量子アルゴリズムを設計する

Q# には、多くのライブラリとユーザー定義型が用意されており、ツールの実装や高度な量子アルゴリズムの構築に役立てることができます。 たとえば、q1 と q2 という 2 つの量子ビットを使用する必要があるとします。 量子ビットを使用するのに、必要なゲートを個別に適用するのではなく、既に組み込まれている操作 `PrepareEntangledState([q1], [q2])` を使用できます。

### <a name="use-q-to-estimate-quantum-resources"></a>Q# を使用して量子リソースを見積もる

Quantum Development Kit (QDK) に付属する完全状態の量子シミュレーターを使用し、Q# プログラムの実行をシミュレーションできます。  QDK からは、リソースの見積もりツールも提供されます。このツールを利用すると、シミュレーターでは大きすぎて実行できない Q# プログラムの実行に関する分析情報が与えられます。  これはアルゴリズム デザイナーにとって非常に貴重な情報です。以前の小規模な量子ハードウェアで実行するため、少ないリソースを使用するようにプログラムを調整できるからです (たとえば、実行されるキュービットの数が少なければ、演算の数も少なくなります)。

### <a name="use-q-to-validate-hardware-performance"></a>Q# を使用してハードウェアのパフォーマンスを検証する

Q# の長所は、プログラムを 1 回記述すれば、デバッグのために量子シミュレーターで実行したり、さまざまな量子コンピューター ハードウェアで実行したりできることです。  Q# で記述されたベンチマーク プログラムを実行し、ハードウェアのパフォーマンスを検証したり、量子コンピューターが進化し、新しい量子コンピューターが利用可能になったとき、結果を比較したりできます。  

## <a name="next-steps"></a>次のステップ

* [量子コンピューティングを学ぶ方法](xref:microsoft.quantum.overview.learn)
* [Microsoft Quantum 開発キットの概要](xref:microsoft.quantum.welcome)
