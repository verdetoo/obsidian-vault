---
cover: https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Simple-Sizing.png
---
## [[Image Positions]] - plugin
Last updated Nov 19, 2022
by [SlRvb](https://slrvb.github.io/Site/) Inspired by **[Lithou’s Image Flags](https://github.com/Lithou/Sandbox/blob/main/.obsidian/snippets/pub-ImageFlags.css)**

### Syntax

> **⚠ Note:** Markdown syntax will need the note to be reopened to see changes.

**Wiki Links:** `![[Image.png|modifier+modifier|#]]`

**Markdown:** `![|modifier+modifier|#](Image.png)`

**Example**: `![[Image.png|left+pt|200]]`

- `|` is required _after_ the image extension and _before_ Obsidian’s sizing numbers
- `+` can be anything:
    - `![[Image.png|left-pt-sban|200]]`
    - `![[Image.png|left_pt_sban|200]]`
    - `![[Image.png|left|pt|sban|200]]`
    - `![[Image.png|left pt sban|200]]`

### Simplified Version

> Optimized for easier comprehension and “quicker” styling

`![[Image.png|left+small]]`

#### Positions

`left`, `center`, `right`

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Simple-Positions.png)


#### Types

Style of sizing for images.


| ---        | ---                                                   | 
| ---------- | ----------------------------------------------------- |
| `banner`   | Expand image to width of the page                     |
| `portrait` | Larger image height than image’s width                |
| `p.rofile` | Width and height of the image are equal               |
| `circle`   | Round edges of a squared image (best with `p.rofile`) |

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Simple-Types.png)

#### Type Sizing

> **⚠ Note**: Obsidian’s number sizing `|##` will **not** work with these.

**Format**: `![[Image.png|modifier+size]]`

- **Banner & Portrait** use `+small` and `+tall` sizing
- **Profile** uses `+medium` only.

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Simple-Sizing.png)

### Customizable Version
> Allows for more adjustments to images than the **Simplified Version**.

##### Position & Types


| ---     | ---                                                           |
| ------- | ------------------------------------------------------------- |
| `locr`  | Location Right, float image to right of note                  |
| `locl`  | Location Left, float image to left of note                    |
| `ctr`   | Center image on page                                          |
| `sban`  | Banner sets width of image to 100%, height will be adjustable |
| `cover` | Fit image in bounds without stretching                        |

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Custom-Pos%2BTypes.png)

#### Inner Photo Position

> Adjustments to target which part of the _inside_ of the image to move around, **not** the where the image is moved to on a page.

#### Basic

|Basic|Position|
|---|---|
|`pt`|Top|
|`pb`|Bottom|
|`pl`|Left|
|`pr`|Right|
|`pc`|Center|

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Custom--Inner-Position-Basic.png)

#### Precise

**Format**: `![[|p+precise]]`

|Top|Bottom|Left|Right|
|---|---|---|---|
|`p+ct`|`p+cb`|`p+cl`|`p+cr`|
|`p+cct`|`p+ccb`|||
|`p+tc`|`p+bc`|||
|`p+tcc`|`p+bcc`|||

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Custom--Inner-Position-Precise.png)

#### Sizing

|Height|Width|PX|
|---|---|---|
|`hmicro`|`wmicro`|70|
|`htiny`|`wtiny`|100|
|`hsmall`|`wsmall`|200|
|`hs-med`|`ws-med`|300|
|`hm-sm`|`wm-sm`|400|
|`hmed`|`wmed`|500|
|`hm-tl`|`wm-tl`|600|
|`htall`|`wtall`|700|

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Custom-Sizing.png)

### Extras

Uses some of the [**Auto-Adaptive Images**](https://forum.obsidian.md/t/auto-adaptive-images-for-dark-light-theme/13494) snippet with slightly different synax.

| ---        | ---                                                         | 
| ---------- | ----------------------------------------------------------- |
| `invertw`  | Flip image colors of white lined images                     |
| `invertb`  | Flip image colors of dark lined images                      |
| `invertwc` | Flip image colors of white lined images                     |
| `invertbc` | Flip image colors of dark lined images                      |
| `clear`    | Image will sit below another image if it’s on the same side |

![|400](https://raw.githubusercontent.com/SlRvb/Obsidian--ITS-Theme/main/Images/Image_Adjustments-Extras_Invert.gif)