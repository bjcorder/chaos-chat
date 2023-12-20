# Chaos-Chat

Chaos-Chat is a simple web chat application designed to be as chaotic as possible. There are no separate channels and no direct messaging. All chat messages are visible to all users all the time.

## Prerequisites

### PocketBase

Chaos-Chat relies on PocketBase for backend authentication and message storage functionality. As such, a configured PocketBase instance is required to get this application up and running.

If using a PocketBase instance hosted on a separate server from Chaos-Chat, please modify the connection information in /src/lib/pocketbase.ts


