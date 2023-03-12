
#!/bin/bash

TOKEN="XXXXXXXXXXXXTOKENXXXXXXXXXXXXXXXXXXX"
ID="----ID-----"
MESSAGE=$1
URL="https://api.telegram.org/bot$TOKEN/sendMessage"

curl -s -X POST $URL -d chat_id=$ID -d text="$MESSAGE"


./bash.sh "Esto es una prueba";
