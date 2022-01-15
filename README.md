# Ryzentosh

OpenCore 0.7.4 EFI configuration for macOS Monterey 12.01 running on a ASUS Crosshair VII Hero X570 motherboard with an
AMD Ryzen 7 5800x.

Inspired by the fantastic Guide by AudioGod on the [AMD OSX Forums](https://forum.amd-osx.com/index.php?threads/audiogods-asus-rog-strix-x570-e-gaming-big-sur-monterey-beta-opencore-0-7-4-efi.1685/)
and the OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/).

# What's Working

| Component                             | Status |                                                                                                              |
|---------------------------------------|:------:|:-------------------------------------------------------------------------------------------------------------|
| AMD Ryzen 5800x                       | ✅     | via [AMD kernel patches](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore)                               |
| Processor tempeture, frequency        | ✅     | via [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor)                                       |
| Sleep                                 | ❓     | System crashes after a sleeping and CMOS needs to be reset (may be my machine)                               |
| NVMe                                  | ✅     | Yes [NVMeFix.kext](https://github.com/acidanthera/NVMeFix)                                                   |
| SATA                                  | ✅     | Native                                                                                                       |
| USB                                   | ✅     | via USBMap.kext                                                                                              |
| On-Board audio                        | ✅     | [AppleALC](https://github.com/acidanthera/AppleALC) working                                                  |
| Intel AX200 Bluetooth                 | ✅     | apparently native (USB)                                                                                      |
| Intel AX200 Wi-Fi                     | ❌     | [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) could perhaps do the trick. I use wired ethernet. |
| Realtek 2.5gbps ethernet              | ✅     | via [LucyRTL8125Ethernet.kext](https://github.com/Mieze/LucyRTL8125Ethernet)                                 |
| Sapphire Pulse RX580                  | ✅     | via [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen)                                       |
| Sapphire Pulse RX580  Audio (DP/HDMI) | ✅     | via [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen)                                       |

## BIOS Settings

- Enter BIOS -> Press Delete -> Enter Setup
- Exit -> Load Optimised Defaults
- Ai Tweaker -> Ai Overclock Tuner -> D.O.C.P.
- Advanced -> APM Configuration -> Power On By PCIe -> Disabled
- Advanced -> PCI Subsystem Settings -> Above 4G Decoding -> Enabled
- Advanced -> PCI Subsystem Settings -> Re-Size BAR Support -> Disabled
- Advanced -> USB Configuration -> Legacy USB Support -> Auto or Disabled
- Boot -> Boot Configuration -> Fast boot -> Disabled
- Boot -> CSM -> Launch CSM -> Disabled
- Boot -> Secure boot -> OS Type -> Windows UEFI mode
- Boot -> Secure boot -> Key Management -> Clear Secure Boot Keys

## Nvram
Before booting into MacOS using the new EFI for the first time make sure you reset your Nvram. (At the bootpicker press 
Space to reveal the option). You should do this every time you upgrade or make changes to your system.


## Core Count
Core Count patch needs to be modified to boot your system. Find the two algrey - Force cpuid_cores_per_package patches 
and alter the Replace value only.

Changing BA000000 0000/BA000000 0090* to BA `<CoreCount>` 0000 0000/BA `<CoreCount>` 0000 0090* substituting 
`<CoreCount>` with the hexadeciamal value matching your physical core count.

## SMBios

The machine configured is an `MacPro7,1`.

As per the guide, use [GenSMBios](https://github.com/corpnewt/GenSMBIOS) to generate valid serials.

You will need to update the following keys in `Config.plist` under `EFI/OC/Config.plist`. All keys reside under 
`PlatformInfo`

- `MLB`    
- `ROM`
- `SystemSerialNumber`
- `SystemUUID`


```
<dict>
    <key>AdviseFeatures</key>
    <false/>
    <key>MLB</key>
    <string>Update here</string>
    <key>MaxBIOSVersion</key>
    <false/>
    <key>ProcessorType</key>
    <integer>0</integer>
    <key>ROM</key>
    <data>
    aNvKvFUv
    </data>
    <key>SpoofVendor</key>
    <true/>
    <key>SystemMemoryStatus</key>
    <string>Auto</string>
    <key>SystemProductName</key>
    <string>MacPro7,1</string>
    <key>SystemSerialNumber</key>
    <string></string>
    <key>SystemUUID</key>
    <string></string>
</dict>
```

## Credits

- [AMD OSX/AudioGod](https://forum.amd-osx.com/index.php?threads/audiogods-asus-rog-strix-x570-e-gaming-big-sur-monterey-beta-opencore-0-7-4-efi.1685/)
- [Steeve/Ryzentosh](https://github.com/steeve/ryzentosh/blob/master/README.md)

