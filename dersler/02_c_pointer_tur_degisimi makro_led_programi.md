### Pointer

```c
int main(){	
	int i=10;
	int *ip;
	ip = &i;
	*ip=20;
	i=30;
	(*ip)--;
}
```


### Tür değişimi
```c
int main(){	
	float x=2.45;
	float tam;
	float ondalik;	
	tam=(int)x;
	ondalik=x-tam;
}
```

### Pointerlar ve tür değişimi ile bazı bellek adreslerindeki yazmaçların bitlerini set etme.


```c
int main(){
	int r=   1073887280; //0x40023830
	int g_m= 1073875968; //0x40020C00
	int g_o= 1073875988; //0x40020C14	
	
	(*((int *)r)) =	(*((int *)r)) | (1<<3);
	
	(*((int *)g_m)) = (*((int *)g_m)) | (1<<30);
	
	(*((int *)g_o)) = (*((int *)g_o)) | (1<<15);	
}
```
Yukarıda verilen program ile kart üzerindeki mavi buton yakıldı.

Aşağıdaki program ile mavi LEDin yanıp sönmesi sağlandı.

```c
int main(){
	int r=   1073887280; //0x40023830
	int g_m= 1073875968; //0x40020C00
	int g_o= 1073875988; //0x40020C14
	int i;
		
	(*((int *)r)) =	(*((int *)r)) | (1<<3);
	
	(*((int *)g_m)) = (*((int *)g_m)) | (1<<30);
	
	while(1){
		(*((int *)g_o)) = (*((int *)g_o)) | (1<<15);
		for(i=0;i<1000000;i++);
		(*((int *)g_o)) = (*((int *)g_o)) & ~(1<<15);
		for(i=0;i<1000000;i++);
	}	
}
```
### Makro tanımlama

```c
#define PI 3.14

int main(){
	float alan;
	float r=5;
	
	alan = PI * r * r;
}
```

Bir önceki mavi LEDi yakıp söndürmek için kullanılan kod makro tanımlamaları ile tekrar yazıldı.

```c
#define R  (*((int *)0x40023830))
#define G_M (*((int *)0x40020C00))
#define G_O (*((int *)0x40020C14))

int main(){	
	int i;

	R =	R | (1<<3);
	G_M =	G_M | (1<<30);
	
	while(1){
		G_O =	G_O | (1<<15);
		for(i=0;i<1000000;i++);
		G_O =	G_O & ~(1<<15);
		for(i=0;i<1000000;i++);
	}	
}
```

C Programlamada a=a+1 şeklinde kısaltılabildiği  gibi bu durum bitwise operatörler için de geçerlidir. Yukarıda verilen programdaki kodlar bu şekilde kısaltılmıştır.

```c
#define R (*((int *)r))
#define G_M (*((int *)g_m))
#define G_O (*((int *)g_o))
int main(){
	int i;
		
	R |=	(1<<3);
	G_M |= (1<<30);
	
	while(1){
		G_O |=	(1<<15);
		for(i=0;i<1000000;i++);
		G_O &=	 ~(1<<15);
		for(i=0;i<1000000;i++);
	}	
}
```
Yukarıda makro tanımlamasında kullanılan bellek bölgelerinin isimleri Reference Manual dokümanında ifade edilmiştir. Bu isimler kullanılarak program tekrar yazılmıştır.

```c
#define RCC_AHB1ENR  (*((int *)0x40023830))
#define GPIOD_MODER (*((int *)0x40020C00))
#define GPIOD_ODR (*((int *)0x40020C14))
int main(){		
	int i;
		
	RCC_AHB1ENR =	RCC_AHB1ENR | (1<<3);
	GPIOD_MODER =	GPIOD_MODER | (1<<30);
	
	while(1){
		GPIOD_ODR =	GPIOD_ODR | (1<<15);
		for(i=0;i<1000000;i++);
		GPIOD_ODR =	GPIOD_ODR & ~(1<<15);
		for(i=0;i<1000000;i++);
	}	
}
```

15 ve 14. pine bağlı LEDleri yakıp söndüren program.

```c
#define RCC_AHB1ENR  (*((int *)0x40023830))
#define GPIOD_MODER (*((int *)0x40020C00))
#define GPIOD_ODR (*((int *)0x40020C14))
int main(){		
	int i;
		
	RCC_AHB1ENR |=	 (1<<3);
	GPIOD_MODER |= (1<<30) | (1<<28);
	
	while(1){
		GPIOD_ODR |= (1<<15) | (1<<14);
		for(i=0;i<1000000;i++);
		GPIOD_ODR &=	 ~(1<<15 | (1<<14));
		for(i=0;i<1000000;i++);
	}
}
```
