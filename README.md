# Sudomy
[![License](https://img.shields.io/badge/license-MIT-red.svg)](https://github.com/Screetsec/Sudomy/blob/master/LICENSE.md)  [![Build Status](https://action-badges.now.sh/screetsec/sudomy)](https://github.com/Screetsec/Sudomy/actions)  [![Version](https://img.shields.io/badge/Release-1.2.1-red.svg?maxAge=259200)]()  [![Build](https://img.shields.io/badge/Supported_OS-Linux-yellow.svg)]()  [![Build](https://img.shields.io/badge/Supported_WSL-Windows-blue.svg)]() [![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/screetsec/sudomy/issues) [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/screetsec)
### Subdomain Enumeration & Analysis
<img width="935" alt="sudomy" src="https://user-images.githubusercontent.com/17976841/102172910-685b5a80-3ecc-11eb-9bac-3003d786dd33.png">



***Sudomy*** is a subdomain enumeration tool to collect subdomains and analyzing domains performing advanced automated reconnaissance (framework). This tool can also be used for OSINT (Open-source intelligence) activities.

## Features !
##### For recent time, ***Sudomy*** has these 20 features:
-  Easy, light, fast and powerful. Bash script (controller) is available by default in almost all Linux distributions. By using bash script multiprocessing feature, all processors will be utilized optimally.
-  Subdomain enumeration process can be achieved by using **active** method or **passive** method
    - **Active Method**
        - *Sudomy* utilize Gobuster tools because of its highspeed performance in carrying out DNS Subdomain Bruteforce attack (wildcard support). The wordlist that is used comes from combined SecList (Discover/DNS) lists which contains around 3 million entries

    - **Passive Method**
        - By evaluating and **selecting** the **good** third-party sites/resources, the enumeration process can be **optimized**. More results will be obtained with less time required. *Sudomy* can collect data from these  well-curated 22 third-party sites:

				https://censys.io
				https://developer.shodan.io
				https://dns.bufferover.run
				https://index.commoncrawl.org 
				https://riddler.io 
				https://api.certspotter.com
				https://api.hackertarget.com 
				https://api.threatminer.org
				https://community.riskiq.com
				https://crt.sh
				https://dnsdumpster.com
				https://docs.binaryedge.io
				https://securitytrails.com
				https://graph.facebook.com
				https://otx.alienvault.com
				https://rapiddns.io
				https://spyse.com
				https://urlscan.io
				https://www.dnsdb.info
				https://www.virustotal.com
				https://threatcrowd.org
				https://web.archive.org
- Test the list of collected subdomains and probe for working http or https servers. This feature uses a third-party tool, [httprobe](https://github.com/tomnomnom/httprobe "httprobe").
- Subdomain availability test based on Ping Sweep and/or by getting HTTP status code.
- The ability to detect virtualhost (several subdomains which resolve to single IP Address). Sudomy will resolve the collected subdomains to IP addresses, then classify them if several subdomains resolve to single IP address. This feature will be very useful for the next penetration testing/bug bounty process. For instance, in port scanning, single IP address won’t be scanned repeatedly
- Performed port scanning from collected subdomains/virtualhosts IP Addresses
- Testing Subdomain TakeOver attack (CNAME Resolver, DNSLookup, Detect NXDomain, Check Vuln)
- Taking Screenshots of subdomains default using gowitness or you can choice another screenshot tools, like (-ss webscreeenshot)
- Identify technologies on websites (category,application,version)
- Detection urls, ports, title, content-length, status-code, response-body probbing.
- Smart auto fallback from https to http as default.
- Data Collecting/Scraping open port from 3rd party (Default::Shodan), For right now just using Shodan [Future::Censys,Zoomeye]. More efficient and effective to collecting port from list ip on target [[ Subdomain > IP Resolver > Crawling > ASN & Open Port ]]
- Collecting Juicy URL & Extract URL Parameter ( Resource Default::WebArchive, CommonCrawl, UrlScanIO) 
- Collect interesting path (api|.git|admin|etc), document (doc|pdf), javascript (js|node) and parameter
- Define path for outputfile (specify an output file when completed) 
- Check an IP is Owned by Cloudflare 
- Generate & make wordlist based on collecting url resources (wayback,urlscan,commoncrawl. To make that, we Extract All the paramater and path from our domain recon
- Generate Network Graph Visualization Subdomain & Virtualhosts
- Report output in HTML & CSV format
- Sending notifications to a slack channel
- Testing for HTTP Request Smuggling / Desync Attack

## How Sudomy Works 
How sudomy works or recon flow, when you run the best arguments to collect subdomains and analyze by doing automatic recon.
```
root@maland: ./sudomy -d bugcrowd.com -dP -eP -rS -cF -pS -tO -gW --httpx --dnsprobe  -aI webanalyze -sS
```
### Recon Worfklow
This Recon Workflow Sudomy v1.1.8#dev 

![Recon Workflow](https://raw.githubusercontent.com/Screetsec/Sudomy/master/doc/Sudomy%20-%20Recon%20Workflow%20v1.1.8%23dev.png)

### Detail information
Detail information File Reconnaissance & Juicy Data
```
------------------------------------------------------------------------------------------------------

- subdomain.txt             -- Subdomain list             < $DOMAIN (Target)
- httprobe_subdomain.txt    -- Validate Subdomain	  < subdomain.txt
- webanalyzes.txt           -- Identify technology scan   < httprobe_subdomain.txt
- httpx_status_title.txt    -- title+statuscode+lenght    < httprobe_subdomain.txt
- dnsprobe_subdomain.txt    -- Subdomain resolv		  < subdomain.txt
- Subdomain_Resolver.txt    -- Subdomain resolv (alt)     < subdomain.txt
- cf-ipresolv.txt           -- Cloudflare scan        	  < ip_resolver.txt 
- Live_hosts_pingsweep.txt  -- Live Host check		  < ip_resolver.txt	 
- ip_resolver.txt           -- IP resolv list          	  < Subdomain_Resolver::dnsprobe
- ip_dbasn.txt		    -- ASN Number Check		  < ip_resolver.txt
- vHost_subdomain.txt       -- Virtual Host (Group by ip) < Subdomain_Resolver.txt
- nmap_top_ports.txt        -- Active port scanning       < cf-ipresolv.txt
- ip_dbport.txt		    -- Passive port scanning	  < cf-ipresolv.txt

------------------------------------------------------------------------------------------------------
- Passive_Collect_URL_Full.txt 		-- Full All Url Crawl (WebArchive, CommonCrawl, UrlScanIO)
------------------------------------------------------------------------------------------------------

- ./screenshots/report-0.html   	-- Screenshoting report    	< httprobe_subdomain.txt
- ./screenshots/gowitness.db   		-- Database screenshot    	< httprobe_subdomain.txt

------------------------------------------------------------------------------------------------------

- ./interest/interesturi-allpath.out	-- Interest path(/api,/git,etc) < Passive_Collect_URL_Full.txt
- ./interest/interesturi-doc.out	-- Interest doc (doc,pdf,xls)   < Passive_Collect_URL_Full.txt
- ./interest/interesturi-otherfile.out	-- Other files (.json,.env,etc) < Passive_Collect_URL_Full.txt
- ./interest/interesturi-js.out		-- All Javascript files(*.js)  	< Passive_Collect_URL_Full.txt
- ./interest/interesturi-nodemodule.out	-- Files from /node_modules/    < Passive_Collect_URL_Full.txt
- ./interest/interesturi-param-full.out	-- Full parameter list 		< Passive_Collect_URL_Full.txt
- ./interest/interesturi-paramsuniq.out -- Full Uniq parameter list 	< Passive_Collect_URL_Full.txt

-  Notes : You can validate juicy/interest urls/param using urlprobe or httpx to avoid false positives
------------------------------------------------------------------------------------------------------

- ./takeover/CNAME-resolv.txt		-- CNAME Resolver 		< subdomain.txt
- ./takeover/TakeOver-Lookup.txt	-- DNSLookup 			< CNAME-resolv.txt
- ./takeover/TakeOver-nxdomain.txt	-- Other 3d service platform	< TakeOver-Lookup.txt
- ./takeover/TakeOver.txt		-- Checking Vulnerabilty	< CNAME-resolv.txt

------------------------------------------------------------------------------------------------------

- ./wordlist/wordlist-parameter.lst     -- Generate params wordlist     < Passive_Collect_URL_Full.txt
- ./wordlist/wordlist-pathurl.lst       -- Generate List paths wordlis  < Passive_Collect_URL_Full.txt

-  Notes : This Wordlist based on domain & subdomain information (path,file,query strings & parameter)
------------------------------------------------------------------------------------------------------
```

## Publication
- [Sudomy: Information Gathering Tools for Subdomain Enumeration and Analysis](https://iopscience.iop.org/article/10.1088/1757-899X/771/1/012019/meta) -  IOP Conference Series: Materials Science and Engineering, Volume 771, 2nd International Conference on Engineering and Applied Sciences (2nd InCEAS) 16 November 2019, Yogyakarta, Indonesia

## User Guide
- Offline User Guide : [Sudomy - Subdomain Enumeration and Analysis User Guide v1.0](https://github.com/Screetsec/Sudomy/blob/master/doc/Sudomy%20-%20Subdomain%20Enumeration%20%26%20Analaysis%20User%20Guide%20v1.0.pdf)
- Online User Guide : [Subdomain Enumeration and Analysis User Guide](https://sudomy.screetsec.web.id/features) - Up to date

## Comparison
Sudomy minimize more resources when use resources (Third-Party Sites) By evaluating and selecting the good third-party sites/resources, so the enumeration process can be optimized. The domain that is used in this comparison is ***tiket.com***.

The following are the results of passive enumeration DNS testing of *Sublist3r v1.1.0, Subfinder v2.4.5*, and *Sudomy v1.2.0*.

<img width="935" alt="Untitled" src="https://user-images.githubusercontent.com/17976841/102053848-00990700-3e1b-11eb-9f48-9a82a8b3e64e.png">

In here subfinder is still classified as very fast for collecting subdomains by utilizing quite a lot of resources. Especially if the resources used have been optimized (?).

For compilation results and videos, you can check here: 

- [Sudomy](https://www.youtube.com/watch?v=ksZkMpLljcY)
- [Subfinder](https://youtu.be/Zxf3pwh7uMI)
- [Sublist3r](https://youtu.be/DexFkrEwtt4)

When I have free time. Maybe In the future, sudomy will use golang too. If you want to contributes it's open to pull requests.

### But it's shit! And your implementation sucks!
- Yes, you're probably correct. Feel free to "Not use it" and there is a pull button to "Make it better". 

## Installation
*Sudomy* is currently extended with the following tools. Instructions on how to install & use the application are linked below.

### To Download Sudomy From Github
```bash
# Clone this repository
git clone --recursive https://github.com/screetsec/Sudomy.git
```

### Dependencies
```
$ python3 -m pip install -r requirements.txt
```
*Sudomy* requires [jq](https://stedolan.github.io/jq/download/) and [GNU grep](https://www.gnu.org/software/grep/) to run and parse. Information on how to download and install jq can be accessed [here](https://stedolan.github.io/jq/download/)

```bash
# Linux
apt-get update
apt-get install jq nmap phantomjs npm chromium parallel
npm i -g wappalyzer wscat

# Mac
brew cask install phantomjs 
brew install jq nmap npm parallel
npm i -g wappalyzer wscat

brew install grep

# Note
All you would need is an installation of the latest Google Chrome or Chromium 
Set the PATH in rc file for GNU grep changes
```

## Running in a Docker Container
```bash
# Pull an image from DockerHub
docker pull screetsec/sudomy:v1.2.1-dev

# Create output directory
mkdir output

# Run an image, you can run the image on custom directory but you must copy/download config sudomy.api on current directory
docker run -v "${PWD}/output:/usr/lib/sudomy/output" -v "${PWD}/sudomy.api:/usr/lib/sudomy/sudomy.api" -t --rm screetsec/sudomy:v1.1.9-dev [argument]

# or define API variable when executed an image.

docker run -v "${PWD}/output:/usr/lib/sudomy/output" -e "SHODAN_API=xxxx" -e "VIRUSTOTAL=xxxx" -t --rm screetsec/sudomy:v1.1.9-dev [argument]
```

### Post Installation
API Key is needed before querying on third-party sites, such as ```Shodan, Censys, SecurityTrails, Virustotal,``` and ```BinaryEdge```.
- The API key setting can be done in sudomy.api file.
```bash
# Shodan
# URL :  http://developer.shodan.io
# Example :
#      - SHODAN_API="VGhpc1M0bXBsZWwKVGhmcGxlbAo"

SHODAN_API=""

# Censys
# URL : https://censys.io/register

CENSYS_API=""
CENSYS_SECRET=""

# Virustotal
# URL : https://www.virustotal.com/gui/
VIRUSTOTAL=""


# Binaryedge
# URL : https://app.binaryedge.io/login
BINARYEDGE=""


# SecurityTrails
# URL : https://securitytrails.com/
SECURITY_TRAILS=""
```
YOUR_WEBHOOK_URL is needed before using the slack notifications
- The URL setting can be done in slack.conf file.
```bash
# Configuration Slack Alert
# For configuration/tutorial to get webhook url following to this site
#     - https://api.slack.com/messaging/webhooks
# Example: 
#     - YOUR_WEBHOOK_URL="https://hooks.slack.com/services/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"

YOUR_WEBHOOK_URL="https://hooks.slack.com/services/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"
```

## Usage

```text
 ___         _ _  _           
/ __|_  _ __| (_)(_)_ __ _  _ 
\__ \ || / _  / __ \  ' \ || |
|___/\_,_\__,_\____/_|_|_\_, |
                          |__/ v{1.2.1#dev} by @screetsec 
Sud⍥my - Fast Subdmain Enumeration and Analyzer      
         http://github.com/screetsec/sudomy

Usage: sud⍥my.sh [-h [--help]] [-s[--source]][-d[--domain=]] 

Example: sud⍥my.sh -d example.com   
         sud⍥my.sh -s Shodan,VirusTotal -d example.com

Best Argument:
  sudomy -d domain.com -dP -eP -rS -cF -pS -tO -gW --httpx --dnsprobe  -aI webanalyze --slack -sS


Optional Arguments:
  -a,  --all             Running all Enumeration, no nmap & gobuster 
  -b,  --bruteforce      Bruteforce Subdomain Using Gobuster (Wordlist: ALL Top SecList DNS) 
  -d,  --domain          domain of the website to scan
  -h,  --help            show this help message
  -o,  --outfile         specify an output file when completed 
  -s,  --source          Use source for Enumerate Subdomain
  -aI, --apps-identifier Identify technologies on website (ex: -aI webanalyze)
  -dP, --db-port         Collecting port from 3rd Party default=shodan
  -eP, --extract-params  Collecting URL Parameter from Engine
  -tO, --takeover        Subdomain TakeOver Vulnerabilty Scanner
  -wS, --websocket       WebSocket Connection Check
  -cF, --cloudfare       Check an IP is Owned by Cloudflare
  -pS, --ping-sweep      Check live host using methode Ping Sweep
  -rS, --resolver        Convert domain lists to resolved IP lists without duplicates
  -sC, --status-code     Get status codes, response from domain list
  -nT, --nmap-top        Port scanning with top-ports using nmap from domain list
  -sS, --screenshot      Screenshots a list of website (default: gowitness)
  -nP, --no-passive      Do not perform passive subdomain enumeration 
  -gW, --gwordlist       Generate wordlist based on collecting url resources (Passive) 
       --httpx           Perform httpx multiple probers using retryablehttp 
       --dnsprobe        Perform multiple dns queries (dnsprobe) 
       --no-probe        Do not perform httprobe 
       --html            Make report output into HTML 
       --graph           Network Graph Visualization
```
To use all 22 Sources and Probe for working http or https servers (Validations):
```
$ sudomy -d hackerone.com
```
To use one or more source:
```
$ sudomy -s shodan,dnsdumpster,webarchive -d hackerone.com
```
To use all Sources Without Validations:
```
$ sudomy -d hackerone.com --no-probe
```
To use one or more plugins:
```
$ sudomy -pS -sC -sS -d hackerone.com
```
To use all plugins: testing host status, http/https status code, subdomain takeover and screenshots. 

Nmap,Gobuster,wappalyzer and wscat Not Included.
```
$ sudomy -d hackerone.com --all 
```

To create report in HTML Format
```
$ sudomy -d hackerone.com --html --all
```

HTML Report Sample:

| Dashboard	| Reports	|
| ------------  | ------------ |
|![Index](https://user-images.githubusercontent.com/17976841/63597336-6ab6e880-c5e7-11e9-819e-91634e347b0c.PNG)|![f](https://user-images.githubusercontent.com/17976841/63597476-bbc6dc80-c5e7-11e9-8985-6a73348a2e02.PNG)|


To gnereate network graph visualization subdomain & virtualhosts
```
$ sudomy -d hackerone.com -rS --graph
```
Graph Visualization [Sample](https://screetsec.github.io/): 
| nGraph	|
| ------------  |
|![nGraph](https://user-images.githubusercontent.com/17976841/104086846-b24a1d00-528d-11eb-88f5-de9bb0b641d1.PNG)|

To use best arguments to collect subdomains, analyze by doing automatic recon and sending notifications to slack
```
./sudomy -d ngesec.id -dP -eP -rS -cF -pS -tO -gW --httpx --dnsprobe --graph  -aI webanalyze --slack -sS
```
Slack Notification Sample:
| Slack 	|
| ------------  |
|![Slacks](https://user-images.githubusercontent.com/17976841/95856703-a4672780-0d84-11eb-9a3e-03ab39e4dc10.png)|




## Tools Overview
- Youtube Videos : Click [here](http://www.youtube.com/watch?v=DpXIBUtasn0)


## Translations
- [Indonesia](https://github.com/Screetsec/Sudomy/blob/master/doc/README_ID.md)
- [English](https://github.com/Screetsec/Sudomy/blob/master/doc/README_EN.md)
- [Portuguese - Brazil](https://github.com/Screetsec/Sudomy/blob/master/doc/README_PT_BR.md)


## Changelog
All notable changes to this project will be documented in this [file](https://github.com/Screetsec/sudomy/blob/master/CHANGELOG.md).



## Alternative Best Tool - Subdomain Enumeration
- [Subfinder](https://github.com/projectdiscovery/subfinder) - Projectdiscovery
- [Sublist3r](https://github.com/aboul3la/Sublist3r) - aboul3la
- [Findomain](https://github.com/Edu4rdSHL/findomain) - Edu4rdSHL
- [Amass](https://github.com/OWASP/Amass) - OWASP

## Credits & Thanks
- [Tom Hudson](https://github.com/tomnomnom/) - Tomonomnom
- [OJ Reeves](https://github.com/OJ/) - Gobuster
- [ProjectDiscovery](https://github.com/projectdiscovery) - Security Through Intelligent Automation
- [Thomas D Maaaaz](https://github.com/maaaaz) - Webscreenshot
- [Dwi Siswanto](https://github.com/dwisiswant0) - cf-checker
- [Robin Verton](https://github.com/rverton/webanalyze) - webanalyze
- [christophetd](https://github.com/christophetd/censys-subdomain-finder) - Censys
- [Daniel Miessler](https://github.com/danielmiessler/) - SecList
- [EdOverflow](https://github.com/EdOverflow/) - can-i-take-over-xyz
- [jerukitumanis](https://github.com/myugan) - Docker Maintainer
- [NgeSEC](https://ngesec.id/) - Community
- [Zerobyte](http://zerobyte.id/) - Community
- [Gauli(dot)Net](https://gauli.net/) - Lab Hacking Indonesia
- [missme3f](https://github.com/missme3f/) - Raditya Rahma
- [Bugcrowd](https://www.bugcrowd.com/) & [Hackerone](https://www.hackerone.com/)
- [darknetdiaries](https://darknetdiaries.com/) - Awesome Art
