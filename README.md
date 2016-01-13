# pyspacenavigator
3Dconnexion Space Navigator in Python using raw HID (windows only)

Implements a simple interface to the 6 DoF 3Dconnexion [Space Navigator](http://www.3dconnexion.co.uk/products/spacemouse/spacenavigator.html) device.

Requires [pywinusb](https://pypi.python.org/pypi/pywinusb/) to access HID data -- this is Windows only.

## Basic Usage:

    import spacenavigator
    import time
    
    success = spacenavigator.open()
    if success:
      while 1:
        state = spacenavigator.read()
        print state.x, state.y, state.z
        time.sleep(0.5)
      
## State objects      
State objects returned from read() have 7 attributes: [t,x,y,z,roll,pitch,yaw,button].

* t: timestamp in seconds since the script started. 
* x,y,z: translations in the range [-1.0, 1.0] 
* roll, pitch, yaw: rotations in the range [-1.0, 1.0].
* button: bit 1: left button, bit 2: right button (e.g. 1=left, 2=right, 3=left+right, 0=none)

## API
    open(callback=None, button_callback=None)      
        Open a 3D space navigator device. Makes this device the current active device, which enables the module-level read() and close()
        calls. For multiple devices, use the read() and close() calls on the returned object instead, and don't use the module-level calls.
    
        Parameters:        
            callback: If callback is provided, it is called on each HID update with a copy of the current state namedtuple  
            button_callback: If button_callback is provided, it is called on each button push, with the arguments (state_tuple, button_state) 
            device: name of device to open. Must be one of the values in supported_devices. If None, chooses the first supported device found.            
        Returns:
            Device object if the device was opened successfully
            None if the device could not be opened
        
    read()              Return a namedtuple giving the current device state (t,x,y,z,roll,pitch,yaw,button)
    close()             Close the connection to the current device, if it is open
    set_led(state)      Set the status of the current devices LED to either on (True) or off (False)
    list_devices()      Return a list of supported devices found, or an empty list if none found
    
    
open() returns a DeviceSpec object. If you have multiple 3Dconnexion devices, you can use the object-oriented API to access them individually.
Each object has the following API, which functions exactly as the above API, but on a per-device basis:

    open()
    read()
    close()
    set_led()
    
There are also attributes:
    
    connected       True if the device is connected, False otherwise
    state           Convenience property which returns the same as read()
    
    
    
    


