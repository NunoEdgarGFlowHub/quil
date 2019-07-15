# Example measurements

The following is an incomplete list of measurements an experimenter may use analog quil to perform.

## Readout cavity scan

```
# Static constants that must be set before program execution
# TODO: Define how $qubit is set. May be used in e.g. DEFCIRCUIT

# Other values that will be set by user input
DECLARE freq_min REAL
DECLARE freq_max REAL
DECLARE freq_step REAL
DECLARE scale REAL

# Values set by program
DECLARE buffer REAL[65536] # 16-bit memory
DECLARE iq REAL[2]
DECLARE freq REAL
DECLARE idx INTEGER
DECLARE continue BIT

# Assign initial values
MOVE idx 0
MOVE freq freq_min


LABEL @scan_step
SET-FREQUENCY $qubit "ro" freq
SET-SCALE $qubit "ro" scale
PULSE $qubit "ro" flat(duration: duration, iq: ?)
# TODO: Determine if capture_delay is needed, and how it is set
DELAY(capture_delay) $qubit # Round trip acquisition delay
CAPTURE $qubit "out" flat(duration: duration, iq: ?) iq

# Store results in the buffer, increment
STORE buffer idx iq[0]
ADD idx 1
STORE buffer idx iq[1]
ADD idx 1

ADD freq freq_step
LE continue freq freq_max
JUMP-WHEN @scan_step continue

# TODO: Finalize
```