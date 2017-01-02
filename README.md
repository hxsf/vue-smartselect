# vue-smartselect

> a smart select

## ScreenShot

![ScreenShot_01](https://raw.githubusercontent.com/hxsf/vue-smartselect/master/screenshots/screenshot_01.gif)

## Usage

`/dist/build.js` is a prebuilt version.

``` bash
# install dependencies
yarn add hxsf/vue-smartselect
```

``` html
<div>
	<smart-select :items="items" :selecteds="selecteds" :label="{label: 'name', value: 'value'}"></smart-select>
</div>
```

``` js
import SmartSelect from 'vue-smartselect'

const items =  [
	{name: "abc", value: "aaa"},
	{name: "aut"}, // same as {name: "aut", value: "aut"}
	{name: "accusamus"},
	{name: "beatae"},
	{name: "culpa"},
	{name: "dicta"},
	{name: "dolorum"},
	{name: "ea"},
	{name: "est"},
	{name: "fuga"},
	{name: "iad"},
	{name: "iusto"},
	{name: "magnam"},
	{name: "maxime"},
	{name: "non"},
	{name: "optio"},
	{name: "tempore"},
	{name: "voluptatem"}
]
const selecteds = []
// to select top three
selecteds.push(items[0])
selecteds.push(items[1])
selecteds.push(items[2])

export default {
	// Options / Data
	data () {
		return {
			items: items,
			selecteds: selecteds
		}
	},
	components: {SmartSelect}
}
```

## License

MIT
