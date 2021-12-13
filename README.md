# interviewbit--math--sorted-permutation-rank-with-repeats


**------> Question:**

Given a string, find the rank of the string amongst its permutations sorted lexicographically. 

Note that the characters might be repeated. If the characters are repeated, we need to look at the rank in unique permutations. 

Look at the example for more details.

Example :

Input : 'aba'
Output : 2

The order permutations with letters 'a', 'a', and 'b' : 
aab
aba
baa
The answer might not fit in an integer, so return your answer % 1000003

NOTE: 1000003 is a prime number

NOTE: Assume the number of characters in string < 1000003


**------>code:**

#define ll long long

ll fact(int n){
    ll f=1; 
    for(int i=2;i<=n;i++){
        f = (f*i)%1000003;
    }
    return f;
}
ll power(ll x,ll y,ll mod){
    ll res=1; 
    while(y){
        if(y&1) res = (res*x)%mod; 
        y>>=1; 
        x = (x*x)%mod;
    }
    return res;
}

int Solution::findRank(string s) {
     int n = s.size(); 
    ll mod = 1000003;
    ll ans = 1;  
    for (int i = 0; i < n; i++) { 
        ll less_than = 0; 
        for (int j = i + 1; j < n; j++) { 
            if ((int)s[i] > (int)s[j]) { 
                less_than += 1; 
            } 
        } 
        vector<int> freq(52, 0); 
        for (int j = i; j < n; j++) { 
            if(s[j]>='A' && s[j]<='Z')
                freq[s[j]-'A'] += 1;
            else
                freq[s[j] - 'a' + 26] += 1; 
        } 
        ll denominator = 1; 
        for (ll ele : freq) {
            denominator = (denominator*fact(ele))%mod; 
        }
        
        ll inverse = power(denominator,mod-2,mod); 
        
        ll permutation = (fact(n-i-1) * inverse) % mod;
        ll total_permutation = (less_than * permutation) % mod;
        
        ans = (ans + total_permutation)%mod; 
    } 
    return ans;
}
