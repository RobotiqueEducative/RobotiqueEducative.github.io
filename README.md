# RobotiqueEducative.github.io

# A first-level heading
## A second-level heading
### A third-level heading

```pyt
from hub import port
import force_sensor
import motor_pair
import runloop

motor_pair.pair(motor_pair.PAIR_1, port.C, port.D)

def pressed_detected():
    return force_sensor.pressed(port.A) == True

async def main():
    while True:
        if pressed_detected():
            motor_pair.move_for_time(motor_pair.PAIR_1, 3000, 0, velocity=200)

runloop.run(main())
```
