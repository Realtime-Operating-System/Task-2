# STM32 FreeRTOS Task Implementation

This project demonstrates the use of FreeRTOS on STM32 to manage multiple tasks for button input, ADC reading, LED control, and UART communication.

## Task Descriptions

### 1. `pickButtonTask`
The `pickButtonTask` is configured in the `pickButton` function to read the status of a button connected to pin PB9. In this function, it checks whether the button is pressed using the `HAL_GPIO_ReadPin` command. If the button is pressed (GPIO logic level `GPIO_PIN_RESET`), the variable `button1_pressed` is set to 1, indicating that the button has been pressed. To avoid button bouncing, a delay of 300 milliseconds (`osDelay`) is used as a debounce mechanism.

### 2. `getADCTask`
The `getADCTask` is configured in the `getADC` function, responsible for reading the value of a potentiometer connected to pin PA0 using the ADC (Analog to Digital Converter) feature. In each loop, this task starts the ADC using `HAL_ADC_Start`, waits for the conversion to complete, and retrieves the ADC value, which is stored in the `x_val` variable using `HAL_ADC_GetValue`. After the conversion is done, the ADC is stopped with `HAL_ADC_Stop`.

### 3. `dispLEDTask`
The `dispLEDTask` is configured in the `dispLED` function, which manages the two LEDs connected to pins PB0 and PB1. The function reads the ADC value (`x_val`) obtained from `getADC`. If `x_val` is greater than half of the full scale (4096/2), both LEDs (PB0 and PB1) will turn on. Otherwise, both LEDs will turn off.

### 4. `dispUARTTask`
The `dispUARTTask` is configured in the `dispUART` function to display information on the serial monitor using UART communication. Initially, this task displays the main menu using `Menu_Display`. Then, it monitors user input via UART (using `HAL_UART_Receive`) and reads the characters sent to determine the selected command. If the user chooses option 1 (by sending the character '1'), this task will display the ADC value (`x_val`) obtained from `getADC` on the serial monitor. If option 2 is chosen (by sending the character '2'), the task will redisplay the menu. Additionally, if the button (monitored by `pickButton`) is pressed, the `dispUART` task will display the message "Button1 pressed" on the serial monitor.

## Diagram
![diagram](https://github.com/user-attachments/assets/617a1c90-4bbc-4c8a-89c1-2543794538af)

## Demo
![Video Demo]([https://github.com/user-attachments/assets/4104e2a2-b2be-41cd-9762-dd3282ddc70a](https://github.com/user-attachments/assets/c93ed753-c58e-407c-b26c-b284fcffe879.mp4))

## Requirements
- STM32CubeIDE
- FreeRTOS library
- STM32 development board (with PB9 button, PA0 potentiometer, and PB0/PB1 LEDs)

## Task Timing
- `pickButtonTask`: 300ms debounce delay
- `getADCTask`: continuous ADC polling
- `dispLEDTask`: LED control based on ADC value
- `dispUARTTask`: UART polling and display

