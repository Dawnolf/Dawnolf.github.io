
Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`.

Each product is **guaranteed** to fit in a **32-bit** integer.

Follow-up: Could you solve it in O(n)O(n) time without using the division operation?

**Example 1:**

```c++
Input: nums = [1,2,4,6]

Output: [48,24,12,8]
```

**Example 2:**

```c++
Input: nums = [-1,0,1,2,3]

Output: [0,-6,0,0,0]
```

**Constraints:**

- `2 <= nums.length <= 1000`
- `-20 <= nums[i] <= 20`


이 문제는 nums[i]를 제외하고 output[i]에 모든 원소의 곱을 반환하는 문제이다.

## Solution1 

내가 떠올린 방법은 브루트 포스 방법인데,
이중포문을 돌려 i == j 가 같을 경우 그 원소값만 제외하고 곱하여 벡터에 반환하는 방법이다.

```c++ 
class Solution {

public:

    vector<int> productExceptSelf(vector<int>& nums) {

        vector<int> v;

        for(int i=0; i<nums.size(); i++){

            int mlp=1;

            for(int j=0; j<nums.size(); j++){
            
                if(i==j) continue;
                mlp*=nums[j];
                
            }
            v.push_back(mlp);
        }
        return v;
    }
};

```

시간복잡도는 $O(n^2)$
방법은 간단하나 시간복잡도가 비효율적이라 추천하지 않는 해결법이다.

## Solution2

고민을 하다가 떠오르는 방법이 없어서 다른 해결책을 찾았다.
찾은 방법 중 하나가 Division이라는 방법이다. 

0을 제외하고 모든 원소의 곱을 곱하고, 0의 개수를 센다
0의 개수(zeroCount)가 2개 이상일 때는 nums[i]를 제외해도 모두 0이니 0을 가지는 벡터를 return 하면 된다.

1개 이하라면 zeroCount가 1일때만 주의하면 된다.
1개 이상일 때 nums[i]== 0인지 확인하고 0이라면 내 곱이 제외되니(나누지 않아도 됨)
바로 prod를 벡터에 넣어주고
nums[i]가 0이 아닐때는 무조건 0을 곱해야하니 0을 벡터에 넣는다.

```c++
class Solution {

public:

    vector<int> productExceptSelf(vector<int>& nums) {
    
        int prod = 1;
        int zeroCount =0;

        for(int i=0; i<nums.size(); i++){       
            if(nums[i]!=0){
                prod*=nums[i];
            }
            else{
                zeroCount++;
            }
        }

        if(zeroCount>1){
            return vector<int>(nums.size(),0);
        }

        vector<int> v(nums.size());
        for(int i=0; i<nums.size(); i++){
            if(zeroCount>0){
                v[i] = (nums[i]==0) ? prod:0;
            }
            else{
                v[i] = prod/nums[i];
            }
        }
        return v;
    }
};
```

시간복잡도는 $O(n)$

## Solution3

prefix & suffix 알고리즘
조금 한국어로는 애매한데 누적합 & 접미사 배열?을 통합해서 누적곱이라고 하는 것 같다.
일단 2개의 벡터를 만들어서 
왼쪽부터 현재 위치보다 앞에 있는 누적곱과 
오른쪽에서는 현재 위치보다 뒤에 있는 누적곱을 구한다.

그리고 왼쪽 곱 과 오른쪽 곱을 서로 곱하는 해결법이다. 

```c++
class Solution {

public:

    vector<int> productExceptSelf(vector<int>& nums) {

  

        int n = nums.size();

        vector<int> sv(n);

        vector<int> pref(n);

        vector<int> suff(n);

        pref[0] =1;
        suff[n-1] =1;

        for(int i=1; i<n; i++){
            pref[i] = nums[i-1]*pref[i-1];
        }
        
        for(int i = n-2; i>=0; i--){
            suff[i] = nums[i+1] * suff[i+1];
        }

        for(int i=0; i<n; i++){
            sv[i] =  pref[i]*suff[i];
        }

        return sv;

    }

};
```

시간복잡도는 $O(n)$

## Solution4

이건 위의 누적곱에 대한 최적화 방법인데
위의 pref, suff 벡터를 제외하고 계산하는 방법이다.

res 벡터를 모두 1로 초기화 하고, res벡터에서 바로 누적곱(왼쪽에서 오른쪽)을 한다
posfix 변수를 하나 둬서 res벡터에 오른쪽에서 왼쪽으로 바로 곱하여 추가 배열 없이 해결하는 방법이다.

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n, 1);

        for (int i = 1; i < n; i++) {
            res[i] = res[i - 1] * nums[i - 1];
        }

        int postfix = 1;
        for (int i = n - 1; i >= 0; i--) {
            res[i] *= postfix;
            postfix *= nums[i];
        }
        return res;
    }
};
```

시간복잡도는 $O(n)$
시간 복잡도는 그대로지만 여분 배열을 사용하지않아 공간 복잡도가 좋아졌다.