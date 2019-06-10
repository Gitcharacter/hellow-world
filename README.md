# hellow-world
jianduan
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
 
struct stud{
   int val;
   int height;
   struct stud* left;
   struct stud* right;
};
 
stud * Empty(stud * cur)//删除树
{
    if(cur==NULL) return NULL;
    Empty(cur->left);
    Empty(cur->right);
    free(cur);
    return NULL;
}
 
inline int Height(stud * cur)//计算高度
{
    if(cur==NULL) return 0;
    return cur->height;
}
 
stud* LLchange(stud * cur) //LL旋转操作
{
     stud * temp=cur->left;
     cur->left=temp->right;
     temp->right=cur;
 
     cur->height=max(Height(cur->left),Height(cur->right))+1;
     temp->height=max(Height(temp->left),Height(temp->right))+1;
     return temp;
}
 
stud* RRchange(stud* cur)//RR旋转操作
{
    stud * temp=cur->right;
    cur->right=temp->left;
    temp->left=cur;
 
    cur->height=max(Height(cur->left),Height(cur->right))+1;
    temp->height=max(Height(temp->left),Height(temp->right))+1;
    return temp;
}
 
stud* LRchange(stud* cur)  //LR旋转操左
{
     cur->left=RRchange(cur->left);
     return LLchange(cur);
}
 
stud* RLchange(stud * cur)
{
    cur->right=LLchange(cur->right);
    return RRchange(cur);
}
 
stud* Insert(stud * cur,int va)
{
     if(cur==NULL) //如果到底了，那么创建新的节点
     {
         cur=(stud*)malloc(sizeof(stud));
         cur->left=cur->right=NULL;
         cur->val=va;
         cur->height=1;
         return cur;
     }
     if(cur->val==va) return cur;//有相同的
     if(cur->val>va)
     {
         cur->left=Insert(cur->left,va);//注意这里是cur->left=   ,请认真思考为什么这么写
         if(Height(cur->left)-Height(cur->right)==2)//插入后看是否平衡，如果不平衡显然是插入的那一边高度大
         {
              if(va<cur->left->val)//判断是LL还是LR,即插入的是left节点的left 还是right
                  cur=LLchange(cur);
              else
                  cur=LRchange(cur);
         }
     }
     else//同理
     {
        cur->right=Insert(cur->right,va);
        if(Height(cur->right)-Height(cur->left)==2)
        {
            if(va>cur->right->val)
                cur=RRchange(cur);
            else
                cur=RLchange(cur);
        }
     }
     cur->height=max(Height(cur->left),Height(cur->right))+1;
     return cur;
}
 
stud * adjust(stud *cur)//对于删除了一个节点，那么是需要判断是否还是平衡，如果不平衡，就要调整
{
     if(cur==NULL) return NULL;
     //printf("adjust %d\n",cur->val);
     if(Height(cur->left)-Height(cur->right)==2)//哪边高度高，那么调节哪一边
     {
         if(Height(cur->left->left)>=Height(cur->left->right))
             cur=LLchange(cur);
         else
            cur=LRchange(cur);
     }
     if(Height(cur->right)-Height(cur->left)==2)
     {
         if(Height(cur->right->right)>=Height(cur->right->left))
              cur=RRchange(cur);
         else
            cur=RLchange(cur);
     }
     cur->height=max(Height(cur->left),Height(cur->right))+1;
     return cur;
}
 
stud * Delete(stud * cur,int va)
{
    if(cur==NULL) return NULL;
    if(cur->val==va)//如果找到删除节点
    {
        if(cur->right==NULL)//如果该节点的右孩子为NULL,那么直接删除
        {
            stud * temp=cur;
            cur=cur->left;
            free(temp);
        }
        else//否则，将其右子树的最左孩子作为这个节点，并且递归删除这个节点的值
        {
           stud * temp;
           temp=cur->right;
           while(temp->left)
              temp=temp->left;
           cur->val=temp->val;
           cur->right=Delete(cur->right,cur->val);
           cur->height=max(Height(cur->right),Height(cur->left))+1;
        }
        return cur;
    }
 
    if(cur->val>va)//调节删除节点后可能涉及的节点
        cur->left=Delete(cur->left,va);
 
    if(cur->val<va)
        cur->right=Delete(cur->right,va);
 
    cur->height=max(Height(cur->left),Height(cur->right))+1;
 
    if(cur->left)
        cur->left=adjust(cur->left);
 
    if(cur->right)
        cur->right=adjust(cur->right);
 
    if(cur) cur=adjust(cur);
    return cur;
}
 
void show(stud *cur)//前序遍历方便看树的结果
{
    if(cur==NULL) return ;
    printf("%d %d\n",cur->val,cur->height);
    show(cur->left);
    show(cur->right);
}
 
int main()
{
    int i,j;
    int n,x;
    stud * root=NULL;
    while(~scanf("%d",&n))
    {
         root=Empty(root);
         for(int i=0;i<n;i++)
         {
             scanf("%d",&x);
             root=Insert(root,x);
         }
         printf("\n");
         show(root);
         printf("\n");
         scanf("%d",&x);
         root=Delete(root,x);
         show(root);
    }
    return 0;
}

