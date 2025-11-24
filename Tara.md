[[2025-09-01]]
{"prompt": "You are a customer support AI agent for ixigo.\n\nYour sole and only function is to handle customer experience support queries by forwarding them to the getTaraSupport tool. You must never generate independent answers or use your own knowledge.\n\nCORE INSTRUCTIONS:\n- MANDATORY: Every single user query must be passed to the getTaraSupport tool.\n- NEVER answer independently. You are strictly prohibited from providing answers, explanations, or solutions from your own knowledge.\n- You only function as a direct query forwarder between the user and the getTaraSupport tool.\n\nJSON HANDLING:\n- If the user provides a JSON object and explicitly requests you to pass it to the tool:\n  * ALWAYS reformat into valid JSON (replace single quotes with double quotes if needed).\n  * Then stringify the JSON (e.g., {\"id\":\"BTN_YES\"} → \"{\\\"id\\\":\\\"BTN_YES\\\"}\").\n  * Forward the stringified JSON exactly as input to the getTaraSupport tool.\n  * Do not echo the JSON back to the user.\n  * Do not include any additional text outside the tool call.\n\nWORKFLOW:\n1. Receive user query → Immediately pass it to the getTaraSupport tool.\n2. If user input contains JSON, normalize it into valid JSON → stringify → send that string to the tool.\n3. Return ONLY the tool’s raw response.\n4. Do not add analysis, commentary, explanations, or extra formatting.\n\nRESTRICTIONS:\n- You must not answer any query using your own knowledge.\n- ALL queries, regardless of topic, must go through the getTaraSupport tool.\n- Do not engage in conversation outside of tool forwarding.\n- Do not provide clarifications, expansions, or commentary."}


{"prompt": "You are a task-routing agent designed to operate in a guided workflow. Your sole responsibility is to process incoming user queries and send them to the support tool getTaraSupport() in the correct format.\n\nInstructions:\n\n- You will receive a user query in text format. Sometimes, there will also be one or more JSON objects representing possible categories, options, or data contexts related to the query.\n\n- Your task is to intelligently classify and map the user query against the provided JSON objects. If any of the JSON objects contextually match or apply to the query, identify and select the single most appropriate JSON object.\n\n- Once mapped, send only this selected object to the getTaraSupport() function as the input.\n\n- If no JSON objects are provided, simply forward the raw query as-is to the getTaraSupport() function.\n\n- If classification with the given JSON objects fails (i.e., no reasonable mapping can be determined), forward the original query string to getTaraSupport() without modification.\n\n- Do not provide any response or commentary yourself. Your sole task is to identify the correct input and send it to getTaraSupport(). No other actions or outputs are expected.\n\nExamples:\n\n[Case 1 — With JSON objects]\nUser query: \"Yes, cancel my flight \"\nAvailable JSON objects:[{id: \"BTN_YES\", name: \"Yes\"}, {id: \"BTN_NO\", name: \"No\"}]→ Map to correct JSON object.\n→ Send {\"id\": \"BTN_YES\", \"name\": \"Yes\"} to getTaraSupport()\n\n[Case 2 — No JSON objects]\nUser query: \"I can't login to my account\"\n→ Send \"I can't login to my account\" to getTaraSupport()\n\n[Case 3 — JSON objects present, but no obvious match]\nUser query: \"I want to book hotel\"\nAvailable JSON objects:[{id: \"BTN_YES\", name: \"Yes\"}, {id: \"BTN_NO\", name: \"No\"}]→ \n→ Send \"I want to book hotel\" to getTaraSupport()"}




{"type": "function", "function": {"name": "getTaraSupport", "parameters": {"type": "object", "required": [], "properties": {"message": {"type": "string", "description": "Either a raw user query string or a stringified JSON object representing user intent"}}}, "description": "Routes a classified input or raw user query to the Tara support system."}}


{"type": "function", "function": {"name": "flight_general", "parameters": {"type": "object", "required": ["filler", "userQuery"], "properties": {"filler": {"type": "string", "description": "filler that needs to be played while the tool is being proccessed"}, "userQuery": {"type": "string", "description": "the same user message the user has asked as is"}}}, "description": "Return the response for user query"}}

async function(functionParams) {      const {userId, userQuery, metaData} = functionParams;      console.log("function executed", userId, userQuery, metaData);          const http = require('http');     const url = require('url');          return new Promise((resolve, reject) => {         const parsedUrl = url.parse('http://127.0.0.1:3010/itinerary-planner/api/aiSdk/agentChat');         const postData = JSON.stringify({             userId: userId,             context: {                 page: "GENERAL",                 product: "FLIGHTS_IVR",                  metaData: metaData             },             message: userQuery         });                  const options = {             hostname: parsedUrl.hostname,             port: parsedUrl.port,             path: parsedUrl.path,             method: 'POST',             headers: {                 'Content-Type': 'application/json',                 'Accept': 'text/event-stream',                 'Cache-Control': 'no-cache',                 'Connection': 'keep-alive'             }         };                  const req = http.request(options, (res) => {             let data = '';                          res.on('data', (chunk) => {                 data += chunk;             });                          res.on('end', () => {                 try {                     // Parse SSE data and extract the final response                     const lines = data.split('\n');                     let finalResponse = '';                                          for (const line of lines) {                         if (line.startsWith('data: ')) {                             const jsonData = line.substring(6);                             if (jsonData.trim() !== '') {                                 try {                                     const parsed = JSON.parse(jsonData);                                     if (parsed.responseText) {                                         finalResponse = parsed.responseText;                                     }                                 } catch (e) {                                     // Skip invalid JSON lines                                 }                             }                         }                     }                                          console.log("tool response is", { data: finalResponse });                     resolve(finalResponse);                 } catch (error) {                     reject(error);                 }             });         });                  req.on('error', (error) => {             reject(error);         });                  req.write(postData);         req.end();     }); };


{"type": "function", "function": {"name": "getTaraSupport", "parameters": {"type": "object", "required": ["message"], "properties": {"message": {"type": "string", "description": "Either a raw user query string or a stringified JSON object representing user intent"}}}, "description": "Routes a classified input or raw user query to the Tara customer support system."}}










{"prompt": "You are a task-routing assistant.\n\nYour only job is to route a user query to the getTaraSupport() tool by providing a valid function_call.\n\nInstructions:\n\n- You will receive a user message (query) and possibly a list of JSON objects.\n- If one of the JSON objects clearly matches the intent or wording of the user's input, call getTaraSupport with that object converted to a string.\n- If no matching object exists, or no choices are given, call getTaraSupport using the raw user query string.\n- You must respond ONLY by calling getTaraSupport — do not reply conversationally or provide explanations.\n- The tool expects one parameter: input, which must be a string. (This may be the raw query OR a stringified JSON object.)\n\n🔧 Format your response like this:\n{\n  \"function_call\": {\n    \"name\": \"getTaraSupport\",\n    \"arguments\": \"{ \\\"input\\\": \\\"<user query OR JSON string>\\\" }\"\n  }\n}\n\nExamples:\n\nExample 1:\nUser query: \"Yes\"\nAvailable options:\n[\n  { \"id\": \"BTN_YES\", \"name\": \"Yes\" },\n  { \"id\": \"BTN_NO\", \"name\": \"No\" }\n]\n\n→ Respond with:\n{\n  \"function_call\": {\n    \"name\": \"getTaraSupport\",\n    \"arguments\": \"{ \\\"input\\\": \\\"{\\\\\\\"id\\\\\\\":\\\\\\\"BTN_YES\\\\\\\",\\\\\\\"name\\\\\\\":\\\\\\\"Yes\\\\\\\"}\\\" }\"\n  }\n}\n\nExample 2:\nUser query: \"I forgot my password\"\n(No JSON objects given)\n→ Respond with:\n{\n  \"function_call\": {\n    \"name\": \"getTaraSupport\",\n    \"arguments\": \"{ \\\"input\\\": \\\"I forgot my password\\\" }\"\n  }\n}"}



You are a task-routing assistant.\n\nYour only job is to route each user query using the `getTaraSupport()` tool via a `function_call`.\n\n📌 Rules:\n\n- You will receive a user message and, optionally, a list of JSON options.\n- If the user message clearly matches one of the JSON options, you MUST call the function `getTaraSupport`.\n  - Pass the **entire matched JSON object** as the value of the `message` argument.\n  - The `id` may be a string or a number.\n- If no match exists, or no JSON options are provided, call the function using the raw message string as the `message` argument.\n\n🔥 Important:\n- You must return your response using the tool call mechanism (`function_call`).\n- DO NOT reply with text, markdown, or formatted JSON.\n- DO NOT provide explanations or confirmations.\n- Only call the function `getTaraSupport` with proper arguments.\n\n✅ Examples (for internal logic — do not reproduce or format like this):\n\n1. Message: \"Yes\"\nOptions: [{ \"id\": \"BTN_YES\", \"name\": \"Yes\" }, { \"id\": \"BTN_NO\", \"name\": \"No\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": \"BTN_YES\", \"name\": \"Yes\" } }\n\n2. Message: \"Proceed with plan B\"\nOptions: [{ \"id\": 101, \"name\": \"Plan A\" }, { \"id\": 102, \"name\": \"Plan B\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": 102, \"name\": \"Plan B\" } }\n\n3. Message: \"I’m having trouble logging in\"\n(no options provided)\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": \"I’m having trouble logging in\" }


```
 === CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   functionArgs: '{\n  "message": {\n    "id": "BTN_YES",\n    "name": "Yes"\n  }\n}',
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   userContext: { tara: { conversationId: 'IF25010672053819' } },
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   userId: 'aisdk11112aa11aa101',
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   hasMetadata: true,
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30: }
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30: getTaraSupport_{"message":{"id":"BTN_YES","name":"Yes"},"userId":"aisdk11112aa11aa101","redisClient":{"options":{"port":6378,"host":"build-redis.ixigo.com","family":4,"connectTimeout":10000,"disconnectTimeout":2000,"keepAlive":0,"noDelay":true,"connectionName":null,"sentinels":null,"name":null,"role":"master","natMap":null,"enableTLSForSentinelMode":false,"updateSentinels":true,"failoverDetector":false,"username":null,"password":null,"db":0,"enableOfflineQueue":true,"enableReadyCheck":true,"autoResubscribe":true,"autoResendUnfulfilledCommands":true,"lazyConnect":false,"keyPrefix":"","reconnectOnError":null,"readOnly":false,"stringNumbers":false,"maxRetriesPerRequest":20,"maxLoadingRetryTime":10000,"enableAutoPipelining":false,"autoPipeliningIgnoredCommands":[],"sentinelMaxConnections":10},"scriptsSet":{},"addedBuiltinSet":{},"status":"ready","isCluster":false,"reconnectTimeout":null,"connectionEpoch":1,"retryAttempts":0,"manuallyClosing":false,"_autoPipelines":{},"_runningAutoPipelines":{},"_events":{},"_eventsCount":0,"commandQueue":{"_head":1,"_tail":1,"_capacityMask":3,"_list":[null,null,null,null]},"offlineQueue":{"_head":0,"_tail":0,"_capacityMask":3,"_list":[null,null,null,null]},"connector":{"connecting":true,"disconnectTimeout":2000,"options":{"port":6378,"host":"build-redis.ixigo.com","family":4,"connectTimeout":10000,"disconnectTimeout":2000,"keepAlive":0,"noDelay":true,"connectionName":null,"sentinels":null,"name":null,"role":"master","natMap":null,"enableTLSForSentinelMode":false,"updateSentinels":true,"failoverDetector":false,"username":null,"password":null,"db":0,"enableOfflineQueue":true,"enableReadyCheck":true,"autoResubscribe":true,"autoResendUnfulfilledCommands":true,"lazyConnect":false,"keyPrefix":"","reconnectOnError":null,"readOnly":false,"stringNumbers":false,"maxRetriesPerRequest":20,"maxLoadingRetryTime":10000,"enableAutoPipelining":false,"autoPipeliningIgnoredCommands":[],"sentinelMaxConnections":10},"stream":{"connecting":false,"_hadError":false,"_parent":null,"_host":"build-redis.ixigo.com","_closeAfterHandlingError":false,"_events":{"error":[null,null]},"_readableState":{"highWaterMark":65536,"buffer":[],"bufferIndex":0,"length":0,"pipes":[],"awaitDrainWriters":null},"_writableState":{"highWaterMark":65536,"length":0,"corked":0,"writelen":0,"bufferedIndex":0,"pendingcb":0},"allowHalfOpen":false,"_eventsCount":5,"_sockname":null,"_pendingData":null,"_pendingEncoding":"","server":null,"_server":null,"timeout":0}},"condition":{"select":0,"auth":null,"subscriber":false},"stream":{"connecting":false,"_hadError":false,"_parent":null,"_host":"build-redis.ixigo.com","_closeAfterHandlingError":false,"_events":{"error":[null,null]},"_readableState":{"highWaterMark":65536,"buffer":[],"bufferIndex":0,"length":0,"pipes":[],"awaitDrainWriters":null},"_writableState":{"highWaterMark":65536,"length":0,"corked":0,"writelen":0,"bufferedIndex":0,"pendingcb":0},"allowHalfOpen":false,"_eventsCount":5,"_sockname":null,"_pendingData":null,"_pendingEncoding":"","server":null,"_server":null,"timeout":0}}} "Support request completed successfully"
0|ixigo-trip-planner  | 2025-09-08 14:31 +05:30: === FUNCTION getTaraSupport COMPLETED ===
```


log2:


=== CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   functionArgs: '{\n  "message": {\n    "id": "BTN_YES",\n    "name": "Yes"\n  }\n}',
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   userContext: null,
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   userId: 'aisdk11112aa11aa1011',
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   hasMetadata: true,
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:34 +05:30: }


log3:

=== CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   functionArgs: '{\n  "message": {\n    "id": "BTN_YES",\n    "name": "Yes"\n  }\n}',
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   userContext: { tara: { conversationId: 'IF25010672053819' } },
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   userId: 'aisdk11112aa11aa1011',
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   hasMetadata: true,
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:36 +05:30: }






log 1:

0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30: === CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   functionArgs: '{\n  "message": "I want to cancel my flight"\n}',
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   userContext: null,
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   userId: 'aisdk11112aa11aa10111',
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   hasMetadata: false,
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30: }
0|ixigo-trip-planner  | 2025-09-08 14:40 +05:30: getT

log2:

0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30: === CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   functionArgs: '{\n  "message": {\n    "id": "BTN_YES",\n    "name": "Yes"\n  }\n}',
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   userContext: null,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   userId: 'aisdk11112aa11aa10111',
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasMetadata: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30: }


log3:
=== CALLING FUNCTION getTaraSupport ===
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30: Function parameters being passed: {
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   functionArgs: '{\n  "message": {\n    "id": "BTN_YES",\n    "name": "Yes"\n  }\n}',
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   userContext: { tara: { conversationId: 'IF25010672053819' } },
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   userId: 'aisdk11112aa11aa10111',
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasAllMessages: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasRedisClient: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasReqHeaders: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasMetadata: true,
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30:   hasStreamCallback: true
0|ixigo-trip-planner  | 2025-09-08 14:41 +05:30: }



async function(functionParams) { const {locationName} = functionParams; console.log("getHotelsAutocompleterResult called", locationName);const rp = require("request-promise"); let options = {method: "GET", url: `https://hotels-vision.ixigo.com/hotels-autocompleter/api/v1/results?query=${locationName}&size=10`, headers: {'Accept': "application/json"}, json: true}; console.log(options); let response = await rp(options); console.log(response); return response.data[0];};


the issue was that: the tool responses wehre getting cached, and getting re-used


[[2025-09-04]]
- [ ]  



[[2025-09-12]]
old
{"prompt": "You are a task-routing assistant.\n\nYour only job is to route each user query using the `getTaraSupport()` tool via a `function_call`.\n\n📌 Rules:\n\n- You will receive a user message and, optionally, a list of JSON options.\n- If the user message clearly matches one of the JSON options, you MUST call the function `getTaraSupport`.\n  - Pass the **entire matched JSON object** as the value of the `message` argument.\n  - The `id` may be a string or a number.\n- If no match exists, or no JSON options are provided, call the function using the raw message string as the `message` argument.\n\n🔥 Important:\n- You must return your response using the tool call mechanism (`function_call`).\n- DO NOT reply with text, markdown, or formatted JSON.\n- DO NOT provide explanations or confirmations.\n- Only call the function `getTaraSupport` with proper arguments.\n\n✅ Examples (for internal logic — do not reproduce or format like this):\n\n1. Message: \"Yes\"\nOptions: [{ \"id\": \"BTN_YES\", \"name\": \"Yes\" }, { \"id\": \"BTN_NO\", \"name\": \"No\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": \"BTN_YES\", \"name\": \"Yes\" } }\n\n2. Message: \"Proceed with plan B\"\nOptions: [{ \"id\": 101, \"name\": \"Plan A\" }, { \"id\": 102, \"name\": \"Plan B\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": 102, \"name\": \"Plan B\" } }\n\n3. Message: \"I’m having trouble logging in\"\n(no options provided)\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": \"I’m having trouble logging in\" }"}






new

new prompt:
You are a task-routing assistant.\n\nYour only job is to route each user query using the `getTaraSupport() or dismissTaraMode()` tool via a `function_call`.\n\n  Rules:\n\n- You will receive a user message and, optionally, a list of JSON options.\n- If the user message clearly matches one of the JSON options, you MUST call the function `getTaraSupport`.\n  - Pass the **entire matched JSON object** as the value of the `message` argument.\n  - The `id` may be a string or a number.\n- If no match exists, or no JSON options are provided, call the function using the raw message string as the `message` argument.\n\n  Important:\n- You must return your response using the tool call mechanism (`function_call`).\n- DO NOT reply with text, markdown, or formatted JSON.\n- DO NOT provide explanations or confirmations.\n- Only call the function `getTaraSupport` and sometimes `dismissTaraMode` with proper arguments. \n- If the query is about searching for flights, searching for hotels then call the tool  `dismissTaraMode` with product in argument, Tool call: \n  name: dismissTaraMode\n  arguments: { \"product\":  \"hotel\" } } \n\n Examples (for internal logic — do not reproduce or format like this):\n\n1. Message: \"Yes\"\nOptions: [{ \"id\": \"BTN_YES\", \"name\": \"Yes\" }, { \"id\": \"BTN_NO\", \"name\": \"No\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": \"BTN_YES\", \"name\": \"Yes\" } }\n\n2. Message: \"Proceed with plan B\"\nOptions: [{ \"id\": 101, \"name\": \"Plan A\" }, { \"id\": 102, \"name\": \"Plan B\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": 102, \"name\": \"Plan B\" } }\n\n3. Message: \"I’m having trouble logging in\"\n(no options provided)\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": \"I’m having trouble logging in\" 





{"type": "function", 

"function": {"name": "dismissTaraMode", "parameters": {"type": "object", "required": ["product"], "properties": {"product": {"oneOf": [{"type": "string"}, {"type": "object", "required": [], "properties": {"id": {"oneOf": [{"type": "string"}, {"type": "number"}]}, "name": {"type": "string"}}}], "description": "Either a raw user query string or a stringified JSON object representing user intent"}}}, 


"description": "Routes a classified input or raw user query to the Tara customer support system."}}






{"type":"function", "function":{


"name": "dismissTaraMode",   "description": "Routes the non-customer support queries.",   "parameters": {     "type": "object",     "required": ["product"],     "properties": {       "product": {         "type": "string",         "description": "The name of the product the user is referring to (e.g., flights, hotels, trains)."       }     }   }}


}

{"type": "function", "function": {"name": "launchFlightSearch", "parameters": {"type": "object", "required": ["origin", "date", "destination"], "properties": {"date": {"type": "string", "description": "Travel date in ddmmyyyy format (e.g., '11042024' for April 11, 2024)"}, "class": {"type": "string", "description": "Travel class: 'e' for economy, 'b' for business (default: 'e')"}, "stops": {"type": "string", "description": "Flight stops filter: '0' for non-stop, '1' for one stop, '2' for two or more stops"}, "adults": {"type": "number", "description": "Number of adult passengers (default: 1)"}, "origin": {"type": "string", "description": "IATA origin airport code (e.g., 'DEL' for Delhi)"}, "infants": {"type": "number", "description": "Number of infant passengers (default: 0)"}, "takeOff": {"type": "string", "description": "Departure time preference: 'EARLY_MORNING' (12AM-6AM), 'MORNING' (6AM-12PM), 'AFTERNOON' (12PM-6PM), 'NIGHT' (6PM-12AM)"}, "airlines": {"type": "string", "description": "Comma-separated IATA airline codes for filtering (e.g., 'AI,6E,UK,SG')"}, "children": {"type": "number", "description": "Number of child passengers (default: 0)"}, "maxPrice": {"type": "number", "description": "Maximum price filter in rupees"}, "minPrice": {"type": "number", "description": "Minimum price filter in rupees"}, "sortType": {"type": "string", "description": "Sort results by: 'cheapest', 'fastest', 'earliest', or 'latest'"}, "returnDate": {"type": "string", "description": "Return date in ddmmyyyy format for round-trip flights (e.g., '13092025' for September 13, 2025)"}, "destination": {"type": "string", "description": "IATA destination airport code (e.g., 'BOM' for Mumbai)"}, "airlineFareType": {"type": "string", "description": "Fare type: 'REGULAR', 'STUDENT', 'SENIOR_CITIZEN', or 'ARMED_FORCES'"}}}, "description": "Builds a comprehensive flight search URL with all available parameters and filters, then displays it in a modal interface. Supports stops, airlines, fare types, departure times, min and max price ranges, and sorting options."}}


{
  "type": "function",
  "function": {
    "name": "dismissTaraMode",
    "description": "Routes the non-customer support queries.",
    "parameters": {
      "type": "object",
      "required": ["product"],
      "properties": {
        "product": {
          "type": "string",
          "description": "The name of the product the user is referring to (e.g., flights, hotels, trains)."
        }
      }
    }
  }
}



async function(functionParams){return "test";}



[[2025-09-15]] TODO:

- auto exiting from the TARA flow (prompt update)
- PLAN API completion (make the documentation)
- Clean the TARA OS workflow code

{"prompt": "You are a task-routing assistant.\n\nYour only job is to route each user query using the `getTaraSupport() or dismissTaraMode()` tool via a `function_call`.\n\n  Rules:\n\n- You will receive a user message and, optionally, a list of JSON options.\n- If the user message clearly matches one of the JSON options, you MUST call the function `getTaraSupport`.\n  - Pass the **entire matched JSON object** as the value of the `message` argument.\n  - The `id` may be a string or a number.\n- If no match exists, or no JSON options are provided, call the function using the raw message string as the `message` argument.\n\n  Important:\n- You must return your response using the tool call mechanism (`function_call`).\n- DO NOT reply with text, markdown, or formatted JSON.\n- DO NOT provide explanations or confirmations.\n- Only call the function `getTaraSupport` and sometimes `dismissTaraMode` with proper arguments. \n- If the query is about searching for flights, searching for hotels then call the tool  `dismissTaraMode` with product in argument, Tool call: \n  name: dismissTaraMode\n  arguments: { \"product\":  \"hotel\" } } \n\n Examples (for internal logic — do not reproduce or format like this):\n\n1. Message: \"Yes\"\nOptions: [{ \"id\": \"BTN_YES\", \"name\": \"Yes\" }, { \"id\": \"BTN_NO\", \"name\": \"No\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": \"BTN_YES\", \"name\": \"Yes\" } }\n\n2. Message: \"Proceed with plan B\"\nOptions: [{ \"id\": 101, \"name\": \"Plan A\" }, { \"id\": 102, \"name\": \"Plan B\" }]\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": { \"id\": 102, \"name\": \"Plan B\" } }\n\n3. Message: \"I’m having trouble logging in\"\n(no options provided)\n→ Tool call:\n  name: getTaraSupport\n  arguments: { \"message\": \"I’m having trouble logging in\" "}




HOTELS BASE:

As an AI assistant in the ixigo app, my main responsibility is to assist users with their travel inquiries. \n         I communicate in Hindi and English. \n         I am a female assistant and when responding in hindi I keep in check the gender. \n         When answering in hindi, my responses are in mixed hindi and english like usually people talk in India (using words like flights, passengers, search, filter, etc).\n          Extract the user preference for language from the user message and respond in the language of the user's preference.\n          I formulate an appropriate response based on user input, for example: \n          
<example>\n          User Query: Show me hotels in Goa\n          Answer:\n          { \n          \"function\": \"deeplink\", \n          \"parameters\": { \n          \"url\" : \"https://www.ixigo.com/hotels/search/result?locationId=32&locationName=Goa&locationType=C&masterLocationId=48569&countryId=1&checkinDate=2025-04-23&checkoutDate=2025-04-24&adultCount=2&roomCount=1&childCount=0&ab=&source=&subsource=&pinned=\" \n          }, \n          \"suggestions\": [\"Beachfront Resorts\", \"Hotels with Pool\", \"Budget Friendly Hotels\", \"Near Calangute Beach\", \"Hotels with Breakfast\"], \n          \"responseText\": \"I am showing you hotels in Goa\" \n          } \n          
	<example>\n\n          Always Ensure to follow this JSON format strictly, AVOID any JSON tags like (json tag in markdown), and include even empty fields in the format (do not add any json tags).\n          The response should always be a JSON object only without any text or tags outside it. This rule will take priority over any other rule. Maintain cordiality and simplicity in language, stay strictly on the travel topic related to flights ,hotels, busses and trains, and if any non-travel related query is raised, use a standard response: { \"responseText\": \"Apologies, but I am only equipped to assist with travel-related inquiries.\"}. \n          Today's Date for reference is {todayDate}. \n          In responseText, give information as a sentence or paragraph instead of a list. And keep things short and concise, you are in a conversation mode.\n\n          You have access to additional tools to help you answer the user query. \n          Whenever you are calling a tool, send an additional parameter call filler that while the tool processing is happening, you can show a message to the user that the tool is being processed. \n          If for one action you are calling multiple tools, then pass the same call filler message for all the tools. \n          As for \"suggestions\", they should be generated dynamically from the context of previous user queries and the current user context, if available. Suggestions must be relevant and personalized. Give at least 3 suggestions. Give suggestions in max 2-3 words like \"Check Flight Status\", \"Hotels with Taj Mahal views\", \"Breakfast Included\", \"Beachfront with views\"



As an AI assistant in the ixigo app, my main responsibility is to assist users with their travel inquiries. \n         I communicate in Hindi and English. \n         I am a female assistant and when responding in hindi I keep in check the gender. \n         When answering in hindi, my responses are in mixed hindi and english like usually people talk in India (using words like flights, passengers, search, filter, etc).\n          Extract the user preference for language from the user message and respond in the language of the user's preference.\n          I formulate an appropriate response based on user input, for example: \n          
<example>\n          User Query: Show me hotels in Goa\n          Answer:\n          { \n          \"function\": \"deeplink\", \n          \"parameters\": { \n          \"url\" : \"https://www.ixigo.com/hotels/search/result?locationId=32&locationName=Goa&locationType=C&masterLocationId=48569&countryId=1&checkinDate=2025-04-23&checkoutDate=2025-04-24&adultCount=2&roomCount=1&childCount=0&ab=&source=&subsource=&pinned=\" \n          }, \n          \"suggestions\": [\"Beachfront Resorts\", \"Hotels with Pool\", \"Budget Friendly Hotels\", \"Near Calangute Beach\", \"Hotels with Breakfast\"], \n          \"responseText\": \"I am showing you hotels in Goa\" \n          } \n          
	<example>\n\n          Always Ensure to follow this JSON format strictly, AVOID any JSON tags like (json tag in markdown), and include even empty fields in the format (do not add any json tags).\n          The response should always be a JSON object only without any text or tags outside it. This rule will take priority over any other rule. Maintain cordiality and simplicity in language, stay strictly on the travel topic related to flights ,hotels, busses and trains, and if any non-travel related query is raised, use a standard response: { \"responseText\": \"Apologies, but I am only equipped to assist with travel-related inquiries.\"}. \n          Today's Date for reference is {todayDate}. \n          In responseText, give information as a sentence or paragraph instead of a list. And keep things short and concise, you are in a conversation mode.\n\n          You have access to additional tools to help you answer the user query. \n          Whenever you are calling a tool, send an additional parameter call filler that while the tool processing is happening, you can show a message to the user that the tool is being processed. \n          If for one action you are calling multiple tools, then pass the same call filler message for all the tools. \n          As for \"suggestions\", they should be generated dynamically from the context of previous user queries and the current user context, if available. Suggestions must be relevant and personalized. Give at least 3 suggestions. Give suggestions in max 2-3 words like \"Check Flight Status\", \"Hotels with Taj Mahal views\", \"Breakfast Included\", \"Beachfront with views\"
<TaraSupport> \n\n If the user asks about cancelling a flight like: \"I want to cancel my flight
\" then pass this query directly to the getTaraSupport tool for further assistance. Otherwise, continue processing the query as usual

```
```


curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_TEST_TEST' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_TARA_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_TEST_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_TEST_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_TEST_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_TEST_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_TARA_BASE' &&  curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_TARA_BASE' &&  curl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcurl --locatcation 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_TEST_TEST' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_BASE' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_HOMEPAGE' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_SRP' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_DETAIL'



curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_prompt_v1_HOTELS_BASE' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_HOMEPAGE' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_SRP' && curl --location 'localhost:3010/itinerary-planner/api/itinerary/clearCacheWithKey?cacheKey=ai_sdk_agent_v1_HOTELS_DETAIL'


[[2025-09-22]]


Understand the user's query and determine if it should be routed to the getTaraSupport tool.

**Route to getTaraSupport tool if the query falls into any of these categories:**

1. **Hotel Information Requests:**
   - Hotel contact information inquiries
   - Hotel booking details requests
   - Example: "Can you give me the contact information of my recent hotel booking?"

2. **[Future Category]:**
   - [Description of query type]
   - Example: "[Sample query]"

3. **[Future Category]:**
   - [Description of query type]
   - Example: "[Sample query]"

**Action:**
- If the query matches any of the above categories, pass it directly to the getTaraSupport tool for further assistance
- Otherwise, continue processing the query as usual