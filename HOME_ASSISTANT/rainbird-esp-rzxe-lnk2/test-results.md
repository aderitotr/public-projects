# Test Results

## Hardware

- Controller: Rain Bird ESP-RZXe
- WiFi module: Rain Bird LNK2 WiFi

## Manual remote irrigation tests

| Duration | Home Assistant | Observation |
|---:|:---:|:---|
| 15 min | PASS | Started and completed |
| 30 min | PASS | Started and completed |
| 60 min | PASS | Started and completed |
| 61 min | PASS | Started |
| 91 min | PASS | Started |
| 99 min | PASS | Started |
| 100 min | PASS | Started |
| 101 min | FAIL | Did not start |
| 120 min | FAIL | Did not start |
| 180 min | FAIL | Did not start |

## Boundary

The exact observed boundary was:

**100 min -> PASS**

**101 min -> FAIL**

## Independent confirmation

The official Rain Bird mobile application limits manual zone watering to:

**1 h 40 min**

which is exactly:

**100 minutes**

This independently matches the Home Assistant test result.

## Conclusion

For the tested ESP-RZXe + LNK2 setup, the effective maximum manual remote irrigation duration is:

**100 minutes**

This should not be confused with programmable station-runtime limits documented elsewhere for the controller.

## Safety note

For long watering sequences implemented by an external automation platform, do not chain blocks blindly. Verify ON/OFF state between blocks and provide an explicit abort/stop mechanism.
