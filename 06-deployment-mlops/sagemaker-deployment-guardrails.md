# SageMaker Deployment Guardrails & Shadow Tests

> Safeguards for rolling out new models without breaking production.

---

## Deployment Guardrails

**What it solves:** You don't just deploy a new model and hope for the best. You control how traffic shifts to the new model and automatically roll back if things go wrong.

**Works with:** Real-time inference endpoints AND asynchronous inference endpoints.

> **Async endpoints** = you send a request, it says "got it, I'll give you the answer when ready" — no waiting for immediate response.

---

### Blue/Green Deployment Model

- **Blue fleet** = your current (previous) model
- **Green fleet** = your new model being deployed

Traffic shifts from Blue → Green in a controlled way. Blue fleet is only terminated after Green proves stable.

---

### Three Traffic Shifting Modes

| Mode | How It Works | Best For |
|------|-------------|----------|
| **All at Once** | Shift 100% traffic to Green immediately. Monitor for a bake period. If OK → terminate Blue. | High confidence in new model, want fast cutover |
| **Canary** | Shift a small % of traffic to Green. Monitor. If OK → shift everything, terminate Blue. | Testing with minimal risk — catch problems early with small exposure |
| **Linear** | Shift traffic in linearly spaced steps (as many steps as you want). Monitor at each step. | Worried about scaling / new model's ability to handle increasing load. Controlled ramp-up. |

---

### Automatic Rollbacks

If things go wrong at any stage → automatically rolls back to the Blue fleet (previous model).

No manual intervention needed. The old model that was working takes back all traffic.

---

## Shadow Tests

**What it solves:** Compare a new model's performance against production *before* promoting it.

**How it works:**
1. A **shadow variant** receives a percentage of production traffic
2. You monitor its performance in the **SageMaker console**
3. You manually decide when (or if) to promote it to production

**Key difference from Deployment Guardrails:**
- Guardrails = automated traffic shifting + automatic rollback
- Shadow Tests = manual monitoring + manual promotion decision

---

## Quick Comparison

| Feature | Deployment Guardrails | Shadow Tests |
|---------|----------------------|--------------|
| Traffic shifting | Automated (3 modes) | Manual % allocation |
| Rollback | Automatic | Manual decision |
| Monitoring | Automated checks | You watch the console |
| Decision to promote | Automatic (if metrics pass) | You decide |
| Use case | Production deployment with safety | Pre-production comparison |

---

## Exam Tips

- Deployment Guardrails = **automated** safety during deployment (blue/green)
- Three modes: All at Once, Canary, Linear — know when to use each
- Shadow Tests = **manual** comparison before promoting
- Both work with real-time endpoints; Guardrails also works with async
- If question mentions "automatic rollback" → Deployment Guardrails
- If question mentions "compare shadow variant" or "monitor in console" → Shadow Tests
- Linear mode = best when concerned about **scaling/load handling**
- Canary mode = best when concerned about **model quality/accuracy** with minimal risk
