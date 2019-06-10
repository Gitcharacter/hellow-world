# hellow-world
jianduan
#include<bits/stdc++.h>
#define N 1200005
#define inf 1000000007
using namespace std;
int mask;char s[3000005];
int Q;
string chars;
void gets(int mask){
    scanf("%s",s);chars=s;
    for(int j=0;j<chars.length();j++){
        mask=(mask*131+j)%chars.length();
        char t=chars[j];
        chars[j]=chars[mask];
        chars[mask]=t;
    }
}
struct Link_Cut_Tree{
    int top,fa[N],c[N][2],val[N],tag[N],q[N];
    inline void add(int x,int y){if(x)tag[x]+=y,val[x]+=y;}
    inline void pushdown(int x){
        int l=c[x][0],r=c[x][1];
        if(tag[x]){
            add(l,tag[x]);add(r,tag[x]);
            tag[x]=0;
        }
    }
    inline bool isroot(int x){return c[fa[x]][0]!=x&&c[fa[x]][1]!=x;}
    inline void rotate(int x){
        int y=fa[x],z=fa[y],l,r;
        if(c[y][0]==x)l=0;else l=1;r=l^1;
        if(!isroot(y)){if(c[z][0]==y)c[z][0]=x;else c[z][1]=x;}
        fa[x]=z;fa[y]=x;fa[c[x][r]]=y;
        c[y][l]=c[x][r];c[x][r]=y;
    }
    inline void splay(int x){
        int top=1;q[top]=x;
        for(int i=x;!isroot(i);i=fa[i])q[++top]=fa[i];
        for(int i=top;i;i--)pushdown(q[i]);
        while(!isroot(x)){
            int y=fa[x],z=fa[y];
            if(!isroot(y)){
                if((c[z][0]==y)^(c[y][0]==x))rotate(x);
                else rotate(y);
            }rotate(x);
        }
    }
    void access(int x){for(int t=0;x;t=x,x=fa[x])splay(x),c[x][1]=t;}
    inline void link(int x,int y){fa[x]=y;access(y);splay(y);add(y,val[x]);}
    inline void cut(int x){
        access(x);splay(x);add(c[x][0],-val[x]);
        fa[c[x][0]]=0;c[x][0]=0;
    }    
}T;
struct Suffix_AutoMaton{
    int l[N],fa[N],ch[N][26];
    int cnt,last;
    Suffix_AutoMaton(){cnt=1;last=1;}
    void ins(int c){
        int p=last,np=++cnt;last=np;T.val[np]=1;l[np]=l[p]+1;
        for(;p&&!ch[p][c];p=fa[p])ch[p][c]=np;
        if(!p)fa[np]=1,T.link(np,1);
        else{
            int q=ch[p][c];
            if(l[p]+1==l[q])fa[np]=q,T.link(np,q);
            else{
                int nq=++cnt;l[nq]=l[p]+1;
                memcpy(ch[nq],ch[q],sizeof(ch[q]));
                fa[nq]=fa[q];T.link(nq,fa[q]);
                fa[np]=fa[q]=nq;
                T.cut(q);T.link(q,nq);T.link(np,nq);
                for(;ch[p][c]==q;p=fa[p])ch[p][c]=nq;
            }
        }
    }
    void build(){
        scanf("%s",s);int len=strlen(s);
        for(int i=0;i<len;i++)ins(s[i]-'A');
    }
    int query(){
        gets(mask);
        int p=1;int len=chars.length();
        for(int i=0;i<len;i++)if(!(p=ch[p][chars[i]-'A']))return 0;
        T.splay(p);
        return T.val[p];
    }
    void add(){
        gets(mask);int len=chars.length();
        for(int i=0;i<len;i++)ins(chars[i]-'A');
    }
}sam;
int main(){
    scanf("%d",&Q);sam.build();
    while(Q--){
        scanf("%s",s);
        if(*s=='A')sam.add();
        else{
            int ans=sam.query();
            printf("%d\n",ans);
            mask^=ans;
        }
    }
    return 0;


}
	
