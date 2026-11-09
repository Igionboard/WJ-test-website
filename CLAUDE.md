# CLAUDE.md — Frontend Website Rules

## Always Do First

- Before writing any frontend code, every session: check `brand_assets/` and re-read the Anti-Generic Guardrails below. (There is no frontend-design skill on this machine.)

## Reference Images

- If a reference image is provided: match layout, spacing, typography, and color exactly. Swap in placeholder content (images via https://placehold.co/, generic copy). Do not improve or add to the design.
- If no reference image: design from scratch with high craft (see guardrails below).
- Screenshot your output, compare against reference, fix mismatches, re-screenshot. Do at least 2 comparison rounds. Stop only when no visible differences remain or user says so.

## Local Server

- Always serve on localhost — never screenshot a file:/// URL.
- Node.js is NOT installed on this machine. The server is Python's built-in `http.server`, configured in `.claude/launch.json` as `strona` (serves the project root at http://localhost:3000).
- Start it with the built-in browser tool `preview_start` (name: `strona`) before taking any screenshots. It reuses the server if already running — never start a second instance.

## Screenshot Workflow

- Take screenshots with the built-in browser pane (`mcp__Claude_Browser__*` tools), not Puppeteer.
- Always screenshot from http://localhost:3000 (`computer` → `screenshot`). Use `zoom` on a region for small details.
- Check mobile with `resize_window` preset `mobile` (375px) and desktop with preset `desktop`; reset to `desktop` when done.
- Use `javascript_tool` with `getComputedStyle` to read exact sizes, spacing and hex colors instead of guessing from the image.
- Fallback when the browser pane is hidden (screenshot times out): headless Edge, saved to the scratchpad, then Read the PNG:
  `& "${env:ProgramFiles(x86)}\Microsoft\Edge\Application\msedge.exe" --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=4000 --window-size=1440,900 "--screenshot=<scratchpad>\shot.png" http://localhost:3000`
  Edge can't go narrower than ~500px, so for mobile load the page in a 390px-wide iframe on a temporary test page (delete it afterwards).
- If a reference image is provided, it lives in the project folder — read it with the Read tool and compare side by side with the screenshot.
- When comparing, be specific: "heading is 32px but reference shows ~24px", "card gap is 16px but should be 24px"
- Check: spacing/padding, font size/weight/line-height, colors (exact hex), alignment, border-radius, shadows, image sizing

## Output Defaults

- Single index.html file, all styles inline, unless user says otherwise
- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Placeholder images: https://placehold.co/WIDTHxHEIGHT
- Mobile-first responsive

## Brand Assets

- Always check the brand_assets/ folder before designing. It may contain logos, color guides, style guides, or images.
- If assets exist there, use them. Do not use placeholders where real assets are available.
- If a logo is present, use it. If a color palette is defined, use those exact values — do not invent brand colors.

## Anti-Generic Guardrails

- Colors: Never use default Tailwind palette (indigo-500, blue-600, etc.). Pick a custom brand color and derive from it.
- Shadows: Never use flat shadow-md. Use layered, color-tinted shadows with low opacity.
- Typography: Never use the same font for headings and body. Pair a display/serif with a clean sans. Apply tight tracking (-0.03em) on large headings, generous line-height (1.7) on body.
- Gradients: Layer multiple radial gradients. Add grain/texture via SVG noise filter for depth.
- Animations: Only animate transform and opacity. Never transition-all. Use spring-style easing.
- Interactive states: Every clickable element needs hover, focus-visible, and active states. No exceptions.
- Images: Add a gradient overlay (bg-gradient-to-t from-black/60) and a color treatment layer with mix-blend-multiply.
- Spacing: Use intentional, consistent spacing tokens — not random Tailwind steps.
- Depth: Surfaces should have a layering system (base → elevated → floating), not all sit at the same z-plane.

## Hard Rules

- Do not add sections, features, or content not in the reference
- Do not "improve" a reference design — match it
- Do not stop after one screenshot pass
- Do not use transition-all
- Do not use default Tailwind blue/indigo as primary color
