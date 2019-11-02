while dongüsünde yanıp sönen led interrupt ile değiştiriliyor.
Zamanlama systick timer ile yapılıyor. 8 saniye yanma 1 saniye sönme.

```c
#include "stm32f4xx.h"

int led=14;

void EXTI0_IRQHandler(){
	EXTI->PR |= EXTI_PR_PR0;
	
	GPIOD->ODR &= ~(1UL<<led);
	
	if(led==14){
		led=15;
	}else{
		led=14;
	}
	GPIOD->ODR |= (1UL<<led);
}

int main(){

	RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN |	RCC_AHB1ENR_GPIODEN;
	GPIOD->MODER |= GPIO_MODER_MODER15_0 | GPIO_MODER_MODER14_0;

	EXTI->IMR |= EXTI_IMR_IM0;
	EXTI->RTSR |= EXTI_RTSR_TR0;
	
	SYSCFG->EXTICR[0] &= ~SYSCFG_EXTICR1_EXTI0;
	
	NVIC_EnableIRQ(EXTI0_IRQn);
	
	SysTick->CTRL &= ~SysTick_CTRL_ENABLE_Msk;
	SysTick->CTRL &= ~SysTick_CTRL_CLKSOURCE_Msk;
	SysTick->LOAD |= 16000000 & SysTick_LOAD_RELOAD_Msk;
	SysTick->CTRL |= SysTick_CTRL_ENABLE_Msk;
	while(1){
		GPIOD->ODR |= (1UL<<led);
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= 2000000 & SysTick_LOAD_RELOAD_Msk;
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));
		
		GPIOD->ODR &= ~(1UL<<led);
		SysTick->LOAD &= ~SysTick_LOAD_RELOAD_Msk;
		SysTick->LOAD |= 16000000 & SysTick_LOAD_RELOAD_Msk;
		while(!(SysTick->CTRL & SysTick_CTRL_COUNTFLAG_Msk));	
	}
}
```