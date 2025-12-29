Let me break this down by **current state** vs **planned features**:

---

## Current State (What You Have Now)

### Resource Requirements
| Component | CPU | RAM | Notes |
|-----------|-----|-----|-------|
| FastAPI + Uvicorn | Low | ~100MB | Async, handles multiple connections |
| WebRTC (aiortc) | Medium | ~50MB per connection | Audio processing |
| LLM Pipeline | Minimal | ~50MB | Just API calls, no local models |
| VAD (Silero) | Low | ~100MB | Small ONNX model |
| TTS/STT | Minimal | ~20MB | External APIs (ElevenLabs, Deepgram) |

**Total for ~5-10 concurrent users**: 1 vCPU, 1GB RAM

### API Costs (the real expense)

| Service | Pricing | Estimate per hour of voice |
|---------|---------|---------------------------|
| **Deepgram** (STT) | $0.0043/min (Nova-2) | ~$0.26/hour |
| **OpenAI** (LLM) | ~$0.15/1M input, $0.60/1M output (gpt-4o-mini) | ~$0.02-0.05/hour |
| **ElevenLabs** (TTS) | $0.30/1000 chars (Creator plan) | ~$0.50-1.00/hour |

**Per hour of active voice conversation: ~$0.80-1.30**

### Monthly Cost Estimate (Current)

| Usage Level | Infra | API Costs | Total |
|-------------|-------|-----------|-------|
| **Light** (1-2 hrs/day) | $5 | ~$30-40 | **~$40/month** |
| **Medium** (5 hrs/day) | $5 | ~$120-180 | **~$150/month** |
| **Heavy** (10+ hrs/day) | $10 | ~$250-400 | **~$350/month** |

**The bottleneck is API costs, not infra.**

---

## Planned Features - Cost Impact

### P1: Core Improvements
| Feature | Infra Impact | API Impact |
|---------|--------------|------------|
| Interruption handling | None | Slight reduction (less wasted TTS) |
| Sentence streaming | None | None (same tokens) |
| Unified sessions | +50MB RAM for shared state | None |
| Reconnection | None | None |

**Net: Negligible**

---

### P2: Canvas Evolution (tldraw)
| Feature | Infra Impact | API Impact |
|---------|--------------|------------|
| tldraw integration | +100-200MB RAM (heavier frontend) | None |
| Custom widgets | Client-side only | None |
| Canvas persistence | +DB storage (SQLite OK, ~1MB/session) | None |

**Net: ~$0-2/month more** (just storage)

---

### P3: Platform Features - HERE'S WHERE COSTS JUMP

| Feature | Infra Impact | API Impact |
|---------|--------------|------------|
| **Web Browsing Agent** | +1-2GB RAM (Playwright/Puppeteer) | + Serper/Google API ($50/month for 5000 searches) |
| **Multimodal (Vision)** | None | + $0.01-0.03 per image (GPT-4V) |
| **Code Execution** | Separate sandbox needed | E2B: $0.01/execution or Modal: pay-per-compute |
| **Document Ingestion** | +500MB RAM (embeddings) | + OpenAI embeddings (~$0.0001/page) |

**Web browsing alone could add $50-100/month if used heavily.**

---

### P4: Multiplayer

| Feature | Infra Impact | API Impact |
|---------|--------------|------------|
| Shared rooms | +RAM per room (~100MB each) | Same (shared LLM calls) |
| Recording | +Storage (audio ~10MB/hour) | None |
| Streaming | +Bandwidth | None |

**For 5-10 concurrent rooms: Need 2GB+ RAM VPS ($10-15/month)**

---

### P5: Infrastructure

| Feature | Cost |
|---------|------|
| Redis (managed) | $5-15/month (or free tier on Upstash) |
| Auth (Clerk/Auth0) | Free up to 10k users |
| Monitoring (Sentry) | Free tier usually enough |

---

## Projected Costs by Stage

### Stage 1: Current + P1 fixes
```
Hetzner CX11 (1vCPU, 2GB): €4.5/month
Domain + Cloudflare: Free
API costs (light use): ~$40/month
────────────────────────────────
Total: ~$45-50/month
```

### Stage 2: + Canvas (P2) + JS Frontend
```
Hetzner CX21 (2vCPU, 4GB): €8/month
Static hosting: Free (Vercel)
API costs: ~$50/month
────────────────────────────────
Total: ~$60/month
```

### Stage 3: + Web Browsing + Multimodal (P3)
```
Hetzner CX31 (2vCPU, 8GB): €14/month
Search API: ~$50/month
Additional API (vision): ~$20/month
────────────────────────────────
Total: ~$150-200/month
```

### Stage 4: + Multiplayer (P4)
```
Hetzner CX41 (4vCPU, 16GB): €28/month
Redis (Upstash): Free tier or $10
Storage (recordings): ~$5/month
API costs scale with users: $200-500/month
────────────────────────────────
Total: ~$300-600/month
```

---

## Cost Optimization Tips

1. **STT**: Deepgram is already cheap. No real alternative.

2. **LLM**: You're on gpt-4o-mini which is cost-effective. For canvas-heavy use, could drop to gpt-3.5-turbo for simple widget generation.

3. **TTS**: ElevenLabs is expensive. Alternatives:
   - **OpenAI TTS**: $0.015/1000 chars (cheaper, less quality)
   - **Cartesia**: Faster, competitive pricing
   - **Deepgram Aura**: $0.0015/1000 chars (10x cheaper than ElevenLabs)

4. **Caching**: Cache LLM responses for common queries. Cache TTS for repeated phrases ("Let me think...", "Here's what I found...").

5. **Self-hosted models** (future, if scale demands):
   - Whisper for STT (free, needs GPU)
   - Llama 3 for LLM (free, needs GPU)
   - Coqui/XTTS for TTS (free, needs GPU)
   
   A $50/month GPU VPS could replace $200+/month in API costs at scale.

---

## TL;DR

| Stage | Monthly Cost |
|-------|--------------|
| **Now (test)** | ~$45-50 |
| **+ Canvas/Frontend** | ~$60 |
| **+ Web/Multimodal** | ~$150-200 |
| **+ Multiplayer** | ~$300-600 |

**API costs dominate. Infra is cheap. ElevenLabs TTS is your biggest line item - consider Deepgram Aura if quality is acceptable.**