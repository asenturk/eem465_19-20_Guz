Systick Timer kesmesi ile LED yakıp söndürme

```c
#include "stm32f4xx.h"

void SysTick_Handler(){
	if(GPIOD->ODR & GPIO_ODR_OD14)
		GPIOD->ODR &= ~GPIO_ODR_OD14;
	else
		GPIOD->ODR |= GPIO_ODR_OD14;
}

int main(){
	SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
	SysTick->LOAD |= 11200000 & SysTick_LOAD_RELOAD_Msk;
	
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	SysTick->CTRL |= SysTick_CTRL_TICKINT_Msk;
	
	NVIC_EnableIRQ(SysTick_IRQn);
	
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER14_0;
	
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	
	while(1);	
}
```

Systick timer ile duty cycle süresi sabit PWM sinyali

```c
#include "stm32f4xx.h"

int frekans=16000;
int h=4000;
int l=12000;

void SysTick_Handler(){
	if(GPIOD->ODR & GPIO_ODR_OD14){
		GPIOD->ODR &= ~GPIO_ODR_OD14;
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= l & SysTick_LOAD_RELOAD_Msk;
	}
	else{
		GPIOD->ODR |= GPIO_ODR_OD14;
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= h & SysTick_LOAD_RELOAD_Msk;
	}
}

int main(){
	SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
	SysTick->LOAD |= h & SysTick_LOAD_RELOAD_Msk;
	
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	SysTick->CTRL |= SysTick_CTRL_TICKINT_Msk;
	
	NVIC_EnableIRQ(SysTick_IRQn);
	
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER14_0;
	
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	GPIOD->ODR |= GPIO_ODR_OD14;
	
	while(1);
}
```

LEDin  parlaklığı kademeli olarak artar, en yükseğe ulaşımca tekrar en düşüğe döner.

```c
#include "stm32f4xx.h"

int frekans=80000;
int duty_cycle=40000;
int artis = 80;

void SysTick_Handler(){
	
	if(duty_cycle > 80000-500 )
		duty_cycle = 500;
	
	if(GPIOD->ODR & GPIO_ODR_OD13){	
		duty_cycle += artis;
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= duty_cycle & SysTick_LOAD_RELOAD_Msk;
		GPIOD->ODR &= ~GPIO_ODR_OD13;
	}
	else{
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= (frekans - duty_cycle) & SysTick_LOAD_RELOAD_Msk;
		GPIOD->ODR |= GPIO_ODR_OD13;
	}
}
int main(){
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER13_0;
	
	NVIC_EnableIRQ(SysTick_IRQn);
	
	SysTick->LOAD |= duty_cycle & SysTick_LOAD_RELOAD_Msk;
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	SysTick->CTRL |= SysTick_CTRL_TICKINT_Msk;
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	
	GPIOD->ODR |= GPIO_ODR_OD13;
	
	while(1);
}
```



Kademeli olarak parlayan ve kademeli olarak sönen LED

```c
#include "stm32f4xx.h"

int frekans=80000;
int h=500;
int l=15500;
int degisim=100;

void SysTick_Handler(){
	
	if(h>79500){
		degisim= degisim*-1;
		h=79350;
	}
	if(h<500){
		degisim= degisim*-1;
		h=650;
	}
	
	//asagidaki sekilde tek bir if icerisinde de yapilabilir.
	//if(duty_cycle > 80000-500 || duty_cycle < 500){
	//	artis = -1*artis;
	//	duty_cycle+=artis;
	//}
	
	if(GPIOD->ODR & GPIO_ODR_OD14){
		
		h=h+degisim;
		l=frekans-h;
		
		GPIOD->ODR &= ~GPIO_ODR_OD14;
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= h & SysTick_LOAD_RELOAD_Msk;
	}
	else{
		GPIOD->ODR |= GPIO_ODR_OD14;
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= l & SysTick_LOAD_RELOAD_Msk;
	}
}
int main(){
	SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
	SysTick->LOAD |= h & SysTick_LOAD_RELOAD_Msk;
	
	SysTick->CTRL |= SysTick_CTRL_CLKSOURCE_Msk;
	SysTick->CTRL |= SysTick_CTRL_TICKINT_Msk;
	
	NVIC_EnableIRQ(SysTick_IRQn);
	
	RCC->AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER14_0;
	
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	GPIOD->ODR |= GPIO_ODR_OD14;
	
	while(1);	
}
```








