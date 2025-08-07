# 主席树
```cpp
#define lc(_x) tr[_x].ls
#define rc(_x) tr[_x].rs
int root[maxn];
struct ZX_Tree{
    int _n;
    int sit=0;
    struct{
        int sum,ls,rs;
    }tr[32*maxn];
    void build(int& ts,int nl,int nr){
        ts=++sit;
        if(nl==nr){
            tr[sit].sum=0;
            return;
        }
        int nmid=(nl+nr)>>1;
        build(lc(ts),nl,nmid);
        build(rc(ts),nmid+1,nr);
    }
    void add(int& ts,int his,int nl,int nr,int x,int val){ //复制一个his版本，并在x处增加val
        ts=++sit;
        tr[ts]=tr[his];
        tr[ts].sum+=val;
        if(nl==nr) return;
        int nmid=(nl+nr)>>1;
        if(x<=nmid) add(lc(ts),lc(his),nl,nmid,x,val);
        else add(rc(ts),rc(his),nmid+1,nr,x,val);
    }
    int query(int id1,int id2,int nl,int nr,int k){
        if(nl==nr) return nl;
        int s=tr[lc(id2)].sum-tr[lc(id1)].sum;
        int nmid=(nl+nr)>>1;
        if(k<=s) return query(lc(id1),lc(id2),nl,nmid,k);
        else return query(rc(id1),rc(id2),nmid+1,nr,k-s);
    }
    int Kth(int l,int r,int k){ //查询l~r区间内第k小
        return query(root[l-1],root[r],1,_n,k);
    }
    int ask_sum(int id1,int id2,int nl,int nr,int lo,int ro){ //查询lo~ro值域内的数量
        if(lo<=nl && ro>=nr) return tr[id2].sum-tr[id1].sum;
        int nmid=(nl+nr)>>1;
        int s=0;
        if(lo<=nmid) s+=ask_sum(lc(id1),lc(id2),nl,nmid,lo,ro);
        if(ro>=nmid+1) s+=ask_sum(rc(id1),rc(id2),nmid+1,nr,lo,ro);
        return s;
    }
    int rank(int l,int r,int k){    //查询k在l~r内的排名
        return 1+ask_sum(root[l-1],root[r],1,_n,1,k-1);
    }
};
ZX_Tree tree;
solve(){
	tree.sit=0;
    tree.build(root[0],1,n);                         //可能要离散化
    for(int i=1;i<=n;++i){
        tree.add(root[i],root[i-1],1,n,a[i],1);
    }
}
```