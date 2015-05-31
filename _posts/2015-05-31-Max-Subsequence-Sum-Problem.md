---
title: Max Subsequence Sum Problem
author: Flowerowl
layout: post
permalink: /4009.html
views:
  - 667
categories:
  - Algorithm
tags:
  - Algorithm
---

The maximum subarray problem is the task of finding the contiguous subarray within a one-dimensional array of numbers (containing at least one positive number) which has the largest sum.


/*
 *O(N^3)
 */
int
MaxSubsequenceSum(const int A[], int N){
    int ThisSum, MaxSum, i, j, k;

    MaxSum = 0;
    for (i=0; i<N; i++){
        for (j=i; j<N; j++){
            ThisSum = 0;
            for(k=i; k<=j; k++){
                ThisSum += A[k];
            }

            if (ThisSum > MaxSum){
                MaxSum = ThisSum;
            }
        }
    }
    return MaxSum;
}

/*
 *O(N^2)
 */
int
MaxSubsequenceSum(const int A[], int N){
    int ThisSum, MaxSum, i, j;

    MaxSum = 0;
    for (i=0; i<N; i++){
        ThisSum = 0;
        for (j=i; j<N; j++){
            ThisSum += A[j];

            if (ThisSum > MaxSum){
                MaxSum = ThisSum;
            }
        }
    }
    return MaxSum;
}

/*
 *O(NlogN)
 */
int
MaxSubsequenceSum(const int A[], int Left, int Right){
    int MaxLeftSum, MaxRightSum;
    int MaxLeftBorderSum, MaxRightBorderSum;
    int LeftBorderSum, RightBorderSum;
    int Center, i;

    if (Left == Right){
        if (A[Left] > 0){
            return A[Left];
        } else {
            return 0;
        }
    }

    Center = (Left + Right) / 2;
    MaxLeftSum = MaxSubsequenceSum(A, Left, Center);
    MaxRightSum = MaxSubsequenceSum(A, Center+1, Right);

    MaxLeftBorderSum = 0; LeftBorderSum = 0;
    for (i=Center; i>=Left; i--){
        LeftBorderSum += A[i];
        if (LeftBorderSum > MaxLeftBorderSum){
            MaxLeftBorderSum = LeftBorderSum;
        }
    }

    MaxRightBorderSum = 0; RightBorderSum = 0;
    for (i=Center+1; i<=Right; i++){
        RightBorderSum += A[i];
        if (RightBorderSum > MaxRightBorderSum){
            MaxRightBorderSum = RightBorderSum;
        }
    }

    return MAX(MAX(MaxLeftSum, MaxRightSum), MaxLeftBorderSum + MaxRightBorderSum);
}

/*
 *O(N)
 */
int
MaxSubsequenceSum(const int A[], int N){
    int ThisSum, MaxSum, i;

    ThisSum = MaxSum = 0;
    for (i=0; i<N; i++){
        ThisSum += A[i];

        if (ThisSum > MaxSum){
            MaxSum = ThisSum;
        } else if(ThisSum < 0){
            ThisSum = 0;
        }
    }
    return MaxSum;
}

### Reference:

Data Structures and Algorithm Analysis in C (2nd Edition) Chapter2
