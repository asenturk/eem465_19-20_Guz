stm32f407xx.h dosyasındaki tanımlamalar kullanılarak yapılan programlama, 4 LED yakılıp söndürülüyor.

```c
#include "stm32f4xx.h"

int main(){
	int i;
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN ;
	
	GPIOD->MODER |= GPIO_MODER_MODER15_0 | GPIO_MODER_MODER14_0 |
			GPIO_MODER_MODER13_0 | GPIO_MODER_MODER12_0;
	
	while(1){
		
		GPIOD->ODR |= GPIO_ODR_OD12 | GPIO_ODR_OD13 |
			GPIO_ODR_OD14 | GPIO_ODR_OD15 ;
		for(i=0; i<1000000;i++);	
	
		GPIOD->ODR &= ~(GPIO_ODR_OD12 | GPIO_ODR_OD13 |
				GPIO_ODR_OD14 | GPIO_ODR_OD15) ;
		for(i=0; i<1000000;i++);
	}
}
```

Bir önceki programa  buton kontrolü eklendi. Buton A portunun 0. pinine bağlı.


```c
#include "stm32f4xx.h"

int main(){
	int i;
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN | RCC_AHB1ENR_GPIOAEN;
	
	GPIOD->MODER |= GPIO_MODER_MODER15_0 | GPIO_MODER_MODER14_0 |
			GPIO_MODER_MODER13_0 | GPIO_MODER_MODER12_0 ; 
	
	
	while(1){
		//if(GPIOA->IDR & 1<<0)
		if(GPIOA->IDR & GPIO_IDR_ID0){
			GPIOD->ODR |= GPIO_ODR_OD15 | GPIO_ODR_OD14 |
				GPIO_ODR_OD13 | GPIO_ODR_OD12;
			for(i=0;i<1000000;i++);
			
			GPIOD->ODR &= ~(GPIO_ODR_OD15 | GPIO_ODR_OD14 |
					GPIO_ODR_OD13 | GPIO_ODR_OD12);
			for(i=0;i<1000000;i++);
		}
	}
	
}
```

SysTick Timer ayarları aşağıdaki kodlarla yapılabilir. Böylece timer 1 saniye bekleme sağlar.

```c
SysTick->LOAD |=  (SysTick_LOAD_RELOAD_Msk &  16000000UL);
SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;

while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
```


SysTick Timer ın programda kullanımı:

```c
#include "stm32f4xx.h"                  // Device header

int main(){
	SysTick->LOAD |=  (SysTick_LOAD_RELOAD_Msk &  16000000UL);
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER13_0;
	
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	
	while(1){
		GPIOD->ODR |= GPIO_ODR_OD13;
		//while(!(SysTick->CTRL  & 1<<16));
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
		
		GPIOD->ODR &= ~GPIO_ODR_OD13;
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
	}	
}

```


Harici çevrim kaynağını seçmek için aşağıdaki kodlama ile ayarlama yapılabilir:

```c
RCC->CR |= RCC_CR_HSEON;
while(!(RCC->CR & RCC_CR_HSERDY));
RCC->CFGR |= RCC_CFGR_SW_0;
```


Programda kullanılışı:
```c
#include "stm32f4xx.h"
int main(){
	
	RCC->CR |= RCC_CR_HSEON;
	while(!(RCC->CR & RCC_CR_HSERDY));
	RCC->CFGR |= RCC_CFGR_SW_0;
	
	SysTick->LOAD |=  (SysTick_LOAD_RELOAD_Msk &  10000UL);
	//SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER13_0;
	
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	
	while(1){
		GPIOD->ODR |= GPIO_ODR_OD13;
		//while(!(SysTick->CTRL  & 1<<16));
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
		
		GPIOD->ODR &= ~GPIO_ODR_OD13;
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
	}	
}
```

**Yukarıda program neticesinde LED ne kadar süre yanık ve sönük kalıyor?**
