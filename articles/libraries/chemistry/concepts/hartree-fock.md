---
title: ハーツリー-Fock 理論
description: 量子システムの初期状態を構築する簡単な方法である、Fock 理論について説明します。
author: nathanwiebe2
ms.author: nawiebe@microsoft.com
ms.date: 10/09/2017
ms.topic: article-type-from-white-list
uid: microsoft.quantum.chemistry.concepts.hartreefock
ms.openlocfilehash: 6fa63cbe13fe98565ffb42b56f3ade86720cedb3
ms.sourcegitcommit: 6ccea4a2006a47569c4e2c2cb37001e132f17476
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "77904453"
---
# <a name="hartreefock-theory"></a>ハーツリー– Fock 理論

おそらく、量子化学シミュレーションの最も重要な数量は、Hamiltonian 行列の最低エネルギー eigenvector であるグラウンド州です。
これは、反応率などのほとんどの分子の温度の量は、反力のある経路でのステップの開始と終了を示すクォンタムの状態と、その中間にある部屋の温度の差によって大きくなります。状態は通常、グラウンドの状態です。
通常、グラウンドの状態は、急激に多くの構成を分散しているため、(quantum コンピューターの場合でも) 学習するのは困難です。
グラウンドステートエネルギーなどの数量を学習できます。
たとえば、$ \ket{\psi} $ が純粋なクォンタムの状態である場合、システムがその状態にある平均エネルギーを、\ begin{\hat{H}} E = \ ロウ {/psi} \ket{\psi} に渡します。
その後、グラウンドの状態は、そのような最小値を示す状態になります。 結果として、実際の状態にできるだけ近い状態を選択することは、エネルギーを直接 (可変の eigensolvers で行っているように)、またはフェーズの推定を通じて直接予測するために非常に重要です。

[ハー] ツリー– Fock 理論では、クォンタムシステムの初期状態を簡単に構築できます。 これにより、量子システムのグラウンドの状態に対して、1対後の決定を行うことができます。 最終的には、Fock 内で、地表のエネルギーを最小にする回転が検出されます。 特に、$N $ 原子のシステムの場合、メソッドは prod_ ローテーションを実行します。 \ket {j = 0} ^ {N-1} a ^ \ dagger_j{0} \ prod_ map {j = 0} ^ {N-1} e ^ {u} a ^ dagger_j e ^ {-u} \ket{0}\defeq\ prod_ {j = 0} ^ {N-1} \widetilde{a} ^ \ dagger_j \ket{0}、Hermitian を使用して、(つまり、$u =-u ^ \ ダガー $) マトリックス $u = \ sum_ {pq} u_ {pq} a ^ \ dagger_p a_q $。 マトリックス $u $ は回転ローテーションを表し、$ \widetilde{a} ^ \ dagger_j $ と $ \widetilde{a} _j $ は、を使用する原子の作成および annihilation 演算子を表していることに注意してください– Fock 分子。


次に、$ $u $ は、予測エネルギー $ \bra{0} \ prod_ {j = 0} ^ {N-1} \widetilde{a}\_j H \ 製品\_{k = 0} ^ {N-1} \widetilde{a} ^ \ dagger_k \ket{0}$ を最小化するように最適化されています。 このような最適化の問題は一般的には困難ですが、実際には、Fock アルゴリズムは、特に均衡ジオメトリの閉じたシェルの分子の最適化の問題に対して、ほぼ最適なソリューションに迅速に収束する傾向があります。 これらの状態は、`FermionWavefunction` オブジェクトのインスタンスとして指定できます。 たとえば、$a ^ \ dagger_{1}^ \ dagger_{2}^ \ dagger_{6}\ket{0}$ は、次のように化学ライブラリでインスタンス化されます。
```csharp
// Create a list of integer indices of the creation operators
var indices = new[] { 1, 2, 6 };

// Convert the list of indices to a `FermionWavefunction` object.
// In this case, the indices are integers, so we use the `int`
// type specialization.
var wavefunction = new FermionWavefunction<int>(indices);
```
`SpinOrbital` インデックスを使用して wave 関数のインデックスを作成し、次のようにこれらのインデックスを整数に変換することもできます。
```csharp
// Create a list of spin orbital indices of the creation operators
var indices = new[] { (0, Spin.d), (1,Spin.u), (3, Spin.u) };

// Convert the list of indices to a `FermionWavefunction` object.
var wavefunctionSpinOrbital = new FermionWavefunction<SpinOrbital>(indices.ToSpinOrbitals());

// Convert a wavefunction indexed by spin orbitals to
// one indexed by integers
var wavefunctionInt = wavefunctionSpinOrbital.ToIndexing(IndexConvention.UpDown);
```

Fock 理論に関する最も印象的な機能は、原子の間に entangを持たないクォンタム状態を生成することです。
これは、多くの場合、分子 systems のプロパティに関する適切な定性的な説明を提供することを意味します。 

Fock の状態は、次のように `FermionHamiltonian` から再構築することもできます。
```csharp
// We initialize a fermion Hamiltonian.
var fermionHamiltonian = new FermionHamiltonian();

// Create a Hartree-Fock state from the Hamiltonian 
// with, say, `4` occupied spin orbitals.
var wavefunction = fermionHamiltonian.CreateHartreeFockState(nElectrons: 4);
```

ただし、厳密に相関されたシステムの場合は特に、正確な結果を取得するために、Fock 理論を超える量子状態が発生するようにします。
