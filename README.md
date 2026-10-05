# RoastMe

RoastMe 🔥

Let AI judge you.

RoastMe is an AI-powered social entertainment platform where users upload photos, screenshots, playlists, rooms, outfits, Instagram profiles, chats, and other content to receive a personalised, funny AI roast.

The platform uses a two-model AI architecture:

                         INTERNET
                            │
                            ▼
                    ┌───────────────┐
                    │    VERCEL     │
                    │   Next.js UI  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ RoastMe API   │
                    │   FastAPI     │
                    └───────┬───────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │   Virtual GPU    │
                   │     Server       │
                   └────────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
          ┌─────────────┐       ┌─────────────┐
          │ Qwen3-VL   │       │  Roast LLM  │
          │ Vision AI  │       │ Text Model  │
          └──────┬──────┘       └──────┬──────┘
                 │                     │
                 ▼                     │
          Image Analysis               │
                 │                     │
                 └──────────┬──────────┘
                            ▼
                     Final Roast
                            │
                            ▼
                    Shareable Card

The goal is to build a real, scalable product, not a simple AI wrapper.

1. Product Vision

RoastMe turns ordinary user content into entertaining, personalised, highly shareable AI-generated roasts.

The primary growth mechanism is:

Create roast
     ↓
Share roast
     ↓
Friend sees roast
     ↓
Friend clicks RoastMe
     ↓
Friend creates roast
     ↓
Friend shares
     ↓
More users

The product should therefore optimise for:

Speed
Humour
Personalisation
Shareability
Simplicity
Repeat usage
2. Core Product

The primary experience is:

Upload something
       ↓
Choose roast intensity
       ↓
AI analyses content
       ↓
AI generates roast
       ↓
User receives roast score
       ↓
Beautiful roast card
       ↓
Share

The first version should not require account creation.

3. Supported Input Types

Initial supported inputs:

1. Photo

Examples:

Selfie
Outfit
Room
Desk
Car
Food
Pet
Setup
2. Instagram screenshot

AI analyses visible:

Grid
Bio
Captions
Visual style
Repeated themes
Posting aesthetic
3. Chat screenshot

Examples:

WhatsApp
Instagram DM
Messenger
Discord

The AI should only analyse information actually visible in the uploaded screenshot.

4. Spotify screenshot

Analyse:

Artists
Songs
Genres
Repeated listening patterns
Playlist names
Visible music choices
4. Future Input Types

Possible future additions:

LinkedIn profile
Dating profile
Desktop screenshot
Phone home screen
Browser tabs
Gaming setup
GitHub profile
Resume
YouTube subscriptions
Camera roll screenshot
Search history screenshot
Friend group photo

These should not be implemented in V1.

5. Roast Modes
Roast Intensity
Light

Friendly teasing.

Savage

More direct.

Brutal

Strong roast.

Nuclear

Maximum comedic intensity while remaining within safety rules.

The intensity should affect the prompt and generation style, not safety boundaries.

6. Roast Styles

Initial styles:

Gen Z
Sarcastic
Dry humour
Best Friend
Stand-up
Deadpan

Future:

Indian Gen-Z
Corporate
Gamer
Programmer
Bollywood-style
Mumbai-style
College student
Luxury critic
7. AI Architecture

The most important architectural decision is to separate visual understanding from roast generation.

Stage 1: Vision
IMAGE
 ↓
Qwen3-VL
 ↓
Structured observations

Qwen3-VL is a multimodal vision-language model from the Qwen team. The official project supports image/video understanding, OCR, spatial reasoning and other multimodal capabilities. The repository is Apache-2.0 licensed.

Stage 2: Roast
Structured observations
        ↓
Roast LLM
        ↓
Final roast

The roast model does not need to see the original image in the basic architecture.

This provides:

Lower compute requirements
Easier debugging
Better prompt control
Model interchangeability
Easier fine-tuning
Better privacy separation
8. Vision Output

Qwen3-VL should NOT simply return a paragraph.

The vision service should return structured JSON.

Example:

{
  "input_type": "room",
  "subject": "bedroom",
  "visual_observations": [
    "gaming setup",
    "RGB lighting",
    "clothes placed on chair",
    "large monitor",
    "water bottle on desk"
  ],
  "environment": {
    "cleanliness": 58,
    "organisation": 42,
    "aesthetic": "gaming"
  },
  "notable_details": [
    "chair appears to be holding clothes",
    "desk has gaming equipment",
    "RGB lighting dominates the setup"
  ],
  "overall_vibe": "gaming setup with moderate mess"
}

The model must only describe things that can actually be observed.

It must not invent hidden information.

9. Roast Output

The roast LLM should return structured JSON.

{
  "score": 87,
  "title": "Certified Chaos",
  "roast": "Your chair has officially retired from being furniture and started working as a clothes storage facility.",
  "verdict": "The gaming setup is serious. The organisation clearly isn't.",
  "stats": {
    "chaos": 91,
    "taste": 72,
    "energy": 88,
    "main_character": 79
  },
  "share_caption": "I let AI roast my room 💀"
}
10. Roast Score

Score range:

0–100

Possible interpretation:

0–20     Innocent
21–40    Mild
41–60    Questionable
61–80    Getting roasted
81–95    Absolutely cooked
96–100   Nuclear

The score should measure roastability/comedic material, not attractiveness or personal worth.

11. Roast Categories

Possible generated stats:

Chaos
Taste
Energy
Main Character
Delusion
Organisation
Drip
NPC Level

Stats should depend on input type.

For example, a room could use:

Chaos
Organisation
Aesthetic
Gaming Energy

An Instagram profile could use:

Main Character
Aesthetic
Try-hard
Personality
Flex Level

A playlist could use:

Taste
Sadness
Main Character
Nostalgia
Chaos
12. Safety System

The application must contain a safety layer.

The AI must not generate insults based on:

Race
Ethnicity
Religion
Disability
Sexual orientation
Gender identity
Protected characteristics
Medical conditions

It should also avoid:

Sexual content involving minors
Sexual harassment
Threats
Encouragement of self-harm
Dangerous behaviour
Doxxing
Personal information exposure

The roast should target visible choices and content, not sensitive personal attributes.

Example:

Allowed

"Your room has the organisational skills of a browser with 47 tabs open."

Not allowed

Insulting someone because of their religion, ethnicity, disability, etc.

13. Backend

Use:

Python + FastAPI

Why:

Excellent AI ecosystem
Easy model integration
Async API support
Easy deployment
Easy integration with vLLM
Easy JSON validation with Pydantic

Architecture:

backend/
│
├── main.py
│
├── api/
│   ├── roast.py
│   ├── upload.py
│   ├── share.py
│   └── health.py
│
├── services/
│   ├── vision_service.py
│   ├── roast_service.py
│   ├── moderation_service.py
│   ├── card_service.py
│   └── cache_service.py
│
├── models/
│   ├── requests.py
│   └── responses.py
│
├── prompts/
│   ├── vision.py
│   ├── roast.py
│   └── safety.py
│
├── config/
│   └── settings.py
│
└── utils/
14. Model Server

Use:

vLLM

The Qwen documentation explicitly recommends vLLM for deployment, and vLLM exposes an OpenAI-compatible API.

That is very useful because our FastAPI backend can communicate with the model server using a standard API.

Example:

FastAPI
   ↓
http://vision-server:8000/v1
   ↓
Qwen3-VL

And:

FastAPI
   ↓
http://roast-server:8001/v1
   ↓
Roast LLM
15. Model Servers

We can keep both models independently deployable.

GPU SERVER
│
├── Port 8000
│     └── Qwen3-VL
│
└── Port 8001
      └── Roast LLM

This means either model can be replaced later.

16. Model Selection
Vision

Initial candidate:

Qwen3-VL

The current family includes multiple sizes, so we should choose based on the actual GPU available rather than blindly deploying the largest checkpoint. The official Qwen repository documents dense and MoE variants and vLLM deployment.

Development

Start with a smaller model.

Production

Benchmark:

Qwen3-VL 8B
Qwen3-VL 30B-A3B
Other suitable VLMs

and choose based on:

Quality
VRAM
Latency
Throughput
17. Roast Model

Initial candidate:

Qwen3

The Qwen3 family can also be served through vLLM and exposes OpenAI-compatible endpoints. Its official deployment documentation also supports structured/JSON output and non-thinking mode, which is useful for predictable production responses.

Other models can be benchmarked later.

The final decision should be based on:

Humour
Instruction following
Speed
VRAM
Output consistency
Licence

Not benchmark scores alone.

18. Model Abstraction

Never hard-code the model directly into application logic.

Use:

VISION_MODEL=Qwen/Qwen3-VL-8B-Instruct
ROAST_MODEL=Qwen/Qwen3-8B

Backend:

VisionProvider
       ↓
QwenVisionProvider

and:

RoastProvider
       ↓
QwenRoastProvider

Later:

VISION_MODEL=another-model
ROAST_MODEL=another-model

without rewriting the application.

19. API Endpoints
POST /api/v1/roast

Main endpoint.

Input:

{
  "image_url": "...",
  "input_type": "room",
  "intensity": "savage",
  "style": "gen-z"
}

Response:

{
  "id": "abc123",
  "score": 87,
  "title": "Certified Chaos",
  "roast": "...",
  "verdict": "...",
  "stats": {},
  "share_url": "/r/abc123"
}
POST /api/v1/upload

Uploads image.

GET /api/v1/roast/{id}

Retrieves a roast.

POST /api/v1/share/{id}

Tracks share event.

GET /api/v1/health

Returns:

{
  "status": "ok",
  "vision": "ready",
  "roast": "ready"
}
20. Frontend

Use:

Next.js + TypeScript

Hosting:

Vercel

Main pages:

/
 /roast
 /r/[id]
 /profile
 /battle
 /privacy
 /terms

Only the first three are required for V1.

21. Homepage

Hero:

LET AI JUDGE YOU.

Subheading:

Upload a photo, screenshot, playlist or room.
We'll tell you what your friends are too scared to say.

CTA:

🔥 ROAST ME

Then show example roast cards.

22. Roast Flow
Homepage
   ↓
Upload
   ↓
Select content type
   ↓
Select intensity
   ↓
Select style
   ↓
Roast button
   ↓
Loading animation
   ↓
AI analysis
   ↓
Roast result
   ↓
Share card
23. Loading Experience

Don't simply show:

Loading...

Use stages:

👁️ Looking at the evidence...

🔍 Finding your questionable decisions...

🧠 Preparing the roast...

🔥 Loading emotional damage...

💀 Almost done...

These are UI messages only.

24. Share Page

URL:

/r/{roast_id}

The page contains:

ROASTME

ROAST SCORE
87/100

Certified Chaos

[Roast]

Chaos       █████████ 91
Taste       ███████   72
Energy      ████████  88

🔥 ROAST YOURSELF

The CTA must lead back to:

/roast
25. Viral Loop

Every public roast must contain:

RoastMe branding
+
Share button
+
"Get Your Roast" CTA

When someone opens a shared result:

Shared Roast
     ↓
Reads roast
     ↓
Laughs
     ↓
"Roast Me"
     ↓
Upload
26. Database

Use:

PostgreSQL

Supabase is suitable for the initial implementation.

Tables:

users
id
username
email
created_at
roasts
id
user_id
input_type
input_hash
intensity
style
score
title
roast
verdict
stats
share_caption
created_at
shares
id
roast_id
platform
created_at
battles
id
creator_id
opponent_id
roast_a
roast_b
winner
created_at
27. Image Storage

Initial:

Supabase Storage

Requirements:

Compress uploads
Resize unnecessarily huge images
Validate MIME type
Limit file size
Generate unique filenames
Don't expose private uploads publicly unless required
28. Caching

This is very important for keeping inference usage low.

Calculate an image hash:

image
 ↓
SHA-256
 ↓
input_hash

Before calling the vision model:

Does input_hash already exist?
       │
   ┌───┴───┐
   YES     NO
    │       │
    ▼       ▼
 return    AI
 cached   analysis

This prevents unnecessary duplicate inference.

29. Rate Limiting

Anonymous users:

3 roasts/day

Logged-in users:

5–10/day

These values can be adjusted later.

Rate limit by:

IP
Session
User ID

Don't rely only on frontend restrictions.

30. Privacy

Uploaded content can contain highly personal information.

Therefore:

Don't permanently retain uploads unnecessarily.
Delete temporary files after processing.
Don't expose original images through public URLs unless required.
Don't store chat screenshots indefinitely.
Provide a deletion mechanism.
Clearly explain how uploaded content is processed.
Don't use user uploads for model training without appropriate consent/legal basis.
31. Security

Backend must:

Validate uploaded files
Limit image dimensions
Limit file size
Validate MIME types
Sanitize filenames
Protect internal model endpoints
Keep secrets server-side
Add authentication where necessary
Rate-limit AI endpoints
Prevent arbitrary URL fetching
Prevent prompt injection from images/text where possible

Never expose:

MODEL_SERVER_URL
GPU credentials
database service keys
private API keys

to the browser.

32. Environment Variables

Example:

NEXT_PUBLIC_API_URL=

SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

VISION_API_URL=
VISION_MODEL=

ROAST_API_URL=
ROAST_MODEL=

INTERNAL_API_SECRET=

MAX_IMAGE_SIZE_MB=10

Secrets must never be committed to Git.

33. Repository Structure

Recommended monorepo:

roastme/
│
├── apps/
│   │
│   ├── web/
│   │   ├── app/
│   │   ├── components/
│   │   ├── lib/
│   │   ├── public/
│   │   └── package.json
│   │
│   └── api/
│       ├── app/
│       │   ├── api/
│       │   ├── services/
│       │   ├── models/
│       │   ├── prompts/
│       │   ├── utils/
│       │   └── main.py
│       └── requirements.txt
│
├── infrastructure/
│   ├── docker/
│   ├── vllm/
│   │   ├── vision/
│   │   └── roast/
│   └── deployment/
│
├── docs/
│   ├── architecture.md
│   ├── api.md
│   ├── ai.md
│   └── security.md
│
├── .env.example
├── docker-compose.yml
├── README.md
└── LICENSE
34. Development Mode

During development, do not constantly run the actual models.

Use:

MOCK_AI=true

Then:

Upload
 ↓
Mock vision result
 ↓
Mock roast
 ↓
UI

This lets you develop the entire website without consuming GPU resources.

Later:

MOCK_AI=false

and the real models activate.

35. AI Prompt System
Vision system prompt

Conceptually:

You are the visual analysis engine for RoastMe.

Analyse only information that can actually be observed
in the supplied image.

Do not infer sensitive personal characteristics.

Do not identify protected characteristics.

Do not invent information.

Extract:
- content type
- visible objects
- visual patterns
- aesthetic
- notable details
- text that is clearly visible
- useful comedic observations

Return valid JSON only.
36. Roast system prompt

Conceptually:

You are RoastMe, a witty AI roast comedian.

Your job is to create funny, specific and personalised
roasts based ONLY on the supplied observations.

The roast should feel like a clever friend teasing the user.

Do:
- be specific
- use observations
- use unexpected comparisons
- use internet/Gen-Z humour when requested
- make the roast memorable
- keep it concise

Do not:
- attack protected characteristics
- invent facts
- make medical claims
- expose private information
- threaten the user
- encourage self-harm
- use hateful content
- make sexual comments involving minors

Return valid JSON.
37. Structured Output

Use a fixed schema:

{
  "score": 0,
  "title": "",
  "roast": "",
  "verdict": "",
  "stats": {
    "chaos": 0,
    "taste": 0,
    "energy": 0
  },
  "share_caption": ""
}

The backend validates this using Pydantic.

If invalid:

AI response
 ↓
Pydantic validation
 ↓
invalid?
 ↓
retry / repair
38. Error Handling

Possible failures:

Image too large
Invalid file
Vision model unavailable
Vision timeout
Invalid AI JSON
Roast model unavailable
GPU overloaded
Database unavailable

User should receive simple messages.

Example:

Something went wrong while cooking your roast. Try again.

Do not expose:

CUDA out of memory
vLLM exception
database credentials
stack trace
39. Observability

Track:

request count
vision latency
roast latency
total latency
GPU utilisation
GPU memory
failed requests
JSON validation failures
cache hits

Useful metrics:

Average roast generation time
Vision success rate
Roast success rate
Cache hit rate
Share rate
40. Product Analytics

Track:

page_view
upload_started
upload_completed
roast_started
roast_completed
roast_shared
roast_downloaded
share_page_opened
new_roast_started_from_share

Most important:

Share conversion
Shared roast
      ↓
How many people start their own roast?

That tells us whether the viral loop actually works.

41. V1 Features

Build only:

Core
Homepage
Upload
Image preview
Input type
Intensity
Style
AI processing
Roast result
Score
Stats
Share page
Download card
Backend
FastAPI
Qwen3-VL
Roast LLM
PostgreSQL
Storage
Rate limiting
Cache
Deployment
Vercel frontend
Virtual GPU inference server
HTTPS
Environment variables
42. V2

After V1 works:

Profiles
roastme.app/@username
Roast history
Badges
🔥 First Roast
💀 Got Cooked
☢️ Nuclear Survivor
🏆 Roast Champion
Roast Battle
You
VS
Friend
Friend Roast

Send:

Think you can survive this?

43. V3
Leaderboard
Most roasted
Highest score
Roast champion
Most chaotic
Daily winners
Daily challenges

Examples:

Show your room.

Show your lock screen.

Show your playlist.

Show your desktop.

44. Future AI Training

Do not fine-tune initially.

First collect quality examples.

Dataset:

input_observation
       ↓
generated_roast
       ↓
human_rating
       ↓
funny / average / bad

Eventually:

Open LLM
   +
RoastMe dataset
   ↓
Fine-tuning
   ↓
RoastMe-specific model

This can become a real technical differentiator.

45. Performance Targets

Initial targets:

Frontend load:
< 2–3 seconds

Image upload:
< 3 seconds

Vision:
< 5–10 seconds

Roast generation:
< 5 seconds

Total:
< 15 seconds

These are targets, not guarantees.

Later optimise toward:

Total roast:
< 8 seconds
46. Cost Strategy

The project is designed around zero paid AI APIs during development.

We use:

Open-weight models
        ↓
Virtual GPU
        ↓
Own inference API

However:

Open-weight model weights may be free under their licence, but GPU compute is not automatically free.

Therefore the project should support:

Free GPU environments for experimentation
Temporary GPU instances
Model quantisation
Smaller models
Caching
Rate limits
Mock mode

The application should never assume unlimited free compute.

47. Model Licence Policy

Every model added to the project must have its licence reviewed before production use.

Do not use:

Leaked proprietary models
Leaked proprietary source code
Pirated model weights
Scraped commercial APIs
Models whose licence prohibits your intended usage

Use legitimate open-weight releases and official repositories.

The Qwen3-VL project itself is marked Apache-2.0 in its official GitHub repository.

48. Deployment Architecture

Production:

                         USERS
                           │
                           ▼
                    ┌─────────────┐
                    │   VERCEL    │
                    │   Next.js   │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  FASTAPI    │
                    │  Backend    │
                    └──────┬──────┘
                           │
                  ┌────────┴─────────┐
                  │                  │
                  ▼                  ▼
             Supabase           GPU Server
             PostgreSQL             │
             Storage                │
                              ┌─────┴──────┐
                              │            │
                              ▼            ▼
                         Qwen3-VL     Roast LLM
49. GPU Server

The GPU server should expose internal/private endpoints where possible.

Example:

GPU_SERVER
│
├── /vision
│
├── /roast
│
└── /health

FastAPI communicates with the model servers.

Qwen's official documentation demonstrates vLLM serving models through an OpenAI-compatible endpoint, which makes this architecture practical.

50. Docker

The project should support Docker:

docker-compose.yml

Services:

web
api
vision
roast
database

For production, database/storage can remain external through Supabase.

51. Local Development

Even though the final models run on a virtual GPU, development should support:

MOCK_AI=true

Therefore:

Developer laptop
      ↓
Next.js
      ↓
FastAPI
      ↓
Mock AI

No GPU required.

When testing the actual AI:

Developer laptop
      ↓
FastAPI
      ↓
Remote GPU
      ↓
Qwen3-VL + Roast LLM
52. Git Workflow

Branches:

main
develop
feature/frontend
feature/backend
feature/vision
feature/roast-engine
feature/share

Commit style:

feat: add image upload
feat: add qwen vision service
feat: add roast generation
fix: handle invalid vision output
feat: add roast share page

Never commit:

.env
API keys
service credentials
private model files
user uploads
53. Development Phases
Phase 1

Project setup.

Next.js
FastAPI
Supabase
GitHub
Docker
Phase 2

Frontend.

Homepage
Upload
Settings
Loading
Result
Phase 3

Mock AI.

Upload
 ↓
Mock JSON
 ↓
Roast card
Phase 4

Vision AI.

Image
 ↓
Qwen3-VL
 ↓
JSON
Phase 5

Roast AI.

JSON
 ↓
Roast LLM
 ↓
Structured result
Phase 6

Share system.

/r/[id]
Phase 7

Production deployment.

Vercel
+
GPU server
+
Supabase
Phase 8

Analytics.

Measure:

Roast completion
Share rate
Share → new roast
54. Definition of Done for V1

RoastMe V1 is complete when a new user can:

1. Open website
2. Upload an image
3. Select roast intensity
4. Select style
5. Click Roast Me
6. Wait for AI
7. Receive roast
8. See score
9. See stats
10. Generate shareable card
11. Open share URL
12. Share with a friend
13. Friend can immediately create their own roast

No login should be required for the core experience.

55. Long-Term Vision

RoastMe eventually becomes:

A social AI entertainment platform built around personalised AI judgement.

Not just:

"Upload photo and get roast."

The larger ecosystem becomes:

Roast
  │
  ├── Roast Battle
  │
  ├── Friend Roast
  │
  ├── Daily Challenge
  │
  ├── Leaderboard
  │
  ├── Profile
  │
  ├── Streak
  │
  └── Community

The AI is the engine.

The sharing/social loop is the product.

56. Official technical references

For implementation, use the official projects rather than random copied repositories:

Qwen3-VL official GitHub — official multimodal model implementation and deployment information.
Qwen3 official GitHub — official Qwen3 model/deployment repository.
vLLM Qwen3-VL deployment guide — practical serving configurations for Qwen3-VL.
The final stack we're locking
FRONTEND
Next.js
TypeScript
Tailwind
shadcn/ui
Framer Motion
        │
        ▼
HOSTING
Vercel
        │
        ▼
BACKEND
FastAPI
Python
Pydantic
        │
        ▼
MODEL SERVING
vLLM
        │
        ├───────────────┐
        ▼               ▼
   Qwen3-VL         Roast LLM
   Vision AI        Text AI
        │               │
        └───────┬───────┘
                ▼
           Roast Engine
                │
                ▼
          Safety Layer
                │
                ▼
          Roast Result
                │
                ▼
           Share Card
                │
                ▼
             VIRAL LOOP

DATABASE
Supabase PostgreSQL

STORAGE
Supabase Storage

CODE
GitHub

DEPLOYMENT
Virtual GPU
