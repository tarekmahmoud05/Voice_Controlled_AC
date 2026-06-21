@startuml

package "High-Level Architecture" {
    [Voice_Controlled_AC.ino] 

}


package "Mid_Level Architecture"{
    package "Dio"{
        [Dio.ino]
        [Dio.h]
    }
    package "LCD"{
        [Lcd.ino]
        [Lcd.h]
    }

    package "Uart"{
    [Uart.ino]
    [Uart.h]
    }    
    package "Button"{
    [Button.ino]
    [Button.h]
    }
    package "Adc"{
    [Adc.ino]
    [Adc.h]
    }
    package "myString"{
    [myString.ino]
    [myString.h]
    }
    package "pwm"{
    [pwm.ino]
    [pwm.h]
    }

    package "DC_Motor"{
    [DC_Motor.ino]
    [DC_Motor.h]
    }
}




"High-Level Architecture".>"Mid_Level Architecture"
    

@enduml
