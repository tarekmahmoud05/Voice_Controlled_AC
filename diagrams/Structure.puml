@startuml
package "pkg" {
    [voice_Controlled_AC.ino]<...[Uart.h] : includes Uart functions for sending data
    [voice_Controlled_AC.ino]<...[Dio.h]: includes functions of inputs and outputs and number and string functions
    [voice_Controlled_AC.ino]<...[Adc.h]: includes functions of ADC reading via LDR sensor
    [voice_Controlled_AC.ino]<...[Lcd.h]: includes functions for displaying on LCD
    [voice_Controlled_AC.ino]<...[Pwm.h]: includes functions of PWM
    [voice_Controlled_AC.ino]<...[DC_Motor.h]: includes CD_Motor control funcitons
    [voice_Controlled_AC.ino]<...[Button.h]: includes buttons reading funcitons
    [voice_Controlled_AC.ino]<...[myString.h]: includes string funcitons
    [Uart.h]...>[Uart.ino]: Declaration
    [Dio.h]...>[Dio.ino] : Declaration
    [Adc.h]...>[Adc.ino] : Declaration
    [Lcd.h]...>[Lcd.ino] : Declaration
    [Pwm.h]...>[Pwm.ino] : Declaration
    [DC_Motor.h]...>[DC_Motor.h] : Declaration
    [Dio.h]...>[Led.ino] : includes digital i/o functions
    [myString.h]...>[myString.ino] : Declaration
    [Button.h]...>[Button.ino] : Declaration

}
@enduml
