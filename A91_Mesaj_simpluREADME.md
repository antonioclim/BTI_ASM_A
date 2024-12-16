### **Program ASM (x86 pe 16 biți) pentru alocarea, inițializarea și afișarea unui șir de caractere**

Acest program **stochează** un mesaj într-un segment de date și **afișează** mesajul pe ecran folosind funcțiile sistemului DOS (folosind întreruperea `int 21h`).


### **Explicație detaliată a codului**

#### **1. Linia `org 100h`**
- **`org 100h`** specifică faptul că programul este un fișier **.COM**.
- Într-un fișier **.COM**, programul începe de la adresa **100h** în memoria RAM.

---

#### **2. Segmentul de date**
```asm
data segment
mesaj db 'Salutare! Acesta este un mesaj in ASM.$'
data ends
```
- **`data segment`**: Secțiunea în care definim variabilele (datele) programului.
- **`mesaj db ...`**:
  - Definește un șir de caractere (tip **BYTE**) care conține mesajul.
  - Mesajul trebuie **terminat cu caracterul `$`**, care este necesar pentru funcția **DOS `int 21h, ah=09h`**.

---

#### **3. Inițializarea segmentului de date**
```asm
mov ax, @data  ; Încărcăm adresa segmentului de date în registrul AX
mov ds, ax     ; Configurăm registrul DS pentru a accesa datele din `data segment`
```
- Segmentul de date **DS** trebuie configurat corect înainte de a accesa variabilele definite în secțiunea `data segment`.

---

#### **4. Afișarea șirului de caractere**
```asm
lea dx, mesaj  ; Încărcăm adresa șirului de caractere în DX
mov ah, 09h    ; Funcția DOS pentru afișarea unui șir de caractere
int 21h        ; Apelează funcția DOS pentru afișare
```
- **`lea dx, mesaj`**: Încarcă adresa șirului de caractere în registrul **DX**.
- **`mov ah, 09h`**: Selectează funcția DOS pentru afișarea unui șir de caractere.
- **`int 21h`**: Apelează întreruperea DOS pentru a executa funcția.

---

#### **5. Terminarea programului**
```asm
mov ah, 4Ch    ; Funcția DOS pentru terminarea programului
int 21h        ; Termină programul și revine la sistemul de operare
```
- **`int 21h, ah=4Ch`**: Termină programul și returnează controlul către sistemul de operare.

---

### **Cum funcționează programul**

1. **Inițializare**:
   - Segmentul de date este configurat pentru a permite accesul la variabila `mesaj`.

2. **Afișarea mesajului**:
   - Mesajul stocat în variabila `mesaj` este afișat folosind funcția DOS `int 21h, ah=09h`.

3. **Terminarea programului**:
   - Programul se încheie folosind funcția DOS `int 21h, ah=4Ch`.

---

### **Rezultatul programului**

După rularea programului, pe ecran va apărea următorul mesaj:

```
Salutare! Acesta este un mesaj in ASM.
```

---

### **Notiuni teoretice esențiale pentru începători**

1. **Șiruri de caractere în ASM**:
   - Un șir de caractere este o succesiune de **BYTE-uri**.
   - Funcția DOS **`int 21h, ah=09h`** afișează caracterele dintr-un șir până când întâlnește simbolul **`$`**.

2. **Registrul DS**:
   - **DS** (Data Segment) este folosit pentru a accesa variabilele definite în segmentul de date.

3. **Registrul DX**:
   - **DX** conține adresa de memorie a șirului de caractere care trebuie afișat.

4. **Funcția `int 21h`**:
   - **`int 21h`** este o întrerupere folosită pentru a apela funcții din sistemul DOS.
   - **`AH = 09h`**: Afișează un șir de caractere terminat cu `$`.
   - **`AH = 4Ch`**: Termină execuția programului.

---

### **Cum să rulați programul**

1. Scrieți codul într-un editor compatibil (de exemplu, editorul din **EMU8086**).
2. Compilați și rulați programul.
3. Verificați mesajul afișat pe ecran.
