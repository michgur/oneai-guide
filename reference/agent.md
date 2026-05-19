# InternalAccAgentSettings Parameters

### Agent Structure

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `display_name` | `null` | Human-readable name for the agent shown in the admin UI. Must contain only letters, digits, underscores, spaces, and hyphens. |
| `public_name` | `null` | Public-facing name of the agent. |
| `parent_agent_id` | `"oneai-base-agent"` | ID of the parent agent whose knowledge base and config this agent inherits from. |
| `director_config_id` | `null` | ID of the director configuration to activate for this agent. |
| `identity` | `""` | Agent identity/persona description prepended to the system message. |
| `fulfillment_tasks` | `[]` | List of fulfillment task definitions executed at the end of a conversation. |
| `cross_call_metadata_retention` | `null` | List of metadata keys whose values are preserved across separate calls. |
| `debug_output` | `false` | Includes internal debug info in the agent's responses (for development). |
| `dont_call_numbers_enabled` | `true` | Allow agent to mark contact as do-not-call based on the conversation. |

### Response Handling

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `forced_responses_threshold` | `null` | Similarity score above which a custom response is forced (bypasses the LLM). |
| `priority_responses_threshold` | `null` | Similarity score above which a custom response is prioritized over the LLM answer. |
| `not_found_detection_prompt` | `", your best response is \"{pattern} the documents do not cover this information\" only, in the beginning of your response"` | Prompt suffix appended when no matching knowledge items are found. |
| `not_found_detection_pattern` | `"UNKNOWN:"` | String pattern that signals a "not found" response from the LLM. |
| `block_not_found_response` | `false` | When `true`, suppresses the not-found response from being returned to the user. |
| `not_found_responses` | `null` | List of `UXOptions` objects presented when no answer is found. |


### Search & Retrieval

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `disable_search` | `false` | Skips knowledge-base search entirely; the LLM answers from context only. |
| `disable_question_detection` | `false` | Disables automatic question/intent detection on incoming messages. |
| `threshold` | `SEARCH_THRESHOLD` | Minimum similarity score for a knowledge item to be included in search results. |
| `max_items` | `6` | Maximum number of knowledge items returned per search. |
| `max_context_tokens` | `3000` | Maximum token budget for knowledge context passed to the LLM. |
| `max_generated_tokens` | `500` | Maximum tokens the LLM may generate in a single response. |
| `max_items_for_rerank` | `50` | Number of candidate items retrieved before re-ranking narrows them down. |
| `relevance_threshold` | `0.0001` | Minimum cosine-similarity score for suggested-question matching. |
| `max_question_items` | `1000` | Maximum number of question items fetched when building suggestions. |
| `enable_link_opening` | `false` | Allows the agent to open/follow URLs found in knowledge items. |
| `rerank` | `false` | Enables a secondary re-ranking pass over the top-`max_items_for_rerank` search results. |
| `resegment` | `false` | Enables re-segmentation of retrieved knowledge items before context building. |
| `use_clustering` | `false` | Groups similar knowledge items into clusters before selecting context. |
| `url_path_schemas` | `null` | URL path schemas used to extract metadata from page URLs (e.g. `["/products/<product_id>"]`). |
| `filter` | `null` | Metadata filter dict applied globally to all knowledge retrieval. |
| `crawler` | `{}` | Crawler configuration for auto-ingesting content. |
| `synonyms` | `null` | Dictionary of term → synonym list used to expand search queries (e.g. `{"car": ["vehicle", "automobile"]}`). |

### LLM & Generation

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `llm_model_to_use` | `"nova-pro"` | LLM used for answer generation. Accepts an `AnswerLLM` enum value or a raw model string. |
| `temperature` | `0.1` | Sampling temperature for the LLM; lower values produce more deterministic responses. |
| `system_message_position` | `"first"` | Where the system message is injected in the chat history sent to the LLM (`first`, `last`, `one_before_last`, `skipped`). |
| `compress_chat` | `"search_query"` | Chat-compression strategy applied on regular turns before calling the LLM (`search_query`, `last_message`, `last_two_messages`, `none`). |
| `compress_chat_on_goal` | `"last_two_messages"` | Chat-compression strategy applied when a director goal is active. |
| `query_generation_type` | `"nova"` | Controls how search queries are generated from the user message (`disabled`, `gpt`, `nova`, `query-rewriter`). |
| `rewrite_enabled` | `false` | Enables the query-rewriter pipeline before search. |
| `include_thinking_in_prompt` | `true` | Appends the model's `<thinking>` block to the prompt context for subsequent turns. |
| `redact_thinking_in_prompt` | `""` | Replacement string used when redacting thinking content from the prompt (empty string means full removal). |

### Voice

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `voice_mode` | `null` | Enables voice mode (`enabled`, `disabled`, `preview`). |
| `voice_config` | *(default)* | `VoiceConfig` object with all voice call settings. |
| `voice_config.voice_model` | `null` | TTS voice model name. |
| `voice_config.voice_id` | `null` | TTS voice ID. |
| `voice_config.voice_speed` | `1.0` | Playback speed multiplier for synthesized speech. |
| `voice_config.voice_emotion` | `"happy"` | Emotional tone for the synthesized voice. |
| `voice_config.voice_volume` | `1.0` | Volume level for synthesized speech. |
| `voice_config.voice_text_guidance` | `0.5` | Text guidance strength for the TTS model. |
| `voice_config.voice_style_guidance` | `0.5` | Style guidance strength for the TTS model. |
| `voice_config.voice_quality` | `null` | Quality preset for the TTS model. |
| `voice_config.voice_sentence_pause` | `0.3` | Pause duration (seconds) inserted between sentences. |
| `voice_config.voice_cache_enabled` | `true` | Whether TTS responses are cached. |
| `voice_config.recording_enabled` | `true` | Whether calls are recorded. |
| `voice_config.agent_crosstalk_threshold_sec` | `1.5` | Duration (seconds) of agent audio that is ignored when the user speaks over the agent. |
| `voice_config.buffering_init` | `0.25` | Initial audio buffer level before playback starts. |
| `voice_config.buffering_target` | `0.2` | Target buffer level to maintain during playback. |
| `voice_config.buffering_threshold` | `0` | Minimum buffer level allowed before rebuffering; defaults to 75% of `buffering_target`. |
| `voice_config.min_delay_before_processing_turn` | `0.0` | Minimum delay (seconds) before processing a new user turn. |
| `voice_config.min_delay_before_agent_speech` | `0.65` | Minimum delay (seconds) before the agent starts speaking. |
| `voice_config.normal_delay_before_agent_speech` | `1.1` | Normal delay (seconds) before the agent starts speaking. |
| `voice_config.uninterruptible_delay_before_agent_speech` | `1.4` | Delay (seconds) used when the agent speech should not be interrupted. |
| `voice_config.max_delay_before_agent_speech` | `1.9` | Maximum delay (seconds) before the agent starts speaking. |
| `voice_config.max_turns_for_voicemail_beep_detection` | `3` | Number of turns to listen for a voicemail beep; set to `0` to disable. |
| `voice_config.stutter_detection` | `true` | Whether to detect and suppress agent audio stuttering. |
| `voice_config.stutter_detection_max_interval` | `3.5` | Maximum latency (seconds) within which stutter is detected. |
| `voice_config.stt_model` | `null` | Speech-to-text model name. |
| `voice_config.stt_smart_formatting` | `true` | Whether to apply smart formatting to transcriptions. |
| `voice_config.transcription_engine` | `null` | Transcription engine to use. |
| `voice_config.transcription_options` | `null` | Additional options dict passed to the transcription engine. |
| `voice_config.idle_messages` | `[{text: "hmm hello?", timeout: 6}, ...]` | Messages sent when the call is idle; each item has `text` and `timeout` (seconds). |
| `voice_config.slow_agent_response_filler` | `[{text: "ummmm...", delay: 0.5}, ...]` | Filler phrases played while waiting for the agent to respond; each item has `text` and `delay` (seconds). |
| `voice_config.unintelligible_responses` | `null` | Agent message played when the user's speech is unintelligible. |
| `voice_config.unintelligible_threshold` | `0.1` | Confidence score below which speech is considered unintelligible. |
| `voice_config.max_call_duration` | `300` | Maximum call duration in seconds before the call is terminated. |
| `voice_config.max_silence_duration` | `20` | Maximum silence duration in seconds before the call is terminated. |
| `voice_config.check_dnc_registry` | `true` | Whether to check the Do-Not-Call registry before dialing. |
| `voice_config.merge_incoming_contacts` | `true` | Whether incoming call contacts are merged with existing contact records. |
| `voice_config.vad` | *(default)* | `VADConfig` object for voice activity detection settings. |
| `voice_config.vad.enabled` | `null` | Whether VAD is enabled (`null` uses the engine default). |
| `voice_config.vad.back_window_ms` | `250` | Milliseconds of audio padded before the first detected voice chunk. |
| `voice_config.vad.forward_window_ms` | `650` | Milliseconds of audio padded after the last detected voice chunk. |
| `voice_config.vad.threshold` | `0.10` | VAD confidence threshold; audio below this is treated as silence. |
| `voice_config.vad.logging_interval_sec` | `0.5` | Interval (seconds) between VAD log entries. |
| `voice_config.vad.send_chunk_size` | `160` | Audio chunk size (samples) sent to the VAD model. |

### Twilio

| Parameter | Default | Description |
| :---- | :---- | :---- |
| `twilio_config` | *(default)* | `TwilioConfig` object with Twilio account and messaging settings. |
| `twilio_config.account_sid` | `null` | Twilio account SID used to authenticate API requests. |
| `twilio_config.auth_token` | `null` | Twilio auth token used to authenticate API requests. |
| `twilio_config.phone_number` | `null` | Twilio phone number (E.164 format) used as the caller/sender ID. |
| `twilio_config.messaging_service_sid` | `null` | Twilio Messaging Service SID used for SMS/MMS sending. |
| `twilio_config.webhook_url` | `null` | URL Twilio calls for incoming call or message events. |
| `twilio_config.status_callback_url` | `null` | URL Twilio calls with call/message status updates. |
| `twilio_config.fallback_url` | `null` | Fallback URL Twilio calls if the primary webhook is unreachable. |
| `twilio_config.record_calls` | `false` | Whether outbound and inbound calls are recorded via Twilio. |
| `twilio_config.transcribe_recordings` | `false` | Whether Twilio transcribes call recordings automatically. |
| `twilio_config.max_retry_attempts` | `3` | Number of times Twilio retries a failed webhook request. |
