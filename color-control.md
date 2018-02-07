# Color control

This example shows how to implement a background color control, but the process is the same for any other color implementation.

## Required parts

```
const {
  InspectorControls,
  ColorPalette,
} = wp.blocks;

const {
  PanelColor
} = wp.components;
```

## Attributes

```
attributes: {
  backgroundColor: {
    type: 'string'
  }
}
```

## Edit props

First you need to handle the updating of the color. This sits directly in the edit part of the block.

```
const onChangeBackgroundColor = value => {
  props.setAttributes( { backgroundColor: value } );
};
```
Secondly you need to render the actual color control.

*Note: this needs to be wrapped in a focus conditional check.*

```
{
  !! props.focus && (
    <InspectorControls key="inspector">
      <PanelColor title={ __( 'Background Color' ) } colorValue={ props.attributes.backgroundColor }>
        <ColorPalette
          value={ props.attributes.backgroundColor }
          onChange={ onChangeBackgroundColor }
        />
      </PanelColor>
    </InspectorControls>
  )
}
```

Lets assume you want to apply the color directly to the `div` as a `style` tag.

```
<div
  className={props.className}
  style={{
    backgroundColor: props.attributes.backgroundColor
  }}
>
```

## Save props

You also need to render the style on the save prop. In this example it uses the same code on the front end, but this may not always be the case.

```
<div
  className={props.className}
  style={{
    backgroundColor: props.attributes.backgroundColor
  }}
>
```

## Full example

```
const { __ } = wp.i18n;
const {
  registerBlockType,
  InspectorControls,
  ColorPalette,
} = wp.blocks;

const {
    PanelColor
} = wp.components;

registerBlockType('example/color-control', {
  title: __('Example color control'),
  category: 'common',
  icon: 'shield',
  keywords: [__('color control')],
  attributes: {
    backgroundColor: {
      type: 'string'
    }
  },

  edit: props => {

    const onChangeBackgroundColor = value => {
      props.setAttributes( { backgroundColor: value } );
    };

    return (
      <div
        className={props.className}
        style={{
          backgroundColor: props.attributes.backgroundColor
        }}
      >
        {
          !! props.focus && (
            <InspectorControls key="inspector">
              <PanelColor title={ __( 'Background Color' ) } colorValue={ props.attributes.backgroundColor }>
                <ColorPalette
                  value={ props.attributes.backgroundColor }
                  onChange={ onChangeBackgroundColor }
                />
              </PanelColor>
            </InspectorControls>
          )
        }

        <p>My content</p>
      </div>
    );
  },
  save: props => {
    return (
      <div
        className={props.className}
        style={{
          backgroundColor: props.attributes.backgroundColor
        }}
      >
        <p>My content</p>
      </div>
    );
  }
});
```