# Project generator definitions

This repository defines definitions for targets and mcus. They are used to set proper data in the tools.

Since pgen v0.7, there's import command, please refer to project generator repository to find out more about how to get the mcu definition file. The idea is to provide a project which has defined MCU. pgen will import required data and creates mcu definition yaml file.

### Target

To generalize MCU's, there's a target definition available. The target name does not change based on the tool used. There's file board_definitions, where are all boards supported defined. There's a dictionary which translates the target name to the tool's specific MCU name.

If a project defines a valid target, the proper mcu settings will be set for a tool. The project must define its target with a name that is a substring of one of the files contained in this repository. For instance, a k64f is defined by the file mk64fn1m0xxx12.yaml. You can see that k64f is a substring of the filename. In your project, use any valid substring of mk64fn1m0xxx12 to specify k64f.

### MCU

MCU record defines the basic information about the mcu and specific settings for each tool. As an example is shown the uvision settings for the lpc1768 mcu. The data can be obtain from the project files. Look for keys TargetOption in uvision project file. You can use pgen's import command to import MCU information from a valid project file into this repository.

```
mcu:
    vendor: nxp
    name: lpc1768
    core: cortex-m3
tool_specific:
    uvision:
        TargetOption:
            Device: LPC1768
            DeviceId: 4868
```


## Tools specific definitions

### uVision

```
    uvision:
        TargetOption:
            Device: LPC1768
            DeviceId: 4868
```

All information in the code above are from uVision project. 

How to get all this information for a new mcu? Create a new project in uVision, select your target, save the project. 

Once you specified all needed information, test to build your project and check if the correct target is set in the uVision project. Then run the import command with the .uvproj file as the project file parameter.

### IAR

```
    iar:
        OGChipSelectEditMenu:
            state: LPC1768 NXP LPC1768
        OGCoreOrChip:
            state: 1
```

In the code above, LPC1768 is defined. To add a new target, create a new project in IAR, select your desired target, save the project. 

Once you specified all needed information, test to build your project and check if the correct target is set in the IAR project.

If it does, use the import commmand with the .ewp project file as a parameter.

