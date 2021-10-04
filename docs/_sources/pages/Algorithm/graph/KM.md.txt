# KM求二分图最大权完美匹配

O(n^3)

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#define N 505
#define M 250005
#define INF 9990365505
#define ll long long
using namespace std;
int n,m,x,y,z,tot,tim,l,r,q[N],fr[N],nxt[M],d1[M],d2[M];
int pre[N],visx[N],visy[N],mchx[N],mchy[N];
ll ex[N],ey[N],slack[N];
void add(int x,int y,int z)
{
    tot++;
    d1[tot]=y;
    d2[tot]=z;
    nxt[tot]=fr[x];
    fr[x]=tot;
}
void modify(int cur)
{
    for (int x=cur,lst;x;x=lst)
        lst=mchx[pre[x]],mchx[pre[x]]=x,mchy[x]=pre[x];
}
void bfs(int cur)
{
    for (int i=1;i<=n;i++)
        slack[i]=INF,pre[i]=0;
    l=1,r=0;
    q[++r]=cur;
    ++tim;
    for (;;)
    {
        while (l<=r)
        {
            int u=q[l];
            l++;
            visx[u]=tim;
            for (int i=fr[u];i;i=nxt[i])
            {
                int v=d1[i],cost=d2[i];
                if (visy[v]==tim)
                    continue;
                ll del=ex[u]+ey[v]-cost;
                if (del<slack[v])
                {
                    slack[v]=del;
                    pre[v]=u;
                    if (!del)
                    {
                        visy[v]=tim;
                        if (!mchy[v])
                        {
                            modify(v);
                            return;
                        }
                        q[++r]=mchy[v];
                    }
                }
            }
        }
        ll del=INF;
        for (int i=1;i<=n;i++)
            if (visy[i]!=tim)
                del=min(del,slack[i]);
        l=1,r=0;
        for (int i=1;i<=n;i++)
        {
            if (visx[i]==tim)
                ex[i]-=del;
            if (visy[i]==tim)
                ey[i]+=del; else
                slack[i]-=del;
        }
        for (int i=1;i<=n;i++)
            if (visy[i]!=tim && !slack[i])
            {
                visy[i]=tim;
                if (!mchy[i])
                {
                    modify(i);
                    return;
                }
                q[++r]=mchy[i];
            }
    }
}
void KM()
{
    for (int i=1;i<=n;i++)
        bfs(i);
    ll ans=0;
    for (int i=1;i<=n;i++)
        ans+=ex[i]+ey[i];
    printf("%lld\n",ans);
    for (int i=1;i<=n;i++)
        printf("%d ",mchy[i]);
    putchar('\n');
}
int main()
{
    scanf("%d%d",&n,&m);
    for (int i=1;i<=n;i++)
        ex[i]=-INF,ey[i]=0;
    for (int i=1;i<=m;i++)
    {
        scanf("%d%d%d",&x,&y,&z);
        add(x,y,z);
        ex[x]=max(ex[x],(ll)z);
    }
    KM();
    return 0;
}
```