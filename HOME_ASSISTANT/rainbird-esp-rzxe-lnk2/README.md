# Rain Bird ESP-RZXe + LNK2 WiFi
## Manual irrigation duration limit

### Summary

During real-world testing with a Rain Bird ESP-RZXe controller and LNK2 WiFi module, manual remote irrigation was found to have an effective maximum duration of **100 minutes**.

The same limit was reproduced independently in two ways:

- Home Assistant / Rain Bird integration
- Official Rain Bird mobile app

The official Rain Bird app limits the manual-zone duration selector to:

**1 h 40 min = 100 minutes**

### Boundary test

The decisive Home Assistant test was:

- **100 minutes: starts successfully**
- **101 minutes: does not start**

Additional tests were performed at several durations.

| Requested duration | Result |
|---:|:---|
| 15 min | PASS |
| 30 min | PASS |
| 60 min | PASS |
| 61 min | PASS |
| 91 min | PASS |
| 99 min | PASS |
| 100 min | PASS |
| 101 min | FAIL |
| 120 min | FAIL |
| 180 min | FAIL |

### Important distinction

Documentation for the ESP-RZXe may mention programmable station runtimes of up to **199 minutes**.

That value should not automatically be interpreted as the maximum duration for a **manual remote start through LNK2**.

On the tested setup:

**Manual remote irrigation maximum = 100 minutes**

### Home Assistant example

A manual start at the tested maximum:

```yaml
action: rainbird.start_irrigation
target:
  entity_id: switch.rain_bird_sprinkler_zone
data:
  duration: 100
```

A request for 101 minutes or more may be accepted by the Home Assistant service call, while the physical irrigation zone does not start.

For this reason, service-call success alone should not be treated as confirmation that irrigation actually started.

### Workaround for durations above 100 minutes

Long watering periods can be divided into sequential blocks no longer than 100 minutes.

Examples:

- 120 min -> 100 + 20
- 180 min -> 100 + 80
- 240 min -> 100 + 100 + 40

A robust implementation should:

1. start one block;
2. verify that the irrigation zone actually became active;
3. wait for the block to finish;
4. verify that the zone is OFF;
5. only then start the next block;
6. provide a safe stop/abort mechanism;
7. log failed starts and each executed block.

### Tested hardware

- Rain Bird ESP-RZXe
- Rain Bird LNK2 WiFi
- Home Assistant Rain Bird integration
- Official Rain Bird mobile app

### Scope and limitations

This result was obtained on one real ESP-RZXe + LNK2 installation.

Firmware versions, hardware revisions, other Rain Bird controllers or future software versions may behave differently.

If you have the same hardware and obtain different results, please open an Issue with your controller model, firmware/software details and test results.

### Detailed results

See [test-results.md](test-results.md).
