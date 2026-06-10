# Test Prompts for generating-ai-ideas

This file contains positive and negative trigger examples used by the `improving-skills` autoresearch loop to measure trigger accuracy.

## Positive Prompts (should trigger the skill)

These are requests where the user clearly wants help discovering, validating, or refining AI-related business or product ideas.

1. "Give me AI business ideas for solo lawyers who are overwhelmed with document review."
2. "What are some good AI product opportunities in the home energy space for renters?"
3. "I want to build an AI tool for independent piano teachers to give feedback on student recordings."
4. "Help me find validated AI SaaS ideas for small e-commerce brands in the supplement space."
5. "Looking for AI agent ideas that help creators with revenue attribution across platforms."
6. "Any interesting AI opportunities for private, local second brain tools for knowledge workers?"
7. "What AI products could help home care agencies with scheduling and family communication?"
8. "Brainstorm AI tools for fitness YouTubers that preserve their specific coaching style when making shorts."
9. "I need AI business ideas for professional services firms that reduce client intake friction."
10. "Are there any strong AI opportunities in post-purchase support for DTC brands?"

## Negative Prompts (should NOT trigger the skill)

These are requests that are off-topic, too broad, or ask for something the skill is not designed for.

1. "Write a Python script to scrape LinkedIn."
2. "Help me fix this bug in my React app."
3. "What are some good business ideas for physical products like kitchen gadgets?"
4. "Explain how neural networks work."
5. "Give me a marketing plan for my existing SaaS product."
6. "Write a sales email for my AI tool."
7. "What are the best AI models to use right now?"
8. "Help me design a database schema for a user management system."
9. "Give me ideas for a new social media app."
10. "How do I raise venture capital for my startup?"

## Notes for Evaluation

- The skill should strongly trigger on clear requests for **AI-related product or business opportunity discovery + validation**.
- It should **not** trigger on general coding help, non-AI product ideas, marketing copy, or technical explanations.
- Borderline cases (e.g. "AI ideas for physical products") should be evaluated based on whether the user is asking for AI-powered solutions vs pure hardware.