# 100 Days Of Code - Log

### Day 1: August 1, 2018

**Today's Progress**: Setup a slack bot/app (Rick) to track our fines and streaks. Have deployed a working bot. Will add the necessary commands and triggers next.

**Technical Notes**: Using Serverless (https://serverless.com/) app framework to complete manage the remote app. Also, found this boilerplate for a slack app using serverless - https://github.com/johnagan/serverless-slack-app. All it needed are AWS credentials and Slack app tokens. Then all you have to do is write the handler logic for slack commands and webhooks.

**Thoughts:** Literal thoughts - Started off trying to do a Node.js + express. Then, to try something different, decided to host on DigitalOcean. But then thought maybe I should try a managed solution. Finally landed on Serverless which is based on AWS Lambda, which was perfect since I've been longing to try it out.
One of the challenges I face usually is working with software or stacks that I don't understand the inner workings of. Although I understand how AWS Lambda works, I still struggled with grappling with how serverless is putting everything together. But I was able to move on to coding and setting it up with just enough understanding to get me going. #smallwins

**Link to work:** https://github.com/ssanthos/serverless-slack-app

### Day 2: August 2, 2018

**Today's Progress**: Got the bot to respond to mentions on a channel. Got a better understanding of what the  serverless-slack ‘micro-framework’ does. Also, got a better understanding of different slack app platform.

**Technical Notes**: Events API helps with starting a conversation (via ‘app_mention’ event) and once store this in db, we should listen to messages in that channel by the same user (via ‘messages.channel’ ) to respond appropriately. Think of it like a support ticket raised.

**Thoughts:** Spent most of the time debugging why the bot was not able to respond. Was stuck with ‘not_authed’ error for a long time when trying to reply. Finally found the problem after almost an hour. But got to dig deeper and understand better about the app platform itself in the process. 
Earlier, I was trying to build a workflow involving the bot which was less text and more action/command oriented. Now, planning to take it step by step and trying to build a more simple conversational style since it involves less of exploring slack APIs and more interesting logic pieces. Want to get a v1 of the workflow by tomorrow that let’s a user ‘register’ with the challenge, and ‘update’ progress each day. Post that, will add non-user initiated things like reminders, fine and streak announcements. 
