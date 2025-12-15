js script injection: [[aisdkjsinjection]]

[[improvements]] [[🚀 Advanced Technical Blog Topics - AI Assistant SDK Analysis]] [[article2]] [[article3]][[testing]] [[design_review]]
```{

"dependencies": {

"@ixigo/voa-flight": "file:path/to/your/monorepo/apps/voa-flight",

"@ixigo/voa-utils": "file:path/to/your/monorepo/packages/voa-utils"

}

}
```

[[new doc]]



```
import { App, initAssistant, setWidget } from '@ixigo/voa-flight';
```


```
Thank you for providing the codebase. I'll be happy to explain the overall structure and purpose of this project. The codebase appears to be a voice-assisted flight booking application built using Preact and TypeScript. Let's break down the main components and their purposes:

- Project Structure:

The project uses a monorepo structure with pnpm workspaces, as evidenced by the pnpm-workspace.yaml file. It consists of two main parts:

- apps/voa-flight: The main application

- packages/voa-utils: A utility package shared across the project

- Main Application (voa-flight):

The main application is a Preact-based web app that provides a voice assistant interface for flight bookings. Key files include:

- src/app.tsx: The main component that renders the voice assistant interface.

- src/assistant.ts: Handles the initialization of the assistant and
- manages the widget's position and size.

- src/PromiseNativeBridge.ts: Provides a bridge for communication between the web app and native mobile app functions.

- Utility Package (voa-utils):

This package contains shared utilities, including the SDK for voice recognition and chat responses. The main file is src/main.ts, which exports functions for initializing the SDK, getting chat responses, and managing the microphone.

- Voice Recognition and Chat:

The application uses the Deepgram SDK for voice recognition. It transcribes user speech and sends it to a chat API for processing. The chat responses can include suggestions, audio responses, and actions like opening deep links.

- Responsive Design:

The application is designed to work on various screen sizes, with the ability to adjust its size and position dynamically. This is handled in the assistant.ts file.

- Build and Deployment:

The project uses Vite for building the application and has a GitLab CI configuration for automated building and deployment to an AWS S3 bucket with CloudFront distribution.

Key Features:

1. Voice input for flight searches and other travel-related queries

- Text-to-speech output for responses

- Suggestion buttons for quick follow-up actions

4. Ability to open deep links based on chat responses

5. Responsive design that can adapt to different screen sizes and positions

The application is designed to be embedded in a larger travel booking platform, providing a voice-assisted interface for users to search for flights, trains, hotels, and buses.
```

imported the library
getting issue in the other local bundling

replaceState is changing the URL



things done:
- [ ] npm package created using voa-flight application
- [ ] integration to hotels repo done
- [ ] all functions from package are getting used by hotels repo and are accessible outside also
- [ ] page routing issue fixed (before, the returning URL from ai sdk was stopping the current state of ai sdk component)

next steps:
- [ ] working on the triggers (discussed with prashant)





https://www.figma.com/design/0NbEV44Ui16A9IYQlhbN6N/AI-voiceover?node-id=2462-51935&node-type=canvas&t=yo9nOZwIULK1wpqS-0



UI design according to figma :



UI elements
animations

*2-3 days (max)*

functionality:

triggers sending actions and events
controlling sdk according to events' response
on path change, preserve the state
adding more functions to perform


SRP

1. AI SDK integration
2. Call Search on Page.
3. Add Filters to search Criteria
4. Events (CT Events, Scroll event) -> Browser scroll event to be passed to AISdk
5. ABility to Move/Drag SDK widget on Page.


| full view | mic | speaker | fab     |
| --------- | --- | ------- | ------- |
| on        | off | off     | inside  |
| on        | off | on      | inside  |
| on        | on  | off     | outside |



fab inside
full view on
mic off
speaker off

![[Screenshot 2024-10-24 at 6.21.36 PM.png]]


fab inside
speaker on
full view on
mic off
![[Screenshot 2024-10-24 at 6.21.51 PM.png]]


fab out

listening on
fulll view on
speaker off
![[Screenshot 2024-10-24 at 6.21.49 PM.png]]

fab inside
speaker on
full view on
mic off

![[Screenshot 2024-10-24 at 6.21.43 PM.png]]

mic on
speaker off
full view on
![[Screenshot 2024-10-24 at 6.21.37 PM.png]]





























today's tasks:
1. complete the UI
2. discuss with abhishek, about the code structure
3. push to the staging branch (dashboard)

PNR:2652447713


what should I use:
cache /vs/ local storage


task:
call an api
```
localhost:3010/itinerary-planner/api/aiSdk/getConfig?product=HOTELS&page=SRP
```
save the data 
```
{
  "status": true,
  "data": [
    {
      "id": 1,
      "aiSdkAgentId": 3,
      "eventName": "HOTEL_SEARCH_LAUNCHED",
      "type": "ct_event",
      "delayTime": 0,
      "actions": null
    },
    {
      "id": 2,
      "aiSdkAgentId": 3,
      "eventName": "HOTEL_FILTERS_APPLIED",
      "type": "ct_event",
      "delayTime": 0,
      "actions": null
    }
  ]
}
```

I will get event's name:

then, according to that event, I need to set few settings every time a new event is called.
those are: 
delayTime, actions, eventName

done



tasks:

AI SDK:
1. UI changes
2. code cleaning
3. event triggering

Voice Agent:
1. download csv structure
2. UI minor fixes
3. create and delete should have a trigger

screenshot2code:
1. put it in git repo
2. check for custom data ingestion and learning





list down all the pending work in AI SDK
- [x] the mic flow
- [ ] UI fixes:
	- [x] mic remove
	- [x] status of the middle state b/w listening and chat response
	- [x] on click fab, start mic (flow)
	- [x] if the url is not provided in the results then: ![[Screenshot 2024-11-07 at 1.00.52 PM.png]]
- [ ]  recording the triggered events and actions based on those
- [ ] drag and drop
- [x] deepgram transcription
- [x] if I am on a different page then SRP and run a voice over search. ![[Screenshot 2024-11-07 at 1.12.44 PM.png]]



[[2024-12-04]]

Issues to solve: 
- [x] the loading state on clicking the suggestions
- [x] the config file should be getting updated when changing the page
- [ ] two calls getting sent on the first hit
- [ ] on scroll multiple calls



simply:
if I am clicking on a suggestion then it means that I am in the processing state, so 



it checks the chatState ```showFullView```  or not
	if ```true``` :
		if ```chatStatus.canSpeak``` if ```true```
			will check ```isProcessing``` 
				true: getting results
				false: listening
			will check: ```transcript``` 
				true: show transcript with animation
				false: directly show transcript
		if ```false``` 
			check ```isInputMode``` 
				true: show the input box
				false: show the heading



scrollActivity:
as soon as the view count is 5 then send an event

only allow integral value to be passed
only allow/set distinct integral value
only pass the event at exact number of views



```
--1) comparision -> 2 + 5 -> on detail page 
--2) if family in search ,i.e., child > 1, then on srp trigger -> do you want to apply any family friendly filters 
--3) on city-> destination specific trigger for famous pois -> like agra, paris, ayodhya, puri, mathura, varanasi, ajmer, ujjain 
4) on SRP -> on long scroll (10 hotels/20 hotels) -> trigger -> help with filters 
--5) if the user comes back to srp after 3 hotel detail pages, then ask him about that specific filter (like star rating/price) 
--6) if opens facilities section -> trigger -> can I help you find a facility XX 7) if user opens review -> trigger -> top reviews feedback on tap suggestions - 1) if one adult -> show filters like business travel filters
2) 2 adults -> couple friendly -> can be run as an experiment on trigger as well 3) hotels with mountain view etc based on that destination 
--4) based on destination + current situation (ayodhya innaugration) -> give suggestions like show hotels near ram janm bhoomi 
dissmiss -> how to handle dismiss
```


discussion on the new 

is structure correct
android/iOS capability of the sdk



# Here are key improvements for the codebase:

1. **State Management Separation**

```tsx 62:71:packages/voa-flight/src/AiChat.tsx
  const [chatState, setChatState] = useState({
    canSpeak: false,
    soundOff: false,
    showFullView: false,
  });
  const [inProgress, setProgress] = useState(false);
  const [response, setResponse] = useState<ChatResponse | undefined>(undefined);
  const [isProcessing, setIsProcessing] = useState(false);
  const [fabPosition, setFabPosition] = useState({ bottom: 40, right: 30 });
  const [transcript, setTranscript] = useState("");
```

Move state management to a custom hook or context. Create a separate `useAiChatState` hook:

```typescript
// hooks/useAiChatState.ts
export const useAiChatState = () => {
  const [chatState, setChatState] = useState({
    canSpeak: false,
    soundOff: false,
    showFullView: false
  });
  const [inProgress, setProgress] = useState(false);
  const [response, setResponse] = useState<ChatResponse>();
  const [isProcessing, setIsProcessing] = useState(false);

  return {
    chatState,
    setChatState,
    inProgress,
    setProgress,
    response, 
    setResponse,
    isProcessing,
    setIsProcessing
  };
};
```

2. **API Layer Separation**
Create a dedicated API service:

```typescript
// services/api.ts
export class AiChatService {
  static async getChatResponse(input: string, context: any) {
    return fetch('http://localhost:3010/itinerary-planner/api/aiSdk/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        event: { name: 'chat', data: { input } },
        userId: 'abcdefg',
        context
      })
    }).then(res => res.json());
  }

  static async getConfigs(product: string, page: string) {
    return fetch(`http://localhost:3010/itinerary-planner/api/aiSdk/getConfig?product=${product}&page=${page}`)
      .then(res => res.json());
  }
}
```

3. **Constants Extraction**
```typescript
// constants/index.ts
export const API_ENDPOINTS = {
  CHAT: 'http://localhost:3010/itinerary-planner/api/aiSdk/chat',
  CONFIG: 'http://localhost:3010/itinerary-planner/api/aiSdk/getConfig'
};

export const ANIMATION_CONFIGS = {
  GRADIENT: {
    duration: 3,
    repeat: Infinity,
    ease: 'linear'
  }
};
```

4. **Component Splitting**
Break down AiChat.tsx into smaller components:

```typescript
// components/ChatInput.tsx
// components/ChatSuggestions.tsx 
// components/FloatingButton.tsx
// components/ChatHeader.tsx
```

5. **Type Safety Improvements**
Create proper type definitions:

```typescript
// types/index.ts
export interface ChatState {
  canSpeak: boolean;
  soundOff: boolean;
  showFullView: boolean;
}

export interface ChatResponse {
  responseText: string;
  responseAudio?: string;
  function?: string;
  parameters?: Record<string, any>;
  suggestions?: string[];
  heading?: string;
  autoPlayAllowed?: boolean;
}
```

6. **Error Handling**
Add proper error boundaries and error handling:

```typescript
// components/ErrorBoundary.tsx
export class ChatErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <div>Something went wrong with the chat.</div>;
    }
    return this.props.children;
  }
}
```

7. **Testing Setup**
Add test configuration and basic tests:

```typescript
// __tests__/AiChat.test.tsx
import { render, fireEvent } from '@testing-library/react';
import { AiChat } from '../src/AiChat';

describe('AiChat', () => {
  it('should render chat interface', () => {
    const { getByText } = render(<AiChat />);
    expect(getByText("Say 'Hi Tara'!")).toBeInTheDocument();
  });
});
```

8. **Performance Optimizations**
- Use `React.memo()` for components that don't need frequent updates
- Implement proper cleanup in useEffect hooks
- Use proper key props in lists
- Consider using `useCallback` for event handlers

9. **Configuration Management**
Move configuration to environment variables:

```typescript
// config/index.ts
export const config = {
  apiUrl: process.env.REACT_APP_API_URL || 'http://localhost:3010',
  defaultProduct: 'HOTELS',
  defaultPage: 'SRP'
};
```

10. **Documentation**
Add proper JSDoc comments and create documentation:

```typescript
/**
 * AiChat Component
 * @component
 * @param {AiChatProps} props - Component props
 * @returns {React.ReactElement} Rendered component
 */
```

These improvements will make the code:
- More maintainable
- Easier to test
- More performant
- Better typed
- More modular
- Easier to debug

Sources:
- React Best Practices Documentation
- TypeScript Handbook
- React Testing Library Documentation
- React Performance Documentation

# (2)

Here's a comprehensive review of the @voa-flight package:

### Critical Issues

1. **Type Safety**
- Excessive use of `any` type:

```1:4:packages/voa-flight/src/types.d.ts
declare module '*.json' {
    const value: any;
    export default value;
}
```

- Missing proper TypeScript interfaces for API responses and configs

2. **Security**
- Hardcoded URLs and no environment configuration:

```54:56:packages/voa-flight/src/assistant.ts
  const response = await fetch(
    `http://localhost:3010/itinerary-planner/api/aiSdk/chat`,
    {
```

- No input sanitization for user inputs
- Missing CORS and CSP configurations

3. **Package Structure**
- Missing essential package.json fields: description, keywords, repository, author, license
- No .npmignore file
- Missing README.md with documentation
- No CHANGELOG.md
- No unit tests configuration

4. **Window Object Pollution**
- Unsafe global window extensions:

```1:5:packages/voa-flight/src/PromiseNativeBridge.ts
declare global {
  interface Window {
    PromiseNativeBridge: PromiseNativeBridgeImpl | undefined;
  }
}
```

- Should use a namespace pattern or Symbol for custom properties

5. **Error Handling**
- Inconsistent error handling patterns
- Missing error boundaries
- Console logs in production code
- No proper error reporting mechanism

### Architecture Issues

1. **Communication Layer**
- Mixed concerns in assistant.ts - handles both UI and business logic
- No clear separation between native bridge and web APIs
- Callback hell in some places instead of proper Promise chains

2. **State Management**
- Scattered state management across components
- Global state mutations without proper patterns
- No clear data flow architecture

3. **Performance**
- No code splitting
- Large JSON animations loaded upfront
- Missing performance optimizations for animations
- No lazy loading implementation

### Fixes Required

1. **Type Safety**
```typescript
// Instead of
declare module '*.json' {
    const value: any;
    export default value;
}

// Use
declare module '*.json' {
    const value: Record<string, unknown>;
    export default value;
}
```

2. **Configuration**
```typescript
// Add configuration management
export interface Config {
  apiUrl: string;
  environment: 'development' | 'production' | 'staging';
  version: string;
}

export const getConfig = (): Config => ({
  apiUrl: process.env.API_URL || 'http://localhost:3010',
  environment: process.env.NODE_ENV as Config['environment'],
  version: process.env.VERSION || '1.0.0',
});
```

3. **Window Extensions**
```typescript
// Instead of direct window extension
const VOA_NAMESPACE = Symbol('VOA_NAMESPACE');

interface VoaWindow {
  PromiseNativeBridge?: PromiseNativeBridgeImpl;
}

declare global {
  interface Window {
    [VOA_NAMESPACE]?: VoaWindow;
  }
}
```

4. **Error Handling**
```typescript
export class VoaError extends Error {
  constructor(
    message: string,
    public code: string,
    public metadata?: Record<string, unknown>
  ) {
    super(message);
    this.name = 'VoaError';
  }
}

export const errorHandler = (error: unknown): void => {
  if (error instanceof VoaError) {
    // Handle known errors
  } else {
    // Handle unknown errors
  }
};
```

### Package.json Improvements
```json
{
  "name": "@ixigo/voa-flight",
  "version": "1.0.0",
  "description": "Voice Assistant Flight Package for Ixigo",
  "keywords": ["voice", "assistant", "flight", "ixigo"],
  "author": "Ixigo Team",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ixigo/voa-flight.git"
  },
  "bugs": {
    "url": "https://github.com/ixigo/voa-flight/issues"
  },
  "scripts": {
    "test": "jest",
    "lint": "eslint .",
    "build:clean": "rimraf dist",
    "build": "npm run build:clean && tsc && vite build",
    "prepublishOnly": "npm run build"
  }
}
```

### Additional Recommendations

1. Add proper documentation
2. Implement proper testing strategy
3. Add CI/CD pipeline configuration
4. Implement proper versioning strategy
5. Add proper logging mechanism
6. Implement proper monitoring
7. Add proper security measures
8. Implement proper caching strategy
9. Add proper error reporting
10. Implement proper performance monitoring

Sources:
- NPM Package Best Practices: https://docs.npmjs.com/cli/v9/configuring-npm/package-json
- TypeScript Handbook: https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html
- React Performance: https://reactjs.org/docs/optimizing-performance.html









1. ai sdk: fixed the api config issue, used mutation observer to check the change in DOM
2. fixed the multiple calls on scroll event
3. the back button clevar tap event is from hotels side (need to search the exact event and send it to the sdk)
4. build time, reduced it by a bit but need to work on that more



```
useEffect(() => {

window.assistantJs?.getPageInfo({

page: 'SRP',

product: 'HOTELS',

});

}, []);
```

things to do:

functionality wise check
UI wise check




use cases - --1) comparision -> 2 + 5 -> on detail page --2) if family in search ,i.e., child > 1, then on srp trigger -> do you want to apply any family friendly filters --3) on city-> destination specific trigger for famous pois -> like agra, paris, ayodhya, puri, mathura, varanasi, ajmer, ujjain 4) on SRP -> on long scroll (10 hotels/20 hotels) -> trigger -> help with filters --5) if the user comes back to srp after 3 hotel detail pages, then ask him about that specific filter (like star rating/price) --6) if opens facilities section -> trigger -> can I help you find a facility XX 7) if user opens review -> trigger -> top reviews feedback on tap suggestions - 1) if one adult -> show filters like business travel filters 2) 2 adults -> couple friendly -> can be run as an experiment on trigger as well 3) hotels with mountain view etc based on that destination --4) based on destination + current situation (ayodhya innaugration) -> give suggestions like show hotels near ram janm bhoomi dissmiss -> how to handle dismiss




[[2024-12-11]]

- [ ] gradient border in the lottie icon
- [ ] shimmer only on each text, not on multiple lines
- [ ] tap to speak mode
- [ ] on listening stage, increase the icon size
- [ ] intermediate animation

[[2024-12-16]] (new design)
- [x] on start listening, the fab should get bigger
- [ ] the draggability fix
- [x] the positioning of the fab and the distances
- [ ] the gradient border
- [ ] shimmer on the status
- [ ] typing module
- [x] the shadow of the chips (suggestions)
- [x] the suggestion chips should be in carousel
- [x] make the whole npm package buildable, keeping the domain name dynamic
- [x] checking overall working of sdk
- [x] hotel filters


[[2024-12-29]]

- [ ] handle the error "Something went wrong"
- [ ] UI:
	- [x] suggestions' gradient
	- [x] ripple effect on the tap 
	- [ ] text mode
	- [x] "Plan with voice mode" gradient and design
	- [ ] check all the flow from starting




# new 
got to know about framer motion ref (I can pass any ref to add drag constrains to the div)


# onboarding flow:


- [ ] should be only once
	controlled using a var point form hotels

will get a check from hotels (as prop)

things I need to control:
1. prohibit from calling our regular chat APIs
2. prohibit listening

[[2025-01-07]] current flow:



# pnpm link/unlink


Certainly! Here's a concise summary of how to connect a package from a `pnpm` repo to a `yarn` project locally:

1. **Build the `pnpm` Package**:
    
    - Navigate to the package directory in your `pnpm` workspace and build it using `pnpm build`.
2. **Create a Symlink with `yarn`**:
    
    - Go to the `pnpm` package directory and run `yarn link` to create a global symlink for that package.
3. **Link in the `yarn` Project**:
    
    - Move to your `yarn` project directory and run `yarn link <package-name>` to link the global symlink of the `pnpm` package into your project.
4. **Verify and Use**:
    
    - Import and use the linked package in your `yarn` project code as you would with any normal dependency.
5. **Maintain and Unlink**:
    
    - Rebuild the `pnpm` package as you make changes to see updates in the `yarn` project.
    - Use `yarn unlink <package-name>` in the `yarn` project and `yarn unlink` in the `pnpm` package to remove the link when no longer needed.

This process allows you to develop and test your `pnpm` workspace packages directly in a `yarn` project without publishing them.


[[2025-01-07]]:

- [ ] drag
- [ ] text input (today) (working)
- [ ] onboarding
- [ ] package release (today)

to do:
create a structured theme with lakshay
create animations



for controlled audio, show modal after fixed timeout
1. 
![[Screenshot 2025-01-09 at 5.26.08 PM.png]]

![[Screenshot 2025-01-09 at 5.26.08 PM (2).png]]

send pausedOnboarding data after each step



use cases - --1) comparision -> 2 + 5 -> on detail page --2) if family in search ,i.e., child > 1, then on srp trigger -> do you want to apply any family friendly filters --3) on city-> destination specific trigger for famous pois -> like agra, paris, ayodhya, puri, mathura, varanasi, ajmer, ujjain 4) on SRP -> on long scroll (10 hotels/20 hotels) -> trigger -> help with filters --5) if the user comes back to srp after 3 hotel detail pages, then ask him about that specific filter (like star rating/price) --6) if opens facilities section -> trigger -> can I help you find a facility XX 7) if user opens review -> trigger -> top reviews feedback on tap suggestions - 1) if one adult -> show filters like business travel filters 2) 2 adults -> couple friendly -> can be run as an experiment on trigger as well 3) hotels with mountain view etc based on that destination --4) based on destination + current situation (ayodhya innaugration) -> give suggestions like show hotels near ram janm bhoomi dissmiss -> how to handle dismiss




things rem: 
[[2025-01-11]]

1. drag fix done
2. mode status fix (show more status) done
3. ripples animation fix
4. 


[[2025-01-13]]

![[Screenshot 2025-01-13 at 1.29.09 AM.png]]
a bit more 3d look, inc padding, dark colors

done
![[Screenshot 2025-01-13 at 1.29.43 AM.png]]
3d look


on dismiss: a note 

on clicking outside, handle the re open view

![[Screenshot 2025-01-13 at 3.09.44 AM.png]]




good command learned:

```
pnpm why <packagename>

eg: 

pnpm why buffterutil
```
![[Screenshot 2025-01-15 at 6.10.34 PM.png]]




# SSE Implmentation:

Let me break down the SSE (Server-Sent Events) implementation step by step from your codebase:

1. **Core Stream Parser**

```45:83:shared/apis/utils.ts
export async function parseResponseStream<Chunk>(response: Response, onData: (chunk: {done: boolean, data: Chunk}) => void) {
    if (!response.ok) {
        throw new Error(response.statusText);
    }

    // This data is a ReadableStream
    const data = response.body;
    if (!data) {
        return;
    }

    const reader = data.getReader();
    const decoder = new TextDecoder();
    let done = false;
    while (!done) {
        const { value, done: doneReading } = await reader.read();
        done = doneReading;
        const chunkValue = decoder.decode(value);

        const parsedData = parseJsonChunk(chunkValue);

        if(typeof window !== 'undefined' && '_chunks' in window){
            // @ts-expect-error
            window._chunks.push({ value: chunkValue, done, parsedData });
        }
        parsedData.slice(0, parsedData.length - 1).forEach(data => {
            onData({
                done: false,
                data,
            });
        });
        const lastItem = parsedData[parsedData.length - 1];
        (lastItem || done) && onData({
            done,
            data: lastItem,
        });

    }
};
```


2. **JSON Chunk Parser**

```84:99:shared/apis/utils.ts
function parseJsonChunk(chunk: string) {
    let contentParts = (chunk || "").split("data: ").filter((part) => !!part);
    if (contentParts && contentParts.length) {
        const content = contentParts.map((part) => {
            try {
                const parsedData = JSON.parse(part);
                return parsedData;
            } catch (error) {
                console.log(error, chunk, part);
            }
            return;
        });
        return content.filter((part) => !!part);
    }
    return [];
}
```


Here's how to replicate this pattern:

1. **Create Stream Response Parser**:
```typescript
async function parseResponseStream<T>(
  response: Response, 
  onData: (chunk: {done: boolean, data: T}) => void
) {
  if (!response.ok) throw new Error(response.statusText);
  
  const data = response.body;
  if (!data) return;

  const reader = data.getReader();
  const decoder = new TextDecoder();
  let done = false;

  while (!done) {
    const { value, done: doneReading } = await reader.read();
    done = doneReading;
    const chunkValue = decoder.decode(value);
    
    const parsedData = parseJsonChunk(chunkValue);
    
    // Emit all chunks except last
    parsedData.slice(0, -1).forEach(data => {
      onData({ done: false, data });
    });

    // Emit last chunk with done status
    const lastItem = parsedData[parsedData.length - 1];
    if (lastItem || done) {
      onData({ done, data: lastItem });
    }
  }
}
```

2. **Create JSON Chunk Parser**:
```typescript
function parseJsonChunk(chunk: string) {
  const parts = chunk.split("data: ").filter(Boolean);
  
  if (parts.length) {
    return parts
      .map(part => {
        try {
          return JSON.parse(part);
        } catch (error) {
          console.error(error);
          return undefined;
        }
      })
      .filter(Boolean);
  }
  return [];
}
```

3. **Usage Example**:
```typescript
// API call
async function fetchStreamData(url: string) {
  const response = await fetch(url);
  return parseResponseStream(response, ({ data, done }) => {
    // Handle each chunk
    console.log(data);
    
    if (done) {
      // Stream complete
      console.log("Stream ended");
    }
  });
}

// React Component Usage
function YourComponent() {
  useEffect(() => {
    fetchStreamData('/api/stream')
      .catch(error => console.error(error));
  }, []);
  
  return <div>Streaming Data...</div>;
}
```

4. **Server-side Implementation (Node.js/Express)**:
```typescript
app.get('/api/stream', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  const sendChunk = (data: any) => {
    res.write(`data: ${JSON.stringify(data)}\n\n`);
  };

  // Send chunks
  sendChunk({ type: 'start', data: 'Starting stream' });
  
  // Send more chunks as needed
  const interval = setInterval(() => {
    sendChunk({ type: 'data', data: 'Some data' });
  }, 1000);

  // Cleanup
  req.on('close', () => {
    clearInterval(interval);
    res.end();
  });
});
```

Key Points:
- Uses ReadableStream API to read response chunks
- Decodes binary chunks to text using TextDecoder
- Parses SSE format ("data: " prefix)
- Handles JSON parsing with error recovery
- Emits chunks via callback with done status
- Supports early termination and cleanup

Your implementation also includes advanced features like:
- Chunk debugging (window._chunks)
- Custom error handling
- Stream state management
- Progress tracking

Let me know if you need more specific details about any part of the implementation!



[{"key":"User-Agent","value":"Mozilla/5.0 (Linux; Android 13; Pixel 7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Mobile Safari/537.36","enabled":true,"uuid":"f9e72c8f-4a6d-4d2f-8aca-9db4f3119409"},{"key":"apiKey","value":"undefined","enabled":false,"uuid":"a257da7a-591c-4eb8-9f70-ac245d0ce467"},{"key":"appVersion","value":"undefined","enabled":false,"uuid":"4ed1fc36-a7f2-4b0e-b157-a6965c3eb385"},{"key":"clientId","value":"undefined","enabled":false,"uuid":"92ee97aa-d3f3-4266-83b5-8f8c02bfa551"},{"key":"deviceId","value":"undefined","enabled":false,"uuid":"24102a07-ea26-485e-af79-029d0b1ca1f9"},{"key":"ixiSrc","value":"undefined","enabled":false,"uuid":"c08b999f-e1e5-478f-abda-0ec5edb03b9f"}]


active (normal)
Ai speaking
Input
loading


Tap to speak- active
Long press to type- on active after no activity (if no tap then 10 secs and full view is there)
want to repeat - (prashant discuss) (understanding long answers)
Tap outside to close  [_
I’m listening  - on listening
Ask me anything  
// you have a great sense of humour  (adding in resposne)
you are awesome  
it was great planning with you 
Want to compare hotels 


tap to speak -> 10 secs later long press -> 15 sec later tap outside


tap / ask me anything / want to repeat



(feedback kind of system )




### the fab animation:  when no interaction form user (not opened the full view)

[[2025-02-05]] todo:

fix that onboarding timings


TODO:

error boundary (working on it)
text view UI -done
Mode status (as above) -done
cookie/local storage -done

**discussed with lakshay**
- error management
- chips (blue fade remove), also no highlight need
- prashant discuss, the suggestions in text mode 

REVIEW ONCE:
- context provider working done
- app Info provider working done
- complex animation slight fix done
- suggestion Chips done


listening
while speaking, don't show any thing.

Final revision:
- mode status show time
- the drag smoothness
- complex animation view time
- context passing correct or not
- error boundary

[[2025-02-11]]
clevartap events:

chat response-  
 user query  
 API response time  
 voice/text  
 user initiated / trigger  
 if error  
 product and page
 
- [x] query
- [x] context (page/product)
- [ ] initiation
- [ ] mode(text/voice)
- [x] API response time



 extra events-dismiss-  





drag-create API in backend for logger  
UTs make for the events

- [ ] suggestions on playback end
- [ ] on exit add some rotation twist
- [ ] nudges shimmer
- [ ] text input
- [ ] filler is loading state
- [x] no scale up anywhere
- [ ] dismiss area UI

[[2025-02-12]]:
discussion on A/B testing
firebase A/B tseting setup


# what possible errors: [[error handling]]

testing on build (all the funnels)


mweb vs in app
desing changes
ping lakshay


[[2025-02-15]]
sonar testing done
all test cases passing
deployment side sorted out.

Rem: 
small development phase.
animation smoothness.
package refactor imprvement.


[[2025-02-19]]





Edge cases ->
bluetooth listening, speaker



iso changes:

on text mode, mic button on click go to voice mode
on left icon, go to voice mode


[[2025-03-17]]

test version: sdk-> v16, utils-> v6



[[next release]]