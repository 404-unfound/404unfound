---
title: 'Changelog'
toc: false
---

# Changelog

## v2.0.0 (2023/07/23)

- Fixed
	- Fix navbar item padding ([#15](https://github.com/kaiiiz/hugo-theme-monochrome/issues/15))
	- Fix TOC collapse cursor style ([#25](https://github.com/kaiiiz/hugo-theme-monochrome/issues/25), [#36](https://github.com/kaiiiz/hugo-theme-monochrome/pull/36))
	- Fix site search resource retrieval bug ([#29](https://github.com/kaiiiz/hugo-theme-monochrome/issues/29))
	- Fix wrong variable in base.scss ([#44](https://github.com/kaiiiz/hugo-theme-monochrome/pull/44))
	- Fix inappropriate navbar decoration for item that links to home page
	- Fix color scheme button FOUC bug
	- Fix twitter card text color in dark mode
- Changed
	- **BREAKING CHANGE** The minimum hugo version requirement is now `0.112.0`
	- **BREAKING CHANGE** Support all feather icons, `vendor="feather"` is required to migrate from old shortcode ([#19](https://github.com/kaiiiz/hugo-theme-monochrome/issues/19))
	- Images in an aritcle are centered by default ([#35](https://github.com/kaiiiz/hugo-theme-monochrome/issues/35))
	- Term's layout now respects the `enable_group_by_year`, `enable_show_date`, `enable_pagination` parameters
	- Upgrade dependencies
		- Prism: v1.27.0 -> v1.29.0
		- Fuse.js: v6.5.3 -> v6.6.2
- New
	- Support custom CSS ([#23](https://github.com/kaiiiz/hugo-theme-monochrome/issues/23), [#33](https://github.com/kaiiiz/hugo-theme-monochrome/issues/33))
	- Support GA4 ([#37](https://github.com/kaiiiz/hugo-theme-monochrome/issues/37))
	- Support custom title for bookcase item ([#32](https://github.com/kaiiiz/hugo-theme-monochrome/issues/32))
	- Support hiding bookcase items ([#27](https://github.com/kaiiiz/hugo-theme-monochrome/issues/27))
	- Add default color scheme setting ([#40](https://github.com/kaiiiz/hugo-theme-monochrome/issues/40))
	- Add `bookcase` shortcode ([#34](https://github.com/kaiiiz/hugo-theme-monochrome/issues/34))
	- Add `recent-posts` shortcode
	- Add an RSS button next to the title in the term's layout

---

## v1.0.1 (2022/04/02)

### Fixed

* Fix `img` style issue, it should be `max-width` instead of `width` ([#10](https://github.com/kaiiiz/hugo-theme-monochrome/issues/10#issuecomment-1086111789))

---

## v1.0.0 - First Major Release (2022/04/01)

### Added

* Layout
  * New Layout: Postcard Layout, Blank Layout
  * Support pagination for Postcard Layout and List Layout
  * Enhance list layout date format. If current list layout is grouped by year, hide list item's year.
  * Redesign default home page layout using Postcard Layout
  * Add sidebar for better navbar responsiveness on small screen
* New Shortcode
  * `emgithub`: directly embed github source code like gist with Prism.js syntax highlighting
* External Library
  * Optimize Prism.js integration
    * Add smooth transition to prevent line number jittering while page rendering
    * Integrate Prism.js autoloader to prevent huge bundle file
  * Optimize Zooming.js integration
    * Background color is now dynamically changed based on current theme
  * Fuse.js integration
    * Support highlight matched text in search menu
* New Theme Parameters
  * `list_layout.enable_group_by_year`
  * `list_layout.enable_show_date`
  * `list_layout.enable_pagination`
  * `syntax_highlight.lib`
  * `syntax_highlight.builtin.enable_code_copy`
  * `syntax_highlight.prism.enable_code_copy`
  * `syntax_highlight.prism.enable_line_no`
* New Page Parameters
  * `search_hidden`: hide page from search index
  * `pagination`: control pagination for List Layout

### Changed

* Change RSS content from full post to summary for smaller resource purpose
* Change all icons to feathericons
* Site search engine has been changed from flexsearch to fuse.js for better CJK language supports
* Parameter name has all been changed to snake_case for consistency
* Syntax highlight settings are now grouped by `syntax_highlight`
* `svg-wrapper` shortcode has been changed to `icon`

### Removed

* **All** javascript package dependencies are removed, Monochrome is now self-contained without the help of npm and become extremely easy to setup and modify.
* Remove `list_posts` property from list layout, this functionality has been replaced by Blank Layout

---

## v0.9.2 (2022/02/25)

* Fix: reimplement terms-cloud to prevent size overflow (max size: 1.6 rem)

---

## v0.9.1 (2022/02/25)

* Change: set terms-cloud max font size to 1.4 rem
* Fix: site search url from absLangURL to relLangURL

---

## v0.9.0 (2022/01/31)

* Improve: search implementation
  * hide the mismatch objects instead of removing it while searching
  * widen the control range of search result
* Improve: gallery layout's key and value now support markdown syntax
* Improve: Zooming.js configuration
  * speed up animations
  * setup transparent background
* Change: for consistency, the names of the following parameters are changed:
  * page's `collapsibleTOC` => `collapsible_toc`
  * page's `collapsibleChangelogs` => `collapsible_changelogs`
* Change: font-size of html is changed from `18px` to `16px`
* Add: `breadcrumbs` shortcode
* Add: list layout properties `group_by_year` and `show_date`
* Fix: terms-cloud typo ([#4](https://github.com/kaiiiz/hugo-theme-monochrome/pull/4))

---

## v0.8.0 (2021/07/29)

* Improve: flexsearch multilingual search capability
* Improve: widen the control range of navbar menu items
* Change: flexsearch can now search index pages
* Change: the post content in RSS is changed from summary to full text
* Add: collapsible TOC and Changelogs

---

## v0.7.1 (2021/07/08)

* Change: adjust the style of changelogs
* Fix: flexsearch search API issue

---

## v0.7.0 (2021/06/24)

* Bump dependencies version
* Add: `changelogs` property
* Add: support disqus for index page (turn off by default)
* Change: theme name from 'Graytr' to 'Monochrome'
* Change: improve navbar responsiveness
* Fix: widen the control range of navbar menu icon

---

## v0.6.2 (2021/04/15)

* Fix: google search console authentication failed if ga code not in `<head>`
* Fix: `clipboard-polyfill/text` dependency resolved issue

---

## v0.6.1 (2021/02/17)

* Fix: codepen shortcode id
* Change baseurl to example.com in exampleSite

---

## v0.6.0 (2021/02/16)

* Support MathJax
* Add shortcodes: `jsfiddle`, `codepen`, `color-block`, `icon-group`, `terms-cloud`
* Add option: `list_posts` in list layout
* Fix: not register touchend event

---

## v0.5.0 (2021/02/15)

* Support multilingual mode
  * Add i18n
* Support Open Graph, Twitter Card
* Adjust svg-wrapper shortcode
  * Add some social icons: `facebook`, `github`, `twitter`, `rss`, `mail`
* Support site search (powered by flexsearch)
* Disable unused features in tailwindcss (speedup building time)
* Reorganize parameters
* Remove default image to figure transformation, using built-in shortcode instead

---

## v0.4.0 (2021/02/12)

* Support dark mode
  * Add `bookcase_cover_src_dark` for bookcase layout
  * Add `balloon_img_src_dark` for balloon layout
  * Add shortcode: svg-wrapper
* Prism.js integration (v1.23.0)
* Code copy feature for code block

---

## v0.3.0 (2021/02/10)

* Support nested navbar
* Implement default homepage layout
* Adjust the purpose of bookcase layout
* Remove catalog layout

---

## v0.2.0 (2021/02/09)

* A better responsive navbar
* Implement default taxonomy and term layout
* Adjust article styles

---

## v0.1.0 (2021/02/08)

Initial release.

