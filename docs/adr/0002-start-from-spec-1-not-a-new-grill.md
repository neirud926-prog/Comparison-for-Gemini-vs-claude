# Challenger Build starts from Spec #1, not a new grilling session

The Reference Build began with a grilling session that produced Spec #1. Repeating that grill with Gemini would not be a fair test, because the person answering already knows every answer. So the Challenger Build starts from Spec #1, copied word for word, and the comparison covers to-tickets onward. The skills are copied into `.agents/skills` unchanged, and any Claude-specific wording that trips Gemini up is logged as a Skill Misfire, not rewritten.
