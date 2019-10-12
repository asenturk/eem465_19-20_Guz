Kart üzerindeki 4 LEDi aynı anda yakıp söndüren kod.

```c
#define RCC_AHB1ENR (*((int *) 0x40023830))
#define GPIOD_MODER (*((int *) 0x40020C00))
#define GPIOD_ODR   (*((int *) 0x40020C14))

int main(){
	int i;
	RCC_AHB1ENR |= (1<<3);
	GPIOD_MODER |= 1<<30 | 1<<28 | 1<<26 | 1<<24;
	
	while(1){
		GPIOD_ODR |= 1<<15 | 1<<14 | 1<<13 | 1<<12;
		for(i=0;i<1000000;i++);
		GPIOD_ODR &= ~(1<<15 | 1<<14 | 1<<13 | 1<<12);
		for(i=0;i<1000000;i++);
	}
}
```


LEDleri sırayla yakıp söndüren program.
```c
#define RCC_AHB1ENR (*((int *) 0x40023830))
#define GPIOD_MODER (*((int *) 0x40020C00))
#define GPIOD_ODR   (*((int *) 0x40020C14))

int main(){
	int i,pin;
	RCC_AHB1ENR |= (1<<3);
	
	for(pin=12;pin<16;pin++)
		GPIOD_MODER |= 1<<(pin*2);
	
	while(1){
		for(pin=12;pin<16;pin++)
			GPIOD_ODR |= 1<<pin;
		
		for(i=0;i<1000000;i++);
		
		for(pin=12;pin<16;pin++)
			GPIOD_ODR &= ~(1<<pin);
		
		for(i=0;i<1000000;i++);
	}
}
```
Bir önceki programda for döngüsü delay fonksiyonu haline getirildi.
```c
#define RCC_AHB1ENR (*((int *) 0x40023830))
#define GPIOD_MODER (*((int *) 0x40020C00))
#define GPIOD_ODR   (*((int *) 0x40020C14))
	
void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	int pin;
	RCC_AHB1ENR |= (1<<3);
	
	for(pin=12;pin<16;pin++)
		GPIOD_MODER |= 1<<(pin*2);
	
	while(1){
		for(pin=12;pin<16;pin++){
			GPIOD_ODR |= 1<<pin;
			delay();
			GPIOD_ODR &= ~(1<<pin);
			//delay();
		}	
	}
}
```
LEDleri sırayla yakan ve daha sonra yine sırayla söndüren program.
```c
#define RCC_AHB1ENR (*((int *) 0x40023830))
#define GPIOD_MODER (*((int *) 0x40020C00))
#define GPIOD_ODR   (*((int *) 0x40020C14))
	
void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	int pin;
	RCC_AHB1ENR |= (1<<3);
	
	for(pin=12;pin<16;pin++)
		GPIOD_MODER |= 1<<(pin*2);
	
	while(1){
		
		for(pin=12;pin<16;pin++){
			GPIOD_ODR |= 1<<pin;
			delay();
		}	
		for(pin=12;pin<16;pin++){
			GPIOD_ODR &= ~(1<<pin);
			delay();
		}
	}
}
```


Bitwise işlemlerin isimlendirilmesi
```c
#define RCC_AHB1ENR (*((int *) 0x40023830))
#define GPIOD_MODER (*((int *) 0x40020C00))
#define GPIOD_ODR   (*((int *) 0x40020C14))
	
#define RCC_AHB1ENR_GPIODEN  (1 << 3)
#define GPIO_MODER_MODER15_0 (1 << 30)
#define GPIO_MODER_MODER14_0 (1 << 28)
#define GPIO_MODER_MODER13_0 (1 << 26)
#define GPIO_MODER_MODER12_0 (1 << 24)

#define GPIO_ODR_OD15  (1<<15)
#define GPIO_ODR_OD14  (1<<14)
#define GPIO_ODR_OD13  (1<<13)
#define GPIO_ODR_OD12  (1<<12)

void delay(){
	int i;
	for(i=0;i<1000000;i++);
}

int main(){
	RCC_AHB1ENR |= RCC_AHB1ENR_GPIODEN;
	
		GPIOD_MODER |= GPIO_MODER_MODER15_0 | GPIO_MODER_MODER14_0 | 
				       GPIO_MODER_MODER13_0 | GPIO_MODER_MODER12_0;
	
	while(1){
		
			GPIOD_ODR |= GPIO_ODR_OD12 | GPIO_ODR_OD13 |
					     GPIO_ODR_OD14 | GPIO_ODR_OD15;
			delay();
			GPIOD_ODR &= ~(GPIO_ODR_OD15 | GPIO_ODR_OD14 |
						   GPIO_ODR_OD13 | GPIO_ODR_OD12);
			delay();
		}	
}
```
Tür tanımlama için genel örnek.
```c
typedef int BOOL;
int main(){
	BOOL a=1;
}
```

Struct ve struct işaretçisi kullanımları için genel örnek.
```c
struct calisan{
	int sicil;
	float maas;
};

int main(){	
	struct calisan calisan1;
	struct calisan *p;
	
	calisan1.sicil = 511;
	calisan1.maas = 4250.30;
	
	p = &calisan1;
	(*p).sicil =511;
	(*p).maas =4250.30;
	
	p->sicil = 511;
	p->maas = 4250.30;
}
```

Struct tür tanımlayarak bu türün adres haline çevrilmesi ile yapılan LED yakma programı.
```c
typedef struct
{
  unsigned int MODER;   
  unsigned int OTYPER;  
  unsigned int OSPEEDR; 
  unsigned int PUPDR;   
  unsigned int IDR;     
  unsigned int ODR;     
  unsigned int BSRR;    
  unsigned int LCKR;    
  unsigned int AFR[2];  
} GPIO_TypeDef;

#define GPIOD ((GPIO_TypeDef*) 0x40020C00)
#define RCC_AHB1ENR (*((int *) 0x40023830))

int main(){
	
	RCC_AHB1ENR |= 1<<3;
	GPIOD->MODER |= 1<<30;	
	GPIOD->ODR |= 1<<15;
}
```