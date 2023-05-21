#### Stability in Sorting Algorithms.
```
A stable sorting algorithm maintains the relative order of the items with equal sort keys. 
An unstable sorting algorithm does not. In other words, when a collection is sorted with a stable sorting algorithm, 
items with the same sort keys preserve their order after the collection is sorted.
```


# Bubble Sort

```
Basically in every iteration we move the highest value at the last (0 to n-i-1). 
```

```c++
vector<int> BubbleSort(vector<int>nums) {

  int length = nums.size();
  for(int i = 0; i < length; i++) {
    bool swapped = false;
    for(int j = 0; j < length - i - 1; j++) {
      if(nums[j] > nums[j + 1]) {
        swap(nums[j], nums[j + 1]);
        swapped = true;
      }
    }
    if(!swapped) break;
  }

  return nums;
}

```


# Selection Sort

```
It basically select next minimum value from unsorted array and push back to the sort array
```

```c++
vector<int> SelectionSort(vector<int>nums) {

  int length = nums.size();
  for(int i = 0; i < length; i++) {
    int minIndex = i;
    for(int j = i + 1; j < length; j++) {
      if(nums[minIndex] > nums[j]) minIndex = j;
    }
    if(minIndex > i) swap(nums[i], nums[minIndex]);
  }

  return nums;
}
```

# Insersion Sort

```
It basically take the first element of right unsorted array and keep the value in the right position in the left sorted array
```

```c++
vector<int> InsersionSort(vector<int>nums) {

  int length = nums.size();
  for(int i = 1; i < length; i++) {
    int key = nums[i];
    int j = i - 1;

    while(j >= 0 && nums[j] > key) {
      nums[j + 1] = nums[j];
      j--;
    }

    nums[j + 1] = key;
  }

  return nums;
}
```


# Quick Sort

```
It uses the divide and conquer algorithm. It takes a element as pivot ( Usaually takes the last element as pivot).
then move all the value less than pivot move in the left and rest of the value keep in the right. 
```

```c++
int Partition(vector<int>&nums, int low, int high) {

  int pivot = nums[high];

  int i = low;

  for(int j = low; j < high; j++) {
    if(nums[j] < pivot) {
      swap(nums[i], nums[j]);
      i++;
    }
  }
  if(nums[i] > pivot) {
    swap(nums[i], nums[high]);
  }

  return i;
}



void QuickSort(vector<int>&nums, int low, int high) {

  if(low < high) {
    int pi = Partition(nums, low, high);
    QuickSort(nums, low, pi - 1);
    QuickSort(nums, pi + 1, high);
  }
}

```


# Merge Sort

```
It also uses the divide and conquer algorithm. First divide the array into some smaller array. Then Merge the smaller array.
```

```c++
void Merge(vector<int>&nums, int begin, int mid, int end) {
  vector<int>temp;
  int i = begin;
  int j = mid;

  while(i < mid || j <= end) {
    if(i == mid) {
      temp.push_back(nums[j++]);
    }else if(j == end + 1) {
      temp.push_back(nums[i++]);
    }else{
      if(nums[i] < nums[j]) temp.push_back(nums[i++]);
      else temp.push_back(nums[j++]);
    }
  }

  for(i = begin, j = 0; i <= end; i++,j++) {
    nums[i] = temp[j];
  }
}

void MergeSort(vector<int>&nums, int begin, int end) {
  if(begin >= end) return;

  int mid = (begin + end + 1) / 2;
  MergeSort(nums, begin, mid - 1);
  MergeSort(nums, mid, end);
  Merge(nums, begin, mid, end);
}
```

# Counting Sort

```
First keep the element count. Then place the elements in the array based on the count value.
```

```c++
void CountingSort(vector<int>&nums) {
  vector<int>count(100, 0);
  vector<int>output(nums.size());

  for(int x : nums) count[x]++;
  for(int i = 1; i<100; i++) count[i] += count[i - 1];

  for(int x : nums) {
    output[--count[x]] = x;
  }
  nums = output;
}
```

# Redix Sort

```
It sort the value digit by digit. For sorting each digit it uses the counting sort.
```

```c++
void CountingSortForRedixSort(vector<int>&nums, int exp) {
  int length = nums.size();
  vector<int>output(length), count(10, 0);

  for(int x : nums) count[(x / exp) % 10]++;
  for(int i = 1; i<9; i++) count[i] += count[i - 1];

  for(int i = length - 1; i>=0; i--) {
    output[--count[(nums[i] / exp) % 10]] = nums[i];
  }
  nums = output;
}


void RedixSort(vector<int>&nums) {
  int mx = 0;
  for(int x : nums) mx = max(mx, x);

  for(int exp = 1; mx / exp > 0; exp *= 10) {
    CountingSortForRedixSort(nums, exp);
  }
}
```

# Heap Sort

```c++
#include <bits/stdc++.h>
using namespace std;


void Max_Heapify(vector<int>&nums, int i, int N) {
  int left = i * 2;
  int right =  i * 2 + 1;

  int largest = i;

  if(left <= N && nums[left - 1] > nums[i - 1]) {
    largest = left;
  }

  if(right <= N && nums[right - 1] > nums[largest - 1]) {
    largest = right;
  }

  if(largest != i) {
    swap(nums[i - 1], nums[largest - 1]);
    Max_Heapify(nums, largest, N);
  }
}


void Build_Maxheap(vector<int>&nums) {
  int N = nums.size();

  for(int i = N / 2; i>=1; i--) {
    Max_Heapify(nums, i, N);
  }
}


void HeapSort(vector<int>&nums) {
  int N = nums.size();
  int heapSize = N;
  Build_Maxheap(nums);

  for(int i = N; i>=2; i--) {
    swap(nums[0], nums[i - 1]);
    heapSize--;
    Max_Heapify(nums, 1, heapSize);
  }
}

int getMaximum(vector<int>&nums) {
  return nums[0];
}

int extractMaximum(vector<int>&nums) {
  if(nums.size() == 0) {
    cout << "Can’t remove element as queue is empty" << endl;
    return -1;
  }

  int max = nums[0];
  nums[0] = nums.back();
  nums.pop_back();

  Max_Heapify(nums, 1, nums.size());

  return max;
}


void addItem(vector<int>&nums, int val) {
  nums.push_back(val);
  int i = nums.size();

  while(i > 1 && nums[i - 1] > nums[(i / 2) - 1]) {
    swap(nums[i - 1], nums[(i / 2) - 1]);
    i = i / 2;
  }
}


void solve(){
  vector<int> nums = {4,3,5,1,2};

  // Build_Maxheap(nums);

  // extractMaximum(nums);

  // HeapSort(nums);
  addItem(nums, 3);
  hea

  for(auto x : nums) cout << x << endl;
}

int32_t main(){
  #ifndef ONLINE_JUDGE
  freopen("input.txt", "r", stdin);
  freopen("output.txt", "w", stdout);
  #endif

  int tt; tt = 1;
  int cas = 1;
  while(tt--){
    //printf("Case %d: ",cas++);
    solve();
  }
}
```


# Bucket Sort

```
Firstly we devide the elements into some bucket, then sort all the bucket.
```


```c++
void BucketSort(vector<int>&nums) {
  int mx = 0;
  for(int x : nums) mx = max(mx, x);

  vector<int>bucket[10];
  
  for(int x : nums) {
    bucket[x / 10].push_back(x);
  }
  for(int i = 0; i<10; i++){
    sort(bucket[i].begin(), bucket[i].end());
  }

  nums.clear();
  for(int i = 0; i<10; i++) {
    for(int x : bucket[i]) {
      nums.push_back(x);
    }
  }
}
```


| Name | Best Case | Average Case | Worst Case | Memory |
| ------------------- | ----------- | ----------- | ----------- | ----------- |
| Bubble Sort |  O(N)  |  O(N^2) | O(N^2) | O(1) |
| Selection Sort | O(N^2) | O(N^2) | O(N^2) | O(1) |
| Insersion Sort | O(N^2) | O(N^2) | O(N^2) | O(N^2) | O(1) |
| Quick Sort | O(NLogN) | O(NLogN) | O(N^2) | O(LogN)(For Call Stack) |
| Merge Sort | O(NLogN) | O(NLogN) | O(NLogN) | O(N) |
| Counting Sort***** | O(N + K) | O(N + K) | O(N + K) | O(N + K) |
| Redix Sort*** | O(N * K) | O(N * K) | O(N * K) | O(N + K) |
| Bucket Sort** | O(N + K) | O(N + K) | O(NLogN) | O(N) |
| Heap Sort | O(NLogN) | O(NLogN) | O(NLogN) | O(1) |

****** K = represent the difference between the smallest and largest value in the array <br/>
*** K = represents the number of digit of the leargest value in the array <br/>
** K = represents the number of bucket
