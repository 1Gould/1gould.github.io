---
title: "OSAI Review"
description: "My experience with AI-300, the OSAI exam, and building an AI pentesting agent."
date: 2026-11-17 00:00:00 +0000
---

## Course

Passed the OSAI exam last week and wanted to share my experience with the course, exam, and building my own AI pentesting harness. [OffSec AI Red Teamer](https://www.offsec.com/courses/ai-300/) is OffSec's certification focused on exploiting AI-enabled enterprise environments. 

AI-300 covers a range of topics such as:

- Attacking LLMs
- Multi-Agent AI Systems
- RAG Pipelines
- Embeddings
- AI Infrastructure

As with all OffSec courses, the material is very thorough with a deep understanding of how the systems are designed, interact with other systems, and how they can be exploited.

## Labs

Initially there were minor issues with the capstone and challenge labs, but in the weeks leading up to my exam I had no issues. It's expected there are a few hiccups for a new course and the stability improved noticeably.

If you have experience taking other OffSec courses then you know what to expect in regards to challenge labs and the exam. They provided a good learning environment with challenging objectives. You will learn a lot of how the systems interacting with each other create new opportunities for exploitation. The last challenge lab is the perfect place to train and iterate on your agent(s), and serves as a simulated exam.

## Agent Setup

Before we get into developing your agent, here's an overview of what kind of options you have available when developing and running it. 

### Models

Going by the discord and some offensive security researchers on X, here are the most popular models to consider:

- 5.6 Sol
- GLM 5.3 / flash
- DeepSeek v4.1 flash 1017
- Kimi k3 or k3.1
- Grok 4.6/4.7

I left claude out because too many refusals makes it very difficult to get any value from it, and the approvals being very selective. [blackboxarena](https://blackboxarena.ai/) provides some rankings in regards to offensive security benchmarks for models to get you started.

For the purpose of the exam, token usage and cost - in my opinion - is not really a useful metric. Ultimately you want two things from your model, power and speed. Providers have DeepSeek with high tokens per second throughput making it a good choice in a balance of speed and reasoning. Standard 5.6 Sol runs at approx 80-100 tok/s, versus DeepSeek at 150-200.

### Harness

There are several popular options when it comes to harnesses:

- Codex
- Claude Code
- Pi
- Oh-My-Pi (omp)
- Hermes
- OpenCode

...and many more.

These harnesses vary in added capabilities giving your agent more access to things such as; subagents, web search, documentation, summarization etc. [Harness Leaderboard](https://huggingface.co/spaces/joelniklaus/harness-leaderboard) details how the harnesses rank against each other in performance and cost per task solved. Ultimately there isn't a huge difference between them in performance so just pick one you like. My only additional comment here would be it can be tricky to get daybreak blue to work on anything other than Codex.

## Building the Agent

Start with taking good notes from the course material, this will help tremendously when you decide to start building your agent and turning notes into skills. I found cheatsheets for command references and syntax useful to prevent the agent from spending time and tokens determining how to execute a command. This is pretty standard pentesting documentation but will be especially useful here. Documenting detailed **prerequisites** for an attack's success will help it avoid rabbit holes, and continuing down a path that can't be exploited.

Here is a sample of some of the structure for my agent:

```text
osai-agent/
|--- artfacts/
     |--enum/
     |--loot/
     |--payloads
     |--tool_calls/
     |--...
|--- docs/
|--- engagement/
     |--engagement.md
     |--operations.md
     |--router.md
     |--state.json
     |--...
|--- payloads/
|--- references/
     |--cheatsheets/
     |--checklists/
     |--operations/
     |--...
|--- tools/
     |--apppend_audit.py
     |--checklist.py
     |--checkpoint.py
     |--init_engagement.py
     |--read_audit.py
     |--refresh.py
     |--run_audit.py
     |--update_state.py
     |-...
```

I recommend creating skills for each category of attack and taking some inspiration from these open source repos:

- [Red Run](https://github.com/blacklanternsecurity/red-run)
- [Claude Red](https://github.com/SnailSploit/Claude-Red)
- [ClaudeAD](https://github.com/ADScanPro/Claude-AD)

I start all of my skills with this general template:

```markdown
# <SKILL NAME>

Follow [AGENTS.md](AGENTS.md), and [osai-engagement](.codex/skills/osai-engagement/SKILL.md); that skill owns authorization, audit, evidence, state changes, and safety.

## MANDATORY CONTRACT - read before doing anything else

If this skill was invoked, you MUST read the ENTIRE SKILL.md before you perform the skill or touch a target. A 150-line skim is enough only to decide whether the skill applies. Read only the reference files selected by the current evidence and routing rules, and read each selected reference in full. Do not load unselected references.

## Mandatory steps

1. Read the exact goal, path, host, access context, route, objective, finding, and cleanup records; run `tools/state.py checklist status` and `next` for the exact context.
2. Select one TTP and cheatsheet needed for the current evidence.
3. Select the checks with the best goal relevance and expected information gain; define each chosen action, expected result, evidence, impact, abort conditions, and cleanup debt.
4. Run every target-facing action through `tools/run_audit.py` and review its audit entry and artifacts before the next action.
5. Hand specialized findings to the correct skill and stop or re-rank when the selected review is sufficient, a higher-value route is ready, or an authorization or safety boundary is reached.
```

### The Core Loop

There are several approaches you can take with the agent, here are the main ones that I tested with using. Naturally there are many more options that you could go with, these are mainly **autonomous** designs, but another option is using the agent as your assistant and not running any commands itself.

| #   | Model             | How it works                                                                          | Main advantage                                     | Main risk                                                                                         |
| --- | ----------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 1   | Single agent      | The main agent plans, executes, reviews evidence, and updates state.                  | Simple control and evidence handling.              | Slow; long investigations block other work; large context.                                        |
| 2   | Pure orchestrator | The main agent assigns all execution to subagents and only coordinates their results. | Clear separation and parallel execution.           | Coordination overhead; the orchestrator can lose important command-level context.                 |
| 3   | Hybrid lead agent | The main agent executes short, dependent, or sensitive actions; workers handle bounded independent investigations. | Good balance of control, speed, and context retention. | Requires clear rules for when to delegate. |
{: .comparison-table}

#### 1. Single Agent

![Single agent sends actions to a target, reviews returned evidence, and produces a result.]({{ '/assets/images/osai-review/single-agent.png' | relative_url }}){: width="552"}

#### 2. Pure Orchestrator

![An orchestrator assigns work to two workers, reviews evidence from their targets, and produces a result.]({{ '/assets/images/osai-review/pure-orchestrator.png' | relative_url }})

#### 3. Hybrid Lead Agent

![A lead agent handles dependent actions directly and delegates independent tasks to workers before producing a result.]({{ '/assets/images/osai-review/hybrid-lead-agent.png' | relative_url }})

Start small and iterate, AI is your best friend here, don't be shy for using the AI to help build itself. Bounce ideas off it, build reusable tools, and test your harness on the labs.

### Feedback Loop

The time with the challenge labs is extremely valuable for testing and iterating on what you have built during the course. Here is where you get to test what you've built on a real challenge environment based on the course material, and if it can adapt to new scenarios. Keep an audit of everything you do in these labs, what worked, what didn't work, what paths were missed etc. 

The harness you are using will keep a session log / history of the entire conversation. This also provides a valuable resources, feed it into your agent and ask it for improvements.

Here are some additional resources for experimenting:

- [PortSwigger's LLM attacks](https://portswigger.net/web-security/llm-attacks)
- [HackAPrompt](https://www.hackaprompt.com/)
- [Lakera Agent Breaker](https://www.play.lakera.ai/)

## Conclusion

![Daniel Ian Gould's OSAI certificate, awarded by OffSec on November 8, 2026.]({{ '/assets/images/osai-review/osai-certificate.png' | relative_url }})

Overall I learned a lot from the material and challenge labs for exploiting AI/ML environments. Highly recommend taking the course if you are interested in improving your knowledge of offensive security in these areas. Developing your own agent is valuable and will continue to be something we see become more prominent in the future for both red and blue teams. The most valuable part is turning your *repeatable* methodology into agent skills that can be benchmarked, tested and improved.

## Further Research

### Harness Engineering

- [Harnessing Harnesses](https://blog.zsec.uk/harnessing-harnesses/)
- [Introduction to Harness Engineering for Vulnerability Research](https://grahamhelton.github.io/harness-engineering-workshop-book/introduction.html)
- [Post on X](https://x.com/bytemohit/status/2063493300884246598)

### Books

- [AI Agents and Applications](https://www.manning.com/books/ai-agents-and-applications)
- [Build a Large Language Model](https://www.manning.com/books/build-a-large-language-model-from-scratch)
- [Practical AI Security](https://nostarch.com/practical-ai-security)