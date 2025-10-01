# Tuya TS0601 Cover Custom Quirks

Custom ZHA quirks for Tuya TS0601 cover and blind devices with enhanced battery support for Zemismart models.

## Overview

This repository contains custom Zigbee quirks for Home Assistant's ZHA integration to properly support Tuya-based window covers and blinds, particularly Zemismart battery-powered models that require special handling for battery percentage reporting.

## Files

- **ts0601_cover_custom.py** - Production quirk file with custom battery handling for Zemismart devices
- **ts0601_cover_orig.py** - Original quirk file for reference

## Features

### Custom Battery Support

The custom quirk includes a specialized `ZemismartManufClusterBattery` class that:
- Intercepts battery percentage reports from DP 13 (Data Point 13)
- Properly decodes battery values and updates the power configuration cluster
- Maintains compatibility with position reporting and cover control

### Supported Devices

#### Battery-Powered Models (TuyaZemismartSmartCover0601_3_battery)
- `_TZE200_68nvbio9` (ZM85EL-1Z)
- `_TZE200_pw7mji0l`
- `_TZE200_cf1sl3tj` (ZM85EL-2Z)

These models include both battery percentage reporting and position updates.

#### Standard Models
Multiple device variants are supported with different configurations:
- Standard control models
- Inverted control models (reversed open/close commands)
- Inverted position models (reversed position reporting)

## Installation

1. Copy `ts0601_cover_custom.py` to your Home Assistant custom quirks directory:
   ```
   config/custom_zha_quirks/ts0601_cover_custom.py
   ```

2. Ensure the custom quirks directory is configured in your ZHA integration

3. Restart Home Assistant

4. Re-pair your Tuya cover devices (or reconfigure existing ones)

## Recent Changes

### Fix: Cover Position Updates for Battery-Powered Devices

Added `TuyaManufacturerWindowCover` to the INPUT_CLUSTERS for battery-powered devices to enable proper position reporting while maintaining battery functionality.

**Before:** Battery reporting worked but position updates were missing
**After:** Both battery percentage and cover position report correctly

## Technical Details

### Battery Reporting

Battery data is received via Tuya DP 13 with command ID `0x020D` (525). The battery percentage is extracted from the last byte of the data payload and converted to Zigbee's 0-200 range (0-100% × 2).

### Cluster Configuration

Battery-powered devices use:
- `ZemismartManufClusterBattery` - Custom battery handling
- `TuyaManufacturerWindowCover` - Position reporting
- `TuyaWindowCoverControl` - Cover commands (open/close/stop)
- `TuyaPowerConfigurationCluster` - Battery state reporting

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss the proposed changes.

## License

This code extends the ZHA quirks from the zigpy project and follows the same license terms.
