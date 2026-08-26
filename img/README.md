# img/

Media for tacedge.org. Referenced normally from `index.html`; this folder is
the one exception to the single-file rule and survives the WordPress migration
as a media folder.

## Partner marks

The logo row in §05 is written and commented out in `index.html`. Drop these
files in and delete the comment markers around `<div class="logos">`:

    logo-unsw-canberra.svg          School of Engineering and Technology, UNSW Canberra
    logo-unsw-dri.svg               UNSW Defence Research Institute
    logo-unsw-canberra-space.svg    UNSW Canberra Space
    logo-kics.svg                   Korean Institute of Communications and Information Sciences
    logo-ntust.svg                  National Taiwan University of Science and Technology
    logo-lpu.svg                    Lyceum of the Philippines University

SVG preferred; transparent PNG at 2x otherwise. They are rendered greyscale at
a uniform 46px optical height, so supply them on a transparent background and
avoid marks that rely on colour to be legible.

Confirm each partner is happy to be shown before publishing the row.

## Photographs and figures

    fig-01-arena.jpg    wide, foyer demonstrations or the robot arena in build
    fig-02-arena.svg    capture-the-flag arena layout
