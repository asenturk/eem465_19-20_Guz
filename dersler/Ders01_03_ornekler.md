```c
#include "Board_LED.h" 
int main(){
	LED_Initialize();
	while(1){	
		LED_On(0);
	}
}
```


```c
#include "Board_LED.h"                
int main(){
	int i;
	LED_Initialize();	
	while(1){
		LED_On(0);
		LED_On(1);
		LED_On(2);
		LED_On(3);
	}
}
```

```c
#include "Board_LED.h"
int main(){
	int i;
	LED_Initialize();
	while(1){
		for(i=0;i<3;i++)
		LED_On(i);
	}
}
```

```c
#include "Board_LED.h"

int main(){
	int i;
	LED_Initialize();
	while(1){
		LED_On(0);
		for(i=0;i<1000000;i++);
		LED_Off(0);
		for(i=0;i<1000000;i++);	
	}
}
```

Yukarıdaki örnekte verilen for döngüsü bir fonksiyon haline getirilmiştir.
```c
#include "Board_LED.h"

void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	LED_Initialize();	
	while(1){
		LED_On(0);
		delay();
		LED_Off(0);
		delay();
	}
}
```


```c
#include "Board_LED.h"

void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	int i;
	LED_Initialize();
	
	while(1){
		for(i=0;i<4;i++){
			LED_On(i);
			delay();
			LED_Off(i);
//			delay();
		}
		
	}
}
```

```c
#include "Board_LED.h"

void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	int i;
	LED_Initialize();
	
	while(1){
		for(i=3;i>=0;i--){
			LED_On(i);
			delay();
			LED_Off(i);
//			delay();
		}
		
	}
}
```

Programa buton kütüphanesi eklenmiştir.
```c
#include "Board_LED.h"
#include "Board_Buttons.h"

void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	int i;
	LED_Initialize();
	Buttons_Initialize();
	
	while(1){
		if(Buttons_GetState()==1){
			for(i=3;i>=0;i--){
				LED_On(i);
				delay();
				LED_Off(i);
	//			delay();
			}
		}
	}
}
```


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

	
	//i değişkeni işaretli tanımlanırsa (int) sağa kaydırılırken en soldaki bit ne ise onunla doldurulur. İşaretsiz ise (unsigned) 0 ile doldurulur.
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

