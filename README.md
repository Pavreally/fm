<center>

![Fast Mixin](./fm.png)

# Fast Mixin 
`BETA`

#### This solution should improve the scss code development experience. This package is a collection of commonly used components whose task is to improve the perception of the source code. The main goal of this solution is to support large code and quickly change class properties.

</center>

## Features:

- improving the perception of scss code, taking into account the order of importance of the properties;
- control of classes and order of layers;
- control and configuration of adding links;
- compatibility with most css-frameworks;
- compatibility taking into account the order of the arrangement of code with gulp-lint.

# Update 1.0.2 [13.05.2020]:
<p>- File _fm.scss is divided into corresponding sections: _fm-fonts.scss, _fm-links.scss, _fm-blocks.scss. This will increase the convenience of controlling a lot of code.
<br>- In order to simplify, the main plug-in file for the project was changed and renamed.

-------
## How to use it?
Сonnect file `_import.scss` to your project by adding it to the end of the code scss.

> `_import.scss` - includes all connections that work in FM.

```
@import './fm/app/import';
```

`_fm-fonts.scss` - is a fonts control file.<br>
`_fm-links.scss` - is a links control file.<br>
`_fm-blocks.scss` - is a blocks control file.

> All FM functions are commented out by default. They leave their mark only after removing comments in the main file.

<br>

### Fonts control (_fm-fonts.scss)
`$default-fonts-path` - is a variable where the path to fonts is indicated (without a closing slash)
`$fonts-format` - is a variable that indicates the format of the fonts to be used. <br>
Сurrently supported: **`eot woff2 woff ttf otf svg`**

In order to connect the font you just need to specify its main parameters.<p>

> `$fonts:` ( *parameters:* );<p>
**( `'Font Name'`, `'File Name'`, `'/Path'`, Style, Weight, Stretch, Unicode_range ),**

Example:

SCSS (FM):
```
$fonts:
(Font Regular, 'font-regular', '/Font Regular', normal, '', '', U+00-FF),
```

CSS:
```
@font-face {
  font-family: Font Regular;
  font-style: normal;
  src: local("Font Regular");
  src: url("../your_path/fonts/Font Regular/font-regular.woff2") format("woff2");
  src: url("../your_path/fonts/Font Regular/font-regular.woff") format("woff");
  src: url("../your_path/fonts/Font Regular/font-regular.ttf") format("truetype");
  unicode-range: U+00-FF;
}
```

#### Refinements:
- If the `font name` matches the `file name`, then the file name can be omitted by putting empty quotation marks.

Example:<br> SCSS (FM):
```
$fonts:
(Font Regular, '', '', normal),
```
CSS:
```
@font-face {
  font-family: Font Regular;
  font-style: normal;
  src: local("Font Regular");
  src: url("../your_path/fonts/Font Regular.woff2") format("woff2");
  src: url("../your_path/fonts/Font Regular.woff") format("woff");
  src: url("../your_path/fonts/Font Regular.ttf") format("truetype");
}
```
- If you want to set your own custom folder for an individual font, then you need to specify the path leading to the font in the place of parameter `'/Path'` (the path starts with variable `$default-fonts-path`).
- If the remaining parameters that are on the right are not important, then you can skip them*.

> **If the parameter remains one and there are indents in it, then it must be wrapped in quotation marks.*

Example:<br> SCSS (FM):
```
$fonts:
('Font Regular'),
```
CSS:
```
@font-face {
  font-family: "Font Regular";
  src: local("Font Regular");
  src: url("../your_path/fonts/Font Regular.woff2") format("woff2");
  src: url("../your_path/fonts/Font Regular.woff") format("woff");
  src: url("../your_path/fonts/Font Regular.ttf") format("truetype");
}
```

<br>

### Links control (_fm-links.scss)
Links can be for a wrapper class (for example `div` and `section`), or single.

> `$links (-wrap):` ( *parameters:* );<p>
**(z_index, class_name, color, hover_color, underline_direction, name_template_class)**

`z_index` - you can leave the value 'n' or '' if the layer order is not required.<br>
`underline_direction_hover` - returns the direction of the line that appears when hover.<br> Option: **`left, right, center, none`**.

Example:<br> SCSS (FM):
```
$links-wrap:
(10, wrap-link, #000, #ccc, left),
```
CSS:
```
.wrap-link a {
  display: inline-block;
  position: relative;
  border-bottom: 1px solid #000;
  color: #000;
  z-index: 10;
}

.wrap-link a::after {
  left: 0;
  background-color: #ccc;
  content: '';
  display: block;
  height: 1px;
  position: absolute;
  width: 0;
}

.wrap-link a:hover, .wrap-link a:focus {
  color: #ccc;
}

.wrap-link a:hover::after, .wrap-link a:focus::after {
  width: 100%;
}

.wrap-link a:hover {
  color: #ccc;
}
```
<br>If you need to make a custom link, then you need to specify the class of the template `name_template_class`.

> Parameter `name_template_class` can take the value "t" if you want the name of the template and class to be the same. By default, all templates should be located in file: `fm/app/mixins/_mixins.scss`).

Example:<br> SCSS (FM):

`/_fm.scss`
```
$links-wrap:
(n, custom-link, #000, #ccc, n, custom-link-template),
```
`/app/mixins/_mixins.scss`
```
%custom-link-template {
  padding: 10px;
  position: absolute;
}
```

CSS:
```
.custom-link a {
  color: #000;
  padding: 10px;
  position: absolute;
}

.custom-link a:hover {
  color: #ccc;
}
```

<br>

### Blocks control (_fm-blocks.scss)
> `$blocks` ( *parameters:* );<p>
**(z_index, class_name, width, height, position, margin, padding, name_template_class),**

For ease of entry, the properties `position` have been compressed. They can be compressed or not.

> relative    = `r`
<br>absolute  = `a`
<br>fixed     = `f`
<br>static    = `sc`
<br>sticky    = `sy`

Example:<br> SCSS (FM):
```
$blocks:
(10, class, 10px, 20px, r, 0, 10px 15px),
```
CSS:
```
.class {
  height: 20px;
  margin: 0;
  padding: 10px 15px;
  position: relative;
  width: 10px;
  z-index: 10;
}
```
<br>If you need more settings for the block, you can shorten the record and specify the class of the template.

> The value for the template class can only be `'t'`. This means that the class of the block and template have the same name.

Example:<br> SCSS (FM):

`/_fm.scss`
```
$blocks:
(n, class, 10px, 20px, r, 't'),
```
`/app/mixins/_mixins.scss`
```
%class {
  background: #ccc;
}
```

CSS:
```
.class {
  background: #ccc;
  height: 20px;
  position: relative;
  width: 10px;
}
```