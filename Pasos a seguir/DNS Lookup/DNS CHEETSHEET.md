# 🕵️‍♂️ Web Reconnaissance Cheat Sheet

> Web reconnaissance is the first step in any security assessment or penetration testing engagement. It's akin to a detective's initial investigation, meticulously gathering clues and evidence about a target before formulating a plan of action.

---

## 🎯 Goals of Web Reconnaissance

- Identifying assets: domains, subdomains, IP addresses.
- Uncovering hidden information: directories, files, technologies.
- Analyzing the attack surface: open ports, services, software versions.
- Gathering intelligence: employees, emails, technologies for social engineering or vulnerability mapping.

---

## 🔍 Types of Reconnaissance

| Type                  | Description                                                  | Risk of Detection |
|-----------------------|--------------------------------------------------------------|-------------------|
| **Active**            | Direct interaction with the target system.                   | Higher            |
| **Passive**           | Uses publicly available information, no direct interaction.  | Lower             |

**Examples:**
- Active: Port scanning, vulnerability scanning, network mapping.
- Passive: Search engine queries, WHOIS lookups, DNS enumeration, social media.

---

## 🧾 WHOIS Lookups

WHOIS is used to retrieve domain info like ownership, registration dates, and contact details.

```bash
whois example.com
```

Note: Data can be masked or inaccurate due to privacy services.

---

## 🌐 DNS (Domain Name System)

DNS translates domain names into IP addresses.

```bash
dig example.com A
```

### Common DNS Record Types

| Record | Description                                         |
|--------|-----------------------------------------------------|
| A      | Maps a hostname to an IPv4 address.                 |
| AAAA   | Maps a hostname to an IPv6 address.                 |
| CNAME  | Alias of one hostname to another.                   |
| MX     | Mail server info for the domain.                    |
| NS     | Authoritative name servers for the domain.          |
| TXT    | Arbitrary text (e.g., SPF records, verification).   |
| SOA    | Start of authority, admin info about the DNS zone.  |

---

## 🧭 Subdomain Enumeration

Subdomains can expose internal systems, dev servers, or staging environments.

### 🔨 Active vs Passive

| Approach   | Description                                               | Examples                                |
|------------|-----------------------------------------------------------|-----------------------------------------|
| **Active** | Interacts with DNS or probes the domain.                  | Brute-forcing, DNS zone transfers       |
| **Passive**| Gathers from public sources.                              | CT logs, search engines                 |

### 🔎 Brute-forcing with `dnsenum`

```bash
dnsenum example.com -f subdomains.txt

ffuf -w /usr/share/seclists/Discovery/DNS/namelist.txt -u http://10.10.11.11 -H "Host: FUZZ.board.htb" -fs 15949 -s

```

---

## 📄 DNS Zone Transfers (AXFR)

```bash
dig @ns1.example.com example.com axfr
```

> May expose full DNS zone info if misconfigured.

---

## 🏠 Virtual Host Discovery

Multiple sites on one IP can be discovered with vhost brute-force.

1. `Target Identification`: First, identify the target web server's IP address. This can be done through DNS lookups or other reconnaissance techniques.
2. `Wordlist Preparation`: Prepare a wordlist containing potential virtual host names. You can use a pre-compiled wordlist, such as SecLists, or create a custom one based on your target's industry, naming conventions, or other relevant information.
   
   - Consider using the `-t` flag to increase the number of threads for faster scanning.
- The `-k` flag can ignore SSL/TLS certificate errors.
- You can use the `-o` flag to save the output to a file for later analysis.
  
  example --> '''gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain'''
  
  example --> caso real '[★]$ gobuster vhost -u http://inlanefreight.htb:40002/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 200
'
dirsearch -u http://underpass.htb/daloradius/ -t 50
fuerza bruta de directorios


```bash
gobuster vhost -u http://192.0.2.1 -w hostnames.txt



```
```shell-session
 gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```
---

## 🔐 Certificate Transparency (CT) Logs

Useful for passive subdomain enumeration.

```bash
curl -s "https://crt.sh/?q=%25.example.com&output=json" \
| jq -r '.[].name_value' \
| sed 's/\*\.//g' \
| sort -u
```


ejemplo -->
 crt.sh lookup
 
While crt.sh offers a convenient web interface, you can also leverage its API for automated searches directly from your terminal. Let's see how to find all 'dev' subdomains on facebook.com using curl and jq:

  Certificate Transparency Logs
pepedavidphone@htb[/htb]$ curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
 
*.dev.facebook.com
*.newdev.facebook.com
*.secure.dev.facebook.com
dev.facebook.com
devvm1958.ftw3.facebook.com
facebook-amex-dev.facebook.com
facebook-amex-sign-enc-dev.facebook.com
newdev.facebook.com
secure.dev.facebook.com

---
## Fingerprinting Techniques


There are several techniques used for web server and technology fingerprinting:

- `Banner Grabbing`: Banner grabbing involves analysing the banners presented by web servers and other services. These banners often reveal the server software, version numbers, and other details.
- `Analysing HTTP Headers`: HTTP headers transmitted with every web page request and response contain a wealth of information. The `Server` header typically discloses the web server software, while the `X-Powered-By` header might reveal additional technologies like scripting languages or frameworks.
- `Probing for Specific Responses`: Sending specially crafted requests to the target can elicit unique responses that reveal specific technologies or versions. For example, certain error messages or behaviours are characteristic of particular web servers or software components.
- `Analysing Page Content`: A web page's content, including its structure, scripts, and other elements, can often provide clues about the underlying technologies. There may be a copyright header that indicates specific software being used, for example.
  

| Tool       | Description                                                                  | Features                                                                                  |
| ---------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Wappalyzer | Browser extension and online service for website technology profiling.       | Identifies a wide range of web technologies, including CMSs, frameworks, analytics tools. |
| BuiltWith  | Web technology profiler that provides detailed reports on a website's stack. | Offers both free and paid plans with varying levels of detail.                            |
| WhatWeb    | Command-line tool for website fingerprinting.                                | Uses a vast database of signatures to identify various web technologies.                  |
| Nmap       | Versatile network scanner for reconnaissance, service and OS fingerprinting. | Can be used with NSE scripts to perform more specialised fingerprinting.                  |
| Netcraft   | Web security service for fingerprinting and reporting.                       | Provides detailed reports on a website's technology, host, and security posture.          |
| wafw00f    | CLI tool for identifying Web Application Firewalls (WAFs).                   | Helps determine if a WAF is present and its type/configuration.                           |
"""
 


### Banner Grabbing

Our first step is to gather information directly from the web server itself. We can do this using the `curl` command with the `-I` flag (or `--head`) to fetch only the HTTP headers, not the entire page content.

  Fingerprinting

```shell-session
pepedavidphone@htb[/htb]$ curl -I inlanefreight.com
``` 

### wafw00f
To detect the presence of a WAF, we'll use the `wafw00f` tool. To install `wafw00f`, you can use pip3:

  Fingerprinting

```shell-session
pepedavidphone@htb[/htb]$ pip3 install git+https://github.com/EnableSecurity/wafw00f

```
## Nikto
To scan `inlanefreight.com` using `Nikto`, only running the fingerprinting modules, execute the following command:

  Fingerprinting

```shell-session
pepedavidphone@htb[/htb]$ nikto -h inlanefreight.com -Tuning b
```

The `-h` flag specifies the target host. The `-Tuning b` flag tells `Nikto` to only run the Software Identification modules.








---------------------------------------------------------

## 🕸️ Web Crawling

Crawling reveals site structure and hidden paths. Tools like Scrapy are useful.

### Example Scrapy Spider

```python
import scrapy

class ExampleSpider(scrapy.Spider):
    name = "example"
    start_urls = ['http://example.com/']

    def parse(self, response):
        for link in response.css('a::attr(href)').getall():
            if any(link.endswith(ext) for ext in self.interesting_extensions):
                yield {"file": link}
            elif not link.startswith("#") and not link.startswith("mailto:"):
                yield response.follow(link, callback=self.parse)
```

## Popular Web Crawlers

1. `Burp Suite Spider`: Burp Suite, a widely used web application testing platform, includes a powerful active crawler called Spider. Spider excels at mapping out web applications, identifying hidden content, and uncovering potential vulnerabilities.
2. `OWASP ZAP (Zed Attack Proxy)`: ZAP is a free, open-source web application security scanner. It can be used in automated and manual modes and includes a spider component to crawl web applications and identify potential vulnerabilities.
3. `Scrapy (Python Framework)`: Scrapy is a versatile and scalable Python framework for building custom web crawlers. It provides rich features for extracting structured data from websites, handling complex crawling scenarios, and automating data processing. Its flexibility makes it ideal for tailored reconnaissance tasks.
4. `Apache Nutch (Scalable Crawler)`: Nutch is a highly extensible and scalable open-source web crawler written in Java. It's designed to handle massive crawls across the entire web or focus on specific domains. While it requires more technical expertise to set up and configure, its power and flexibility make it a valuable asset for large-scale reconnaissance projects.

### Installing Scrapy

Before we begin, ensure you have Scrapy installed on your system. If you don't, you can easily install it using pip, the Python package installer:

```shell-session
[!bash!]$ pip3 install scrapy
```

This command will download and install Scrapy along with its dependencies, preparing your environment for building our spider.

### ReconSpider

First, run this command in your terminal to download the custom scrapy spider, `ReconSpider`, and extract it to the current working directory.

```shell-session
[!bash!]$ wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
[!bash!]$ unzip ReconSpider.zip 
```

With the files extracted, you can run `ReconSpider.py` using the following command:

```shell-session
[!bash!]$ python3 ReconSpider.py http://inlanefreight.com
```

Replace `inlanefreight.com` with the domain you want to spider. The spider will crawl the target and collect valuable information.

## Well known uris
| URI Suffix                        | Description                                                                                      | Status       | Reference                                                                                          |
|----------------------------------|--------------------------------------------------------------------------------------------------|--------------|----------------------------------------------------------------------------------------------------|
| `security.txt`                   | Contains contact information for security researchers to report vulnerabilities.                | Permanent    | [RFC 9116](https://www.rfc-editor.org/rfc/rfc9116.html)                                            |
| `/.well-known/change-password`   | Provides a standard URL for directing users to a password change page.                          | Provisional  | [W3C Spec](https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri) |
| `openid-configuration`           | Defines configuration details for OpenID Connect, an identity layer on top of OAuth 2.0.        | Permanent    | [OpenID Connect Discovery](http://openid.net/specs/openid-connect-discovery-1_0.html)              |
| `assetlinks.json`                | Used for verifying ownership of digital assets (e.g., apps) associated with a domain.           | Permanent    | [Digital Asset Links](https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md) |
| `mta-sts.txt`                    | Specifies the policy for SMTP MTA Strict Transport Security (MTA-STS) to enhance email security.| Permanent    | [RFC 8461](https://www.rfc-editor.org/rfc/rfc8461.html)                                            |



### Extract links from result:

```bash
jq -r '.[] | select(.file != null) | .file' example_data.json | sort -u
```

---

## 🔎 Search Engine Discovery (Google Dorks)
## Search Operators

Search operators are like search engines' secret codes. These special commands and modifiers unlock a new level of precision and control, allowing you to pinpoint specific types of information amidst the vastness of the indexed web.

While the exact syntax may vary slightly between search engines, the underlying principles remain consistent. Let's delve into some essential and advanced search operators:

|Operator|Operator Description|Example|Example Description|
|:--|:--|:--|:--|
|`site:`|Limits results to a specific website or domain.|`site:example.com`|Find all publicly accessible pages on example.com.|
|`inurl:`|Finds pages with a specific term in the URL.|`inurl:login`|Search for login pages on any website.|
|`filetype:`|Searches for files of a particular type.|`filetype:pdf`|Find downloadable PDF documents.|
|`intitle:`|Finds pages with a specific term in the title.|`intitle:"confidential report"`|Look for documents titled "confidential report" or similar variations.|
|`intext:` or `inbody:`|Searches for a term within the body text of pages.|`intext:"password reset"`|Identify webpages containing the term “password reset”.|
|`cache:`|Displays the cached version of a webpage (if available).|`cache:example.com`|View the cached version of example.com to see its previous content.|
|`link:`|Finds pages that link to a specific webpage.|`link:example.com`|Identify websites linking to example.com.|
|`related:`|Finds websites related to a specific webpage.|`related:example.com`|Discover websites similar to example.com.|
|`info:`|Provides a summary of information about a webpage.|`info:example.com`|Get basic details about example.com, such as its title and description.|
|`define:`|Provides definitions of a word or phrase.|`define:phishing`|Get a definition of "phishing" from various sources.|
|`numrange:`|Searches for numbers within a specific range.|`site:example.com numrange:1000-2000`|Find pages on example.com containing numbers between 1000 and 2000.|
|`allintext:`|Finds pages containing all specified words in the body text.|`allintext:admin password reset`|Search for pages containing both "admin" and "password reset" in the body text.|
|`allinurl:`|Finds pages containing all specified words in the URL.|`allinurl:admin panel`|Look for pages with "admin" and "panel" in the URL.|
|`allintitle:`|Finds pages containing all specified words in the title.|`allintitle:confidential report 2023`|Search for pages with "confidential," "report," and "2023" in the title.|
|`AND`|Narrows results by requiring all terms to be present.|`site:example.com AND (inurl:admin OR inurl:login)`|Find admin or login pages specifically on example.com.|
|`OR`|Broadens results by including pages with any of the terms.|`"linux" OR "ubuntu" OR "debian"`|Search for webpages mentioning Linux, Ubuntu, or Debian.|
|`NOT`|Excludes results containing the specified term.|`site:bank.com NOT inurl:login`|Find pages on bank.com excluding login pages.|
|`*` (wildcard)|Represents any character or word.|`site:socialnetwork.com filetype:pdf user* manual`|Search for user manuals (user guide, user handbook) in PDF format on socialnetwork.com.|
|`..` (range search)|Finds results within a specified numerical range.|`site:ecommerce.com "price" 100..500`|Look for products priced between 100 and 500 on an e-commerce website.|
|`" "` (quotation marks)|Searches for exact phrases.|`"information security policy"`|Find documents mentioning the exact phrase "information security policy".|
https://www.exploit-db.com/google-hacking-database
## 🧠 Web Archives (Wayback Machine)

| Feature              | Description                              | Recon Use Case                            |     |
| -------------------- | ---------------------------------------- | ----------------------------------------- | --- |
| Historical Snapshots | View older versions of a website.        | Identify past exposed features or files.  |     |
| Hidden Directories   | Access previously public paths or files. | Discover outdated or sensitive resources. |     |
| Content Changes      | Track modifications over time.           | Analyze content for security evolution.   |     |
## Reconnaissance Frameworks

These frameworks aim to provide a complete suite of tools for web reconnaissance:

- [FinalRecon](https://github.com/thewhiteh4t/FinalRecon): A Python-based reconnaissance tool offering a range of modules for different tasks like SSL certificate checking, Whois information gathering, header analysis, and crawling. Its modular structure enables easy customisation for specific needs.
- [Recon-ng](https://github.com/lanmaster53/recon-ng): A powerful framework written in Python that offers a modular structure with various modules for different reconnaissance tasks. It can perform DNS enumeration, subdomain discovery, port scanning, web crawling, and even exploit known vulnerabilities.
- [theHarvester](https://github.com/laramies/theHarvester): Specifically designed for gathering email addresses, subdomains, hosts, employee names, open ports, and banners from different public sources like search engines, PGP key servers, and the SHODAN database. It is a command-line tool written in Python.
- [SpiderFoot](https://github.com/smicallef/spiderfoot): An open-source intelligence automation tool that integrates with various data sources to collect information about a target, including IP addresses, domain names, email addresses, and social media profiles. It can perform DNS lookups, web crawling, port scanning, and more.
- [OSINT Framework](https://osintframework.com/): A collection of various tools and resources for open-source intelligence gathering. It covers a wide range of information sources, including social media, search engines, public records, and more.

### FinalRecon

`FinalRecon` offers a wealth of recon information:

- `Header Information`: Reveals server details, technologies used, and potential security misconfigurations.
- `Whois Lookup`: Uncovers domain registration details, including registrant information and contact details.
- `SSL Certificate Information`: Examines the SSL/TLS certificate for validity, issuer, and other relevant details.
- `Crawler`:
    - HTML, CSS, JavaScript: Extracts links, resources, and potential vulnerabilities from these files.
    - Internal/External Links: Maps out the website's structure and identifies connections to other domains.
    - Images, robots.txt, sitemap.xml: Gathers information about allowed/disallowed crawling paths and website structure.
    - Links in JavaScript, Wayback Machine: Uncovers hidden links and historical website data.
- `DNS Enumeration`: Queries over 40 DNS record types, including DMARC records for email security assessment.
- `Subdomain Enumeration`: Leverages multiple data sources (crt.sh, AnubisDB, ThreatMiner, CertSpotter, Facebook API, VirusTotal API, Shodan API, BeVigil API) to discover subdomains.
- `Directory Enumeration`: Supports custom wordlists and file extensions to uncover hidden directories and files.
- `Wayback Machine`: Retrieves URLs from the last five years to analyse website changes and potential vulnerabilities.

##### Installation 

pepedavidphone@htb[/htb]$ git clone https://github.com/thewhiteh4t/FinalRecon.git
pepedavidphone@htb[/htb]$ cd FinalRecon
pepedavidphone@htb[/htb]$ pip3 install -r requirements.txt
pepedavidphone@htb[/htb]$ chmod +x ./finalrecon.py
pepedavidphone@htb[/htb]$ ./finalrecon.py --help


ejemplo -->
```shell-session
./finalrecon.py --headers --whois --url http://inlanefreight.com
```

![[Information_Gathering_Web_Edition_Module_Cheat_Sheet.pdf]]