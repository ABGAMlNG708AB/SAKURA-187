import requests
import time
import os

# ألوان التنسيق (The Architect Palette)
RED = '\033[1;31m'
GREEN = '\033[1;32m'
WHITE = '\033[1;37m'
YELLOW = '\033[1;33m'
CYAN = '\033[1;36m'
RESET = '\033[0m'

def banner():
    os.system('clear')
    print(f"""
{RED}   _____         _    _    _   _____            
{RED}  / ____|  /\   | |  / /  | | |  __ \    /\     
{WHITE} | (___   /  \  | | / /   | | | |__) |  /  \    
{WHITE}  \___ \ / /\ \ | |/ /    | | |  _  /  / /\ \   
{GREEN}  ____) / ____ \| | \ \   | | | | \ \ / ____ \  
{GREEN} |_____/_/    \_\_|  \_\  |_| |_|  \_/_/    \_\ 
                                                
{WHITE}             --- [ {RED}#187{WHITE} ] ---
    """)

def start_spam():
    banner()
    
    # إدخال البيانات المطلوبة
    token = input(f"{WHITE}[+] Enter Your Token: {YELLOW}")
    channel_id = input(f"{WHITE}[+] Enter Channel/DM ID: {YELLOW}")
    message = input(f"{WHITE}[+] Enter Your Message: {YELLOW}")
    speed = float(input(f"{WHITE}[+] Enter Speed (0 for Max): {YELLOW}"))
    
    print(f"\n{RED}[!] SAKURA SYSTEM ACTIVATED...")
    print(f"{RED}[!] TARGETING ID: {channel_id}")
    time.sleep(2)

    url = f"https://discord.com/api/v9/channels/{channel_id}/messages"
    headers = {
        'Authorization': token,
        'Content-Type': 'application/json'
    }
    data = {
        'content': message
    }

    count = 1
    try:
        while True:
            response = requests.post(url, headers=headers, json=data)
            
            if response.status_code == 200 or response.status_code == 204:
                print(f"{GREEN}[{count}] {WHITE}SAKURA SENT: {GREEN}SUCCESS")
            elif response.status_code == 429:
                wait_time = response.json().get('retry_after', 1)
                print(f"{YELLOW}[!] RATE LIMIT! SLEEPING {wait_time}s")
                time.sleep(wait_time)
            else:
                print(f"{RED}[-] ERROR {response.status_code}: {response.text}")
                break
                
            count += 1
            if speed > 0:
                time.sleep(speed)
                
    except KeyboardInterrupt:
        print(f"\n{RED}[!] SAKURA SYSTEM STOPPED.")

if __name__ == "__main__":
    start_spam()
