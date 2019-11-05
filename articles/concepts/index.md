---
title: 量子コンピューティングの歴史と背景
description: 量子コンピューティングの歴史、そのしくみに関する背景、および Microsoft Quantum Development Kit について説明します。
author: QuantumWriter
ms.author: nawiebe
uid: microsoft.quantum.concepts.intro
ms.date: 12/11/2017
ms.topic: article
ms.openlocfilehash: fb1df9e3460c18d0cdc0ff430fa236192b3aa2fa
ms.sourcegitcommit: aa5e6f4a2deb4271a333d3f1b1eb69b5bb9a7bad
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/02/2019
ms.locfileid: "73442298"
---
# <a name="quantum-computing-history-and-background"></a><span data-ttu-id="2bbdb-103">量子コンピューティングの歴史と背景</span><span class="sxs-lookup"><span data-stu-id="2bbdb-103">Quantum computing history and background</span></span>

<span data-ttu-id="2bbdb-104">過去数年で多数の新しいコンピューター テクノロジが登場した中、量子コンピューティングは、開発者にとって最大限のパラダイムシフトとなるテクノロジであると考えられています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-104">A host of new computer technologies has emerged within the last few years, and quantum computing is arguably the technology requiring the greatest paradigm shift on the part of developers.</span></span>  <span data-ttu-id="2bbdb-105">量子コンピューターは、1980年に [Richard Feynman](https://en.wikipedia.org/wiki/Richard_Feynman) と [Yuri Manin](https://en.wikipedia.org/wiki/Yuri_Manin) によって提案されました。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-105">Quantum computers were proposed in the 1980s by [Richard Feynman](https://en.wikipedia.org/wiki/Richard_Feynman) and [Yuri Manin](https://en.wikipedia.org/wiki/Yuri_Manin).</span></span>  <span data-ttu-id="2bbdb-106">量子コンピューティングを推進させたのは、脅威的な科学の進化に立ちはだかった、単純なシステムでさえもモデル化することができないという、物理学史上最も大きな課題の 1 つから来ています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-106">The intuition behind quantum computing stemmed from what was often seen as one of the greatest embarrassments of physics: remarkable scientific progress faced with an inability to model even simple systems.</span></span> <span data-ttu-id="2bbdb-107">このように、量子力学は 1900 年から 1925 年の間に開発され、化学、物性物理学、そして最終的にはコンピューター チップから LED ライトに至るテクノロジの基礎となります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-107">You see, quantum mechanics was developed between 1900 and 1925 and it remains the cornerstone on which chemistry, condensed matter physics, and technologies ranging from computer chips to LED lighting ultimately rests.</span></span>  <span data-ttu-id="2bbdb-108">しかし、これらの成功にもかかわらず、量子力学をもってしてもモデル化することができなかった最も単純なシステムもあります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-108">Yet despite these successes, even some of the simplest systems seemed to be beyond the human ability to model with quantum mechanics.</span></span>  <span data-ttu-id="2bbdb-109">これは、たった数十個の粒子が相互作用するシステムのシミュレートであっても、従来のコンピューターでは数千年分以上の処理能力が必要になるためです。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-109">This is because simulating systems of even a few dozen interacting particles requires more computing power than any conventional computer can provide over thousands of years!</span></span>

<span data-ttu-id="2bbdb-110">量子力学をシミュレートすることが難しい理由を理解するには、さまざまな方法があります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-110">There are many ways to understand why quantum mechanics is hard to simulate.</span></span>  <span data-ttu-id="2bbdb-111">おそらく、量子論を解釈する最も簡単な方法が、物質は、量子レベルでは同時に異なる複数の配位にある (*状態*) と理解することです。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-111">Perhaps the simplest is to see that quantum theory can be interpreted as saying that matter, at a quantum level, is simultaneously in a host of different possible configurations (known as *states*) at the same time.</span></span>  <span data-ttu-id="2bbdb-112">従来の確率論とは異なり、このような多くの量子状態の配位は、観察することで波のように干渉する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-112">Unlike classical probability theory, these many configurations of the quantum state, which can be potentially observed, may interfere with each other like waves in a tide pool.</span></span>  <span data-ttu-id="2bbdb-113">この干渉により、統計サンプリングを使用して量子状態の配位を取得することは困難です。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-113">This interference prevents the use of statistical sampling to obtain the quantum state configurations.</span></span>  <span data-ttu-id="2bbdb-114">量子進化について理解するには、代わりに量子システムで*可能なすべての*配位を追跡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-114">Rather, we have to track *every possible* configuration a quantum system could be in if we want to understand the quantum evolution.</span></span>  

<span data-ttu-id="2bbdb-115">ここで、$40$ 通りの配置が可能な原子システムを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-115">Consider a system of electrons where electrons can be in any of say $40$ positions.</span></span>  <span data-ttu-id="2bbdb-116">この場合、原子は $2^{40}$ 通りのいずれかの配位になります (配置ごとに電子が存在するかどうかが異なるため)。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-116">The electrons therefore may be in any of $2^{40}$ configurations (since each position can either have or not have an electron).</span></span> <span data-ttu-id="2bbdb-117">原子の量子状態を従来のコンピューターのメモリに格納するには、$130$ GB を超えるメモリが必要になります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-117">To store the quantum state of the electrons in a conventional computer memory would require in excess of $130$ GB of memory!</span></span>  <span data-ttu-id="2bbdb-118">非常に大きな数ですが、一部のコンピューターでは対応可能です。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-118">This is substantial, but within the reach of some computers.</span></span>  <span data-ttu-id="2bbdb-119">粒子の位置を $41$ 通りにすると、$2^{41}$ 通りと比べて 配置の数は 2 倍になります。この場合、量子状態を格納するには $260$ GB 以上のメモリが必要になります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-119">If we allowed the particles to be in any of $41$ positions, there would be twice as many configurations at $2^{41}$ which in turn would require more than $260$ GB of memory to store the quantum state.</span></span> <span data-ttu-id="2bbdb-120">今後も状態を保存する場合、存在している最も強力なメモリの容量を超えてしまうため、配置の数を永久に増やし続けることはできません。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-120">This game of increasing the number of positions cannot be played indefinitely if we want to store the state conventionally as we quickly exceed memory capacities of the world's most powerful machines.</span></span>  <span data-ttu-id="2bbdb-121">原子の数が数百を超えた時点で、システムを格納するために必要なメモリ数は全宇宙に存在する粒子の数を超えるため、従来のコンピューターでは量子力学をシミュレートすることは不可能です。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-121">At a few hundred electrons the memory required to store the system exceeds the number of particles in the universe; thus there is no hope with our conventional computers to ever simulate their quantum dynamics.</span></span> <span data-ttu-id="2bbdb-122">それでもなお、自然界のこのようなシステムは、従来の計算能力ではこの進化を設計およびシミュレートできないことを横目に、量子力学の法則に従って時間の経過と共に進化を続けています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-122">And yet in nature, such systems readily evolve in time according to quantum mechanical laws, blissfully unaware of the inability to engineer and simulate their evolution with conventional computing power.</span></span>

<span data-ttu-id="2bbdb-123">この観察によって、このような問題をどのようにチャンスに変えることができるのか、という量子力学の初期構想につながるシンプルかつ強力な疑問が問われることになったのです。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-123">This observation led those with an early vision of quantum computing to ask a simple yet powerful question: can we turn this difficulty into an opportunity?</span></span>  <span data-ttu-id="2bbdb-124">具体的には、もし量子力学のシミュレーションを行うことが困難な場合、基本演算に量子効果を採用したハードウェアを構築した場合に何が起こるかという問いです。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-124">Specifically, if quantum dynamics are hard to simulate what would happen if we were to build hardware that had quantum effects as fundamental operations?</span></span>  <span data-ttu-id="2bbdb-125">自然界で粒子を支配している法則と全く同じシステムを使用して、粒子が相互作用するシステムをシミュレートすることはできるのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-125">Could we simulate systems of interacting particles using a system that exploits exactly the same laws that govern them naturally?</span></span> <span data-ttu-id="2bbdb-126">本質的に自然界には存在しないが、量子力学の法則に従う、または恩恵を受けているタスクを調査することはできるのでしょうか。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-126">Could we investigate tasks that are entirely absent from nature, yet follow or benefit from quantum mechanical laws?</span></span>  <span data-ttu-id="2bbdb-127">このような質問が、量子コンピューティングの起源となりました。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-127">These questions led to the genesis of quantum computing.</span></span>

<span data-ttu-id="2bbdb-128">量子コンピューティングの根幹では、プログラムの量子干渉を利用および学習し、情報を物質の量子状態に格納して、その情報に対して量子ゲート演算を使用して計算を実行します。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-128">The foundational core of quantum computing is to store information in quantum states of matter and to use quantum gate operations to compute on that information, by harnessing and learning to "program" quantum interference.</span></span>  <span data-ttu-id="2bbdb-129">従来のコンピューターでは困難と考えられていた問題を解決するためのプログラミング干渉の初期の例は、1994に [Peter Shor](https://en.wikipedia.org/wiki/Peter_Shor) が行ったファクタリングと呼ばれる問題です。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-129">An early example of programming interference to solve a problem thought to be hard on our conventional computers was done by [Peter Shor](https://en.wikipedia.org/wiki/Peter_Shor) in 1994 for a problem known as factoring.</span></span>  <span data-ttu-id="2bbdb-130">ファクタリングを解決することで、RSA や楕円曲線暗号など、現在の e コマースのセキュリティの基盤となる公開キー暗号システムの多くを破ることができます。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-130">Solving factoring brings with it the ability to break many of our public key cryptosystems underlying the security of e-commerce today, including RSA and Elliptic Curve Cryptography.</span></span>  <span data-ttu-id="2bbdb-131">その後、高速で効率的な量子コンピューターのアルゴリズムが、化学、物理的、マテリアルサイエンスにおける物理システムのシミュレーション、順序付けられていないデータベースの検索、線型方程式系の計算、および機械学習のような多くのハードな従来型のタスク向けに開発されています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-131">Since that time, fast and efficient quantum computer algorithms have been developed for many of our hard classical tasks: simulating physical systems in chemistry, physics, and materials science, searching an unordered database, solving systems of linear equations, and machine learning.</span></span>

<span data-ttu-id="2bbdb-132">干渉を活用するよう量子プログラムを設計するのは困難な課題のようにも思えます。もちろんこれは間違いではありませんが、同時に、量子プログラミングとアルゴリズムの開発をより身近にする、Microsoft Quantum Development Kit を含む多くの手法とツールが登場しています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-132">Designing a quantum program to harness interference may sound like a daunting challenge, and while it is, many techniques and tools, including our Microsoft Quantum Development Kit, have been introduced to make quantum programming and algorithm development more accessible.</span></span> <span data-ttu-id="2bbdb-133">ソリューションがあらゆる可能性のもつれの中失われたないようにしつつ、計算を行う上で便利なように量子の干渉を操作する基本的な手法は多数存在します。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-133">There are a handful of basic strategies that can be used to manipulate quantum interference in a way useful for computing, while at the same time not causing the solution to be lost in a tangle of quantum possibilities.</span></span> <span data-ttu-id="2bbdb-134">量子プログラミングは、量子アルゴリズムの考え方を理解し、表現するためのツールが非常に異なる従来のプログラミングとは全く異なるものです。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-134">Quantum programming is a distinct art from classical programming requiring very different tools to understand and express quantum algorithmic thinking.</span></span> <span data-ttu-id="2bbdb-135">ご想像の通り、量子開発者を支援するための一般的なツールがなければ、量子プログラミングの技術である量子アルゴリズムの開発は容易ではありません。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-135">Indeed, without general tools to aid a quantum developer in tackling the art of quantum programming, quantum algorithmic development is not so easy.</span></span>

<span data-ttu-id="2bbdb-136">Microsoft Quantum Development Kit は、成長を続けるコミュニティに、タスク、問題、ソリューションの量子革命をもたらすツールを提供します。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-136">We present the Microsoft Quantum Development Kit to empower a growing community with tools to unlock the quantum revolution for their tasks, problems, and solutions.</span></span> <span data-ttu-id="2bbdb-137">Microsoft の高度なプログラミング言語である Q# は、量子プログラミングの課題に対処するよう設計されています。これはソフトウェア スタックに統合されており、量子アルゴリズムを量子コンピューターのプリミティブ操作にコンパイルできるようになります。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-137">Our high-level programming language, Q#, was designed to address the challenges of quantum information processing; it is integrated in a software stack that enables a quantum algorithm to be compiled down to the primitive operations of a quantum computer.</span></span>  <span data-ttu-id="2bbdb-138">プログラミング言語を扱う前に、量子コンピューティングの基になっている基本的な原則を確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-138">Before approaching the programming language, it's helpful to review the basic principles on which quantum computing is based.</span></span> <span data-ttu-id="2bbdb-139">量子力学の基礎を詳述する代わりに、量子コンピューティングの基本的な規則は自明の理として扱います。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-139">We will take the fundamental rules of quantum computing to be axioms, rather than detailing their foundations in quantum mechanics.</span></span> <span data-ttu-id="2bbdb-140">また、線形代数 (ベクトル、マトリクスなど) に関する基本的な知識を前提としています。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-140">Additionally, we will assume basic familiarity with linear algebra (vectors, matrices, and so on).</span></span> <span data-ttu-id="2bbdb-141">量子コンピューティングの歴史と原則をより深く学ぶには、詳しい情報が記載されている[リファレンス セクション](xref:microsoft.quantum.more-information)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2bbdb-141">If a deeper study of quantum computing history and principles is desired, we refer you to the  [reference section](xref:microsoft.quantum.more-information) containing more information.</span></span>