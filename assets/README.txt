IMAGES THE SITE USES
====================

In place and live:

  logo.png          header mark, transparent, 340px, 20KB
  icon.png          favicon / home-screen icon, 512x512
  logo-social.png   link preview card, 1200x630
  shot1.jpg         Porsche Taycan      | the "Recent work" grid,
  shot2.jpg         Audi S4             | 1200x900 each, cropped
  shot3.jpg         Mercedes E-Class    | and compressed for web
  shot4.jpg         BMW 5 Series        |

To swap a grid photo: replace the file, keep the name. To add or remove
one, edit CFG.SHOTS in index.html.

NOTE: the full-resolution 1254x1254 logo original is no longer on disk.
Drop it back here as logo-full.png whenever and the three logo assets
can be regenerated larger. The site will not change; the header renders
at 66px.


BEFORE / AFTER SLIDERS (not on yet)

CFG.BA is empty, so no sliders render and the heading reads "Recent
work." The moment you shoot a real pair, add it:

  BA: [
    {before:'assets/ba1-before.jpg', after:'assets/ba1-after.jpg',
     cap:'Interior, shampooed and steamed'}
  ]

and drop those two files here. Sliders appear above the grid and the
heading switches back to "Drag the slider."

Shoot the SAME ANGLE both times. Stand in one spot, take the before,
do the job, stand in the same spot, take the after. That is the whole
effect, and it is the single highest-converting thing you can add to
this page.

Do not generate a "before" image. The caption claims it is the same
car on the same day, and car people spot AI artifacts in reflections,
plates and trim.
