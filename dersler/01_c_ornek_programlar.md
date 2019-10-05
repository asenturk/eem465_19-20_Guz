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
