Title: Style's mother is constraint
Date: 08/09/26

# The rendering pipeline nobody asked for.

*except I did*

I said in the last post I had a personal ruleset for how this game looks, feels, and behaves, and that I wasn't going to bore you with it. Well... I kinda lied.

Not the whole thing, LECP stays on the lappy where it belongs (for now), but the *rendering* half of the ruleset is done enough to talk about without embarrassing myself, so here we are.

## The one rule that explains all the other rules
*Style is a function of constraint, not a decoration you slap on afterward*

Every single thing below exists because it does one of three jobs: it saves runtime cost, it saves my future self development effort, or it makes a low-spec baseline look like a choice instead of a compromise. Nothing here is in the doc because it's fancy.

## Modeling: build it nice, ship it lean
*the high-poly version never sees the light of day*

Hero assets get a high-detail pass first, purely to bake surface detail down into textures. That version never ships, it's a means to an end. Everything that actually ends up in the game gets retopologized down to a low-poly mesh sized for the target platform, and detail lives in the textures from that point on, not the geometry. Silhouette and big readable shapes win over fine surface detail, every time.

Multiple LODs per object are non-negotiable, and they crossfade, they must NEVER pop, because a visible LOD pop breaks immersion horribly.
And poly counts themselves are a budget I profile per the minimum hardware requirements set.

## Texturing: PBR, but with restraint
*base color, normal, roughness, metallic, AO. that's it. that's the list.*

Materials are built from a small, physically-grounded set of maps, packed into as few channels as I can get away with. Resolution scales with how close and how central something is to the camera, hero assets get the budget, background stuff leans on tiling and trim textures instead of unique art per object.

The important part: the material response has to be correct and continuous *everywhere*. Not by discretizing how light actually behaves, that's too much realism and cost for what I'm going for, so for this, texture authorship and color grading will do nicely.

## Rendering: Stylized, but not cel shading stylized.
*I hate realism in games. All must stylize...*

This is the rule doing most of the heavy lifting: lighting response stays continuous across every surface, full stop. No toon banding anywhere.

Where outlines show up at all, and they show up rarely, they exist to help silhouette read at range, not to announce "hey, this is a comic book now." A Fresnel/rim-light-driven edge is the preferred approach, it should look like light catching a surface, not ink drawn around one. If a proper geometric outline is used for reliability at distance, it stays thin and its opacity/color modulates with the underlying surface tone instead of being a flat black line. And it's reserved for characters and things you can actually interact with, environment geometry earns its readability from material contrast and lighting, not a line traced around it.

Post-processing, color grading, and that whole "cinematic" tone, is a required pipeline step, not an optional polish pass tacked on at the end. It's the cheapest, highest-impact lever for unifying the whole look.

One small detail I'm weirdly fond of: the camera movement itself never produces motion blur, but objects moving through the world do, this makes motion blur feel nice instead of dizzying, but that's just how I see it.

## Lighting: Baked with optional RT.
*Ray tracing? whaaaat?*

The baseline lighting solution runs with zero dependency on hardware ray tracing, period. Baked lightmaps and light probes are the primary system, not the fallback. If the game only looks correct with ray tracing switched on, the minimum-spec promise I keep talking about is already broken, so that's simply not an option on the table, however, their should exist some kind of RT, since it's alredy mature in today's age and a good chunk of hardware supports it.

Where hardware-accelerated ray tracing *is* present on a player's machine, it's an optional, additive layer for specific effects, reflections, translucent shadows, never base scene lighting. The game has to look complete with that layer entirely absent, because for a meaningful chunk of players, it will be.

Static geometry gets baked direct and indirect lighting. Anything that moves gets lit from a sparse grid of precomputed probes instead of being lit from scratch in real time. Dynamic lights the player actually triggers, a flashlight, a muzzle flash, an explosion, get layered on top as true real-time lights, and that's cheap precisely because there's never many of them on screen at once.

## The Oren-Nayar detour
*didn't you just say you hate realism?*

Alright, hear me out, Lambertian shading is the standard diffuse model, but it's wrong in a specific, boring way for rough matte surfaces, concrete, plaster, cloth, that kind of thing. Oren-Nayar accounts for microfacet roughness in a way Lambertian just ignores, and it shows up as noticeably more correct light falloff, especially near grazing angles. With no cel banding around to hide behind (see two sections up), getting rough materials to actually read correctly matters more than it would in a stylized-shading pipeline where nobody's looking that closely anyway.

It's also too expensive to run on every pixel in real time, so it doesn't. It gets baked into lightmaps wherever lighting is already being baked, which is free at runtime since the cost was already paid once, offline. A real-time approximated version is reserved for hero materials in close-up shots where the surface is rough enough for the difference to actually matter. Everywhere else, standard diffuse is cheaper and good enough, and if even reserving Oren-Nayar for hero materials starts eating into budget somewhere, it gets dropped. It's a tool, not a commandment.

Think of it as those close-up shots in Spongebob, they show up now and then but not all the time.

## Why I'm telling you this now

Same reason as the LECP post: I had a chunk of the ruleset that could stand on its own without needing the whole game finished first, so I wrote it down while it's fresh instead of sitting on it for another year.

The next question is whether it survives contact with an actual scene instead of a rulebook. That's a different post, for a different day.
