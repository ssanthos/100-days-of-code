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

### Missed: August 3, 2018

### Day 3: August 4, 2018

**Today's Progress**: Bot now responds to ‘help’ private message and gives a bunch of interactive options like ‘Register’, ‘Peek into pot’, ‘Post my progress’. Also got a ngrok + serverless-offline (serverless plugin) setup to work to cut down on development cycle time significantly

**Notes**: Struggled with 3 things today. 
* The time to deploy was just too much (about 20 seconds to test every each code change). Found a serverless-offline plugin which basically simulates the API gateway and AWS Lambda to an extent. Since Slack needs to hit these urls, used ngrok to tunnel through a public url. 
* Was unable to trigger the message handler upon dm or public channel message. Turned out to be misinterpretation of the API on my part. Had thought that event permissions  like ‘message.im’ and ‘message.channels’ are event keys as well. The key is just ‘message’. Once I hooked that up, it worked as expected.
* Was unable to responds with attachments. Dug deeper into serverless-slack code and found that the message object passed bot.reply or other methods is converted into a query string and that meant values themselves should be converted to JSON upfront. Did that in my code and got that working.
Overall, still getting acquainted with some quirks here and there. But time spent on getting to know some knew stuff should be useful. But should focus on functionality tomorrow.

### Day 4: August 5, 2018

**Today’s Progress**: Got click on actions to call callback handler. Made some progress on the register sequence.

**Notes**: 
* Spent about 30 mins debugging why clicking an action button wasn’t calling my registered handler. After some hair-pulling, the issue turned out to be the `serverless-offline` plugin when the request uses `application/x-form-url-encoded` params rather than JSON. Solution : Screw offline plugin. Too much down the rabbit hole. Back to pure `serverless deploy.` SLS_DEBUG=1 helps a lot when trying to understand such issues
* Was thinking of storing data per user since I thought serverless-slack was exposing a pure key/value store. But it seems a little unconventional. The `get` seems to be a lookup by key, but the `save` seems to be saving all data (not per key/record). Highly unlikely the underlying `DynamoDB` works this way, so maybe a restriction on `serverless-slack` package. Will dig deeper.
* Found a cool library https://github.com/wanasit/chrono to parse dates typed out in a chat by user (not adhering to any formats). Yet to try it out well.

Making slow progress. Trying new stuff definitely comes with overhead when things don’t work as expected. Should get better at doing course correction in time.

### Day 5: August 6, 2018

**Today’s Progress**: Explored `botkit` (https://github.com/howdyai/botkit), which is a much more solid bot framework than `serverless-slack`. Setup a new bot based on botkit. Still going through some documentation and sample code.

**Notes**: `serverless-slack` was a nice way to understand how to use `serverless` to build infrastructure-less apps. But one critical aspect it lacks when it comes to building a bot is any help with conversation management. This is where `botkit` is supposed to shine since it’s API is built to mainly enable bot conversations. Also includes plugin/middleware for NLP. I want to find out how fast I’m able to implement using `botkit` compared to `serverless-slack`.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Day 6: August 7, 2018
**Today’s Progress**: Added a flow to register a user via a conversation with the bot. Currently just stores a nick name and favourite color.

**Notes**: Botkit is a awesome! I started with a sample provided by them and modified it to so that you can say ‘register’ to the bot and it asks you 2 questions one after the other (name and favourite color) and stores them under user data. Can be queried later. Also added an ‘erase’ trigger to help with iterating. It’s amazing how easy botkit has made all this stuff. I just read some 30 mins of docs that’s was pretty much it, I could then focus directly on conversations I wanted to build. Tomorrow, planning to add flow to collect user’s 100daysofcode repo url and oauth flow so that the bot can update the progress log by itself.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Day 7: August 8, 2018
**Today’s Progress**: Added more questions to the flow to register user with the challenge. Also explored ways to store dates and date ranges using moment. and moment-range library.

**Notes**: Bot now asks date you started the challenge and dates you missed in between and github url. Moment.js is excellent for working with dates. To keep track of date ranges, I’m using a library called moment-range. What I need is to keep track of every day progress etc., so this library will help query and iterate over ranges etc. 

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Day 8: August 9, 2018
**Today’s Progress**: Learnt about ES6 generators and how to use them to implement async flows with less syntax.

**Notes**: Was trying to write bot conversation code better. Currently every question asked involves one level on nested callback. Yesterday, I followed a pattern of defining each question in the conversation as it’s own function based on one of the examples. Today, I came across this comment - [Stopping a conversation immediately after saying something silences previous message · Issue #149 · howdyai/botkit · GitHub](https://github.com/howdyai/botkit/issues/149#issuecomment-298939419)
It links to code that uses ES6 generators as ‘stateful conversations’. https://github.com/taxigy/slackbot-starter/
From there, I went off tangentially learning about ES6 generators. I’d always thought I’d never have to learn them since async/await would make them unnecessary. But I found this excellent 4-part blog series explaining the concept really well enough to make it interesting. [The Basics Of ES6 Generators](https://davidwalsh.name/es6-generators) . The author is David Walsh, his posts on promises are awesome as well.
After this, tried implementing one of my bot conversation flow using generators. Not working yet.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Day 9: August 10, 2018
**Today’s Progress**: Implemented register conversation flow using es6 generator rather than callbacks. 
Tried out Typescript.

**Notes**: Conversation using generator is a thing of beauty! The mechanism seems pretty powerful. Wrote the sequence of sequence and bot messages in a single generator function which isn’t concerned with bot or conversation objects. It only yields objects with details about the questions. I isolated everything else completely into a runner function. Lot of boilerplate can be isolated into the runner this way, while keeping it generic. Writing any conversation then becomes just a matter of yielding a bunch of strings and objects with the runner taking care of validating, transforming, aborting, repeating questions, aborting and all that stuff. 
All this is super cool, but still yet to see what the advantages of using generators over async/await might be.

Came across Typescript searching for something around validation, so decided to try it out. Was able to try compiling TS to JS and see what the transpilation looks like for basic classes and interfaces. Planning to move to using Typescript over the next couple of days

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Missed: August 11, 2018

### Day 10: August 12, 2018
**Today’s Progress**: Implemented multiple conversations using the same generator mechanism. Started working on github oauth flow.

**Notes**: Implemented ‘test’ and ‘erase’ commands that use storage apis and added options to do that from the generator. 
The github oauth part is a bit tricky to work into the conversation. So far, the botkit based conversations I’ve built are purely back and forth with a clear end of conversation. To implement the github oauth flow. I need to ask the user to open a url in the browser and provide a callback URI. With the callback, I’ll get the token, but don’t have a way yet to bring that into the current conversation the bot is having the user. Will need to see if botkit apis make this easier. If there isn’t a good way to do that, I can prompt the user to tell the bot that the authorization is done.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Missed: August 13, 2018

### Day 11: August 14, 2018
**Today’s Progress**: Got the github oauth flow working. Need to integrate that with the current conversation.

**Notes**: Github oauth flow works like this - open the authorize url in the user’s browser. Will need the client id for this step. I’m setting the scope to ‘public_repo’ since the 100daysofcode repo will be public. Once the user authorizes, the browser will post the a `code` to the callback uri. I make a request from the app to github with my client id and secret to get an `access_token` for the user. There is also a parameter called `state` we can use to prevent CSRF. To generate CSRF tokens, I used a package called `csrf`.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Day 12: August 15, 2018
**Today’s Progress**: Integrated github auth flow into the register conversation

**Notes**: While generator functions and the concept is cool, writing a generator executor is a lot of boilerplate. Should probably use a library like `co` which helps to reduce a lot of that. But based on my experience with it so far, `async/await` seems a better suited option. `async/await` is really a special case of generators but easier to write. Will think about generators when there is something else possibly more suited. Moving to `async/await` tomorrow.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)

### Missed: August 16, 2018
### Missed: August 17, 2018
### Missed: August 18, 2018
### Missed: August 19, 2018
### Missed: August 20, 2018
### Missed: August 21, 2018
### Missed: August 22, 2018
### Missed: August 23, 2018
### Missed: August 24, 2018

### Day 13: August 25, 2018
**Today’s Progress**: Moved to async/await from generators. Also got reacquainted with the code.

**Notes**: `async/await` is a special case of generators. And frankly, generators are too much pain for very little gain. Also, I’ll be making more feature wise progress over the next couple of days rather than trying to figure out ways to write better code. A sense of completion is long overdue.

**Link to work**: [GitHub - ssanthos/rick-botkit](https://github.com/ssanthos/rick-botkit)
