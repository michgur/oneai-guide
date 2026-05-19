# Script Settings

A script defines what the agent does turn by turn during a call. It's made up of *goals* (the conversation flow), *responses* (FAQ-style canned answers), *metadata & variables* (script-wide values and runtime context), *custom vocabulary*, and *experiments*.

## Built-in goals

We ship a library of pre-built goals covering the common pieces of a phone conversation. You don't author these from scratch — they're enabled and configured through metadata switches and prompts in `default_metadata`. Most scripts start by configuring built-ins and only add custom goals for use cases the built-ins don't cover.

**Outgoing call opening**

The sequence that runs at the start of every outbound call. The agent introduces itself, confirms it has the right party, and navigates any obstacles before reaching the contact.

Things you configure:

- **Call intro** — what the agent says when the contact picks up; introduces itself and states why it's calling. (`%call_intro%`)
- **Acknowledge** — short line said after the contact confirms they're the right person. (`%speaker_verification_acknowledge%`)
- **Acknowledge on callback** — variant of the acknowledgement used on scheduled callbacks. (`%speaker_verification_acknowledge_callback%`)
- **Call screener handling** — how the agent responds when asked "who's calling? what's this about?" before reaching the contact. (`%call_screener_prompt%`)
- **IVR** — which extension or department to navigate to when the call hits a phone-tree menu. (`%IVR_TARGET%`)
- **Voicemail** — what the agent leaves when the call reaches voicemail, or disable voicemail-leaving entirely. (`%voicemail_message%`, `%voicemail_disabled%`)
- **Gatekeeper** — message left with a human gatekeeper who isn't the target contact, and how the agent opens the request to transfer the call to them. (`%human_message%`, `%transfer_opening%`)

**Incoming call opening**

The greeting sequence at the start of every inbound call.

Things you configure:

- **Welcome message** — what the agent says when the caller connects. (`%inbound_welcome_message%`)
- **Whether to ask for contact first name** — prompt the caller for their first name as part of the greeting. (`%inbound_ask_for_name%`)
- **Whether to ask for contact last name** — prompt the caller for their last name. (`%inbound_ask_for_last_name%`)

**Contact discovery**

When the wrong person answers, the agent asks for the right contact before continuing.

Things you configure:

- **What the call is about** — context given to the person who answered when asking if they know the contact. (`%contact_discovery_ownership%`)
- **Why the agent is looking for them** — reason given for needing to reach the contact. (`%contact_discovery_reason%`)
- **Discovery for named contacts** — whether discovery runs even when the platform already has a name on file for the contact. (`%contact_discovery_for_named_contacts%`)

**Meeting scheduling**

Books a meeting with the contact. Supports three modes: *voice* (book in-conversation), *SMS link* (text a booking link), and *callback* (schedule a callback time).

Things you configure:

- **Enabled modes** — which of the three scheduling methods are available. (`%scheduling-via-voice-allowed%`, `%scheduling-via-link-allowed%`)
- **Meeting type** — the kind of meeting being booked. (`%scheduling_meeting_type%`)
- **Scheduling link** — the booking URL sent via SMS. (`%scheduling_link%`)
- **Copy** — explainer and confirmation messages shown during scheduling, and SMS content. (related `%scheduling_…%` switches)
- **Timezone requirement** — whether the agent must confirm the contact's timezone before booking.
- **Convert on schedule** — whether a successful booking marks the contact as converted.
- **Next goal** — which goal to transition to after scheduling completes. (`%post_scheduling_goal%`)

## Goals & flow

A goal is a single step in the conversation. The script's goals run sequentially by default — each one activates, does its thing, and completes before the next.

Things you configure on each goal:

- **Execution mode** — *say* (the agent says something) or *ask* (the agent asks and collects a value back). (`goal_type`)
- **Generative vs scripted** — let the LLM rephrase the message naturally, or use the text verbatim. (`goal_type` ending in `_generative`)
- **Nesting** — group child goals under a parent. (`goals`)
- **Selection mode** — parent picks one child to run based on the situation rather than running them all. (`is_selection`, `choices`)
- **Template** — start from a built-in goal template and customize. (`copy_from`)
- **Enabled flag** — turn the goal off without removing it. (`enabled`)

## When a goal activates

Beyond the default sequential order, each goal can opt into different activation behaviors. Most goals only use one or two.

Things you configure:

- **Conditions** — eligibility rules using script variables and previously collected values.
- **Triggers** — user-intent phrases that reactively pull the goal in when the user says something matching, plus negative triggers to suppress unwanted matches.
- **Proactive firing** — whether the agent can decide on its own when to bring up the goal (on by default).
- **Priority** — overrides config order when multiple goals are eligible at the same time.
- **Qualifiers** — gate other goals: this goal must be satisfied before its siblings or children become eligible.
- **Transitions** — explicit branches from this goal to another, each named with the natural-language situation that should pick it. Can also assign or compute values during the transition.
- **Repeat behavior** — once, indefinitely, or up to a max count; with cooldown before retrying after a failure.

## What the goal says

Things you configure:

- **Messages** — one or more variants of what the agent will say or use as a prompt. By default the agent rephrases them naturally. Each message supports:
  - *Text* — the line itself. (`message`)
  - *Conditions* — when this variant is picked. (`conditions`)
  - *Pre-message* — text spoken before the main line in the same turn (e.g. an acknowledgement or filler). (`pre_message`)
  - *Explicit part* — the portion of the message that counts as the actual question, used to control where the user can interrupt. (`explicit_part`)
  - *Objection-handler flag* — mark this variant as the one used when the user objects, rather than a regular ask. (`objection_handler`)
  - *Generative* — whether the message should be said verbatim or used as instructions for the agent. (`generative`)
- **Force exact wording** — bypass LLM rephrasing for compliance or scripted moments. (`force_exact_response`)
- **Acknowledgement** — short filler after the goal completes ("got it", "perfect"); customizable or off. (`acknowledge`)
- **Objection handling** — top-level messages used when the user refuses to answer, plus how many times to retry before moving on. (`objection_handling`, `objection_handling_attempts`)
- **Interruptibility** — by default the user can interrupt the agent mid-sentence; mark critical goals (e.g. just before a transfer) uninterruptible. (`uninterruptible`)

## Value collection

When a goal asks for something, you tell the platform what kind of value it expects and how to validate it.

Things you configure:

- **Value type** — approval (yes/no), selection from choices, a named entity (email, phone, date, number, name, unit like money), or a custom type for free-form data. (`value_type`)
- **Field name** — where the collected value is stored on the call (defaults to the goal name). (`detection_name`)
- **Numeric range / unit type** — declarative constraints for number- and unit-typed values. (`options.range`, `options.unit_type`)
- **Custom validation prompt** — for nuanced extraction or validation beyond declarative options. (`validation_prompt`)
- **Triggering-message evaluation** — if the user already supplied the value in the message that triggered the goal, recognize that and skip re-asking. (`evaluate_on_trigger`)

## Fulfillment & tools

How a goal *does things* beyond talking. The same execution primitives are exposed two ways:

- **Tools** — the LLM decides when to call them mid-conversation. Each tool has a name, description, and parameter schema. (`options.llm_tools`)
- **Fulfillment** — actions wired to platform events on the goal's lifecycle: when it's *triggered*, *performed*, *collected*, *completed*, or when the agent says a specific phrase. (`fulfillment[].timing`)

Action types available to both:

- **Webhook** — POST to an external URL with custom headers and body; optionally map the response back into call state.
- **SMS** — send a text via the configured messaging service, to the contact or any other number.
- **CRM update** — push fields to HubSpot (and similar integrations).
- **Hang up** — end the call after the current line or immediately.
- **Forward call** — transfer the call to another number (the AI hands off after).
- **Warm transfer** — place a parallel child call to merge in, with idle messages while waiting and a fallback message if it fails.
- **Set values** — assign values into the call context.
- **Update call result** — set the contact's outcome (qualified, converted, disqualified, do-not-call).
- **Trigger another goal** — bring another goal into the flow.
- **Override agent response or user query** — force a specific agent line or rewrite the user's query for the current turn.

**Automations**

The same action types are also available outside the real-time call loop. Automations fire at end-of-call or end-of-sequence — use them for post-call work like running an LLM query over the transcript, pushing fields to a CRM, or marking contacts do-not-call. (`fulfillment_tasks`)

## Metadata & variables

Two flavors of variables are available in goal messages, prompts, and conditions.

**Dynamic variables** you define yourself, with default values in `default_metadata` and per-call overrides at call creation. Use these for anything that varies per customer, campaign, or contact.

**Read-only runtime variables** the platform exposes — useful in conditions and message templates:

- **Time & schedule** — current date/time, this week / next week, whether the current time is within hot-transfer working hours. (`%today%`, `%now%`, `%this_week%`, `%in_transfer_working_hours%`)
- **Conversation state** — current turn number, the user's last message (raw and lowercase), whether the call is inbound or outbound. (`%turn_number%`, `%user_message%`, `%call_type%`)
- **Contact** — first/last/full name, phone, timezone, contact source. (`contact_name`, `phone`, `timezone_name`, `contact_source`)
- **Outcome state** — meeting scheduled (local + UTC times), voicemail count, callback requested, whether a new contact was discovered. (`%meeting_scheduled%`, `%voicemail_count%`, `%callback_requested%`, `%contact_discovered%`)

## Custom vocabulary

Words and phrases the transcription engine might otherwise mishear — product names, brand terms, jargon.

Things you configure per entry:

- **The word or phrase** itself.
- **Entity label** — optionally tag what kind of thing it is.
- **Goal hint** — optionally bias the recognized phrase toward routing to a specific goal.

## Experiments

A/B test groups defined at the script level.

Things you configure per experiment:

- **Name & description** — what's being tested.
- **Enabled flag** — toggle the whole experiment without removing it.
- **Test groups** — one or more variants, each with a name and the percentage of calls assigned.
- **Per-group overrides** — voice settings (voice id, speed, volume), telephony settings (max dial seconds, area code preference), or goal-level changes applied only to that group.
