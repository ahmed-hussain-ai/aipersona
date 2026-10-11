# AI Persona: a real-time talking avatar that acts on your accounts, asks before it acts, and remembers you

**Team:** Ahmed Hussain and Maria, Department of Artificial Intelligence, COMSATS University Islamabad, Lahore Campus  
**Status:** built and running: an Android app, a web app and the full server system, developed from January to October 2026  
**Website:** https://aipersona.ahmedhussain.tech

---

## 1. Project Overview and Problem Statement

A student or a professional keeps their day in apps that do not talk to each other: the calendar, the mail, the contacts, the files, a to-do list. Every small job (check a meeting, reply to someone, find a file) means opening an app, tapping and reading on a small screen.

Voice assistants should solve this, but the ones people have today do not:

- **Phone assistants (Siri, Google Assistant, Alexa)** are fast, but they act on personal data in only a few fixed ways. They rarely confirm before acting, and when a name is misheard they guess or give up.
- **Chat assistants (ChatGPT, Claude and similar)** understand almost any request, but they are built for typing. Replies take several seconds, they cannot be interrupted mid-sentence, and when they call a tool they can invent a detail such as an email address and act on it.
- **Voice-agent platforms** let anyone build a talking agent, and many teams now do. A voice alone is ordinary today.
- **None of them remembers the person** well across conversations, and **none has a face.** Talking to a voice with no face is tiring; people talk most naturally to someone they can see.

The problem we address: **there is no assistant a person can both see and trust with their own accounts**, one with a face that speaks in real time, fast enough for a real conversation, safe enough to send an email, and personal enough to know who "my supervisor" is.

## 2. Proposed Solution

AI Persona is a personal assistant with a face. On the phone and in the browser, the user sees an avatar, talks to it like a person, and it answers in a natural voice while its lips, eyes and head move with the speech.

**The avatar is the centre of the product, not decoration.** It is generated live from the assistant's own speech by a talking-head model, faster than real time on a single GPU, and it can be made from one photograph, so each user can have their own persona.

Behind the face is an agent that does real work:

> "Email Maria and ask her for the slides by 5 PM. If she hasn't sent them by then, send a follow-up, and when she does, thank her."

The assistant understands the request, finds Maria in the user's contacts, writes the email, shows it on a card, and waits for a "yes". It then watches the inbox and does the right thing later, even when the user is away, and tells them what happened.

The system has four parts:

| Part | Built with | What it does |
|---|---|---|
| Mobile app | React Native (Android) | Microphone, voice playback, cards, memory screen, avatar |
| Web app | React | The same assistant in a browser |
| Account backend | Node.js, MongoDB | Accounts, login, each user's connected tools |
| Agent engine | Python, FastAPI | The voice pipeline, tool choice, planning, confirmation, memory, speech |
| Avatar server | Ditto talking-head model on a GPU | Turns the assistant's speech into a talking face |

One design rule runs through the whole system: **the language model decides what the user means; ordinary code decides what happens.** The model reads the sentence, but code checks the name, builds the confirmation, and refuses anything the user did not approve.

## 3. Objectives and Expected Outcomes

| Objective | Target | Where we are |
|---|---|---|
| A real-time face | 25 frames per second on one GPU, first frame within half a second | Achieved on the avatar server: 26.3 frames per second on one RTX 4090 in streaming mode, 258 ms of compute to the first frame. Demonstrated live at the final |
| A reply fast enough to feel like conversation | The reply starts within 1 second of the user finishing | Measured on the live system: about 0.9 seconds (median) from the end of the user's speech to the start of the spoken reply, 0.77 seconds at its fastest |
| No change without permission | Every email, booking, deletion confirmed first | Built in: every action that changes something goes through a confirmation step |
| Never act on a misheard name | Ask instead of guess | Built in: "Did you mean Ali Hussain or Ali Malik?" |
| Remember the user | Facts kept across sessions, visible and removable by the user | Working: a memory graph the user can see and edit |
| Many users, separate data | Each user's own account, tools and memory | Working |

Expected outcome: a persona a person can see and talk to every day, that runs their email, calendar, files and tasks, with nothing done behind their back.

## 4. Target Users

- **Students and professionals** whose work is spread over email, calendar and files.
- **People whose hands or eyes are busy**: driving, cooking, walking.
- **People with low vision or difficulty typing**, for whom voice is the practical way to use a phone.
- **Speakers of Pakistani English and Roman Urdu.** The assistant is built to understand broken English, Roman Urdu words and local names, and it includes local touches such as prayer-time reminders for the user's city.

## 5. Key Features and Deliverables

**A real-time talking avatar**
- A face generated live from the assistant's speech: lips, blinking and head movement, at video rate on one GPU.
- Made from a single photograph: a default persona or the user's own.
- Shown in a scene the user picks (or with no background), in the app and on the web.

**Acts on the user's own accounts**
- Gmail: read, search, write, reply and send, always with a confirmation card.
- Google Calendar: check, book, move and cancel meetings; understands "the day after tomorrow", "next Friday", "kal".
- Contacts and Google Drive: find people and files by meaning, not exact spelling.
- To-do list, reminders (including daily and prayer-time reminders), weather, web search, nearby places.
- The user's own documents: upload a PDF and ask questions about it, with the source named.

**Works while the user is away**
- Conditional tasks in one sentence: "if she doesn't reply by 5, send a follow-up; if she does, thank her."
- A phone notification tells the user what was done.

**Safe by design**
- Nothing is sent, booked or deleted without a "yes".
- An unclear name is asked about, never guessed.
- Text inside an incoming email is treated as data, never as an instruction.

**Remembers, and the user is in control**
- Learns facts from conversation (family, work, plans) and links them in a graph, so it can answer "how is Waleed related to Sajid?" from what it was told.
- A Memory screen shows everything it knows; the user can correct or forget any of it, by tap or by voice.

**Feels like a conversation**
- Can be interrupted at any moment; tells real interruptions from "okay" and "yeah".
- The reply starts about 0.9 seconds after the user stops speaking.

**Open to other apps**
- Any app that offers an MCP server can be connected as the user's own tool.

**Deliverables:** the Android app, the web app, the server system, an evaluation report, and a demo.

## 6. Implementation Plan and Timeline

| Period | Work | State |
|---|---|---|
| Dec 2025 | Proposal and design | Done |
| Jan to Sep 2026 | Real-time voice pipeline, the agent and its tools, confirmation flow, memory, documents, accounts for many users, the Android app, avatar model and benchmark | Done |
| Oct 2026 | Scheduled tasks, memory graph, web app, the avatar in the app and on the web with selectable scenes, the live avatar demo | Shown live at the final on 24 October |
| Nov 2026 | Fine-tune the avatar model on our own personas for sharper mouth detail and a steadier face; avatars from the user's own photo for every user | Planned |
| Dec 2026 | Emotion: the avatar's tone and expression follow the conversation; Urdu speech alongside English | Planned |
| Jan 2027 | Testing with outside users, reliability (backups, monitoring, alerts), first public release | Planned |

## 7. Resources and Requirements

- **Server:** one small cloud server (2 CPU cores, 8 GB memory) runs the engine, the backend and the website today.
- **GPU:** one NVIDIA RTX 4090 (rented) for the live avatar. This is the main cost.
- **Cloud services:** speech-to-text and text-to-speech (Deepgram), fast language models (Cerebras, with Groq and OpenAI as fallbacks), Google APIs for the user's own accounts, Firebase for notifications.
- **Free and open parts:** weather, prayer times, maps, web search and OCR all use free services or open-source software.

## 8. Potential Challenges and Risks

| Risk | What we do about it |
|---|---|
| **Misheard names**, especially Urdu names, written in many spellings | Names are matched by meaning and sound, the user's own contacts are given to the recogniser as hints, and an unsure name becomes a question |
| **The model inventing facts** | Answers about the user come only from stored memory; a change is always shown and confirmed; "I don't know yet" is an allowed answer |
| **GPU cost of a live avatar** | One GPU serves the avatar faster than real time; a lighter pre-rendered avatar is the fallback when no GPU is available |
| **Privacy of personal data** | Each user's data is kept separate; Google access is the user's own and can be switched off per tool; memory is visible and removable |
| **Dependence on outside services** | Three language-model providers in a fallback chain; free services have a second source |
| **Latency on weak networks** | The pipeline overlaps its steps and streams speech in small pieces |

## 9. Additional Details

**What makes it different**

1. It has a face that speaks in real time. Voice agents are common; a live avatar joined to a working agent is not.
2. It acts, and it asks first. Most assistants do one or the other.
3. It keeps working after the conversation ends, and reports back.
4. Its memory is a graph the user can see, not a hidden profile.
5. It is built for how people here actually speak.

**Where it can go next**

The personal assistant is the first product, not the only one. The same four parts (a live avatar, a fast voice loop, an agent that uses tools safely, and a memory of the person) are what any talking persona needs. With different tools and knowledge the same system can become a teacher that explains a course and remembers each student, a receptionist or support agent for a business, or a guide for a service. We are keeping the scope to the personal assistant until it is solid, and we treat these as directions, not promises.

**Screenshots**

**The web app**

*The assistant on the web: the avatar in its scene, with the conversation beside it.*

![The assistant with the avatar and the conversation](https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-assistant-conversation.png)

*The avatar speaking, with the reply shown over the scene.*

![The avatar speaking](https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-assistant.png)

*The memory graph: what the assistant has learned about the user, as people, places and things with the relation written on each line.*

![The memory graph](https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-memory-graph.png)

*The Today page: mail, calendar, contacts and what is coming up.*

![The Today page](https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/web-today.png)

**The mobile app**

<table>
<tr>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-asks-which-ali.png" width="250"><br><sub><b>It asks, it never guesses.</b> Two contacts are called Ali, so the avatar asks which one.</sub></td>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-email-confirm.png" width="250"><br><sub><b>Nothing is sent without a yes.</b> The finished email waits on a card while the avatar reads it out.</sub></td>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-email-sent.png" width="250"><br><sub><b>Done, and said so.</b> After the yes, the card shows the email as sent.</sub></td>
</tr>
<tr>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-weather-card.png" width="250"><br><sub><b>Answers with a card.</b> The avatar speaks the weather and the card shows the figures.</sub></td>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-memory-about-you.png" width="250"><br><sub><b>What it knows about you.</b> Facts it learned in conversation, sorted into groups by itself.</sub></td>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-memory-graph.png" width="250"><br><sub><b>The memory graph.</b> People, places and things, linked the way the user described them.</sub></td>
</tr>
</table>

<table>
<tr>
<td align="center"><img src="https://raw.githubusercontent.com/ahmed-hussain-ai/aipersona/main/screenshots/app-profile.png" width="250"><br><sub><b>The user's own assistant.</b> Their voice, microphone mode and connected Google apps.</sub></td>
</tr>
</table>

**Demo video**

A screen recording of the assistant in use: the avatar speaking, a request carried out, and the confirmation before it acts.

[Watch the demo video](https://github.com/ahmed-hussain-ai/aipersona/blob/main/demo/aipersona-web-demo.mp4)

**Supporting material:** https://github.com/ahmed-hussain-ai/aipersona
