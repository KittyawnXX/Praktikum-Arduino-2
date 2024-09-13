// Code made with Luv 💙 by Kittyawn aka. Muhammad Alvian Akbar and Teams (Kaka, Ali, Diki, Yanuar)

const int bPin[] = {A1, A2, A3, A4, A5}; // Alamat untuk tiap tombol (disini saya menggunakan singkatan B<no>)
const int buttons = 5;
bool bState[] = {HIGH, HIGH, HIGH, HIGH, HIGH}; // Variable status last-Button

void setup() {
  // Pin LED D2-D7 (PORTD) sbg Output
  DDRD |= 0b11111100; // D2-D7 output (bits 2-7)

  // Pin LED D8-D9 (PORTB) sbg Output
  DDRB |= 0b00000011; // D8-D9 output (bits 0-1)

  // Setup untuk mematikan LED tiap program sedang "Break"
  PORTD &= ~0b11111100; // Turn off D2-D7
  PORTB &= ~0b00000011; // Turn off D8-D9

  // Setup internal Pull Up untuk tombol
  for (int i = 0; i < buttons; i++) {
    pinMode(bPin[i], INPUT_PULLUP);
  }
}

void loop() {
  for (int i = 0; i < buttons; i++) {
    int bPressed = digitalRead(bPin[i]);

    // Jika tombol baru saja di tekan
    if (bPressed == LOW && bState[i] == HIGH) {
      switch (i) {
        case 0:
          bt1();
          break;
        case 1:
          bt2();
          break;
        case 2:
          bt3();
          break;
        case 3:
          bt4();
          break;
        case 4:
          bt5();
          break;
      }
      delay(300); // Debounces
    }

    // Simpan status tombol
    bState[i] = bPressed;
  }
}

void bt1() {
  // Running Animations untuk Tombol1
  for (int i = 0; i < 8; i++) {
    setLed(i, true);
    delay(110);
    setLed(i, false);
  }
}

void bt2() {
  // Running Animations untuk Tombol2
  for (int i = 0; i < 8; i++) {
    setLed(i, true);
    delay(110);
    setLed(i, false);
  }
  
  // Running Animations kebalikannya
  for (int i = 7; i >= 0; i--) {
    setLed(i, true);
    delay(110);
    setLed(i, false);
  }
}

void bt3() {
  // Running Animations untuk Tombol3
  for (int i = 0; i < 4; i++) {
    setLed(i, true);
    setLed(7 - i, true);
    delay(110);
    setLed(i, false);
    setLed(7 - i, false);
  }
  
  for (int i = 3; i >= 0; i--) {
    setLed(i, true);
    setLed(7 - i, true);
    delay(110);
    setLed(i, false);
    setLed(7 - i, false);
  }
}

void bt4() {
  // Running Animations untuk Tombol4
  for (int i = 0; i < 8; i++) {
    setLed(i, true);
  }
  delay(110);
  
  for (int i = 0; i < 8; i++) {
    setLed(i, false);
    delay(110);
    setLed(i, true);
  }
  
  for (int i = 7; i >= 0; i--) {
    setLed(i, false);
    delay(110);
    setLed(i, true);
  }

  // Mematikan semua lampu setelah animasi selesai
  PORTD &= ~0b11111100; // Turn off D2-D7
  PORTB &= ~0b00000011; // Turn off D8-D9
}

void bt5() {
  // Running Animation untuk Tombol5
  byte pattern[] = {0b11111111, 0b11100111, 0b11011011, 0b10111101, 0b01111110, 0b10111101, 0b11011011, 0b11100111, 0b00000000};
  for (int step = 0; step < 9; step++) {
    setPattern(pattern[step]);
    delay(110);
  }
}

// Func untuk kontrol penstabil
void setLed(int index, bool state) {
  if (index < 6) { // LED D2 to D7 (PORTD)
    if (state) {
      PORTD |= (1 << (index + 2));
    } else {
      PORTD &= ~(1 << (index + 2));
    }
  } else { // LED D8 and D9 (PORTB)
    if (state) {
      PORTB |= (1 << (index - 6));
    } else {
      PORTB &= ~(1 << (index - 6));
    }
  }
}

// Func pattern untuk LED
void setPattern(byte pattern) {
  // D2-D7 pattern untuk 2 - 7 bit
  PORTD = (PORTD & ~0b11111100) | ((pattern & 0b111111) << 2);
  // D8-D9 pattern untuk 0 - 1 bit
  PORTB = (PORTB & ~0b00000011) | (pattern >> 6);
}
