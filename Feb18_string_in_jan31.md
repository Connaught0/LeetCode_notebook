# carlProgram 54
主要是了解一下ACM模式。。。
好久没写纯c++的这种代码了cout和cin都忘记了

solution：2180kb
```
#include<iostream>
using namespace std;
int main()
{
    string s;
    while(cin>>s){
        int count = 0;
        int oldSize = s.size();

        for(int i = 0; i < oldSize; i++){
            if(s[i] <= '9' && s[i] >= '0'){
                count++;
            }
        }
        int newSize = oldSize + (count*(6-1)); 
        s.resize(newSize);

        int i = newSize - 1;//point to the tale of new string 
        for(int j = oldSize - 1; j >= 0; j--)
        {
            if(s[j] > '9' || s[j] < '0')
            {
                s[i] = s[j];
                i--;
            }
            else
            {
                s[i] = 'r';
                s[i - 1] = 'e';
                s[i - 2] = 'b';
                s[i - 3] = 'm';
                s[i - 4] = 'u';
                s[i - 5] = 'n';
                i -= 6;
            }
        }
        cout<<s;
    }
    return 0;
}
```
如果一般我的思路话就是遍历数组，然后把前面和后面解开，往中间插number，这种写法似乎只有python能够这么做。 2180kb
solution：
```
#include<iostream>
using namespace std;
int main()
{
    string s;
    while(cin>>s){
        int count = 0;
        int oldSize = s.size();
        string ans = "";
        for(int i = 0; i < oldSize; i++){
            if(s[i] <= '9' && s[i] >= '0')
            {
                ans += "number";
            }
            else
            {
                ans += s[i];
            }
        }
        cout<<ans;
    }
    return 0;
}
```