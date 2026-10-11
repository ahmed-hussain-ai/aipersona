<div align="center">

# AI Persona

### A real-time talking avatar that acts on your accounts, asks before it acts, and remembers you

**[Read the full proposal](PROPOSAL.md)** &nbsp;·&nbsp; **[Watch the demo video](https://aipersona.ahmedhussain.tech/assets/aipersona-web-demo.mp4)** &nbsp;·&nbsp; **[Website](https://aipersona.ahmedhussain.tech)** &nbsp;·&nbsp; **[Proposal as PDF](AI_Persona_Proposal.pdf)**

<img src="screenshots/web-assistant-conversation.png" alt="The AI Persona assistant: the avatar in its scene with the conversation beside it" width="900">

</div>

AI Persona is a personal assistant with a face. You see an avatar, talk to it like a person, and it answers in a natural voice while its lips, eyes and head move with the speech. Behind the face is an agent that does real work on your own email, calendar, contacts and files, and never changes anything without your "yes".

**Team:** Ahmed Hussain and Maria Bano, Department of Artificial Intelligence, COMSATS University Islamabad, Lahore Campus.

## At a glance

| A face that speaks | Fast enough to talk to | Tested on a real account | Safe to trust |
|:---:|:---:|:---:|:---:|
| **26.3 frames per second** | **0.9 seconds** | **98% correct** | **Every change confirmed** |
| A talking avatar generated from the assistant's own speech on one GPU | From the end of the user's speech to the start of the reply (median) | On our 510-request benchmark, run end to end with real Gmail, Calendar, Drive and Contacts | Nothing is sent, booked or deleted without a yes |

## How one request works

<table>
<tr>
<td align="center" width="33%"><img src="screenshots/app-asks-which-ali.png" width="240"><br><b>1. It asks, it never guesses</b><br><sub>Two contacts are called Ali, so the avatar asks which one.</sub></td>
<td align="center" width="33%"><img src="screenshots/app-email-confirm.png" width="240"><br><b>2. Nothing is sent without a yes</b><br><sub>The finished email waits on a card while the avatar reads it out.</sub></td>
<td align="center" width="33%"><img src="screenshots/app-email-sent.png" width="240"><br><b>3. Done, and said so</b><br><sub>After the yes, the card shows the email as sent.</sub></td>
</tr>
</table>

## What it does

| Feature | What the user gets |
|---|---|
| **A face that speaks** | An avatar generated from the assistant's own speech, at video rate on one GPU, made from a single photograph |
| **Acts on your accounts** | Gmail, Google Calendar, Contacts, Drive, to-do list, reminders, weather, web search, nearby places, your own documents |
| **Asks before it acts** | Every email, booking and deletion is shown on a card and waits for a yes; an unclear name is asked about, never guessed |
| **Works while you are away** | "If she hasn't replied by 5, send a follow-up; if she does, thank her", and it tells you what happened |
| **Remembers you** | Learns facts from conversation and keeps them in a graph you can see, correct and forget |
| **Fast** | The reply starts about 0.9 seconds after you stop speaking |

<div align="center">
<img src="screenshots/web-memory-graph.png" alt="The memory graph" width="860">
<br><sub><b>The memory graph.</b> What the assistant has learned about the user: people, places and things, with the relation written on each line.</sub>
</div>

## How it is built

| Part | Built with |
|---|---|
| **Mobile app** | React Native (Android) |
| **Web app** | React |
| **Account backend** | Node.js, MongoDB |
| **Agent engine** | Python, FastAPI |
| **Avatar server** | Ditto talking-head model on an NVIDIA RTX 4090 |

## In this repository

| File | What it is |
|---|---|
| [PROPOSAL.md](PROPOSAL.md) | The full project proposal |
| [AI_Persona_Proposal.pdf](AI_Persona_Proposal.pdf) | The same proposal as a PDF |
| [demo/](demo) | The demo video |
| [screenshots/](screenshots) | Screenshots of the web app and the mobile app |

The source code is kept in private repositories.
