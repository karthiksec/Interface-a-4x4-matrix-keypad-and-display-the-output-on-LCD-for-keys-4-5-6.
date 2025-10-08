# Interface-a-4x4-matrix-keypad-and-display-the-output-on-LCD-for-keys-4-5-6.
# NAME:G.KARTHIK
# REG NO:21223220043
# Step 1: Hardware Setup

Connect the 4x4 keypad to STM32:

Rows (output pins): PC0, PC1, PC2, PC3

Columns (input pins with pull-up): PC4, PC5, PC6, PC7

Connect the LCD to STM32:

Data pins (4-bit mode): PA0, PA1, PA2, PA3

Control pins (RS, EN): PB0, PB1

# Step 2: CODE
```
#include "main.h"
#include "lcd.h"
#include "stdbool.h"

bool col1, col2, col3, col4;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

void key(void);
  Lcd_PortType ports[] = {GPIOA, GPIOA, GPIOA, GPIOA};
  Lcd_PinType pins[] = {GPIO_PIN_3, GPIO_PIN_2, GPIO_PIN_1, GPIO_PIN_0};
  Lcd_HandleTypeDef lcd;

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  lcd = Lcd_create(ports, pins, GPIOB, GPIO_PIN_0, GPIOB, GPIO_PIN_1, LCD_4_BIT_MODE);

  while (1)
  {
    key();

  }
}

void key()
{


  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

  col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
  col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
  col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
  col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);

  if(!col1)
  {
    Lcd_cursor(&lcd, 0, 1);
    Lcd_string(&lcd, "Key 4\n");
    HAL_Delay(1500);
    col1 = 1;
  }
  else if(!col2)
  {
    Lcd_cursor(&lcd, 0, 1);
    Lcd_string(&lcd, "Key 5\n");
    HAL_Delay(1500);
    col2 = 1;
  }
  else if(!col3)
  {
    Lcd_cursor(&lcd, 0, 1);
    Lcd_string(&lcd, "Key 6\n");
    HAL_Delay(1500);
    col3 = 1;
  }
  else if(!col4)
  {
    Lcd_cursor(&lcd, 0, 1);
    Lcd_string(&lcd, "NIL\n");
    HAL_Delay(1500);
    col4 = 1;
  }

}


void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0|GPIO_PIN_1, GPIO_PIN_RESET);

  /*Configure GPIO pins : PC0 PC1 PC2 PC3 */
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  /*Configure GPIO pins : PA0 PA1 PA2 PA3 */
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  /*Configure GPIO pins : PC4 PC5 PC6 PC7 */
  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  /*Configure GPIO pins : PB0 PB1 */
  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}



```
# OUTPUT

<img width="1912" height="979" alt="Screenshot 2025-10-08 090656" src="https://github.com/user-attachments/assets/9d443d07-5cfc-432d-a6e1-1044c2b933b2" />
<img width="1919" height="981" alt="Screenshot 2025-10-08 090645" src="https://github.com/user-attachments/assets/45691555-3414-4ebc-80b8-21a54048df5c" />

<img width="1919" height="1021" alt="Screenshot 2025-10-08 090639" src="https://github.com/user-attachments/assets/a5e24e1d-2153-4e1c-b2f3-faad8a6abe10" />

# RESULT

The STM32 successfully scans ROW2 of the 4x4 keypad, detects keys 4, 5, and 6, and displays the pressed key on the LCD in real-time, while ignoring all other keys.
