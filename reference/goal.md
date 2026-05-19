# Goal-Level Parameters

### Identity & Structure

| Parameter | Description |
| :---- | :---- |
| `name` | **Unique** goal name; also used as the collected value's field name. |
| `id` | Optional identifier string for referencing the goal. |
| `goals` | List of child goals nested under this goal. |
| `choices` | Child goals used as selection options for `selection` or `approval` value types. |
| `is_selection` | Set to `true` to only select a single child goal instead of running them all sequentially. |
| `copy_from` | Copies a built-in goal template (e.g. `"confirm-email"`) as the base for this goal. |
| `options.llm_model` | Specifies which LLM model to use. |
| `options.llm_tools` | Tool definitions passed to the agent. |
| `options.llm_tools[].name` | Tool name. |
| `options.llm_tools[].description` | Tool description. |
| `options.llm_tools[].parameters` | Tool parameters, JSON schema. |
| `options.llm_tools[].enabled` | Whether the tool is enabled, default `true`. |
| `options.llm_tools[].fulfillment` | Tool execution configuration, same structure as `fulfillment`. |
| `transitions` | List of transitions to other goals for the agent to select. |
| `transitions[].name` | Transition tool name. |
| `transitions[].prompt` | Natural language description of when to activate the transition. |
| `transitions[].target` | Name of goal to move to. |
| `transitions[].properties` | Optional values for the agent to generate and update call state before activating the transition, JSON schema. |
| `transitions[].values` | Optional values for the system to compute and update call state before activating the transition, key-value pairs. |
| `repeat` | `true` to repeat indefinitely, a number to set a max repeat count, `false` to disable. |
| `repeat_conditions` | Condition that must be true for the goal to repeat again. |
| `cooldown` | Number of turns to wait before retrying a goal after it fails or is skipped. |
| `backoff` | Instructs the agent to back off the entire goal branch if any goal in it enters cooldown. |

### Goal Type & Value

| Parameter | Description |
| :---- | :---- |
| `goal_type` | Execution mode: `ask`, `ask_generative`, `say`, `say_generative`. |
| `value_type` | Type of value to collect: `approval` (yes / no), `selection`, `name`, `phone`, `email`, `date`, `number`, `unit`, `custom`, etc. |
| `detection_name` | Definition of the collected value, goal name is used by default. |
| `options.range` | Min/max array to restrict valid numeric values `[min, max]`. |
| `options.unit_type` | Restricts unit detection to a specific type (e.g. `"money"`). |
| `options.all_messages` | When true, evaluates all messages for value detection, not just the most recent. |
| `options.llm_function_property_name` | Property name hint passed to the LLM function for value extraction. |
| `options.llm_function_property_description` | Description hint passed to the LLM function for value extraction. |
| `validation_prompt` | Custom prompt(s) used to validate or extract the collected value. |
| `detection_model` | Detection model override, e.g. `"llm_function"` for stronger/nuanced detection. |
| `evaluate_on_trigger` | When true (default), checks if the triggering message already satisfies the ask goal. |

### Triggering & Conditions

| Parameter | Description |
| :---- | :---- |
| `enabled` | Set to `false` to completely disable a goal. |
| `conditions` | Logical expression or structured object controlling when this goal is eligible. |
| `triggers` | List of user intent phrases that reactively trigger this goal. |
| `negative_triggers` | Phrases that prevent this goal from triggering (reactive or proactive). |
| `trigger_scope` | Limits trigger detection scope: `global` (default), `local`, `active`, or a specific goal name. |
| `proactive` | Set to `false` to prevent the goal from being triggered proactively. |
| `cost` | Minimum conversation satisfaction score required for proactive triggering. |
| `priority` | Overrides config order for goal selection; higher values execute first. |
| `handle_response_type` | Set to `"information-not-found"` to handle unknown/unanswerable questions. |
| `is_qualifier` | When true, this goal must be completed before child/sibling goals can trigger. |
| `qualified_value` | The value that satisfies the qualifier (default: `yes` for approval, any for others). |
| `qualifier-blocks` | Controls qualifier blocking scope: `auto`, `children`, `siblings`, or `None`. |

### Messages & Response

| Parameter | Description |
| :---- | :---- |
| `messages` | The message(s) the agent will say or use as a prompt; string, list, or list of objects. |
| `messages[].message` | The text of a specific message variant. |
| `messages[].conditions` | Conditions that must be met for this specific message to be used. |
| `messages[].explicit_part` | The portion of the message that constitutes the actual question, used for interruption handling. |
| `messages[].pre_message` | Text spoken before the main message in the same turn. |
| `messages[].objection_handler` | Set to `true` on a message object to mark it as an objection handling message. |
| `force_exact_response` | When true, the agent delivers the message exactly as written. |
| `exclusive_message` | When true, blocks any acknowledgement or other goal content from being appended. |
| `allow_forced` | Controls whether forced (non-generative) text is allowed for this goal. |
| `uninterruptible` | When true, the agent will not be interrupted mid-execution (required for transfers). |
| `acknowledge` | Custom acknowledgement text for the turn following this goal; set `false` to disable. |
| `appended` | When true, this goal's message is combined with the next goal's turn rather than standing alone. |
| `refusal_handling` | Messages used when the user refuses to answer the question. |
| `objection_handling` | Alias for `refusal_handling` used in this script; messages for handling objections. |
| `objection_handling_attempts` | Number of objection handling retries before the goal completes. |
| `objection_conditions` | Condition defining what counts as an undesired/objection value (required for non-approval goals). |

### Fulfillment

| Parameter | Description |
| :---- | :---- |
| `fulfillment` | List of fulfillment action objects to execute at defined timings. |
| `fulfillment[].timing` | When the action fires: `triggered`, `triggered_once`, `performed`, `collected`, `completed`, `agent-said`. |
| `fulfillment[].agent_triggers` | Phrases the agent says that trigger this fulfillment (used with `"agent-said"` timing). |
| `fulfillment[].conditions` | Conditions that must be met for this fulfillment block to execute. |
| `fulfillment[].values` | Key-value pairs to assign to the conversation context. |
| `fulfillment[].trigger` | Goal name or trigger phrase to activate another goal. |
| `fulfillment[].call_result` | Updates the contact's result: `qualified`, `converted`, `disqualified`, `do_not_call`. |
| `fulfillment[].voice_action` | Voice call control: `hang_up` (after speaking) or `hang_up_immediately`. |
| `fulfillment[].override_query` | Overrides the user's query for the current turn's processing. |
| `fulfillment[].override_response` | Forces a specific agent response for the upcoming turn. |
| `fulfillment[].webhook` | Webhook call configuration object. |
| `fulfillment[].webhook.url` | Target endpoint URL (supports placeholders). |
| `fulfillment[].webhook.method` | HTTP method (default: `POST`). |
| `fulfillment[].webhook.body` | Custom request body (supports placeholders). |
| `fulfillment[].webhook.headers` | HTTP headers to include in the request. |
| `fulfillment[].webhook.timeout` | Timeout in seconds for the webhook call. |
| `fulfillment[].webhook.mapping` | Maps webhook response fields to conversation context values. |
| `fulfillment[].webhook.body_mapping` | Maps specific body fields from context values. |
| `fulfillment[].text_message` | SMS message configuration object. |
| `fulfillment[].text_message.message` | Content of the SMS (supports placeholders). |
| `fulfillment[].text_message.to_number` | Override target phone number for the SMS. |
| `fulfillment[].text_message.phone_number_field` | Context field to use as the SMS target number (default: `phone`). |
| `fulfillment[].hubspot` | HubSpot CRM update configuration. |
| `fulfillment[].forward_call` | Immediately forwards the call to a target number (no AI control after). |
| `fulfillment[].forward_call.phone_number` | Target phone number for call forwarding. |
| `fulfillment[].forward_call.pre_merge_message` | Message spoken before forwarding. |
| `fulfillment[].forward_call.from_number` | Caller ID to use for the forwarded call. |
| `fulfillment[].new_call` | Initiates a parallel child call for a warm/merge transfer. |
| `fulfillment[].new_call.agent` | The AI agent to use for the child call (default: `"default-callcenter"`). |
| `fulfillment[].new_call.phone_number` | Phone number to dial for the transfer. |
| `fulfillment[].new_call.from_number` | Caller ID for the child call (set to `{phone}` for user's number as caller ID). |
| `fulfillment[].new_call.pre_merge_message` | Message spoken when the transfer is connected. |
| `fulfillment[].new_call.parent_fail_message` | Message spoken to the user if the transfer fails. |
| `fulfillment[].new_call.contact_name` | Display name for the transfer target. |
| `fulfillment[].new_call.check_dnc_registry` | Whether to check the Do Not Call registry before dialing. |
| `fulfillment[].new_call.metadata` | Key-value data passed to the child call's context. |
| `fulfillment[].new_call.idle_messages` | Messages the agent says to the user while waiting for the transfer to connect. |
| `fulfillment[].new_call.idle_messages[].text` | Text of the idle message. |
| `fulfillment[].new_call.idle_messages[].timeout` | Seconds of silence before this idle message is triggered. |
| `fulfillment[].new_call.stop_recording_on_merge` | When true, stops recording after the transfer merges. |
| `fulfillment[].note` | Internal documentation note; has no effect on execution. |
