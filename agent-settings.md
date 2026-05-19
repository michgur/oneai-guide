# Agent Settings

Settings that define *who* the agent is. The *what it does in a call* lives in the script (call & goal settings).

## Identity & responses

**Identity**

Who the agent is and how it should behave — these values shape the agent's system message and are referenced throughout the conversation.

Things you configure:

- **Name** (string) — the agent's public-facing name used in conversation. (`%agent_name%`)
- **Company name** (string) — the company the agent represents. (`%company_name%`)
- **Voice** (object) — how the agent sounds; see [Voice](#voice) below.
- **Objective** (string) — the agent's reason for calling; frames its goal and shapes how it handles the conversation.
- **Guardrails** (string) — things the agent should never say or do, regardless of what the user asks.
- **Additional instructions** (string) — freeform instructions appended to the system message; normally left empty.

**Responses**

A flat list of question→answer pairs that fire when the user asks something the goal flow wasn't designed to handle. Two layers — built-in responses (pre-configured answers for common questions) and custom responses (entries you define yourself).

Things you configure per entry:

- **Sample questions** (array) — phrasings of the question this entry should match.
- **Answers** (array) — one or more answers; the agent picks among them.
- **Conditions** (array) — required state for this entry to be eligible.
- **Negative triggers** (array) — phrases that should suppress this match.
- **Force exact wording** (boolean) — deliver the answer verbatim. (`force_response`)

## Voice

How the agent sounds.

Things you configure:

- **Model** (string) — the underlying voice synthesis model. (`voice_config.voice_model`)
- **Voice** (string) — the specific voice ID within the model. (`voice_config.voice_id`)
- **Speed** (number) — playback speed multiplier. (`voice_config.voice_speed`)
- **Volume** (number) — volume level. (`voice_config.voice_volume`)

## Call operations

How and when the agent runs calls — both the dialer's behavior across calls and the limits on each running call.

Things you configure:

- **Agent enabled** (boolean) — master switch for whether the agent dials at all. (`agent_is_enabled`)
- **Working hours** (object) — allowed call windows per day of the week, evaluated in the configured timezone. (`working_hours`, `timezone`)
- **Blackout dates** (array) — date ranges during which no calls go out (holidays, freezes). (`excluded_date_ranges`)
- **DNC registry check** (boolean) — check the Do-Not-Call registry before dialing and skip listed numbers. (`voice_config.check_dnc_registry`)
- **Concurrency** (integer) — maximum simultaneous outbound calls. (`max_concurrent_calls`)
- **Retries** (integer, array) — how many times to retry after a failed attempt and the delay between attempts. (`retry_limit`, `retry_intervals`)
- **Fresh vs. retry mix** (number) — fraction of each dialing batch that targets never-contacted contacts vs. retrying existing ones. (`fresh_ratio`)
- **Hot transfer queue** (string) — the human-agent queue warm/hot transfers route to. (`hot_transfer_agent_id`)
- **Human team working hours** (object) — separate working hours for the human transfer team; used both to gate dialing and to decide whether hot transfers are available. Calls outside this window can optionally still be placed. (`expert_humans_working_hours`, `expert_humans_timezone`, `allow_outside_expert_humans_working_hours`)
- **Contact dedup keys** (array) — additional fields beyond phone number used to identify a contact uniquely. (`unique_keys_name`)
- **Per-call duration cap** (integer) — how long a call can run before the platform ends it. (`max_call_duration`)
- **Silence cutoff** (integer) — how much continuous silence ends a call. (`max_silence_duration`)
- **Recording** (boolean) — whether calls are recorded. (`recording_enabled`)

## Telephony

By default the platform provisions a Twilio subaccount for you and manages credentials. BYO Twilio is supported for orgs that want full control.

Things you configure:

- **Regulatory provisioning** (object) — carrier requirements for the regions you call; some jurisdictions require registration before dialing can begin.
- **Number management** (object) — acquiring, assigning, and retiring phone numbers used as caller IDs.
- **BYO Twilio credentials** (string) — for orgs running their own Twilio account, the account SID, auth token, and phone number that calls route through. (`twilio_config.account_sid`, `auth_token`, `phone_number`)
