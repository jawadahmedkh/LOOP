# Running Assembly

## In Linux

1. Create run-asm.sh and write following lines:

```sh
#!/bin/bash

if [ -z "$1" ]; then
    echo "❌ Error: No file name provided."
    echo "Usage: ./run-asm.sh file.asm"
    exit 1
fi

FILE="$1"
BASENAME=$(basename "$FILE" .asm)


if grep -q ";mode:32" "$FILE"; then
    echo "🔧 32-bit Assembly Mode"
    nasm -f elf32 "$FILE" -o "$BASENAME.o" &&
    ld -m elf_i386 -s -o "$BASENAME" "$BASENAME.o" &&
    echo "Running:" &&
    ./"$BASENAME"
else
    echo "🔧 64-bit Assembly Mode"
    nasm -f elf64 "$FILE" -o "$BASENAME.o" &&
    ld -s -o "$BASENAME" "$BASENAME.o" &&
    echo "Running:" &&
    ./"$BASENAME"
fi

```

2. Adding Following lines while opening settings of vs code and searching `code-runner.executorMap`

```json
"files.associations": {
            "*.asm": "asm"
        },
```

and also add following lines in `"code-runner.executorMapByFileExtension"` section

```json
".asm": "cd $dir && bash run-asm.sh \"$fileName\""
```

That's It

---

## In Windows

### 1. Required Tools

* **[NASM for Windows](https://www.nasm.us/pub/nasm/releasebuilds/)**
  Download and extract (e.g., `C:\nasm`). Add the `nasm.exe` path to **System Environment Variables** → `Path`.

* **[MinGW](https://sourceforge.net/projects/mingw/)** (for `ld.exe` or `gcc`)
  Install `mingw32-base` and `mingw32-gcc-g++`. Add `C:\MinGW\bin` to your `Path`.

---

### 2. Create Your Folder Structure

```plaintext
AssemblyProject/
│
├── run-asm.bat       ← Your build script for Windows
├── test.asm          ← Your assembly file
└── .vscode/
    └── settings.json ← VS Code config
```

---

### 3. 📜 `run-asm.bat`

```bat
@echo off
setlocal

REM Get full path to file
set "FILE=%~1"
for %%F in ("%FILE%") do set "NAME=%%~nF"

REM Detect 32-bit mode using marker
findstr /C:";mode:32" "%FILE%" >nul
if %errorlevel%==0 (
    echo 🔧 32-bit Assembly Mode
    nasm -f win32 "%FILE%" -o "%NAME%.obj"
    gcc -m32 -o "%NAME%.exe" "%NAME%.obj"
) else (
    echo 🔧 64-bit Assembly Mode
    nasm -f win64 "%FILE%" -o "%NAME%.obj"
    gcc -m64 -o "%NAME%.exe" "%NAME%.obj"
)

echo ✅ Running: %NAME%.exe
"%cd%\%NAME%.exe"

endlocal
```

---

### 4. VS Code `settings.json`

Adding Following lines while opening settings of vs code and searching `code-runner.executorMap`

Add this line into `"code-runner.executorMapByFileExtension"`

```json
    ".asm": "cmd /c run-asm.bat \"$fileName\""
```

and also add this line sloely

```json
  "files.associations": {
    "*.asm": "asm"
  }
```

---

### 5. Sample `test.asm`

#### 32-bit

```nasm
;mode:32
section .data
    msg db "✅ Hello from 32-bit!", 0xA
    len equ $ - msg

section .text
    global _start

_start:
    mov eax, 4
    mov ebx, 1
    mov ecx, msg
    mov edx, len
    int 0x80

    mov eax, 1
    xor ebx, ebx
    int 0x80
```

#### 64-bit

```nasm
section .data
    msg db "✅ Hello from 64-bit!", 0xA
    len equ $ - msg

section .text
    global main
    extern printf

main:
    push rbp
    mov rdi, msg
    call printf
    pop rbp
    ret
```

---

