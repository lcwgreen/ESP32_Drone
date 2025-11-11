# ESP32_Drone
#Hardware development
  #schemetic drawing
  Power supply
  --> type C charger
      port: vbus (5V line) (1. need input protection elecments (fuse) 2.filter (big cap)
            CC1 and CC2 (control pin) : Define the usage of the USB C)
                if CC1,2 both in gnd, then this is a sink (power consumer, like my drone)
                if CC1,2 both in vbus, then this is a source (power provider, like battery)
            D+ and D- (data pins)
            Sbus1 and Sbus2 (sub-band)
            Shield (can connect to 0 ohm resistor then gnd)
     Routing: 
         VBUS:
           1. vbus --> 100k --> gnd (for detecting any power supply come in (safety))
           2. vbus --> P-type Mosfet --> battery (for switching power supply between usb and battery)
           3. vbus --> diode --> to main power rail (vbus symbol)
               mosfet --> main power rail (power supply from battery)
               main power rail --> TP4056 (charging battery)
               main power rail --> MIC5219-3.3YM5 (regulate the power from 5V to 3.3V)
               main power rail --> 10u cap --> gnd (filtering and stability)
        CC1, CC2: seperately --> 5.1k ohm --> gnd (tell the system this device is a sink, if no ohm then new type c cannot be used)


#Hardware development
