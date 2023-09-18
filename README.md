# Morsejev Oddajnik na STM32

**Avtor:** Bor Križaj

## Uvod

Cilj te naloge je bil ustvariti Morsejev oddajnik, ki uporablja mikrokrmilnik STM32 za generiranje Morsejeve kode in jo prikazuje prek LED lučke. Morsejeva koda je bila uporabljena za kodiranje črk abecede in predstavlja temeljno komunikacijsko metodo s pomočjo kratkih pik in dolgih črt. To poročilo bo podrobno predstavilo način delovanja Morsejevega oddajnika in kako je bil program napisan ter izveden.

## Opis Delovanja

### Morsejeva Abeceda

Program Morsejevega oddajnika uporablja Morsejevo abecedo za pretvorbo črk v ustrezno Morsejevo kodo. Ta koda je bila v programu zapisana s pomočjo `switch case` stavka za vsako črko od A do Z. Primer kode za črko A je prikazan spodaj:

```c
case 'A':
    pika();
    crta();
    nula();
    break;


Funkcija pika() prižge LED lučko za 1 sekundo, crta() pa za 3 sekunde. Funkcija nula() se uporablja za ustvarjanje presledka med znaki in je dolg 5 sekund.



### Neskončna Zanka

Glavna funkcija `main()` vsebuje neskončno zanko, ki se izvaja neprestano. V tej zanki se vsaka črka besede preveri z uporabo funkcije `start()`. Za vsako črko se kliče funkcija `MorsejevaAbeceda()`, ki ustreznemu znaku dodeli ustrezno Morsejevo kodo. Program nato vklopi in izklopi LED lučko v skladu s to kodo.

## Koda Programa

Spodaj je celoten program Morsejevega oddajnika za mikrokrmilnik STM32:

```c
#include "stm32f4xx_hal.h"

uint8_t beseda; //array container of 2 initials

// FUNCTION : start
// DESCRIPTION :
// PARAMETERS : the character passing through to match.
// RETURNS : Void
void start(uint8_t input) {
    MorsejevaAbeceda(input);
    return;
}
// FUNCTION : MorsejevaAbeceda
// DESCRIPTION : Match the character if it is between A-Z//
// PARAMETERS : input : the character passing through to match.
// RETURNS : Void
void MorsejevaAbeceda(uint8_t input) {
    switch (input) {
        case 'A':
            pika();
            crta();
            nula();
            break;
        case 'B':
            crta();
            pika();
            pika();
            pika();
            nula();
            break;
        case 'C':
            crta();
            pika();
            crta();
            pika();
            nula();
            break;
        //... (ostale črke Morsejeve abecede)
    }
}
// FUNCTION : pika
// DESCRIPTION : "pika" indication of morse alphabet. On time: 1s. Off time: 1s.
// PARAMETERS : Void
// RETURNS : Void
void pika() {
    HAL_GPIO_WritePin(GPIOI, GPIO_PIN_2, GPIO_PIN_SET);
    HAL_Delay(1000);
    HAL_GPIO_WritePin(GPIOI, GPIO_PIN_2, GPIO_PIN_RESET);
    HAL_Delay(1000);
    return;
}
// FUNCTION : crta
// DESCRIPTION : "crta" indication of morse alphabet. On time: 3s. Off time: 1s.
// PARAMETERS : Void
// RETURNS : Void
void crta() {
    HAL_GPIO_WritePin(GPIOI, GPIO_PIN_2, GPIO_PIN_SET);
    HAL_Delay(3000);
    HAL_GPIO_WritePin(GPIOI, GPIO_PIN_2, GPIO_PIN_RESET);
    HAL_Delay(1000);
    return;
}
// FUNCTION : nula
// DESCRIPTION : "nula" indication of morse alphabet. Off time: 1s.
// PARAMETERS : Void
// RETURNS : Void
void nula() {
    HAL_Delay(5000);
    return;
}
int main(void) {
    while (1) {
        for (int i = 0; i < strlen(beseda); i++) {
            start(beseda[i]);
        }
    }
}


## Rezultati

Po izvajanju programa se LED lučka na STM32 mikrokrmilniku vključuje in izključuje v skladu z Morsejevo kodo, ki ustreza vneseni besedi. Tu je vizualna reprezentacija Morsejeve kode.

```lua
Beseda: "HELLO"
Morsejeva Koda: .... . .-.. .-.. ---



## Izzivi in Izkušnje

Med izvajanjem naloge smo se srečali z nekaj izzivi:

1. **Programiranje v C++**: Uporaba C++ je zahtevnejša kot na prejšnjih vajah, vendar bolj razumljiva.

2. **Natančno časovno upravljanje**: Morsejeva koda zahteva natančno časovno upravljanje za prikaz pike, črte in presledka med znaki. Funkcije `HAL_Delay()` so bile uporabljene za te namene, vendar obstajajo bolj napredne metode za upravljanje časa.

## Zaključek

Naloga je bila uspešno izvedena, saj smo ustvarili Morsejev oddajnik na STM32 mikrokrmilniku. Program je omogočal pretvorbo črk v Morsejevo kodo in prikaz te kode s pomočjo utripanja LED lučke. To predstavlja osnovno komunikacijsko metodo, ki je bila v preteklosti uporabljena za oddajanje sporočil prek telegrafije.

