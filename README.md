# StandableFBE

Standable Full Body Estimation (SFBE) is a C++ library that provides an efficient and easy-to-use solution for estimating a user's full-body pose from a limited set of input data. The library is designed to work with VR/AR applications, enabling developers to create immersive experiences that rely on accurate body tracking.

## Features
- Estimation of full-body pose using input data from Head-Mounted Display (HMD), and left and right hand controllers.
- Proportional body part scaling based on user's eye level.
- Gesture-based calibration for height adjustment.
- Robust pose estimation that accounts for various body proportions and user postures.

## Installation
- Download the pre-built library files for your target platform (macOS, Windows32, Windows64, or Android).
- Add the library files (.a, .dylib, .lib, .dll, or .so) to your project or system's library directory.
- Include the *SFBE.h* and *Geometry3DMath.h* header files in your project.

## Usage
1. Include the header files
```
#include "SFBE.h"
#include "Geometry3DMath.h"
```

2. Create an instance of the FBE class
```
standable::FBE fbe;
```

3. Set the skeleton proportions based on the user's eye level
```
fbe.proportionsFromEyeLevel(eyeLevel);
```

4. Optionally, enable gesture-based calibration for height adjustment
```
fbe.gestureCalibration = true;
```

5. Update the input transforms (HMD, left and right hand controllers) every frame
```
fbe.HMD = hmdTransform;
fbe.HandLeft = leftControllerTransform;
fbe.HandRight = rightControllerTransform;
```

Optionally, set the rotational offsets for the hand controllers.
For elbow estimation to work properly, forward axis of the controller should line up with the user's index finger.
```
fbe.setHandOffsets(Rotator);
```

6. Call the tick() function every frame, passing in the time elapsed since the previous frame (delta time)
```
fbe.tick(deltaTime);
```

7. Retrieve the final estimated pose
```
standable::Pose finalPose = fbe.Pose;
```
OR
```
geometryMath::Tform head = fbe.Pose.HMD;
geometryMath::Tform lFoot = fbe.Pose.FootLeft;
geometryMath::Tform rFoot = fbe.Pose.FootRight;
geometryMath::Tform hips = fbe.Pose.Pelvis;
...
```
In some cases a Pose component's Scale contains a Joint Target position:
- `Pose.HMD.Scale` - *none*
- `Pose.HandLeft.Scale` - Left Elbow joint target
- `Pose.HandRight.Scale` - Right Elbow joint target
- `Pose.Torso.Scale` - *none*
- `Pose.Pelvis.Scale` - *none*
- `Pose.FootLeft.Scale` - Left Knee joint target
- `Pose.FootRight.Scale` - Right Knee joint target

## Coordinate System / Geometry3DMath
StandableFBE uses the **Geometry3DMath** library for it's linear algebra calculations.

The `Pose` component of FBE is composed of multiple `Tform` variables. The `Tform` struct contains three components: `Tform.Vec3`, `Tform.Rotator`, and `Tform.Scale`.

This library uses a right-handed coordinate system, where:
- The positive X-axis points forward.
- The positive Y-axis points to the right.
- The positive Z-axis points upward.

Rotations follow the right-hand rule and are expressed in degrees:
- Roll (X-axis rotation): Positive values cause a clockwise rotation around the forward axis.
- Pitch (Y-axis rotation): Positive values cause a clockwise rotation around the right axis.
- Yaw (Z-axis rotation): Positive values cause a clockwise rotation around the up axis.

## Dependencies
**Geometry3DMath** is included within the SFBE library. Make sure to include the corresponding header file in your project.
```
# include "Geometry3DMath.h"
```
## Support
SFBE is not an open-source project, and its source code is not publicly available. If you find the library useful and would like to support its development, please consider becoming a patron on Standable's [Patreon Page](https://www.patreon.com/standable).

For any questions, issues, or feature requests, please contact the library's maintainer directly.

## Contact
For further information or support, please contact the library's maintainer at standablegames@gmail.com or the Contacts page on their [official website](https://www.standablevr.com).
