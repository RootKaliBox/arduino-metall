int ss0 = 0;
int ss1 = 0;
int ss2 = 0;
long c0 = 0;
long c1 = 0;
long c2 = 0;
byte i = 0;
int sss0 = 0;
int sss1 = 0;
int sss2 = 0;
int s0 = 0;
int s1 = 0;
int s2 = 0;

void setup()
{
  DDRB = 0xFF; // port B - all out
  DDRD = 0xFF; // port D - all out

  for (i = 0; i <255; i++) // калибровка
  {
    PORTB = B11111111; 
    delayMicroseconds(200); 
    PORTB = 0; 
    delayMicroseconds(20);
    s0 = analogRead(A0);
    s1 = analogRead(A0);
    s2 = analogRead(A0);
    c0 = c0 + s0;
    c1 = c1 + s1;
    c2 = c2 + s2;

    delay(3);
  }
  c0 = c0 / 255;
  c0 = c0 - 5;
  c1 = c1 / 255;
  c1 = c1 - 5;
  c2 = c2 / 255;
  c2 = c2 - 5;
}

void loop()
{
  PORTB = B11111111; 
  delayMicroseconds(200); 
  PORTB = 0; 
  delayMicroseconds(20); 
  s0 = analogRead(A0);
  s1 = analogRead(A0);
  s2 = analogRead(A0);
  ss0 = s0 - c0;

  if (ss0 < 0) 
  {
    sss0 = 1;
  } 
  ss0 = ss0 / 16; 
  PORTD = ss0; // посылаем на индикатор (send to LEDs)
  delay(1);

  ss1 = s1 - c1; 
  if (ss1 < 0) 
  {
    sss1 = 1; 
  } 
  ss1 = ss1 / 16;
  PORTD = ss1; // посылаем на индикатор (send to LEDs)
  delay(1);

  ss2 = s2 - c2;
  if (ss2 < 0)
  {
    sss2 = 1;
  }
  ss2 = ss2 / 16;
  PORTD = ss2; // посылаем на индикатор (send to LEDs)
  delay(1);

  if ( sss0+sss1+sss2 > 2)
  {
    digitalWrite(7,HIGH);
    digitalWrite(6,HIGH);
    digitalWrite(5,HIGH);
    digitalWrite(4,HIGH);
    digitalWrite(3,HIGH);
    digitalWrite(2,HIGH);
    digitalWrite(1,HIGH);
    digitalWrite(0,HIGH);
    delay(1);
    sss0 = 0;
    sss1 = 0;
    sss2 = 0;
  }
}
