# Wireless USB keyboard (HID) using Raspberry Pi Zero W

## References

### (DEMO) Turn Your Raspberry Pi Zero into a USB Keyboard (HID)

<https://www.youtube.com/watch?v=-tC7nL7rVRE>

Companion Webpage:

<https://randomnerdtutorials.com/raspberry-pi-zero-usb-keyboard-hid/>

**[PDF](./zero-hid/Turn-Raspberry-Pi-Zero-in-USB-Keyboard_Random-Nerd-Tutorials.pdf)** Copy of the same page.

### ZeroLife Channel = Raspberry Pi Zero W: Into a Wireless USB Keyboard (HID)

#### Part 1 - Setup

<https://www.youtube.com/watch?v=fooKk-OBGyY>

- Reference Links to Original Article

    <https://gndtovcc.home.blog/2020/04/17/turn-your-raspberry-pi-zero-into-a-usb-keyboard-hid/>

    - **[PDF](./zero-hid/Turn-Your-Raspberry-Pi-Zero-into-a-USB-Keyboard-HID_gnd-to-vcc.pdf)** Copy
- Forum Link for the USB HID Codes

    <https://forum.flirc.tv/index.php?%2Ftopic%2F2209-usb-hid-codes-keys-and-modifier-keys-for-flirc_util-record_api-x-y%2F>

    - **[PDF](./zero-hid/USB-HID-codes_Flirc-Forums.pdf)** Copy
- FreeBSD USB HID usage Table <https://www.freebsddiary.org/APC/usb_hid_usages.php>
    - **[PDF](./zero-hid/FreeBSD-USB-HID-usage-table.pdf)** Copy

#### Part 2 Python Typing Script

<https://www.youtube.com/watch?v=GSIKft286dE>

This code includes the fixes from [Part 3](#part-3-–-python-translate) :

```py
#!/usr/bin/env python3
NULL_CHAR = chr(0)

def write_report(report):
    try:
        with open('/dev/hidg0', 'rb+') as fd:
            fd.write(report.encode())
    except: pass

def transcribe(key):
    lower_case = {
        'a':4,'b':5,'c':6,'d':7,'e':8,'f':9,'g':10,'h':11,'i':12,'j':13,'k':14,
        'l':15,'m':16,'n':17,'o':18,'p':19,'q':20,'r':21,'s':22,'t':23,'u':24,
        'v':25,'w':26,'x':27,'y':28,'z':29,'-':45,'=':46,'[':47,']':48,'\\':49,
        ';':51,"'":52,',':54,'.':55,'/':56,'1':30,'2':31,'3':32,'4':33,'5':34,
        '6':35,'7':36,'8':37,'9':38,'0':39,'`':53,r'\n':40,'\t':43,' ':44
    }
    #Note: for upper case, you have add the angle brackets

    upper_case = {
        'A':4,'B':5,'C':6,'D':7,'E':8,'F':9,'G':10,'H':11,'I':12,'J':13,'K':14,
        'L':15,'M':16,'N':17,'O':18,'P':19,'Q':20,'R':21,'S':22,'T':23,'U':24,
        'V':25,'W':26,'X':27,'Y':28,'Z':29,'_':45,'+':46,'{':47,'}':48,'|':49,
        ':':51,'"':52,'<':54,'>':55,'?':56,'!':30,'@':31,'#':32,'$':33,'%':34,
        '^':35,'&':36,'*':37,'(':38,')':39,'~':53
    }

    if key in lower_case:
        write_report(NULL_CHAR*2+chr(lower_case[key])+NULL_CHAR*5)
    elif key in upper_case:
        # Shift key 'chr(32)+NULL_CHAR'
        write_report(chr(32)+NULL_CHAR+chr(upper_case[key])+NULL_CHAR*5)
    write_report(NULL_CHAR*8)
```

#### Part 3 – Python Translate

<https://www.youtube.com/watch?v=coXudrCHxhI>

```py
#!/usr/bin/env python3
NULL_CHAR = chr(0)

def write_report(report):
    try:
        with open('/dev/hidg0', 'rb+') as fd:
            fd.write(report.encode())
    except: pass

def transcribe(key):
    lower_case = {
        'a':4,'b':5,'c':6,'d':7,'e':8,'f':9,'g':10,'h':11,'i':12,'j':13,'k':14,
        'l':15,'m':16,'n':17,'o':18,'p':19,'q':20,'r':21,'s':22,'t':23,'u':24,
        'v':25,'w':26,'x':27,'y':28,'z':29,'-':45,'=':46,'[':47,']':48,'\\':49,
        ';':51,"'":52,',':54,'.':55,'/':56,'1':30,'2':31,'3':32,'4':33,'5':34,
        '6':35,'7':36,'8':37,'9':38,'0':39,'`':53,r'\n':40,'\t':43,' ':44
    }
    #Note: for upper case, you have add the angle brackets

    upper_case = {
        'A':4,'B':5,'C':6,'D':7,'E':8,'F':9,'G':10,'H':11,'I':12,'J':13,'K':14,
        'L':15,'M':16,'N':17,'O':18,'P':19,'Q':20,'R':21,'S':22,'T':23,'U':24,
        'V':25,'W':26,'X':27,'Y':28,'Z':29,'_':45,'+':46,'{':47,'}':48,'|':49,
        ':':51,'"':52,'<':54,'>':55,'?':56,'!':30,'@':31,'#':32,'$':33,'%':34,
        '^':35,'&':36,'*':37,'(':38,')':39,'~':53
    }

    if key in lower_case:
        write_report(NULL_CHAR*2+chr(lower_case[key])+NULL_CHAR*5)
    elif key in upper_case:
        # Shift key 'chr(32)+NULL_CHAR'
        write_report(chr(32)+NULL_CHAR+chr(upper_case[key])+NULL_CHAR*5)
    write_report(NULL_CHAR*8)

def translate(doc):
    for index in range(len(doc)):
        hit_key = repr(doc[index])

        if eval(hit_key) == "'" or eval(hit_key) == '\\':
            print(eval(hit_key))
            transcribe(eval(hit_key))
        else:
            print(hit_key.strip("'"))
            transcribe(hit_key.strip("'"))

if _name_ == '__main__':
    translate('Hello')
```

#### Part 4 – Python Interpret

<https://www.youtube.com/watch?v=yW8K7-MZuxo>

We would need to create the `password.txt` in the same place as the Python script.

```py
#!/usr/bin/env python3
import os, time

NULL_CHAR = chr(0)
current_dir = os.path.dirname(os.path.realpath(__file__))
path = os.path.join(current_dir,'password.txt')

def write_report(report):
    try:
        with open('/dev/hidg0', 'rb+') as fd:
            fd.write(report.encode())
    except: pass

def transcribe(key):
    lower_case = {
        'a':4,'b':5,'c':6,'d':7,'e':8,'f':9,'g':10,'h':11,'i':12,'j':13,'k':14,
        'l':15,'m':16,'n':17,'o':18,'p':19,'q':20,'r':21,'s':22,'t':23,'u':24,
        'v':25,'w':26,'x':27,'y':28,'z':29,'-':45,'=':46,'[':47,']':48,'\\':49,
        ';':51,"'":52,',':54,'.':55,'/':56,'1':30,'2':31,'3':32,'4':33,'5':34,
        '6':35,'7':36,'8':37,'9':38,'0':39,'`':53,r'\n':40,'\t':43,' ':44
    }
    #Note: for upper case, you have add the angle brackets

    upper_case = {
        'A':4,'B':5,'C':6,'D':7,'E':8,'F':9,'G':10,'H':11,'I':12,'J':13,'K':14,
        'L':15,'M':16,'N':17,'O':18,'P':19,'Q':20,'R':21,'S':22,'T':23,'U':24,
        'V':25,'W':26,'X':27,'Y':28,'Z':29,'_':45,'+':46,'{':47,'}':48,'|':49,
        ':':51,'"':52,'<':54,'>':55,'?':56,'!':30,'@':31,'#':32,'$':33,'%':34,
        '^':35,'&':36,'*':37,'(':38,')':39,'~':53
    }

    if key in lower_case:
        write_report(NULL_CHAR*2+chr(lower_case[key])+NULL_CHAR*5)
    elif key in upper_case:
        # Shift key 'chr(32)+NULL_CHAR'
        write_report(chr(32)+NULL_CHAR+chr(upper_case[key])+NULL_CHAR*5)
    write_report(NULL_CHAR*8)

def translate(doc):
    for index in range(len(doc)):
        hit_key = repr(doc[index])

        if eval(hit_key) == "'" or eval(hit_key) == '\\':
            print(eval(hit_key))
            transcribe(eval(hit_key))
        else:
            print(hit_key.strip("'"))
            transcribe(hit_key.strip("'"))

def interpret(path):

    action_key = {
        '\ENTER':40,'\ESCAPE':41,'\DELETE':42,'\TAB':43,'\SPACE':44,
        '\PRINT':70,'\SCROLL':71,'\PAUSE':72,'\INSERT':73,'\HOME':74,
        '\PAGEUP':75,'\END':77,'\PAGEDOWN':78,'\RIGHTARROW':79,
        '\LEFTARROW':80,'\DOWNARROW':81,r'\UPARROW':82,'\POWER':102,
        '\LEFTCTRL':224,'\LEFTSHIFT':225,'\LEFTALT':226,'\LEFTGUI':227,
        '\RIGHTCTRL':228,'\RIGHTSHIFT':229,'\RIGHTALT':230,'\RIGHTGUI':231
    }

    function_key = {
        '\F1':58,'\F2':59,'\F3':60,'\F4':61,'\F5':62,'\F6':63,'\F7':64,
        '\F8':65,'\F9':66,'\F10':67,'\F11':68,'\F12':69,'\F13':104,
        '\F14':105,'\F15':106,'\F16':107,'\F17':108,'\F18':109,'\F19':110,
        '\F20':111,'\F21':112,'\F22':113,'\F23':114,'\F24':115
    }

    menu_key = {
        '\EXECUTE':116,'\HELP':117,'\MENU':118,'\SELECT':119,'\STOP':120,
        '\AGAIN':121,r'\UNDO':122,'\CUT':123,'\COPY':124,'\PASTE':125,
        '\FIND':126,'\MUTE':127,'\VOLUP':128,'\VOLDOWN':129,'\CAPLOCK':130,
        r'\NUMLOCK':131,'\SCROLLLOCk':132
    }

    #mod_key = {
    #    '\LEFTCTRL':1,'\LEFTSHIFT':2,'\LEFTALT':4,'\LEFTWIN':8,
    #    '\RIGHTCTRL':16,'\RIGHTSHIFT':32,'\RIGHTALT':64,'\RIGHTWIN':128
    #}

    merged_key = {}
        merged_key.update(action_key)
        merged_key.update(function_key)
        merged_key.update(menu_key)

    with open(path,'r') as script:
            wordlist = script.read()

    for count1, line in enumerate(wordlist.split('\n')):
        for count2, word in enumerate(line.split(' ')):
            if word in merged_key:
                write_report(NULL_CHAR*2+chr(merged_key[word])+NULL_CHAR*5)
            elif word[:6] == '\SLEEP':
                time.sleep(int(word[6:]))
            elif word not in merged_key and count2 == 0:
            # elif word not in merged_key and count2 < 1:
                translate(word)
            else:
                translate(' ')
                translate(word)
        if count1 == len(wordlist.split('\n')) - 2:
        # if count1 < len(wordlist.split('\n')) - 1:
            translate('\n')

    write_report(NULL_CHAR*8)

if _name_ == '__main__':
    interpret(path)
```

#### Part 5 – Python Keyboard Input

<https://www.youtube.com/watch?v=iYMN1hIVmX0>

```py
#!/usr/bin/env python3
import time

NULL_CHAR = chr(0)

def write_report(report):
    try:
        with open('/dev/hidg0', 'rb+') as fd:
            fd.write(report.encode())
    except: pass

def transcribe(key):
    lower_case = {
        'a':4,'b':5,'c':6,'d':7,'e':8,'f':9,'g':10,'h':11,'i':12,'j':13,'k':14,
        'l':15,'m':16,'n':17,'o':18,'p':19,'q':20,'r':21,'s':22,'t':23,'u':24,
        'v':25,'w':26,'x':27,'y':28,'z':29,'-':45,'=':46,'[':47,']':48,'\\':49,
        ';':51,"'":52,',':54,'.':55,'/':56,'1':30,'2':31,'3':32,'4':33,'5':34,
        '6':35,'7':36,'8':37,'9':38,'0':39,'`':53,r'\n':40,'\t':43,' ':44
    }
    #Note: for upper case, you have add the angle brackets

    upper_case = {
        'A':4,'B':5,'C':6,'D':7,'E':8,'F':9,'G':10,'H':11,'I':12,'J':13,'K':14,
        'L':15,'M':16,'N':17,'O':18,'P':19,'Q':20,'R':21,'S':22,'T':23,'U':24,
        'V':25,'W':26,'X':27,'Y':28,'Z':29,'_':45,'+':46,'{':47,'}':48,'|':49,
        ':':51,'"':52,'<':54,'>':55,'?':56,'!':30,'@':31,'#':32,'$':33,'%':34,
        '^':35,'&':36,'*':37,'(':38,')':39,'~':53
    }

    if key in lower_case:
        write_report(NULL_CHAR*2+chr(lower_case[key])+NULL_CHAR*5)
    elif key in upper_case:
        write_report(chr(32)+NULL_CHAR+chr(upper_case[key])+NULL_CHAR*5)
    write_report(NULL_CHAR*8)

def translate(doc):
    for index in range(len(doc)):
        hit_key = repr(doc[index])

        if eval(hit_key) == "'" or eval(hit_key) == '\\':
            print(eval(hit_key))
            transcribe(eval(hit_key))
        else:
            print(hit_key.strip("'"))
            transcribe(hit_key.strip("'"))

def interpret():
    typing = True

    action_key = {
        '\ENTER':40,'\ESCAPE':41,'\DELETE':42,'\TAB':43,'\SPACE':44,
        '\PRINT':70,'\SCROLL':71,'\PAUSE':72,'\INSERT':73,'\HOME':74,
        '\PAGEUP':75,'\END':77,'\PAGEDOWN':78,'\RIGHTARROW':79,
        '\LEFTARROW':80,'\DOWNARROW':81,r'\UPARROW':82,'\POWER':102,
        '\LEFTCTRL':224,'\LEFTSHIFT':225,'\LEFTALT':226,'\LEFTGUI':227,
        '\RIGHTCTRL':228,'\RIGHTSHIFT':229,'\RIGHTALT':230,'\RIGHTGUI':231
    }
    function_key = {
        '\F1':58,'\F2':59,'\F3':60,'\F4':61,'\F5':62,'\F6':63,'\F7':64,
        '\F8':65,'\F9':66,'\F10':67,'\F11':68,'\F12':69,'\F13':104,
        '\F14':105,'\F15':106,'\F16':107,'\F17':108,'\F18':109,'\F19':110,
        '\F20':111,'\F21':112,'\F22':113,'\F23':114,'\F24':115
    }
    menu_key = {
        '\EXECUTE':116,'\HELP':117,'\MENU':118,'\SELECT':119,'\STOP':120,
        '\AGAIN':121,r'\UNDO':122,'\CUT':123,'\COPY':124,'\PASTE':125,
        '\FIND':126,'\MUTE':127,'\VOLUP':128,'\VOLDOWN':129,'\CAPLOCK':130,
        r'\NUMLOCK':131,'\SCROLLLOCk':132
    }

    #mod_key = {
    #    '\LEFTCTRL':1,'\LEFTSHIFT':2,'\LEFTALT':4,'\LEFTWIN':8,
    #    '\RIGHTCTRL':16,'\RIGHTSHIFT':32,'\RIGHTALT':64,'\RIGHTWIN':128
    #}

    merged_key = {}
    merged_key.update(action_key)
    merged_key.update(function_key)
    merged_key.update(menu_key)

    while typing:
        word = input('~~#:')
        if word =='exit()':
            typing = False # End the Loop
        elif word in merged_key:
            write_report(NULL_CHAR*2+chr(merged_key[word])+NULL_CHAR*5)
        elif word[:6] == '\SLEEP':
            time.sleep(int(word[6:]))
        else:
            translate(word)

        write_report(NULL_CHAR*8)

if _name_ == '__main__':
    interpret()
```

#### Part 6 – Python Password Cracking

<https://www.youtube.com/watch?v=nyThp_0Nc0E>

```py
current_dir = os.path.dirname(os.path.realpath(__file__))
path_pass = os.path.join(current_dir,'password.txt')
path_action1 = os.path.join(current_dir,'action1.txt')
path_action2 = os.path.join(current_dir,'action2.txt')

def cracking(login):
    log = login

    with open(path_pass,'r') as fout:
        var_pass = fout.readlines()
    with open(path_action1,'r') as fout:
        var_action1 = fout.readlines()
    with open(path_action2,'r') as fout:
        var_action2 = fout.readlines()

    for passwd in var_pass:
        interpret(log)
        interpret(action1)
        interpret(passwd)
        interpret(action2)

if _name_ == '__main__':
    cracking('joe@something.com')
```

#### Part 7 – Python Final Notes

<https://www.youtube.com/watch?v=mzCau3jScmk>

```
Modifier Keys:
mod_key = {'\LEFTCTRL':1,'\LEFTSHIFT':2,'\LEFTALT':4,'\LEFTWIN':8,'\RIGHTCTRL':16,'\RIGHTSHIFT':32,'\RIGHTALT':64,'\RIGHTWIN':128}

Syntax:
write_report(chr(modifyer key number)+NULL_CHAR+chr(key stroke number)+NULL_CHAR*5)

Example (where '32' is right shift key and '4' is 'a' key):
write_report(chr(32)+NULL_CHAR+chr(4)+NULL_CHAR*5)

Output:
 'A'
```

- Forum Link for the USB HID Codes

    <https://forum.flirc.tv/index.php?%2Ftopic%2F2209-usb-hid-codes-keys-and-modifier-keys-for-flirc_util-record_api-x-y%2F>

    - **[PDF](./zero-hid/USB-HID-codes_Flirc-Forums.pdf)** Copy
- FreeBSD USB HID usage Table <https://www.freebsddiary.org/APC/usb_hid_usages.php>
    - **[PDF](./zero-hid/FreeBSD-USB-HID-usage-table.pdf)** Copy

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)