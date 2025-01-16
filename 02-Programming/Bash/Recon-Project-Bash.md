# Tools

## subfinder
- `sudo pacman -s subfinder`
- `subfinder -silent -d target`
## Findomain
- `sudo pacman -S findomain`

## assetfinder
```shell
assetfinder -subs-only target
```
## extracting all subdomain in all file but no تکراری
```shell
cat ./subdomains/*.txt | sort | uniq > all_Subdomains.txt
```
## httpx
- install
	- `go install github.com/projectdiscovery/httpx/cmd/httpx@latest`
	- ##### usage
		- `httpx -l subdomains.txt -o results.txt`
		- ###### check only live domains
			- `httpx -silent -l subdomains.txt -o live_subdomains.txt`
			-
# Scripts
- ## port Scan
```shell
#!/bin/bash
target=`cat ./target`
echo $target
port_baner() {
		
	echo "Scanning ports 1-8000 on $target..."

	for port in {1..8000}; do
		timeout 1 bash -c "echo > /dev/tcp/$target/$port" 2>/dev/null && \
		echo "Port $port is open"
	done

	echo "Scan complete."
	DEFAULT_PORT="80"

	ip=$1
	if [[ "$ip" =~ ^[a-zA-Z]+$ ]]; then
		ip=dig +short ${ip}
	fi

	if [[ -z "${ip}" ]]; then
	  echo "You must provide an IP address."
	  exit 1
	fi

	if [[ -z "${port}" ]]; then
	  echo "You did not provide a specific port, defaulting to ${DEFAULT_PORT}"
	  port="${DEFAULT_PORT}"
	fi

	echo "Attempting to grab the Server header of ${ip}..."

	result=$(curl -s --head "${ip}:${port}" | grep Server | awk -F':' '{print $2}') 

	echo "Server header for ${ip} on port ${port} is: ${result}"
}
port_baner  

```