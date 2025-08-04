# OpenAI API Setup for Hoopscope

## Security & Key Handling
- Never expose your OpenAI API key in client-side code.
- Store the API key in a secure environment variable (e.g., `VITE_OPENAI_API_KEY`, `.env.local`).
- Proxy all API calls through a secure backend or cloud function.

## Example: Secure Fetch (Node/Server, NOT client-side)
```js
// .env
OPENAI_API_KEY=sk-...yourkey

// fetchPlan.js (server)
const { Configuration, OpenAIApi } = require('openai');
const configuration = new Configuration({ apiKey: process.env.OPENAI_API_KEY });
const openai = new OpenAIApi(configuration);
app.post('/api/generate', async (req, res) => {
  try {
    const userPrompt = req.body.prompt;
    const gptResponse = await openai.createChatCompletion({
      model: 'gpt-4',
      messages: [{ role: 'user', content: userPrompt }],
      max_tokens: 700
    });
    res.json({ result: gptResponse.data.choices[0].message.content });
  } catch (err) {
    res.status(500).json({ error: 'API error: ' + err.message });
  }
});
```

## Notes
- Frontend should POST to `/api/generate` (or similar), not OpenAI directly!
- See docs/ for serverless/cloud function deployment templates.

---

For React/Vite, use Vite env system (`import.meta.env.VITE_OPENAI_API_KEY`) and an API proxy route.