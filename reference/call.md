# Call-Level Parameters

### Core Configuration

| Parameter | Description |
| :---- | :---- |
| `default_metadata` | Used for(i) dynamic variable default values(ii) configuring built in goals |
| `goals` | Goals that define the conversation flow, by default are run sequentially |
| `goals_update` | Override built in goals |
| `responses` | FAQ |
| `custom_vocabulary` | Words/phrases to improve transcription accuracy, with optional entity labels and goal hints. |
| `experiments` | A/B test groups that apply config overrides to a percentage of calls. |

### Built-in Control Variables (set in `default_metadata`)

| Variable | Description |
| :---- | :---- |
| `%company_name%` | The company name the agent represents. |
| `%agent_name%` | The name of the AI agent. |
| `%agent_phone_number%` | The outgoing phone number used for the current call. |
| `%call_intro%` | How the agent introduces itself at the start of a call. |
| `%speaker_verification_acknowledge%` | How the agent acknowledges the user after identity confirmation (outbound). |
| `%speaker_verification_acknowledge_callback%` | Acknowledgement used when answering a scheduled callback. |
| `%inbound_welcome_message%` | How the agent greets callers on inbound calls. |
| `%inbound_ask_for_name%` | Controls whether the agent asks for the caller's name on inbound calls (default: yes). |
| `%inbound_ask_for_last_name%` | Controls whether the agent proactively collects last name on inbound calls (default: no). |
| `%voicemail_message%` | Message left when the call reaches voicemail. |
| `%human_message%` | Message left with a human gatekeeper. |
| `%call_screener_prompt%` | Custom instructions appended to default call screener handling behavior. |
| `%contact_discovery_ownership%` | Describes what the agent is calling about, used for contact discovery. |
| `%contact_discovery_reason%` | Explains why the agent is looking for the right contact. |
| `%contact_discovery_for_named_contacts%` | Applies contact discovery even to known contacts when set to "yes". |
| `%max_not_interested%` | Number of "not interested" statements before the agent hangs up (default: 2). |
| `%email_collection_message%` | Custom message used when asking for the user's email address. |
| `%callback_blocked%` | Set to "yes" to prevent the schedule-callback goal from triggering. |
| `%scheduling-via-voice-allowed%` | Enables the built-in voice scheduling goal when set to "yes". |
| `%scheduling_meeting_type%` | Type of meeting to schedule (default: "online"). |
| `%scheduling_voice_message_explainer%` | How the agent introduces the scheduling process. |
| `%scheduling_voice_confirmation_question%` | Custom question used to confirm a meeting slot with the user. |
| `%scheduling_voice_confirmation_email%` | Confirmation message used when user email is available. |
| `%scheduling_voice_confirmation_text%` | Confirmation message used when sending an SMS (no email). |
| `%scheduling_voice_sms_message%` | Content of the SMS sent after scheduling; empty to disable. |
| `%scheduling_voice_just_moment%` | What the agent says while booking a meeting (default: "Just a moment.."). |
| `%scheduling-via-link-allowed%` | Enables scheduling via SMS link instead of voice (default: "no"). |
| `%scheduling_text_message_explainer%` | What the agent says before sending a scheduling link. |
| `%scheduling_text_message_content%` | Content of the scheduling SMS message. |
| `%scheduling_callback_intro%` | How the agent opens callback scheduling (default: "I understand, I will call you back..."). |
| `%scheduling_filter%` | Custom scheduling filters the agent should be aware of. |
| `%post_scheduling_goal%` | Goal to transition to after scheduling is completed. |
| `%scheduling-invite-to-email%` | When set, collects user email instead of phone during scheduling. |
| `%scheduling_requires_timezone%` | Controls whether timezone is required during scheduling. |
| `%scheduling-is-conversion%` | Whether successful scheduling marks the contact as converted (default: "yes"). |
| `%schedule_message_ask_for_mobile_phone%` | Message used to ask for mobile phone during scheduling. |
| `%scheduling_link%` | URL for the scheduling link sent via SMS. |
| `%transfer_opening%` | How the agent opens a gatekeeper transfer attempt. |
| `%IVR_TARGET%` | Directs the agent on which extension/department to select in an IVR. |
| `%callback_requested%` | Indicates whether a callback was successfully scheduled (yes/no). |
| `%callback_schedule%` | The scheduled callback time; check at call start to identify callbacks. |

### Built-in Response Variables (set in `default_metadata`)

| Variable | Description |
| :---- | :---- |
| `%responses_agent_name%` | How the agent answers "what is your name?" |
| `%responses_reason_for_call%` | How the agent explains why it's calling. |
| `%responses_company_name%` | How the agent identifies the company it represents. |
| `%responses_are_you_ai%` | How the agent responds when asked if it's an AI. |
| `%responses_dont_like_ai%` | How the agent responds when the user dislikes speaking to AI. |
| `%responses_list_source%` | How the agent explains how it got the user's contact info. |
| `%responses_talk_to_a_human%` | How the agent responds when the user asks to speak to a human. |
| `%responses_is_this_a_scam%` | How the agent responds when the user suspects a scam. |

### Read-Only Built-in Variables (usable in conditions/messages)

| Variable | Description |
| :---- | :---- |
| `%call_type%` | Whether the call is `phone_inbound` or `phone_outbound`. |
| `%turn_number%` | The current turn number in the conversation. |
| `%today%` | Full current date including year. |
| `%today_short%` | Current date excluding year. |
| `%now%` | Current date and time. |
| `%this_week%` / `%next_week%` | Text description of the current or next week. |
| `%utc_minute%` | Minutes from the start of the week in UTC, used for working hours checks. |
| `%in_transfer_working_hours%` | Whether the current time is within configured hot-transfer working hours (yes/no). |
| `%user_message%` | The raw text of the user's last message. |
| `%user_message_lower%` | Lowercase version of the user's last message. |
| `%meeting_scheduled%` | Whether a meeting has been scheduled (yes/no). |
| `%meeting_scheduled_time_local%` | Scheduled meeting time in the contact's local timezone. |
| `%meeting_scheduled_time_utc%` | Scheduled meeting time in UTC. |
| `%voicemail_count%` | Number of voicemails left for the contact. |
| `%voicemail_disabled%` | Set to "yes" to prevent voicemail messages from being left. |
| `%contact_discovered%` | Indicates whether a new contact was discovered during the call (yes/no). |
| `%child_call_start%` | Timestamp when the child transfer call was initiated. |
| `%merge_time%` | Timestamp when the contact was merged with the transfer call. |

### Contact Built In Variables

| Variable | Description |
| :---- | :---- |
| `contact_name` | Contact's first name. |
| `contact_last_name` | Contact's last name. |
| `contact_full_name` | Contact's full name. |
| `phone` | Contact's phone number. |
| `timezone_name` | Contact's timezone in ISO format. |
| `contact_source` | The list, URL, or campaign the contact came from. |

### Experiments

| Parameter | Description |
| :---- | :---- |
| `experiments[].name` | Display name for the experiment. |
| `experiments[].description` | Optional description of what is being tested. |
| `experiments[].enabled` | Whether the experiment is currently active. |
| `experiments[].test_groups[].name` | Name of the test group/variant. |
| `experiments[].test_groups[].percent` | Percentage of calls assigned to this group. |
| `experiments[].test_groups[].config` | Config overrides applied to this group (e.g. `voice_config`, `twilio_config`). |
| `experiments[].test_groups[].goals_update` | Goal-level overrides applied only to this test group. |
| `voice_config.voice_id` | ElevenLabs or other TTS voice ID to use. |
| `voice_config.speech_speed` | Playback speed multiplier for the agent's voice. |
| `voice_config.speech_volume` | Volume multiplier for the agent's voice. |
| `twilio_config.max_dial_seconds` | Maximum seconds to wait for the call to be answered. |
| `twilio_config.prefer_same_area_code` | Whether to prefer a caller ID matching the contact's area code (true/false). |

### Responses

| Parameter | Description |
| :---- | :---- |
| `responses[].questions` | List of sample questions that trigger this response. |
| `responses[].answers` | List of answer strings or message objects to use. |
| `responses[].conditions` | Conditions that must be met for this response to be eligible. |
| `responses[].negative_triggers` | Phrases that prevent this response from triggering. |
| `responses[].force_response` | When true, the agent delivers the answer exactly as written without rephrasing. |
