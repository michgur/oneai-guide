# Agent Settings

Settings that define *who* the agent is. The *what it does in a call* lives in the script (call & goal settings).

## Identity

The persona is the agent's self-description — written in natural language and prepended to its system message. This is where you say "you are a helpful onboarding specialist at Acme who is warm but to the point." (`identity`)

The agent has two names: an internal one shown in the admin UI (`display_name`) and a public-facing one used in conversations (`public_name`).

You pick the model that generates the agent's responses (`llm_model_to_use`) and how predictable you want it to be — lower temperature for compliance-heavy use cases, higher for friendlier improvisation (`temperature`).

Finally, an agent binds to a single script version that defines its conversation flow (`director_config_id`). The script itself is configured separately — see the call & goal guides.

## Voice

The voice character — the model, the specific voice, its speed, emotional tone, and volume — determines how the agent sounds. (`voice_config.voice_model`, `voice_id`, `voice_speed`, `voice_emotion`, `voice_volume`)

## Call operations

Three operational limits per call: how long a call can run before the platform ends it (`max_call_duration`), how much continuous silence ends it (`max_silence_duration`), and whether the call is recorded (`recording_enabled`).

> **Sequence data retention.** We run call *sequences*, not isolated calls. During a call, the agent collects fields from the contact. By default these reset between calls in the sequence — useful when you want to re-ask. For fields you only ever want to collect once, list them here. (`cross_call_metadata_retention`)

## Automations

Automations that run *outside* the call's real-time loop. They fire at end-of-call or end-of-sequence and can call webhooks, run LLM queries over the conversation, or override contact state (e.g. mark as do-not-call, update CRM fields). Use them for anything you'd otherwise do manually after a call. (`fulfillment_tasks`)

## Telephony

By default, the platform provisions a Twilio subaccount for you — you don't manage credentials. What you *will* still deal with: regulatory provisioning (carrier requirements for the regions you call) and number management (acquiring, assigning, and retiring phone numbers).

For orgs that want full control of their telephony, BYO Twilio is supported — you provide the account SID, auth token, and number, and the platform routes through it. (`twilio_config.account_sid`, `auth_token`, `phone_number`)
