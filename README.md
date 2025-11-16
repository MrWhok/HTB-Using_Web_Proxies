# HTB-Using_Web_Proxies

## Table of Contents
1. [Web Proxy](#web-proxy)
    1. [Intercepting Web Requests](#intercepting-web-requests)
    2. [Repeating Requests](#repeating-requests)
    3. [Encoding/Decoding](#encodingdecoding)
    4. [Proxying Tools](#proxying-tools)

## Web Proxy
### Intercepting Web Requests
#### Tools
1. burpsuite
2. zap

#### Challenges
1. Try intercepting the ping request on the server shown above, and change the post data similarly to what we did in this section. Change the command to read 'flag.txt'

    We can solve this by intercepting the request. First we manuplate the parameter `ip=;ls;`. It will gave us the list of the files, including flag.txt. Then we can reload and intercept again. After that, wec can manipluate the parameter `ip=;cat flag.txt;`. It will gave us the flag. The answer is `HTB{1n73rc3p73d_1n_7h3_m1ddl3}`.

### Repeating Requests
#### Challenges
1. Try using request repeating to be able to quickly test commands. With that, try looking for the other flag.

    We can do exactly like the module said. We can send the request to the repeater to make it faster, so we dont need to forward it manually/![alt text](image.png). After that we can type `;ls -la /;` in the ip section. 

    ![alt text](<Assets/Repeating Requests - 1.png>)

    We can see the other flag location. The answer is `HTB{qu1ckly_r3p3471n6_r3qu3575}`.

### Encoding/Decoding
#### Challenges
1. The string found in the attached file has been encoded several times with various encoders. Try to use the decoding tools we discussed to decode it and get the flag.

    We can copy the challenge and use burpsuite decoder section to solve this. 

    ![alt text](<Assets/Encoding_Decoding - 1.png>)

    Base64 -> Base64 -> Base64 -> Base64 -> URL. The answer is `HTB{3nc0d1n6_n1nj4}`.

### Proxying Tools
#### Challenges
1. Try running 'auxiliary/scanner/http/http_put' in Metasploit on any website, while routing the traffic through Burp. Once you view the requests sent, what is the last line in the request?

    To do this, we can use `msfconsole`.

    ```bash
    [msf](Jobs:0 Agents:0) >> use auxiliary/scanner/http/http_put
    [msf](Jobs:0 Agents:0) auxiliary(scanner/http/http_put) >> set PROXIES HTTP:127.0.0.1:8080
    [msf](Jobs:0 Agents:0) auxiliary(scanner/http/http_put) >> SET RHOSTS 83.136.253.5
    [msf](Jobs:0 Agents:0) auxiliary(scanner/http/http_put) >> set RPORT 51387
    [msf](Jobs:0 Agents:0) auxiliary(scanner/http/http_put) >> run
    ```
    Once we run it, burpsuite will caputure it. 

    ![alt text](<Assets/Proxying Tools - 1.png>)

    The answer is `msf test file`.

