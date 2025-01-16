### in medium Eslam Aki [link of writeup](https://infosecwriteups.com/simple-recon-methodology-920f5c5936d4)

## What’s Recon ?
- #### **Recon** is the process by which you collect more information about your target, more information like subdomains, links, open ports, hidden directories, service information, etc.
- ####  Recon based scope
	- ###### small scope
	- ###### Medium  scope
	- ###### large scope
### Small Scope
- ###### all processes hare will be performed on specifies subdomain
- In this type of scopes, you have the only _subdomain_ which you are allowed to test on it like `sub.domain.com` and you don’t have any permission to test on any other subdomain, the information which you should collect will be like this…
	- Directory enum
	- GitHub Dorking
	- Server enum
	- Database enum
	- Google dorking for sensitive files
	- Extract juicy vulnerable links by GF-Patterns
	- Waybackurls enum
	- Parameter discovery
	- Automation vulnerability
	- JS file analysis
	- Backend enum
	- WAF detection
	- Git Hub search links
	- Port Scan
### **Medium scope**
- Here your testing area will be increased to contain all subdomains related to a specific domain, for example, you have a domain like `example.com` and on your program page, you’re allowed to test all subdomains like `*.domain.com` In this step the information which you should collect will be more than the small scope to contain for example all subdomains and treat every subdomain as small scope “_we will talk more about this point_”, just know the type of the information only.
- ##### All processes here will be performed on all *subdomains*
	- List of subdomains
	- subdomain takeover
	- Misconfiguration in storage vuln (s3 buckets)
	- Directory enum
	- GitHub search links
	- Server enum.
	- Google dorking for sensitive files
	- Database enum
	- Waybackurls enum
	- Parameter discovery 
	- Automation vulnerability scanning
	- JS file analysis
	- Backend enum
	- Search Engine
	- discovery(shodan, Spyse, censys, etc)
	- Port scan
	- WAF detection
### Large Scope
- In this type of scopes, you have the permission to test all websites which belong to the main company, for example, you started to test on `IBM` company, so you need to collect all domains, subdomains, acquisitions, and ASN related to this company and treat every domain as medium scope. This type of scopes is the best scopes ever ❤
- ###### all Processes here will be performed on all targets
	- Seeds/Roots
	- ASN to get IP range
	- DNS & SSl enum
	- List of subdomains
	- subdomains takeover 
	- Misconfiguration in storage vuln (s3 buckets)
	- Directory enum
	- GihHub enum
	- Git Hub search links
	- sensitive files
	- Waybackurls enum
	- Parameter discovery
	- automation vulnerability Scanning
	- JS file analysis
	- backend enum
	- Search Engine discovery(shodan, Spyse, censys, etc)
	- Port Scan
	- WAF detection 
	- Database enum
	- Server enum
	- Google dorking 
## Lets see how to collect this!
- #### Simple steps to collect all information
	- we will work here as medium scope to be simple to understand
	-  All the tools used here are free as open source on GitHub
- #### Collect all subdomains
	- tools
		- subfinder 
		- amass
		- crtfinder
		- sublist3r
	-  Use Google dorks for example 
```shell
site:*.example.com
site:example.com -www
inurl:example.com
intitle:"example.com"
site:*.example.com inurl:dev
site:*.example.com inurl:admin
site:pastebin.com "example.com"
site:github.com "example.com"
site:*.example.com -www.example.com
site:*.example.com filetype:txt
site:*.example.com filetype:pdf
site:*.example.com filetype:log
site:*.example.com filetype:xml
site:*.example.com inurl:login
site:*.example.com inurl:signup
site:*.example.com inurl:dashboard
site:*.example.com inurl:config
site:*.example.com inurl:test
site:*.example.com inurl:staging
site:*.example.com inurl:api
intext:"example.com" "subdomain"
intext:"example.com" "server at"
intext:"example.com" "site hosted"
site:example.com -www

```
- collect all these information's from `subdinder + amass + crtfinder + sublist3r + google_dorks` and collect all of them into one text file `all_subdomains.txt`
**[*] Now we have 1 text file contains all subdomains** `all_subdomains.txt`**, let’s continue…**
- ##### filter live subdomains
	- httpx
	- httprobe
		-  `live_subdomains.txt`
**[*] Now we have 2 text files** `all_subdomains.txt + live_subdomains.txt`
- ##### collect all links which related to all live subdomains
	- waybackurls
		- `waybackurls.txt`
**[*] Now we have 3 text files** `all_subdomains.txt + live_subdomains.txt+ waybackurls.txt`
- take all subdomains text file and pass it over `dirsearch` or `ffuf` to discover all hidden directories like `[https://community](https://community).ibm.com/database_conf.txt`
-  collect and filter all the results to show only 2xx, 3xx, 403 response codes from the tool itself (use -h to know how to filter the results)
-  collect all these information's into text file `hidden_directories.txt` and try to discover the leakage data or the forbidden pages and try to bypass them
**[*] Now we have 4 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt`
- pass `all_subdomains.txt` to `nmap` or `masscan` to scan all ports and discover open ports + try to brute force this open ports if you see that this ports may be brute forced, use `brute-spray`to brute force this credentials
- collect all the results into text file `nmap_results.txt`
**[*] Now we have 5 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt + nmap_results.txt`
- use `live_subdomains.txt` and search for credentials in GitHub by using automated tools like `GitHound` or by manual search (I’ll put pretty reference in the references section)
- - collect all these information into text file `GitHub_search.txt`
**[*] Now we have 6 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt + nmap_results.txt + GitHub_search.txt`
- use `altdns` to collect subdomains from subdomains, for example `sub.sub.sub.domain.com`
- As usual :) collect all this info into text file `altdns_subdomain.txt`

**[*] Now we have 7 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt + nmap_results.txt + GitHub_search.txt + altdns_subdomain.txt`
- pass `waybackurls.txt` file over `gf` tool and use `gf-patterns` to filter the links to possible vulnerable links, for example if the link has parameter like `?user_id=` so this link may be vulnerable to **sqli** or **idor**, if the link has parameter like `?page=` so this link may be vulnerable to **lfi**
-  collect all this vulnerable links into directory `vulnerable_links.txt` and into this directory have separated text files for all vulnerable links `gf_sqli.txt` , `gf_idor.txt` ,etc
**[*] Now we have 7 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt + nmap_results.txt + GitHub_search.txt + altdns_subdomain.txt` **and one directory** `vulnerable_links.txt`
- use `grep` to collect all JS files form `waybackurls.txt` as `cat waybackurls.txt | grep js > js_files.txt`
- you can analyze these files manually or use automation tools (I recommend manual scan, see references)
- save all the results to `js_files.txt`
**[*] Now we have 8 text files** `all_subdomains.txt + live_subdomains.txt + waybackurls.txt + hidden_directories.txt + nmap_results.txt + GitHub_search.txt + altdns_subdomain.txt + js_files.txt` **+ one directory** `vulnerable_links.txt`
- Pass `all_subdomain.txt + waybackurls.txt + vulnerable_links.txt` to `nuclei` “Automation scanner” to scan all of them.
## Recommended tools and automation frameworks
 - For the tools

    - [3klector]( https://github.com/eslam3kl/3klector)
    - [crtfinder](https://github.com/eslam3kl/crtfinder)
    - Subfinder https://github.com/projectdiscovery/subfinder
    - Assetfinder https://github.com/tomnomnom/assetfinder
    - Altdns https://github.com/infosec-au/altdns
    - Dirsearch https://github.com/maurosoria/dirsearch
    - Httpx https://github.com/projectdiscovery/httpx
    - Waybackurls https://github.com/tomnomnom/waybackurls
    - Gau https://github.com/lc/gau
    - Git-hound https://github.com/tillson/git-hound
    - Gf https://github.com/tomnomnom/gf
    - Gf-pattern https://github.com/1ndianl33t/Gf-Patterns
    - Nuclei https://github.com/projectdiscovery/nuclei
    - Nuclei-templets https://github.com/projectdiscovery/nuclei-templates
    - Subjack https://github.com/haccer/subjack
- **> For Automation frameworks, I recommend 2 frameworks**
-  `3klcon` [https://github.com/eslam3kl/3klCon](https://github.com/eslam3kl/3klCon) — My own framework and it depends on the upper methodology
- `Bheem` [https://github.com/harsh-bothra/Bheem](https://github.com/harsh-bothra/Bheem)

# let s go Write python and Bash Code For this Code
[[Recon-Project]]
