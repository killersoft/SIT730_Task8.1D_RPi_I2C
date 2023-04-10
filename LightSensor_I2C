#Data output of device typically:
# 
#[0, 23, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255, 255]
import smbus, time, math

DEVICE     = 0x23 # Default device I2C address when ADO pin is grounded OR 0x5C when a 3.3Volt signal is on the pin  
POWER_DOWN = 0x00 # (00000000 Control Code binary)=No active state
POWER_ON   = 0x01 # (00000001 Control Code binary )=Power on ()
RESET      = 0x07 # (00000111 Control Code binary )=Reset data register value.

#Low Resolution Mode - (4 lx precision, 16ms measurement time)
#High Resolution Mode - (1 lx precision, 120ms measurement time)
#High Resolution Mode 2 - (0.5 lx precision, 120ms measurement time)

# Start measurement at 0.5lx resolution. Time typically 120ms
CONTINUOUS_HIGH_RES_MODE_2 = 0x11

bus = smbus.SMBus(1) #Need to enable I2C on PI(Prefertences->Raspberri Pi Configuration->Interfaces (tick i2c))

def convertToNumber(data): #data is an array with 0=
  result=(data[1] + (256 * data[0])) / 1.2  #Get value in array[1] and if large value(include array[0] : 1.2=Per datasheet accuracy-typical
  return (result)

def readLight(addr=DEVICE):
  # Read data from I2C interface
  data = bus.read_i2c_block_data(addr,CONTINUOUS_HIGH_RES_MODE_2) #Using one time, automatically set to power down after measurement so lets not do that
  return convertToNumber(data)

def main():

    while True:
        lightLevel=math.ceil(readLight()) # Get rounded value
        try:
            if lightLevel <10 :
                print("Too Dark")
            if lightLevel >=10 and lightLevel <100 :
                print("Dark")     
            if lightLevel >=100 and lightLevel <500 :
                print("Medium")
            if lightLevel >=500 and lightLevel <50000 :
                print("Bright")
            if lightLevel >=50000 :
                print("Too Bright")
                
        except KeyboardInterrupt:
            print("done")
        time.sleep(0.5)
    
if __name__=="__main__":
   main()
