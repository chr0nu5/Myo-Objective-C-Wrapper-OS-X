Myo-Objective-C-Wrapper-OS-X by ShirazO
============================

<B>Objective-C Wrapper for C++ Myo.framework for OS X </B>

You need to add myo.framework from the Myo SDK to the Project, instructions for that can be found on the SDK.

<B>Here is an example of how you can get started:</B>

    // Create Myo Object
    Myo *aMyo = [[Myo alloc] initWithApplicationIdentifier:@"com.YourCompany.ExampleApp"];
    self.myo.delegate = self; // Set self as Delegate
    
    // Create Block To Run Commands In Background Thread
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, (unsigned long)NULL), ^(void) {
      
      // Create Loop To Keep Trying To Find & Connect To Myo
      BOOL found = false;
      while (!found) {
          found = [self.myo connectMyoWaiting:10000];
      }
      
      // Create Block To Run Commands on Main Thread
      dispatch_async(dispatch_get_main_queue(), ^(void) {
          
          self.myo.updateTime = 1000; // Set the Update Time
          [self.myo startUpdate]; // Start Getting Updates From Myo (This Command Runs on Background Thread In Implementation)
      });
    });
    
    // Add MyoDelegate To Your Class
    @interface ViewController : UIViewController <MyoDelegate>
    
    // Implement The Following Optional Delegate Methods To Handle The Events Triggered By The Listener
	- (void)myoOnLock:(Myo *)myo timestamp:(uint64_t)timestamp;
	- (void)myoOnUnlock:(Myo *)myo timestamp:(uint64_t)timestamp;
	- (void)myoOnUnpair:(Myo *)myo timestamp:(uint64_t)timestamp;
	- (void)myoOnArmUnsync:(Myo *)myo timestamp:(uint64_t)timestamp;
	- (void)myoOnDisconnect:(Myo *)myo timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onRssi:(int8_t)rssi timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onPose:(MyoPose *)pose timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onEmgData:(int *)emgData timestamp:(uint64_t)timestamp;
	- (void)myoOnPair:(Myo *)myo firmwareVersion:(NSString *)firmware timestamp:(uint64_t)timestamp;
	- (void)myoOnConnect:(Myo *)myo firmwareVersion:(NSString *)firmware timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onGyroscopeDataWithVector:(MyoVector *)vector timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onAccelerometerDataWithVector:(MyoVector *)vector timestamp:(uint64_t)timestamp;
	- (void)myoOnArmSync:(Myo *)myo arm:(MyoArm)arm direction:(MyoDirection)direction timestamp:(uint64_t)timestamp;
	- (void)myo:(Myo *)myo onOrientationDataWithRoll:(float)roll pitch:(float)pitch yaw:(float)yaw timestamp:(uint64_t)timestamp;
    
*Based on Myo-ObjectiveC-OSX by Kemcake
