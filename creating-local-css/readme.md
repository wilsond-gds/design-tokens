# Creating a CSS file from the JSON file

## Convert the JSON to SASS

Install [json-to-scss](https://www.npmjs.com/package/json-to-scss). For this repository, you can create a `sass` file
from the `shared-code` folder by running `npm run process-json` on the command line.

The SCSS file will be created in the `creating-local-css` folder.

## Include the converted file in your project’s SASS file

From there, you can create/modify your project’s SASS file as usual, remembering to turn on using SASS maps and to
include the design token sass file, e.g.

```scss
@use "sass:map";
@use "design-tokens";
```

## Referring to design assets

Currently the syntax looks like this:

```scss
 color: map.get

(
design-tokens.$design-tokens, gov-uk, govuk-text-colour, value

)
;

```

We ask SASS for the map, then work through the values in the array until we have referenced the one we need. This could
be several levels deep and depends on the structure of the JSON file - so compare:

```scss
$design-tokens: (
        gov-uk: (
                govuk-text-colour: (
                        value: #0b0c0c,
                        type: color
                ),
                gds-body-typography: (
                        value: (
                                fontFamily: "GDS Transport Website",
                                fontWeight: light
                        ),
                        type: typography
                )
        )
);


p {
  color: map.get(design-tokens.$design-tokens, gov-uk, govuk-text-colour, value);
  font-family: map.get(design-tokens.$design-tokens, gov-uk, gds-body-typography, value, fontFamily), sans-serif;
}

```
The value we need for the `font-family` is at a different depth to the value for the `color`.

## Output CSS

From there you can output CSS as normal. In this demo you can run `npm run build-sass` to output a CSS file in the same
folder as your SCSS file.