## 算法竞赛进阶指南, 0x10, 130. 火车进栈
Link: https://www.acwing.com/problem/content/description/132/

this is  hard (many steps) question. 

First: to figure out how many ways the train can be enter and leave the train station, looking the graph. The train has to go into station before it leave the station.  if we use '+' indicates enter and '-' indicates leave. then at any time the number of the '+' must be larger than the number of  '-'. So this problem become some like "There are n '+' and n'-', how many ways we can arrange the + and - to make sure before any location. number of '+' must be larger than '-'. So This is **Catalan Number**, which is 

This is the code
```
    #include <iostream>
    #include <vector>
    #include <cstring>
    #include <cmath>
    #include <queue>
    #include <stack>

    using namespace std;
    #define maxn 6000010
    #define INF 0x3f3f3f3f

    typedef long long LL;
    typedef pair<int,int> pii;
    typedef vector<pair<int,int> > vii;
    typedef vector<int> vi;
    typedef vector<vector<int> > vvi;
    int n;
    bool isPrime[maxn];
    int q[maxn];
    vector<LL> res(maxn);
    int top;

    // standard way to find all prime number below a.
    // have a boolean array, if i is prime mark isPrime[i] to true.
    void findPrimes(int a) {
    for (int i = 2; i <= a; i++) {
      if (isPrime[i]) {
        for (int j = 2*i; j <=a; j += i) {
          isPrime[j] = false;
        }
      }
    }
    }

    // standar way to multi a large integer (vector res in this case)
    // with n integer.
    void multi(int b) {
    int t = 0;
    for (int i = 0;i <= top; i++) {
      res[i] = res[i] * b + t;
      t = res[i] / 100000000;
      res[i] %= 100000000;
    }
    while(t) {
      res[++top] = t % 100000000;
      t /= 100000000;
    }
    }

    void out() {
    printf("%lld", res[top]);
    top--;
    while(top >= 0) {
      printf("%08lld", res[top]);
      top--;
    }
    cout << endl;
    }

    // find the power of a prime number in n!
    int findPimePowerInFactoria(int n, int p) {
    int s = 0;
    while(n) {
      s += n/p;
      n /=p;
    }
    return s;
    }

    int main() {
    cin >> n;

    for (int i = 2;i <= 2*n; i++) {
      isPrime[i] = true;
    }

    findPrimes(2*n);

    for (int i = 2; i < 2*n; i++) {
      if(isPrime[i]) {
        q[i] = findPimePowerInFactoria(2*n, i)
              - findPimePowerInFactoria(n, i)
              - findPimePowerInFactoria(n+1, i);
      }
    }
    //
    // int k = n+1;
    // for (int i = 2; i <= k; i++) {
    //   while(k%i == 0) {
    //     q[i]--;
    //     k = k/i;
    //   }
    // }

    // for (int i = 0; i < 2*n; i++) {
    //   cout << q[i] << " ";
    // }
    // cout << endl;

    res[0] = 1;
    for (int i = 0;i < 2*n; i++) {
      for (int j = 0; j < q[i]; j++) {
        multi(i);
      }
    }
    // cout << "top " << top << endl;
    out();
    return 0;
    }
```
