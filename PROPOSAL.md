<div align="center">

# AI Persona

### A real-time talking avatar that acts on your accounts, asks before it acts, and remembers you

**[Website](https://aipersona.ahmedhussain.tech)** &nbsp;·&nbsp; **[Demo video](https://github.com/ahmed-hussain-ai/aipersona/blob/main/demo/aipersona-web-demo.mp4)** &nbsp;·&nbsp; **[Screenshots and material](https://github.com/ahmed-hussain-ai/aipersona)**

<img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-assistant-conversation.png" alt="The AI Persona assistant: the avatar in its scene with the conversation beside it" width="900">

</div>

| Team | Ahmed Hussain and Maria |
|:---|:---|
| **Institution** | Department of Artificial Intelligence, COMSATS University Islamabad, Lahore Campus |
| **Status** | A working prototype, not a concept: an Android app, a web app and the full server system are built and running |
| **Try it** | https://aipersona.ahmedhussain.tech |

## At a glance

| A face that speaks | Fast enough to talk to | Tested on a real account | Safe to trust |
|:---:|:---:|:---:|:---:|
| **26.3 frames per second** | **0.9 seconds** | **98% correct** | **Every change confirmed** |
| A talking avatar generated from the assistant's own speech on one GPU | From the end of the user's speech to the start of the reply (median) | On our 510-request benchmark, run end to end with real Gmail, Calendar, Drive and Contacts | Nothing is sent, booked or deleted without a yes |

**Contents:** [1. Overview and Problem](#1-project-overview-and-problem-statement) · [2. Solution](#2-proposed-solution) · [3. Objectives and Outcomes](#3-objectives-and-expected-outcomes) · [4. Target Users](#4-target-users) · [5. Features and Deliverables](#5-key-features-and-deliverables) · [6. Plan and Timeline](#6-implementation-plan-and-timeline) · [7. Resources](#7-resources-and-requirements) · [8. Risks](#8-potential-challenges-and-risks) · [9. Additional Details](#9-additional-details)

---

## 1. Project Overview and Problem Statement

People run their day across apps that do not talk to each other: mail, calendar, contacts, files, a to-do list. Every small job means opening an app, tapping, and reading on a small screen.

Assistants should solve this. The ones people have today do not:

| Today's option | What it does well | Where it falls short |
|---|---|---|
| **Phone assistants** (Siri, Google Assistant, Alexa) | Fast and always available | Act on personal data in only a few fixed ways, rarely confirm before acting, and guess or give up on a misheard name |
| **Chat assistants** (ChatGPT, Claude and similar) | Understand almost any request | Built for typing: replies take several seconds, they cannot be interrupted, and a tool call can invent a detail such as an email address |
| **Voice-agent platforms** | Let anyone build a talking agent | A voice alone is ordinary today, and many teams now build one |
| **All of them** | | Do not remember the person well across conversations, and **none has a face** |

> **The problem we address:** there is no assistant a person can both **see** and **trust with their own accounts**: one with a face that speaks in real time, fast enough for a real conversation, safe enough to send an email, and personal enough to know who "my supervisor" is.

---

## 2. Proposed Solution

AI Persona is a personal assistant with a face. On the phone and in the browser, the user sees an avatar, talks to it like a person, and it answers in a natural voice while its lips, eyes and head move with the speech.

**The avatar is the centre of the product, not decoration.** It is generated from the assistant's own speech by a talking-head model, faster than real time on a single GPU, and it can be made from one photograph, so each user can have their own persona.

Behind the face is an agent that does real work. One spoken sentence is enough:

> "Email Maria and ask her for the slides by 5 PM. If she hasn't sent them by then, send a follow-up, and when she does, thank her."

The assistant finds Maria in the user's contacts, writes the email, shows it on a card and waits for a "yes". It then watches the inbox and does the right thing later, even when the user is away, and tells them what happened.

### From a spoken sentence to a finished task

```
Speak  ->  Understand  ->  Plan  ->  Show and wait for "yes"  ->  Act  ->  Report back

Every reply is spoken by the avatar, with its lips in step with the voice.
```

### What that looks like in the app

<table>
<tr>
<td align="center" width="33%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-asks-which-ali.png" width="240"><br><b>1. It asks, it never guesses</b><br><sub>Two contacts are called Ali, so the avatar asks which one.</sub></td>
<td align="center" width="33%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-email-confirm.png" width="240"><br><b>2. Nothing is sent without a yes</b><br><sub>The finished email waits on a card while the avatar reads it out.</sub></td>
<td align="center" width="33%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-email-sent.png" width="240"><br><b>3. Done, and said so</b><br><sub>After the yes, the card shows the email as sent.</sub></td>
</tr>
</table>

### How it is built

| Part | Built with | What it does |
|---|---|---|
| **Mobile app** | React Native (Android) | Microphone, voice playback, cards, memory screen, avatar |
| **Web app** | React | The same assistant in a browser |
| **Account backend** | Node.js, MongoDB | Accounts, login, each user's connected tools |
| **Agent engine** | Python, FastAPI | The voice pipeline, tool choice, planning, confirmation, memory, speech |
| **Avatar server** | Ditto talking-head model on a GPU | Turns the assistant's speech into a talking face |

> **One design rule runs through the whole system:** the language model decides what the user *means*; ordinary code decides what *happens*. The model reads the sentence, but code checks the name, builds the confirmation, and refuses anything the user did not approve.

---

## 3. Objectives and Expected Outcomes

| Objective | Target | Where we are |
|---|---|---|
| **A real-time face** | 25 frames per second on one GPU, first frame within half a second | Achieved on the avatar server: 26.3 frames per second on one RTX 4090 in streaming mode, 258 ms of compute to the first frame. Demonstrated live at the final |
| **A reply fast enough to feel like conversation** | The reply starts within 1 second of the user finishing | Measured on the live system: about 0.9 seconds (median) from the end of the user's speech to the start of the spoken reply, 0.77 seconds at its fastest |
| **No change without permission** | Every email, booking and deletion confirmed first | Built in: every action that changes something goes through a confirmation step |
| **Never act on a misheard name** | Ask instead of guess | Built in: "Did you mean Ali Hussain or Ali Malik?" |
| **Remember the user** | Facts kept across sessions, visible and removable by the user | Working: a memory graph the user can see and edit |
| **Many users, separate data** | Each user's own account, tools and memory | Working |

### Evidence so far: tested, not just built

| What we tested | Result |
|---|---|
| **The whole system on a live account,** with every tool real. A request passes only if every check on it holds: the right tool, the right details, the right reply | |
| &nbsp;&nbsp;510-request benchmark | **98%** (90% on the first run, before fixing what it found) |
| &nbsp;&nbsp;512 new requests the system had never seen | **90%** |
| **Memory.** 50 questions from ten angles: working out an unstated relation, a wrong premise, counting, before and now, misheard names, Roman Urdu | **50 of 50** answered correctly |
| **Reply speed** on the live system | **0.9 seconds** (median) from the end of speech to the start of the reply |
| **Avatar** on one RTX 4090 | **26.3 frames per second**, 1 late frame in 430 |
| **Automated tests** of the engine | **850 tests** |

**Expected outcome:** a persona a person can see and talk to every day, that runs their email, calendar, files and tasks, with nothing done behind their back.

---

## 4. Target Users

| Who | Why AI Persona fits |
|---|---|
| **Students and professionals** | Their work is spread over email, calendar and files |
| **People whose hands or eyes are busy** | Driving, cooking, walking: speaking is the only practical way to use a phone |
| **People with low vision or difficulty typing** | Voice and a face replace small text and small buttons |
| **Speakers of Pakistani English and Roman Urdu** | Built to understand broken English, Roman Urdu words and local names, with local touches such as prayer-time reminders for the user's city |

---

## 5. Key Features and Deliverables

| Feature | What the user gets |
|---|---|
| **A real-time talking avatar** | A face generated from the assistant's speech, with lips, blinking and head movement, at video rate on one GPU. Made from a single photograph (a default persona or the user's own) and shown in a scene the user picks |
| **Acts on the user's own accounts** | Gmail (read, search, write, reply, send), Google Calendar (check, book, move, cancel; understands "the day after tomorrow", "next Friday", "kal"), Contacts and Google Drive (found by meaning, not exact spelling), to-do list, reminders, weather, web search, nearby places |
| **Answers from the user's documents** | Upload a PDF and ask questions about it; the source is named |
| **Works while the user is away** | Conditional tasks in one sentence ("if she doesn't reply by 5, send a follow-up; if she does, thank her"), with a phone notification saying what was done |
| **Safe by design** | Nothing is sent, booked or deleted without a "yes". An unclear name is asked about, never guessed. Text inside an incoming email is treated as data, never as an instruction |
| **Remembers, and the user is in control** | Learns facts from conversation and links them in a graph, so it can answer "how is Waleed related to Sajid?" from what it was told. The user can see, correct or forget any of it, by tap or by voice |
| **Feels like a conversation** | The reply starts about 0.9 seconds after the user stops speaking, and the assistant can be interrupted at any moment; it tells real interruptions from "okay" and "yeah" |
| **Open to other apps** | Any app that offers an MCP server can be connected as the user's own tool |

<div align="center">
<img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-memory-graph.png" alt="The memory graph" width="860">
<br><sub><b>The memory graph.</b> What the assistant has learned about the user: people, places and things, with the relation written on each line.</sub>
</div>

**Deliverables:** the Android app, the web app, the server system, an evaluation report, and a live demo.

---

## 6. Implementation Plan and Timeline

| Period | Work | State |
|---|---|:---:|
| **Dec 2025** | Proposal and design | Done |
| **Jan to Sep 2026** | Real-time voice pipeline, the agent and its tools, confirmation flow, memory, documents, accounts for many users, the Android app, avatar model and benchmark | Done |
| **Oct 2026** | Scheduled tasks, memory graph, web app, the avatar in the app and on the web with selectable scenes, the live avatar demo | Shown live at the final on 24 October |
| **Nov 2026** | Fine-tune the avatar model on our own personas for sharper mouth detail and a steadier face; avatars from the user's own photo for every user | Planned |
| **Dec 2026** | Emotion: the avatar's tone and expression follow the conversation; Urdu speech alongside English | Planned |
| **Jan 2027** | Testing with outside users, reliability (backups, monitoring, alerts), first public release | Planned |

---

## 7. Resources and Requirements

| Resource | What we use |
|---|---|
| **Server** | One small cloud server (2 CPU cores, 8 GB memory) runs the engine, the backend and the website today |
| **GPU** | One NVIDIA RTX 4090 (rented) for the live avatar. This is the main cost |
| **Cloud services** | Speech-to-text and text-to-speech (Deepgram), fast language models (Cerebras, with Groq and OpenAI as fallbacks), Google APIs for the user's own accounts, Firebase for notifications |
| **Our own code** | About 89,000 lines across four codebases (agent engine, mobile app, web app, account backend), with 12 tool agents |
| **Free and open parts** | Weather, prayer times, maps, web search and OCR all use free services or open-source software |

---

## 8. Potential Challenges and Risks

| Risk | What we do about it |
|---|---|
| **Misheard names**, especially Urdu names written in many spellings | Names are matched by meaning and sound, the user's own contacts are given to the recogniser as hints, and an unsure name becomes a question |
| **The model inventing facts** | Answers about the user come only from stored memory; a change is always shown and confirmed; "I don't know yet" is an allowed answer |
| **GPU cost of a live avatar** | One GPU serves the avatar faster than real time; a lighter pre-rendered avatar is the fallback when no GPU is available |
| **Privacy of personal data** | Each user's data is kept separate; Google access is the user's own and can be switched off per tool; memory is visible and removable |
| **Dependence on outside services** | Three language-model providers in a fallback chain; free services have a second source |
| **Latency on weak networks** | The pipeline overlaps its steps and streams speech in small pieces |

---

## 9. Additional Details

### Why this project

| The championship looks for | What AI Persona brings |
|---|---|
| **Innovation** | A live talking avatar joined to an agent that really acts. Voice agents are common; a face that speaks in real time on top of a working, trusted agent is not. Its memory is a graph the user can see and edit, not a hidden profile |
| **Technical depth** | A real-time voice pipeline with a 0.9 second reply, a planner for requests with several parts, a confirmation step enforced in code, tasks that run later under a condition, a graph memory, and a talking-head model running faster than real time. All of it built, measured and tested |
| **Real-world impact** | It works today on real Gmail, Calendar, Drive and Contacts. It keeps working after the conversation ends and reports back. It is built for how people here actually speak: Pakistani English, Roman Urdu and local names |

### Where it can go next

The personal assistant is the first product, not the only one. A live avatar, a fast voice loop, an agent that uses tools safely and a memory of the person are what any talking persona needs: a teacher that remembers each student, a receptionist, a support agent. We are keeping the scope to the personal assistant until it is solid; these are directions, not promises.

### Demo video

A screen recording of the assistant in use: the avatar speaking, the memory graph, mail, calendar, contacts, voices and connected tools.

**[Watch the demo video](https://github.com/ahmed-hussain-ai/aipersona/blob/main/demo/aipersona-web-demo.mp4)**

### More screenshots

**The web app**

<table>
<tr>
<td align="center" width="50%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-assistant.png" width="430"><br><sub><b>The avatar speaking</b>, with its reply shown over the scene.</sub></td>
<td align="center" width="50%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-today.png" width="430"><br><sub><b>The Today page:</b> mail, calendar, contacts and what is coming up.</sub></td>
</tr>
</table>

**The mobile app**

<table>
<tr>
<td align="center" width="25%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-weather-card.png" width="200"><br><sub><b>Answers with a card.</b> The avatar speaks the weather and the card shows the figures.</sub></td>
<td align="center" width="25%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-memory-about-you.png" width="200"><br><sub><b>What it knows about you.</b> Facts sorted into groups by itself.</sub></td>
<td align="center" width="25%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-memory-graph.png" width="200"><br><sub><b>The memory graph</b> on the phone.</sub></td>
<td align="center" width="25%"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-profile.png" width="200"><br><sub><b>The user's own assistant.</b> Voice, microphone and connected apps.</sub></td>
</tr>
</table>

---

<div align="center">

**AI Persona** &nbsp;·&nbsp; Ahmed Hussain and Maria &nbsp;·&nbsp; COMSATS University Islamabad, Lahore Campus

[Website](https://aipersona.ahmedhussain.tech) &nbsp;·&nbsp; [Demo video](https://github.com/ahmed-hussain-ai/aipersona/blob/main/demo/aipersona-web-demo.mp4) &nbsp;·&nbsp; [GitHub](https://github.com/ahmed-hussain-ai/aipersona)

</div>
