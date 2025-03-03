# Juego de Adivinar el número almacenado en Memoria: Jair Isaac Pérez Ricardez (21212024).

Este código es una implementación en ensamblador de un sencillo juego de adivinanza, donde el jugador debe adivinar un número secreto almacenado en la memoria. El programa proporciona retroalimentación al jugador sobre si la adivinanza es correcta, demasiado baja o demasiado alta.

## Descripción del Código

### Constantes

Se definen varias direcciones de memoria para almacenar valores relevantes del juego:

- **`SECRET_NUMBER_ADDR`**: Dirección de memoria para almacenar el número secreto.
- **`GUESS_ADDR`**: Dirección de memoria para almacenar la adivinanza del jugador.
- **`SUCCESS_FLAG`**: Dirección de memoria que almacena una bandera de éxito (1 si la adivinanza es correcta, 0 si es incorrecta).

### Inicialización

- Se carga un valor inmediato de 42 (el número secreto) en el registro `Ra` y luego se guarda en la memoria en la dirección de `SECRET_NUMBER_ADDR`.

### Bucle Principal del Juego

1. **Ingreso de la Adivinanza:**
   - El jugador ingresa su adivinanza, que se simula en este caso con un valor fijo (0). Este valor se guarda en `GUESS_ADDR`.

2. **Comparación:**
   - Se carga el número secreto de la memoria en el registro `Rb`.
   - Se compara la adivinanza del jugador (`Ra`) con el número secreto (`Rb`) utilizando la instrucción `cmp`.

3. **Condiciones:**
   - **Adivinanza correcta**: Si la adivinanza es igual al número secreto (`cmp Ra, Rb` con resultado cero), se establece la bandera de éxito (`SUCCESS_FLAG = 1`).
   - **Adivinanza demasiado baja**: Si la adivinanza es menor que el número secreto, se indica que la adivinanza es demasiado baja.
   - **Adivinanza demasiado alta**: Si la adivinanza es mayor que el número secreto, se indica que la adivinanza es demasiado alta.

### Flujo del Juego

El flujo básico del juego está determinado por los siguientes saltos condicionales:

- **Si la adivinanza es correcta**:
  - Se establece el `SUCCESS_FLAG` en 1 y el juego termina.

- **Si la adivinanza es demasiado baja**:
  - Se indica que la adivinanza es baja y el juego vuelve a pedir otra adivinanza.

- **Si la adivinanza es demasiado alta**:
  - Se indica que la adivinanza es alta y el juego vuelve a pedir otra adivinanza.

### Fin del Juego

Cuando el jugador adivina el número correctamente, se muestra un mensaje de éxito (simulado con un valor en `Rd`), y el programa se detiene con la instrucción `hlt`.

---

## Código Explicado

### Inicialización

```assembly
; Inicialización del número secreto
mva #42                ; Cargar el valor inmediato 42 en Ra
sto Ra, SECRET_NUMBER_ADDR  ; Almacenar Ra en la dirección de memoria SECRET_NUMBER_ADDR

; Constants
SECRET_NUMBER_ADDR = 0x10  ; Memory address to store the secret number
GUESS_ADDR        = 0x11  ; Memory address to store the player's guess
SUCCESS_FLAG      = 0x12  ; Memory address to store success flag (1 = correct, 0 = incorrect)

; Initialize secret number (e.g., 42)
mva #42                ; Load immediate value 42 into Ra
sto Ra, SECRET_NUMBER_ADDR  ; Store Ra into memory at SECRET_NUMBER_ADDR

; Main game loop
.game_loop:
  ; Prompt player to input guess (store in Ra)
  ; (Assume input is stored in Ra via some mechanism)
  ; For simplicity, we'll simulate input by loading a value
  mva #0                ; Simulate input (replace with actual input mechanism)
  sto Ra, GUESS_ADDR    ; Store Ra into memory at GUESS_ADDR

  ; Load secret number into Rb
  lod Rb, SECRET_NUMBER_ADDR

  ; Compare guess (Ra) with secret number (Rb)
  cmp Ra, Rb

  ; Check if guess is correct
  jz .correct_guess

  ; Check if guess is too low
  jn .too_low

  ; Otherwise, guess is too high
  jmp .too_high

; Guess is correct
.correct_guess:
  mva #1                ; Load immediate value 1 into Ra
  sto Ra, SUCCESS_FLAG  ; Store Ra into memory at SUCCESS_FLAG
  jmp .end_game

; Guess is too low
.too_low:
  ; Display "Too low" message (e.g., light up an LED or display on Rd)
  mvd #1                ; Load immediate value 1 into Rd (indicate "Too low")
  jmp .game_loop

; Guess is too high
.too_high:
  ; Display "Too high" message (e.g., light up an LED or display on Rd)
  mvd #2                ; Load immediate value 2 into Rd (indicate "Too high")
  jmp .game_loop

; End game
.end_game:
  ; Display success message (e.g., light up an LED or display on Rd)
  mvd #3                ; Load immediate value 3 into Rd (indicate "Correct guess")
  hlt                   ; Halt the program

