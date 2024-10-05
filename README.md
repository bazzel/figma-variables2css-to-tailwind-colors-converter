# figma-variables2css-to-tailwind-colors-converter

I created this repo to add the missing link between defining color schemes in Figma and have them available in Tailwind taken the following requirements into account:

- not too many manual work or adjustments should be required 😅,
- in Figma, colors are defined as variables so we can have modes for [light and dark color themes](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme),
- we use the [tailwind-mode-aware-colors](https://github.com/JavierM42/tailwind-mode-aware-colors) plugin to define the colors in Tailwind - this will automatically 'generate dynamic colors that will automatically switch between light and dark mode variants' so we don't need to add separate classes (e.g. `dark:bg-primary-400`) for dark mode.

All this is possible with Figma plugins, however I didn't succeed in generating a `tailwind.config.js` with colors structured [as described](https://github.com/JavierM42/tailwind-mode-aware-colors?tab=readme-ov-file#tailwindconfigjs) by the tailwind-mode-aware-colors plugin. So I wrote a simple script to accomplish this.

## Prerequisites

You will need the following properly installed on your computer:

- [Git](https://git-scm.com/)
- [Ruby](https://www.ruby-lang.org/)

## Installation

```bash
$ git clone git@github.com:bazzel/figma-variables2css-to-tailwind-colors-converter.git
$ cd figma-variables2css-to-tailwind-colors-converter
$ chmod a+x var-parser
```

## Running

To run this script successfully, you need to define your colors as variables in Figma and the colors should have modes named `light` and `dark`. The image below shows an example - it uses the names `Colors` for the collection, `primary`, `secondary`, etc. for the groups and has a variable every shade (`50`, `100`, ...), however the names are subordinate, as long as the modes are named properly.

> To work with variables, you need a paid account.

![Figma color variables](/images/variables.png)

- Once you have defined you variables, switch to Figma and run the plugin **variables2css**.
- Select the collection that contain the colors.
- Select **Json** as **Type** and hex as **Color**.

![variables2css config](/images/config.png)

- Generate the output and copy it to a file (here we name the file `export.json`).
- Transform the file's content that we can use in Tailwind:

```bash
$ cd figma-variables2css-to-tailwind-colors-converter
$ var-parser ./export.json
```

- You now have a file `colors.js`. Move this file to the same folder where `tailwind.config.js` is located.
- Adjust `tailwind.config.js` as follows:

```javascript
const myColors = require(./colors);

module.exports = require('tailwind-mode-aware-colors')({
  theme: {
    colors: myColors.colors,
  },
  ...
});
```

> Notice that `require('tailwind-mode-aware-colors')` is added. You need [this plugin](https://github.com/JavierM42/tailwind-mode-aware-colors) for [mode-aware colors](https://www.wyeworks.com/blog/2022/10/12/mode-aware-colors-with-tailwind-css/).

And that's it.

Figma plugins:

- [Tailwind CSS Color Generator](https://www.figma.com/community/plugin/1242548152689430610)
- [variables2css](https://www.figma.com/community/plugin/1261234393153346915)
- [Dark Mode Magic](https://www.figma.com/community/plugin/834062945643616879)
