# Victory Changelog

## 33.1.6 (2019-12-01)

-[#1450](https://github.com/FormidableLabs/victory/pull/1450) - Skips `stringMap` computation when data is preformatted (_i.e._ data uses has `_x` and `_y` values and accessors). Thanks @na9da!

## 33.1.5 (2019-11-26)

-[#1443](https://github.com/FormidableLabs/victory/pull/1443) - Adds a `mouseMoveThreshold` prop for `VictoryBrushContainer` to prevent accidental redraws. Thanks @Hypnosphi!
-[#1448](https://github.com/FormidableLabs/victory/pull/1448) - Removes an unnecessary check for svg target in `VictoryBrushContainer` `onMouseLeave`

## 33.1.4 (2019-11-25)

-[#1447](https://github.com/FormidableLabs/victory/pull/1447) - Adds support for function values for the `labelPosition` prop on `VictoryPie`

## 33.1.3 (2019-11-08)

-[#1434](https://github.com/FormidableLabs/victory/pull/1434) Ensure the correct handles are used in `VictoryBrushArea` when axes are inverted. Thanks @Hypnosphi!
-[#1435](https://github.com/FormidableLabs/victory/pull/1435) Adds a "move" option for the `defaultBrushArea` prop in `VictoryBrushContainer`. When this option is selected, clicking outside of the brush area will pan the brush to a the area the user clicked without resizing the brush. Thanks @Hypnosphi!

## 33.1.2 (2019-10-31)

-[#1425](https://github.com/FormidableLabs/victory/pull/1425) Corrects padding on zero value tooltips.
-[#1423](https://github.com/FormidableLabs/victory/pull/1423) Make sure `VictoryChart` does not override axis orientations defined in themes.
-[#1422](https://github.com/FormidableLabs/victory/pull/1422) Use the correct domain in the `onBrushEnd` domain callback.
-[#1420](https://github.com/FormidableLabs/victory/pull/1420) Fixes behavior with `allowZoom` prop. Thanks @Hypnosphi!
-

## 33.1.1 (2019-10-08)

-[#1409](https://github.com/FormidableLabs/victory/pull/1409) Updates `delaunay-find` to `0.0.4` to correct incorrect nearest values for sets of points with collinear values. Corrects bugs in `VictoryVoronoiContainer`

## 33.1.0 (2019-09-25)

- [#1404](https://github.com/FormidableLabs/victory/pull/1404) Adds a11y improvements
  - corrects behavior of VictoryContainer so that it only adds aria-labelledby and aria-describedby attributes when there are actually title and / or desc elements that are rendered (controlled by the title and desc props on VictoryContainer
  - adds a tabIndex prop to all primitive components that Victory renders (i.e. VictoryLabel, Bar etc). This prop may be given as a number or a function of other props
  - adds a desc prop to all primitive components. This prop may be given as a number or a function of other props

## 33.0.6 (2019-09-20)

- [#1401](https://github.com/FormidableLabs/victory/pull/1401) - Fixes a bug affecting automatic candle coloring

- [#1402](https://github.com/FormidableLabs/victory/pull/1402) - Fixes a bug affecting user-provided `containerRef` callbacks


## 33.0.5 (2019-08-29)

- [#1387](https://github.com/FormidableLabs/victory/pull/1387) - Add support for custom functions for the `interpolation` prop. Thanks @sanniassin!

## 33.0.4 (2019-08-27)

- [1384](https://github.com/FormidableLabs/victory/pull/1384) - Fixes a bug affecting functional `cornerRadius` on `VictoryTooltip`

- [1385](https://github.com/FormidableLabs/victory/pull/1385) - Adds `pointerOrientation` prop on `VictoryTooltip` to make it possible to control which side of the tooltip the pointer extends from independent of which side of the data point the entire flyout is oriented towards. This prop only became sensible with the addition of `center` and `centerOffset` props added in `victory@33.0.0`

## 33.0.3 (2019-08-25)

-[#1379](https://github.com/FormidableLabs/victory/pull/1379) - Fixes a bug affecting functional `padAngle` prop in `VictoryPie`

## 33.0.2 (2019-08-25)

-[#1378](https://github.com/FormidableLabs/victory/pull/1378) - Fixes a bug affecting function labels in `VictoryPie`

## 33.0.1 (2019-08-22)

- [#1375](https://github.com/FormidableLabs/victory/pull/1375) - Evaluates / calculates some props early so they are available when functional props are being evaluated. Most notably, `text` is evaluated before any other functional props for `VictoryLabel` so it may be used to determine things like style, etc.

## 33.0.0 (2018-08-21)

### Breaking Changes

**Changes for functional props and styles:**

Related PR: [#1360](https://github.com/FormidableLabs/victory/pull/1360)

Functional props like `labels` and functional styles will now be called with a single argument instead of `datum` and `active`. The argument passed to functional props and styles will be an object containing all the props that control the rendering of the the target the prop applies to. Including things like `datum`, `active`, `index`, `data`, `scale`, etc. We hope this will give users a lot more flexibility and control. In most cases, this change should be very straightforward to apply

old:
```
labels={(d) => `x: ${d.x}`}
```
new
```
labels={({ datum }) => `x: ${d.x}`}
```

Gotchas:
- Some of the props passed into functional props and styles may themselves be functions. These will _not_ be evaluated, because we have no way to determine evaluation order. So, if you create a `cornerRadius` function that depends on `barWidth`, do not also make `barWidth` a function of some other prop.
- A few props that take functions do not follow this pattern. These include data accessor functions like `y` and `x`, and `tickFormat`. The arguments for these props have not changed.


**Changes for `VictoryCandlestick` labels**

Related PR: [#1295](https://github.com/FormidableLabs/victory/pull/1295)

`VictoryCandlestick` now has granular support for labels corresponding to each portion of the candle. The current `labels` and `labelComponent` props will be joined by new props corresponding to each part of the candle.
*New props*
  `lowLabels`
  `lowLabelComponent`
  `highLabels`
  `highLabelComponent`
  `openLabels`
  `openLabelComponent`
  `closeLabels`
  `closeLabelComponent`

This will be a breaking change affecting the positioning of the default `label`. In earlier versions, the default label was positioned above the candle, it will now be positioned next to the center of the candle.
To use older label positioning, use `highLabels` / `highLabelComponent` rather than `label` / `labelComponent`. If you are using tooltips with `VictoryCandlestick`, you will need to register a custom event to trigger your `highLabels` tooltip:

example:
```
<VictoryCandlestick
  highLabels={({ datum }) => datum.high}
  highLabelComponent={<VictoryTooltip />}
  events={[{
    target: "data",
    eventHandlers: {
      onMouseOver: () => ({ target: "highLabels", mutation: () => ({ active: true }) }),
      onMouseOut: () => ({ target: "highLabels", mutation: () => ({ active: false }) })
    }
  }]}
/>
```

The `style` prop for `VictoryCandlestick` now also has namespaces for the new labels in addition to the current `labels` namespace. When both `labels` and specific label styles (_e.g._ `highLabels`) are provided, the styles will be merged

**Changes for `VictoryVoronoiContainer`**

Related PR: [#1371](https://github.com/FormidableLabs/victory/pull/1371)

Before this version `VictoryVoronoiContainer` had limited functionality for mouse-following tooltips, and for constraining a tooltip to the chart area, but it was only usable for multi-point tooltips (with `voronoiDimension`), and was not user configurable. This version aims to correct these limitations:

- `mouseFollowTooltips`: This new boolean prop on `VictoryVoronoiContainer` determines whether the labels should follow the mouse position or snap into place. (Note that in charts using `voronoiDimension`, the tooltip still follows the mouse in the non-`voronoiDimension`, as demonstrated in the charts below (both with `voronoiDimension="x"`)

`mouseFollowLabels={true}`
![mouseFollowLabels](https://user-images.githubusercontent.com/3719995/63392113-236ff400-c36a-11e9-91d0-64e674f481d5.gif)


`mouseFollowTooltips={false}`
![non-mouseFollowTooltips](https://user-images.githubusercontent.com/3719995/63392116-2834a800-c36a-11e9-8a73-a951b131ae2f.gif)

- constrained tooltips: multi-point tooltips rendered by `VictoryVoronoiContainer` will no longer be constrained to the chart area by default. Instead, add the `constrainToVisibleArea` prop to `VictoryTooltip` to enable this behavior for both multi-point and single point tooltips:
example:
```
containerComponent={
  <VictoryVoronoiContainer
    labelComponent={<VictoryTooltip constrainToVisibleArea />}
  />
}
```

**Changes for `VictoryTooltip` and `VictoryLabel`**

Related PR:[#1371](https://github.com/FormidableLabs/victory/pull/1371)

The changes we wanted to make to support new behaviors in `VictoryVoronoiContainer` required some changes to `VictoryTooltip` and `VictoryLabel`

New props for `VictoryTooltip`:

- `constrainToVisibleArea` is a boolean prop that, when true, will alter the position of the tooltip so that it exactly fits within the svg Victory renders. The tooltip's center will be moved, but the pointer will remain pointing at the associated `x`, `y` value of the tooltip. When this prop is set to true, `pointerLength` may not be respected

- `center` is a prop that may be given as an object with values or functions for "x" and "y". When this prop is set, it will position the center of the tooltip (centered around the main body of the tooltip, minus the pointer). When this prop is not set, it will be calculated from other props such as `x`, `y`, `pointerLength`, etc. This prop was added to enable mouse-following tooltips in `VictoryVoronoiContainer`.

- `centerOffset` is a prop that may be given as an object with values or functions for "x" and "y". When this prop is set, the center of the tooltip will be offset by some amount from the x, y value it points to, resulting in a slanted pointer. When this prop is set, `pointerLength` will not be respected (because the pointer will be slanted)

- `flyoutHeight` (formerly `height`): This optional prop determines the height of the tooltip flyout (minus pointer). The name of this prop was changed so that it would not conflict with the `height` prop now passed to `VictoryTooltip` by its parents

- `flyoutWidth` (formerly `width`): This optional prop determines the width of the tooltip flyout (minus pointer). The name of this prop was changed so that it would not conflict with the `width` prop now passed to `VictoryTooltip` by its parents

- `width`: the overall width of the parent svg. This prop will be passed down from any victory component that uses `VictoryTooltip` as a label

- `height`: the overall height of the parent svg. This prop will be passed down from any victory component that uses `VictoryTooltip` as a label

**Changes Affecting `VictoryLabel` and `VictoryTooltip`**

- The `x` and `y` values passed to labels by their parent components have all been adjusted so that their values match the position of the data point they correspond to. All padding is now accounted for in the `dx` and `dy` props instead of being added directly to `x` and `y`.
This will be a breaking change for anyone who is wrapping label components and relying on the `x` and `y` props they receive, or providing their own `dx` / `dy` props. These breaking changes may take a bit of manual adjustment to correct, but we hope this change will make label positioning easier to reason about in the long run.


**Other Changes**
We have been concurrently working on improving performance and the stability of events in `victory-native`. The following changes have been added to support these efforts:

Related PR: [#1373](https://github.com/FormidableLabs/victory/pull/1373)

- A `prependDefaultAxes` boolean prop has been added to `VictoryChart`. This prop will be set true by default in `victory-native` to reduce the possibility of axis elements to interfere with events.
- Invisible ticks and grids will no longer be rendered unless they have events attached to them. This is again to reduce interference with events.

Related PR: [#1365](https://github.com/FormidableLabs/victory/pull/1365)

- Swapped out React component primitives (`Bar`, `Path` etc) with for function primitives to match performance-improving changes in `victory-native`. (This is a breaking change for anyone extending from `victory` components)


## 32.3.7 (2018-08-19)

[#1368](https://github.com/FormidableLabs/victory/pull/1368) Ensures that animations finish for unmounting components. Thanks @fbarbat!

## 32.3.6 (2019-08-08)

[#1363](https://github.com/FormidableLabs/victory/pull/1363) Fix regression affecting `createContainer`. Thanks @stefanscript

## 32.3.5 (2019-08-07)

[#1362](https://github.com/FormidableLabs/victory/pull/1362) Fix scroll wheel interaction in newer versions of Chrome.

## 32.3.4 (2019-08-05)

[#1361](https://github.com/FormidableLabs/victory/pull/1361) Updates to `lodash^4.17.15`. Thanks @tomciopp

## 32.3.3 (2019-07-02)

[#1346](https://github.com/FormidableLabs/victory/pull/1346) - Fixes a bug which was preventing `VictoryVoronoiContainer` from activating all coincident points when no `voronoiDimension` was set

## 32.3.2 (2019-07-02)

[#1345](https://github.com/FormidableLabs/victory/pull/1345) - Uses updated `delaunay-find` instead of `d3-voronoi` in `VictoryVoronoiContainer`

## 32.3.1 (2019-06-28)

[#1344](https://github.com/FormidableLabs/victory/pull/1344) - update `lodash`
[#1343](https://github.com/FormidableLabs/victory/pull/1343) - revert to `d3-voronoi`

## 32.3.0 (2019-06-14)

[#1322](https://github.com/FormidableLabs/victory/pull/1322) - Replaces `d3-voronoi` with `d3-delaunay` in `VictoryVoronoiContainer`

[#1333](https://github.com/FormidableLabs/victory/pull/1333) - replace `d3-delaunay` with `delaunay-find`

## 32.2.3 (2019-05-13)

[#1306](https://github.com/FormidableLabs/victory/pull/1306) - Calls `eventKey` functions with both `datum` and `index`

## 32.2.2 (2019-05-03)

[#1304](https://github.com/FormidableLabs/victory/pull/1304) - renames private variables to reduce potential for conflict

## 32.2.1 (2019-05-02)

[#1302](https://github.com/FormidableLabs/victory/pull/1302) - Fixes slight vertical offset for elements rendered in `VictoryPortal`

[#1299](https://github.com/FormidableLabs/victory/pull/1299) - Support rendering array labels on separate lines. Previously `labels={() => {"one", "two"}}` would stringify the array rather than rendering it on two lines

## 32.2.0 (2019-04-10)

[#1292](https://github.com/FormidableLabs/victory/pull/1292) Reverses the render order for children of `VictoryStack`. This may cause minor visual changes for stacked charts, but should be an improvement for most users, as stokes and labels will no longer be cut off by higher stacks being rendered after lower stacks

[#1293](https://github.com/FormidableLabs/victory/pull/1293) Fixes a bug related to custom `labelComponent` rendering in `VictoryLegend`

## 32.1.0 (2019-03-18)

[#1278](https://github.com/FormidableLabs/victory/pull/1278) Adds regex support for the `voronoiBlacklist` prop. This prop may now be given as an array of strings or an array of regex patterns. Thanks @narinluangrath!

## 32.0.2 (2019-03-12)

[#1271](https://github.com/FormidableLabs/victory/pull/1271) Replace `PropTypes.exact` with `PropTypes.shape`

## 32.0.1 (2019-03-11)

[#1266](https://github.com/FormidableLabs/victory/pull/1266) Fixes area stroke on polar charts

## 32.0.0 (2019-02-27)

**Horizontal Chart Improvements!**

[#1258](https://github.com/FormidableLabs/victory/pull/1258)

The goal of this change is to make it possible to turn any existing chart into a horizontal chart by adding the `horizontal` prop to the chart without needing to alter any other props.

- supports horizontal versions of all chart types without needing to alter data
- supports all event containers for horizontal charts
- enforces consistency across props that take x and y values so that the `x` value always refers to the _independent_ dimension, and the `y` value always refers to the _dependent_ dimension.
- the orientation of `VictoryAxis` components is no longer tied to whether or not they are the `dependentAxis`

**Breaking Changes**

**Most Horizontal Charts**
The change in how props with x and y values are treated (i.e. `scale`, `domain`, etc) will be a breaking change for most horizontal charts. In most cases, reversing the `x` and `y` values of props you are providing to your horizontal chart will be sufficient to correct the change. For example:

```
<VictoryChart horizontal scale={{ x: "log" }} domain={{ y: [4, 9] }}>
  <VictoryBar
    data={[
      { x: 5, y: 0.1 },
      { x: 6, y: 1 },
      { x: 7, y: 10 },
      { x: 8, y: 100 }
    ]}
  />
</VictoryChart>
```
Should be changed to:
```
<VictoryChart horizontal scale={{ y: "log" }} domain={{ x: [4, 9] }}>
  <VictoryBar
    data={[
      { x: 5, y: 0.1 },
      { x: 6, y: 1 },
      { x: 7, y: 10 },
      { x: 8, y: 100 }
    ]}
  />
</VictoryChart>
```
Props affected by this change are: `scale`, `domain`, `maxDomain`, `minDomain`, `domainPadding`, and `categories`

**Horizontal Charts with Event Containers**
Dimension props such as `brushDimension` have changed so that they always refer to the dimension of the target variable (x for the independent variable, and y for the dependent variable). For example, a `VictoryBrushContainer` component with `brushDimension="x"` will move and expand only in the _independent_ dimension regardless of whether the chart is horizontal.

Props affected by this change are: `brushDimension`, `cursorDimension`, `selectionDimension`, `voronoiDimension`, and `zoomDimension`

**Horizontal Charts with Custom dataComponents**
The position values (i.e. `x`, `y`, `x0`, `y0`) supplied to custom `dataComponents` from components like `VictoryChart` will be scaled for the layout of the horizontal chart. Users who rely on these values may need to flip them or adjust them depending on their use case

**Horizontal VictoryBoxPlot**
Previously `VictoryBoxPlot` required data to be flipped (x values flipped with y values) in order to plot horizontal charts. This is no longer required, and providing data in this format will no longer work correctly. To correct for this change, it should be sufficient to flip the data used in horizontal charts

## 31.3.0 (2019-02-23)

- [1247](https://github.com/FormidableLabs/victory/pull/1247) Adds support for `labelOrientation` as an object for `VictoryBoxPlot`. Thanks @mAAdhaTTah

## 31.2.0 (2019-01-27)

**Axis improvements**

- [1244](https://github.com/FormidableLabs/victory/pull/1244) Supports the `axisValue` prop for both cartesian and polar charts. This prop allows users to position an axis relative to a value on the opposite axis. Values may be given as numbers, dates, or strings. This prop only works for axis components when they are nested within `VictoryChart`. Standalone axes can still be positioned using `offsetX` and offsetY` props.
- [1240](https://github.com/FormidableLabs/victory/pull/1240) Allows multiple independent axes in a single chart

**Removes all deprecated lifecycle methods**

- [1239](https://github.com/FormidableLabs/victory/pull/1239) Removes all `componentWillReceiveProps` lifecycle methods and adds `shouldComponentUpdate` logic for higher level components. Previously only the lowest level components performed `sCU` checks.
- [1228](https://github.com/FormidableLabs/victory/pull/1228) Replaces `componentWillMount` with `componentDidMount`

**bug fixes**

- [1243](https://github.com/FormidableLabs/victory/pull/1243) Prevents `VictoryBrushLine` active brushes from overflowing the brush area when a chart is zoomed
- [1241](https://github.com/FormidableLabs/victory/pull/1241) Fixes a regression effecting immutable data rendering
- [1227](https://github.com/FormidableLabs/victory/pull/1227) Fixes a minor regression effecting parent event keys introduced by [1211](https://github.com/FormidableLabs/victory/pull/1211)

## 31.1.0 (2019-01-08)

- [1222](https://github.com/FormidableLabs/victory/pull/1222) Bugfix: Avoid rendering null values on discrete data
- [1218](https://github.com/FormidableLabs/victory/pull/1218) Bugfix: Ensure that calculated domains respect custom baselines for VictoryBar and VictoryArea
- [1202](https://github.com/FormidableLabs/victory/pull/1202) Bugfix: Correct missing bar values
- [1208](https://github.com/FormidableLabs/victory/pull/1208) Performance: Improve performance in `addEventKeys`
- [1211](https://github.com/FormidableLabs/victory/pull/1211) Performance: Optimize stringMap calculations
- [1212](https://github.com/FormidableLabs/victory/pull/1212) Performance: Optimize for preformatted data:
  When data objects already contain `_x`, `_y` (and `_y0` where appropriate) and have data accessor props like `x="_x"`, data will no longer be formatted by Victory, but will be used as is.
  Example:
  ```
  <VictoryArea
    data={[ { _x: 0, _y0: 1, _y: 2 } ...]}
    x="_x"
    y="_y"
    y0="_y0"
  />
  ```
- [1209](https://github.com/FormidableLabs/victory/pull/1209) Feature: Adds an `animationWhitelist` property to the `animate` prop. This property should be given as an array of strings. When this prop is given, only the prop names matching the array will be animated, and all other props will be instantaneously updates.

  Example:

  ```
  <VictoryBar
    animate={{
      animationWhitelist: ["data"]
    }}
    data={this.state.data}
    style={{
      data: this.state.style
    }}
  />
  ```

  When this property is not set, the `static animationWhitelist` of any given component will be used.

## 31.0.2 (2018-12-12)

- [1200](https://github.com/FormidableLabs/victory/pull/1200) Export `defaultStyles` from `VictoryLabel`. Thanks @rudasoftware!

## 31.0.1 (2018-11-17)

- [1182](https://github.com/FormidableLabs/victory/pull/1182) Bugfix: fixes incorrect cornerRadius
- [1185](https://github.com/FormidableLabs/victory/pull/1185) Bugfix: corrects bug in zooming for horizontal charts

## 31.0.0 (2018-11-10)

- [#1177](https://github.com/FormidableLabs/victory/pull/1177) Adds support for controlling `radius`, `innerRadius`, `cornerRadius`, `padAngle`, `sliceStartAngle` and `sliceEndAngle` for each individual slice of a pie:

Details:
The `Slice` primitive used by `VictoryPie` now takes `radius`, `cornerRadius`, `innerRadius`, `padAngle`, `sliceStartAngle` and `sliceEndAngle` props. Each of these props may be given as number or a function of `datum` and `active`. `padAngle`, `radius`, `cornerRadius` and `innerRadius` will be passed down from props on `VictoryBar`, but `sliceStartAngle` and `sliceEndAngle` must be defined directly on the `Slice` instance, These values should be given in degrees. To make these values easier to use as functional props, `startAngle`, `endAngle`, and `padAngle` (in degrees) are added to each `datum` passed into `Slice`. (If your data already has these properties they will _not_ be overridden)

_Breaking Changes_
The `Slice` primitive will still take a `pathFunction` prop, but this prop will no longer be provided by `VictoryPie`. This will not be a breaking change for most users. This will only affect users who were wrapping the `Slice` component and making use of the `pathFunction` prop provided by `VictoryPie` Users who were providing their own `pathFunction` prop to `Slice` should not be effected.

## 30.6.1 (2018-11-09)

- [#1178](https://github.com/FormidableLabs/victory/pull/1178) Bugfix: single point charts with time scale data are appropriately centered
- [#1171](https://github.com/FormidableLabs/victory/pull/1171) Bugfix: `fixLabelOverlap` no longer causes errors with string `tickValues`

## 30.6.0 (2018-10-26)

- [#1152](https://github.com/FormidableLabs/victory/pull/1152) Bugfix: `barWidth` functions were not being correctly evaluated
- [#1158](https://github.com/FormidableLabs/victory/pull/1158) `cornerRadius` improvements for `VictoryBar`
  - Artifacts caused by `cornerRadius` values larger than the height of the bar have been corrected
  - `cornerRadius` now supports `topLeft`, `topRight`, `bottomLeft` and `bottomRight` values in addition to existing values. This is not a breaking change. These values may be used alongside existing values for `top` and `bottom`, but more specific values will override less specific values.

## 30.5.1 (2018-10-18)

- [#1149](https://github.com/FormidableLabs/victory/pull/1149) Adds `onTouchEnd` helper for `VictoryCursorContainer`

## 30.5.0 (2018-10-03)

- [#1135](https://github.com/FormidableLabs/victory/pull/1135) - Allow closed paths for cartesian and polar `VictoryLine`
- [#1130](https://github.com/FormidableLabs/victory/pull/1130) - Adds a `labelPosition` prop to `VictoryPie` with options `startAngle`, `endAngle` and `centroid` (default). Thanks @sikolio!
- [#1126](https://github.com/FormidableLabs/victory/pull/1126) - Ensures that `onBrushDomainChangeEnd` is called for dragging and panning actions. Thanks @jeloagnasin!

## 30.4.1 (2018-09-26)

- [#1127](https://github.com/FormidableLabs/victory/pull/1127) - Fixes a bug with cornerRadius when defined as a function
- [#1124](https://github.com/FormidableLabs/victory/pull/1124) - Changes cursors in `VictoryBrushContainer` when `allowResize` and `allowDrag` are set to false. Thanks @erick2014

## 30.4.0 (2018-09-24)

- [#1121](https://github.com/FormidableLabs/victory/pull/1121) - Adds a separate `onBrushDomainChangeEnd` event handler for `VictoryBrushContainer` that is only called on mouse up events. Thanks @jeloagnasin!

## 30.3.1 (2018-08-30)

- [#1104](https://github.com/FormidableLabs/victory/pull/1104) - Bugfix: correct sorting and stacking horizontal bars with categorical data

## 30.3.0 (2018-08-24)

- [#1088](https://github.com/FormidableLabs/victory/pull/1088) - Bugfix: don’t ignore angle: 0 for text styles
- [#1091](https://github.com/FormidableLabs/victory/pull/1091) - Fix inconsistent defaultBrushArea behavior. Thanks @bees
- [#1093](https://github.com/FormidableLabs/victory/pull/1093) - Remove trailing whitespace and incorrect zeroes in transform strings
- [#1094](https://github.com/FormidableLabs/victory/pull/1094) - Support direction prop for VictoryLabel and Text primitive
- [#1096](https://github.com/FormidableLabs/victory/pull/1096) - Fix bug in horizontal zooming and panning
- [#1101](https://github.com/FormidableLabs/victory/pull/1101) - Fix arguments in `VictoryVoronoiContainer` label function. Thanks @evsheino

## 30.2.0 (2018-08-06)

- [#1072](https://github.com/FormidableLabs/victory/pull/1072) Fixes a bug related to correctly stacking grouped components
- [#1074](https://github.com/FormidableLabs/victory/pull/1074) Fixes a bug that was causing time scale data to be ignored by `VictoryVoronoiContainer` when calculating voronoi layouts
- [#1076](https://github.com/FormidableLabs/victory/pull/1076) Implements a whitelist based on `static role` when calculating data and domain from child components.
- [#1077](https://github.com/FormidableLabs/victory/pull/1077) Prevents `VictoryZoonContainer` from downsampling stacked data
- [#1078](https://github.com/FormidableLabs/victory/pull/1078) Adds `barWidth` and `candleWidth` props to `VictoryBar` and `VictoryCandlestick`. Adds `candleRatio` prop to `VictoryCandlestick`
- [#1079](https://github.com/FormidableLabs/victory/pull/1079) Adds `onBrushCleared` callback prop for `VictoryBrushContainer`
- [#1080](https￼://github.com/FormidableLabs/victory/pull/1080) Changes how tooltips are deactivated so that multiple sources may reactivate tooltips (_i.e._ multiple triggers in `VictorySharedEvents` or direct triggers and voronoi triggers)
  [#1081](https://github.com/FormidableLabs/victory/pull/1081) Legends now render title and border when data is an empty array (previously nothing was rendered)

## 30.1.0 (2018-07-27)

-[1061](https://github.com/FormidableLabs/victory/pull/1061) Fixes default bar width for chart with only one bar. Thanks @40x -[1062](https://github.com/FormidableLabs/victory/pull/1062) Improvements for `victory-native` stability

- Supports `clipPath` prop on all primitive components
- generates keys based on `name` or `id` prop. -[1063](https://github.com/FormidableLabs/victory/pull/1063) Makes `prop-types` a real dependency

## 30.0.0 (2018-07-17)

Victory is becoming a monorepo!

This will not be a breaking change for the majority of users, especially those importing all components from the main `victory` npm package

**Breaking Changes**

- The `Axis` / `Grid` primitive component has been renamed `LineSegment`
- Victory no longer supports git installs
- `victory-chart` and `victory-core` packages export different sets of packages than they used to. See the complete list below

**New Package Organization**

- **`victory` exports everything exported from the packages below**
- `victory-axis@30.0.0` exports `VictoryAxis`
- `victory-area@30.0.0` exports `VictoryArea` and `Area`
- `victory-bar@30.0.0` exports `VictoryBar` and `Bar`
- `victory-box-plot@30.0.0` exports `VictoryBoxPlot`
- `victory-brush-container@30.0.0` exports `VictoryBrushContainer`, `BrushHelpers` and `brushContainerMixin`
- `victory-brush-line@30.0.0` exports `VictoryBrushLine`
- `victory-candlestick@30.0.0` exports `VictoryCandlestick` and `Candle`
- `victory-chart@30.0.0` exports `VictoryChart`
- `victory-core@30.0.0` still exports several packages that are used by several Victory components:
  - `VictoryAnimation`
  - `VictoryClipContainer`
  - `VictoryContainer`
  - `VictoryLabel`
  - `VictoryPortal` and `Portal`
  - `VictoryTheme`
  - `VictoryTransition`
  - Several primitive components:
    `Arc`, `Border` / `Box`, `Circle`, `ClipPath`, `LineSegment` (formerly `Axis` / `Grid`), `Line`, `Path`, `Point`, `Rect`, `Text`, `TSpan`, `Whisker`
  - Several utilities:
    - `addEvents`, `Axis`, `Collection`, `CommonProps`, `Data`, `DefaultTransitions`, `Domain`, `Events`, `Helpers`, `Immutable`, `LabelHelpers`, `Log`, `PropTypes`, `Scale`, `Selection`, `Style`, `TextSize`, `Timer`, `Transitions`, `Wrapper`
- `victory-create-container@30.0.0` exports `createContainer`, `combineContainerMixins` and `makeCreateContainerFunction`
- `victory-cursor-container@30.0.0` exports `VictoryCursorContainer`, `CursorHelpers` and `cursorContainerMixin`
- `victory-errorbar@30.0.0` exports `VictoryErrorBar` and `ErrorBar`
- `victory-group@30.0.0` exports `VictoryGroup`
- `victory-legend@30.0.0` exports `VictoryLegend`
- `victory-line@30.0.0` exports `VictoryLine` and `Curve`
- `victory-pie@30.0.0` exports `VictoryPie` and `Slice`
- `victory-scatter@30.0.0` exports `VictoryScatter`
- `victory-selection-container@30.0.0` exports `VictorySelectionContainer`, `SelectionHelpers` and `selectionContainerMixin`
- `victory-shared-events@30.0.0` exports `VictorySharedEvents`
- `victory-stack@30.0.0` exports `VictoryStack`
- `victory-tooltip@30.0.0` exports `VictoryTooltip` and `Flyout`
- `victory-voronoi@30.0.0` exports `VictoryVoronoi` and `Voronoi`
- `victory-voronoi-container@30.0.0` exports `VictoryVoronoiContainer`, `VoronoiHelpers` and `voronoiContainerMixin`
- `victory-zoom-container@30.0.0` exports `VictoryZoomContainer`, `RawZoomHelpers`, `ZoomHelpers` and `zoomContainerMixin`

## 0.27.2

VictoryPie -[182](https://github.com/FormidableLabs/victory-pie/pull/182) Adds optional `radius` and `origin` props to `VictoryPie`. The `radius` prop should be given as a single number. The `origin` prop should be given as an object with number values specified for "x" and "y". When these props are not given, radius and origin are determined by `width`, `height`, and `padding` as previously.

## 0.27.1 (2018-06-21)

VictoryCore -[390](https://github.com/FormidableLabs/victory-core/pull/390) _Breaking Change for other Victory packages_
This PR changes how the exported helper `reduceChildren` operates, and removes `getDomainFromGroupedData` -[392](https://github.com/FormidableLabs/victory-core/pull/392) Make sure transforms are applied to primitive components

VictoryChart -[555](https://github.com/FormidableLabs/victory-chart/pull/555) Refactors how `VictoryStack` and `VictoryGroup` interact with child data. Fixes bugs related to stacked and grouped charts in `VictoryVoronoiContainer`

## 0.27.0 (2018-06-05)

**Breaking Changes**

- Refactors utility methods. This is an internal breaking change, but should not be a breaking change for most Victory users. See [victory-core/380](https://github.com/FormidableLabs/victory-core/pull/380) for details
- Upgrades to `react-fast-compare@^2.0.0` which changes function comparison. This means that Victory components _will_ update when functions are not equal. This closes several Victory issues, but may cause a slight performance decline

**New Features**

- Adds `minDomain` and `maxDomain` props. These props may be used to set one edge of a domain while allowing the other edge to be determined by data or other props. `minDomain` and `maxDomain` override `domainPadding`.
- Adds `singleQuadrantDomainPadding` prop. This prop may be given as a boolean or an object with boolean values for x and y. When this prop is set to `false` for a given dimension, any `domainPadding` applied in that dimension will _not_ be constrained to existing quadrants.

## 0.26.1 (2018-05-17)

VictoryCore -[374](https://github.com/FormidableLabs/victory-core/pull/374) Consistent `PropTypes` for `clipId` -[373](https://github.com/FormidableLabs/victory-core/pull/373) Evaluate styles for polar bars -[372](https://github.com/FormidableLabs/victory-core/pull/372) Support top and bottom cornerRadius for bars. Support functional cornerRadius -[371](https://github.com/FormidableLabs/victory-core/pull/371) Evaluate Whisker styles -[370](https://github.com/FormidableLabs/victory-core/pull/370) Refactor to remove lifecycle methods

VictoryChart -[594](https://github.com/FormidableLabs/victory-chart/pull/594) Support functional `cornerRadius` and objects with `cornerRadius` defined for "top" and "bottom" -[593](https://github.com/FormidableLabs/victory-chart/pull/593) Add `defaultBrushArea` prop with supported options "all", "none" and "disable" -[591](https://github.com/FormidableLabs/victory-chart/pull/591) Ensure that `VictoryVoronoiContainer` works correctly with `VictoryGroup` data.

## 0.26.0 (2018-04-21)

**BREAKING CHANGES**

_Disable arbitrary styles from data_
This change deprecates Victory's ability to automatically pick up style attributes from the data object. This change will improve performance, but will be a breaking change for many users. Fortunately the upgrade path is simple:

If your data object looks like

```
data={[
  { x: 1, y: 1, fill: "red", opacity: 0.2 },
  ...
]}
```

Add the following functional styles:

```
style={{ data:  { fill: (d) => d.fill, opacity: (d) => d.opacity } }}
```

and everything will work as before.

_Limit Pre-calculating label props_
Base props for labels will no longer be pre-calculated unless a labels prop exists. This change improves performance, but it will be a breaking change for users who were using events for adding labels to elements that did not already have them using an event mutation like:

```
events={[{
  target: "data",
  eventHandlers: {
    onClick: () => {
      return [{ target: "labels", mutation: () => ({ text: "clicked" }) }];
    }
  }
}]}
```

If you are using this pattern, you can make labels work as expected by adding a dummy labels prop like: `labels={() => null}`

Note: This change _does not_ affect tooltips, which exist, but are invisible until they receive the `active` prop

**All Changes**
VictoryCore -[364](https://github.com/FormidableLabs/victory-core/pull/364) Perf: Remove style whitelist filter. -[369](https://github.com/FormidableLabs/victory-core/pull/369) Ensure state -[368](https://github.com/FormidableLabs/victory-core/pull/368) Audit lodash methods -[367](https://github.com/FormidableLabs/victory-core/pull/367) Simplify state filtering -[365](https://github.com/FormidableLabs/victory-core/pull/365) Perf: Return early when label content is null or undefined -[362](https://github.com/FormidableLabs/victory-core/pull/362) Perf: Filter falsey mutations from state

VictoryChart -[587](https://github.com/FormidableLabs/victory-chart/pull/587) Disable styles on data -[584](https://github.com/FormidableLabs/victory-chart/pull/584) Check for labels prop before computing baseProps for labels -[589](https://github.com/FormidableLabs/victory-chart/pull/589) Audit lodash methods -[583](https://github.com/FormidableLabs/victory-chart/pull/583) Perf improvement for `VictorySelectionContainer`

VictoryPie -[176](https://github.com/FormidableLabs/victory-pie/pull/176) Disable styles on data -[177](https://github.com/FormidableLabs/victory-pie/pull/177) Audit lodash methods

## 0.25.7 (2018-03-27)

VictoryCore -[343](https://github.com/FormidableLabs/victory-core/pull/343) Changes the render order of lines and areas in the `Area` primitive -[344](https://github.com/FormidableLabs/victory-core/pull/344) bug fix for `VictoryTransition` with fewer children -[334](https://github.com/FormidableLabs/victory-core/pull/334) Add `Whisker` primitive for `VictoryBoxPlot`

VictoryChart -[557](https://github.com/FormidableLabs/victory-chart/pull/557) `VictoryBoxPlot` -[575](https://github.com/FormidableLabs/victory-chart/pull/575) Stack datasets with differeing domains -[574](https://github.com/FormidableLabs/victory-chart/pull/574) Refactor helper method exports

VictoryPie -[168](https://github.com/FormidableLabs/victory-pie/pull/168) Refactor helper method exports

## 0.25.6 (2018-02-14)

VictoryChart -[573](https://github.com/FormidableLabs/victory-chart/pull/573) Use fallback styles in VictoryBrushLine

## 0.25.5 (2018-02-12)

VictoryCore -[339](https://github.com/FormidableLabs/victory-core/pull/339) Adds a "minus" option for `Point`

VictoryChart -[571](https://github.com/FormidableLabs/victory-chart/pull/571)

- Adds `selectionBlacklist` to `VictorySelectionContainer`
- Adds `activateData` and `activateLabels` to `VictoryVoronoiContainer` (true by default)
- Adds `activateSelectedData` to `VictorySelectionContainer` (true by default) -[572](https://github.com/FormidableLabs/victory-chart/pull/572) Changes behavior of `labels` in `VictoryVoronoiContainer`
- `labels` is now called with `point, index, points` instead of `point, active`. This will not be a breaking change for most users, as this function was only called when labels were `active` -[570](https://github.com/FormidableLabs/victory-chart/pull/570) Add "minus" option for `VictoryScatter` `symbol` prop -[569](https://github.com/FormidableLabs/victory-chart/pull/569) Fixes a bug in `createContainer` -[568](https://github.com/FormidableLabs/victory-chart/pull/568) Adds `brushAreaWidth` prop for `VictoryBrushLine` -[567](https://github.com/FormidableLabs/victory-chart/pull/567) Fixes brushArea active state in `VictoryBrushLine` -[565](https://github.com/FormidableLabs/victory-chart/pull/565) Prevent re-renders with disable prop

## 0.25.4 (2018-02-07)

VictoryCore -[341](https://github.com/FormidableLabs/victory-core/pull/341) Improve sCU logic for primitive components

VictoryChart -[563](https://github.com/FormidableLabs/victory-chart/pull/563) `stopPropagation` when panning or selecting `VictoryBrushLine`

-[565](https://github.com/FormidableLabs/victory-chart/pull/565) Adds `disable` prop to all interactive containers and addon components

## 0.25.3 (2018-02-06)

-[victory-chart/562](https://github.com/FormidableLabs/victory-chart/pull/562) Bugfix for `VictoryCursorContainer`

## 0.25.1 (2018-02-05)

-[918](https://github.com/FormidableLabs/victory/pull/918) Add `sideEffects: false

VictoryCore -[337](https://github.com/FormidableLabs/victory-core/pull/337) Add `sideEffects: false` -[338](https://github.com/FormidableLabs/victory-core/pull/338) Fix bar path bug in Firefox

VictoryChart -[560](https://github.com/FormidableLabs/victory-chart/pull/560) Add `sideEffects: false` -[561](https://github.com/FormidableLabs/victory-chart/pull/561) Bugfix for createContainer

VictoryPie -[167](https://github.com/FormidableLabs/victory-pie/pull/167) Add `sideEffects: false`

## 0.25.0 (2018-02-04)

**Major Features**

- `VictoryBrushLine` for multi-brush support

**Breaking Changes**

- `Candle` expects a new set of props from `VictoryCandlestick`
- The `Line` component has been renamed to `Axis` / `Grid`
- Internal methods for _all_ Victory primitive components have changed. This will be a breaking change for users who are extending primitive components, including `victory-native`.

VictoryCore -[336](https://github.com/FormidableLabs/victory-core/pull/336) Fixes key names for `ErrorBar` -[325](https://github.com/FormidableLabs/victory-core/pull/325) Adds a `getDimension` static method for `VictoryLegend` -[326](https://github.com/FormidableLabs/victory-core/pull/326) Adds a fallback prop for `VictoryLegend` `titleOrientation` -[327](https://github.com/FormidableLabs/victory-core/pull/327) Use `pointerEvents: "painted"` for grid styles -[328](https://github.com/FormidableLabs/victory-core/pull/328) Adds `inline` prop for `VictoryLabel` -[329](https://github.com/FormidableLabs/victory-core/pull/329) Bugfix `add-events` -[330](https://github.com/FormidableLabs/victory-core/pull/330) Adds `wickStyleWidth` prop for `Candle`
_This is a breaking change as it changes the expected props for `Candle`_ -[331](https://github.com/FormidableLabs/victory-core/pull/331) Bugfix portal rendering -[333](https://github.com/FormidableLabs/victory-core/pull/333) Whitelist style attributes -[335](https://github.com/FormidableLabs/victory-core/pull/335) Update primitives
_This is a breaking change for `victory-native` and anyone extending primitive components._
\*This is a breaking change for for the `Line` component. Renamed `Axis` / `Grid`

VictoryChart -[551](https://github.com/FormidableLabs/victory-chart/pull/551) Bux fixes and improvements for VictoryStack -[553](https://github.com/FormidableLabs/victory-chart/pull/553) Allow renderInPortal to be false for tooltips in `VictoryVoronoiContainer` -[554](https://github.com/FormidableLabs/victory-chart/pull/554) Add support for `wickStrokeWidth` in `VictoryCandlestick`
**This is a breaking change as the expected props for `Candle` are changed** -[556](https://github.com/FormidableLabs/victory-chart/pull/556) Fixes undefined context variable -[558](https://github.com/FormidableLabs/victory-chart/pull/558) Allow `func` PropType for `color` on `VictoryGroup` -[559](https://github.com/FormidableLabs/victory-chart/pull/559) Implement `VictoryBrushLine` and use updated primitive components
**This is a breaking change for anyone using the `Line` primitive. It has been renamed to `Axis` / `Grid`.
**This may be a breaking change for anyone who was _extending_ Victory primitives\*\*

VictoryPie -[166](https://github.com/FormidableLabs/victory-pie/pull/166) Update Victory primitives

## 0.24.4 (2018-01-08)

VictoryCore

- [324](https://github.com/FormidableLabs/victory-core/pull/324) Adds support for external event mutations
  - Adds `externalEventMutations` prop to `VictorySharedEvents` and all components enhanced with the `add-events` HOC
  - `externalEventMutations` prop format:
  ````js
  externalEventMutations: PropTypes.arrayOf(PropTypes.shape({
   callback: PropTypes.function,
   childName: PropTypes.oneOfType([
     PropTypes.string,
     PropTypes.array
   ]),
   eventKey: PropTypes.oneOfType([
     PropTypes.array,
     CustomPropTypes.allOfType([CustomPropTypes.integer, CustomPropTypes.nonNegative]),
     PropTypes.string
   ]),
   mutation: PropTypes.function,
   target: PropTypes.oneOfType([
     PropTypes.string,
     PropTypes.array
   ])
  }))
  ```
  *Note:* `eventKey` and `target` must be specified for externalEventMutations. When using `extenalEventMutations` with shared events (_i.e._ events on VictoryChart etc), `childName` is also required.
  ````

_Note:_ The `callback` supplied to `externalEventMutations` should be used for clearing mutations. This is crucial for animating charts

-[323](https://github.com/FormidableLabs/victory-core/pull/323) Add support for immutable data structures
_note: compatible with `immutable@v3.8.x` and will be compatible with `immutable@v4.0.x` when released_

VictoryChart

- [550](https://github.com/FormidableLabs/victory-chart/pull/550) Fixes a bug related to voronoi tooltip positioning
- [549](https://github.com/FormidableLabs/victory-chart/pull/549) Fixes a prop type warning for `categories` supplied to `VictoryAxis`
- [548](https://github.com/FormidableLabs/victory-chart/pull/548) Adds a `voronoiBlacklist` prop to `VictoryVoronoiContainer`
- [547](https://github.com/FormidableLabs/victory-chart/pull/547) Fixes downsampling in `VictoryZoomContainer` with function plotting
- [545](https://github.com/FormidableLabs/victory-chart/pull/545) Fixes a bug related to panning in `VictoryZoomContainer`
- [544](https://github.com/FormidableLabs/victory-chart/pull/544) Adds support for external event mutations
- [543](https://github.com/FormidableLabs/victory-chart/pull/543) Fixes a bug related to dates in `VictoryBrushContainer`
- [545](https://github.com/FormidableLabs/victory-chart/pull/545) Fixes a bug in `VictoryZoomContainer`
- [542](https://github.com/FormidableLabs/victory-chart/pull/542) Adds support for immutable data _note: compatible with `immutable@v3.8.x` and will be compatible with `immutable@v4.0.x` when released_

VictoryPie

- [164](https://github.com/FormidableLabs/victory-pie/pull/164) Correct typo
- [163](https://github.com/FormidableLabs/victory-pie/pull/163) Adds support for external event mutations

## 0.24.3 (2017-12-17)

VictoryCore

- [320](https://github.com/FormidableLabs/victory-core/pull/320) Adds `rowGutter` and support for asymmetric gutters for both `gutter` and `rowGutter` in `VictoryLegend`
- [322](https://github.com/FormidableLabs/victory-core/pull/322) Adds support for a `sortOrder` prop with values "ascending" or "descending"

VictoryChart

- [540](https://github.com/FormidableLabs/victory-chart/pull/540) Adds `allowSelection` boolean prop for `VictorySelectionContainer` (true by default)
- [541](https://github.com/FormidableLabs/victory-chart/pull/541) Implements `sortOrder` prop with "ascending" and "descending" options

VictoryPie

- [160](https://github.com/FormidableLabs/victory-pie/pull/160) Add `sortOrder` prop

## 0.24.2 (2017-11-14)

VictoryCore

- [316](https://github.com/FormidableLabs/victory-core/pull/316)
  - adds `cornerRadius` prop for `Bar`
  - adds `barRatio` prop for `Bar`
  - removes rounding from calculated paths
  - fixes a domain bug for negative bars and areas

VictoryChart

- [538](https://github.com/FormidableLabs/victory-chart/pull/538) Adds `barRatio` and `cornerRadius` props to `VictoryBar`

- [539](https://github.com/FormidableLabs/victory-chart/pull/539) Fixes a bug in bubble plots

## 0.24.1 (2017-11-08)

- Update infrastructure for React 16
- [victory-chart/534](https://github.com/FormidableLabs/victory-chart/pull/534) Add `allowResize` and `allowDrag` props for `VictoryBrushContainer`

## 0.24.0 (2017-10-19)

**Breaking Changes**

- [victory-chart/527](https://github.com/FormidableLabs/victory-chart/pull/527)
  - adds an `invertAxis` prop for `VictoryAxis` that will flip the domain of a given axis when true. Changing the `orientation` prop of a given axis will no longer flip the domain on that axis _unless_ the `invertAxis` prop is also set.
  - `tickFormat` as an array will set the number of ticks if `tickValues` are not given.
  - `tickValues` will be forced to a unique array. `tickFormat` may still have non-unique values.
  - `tickCount` will now always have an effect when set. Previously, this prop would do nothing when `tickValues` were provided. Now `tickCount` will downsample any array provided to either `tickValues` or `tickFormat`.

Other Changes

VictoryChart

- [529](https://github.com/FormidableLabs/victory-chart/pull/529) `VictoryChart` no longer calculates `tickValues` or `tickFormat` for axis children. `stringMap` and `categories` are passed to axis components instead.
- [528](https://github.com/FormidableLabs/victory-chart/pull/528) and [530](https://github.com/FormidableLabs/victory-chart/pull/530) Remove numeric keys from styles
- [526](https://github.com/FormidableLabs/victory-chart/pull/526) Always set animation state

VictoryCore

- [310](https://github.com/FormidableLabs/victory-core/pull/310) Add `getPath` prop for `Point` to support custom path calculation
- [311](https://github.com/FormidableLabs/victory-core/pull/311) Fix bug in `TextSize`
- [313](https://github.com/FormidableLabs/victory-core/pull/313) Use `tickFormat` for calculating a stringMap when appropriate
- [314](https://github.com/FormidableLabs/victory-core/pull/314) Theme changes: Change `pointerEvents` to "visible" for grids

## 0.23.1 (2017-10-04)

- [victory-core/307](https://github.com/FormidableLabs/victory-core/pull/307) Adds a default className for containers
- [victory-core/308](https://github.com/FormidableLabs/victory-core/pull/308) Fix borderWidth on error bars
- [victory-core/309](https://github.com/FormidableLabs/victory-core/pull/309) Fix non-responsive container styles, add container ref
- [victory-chart/524](https://github.com/FormidableLabs/victory-chart/pull/524) Fix naming changes for `VictoryCursorContainer`

## 0.23.0 (2017-09-30)

**BREAKING CHANGES:**

**[victory-core/299](https://github.com/FormidableLabs/victory-core/pull/299) Containers are now rendered in parent divs.** This may be a breaking change for parent styles.

**[victory-chart/518](https://github.com/FormidableLabs/victory-chart/pull/518) Naming changes for container props**

`VictoryBrushContainer`

- `dimension` -> `brushDimension`
- `selectionComponent` -> `brushComponent`
- `selectedDomain` -> `brushDomain`
- `selectionStyle` -> `brushStyle`
- `onDomainChange` -> `onBrushDomainChange`

`VictoryCursorContainer`

- `dimension` -> `cursorDimension`
- `onChange` -> `onCursorChange`

`VictorySelectionContainer`

- `dimension` -> `selectionDimension`

`VictoryVoronoiContainer`

- `dimension` -> `voronoiDimension`

`VictoryZoomContainer`

- `dimension` -> `zoomDimension`
- `onDomainChange` -> `onZoomDomainChange`

**Other Changes**

- [victory-core/300](https://github.com/FormidableLabs/victory-core/pull/300) Fixes path rendering for decimal values
- [victory-core/302](https://github.com/FormidableLabs/victory-core/pull/302) Adds an `alignment` prop for `<Bar/>` so that bars may be rendered with "start", "middle" (default), or "end" alignment relative to their value.
- [victory-core/304](https://github.com/FormidableLabs/victory-core/pull/304) `VictoryLabel` positioning is calculated from `datum` when `x` and `y` positioning props are not provided. This features supports data annotations
- [victory-core/305](https://github.com/FormidableLabs/victory-core/pull/305) Adds a `groupComponent` prop for `VictoryPortal`. This prop is used by `VictoryZoomContainer` so that children rendered within `VictoryPortal` may still be clipped when zooming

* [victory-chart/516](https://github.com/FormidableLabs/victory-chart/pull/516) Ensure that `VictoryZoomContainer` respects `clipId`
* [victory-chart/517](https://github.com/FormidableLabs/victory-chart/pull/517) `VictoryZoomContainer` and `VictoryVoronoiCOntainer` should ignore legend children
* [victory-chart/519](https://github.com/FormidableLabs/victory-chart/pull/519) Adds an `alignment` prop for `VictoryBar` so that bars may be rendered with "start", "middle" (default), or "end" alignment relative to their value.
* [victory-chart/520](https://github.com/FormidableLabs/victory-chart/pull/520) Adds an `allowPan` prop for `VictoryZoomContainer`. (Default true)
* [victory-chart/521](https://github.com/FormidableLabs/victory-chart/pull/521) Changes how children of `VictoryZoomContainer` are clipped to enable better zooming for `VictoryPortal`
* [victory-chart/522](https://github.com/FormidableLabs/victory-chart/pull/522) Fixes a bug in `VictoryZoomContainer` that effected time scale charts with `zoomDomain` specified

## 0.22.2 (2017-09-12)

- [victory-core/297](https://github.com/FormidableLabs/victory-core/pull/297) Adjusts automatic width in `VictoryLegend`

## 0.22.1 (2017-09-09)

- [victory-core/296](https://github.com/FormidableLabs/victory-core/pull/296) Fixes layout bug in `VictoryLegend`

## 0.22.0 (2017-09-09)

**BREAKING CHANGES:** Styling and layout for `VictoryTooltip` and `VictoryLegend` are impacted

- [victory-core/293](https://github.com/FormidableLabs/victory-core/pull/293) **This may be a breaking change**
  Removes the default theme from `VictoryTooltip`. See PR for additional details.
- [victory-core/294](https://github.com/FormidableLabs/victory-core/pull/294) **This may be a breaking change** Improvements for `VictoryLegend`
  - Adds a legend border that can be styled or replaced with a custom component
    - new props: `borderComponent={<Border/>}` and `borderPadding`
    - new style / event namespace: "border"
  - Adds new primitive component `Border` which renders a `rect` element
    - props: common props + `width`, `height`, `x` and `y`
  - Adds a legend title with supporting props for positioning and centering titles
    - new props: title, titleComponent={<VictoryLabel/>}, titleOrientation, centerTitle
    - new style / event namespace: "title"
- [victory-core/295](https://github.com/FormidableLabs/victory-core/pull/295) Fixes deprecation error for React 16

## 0.21.5 (2017-08-17)

VictoryCore

- [289](https://github.com/FormidableLabs/victory-core/pull/289) Bugfix: createContainer + containerId
- [287](https://github.com/FormidableLabs/victory-core/pull/287) Allow users to override `touchAction` style in containers
- [286](https://github.com/FormidableLabs/victory-core/pull/286) bugfix: tooltip positioning with `dx` and `dy`

VictoryChart

- [510](https://github.com/FormidableLabs/victory-chart/pull/510) Render axis line under other axis elements

VictoryPie

- [153](https://github.com/FormidableLabs/victory-pie/pull/153) Bugfix for asymmetric padding

## 0.21.4 (2017-08-07)

VictoryCore

- [285](https://github.com/FormidableLabs/victory-core/pull/285) bugfix: tooltips with `activateData`
- [278](https://github.com/FormidableLabs/victory-core/pull/278) Support touch events
- [280](https://github.com/FormidableLabs/victory-core/pull/280) Fix tooltip rendering in VictoryPortal
- [281](https://github.com/FormidableLabs/victory-core/pull/281) bugfix: functional label padding
- [282](https://github.com/FormidableLabs/victory-core/pull/282) bugfix: fix "unknown props on `<g>` tag" warning
- [283](https://github.com/FormidableLabs/victory-core/pull/283) Allow boolean value for animate prop
- [284](https://github.com/FormidableLabs/victory-core/pull/284) Sort arrays by "key" when animating. See [#684](https://github.com/FormidableLabs/victory/issues/684)
- [274](https://github.com/FormidableLabs/victory-core/pull/274) Fixes automatic bar width calculation for horizontal bars
- [275](https://github.com/FormidableLabs/victory-core/pull/275) Adds an optional `containerId` prop for all Victory containers
- [276](https://github.com/FormidableLabs/victory-core/pull/276) Adds `downsample` method for `Data`

VictoryChart

- [504](https://github.com/FormidableLabs/victory-chart/pull/504) bugfix: padding bug in `VictoryCursorContainer`
- [505](https://github.com/FormidableLabs/victory-chart/pull/505) Support touch events on all containers
- [506](https://github.com/FormidableLabs/victory-chart/pull/506) Add `props` argument for `VictoryVoronoiContainer` callbacks
- [507](https://github.com/FormidableLabs/victory-chart/pull/507) bugfix: Support array styles for `VictoryLabel` when it is used as a direct child of `VictoryChart` or other wrappers
- [508](https://github.com/FormidableLabs/victory-chart/pull/508) Add support for `animate` as a boolean prop
- [509](https://github.com/FormidableLabs/victory-chart/pull/509) Add `props` as the last argument for all container callbacks
- [502](https://github.com/FormidableLabs/victory-chart/pull/502) Fix bug in `VictoryZoomContainer`
- [503](https://github.com/FormidableLabs/victory-chart/pull/503) Add downsample option for `VictoryZoomContainer`
- [504](https://github.com/FormidableLabs/victory-chart/pull/504) Fix padding bug in `VictoryCursorContainer`

VictoryPie

- [151](https://github.com/FormidableLabs/victory-pie/pull/151) bugfix: support functional padding for labels
- [152](https://github.com/FormidableLabs/victory-pie/pull/152) Support boolean animate prop

## 0.21.3 (2017-07-24)

VictoryCore

- [267](https://github.com/FormidableLabs/victory-core/pull/267) Correct single-point domain logic
- [268](https://github.com/FormidableLabs/victory-core/pull/268) Correct stacked domain logic
- [269](https://github.com/FormidableLabs/victory-core/pull/269) `VictoryLabel` accepts percentage values for `x` and `y`
- [270](https://github.com/FormidableLabs/victory-core/pull/270) `VictoryLegend` supports events. Also enforces consistent parent styles as other Victory components. **Breaking style change for VictoryLegend**
- [272](https://github.com/FormidableLabs/victory-core/pull/272) Aggressive `shouldComponentUpdate` logic for evented Victory components **Breaking change for some components using `addEvents`**

VictoryChart

- [496](https://github.com/FormidableLabs/victory-chart/pull/496) `VictoryZoomContainer` improvements for real-time, updating data
- [497](https://github.com/FormidableLabs/victory-chart/pull/497) bugfix: apply `domainPadding` to explicit domains
- [498](https://github.com/FormidableLabs/victory-chart/pull/498) `VictoryAxis` determines its own default `tickFormat`
- [499](https://github.com/FormidableLabs/victory-chart/pull/499) Fix label prop merge order for `VictoryVoronoiContainer` labels
- [500](https://github.com/FormidableLabs/victory-chart/pull/500) Support performance improvements for evented components _e.g._ `VictoryArea`, `VictoryBar` ...

## 0.21.2 (2017-07-13)

Victory:

- [651](https://github.com/FormidableLabs/victory/pull/651) Support Webpack 3 and ES6 exports

VictoryCore:

- [266](https://github.com/FormidableLabs/victory-core/pull/266) Support Webpack 3 and ES6 exports
- [265](https://github.com/FormidableLabs/victory-core/pull/265) Translates `Slice` when an `origin` prop is given

VictoryChart:

- [495](https://github.com/FormidableLabs/victory-chart/pull/495) Support Webpack 3 and ES6 exports
- [494](https://github.com/FormidableLabs/victory-chart/pull/494) Ensure that `tickFormat` has access to `tick`, `index`, and `ticks`
- [491](https://github.com/FormidableLabs/victory-chart/pull/491) Corrects dependent axis for `innerRadius`
- [490](https://github.com/FormidableLabs/victory-chart/pull/490) Fixes a bug in domain calculation for polar axes
- [488](https://github.com/FormidableLabs/victory-chart/pull/488) `VictoryZoomContainer` only updates the domain in the dimension it controls
- [489](https://github.com/FormidableLabs/victory-chart/pull/489) Adds support for `innerRadius` on polar charts

VictoryPie:

- [149](https://github.com/FormidableLabs/victory-pie/pull/149) Support Webpack 3 and ES6 exports
- [148](https://github.com/FormidableLabs/victory-pie/pull/148) Translates individual slices rather than an entire group translation for pie and labels

## 0.21.1 (2017-06-29)

Minor bug fixes:

VictoryCore

- [victory-core/255](https://github.com/FormidableLabs/victory-core/pull/255) style typo
- [victory-core/257](https://github.com/FormidableLabs/victory-core/pull/257) single point domain calculation bug
- [victory-core/259](https://github.com/FormidableLabs/victory-core/pull/259) Area render bug
- [victory-core/260](https://github.com/FormidableLabs/victory-core/pull/260) Area render order
- [victory-core/515](https://github.com/FormidableLabs/victory/issues/515) Bugfix for ARIA title and desc. Now both have unique IDs.
- [victory-core/262](https://github.com/FormidableLabs/victory-core/pull/262) Consistent widths for polar bars. **This change may require style adjustments to maintain visual continuity**

VictoryChart

- [victory-chart/482](https://github.com/FormidableLabs/victory-chart/pull/482) Add `minBubbleSize` prop to `VictoryScatter`
- [victory-chart/484](https://github.com/FormidableLabs/victory-chart/pull/484) Remove inappropriate interpolation options for `VictoryArea` and `VictoryLine`
- [victory-chart/487](https://github.com/FormidableLabs/victory-chart/pull/487) Fix a bug in `VictoryPolarAxis` domain calculation

## 0.21.0 (2017-06-06)

- [victory-core/240](https://github.com/FormidableLabs/victory-core/pull/240) Polar Charts
- [victory-chart/466](https://github.com/FormidableLabs/victory-chart/pull/466) Polar Charts
- [victory-chart/475](https://github.com/FormidableLabs/victory-chart/pull/475) Brush and Zoom fixes
- [victory-chart/476](https://github.com/FormidableLabs/victory-chart/pull/476) Zoom Improvements

**Breaking Changes**

- Removes default bar width from themes
- Changes how default bar widths are calculated
- Changes render methods for `Area`, `Bar` and `Curve` primitives (Breaking change for `victory-native` and others extending primitives)
- Changes function signatures for `Selection.getDomainCoordinates` and `Selection.getDataCoordinates` (Breaking change for `victory-native`)

**Overview**

- Supports polar charts by adding the `polar` prop to charts.
- Polar charts are supported for `VictoryArea`, `VictoryChart`, `VictoryGroup`, `VictoryLine`, `VictoryScatter` `VictoryStack` and `VictoryVoronoi`
- Polar charts are supported for `VictoryBar`, but horizontal (radial) bars are not yet supported
- Polar charts work with `VictoryVoronoiContainer`
- Polar charts work with `VictorySelectionContainer`, but the dimension prop is not supported for polar selections
- Polar charts work with `VictoryZoomContainer`, but zooming is limited to centered radial zooming. Panning has no effect.
- Polar-specific default animations for `VictoryLine` and `VictoryArea`
- _horizontal polar charts are not yet supported_
- _`VictoryCandlestick` and `VictoryErrorBar` do not yet work with polar charts_
- _`VictoryCursorContainer` does not yet work with polar charts_
- _`VictoryBrushContainer` does not work with polar charts_
- _`VictoryZoomContainer` has limitations for polar charts_

**Planned additional work**

- Support for radial bars
- Support for spider charts (i.e. linear grid lines rather than arcs on polar charts)
- Support separate theming for polar charts (at least axes)
- Support for a polar version of `VictoryCursorContainer`
- Minimal support for polar versions of `VictoryCandleStick` and `VictoryErrorBar` (Elements will be correctly positioned and angled, but path elements will not be altered to reflect curvature, _i.e._ candles will still be `rects` rather than arc paths)
- Investigate hollow polar charts

**Details**

- Adds `VictoryPolarAxis` with new props: `axisAngle` `startAngle`, `endAngle`, and `labelPlacement`
- Adds `polar`, `startAngle`, `endAngle`, and `defaultPolarAxes` props for `VictoryChart`
- Adds `polar`, `origin`, and `range` props to all chart types.
- Adds `defaultPolarTransitions` static methods to `VictoryLine` and `VictoryArea`
- Uses `LabelHelpers` to simplify all `helper-methods`
- Adds a new `Arc` primitive which is used for polar axes and grid lines
- Adds `polar` and `origin` props to rendered components (primitives, `VictoryLabel`, `VictoryClipContainer` `VictoryContainer`)
- Supports radial areas for `Area` and `Curve`. These props have no effect for cartesian charts
- Adds an `openPath` prop for `Curve`. This prop is used to determine whether radial curves should be closed. Curves are closed by default, but when this prop is set to true they will not be. This prop has no effect for cartesian charts
- Supports polar bars in the `Bar` primitive. (Angular bars only, radial bars are not yet supported)
- Adds a `labelPlacement` prop to `VictoryLabel` and `VictoryTooltip`. Values are "parallel", "perpendicular", and "vertical". These flags help to appropriately position labels in polar charts. Polar charts will use "parallel" label placement by default. Cartesian charts will only use "'vertical" placement.
- Adds support for circular clipPath
- Adds support for polar animation transitions for continuous chart types. During `onLoad`, all points grow from zero. During `onEnter` and `onExit` new points are added / removed at the location of an adjacent point to keep path interpolation as smooth as possible. This implementation obviates the need for radial clip-path animations for these chart types.
- `before` and `after` callbacks for `onLoad`, `onEnter` and `onExit` are now called with `datum`, `index`, and `data` instead of only `datum`.
- Adds `LabelHelpers`
- Adds helper methods for polar charts

## 0.20.0 (2017-05-24)

**Breaking Changes** -[victory-chart/471](https://github.com/FormidableLabs/victory-chart/pull/471) Passes the string value of ticks to the `tickFormat` function rather than the associated index. _This may be a breaking change for users who are using categorical data and formatting tick values by index_

**New Features** -[victory-chart/474](https://github.com/FormidableLabs/victory-chart/pull/474) Adds support for a y0 accessor so that users can have granular control over the baseline of components like `VictoryArea` -[victory-core/246](https://github.com/FormidableLabs/victory-core/pull/246) Adds an `itemsPerRow` prop to `VictoryLegend` to support automatic legend wrapping

**Minor Changes** -[victory-chart/472](https://github.com/FormidableLabs/victory-chart/pull/472) Fixes a bug that was causing `VictoryGroup` to override styles on any independent `VictoryLabel` children -[victory-core/244](https://github.com/FormidableLabs/victory-core/pull/244) Passes missing `datum` and `index` props to `Flyout` -[victory-chart/250](https://github.com/FormidableLabs/victory-core/pull/250) Audits `shouldComponentUpdate` logic for all primitive components so that changes to optional props like `className` will cause components to re-render. -[victory-pie/146](https://github.com/FormidableLabs/victory-pie/pull/146) Rounds label positions for `VictoryPie`

## 0.19.1 (2017-05-12)

- [victory-chart/469](https://github.com/FormidableLabs/victory-chart/pull/469) Adds `VictoryCursorContainer`
- [victory-core/241](https://github.com/FormidableLabs/victory-core/pull/241) Adds optional `title` an `desc` props to `VictoryLabel`
- [victory-core/243](https://github.com/FormidableLabs/victory-core/pull/243) Improvements to `VictoryContainer`
  - Automatic `overflow: "visible"` for elements rendered in `VictoryPortal` (tooltips)
  - `VictoryContainer` no longer renders `g` tags (this was causing confusion with evented containers)
  - Default responsive styles are now `width: "100%"` `height: "100%"` (fixes a bug in safari)
  - Changes the merge order for responsive styles so that `width` and `height` attrs may be overridden
- [victory-core/244](https://github.com/FormidableLabs/victory-core/pull/244) adds missing `index` and `datum` props to `Flyout`
- [victory-core/245](https://github.com/FormidableLabs/victory-core/pull/245) fixes `dy` calculation in `VictoryLabel`

## 0.19.0 (2017-05-02)

**BREAKING CHANGE**

- Updates to `react@^15.5.0`
- Uses separate `prop-types` package
- Projects using Victory must also depend on `prop-types`
- Removes `VictoryVoronoiTooltip`

## 0.18.4 (2017-04-04)

- Supports combining container behaviors with the `createContainer` method.
- Individual `combineContainerMixins` are also exported.
- For more detail, see [victory-chart/453](https://github.com/FormidableLabs/victory-chart/pull/453)

## 0.18.3 (2017-03-21)

[victory-chart/447](https://github.com/FormidableLabs/victory-chart/pull/447) Improves performance for container components

- Fixes [#511](https://github.com/FormidableLabs/victory/issues/511)
- Fixes [#526](https://github.com/FormidableLabs/victory/issues/526)

## 0.18.2 (2017-03-15)

Minor bug fixes -[508](https://github.com/FormidableLabs/victory/issues/508) -[509](https://github.com/FormidableLabs/victory/issues/509) -[510](https://github.com/FormidableLabs/victory/issues/510) -[517](https://github.com/FormidableLabs/victory/issues/517) -[520](https://github.com/FormidableLabs/victory/issues/520)

## 0.18.1 (2017-03-14)

[518](https://github.com/FormidableLabs/victory/pull/518)

- Support multi-repo tooling

## 0.18.0 (2017-02-27)

**(VictoryVoronoiContainer)**

**BREAKING CHANGES**

- `VictoryTooltip` no longer automatically adds the `active` prop to data when hovered. To turn this behavior on, set the new `activateData` boolean prop on `VictoryTooltip`
- Deprecates `label` in favor of `labels` in `VictoryLine` and `VictoryArea`, allowing individual data labels for these components like in other Victory components. This will be a breaking change for anyone using the `label` prop in `VictoryLine` or `VictoryArea`. Series labels will need to be configured manually.
- `VictoryZoomContainer` now zooms both x and y dimensions, use the prop `dimension="x"` to return to the old behavior
- `VictoryZoomContainer` now centers zoom behavior on the mouse position rather than the center of the chart
- `VictoryZoomContainer` has a minimum zoom level of the extent of the domain / 1000. Set a custom minimum with the `minimumZoom` prop, which takes an object with numeric values for x and/ or y.
- `VictoryBrushContainer` no longer has `dimension="x"` as the default value.

**Deprecation Notice**
`VictoryVoronoi` and `VictoryVoronoiTooltip` have been replaced by `VictoryVoronoiContainer` and will be deprecated in version 0.20.0

[victory-core/196](https://github.com/FormidableLabs/victory-core/pull/196)

- `VictoryTooltip` no longer automatically adds the `active` prop to data when hovered. To turn this behavior on, set the new `activateData` boolean prop on `VictoryTooltip`
- Adds a `theme` prop to `VictoryContainer` so that custom containers may pick up themes from their parents
- Removes default `title` and `desc` props from `VictoryContainer`
- Adds support for providing `text` as an array for `VictoryLabel`
- Adds support for providing `style` as an array for `VictoryLabel` so that each line of a multi-line label may be styled independently
- Changes how null data values are handled by `Area` and `Curve` primitives
- Adds a `reduceChildren` method to `Helpers` to ensure order consistency when working with nested children

[victory-core/201](https://github.com/FormidableLabs/victory-core/pull/201)

- implements data sorting for all components with a `sortKey` props

[victory-chart/432](https://github.com/FormidableLabs/victory-chart/pull/432)

- Adds `VictoryVoronoiContainer` for hover events (tooltips). `VictoryVoronoiContainer` has several benefits over `VictoryVoronoi` and `VictoryVoronoiTooltip`
  - Supports multi-dataset voronoi
  - Much better performance (voronoi polygons are not actually rendered, so the number of nodes rendered is dramatically lower)
  - Supports multi-data tooltips
  - Supports rectangular selections with a dimension prop
    _i.e._ `dimension="x"` creates vertical hover areas for every unique x value in all child data
- Deprecates `label` in favor of `labels` in `VictoryLine` and `VictoryArea`, allowing individual data labels for these components like in other Victory components. This will be a breaking change for anyone using the `label` prop in `VictoryLine` or `VictoryArea`. Series labels will need to be configured manually
- Changes how null values are handled in `VictoryArea`, and groups all line and area segments (i.e. split by null values) into the same `eventKey`, so that they operate as a single line for the purposes of events.

[victory-chart/438](https://github.com/FormidableLabs/victory-chart/pull/438)

- Supports x and y dimension zooming in `VictoryZoomContainer`
- Adds a `minimumZoom` prop for `VictoryZoomContainer`
- Zooming centers on mouse position rather than in the center of the chart

[victory-core/207](https://github.com/FormidableLabs/victory-core/pull/207)

- Adds a `translateY` prop for `ClipPath` to support x, y zoom behavior
- Removes default `clipPadding` in `ClipPath`

## 0.17.0 (2017-02-05)

- [victory-core/195](https://github.com/FormidableLabs/victory-core/pull/195)
  - Fixes null event state bug
- [victory-chart/431](https://github.com/FormidableLabs/victory-chart/pull/431)
  - Sets a maximum amount of scale per zoom event for smoother interaction with fast onWheel events

## 0.16.1 (2017-02-03)

- [victory-chart/429](https://github.com/FormidableLabs/victory-chart/pull/429)
- Throttles `onWheel` and `onMouseMove` events on Victory container components
- Exports container event helpers

## 0.16.0 (2017-01-30)

**This release includes major breaking changes related to `VictoryZoom`**

- [victory-core/189](https://github.com/FormidableLabs/victory-core/pull/189) and [191](https://github.com/FormidableLabs/victory-core/pull/190)
  - Adds `VictoryLegend` component
- [victory-core/190](https://github.com/FormidableLabs/victory-core/pull/190)
  - Allows `VictoryContainer` to render either `<g>` or `<svg>` depending on the value of the standalone prop
  - Passes a timer down in context for `VictorySharedEvents`
  - Event handlers have access to the context they are being called from via an argument
    - _i.e._ `onClick: (event, targetProps, eventKey, context) => {...}`
  - Enhances `addEvents` so that evented components can pick up "parentControllerProps" from parent state
    - useful for `VictoryZoomContainer`
  - Adds the ability to define callbacks in the events prop that will be called after `setState`
    - useful for allowing `VictoryZoomContainer` to call methods like `resumeAnimation`
- [victory-chart/427](https://github.com/FormidableLabs/victory-chart/pull/427)
  - Adds `VictoryBrushContainer`
  - Adds `VictoryZoomContainer` to replace `VictoryZoom`
  - [See pull request for examples](https://github.com/FormidableLabs/victory-chart/pull/427)
  - **Deprecates `VictoryZoom`**
  - Changes default styles for `VictorySelectionContainer`
  - Adds override-able `selectionComponent` for `VictorySelectionContainer`
  - Adds `domain` and `standalone` to list of props that get stored in parent state
  - Simplifies and standardizes container rendering across components.

## 0.15.0 (2017-01-03)

- Adds `VictorySelectionContainer`
- Changes when functional styles and props are evaluated (this may be a breaking change)
  Functional styles and props are now evaluated in the primitive components (`Point`, `Bar` etc.)
- Supports an `active` prop on all primitive components that is used when evaluating functional styles and props
- Tooltips now trigger `active: true` on both labels and data components
- `defaultEvents` are supported for `containerComponents`

## 0.14.2 (2016-12-13)

- Fixes date handling in VictoryZoom
- Adds support for className in primitive components

## 0.14.1 (2016-12-12)

- Fixes minor animation bugs
- Fixes bugs in VictoryZoom
- Adds `pointerEvent: "none"` to tooltip

## 0.14.0 (2016-12-02)

_This may be a breaking change for animating VictoryArea and VictoryLine. Animation behavior is changed._

- Change how animations behave for continuous data _i.e._ VictoryArea and VictoryLine
  - clipPath curtain will never be smaller than the range except during `onLoad`
- Simplify transitions code
- Ensure that animations and transitions use the global timer passed in context or create their own
- Fix `bypassAnimation` bug
- Ensure that clipPath width and height are never negative

## 0.13.7 (2016-11-09)

- Fix timer issues and export VictoryZoom

## 0.13.6 (2016-11-09)

- Temporarily revert global animation timer and VictoryZoom changes

## 0.13.5 (2016-11-09)

- Export VictoryZoom

## 0.13.4 (2016-11-09)

- Add global animation timer
- Add VictoryZoom

## 0.13.3 (2016-10-31)

- Stricter npmignore

## 0.13.2 (2016-10-28)

- Uses `publishr` to reduce npm installed package size [#413](https://github.com/FormidableLabs/victory/issues/413)
- Fixes a bug where label padding was not being applied to tick labels [#408](https://github.com/FormidableLabs/victory/issues/408)
- Removes default tick padding in themes
- Changes how the domain is calculated when there is only one data point, or when the minimum and maximum of the data is equal in a given dimension [#407](https://github.com/FormidableLabs/victory/issues/407)
- Removes hard-coded `<g>` from `VictorySharedEvents` [#402](https://github.com/FormidableLabs/victory/issues/402)
- Ensures that ticks array is not empty after filtering zeroes for crossed axes
- Fixes naming for "stack" and "group" roles

## 0.13.1 (2016-10-26)

- Allows npm 2 installs
- Fixes incorrect Aria roles

## 0.13.0 (2016-10-13)

- Removes support for `children` for VictoryLabel. Use `text` instead
- Upgrades all d3 packages
- Greater consistency of props for props passed to primitive components
- Adds `VictoryPortal` which renders any child elements in a top level portal container if it exists
- `VictoryTooltip` uses `VictoryPortal` by default
- Adds `VictoryClipContainer` which renders children in a group container with a `clipPath` if `clipPath` props exist. This component is used for animating continuous data types like `VictoryLine`. It should not be used for custom clipPaths.
- `VictoryArea` and `VictoryLine` use `VictoryClipContainer` as their `groupComponent`
- Removes `clipPath` properties from `VictoryLine` and `VictoryArea`
- Extracts event logic into a new inverted inheritance higher order component `addEvents` which is used by all chart components
- Fixes a bug in `VictoryTransition` that was causing unnecessary rerendering
- Exposes `Data`, `Domain`, `Scale`, and other helpers
- Fixes date related domain bugs
- Fixes stacking for time scale data
- Supports separate theming for x and y axes

## 0.12.1 (2016-09-05)

- Fixes ordering for stacked and grouped data
- Fixes broken tooltips in Safari
- Fixes tooltip fill bug
- Corrects `propTypes` in `VictoryTooltip`
- Removes unused `flyoutProps` prop in `VictoryVoronoiTooltip`

## 0.12.0 (2016-09-09)

**This is a breaking change for label placement in VictoryPie**

- Adds support for `VictoryTooltip`
- Adds `VictoryVoronoi` component
- Adds `VictoryVoronoiTooltip`
- Moves all primitive rendered components to `victory-core` where they are exported for external use
- Enhances `VictoryGroup` so that it can accept a `data` prop which it will pass to all children. This also allows groups of components to be stacked as one
- Adds support for `defaultEvents` in any primitive component (_i.e._ `dataComponent`, `labelComponent`)
- Adds `onLoad` animations
- Adds a `sortKey` prop to `VictoryLine` to allow sorting by fields other than "x"
- Adds a `fixLabelOverlap` boolean prop to `VictoryAxis`. When enables, this feature renders a smaller subset of ticks when the full set of ticks would cause overlapping labels. This feature is currently limited to evenly spaced labels.
- Fixes a bug related to `bubbleProperty` in `VictoryScatter`
- Allows string data in `VictoryCandlestick` and `VictoryErrorBar`
- Performance optimizations

## 0.11.0 (2016-08-18)

**This is a breaking change for themes across all components and for label placement in VictoryPie**

- Updates VictoryTheme API, uses `VictoryTheme.grayscale` for default styling
- Alters label placement in VictoryPie so that when label styles include padding, the `innerRadius` of the pie does not effect label placement.
- Adds a `displayName` to all components for ease of debugging
- Improves animation for continuous data components (_i.e._ VictoryLine, VictoryArea) using clipPath
- Improves performance by simplifying scale type checking for VictoryBar and VictoryArea
- Supports for arrays of `childName` in events
- Fixes a bug related to bar width

## 0.10.4 (2016-08-05)

- Fix PropType warnings in React 15.3+
- Add animationInfo as 2nd argument to victory-animation child function call

## 0.10.3 (2016-08-04)

- Fixes minor domainPadding bugs for stacked and grouped charts
- Fixes a bug in generated data

## 0.10.2 (2016-08-02)

- Fix animation and style bugs for VictoryCandlestick
- Fix layout bug effecting negative axes in VictoryChart
- Update docs

## 0.10.1 (2016-08-01)

- Fix minor error bar bug
- Fix minor axis style bugs

## 0.10.0 (2016-07-29)

**Breaking Changes**

- Default styles and some default props have changed across all components in this release.

_VictoryTheme_

- All Victory components support a `theme` prop that can be used to define styles and props across different component types.
- `victory-core` includes the [material theme](https://github.com/FormidableLabs/victory-core/blob/master/src/victory-theme/material.js)

_VictoryCandlestick_

- The new `VictoryCandlestick` component may be used standalone or in conjunction with `VictoryChart`. It has an identical API and feature set as other chart compatible components with the exception of the `data` and data accessor props. `VictoryCandlestick` expects `data` in the form `[{x: value, high: NUMBER, low: NUMBER, open: NUMBER, close: NUMBER}...]`, and includes data accessor props `x`, `high`, `low`, `open`, and `close`.

_VictoryErrorBar_

- The new `VictoryErrorBar` component may be used standalone or in conjunction with `VictoryChart`. It has an identical API and feature set as other chart compatible components with the exception of the `data` and data accessor props. `VictoryErrorBar` expects `data` in the form `[{x: value, y: value, errorX: ERR, errorY: ERR}...]`, Where `ERR` is a number or a two value array for asymmetric errors. `VictoryErrorBar` also includes data accessor props `errorX` and `errorY`.

_VictoryNative_

- Changes have been made across all components in order to support [victory-native](https://github.com/FormidableLabs/victory-native). `VictoryNative` has an identical API to `Victory`, and reuses most of the code. Changes made to `Victory` to support `VictoryNative` are all non-breaking, and minimal. They include the addition of a `groupComponent` prop in all components (which defaults to `<g>`), removing svg transforms whenever possible in favor of absolute positioning, and code reorganization.

_Performance improvements_

- Low-hanging performance improvements included in this release:
  - Replace `Object.assign` with lodash `assign`
  - Replace `map` / `reduce` array methods with length-cached `for` loops in methods responsible for rendering elements

_Misc_

- Improvements for `domainPadding`
  - `domainPadding` is supported in all components compatible with `VictoryChart`
  - Negative and asymmetric `domainPadding` is supported. Example: `domainPadding={{x: [-20, 20], y: 50}}`
  - Grouped bar charts get automatic `domainPadding` so that bars wont overflow axes in most cases.
- Adds Aria roles for all rendered elements
- Fixes [bugs related to log scales](https://github.com/FormidableLabs/victory-chart/pull/317)
- Fixes [a bug related to time scales](https://github.com/FormidableLabs/victory-chart/pull/318)
- Improves consistency for charts with empty and single value data arrays
- Removes `reduce-calc-css` as a dependency

## 0.9.0 (2016-06-17)

_Events enhancements_

- Supports events on parent containers (_i.e._ top level `<svg>`) via the `parent` namespace in the `events` prop
- In VictoryChart, `parent` events have access to `width`, `height`, `style` and the calculated `scale` (with `domain` and `range` already applied). Where applicable `parent` events also have access to `data`
- in VictoryPie `parent` events have access to `width`, `height`, `style` and the calculated `slices` and the calculated `pathFuncton`
- When mutating elements via the return from event handlers, mutation objects may now take arrays for `eventKey` to target several individual elements, or the special value "all" to apply changes to all elements of a particular target type
- Associates parent events with child events via a `container` prop on `VictorySharedEvents`. This is useful where shared events are automatic as in `VictoryChart`, `VictoryStack` and `VictoryGroup`

_VictoryContainer_

- Supports a custom `containerComponent` in all chart types.
- `containerComponent` defaults to the new `VictoryContainer` which renders an `<svg>` with default `title` and `description` aria roles

_Full support for horizontal bar charts_

- Fixes bugs related to axis layout of horizontal bar charts

_Misc improvements_

- Adds `vectorEffect: "non-scaling-stroke"` where applicable for improved readability in responsive charts
- Increases default `fontSizes` for improved readability
- Removes parent transform from `VictoryAxis` so that custom elements can be absolutely positioned more easily
- Alters `VictoryAxis` render order to that grids are rendered _under_ labels
- Alters the render order of _default_ axes in `VictoryChart` so that default axes will render under data. Explicitly defined axes will still render in whatever order they are defined
- Adds a `cornerRadius` prop to `VictoryPie` to enable pie slices with rounded corners. Thanks @judikdavid!
- Renders all pie slices _before_ labels to prevent slices from overlapping labels

_Bug fixes_

- Fixes a bug related to transforms for `VictoryLabel`
- Fixes a bug in `VictoryGroup` that was causing custom `labelComponents` in its children to be overridden.
- Fixes a bug related to incorrect an incorrect default `tickFormat` being applied to dates

## 0.8.0 (2016-06-01)

- Upgrades to React 15
- Supports wrapped components in `VictoryChart`
- Adds `VictorySharedEvents` wrapper for coordinating events between supported Victory Components. An annotated example of the new events API:

```
<VictorySharedEvents
  events={[
    {
      childName: "firstBar", // if a child name is not provided, event will be attached to all children.
      target: "data", // what type of element to attach to. Matches the style namespaces
      eventKey: 1, // What event key of element to attach to. Defaults to the index in data.
      eventHandlers: {
        onClick: () => {
          return {
            childName: "secondBar", // the child to be modified
            // props here are the props that define the targeted component i.e. what is passed to an individual bar
            mutation: (props) => {
              return {style: merge({}, props.style, {fill: "blue"})}; // Whatever is returned here will override the existing props
            }
          };
        }
      }
    }, {
      childName: "secondBar",
      target: "data",
      eventKey: 0,
      eventHandlers: {
        onClick: () => { // event handlers can return an array of mutation objects with different targeted elements
          return [
            {
              childName: "firstBar",
              mutation: (props) => {
                return {style: merge({}, props.style, {fill: "cyan"})};
              }
            }, {
              mutation: (props) => { // the default target element is whatever element the handler is attached to
                return {style: merge({}, props.style, {fill: "orange"})};
              }
            }, {
              target: "labels",
              eventKey: 1,
              mutation: () => {
                return {text: "CLICKED"};
              }
            }
          ];
        }
      }
    }
  ]}
>
  <VictoryBar
    name="firstBar" // if children don't have name props they can be referenced by index in shared events
    style={{
      data: {width: 25, fill: "gold"}
    }}
    data={[{x: "a", y: 2}, {x: "b", y: 3}, {x: "c", y: 4}]}
  />
  <VictoryBar
    name="secondBar"
    data={[{x: "a", y: 2}, {x: "b", y: 3}, {x: "c", y: 4}]}
  />
</VictorySharedEvents>
```

## 0.7.0 (2016-05-13)

- improves consistency for `labelComponent` and `dataComponent` props. Replaces a custom label components with `VictoryLabel` to make the api more consistent and predictable. **This is a breaking change for custom label components**, as `VictoryLabel` expects a different set of props than the previous label components. See [VictoryLabel](http://formidable.com/open-source/victory/docs/victory-label) for more detail.

- Custom components are now supported for all rendered axis elements (axis, axisLabel, grid, ticks, tickLabels)

- All data and label components now have access to scale so that they can create correctly scaled elements from data i.e. error bars.

- Functional styles and props are now all evaluated before they are passed as props to `labelComponent` or `dataComponent`, so that custom components will have access to the final values.

- events are bound and partially applied prior to being passed as props to `labelComponent` or `dataComponent`

- it is now possible to specify `angle` and `verticalAnchor` props for`VictoryLabel` via the style object

- event return values are stored differently on state to facilitate interaction between data and labels. **This is a breaking change for events** as event handlers must now return an object with `data` and/or `labels` keys so that these values may be applied appropriately to data and label elements respectively.

## 0.6.1 (2016-04-19)

- Fixes a bug in VictoryChart, VictoryGroup and VictoryStack, which was causing null animation props to be ignored.

## 0.6.0 (2016-04-15)

**BREAKING CHANGES**

- VictoryBar and VictoryArea no longer support multiple datasets.
- VictoryStack and VictoryGroup define stacked and grouped layouts for their children
- Custom data components supported on for VictoryBar, VictoryLine, VictoryScatter,
  VictoryArea, and VictoryPie via the `dataComponent` prop
- Enter and exit transitions animate. Enter and exit transition defaults defined
  VictoryBar, VictoryArea, VictoryScatter, VictoryLine and VictoryPie. Custom transitions may be
  defined via the `onExit` and `onEnter` properties of the `animation` prop
- Top level svgs are all responsive by default (using svg viewBox). To render a fixed size
  component, set the `standalone` prop to false and render the component inside an svg tag

## 0.5.0 (2016-03-15)

- Adds VictoryArea as a data type compatible with VictoryChart.
- Supports custom label components
- Upgrades all repos to lodash 4

## 0.4.1 (2016-03-01)

- Remove dependency on Radium
- Many performance improvements via memoization and replacing expensive merge operations
- Ignore source maps for smaller NPM installation
- Add `npm start` and `npm test` scripts for easier contribution dev workflow
- Show gzipped size for minified distribution file
- Code reorganization
- Update roadmap

## 0.4.0 (2016-01-31)

- Data Accessors are supported on all relevant components
- Shared code has been factored into VictoryUtil

Breaking Changes:

- VictoryLine and VictoryScatter:
  - plotting functions via the `y` prop must now be expressed as functions of
    data rather than functions of x. _i.e._ `y={(data) => Math.sin(data.x)}`
- VictoryBar:
  - the domain of the dependent axis will automatically include zero unless a domain is specified via props
  - automatic alphabetic data sorting has been removed
  - automatic 1% domain padding has been removed
- VictoryChart:
  - automatic 1% domain padding has been removed
  - default line data has been removed, so `<VictoryChart/>` with no additional props will now only render a set of
    axes with no data

## 0.3.0 (2016-01-26)

- Demo application now works with hot reloading.
- Application dependencies like `radium` and `lodash` now live in components, not in the Builder archetype. This is a breaking change. https://github.com/FormidableLabs/victory/issues/176

## 0.2.0 (2016-01-15)

- Upgrade to Radium 0.16.2. This is a breaking change if you're using media queries or keyframes in your components. Please review upgrade guide at https://github.com/FormidableLabs/radium/blob/master/docs/guides/upgrade-v0.16.x.md

## 0.1.3 (2015-12-30)

- Fix build

## 0.1.2 (2015-12-30)

- Add Cumulative min and max
- Fix Firefox date issue

## 0.1.1 (2015-12-29)

- Fix issue with exported `dist` file. You can now include Victory via `npmcdn`: `https://npmcdn.com/victory/dist/victory.min.js`

## 0.1.0 (2015-12-18)

The following components documented and ready to use; API subject to change:

- victory-animation
- victory-axis
- victory-bar
- victory-chart
- victory-label
- victory-line
- victory-pie
- victory-scatter

Functional styles and functional props (where appropriate) are implemented for all the data primitives (VictoryBar, VictoryLine etc.) and VictoryAxis

Components use d3-modules

Basic code coverage across all Victory components.

We make no promises about any code prior to this release. From this point on, you can expect a regular release schedule (~every two weeks) with detailed release notes. Check out our roadmap for upcoming features
