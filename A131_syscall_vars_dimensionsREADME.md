### **Program ASM (x86 pe 16 biți) pentru definirea variabilelor de diferite dimensiuni și afișarea lor**

În acest program, vom defini variabile de **diferite dimensiuni** (BYTE, WORD și DWORD) în segmentul de date și le vom afișa utilizând apeluri de sistem (**syscall** folosind **int 21h**).

### **Explicație detaliată a codului**

#### **1. Inițializarea segmentului de date**
```asm
mov ax, @data
mov ds, ax
```
- Configurăm **DS** (Data Segment) pentru a putea accesa variabilele definite în secțiunea **data segment**.

---

#### **2. Definirea variabilelor**
```asm
byte_var db 5           ; Variabilă BYTE (8 biți)
word_var dw 0ABCCh      ; Variabilă WORD (16 biți)
dword_var dd 12345678h  ; Variabilă DWORD (32 biți)
```
- **`db`**: Definește o variabilă BYTE (8 biți).
- **`dw`**: Definește o variabilă WORD (16 biți).
- **`dd`**: Definește o variabilă DWORD (32 biți).

---

#### **3. Afișarea variabilelor**

- **Afișarea variabilei BYTE**:
   ```asm
   mov dl, byte_var
   call print_byte
   ```
   - Valoarea BYTE este mutată în **DL** și afișată prin apelarea subrutinei `print_byte`.

- **Afișarea variabilei WORD**:
   ```asm
   mov ax, word_var
   call print_word
   ```
   - Valoarea WORD este mutată în **AX** și afișată ca 4 cifre hex folosind subrutina `print_word`.

- **Afișarea variabilei DWORD**:
   ```asm
   mov ax, word ptr dword_var
   call print_word
   mov ax, word ptr dword_var+2
   call print_word
   ```
   - **`word ptr dword_var`**: Extrage partea inferioară a DWORD-ului (primii 16 biți).
   - **`word ptr dword_var+2`**: Extrage partea superioară a DWORD-ului (ultimii 16 biți).

---

#### **4. Subrutine**

1. **Subrutina `print_byte`**:
   - Afișează un BYTE (conținutul din **DL**) prin conversie la ASCII.

2. **Subrutina `print_word`**:
   - Afișează un WORD (4 cifre hexazecimale):
     - Rotește nibble-urile (4 biți) spre stânga.
     - Afișează fiecare nibble convertit la ASCII.

3. **Subrutina `print_newline`**:
   - Afișează un **newline** pentru formatarea rezultatului.

---

#### **5. Terminarea programului**
```asm
mov ah, 4Ch
int 21h
```
- Încheie programul și revine la sistemul de operare.

---

### **Cum funcționează programul**

1. Variabilele **`byte_var`**, **`word_var`** și **`dword_var`** sunt definite în secțiunea de date.
2. Programul le afișează pe rând:
   - **BYTE** este afișat ca o cifră.
   - **WORD** este afișat ca 4 cifre hex.
   - **DWORD** este afișat în două părți (fiecare parte este un WORD).
3. Rezultatele sunt afișate pe ecran cu ajutorul apelurilor sistemului DOS.

---

### **Rezultatul programului**

După rularea programului, rezultatul afișat va fi:

```
5
ABCC
5678
1234
```

---

### **Noțiuni esențiale pentru începători**

1. **Definirea variabilelor**:
   - **`db`**: Definește o variabilă de 8 biți.
   - **`dw`**: Definește o variabilă de 16 biți.
   - **`dd`**: Definește o variabilă de 32 biți.

2. **Registrul AX**:
   - Este folosit pentru afișarea unui WORD (16 biți).

3. **Funcția DOS `int 21h`**:
   - **AH = 02h**: Afișează un caracter.
   - **AH = 4Ch**: Termină execuția programului.

4. **Adresare segmentară**:
   - **`word ptr`**: Specifică că accesăm 16 biți dintr-o variabilă DWORD.

---

### **Cum să rulați programul**

1. Scrieți codul într-un editor compatibil (**EMU8086**).
2. Compilați și rulați programul.
3. Observați rezultatele afișate pe ecran.
