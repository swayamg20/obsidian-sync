---
author: swayam gupta
---
features in UI:

create agent
update agent
delete agent
test agent (using dialer) (done)
check the call logs/history

update agent-->
have an edit button, click on it then let the user update and then publish it.


{
    "id": 1,
    "name": "HOTEL_NPS",
    "prompt": [
        {
            "role": "system",
            "content": "You are a creative, funny, friendly, and amusing Female AI travel assistant named TARA by ixigo. You are handling a casual conversation over the telephone with users in India so please provide engaging but concise responses, using just 1 or 2 sentences answers and use casual language and words like they do in India. If the user response are in HINGLISH, respond in HINGLISH only. You need to ask the user which language he wants to talk in - ENGLISH or HINDI. Based on that keep the converation in that language only. If the language is HINDI, then give the response in HINDI characters. You are a female Assistant, so if the language is hindi respond as female. Always First, greet the user and tell him that you are TARA from ixigo and would like to know about his experience with the hotel {name}. Ask him if he has a few mins to talk to you about this. Ask only one question at a time. Do not ask two question in one go as this is a call, it will be difficult for the user to answer multiple questions simultaneously. Before any answer, check if the language of the question and respect that. , user's language may switch between Hindi and English or sometimes a blend of both. Switch your language based on the user's preference. If the user communicates in Hindi, continue the conversation in Hindi (mix of Hindi and English, referred as 'Hinglish' in India). NOTE: Always Use English words for hindi like in the sample. If responding in Hindi, then reply as you are female assistant. Some samples - kya aap hotel {hotelName} mai ruke thee? , Thank you feedback dene ke liye, etc."
        },
        {
            "role": "user",
            "content": " If the user wants to hang, say bye and add [DISCONNECT] in the response text. Try to ask more questions to the user to keep the conversation fluid. IMPORTANT: Your objective on this call is to confirm if the user stayed in the hotel he had booked with us or not and get his feedback. You need to collect the Hotel NPS from user. You need to ask the two things from the user - - Did you stay at the hotel {hotelName}? If the answer of this is yes, the ask the second question to get the feedback of the hotel- - On a scale from 1-10, how would you rate the hotel {hotelName}? You can change the way the question is asked. NOTE : If the rating of the above question is less than 9, ask the user to give a reason for such a low rating. IMPORTANT: Ask only one question at a time. In the end, you can ask user if he wants to give any additional feedback. If yes, then ask him to give the feedback. If no, then end the call. If the user says he did not stay at the hotel then get to know the reason of that and then end the call. Also if the user says he does not have time for the call, ask him when can we call him back (maybe tomorrow same time). NOTE: If the user gives a negative feedback, acknowledge it with a small comment on the feedback like if the user say that the hotel bathroom was not great you can give a quick comment like I understand that bad bathroom does not feel good and ruins the experience. Once the user is done with the data, add [DISCONNECT] in the response text to finish the call. In the end, thank the user for his precious time. We are having a casual conversation over the telephone so please provide engaging but concise responses, using just 1 or 2 sentences answers. Avoid numeric list items. IMPORTANT: Your objective on this call is to confirm if the user stayed in the hotel he had booked with us or not and get feedback about his trip and booking. Always just one thing only in a single sentence. My name is {name} maybe you can start with a greeting from ixigo side: 1. Ensure is the right time to talk. If not ask for when. 2. Ensure {name} has stayed on {hotelName} 3. Check NPS on the {hotelName} stay. My hotel details are : Hotel Name - {hotelName} Once captured data, add [DISCONNECT] in the response text to finish the call."
        },
        {
            "role": "assistant",
            "content": "Hello {name}! This is TARA from ixigo. I hope you're doing well. Can I take a few minutes of your time to talk about your experience at the hotel {hotelName}?"
        }
    ],
    "preRequsiteApi": {},
    "postProcessingPrompt": {

![[Screenshot 2024-11-20 at 1.15.41 PM (2).png]]
            "lob": "ai_product_htlnps",
            "botID": 102
        }
    },
    "organisation": null,
    "isActiveFlag": null,
    "updatedBy": null,
    "voiceProvider": "polly",
    "voiceID": "Kajal",
    "llmProvider": "openai",
    "llm": "gpt-4o",
    "transcriberLanguage": "hi",
    "maxCallDuration": 300,
    "callClient": "PLIVO",
    "createdAt": "2024-07-19T07:58:25.000Z",
    "updatedAt": "2024-09-10T09:02:42.000Z"
}

id --> on top
name --> on top
prompt --> model
preReqAPI --> advanced
postProcessingPrompt --> advanced
updatedBy --> ( )
voice Pr

tooltip (at every fields, specifically in )



***things to change:***
UI: change color of save/delete
	add updated by tag
	fix the timer, add call logs button there
add number of calls in last 7 days stats in the agent name button
make the name copy from copy button
provider/model --> make the list
voice: provider > polly > voice id dropdown
	 provider > 11labs > voice id writable
call button (make it a mic)
add ixigo logo
on clicking save, add a dialouge box to review it once


advanced: 
	transcriber lang
	call_client
	post process prompt as JSON
	pre req remove

New agent:
create a new agent just by name
add a button below the prompt to let the user generate a prompt using AI (in a modal)


[[/Users/swayam.gupta/Downloads/image.png]]

![[image.png]]
![image](image.png)

![[image.png]]

[[Dev Voice Bot SDK Frontend]]


Flow change:
prompt: not showing to users
conversation: b/w user and ai
take usecase, (first case, usecase) then generate prompt (save the prompt and show success)

then for additional info

usecase: usecase, extra instruction, context : will take variables


UI improvements
Backend API building


current data:
```
{
  "id": 1,
  "name": "HOTEL_NPS",
  "usecase": [
  "",
  ""
  ],
  "instruction": [
  "",
  ""
  ],
  
  "param": [
	  {
		"key": "string",
		"description": "string"  
	  },
	  {
		"key": "string",
		"description": "string"  
	  }
	]
	

  "prompt": [
    {
      "role": "system",
      "content": "string"
    },
    {
      "role": "user",
      "content": "string"
    },
    {
      "role": "assistant",
      "content": "string"
    }
  ],
  "preRequsiteApi": {},
  "postProcessingPrompt": {
    "callSummarisation": {
      "lob": "ai_product_htlnps",
      "bot_id": 102
    }
  },
  "organisation": null,
  "isActiveFlag": null,
  "updatedBy": null,
  "voice_provider": "polly",
  "voice_id": "Kajal",
  "llm_provider": "openai",
  "llm": "gpt-4o",
  "transcriber_language": "hi",
  "max_call_duration": 300,
  "call_client": "PLIVO",
  "createdAt": "2024-07-19T07:58:25.000Z",
  "updatedAt": "2024-09-10T09:02:42.000Z"
}
```

new fields added:
```
 
"usecase": [
  "",
  ""
  ],
  "instruction": [
  "",
  ""
  ],
  
  "param": [
	  {
		"key": "string",
		"description": "string"  
	  },
	  {
		"key": "string",
		"description": "string"  
	  }
	]
	

```

[[new]]

usecase: array of strings,
instruction: array of strings,
param: array of objects



AI SDK





![[Screenshot 2024-09-25 at 12.09.21 PM.png]]


editing mode: always on

voice: 
voice provider, voice id, transcriber_language,

max_call_duration, call_client



advanced: pre-req, post-req



delete: 
update --> 

isActiveFlag


create new table:

id, 
agent ID (foreign key),
usecases,
params,
instructions,
generated_prompt



![[Screenshot 2024-09-26 at 2.05.37 PM.png]]

```
getAllPromptMeta: async function (req, res, next) {

try {

const prompts = await promptMetaUtility.getAllPromptMeta();

req.data = prompts;

} catch (error) {

console.error(error);

req.displayError = "Failed to retrieve data";

}

next();

return;

},
```



promptMeta table:
now:

structure should be: 

usecase: string
instruction: string
parameter: JSON
generatedPrompt: string

: promptMeta {
[{
usecase. 
}]
}



agent -> add meta -> add them into state, send to db

state: 
usecase: {}



fetch all the rows from that agent Id, sorted in createdAt order:


```

I have a system prompt for a voicebot powered by LLM. Using the user prompt, generate a similar prompt for the given use case.Give the response in the similar format as the sample prompt provided for the user's provided use case. Sample system prompt - You're an AI assistant. \n- IMPORTANT: Your objective on this call is to inform the user that his booking is in pending and we are trying to confirm it with the airline. The user's flight booking is currently in pending state and it should get confirmed in the next 15 mins. The maximum time it can take to confirm the booking is 2 hours from the booking time. You need to assure the user to be patient and once the booking gets confirmed he will get his e ticket on his whatsapp and email. Also, in case the booking the booking fails, the user will get his full refund in the origin source of payment. There is no action required from the user. In case, prices increase by the time user books again, request user to reach out to ixigo customer service team for assistance. Once the user is satisfied and is okay with the information. \n- The user's responses are being transcribed from speech, so expect some mistakes and don't correct them. \n- Use just SHORT sentences answers whenever you are giving information, keep it short you are talking by the phone, use max 20-30 words per answer. If user is frustrated and is showing anger, try to calm user down with information provided succintly. \n- Keep all your responses short and simple. \n- Try to avoid start sentences with an exclamation. this will help you to keep shorter and direct answers. Use simple language. \n- This is a voice conversation, so keep your responses short, like in a real conversation. Don't ramble for too long. \n- Use only the information on this prompt to answer we add more information in the context Information block: \n- Don't repeat yourself. If the user wants to take it in a funny direction, you can go with it. \n- IMPORTANT: If the user wants to hang, or you think that the conversation is finished, say bye and add [DISCONNECT] in the response text. \n\nGuidelines:\n\nBe concise: Provide the necessary information using as few words as possible.\nBe clear: Ensure that the message is straightforward and easy to understand.\nAvoid informalities: Do not use casual greetings, exclamations, or overly friendly language.\nFocus on the customer's need: Quickly address the reason for the communication and what the customer needs to know or do.\n\nContext Information: \n- Name: {name}\n- Booking details: {originDestination}\n- Departure Date: {departDate} \nBooking Amount: {totalFare}




```

```
usecases: ...
instructions: ...
parameters: ...


--> generated Prompt: ....






```



{

}



call logs page UI //done
parsing logic check
clerk roles and permission
download the data in excel form
get last 7 days number of calls per agent

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXevTdnVTjNJsb7ku37MSF3F82OwuR5LOMjKvPgWyVZSA7tNIPo0sdr9Til87vh5Xyt3pgfQn5EGpthCd6PnDaNkmMJT_UNAUqVOB4Bkssx0veJY-zakabmhI1O32y40kB6-iwufdlQiOZy0UoIZXyNiQ-fz?key=-hUryXokPFbz7STU5DMQ_w)**



UI: 
1. llm provider/llm list (pair)
2. voice provider/id list (pair)
3. role/permission management on frontend
4. few templates for creating a new agent (optional)
5. post call data view
6. Settings/Profile page 
7. analytics (dashboard data)
8. last 7 days calls numbers on agent (api)
9. UI improvements. 


status:
summary: block:
	tags: 
	call Summary:
	user sentiments:
	user satisfaction:




{
  "status": "Call analysis successful!",
  "callSummary": {
    "callTags": [
      "Customer Experience",
      "Hotel Services",
      "Maintenance Notification"
    ],
    "callSummary": "The agent, तारा from ixigo, contacted the user to discuss their experience at the hotel. The agent informed the user about scheduled maintenance for the in-head service, which would be interrupted from 2am to 4am local time. The user acknowledged the information and thanked the agent.",
    "userSentimentAnalysis": "Positive",
    "userSatisfactionPercentage": 80
  },
  "transcription": [
    {
      "who": "Agent",
      "message": "hello name this is तारा from ixigo hope you are doing well can i take a few minutes of your time to talk about your experience at the hotel hotel name",
      "timeEnd": 10.98,
      "timeStart": 0.88
    },
    {
      "who": "User",
      "message": "yes tell me",
      "timeEnd": 12.66,
      "timeStart": 11.599999
    },
    {
      "who": "Agent",
      "message": "alright thank you i wanted to inform you about the scheduled maintenance for your in head service it will be temporarily interrupted from 2am to 4am local time",
      "timeEnd": 25.165,
      "timeStart": 14.585
    },
    {
      "who": "User",
      "message": "got it thank you",
      "timeEnd": 28.335,
      "timeStart": 27.035
    },
    {
      "who": "Agent",
      "message": "great you're welcome",
      "timeEnd": 32.27,
      "timeStart": 30.810001
    }
  ]
}




fix download csv data
transcript (on click) reach the timestamp
organization route (id)



orgid: ixigo-voice: org_2mmJzV7jXP9T1QrHV3qbD2uJHVn

Agent: Hi, this is Kate from the appointment department at Retell Health. May I speak with Cindy, please?
User: Yes.
Agent: Great! Cindy, I'm calling to remind you about your annual body checkup coming up on Thursday, April 4th, 2024, at 10am PDT. Are you available at that time?
User: No.
Agent: No worries! You can reschedule your appointment online.
User: Uh, 
Agent: Is there anything else I can help you with today?
User: Uh, no. Can you reschedule the appointment for me?
Agent: I'm sorry, but I can't reschedule appointments directly. Please visit our website to reschedule at your convenience. Is there anything else you need help with?
User: Yeah. What are the functions within the home?
Agent: I'm not sure about that. Do you have any other questions related to your appointment or health checkup?
User: Yes. What kind of algorithm are you using?
Agent: I'm not sure about that either. Do you have any other questions related to your appointment or health checkup?

- [ ] to do [[2024-10-08]]
1. [ ] copy the transcription
2. [x] get call logs into org id %% done %%
3. [x] set base route
4. [ ] on copy, don't open the drawer (call logs) 
5. [x] fix the download csv format
6. [ ] fix the share call logs page
7. [ ] voice tab new UI  
8. [x] if voice is empty then don't throw error %% done %%
9. [x] on agents page, don't open the first agent, let the user select %% done %%
10. [ ] after saving call logs, show it (directly) 
11. [x] user profile page (look good) 
12. [x] pick-a-date (add default dates) %% done %%





bugs and fixes [[2024-11-07]]

- [x] on agent page (both texts ("no agent found" and "loading...") coming together)
- [ ] some UI disturbance in call details slider
- [ ] creating organization/other clerk UI on our own


        "callSummarisation": 
        
        
        
	        ![[Screenshot 2024-11-20 at 1.15.41 PM.png]]

[[2024-11-20]]


![[Screenshot 2024-11-20 at 1.20.05 PM.png]]

analytics should include:

- [ ] NPS/user satisfaction filter

no.
created at
call SID
user phone
user name
duration
parameters (columns)
status
tags
call summary
user sentiment analysis
user satisfactoin
recording URL


# data analytics
filters:
- [ ] userSatisfactionPercentage > 50
- [ ] user sentiment filter
- [ ] call success or not filter
- [ ] duration filters (>0, <10, >30 etc)

analytics:

last 7d, 30d, 60d, current_selected_range:

- [x] total no of calls
- [x] avg duration
- [x] total cost
- [x] ratio of user sentiments
- [x] distribution of userSatisfactionPercentage
- [x] avg user satisfaction %
- [x] sentiment analysis
- [x] tags pie chart
- [x] avg cost per call
- [x] avg cost per second of a call
- [x] total cost


table:
total calls
avg duration
total picked calls
total cost
cost per call
cost per second
average user satisfaction analysis


graph:
picked vs non picked (pie chart)
sentiment (pie chart)
tags (histogram)
user satisfaction distribution (histogram)


```
{
  "totalCalls": 53,
  "pickedCalls": 12,
  "averageDuration": 18,
  "totalCost": "0.00",
  "costPerCall": "0.00",
  "costPerSecond": "0.0000",
  "sentimentAnalysis": {
    "Positive": 3,
    "Neutral": 3
  },
  "tagAnalysis": {
    "Hotel Service Update": 1,
    "Internet Maintenance": 1,
    "Customer Communication": 1,
    "Customer Feedback": 2,
    "Hotel Experience": 2,
    "Customer Experience": 2,
    "Hotel Feedback": 1,
    "Follow-up Call": 1,
    "Hotel Services": 1,
    "Maintenance Notification": 1,
    "Travel Inquiry": 1,
    "Customer Service": 1
  },
  "userSatisfactionAnalysis": {
    "50": 1,
    "70": 2,
    "80": 3
  }
}
```



### Visual Analytics

1. **Call Volume Over Time**
    
    - **Line Chart**: Display total calls over time (daily, weekly, monthly).
    - **Heatmap**: Calls by hour/day to identify peak calling times.
2. **Call Duration Analysis**
    
    - **Histogram/Bar Chart**: Show distribution of call durations, highlighting average and outliers.
3. **Sentiment Analysis**
    
    - **Pie/Bar Chart**: Proportion of positive, neutral, and negative sentiments.
    - **Trends**: Over time to track changes in customer sentiment.
4. **User Satisfaction Score**
    
    - **Gauge Chart**: Average satisfaction percentage.
    - **Trendline**: Satisfaction changes over time.
5. **Call Outcome**
    
    - **Pie/Donut Chart**: Successful vs. unsuccessful analyses or resolved vs. unresolved issues.
6. **Call Topics/Tags**
    
    - **Word Cloud**: Frequently discussed topics/tags like "Customer Feedback" or "Internet Maintenance".
    - **Bar Chart**: Most common tags/topics discussed in calls.
7. **Agent Performance**
    
    - **Bar Chart**: Average call duration or satisfaction score per agent.
    - **Leaderboard**: Top-performing agents by satisfaction score or successful call resolution.
8. **Response Time**
    
    - **Bar/Line Chart**: Average time to respond or resolve customer inquiries.

### Data-wise Analytics

1. **Summary Statistics**
    
    - Average, median, min, max of call duration.
    - Overall sentiment distribution percentages.
2. **Key Performance Indicators (KPIs)**
    
    - Percentage of calls analyzed successfully.
    - Average user satisfaction score per call.
3. **Agent Statistics**
    
    - Calls handled per agent.
    - Average satisfaction and sentiment scores per agent.
4. **Customer Insights**
    
    - Identify repeat callers to understand frequent issues.
    - Cross-reference user sentiment with duration for deeper insight.
5. **Account/Usage Analytics**
    
    - Total number of different users called.
    - Frequency distribution of calls per user.
6. **Service Feedback Loop**
    
    - Highlight recurring issues or topics for product/service improvement suggestions.
7. **Anonymized Call Patterns**
    
    - Patterns in anonymized caller data, like call times and satisfaction scores, could relate to geographic/demographic factors.


tab



```
getAgentCallLogs: async function (req, res, next) {

try {

const {

agentName,

startDate,

endDate,

limit = 10,

offset = 0,

type,

} = req.query;

const { auth } = req;

let whereClause =

auth.orgRole === "org:super_admin" ? {} : { organisation: auth.orgId };

console.log(new Date(startDate), new Date(endDate));

const callLogs = await callLogUtility.getCallLogs({

where: {

agentName: agentName,

...whereClause,

createdAt: {

[Op.between]: [

new Date(startDate),

new Date(

new Date(endDate).setDate(new Date(endDate).getDate() + 1)

),

],

},

},

order: [["createdAt", "DESC"]],

limit: parseInt(limit),

offset: parseInt(offset),

raw: true,

});

  

if (type === "csv") {

const csvData = callLogs.map((log, index) => {

console.log("status:", log || "");

return {

SerialNo: index + 1,

CreatedAt: new Date(log.createdAt)

.toLocaleString()

.replace(/,/g, ""),

CallSid: log.callSid,

UserPhone: functions.piiMask.pii_mask.encrypt(log.userPhone),

Username: log.agentName,

Duration: log.duration,

Status:

log.post_call_data && log.post_call_data.status

? log.post_call_data.status

: "",

UserSentiment:

log.post_call_data && log.post_call_data.call_summary

? log.post_call_data.call_summary.user_sentiment_analysis

: "",

UserSatisfaction:

log.post_call_data && log.post_call_data.call_summary

? log.post_call_data.call_summary.user_satisfaction_percentage

: "",

Tags:

log.post_call_data && log.post_call_data.call_summary

? log.post_call_data.call_summary.call_tags.join(" || ")

: "",

};

});

const headers = Object.keys(csvData[0]);

const csvDataString = [

headers.join(","),

...csvData.map((row) =>

headers.map((header) => row[header]).join(",")

),

].join("\n");

res.setHeader("Content-Type", "text/csv");

res.setHeader("Content-Disposition", 'attachment; filename="data.csv"');

return res.send(csvDataString);

} else {

const maskedCallLogs = callLogs.map((log) => ({

...log,

userPhone: functions.piiMask.pii_mask.encrypt(log.userPhone),

userPhoneUnMasked:

log.userPhone.slice(0, 4) + "xxxxxx" + log.userPhone.slice(-2),

}));

  

maskedCallLogs.forEach((log) => {

const parsedCallData = JSON.parse(log.callData);

if (Array.isArray(parsedCallData) && parsedCallData.length > 0) {

const firstElement = parsedCallData[0];

if (parsedCallData[0].to) {

log.callData = {

from: functions.piiMask.pii_mask.encrypt(

parsedCallData[0].from

),

fromUnMasked:

parsedCallData[0].from.slice(0, 4) +

"xxxxxx" +

parsedCallData[0].from.slice(-2),

request: parsedCallData[0].request,

};

} else {

log.callData = {

from: functions.piiMask.pii_mask.encrypt(

parsedCallData[0].from

),

fromUnMasked:

parsedCallData[0]["0"].from.slice(0, 4) +

"xxxxxx" +

parsedCallData[0]["0"].from.slice(-2),

request: parsedCallData[0]["0"].request,

transcript:

parsedCallData.length > 1 ? parsedCallData.slice(1) : [],

};

}

} else {

log.callData = parsedCallData;

}

});

  

req.data = maskedCallLogs;

}

} catch (error) {

console.error(error);

req.displayError = "Failed to retrieve call logs";

}

next();

return;

},

getAgentAnalytics: async function (req, res, next) {

try {

const { agentName, day = 7 } = req.query;

const { auth } = req;

let whereClause =

auth.orgRole === "org:super_admin" ? {} : { organisation: auth.orgId };

const today = new Date();

const lastWeek = new Date(today.getTime() - day * 24 * 60 * 60 * 1000);

  

const analytics = await callLogUtility.getCallLogs({

where: {

agentName: agentName,

...whereClause,

createdAt: {

[Op.between]: [lastWeek, today],

},

},

attributes: [

[

models.sequelize.fn("COUNT", models.sequelize.col("*")),

"totalCalls",

],

[

models.sequelize.fn(

"COUNT",

models.sequelize.literal(`CASE

WHEN duration IS NOT NULL

THEN 1 END`)

),

"pickedCalls",

],

[

models.sequelize.fn("AVG", models.sequelize.col("duration")),

"avgDuration",

],

[

models.sequelize.fn("SUM", models.sequelize.col("cost")),

"totalCost",

],

[

models.sequelize.fn("SUM", models.sequelize.col("duration")),

"totalDuration",

],

],

raw: true,

});

  

const detailedAnalytics = await callLogUtility.getCallLogs({

where: {

agentName: agentName,

...whereClause,

duration: { [Op.gt]: 0 },

post_call_data: { [Op.not]: null },

createdAt: {

[Op.between]: [lastWeek, today],

},

},

attributes: [

[

models.sequelize.literal(

`JSON_EXTRACT(post_call_data, '$.call_summary.user_sentiment_analysis')`

),

"sentiment",

],

[

models.sequelize.literal(

`JSON_EXTRACT(post_call_data, '$.call_summary.call_tags')`

),

"tags",

],

[

models.sequelize.literal(

`JSON_EXTRACT(post_call_data, '$.call_summary.user_satisfaction_percentage')`

),

"user_satisfaction_percentage",

],

],

raw: true,

});

  

const sentimentMap = {};

detailedAnalytics.forEach((record) => {

if (record.sentiment) {

const sentiment = record.sentiment.replace(/"/g, "");

sentimentMap[sentiment] = (sentimentMap[sentiment] || 0) + 1;

}

});

  

const tagMap = {};

detailedAnalytics.forEach((record) => {

if (record.tags && Array.isArray(record.tags)) {

record.tags.forEach((tag) => {

tagMap[tag] = (tagMap[tag] || 0) + 1;

});

}

});

  

const userSatisfactionMap = {};

detailedAnalytics.forEach((record) => {

if (record.user_satisfaction_percentage) {

userSatisfactionMap[record.user_satisfaction_percentage] =

(userSatisfactionMap[record.user_satisfaction_percentage] || 0) + 1;

}

});

  

req.data = {

totalCalls: parseInt(analytics[0].totalCalls),

pickedCalls: parseInt(analytics[0].pickedCalls),

averageDuration: Math.round(analytics[0].avgDuration || 0),

totalCost: parseFloat(analytics[0].totalCost || 0).toFixed(2),

costPerCall: parseFloat(

(analytics[0].totalCost || 0) / (analytics[0].pickedCalls || 1)

).toFixed(2),

costPerSecond: parseFloat(

(analytics[0].totalCost || 0) / (analytics[0].totalDuration || 1)

).toFixed(4),

sentimentAnalysis: sentimentMap,

tagAnalysis: tagMap,

userSatisfactionAnalysis: userSatisfactionMap,

};

} catch (error) {

console.error(error);

req.displayError = "Failed to retrieve call analytics";

}

next();

return;

},
```


Build time improvement:
	tools: ```npx depcheck```


[[2024-12-16]] (issues)

- [ ] transcript on the log detail page


‭deepgram API KEY:
d81da4176d400b38e14b17c4598f72fe71ec9108    

```

```





