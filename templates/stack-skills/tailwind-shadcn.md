# Stack skill: Tailwind / shadcn-ui

Injected for `tailwind` or `shadcn` tasks.

- Use design tokens from the project's Tailwind config; no arbitrary values (`[13px]`) when a token exists.
- Check `memory/global/design-system.md` for the component inventory before building anything — extend existing shadcn components, don't fork them.
- New shadcn components go through the project's generator/CLI convention; customize via the component file, not call-site class soup.
- Class order and merging: use the project's `cn`/`clsx` helper; conditional variants via cva when a component has 2+ visual variants.
- Dark mode: every color decision must work in both modes if the project supports it.
- Responsive: mobile-first breakpoints; test the smallest supported width.
- A11y: shadcn primitives keep their aria wiring — never strip roles/labels when customizing.
