# Discord No Auth 0day | User Full Account Take-Over
A write up on a Discord No-Auth 0day also known in the community as "Otax"

```
                          
.--.        .--.          
|__|        |__|          
.--..-,.--. .--.          
|  ||  .-. ||  |          
|  || |  | ||  |     _    
|  || |  | ||  |   .' |   
|  || |  '- |  |  .   | / 
|__|| |     |__|.'.'| |// 
    | |       .'.'.-'  /  
    |_|       .'   \_.'   
                          
```

## Basic Knowledge
Discord is a VoIP, instant messaging and digital distribution platform designed for creating communities. Users communicate with voice calls, video calls, text messaging, media and files in private chats or as part of communities called "servers". Servers are a collection of persistent chat rooms and voice chat.

### Details
**Severity**: High

**Exploit Date**: June 26, 2016

**Public**: Augest 22, 2021

### Vulnerability Scope
This vulnerability affects all Discord accounts.
2fa does not affect the attackers side.

### Description
A malicious attacker can log on using any account to any Discord servers’ official authentication servers to verify user authenticity. This can allow an attacker to gain access to users’ accounts, or allow an attacker to gain access to a privileged account in Discord servers'. Depending on common server modifications, privileged accounts could be used to acquire access to the operating system, or cause serious damage, which includes but is not limited to:

  * Server Nuking
  * Account Take Over
  * Full Admin Access
  * Email & Phone Leak
  * Credit & Payment Method Leaks

# The Discord Authentication

### Basic Authentication
Discord uses a token based authentication system, the token is stored in the headers or every request.
Your user token is how the Discord app authenticates you as it interacts with Discord's backend servers, so you don't have to enter your username and password every time you want to send a message or make a request.
It will give anyone almost unlimited access to your account.

### Gateways Affected
```json
GatewayGuildBanModifyDispatch
GatewayGuildDeleteDispatch
GatewayGuildEmojisUpdateDispatch
GatewayGuildIntegrationsUpdateDispatch
GatewayGuildMemberAddDispatch
GatewayGuildMemberRemoveDispatch
GatewayGuildMembersChunkDispatch
GatewayGuildMemberUpdateDispatch
GatewayGuildModifyDispatch
GatewayGuildRoleDeleteDispatch
GatewayGuildRoleModifyDispatch
GatewayGuildStickersUpdateDispatch
GatewayUserUpdate
GUILD_MEMBER_UPDATE
CHANNEL_PINS_UPDATE
GatewayVoiceStateUpdateDispatch
```

# Reproduction of this vulnerability
This vulnerability seems to be caused by a leakage of user headers in the POST Request Session, the token was returned when a friend request was sent 
to an user.

To reproduce this issue an attacker needs to follow the following steps.

   1. Log all trafic on the current network.
   2. Have a user send a friend request
   3. Wait for the request to be logged
   4. Look for a request to *https://discord.com/api/v7/science*
   5. Look in the JSON data for a base64 encoded string under the param **AUTH**
   6. Decode the Base64 string to see the token

### Resolution
This vulnerability was patched on March 21st of 2020.

### Contact Us
Any requests for information, questions, or comments regarding this advisory should be forwarded to hellsec@fbi.ac
