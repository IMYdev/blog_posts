Title: LECP: one logic, all input
Date: 20/10/26

# LECP UX

Most interfaces are designed for one input method and then patched for the rest. A layout gets built around a touchscreen, and later someone bolts on a focus ring for keyboards. It works, but it never feels considered. I wanted to see what happens if you design the other way around.

**LECP** (Low Effort, Cross Platform) is a design philosophy built on one idea: define the interaction logic once, for no input method in particular, and let each input express it in its own way. The "low effort" is **the** goal, not an excuse (~~trust me~~.) I think a layout shouldn't need redesigning every time a new screen size or input method shows up.

## The problems LECP hunts

- **Redesign per platform.** Teams end up maintaining a desktop layout, a mobile layout, and maybe a TV layout, and the three drift apart over time.
- **Assumed affordances.** Hover states, right-click menus and pixel-precise pointing quietly become load-bearing. Take the mouse away and parts of the interface are simply unreachable.
- **Ambient clutter.** Persistent status bars, badges and toolbars pile up because each one solved a small problem on its own. Nobody ever designs the frame as a whole.

## The rules, in short

- **Everything is a tile.** Every interactive element is focusable and navigable directionally, and nothing assumes a cursor exists.
- **Size is meaning.** The most important option gets the biggest tile.
- **Navigation is predictable.** LECP forbids not having the visual heirarchy shown in an obvious way - read more in next section.
- **Layouts reflow, not rebuild** A phone and a wide desktop window run the same logic.
- **Silence is a bug.** Every focus change and activation gets a visible, consistent response.
- **No persistent status layer.** Status information is either checked deliberately in one consolidated place or surfaced briefly and dismissed depending on its nature.

## What testing taught me

One rule didn't survive contact with a controller. "Size is meaning" and "Navigation is predictable" sound like the same rule, but they aren't. When the nearest-neighbour logic was influenced by tile size, pressing right on a D-pad could land on the biggest tile nearby instead of the one actually to the right. Forcing it to go right made the size rule look meaningless.

The fix was to split them into two layers. Size decides how a tile looks and how prominent it is, and navigation is resolved purely by grid adjacency. The two never touch. I'd have missed it without prototyping on real input.

There is never one tile with two tiles on its left stacked on each other, because then which one would we go to? You see it now?

## The vision

I'd like LECP to stretch beyond menus: window managers that treat windows as tiles whose size follows focus, shells where a clipboard and an action history come from one shared event log, and consoles where the interface works the same on a gamepad as on a keyboard. It's a logical structure rather than an implementation, so it should hold up in any toolkit.

An example CSS implementation is already public if you want to see how some of this looks in practice:

- [Source code](https://github.com/IMYdev/LECP-CSS)
- [Live preview](https://lecp.imy.com.ly)
- [Console shell concept](https://lecp-console-shell.vercel.app/)
- This blog itself.

It's early, opinionated, and very much still cooking. Feedback and bug reports are welcome.