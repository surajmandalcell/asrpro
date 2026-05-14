---
version: alpha
name: ASR Pro Desktop
description: Quiet macOS-style desktop design system for private speech recognition and transcription workflows.
colors:
  background: "#2F2F2F"
  background-raised: "#333333"
  sidebar: "#3C3C3C"
  sidebar-active: "#686868"
  surface: "#3A3A3A"
  surface-soft: "#383838"
  surface-control: "#303030"
  surface-control-hover: "#3A3A3A"
  surface-row-hover: "#424242"
  surface-selected: "#5A5A5A"
  surface-elevated: "#2B2B2B"
  border: "#3F3F3F"
  border-sidebar: "#545454"
  border-control: "#5C5C5C"
  divider: "#474747"
  text-primary: "#EEEEEE"
  text-heading: "#F4F4F4"
  text-body: "#CFCFCF"
  text-muted: "#A8A8A8"
  text-subtle: "#8E8E8E"
  text-disabled: "#8A8A8A"
  focus: "#9BCFFF"
  accent-blue: "#0A84FF"
  accent-orange: "#FF7A32"
  accent-purple: "#7167FF"
  accent-teal: "#92C2C6"
  status-error: "#FF9C8F"
  status-warning: "#FFB3AA"
  logo-surface: "#F6F4EF"
  logo-ink: "#26343B"
  window-close: "#FF5F57"
  window-close-glyph: "#6E140F"
  window-minimize: "#FEBC2E"
  window-minimize-glyph: "#8F5B00"
typography:
  display-md:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 24px
    fontWeight: 600
    lineHeight: 1.17
    letterSpacing: 0em
  title-md:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 15px
    fontWeight: 600
    lineHeight: 1
    letterSpacing: 0em
  row-title:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 14px
    fontWeight: 600
    lineHeight: 1.43
    letterSpacing: 0em
  body-md:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 13px
    fontWeight: 500
    lineHeight: 1.54
    letterSpacing: 0em
  body-sm:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 12px
    fontWeight: 500
    lineHeight: 1.67
    letterSpacing: 0em
  label-md:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 12px
    fontWeight: 600
    lineHeight: 1
    letterSpacing: 0em
  label-caps:
    fontFamily: "Inter, -apple-system, BlinkMacSystemFont, SF Pro Text, Segoe UI, sans-serif"
    fontSize: 11px
    fontWeight: 600
    lineHeight: 1.82
    letterSpacing: 0em
rounded:
  none: 0px
  xs: 5px
  sm: 7px
  nav: 9px
  control: 10px
  md: 12px
  app-icon: 16px
  panel: 22px
  full: 9999px
spacing:
  hairline: 1px
  xxs: 2px
  xs: 4px
  sm: 8px
  md: 12px
  lg: 16px
  xl: 20px
  "2xl": 24px
  "3xl": 32px
  sidebar-width: 208px
  content-max-width: 520px
  window-width: 780px
  window-height: 520px
  toolbar-height: 34px
  sidebar-title-height: 48px
components:
  app-window:
    backgroundColor: "{colors.background}"
    textColor: "{colors.text-primary}"
    width: "{spacing.window-width}"
    height: "{spacing.window-height}"
  sidebar:
    backgroundColor: "{colors.sidebar}"
    textColor: "{colors.text-body}"
    width: "{spacing.sidebar-width}"
    borderColor: "{colors.border-sidebar}"
  nav-item:
    typography: "{typography.body-md}"
    rounded: "{rounded.nav}"
    height: 36px
    padding: 10px
    textColor: "{colors.text-body}"
  nav-item-active:
    backgroundColor: "{colors.sidebar-active}"
    textColor: "{colors.text-primary}"
  panel:
    backgroundColor: "{colors.surface}"
    borderColor: "{colors.divider}"
    rounded: "{rounded.panel}"
    padding: 20px
  panel-divider:
    borderColor: "{colors.divider}"
    size: 1px
  button-panel:
    backgroundColor: "{colors.surface-control}"
    textColor: "{colors.text-primary}"
    borderColor: "{colors.border-control}"
    rounded: "{rounded.md}"
    height: 32px
    padding: 10px
    typography: "{typography.label-md}"
  button-panel-hover:
    backgroundColor: "{colors.surface-control-hover}"
  dropdown-surface:
    backgroundColor: "{colors.surface-control}"
    borderColor: "{colors.border-control}"
    rounded: "{rounded.md}"
    padding: 4px
  dropdown-option:
    rounded: "{rounded.control}"
    padding: 10px
    typography: "{typography.label-md}"
  dropdown-option-selected:
    backgroundColor: "{colors.surface-selected}"
    textColor: "{colors.text-primary}"
  segmented-control:
    backgroundColor: "{colors.surface-elevated}"
    borderColor: "{colors.divider}"
    rounded: "{rounded.md}"
    padding: 2px
  segmented-item:
    rounded: "{rounded.control}"
    height: 28px
    typography: "{typography.label-md}"
  icon-tile:
    backgroundColor: "{colors.surface-soft}"
    textColor: "{colors.text-body}"
    rounded: "{rounded.md}"
    size: 28px
  focus-ring:
    borderColor: "{colors.focus}"
    size: 2px
  traffic-light-close:
    backgroundColor: "{colors.window-close}"
    textColor: "{colors.window-close-glyph}"
    size: 13px
    rounded: "{rounded.full}"
  traffic-light-minimize:
    backgroundColor: "{colors.window-minimize}"
    textColor: "{colors.window-minimize-glyph}"
    size: 13px
    rounded: "{rounded.full}"
---

# ASR Pro Design System

## Overview

ASR Pro is a quiet, fixed-size desktop workspace for private dictation, file transcription, local model selection, and playback. It should feel native to macOS, focused, and workmanlike: a compact utility that stays out of the way while still feeling carefully finished.

The product is not a marketing site and should never use a hero layout, promotional panels, or oversized decorative compositions. The first screen should be the working app. Layouts should be dense enough for repeated daily use, but calm enough that recording and transcription states remain legible.

The visual personality is graphite, local-first, and precise. Use familiar macOS cues where they help: traffic lights, a left navigation rail, soft grouped surfaces, subtle blur, small type, and icon-led controls. Avoid exposing implementation details such as framework names, runtime stacks, or internal architecture in the UI.

## Colors

The palette is built from dark graphite neutrals with a few purposeful accents.

- **Background (#2F2F2F):** The app shell background. It should feel steady and quiet, never pure black.
- **Sidebar (#3C3C3C):** The navigation rail. It is slightly lifted from the app background and uses muted text.
- **Surface (#3A3A3A):** Grouped content panels. Surfaces should be translucent or tonal, not bright cards.
- **Control (#303030):** Buttons, dropdowns, and compact input-like controls.
- **Text primary (#EEEEEE):** Main readable text on dark surfaces.
- **Text muted (#A8A8A8):** Section headings, captions, and secondary information.
- **Focus (#9BCFFF):** Keyboard focus and selected device/model affordances.
- **Blue (#0A84FF):** Active playback or selected confirmation. Use sparingly.
- **Orange (#FF7A32):** Home icon accent only.
- **Purple (#7167FF):** History icon accent only.
- **Teal (#92C2C6):** Product identity accent for the logo mark and About content, not for the sidebar icon.
- **Error (#FF9C8F):** Recording and device errors.

Do not use pure white borders. Dividers should feel like tonal changes within the dark surface, using `divider` or low-opacity white equivalents. Do not create multiple nested cards by stacking bright outlines inside panels.

## Typography

Typography follows the app chrome: compact, system-native, and high signal. The default stack is Inter with Apple system fallbacks.

- **Display:** 24px semibold for product names and About headings.
- **Titles:** 14-15px semibold for row titles, statistics, and compact headings.
- **Body:** 13px medium or semibold for row detail and useful descriptions.
- **Labels:** 12px semibold for buttons, toolbar controls, and metadata.
- **Caps labels:** 11px semibold uppercase for definition-list keys such as Version, Recognition, and Data folder.

Letter spacing stays at `0em`. Do not use wide tracking except where required by native controls. Avoid using more than two weights in one view. Most screens should be readable with 500 and 600 only.

## Layout

The primary app window is fixed at 780px by 520px. Design all core screens to fit this shell without requiring window resizing. The desktop layout uses a 208px sidebar and a content area with a 34px toolbar above a scrollable pane.

Content should center inside a maximum width of 520px. This keeps the app compact and prevents panels from becoming wide, low-density strips. Use 8px, 12px, 16px, and 20px as the dominant spacing rhythm. Use 2px and 4px only for inset control adjustments.

Page sections should be unframed or contained in a single grouped surface. Do not place cards inside cards. For About, use one grouped product summary surface with flat rows and action links inside that surface.

Scrollable areas should keep stable gutters and hidden scrollbars until interaction. Text must truncate or wrap intentionally inside fixed containers. Long local paths should display as `~/...` when they are under the user home directory.

## Elevation & Depth

Depth is tonal, not shadow-heavy. The app uses glass-like grouped surfaces through dark fill, subtle border, and backdrop blur. Heavy shadows are reserved for floating dropdowns and menus only.

Use these hierarchy methods in order:

- tonal contrast between background, sidebar, panel, and controls
- one-pixel tonal dividers
- hover fills on rows and links
- focus rings for keyboard navigation
- shadow only for overlays, dropdowns, and menus

Avoid white outlines, stacked borders, and large drop shadows on normal content panels.

## Shapes

The shape language is compact macOS utility.

- **Panels:** 22px radius for grouped surfaces.
- **App logo tile:** 16px radius.
- **Buttons and icon tiles:** 12px radius.
- **Inset active states and dropdown options:** 10px radius.
- **Sidebar navigation:** 9px radius.
- **Small badges and waveform bars:** 5-7px radius.
- **Traffic lights:** fully round.

Do not mix sharp rectangular controls with rounded grouped panels in the same view. Corners should feel consistent and deliberate.

## Components

**App Shell:** The shell is fixed-size and non-resizable. It has a left sidebar, a top toolbar, and a scrollable content region. The frame is custom, with close and minimize traffic lights only.

**Sidebar Navigation:** Items are 36px high with 20px icon tiles and compact labels. The active state uses `sidebar-active` and white text. Most icons use neutral gray tiles. Home and History may keep their section accents, but About stays neutral.

**Grouped Panels:** Panels use `panel` styling: dark tonal fill, 22px radius, clipped overflow, and tonal dividers. Use rows inside a panel instead of nested cards.

**Panel Rows:** Rows use icon tile, title, optional detail, and optional trailing control. Hover states use a dark tonal fill, not a border.

**Buttons:** Panel buttons are compact, 32px minimum height, 12px radius, semibold 12px labels, and subtle active scale. Use icon buttons where the action is familiar.

**Dropdowns:** Dropdown surfaces use a 12px radius, `surface-control`, a 1px control border, and a strong overlay shadow. Options use 10px radius and should support icons and checkmarks.

**Segmented Controls:** Segmented controls use a dark inset background, 12px outer radius, 10px inner item radius, 28px height, and clear pressed state.

**Traffic Lights:** Window controls are 13px circles with hover-only glyphs. Do not add a maximize control or any alternate maximize affordance.

**About Links:** GitHub and issue links live inside the About grouped surface as flat action rows. They should use icon tiles, title, detail text, and a trailing arrow.

**Recording Overlay:** The overlay should be a compact dark waveform pill, not a text-heavy modal. Motion should be functional and stable.

## Do's and Don'ts

- Do keep the app utility-first, dense, and calm.
- Do use one grouped surface per information cluster.
- Do use tonal dividers instead of white borders.
- Do use `~/...` for home-relative storage paths in user-facing UI.
- Do use lucide icons for recognizable actions.
- Do keep the About page product-focused: version, recognition purpose, data folder, GitHub, and issue link.
- Don't expose implementation stack names such as Electron or React in product UI.
- Don't add highlights sections or marketing copy to About.
- Don't stack multiple card levels inside a single panel.
- Don't make the window resizable, maximizable, or fullscreenable.
- Don't use decorative gradients, blobs, or oversized hero sections.
- Don't use pure black or pure white as dominant UI colors.
