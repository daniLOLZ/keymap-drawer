# Configuration options

This page details the configuration options available for parsing and drawing, which can be provided to the CLI or can be set in the web UI in the "Configuration" box.
Also see the [customization section](README.md#configuration) in the README for usage.

## Draw Configuration

These settings are nested under the `draw_config` field and applies to `keymap draw` subcommand in the CLI, as well as the conversion from keymap YAML input to SVG in the web app.
In addition to the configuration file, this field can also be set in the [keymap YAML](KEYMAP_SPEC.md#draw_config) which overrides the former.

#### `key_w`, `key_h`

Key dimensions. Non-ortho layouts (e.g. via `qmk_keyboard`) use `key_h` for both width and height, whereas `ortho_layout` uses both.

_Type:_ `float`

_Default:_ `60`, `56`

#### `split_gap`

_Type:_ `float`

_Default:_ `30`

#### `combo_w`, `combo_h`

Dimensions of combo boxes that are drawn on layer diagrams.

_Type:_ `float`

_Default:_ `28`, `26`

#### `key_rx`, `key_ry`

Curvature of rounded key rectangles, used both for key and combo boxes

_Type:_ `float`

_Default:_ `6`

#### `n_columns`

Number of layer columns in the output drawing.
For example if this is set to 2, two layers will be shown side-by-side and layers will go right then down.

_Type:_ `int`

_Default:_ `1`

#### `separate_combo_diagrams`

When set, visualize combos using separate mini-layout diagrams rather than drawing them on layers.
This sets the default behavior, which can be overridden by the `draw_separate` field of the [`ComboSpec`](KEYMAP_SPEC.md#combos).

_Type:_ `bool`

_Default:_ `false`

#### `combo_diagrams_scale`

For combos visualized separate from layers, this is the scale factor for the mini-layout used to show their key positions.

_Type:_ `int`

_Default:_ `2`

#### `inner_pad_w`, `inner_pad_h`

The amount of padding between adjacent keys, in two axes.

_Type:_ `float`

_Default:_ `2`, `2`

#### `outer_pad_w`, `outer_pad_h`

Padding amount between layer diagrams, in two axes.

_Type:_ `float`

_Default:_ `30`, `56`

#### `line_spacing`

Spacing between multi-line text in key labels in units of `em`.

_Type:_ `float`

_Default:_ `1.2`

#### `arc_radius`

Radius of the curve for combo dendrons that are drawn from the combo box to the key positions.

_Type:_ `float`

_Default:_ `6`

#### `append_colon_to_layer_header`

Whether to add a colon after layer name while printing the layer header.

_Type:_ `bool`

_Default:_ `true`

#### `enable_checkerboard_background`

Whether to make a checkerboard pattern for the various layers.

_Type:_ `bool`

_Default:_ `false`

#### `small_pad`

Padding from edge of a key representation to top ("shifted") and bottom ("hold") legends.

_Type:_ `float`

_Default:_ `2`

#### `legend_rel_x`, `legend_rel_y`

Position of center ("tap") key legend relative to the center of the key.
Can be useful to tweak when `draw_key_sides` is used.

_Type:_ `float`

_Default:_ `0`, `0`

#### `draw_key_sides`

Draw "key sides" on key representations, which can be made to look like keycap profiles.
The shape is determined by `key_side_pars`.

_Type:_ `bool`

_Default:_ `false`

#### `key_side_pars`

A mapping of certain field names to their values, characterizing key side drawing. Valid fields:

- **`rel_x`**, **`rel_y`** (type: `float`): Position of internal key rectangle relative to the center of the key. _Default:_ `0`, `4`
- **`rel_w`**, **`rel_y`** (type: `float`): Delta dimension between external key rectangle and internal key rectangle. _Default:_ `12`, `12`
- **`rx`**, **`ry`** (type: `float`): Curvature of the rounded internal key rectangle. _Default:_ `4`, `4`

#### `svg_style`

The CSS used for the SVG styling. This includes font settings, styling of key and combo rectangles and texts within them, along with some tweaks for external SVG glyphs.
Users are encouraged to not change the default value and use `svg_extra_style` to specify overrides instead.

_Type:_ `string`

_Default:_ See [`config.py`](keymap_drawer/config.py)

#### `svg_extra_style`

Extra CSS that will be appended to `svg_style`, enabling augmenting and overriding properties.

_Type:_ `string`

_Default:_ Empty

#### `shrink_wide_legends`

Shrink font size for legends wider than this many chars, set to 0 to disable.
Ideal value depends on the font size defined in `svg_style`/`svg_extra_style` and width of the boxes.

_Type:_ `int`

_Default:_ `7`

#### `glyph_tap_size`, `glyph_hold_size`, `glyph_shifted_size`

Height in `px` for SVG glyphs, in different key fields.

_Type:_ `int`

_Default:_ `14`, `12`, `10`

#### `glyphs`

Mapping of glyph names to be used in key fields to their SVG definitions.

_Type:_ `dict[str, str]`

_Default:_ Empty

#### `glyph_urls`

Mapping of sources to (possibly templated) URLs for fetching SVG glyphs.
For instance, `$$material:settings$$` will use the value for `material` and replace `{}` in the value with `settings`.

_Type:_ `dict[str, str]`

_Default:_ See [`config.py`](keymap_drawer/config.py)

#### `use_local_cache`

Use a local filesystem cache on an OS-specific location for downloaded QMK keyboard jsons and SVG glyphs.

_Type:_ `bool`

_Default:_ `true`

## Parse configuration

These settings are nested under the `parse_config` field and applies to `keymap parse` subcommand in the CLI, as well as the conversion from "Parse from..." input forms to the keymap YAML text area in the web app.

#### `preprocess`

Run C preprocessor on ZMK keymaps.

_Type:_ `bool`

_Default:_ `true`

#### `skip_binding_parsing`

Do not do any keycode/binding parsing (except as specified by `raw_binding_map`).

_Type:_ `bool`

_Default:_ `false`

#### `raw_binding_map`

Convert raw keycode/binding strings specified as keys to the representations given by their values.[^1]

[^1]: The value can be a [`LayoutKey` mapping](KEYMAP_SPEC.md#layers) or a string representing the tap legend.

If a conversion was made, shortcut any further processing.
E.g. `{"QK_BOOT": "BOOT", "&bootloader": "BOOT"}`.

_Type:_ `dict[str, str | dict]`

_Default:_ `{}`

#### `sticky_label`

Display text to place in hold field for sticky/one-shot keys.

_Type:_ `str`

_Default:_ `"sticky"`

#### `toggle_label`

Display text to place in hold field for toggled keys.

_Type:_ `str`

_Default:_ `"toggle"`

#### `trans_legend`

Legend to output for transparent keys.[^1]

_Type:_ `str | dict`

_Default:_ `{"t": "▽", "type": "trans"}`

#### `mark_alternate_layer_activators`

Rather than only marking the first sequence of key positions to reach a layer as "held",
mark all of the sequences to reach a given layer. this is disabled by default because it
creates ambiguity: you cannot tell if _all_ the marked keys need to be held down while a
layer is active (which is the default behavior) or _any_ of them (with this option).

_Type:_ `bool`

_Default:_ `false`

#### `qmk_remove_keycode_prefix`

Remove these prefixes from QMK keycodes before further processing.
Can be augmented with other locale prefixes, e.g. `"DE_"` for German locale headers.

_Type:_ `list[str]`

_Default:_ `["KC_"]`

#### `qmk_keycode_map`

Mapping to convert QMK keycodes to their display forms, applied after removing prefixes in `qmk_remove_keycode_prefix`.[^1]

_Type:_ `dict[str, str | dict]`

_Default:_ See [`config.py`](keymap_drawer/config.py)

#### `zmk_remove_keycode_prefix`

Remove these prefixes from ZMK keycodes before further processing.
Can be augmented with other locale prefixes, e.g. `"DE_"` for German locale headers generated by `zmk-locale-generator`.

_Type:_ `list[str]`

_Default:_ `[]`

#### `zmk_keycode_map`

Mapping to convert ZMK keycodes to their display forms, applied after removing prefixes in `zmk_remove_keycode_prefix`.[^1]

_Type:_ `dict[str, str | dict]`

_Default:_ See [`config.py`](keymap_drawer/config.py)

#### `zmk_combos`

Mapping to augment the output field for parsed combos. The key names are the devicetree node names for
combos in the keymap and the value is a dict containing fields from the [`ComboSpec`](KEYMAP_SPEC.md#combos).

E.g. `{"combo_esc": {"align": "top", "offset": 0.5}}` would add these two fields to the output for combo that has node name `combo_esc`.

_Type:_ `dict[str, dict]`

_Default:_ `{}`
