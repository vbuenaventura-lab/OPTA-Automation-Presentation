# OPTA Test Automation Journey — Presentation Script

A slide-by-slide speaking script / ad-lib guide for presenting the OPTA Test Automation deck.
Use it as a guide, not a word-for-word script — keep it conversational and natural.

**Tip:** Use the bottom nav bar (or arrow keys) to move between slides. The slide picker dropdown lets you jump to any slide.

---

## Opening / Intro (before Slide 1)

> "Good afternoon Darren, everyone. So today we'll be sharing with you how we've been automating our test cases for OPTA.
>
> We'll walk through a little bit of background — how we moved from manually creating each test file, to now being AI-assisted in generating those test files.
>
> We'll also talk about some of the integrations we did along the way, specifically with **Zephyr**, **Jira**, and **Allure reporting**. And towards the end, we'll be honest about some of the challenges we hit and where we're heading next.
>
> I've also got a few short recordings to show you the tooling in action. Feel free to jump in with questions at any point."

---

## Slide 1 — Test Automation Journey (Hero)

**What's on screen:** Title slide with three phases — Manual Era → AI-Assisted → Integrated.

> "So this is the journey at a glance. It really breaks down into three chapters.
>
> First, the **Manual Era** — where every test was hand-built: inspecting elements, crafting XPaths by hand, and coding everything from scratch.
>
> Then the **AI-Assisted** phase — where we brought in a GitHub Copilot spec generator that reads directly from Zephyr.
>
> And finally the **Integrated** phase — where everything ties together with Zephyr, Jira, and Allure.
>
> Let me take you through each of these."

**Transition:** "But before we get into the journey, let me quickly set the foundation — the tool that makes all of this possible."

---

## Slide 2 — Automation using Playwright

**What's on screen:** Two bullets at top, then "How It Works" flow on the left and "Why Playwright" on the right.

> "I'm sure most of you are already familiar with Playwright, so I won't dwell on it too long.
>
> The key things for us: Playwright supports **Web GUI based automation** — it drives a real browser and interacts with our web-based SAP screens exactly like a human tester would. And we write our scripts in **TypeScript**, which gives us cleaner code, autocomplete, and error checking right inside VS Code.
>
> On the left here you can see *how it works* — our TypeScript test code drives the Playwright runner, which controls a Chromium browser, which loads the SAP WebGUI. So it's a real browser hitting the real system.
>
> And on the right is *why* we picked Playwright specifically for SAP — it handles XPath selectors well, it has built-in auto-wait for SAP's slow load times, and it gives us screenshots and traces when something fails."

**Transition:** "So that's our engine. Now let me take you back to where we started — the manual way of doing things."

---

## Slide 3 — The Manual Approach

**What's on screen:** Three red cards — Inspect Element, Hunt for XPath, Write Spec Manually.

> "Before any AI tooling, this was the reality. Every single test file was built by hand in three painful steps.
>
> First, you'd open Inspect Element on the SAP screen and dig through the HTML. Then you'd hunt for a reliable XPath — and if you've ever looked at the SAP DOM, you know it's deeply nested with generated IDs that change all the time. Finally, you'd hand-write the whole spec file and debug it until it passed.
>
> The bottleneck here is the real story — a single scenario with 8 to 10 steps could take **half a day or more**, and most of that time was spent fighting broken locators, not actually writing test logic."

**Transition:** "So that was the pain point. And this is where things changed for us."

---

## Slide 4 — Introducing the AI-Powered Spec Generator

**What's on screen:** The turning point — flow diagram from Jira → Zephyr → AI → Pattern Matching → .spec.ts.

> "This was the turning point. Instead of writing test files from scratch, we built a **GitHub Copilot agent skill** that reads the test steps directly out of Zephyr Squad and generates a ready-to-run Playwright spec file.
>
> So the flow is: we start from a Jira issue, it pulls the linked Zephyr test steps, the AI skill processes them, matches them against known SAP patterns, and out comes a `.spec.ts` file.
>
> What used to take hours of manual setup now takes **minutes to scaffold** — which means the team can focus on refining the actual test logic instead of locator hunting."

**Transition:** "Let me show you what that looks like end-to-end with a real scenario."

---

## Slide 5 — How One Scenario Gets Fully Automated

**What's on screen:** OPTA-91 walkthrough — 4 steps, plus "What Still Needs Human Input."

> "This is a real example — OPTA-91, start to finish.
>
> Step one, we pick the Jira ticket, which is already linked to a Zephyr test with written steps. Step two, we run the spec generator — one command — and it scaffolds the spec file for us. Step three, we review it, fix any TODOs, and where a locator is missing we use Playwright's **Record at cursor** — you interact with SAP live and it captures the locator automatically. And step four, once it passes, we generate the automation documentation, which publishes to Google Docs and links back to Jira.
>
> But I want to be clear — this isn't fully hands-off. On the right you can see what **still needs a human**: reviewing the logic, filling TODOs, handling complex interactions like drag-and-drop, and the edge cases that Zephyr steps don't capture."

**Transition:** "Now let me go a level deeper into *how* that generator actually turns plain English into code."

---

## Slide 6 — How the GitHub Copilot Skill Generates the Spec

**What's on screen:** 5 steps + a simplified code sample of the generator flow.

> "So under the hood, here's what's happening.
>
> We trigger the generator — and a quick note here, our preferred way is the terminal command because it doesn't burn GitHub Copilot tokens. You *can* invoke it through Copilot Chat as well, but that uses more token budget, so we save that for one-off requests.
>
> From there, the script resolves the linked Zephyr test, pulls every step, and the AI does pattern matching — it recognises things like navigation, dropdowns, tree drilling — and maps them to our existing page object methods. The spec file gets written to disk, and importantly, every step carries a comment pointing back to its Zephyr step number for traceability.
>
> And when the AI *can't* confidently map a step, it doesn't guess — it leaves a TODO placeholder, and we fill it in using Record at cursor."

**Transition:** "Rather than just talk about it, let me show you the generator actually running."

---

## Slide 7 — DEMO: Generating a Test Spec from Zephyr Steps

**What's on screen:** Embedded video player.

> "So here's a quick recording of the spec generation. Watch as we run the command, it reaches out to Zephyr, pulls the steps, and you'll see the spec file scaffold itself in VS Code."

*(Play the video. Pause/narrate as needed.)*

> "And that's it — what would've taken hours of manual setup, done in under a minute."

**Transition:** "Okay, so we've generated a test. Now — how do we actually run it and see the results?"

---

## Slide 8 — Running Tests & Seeing Results

**What's on screen:** How testers run tests, Allure reports, Jira reporting, and CI/CD under research.

> "This is the execution and reporting side.
>
> Right now, testers run tests through the **VS Code Test Explorer** — they open the project, pick the scenario, hit run, and the browser launches and executes live. They can see pass/fail right inside VS Code, or via an automated comment that gets posted to the Jira ticket. There's also a command to generate the local Allure report — though heads up, that one's still pending automation in a future update.
>
> On the reporting side — the **Jira reporting is now live and working**. After every run, our custom reporter posts a structured comment to the ticket: the test name, the status, and the product version tested. So Jira stays up to date without anyone lifting a finger.
>
> The one piece still in research is the **CI/CD pipeline** — automated, trigger-based runs. The web dev team is actively looking into that."

**Transition:** "Let me show you a full test run in action."

---

## Slide 9 — DEMO: Running an Automated Test Case

**What's on screen:** Embedded video player.

> "Here's a complete test run — from launching the Test Explorer, through the live browser execution against SAP, all the way to the final PASS result and the Jira update."

*(Play the video.)*

> "So you can see it's all hands-off once it's running — it drives SAP, validates, and reports back automatically."

**Transition:** "And there's one more piece of automation I want to show you — the documentation."

---

## Slide 10 — DEMO: Generating the CR / Automation Specification Document

**What's on screen:** Embedded video player + link to a sample generated document.

> "Once a test is automated and passing, we also auto-generate a **CR / Automation Specification Document**. This captures the scenario, the methods used, the page objects, the Zephyr step mapping, and the environment details.
>
> It publishes straight to Google Docs and links back to the Jira ticket — so there's full traceability. This recording shows it generating, and there's a link here to a real sample document if you want to see the output."

*(Play the video, optionally open the sample doc link.)*

**Transition:** "Now — I've shown you the happy path. But I want to be transparent about the challenges, because SAP automation is genuinely hard."

---

## Slide 11 — Key Challenges & Constraints

**What's on screen:** Six challenge cards — red (critical) and gold (manageable).

> "So let's be honest about the constraints.
>
> The biggest one is **dynamic, unstable element IDs**. SAP generates IDs at runtime — they change between page loads, sessions, even releases. For example, the same field is labelled differently between MT8 and MT4, and if you expand the example, you'll see the generated table IDs are completely different numbers across environments. So we can't rely on IDs — we use text-based and structural XPath instead.
>
> On top of that: the DOM is deeply nested, locators can differ across environments — which is also why our sanity tests currently only support EHP8 systems, because anything below that renders SAP inside an iframe and behaves completely differently.
>
> The gold cards are the more manageable ones — SAP's timing sensitivity, the fact that Zephyr steps are written for humans and can be vague, and that we don't have an automated pipeline yet."

**Transition:** "So given all of that, the natural question is — what do we automate first? How do we prioritise?"

---

## Slide 12 — Prioritisation Strategy

**What's on screen:** Automate First / Later / Don't Automate, plus a 3-month target.

> "We think about this in three buckets.
>
> **Automate first** — the high-value stuff: regression-critical scenarios, high-frequency tests we run every sprint, the ones with well-documented Zephyr steps, and cross-environment checks.
>
> **Automate later** — things with frequently changing UI or complex multi-system setup, where the maintenance cost is higher.
>
> And **don't automate** — the low-ROI work: one-off sanity checks, anything needing human visual judgement, or tests for features being deprecated soon.
>
> Realistically, our 3-month target is around **15 to 20 scenarios fully automated**, covering 5 to 6 OPTA products, with 100% Zephyr traceability — and the CI/CD pipeline still in research.
>
> So to quickly wrap up — that's our journey: we've gone from manually hunting XPaths and hand-coding every test, to an AI-assisted flow that scaffolds specs straight from Zephyr, with Jira and Allure tying the reporting together. The result is roughly an **80% reduction** in setup time, **11-plus scenarios** automated so far, and full traceability back to Zephyr. The framework is built to scale — every new page object method we add unlocks many future scenarios at no extra locator-hunting cost.
>
> That's it from me — thank you all for your time. Happy to take any questions."

---

## Closing / Q&A

> "So that's our automation journey — from manual XPath hunting to AI-assisted generation, with Zephyr, Jira, and Allure tying it all together. What questions can I answer for you?"

### Likely questions to prepare for
- **"How reliable are the AI-generated specs?"** → They scaffold the structure well, but always need human review for business logic and locators — it's assistive, not fully autonomous.
- **"When will CI/CD be ready?"** → Still under active research by the web dev team; Jira reporting is already live.
- **"Can this handle non-web / backend SAP?"** → Current focus is Web GUI; backend automation needs a different, more complex approach.
- **"Why only EHP8 systems?"** → Older systems render SAP inside an iframe, which changes how Playwright locates elements — separate approach needed.
