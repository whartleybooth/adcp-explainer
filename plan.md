# Plan: AdCP Explained — Single-Page Landing Site

## Overview
Build a single-page website (`index.html`) that explains the Ad Context Protocol (AdCP) to advertising industry professionals, with AI-personalised explanations powered by the Claude API.

## Deliverable
**One file:** `index.html` — self-contained with embedded CSS and JavaScript.

---

## Implementation Steps

### Step 1: Create `index.html` scaffold
- Single HTML file with `<!DOCTYPE html>`, meta viewport for mobile, Inter font from Google Fonts CDN
- Embed all CSS in a `<style>` block
- Embed all JS in a `<script>` block at the bottom

### Step 2: API Key Configuration
- JavaScript constant `const API_KEY = "your-api-key-here"` at the top of the script block
- User pastes their Anthropic API key once before opening the file — no modal or prompt needed

### Step 3: Hero Section
- Full-width section, vertically centered content
- Bold headline: **"AdCP Explained"** (large, white, Inter font)
- Subtitle: *"The open standard for agentic advertising — explained for your role"*
- Accent color (#00cfff) used for key words and decorative elements

### Step 4: Role Selector
- 5 pill/button-style selectors in a horizontal row (wraps on mobile):
  1. Brand / Advertiser
  2. Agency / Media Planner
  3. Publisher
  4. DSP / Ad Tech Platform
  5. Developer / Engineer
- Styled with border in accent color, transparent background
- On hover: subtle glow effect
- On click: fills with accent color, triggers API call

### Step 5: AI Explanation Panel
- Container below role selector, initially hidden
- When a role is selected:
  1. Show a loading spinner (CSS-only spinner, centered, accent color)
  2. Call the Anthropic Messages API (`POST https://api.anthropic.com/v1/messages`)
     - Model: `claude-sonnet-4-20250514`
     - System prompt includes AdCP context
     - User message uses the role-specific prompt template
     - Headers: `x-api-key`, `anthropic-version: 2023-06-01`, `anthropic-dangerous-direct-browser-access: true`, `content-type: application/json`
  3. On response: hide spinner, fade-in the 3-paragraph explanation
  4. Smooth CSS fade-in animation (opacity 0→1, transform translateY)
- Re-selecting a different role replaces the content with a new API call

### Step 6: Static "What is AdCP?" Section
- Below the AI panel
- Heading: "What is AdCP?"
- 3-4 sentence static explainer:
  > AdCP (Ad Context Protocol) is an open standard for agentic advertising, built on top of MCP (Model Context Protocol). It enables AI agents to autonomously discover ad inventory, negotiate deals, and execute media buys across platforms — without human involvement. Governed by AgenticAdvertising.org, AdCP v3.0 beta includes modules for Media Buys, Signals Activation, Creative, and Brand identity. Discovery is powered by `adagents.json` and `brand.json` files that make advertising infrastructure machine-readable.
- Clean card-style container with subtle border

### Step 7: Footer
- Simple centered footer
- Links to [adcontextprotocol.org](https://adcontextprotocol.org) and [agenticadvertising.org](https://agenticadvertising.org)
- Muted text color, small font

### Step 8: CSS Styling & Animations
- **Background:** #0a0a0f (near-black)
- **Accent:** #00cfff (electric blue)
- **Text:** white (#ffffff) for headings, light gray (#b0b0b0) for body
- **Font:** Inter from Google Fonts, fallback to system sans-serif
- **Animations:**
  - `@keyframes fadeInUp` for AI response panel
  - `@keyframes spin` for loading spinner
  - Subtle hover transitions on role buttons (0.2s ease)
- **Layout:** max-width container (~800px), centered, generous padding
- **Mobile responsive:** media queries for <768px (stack buttons vertically, adjust font sizes)
- **Linear/Vercel aesthetic:** minimal, spacious, subtle gradients or glow effects on accent elements

### Step 9: JavaScript Logic
- `selectRole(role)` function:
  1. Highlights selected button, unhighlights others
  2. Shows spinner in explanation panel
  3. Calls `fetchExplanation(role)`
- `fetchExplanation(role)` async function:
  1. Constructs API request with system context and role-specific user prompt
  2. `fetch()` to Anthropic API
  3. Parses response, extracts text content
  4. Renders paragraphs into the explanation panel with fade-in class
  5. Error handling: shows user-friendly error message if API call fails

---

## API Call Details

**Endpoint:** `POST https://api.anthropic.com/v1/messages`

**Headers:**
```
x-api-key: [USER_API_KEY]
anthropic-version: 2023-06-01
anthropic-dangerous-direct-browser-access: true
content-type: application/json
```

**Body:**
```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1024,
  "system": "You are an expert in AdCP (Ad Context Protocol)... [full context]",
  "messages": [
    {
      "role": "user",
      "content": "Explain what AdCP means specifically for a [ROLE] in 3 short paragraphs..."
    }
  ]
}
```

---

## File Structure
```
/index.html    ← single deliverable file
/plan.md       ← this plan
```

---

## Quality Checks
- [ ] Renders correctly on desktop (Chrome, Safari, Firefox)
- [ ] Mobile responsive (viewport < 768px)
- [ ] API key prompt appears when key is missing
- [ ] Loading spinner shows during API call
- [ ] AI response fades in smoothly
- [ ] Role buttons highlight correctly
- [ ] Switching roles triggers new API call
- [ ] Error state handled gracefully
- [ ] Links in footer work
- [ ] No console errors
