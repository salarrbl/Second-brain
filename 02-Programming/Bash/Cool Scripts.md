- ### Stopwatch
```shell
#!/bin/bash
now=`date +%s`sec
while true; do
	printf "%s\r" $(TZ=UTC date --date now-$now +%H:%M:%S.%N) 
	sleep 0.1 
	if read -t 0.1 -n 1 input; then
			if [[ $input == "q" ]]; then
				break
			fi
	fi
done
```
- ### Play song with [this site](https://songsara.net)
```bash
#!/bin/bash
if [ -z "$1" ]; then
  echo "Usage: $0 <playlist-url>"
  exit 1
fi
PLAYLIST_URL=$1
HTML_CONTENT=$(curl -s "$PLAYLIST_URL")
AUDIO_URLS=$(echo "$HTML_CONTENT" | grep -oP 'https?://[^"]+\.(mp3|m4a)')
if [ -z "$AUDIO_URLS" ]; then
  echo "No audio files found on the page. Check the URL or update the regex pattern."
  exit 1
fi
echo "Playing playlist..."
echo "$AUDIO_URLS" | while read -r AUDIO_URL; do
  echo "Playing: $AUDIO_URL"
  mpg123 "$AUDIO_URL"
done
```
- ### Alarm Create by [Woland](https://github.com/wolandark)
```bash
#!/usr/bin/env bash 
if [[ -z $1 ]]; then
	echo -e "\n\t Usage: ./Alarm.sh 8h for 8 hours of sleep"
	echo -e "\t\t./Alarm.sh 20m for 20 minutes of sleep"
	echo -e "\t\t See man sleep\n"
	exit 0
fi

sleep "$1";
figlet "sleep time over"

alarm=(
	"/home/rebel/Music/alarm1.mp3"
	"/home/rebel/Music/alarm2.mp3"
	"/home/rebel/Music/alarm3.mp3"
	"/home/rebel/Music/alarm4.mp3"
	"/home/rebel/Music/alarm5.mp3"
)

for ((i=0; i<${#alarm[@]}; i++)); do
  figlet -f slant "Wake Up-$((i+1))"
  sleep 1; mpv --no-audio-display --no-resume-playback "${alarm[i]}" &
  sleep 45; killall mpv
  sleep 5m;
done
```
- ### [subfinder](https://github.com/salarrbl/webH-tools/tree/master/subfinder)
```bash
#!/bin/bash
RESET='\033[0m'
GREEN='\033[32m'
# Check if domain is provided as an argument
if [ $# -ne 1 ]; then
    echo "Usage: $0 <domain>"
    exit 1
fi
figlet -f slant RSubfinder 
DOMAIN=$1

echo "Searching for subdomains of $DOMAIN..."
cat ./subs.txt | while read subs; do
	# echo $subs
	FULL_SUBDOMAIN="${subs}.${DOMAIN}"
	# echo $FULL_SUBDOMAIN
	 IP=$(dig +short $FULL_SUBDOMAIN)
    
    if [ ! -z "$IP" ]; then
        echo -ne "${GREEN}Found: $FULL_SUBDOMAIN -> $IP ${RESET}\n"
    fi

done
echo "Search complete."
```

# copy text's in a file
```shell
#!/bin/bash
file="~/Codes/toekn"
if [ ! -f "$file" ]; then
    echo "file not exsits"
    exit 1
fi
xclip -selection clipboard < "$file"
```
# copy text in image
```shell
#!/usr/bin/env bash
ocr_dir="$HOME/ocr"
if [[ ! -d "$ocr_dir" ]]; then
    mkdir -p "$ocr_dir"
fi
takeshot() {
    spectacle -r -o -b -n "$ocr_dir/ocr.png"
}
convert() {
    tesseract "$ocr_dir/ocr.png" "$ocr_dir/ocr" > /dev/null 2>&1
}
sendToClipboard() {
    xsel -b < "$ocr_dir/ocr.txt"
}
takeshot
convert
sendToClipboard
echo "Text extracted and copied to clipboard."
```

