# ESP32_Drone
#Hardware development
  #schemetic drawing
  Power supply
  --> type C charger
      pins: vbus (5V line) (1. need input protection elecments (fuse) 2.filter (big cap)
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
        D-, D+: D- connecting to other D-, same as D+ (able reverse connection of type C)
        SBUS1, SBUS2: they both floating, no need sub-band function
        Shield, GND: connecting together
  --> type C charger
  --> MIC5219-3.3YM5
      pins: 
            IN: higher power supply in (5V)
            Out: 3.3V out
            EN (Enable): =1 (able the chip work), =0 (unable the chip work)
            BP: small capacitor pin (reduce the output noise
            GND: common return
      Routing:
            IN: connecting the bus after passing the diode (power from USB-C or battery: 5V)
            Out: 
              1. 2.2uF to GND (small and faster capacitor -> filter out the high frequency noise)
              2. 10uF to GND (smooths slow voltage dips, if ESP suddenly draw lots current, this can provide a support)
              3. 470 ohm --> LED --> GND (indicate there is power supply from 3V3 chip)
              4. 3V3 net (power from the ESP/ other components)
            EN: connecting to a switch, switching from 5V to GND (provide a control for 3V3 supply)
            BP: passing a 470pF to gnd
            GND: to gnd      
  --> MIC5219-3.3YM5   
  
  Power supply
  
  Battery charger
  -->TP4056  
        pins: 
            TEMP: Temperature detector, if too hot then disable charging
            PROG: Determine the charing rate based on the resistor value (1100/ohm)
            GND: Same GND as the power input
            VCC: connecting to the power input (charge when usb pluged in)
            EP (Expose Pad): for heat disspitation, must connecting to the GND
            CE (Charging enable): high = enable charging, low stop charge
            CHRG-: Charging indicator 
            STDBY-: Charging complete indicator
            BAT: Connecting to battery 
        routing: 
            TEMP: directly to gnd, No need tempearture checking
            PROG: connecting 3.3Rohm, 1000/3.3= 333mA
            VCC: connecting to VBUS, directly draw current from VBUS
            EP: directly to gnd (outer shell)
            CE: Connecting to vbus
  -->TP4056  
  Battery charger
  
#Hardware development
