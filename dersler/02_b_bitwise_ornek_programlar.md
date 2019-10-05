### Bitwise (Bit Düzeyi) İşlemler

```c
int main(){
	int i, j, k;
	i=5, j=3;
	//VEYA
	k=i | j;
	//VE
	k=i&j;
	//XOR
	k=i^j;
	//DEĞİL
	k=~i;
	k=~j;
	//SOLA KAYDIR
	k= j<<1;
	k= j<<3;
	//SAĞA KAYDIR
	k= j>>1;
	k= j>>3;
}
```

```c
int main(){
	int i,k;
	unsigned int j;

	//i değişkeni işaretli tanımlanırsa (int) sağa kaydırılırken
	//en soldaki bit ne ise onunla doldurulur.
	//İşaretsiz ise (unsigned) 0 ile doldurulur.
	i= 0x81000000;
	k= i>>2;
	j= 0x81000000;
	k= i>>2;
	
	i=0xEA69B1D7;
	//i değişkeninin 11. bitini 1 yapma
	k= i | (1<<11);
	
	//i değişkeninin 27. bitini 0 yapma
	k= i & ~(1<<27);
	
	//i değişkeninin 31. bitini tersi haline getirme
	k= i ^ (1<<31);
}
```

```c
int main(){	
	int i = 0xABC123F7;
	int n_bitin_degeri;
	int n;
	n=15;
	n_bitin_degeri=!((i&(1<<15)) == 0);
	n=16;
	n_bitin_degeri=!((i&(1<<16)) == 0);
}
```

Herhangi bir bitin değerinin 1 veya 0 olması genelde if veya while ifadelerinin içinde kullanılır.
```c
int main(){	
	int i = 0xABC123F7;
	int n=15;
	
	if(i & (1<<n)){
		//i'nin n. biti 1 ise
	}else{
		////i'nin n. biti 0 ise
	}
	
}
```




