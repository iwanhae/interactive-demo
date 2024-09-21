# TCP Congestion Control Interactive Demo

## Overview

This document serves as a comprehensive guide to replicate the TCP Congestion
Control Interactive Demo project. The project simulates various TCP congestion
control algorithms, allowing users to visualize and compare their behaviors
under different network conditions, including single and bulk packet loss
scenarios.

This guide includes:

- **Project Objectives**
- **Key Features**
- **Architecture Overview**
- **Algorithm Implementations**
- **Interface Design**
- **Important Considerations**
- **Recommendations for React Implementation**

---

## Project Objectives

- **Educational Tool**: Provide an interactive platform to understand and
  visualize how different TCP congestion control algorithms behave.
- **Algorithm Comparison**: Allow users to compare algorithms like TCP Tahoe,
  Reno, NewReno, CUBIC, Vegas, and BBR.
- **User Interaction**: Enable users to adjust network parameters, select
  algorithms, and inject packet loss to observe the effects.
- **Visualization**: Display real-time graphs of throughput, transmitted
  packets, and retransmitted packets.

---

## Key Features

- **Simulation Control**: Start, pause, and reset the simulation.
- **Network Configuration**: Adjust bandwidth, RTT, packet size, queue size, and
  background traffic.
- **Algorithm Selection**: Choose which TCP algorithms to simulate.
- **Algorithm Parameters**: Customize parameters specific to each algorithm.
- **Packet Loss Injection**: Inject single or bulk packet loss to observe
  algorithm responses.
- **Real-Time Graphs**: Visualize throughput, transmitted packets, and
  retransmitted packets.
- **Event Log**: Monitor events such as timeouts, packet losses, and state
  transitions.

---

## Architecture Overview

### High-Level Structure

The project consists of the following main components:

1. **Simulation Engine**: Handles the logic of simulating network conditions and
   algorithm behaviors.
2. **TCP Algorithm Classes**: Individual classes representing each TCP
   congestion control algorithm.
3. **User Interface (UI)**: Allows user interaction and displays graphs and
   event logs.
4. **Event Logging**: Captures and displays significant events during the
   simulation.

### Component Breakdown

- **Simulation Engine**:
  - Manages the simulation loop, timing, and coordination between algorithms and
    network conditions.
  - Updates the state of each algorithm based on packet transmissions and
    network events.

- **TCP Algorithm Classes**:
  - **Base Class (`TcpAlgorithm`)**: Defines common properties and methods.
  - **Derived Classes**: Each algorithm (Tahoe, Reno, NewReno, CUBIC, Vegas,
    BBR) extends `TcpAlgorithm` and implements specific behaviors.

- **UI Components**:
  - **Control Panel**: Contains simulation controls and packet loss injection
    buttons.
  - **Graphs**: Displays real-time data for throughput, transmitted packets, and
    retransmitted packets.
  - **Event Log**: Shows a log of events occurring during the simulation.
  - **Network Configuration**: Allows users to adjust network parameters.
  - **Algorithm Selection and Parameters**: Enables algorithm selection and
    parameter customization.

---

## Algorithm Implementations

Each TCP algorithm is implemented as a class that extends the base
`TcpAlgorithm` class. Below is a summary of each algorithm's implementation.

### 1. TcpAlgorithm (Base Class)

- **Properties**:
  - `name`: Algorithm name.
  - `params`: Algorithm-specific parameters.
  - `state`: Holds the current state of the algorithm (e.g., `cwnd`, `ssthresh`,
    `rtt`).

- **Methods**:
  - `initializeState()`: Initializes state variables.
  - `simulate(deltaTime)`: Simulates packet transmissions, loss detection, and
    congestion window updates.
  - `handleTimeout()`: Handles retransmission timeouts.
  - `onPacketAcked()`: Defines behavior when packets are acknowledged.
  - `onPacketLost()`: Defines behavior when packet loss is detected.
  - `logEvent(eventType, details)`: Logs significant events.

### 2. TCP Tahoe

- **Behavior**:
  - On timeout or packet loss, sets `cwnd` to 1 and halves `ssthresh`.
  - Implements slow start and congestion avoidance phases.

### 3. TCP Reno

- **Behavior**:
  - Implements fast retransmit and fast recovery.
  - On three duplicate ACKs, halves `ssthresh` and enters fast recovery.

### 4. TCP NewReno

- **Behavior**:
  - Enhances fast recovery to handle multiple packet losses within a window.
  - Remains in fast recovery until all lost packets are acknowledged.

### 5. TCP CUBIC

- **Behavior**:
  - Uses a cubic function to adjust `cwnd` based on time since the last
    congestion event.
  - Provides more aggressive window growth and better performance in
    high-bandwidth networks.

### 6. TCP Vegas

- **Behavior**:
  - Adjusts `cwnd` based on RTT measurements.
  - Aims to detect congestion before packet loss occurs by monitoring the
    difference between expected and actual throughput.

### 7. TCP BBR

- **Behavior**:
  - Estimates bottleneck bandwidth and minimizes queuing delay.
  - Adjusts `cwnd` based on estimated bandwidth and pacing gain.

---

## Interface Design

### Overview

The user interface is designed to be intuitive and interactive, allowing users
to control the simulation and observe results in real-time.

### Layout Components

1. **Control Panel**: Located at the top or side, containing:
   - **Simulation Controls**: Start, pause, and reset buttons.
   - **Packet Loss Injection**: Buttons to inject single or bulk packet loss.
2. **Graphs**: Central area displaying real-time graphs for:
   - **Throughput (Mbps)**
   - **Transmitted Packets per Second**
   - **Retransmitted Packets per Second**
3. **Event Log**: A scrollable area showing logged events with timestamps.
4. **Network Configuration**: Input fields for adjusting network parameters.
5. **Algorithm Selection**: Checkboxes to select which algorithms to simulate.
6. **Algorithm Parameters**: Dynamic input fields for algorithm-specific
   parameters.

### User Interaction Flow

1. **Configure Network and Algorithms**:
   - Adjust network parameters (bandwidth, RTT, packet size, etc.).
   - Select algorithms to simulate.
   - Set algorithm-specific parameters.
2. **Control Simulation**:
   - Start the simulation.
   - Pause or reset as needed.
   - Inject packet loss scenarios to observe algorithm responses.
3. **Observe Results**:
   - Monitor real-time graphs updating with simulation data.
   - Review the event log for detailed algorithm behaviors.

### UI Elements Details

- **Buttons**:
  - **Start**: Begins the simulation.
  - **Pause**: Pauses the simulation without resetting data.
  - **Reset**: Stops the simulation and clears data.
  - **Single Packet Loss**: Injects a one-time packet loss event.
  - **Bulk Packet Loss**: Simulates a period (e.g., 1 second) where all packets
    are lost.

- **Input Fields**:
  - **Network Parameters**:
    - Bandwidth (Mbps)
    - RTT (ms)
    - Packet Size (bytes)
    - Queue Size (packets)
    - Background Traffic (Mbps)
  - **Algorithm Parameters**:
    - Initial `cwnd` (packets)
    - Initial `ssthresh` (packets)
    - Algorithm-specific constants (e.g., CUBIC's scaling constant).

- **Graphs**:
  - Display data over a fixed time window (e.g., the last 30 seconds).
  - Use different colors for each algorithm for clarity.
  - Include axes labels and legends.

- **Event Log**:
  - Shows events with timestamps, algorithm names, event types, and details.
  - Automatically scrolls to the latest event.

---

## Important Considerations

### Simulation Accuracy

- **Packet Loss Modeling**:
  - Implement precise single packet loss to accurately test algorithm responses.
  - Ensure bulk packet loss correctly simulates prolonged periods without ACKs.

- **Time Management**:
  - Use consistent timing intervals for simulation steps and animations.
  - Update `timeSinceLastAck` appropriately to simulate timeouts.

- **Congestion Window Adjustments**:
  - Prevent `cwnd` growth during packet loss periods unless ACKs are received.
  - Implement algorithm-specific behaviors for `cwnd` adjustments after loss.

### Algorithm Behavior

- **TCP Tahoe and Reno**:
  - Handle timeouts and duplicate ACKs correctly.
  - Implement slow start, congestion avoidance, fast retransmit, and fast
    recovery.

- **TCP NewReno**:
  - Accurately model partial ACKs during fast recovery.
  - Stay in fast recovery until all lost packets are recovered.

- **TCP CUBIC**:
  - Calculate the cubic function correctly, considering time since the last
    loss.

- **TCP Vegas**:
  - Use RTT measurements to adjust `cwnd`.
  - Detect congestion before packet loss.

- **TCP BBR**:
  - Estimate bandwidth and adjust pacing accordingly.
  - Handle loss by reducing `cwnd` appropriately.

### User Experience

- **Responsive UI**:
  - Ensure the interface remains responsive during simulations.
  - Update graphs and event logs smoothly.

- **Error Handling**:
  - Validate user inputs for network and algorithm parameters.
  - Provide feedback or default values if inputs are invalid.

- **Accessibility**:
  - Use clear labels and instructions.
  - Consider colorblind-friendly palettes for graphs.

---

## Recommendations for React Implementation

### Component Structure

- **App Component**:
  - Root component managing global state and coordinating child components.

- **SimulationEngine Component**:
  - Handles the simulation logic, timing, and algorithm instances.
  - Uses React's state or context to update simulation data.

- **Algorithm Components**:
  - Define classes or hooks for each TCP algorithm.
  - Encapsulate algorithm-specific logic and state.

- **ControlPanel Component**:
  - Contains simulation controls and packet loss buttons.
  - Dispatches actions to start, pause, reset the simulation, or inject packet
    loss.

- **NetworkConfig Component**:
  - Provides input fields for network parameters.
  - Updates simulation settings on change.

- **AlgorithmSelector Component**:
  - Displays checkboxes for algorithm selection.
  - Manages algorithm activation and parameter inputs.

- **Graphs Component**:
  - Renders real-time graphs using a library like Chart.js or D3.js.
  - Receives simulation data as props and updates accordingly.

- **EventLog Component**:
  - Displays the event log.
  - Automatically scrolls to the latest event.

### State Management

- Use React's `useState` and `useEffect` hooks to manage component state.
- Consider using `useReducer` or a state management library like Redux for
  complex state updates.
- Implement a global context to share simulation data between components.

### Timing and Animation

- Use `setInterval` within `useEffect` hooks to manage the simulation loop and
  animations.
- Ensure timers are properly cleaned up to avoid memory leaks.

### Performance Optimization

- Avoid unnecessary re-renders by memoizing components and data where
  appropriate.
- Use React's `useMemo` and `useCallback` hooks to optimize performance.

### UI Libraries

- Consider using a UI framework like Material-UI or Ant Design for consistent
  styling and components.
- Use charting libraries compatible with React for graphs (e.g.,
  `react-chartjs-2`, `recharts`).

### Accessibility and Responsiveness

- Ensure the application is accessible by following ARIA guidelines.
- Make the UI responsive using CSS Flexbox or Grid and media queries.
- Test the interface on various screen sizes and devices.

---

## Additional Notes

- **Testing**:
  - Write unit tests for algorithms to verify correct behavior under various
    scenarios.
  - Use tools like Jest and React Testing Library for component testing.

- **Documentation**:
  - Comment code thoroughly to explain complex logic.
  - Maintain a README with setup instructions and usage guidelines.

- **Version Control**:
  - Use Git for version control.
  - Structure commits logically and provide meaningful commit messages.

---

## Conclusion

This document provides the necessary information for your team to replicate and
enhance the TCP Congestion Control Interactive Demo using React. By following
the architecture overview and considering the important aspects highlighted,
your team can create an educational and interactive tool that effectively
demonstrates TCP congestion control algorithms.

Feel free to reach out if you have any questions or need further clarification
on any part of this project.

---

## References

- **TCP Congestion Control Algorithms**:
  - [RFC 5681 - TCP Congestion Control](https://tools.ietf.org/html/rfc5681)
  - [RFC 8312 - CUBIC for Fast Long-Distance Networks](https://tools.ietf.org/html/rfc8312)
  - [TCP Vegas: New Techniques for Congestion Detection and Avoidance](https://www2.cs.duke.edu/courses/fall16/compsci514/papers/tcp-vegas.pdf)
  - [BBR Congestion Control](https://cloud.google.com/blog/products/gcp/tcp-bbr-congestion-control-comes-to-gcp-your-internet-just-got-faster)

- **React Documentation**:
  - [Official React Documentation](https://reactjs.org/docs/getting-started.html)
  - [Using the Effect Hook](https://reactjs.org/docs/hooks-effect.html)
  - [Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)

- **Charting Libraries**:
  - [Chart.js](https://www.chartjs.org/)
  - [React Chart.js 2](https://github.com/reactchartjs/react-chartjs-2)
  - [Recharts](https://recharts.org/en-US/)

- **UI Frameworks**:
  - [Material-UI](https://material-ui.com/)
  - [Ant Design](https://ant.design/)

---
