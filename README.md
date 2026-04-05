# 🔐 Locker Security System (FPGA Implementation)

A robust Java-simulated or FPGA-based digital numerical system designed to secure lockers (similar to those found in gyms, malls, or changing rooms). It allows users to set a custom 3-character passcode to lock and subsequently unlock a storage unit using hardware-mapped components.

---

## 🛠️ Hardware Features & Interface

The system is designed to interface with standard development board peripherals:

* **7-Segment Display (SSD)**: Used to visualize the character currently being entered (range: 0-9, A-F).
* **Status LEDs**:
    * `FREE_OCCUPIED` (LIBER_OCUPAT): Indicates if the locker is available (LED Off) or secured (LED On).
    * `ENTER_CHARACTERS` (INTRODU_CARACTERE): Signals that the system is ready for user input.
* **Control Buttons**:
    * **`ADD_DIGIT` (ADAUGA_CIFRA)**: Confirms the current digit/character and moves the sequence forward.
    * `UP / DOWN`: Used to cycle through the permitted alphanumeric characters (0-F).
    * `RESET`: Restores the system to its initial "Free" state and clears the SSD.

---

## 📋 Functional Workflow

### 1. Locking Phase (Setting the Code)
1.  **Start**: The locker begins in the **FREE** state (`FREE_OCCUPIED` LED is Off).
2.  **Initiate**: The user presses **`ADD_DIGIT`**. The `ENTER_CHARACTERS` LED lights up to signal the start of input.
3.  **Input**: The user selects **3 distinct characters** using the `UP/DOWN` buttons.
4.  **Confirm**: Each character is confirmed by pressing **`ADD_DIGIT`**. The current character remains visible until the next one is initiated.
5.  **Lock**: After the 3rd character is confirmed via **`ADD_DIGIT`**, the SSD turns off, the locker enters the **OCCUPIED** state (LED On), and input mode ends.

### 2. Unlocking Phase (Verification)
1.  **Initiate**: The user presses **`ADD_DIGIT`** to begin entering the unlock code.
2.  **Input**: The same 3-character entry process is repeated using `UP/DOWN` and **`ADD_DIGIT`**.
3.  **Verification**: Upon the final press of **`ADD_DIGIT`**, the system validates the attempt:
    * **Match**: `FREE_OCCUPIED` turns Off. The locker is now **FREE**.
    * **Mismatch**: `FREE_OCCUPIED` remains On, signaling the locker remains locked.

---

## 🏗️ Technical Architecture

The implementation is governed by a **Finite State Machine (FSM)**:
* **Debouncing Module**: Processes raw button signals to prevent multiple triggers from a single press.
* **SSD Decoder**: Maps the internal 4-bit hexadecimal value to the correct 7-segment patterns.
* **Security Registers**: A small memory block that holds the 3-digit code set during the locking phase.
* **Comparator Logic**: A hardware-level comparison between the "Stored Code" and "Entered Code" registers.

---

## 🎮 Use Case Example
1.  **Status**: Locker is Free.
2.  **Action**: Press **`ADD_DIGIT`**. SSD shows "0".
3.  **Selection**: Press `UP` twice ("2"). Press **`ADD_DIGIT`**.
4.  **Selection**: Press `UP` until "3" appears. Press **`ADD_DIGIT`**.
5.  **Selection**: Select "1". Press **`ADD_DIGIT`**.
6.  **Result**: Code "2-3-1" is saved. Locker is now **Occupied**.
