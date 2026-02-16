# Implement-multiple-patterns-on-LEDs-controlled-by-switches
This Embedded C program is designed to control 8 LEDs using 4 switches from a digital keypad. Each switch is assigned a unique LED glowing pattern. The main objective of this program is to display different LED patterns based on the switch pressed
 * File:   newmain.c
 * Author: HP
 * Created on February 11, 2026, 3:45 PM
 */


#include <xc.h>
#include "dkp.h"

void init_config()
{
    init_digital_keypad();
    TRISB=0;
    PORTB=0;
}



void main(void) {
    
    init_config();
    
    
    int unsigned i=0;
    int unsigned delay = 0;
    char prev=0x0f;
    while(1)
    {
     
        char key=read_digital_keypad(STATE_CHANGE);
        
        
        if(key!= ALL_RELEASED)
        {
            prev=key;
        }
        
        if(prev==SWITCH1)
        {
            if(delay++ == 5000)
            {
                delay = 0;
                
                                if(i<8)
                                {
                                 PORTB = PORTB | (0x01<<i);
                                 }
                                else if(i>=8 && i<16)
                                {
                                    PORTB=PORTB << 0x01;
                                }
                                else if(i>=16 && i<24 )
                                {
                                    PORTB=PORTB | (0x80>>(i-16));
                                }
                                else if(i>=24 && i<32)
                                {
                                    PORTB =PORTB>>0x01;
//                                     for(int i=0;i<2500;i++);
                                }
                                else
                                {
                                    i=-1;
                                }
                                i++;
            }
        }
            
        
        else if(prev==SWITCH2)
        {
            PORTB=0xF0;
            for(unsigned int i=50000;i--;);
            PORTB=0X0F;
            for(unsigned int i=50000;i--;);
        } 
        
        else if(prev==SWITCH3)
        {
            PORTB=0xAA;
            for(unsigned int i=50000;i--;);
            PORTB=0X55;
            for(unsigned int i=50000;i--;);
        }
        else if(prev==SWITCH4)
        {
            if(delay++ == 5000)
            {
                delay=0;
                       if(i<8)
                       {
                       PORTB = PORTB | (0x01 << i);
                       }
                       else if(i>=8 && i<16)
                       {
                        PORTB=PORTB << 0x01;
                       }
                       else
                       {
                           i=-1;
                       }
                i++;
            }
            
        }
        
            
    }
            
    return;
}

