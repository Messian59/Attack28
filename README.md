# Attack28


# Arduino v1.0

It works with 2 optic encoders.

enco.h

#ifndef ENCO_H
#define ENCO_H


#include <Arduino.h>

class enco //clase carro_encoder
{
  private:
    int inte1; //
	int inte2; // 
	int ena; // 
    int in1; // 
	int in2; // 
	int enb; // 
	int in3; //
	int in4; // 	
 public:
     enco(int,int,int,int,int,int,int,int);
     void avanzar(int,int);	 // 
	 
};

#endif

enco.cpp

#include "enco.h"


enco::enco(int inz, int inx, int inc, int ind,  int inv, int ini, int inu, int iny)
{
     inte1=inz;
	 inte2=inx;
	 ena=inc;
     in1=ind;
	 in2=inv;
	 enb=ini;
	 in3=inu;
	 in4=iny;
	 
	 
     pinMode(inte1, OUTPUT);
	 pinMode(inte2, OUTPUT);
	 pinMode(ena, OUTPUT);
	 pinMode(in1, OUTPUT);
	 pinMode(in2, OUTPUT);
	 pinMode(enb, OUTPUT);
	 pinMode(in3, OUTPUT);
	 pinMode(in4, OUTPUT);
	 
	 
}
int contador_A = 0;
	int contador_B = 0;
void ISR_contador_A()
{
	contador_A++;
}
void ISR_contador_B()
{
	contador_B++;
}
int convertidor(float cm) 
{

  int resultado;  
  float circunferencia = (66.10 * 3.14) / 10; 
  float cm_step = circunferencia / 20;  
  
  float f_resultado = cm / cm_step;  
  resultado = (int) f_resultado; 
  
  return resultado;  

}



void enco::avanzar(int t,int p)
{
	 contador_A = 0;
	 contador_B = 0;
	attachInterrupt(digitalPinToInterrupt (inte1), ISR_contador_A, RISING);  
  attachInterrupt(digitalPinToInterrupt (inte2), ISR_contador_B, RISING);
	digitalWrite(in1,1);
	digitalWrite(in2,0);
	digitalWrite(in3,1);
	digitalWrite(in4,0);
	
	while (t > contador_A && t > contador_B)
	{
		if(t> contador_A)
		{
		 analogWrite(ena,p);
		 }
		 else{
			 analogWrite(ena,0);
		 }
	    if(t> contador_B)
		{
		 analogWrite(enb,p);
		 }
		 else{
			 analogWrite(enb,0);
		 }
	    
	}
	
	analogWrite(ena,0);
	analogWrite(enb,0);
	contador_A=0;
	contador_B=0;
}

principal
#include <enco.h>

void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}
# Arduino V2.0

It works with stepper motors.

componentes:

2 x 28BYJ-48 Stepper Motor with ULN2003 // 24 soles
1 x Arduino Uno // 30 soles
1 x tp4056 // 6 soles
1 x battery litio 3.7v 1200mah // 10 soles
1 X DC-DC converter // 8 soles
1 x servo SG90  // 10 soles
1 x protoboard  // 8 soles
Jumpers Hembra-Macho // 2 soles
Jumpers Macho-Macho // 2 soles
1 x Bola // 7 soles
1 x switch  // 0.50 soles
1 x o ring  // 3 soles
3d printend parts // 15 soles
1 x marcador negro // 2 soles 

______Programa principal___

#include <car.h>
#include <Servo.h>
Servo servo;

car C(13,12,11,10,5,4,3,2);

void setup() {
  
 servo.attach(7); 

}

void loop() {

delay(3000);
servo.write(15);
delay(3000);
C.derecha(40);
servo.write(0);
delay(3000);
C.avanzar(10);
servo.write(15);
delay(2000);
C.izquierda(200);
servo.write(0);
delay(3000);
C.avanzar(10);
servo.write(15);
delay(2000);
C.derecha(40);                                                                                 
servo.write(0);
delay(3000);
C.avanzar(10);
servo.write(15);
delay(200);
C.izquierda(200);
servo.write(0);
delay(3000);
C.avanzar(10);
servo.write(15);
delay(2000);
C.derecha(40);
servo.write(0);
delay(3000);
C.avanzar(10);
servo.write(15);
delay(2000);
C.izquierda(200);
servo.write(0);
delay(3000);
C.avanzar(10);
}

____car.h_________

#include <Arduino.h>

class car //clase carro
{
  private:
    int in1; //motor izquierdo
	int in2; // motor izquierdo
	int in3; // motor izquierdo
    int in4; // motor izquierdo
	int in5; // motor derecho	
	int in6; // motor derecho
	int in7; // motor derecho
	int in8; // motor derecho
	
 public:
     car(int,int,int,int,int,int,int,int);
     void avanzar(int);	 // avanzar por cm
	 void derecha(int);	 // angulo
	 void izquierda(int);
};

#endif

_____car.cpp______
#include "car.h"


car::car(int ina, int inb, int inc, int ind,  int ine, int inf, int ing, int inh)
{
     in1=ina;
	 in2=inb;
	 in3=inc;
     in4=ind;
	 in5=ine;
	 in6=inf;
	 in7=ing;
	 in8=inh;
	 
	 
     pinMode(in1, OUTPUT);
	 pinMode(in2, OUTPUT);
	 pinMode(in3, OUTPUT);
	 pinMode(in4, OUTPUT);
	 pinMode(in5, OUTPUT);
	 pinMode(in6, OUTPUT);
	 pinMode(in7, OUTPUT);
	 pinMode(in8, OUTPUT);
	 
	 
}
void car::avanzar(int t)
{
	
	
	int paso [4][4] =	
{
  {1, 1, 0, 0},
  {0, 1, 1, 0},
  {0, 0, 1, 1},
  {1, 0, 0, 1}
};
	  
 for (int i = 0; i < t*26; i++)	
  {
    for (int i = 0; i < 4; i++)		 
    {					
      digitalWrite(in1, paso[i][0]);	
      digitalWrite(in2, paso[i][1]);
      digitalWrite(in3, paso[i][2]);
      digitalWrite(in4, paso[i][3]);
	  digitalWrite(in5, paso[i][0]);	
      digitalWrite(in6, paso[i][1]);
      digitalWrite(in7, paso[i][2]);
      digitalWrite(in8, paso[i][3]);
      delay(10);
    }
  }


  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  digitalWrite(in5, LOW);
  digitalWrite(in6, LOW);
  digitalWrite(in7, LOW);
  digitalWrite(in8, LOW);
   
  
}

void car::derecha(int t)
{
	
	
	int paso [4][4] =	
{
  {1, 1, 0, 0},
  {0, 1, 1, 0},
  {0, 0, 1, 1},
  {1, 0, 0, 1}
};
	  
 for (int i = 0; i < t*2; i++)	
  {
    for (int i = 0; i < 4; i++)		 
    {					
      digitalWrite(in1, paso[i][0]);	
      digitalWrite(in2, paso[i][1]);
      digitalWrite(in3, paso[i][2]);
      digitalWrite(in4, paso[i][3]);
	  digitalWrite(in5, paso[i][3]);	
      digitalWrite(in6, paso[i][2]);
      digitalWrite(in7, paso[i][1]);
      digitalWrite(in8, paso[i][0]);
      delay(10);
    }
  }


  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  digitalWrite(in5, LOW);
  digitalWrite(in6, LOW);
  digitalWrite(in7, LOW);
  digitalWrite(in8, LOW);
   
  
}

void car::izquierda(int t)
{
	
	
	int paso [4][4] =	
{
  {1, 1, 0, 0},
  {0, 1, 1, 0},
  {0, 0, 1, 1},
  {1, 0, 0, 1}
};
	  
 for (int i = 0; i < t*2; i++)	
  {
    for (int i = 0; i < 4; i++)		 
    {					
      digitalWrite(in1, paso[i][3]);	
      digitalWrite(in2, paso[i][2]);
      digitalWrite(in3, paso[i][1]);
      digitalWrite(in4, paso[i][0]);
	  digitalWrite(in5, paso[i][0]);	
      digitalWrite(in6, paso[i][1]);
      digitalWrite(in7, paso[i][2]);
      digitalWrite(in8, paso[i][3]);
      delay(10);
    }
  }


  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  digitalWrite(in5, LOW);
  digitalWrite(in6, LOW);
  digitalWrite(in7, LOW);
  digitalWrite(in8, LOW);
   
  
}
