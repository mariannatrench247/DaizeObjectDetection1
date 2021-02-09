Object Detection for NANO 33 SENSE. Recognizes objects
Says "Hello" .When user responds with "Hello" "Hi" or similar WAKE WORD. Screen with pop up and correspond with response phrase. 


//TFLite(micro) library and Arduino NANO BLE 33 SENSE colorimeter and proximity sensor 

//proximity sensor 
    -instant depth reading of object in front of board  
    -instead of use of camera (no machine vision) 
    
  // if object close enough --> WE CAN SAMPLE COLOR 
            -ONBOARD RGB SENSOR 
            
 
 
 


// ARDUINO CREATE --> program board w/ application 
        // object_color_capture.ino
              // Samples color data from objects you place near it. 
              // Board sends color data as csv log to desktop machine over usb cable
              
              
  //https://blog.arduino.cc/2019/11/07/fruit-identification-using-arduino-and-tensorflow/            
              
              /*
  Object color sampler
  --------------------
  Samples the color of objects and outputs CSV logfile to serial console

  Hardware: Arduino Nano 33 BLE Sense board.

  Usage: Place object of interest to the color sensor

  This example code is in the public domain.
*/

#include <Arduino_APDS9960.h>

void setup() {

  Serial.begin(9600);
  while (!Serial) {};

  if (!APDS.begin()) {
    Serial.println("Error initializing APDS9960 sensor.");
  }

  // print the header
  Serial.println("Red,Green,Blue");
}

void loop() {
  int r, g, b, c, p;
  float sum;

  // wait for proximity and color sensor data
  while (!APDS.colorAvailable() || !APDS.proximityAvailable()) {}

  // read the color and proximity data
  APDS.readColor(r, g, b, c);
  sum = r + g + b;
  p = APDS.readProximity();

  // if object is close and well enough illumated
  if (p == 0 && c > 10 && sum > 0) {

    float redRatio = r / sum;
    float greenRatio = g / sum;
    float blueRatio = b / sum;

    // print the data in CSV format
    Serial.print(redRatio, 3);
    Serial.print(',');
    Serial.print(greenRatio, 3);
    Serial.print(',');
    Serial.print(blueRatio, 3);
    Serial.println();
  }
}
