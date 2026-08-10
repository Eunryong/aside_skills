---
name: "youtube-summarizer"
description: "Generate a detailed timestamped timeline and section breakdown for a YouTube video using its transcript and metadata."
---

# YouTube Summarizer (Timestamped Breakdown)

Use this skill whenever the user requests a timestamped content breakdown, timeline summary, or detailed breakdown of a YouTube video.

## Workflow

1. **Identify Target Video**
   - Extract the video URL from the request or use the active tab (`page.url()`).

2. **Fetch Metadata and Timestamped Transcript**
   - Execute in REPL using the `youtube` global helper:
     ```js
     const videoUrl = "<TARGET_URL_OR_PAGE_URL>";
     const [metadata, transcript] = await Promise.all([
       youtube.getMetadata(videoUrl),
       youtube.getTranscript(videoUrl, { includeTimestamp: true }).catch(() => null)
     ]);
     ```

3. **Handle Missing Captions**
   - If official transcript/captions are unavailable, state clearly that captions could not be retrieved, and present a structured breakdown based on `metadata.description` and video metadata.

4. **Structure Output by Timestamp**
   Instead of a brief recap or 3-line summary, format the output as a comprehensive, chronological timeline breakdown:

   - **영상 정보**: 제목, 채널명, 총 재생 시간, 조회수
   - **시간대별 세부 내용 정리**:
     - `[00:00 - 03:15]` **주제/섹션 제목**
       - 세부 내용 1
       - 세부 내용 2
     - `[03:15 - 07:40]` **주제/섹션 제목**
       - 세부 내용 1
       - 세부 내용 2
     - (영상 전체 구간에 대해 시간대별로 누락 없이 정리)
   - **핵심 키포인트 요약**: 영상에서 전달하는 가장 중요한 핵심 인사이트
