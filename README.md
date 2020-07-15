# REGISTER_ADC
This project is ADC control and used Registers Programming with Stm32f407-Discovery Kit. This project was created on Atollic TrueSTUDIO.
//CODES

#include "stm32f4xx.h"
#include "stm32f4_discovery.h"

uint8_t adc_value; //8 bitlik veri okunacak

void GPIO_Config()
{
	RCC->AHB1ENR |= 0x00000001; //GPIOA Clock Enable // |= kullanılması daha uygun promramlama acısından. Aksi takdirde GPIOA portunun haberleşme protokolleri bozulabilir
	GPIOA->MODER |= 0x00000003; //Pin 0 Analog mod
	GPIOA->OSPEEDR |= 0x00000003; //Pin 0 100MHz
	GPIOA->OTYPER |= 0x00000000; //Push-Pull ayarlandı
	GPIOA->PUPDR |= 0x00000000; // No-Pull ayarlandı
}

void ADC_Config()
{
	RCC->APB2ENR |= 0x00000100; //ADC1 Clock Enable
	ADC1->CR1 |= 0x02000000; //ADC1 Enable
	ADC1->SMPR2 |= 0x00000003; //56 Cycles
	ADC ->CCR |= 0x00010000; //Div 4 (APB2 hattı 84MHz. ADC max 36 Mhz de çalışır. 84MHz 4 e bolunerek 21 MHz yapıldı)
}

uint8_t Read_ADC()
{
	uint8_t value;
	ADC1->CR2  |= 0x40000000; //ADC1 çevrimi başlatıldı
	while(!(ADC1->SR |= 0x00000002)); //Çevrim bitene kadar bekle
	value = ADC1->DR; //ADC1 çıktısı alındı
	return value;
}

int main(void)
{
  //RCC_Config(); --> atollic programı Clock Configuration ayarlarını yaptığı için clock ayarları yapılmadı
  GPIO_Config();
  ADC_Config();

  while (1)
  {
	  adc_value = Read_ADC();
  }
}



void EVAL_AUDIO_TransferComplete_CallBack(uint32_t pBuffer, uint32_t Size){
  /* TODO, implement your code here */
  return;
}

uint16_t EVAL_AUDIO_GetSampleCallBack(void){
  /* TODO, implement your code here */
  return -1;
}
