---
title: "How I build with AI as a 1-person product team"
source: "https://georgexing.substack.com/p/how-i-build-with-ai-as-a-1-person?just_subscribed=true"
author:
  - "[[George Xing]]"
published: 2026-05-07
created: 2026-06-18
description: "Tools and techniques I use to build higher-quality products, 10x faster"
tags:
  - "clippings"
---
Over the last couple months, I’ve been experimenting with many different AI coding tools and techniques with the goal of streamlining my own product-building workflow as a 1-person team for my personal projects. My goal was to create a system that made building software efficient, effortless, and fun. While there is always room for improvement with new models, features, and tools, I’ve also significantly improved my workflow from when I started.

There are 3 classes of things that made a real difference: using an agent skills framework, setting up test automation, and designing a remote-first, ergonomic workflow. The productivity gains from implementing these changes compound on each other, and together I estimate they’ve made me at least 10x more productive while producing higher-quality output than I would have otherwise.

## 1\. Building with /superpowers

When I first started building personal apps with Claude Code, I relied heavily on Plan Mode: I would Shift+tab into Plan Mode on a fresh session running the latest Opus model, dump in as much product and technical context as I felt was useful, iterate until the plan looked right, then green-light the execution and watch it run.

This worked for simple features, but it was hit-or-miss for anything more complex. I have a cooking app called LemmeCook where the onboarding flow, the suggested meals logic, recipe imports, and inventory management all interacted in subtle ways. Claude Code would consistently make unintuitive product and UX choices when those interactions mattered.

I couldn’t tell whether the problem came from the plan being underspecified, the execution drifting from the plan, or both. I found myself spending hours fixing bugs and unintended behavior. It was tiring and made building less fun.

What changed the game for me was using [/superpowers](https://github.com/obra/superpowers), a Claude plugin created by Jesse Vincent that you can easily install from Anthropic’s official plugin marketplace. The [^1] bundles four key skills that map to the steps of traditional product development: brainstorming, writing-plans, executing-plans, and code-review [^2]. The four skills can be invoked directly as individual steps, or together as a chained sequence.

### brainstorming

The brainstorming skill helps you generate a structured PRD from your product ideas. When I have an idea – usually a half-formed thought about how a feature should work – I do a brain-dump into Claude and let the brainstorming skill take over. It summarizes what it heard back to me, asks clarifying questions, and surfaces product gaps and tensions I hadn’t considered. For front-end work, it generates wireframes that let me decide on visual and UX direction in my browser. It feels like having a PM sounding board and designer at the same time.

As a builder, I really enjoy the brainstorming phase. I find it much easier to answer questions from a set of options (especially with the visual companion) than to generate the right questions and potential answers from scratch. I also have the ability to manage the process and inject my personal taste: it’s easy to steer Claude to explore more options and be creative, or ask it to converge faster toward a good-enough solution.

![/superpowers brainstorming](https://substackcdn.com/image/fetch/$s_!CHW_!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0d58a81-1d89-455a-b714-65d08eee8963_1432x1182.png)

/superpowers guides you through an interactive brainstorm process for key product and design decisions

### writing-plans

Once the brainstorming skill produces a product spec I’m happy with, the writing-plans skill turns the spec into a detailed implementation plan. The plan is a markdown file with discrete tasks, and each task contains detailed instructions. Here’s a snippet from the plan it wrote for the spec we just brainstormed:

```markup
# Cook Tab — "Tonight" Trio Implementation Plan  

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (\`- [ ]\`) syntax for tracking.

**Goal:** Replace the Cook tab's filtered 6+ card grid with a daily-ritual surface showing exactly 3 AI-curated picks — each with a personalization reason chip — plus a single reroll button, refreshing once per local calendar day.

**Architecture:** Mobile rewrites \`cook.tsx\` to render three \`TonightCard\` rows (new) + one reroll button. \`usePoolSuggestions\` is simplified to a 3-pick query with a \`reroll\` action; filter state is removed. The API engine \`generateSuggestionsForUser\` accepts an \`excludeTitles\` array (used to avoid repeats on reroll) and produces a \`reason: { type, text }\` field per suggestion via a new post-composition reasoning step. The \`suggestion_pool\` table gains a small \`reason\` JSONB column.

**Tech Stack:** React Native / Expo Router / TanStack Query / NativeWind on mobile · Express / Zod / Anthropic SDK / Supabase on API · TypeScript end-to-end · Maestro for E2E.

**Spec reference:** \`docs/superpowers/specs/2026-05-02-cook-tab-tonight-trio-design.md\`
---
## File Structure

| File | Action | Responsibility |
|------|--------|----------------|
| \`packages/shared/src/types/suggestion.ts\` | Modify | Add \`ReasonType\` union and optional \`reason\` field on \`PreGeneratedSuggestion\` |
| \`supabase/migrations/00031_suggestion_pool_reason.sql\` | Create | Add nullable \`reason\` JSONB column to \`suggestion_pool\` |
| \`apps/api/src/services/preGenerationEngine.ts\` | Modify | Accept \`excludeTitles\` option; replace feasibility-only selection with \`(cuisine, protein-class)\` diversity-aware selection; add post-composition reason selection step; persist \`reason\` to row |
...
Total: 7 new files, 6 modifies, 2 deletes, 4 Maestro flows. ~700-900 LOC delta.
---
### Task 1: Add \`ReasonType\` and \`reason\` field to shared types

**Files:**
- Modify: \`packages/shared/src/types/suggestion.ts\`
- [ ] **Step 1: Add the \`ReasonType\` union and \`Reason\` interface**

In \`packages/shared/src/types/suggestion.ts\`, add **before** \`PreGeneratedSuggestion\`:
export type ReasonType = 'history' | 'inventory' | 'variety' | 'library'
export interface Reason {
  type: ReasonType
  text: string
}
...
```

Depending on what I’m working on, I usually take a few minutes to sanity-check the high-level plan and tasks. I’m usually not spending time digging into the implementation details within each task myself. However, I do often probe Claude to understand how the plan accounts for certain edge cases and verify it will implement the desired product behavior.

For example, here I asked Claude if the plan ensures all suggested meals are sufficiently different (it doesn’t), and recommended a fix:

![](https://substackcdn.com/image/fetch/$s_!GIop!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe01d74af-d629-4c4f-bd9e-b40f796f4241_1642x1160.png)

Claude recommends a fix for an undesired product behavior I found in the plan

### executing-plans

Once the implementation plan looks good, the executing-plans skill reads it and writes the code. I’m usually given a choice between executing serially or spinning up a fresh subagent per task, which uses another bundled skill called subagent-driven development. I generally pick the faster parallel option. Claude automatically identifies which tasks can be parallelized, which have dependencies on each other, and which tasks can be handled by less powerful models (e.g. Sonnet or Haiku instead of Opus).

In my experience, execution is typically the longest step and can run autonomously for anywhere from 10 minutes to hours, depending on the task. It’s a good time to step away from your computer. I usually kick off the execution phase and use it as a breakpoint to go on a walk or grab a coffee.

![](https://substackcdn.com/image/fetch/$s_!kbHr!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe8687a8-ea7b-412d-b8a0-e6ccf8fee382_1638x1048.png)

/superpowers offering different execution options

### code-review

The fourth skill, code-review, runs after the build and checks the work. It identifies and fixes bugs, flags places where the implementation drifted from the plan, and catches things you’d want a more experienced engineer to catch.

This step can be chained with executing-plans, so that Claude can autonomously run code-review and edit in a loop until all issues flagged by the reviewer are addressed, without my involvement.

Building with superpowers helped me appreciate that AI hasn’t yet removed the need to identify problems, users, use cases, and jobs-to-be-done. Product thinking is essential to product development with agents, but now we can do the work in an AI-assisted, highly compressed way – and having an agent skills framework to *enforce* these best practices makes a big difference.

In fact, in a world where code is cheap and easy to generate, the upstream work actually matters *more*, not less. It’s tempting to dive into execution and iterate with the final output, but time and time again, I find that being diligent in brainstorming, reviewing specs, and interrogating implementation plans in the beginning yields shorter time to satisfaction and better product quality at the end.

![](https://substackcdn.com/image/fetch/$s_!iNdU!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c5a5d27-f0c0-4902-8855-1a08aee4608b_1979x807.png)

You save a lots of human and calendar time by investing more time with product definition upfront!

## 2\. Testing automation

Using a structured product development framework like superpowers improved my output quality significantly, but it was far from bulletproof. For example, Claude Code sometimes generated overlapping drawers and popover modals that made certain UI elements unclickable in my cooking app. On the backend, it sometimes missed race conditions that would cause incorrect or no data to load. These classes of bugs were not easily testable in the testing plan from the product spec.

### Codex review

Though I preferred using Claude Code as my primary coding companion, I’d also tried Codex and found it generally more rigorous and methodical as a model/harness. Compared to Claude Code, Codex was less creative but would follow instructions more closely. I was already pointing Codex at larger commits from Claude.

In late March, OpenAI released a Codex plugin for Claude Code, and I decided to incorporate it into my workflow. I would have Codex review the implementation plan written by Claude, using the codex:rescue skill. After the implementation step, Claude Code would kick off Codex as a subagent to review the code. Claude would then address any issues Codex flagged and submit for review again. This would loop until all issues were resolved, sometimes taking as many as 3 or 4 iterations.

While the extra review cycles took time (sometimes 30+ minutes) and Codex can sometimes be overly pedantic in its reviews (e.g. classifying a minor issue as a critical issue), it usually caught real bugs or inconsistencies that Claude Code missed. I found that incorporating Codex review significantly improved overall product quality, especially in backend features and data-intensive flows.

![](https://substackcdn.com/image/fetch/$s_!uiLD!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbbc55a87-85ce-4fbf-a8df-303e62a38d85_1644x1060.png)

Codex flagging issues in Claude’s implementation, which Claude then fixed

### implementation-review skill

No matter how good code review is, there’s no substitute for end-to-end testing of actual user behavior and product flows. At Stripe, implementation review was one of the most important pre-launch steps in product development, where a trained human reviewer would audit your product release and compile a list of issues (including blocking feedback) for teams to address. For new or complex products with many flows, it could take weeks of back and forth, but it was a key part of upholding the product quality bar.

To simulate this process, I created my own implementation-reviewClaude skill. For my mobile app, it works with me to generate a set of scenarios to test. For each scenario, it runs a loop similar to how a human reviewer would test an app: take a screenshot of the current state, analyzes it, logs findings, decides and performs the next action, take another screenshot to understand behavior. At the end, it generates a summary of findings organized by severity.

Under the hood, the skill uses different tools to review different kinds of products. For mobile apps, it uses an MCP server called XcodeBuildMCP that’s maintained by Sentry that lets Claude build and test mobile iOS apps. XcodeBuildMCP wraps many of the low-level AppleScript and simctl Xcode simulator tools in an agent-friendly interface. For web apps, it uses playwright-mcp to navigate a browser.

This setup becomes very powerful when you chain it together:

1. Every implementation plan and line of code is reviewed by Codex.
2. Every key user scenario is reviewed automatically by the implementation-review skill.
3. Every implementation review issue is captured and sent back to Claude to address (and then is code-reviewed by Codex).

Adding implementation review can add hours onto my development cycle, but when set up properly it runs autonomously and is well worth the investment.

![](https://substackcdn.com/image/fetch/$s_!Uuyn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e5884e6-ef77-446a-9ae0-7e404fc8f496_1543x1680.png)

Automated, iterative testing with implementation-review skill and Codex review can save days of manual debugging

## 3\. Remote-first ergonomics

After I started using superpowers and my testing automation setup, my work with Claude shifted from mostly interactive sessions to short bursts of interactive brainstorming and product definition, followed by long-running sessions where Claude ran autonomously. This made my workflow more efficient, because I could do things like kick off a job before bed and check the results in the morning.

But I started running into a few other problems:

1. Typing became a major bottleneck. Feeding more context into LLMs has always led to better results, but in my new workflow the volume of context became a limiting factor — more context meant better brainstorms, which meant better specs, which meant better execution and output. But I didn’t want to type an essay for every new feature I wanted to build.
2. Monitoring and keeping long-running sessions alive. I needed to keep my laptop open to keep the long-running execution sessions going, which was hard to do when I was working from a coworking space and needed to go home. Other times, jobs would require a decision and would be waiting on my input for hours while I was away from my keyboard. I wanted to be able to check on progress and unblock my agents on the go.
3. Managing parallel agents. While I was waiting for Claude, I started working on multiple projects and multiple features within the same project in parallel. It was hard to manage everything in Ghosttytabs and windows [^3]

These are novel developer ergonomics problems that I found decent solutions for:

### Voice input

I started defaulting to speaking rather than typing using WisprFlow, which has had a dramatic positive impact on the quality and speed of my brainstorming iterations [^4] The throughput of information I’m able to communicate is at least 3-4x compared to typing, especially when I’m working on my phone. While my stream-of-consciousness isn’t nearly as well-structured or grammatically correct, it actually works quite well in achieving the goal: frontier models today are quite forgiving and good at extracting the signal from the noise.

In the last couple months, the vast majority of my interactions with Claude Code on both desktop and mobile have been through WisprFlow. Quick tip: spending a few minutes adding commonly used words (e.g. Claude, Tailscale, Codex) to its dictionary is a great investment that will save you from a lot of unnecessary mistakes and wasted cycles later.

![](https://substackcdn.com/image/fetch/$s_!7VSq!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb49a9247-a0e1-4102-ade2-1c8507ea9f7b_1072x446.png)

One of the unexpected benefits of working from home is you can dictate freely without whispering like a maniac:)

### Remote development

To keep persistent sessions going, I moved all my development to my Mac Mini [^5] I considered going the cloud VM route, but for my iOS development I needed to be running MacOS and most cost-effective cloud options ran Linux.

I adapted [this guide](https://kareemf.com/on-agentic-coding-from-anywhere) and configured tmux (for persistent sessions), Tailscale (for reliable network between my phone, Macbook, and my Mac Mini), and [Claude remote control](https://code.claude.com/docs/en/remote-control) (for synchronized Claude sessions). Assuming you have a Mac Mini, the setup is pretty fast. tmux and Tailscale are free, and I also paid $20 for Blink’s iOS app which gives me a terminal and direct ssh access from my phone. This last part is not strictly necessary, but I wanted a backup option in case the remote control connections drop and I needed to restart them.

Now, when working from my laptop at home or at a coworking space, I ssh into my Mac Mini so that my agents continue running when I close my laptop lid and need to go home. When I’m in transit, I continue to monitor my sessions on the train or in a car through the Claude mobile app.

Occasionally I get blocked by system permissions and have to turn to the native Screen Sharing app on my laptop (or less ideally, the RVNC Viewer iOS app on my phone) that lets me control my Mac Mini directly.

![Claude Code sessions via remote control](https://substackcdn.com/image/fetch/$s_!KeGR!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8da3244-9056-4c6a-ab35-c20a142d5ca4_1810x1372.png)

Working on the same Claude Code session via the Claude desktop app, mobile app, and TUI (not shown) via remote control

### Agentic coding environment

There’s an entire category of products that’s emerged in the last 6 months to better serve these new agentic coding workflows.

To better manage my parallel coding sessions, I’ve experimented with a few tools:[^6]

- [Superset.sh](https://superset.sh/) is a agent-native terminal product that ships with a native integration with Github and Git worktrees, enabling you to spin up new branches and features easily. You can configure setup and teardown scripts that handle ops tasks like setting up your.env files. It comes with a diff and markdown viewer and a built-in browser. When Claude needs my input, I get a notification to switch to that tab. The Claude Code TUI is the snappiest, most full-featured of Claude’s product interfaces, so that’s a nice benefit of using the tool.
- The Claude Desktop app has gotten a lot of love in the last couple months and now supports both SSH connections as well as multiple projects and parallel sessions, also with Git worktree integration. I also like having Claude Cowork and Chat in the same app, which makes it easy to navigate with keyboard shortcuts: there’s still some product research and general-purpose knowledge work I prefer to do in a non-code-first UI.[^7]
- [cmux](https://cmux.com/) is an open-source terminal powered by libghostty (the terminal library Ghostty is built on) with built-in vertical tabs and notifications for parallel sessions. Unlike Superset, it’s focused on the pure terminal experience and not an end-to-end coding workflow. What I like about cmux is the speed and the [native ssh support](https://cmux.com/blog/cmux-ssh) that handles things like automatic port forwarding – which is super useful when testing and visualizing mocks during brainstorming.

I landed on a hybrid setup. I like using the Claude Desktop app on my Macbook while SSH’d into my Mac Mini, but I keep the same interactive sessions open through the TUI in Ghostty. Each session is configured to run with /remote-control, which synchronizes all my sessions across both interfaces. It also makes my sessions accessible through the Claude mobile app, which gives me a nice interface to monitor, unblock, and steer on the go.

![](https://substackcdn.com/image/fetch/$s_!62dn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F751da239-6908-4089-b380-3577166c94a7_2120x1650.png)

My remote development setup that uses my Mac Mini as an always-on hub

For my actual workflow, I like interacting (read: speaking) with the TUI because it’s more responsive, reliable, and the first interface to receive new features. But the hybrid setup lets me scroll and read through execution history on the Claude desktop app, or quickly toggle between sessions, without interrupting the CLI. It’s a nicer interface than the terminal for reading specs and plan markdown files. Copy-paste and image input also work a lot better in the Desktop app: for example, you can’t drag an image from your local machine into the remote CLI session.

My setup definitely has some rough edges, and I think it reflects the current reality: there’s a lack of feature and UX parity across TUIs, desktop, mobile, and web interfaces. All the products I’ve tested are relatively new and immature, evidenced by the pace of development where significant features land every week. The good news is these products are maturing quickly,[^8] and I’m optimistic that within 6 months most of the kinks will be ironed out. Perhaps we’ll standardize on a less fragmented setup (and ideally one without platform lock-in!). For now, I’m relatively happy with what I have.

## Closing thoughts

An agent skills framework like /superpowers, testing automation, and better remote ergonomics compound on each other. The agent framework compresses my active keyboard (or dictation) time into efficient bursts. Testing automation lets my coding agents run autonomously for much longer, while producing better output. And remote-first ergonomics let me check in on those agents from anywhere, putting my idle time to work.

In the process of building and rebuilding with different tools and techniques, I’ve become convinced that building with AI is a skill, just like software engineering or product management. We all have access to the same frontier models, harnesses, and coding tools for the same [$100/month](https://georgexing.substack.com/p/all-you-need-is-a-claude-max-subscription), but the ability to use them to build good products is quite unevenly distributed.

In my experience, the most important skill for AI builders is two-fold: (1) being able to define what you want to build by making concrete product and design decisions during brainstorming, and (2) being able to define what success looks like clearly enough that your coding agent can run a code + verify loop with the right goals. To do both today, it still helps to understand software engineering fundamentals like data modeling, API design, and client/server architecture. In a year, I sense the models, harnesses, and tools might abstract this away. We might just be left with product taste and conviction.[^9]

The best way to learn the skills is simply to start building something – there’s no substitute for first-hand experience and knowledge. With how quickly tools and technologies are evolving, static resources (including this blogpost) become outdated quickly. In fact, the setup I’ve shared wasn’t even possible a couple months ago, and I fully expect it will be outdated a few months from now.

Lastly, as a builder, it feels incredibly exciting and empowering to surf such a big wave, but also dizzying because of how fast it moves. What I learned a couple months ago already feels dated. Every week I meet people attempting ever more ambitious things – running fully autonomous companies, orchestrating swarms of thousands of agents – and just keeping up feels like a full-time job.

If you’re also building, please reach out (contact info on my about page). I’d love to connect, learn, and trade notes!

[^1]: Garry Tan has a plugin called [gstack](https://github.com/garrytan/gstack) to solve the same problem. It’s the same conceptual approach but a different implementation. I haven’t yet tried gstack personally but I know folks who like it.

[^2]: There are also some additional optional skills that can be bolted onto the core ones, like managing git worktrees, and test-driven development.

[^3]: Ghostty is a terminal emulator that’s noticeably faster than the default macOS Terminal – once you’ve used it, the latency in the default Terminal app is hard to unsee.

[^4]: I’ve also tried the native Voice Mode in Claude Code, but found the transcription not as good. Also, since I use WisprFlow in all my other apps on both desktop and mobile, it’s easier to centralize on one set of global keyboard shortcuts.

[^5]: I originally bought my Mac Mini to set up OpenClaw and other personal AI assistants, but this was a nice side benefit. An old MacBook also works as well.

[^6]: Conductor, Cursor, and Warp all seem to be converging on the same problem space. I’m sure I’m missing more names.

[^7]: Feedback for the Anthropic team: it would be nice to have a more seamless experience here between Chat, Cowork, and Code.

[^8]: As of this writing, Superset has an alpha for supporting remote connections, as does Codex in their Desktop app. Claude has squashed a bunch of bugs in their Desktop app over the last couple weeks.

[^9]: Building with AI has definitely raised some existential questions for me on what software and software engineering actually means. More thoughts on this in a future post.