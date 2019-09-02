#include<stdio.h>

/////////////////////////////////////////////// Functions
void sort(int* a, int length);
void mergeSort(int* a, int left, int right);
void merge(int* a, int left, int mid, int right);

/////////////////////////////////////////////// Test cases
void testMergeSort();
void testMerge();
/////////////////////////////////////////////// Helpers
void printArray(int* a, int length);

/////////////////////////////////////////////// Functions
void sort(int* a, int length)
{
    mergeSort(a, 0, length);
}

void mergeSort(int* a, int left, int right)
{
    if(left < right)
    {
        int mid = (left + right) / 2;
        mergeSort(a, left, mid);
        mergeSort(a, mid + 1, right);
        merge(a, left, mid, right);
    }
}

void merge(int* a, int left, int mid, int right)
{
    int leftIndex = 0, rightIndex = 0, mergedIndex = left;
    int leftSize  = mid - left;
    int rightSize = right - mid;
    
    int L[leftSize], R[rightSize];
    
    for(int i = 0 ; i < leftSize ; i++)
        L[i] = a[left + i];
    
    for(int j = 0 ; j < rightSize ; j++)
        R[j] = a[mid + j];
    
    while(leftIndex < leftSize && rightIndex < rightSize)
    {
        if(L[leftIndex] <= R[rightIndex])
        {
            a[mergedIndex++] = L[leftIndex++];
        }
        else
        {
            a[mergedIndex++] = R[rightIndex++];
        }
    }
    
    while(leftIndex < leftSize)
    {
        a[mergedIndex++] = L[leftIndex++];
    }
    
    while(rightIndex < rightSize)
    {
        a[mergedIndex++] = R[rightIndex++];
    }
}

/////////////////////////////////////////////// Testcases
void testMergeSort()
{
    int a1[] = {7, 4, 2, 3, 9, 1, 1, 10};
    sort(a1, 8);
    printArray(a1, 8);
    
    int a2[] = {74, 34, 62, 73, 29, 91, 81, 510};
    sort(a2, 8);
    printArray(a2, 8);
    
    int a3[] = {9, 1, 1, 10, 9, 1, 1, 10};
    sort(a3, 8);
    printArray(a3, 8);
}

void testMerge()
{
    int a1[] = {1, 2, 3, 2, 3, 4};
    merge(a1, 0, 6/2, 6);
    printArray(a1, 6);
}

/////////////////////////////////////////////// Helpers
void printArray(int* a, int length)
{
    for(int i = 0 ; i < length - 1 ; i++)
        printf("%d, ", a[i]);
    printf("%d\n", a[length - 1]);
}


////////////////////////////////////////////// Driver

int main() {
   //testMerge();
   testMergeSort();
}