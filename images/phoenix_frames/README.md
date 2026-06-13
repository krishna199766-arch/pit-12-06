Instructions to convert the SVG frames into an animated GIF or sprite sheet

Prerequisites (choose one):
- ImageMagick (magick/convert) installed
- Inkscape (for SVG -> PNG rasterization) OR librsvg (rsvg-convert)

Recommended workflow (Windows PowerShell):

# 1) Rasterize frames to PNG using Inkscape (or rsvg-convert)
# If you have Inkscape 1.0+ installed:
# Run from repository root (where images/phoenix_frames exists)

for ($i=1; $i -le 8; $i++) {
  $n = $i.ToString("00")
  & inkscape "images/phoenix_frames/frame-$n.svg" --export-type=png --export-filename="images/phoenix_frames/frame-$n.png"
}

# 2) Create animated GIF using ImageMagick
# Adjust -delay (in 1/100ths second) to speed up or slow down the animation
magick convert -delay 10 -loop 0 images/phoenix_frames/frame-*.png -layers Optimize images/phoenix_frames/phoenix_chat.gif

# 3) (Optional) Create a horizontal sprite sheet
magick convert images/phoenix_frames/frame-*.png +append images/phoenix_frames/phoenix_sprite.png

# 4) Use the GIF directly as chat trigger (change src) or use the sprite with CSS background-position animation.

Notes:
- If you don't have Inkscape, you can try rsvg-convert to generate PNGs:
# rsvg-convert -w 160 -h 160 images/phoenix_frames/frame-01.svg -o images/phoenix_frames/frame-01.png

- These frames apply small translate/rotate/scale transforms to `images/logo.svg` (so the bird will appear to bob/tilt). If you want wing-level animation, we'd need a vector version with separable wing paths.

If you'd like, I can also:
- Generate a CSS sprite animation example using the produced sprite sheet.
- Create more frames or tweak motion (faster wings, more bobbing).
