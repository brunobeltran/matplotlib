# PRs

## GSOD
0. hatch - gsod hatch impl
1. `join_cap_style` - joinstyle/capstyle and generic `_types` doc formatting

## Transform stuff
0. bbox-union-intersection - same as below but with less changes. leave Bbox as is but only under union and intersection cause the behavior to be correct.
1. bbox-null-fix - make Bbox behave better mathematically. need to comb through tests to see if I fail anything important
2. transform-matmul - UNIMPLEMENTED. Lots of push back but seems fun. Issue \#17230.

## Rounding-to-pixel issues in `draw_image`
1. half-pixel-offset-fix - UNIMPLEMENTED. Fix issue \#1441
2. colorbar-rounding-fix - OLD. PR to center colorbars. After we get the correct transform to pass to `draw_image` in general to fix rounding issues in `image.py`, we should rebase this PR on top of that, as that should invalidate about half the work done here (this PR tried to do both those jobs initially)
3. draw-image-rounding-fix - PART. Has `_ImageBase.get_window_extent` and `composite_images_bbox` to keep track of intended output bbox size.
4. get-final-transform - PART. Has `_ImageBase._get_final_transform` to mimic the `trans` return value of the `_make_image` helper.
4. image-bbox-tracking - OLD. UNTESTED. Tries to do colorbar-rounding-fix, but only the parts not covered by the new image.py work. Unclear if works.
5. new-draw-image-rounding-fix - first attempt at image.py rounding fixes. only works when user does not set transform.


## Marker/Path Size
1. equal_area_markers - HAS TODO. `normalization=` proposal to `MarkerStyle.__init__`. Needs *path_area*. Needs to be coded then rebased on top of just *path_area*.
2. path_area - compute signed area inside of simple, closed paths
3. path_length - compute the length of an arbitrary path using iterative approximation. Needs to be rebased on master now that path_size_calc is merged.
4. path_center_of_mass - compute the center of mass of arbitrary path, both along edge and interior. needs *path_length* to do separable paths.
5. centered_markers - UNIMPLEMENTED. Build on top of *path_center_of_mass*.
6. old-markeredge-bbox-fix - HAS TODO. currently rebased on top of ead45c7e6.

## Marker/Path/Line2D Bbox/window_extents
1. stroked_path_width - compute the actual window extents of a path with arbitrary stroke parameters.
2. line2d-window-extents - HAS TODO. correct window extents for Line2D. Needs *stroked_path_width*.
3. ??? - UNIMPLEMENTED. Check if we need to fix any other artists' get_window_extents due to strokes.
4. miterlimit - UNIMPLEMENTED. Have matplotlib track the miterlimit. Use it correctly in all the backends. Some skeleton code was added as I explored the codebase. check where "stroke-linecap: butt" is being written into the SVG file too see how SVG get sthis information for the linecap.
5. hatch\_replacement: full refactor, allow arbitrary markers, arbitrary
   spacing/offset, and easy density adjustments

## Meta
1. pr-template-improvements - title is self-explanatory.



