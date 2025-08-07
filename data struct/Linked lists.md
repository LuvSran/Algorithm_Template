# 链表
## 板子
 ```
int val[maxn],ne[maxn],pt; //地址0为虚拟头节点,-1表空地址
void add(int k,int x){ //将值为x的节点添加到地址为k的节点后
    ++pt;
    val[pt]=x;
    ne[pt]=ne[k];
    ne[k]=pt;
}
void init(){
    pt=0;
    ne[0]=-1;
}
```
## 单链表
数组模拟 ==e[n]== 存value ==ne[n]==存next
```
const int N=1e5+10;
int e[N],ne[N];
int head,idx;
// head表头节点（下标）
// e[i]表节点i的value
// ne[i]表节点i的next指针是多少（下一个节点的下标）
// idx表示当前用到了哪个节点（即下一次添加节点是对下标idx的点操作,一般idx为head下一个）;

//初始化
void init(){
head=-1; 
idx=0;
}

//将x添加到头节点前（x为新的head）
void add_to_head(int x){
e[idx]=x; 
ne[idx]=head; 
head=idx; 
idx++;
}

//将x添加到下标k的点（即第k+1次添加的点）后面
void add(int k,int x){
e[idx]=x;  
ne[idx]=ne[k]; 
ne[k]=idx; 
idx++;
}

//将下标k（即第k+1次添加的点）后面的一个点删除
void remove(int k){
ne[k]=ne[ne[k]];
} 
int main(){

}
```
## 双链表
==e[N]==存value,==l[N]==存第i个点左边点，==r[N]==存右边点
```
const int N=1e5+10;
int e[N],l[N],r[N];
int head,index;

void init(){
head=0; 
index=2;
r[0]=1;
l[1]=0;
}

//在下标是k的点右边插入x
void add(int k,int x){
e[idx]=x;
r[idx]=r[k];
l[idx]=k;
r[k]=idx;
l[r[idx]]=idx;
idx++;
}

//删除第k个点
void remove(int k){
r[l[k]]=r[k];
l[r[k]]=l[k];
}
```