# 快速幂，o(logn)
*模板*
```cpp
ll qpow(ll _a,ll _b,ll _p){ //快速幂求a^b%p  
    ll res=1;  
    while(_b){  
        if(_b&1) res=res*_a%_p;  
        _a=_a*_a%_p;  
        _b>>=1;     
    }  
    return res%_p;  
}
```
# 矩阵快速幂
``` cpp
const int mod=1e9+7;
int mat_n=100;   //矩阵大小_n*_n
struct Mat{                      
	ll cc[105][105];			//矩阵，注意大小
	void init(int type){        //type0,设为全0矩阵;type1，设为单位矩阵
		for(int i=1;i<=mat_n;++i){
			for(int j=1;j<=mat_n;++j) cc[i][j]=0;
		}
		if(type==1) for(int i=1;i<=mat_n;++i) cc[i][i]=1;
	}
};
Mat mat_res;  //矩阵快速幂结果
Mat A;        //进行运算的矩阵
Mat operator*(Mat &_x,Mat &_y){
	Mat _t;
	_t.init(0);
	for(int i=1;i<=mat_n;++i){
		for(int j=1;j<=mat_n;++j){
			for(int k=1;k<=mat_n;++k){
				_t.cc[i][j]=(_t.cc[i][j]+_x.cc[i][k]*_y.cc[k][j])%mod;
			}
		}
	}
	return _t;
}
void matpow(int _k){  //求矩阵mat_res乘矩阵A的_k次方。
	mat_res.init(1);  //原始矩阵，求A的k次方则构造为单位矩阵
	for(int i=1;i<=mat_n;++i) mat_res.cc[i][i]=1;
	while(_k){
		if(_k&1) mat_res=mat_res*A;
		A=A*A;
		_k>>=1;
	}
}
```
 矩阵加速斐波那契
```
void solve(){  
    int n;  //求斐波那契第n项
    cin >> n;  
    if(n<=2) {cout << 1; return;}  
    A.cc[1][1]=1,A.cc[1][2]=1;   //构造矩阵 
    A.cc[2][1]=1,A.cc[2][2]=0;  
    matpow(n-2);  
    Mat t;  
    t.init(0);  
    t.cc[1][1]=1,t.cc[1][2]=1;  //原始矩阵,[f(2),f(1)]
    t=t*mat_res;      //注意不满足交换律
    cout << t.cc[1][1];  
}
```