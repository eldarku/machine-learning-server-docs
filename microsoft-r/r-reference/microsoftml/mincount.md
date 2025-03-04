--- 

# required metadata 
title: "minCount function (MicrosoftML) " 
description: " Count mode of feature selection used in the feature selection transform [selectFeatures](selectFeatures.md). " 
keywords: "(MicrosoftML), minCount, count, feature, selection" 
author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 




 # minCount: Feature Selection Count Mode 
 ## Description

Count mode of feature selection used in the feature selection transform
[selectFeatures](selectFeatures.md).


 ## Usage

```   
  minCount(count = 1, ...)

```

 ## Arguments



 ### `count`
 The threshold for count based feature selection. A feature is selected if and only if at least `count` examples have non-default value in the feature. The default value is 1. 



 ### ` ...`
 Additional arguments to be passed directly to the Microsoft Compute Engine. 



 ## Details

When using the count mode in feature selection transform, a feature is
selected if the number of examples have at least the specified count
examples of non-default values in the feature. The count mode feature
selection transform is very useful when applied together with a categorical
hash transform (see also, [categoricalHash](categoricalHash.md). The count feature
selection can remove those features generated by hash transform that have no
data in the examples.


 ## Value

A character string defining the count mode.

 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## See Also

[mutualInformation](mutualInformation.md) [selectFeatures](selectFeatures.md)

 ## Examples

 ```

  trainReviews <- data.frame(review = c( 
          "This is great",
          "I hate it",
          "Love it",
          "Do not like it",
          "Really like it",
          "I hate it",
          "I like it a lot",
          "I kind of hate it",
          "I do like it",
          "I really hate it",
          "It is very good",
          "I hate it a bunch",
          "I love it a bunch",
          "I hate it",
          "I like it very much",
          "I hate it very much.",
          "I really do love it",
          "I really do hate it",
          "Love it!",
          "Hate it!",
          "I love it",
          "I hate it",
          "I love it",
          "I hate it",
          "I love it"),
       like = c(TRUE, FALSE, TRUE, FALSE, TRUE,
          FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE,
          FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, 
          FALSE, TRUE, FALSE, TRUE), stringsAsFactors = FALSE
      )

      testReviews <- data.frame(review = c(
          "This is great",
          "I hate it",
          "Love it",
          "Really like it",
          "I hate it",
          "I like it a lot",
          "I love it",
          "I do like it",
          "I really hate it",
          "I love it"), stringsAsFactors = FALSE)

  # Use a categorical hash transform which generated 128 features.
  outModel1 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7)))
  summary(outModel1)

  # Apply a categorical hash transform and a count feature selection transform
  # which selects only those hash features that has value.
  outModel2 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(
    categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7), 
    selectFeatures("reviewCatHash", mode = minCount())))
  summary(outModel2)

  # Apply a categorical hash transform and a mutual information feature selection transform
  # which selects those features appearing with at least a count of 5.
  outModel3 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(
    categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7), 
    selectFeatures("reviewCatHash", mode = minCount(count = 5))))
  summary(outModel3)
```





