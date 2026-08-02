# Encode and Decode Strings

MediumTopicsCompany TagsHints

Design an algorithm to encode **a list of strings** to **a string**. The **encoded string** is then sent over the network and is **decoded** back to the **original list** of strings.

**Machine 1 (sender)** has the function:

```java
String encode(List<String> strs) {
    // ... your code
    return encoded_string;
}
```

**Machine 2 (receiver)** has the function:

```java
List<String> decode(String encoded_string) {
    // ... your code
    return decoded_strs;
}
```

So **Machine 1** does:

```java
String encoded_string = encode(strs);
```

and **Machine 2** does:

```java
List<String> decoded_strs = decode(encoded_string);
```

`decoded_strs` in Machine 2 should be the **same** as the input `strs` in Machine 1.

Implement the `encode` and `decode` methods.

**Example 1:**

```java
Input: strs = ["Hello","World"]

Output: ["Hello","World"]
```

**Explanation:**

```java
Solution solution = new Solution();
String encoded_string = solution.encode(strs);

// Machine 1 ---encoded_string---> Machine 2

List<String> decoded_strs = solution.decode(encoded_string);
```

  

**Example 2:**

```java
Input: strs = [""]

Output: [""]
```

  

**Constraints:**

- `0 <= strs.length < 100`
- `0 <= strs[i].length < 200`
- `strs[i]` contains any possible characters out of `256` valid ASCII characters.

  

**Follow up:** Could you write a generalized algorithm to work on any possible set of characters?

```
class Solution {

public:

  

    string encode(vector<string>& strs) {

        string str="";

        for(int i =0; i<strs.size(); i++){

            str+=to_string(strs[i].size()) +"#"+strs[i];

        }

        return str;

    }

  

    vector<string> decode(string s) {

        vector<string> v;

        string str = "";

        int i=0;

        while(i<s.length()){

            int j = i;

            while(s[j]!='#'){

                j++;

            }

            int length = stoi(s.substr(i,j-i));

            i=j+1;

            j=i+length;

            v.push_back(s.substr(i,length));

            i =j;

        }

        return v;

    }

};
```

길이만큼 잘라줬다. 복습이 한 번 더 필요한 내용임