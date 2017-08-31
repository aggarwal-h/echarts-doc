
{{target: rich-text}}

# Rich Text

Rich text can be used in labels of series, axis or other components. For example:

~[800x400](${galleryViewPath}pie-rich-text&edit=1&reset=1)

~[800x550](${galleryViewPath}treemap-obama&edit=1&reset=1)

~[800x400](${galleryViewPath}bar-rich-text&edit=1&reset=1)

<br>

More examples:
[Map Labels](${galleryEditorPath}map-labels&edit=1&reset=1),
[Pie Labels](${galleryEditorPath}pie-nest&edit=1&reset=1),
[Gauge](${galleryEditorPath}gauge-car&edit=1&reset=1).


<br>

Before v3.7, the style options was only able to applied to the whole label text block, and only color and font can be configured, which restricted the expressability of text descriptions.

Since v3.7, rich text has been supported:

+ Box styles (background, border, shadow, etc.), rotation, position of a text block can be specified.
+ Styles (color, font, width/height, background, shadow, etc.) and alignment can be customzied on fregments of text.
+ Image can be used in text as icon or background.
+ Combine these configurations, some special effects can be made, such as simple table, horizontal rule (hr).


At the begining, the meanings of two terms that will be used below should be clarified:
+ Text Block: The whole block of label text.
+ Text Fregment: Some piece of text in a text block.

For example:
~[340x240](${galleryViewPath}doc-example/text-block-fregment&edit=1&reset=1)


<br>

---

<br>


**Options about Text**

echarts provides plenty of text options, including:

+ Basic font style: `fontStyle`, `fontWeight`, `fontSize`, `fontFamily`.
+ Fill of text: `color`.
+ Stroke of text: `textBorderColor`, `textBorderWidth`.
+ Shadow of text: `textShadowColor`, `textShadowBlur`, `textShadowOffsetX`, `textShadowOffsetY`.
+ Box size of text block or text fregment: `lineHeight`, `width`, `height`, `padding`.
+ Alignment of text block or text fregment: `align`, `verticalAlign`.
+ Border, background (color or image) of text block or text fregment: `backgroundColor`, `borderColor`, `borderWidth`, `borderRadius`.
+ Shadow of text block or text fregment: `shadowColor`, `shadowBlur`, `shadowOffsetX`, `shadowOffsetY`.
+ Position and rotation of text block: `position`, `distance`, `rotate`.


User can defined styles for text fregment in `rich` property. For example, [series-bar.label.normal.rich](option.html#series-bar.label.normal.rich)

For example:

```js
label: {
    normal: {

        // Styles defined in 'rich' can be applied to some fregments
        // of text by adding some markers to those fregment, like
        // `{styleName|text content text content}`.
        // `'\n'` is the newline character.
        formatter: [
            '{a|Style "a" is applied to this fregment}'
            '{b|Style "b" is applied to this fregment}This fregment use default style{x|use style "x"}'
        ].join('\n'),

        // Styles for the whole text block are defined here:
        color: '#333',
        fontSize: 5,
        fontFamily: 'Arial',
        borderWidth: 3,
        backgroundColor: '#984455',
        padding: [3, 10, 10, 5],
        lineHeight: 20,

        // Styles for text fregments are defined here:
        rich: {
            a: {
                color: 'red',
                lineHeight: 10
            },
            b: {
                backgroundColor: {
                    image: 'xxx/xxx.jpg'
                },
                height: 40
            },
            x: {
                fontSize: 18,
                fontFamily: 'Microsoft YaHei',
                borderColor: '#449933',
                borderRadius: 4
            },
            ...
        }
    }
}
```

> Notice: `width` 和 `height` only work when `rich` specified.



<br>

---

<br>

**Basic Styles of Text, Text Block and Text Fragment**

Basic font style can be set to text: `fontStyle`, `fontWeight`, `fontSize`, `fontFamily`.

Fill color and stroke color can be set to text: `color`, `textBorderColor`, `textBorderWidth`.

Border style and background style can be set to text block: `borderColor`, `borderWidth`, `backgroundColor`, `padding`.

Border style and background style can be set to text fregment too: `borderColor`, `borderWidth`, `backgroundColor`, `padding`.

For example:
~[700x300](${galleryViewPath}doc-example/text-options&edit=1&reset=1)



<br>

---

<br>

**Label Position**

`label` option can be use in charts like `bar`, `line`, `scatter`, etc. The position of a label, can be specified by [label[normal|emphasis].position](option.html#series-scatter.label.normal.position)、[label[normal|emphasis].distance](option.html#series-scatter.label.normal.distance).

For example:
~[800x400](${galleryViewPath}doc-example/label-position&edit=1&reset=1)

> Notice, there are different optional values of `position` by different chart types. And `distance` is not supported in every chart. More detailed info can be checked in [option doc](option.html).



<br>

---

<br>

**Label Rotation**

Sometimes label is needed to be rotated. For example:

~[900x500](${galleryViewPath}bar-label-rotation&edit=1&reset=1)

[align](option.html#series-bar.label.normal.align) and[verticalAlign](option.html#series-bar.label.normal.verticalAlign) can be used to adjust location of label in this scenario.

Notice, `align` and `verticalAlign` are applied firstly, then rotate.

<br>

---

<br>

**Layout and Alignment of Text Fregment**

To understand the layout rule, every text fregment can be imagined as a `inline-block` dom element in CSS.

`content box size` of a text fregment is determined by its font size by default. It can also be specified directly by `width` and `height`, although they are rarely set. `border box size` of a text fregment is calculated by adding the `border box size` and `padding`.

Only `'\n'` is the newline character, which breaks a line.

Multiple text fregment exist in a single line. The height of a line is determined by the biggest `lineHeight` of text fregments. `lineHeight` of a text fregment can be specified in `rich`, or in the parent level of `rich`, otherwise using `box size` of the text fregment.

Having `lineHeight` determined, the vertical position of text fregments can be determined by `verticalAlign` (there is a little different from the rule in CSS):

+ `'bottom'`: The bottom edge of the text fregment sticks to the bottom edge of the line.
+ `'top'`: The top edge of the text fregment sticks to the top edge of the line.
+ `'middle'`: In the middle of the line.

The width of a text block can be specified by `width`, otherwise, by the longest line. Having the width determined, text fregment can be placed in each line, where the horizontal position of text fregments can be determined by its `align`.

+ Firstly, place text fregments whose `align` is `'left'` from left to right continuously.
+ Secondly, place text fregments whose `align` is `'right'` from right to left continuously.
+ Finally, the text fregments remained will be sticked and placed in the center of the rest of space.

The position of text in a text fregment:
+ If `align` is `'center'`, text aligns at the center of the text fregment box.
+ If `align` is `'left'`, text aligns at the left of the text fregment box.
+ If `align` is `'right'`, text aligns at the right of the text fregment box.

For example:

~[800x220](${galleryViewPath}doc-example/text-fregment-align&edit=1&reset=1)



<br>

---

<br>

**Effects: Icon, Horizontal Rule, Title Block, Simple Table**


See example:

~[600x400](${galleryViewPath}doc-example/title-block&edit=1&reset=1)

Icon is implemented by using image in `backgroundColor`.

```js
rich: {
    Sunny: {
        backgroundColor: {
            image: './data/asset/img/weather/sunny_128.png'
        },
        // Can only height specified, but leave width auto obtained
        // from the image, where the aspect ratio kept.
        height: 30
    }
}
```

Horizontal rule (like HTML &lt;hr&gt; tag) can be implemented by border:

```js
rich: {
    hr: {
        borderColor: '#777',
        // width is set as '100%' to fullfill the text block.
        // Notice, the percentage is based on the content box, without
        // padding. Although it is a little different from CSS rule,
        // it is convinent in most cases.
        width: '100%',
        borderWidth: 0.5,
        height: 0
    }
}
```

Title block can be implemented by `backgroundColor`:

```js
// Title is at left.
formatter: '{titleBg|Left Title}',
rich: {
    titleBg: {
        backgroundColor: '#000',
        height: 30,
        borderRadius: [5, 5, 0, 0],
        padding: [0, 10, 0, 10],
        width: '100%',
        color: '#eee'
    }
}

// Title is in the center of the line.
// This implementation is a little tricky, but is works
// without more complicated layout mechanism involved.
formatter: '{tc|Center Title}{titleBg|}',
rich: {
    titleBg: {
        align: 'right',
        backgroundColor: '#000',
        height: 30,
        borderRadius: [5, 5, 0, 0],
        padding: [0, 10, 0, 10],
        width: '100%',
        color: '#eee'
    }
}
```

Simple table can be implemented by specify the same width to text fregments that are in the same column of different lines. See the [example](${galleryViewPath}pie-rich-text&edit=1&reset=1) at the mentioned above.


