# Simon — Discord AI with three original knowledge files

Simon is a Cat Goes Fishing modding assistant. It uses all three uploaded files as read-only sources, and retrieves relevant passages per question instead of sending ~1 MB to the model.

## Setup
1. Create a Discord bot and invite it with bot + applications.commands scopes.
2. Install Python 3.10+ and run `pip install -r requirements.txt`.
3. Copy `.env.example` to `.env`; set `DISCORD_TOKEN`, `HF_TOKEN`, and a supported `HF_MODEL` (verify the model is available for your account).
4. Run `python main.py` on a host that supports a persistent Python process.
5. Use `/ask`, `/ask browse:true`, or `/readurl`.

## Limitations
- Hugging Face free inference quotas and hosted model support vary. Free 24/7 hosting is not guaranteed.
- Memory is process-local and resets when the bot restarts.
- Search uses lexical matching; obscure synonyms may be missed.
- The site fetcher rejects non-HTTPS and non-public resolved IPs, but DNS rebinding risk remains; use a trusted-site allowlist for hostile environments.
- Uploaded website snapshots are historical, not guaranteed live.
- Never commit `.env` or tokens to a public repository.

## Mention-only plus occasional conversation
- Simon answers human `@Simon` mentions in servers, without requiring `/ask`.
- To enable occasional unprompted replies, set `SPONTANEOUS_CHANNEL_IDS` to comma-separated Discord text channel IDs. These features are **off by default**.
- In those channels Simon can occasionally respond to other bots, including when mentioned, with stricter bot-to-bot cooldowns and limits to prevent loops.
- Edit `SPONTANEOUS_CHANCE`, `BOT_REPLY_CHANCE`, and cooldowns in `.env` to tune frequency.
- Enable **Message Content Intent** in the Discord Developer Portal under Bot > Privileged Gateway Intents. The bot also needs View Channel, Read Message History, and Send Messages permissions.
- AI requests consume free-tier inference quota. Low reply probabilities and channel limits help control usage.

## Conversation awareness update
Simon now scores incoming messages locally for Cat Goes Fishing, GML, sprites, modding, Litterbox, and help requests. Only sufficiently relevant messages in explicitly enabled channels can trigger unsolicited replies. Relevance affects reply probability; mentions still work regardless of topic. Bot messages use the same topic filter plus their own cooldown. This avoids paying inference costs for irrelevant chatter. Tune `AWARENESS_THRESHOLD` and `AWARENESS_CHANCE` in `.env`. This is keyword-based awareness, not full semantic understanding.

## High-awareness preset
Set SPONTANEOUS_CHANNEL_IDS. High awareness uses 85% base spontaneous chance, 90-second cooldown, 40% bot reply chance and 180-second bot cooldown. Actual chance is relevance-adjusted. Free inference quota can be exhausted quickly.
