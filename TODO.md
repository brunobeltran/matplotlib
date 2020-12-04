# STACK

1. Add path with no codes test, and explicit bar call

To improve docs at top level:
1. "Matplotlib is a 'layers-based' drawing engine" should be front-and-center somewhere.

To update latest colorbar-rounding-fix stuff....
1. rebase on top of new PR for the actual image stuff, only
MixedModeRenderer's calls to draw_image are dealt with.
    a. DEPENDS: make that PR
    b. DEPENDS: figure out intended behavior for clipping images...

To get marker size normalization:
1. rebase equal_area_markers onto path_area, implement just normalize=...

To get marker bbox correct for tight_bbox:
1. add tests to line2d-window-extents, from old-colorbar-rounding-fix

To get constrained layout with fixed aspect ratio
1. wait on Jody
2. make blog post about figure sizing
    a. DEPENDS: make website to put blog post on

To document colorbar
1. make blog post about matplotlib colors/colorbars
    a. DEPENDS: make website to put blog post on

# Wishlist (Tom/Tim/Jody/etc.)

1. Change the docs deploy process to use rsync --delete but also ignore delete
   on the files that aren't part of the actual docs build.
2. Add links out to tutorials in the API docs using Sphinx.
