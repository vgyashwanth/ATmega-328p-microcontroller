/*
 * timers.c
 *
 * Created: 15-06-2023 19:54:44
 * Author : HP
 */ 
//Normal Programming of the Timer0
#include <avr/io.h>
void timer0(uint32_t);
void fast_pwm(uint8_t);
void ctc_mode(uint8_t);
//#define TIMER_MODE
//#define FAST_PWM_MODE
//#define CTC_MODE
/********MAIN PROGRAM*************/
int main(void)
{    //One time Setup
   
    while (1) 
    {
    
  }
  
}
/********END OF THE MAIN PROGRAM*********/











#ifdef TIMER_MODE
void timer0(uint32_t x){
  for(uint32_t i=0;i<x;i++){
      TCNT0 = 0X00;
      TCCR0A = 0X00;
      TCCR0B = ((TCCR0B&(~(1U<<3)))|1U); // timer started along with mode setup
      unsigned char flag;
      flag = TIFR0&(1U);
      while(flag==0) //poling
      flag = TIFR0&(1U);
      TIFR0 = 0x00; // Clearing the flag
      TCCR0B = (TCCR0B&0x00); // stop the timer
  }
}
#endif

#ifdef FAST_PWM_MODE
void fast_pwm(uint8_t pwm_value){
	 DDRD |= (1U<<6); // making the pin as output pin ;
	 TCNT0 = 0X00;
	 OCR0A = pwm_value; // some unknown duty cycle
	 TCCR0A = TCCR0A |(1U<<7)|(1U<<1)|(1U); //Fast pwm mode
	 TCCR0B  = TCCR0B | (1U); // turn on the clock supply with no prescaler			
}
#endif
#ifdef CTC_MODE
void ctc_mode(uint8_t ocroa_value){
	   pinMode(13,OUTPUT);
	   Serial.begin(250000);
	   while (1)
	   {
		   TCNT0 = 0X00;
		   OCR0A = 0XFF;
		   TCCR0A = TCCR0A |(1U<<1); //Normal ctc mode CTC mode
		   uint8_t flag = TIFR0&(1U<<1);
		   TCCR0B  = TCCR0B | (1U); // turn on the clock supply with no prescaler
		   //  Serial.print("Status of the flag: ");
		   //  Serial.println(flag);
		   uint32_t count = 0;
		   while(count<500000){
			   while(flag==0){ //POLLING
				   flag = TIFR0&(1U<<1);
				   Serial.println("Flag monitoring take place");
			   }
			   if(flag==2){
				   count++;
				   TIFR0 = TIFR0&(~(1U<<1)); //clearing the flag
				   TCCR0B  = TCCR0B&(~(1U)); // disconnecting the clock supply
			   }
			   
		   }
		   Serial.print("Count Outside:");
		   Serial.println(count);
		   if(count==500000){
			   state = state^1;
			   digitalWrite(13,state);
			   Serial.println("Completed");
			   count=0; //making the count = 0;
			   Serial.print("Count inside:");
			   Serial.println(count);
		   }
	   }	
}
#endif
