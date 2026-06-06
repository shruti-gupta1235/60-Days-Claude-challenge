role based prompting


Role-based prompting is a context engineering technique where you assign Claude a specific persona or expert role before giving your actual request. It's one of the most powerful ways to shape the quality, tone, and depth of AI output.

Here's how it works in practice:



Role-based prompting works because AI models are trained on vast amounts of human-written content — doctors write like doctors, investors think like investors. When you assign a role, you're essentially activating that specific subset of knowledge, vocabulary, reasoning style, and professional standards.

Founder persona
Vision · Market · Growth
You are a serial entrepreneur who has built and sold two startups. You think in terms of markets, user psychology, and growth loops. How should I think about building a product people actually use? Be opinionated. Share what most first-time founders get wrong.
Developer persona
Systems · UX · Craft
You are a senior software engineer who has shipped products used by millions. You think in terms of architecture, user flows, and feedback loops. How should I think about building a product people actually use? Be technical and specific. Focus on what separates products that stick from those that don't.
Responses

Founder response
Most founders build for themselves. That's the trap.

Start with the problem, not the product. Talk to 20 people before writing a line of code. If you can't find 20 people with the problem, you don't have a market.

The question isn't "will people use this?" — it's "why would they stop using what they already have?" Switching costs are real. Your product has to be 10x better at something, not 10% better at everything.

Build for the critical path — the one flow that delivers the core value. Kill everything else. Version 1 should embarrass you.

Distribution beats product. A mediocre product with great distribution wins over a great product nobody hears about.

Market-first thinking Customer discovery Distribution Switching costs
Developer response
Products fail when the happy path has too much friction. Users don't read — they click, get confused, and leave.

Design the time-to-value first. How fast can a new user reach the moment they get the point? That number predicts retention more than any feature.

Build feedback loops into the product itself — not just analytics dashboards. Error states, empty states, and onboarding flows are the product. They're where 60% of users form their opinion.

Instrument everything early. Track the specific actions that correlate with users who come back. Then remove every obstacle between signup and that action.

The difference between a product people use and one they abandon: the first time it works exactly as expected, without explanation.

Time-to-value UX friction Instrumentation Retention loops
Side-by-side comparison

Dimension	Founder	Developer
Starting point	The market and the customer's existing behavior	The product interface and user's first session
Key question	"Why would they switch from what they use now?"	"How fast can they reach the core value?"
Biggest risk	Building something nobody wants	Building something users can't figure out
Language used	Markets, distribution, switching costs, growth loops	Friction, time-to-value, instrumentation, retention
Advice style	Opinionated, strategic, contrarian	Specific, measurable, systems-oriented
What they skip	How the product actually works inside	Whether the market is worth building for
Best used when	Deciding what to build and whether to build it	Deciding how to build it and measure success
The insight
Same question. Same AI. Completely different answers — because the role defines the lens. The Founder thinks outside-in: market → user → product. The Developer thinks inside-out: interface → flow → retention. Neither is wrong. The best builders do both. Role-based prompting lets you summon whichever perspective you need, on demand.
The core lesson here: the AI didn't change — the lens did.
The Founder thinks outside-in — market, competition, distribution, switching costs. They don't care how the product is built, only whether it should exist and whether people will choose it.
The Developer thinks inside-out — friction, time-to-value, instrumentation, retention loops. They assume the market exists and focus on whether users can actually get value from what's built.
