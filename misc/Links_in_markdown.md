Put links inside codeblocks on GitHub:

```html
<pre>
import { assign, map } from '<a href="https://www.npmjs.com/package/lodash" title="Lodash on npm">lodash</a>';

<a href="https://lodash.com/docs#assign" title="assign documentation">assign</a>({ 'a': 1 }, { 'b': 2 }, { 'c': 3 });
// → { 'a': 1, 'b': 2, 'c': 3 } 
<a href="https://lodash.com/docs#map" title="map documentation">map</a>([1, 2, 3], function(n) { return n * 3; });
// → [3, 6, 9] 
</pre>
```

And we get (try clicking "_lodash_", "_assign_", and "_map_"):

<pre>
import { assign, map } from '<a href="https://www.npmjs.com/package/lodash" title="Lodash on npm">lodash</a>';

<a href="https://lodash.com/docs#assign" title="assign documentation">assign</a>({ 'a': 1 }, { 'b': 2 }, { 'c': 3 });
// → { 'a': 1, 'b': 2, 'c': 3 } 
<a href="https://lodash.com/docs#map" title="map documentation">map</a>([1, 2, 3], function(n) { return n * 3; });
// → [3, 6, 9] 
</pre>

Note that it's not possible to combine with syntax highlighting (as we [can't use the `class` attribute](https://github.com/jch/html-pipeline/blob/master/lib/html/pipeline/sanitization_filter.rb#L44-L76)).