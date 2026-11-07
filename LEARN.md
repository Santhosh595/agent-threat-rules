# Learn — agent-threat-rules

## What it is

An open detection-rule standard for AI-agent security threats — "Sigma, but for AI agents." Ships executable, testable rules for prompt injection, tool poisoning, context exfiltration, and MCP attacks. **Merged upstream** into projects at Microsoft, Cisco, Gen Digital, MISP, and FINOS. MIT-licensed. This is the repo most likely to be recognized by interviewers.

**Read this first, then go file-by-file in the repo.**

## 5-minute quickstart

```bash
git clone https://github.com/Santhosh595/agent-threat-rules && cd agent-threat-rules
cat README.md | head -80   # the taxonomy + format spec are here
npm test 2>/dev/null || python -m pytest -q   # check README for test command
ls rules/  # or wherever rules live — list them
```

## Concepts — the "why" behind each file

| If you see this in the code | What it means | Viva answer |
|---|---|---|
| Rule schema (fields, levels, tags) | Detection-as-code: rules are versioned artifacts | Rules beat prose — they execute and test |
| Sigma analogy (`logsource`, `detection`, `condition`) | Familiar structure for SOC folks | Mapped to agent telemetry (tool calls, prompts) |
| 4 rule families: injection, tool poisoning, exfiltration, MCP | Your taxonomy | Each family = an attack class with examples |
| Telemetry sources per rule | Every rule names its data source | A rule without telemetry is a wish |
| Positive/negative fixtures | Rule tests: this must fire, this must not | That's why upstream merged it — it's tested |
| Rule versioning / deprecation | Maintenance of detection content | Rules rot; version + test or they lie |
| False-positive notes | Honest error profile | Every rule states when it over-fires |

## Common pitfalls

1. **Rules without telemetry** — a perfect rule on data you never collect = zero detections. Always name the source.
2. **One rule, one string match** — brittle. Rules need structure (sequences, thresholds), not just keywords.
3. **No negative fixtures** — if you only test "must fire", you'll ship a false-positive factory.
4. **Copying Sigma's syntax blindly** — agent events (tool calls, context chunks) aren't log lines. Adapt the model.
5. **Claiming detection rates** — you haven't measured them. "Tested fixtures, not production rates" is the honest line.

## Hands-on, in this order

1. List every rule. For each: name the attack family, the telemetry source, the match logic.
2. Run the test suite. Pick one rule — find its positive fixture (must fire) and negative (must not).
3. Write a new rule for a threat not covered. Follow the schema exactly. Add fixtures.
4. Read MITRE ATLAS: https://atlas.mitre.org/ — map 3 of your rules to ATLAS techniques.
5. Read the OWASP Agentic AI threats: https://github.com/OWASP/Agentic-AI-Threats-and-Mitigations — does your taxonomy cover their top items? Gaps?
6. Study a mature rule set for structure: https://github.com/SigmaHQ/sigma-specification (the spec you modeled on)

## Viva questions (with the trap)

1. **"Why rules instead of a threat-model PDF?"** Answer: rules execute, test, and version; PDFs rot. Trap: naming rules without telemetry.
2. **"What telemetry does a rule need?"** Answer specifically per rule: tool-call logs, prompt text, retrieved-chunk metadata, MCP server configs. Vague = fail.
3. **"Design a rule live."** Answer: schema → data source → match → threshold → false-positive risk. Practice one out loud.
4. **"What does 'merged upstream' prove?"** Answer: maintainers at 5 orgs reviewed and accepted it — design quality signal.
5. **"Python/TS — why TS here?"** Answer: ecosystem fit for the consumers (or say honestly: it matches where agent tooling lives).

## 3 things to say if stuck

- "Every rule is executable and has fixtures — that's the difference from documentation."
- "The schema is Sigma-inspired, adapted for agent telemetry."
- "Upstream merges are the quality proof; I can show the PRs."

Good luck. This is your strongest repo for AI-security roles — know the taxonomy cold.
