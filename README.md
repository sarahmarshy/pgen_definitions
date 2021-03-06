# Project generator definitions

This repository defines definitions for targets and mcus. They are used to set proper data in the tools.


### Target

To generalize MCUs, there's a target definition available. The target name does not change based on the tool used. 

If a project defines a valid target, the proper mcu settings will be set for a tool. The project must define its target with a name that is a substring of one of the filenames contained in this repository. For instance, a k64f is defined by the file mk64fn1m0xxx12.yaml. You can see that k64f is a substring of the filename. In your project, use any valid substring of mk64fn1m0xxx12 to specify k64f.

### MCU

MCU record defines the basic information about the mcu and specific settings for each tool. As an example is shown the uvision settings for the lpc1768 mcu. The data can be obtain from the project files. Look for keys TargetOption in uvision project file. You can use pgen's extract command to extract MCU information from a valid project file. You can submit a pull request to this repository if you want the target to be officially affed to pgen.

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
### Extract Command

Once you have a valid project file from a tool, you can use pgen's extract command.

The commands usage is as follows:
```python
pgen extract -m (MCU name) -f (Project File)
```
For a uvision project with lpc1768:
```python
pgen extract -mcu lpc1768 -f project.uvproj
```
The output yaml will be saved in the current directory.

The name you choose is important. I advocate using the part name. The name you choose will determine how projects can be specified for this target. When you specify a target, pgen will check for filenames of which the target name is a substring. For example, the above command will create lpc1768.yaml. To specify this target, you might use lpc1768 or 1768, as both are valid substrings. If the specified target is a substring of multiple filenames, pgen will prompt you to choose only one. If your project specifies "lpc" as its target, this might be a substring of both lpc1768 and lpc1766, etc. 



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

Once you specified all needed information, build your project and check if the correct target is set in the uVision project. Then run the extract command with the .uvproj file as the project file parameter.

### IAR

```
    iar:
        OGChipSelectEditMenu:
            state: LPC1768 NXP LPC1768
        OGCoreOrChip:
            state: 1
```

In the code above, LPC1768 is defined. To add a new target, create a new project in IAR, select your desired target, save the project. 

Once you specified all needed information, build your project and check if the correct target is set in the IAR project.

If the project builds, use the extract commmand with the .ewp project file as a parameter.


#####Note
The extension provided must be for of the tools that requires extra information about targets (Uvison, IAR, and Uvision5). The accepted extensions are '.uvproj', '.uvprojx', and '.ewp'.

