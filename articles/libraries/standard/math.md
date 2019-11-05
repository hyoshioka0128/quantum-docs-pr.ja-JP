---
title: 'Q # 標準ライブラリ-math |Microsoft Docs'
description: 'Q # 標準ライブラリ-数値演算'
author: cgranade
uid: microsoft.quantum.libraries.math
ms.author: chgranad@microsoft.com
ms.topic: article
ms.openlocfilehash: 7ac9d321f59f7cc95ad8939a4cf7c36e0c5e0491
ms.sourcegitcommit: 8becfb03eb60ba205c670a634ff4daa8071bcd06
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2019
ms.locfileid: "73184459"
---
# <a name="classical-mathematical-functions"></a><span data-ttu-id="9851e-103">古典数学関数</span><span class="sxs-lookup"><span data-stu-id="9851e-103">Classical Mathematical Functions</span></span> #

<span data-ttu-id="9851e-104">これらの関数は、主に Q # の組み込みデータ型 `Int`、`Double`、および `Range`を操作するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9851e-104">These functions are primarily used to work with the Q# built-in data types `Int`, `Double`, and `Range`.</span></span>

<span data-ttu-id="9851e-105"><xref:microsoft.quantum.intrinsic.random> 操作にはシグネチャ `(Double[] => Int)`があります。</span><span class="sxs-lookup"><span data-stu-id="9851e-105">The <xref:microsoft.quantum.intrinsic.random> operation has signature `(Double[] => Int)`.</span></span>
<span data-ttu-id="9851e-106">このメソッドは、入力として double の配列を取得し、ランダムに選択されたインデックスを `Int`として配列に返します。</span><span class="sxs-lookup"><span data-stu-id="9851e-106">It takes an array of doubles as input, and returns a randomly-selected index into the array as an `Int`.</span></span>
<span data-ttu-id="9851e-107">特定のインデックスを選択する確率は、そのインデックス位置にある配列要素の値に比例します。</span><span class="sxs-lookup"><span data-stu-id="9851e-107">The probability of selecting a specific index is proportional to the value of the array element at that index.</span></span> <span data-ttu-id="9851e-108">0に等しい n 個の配列要素は無視され、そのインデックスは返されません。</span><span class="sxs-lookup"><span data-stu-id="9851e-108">n Array elements that are equal to zero are ignored and their indices are never returned.</span></span>
<span data-ttu-id="9851e-109">配列要素が0未満の場合、または配列要素がゼロより大きい場合、操作は失敗します。</span><span class="sxs-lookup"><span data-stu-id="9851e-109">If any array element is less than zero, or if no array element is greater than zero, then the operation fails.</span></span>