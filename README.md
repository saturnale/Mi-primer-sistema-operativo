/* Autores:                                   // Universidad de Colima - Facultad de Telemática
  Karen Alejandra Hernández Tomas             // Carrera: Ingeniería de Software - Primer semestre - Grupo I
  Stephanie Verduzco de la Mora               
  David Gustavo Fajardo Plascencia            // Mtro. Castellanos Berján Esli
  Angel Daniel Guerrero Mancilla              // Materia : Sistemas Operativos
  Diego Rincón Gallegos */

// Librerias ---------------------------------------------------------------------------------
#include <Wire.h>     // Sirve para la comunicación con dispositivos, por ejemplo, pantallas        
#include <LiquidCrystal_I2C.h>  // Permite controlar la pantalla LCD                                                
#include <RTClib.h>   //Reloj a tiempo real
LiquidCrystal_I2C lcd ( 0x27, 16, 2 );  // Inicializa el pantalla con 16 columnas y 2 filas
// -------------------------------------------------------------------------------------------

// -------------------- Definición de pines ---------------------------------------------
int ZumbadorPin = 2;
int buttonDec = 3;
int buttonIn = 4;
int buttonOp = 5;
int buttonDec2 = 6;
int buttonIn2 = 7;
int buttonRes = 8;
// --------------------------------------------------------------------------------------

// -------------- Usuario y contraseña preterminada para el inicio de sesión en consola ----------------
String userV = "xd";    
String passV = "xd";
// -----------------------------------------------------------------------------------------------------

float calculadora[2] = {0, 0}; // Vector para almacenar dos numero
const char operations[] = {'+', '-', '*', '/'};  // Vector que determinará la operación
int opIndex = 0; // Indice de la operación actual

// -------------------------------------------------------------------------------------
int c=261; 
int d=294;
int e=329;
int f=349;
int g=391;
int gS=415;
int a=440;
int aS=455;
int b=466;          // Notas para crear la cancion de Star Wars, se definen las frecuencias en Hertz
int cH=523;
int cSH=554;
int dH=587;
int dSH=622;
int eH=659;
int fH=698;
int fSH=740;
int gH=783;
int gSH=830;
int aH=880;
// ----------------------------------------------------------------------------------- 

void setup() {
  Serial.begin(9600); // Inicializa la comunicación serial
  lcd.init();         // Inicializa la pantalla LCD             
  lcd.backlight();    // Enciende la luz de fondo de la pantalla LCD

  pinMode(ZumbadorPin, OUTPUT); // Se configura el buzzer como salida
  // ----------------------------------------
  pinMode(buttonDec, INPUT_PULLUP);
  pinMode(buttonIn, INPUT_PULLUP);
  pinMode(buttonOp, INPUT_PULLUP);          // Se configuran los botones como salida con resistencias internas
  pinMode(buttonDec2, INPUT_PULLUP);
  pinMode(buttonIn2, INPUT_PULLUP);
  pinMode(buttonRes, INPUT_PULLUP);
  // --------------------------------------- 

  // -----------------------------------------
  delay(10);
  lcd.setCursor ( 0, 0 );             
  lcd.print ( "CleoOS 2kb RAM - 32kb flash" );      
  lcd.setCursor(0,1);                                    
  lcd.print("Hora: ");lcd.print(__TIME__);      // Muestra la información, hora y fecha
  delay ( 3000 );   
  lcd.setCursor(0,1);
  lcd.print("F: ");lcd.print(__DATE__);
  delay ( 1000 ); 
  // -----------------------------------------

  // -------------------------------------------- Generación de tonos para crear la canción de Star Wars
  tone(ZumbadorPin, a, 500);
  delay(500+50);
  tone(ZumbadorPin, a, 500);
  delay(500+50);     
  tone(ZumbadorPin, a, 500); 
  delay(500+50);
  tone(ZumbadorPin, f, 350);  
  delay(350+50);
  tone(ZumbadorPin, cH, 150);
  delay(150+50); 
  
  tone(ZumbadorPin, a, 500);
  delay(500+50);
  tone(ZumbadorPin, f, 350);
  delay(350+50);
  tone(ZumbadorPin, cH, 150);
  delay(150+50);
  tone(ZumbadorPin, a, 1000);   
  delay(1000+50);
  
  tone(ZumbadorPin, eH, 500);
  delay(500+50);
  tone(ZumbadorPin, eH, 500);
  delay(500+50);
  tone(ZumbadorPin, eH, 500); 
  delay(500+50);   
  tone(ZumbadorPin, fH, 350);
  delay(350+50); 
  tone(ZumbadorPin, cH, 150);
  delay(150+50);
  
  tone(ZumbadorPin, gS, 500);
  delay(500+50);
  tone(ZumbadorPin, f, 350);
  delay(350+50);
  tone(ZumbadorPin, cH, 150);
  delay(150+50);
  tone(ZumbadorPin, a, 1000);
  delay(1000+50);
  
  tone(ZumbadorPin, aH, 500);
  delay(500+50);
  tone(ZumbadorPin, a, 350);
  delay(350+50); 
  tone(ZumbadorPin, a, 150);
  delay(150+50);
  tone(ZumbadorPin, aH, 500);
  delay(500+50);
  tone(ZumbadorPin, gSH, 250); 
  delay(250+50);
  tone(ZumbadorPin, gH, 250);
  delay(250+50);
  
  tone(ZumbadorPin, fSH, 125);
  delay(125+50);
  tone(ZumbadorPin, fH, 125);
  delay(125+50);    
  tone(ZumbadorPin, fSH, 250);
  delay(250+50);
  delay(250);
  tone(ZumbadorPin, aS, 250);
  delay(250+50);    
  tone(ZumbadorPin, dSH, 500); 
  delay(500+50); 
  tone(ZumbadorPin, dH, 250); 
  delay(250+50); 
  tone(ZumbadorPin, cSH, 250);
  delay(250+50);  
  
  
  tone(ZumbadorPin, cH, 125);
  delay(125+50);  
  tone(ZumbadorPin, b, 125); 
  delay(125+50); 
  tone(ZumbadorPin, cH, 250); 
  delay(250+50);     
  delay(250);
  tone(ZumbadorPin, f, 125); 
  delay(125+50); 
  tone(ZumbadorPin, gS, 500); 
  delay(500+50); 
  tone(ZumbadorPin, f, 375); 
  delay(375+50); 
  tone(ZumbadorPin, a, 125);
  delay(125+50); 
  
  tone(ZumbadorPin, cH, 500); 
  delay(500+50);
  tone(ZumbadorPin, a, 375);  
  delay(375+50);
  tone(ZumbadorPin, cH, 125); 
  delay(125+50);
  tone(ZumbadorPin, eH, 1000); 
  delay(1000+50);

  
  tone(ZumbadorPin, aH, 500);
  delay(500+50);
  tone(ZumbadorPin, a, 350); 
  delay(350+50);
  tone(ZumbadorPin, a, 150);
  delay(150+50);
  tone(ZumbadorPin, aH, 500);
  delay(500+50);
  tone(ZumbadorPin, gSH, 250);
  delay(250+50); 
  tone(ZumbadorPin, gH, 250);
  delay(250+50);
  
  tone(ZumbadorPin, fSH, 125);
  delay(125+50);
  tone(ZumbadorPin, fH, 125);  
  delay(125+50);  
  tone(ZumbadorPin, fSH, 250);
  delay(250+50);
  delay(250);
  tone(ZumbadorPin, aS, 250);  
  delay(250+50);  
  tone(ZumbadorPin, dSH, 500);  
  delay(500+50);
  tone(ZumbadorPin, dH, 250);  
  delay(250+50);
  tone(ZumbadorPin, cSH, 250);
  delay(250+50);  

  
  tone(ZumbadorPin, cH, 125);  
  delay(125+50);
  tone(ZumbadorPin, b, 125);  
  delay(125+50);
  tone(ZumbadorPin, cH, 250);
  delay(250+50);      
  delay(250);
  tone(ZumbadorPin, f, 250); 
  delay(250+50); 
  tone(ZumbadorPin, gS, 500); 
  delay(500+50); 
  tone(ZumbadorPin, f, 375);  
  delay(375+50);
  tone(ZumbadorPin, cH, 125); 
  delay(125+50);
          
  tone(ZumbadorPin, a, 500); 
  delay(500+50);           
  tone(ZumbadorPin, f, 375);  
  delay(375+50);          
  tone(ZumbadorPin, c, 125); 
  delay(125+50);  
  tone(ZumbadorPin, a, 1000);
  delay(1000+50);
  // -------------------------------------------------------------- Fin de la canción

  // --------------------------------------------------------------
  for ( uint8_t i = 0; i < ( 40 ); i++ ) {  
    
    lcd.scrollDisplayRight ( );       
    delay ( 100 );                  
    
  }                                                                // Ciclos que crean un efecto de movimiento, primero a la derecha y luego a la izquierda
  
  for ( uint8_t i = 0; i < ( 40 ); i++ ) {  

    lcd.scrollDisplayLeft ( );       
    delay ( 100 );                    
    
  }
  // --------------------------------------------------------
  lcd.clear(); // Limpiar pantalla

}


void loop() {
// --------------------------------------------------- Lectura del usuario y contraseña
   Serial.println("Usuario: ");
    while (!Serial.available()); // Espera que el usuario ingrese el usuario
     String user = Serial.readStringUntil('\n'); // Lee el texto hasta que el usuario de "Enter"
    
    Serial.println("Contraseña: ");
    while (!Serial.available()); // Espera que el usuario ingrese la contraseña
     String pass = Serial.readStringUntil('\n'); // Lee el texto hasta que el usuario de "Enter"
// -------------------------------------------------------


    while (user == userV && pass == passV) {   /*Verificar que el usuario y contraseña sean iguales a las preterminadas, si se cumple ejecutará la calculadora  
                                                   si no, esperará nuevas entradas  */
    lcd.setCursor ( 0, 0 );
    lcd.print("Calculadora: "); // Imprime "calculadora" en el LCD
    lcd.setCursor ( 0, 1 );
    lcd.print(calculadora[0]); //Muestra el primer numero en el LCD

    if(digitalRead(buttonDec) == LOW) {  // Si se presiona el botón de Decrementa, asignado para para el primer número para la operación
      calculadora[0] = (calculadora[0] == 0) ? 9 : calculadora[0] - 1;  // Decrementa el primer número
      delay(200); // Espera para evitar lecturas rápidas
    }

    if(digitalRead(buttonIn) == LOW) { // Si se presiona el botón "In"
      calculadora[0] = (calculadora[0] == 9) ? 0 : calculadora[0] + 1; // Incrementa el primer número
      delay(200);
    }

    if(digitalRead(buttonOp) == LOW) { // Si se presiona el botón "Op"
      lcd.setCursor ( 5, 1 );
      lcd.print(operations[opIndex]);  // Imprime el operador actual
      opIndex = (opIndex == 3) ? 0 : opIndex + 1; // Cambia el operador
      delay(200); 
    }

    if(digitalRead(buttonDec2) == LOW){ // Si se presiona el botón "Dec2"
      calculadora[1] = (calculadora[1] == 0) ? 9 : calculadora[1] - 1; // Decrementa el segundo número para la operación
      lcd.setCursor( 7, 1 );
      lcd.print(calculadora[1]); // Imprime el segundo número en el LCD
      delay(200);
    }

    if(digitalRead(buttonIn2) == LOW){ // Sise presiona el botón "In2"
      calculadora[1] = (calculadora[1] == 9) ? 0 : calculadora[1] + 1; // Incrementará el segundo número
      lcd.setCursor( 7, 1 );
      lcd.print(calculadora[1]); // Imprime el segundo número
      delay(200);
    }

    if(digitalRead(buttonRes) == LOW){  // Si se presiona el botón "Res"
      lcd.setCursor( 10, 1 );
      lcd.print("= "); // Imprimirá el signo de "="
      float resultado = 0; // Se declara variable float, si es el resultado aparecen decimales

      // Dependiendo que operación seleccione el usuario, realizará la operación correspondiente
      switch(operations[opIndex - 1]){
        case '+':
          resultado = calculadora[0] + calculadora[1];  // Suma
          lcd.setCursor( 12, 1 );
          lcd.print(resultado); // Imprime resultado
        break;
        case '-':
          resultado = calculadora[0] - calculadora[1];  // Resta
          lcd.setCursor( 12, 1 );
          lcd.print(resultado);  // Imprime resultado
        break;
        case '*':
          resultado = calculadora[0] * calculadora[1];  // Multiplicación
          lcd.setCursor( 12, 1 );
          lcd.print(resultado);  // Imprime resultado
        break;
        default:
          break;
      }

      // La división se hace de manera especial
      if(opIndex == 0){
        Serial.println("division");
        if(calculadora[0] == 0 || calculadora[1] == 0){  // Verficar si hay división por 0
          lcd.setCursor( 12, 1 );
          lcd.print("Error"); // Si es asi, muestra error
          
        }else{
          resultado = calculadora[0] / calculadora[1];  // Sno es asi, realiza la división
          lcd.setCursor( 12, 1 );
          lcd.print(resultado); muestra el resultado
        }
      }
    }
    }
  }
