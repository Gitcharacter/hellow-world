# hellow-world
jianduan
#include<algorithm>
#include<iostream>
#include<queue>
#include<vector>
#include <cstdio>
usingnamespace std;
const int MAX = 20;
int          P[MAX][MAX];
int          Q[MAX][MAX];
int          maxout[MAX];
int          n;
structnode {
       int   id;
       int   sum;
       int   r;
       int   up;
       int   *x;
};
structcmp {
       bool operator()( node* a, node* b )
       {
              return(a->up < b->up);
       }
};
voidsolve()
{
       int                                      ans= 0;
       priority_queue<node*,vector<node*>, cmp>  que;
       node                                         *E = new node();
       E->id      =1;
       E->sum  =0;
       E->r       =0;
       E->up    =0;
       for ( int i = 1; i <= n; i++ )
       {
              E->r += maxout[i];
       }
       E->up    =E->r;
       E->x       =new int[n + 1];
       for ( int i = 1; i <= n; i++ )
       {
              E->x[i] = i;
       }
       while ( E->id != n + 1 )
       {
              for ( int i = E->id; i <= n;i++ )
              {
                     node* nE = new node();
                     nE->id    = E->id + 1;
                     nE->x     = new int[n + 1];
                     for ( int t = 1; t <= n;t++ )
                     {
                            nE->x[t] =E->x[t];
                     }
                     nE->x[E->id] = E->x[i];
                     nE->x[i]  = E->x[E->id];
                     nE->sum              = E->sum +P[E->id][nE->x[E->id]] * Q[nE->x[E->id]][E->id];
                     nE->r            = E->r - maxout[E->id];
                     nE->up         = nE->sum + nE->r;
                     que.push( nE );
              }
              if ( !que.empty() )
              {
                     E= que.top();
                     que.pop();
              }else  {
                     ans = 0;
                     break;
              }
       }
       ans = E->sum;
       cout << ans << endl;
}
 
 
intmain()
{
       cin >> n;
       
       
       
       for ( int i = 1; i <= n; i++ )
       {
              for ( int j = 1; j <= n; j++ )
              {
                     cin >> P[i][j];
              }
       }
       for ( int i = 1; i <= n; i++ )
       {
              for ( int j = 1; j <= n; j++ )
              {
                     cin >> Q[i][j];
              }
       }
       for ( int i = 1; i <= n; i++ )
       {
              int ma = 0;
              for ( int j = 1; j <= n; j++ )
              {
                     ma = max( ma, Q[i][j] *P[j][i] );
              }
              maxout[i] = ma;
       }
       solve();
       return(0);
}
	
