# TLSfeat - a Dedicated, Robust and Fast TLS Feature Extraction Tool

## Overview

TLSfeat is an easy-to-use and dedicated TLS feature extraction tool, which extracts comprehensive features from pcaps automatically, robustly and efficiently. TLSfeat aims to relieve the pain of TLS feature extraction, which can drastically accelerate analysis pipeline. TLSfeat is intended to be used by data scientists and security analysts to explore massive data at scale and in speed for building models and threat hunting, which can also be used in academic research.

TLSfeat has been well tested in Linux(Centos, Fedora, Ubuntu) and Mac OSX(x86, M1) using thousands of public pcaps up to 12G single pcaps.

TLSfeat software and its documents are free to use and distribute licensed by CC BY-NC-ND 4.0.  


## Design goals

- Easy to use and setup 
- Comprehensive feature extraction
  - meta, statistical
  - SPLT, byte distribution
  - TLS(handshakes, fingerprints, certificates)
- Robust to process real-world traffic with broken and malicious messages
- Native support of parallel analysis for significant performance boost
- Cross-platform

## Documentation

Before using TLSfeat, it's strongly suggested to read these documents:
 - **TLSfeat_User_Guide.pdf**: general, install, usage, feature description 
 - **TLSfeat_Benchmarks.pdf:** benchmarks 
 - **TLSfeat_FAQ.pdf**: FAQ

Documents are published in [Releases](https://github.com/zliucd/TLSfeat/releases/tag/v0.90).
 
## Download and install

**Hardware requirement**
 - Operating systems: Linux(x86_64)
 - Memory: 8 GB or higher
 - CPU: i5 or higher
 - Disk: 2 GB or higher (recommended, depending on feature files)

**Download**

TLSfeat is distributed as a binary, and download it from [Releases](https://github.com/zliucd/TLSfeat/releases/tag/v0.90). Unzip the file, and TLSfeat executable is located as ```bin/tlsfeat```.

To run TLSfeat executable, the only dependency is the shared library of libpcap.

**Install libpcap**

Centos/Fedora

```sudo dnf install libpcap-devel```

Ubuntu

```sudo apt-get install libpcap-dev```

For Ubuntu, please copy following command to create a symlink. 

```sudo ln -s /usr/lib/x86_64-linux-gnu/libpcap.so /usr/lib/x86_64-linux-gnu/libpcap.so.1```

**Usage**

TLSfeat is used in command line with several options, which accepts both relative and absolute path of pcap and output dir.

```
tlsfeat -d <dir> [-o <output>] [-t <thread>] [--small|--large] 
                 [--no-color] [--version] [--license]


OPTIONS

-d, --dir   [required]pcap dir path, default small mode

-o, --output [optional]output dir path, default '../featsout' if not specified

-t, --thread [optional]number of threads

--small     enable analysis of small pcaps, pcap max 100.0 MB, max 100 pcaps, max 6 threads

--large     enable analysis of large pcaps, pcap max 1.0 GB, max 10 pcaps, max 2 threads. As analyzing large pcaps may consume substantial memory, use 'large mode' at discretion.

--no-color  print in no color, default multicolor

--version   show version

--license   show license


```

**Examples**

There are sample pcaps to download [here](https://figshare.com/s/fe972fea50aad1d54f56), and unzip them at ```/opt/pcaps``` (change it at your flavor).

Analyze small pcaps

```tlsfeat -d /opt/pcaps/small_pcaps -o /opt/featsout/tests```

Analyze medium sized pcaps with default output dir and 4 threads

```tlsfeat -d /opt/pcaps/medium_pcaps -t 4```
 
Analyze large pcaps

```tlsfeat -d /opt/pcaps/large_pcaps --large```


**Printing in multicolors**

TLSfeat provides nice printing in terminal with multicolors as default, which works well in terminal's dark theme. However, it may not work properly in terminal's light theme. 

To print in mono color, use ```--no-color``` option.

## Benchmarks

Benchmarks of performance and artifacts extraction are well documented in TLSfeat_Benchmarks.pdf. 

Artifacts are also available to [download](https://figshare.com/s/eeb7d1e574fa860dcc6e).

## Releases

 - 2023-07: [v0.90](https://github.com/zliucd/TLSfeat/releases/tag/v0.90)

## License

TLSfeat and its documents are free to use and distribute licensed by [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode), see LICENSE. 

TLSBenchmarks is licensed by CC BY-NC.

## FAQ

(also see TLSfeat_FAQ.pdf.)

**1. Why TLSfeat has only command line?**

TLSfeat is designed for feature extraction, and command line is easier to use for this goal. A GUI version might be released, but it’s not clear if GUI will be useful. If you need GUI, let us know.


**2. Can TLSfeat run in Windows?**

TLSfeat supports only Linux for now, and the Windows version is going to be released. Meanwhile, virtual machines is recommended to use if you don’t have Linux at hand.   

   
**3. Does TLSfeat support live capture?**

No, it analyzes only pcaps.


**4. Environment and running issues**

 - Is virtual machine okay to run TLSfeat?

Though physical machines are preferred, TLSfeat can definitely run in virtual machines(we have tested it). Pay attention to hardware resources of virtual machines, especially CPU, memory and disk.

 - libpcap.so not found

This is a common issue. First, use locate libpcap.so to check if libpcap.so has been installed. Second, for Ubuntu, libpcap.so must be named as libpcap.so.1, and correct path should be /usr/lib/x86_64-linux-gnu/libpcap.so.1.

 - TLSfeat process is killed during analysis

A very likely reason is thread number specified is beyond CPU capability. e.g., a virtual machine has one 1 CPU, but TLSFeat is using 4 threads. Adjust threads to a proper value.


**5. Is TLSfeat free to use?**

Yes, it's free and distribute the program and documents under CC BY-NC-ND 4.0, e.g., for research and security analysis. However, you should not use the program and documents for commercial purposes or modify them.

   
**6. Is it okay to publish a derived 'dataset' using TLSfeat?**

‘Dataset' in this context means structured features rather than pcaps. It' okay to publish a derived 'dataset' using TLSfeat, as long as the 'dataset' does not conflict interests with parties who collected or published the pcaps.  


**7. How to analyze huge pcaps?**

TLSfeat has been tested to analyze single 10GB pcaps. It provides 'large mode' to analyze large pcaps (currently up to 1GB), but as the program may consume substantial memory, the thread number is limited. We are actively working on reducing memory usage.
   
For pcaps over 1GB, you can use Wireshark or other tools to filter non-TLS traffic.


**8. Does TLSfeat have logs?**

No, TLSfeat is 'log free', and all information is displayed in terminal. 


**9. Why messages printed in terminal is problematic?**

If the terminal is using light theme,  use --no-color option in common line. But it’s suggested to use dark theme to show multicolors properly.

**10. How to cite TLSfeat in research papers?**

You may include the name, author and URL of the tool in research papers as citation. 

**11. How to report bugs, feedbacks and other questions?**

TLSfeat is young, and there might be lots of bugs. Give us feedbacks through email <zliucd66@gmail.com> or post an issue. We will be glad to hear from you!

For bugs, please describe how to reproduce the bug, e.g., pcap, environment and screenshot.  For feature requests, please describe use case.
