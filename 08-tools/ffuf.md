### **Basic Usage**

#### Command Structure:

`ffuf -w <wordlist> -u <URL> [options]`

#### Example: Directory Fuzzing

`ffuf -w /path/to/wordlist.txt -u http://example.com/FUZZ`
- `-w`: Path to the wordlist.
- `-u`: Target URL with `FUZZ` as the placeholder.
- `FUZZ`: The keyword `FUZZ` is replaced by words from the wordlist.
    
#### Example: Subdomain Fuzzing
`ffuf -w /path/to/subdomains.txt -u http://FUZZ.example.com`

---

### 4. **Common Options**

- `-w`: Specify a wordlist.
    
- `-u`: Specify the target URL.
    
- `-H`: Add a custom header (e.g., `-H "Authorization: Bearer token"`).
    
- `-X`: Specify the HTTP method (e.g., `-X POST`).
    
- `-d`: Specify POST data (e.g., `-d "username=admin&password=FUZZ"`).
    
- `-fs`: Filter responses by size (e.g., `-fs 1234`).
    
- `-fw`: Filter responses by words (e.g., `-fw 10`).
    
- `-mc`: Match responses by status code (e.g., `-mc 200,301`).
    
- `-t`: Set the number of threads (e.g., `-t 50`).
    
- `-o`: Save output to a file (e.g., `-o results.json`).
    

---

### 5. **Wordlists**

Wordlists are crucial for fuzzing. Some popular wordlists include:

- **SecLists**: A collection of wordlists for security testing.
    
    - Install: `git clone https://github.com/danielmiessler/SecLists.git`
        
    - Example: `/path/to/SecLists/Discovery/Web-Content/common.txt`
        
- **DirBuster**: Wordlists for directory and file discovery.
    
- **Custom Wordlists**: Create your own based on the target.