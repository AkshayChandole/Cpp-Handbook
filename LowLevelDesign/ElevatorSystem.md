
# [Elevator System](#elevator-system)

<img width="854" height="421" alt="image" src="https://github.com/user-attachments/assets/8da1c5ba-79c8-4c1a-9dda-ea192f5a71d3" />


## [Requirement collection](#requirement-collection)

- R1: The elevator car system shall support a configurable number of floors (up to 15) and a configurable number of elevator cars (up to 3).
- R2: Each elevator car shall be capable of serving every floor and exist in one of four states: moving up, moving down, maintenance, or idle.
- R3: Elevator doors shall only open when the car is idle and auto-close after a configurable timeout unless the “Open” button is actively held.
- R4: Each floor shall have a panel with “Up”/“Down” call buttons, indicator lights, and an external display of the car’s current floor and direction.
- R5: Each elevator car shall have buttons, ground (0) to 15th floor, for every floor, “Open,” “Close,” and an emergency-stop button, plus an internal display showing the current floor, direction, and load status.
- R6: Pressing the emergency-stop button shall immediately halt the car, keep doors closed, and alert building security/operators.
- R7: Each elevator car shall enforce a maximum load of 680 kg, inhibit motion if this limit is exceeded, and emit an audible and visual alarm when overloaded.
- R8: The central controller shall assign the most appropriate car to each floor-call request, aiming to minimize average wait time, and command that car to move accordingly.
- R9: The elevator car system shall support calls from multiple passengers, with each passenger able to go to the same or different floors in the same or opposite direction.
- R10: The system shall support a maintenance state for each car in which:
  - R10a: The car is removed from normal dispatch (no new requests assigned).
  - R10b: All elevator doors remain closed, and panel inputs are ignored.
  - R10c: All UIs display “Maintenance” for that car.
  - R10d: Upon exiting maintenance, the car returns to idle state at its current floor before resuming service. 
- R11: All internal and external displays and indicators shall update in real time to reflect each elevator car’s current floor, direction, and operational state (idle, moving, maintenance, or emergency).
- R12: The system shall communicate all operational states, errors, and safety messages to users via audio/visual indicators and displays, ensuring passengers are always aware of the elevator’s status.

## [Different elements of our elevator system](#different-elements-of-our-elevator-system)
### System
The elevator control system includes the physical elevator cars, floor and in-car panels, and the central controller, which manages requests and car dispatch.

### Actors
#### Primary actors 
- Passenger: A passenger can do the following.
  - Call the elevator from any floor by pressing the “Up” or “Down” button on the floor panel.
  - Select a destination floor using the buttons on the in-car panel.
  - Request to open or close the doors via the “Open” and “Close” buttons on the in-car panel (when the car is stopped).
  - Trigger an emergency stop by pressing the emergency button inside the car.

#### Secondary actors
- Operator: The operator can place a car in or remove it from maintenance mode; the control system responds accordingly. It can:
  - Enter maintenance mode for a selected car, causing the elevator control system to remove it from normal dispatch and ignore hall or in-car requests.
  - Exit maintenance mode, returning the car to the idle state so it can resume service.
  - Receives alerts for emergencies.
 
## [Use Cases](#use-cases)
### Passenger
- **Call elevator:** To press the “Up” or “Down” button on the floor panel to call an elevator (only when the car is idle or passing).
- **Select destination floor:** To use the panel inside the elevator car, select the desired floor (only when the doors are open and the car is idle).
- **Request to open/close door:** To press the “Open” or “Close” button on the in-car panel to request door operation (only when the car is idle).
- **Trigger emergency stop:** To press the “Emergency” button inside the car to immediately halt the motion and alert the support team (allowed at any state except maintenance).


### Elevator control system
- **Move/stop elevator:** To move up or down or to stop the elevator on a specific floor. Transitions cars between Up, Down, and Idle states; respects the maintenance state.
- **Dispatch elevator:** To run the elevator-assignment algorithm on each CallElevator use case.
- **Update display (inside/outside):** To refresh in-car and floor displays with the current floor number and travel direction within 200 ms of any state change.
- **Operate doors:** To open or close doors on command (only in idle state).
- **Detect overload/sign alarm:** To ensure safety, inhibit motion and sound/flash overload alarm until cleared when the load exceeds capacity.
- **Notify operator:** To alert the operator or security in emergency or fault scenarios.

 
    
