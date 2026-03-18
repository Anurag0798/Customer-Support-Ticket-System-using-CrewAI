# Customer Support Ticket System using CrewAI

This repository demonstrates a **multi-agent AI pipeline** built with CrewAI that simulates a real-world engineering incident response workflow. Given a batch of raw customer complaints, it automatically triages, investigates, and produces a full engineering action plan - all without human intervention.

The fictional EdTech mobile/web platform is modeled.

## Overview

The system processes:

- **5 customer complaints**, including issues such as:
  - Video buffering on mobile after 15 minutes
  - Payment deducted but course not unlocked
  - OTP not arriving during evening hours
  - App crashes when starting live tests
  - Dashboard performance issues after last update

- **Organizational context (`ORG_CONTEXT`)**:
  - Tech stack details (FastAPI, PostgreSQL, Redis, HLS/CDN)
  - Recent release notes (v2.7.0)
  - Past incidents
  - SLA targets


## The 7 AI Agents

Each agent specializes in a distinct role within the incident response workflow:

| Agent            | Role                                                                                     |
|------------------|------------------------------------------------------------------------------------------|
| `triage_lead`    | Incident Triage Lead  classifies priority (P0/P1/P2), estimates blast radius             |
| `support_analyst`| Customer Support Analyst  identifies complaint patterns, repro steps, affected cohorts  |
| `sre_infra`      | SRE/Infrastructure Analyst - investigates CDN, queues, autoscaling, rollback strategies  |
| `backend_analyst`| Backend & Data Analyst - analyzes APIs, async workers, race conditions                    |
| `qa_lead`        | QA Lead - creates test plans, device matrices, acceptance criteria                       |
| `comms_manager`  | Communications Manager - drafts status page updates, support macros                      |
| `tech_lead`      | Engineering Tech Lead - finalizes action plan with owners and timelines                   |

All agents use the `gpt-4o-mini` model via CrewAI’s LLM wrapper.


## The 7 Tasks (Sequential Pipeline)

The workflow runs tasks sequentially, each building upon the previous results:

1. **Task 0 (`triage_lead`)**
   Reads organizational context, generates risk hypotheses, and identifies key logs/metrics to monitor.

2. **Task 1 (`support_analyst`)**
   Groups complaints into incident buckets, assigns severity (P0/P1/P2), and provides reproduction hints.

3. **Task 2 (`sre_infra`)**
   Investigates infrastructure causes such as CDN or caching issues and proposes safe mitigations.

4. **Task 3 (`backend_analyst`)**
   Maps issues to backend services, identifies race conditions, and suggests code fixes.

5. **Task 4 (`qa_lead`)**
   Develops a comprehensive QA plan including reproduction steps, device matrix, test cases, and release checklist.

6. **Task 5 (`comms_manager`)**
   Drafts customer-facing communications such as status page updates and support macros.

7. **Task 6 (`tech_lead`)**
   Synthesizes all findings into a final engineering action plan, including bug tickets, postmortem outline, and JSON summary.


## Execution

To run the pipeline:

```python
crew = Crew(agents=[triage_lead, support_analyst, sre_infra, backend_analyst, qa_lead, comms_manager, tech_lead],
            tasks=[task0, task1, task2, task3, task4, task5, task6],
            process=Process.sequential,
            verbose=True)

result = crew.kickoff()
print("Final Incident + Engineering Report:\n", result)
```

`crew.kickoff()` executes all tasks in order and outputs a comprehensive incident report.

## Key Takeaway

This project illustrates agentic automation for operations and engineering workflows. It replaces what traditionally requires a team of specialists with an automated AI pipeline that produces structured, actionable outputs in minutes, demonstrating the power of multi-agent systems in real-world scenarios.

---

## Getting Started

1. Clone the repository:

```bash
git clone https://github.com/Anurag0798/Customer-Support-Ticket-System-using-CrewAI.git

cd Customer-Support-Ticket-System-using-CrewAI
```

2. Set up your Python environment and install dependencies:

```bash
pip install -r requirements.txt
```

3. Configure your environment variables, including API keys for CrewAI and OpenAI as needed.

4. Run the notebook or Python scripts provided to execute the multi-agent workflow.

## Contributing

Contributions and improvements are welcome! Please fork the repository, make your changes, and submit a pull request with a detailed description.


## License

This project is open-source. See the `LICENSE` file for full details.