# BLACKOUT CTF – Reverse Engineering Write-ups

Two reverse engineering challenges from the Nexploit BLACKOUT CTF, solved step by step. These notes explain the commands and techniques used.

**Category:** Reverse Engineering  
**Platform:** Linux ELF and Android APK  
**Total Points:** 250

---

# 1. Forget Password! (50 Points)

**Challenge:** "I accidentally forgot the password again!"

**File provided:** `cracked_code`

**Flag format:** `NEX{.......}`

### Approach

The program asks for a passcode and compares it with a hardcoded string. By examining the binary, we can identify the expected password.

### Step 1: Identify the file

```bash
file cracked_code
```

The command identifies the file type without executing it.

**Expected result:** An ELF 64-bit Linux executable. The `not stripped` description means useful symbol names may still be available.

### Step 2: Search for readable strings

```bash
strings -n 6 cracked_code | head -60
```

- `strings` extracts readable text.
- `-n 6` displays strings with at least six characters.
- `head -60` limits the output to the first 60 lines.

Among the output, we find:

```text
Enter the passcode:
sscit{r3v_3ng_pro}
Mujhay bhul gya hai password... Ye bhi nai he password!
```

The flag-looking string is a success message, not necessarily the password.

### Step 3: Examine the relevant functions

```bash
objdump -d -M intel --no-show-raw-insn cracked_code | sed -n '/\<check_flag>:/,/^$/p;/\<main>:/,/^$/p'
```

`objdump -d` disassembles the executable into assembly instructions.

The `check_flag` function compares the supplied input with a string using `strcmp`. The `main` function reads the input and passes it to `check_flag`.

### Step 4: Inspect the read-only data section

```bash
objdump -s -j .rodata cracked_code
```

The `.rodata` section contains constant data, including strings used by the program.

At address `0x2008`, we find:

```text
reverseme
```

This is the expected passcode.

### Step 5: Verify the password

```bash
chmod +x cracked_code
echo reverseme | ./cracked_code
```

`chmod +x` gives the file executable permission. `echo` sends the password to the program.

**Result:**

```text
Enter the passcode:
sscit{r3v_3ng_pro}
```

**Recovered passcode:** `reverseme`

**Flag printed by the program:**

```text
sscit{r3v_3ng_pro}
```

The challenge description specifies `NEX{...}`, but the binary prints `sscit{...}`. The supplied team write-up reports that the scoreboard accepted the raw `sscit{r3v_3ng_pro}` value.

### What I learned

- `strings` helps locate readable information in binaries.
- A visible flag-like string can be a decoy.
- `strcmp` is useful to recognize when examining password comparisons.
- Disassembly helps reveal how a program validates input.

---

# 2. Feed Me (200 Points)

**Challenge:** A cookie-click application with a unique reward.

**File provided:** `hungry.apk`

**Flag format:** `NEX{.......}`

### Approach

The cookie counter is a distraction. The interesting logic is inside a native Android library called `libclickme.so`.

The flag is constructed by XOR operations on two byte tables. We can reproduce the calculations in Python without needing an Android emulator.

### Step 1: Identify the APK

```bash
file hungry.apk
```

The result identifies the file as an Android package (APK).

An APK is essentially a ZIP archive containing application files.

### Step 2: List the contents

```bash
unzip -l hungry.apk | grep -E 'classes.dex|\.so'
```

- `unzip -l` lists files inside the APK.
- `grep -E` filters the output.
- `.so` files are native shared libraries.

We find four versions of `libclickme.so` for different CPU architectures.

### Step 3: Extract the required files

```bash
mkdir -p /tmp/apk
cd /tmp/apk
unzip -oq /path/to/hungry.apk 'lib/*' classes.dex AndroidManifest.xml
```

Replace `/path/to/hungry.apk` with the actual location of your APK.

We use the `x86_64` library because it can be examined directly on a compatible Linux PC.

### Step 4: Find the native function

```bash
nm -D lib/x86_64/libclickme.so | grep -v ' U ' | grep -i getFlag
```

`nm -D` lists exported symbols in the library.

The relevant function is:

```text
Java_com_example_clickme_MainActivity_getFlag
```

This is a JNI function called by the Android application's Java code.

### Step 5: Search for visible flag strings

```bash
strings -n 5 lib/x86_64/libclickme.so | grep -iE 'NEX|flag'
```

The search does not reveal the complete flag. This suggests that the value is constructed dynamically.

### Step 6: Disassemble the function

```bash
objdump -d -M intel --no-show-raw-insn \
--start-address=0xe260 --stop-address=0xe6a0 \
lib/x86_64/libclickme.so | head -250
```

The disassembly reveals two loops:

- The first loop processes five bytes, XORs corresponding values from two tables, and adds one.
- The second loop processes sixteen bytes, XORs corresponding values, and formats the results as hexadecimal.
- The function then appends the closing brace `}`.

### Step 7: Dump the byte tables

```bash
objdump -s --start-address=0x2af20 --stop-address=0x2af60 \
lib/x86_64/libclickme.so
```

The output contains the two byte tables used by the function.

The supplied analysis identifies the first 22 bytes as table B and the next 22 bytes as table A.

### Step 8: Reproduce the calculations in Python

```python
d = bytes.fromhex(
    "07871704d1f28f49dbbc19660d4f0fed23f83d4d580f62ec"
    "7762ab7612d78f9ddcf555a142bba92681172872"
)

B, A = d[:22], d[22:44]

p = ''.join(chr(((A[i] ^ B[i]) + 1) & 0xff) for i in range(5))
h = ''.join('%02x' % (A[i+5] ^ B[i+5]) for i in range(16))

print(p + h + '}')
```

The Python code reproduces the operations observed in the native function.

**Raw output reported by the challenge write-up:**

```text
flag{849d9e5421c59358ee4d568adebc5a70}
```

**Suggested submission format:**

```text
NEX{849d9e5421c59358ee4d568adebc5a70}
```

The supplied write-up reports that the platform expects the `NEX{...}` wrapper. If that format is rejected, verify the expected format against the challenge instructions.

### What I learned

- APK files can be inspected as ZIP archives.
- Android applications can contain native libraries under `lib/`.
- JNI function names can help locate native application logic.
- `strings` may not reveal values constructed at runtime.
- Disassembly helps explain how data is transformed.
- Python can reproduce simple byte-level operations such as XOR.

---

## Summary

| Challenge | Points | Technique |
|---|---:|---|
| Forget Password! | 50 | ELF analysis, strings, objdump |
| Feed Me | 200 | APK analysis, JNI, native disassembly, XOR |
| **Total** | **250** | **Reverse Engineering** |

**Main lesson:** Do not rely only on what an application displays. Inspect its underlying logic to understand how it works.
