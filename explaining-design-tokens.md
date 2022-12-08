# What are design tokens?

Design tokens are JSON files that can be consumed by different parts of an overall design and development process so design choices (colours, typefaces) can be kept in sync.

The design tokens file should be stored in a version controlled environment that can be accessed online. Ensure the CORS setting for the file allows connections from outside the domain.

For typefaces the name has to match exactly, including the case, so `Georgia` not `georgia`. Font names with spaces should be quoted, e.g. `'Work Sans'`.

Test JSON file: https://lovely-peony-e39f59.netlify.app/design-tokens.json

## In Figma

The Figma Tokens plugin allows you to use your design tokens in Figma, syncing to your design tokens file. Some hosting methods are read-only, while others allow read and write access. Your design settings are displayed in the plug-in and act like any other palette, so (for example) you can select a shape and use one of the colours from your design tokens.

If you make a change to your design tokens file, you will need to manually sync the Figma Tokens plugin to update your Figma document, then choose 'Update' for changes to show.

## In SASS

To use your design tokens in SASS, the JSON file first needs to be converted to a SASS Map (a multi-dimensional array) and then `@use`d in your main SASS file. 

Converting the JSON to SASS uses `json-to-scss`. Variables created in the resulting `.scss` file can then be referenced using `map.get` (remember to initialise using maps in SASS with `@use "sass:map"`). To access a value, reference the full list of keys that lead to the value, for example: 

```json
// JSON
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
