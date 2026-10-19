## REQ-001: Engine Temperature Input

Requirement:
The system shall accept engine temperature as an integer value measured in degrees Celsius (°C).

Input:
Engine temperature (integer)

Expected behavior:
- 25°C → Accepted
- 100°C → Accepted
- -40°C → Accepted
- 100.5°C → INVALID_INPUT
- "100" → INVALID_INPUT

Questions:
- Does the system receive temperature directly from a sensor or through an API?
- Should numeric strings be converted to integers?
- How should the system handle floating-point values?

Test Conditions:
- Input is a positive integer
- Input is a negative integer
- Input is zero
- Input is a floating-point number
- Input is a string


## REQ-002: Valid Temperature Range

Requirement:
The system shall accept engine temperature values from -40°C to 150°C, including both boundary values.

Input:
Engine temperature (integer)

Expected behavior:
- -40°C → NORMAL
- 0°C → NORMAL
- 150°C → WARNING
- -41°C → INVALID_INPUT
- 151°C → INVALID_INPUT

Questions:
- Are the minimum and maximum temperature limits configurable?
- Should values outside the valid range be logged?
- Is a separate error notification required for out-of-range values?

Test Conditions:
- Temperature at minimum valid value (-40°C)
- Temperature just below minimum (-41°C)
- Temperature just above minimum (-39°C)
- Temperature at maximum valid value (150°C)
- Temperature just above maximum (151°C)
- Temperature just below maximum (149°C)

## REQ-003: Normal Temperature Status

Requirement:
The system shall return NORMAL when the engine temperature is greater than or equal to -40°C and less than or equal to 100°C.

Input:
Engine temperature (integer)

Expected behavior:
- -40°C → NORMAL
- 0°C → NORMAL
- 50°C → NORMAL
- 99°C → NORMAL
- 100°C → NORMAL

Questions:
- Is the normal temperature threshold configurable?
- Does the system need to continuously monitor temperature?
- Should the system record temperature changes within the normal range?

Test Conditions:
- Temperature at minimum valid value
- Temperature within normal range
- Temperature just below warning threshold
- Temperature exactly at warning threshold (100°C)

## REQ-004: High Temperature Warning

Requirement:
The system shall return WARNING when the engine temperature is greater than 100°C and less than or equal to 150°C.

Input:
Engine temperature (integer)

Expected behavior:
- 101°C -> WARNING
- 120°C -> WARNING
- 150°C -> WARNING

Questions:
- Is the threshold configurable?
- Is a time delay required?
- Does the warning persist after temperature drops?

Test Conditions:
- Temperature just above threshold
- Temperature within warning range
- Temperature just below maximum valid value

## REQ-005: Sensor Data Error Handling

Requirement:
The system shall return SENSOR_ERROR when the temperature sensor provides no data (None).

Input:
Engine temperature (integer)

Expected behavior:
- None → SENSOR_ERROR
- 0 → NORMAL
- 100 → NORMAL
- 101 → WARNING

Questions:
- How should the system distinguish missing sensor data from invalid input?
- Should the system retry reading sensor data?
- Is a timeout required before reporting a sensor error?
- Should SENSOR_ERROR trigger an alarm or notification?

Test Conditions:
- Sensor returns None
- Sensor returns zero instead of None
- Sensor returns valid temperature after previously returning None
- Sensor repeatedly returns None

## REQ-006: Invalid Input Handling

Requirement:
The system shall return INVALID_INPUT when the engine temperature is outside the valid range of -40°C to 150°C or when the input is not an integer, excluding None, which shall return SENSOR_ERROR.

Input:
Engine temperature (any data type)

Expected behavior:
- -41°C → INVALID_INPUT
- 151°C → INVALID_INPUT
- 100.5 → INVALID_INPUT
- "100" → INVALID_INPUT
- "abc" → INVALID_INPUT
- None → SENSOR_ERROR

Questions:
- Should the system automatically convert numeric strings to integers?
- Should boolean values (True, False) be considered invalid inputs?
- Should the system log invalid input values?
- Does SENSOR_ERROR take priority over INVALID_INPUT when the input is None?

Test Conditions:
- Temperature below minimum valid value
- Temperature above maximum valid value
- Input is a floating-point number
- Input is a numeric string
- Input is a non-numeric string
- Input is a boolean
- Input is None to verify error classification