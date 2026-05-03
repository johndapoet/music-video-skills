# Frontend Animation Asset Sourcing

Use this reference when a music video or Remotion project needs high-quality frontend animation assets, including Lottie, dotLottie, SVG, CSS animation snippets, animated icons, 3D models, HDRIs, textures, or React Three Fiber compatible assets.

## Search Standard

Do not settle for the first search results. Build a shortlist, inspect licenses, and choose assets that fit the song meaning, visual motif, and technical format.

Use this process:

1. Define the needed asset as a visual role, not a vague keyword.
   - Weak: "heart animation"
   - Strong: "fractured glass heart pulsing on downbeats, dark cinematic, transparent background"
2. Search at least 3 relevant sources for important hero assets.
3. Prefer creator portfolios, curated collections, recent uploads, and assets with editable source or clean export formats.
4. Avoid overused first-page assets unless they can be recolored, re-timed, cropped, or combined into a unique motif.
5. Verify license before download and record attribution requirements in project notes.
6. Test the downloaded asset in the target stack before building scenes around it.

## Source Map

| Need | Format | Tech | Start With |
| --- | --- | --- | --- |
| Complex motion, UI motifs, abstract loops | `.json`, `.lottie` | `lottie-react`, `dotlottie-js`, `@remotion/lottie` | [LottieFiles](https://lottiefiles.com/), [LottieFlow](https://lottieflow.com/) |
| Animated icons and small symbolic gestures | SVG, Lottie JSON, GIF, MP4 | Inline SVG, Lottie, Remotion media | [Lordicon](https://lordicon.com/) |
| CSS micro-interactions | CSS, keyframes | CSS modules, styled components, Tailwind-safe static classes | [Animista](https://animista.net/play) |
| Loaders, pulse loops, progress graphics | SVG, CSS, GIF, Lottie | SVG, CSS, `<Img>`, Lottie | [Loading.io](https://loading.io/) |
| Custom animated SVG logos and vector loops | SVG, Lottie JSON | Inline SVG, `<Img>`, Lottie | [SVGator](https://www.svgator.com/) |
| Photoreal 3D lighting, textures, props | HDRI, PBR textures, GLB | Three.js, React Three Fiber | [Poly Haven](https://polyhaven.com/) |
| Unique community 3D models | GLB, glTF, USDZ | Three.js, React Three Fiber `useGLTF()` | [Sketchfab](https://sketchfab.com/features/gltf) |
| Stylized low-poly 3D packs | GLB, glTF, FBX, OBJ | Three.js, React Three Fiber | [Quaternius](https://quaternius.com/) |

## Resource Notes

- **LottieFiles**: Use for lightweight, scalable Lottie or dotLottie motion. Prefer animations with editable colors or clean transparent backgrounds. Check the asset-specific license.
- **Lordicon**: Use for polished animated icons. Good for symbolic moments like hearts, hands, clocks, tears, phones, money, fire, doors, arrows, and warning signs.
- **LottieFlow**: Use for common UI motion patterns such as arrows, toggles, loaders, social icons, and simple interaction loops.
- **Animista**: Use for generated CSS keyframes when a pure CSS micro-interaction is enough. In Remotion, convert timing to frame-driven `interpolate()` when possible.
- **Loading.io**: Use for loaders, rhythmic loops, progress bars, and simple repeated shapes. Prefer SVG or Lottie over GIF when transparency and scaling matter.
- **SVGator**: Use when an SVG logo, lyric symbol, or custom vector needs professional motion without writing heavy JavaScript.
- **Poly Haven**: Use for CC0 HDRIs, textures, and models when realistic lighting or physical materials matter. Excellent for reflective, cinematic, and 3D scene work.
- **Sketchfab**: Use for distinctive GLB or glTF models. Always filter for downloadable assets and verify the exact license. Many free models require attribution.
- **Quaternius**: Use for stylized low-poly packs, game-like worlds, and optimized scenes that need to render smoothly.

## Query Patterns

Search by theme, material, motion, and format:

```text
site:lottiefiles.com/animations broken glass dark loop lottie
site:lordicon.com mirror animated icon json
site:polyhaven.com neon alley wet asphalt HDRI
site:sketchfab.com downloadable glb shattered mirror cc0
site:quaternius.com stylized city low poly gltf
```

For unique finds:

- Search synonyms and metaphors from the confirmed lyrics.
- Search for scene objects, not only emotions: "cracked photo", "empty hallway", "red cloth", "burning letter", "chrome mask".
- Open creator profiles and browse related packs.
- Compare at least 5 candidates for hero assets and 2 candidates for small supporting assets.
- Prefer assets that can be transformed into a motif across verse, chorus, bridge, and final chorus.

## Quality Checklist

Before choosing an asset, verify:

- **License**: CC0 is safest. CC BY requires credit. Avoid non-commercial licenses for commercial or public releases unless the user explicitly approves.
- **Format**: Prefer `.json` or `.lottie` for Lottie, SVG for vector, and `.glb` or `.gltf` for 3D.
- **Performance**: Check file size, polygon count, texture size, and animation duration.
- **Editability**: Prefer assets with separate layers, color controls, transparent backgrounds, or clean material slots.
- **Visual fit**: Match the approved lyric meaning, motifs, palette, and camera language.
- **Uniqueness**: Avoid generic top-result assets unless heavily customized.
- **Attribution**: Save author name, URL, license, and required credit text.

## Remotion Integration

When implementing assets:

- Use `$remotion-best-practices`.
- For Lottie, load `rules/lottie.md`.
- For images and SVG, load `rules/images.md`.
- For videos, load `rules/videos.md`.
- For 3D, load `rules/3d.md`.
- Place downloaded assets in `public/` unless the existing project uses a different asset convention.
- Reference local assets with `staticFile()`.
- Animate with Remotion frame logic such as `useCurrentFrame()`, `interpolate()`, `spring()`, and `Easing`; do not depend on CSS transitions or CSS animations for rendered timing.

## Attribution Log Template

Keep a small project note for every external asset:

```markdown
## Asset Credits

- Asset:
- Source URL:
- Author:
- License:
- Attribution required:
- Used in scenes:
- Local file:
- Notes/customization:
```
