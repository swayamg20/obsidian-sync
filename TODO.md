[[what are we trying to achieve]]
[[2025-10-07]]
- [ ] gpt 5 instruction based model support
- [ ] API calling through tools
- [ ] independent system to test

PS for today: 
I need to test taraOS independently, isolated from the current trip-planner system. In which I can just ask any support related query and it will give me normal response in a simple natural language way.


[[2025-10-08]]
- yesterday the poc on gpt-5 response API was done, these were the results:
	- gpt-4o-mini is much faster and can be relied upon.
	- need to break down the tools and make them much simple to understand by llm

Today: 
- [ ] Setting up the right config, try gpt-4o-mini more
- [ ] connect with vivek, for the flows

[[2025-10-09]]
- the discussion happened on these lines:
	- We need to test that if the llm is able to go to different steps via itself by filling the required details
	- eg: if I switch to a different flow, it will return me a booking selection widget but if I have the booking widget in my memory, then I should automatically send this booking widget in the response.
	- eg: if a user is asked to select language, and it selected english then in future, it should not be asked the same question, that question should be pre-filled by the llm.

[[2025-10-11]]
- [ ] The idea is to never ask the same question again, if the API is asking the same data then LLM should be responsible to send 

[[2025-10-12]]
prompts: [[agenticAPI]]
prompts: [[IVR FLOW]]
task that I want LLM to pefrom:
current information:
`user_context` (what user has done past)
`available_choice` available choices (in json)


