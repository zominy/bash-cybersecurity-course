# 📖 List of Commands used in Module 10: API-Driven Security Monitoring

---

## Setup ⚙️

For this module, we’ll interact with an external API: AbuseIPDB. It allows us to check if an IP address has been reported for malicious activity.

Before diving in, we need a few dependencies to parse and process the data returned by the API. Make sure your environment has the following installed:

```bash
sudo apt update; sudo apt upgrade -y
```
Updates the local package index and upgrades all installed packages.

```bash
sudo apt install -y curl jq
```
`curl`: Command-line tool to send HTTP requests.

`jq`: A lightweight and flexible command-line JSON processor.

---

## curl -s -G https://api.abuseipdb.com/api/v2/check 🔎🌐

Explanation: This is the foundational command to query AbuseIPDB for information about a specific IP address.

```bash
curl -s -G https://api.abuseipdb.com/api/v2/check \
--data-urlencode "ipAddress=1.1.1.1" \
-H "Key: <YOUR_API_KEY>" \
-H "Accept: application/json"
```
Breakdown:

`-s`: Silent mode – suppresses progress and error messages.

`-G`: Forces curl to send the --data-urlencode parameters as a GET request.

`--data-urlencode`: Encodes and appends parameters to the URL query string.

`-H`: Adds headers to the request:

`"Key: ..."`: is your API key for authentication. (**What is an API key and what does it do?**: An API key is like a password used to authenticate your access to an external service. In this case, AbuseIPDB. It tells the server who you are and that you're allowed to make requests. Without a valid API key, the service won’t respond with useful information, or might block your request entirely.)

`"Accept: application/json"`: specifies that we expect JSON-formatted output.

##### Note: Backslashes at the end of a line let you split a long command across multiple lines in Bash. They tell the shell that the command continues on the next line, improving readability without running the command prematurely. You COULD use `;` instead of `\` but you sacrifice readability, take a look for yourself (using a fake API key):
```bash
curl -s -G https://api.abuseipdb.com/api/v2/check; --data-urlencode "ipAddress=1.1.1.1"; -H "Key: f135f70fc93bf6b9f77d9d53de404e72675f9b195441049777d5a5cfb55cc3701aec0dbea66d1d84"; -H "Accept: application/json"
```

#### 🔐 Important: Always keep your API key safe and never share it publicly. (The key that was used in the video has been fully revoked prior to uploading the video and poses no security risk to me.)

---

## curl ... | jq -r '.data.abuseConfidenceScore' 🧪📊

Explanation: This command filters the API's JSON response and extracts the Abuse Confidence Score, which is a number from 0 to 100 indicating how risky an IP address is.

```bash
curl -s -G https://api.abuseipdb.com/api/v2/check \
--data-urlencode "ipAddress=1.1.1.1" \
-H "Key: <YOUR_API_KEY>" \
-H "Accept: application/json" \
| jq -r '.data.abuseConfidenceScore'
jq -r '.data.abuseConfidenceScore': Grabs just the score and outputs it in raw text format (without quotes).
```

Further Explanation:

The output from curl is a big JSON object with lots of information (ISP, country, domain, report history, etc).

We pipe (`|`) this output into `jq`, a powerful tool designed to work specifically with JSON data.

`.data.abuseConfidenceScore` tells `jq` to navigate into the data object and grab the abuseConfidenceScore field specifically.

The `-r` flag tells jq to return just the raw number, without quotes, formatting, or extra JSON. This is really useful for scripting, because you can store it in a variable or use it in comparisons.

This makes it perfect for use inside scripts where we need to evaluate the score numerically.

---

# threat_scan.sh 🧠📜

Here is the script we wrote and used in this module:

```bash
#!/bin/bash

if [ -z "$1" ]; then
        echo "Error: No IP Given"
        echo "Usage: $0 <IP_ADDRESS>"
        exit 1
fi

IP="$1"

SCORE=$(curl -s -G https://api.abuseipdb.com/api/v2/check \
--data-urlencode "ipAddress=$IP" \
-H "Key: <YOUR_API_KEY>" \
-H "Accept: application/json" \
| jq -r '.data.abuseConfidenceScore // empty')

if [ -z "$SCORE" ]; then
        echo "Failed to retrieve score from AbuseIPDB"
        exit 1
fi

echo "Abuse Confidence Score for $IP: $SCORE"

RED='\033[0;31m'
YELLOW='\033[0;33m'
GREEN='\033[0;32m'
NC='\033[0m'

if [ "$SCORE" -gt 75 ]; then
        echo -e "${RED} RED ALERT: IP IS VERY DANGEROUS.${NC}"
elif [ "$SCORE" -gt 50 ]; then
        echo -e "${YELLOW} WARNING: THIS IP IS SUSPICIOUS.${NC}"
elif [ "$SCORE" -gt 0 ]; then
        echo -e "${YELLOW} RISKY IP: KEEP AN EYE ON THIS IP.${NC}"
else
        echo -e "${GREEN} THIS IP IS SAFE.${NC}"
fi
```

**What this script does**:

- *Input Validation*:

If no IP address is passed to the script, it exits with a helpful usage message.

```bash
if [ -z "$1" ]; then
    echo "Error: No IP Given"
    echo "Usage: $0 <IP_ADDRESS>"
    exit 1
fi
```
- *Storing the Input IP:*

The first command-line argument is saved as the target IP.

```bash
IP="$1"
```
- *API Query & Score Extraction:*

This block sends a request to AbuseIPDB and extracts the abuseConfidenceScore from the JSON response using `jq`.

```bash
SCORE=$(curl -s -G https://api.abuseipdb.com/api/v2/check \
--data-urlencode "ipAddress=$IP" \
-H "Key: <YOUR_API_KEY>" \
-H "Accept: application/json" \
| jq -r '.data.abuseConfidenceScore // empty')
```
Also:

.data.abuseConfidenceScore tells `jq` to pull just the score from the response.

The `// empty` part is a fallback. If the field is missing (due to an invalid IP or API error or something like that), it returns an empty string instead of "null" or an error which makes it easier to check and handle in the next step.

This makes the variable `SCORE` either a clean number like 94 or an empty string ("").

Without `// empty`, you might get "null" back from jq, which could confuse your script or trigger false positives.

- *Error Handling for API Response*

If the score wasn't retrieved successfully (invalid IP or API failure), the script exits.

```bash
if [ -z "$SCORE" ]; then
    echo "Failed to retrieve score from AbuseIPDB"
    exit 1
fi
```
- *Print the Score*

Shows the abuse confidence score for the given IP pre-ranking.

```bash
echo "Abuse Confidence Score for $IP: $SCORE"
```
- *Define Color Codes for Output*

Sets up ANSI escape sequences for coloured terminal output.

```bash
RED='\033[0;31m'
YELLOW='\033[0;33m'
GREEN='\033[0;32m'
NC='\033[0m'
```
- *Score Evaluation and Risk Assessment*

Based on the score, the script prints different coloured warnings or a safety message.

```bash
if [ "$SCORE" -gt 75 ]; then
    echo -e "${RED} RED ALERT: IP IS VERY DANGEROUS.${NC}"
elif [ "$SCORE" -gt 50 ]; then
    echo -e "${YELLOW} WARNING: THIS IP IS SUSPICIOUS.${NC}"
elif [ "$SCORE" -gt 0 ]; then
    echo -e "${YELLOW} RISKY IP: KEEP AN EYE ON THIS IP.${NC}"
else
    echo -e "${GREEN} THIS IP IS SAFE.${NC}"
fi
```
`-gt` means 'greater than' in Bash. So:

`"$SCORE" -gt 75` simply means if the score is greater than 75.

The script checks from highest to lowest, so it catches the most dangerous first.

`${RED}`, `${YELLOW}`, and `${GREEN}` insert ANSI escape codes that tell the terminal to change the text colour.

`${NC}` stands for No Color. It resets the terminal back to normal color after the warning message. Without it, everything printed afterward would stay coloured, which could get messy.

##### 📘 Order matters: The script checks conditions in sequence; it doesn’t go further once one is true.

Very nice.
