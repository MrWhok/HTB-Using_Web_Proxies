# HTB-Using_Web_Proxies

## Table of Contents
1. [Web Proxy](#web-proxy)
    1. [Intercepting Web Requests](#intercepting-web-requests)
    2. [Repeating Requests](#repeating-requests)
    3. [Encoding/Decoding](#encodingdecoding)
    4. [Proxying Tools](#proxying-tools)
2. [Web Fuzzer](#web-fuzzer)
    1. [Burp Intruder](#burp-intruder)
    2. [ZAP Fuzzer](#zap-fuzzer)
3. [Web Scanner](#web-scanner)
    1. [ZAP Scanner](#zap-scanner)
4. [Skills Assessment](#skills-assessment)

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

## Web Fuzzer
### Burp Intruder
#### Challenges
1. Use Burp Intruder to fuzz for '.html' files under the /admin directory, to find a file containing the flag.

    We can solve this by using intruder option on burptsuite. After we send request to the intruder, we can set several setting. 

    ![alt text](<Assets/Burp Intruder - 1.png>)

    Once we have done setting, we can click `Start Attack` button. After several tries, we will got 200 response code.

    ![alt text](<Assets/Burp Intruder - 2.png>)

    We can go to the browser and check `http://83.136.254.49:36811/admin/2010.html`. The answer is `HTB{burp_1n7rud3r_fuzz3r!}`.

### ZAP Fuzzer
#### Challenges
1. The directory we found above sets the cookie to the md5 hash of the username, as we can see the md5 cookie in the request for the (guest) user. Visit '/skills/' to get a request with a cookie, then try to use ZAP Fuzzer to fuzz the cookie for different md5 hashed usernames to get the flag. Use the "top-usernames-shortlist.txt" wordlist from Seclists.

    We can type `zaproxy` to open zap. Then, open browser option from there and type this url `83.136.252.27:56950/skills/`. AFter that, we need to refresh the browser. Back to our zap, click the request section and select attack -> fuzzer. Then, select the cookies value and click add. We can choose this `/usr/share/seclists/Usernames/top-usernames-shortlist.txt` as a payload and click add. Back to our payload option, choose `processors` and then add `MD5 Hash`. Then, we can start fuzzer.

    ![alt text](<Assets/ZAP Fuzzer - 1.png>)

    We can choose the response with code 200 and the response that has bigger size resp body bytes. The answer is `HTB{fuzz1n6_my_f1r57_c00k13}`.

## Web Scanner
### ZAP Scanner
#### Challenges
1. Run ZAP Scanner on the target above to identify directories and potential vulnerabilities. Once you find the high-level vulnerability, try to use it to read the flag at '/flag.txt'

    We can solve it by using `ZAP`. After opening `ZAP`, go to quick start tab and select automate scan. Once it finished, go to `alerts` section. We also can generate report.

    ![alt text](<Assets/ZAP Scanner - 1.png>)

    We can see it has `OS Remote Command Injection`. By using this url, `http://94.237.59.225:52774/devtools/ping.php?ip=127.0.0.1;cat /flag.txt`, we can get the flag. The answer is `HTB{5c4nn3r5_f1nd_vuln5_w3_m155}`.

## Skills Assessment
### Skills Assessment - Using Web Proxies
1. The /lucky.php page has a button that appears to be disabled. Try to enable the button, and then click it to get the flag.

    In here, i used `ZAP` with UHD feature to do this challenge. We can click second button on the left pane and do hard refresh, `ctrl+shift+R`. After that, click step until we intercept the response section.

    ![alt text](<Assets/Skills Assessment - 1.png>)

    We can see the submit button is disabled. To enable it, just remove `disabled`. We can click `continue`. Then, click the flag button. The answer is `HTB{d154bl3d_bu770n5_w0n7_570p_m3}`.

2. The /admin.php page uses a cookie that has been encoded multiple times. Try to decode the cookie until you get a value with 31-characters. Submit the value as the answer.

    We can intercept the response and get the user cookies.

    ![alt text](<Assets/Skills Assessment - 2.png>)

    Then, select cookies value and right click. We can choose encode/decode option. From that, we can select Full URL decode -> Base64 decode. The asnwer is `3dac93b8cd250aa8c1a36fffc79a17a`.

3. Once you decode the cookie, you will notice that it is only 31 characters long, which appears to be an md5 hash missing its last character. So, try to fuzz the last character of the decoded md5 cookie with all alpha-numeric characters, while encoding each request with the encoding methods you identified above. (You may use the "alphanum-case.txt" wordlist from Seclist for the payload)

    To solve this,i change to use `burpsuite`. We need to intercept the request of /admin.php. We can try to fuzz from there. Here how it looks like.

    ![alt text](<Assets/Skills Assessment - 3.png>) 

    Once we have done, we can start the attak. We can look the response with different length. 

    ![alt text](<Assets/Skills Assessment - 4.png>)

    The answer is `HTB{burp_1n7rud3r_n1nj4!}`.

4. You are using the 'auxiliary/scanner/http/coldfusion_locale_traversal' tool within Metasploit, but it is not working properly for you. You decide to capture the request sent by Metasploit so you can manually verify it and repeat it. Once you capture the request, what is the 'XXXXX' directory being called in '/XXXXX/administrator/..'?

    To solve this, we can use metasploit and set the `PROXIES`. 

    ```bash
    use auxiliary/scanner/http/coldfusion_locale_traversal
    set PROXIES HTTP:127.0.0.1:8080
    set RHOSTS 83.136.254.49
    set RPORT 40637
    run
    ```
    ![alt text](<Assets/Skills Assessment - 5.png>)

    The answer is `CFIDE`.