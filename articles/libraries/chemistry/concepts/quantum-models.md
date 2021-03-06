---
title: 電子システムの Quantum モデル
description: クォンタムモデリングを使用して分子の電子システムをシミュレートする方法について説明します。
author: nathanwiebe2
ms.author: nawiebe@microsoft.com
ms.date: 10/09/2017
ms.topic: article-type-from-white-list
uid: microsoft.quantum.chemistry.concepts.quantummodels
ms.openlocfilehash: 9f9fc37944dd76026c2641d9cdf126e71053a598
ms.sourcegitcommit: 6ccea4a2006a47569c4e2c2cb37001e132f17476
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "77904419"
---
# <a name="quantum-models-for-electronic-systems"></a>電子システムの Quantum モデル

電子システムをシミュレートするには、最初に Hamiltonian を指定する必要があります。これは、上記で説明した標準の量子化手順で確認できます。
具体的には、$N _e $ 原子と momenta $p _i $ (3 つの次元) を使用し、nuclei を料金 $m _e $ と $x $ _i $Z e $ _k $y $ に配置します。 Hamiltonian 演算子は、\hat{H} = \ sum\_{i = 1} ^ {N\_e} \frac{\hat{p}\_i ^ 2} {2m\_e} + +-frac{1}{2}\ sum\_{* * e} \frac{e ^ 2} {| \hat{x}\_i-\hat{x}\_j |}-\ sum\_{i, k} \frac{Z\_ke ^ 2} {| \hat{x}\_i-{y}\_k |}+ \ frac{1}{2} \ sum_ {k\n e k '} \frac{Z\_kZ\_{k '} e ^ 2} {| y\_k-y\_k ' |}。 & # {eq: Ham} \ end{momenta operator $ \hat{p}\_i ^ 2 $ は、Laのように、実数空間で表示できます。つまり、$ \hat{p}\_i ^ 2 =-\ partial\_{x\_i} ^ 2-\ partial\_{y\_i} ^ 2-\ partial\_{z\_i} ^ 2 $。
ここでは、nuclei が分子に保存されていることを前提としています。
これは、Oppenheimer 近似として知られており、$ \hat{H} $ の低エネルギーエネルギー範囲で有効になる傾向があります。これは、電子質量が proton の質量を約 $ 1/1836 $ にするためです。
この Hamiltonian 演算子は、$N\_e $ 原子のシステムのエネルギーを書き出し、 [Quantum Dynamics](xref:microsoft.quantum.chemistry.concepts.quantumdynamics)に記述されている正規の量子化プロセスを適用することで簡単に見つけることができます。

$E ^ {-i\ hat {H} t} $ のためのユニタリ行列表現を構築するには、演算子 $ \hat{H} $ をマトリックスとして表す必要があります。
そのためには、の問題を表すために、座標系または基準を選択する必要があります。
たとえば、$ \ psi_j $ が $N _e $ 原子の直交ベース関数のセットである場合、マトリックスを定義できます。

\ begin{\hat{H}} H\_{i, j} = \ int\_{-\ infty} ^ \ inf\_{-\ inf} ^ \ inf {-{\*}\_i (x\_1)/psi\_j (x\_2)、dd ^ 3x\_1、dd ^ 3x\_2。 \ ラベル {eq: discreteHam} \

原則として、$ \hat{H} $ 演算子は無制限で、有限の次元空間では動作しませんが、$H 要素を含む行列は \{i, j\}$ 上に\_ます。
これは、ベースセットの一部を選択しすぎるとエラーが発生することを意味します。ただし、大きな基礎を選択すると、化学力学を実用的にシミュレートできます。
このため、問題を簡潔に表す基準を選択することは、電子構造の問題を解決するうえで重要です。

使用できる適切なベースは多数あります。また、問題に対応するための適切な基準を選択することは、多くの場合、量子化学の最先端です。
おそらく、最も単純な基本的なセットは、hydrogen に似たアトムの Schrödinger 式 ($ \hat{H} $ の eigenfunctions) に対する (orthogonalized) ソリューションであるということです。
平面や実数など、その他の基準セットを使用することもできます。詳細については、通常のテキスト[「分子の電子構造理論」](https://onlinelibrary.wiley.com/doi/book/10.1002/9781119019572)を参照してください。

上記のモデルで使用されている状態は任意であるように見えますが、量子化メカニズムによって、自然な状態になる場合があります。
特に、すべての有効な電子 quantum の状態は、電子ラベルの交換の下で対称である必要があります。
つまり、$ \ psi (x_1, x_2) $ が2つの原子のジョイントクォンタム状態の wave 関数であった場合、$ $ \ psi (x_1, x_2) =-\ psi (x_2, x_1) である必要があります。
$ $ は、2つの原子を同じクォンタム状態にすることを禁止する Pforce Li 除外原則です。 fascinatingly は、この法律の直接的な結果を intuited できます。これは、同じ位置 $/psi (x_1、x_1) にある2つの原子をスワップした場合、x_1、x_1) \n e-\ psi (x_1、x_1) $ \ psi (x_1、x_1) = 0 $ を除きます。
したがって、このアンチ対称性プロパティに従うために初期状態を選択する必要があります。また、2つの原子を同時に同じ状態にすることはできません。
これは、複数の原子が同じ状態になるのを防ぐことができるため、電子的な構造には非常に重要です。また、クォンタムのコンピューターで1つのクォンタムビットを使用して、特定のクォンタムの状態に原子の数を格納することもできます。

量子機構は、これらの状態を分離ことによってクォンタムコンピューターでシミュレートできますが、この方法では、状態を格納するために多くの qubits が必要であり、また、対称の初期状態。
ただし、これらの問題は、さまざまな観点からシミュレーションの問題を表示することによって階段状にすることができます。
