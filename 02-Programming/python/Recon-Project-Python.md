# library
```python
import sys
import subproxess
import os
```
# Step One
- ## Collect all Subdomains
	- #### with `subfinder`
```python
# Get the target from command-line argument
target = sys.argv[1]

# Run Subfinder
result = subprocess.run(['subfinder', '-d', target, '-silent'], stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

# Storing the result
output = result.stdout
error = result.stderr
if (output):
    with open('all_subdomains.txt', 'a') as file:
        file.write(output)
if (error):
    print("Error:", error)

```
- ##### `target = sys.argv[1]`
	- give target in terminal
- ##### `result = subprocess.run(['subfinder', '-d', target, '-silent'], stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)`
	- Run subfinder on target and redirects the standard output (stdout) of the command to a pipe
###  function for extract all subdomain with all file and give all_Subdomain

```python
def all_Subdomains():
    # result_all = subprocess.run(['cat ./subdomains/*.txt | sort | uniq > all_Subdomains.txt'])
    files = glob.glob('./subdomains/*.txt')
    #my command
    command = 'cat ' + ' '.join(files) + ' | sort | uniq > ./subdomains/all_Subdomains.txt'
    result = subprocess.run(['bash', '-c', command], stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

```
### function for test live subdomains
```python
def live_subs():
    print("extracting Live Subdomains") 
    direc_subs = './live_subs'
    if os.path.exists(direc_subs):
        print('')
    else:
        subprocess.run(['mkdir' , 'live_subs'])

    #httpx -silent -l all_Subdomains.txt -o live_subdomains.tx
    subprocess.run(['httpx', '-silent', '-l', './subdomains/all_Subdomains.txt', '-o', './live_subs/live_subdomains.txt'], stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

```
