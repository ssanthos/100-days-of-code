# 100 Days Of Code - Log

### Day 1: August 1, 2018

**Today's Progress**: Setup a slack bot/app (Rick) to track our fines and streaks. Have deployed a working bot. Will add the necessary commands and triggers next.

**Technical Notes**: Using Serverless (https://serverless.com/) app framework to complete manage the remote app. Also, found this boilerplate for a slack app using serverless - https://github.com/johnagan/serverless-slack-app. All it needed are AWS credentials and Slack app tokens. Then all you have to do is write the handler logic for slack commands and webhooks.

**Thoughts:** Literal thoughts - Started off trying to do a Node.js + express. Then, to try something different, decided to host on DigitalOcean. But then thought maybe I should try a managed solution. Finally landed on Serverless which is based on AWS Lambda, which was perfect since I've been longing to try it out.
One of the challenges I face usually is working with software or stacks that I don't understand the inner workings of. Although I understand how AWS Lambda works, I still struggled with grappling with how serverless is putting everything together. But I was able to move on to coding and setting it up with just enough understanding to get me going. #smallwins

**Link to work:** https://github.com/ssanthos/serverless-slack-app
