# evaldiff example

**A red/green LLM eval gate in 2 minutes.** Fork this repo and watch CI grade
your model on your test cases.

```
push code → evaldiff runs your cases → green or red checkmark on the PR
```

## What's in here

| file | what it does |
|---|---|
| `evals/cases.json` | 3 tiny test cases for any OpenAI-compatible chat model |
| `.github/workflows/eval.yml` | the whole integration — one action, three inputs |

That's it. No test framework, no harness code. evaldiff hosts the runs,
keeps history, and gates the pipeline on your pass rate.

## Try it

1. **Fork** this repo.
2. In your fork: **Settings → Secrets and variables → Actions** and add:
   - `EVALDIFF_API_KEY` — your evaldiff.io key (free signup, [evaldiff.io](https://evaldiff.io))
   - `MODEL_API_KEY` — your key for the model under test (OpenAI or any
     OpenAI-compatible endpoint; point `model_url` at it in the workflow)
3. Push anything. The **eval** check appears on your commit/PR:
   green when ≥ 80% of cases pass, red when they don't.
4. Break a case on purpose → red. Fix it → green. That's the product.

## The whole integration (`.github/workflows/eval.yml`)

```yaml
- uses: evaldiff/action@v1
  with:
    api_key: ${{ secrets.EVALDIFF_API_KEY }}
    dataset_json: evals/cases.json
    model: gpt-4o-mini
    threshold: 0.8
```

## Add your own cases

`evals/cases.json` is a plain JSON array — one object per case:

```json
{ "input": "What is the capital of France?", "expected": "Paris" }
```

The model's answer is graded against `expected` (exact match, or a judge
model for open-ended questions — see the evaldiff.io docs).

## License

MIT
