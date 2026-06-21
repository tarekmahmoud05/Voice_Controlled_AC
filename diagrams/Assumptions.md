@startuml
package "Assumptions & Constraints" {
    package "Assumptions"{
  
    [The clock frequency is 16000000UL]
    [Push button connected in pulldown]
    [The target hardware platform is an AVR ATmega328P ]
    [The ADC reference voltage is set to AVcc, assumed to be 5V.] 
    [The LCD is in 4-bit mode]
    [PWM pin is connected to PB2]
}
    package "Constraints"{
    [UART baud rate fixed to 9600]
    [The motor speed is controlled via Timer 1 Fast PWM using an 8-bit resolution (0-255)]
    [Button must be debounced]
    [The button mapped speed is betweeen 165 and 255 due curent limitations]
    [UART input buffer size is 5]
    [Debounce is applied, button should be release after it is pressed]
    [The DC motor is only engaged when the temperature difference exceeds a threshold]
    }
}
@enduml
