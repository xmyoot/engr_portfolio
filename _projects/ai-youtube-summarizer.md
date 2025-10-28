---
title: AI Youtube Video Summarizer
date: 2025-03-02
tags: [web, ai, tools, react, flask, openai]
thumbnail: /assets/images/ai_youtube_summarizer.png
excerpt: "Tool that summarizes YouTube videos using AI — transcript extraction, summarization and short highlights for faster consumption."
---

An AI-driven utility that extracts a video's transcript and produces concise, readable summaries and highlights using OpenAI models. Built as a small web app for rapidly understanding long-form videos.

# Key features
- Transcript extraction from YouTube videos (via public captions or speech-to-text pipeline).
- Multi-level summaries: short TL;DR, medium-length synopsis, and sectioned highlights.
- Optionally generates timestamps/chapters and suggested highlights for sharing.

# Tech stack
- Front end: React
- Back end: Flask (API for processing and talking to OpenAI)
- AI: OpenAI API for summarization and text extraction

# Usage
- See repository for setup and API key configuration: https://www.github.com/xmyoot/ai-yt-sum
- Typical flow: supply a YouTube URL → service extracts transcript → OpenAI produces summary and highlights.

# Repository / more
- https://www.github.com/xmyoot/ai-yt-sum
