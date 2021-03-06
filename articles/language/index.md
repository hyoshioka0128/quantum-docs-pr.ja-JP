---
title: Q# プログラミング言語
description: 量子プログラム開発のための Q# 言語の概要。
author: QuantumWriter
ms.author: Alan.Geller@microsoft.com
ms.date: 12/11/2017
ms.topic: article
uid: microsoft.quantum.language.intro
ms.openlocfilehash: b62e6866fc3609d95c26a5eab2a6eac325dfe330
ms.sourcegitcommit: 9d1c045cf1a2c3e19030cb38dbc7496dbd24ab58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "77907377"
---
# <a name="the-q-programming-language"></a>Q# プログラミング言語

## <a name="introduction"></a>はじめに

量子コンピューティングの自然モデルは、GPU、FPGA、およびその他の補完プロセッサで使用されるものと同様に、量子コンピューターをコプロセッサとして扱います。
主要な制御ロジックは、従来の "ホスト" コンピューターでクラシック コードを実行します。
必要に応じて、ホスト プログラムは補完プロセッサ上で実行されるサブルーチンを呼び出すことができます。
サブルーチンが完了すると、ホスト プログラムはサブルーチンの結果にアクセスできるようになります。

Q# (Q シャープ) は、量子アルゴリズムの表現に使用されるドメイン固有のプログラミング言語です。
これは、従来型のホスト プログラムとコンピューターのコントロール下で、補完量子プロセッサ上で実行されるサブルーチンの作成に使用されます。
量子プロセッサが普及するまで、Q# サブルーチンはシミュレーターで実行されます。

Q# には、プリミティブ型の小さなセットと、新しい構造化型を作成するための 2 つの方法 (配列とタプル) が用意されています。
ループと if/then ステートメントを使用してプログラムを作成するための基本的な手続き型のモデルをサポートします。
Q# の最上位レベルのコンストラクトは、ユーザー定義型、操作、および関数です。

次のセクションで詳細を説明します。
- [タイプ モデル](xref:microsoft.quantum.language.type-model)
- [式](xref:microsoft.quantum.language.expressions)
- [ステートメント](xref:microsoft.quantum.language.statements)
- [ファイル構造](xref:microsoft.quantum.language.file-structure)

## <a name="conventions"></a>規則

一般的な句読点がすべての状況で一貫して使用されるようにするために取り組んでいます。
このような記号は常に同じことを意味し、同じ概念は常に同じ方法で表されるため、Q# が学習しやすく、かつ読みやすくなることが期待されます。

具体的な内容は次のとおりです。

- セミコロン (`;`) は、ステートメントまたは単一行のディレクティブを終了するために使用されます。
  他の目的では使用できません。
- コンマ (`,`) は、シーケンスの要素を区切るために使用されます。 これは、タプル リテラル、配列リテラル、引数リスト、タプル定義、および型リストに使用されます。 **以前のバージョンからの変更では、`;` は配列リテラルの区切りとしてサポートされなくなりました。**
- コロン (`:`) は、型の注釈を挿入するために使用されます。 これは主に呼び出し可能なシグネチャで使用されます。
  コロンは常に型シグネチャを挿入するため、0.3 で導入された三項条件付き演算子では、垂直バー (`|`) を使用して、true と false の値を区切ります。このため、Q# では、コロンではなく、C のように `cond ? tval | fval` を 使用します。
  
