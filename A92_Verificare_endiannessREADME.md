### **Program ASM (x86 pe 16 biți) pentru verificarea endianness**

Arhitectura unui sistem poate fi **little-endian** sau **big-endian**.  
- În **little-endian**, cel mai puțin semnificativ byte (LSB) este stocat primul (la adresa cea mai mică).  
- În **big-endian**, cel mai semnificativ byte (MSB) este stocat primul.

Procesorul **x86** este **little-endian**, dar putem scrie un program pentru a verifica această proprietate.

---

### **Explicație detaliată a codului**

#### **1. Ce este Endianness?**
- **Little-endian**: Byte-ul cel mai puțin semnificativ este stocat la adresa cea mai mică.
  - Exemplu: `1234h` este stocat în memorie ca **`34 12`**.
- **Big-endian**: Byte-ul cel mai semnificativ este stocat la adresa cea mai mică.
  - Exemplu: `1234h` este stocat în memorie ca **`12 34`**.

---

#### **2. Inițializarea segmentului de date**
```asm
mov ax, @data
mov ds, ax
```
- Configurăm segmentul **DS** pentru a putea accesa variabilele din secțiunea `data segment`.

---

#### **3. Verificarea endianness**
```asm
mov ax, 1234h       ; Încărcăm valoarea 0x1234 în AX
mov [test_var], ax  ; Stocăm valoarea în memoria variabilei `test_var`

mov si, offset test_var ; Mutăm adresa lui `test_var` în SI
mov al, [si]        ; Încărcăm primul byte din memorie (LSB)
mov ah, [si+1]      ; Încărcăm al doilea byte din memorie (MSB)
```
- **Pas 1**: Inițializăm o variabilă cu valoarea `0x1234`.
- **Pas 2**: Încărcăm:
  - **[si]**: Primul byte din memorie.
  - **[si+1]**: Al doilea byte din memorie.

---

#### **4. Compararea valorilor**
```asm
cmp al, 34h         ; Verificăm dacă primul byte este 0x34 (LSB)
jne big_endian      ; Dacă nu este, sistemul este big-endian
lea dx, little_msg  ; Dacă da, sistemul este little-endian
jmp display_result

big_endian:
lea dx, big_msg     ; Sistemul este big-endian
```
- Verificăm dacă **primul byte** (LSB) este **0x34**:
  - Dacă **da**, sistemul este **little-endian**.
  - Dacă **nu**, sistemul este **big-endian**.

---

#### **5. Afișarea rezultatului**
```asm
display_result:
mov ah, 09h         ; Funcția DOS pentru afișarea unui șir
int 21h             ; Afișăm mesajul corespunzător
```
- Afișăm mesajul corespunzător (LITTLE-ENDIAN sau BIG-ENDIAN) folosind funcția DOS **int 21h, ah=09h**.

---

#### **6. Terminarea programului**
```asm
mov ah, 4Ch
int 21h
```
- Terminăm execuția programului și revenim la sistemul de operare.

---

### **Cum funcționează programul?**

1. Variabila `test_var` este inițializată cu valoarea **`1234h`**.
2. Programul verifică conținutul memoriei:
   - Dacă primul byte conține **`34h`** → sistemul este **little-endian**.
   - Dacă primul byte conține **`12h`** → sistemul este **big-endian**.
3. Programul afișează un mesaj corespunzător.

---

### **Rezultatul programului**

Pe procesoarele **x86**, rezultatul va fi:

```
Sistemul este LITTLE-ENDIAN!
```

---

### **Explicații teoretice pentru începători**

1. **Memoria și endianness**:
   - **Little-endian**: Memoria stochează mai întâi byte-ul cel mai mic.
   - **Big-endian**: Memoria stochează mai întâi byte-ul cel mai mare.

2. **Accesarea memoriei**:
   - **[si]**: Adresa primului byte al unei variabile.
   - **[si+1]**: Adresa celui de-al doilea byte.

3. **Instrucțiunea `cmp`**:
   - Compara două valori.
   - Dacă sunt egale, **flag-ul ZF** (Zero Flag) este setat.

4. **Funcția DOS `int 21h`**:
   - **AH = 09h**: Afișează un șir de caractere terminat cu `$`.

---

### **Cum să rulați programul**

1. Scrieți codul într-un editor compatibil (de exemplu, EMU8086).
2. Compilați și rulați programul.
3. Observați mesajul afișat pe ecran.
