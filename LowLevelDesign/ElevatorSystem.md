<img width="1181" height="625" alt="image" src="https://github.com/user-attachments/assets/49cfd6e1-05cb-41c8-857a-5d5733cd148b" />
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

### Operator
- **Enter maintenance mode:** To place a car in maintenance so it ignores hall- and in-car requests.
- **Exit maintenance mode:** To return the car to idle, it re-enters normal dispatch.
- **Acknowledge/resolve alerts:** To address alarms or emergencies (optional but useful).

### Use case diagram
Here’s the use case diagram of the elevator system:

<img width="500" height="700" alt="image" src="https://github.com/user-attachments/assets/bf70504b-433b-4903-88a4-1bd8fa371290" />


## [Class components of an elevator system](#class-components-of-an-elevator-system)

### Button
- `Button` is an abstract class. There can be four types of buttons: the door button, the elevator button, the hall button, and the emergency stop button. The status of the button determines whether it is pressed or unpressed. We can press the button or check its status through the Button class.
- The `ElevatorButton` subclass is inherited from the Button class and represents the buttons inside the elevator. When the elevator button is pressed, it specifies the destination floor of the elevator car or where the passenger wants to go.
- Similar to `ElevatorButton`, `HallButton` is also a subclass of the Button class. This class represents the buttons outside the elevator. It uses the enumeration `Direction` to specify whether the button is for going up or down. The hall button has two important pieces of information: the floor from where the button is pressed and the direction the passenger wants to move.
- Furthermore, there’s an `EmergencyButton` class that also inherits the behavior of the abstract Button class. This class responds to the passengers’ call when they press the emergency button. It alerts the support team to take prompt action and prevents other passengers from using the elevator car.
  
<img width="600" height="250" alt="image" src="https://github.com/user-attachments/assets/2f9ce37b-a059-4d68-b394-9bb3d0f41bda" />

### Elevator panel and hall panel
- `ElevatorPanel` is a class that represents the complete grid of buttons inside the elevator. In the elevator panel, we will have a list of buttons for selecting the destination floor, two buttons for closing and opening the elevator, and an emergency stop button.
- The `HallPanel` class represents the buttons that are outside the elevators. The hall panel consists of only two buttons: up and down.
- The elevator and hall panels are used to take input from the passenger.

<img width="400" height="150" alt="image" src="https://github.com/user-attachments/assets/22c5dee5-d65e-440f-ba1b-edcfac1932b3" />

### Display
- Every elevator has a display to represent the current floor number and direction (up or down) of the elevator. It also gives information about the capacity of the elevator.
- The `Display` class consists of the floor number, capacity, and direction. It has separate methods for both elevator display and hall display.
- The `showElevatorDisplay()` will display all of the class attributes. The `Display` class also has an `update()` method that changes the floor number, the capacity, and the direction of the elevator car.

<img width="150" height="150" alt="image" src="https://github.com/user-attachments/assets/28e927c0-1838-46e7-8aa8-51f1dcee4205" />

### Door
- The `Door` class symbolizes the door of an elevator. This class references the enum `DoorState`, which depicts that the door’s status can be open or closed.

  <img width="189" height="151" alt="image" src="https://github.com/user-attachments/assets/50449ca4-8a25-4696-af36-af9c92eb3ca1" />

### Elevator car
- `ElevatorCar` is the class that expresses the elevators of the building. Each elevator has a unique ID.
- This class consists of an enumeration named `ElevatirState` that tells the present state of the elevator.
- Moreover, every elevator car has a door, an elevator panel, and a display. The elevator car can start moving or stop on any floor.

<img width="221" height="486" alt="image" src="https://github.com/user-attachments/assets/03a77a1b-e29b-464e-92a0-fc2b2239991b" />

### Floor
- The `Floor` class represents the floors of a building. Each floor consists of several hall panels to call the lift and has displays to indicate the current floor and direction of the lift, as there is a separate panel and display for each elevator.
- Moreover, we will have `getFloorNumber()`, `getPanel()`, and `getDisplay()` functions to check if the floor is at the bottom or top, and update the panels and displays respectively.
- The “Down” button will be disabled if the floor is at the lowest level of the building, and the “Up” button will be disabled if the floor is topmost.

<img width="222" height="175" alt="image" src="https://github.com/user-attachments/assets/c793bd30-0a80-445e-af83-dad8552dc22d" />


### Building
- The `Building` class represents an actual building consisting of several floors and elevators.
  
<img width="203" height="116" alt="image" src="https://github.com/user-attachments/assets/5db72e51-07ec-4aa9-be77-65fbbb598d78" />

### Elevator system
- `ElevatorSystem` is the main functional class of the whole elevator control system.
- The elevator system displays each elevator and monitors the elevator cars.
- The elevator system has a `dispatcher` to select the best elevator car. Moreover, the system takes control of the elevator doors.

<img width="248" height="194" alt="image" src="https://github.com/user-attachments/assets/42825804-fbd4-45ee-8fd9-8d2501eb5eb9" />

### Enumerations
- The following is the list of enumerations required in the elevator system:
  - `ElevatorState`: It describes the state of an elevator, which could be idle, up, down, or maintenance.
  - `Direction`: When the elevator is not idle, it describes the direction of its motion, which could be up or down.
  - `DoorState`: When the elevator is idle, it describes the status of an elevator door that could be open or closed.

<img width="618" height="189" alt="image" src="https://github.com/user-attachments/assets/11065a8c-05f4-4a11-809c-9a0c8ecd15a7" />

## [Relationship between the classes](#relationship-between-the-classes)

### Aggregation
Aggregation is a “has-a” relationship where the container (whole) can exist independently of the contained (part). The part can also exist independently, and is often shared or referenced.
- `ElevatorSystem` has an aggregation relationship with `Building`.
  - The `ElevatorSystem` is the high-level controller and contains a `Building` instance (which has floors and elevator cars). The `Building` object can be reused or replaced if the system is reset.
- `Building` aggregates, `Floor` and `ElevatorCar`.
  - The `Building` has multiple floors and elevator cars. Even if the Building is destroyed, the floors and cars might still be conceptual entities (e.g., for reporting or migration).

<img width="537" height="358" alt="image" src="https://github.com/user-attachments/assets/a55dce35-ad61-41ec-bc6d-71dc38c38bb4" />

### Composition
Composition is a strong “part-of” relationship where the part cannot exist without the whole. If the whole is destroyed, its parts are destroyed too.
- `ElevatorCar` is composed of `Door`, `Display`, and `ElevatorPanel`. Each `ElevatorCar` owns its Door, Display, and Panel—these have no meaningful existence outside the car. If the car is decommissioned, so are its door, display, and panel.
- `Floor` is composed of `HallPanel` and `Display`. Each floor has a unique HallPanel and Display, which do not make sense independently of their floor.
- `HallPanel` contains `HallButtons`. A HallPanel comprises up/down buttons specific to it; these buttons are not shared with other panels.
- `ElevatorPanel` contains `ElevatorButtons`, `DoorButtons`, and an `EmergencyButton`. These buttons are part of the elevator’s control panel and are not shared across panels or elevators.

<img width="561" height="648" alt="image" src="https://github.com/user-attachments/assets/cfd6f22c-2c45-4703-b9ff-a5f23bf4d91a" />


### Inheritence
Inheritance is an “is-a” relationship. Subclasses share a contract and behavior with the superclass, and can be used wherever the superclass is expected.
- `ElevatorButton`, `HallButton`, `DoorButton`, and `EmergencyButton` all extend the abstract `Button` class.
  -  All buttons share common features (pressed state, press/reset actions), but each has specialized behavior and context. Inheriting from a base Button reduces duplication and supports polymorphism if needed.

## [Class Diagram of an Elevator System](#class-diagram-of-an-elevator-system)

<img width="1181" height="625" alt="image" src="https://github.com/user-attachments/assets/51f6f7da-d671-438a-a55e-07a34e8abf36" />

## [Sequence Diagram of an Elevator System](#sequence-diagram-of-an-elevator-system)

### Sequence diagram for Elevator call
The sequence diagram for an elevator call should have the following actors and objects that will interact with each other:
- **Actor:** `Passenger`
- **Objects:** `HallButton`, `ElevatorSystem`, `Dispatcher`, `ElevatorCar`, and `Door`

Here are the steps in the elevator call interaction:
1. The passenger presses the hall button to call the elevator.
2. The hall button signals the elevator system to call an elevator car to the passenger’s floor.
3. The elevator system checks for available cars (idle state) and ignores all cars in maintenance state.
4. The elevator system informs the dispatcher to select the best car.
5. The dispatcher returns the best car to the system.
6. The elevator system signals the elevator car to move to the passenger’s floor.
7. The elevator car signals the system when it arrives on the floor.
8. The system signals the hall button that the elevator has arrived.
9. The hall button is unpressed.
10. The elevator system signals the doors to open.
11. The door opens for the passenger.

<img width="780" height="596" alt="image" src="https://github.com/user-attachments/assets/d17f3b3d-4c9f-454f-aff9-ba2f6efecd59" />

## Sequence diagram for Elevator ride from one floor to another

<img width="968" height="837" alt="image" src="https://github.com/user-attachments/assets/bca0253d-0494-4805-bc08-1ee16f9136ee" />





    
