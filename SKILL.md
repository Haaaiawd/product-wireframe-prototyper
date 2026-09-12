---
name: product-wireframe-prototyper
description: Turn an early product idea, feature discussion, PRD, or page list into a self-contained, clickable HTML blueprint with UX annotations, observable user journeys, and optional asset mapping. Use when the user wants a low-fidelity UI/UX sketch, interaction demo, page-flow prototype, mobile or desktop product form, visual-asset placeholders, or a concrete interface to discuss with AI or collaborators. Do not use for polished visual design or production frontend implementation.
---

# Product Wireframe Prototyper

Convert an abstract product discussion into an interface hypothesis that people can open, click, annotate, and revise together. The deliverable is not a gallery of screens. It is a small working model of the product's form, navigation, states, and unresolved decisions.

## Establish the product shape

Infer the user's real alignment problem before drawing. Identify:

- the product's target form: mobile app, desktop application, responsive web product, or a focused component;
- the primary user and the central job they are trying to complete;
- the smallest end-to-end path that must become visible;
- the screens, overlays, states, and transitions needed to express that path; and
- the decisions that remain uncertain and deserve annotations.

Ask a question only when the missing answer would materially change the product form or core flow. Otherwise state a reasonable assumption and build a first version that the user can react to.

Do not turn every mentioned feature into a screen. Preserve the core path, group secondary actions, and mark uncertain branches instead of inventing product decisions silently.

## Separate target device from viewing device

Treat these as independent:

1. **Target device** determines the product layout.
2. **Viewing device** determines how the prototype is presented.

For a mobile product:

- use a narrow app canvas with mobile navigation and touch-sized controls;
- center the canvas when viewed on desktop;
- let it fill the available width and height when viewed on a phone;
- avoid decorative phone hardware unless it helps the discussion.

For a desktop product:

- use a realistic desktop canvas and desktop interaction patterns such as sidebars, toolbars, panes, tables, or inspectors;
- let it use the browser viewport on desktop;
- on a phone, preserve the desktop information architecture through an explicit compact overview or deliberate reflow; never silently remove a critical pane or squeeze the canvas until text becomes illegible;
- show a brief viewing hint only when an interaction truly requires a larger screen.

For a responsive product, demonstrate the meaningful layout change rather than presenting the same canvas at a different scale.

Account for the actual viewing **container**, including a narrow in-chat preview or embedded frame, rather than assuming the browser fills the entire device. The same offline file should remain usable when opened directly or previewed inside a smaller pane; avoid viewport-fixed explanations that float far outside the product canvas or become clipped. An embedding platform may not support HTML preview, so deliver the downloadable HTML regardless.

## Build an interaction model

Deliver one self-contained `.html` file unless the user requests another format. Use inline CSS and JavaScript so the prototype opens locally without setup. Avoid external libraries, network calls, accounts, build steps, and real backend logic unless the user explicitly needs them.

Choose one reusable base according to the **product target**, not the device currently viewing the prototype:

- use [assets/mobile-shell.html](assets/mobile-shell.html) for a mobile product;
- use [assets/desktop-shell.html](assets/desktop-shell.html) for a desktop workspace, dashboard, console, or multi-pane application.

Copy and adapt the selected base; do not modify the asset in place for an individual product. Replace all sample content and remove unused primitives. Never create a desktop prototype by widening the mobile shell. For a responsive product, start from the closer structural model and deliberately implement the meaningful layout changes at both breakpoints.

Treat the shells as infrastructure, not product templates. Preserve routing, observation layers, and validation when useful, but replace their navigation, page composition, and sample controls whenever the product demands another structure.

Keep the **final artifact** self-contained, not the entire authoring process. For longer flows, establish a short screen/transition outline first, then build the core path, add annotations, add optional assets, and validate after each increment. Reuse the shell's generic routing and marker behavior instead of rewriting its engine for each screen. Add a second journey or complex state only if the discussion genuinely needs it; do not try to generate every page, state, and specification in one unverified pass.

Make drawn controls behave according to the product hypothesis:

- primary and secondary navigation must switch to real prototype states;
- back, close, cancel, and overlay-dismiss actions must work;
- tabs, menus, drawers, modals, and expandable controls must expose their relevant state;
- important cards and calls to action must lead somewhere meaningful;
- controls intentionally outside scope may be visibly disabled or marked as placeholders;
- never present a control as active if clicking it does nothing.

Keep interaction local and reversible. Do not simulate authentication, payment, deletion, or external side effects beyond the minimum state change needed to discuss the UX.
If the path promises a saved result or completed state, render that outcome only after the relevant product action. Reaching the result page via general navigation must not fabricate completion.

## Make the user journey observable

Do not confuse a clickable page graph with a user journey. A page graph says where links go; a journey explains who is acting, what they are trying to achieve, what they do at each step, and what changes when they succeed.

Define one primary journey for every prototype. Add secondary or exception journeys only when they materially change the product discussion. Model the journey as transitions, not a list of screens. Each transition should contain:

```js
{ id, from, event, to, result }
```

Add optional state data only when a visible product state must change. Each journey should also contain:

- an actor described by their situation rather than a broad demographic label;
- one concrete goal;
- an entry state;
- a short ordered sequence of transitions; and
- a visible completion outcome.

Expose journeys behind one quiet, clearly labeled **查看路径** control. Opening it reveals a closable, readable summary of the actor, goal, actions, state changes, and outcome. Keep journey steps read-only by default: reviewers experience the path by clicking the product's own controls, not by learning a second navigation system. Product actions may reference transition IDs for validation, but completion counters, checkmarks, an inspection state, and jump-to-step controls are optional only when a specific review requires them. If direct scenario inspection is needed, label it explicitly as a preview and never portray inspecting a state as completing a user action. Present the path vertically on mobile and in a spacious, closable panel on desktop.

Keep three explanation levels distinct:

- the product interface shows what the user can do;
- numbered annotations explain local product decisions; and
- the journey layer explains why the user is here, how the experience unfolds, and what success means.

Do not place a permanent flowchart over the product or require a reviewer to understand "inspection" and "experience" modes. Hide journey detail by default and let the product remain clickable as soon as the panel closes.

## Use annotations as a decision layer

Annotations are a defining feature, not decoration. Place numbered markers beside areas where product reasoning matters. Clicking a marker must open a concise explanation covering one or more of:

- the purpose of the area;
- why it is placed there;
- what action or transition it controls;
- when material, where displayed data comes from and what empty, loading, error, or completed states mean;
- an assumption currently being made; or
- a question that still needs the user's decision.

Show a small set of UX markers by default; they are a distinctive part of the wireframe, not an expert mode. Do not annotate obvious labels or every component. Keep markers attached to their target and prevent them from blocking primary controls. If a marker lives inside a clickable card, handle the marker before the parent card: clicking the marker must only open its note and must not navigate. Keep notes beside their target while visible, reposition or dismiss on scroll/reflow, and close when the user clicks outside or uses an explicit close action.

Provide a page-level explanation behind a small, clearly labeled information control. Keep it hidden by default and closable, including in the desktop shell; a persistent inspector should exist only when it is part of the product being designed. Describe the current screen's role in the flow, not repeat every annotation.

## Map assets without designing them prematurely

Assess where the product will eventually depend on authored visual, sound, typography, or theme assets. Represent those needs in an **Asset Map** layer before producing the assets or applying a polished visual system.

Keep UX and asset annotations separate. Use numbered circles such as `1`, `2`, and `3` for UX decisions. Use typed identifiers for asset needs:

- `A01`, `A02` for image, illustration, texture, animation, or decorative visual assets;
- `♫01`, `♫02` for sound and ambience;
- `T01`, `T02` for typography, type treatment, or theme assets.

Do not label ordinary layout, CSS geometry, standard UI controls, charts, or every icon as art assets. Treat a shared icon library as one system-level dependency when its style requires a decision. Prefer reuse and derived crops over creating separate assets for every placement.

For each asset annotation, record:

- name and type;
- page and placement;
- priority: `P0`, `P1`, or `Post-MVP`;
- intent: what experience or communication job it performs;
- variants that are actually required;
- constraints such as aspect ratio, safe text area, contrast, looping, file weight, licensing, or accessibility; and
- reuse: where the same source can be cropped, adapted, or shared.

Show asset markers only when the Asset Map layer is enabled. Provide a compact **Layers** control that independently toggles UX annotations and asset annotations. Keep user journeys separate from both because they describe sequence rather than a point on the screen. Include the Asset Map only when authored assets materially affect the experience or the user asks for asset planning; otherwise omit the layer or leave it explicitly empty.

Do not create assets while the core flow is still unstable. Use this progression as a decision gate, not a ceremonial waterfall:

1. establish the UX wireframe;
2. stabilize the core user journey enough to design against;
3. mark asset locations and dependencies;
4. specify the assets;
5. establish the visual direction;
6. produce assets and build a mid-fidelity prototype.

Generate a separate `ASSETS.md` only when the user requests an inventory or the prototype is being handed to visual production. Otherwise keep the asset specifications inside the interactive HTML so the first alignment artifact remains self-contained.

## Visual language

Favor a calm black-and-white low-fidelity wireframe:

- clear borders, restrained rounded corners, generous spacing, and readable hierarchy;
- system fonts or safe local font fallbacks;
- simple symbolic icons or text labels whose meaning remains clear;
- minimal shadows used only to separate overlays and floating surfaces;
- no decorative gradients, brand styling, stock imagery, or polished mockup effects unless requested.

Low fidelity does not mean careless geometry. Align elements, preserve spacing rhythm, keep touch targets usable, and prevent overlapping text or controls. The result should feel like a deliberate UX drawing rather than unfinished frontend CSS.

## Represent flows inside the product

Do not add an external row of prototype-only screen buttons above the interface. Let users move through the product by using its own navigation and controls.

When a screen is not naturally reachable but is necessary for discussion, expose it through one of these methods:

- a clearly marked scenario selector inside the page-information drawer;
- an entry state or seeded example that makes the path reachable; or
- a small prototype-only control visually separated from the product canvas.

Prefer overlays for actions that conceptually preserve the current screen. For example, a central create button may reveal a four-option floating menu above the navigation bar instead of replacing the entire page.

## Preserve scope and communicate assumptions

Use realistic placeholder copy so the interface can be judged, but do not invent claims, integrations, metrics, or business rules. Label deferred capabilities honestly. Keep business data and product decisions easy to replace.

If the user supplies an existing prototype, preserve working parts and make the smallest coherent intervention. Do not rewrite the product merely to fit the starter asset.

## Validate before delivery

Open the generated file in an available browser when possible and inspect at least one desktop viewport and one phone viewport. Exercise the central path and every navigation item shown as active. Also verify:

- the initial screen renders without setup;
- there are no console or JavaScript syntax errors;
- every `data-go`, tab, menu, and close target reaches an existing state;
- every visually active control has a behavior; deferred controls are disabled and labeled as out of scope;
- the primary journey names an actor, goal, ordered steps, and outcome;
- journey transitions have unique IDs and valid `from`/`to` states;
    - a path step is read-only unless an explicit scenario-preview feature was requested; inspecting a scenario must never imply task completion;
- UX and asset markers can be toggled independently and remain visually distinct;
- every asset marker resolves to a specification with priority, intent, constraints, and reuse;
- the Asset Map does not misclassify routine UI construction as authored art;
- overlays open, dismiss, and remain within the viewport;
- annotations match visible elements and do not cover essential controls;
- annotations stay attached to their target while content scrolls or reflows;
- fixed navigation does not hide scrollable content;
- text remains readable on the intended target and viewing devices; and
- the final file contains no unexplained sample content from the starter asset.

Retain or adapt the shell's development-time `validateBlueprint()` check. Run it before delivery and treat reported dangling routes, missing note or asset specifications, invalid journey transitions, and active controls without behavior as failures. This check complements browser testing; it does not replace it.

If browser rendering is unavailable, perform structural and script checks and disclose that visual verification was not completed.

## Deliver the prototype

Give the user the HTML file and a compact summary of:

- the product form assumed;
- the main flow made interactive;
- the primary user journey and its success outcome;
- the decisions represented by annotations; and
- the P0 asset dependencies, when an Asset Map is present; and
- any important limitation or unresolved choice.

Stop when the interface is concrete enough to provoke useful product feedback. Further visual polish belongs to a later design or implementation pass.
