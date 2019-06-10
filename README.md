# hellow-world
jianduan
#include<iostream>
#include<algorithm>
using namespace std;

bool cmp(int a,int b)
{
    return a<b;
}
int main()
{
    int num[]={2,3,1,6,4,5};
    cout<<"最小值是 "<<*min_element(num,num+6)<<endl;
    cout<<"最大值是 "<<*max_element(num,num+6)<<endl;
    cout<<"最小值是 "<<*min_element(num,num+6,cmp)<<endl;
    cout<<"最大值是 "<<*max_element(num,num+6,cmp)<<endl;
    return 0; 
}
//最小值是 1
//最大值是 6
//最小值是 1
//最大值是 6
