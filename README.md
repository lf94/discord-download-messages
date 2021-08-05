```bash
#!/bin/sh
# Download messages from Discord
#
# Usage: discord-download-messages.sh session-id channel-id from-message-id to-message-id
# 
# You can get a position to start and end from by using "Copy Message Link", and using
# the channel id and message id from that URL.
#
# Obtain a session id from the browser, in the Authorization header.
# 

SESSION_ID="$1"
CHANNEL_ID="$2"
START_MESSAGE_ID="$3"
END_MESSAGE_ID="$4"

MESSAGE_ID="$START_MESSAGE_ID"
while :; do
  URL="https://discord.com/api/v9/channels/$CHANNEL_ID/messages?before=$MESSAGE_ID&limit=50"
  JSON=$(curl --header "Authorization: $SESSION_ID" "$URL")
  
  MESSAGE_ID=$(echo "$JSON" | jq -r '.[-1].id')
  OUTPUT="$OUTPUT""\n"$(echo "$JSON" | jq -r '.[] | [.author.username, .content] | join(": ")')
done

echo "$OUTPUT" | tac
```
