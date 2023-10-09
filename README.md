# Grove - Vision AI Module
## How to connect Node MCU with Grove vision AI module
### You will need the following:
1. NodeMCU.
2. Grove Vision AI Module.
3. USBc cable.
4. Connection cable between AIgrove and nodemcu.

## Install GroveAI libraries
Link: https://github.com/Seeed-Studio/Seeed_Arduino_GroveAI?search=1
This is the link given to install the libraries. I don't get it, there is is some files with code, where is the download button??
Update: after looking for a while i found it here:

### Library Installation
Since we have downloaded the zip Library, open your Arduino IDE, click on Sketch > Include Library > Add .ZIP Library. Choose the zip file you just downloaded，and if the library install correct, you will see Library added to your libraries in the notice window. Which means the library is installed successfully.










## Troubleshoot
Hoe sluit ik grove Ai module aan Node MCU?
welke kabels heb ik nodig?

NodeMCU I2C with Arduino IDE
Geverivierd en Geupload

Seeed_Arduino_GroveAI.h staat niet in de library (tenminste niet op deze manier

Compilation error status exit 1 na het uploaden van deze code:

#include "Seeed_Arduino_GroveAI.h"
#include <Wire.h>

GroveAI ai(Wire);
uint8_t state = 0;
void setup()
{
  Wire.begin();
  Serial.begin(115200);

   Serial.println("begin");
  if (ai.begin(ALGO_OBJECT_DETECTION, (MODEL_INDEX_T)0x11)) // Object detection and pre-trained model 1
  {
    Serial.print("Version: ");
    Serial.println(ai.version());
    Serial.print("ID: ");
    Serial.println( ai.id());
    Serial.print("Algo: ");
    Serial.println( ai.algo());
    Serial.print("Model: ");
    Serial.println(ai.model());
    Serial.print("Confidence: ");
    Serial.println(ai.confidence());
    state = 1;
  }
  else
  {
    Serial.println("Algo begin failed.");
  }
}

void loop()
{
  if (state == 1)
  {
    uint32_t tick = millis();
    if (ai.invoke()) // begin invoke
    {
      while (1) // wait for invoking finished
      {
        CMD_STATE_T ret = ai.state();
        if (ret == CMD_STATE_IDLE)
        {
          break;
        }
        delay(20);
      }

     uint8_t len = ai.get_result_len(); // receive how many people detect
     if(len)
     {
       int time1 = millis() - tick;
       Serial.print("Time consuming: ");
       Serial.println(time1);
       Serial.print("Number of people: ");
       Serial.println(len);
       object_detection_t data;       //get data

       for (int i = 0; i < len; i++)
       {
          Serial.println("result:detected");
          Serial.print("Detecting and calculating: ");
          Serial.println(i+1);
          ai.get_result(i, (uint8_t*)&data, sizeof(object_detection_t)); //get result

          Serial.print("confidence:");
          Serial.print(data.confidence);
          Serial.println();
        }
     }
     else
     {
       Serial.println("No identification");
     }
    }
    else
    {
      delay(1000);
      Serial.println("Invoke Failed.");
    }
  }
  else
  {
    state == 0;
  }
}



Sources:
https://www.electronicwings.com/nodemcu/nodemcu-i2c-with-arduino-ide
https://wiki.seeedstudio.com/Grove-Vision-AI-Module/
(Oplossing?)
https://www.14core.com/wiring-nodemcu-esp8266-12e-with-i2c-16x2-lcd-screen/


