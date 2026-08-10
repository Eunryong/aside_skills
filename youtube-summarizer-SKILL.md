---
name: "youtube-summarizer"
description: "Summarize a YouTube video from a URL or current YouTube tab by extracting its transcript and metadata."
---

# YouTube Summarizer

Use this skill whenever the user requests a summary, key takeaways, or quick overview of a YouTube video.

## Workflow

1. **Identify Target Video**
   - Use the URL provided in the request or check the current tab (`page.url()`).

2. **Fetch Metadata and Transcript**
   - Execute in REPL using the `youtube` global helper without navigating the page:
     ```js
     const videoUrl = "<TARGET_URL_OR_PAGE_URL>";
     const [metadata, transcript] = await Promise.all([
       youtube.getMetadata(videoUrl),
       youtube.getTranscript(videoUrl, { includeTimestamp: true }).catch(() => null)
     ]);
     console.log({ metadata, transcriptSnippet: transcript ? transcript.slice(0, 500) : "No transcript" });
     ```

3. **Handle Missing Transcripts**
   - If `transcript` is `null` or unavailable, fallback to using `metadata.description` and mention that official captions were unavailable.

4. **Format Summary**
   Structure the response clearly using concise Korean:
   - **기본 정보**: 제목, 채널명, 영상 길이, 조회수
   - **핵심 요약**: 3~5문장 이내 핵심 요약
   - **주요 내용 (타임스탬프별)**: 타임스탬프와 함께 주요 섹션별 세부 내용
   - **결론 및 시사점**: 영상의 핵심 메시지
