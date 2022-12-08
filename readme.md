# What are design tokens?

Design tokens are JSON files that can be consumed by different parts of an overall design and development process so design choices (colours, typefaces) can be kept in sync between design and development.

The design tokens file is ideally stored in a version controlled environment that can be accessed online (ensure the CORS setting for the file allows connections from outside the domain so Figma can connect to it).

For typefaces the name has to match exactly, including the case, so `Georgia` not `georgia`. Font names with spaces should be quoted, e.g. `'Work Sans'`.

For this example you can see the JSON file (also in this repository) hosted at Netlify at: https://lovely-peony-e39f59.netlify.app/design-tokens.json

## How do designers use these values in Figma?

The [Figma Tokens plugin](https://tokens.studio/) allows you to use your design tokens in [Figma](https://www.figma.com/), syncing to your JSON design tokens file. Some hosting methods are read-only, while others allow read and write access. 

Your design assets (colours, typography) are displayed in the plug-in and act like any other palette, so (for example) you can select a shape and give it a colour from your design tokens.

If changes are made to the design tokens file, designers will currently need to manually sync the Figma Tokens plugin with the current version of the JSON file, then choose ‘Update’ for changes to show in their Figma document.

## How do developers get access to these values in SASS?

To use your design tokens in SASS, the JSON file first needs to be converted to [a SASS Map](https://sass-lang.com/documentation/values/maps) (a multi-dimensional array) and then `@use`d in your main SASS file. 

For this example workflow, [converting the JSON to SASS uses `json-to-scss`](https://www.npmjs.com/package/json-to-scss). Variables created in the resulting `.scss` file can then be referenced using `map.get` (remember to [initialise using maps in SASS with `@use "sass:map"`](https://sass-lang.com/documentation/at-rules/use)). To access a value, reference the full list of keys that lead to the value, for example: 

```json
{
  "global": {
    "red": {
      "value": "#aa3300",
      "type": "color"
    }
  }
}
```

```sass
// SASS generated from the JSON
$design-tokens (
  global: (
      red: (
       value: #aa3300,
        type: color
    )
  )
)
```

```sass
// SASS file for your CSS
p {
color: map.get(
  design-tokens.$design-tokens,
  global, 
  red, 
  value
  );
}

// where design-tokens is the name of the included file
// $design-tokens is the name of the variable holding the map
```
