![unshell_hero](./unshell-banner.png)
# Unshell
> The Script Kiddies Nightmare 👻

Effortlessly deobfuscate shell scripts back into source code, even with heavy multi-layered obfuscation. Unshell will detects patterns in shell scripts, determines obfuscation techniques, and effectively deobfuscates them. 

## Features
- Zero configuration: There's no need for any configuration
- Penetrate: Multi layered obfuscation is not a problem
- Easy to use: just `unshell -f encrypted1.sh encrypted2.sh` in cmd (Terminals like Termux)

## Supported Obfuscation Methods
<details>
<summary>Shell Script Compiler (SHC)</summary>
SHC works internally called execve to shell, it decrypted at runtimes and visible via command line args process

eg: <code>/bin/sh -c "decrypted shell"</code>
</details>

<details>
<summary>Simple Script Compiler (SSC)</summary>
Similar to SHC but uses C++. The shell script is read from file descriptor `3`, which can be accessed via `fd`.
</details>

<details>
<summary>Ri-crypt</summary>
Ri-crypt works internally called execve to shell, it decrypted at runtimes and visible via command line args process. We can retrieve the shell script using `strace`.
</details>

<details>
<summary>bash-obfuscate (Node.js CLI)</summary>
bash-obfuscate works by randomizing the script with random variable names and executing them via `eval`.  
</details>

<details>
<summary>BashRock</summary>
Bashrock works almost the same way as bash-obfuscate.
</details>

<details>
<summary>TTP Tool</summary>
It uses eval "$dec_1", variable (enc), random functions, random strings, random values, and multi-layered base64.
The creator of this obfuscation said "it has anti-decode feature" despite of "multi-layered Base64 encoding" and "hard-to-detect random lines" that he uses can EASILY be decoded.
As of this writing, unshell supports up to version 23 of this "tool". But, if it is the version 23, he adds a compare to check if there is any temp (or check sha256). If yes, it'll remove decode of the script, and say `echo "⚠️ Error: The file structure was changed 🤣."
sleep 999999`.
So you must decrypt it manually 🙃.
</details>

<details>
<summary>BashProtector</summary>
Bashrock randomizes the script with random variables layered by single `base64` encryption, then executes it in a single `eval` command.
</details>

<details>
<summary>Extreme comment/editor EOF trick</summary>
Some people obfuscate their script by adding generous amounts of comments until it becomes a really big file, tricking average text editors to crash while opening the script so people can't open it.
</details>

<details>
<summary>BZip2</summary>
Usually used for obfuscating tunneling/VPN scripts. The actual script is compressed with bzip2 and snuck inside the decompression script itself.
</details>

<details>
<summary>Axeron online modules</summary>
The script is actually stored somewhere online (usually public GitHub pages, a script kiddies' behavior), and the module script does only execution of the actual script after being downloaded from the cloud. The file link itself is obfuscated with base64 and rot17.
</details>

<details>
<summary>Base64</summary>
Not too crazy, just classic
`sh
echo "ZWNobyBzb21lIGJhc2U2NCBlbmNyeXB0ZWQgc2hpdAo=" | base64 -d | sh
`
</details>

<details>
<summary>Binary Encoding</summary>
Binary encoding involves converting the script into binary (0s and 1s) format. This is usually done by encoding each character into its binary equivalent, making the script unreadable to the human eye. It can be executed with a simple shell script after conversion.
  Example:
`
echo "01001000 01100101 01101100 01101100 01101111" | xxd -r -p | sh
`
</details>

<details>
<summary>URL Encoding</summary>
URL encoding replaces special characters with percent-encoded characters. It is often used to obfuscate URLs but can also be used to obscure shell scripts. The encoded string must be decoded back to its original form for execution.
  Example:
`
echo "echo%20Hello%20World%21" | sed 's/%/\\x/g' | xargs -0 bash -c
`
</details>

<details>
<summary>Reverse Encoding</summary>
Reverse encoding simply reverses the string of the script. It can make the script hard to read but is easily reversible by reversing the string back.
  Example:
`
echo "dlrow olleh" | rev | sh
`
</details>

<details>
<summary>Hexadecimal Encoding</summary>
Hexadecimal encoding is a method of representing the shell script characters in hexadecimal format. This can make the script unreadable until it's converted back to its original form.
  Example:
`
echo "68656c6c6f" | xxd -r -p | sh
`
</details>

<details>
<summary>ROT47 Encoding</summary>
ROT47 is similar to ROT13, but it shifts the ASCII characters by 47 positions. It is used to obscure the text by shifting readable characters.
  Example:
`
echo "U29tZSB0ZXh0" | tr 'A-Za-z0-9' 'P-ZA-Op-z0-9' | base64 -d
`
</details>

<details>
<summary>ROT18 Encoding</summary>
ROT18 is a variant of ROT13 that applies both ROT13 and ROT5 encoding. It shifts alphabetic characters by 13 positions and numeric characters by 5 positions.
  Example:
`
echo "12345" | tr '0-9' '5-9A-Ea-e' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
`
</details>

<details>
<summary>ROT57 Encoding</summary>
ROT57 is a more complex variant of ROT47 that shifts letters by 57 positions in the ASCII table, making the script harder to decode.
  Example:
`
echo "TestMessage" | tr 'A-Za-z0-9' 'C-ZA-B1-9A-B' | base64 -d
`
</details>

<details>
<summary>ROT13 Encoding</summary>
ROT13 shifts the alphabet by 13 positions. Itâ€™s often used in forums and newsgroups to obscure spoilers, puzzles, or simple ciphers.
  Example:
`
echo "Uryyb Jbeyq" | tr 'A-Za-z' 'N-ZA-Mn-za-m' | sh
`
</details>

<details>
<summary>Base64Eval</summary>
Combines Base64 encoding with the eval command to obfuscate and execute shell commands dynamically.

The script is encoded using Base64 and then decoded and executed via eval, hiding the original command in the process.
  Example:
`
echo "ZWNobyAnSGVsbG8gd29ybGQhJwo=" | base64 --decode | eval
`
This decodes the Base64 string and executes the command ` echo 'Hello world!' `.
</details>

<details>
<summary>Eval</summary>
eval is used to evaluate and execute a string as a shell command. It can be used for code obfuscation, often alongside Base64 encoding or other encoding techniques, to dynamically execute potentially hidden or obfuscated commands.

The main risk is that eval can execute arbitrary code, making it a common tool for obfuscating code.
  Example:
`
encoded="ZWNobyAnSGVsbG8gd29ybGQhJwo="
decoded=$(echo $encoded | base64 --decode)
eval $decoded
`
In this case, the Base64 string is decoded and then executed by eval, revealing and running the hidden command (echo 'Hello World').
</details>

## Installation
```shell
spath=$(echo $PATH | cut -d: -f1)
curl -sLo $spath/unshell https://github.com/JeremiManaluYT/unshell/raw/main/unshell
chmod +x $spath/unshell
```

## Usage
```yaml
unshell - Deobfuscate any shell scripts with multiple methods for FREE!
  Usage: unshell [OPTIONS] [FILE]
  Usage: unshell [OPTIONS] [DIR]

  Options:
    -h, --help
      Show this message
    -f, --file [FILE]
      Specify script(s) to deobfuscate (supports multiple inputs)
    -r, --recursive [DIR]
      Recursively find and deobfuscate all shells in the specified directory
    -v, --verbose
      Enable verbose mode (for debugging or troubleshooting)
    -d, --execve-delay [SECOND]
      Set a custom execve delay in seconds (for SHC/SSC encryption)
    -U, --update
      Update the script

  Example usages:
    unshell -f install.sh menu.sh
    unshell -v -f /system/bin/gaming_script.sh
    unshell -d 6.018 -f ./VTK
    unshell -r dir
```

## WARNING
Using Unshell to deobfuscate SHC, SSC, or Ri-crypt scripts **may be dangerous** as these require executing the script to retrieve the original content. If the script contains malicious code, it could harm your system. **Do not run Unshell with ROOT privileges unless you fully trust the script!**

## Special Credits
- [Rem01Gaming](https://github.com/Rem01Gaming/unshell) for the original repository.
- [kawaii-ghost](https://github.com/kawaii-ghost/deshc) for decsh (shc and ssc deobfucator).
- [RiProG-id](https://github.com/RiProG-id/Universal-Shell-Dec.git) for universal-shell-dec, the inspiration and foundation of (https://github.com/Rem01Gaming/unshell) and this project.
