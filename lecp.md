Title: LECP CSS: An abomination that thinks its the actual LECP.
Date: 07/08/26

# I am LECPing it

If you read the game post a while back, you know I mentioned a name and then immediately refused to explain it. **LECP: Low Effort-Cross Platform.** I said it'd get its own post eventually.

This is that post. Kind of.

## I'm not handing over the ruleset
*if you want the whole thing, reverse-engineer it from the game when it ships, if you actually did that then wow, the motivation you have is... something*

The actual LECP document, the one with every rule about focus behavior, tile importance, world-anchored menus, all of the juice, stays exactly where it is: on my thiccpad lappy, waiting to be used for something... *eventually... hopefully*. That's not me being precious about it, it's that half of it doesn't mean anything outside the context of the thing it was written for. Dumping a design bible onto the internet isn't generosity, it's just... static ig.

What I *am* willing to tell you is the story of how a chunk of it escaped into something you can actually go look at right now.

## Why I needed a shortcut
*applying the full ruleset requires an actual game. I do not have an actual game (yet?)*

Here's the problem with finishing a design philosophy before you've built anything with it: you want to see it work immediately, and "immediately" is not a word that applies to game development. *(How I wish if games could be built on random Tuesday mornings)*

So I went looking for the laziest possible way to watch LECP behave like a real interface instead of a paragraph. Turns out the answer was sitting right there the whole time: the web. HTML and CSS render instantly, no compile step, no asset pipeline, no waiting. If the ruleset was actually good, it should survive getting ported to the dumbest, fastest medium available.

## Enter: Metro nostalgia
*yes, I still install Windows 8.1 for fun. no, I will not be taking questions*

Worth saying out loud since it's all over the resulting thing: I love Metro design. Not "appreciate it academically," I mean I still boot Windows 8.1 on an old 2012 Toshiba lappy that's been demoted to full-time labrat duty, purely to sit in that UI for a while. Flat tiles, nice lil' typography, LECP was always going to inherit that visual language, that part was never in question.

## And Skeleton, which I already loved
*getskeleton.com, RIP to my old blog's CSS*

Separately, I've been a fan of Skeleton CSS for a while, I've used it in a few places, most notably this very blog. smol, no build step, styles raw HTML and gets out of your way. Exactly the kind of boilerplate philosophy I like *~~(and copy)~~*.

But Skeleton doesn't know what a tile is. It doesn't know focus is supposed to be mandatory instead of optional. It has zero opinions about motion, because it has zero opinions about anything beyond a grid and some sane defaults. Which is fine, that's the point of Skeleton, but it meant I was always going to end up writing my own version eventually once I had actual opinions to bake in.

## So: LECP CSS
*the derived version, not the source material*

This is the important part, and I want to be direct about it: **LECP CSS is not LECP.** It's a CSS "framework"? that implements the parts of LECP's ruleset that make sense on the web: content before chrome, focus as a first-class state, motion that has to justify its own existence, tile size meaning actual importance instead of decoration. The parts of LECP that only make sense inside a 3D space with a camera and a controller obviously didn't come along for the ride.

Think of it as LECP with the game-specific load-bearing walls removed and the rest reassembled into something a `<link rel="stylesheet">` tag can carry. Same soul, smaller body *(way smaller)*, different job.

Practically: one CSS file, no build step, no dependencies, themeable through CSS variables. Tiles, nav, buttons, forms, toasts, all built around the same rules: visible focus that isn't just a color change, motion tied to actual state changes instead of running forever in the background, layouts that reflow instead of forking into a separate mobile version. ~~*(this rule mostly exists because I'm lazy, hell, that's kinda the whole point of LECP)*~~

## It's public now

Repo's up: **[LECP-CSS Source Code](https://github.com/IMYdev/LECP-CSS)**

Live preview: **[LECP-CSS Live Preview](https://lecp.imy.com.ly)**

And depending on how motivated I am today, you might already be reading this post on the redesigned version of this exact blog, running on LECP CSS instead of Skeleton. I just published the repo a few hours before writing this, so no promises.

It's fully open, same deal as always: find a bug or an inconvenience, open an issue, send a PR, I won't yell at you. Just don't ask me for the full LECP document. That one's still cooking.